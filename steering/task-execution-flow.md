---
inclusion: always
---

# Task Execution Flow — Hook Compliance

## Overview

Panduan WAJIB untuk memastikan semua hooks (pre-task dan post-task) dijalankan dengan benar saat mengeksekusi spec tasks. Dokumen ini menjelaskan **seluruh hook lifecycle** dari awal sprint hingga akhir sprint.

## Hook Registry (8 Hooks)

| # | Hook | Event | Kapan Aktif |
|---|------|-------|-------------|
| 1 | **spec-context-loader** | `promptSubmit` | Saat user minta buat spec (requirements/design/tasks) |
| 2 | **pre-task-pipeline** | `preTaskExecution` | Setiap task dimulai |
| 3 | **post-task-pipeline** | `postTaskExecution` | Setiap task selesai |
| 4 | **sprint-completion** | `postTaskExecution` | Bug fix task + task terakhir sprint |
| 5 | **health-check** | `userTriggered` | Manual trigger |
| 6 | **quality-scorecard** | `userTriggered` | Manual trigger |
| 7 | **sprint-retrospective** | `userTriggered` | Manual trigger |

---

## Complete Lifecycle Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                        SPRINT LIFECYCLE                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  [User submits prompt]                                               │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────────────────┐                                            │
│  │ spec-context-loader  │ ← promptSubmit                            │
│  │ (detect phase →      │                                            │
│  │  load 11-17 docs)    │                                            │
│  └─────────────────────┘                                            │
│                                                                      │
│  ═══════════════════════════════════════════════════════════════     │
│                                                                      │
│  [Task N starts]                                                     │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────────────────┐                                            │
│  │ pre-task-pipeline    │ ← preTaskExecution                        │
│  │                      │                                            │
│  │ Phase 0: CURRENT-STATE → In Progress                             │
│  │ Phase 1: Architect Gate (validate spec/design)                   │
│  │ Phase 2: GitLab Issue (create/find)                              │
│  │ Phase 3: Sprint Kickoff (task pertama saja):                     │
│  │          - Create git branch                                      │
│  │          - Create GitLab milestone                                │
│  │          - Update wiki Sprint Plan                                │
│  │          (task lainnya: checkout branch + assign milestone)       │
│  └─────────────────────┘                                            │
│       │                                                              │
│       ▼                                                              │
│  [Implementation by subagent]                                        │
│       │                                                              │
│       ▼                                                              │
│  [Task N completes]                                                  │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────────────────┐                                            │
│  │ post-task-pipeline   │ ← postTaskExecution                       │
│  │                      │                                            │
│  │ Phase 1: QA (lint + typecheck + test ≥80%)                       │
│  │ Phase 2: Git (add + commit + push)                               │
│  │ Phase 3: GitLab Issue (close + label done)                       │
│  │ Phase 4: Metrics (append to metrics-log.jsonl)                   │
│  │ Phase 5: CURRENT-STATE → Done                                    │
│  └─────────────────────┘                                            │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────────────────┐                                            │
│  │ sprint-completion    │ ← postTaskExecution                       │
│  │                      │                                            │
│  │ Branch A: Bug Learning (jika bug fix task)                       │
│  │   → Root cause + prevention → docs/learnings/                    │
│  │                                                                   │
│  │ Branch B: Sprint End (jika task TERAKHIR)                        │
│  │   → Health check + npm audit                                      │
│  │   → Close GitLab milestone                                        │
│  │   → Wiki Sprint Summary + Changelog                              │
│  │   → Inform user + offer retro/scorecard                          │
│  └─────────────────────┘                                            │
│       │                                                              │
│       ▼                                                              │
│  [Next wave / Sprint complete]                                       │
│                                                                      │
│  ═══════════════════════════════════════════════════════════════     │
│                                                                      │
│  [Manual triggers — user dapat invoke kapan saja]                    │
│       │                                                              │
│       ├── health-check ──→ Full framework compliance scan            │
│       ├── quality-scorecard ──→ Generate scorecard dari metrics      │
│       └── sprint-retrospective ──→ Generate retro data-driven       │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Rules

### Spec Context Loader (promptSubmit)

Saat user mengirim pesan yang berkaitan dengan pembuatan spec:

1. Hook `promptSubmit` ter-trigger pada SETIAP pesan user
2. Hook detect kata kunci untuk menentukan phase (requirements/design/tasks)
3. Jika match → load dokumen referensi yang sesuai (11-17 docs)
4. Jika tidak match → skip tanpa output
5. Dokumen yang di-load menjadi context untuk pembuatan spec

### Pre-Task Pipeline (preTaskExecution)

SEBELUM memulai implementasi task apapun:

1. Hook `preTaskExecution` akan otomatis ter-trigger saat task status di-set ke `in_progress`
2. WAJIB menjalankan SEMUA phase (0-3) sebelum dispatch ke subagent
3. Jangan skip phase manapun — jika ada yang gagal (misal GitLab auth), catat dan lanjut ke phase berikutnya
4. Untuk batch/parallel tasks: jalankan pre-task pipeline SEKALI untuk seluruh batch (bukan per-task)

**Phase breakdown:**

| Phase | Aksi | Selalu? |
|-------|------|---------|
| 0 | Update CURRENT-STATE → In Progress | ✅ Setiap task |
| 1 | Architect Gate (validate spec/design) | ✅ Setiap task |
| 2 | Create/find GitLab issue | ✅ Setiap task |
| 3a | Create branch + milestone + wiki | ❌ Task pertama saja |
| 3b | Checkout branch + assign milestone | ✅ Task lainnya |

### Post-Task Pipeline (postTaskExecution)

SETELAH task selesai (status → `completed`):

1. Hook `postTaskExecution` akan otomatis ter-trigger saat task status di-set ke `completed`
2. WAJIB menjalankan SEMUA phase (1-5) SEBELUM melanjutkan ke wave/task berikutnya
3. Urutan: task complete → post-task pipeline → sprint-completion → next wave
4. Untuk batch/parallel tasks: jalankan post-task pipeline SEKALI setelah seluruh batch selesai

**Phase breakdown:**

| Phase | Aksi | Blocking? |
|-------|------|-----------|
| 1 | QA: lint + typecheck + test (≥80%) | ⛔ Jika gagal → fix dulu |
| 2 | Git: add + commit + push | ✅ Setelah QA pass |
| 3 | GitLab: close issue + label done | ✅ |
| 4 | Metrics: append to metrics-log.jsonl | ✅ |
| 5 | CURRENT-STATE: update → Done | ✅ |

### Sprint Completion (postTaskExecution)

Ter-trigger SETELAH post-task-pipeline, dengan 2 conditional branches:

| Branch | Kondisi | Aksi |
|--------|---------|------|
| A: Bug Learning | Task title mengandung 'fix'/'bug' | Capture root cause → docs/learnings/ |
| B: Sprint End | Semua task done (task terakhir) | Health check + npm audit + close milestone + wiki summary + changelog |

### Manual Hooks (userTriggered)

User dapat trigger kapan saja via Agent Hooks panel:

| Hook | Output |
|------|--------|
| health-check | `docs/quality/health-check-[date].md` + score (Healthy/Warning/Critical) |
| quality-scorecard | `docs/quality/sprint-[N]-scorecard.md` + grade (A-F) |
| sprint-retrospective | `docs/retrospectives/sprint-[N].md` + wiki Changelog update |

---

## Flow Diagrams

### Single Task Execution

```
[Set in_progress] → [PRE-TASK PIPELINE] → [Dispatch to subagent]
→ [Subagent completes]
→ [Set completed] → [POST-TASK PIPELINE] → [SPRINT-COMPLETION]
→ [Next wave]
```

### Batch Execution Pattern (Wave-Based)

```
1. Get ready tasks (wave N)
2. Set all to in_progress
3. Run PRE-TASK pipeline (once for batch)
4. Dispatch all subagents in parallel
5. Wait for all to complete
6. Set all to completed
7. Run POST-TASK pipeline (once for batch)
8. Run SPRINT-COMPLETION (once for batch)
9. Proceed to wave N+1
```

### Sprint First Task (Special)

```
[Task 1 starts]
→ Phase 0: CURRENT-STATE → In Progress
→ Phase 1: Architect Gate ✅
→ Phase 2: Create GitLab Issue #1
→ Phase 3a: 🌿 Create branch feature/sprint-1-[project]
→ Phase 3b: 🏁 Create milestone 'Sprint 1'
→ Phase 3c: 📖 Update wiki Sprint-1-Plan
→ [Implementation...]
```

### Sprint Last Task (Special)

```
[Task N completes]
→ POST-TASK PIPELINE (QA + push + close issue + metrics + CURRENT-STATE)
→ SPRINT-COMPLETION:
  → Branch A: Bug learning (if applicable)
  → Branch B: Sprint End ✅
    → Health check + npm audit
    → 🏁 Close milestone 'Sprint 1'
    → 📖 Wiki Sprint-1-Summary + Changelog
    → 🎉 "Sprint 1 selesai!"
    → Offer: retro / scorecard / health check
```

---

## GitLab Formatting Rules

Saat membuat atau update content di GitLab (issue, wiki, MR, milestone, comment):

- ❌ JANGAN gunakan escaped newlines (`\\n`) dalam description
- ✅ Gunakan actual newlines (multiline string)
- ✅ Format: proper Markdown dengan heading, bullet points, dan checklist

---

## Enforcement

- Agent WAJIB menunggu dan menjalankan hook instructions
- Agent DILARANG skip post-task pipeline untuk "efisiensi"
- Agent DILARANG skip sprint-completion setelah post-task-pipeline
- Jika hook gagal, catat error dan lanjut — jangan silent fail
- Manual hooks (health-check, scorecard, retro) hanya jalan saat user trigger
