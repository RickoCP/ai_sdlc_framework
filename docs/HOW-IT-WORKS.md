# Cara Kerja — Enterprise AI-Native SDLC Framework

Dokumen ini menjelaskan **arsitektur internal** power dan bagaimana setiap komponen bekerja di project user.

---

## 1. Arsitektur Power

Power ini terdiri dari 3 komponen utama yang saling terhubung:

```
┌─────────────────────────────────────────────────────────────┐
│                    KIRO POWER (Enterprise AI-SDLC)          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │  POWER.md    │  │  Steering    │  │  MCP Servers     │  │
│  │  (Manifest)  │  │  Files (26)  │  │  (GitLab + Git)  │  │
│  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘  │
│         │                  │                    │            │
│         ▼                  ▼                    ▼            │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              AI Agent Runtime                        │    │
│  │  (Role switching, Hook execution, MCP calls)        │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Komponen 1: POWER.md (Manifest & Rules)

File utama yang mendefinisikan:
- Metadata power (nama, keywords, author)
- 15 layer framework dan deskripsinya
- Agent Workflow Rules (auto-detect, display format, konfirmasi)
- Solo Mode configuration
- MCP server configuration
- Automated Project Setup steps

### Komponen 2: Steering Files (26 files)

Steering files adalah **instruksi detail** per-layer yang di-load ke context AI Agent:

| Tipe Inclusion | Behavior | Contoh |
|----------------|----------|--------|
| `always` | Selalu aktif di setiap session | architecture-standards, test-writing-patterns, project-memory, hooks-registry |
| `manual` | Di-load saat layer tertentu dikerjakan | layer-0 s/d layer-14, fast-track-mode |

### Komponen 3: MCP Servers (GitLab + Git)

```
┌─────────────────────┐     ┌─────────────────────┐
│  GitLab MCP         │     │  Git MCP            │
│  @zereight/         │     │  @cyanheads/        │
│  mcp-gitlab         │     │  git-mcp-server     │
├─────────────────────┤     ├─────────────────────┤
│ • create_project    │     │ • git_init          │
│ • create_issue      │     │ • git_commit        │
│ • create_branch     │     │ • git_push          │
│ • create_mr         │     │ • git_branch        │
│ • push_files        │     │ • git_status        │
│ • manage_labels     │     │ • git_diff          │
│ • manage_milestones │     │ • git_log           │
└─────────────────────┘     └─────────────────────┘
```

---

## 2. Flow Aktivasi Power (Auto-Detect)

Saat power diaktifkan atau session baru dimulai, AI Agent menjalankan **auto-detect sequence**:

```
┌─────────────────────────────────────────────────────────┐
│                  SESSION DIMULAI                          │
└────────────────────────┬────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│  AUTO-DETECT 1: Project existing?                        │
│  Cek: .kiro/ ada? docs/ ada? src/ ada?                  │
├─────────────────────────────────────────────────────────┤
│  YA (minimal 1) → project existing → lanjut detect 2-4  │
│  TIDAK → project baru → tanyakan 5 pertanyaan           │
└────────────────────────┬────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│  AUTO-DETECT 2: Hooks lengkap?                           │
│  Cek: .kiro/hooks/ ada? 8 files lengkap?                │
├─────────────────────────────────────────────────────────┤
│  TIDAK ADA → generate semua 8 hook files                 │
│  KURANG → generate yang missing                          │
│  LENGKAP → skip                                          │
└────────────────────────┬────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│  AUTO-DETECT 3: Steering lengkap?                        │
│  Cek: .kiro/steering/ ada? 3 files minimum?             │
├─────────────────────────────────────────────────────────┤
│  TIDAK ADA → generate steering files                     │
│  KURANG → generate yang missing                          │
│  LENGKAP → skip                                          │
└────────────────────────┬────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│  AUTO-DETECT 4: Current State ada?                       │
│  Cek: docs/CURRENT-STATE.md ada?                        │
├─────────────────────────────────────────────────────────┤
│  ADA → baca, resume dari last state                      │
│  TIDAK ADA → buat template kosong                        │
└────────────────────────┬────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│  WORKFLOW NORMAL                                         │
│  (Tanya project / Resume state / Eksekusi task)          │
└─────────────────────────────────────────────────────────┘
```

**Prinsip Self-Heal:** Jika file hilang atau terhapus, auto-detect akan membuatnya ulang tanpa perlu intervensi user.

---

## 3. Apa yang Di-generate di Project User

Setelah setup selesai, power ini menghasilkan file-file berikut di project user:

### `.kiro/steering/` — Workspace Steering (3 files)

| File | Fungsi | Inclusion |
|------|--------|-----------|
| `architecture-standards.md` | Clean Architecture + DI rules (compact version) | always |
| `test-writing-patterns.md` | Test patterns + checklist (compact version) | always |
| `coding-conventions.md` | Commit, naming, lint rules | always |

File-file ini adalah **compact reference** yang di-generate (bukan copy dari power). Berisi rules dan checklist tanpa contoh code panjang. Contoh code lengkap tetap tersedia via power steering saat power aktif.

**Kenapa compact, bukan full copy:**
- AI Agent tidak bisa copy file antar workspace (power ≠ project)
- Compact version (~100-150 lines) lebih efisien untuk context window
- Full version tetap tersedia saat power aktif (loaded dari power steering)

File-file ini menjadi **acuan permanen** — aktif di setiap chat session tanpa perlu memanggil power lagi.

### `.kiro/hooks/` — Automation Hooks (8 files)

| File | Trigger | Agent |
|------|---------|-------|
| `architect-gate.json` | preTaskExecution | 🏗️ Architect + GitLab (issue → in-progress) |
| `code-quality-scan.json` | postToolUse (write) | 🔒 Security + 📊 Observability |
| `qa-devops-post-task.json` | postTaskExecution | 🧪 QA + 🚀 DevOps + GitLab (issue → review + comment) |
| `bug-learning-capture.json` | postTaskExecution | 📚 Learning |
| `metrics-collector.json` | postTaskExecution | 📊 Metrics |
| `sprint-retrospective.json` | userTriggered | 📚 Learning + GitLab (milestone close + wiki update) |
| `quality-scorecard.json` | userTriggered | 📊 Metrics |
| `health-check.json` | userTriggered | 🏗️ Architect |

### `docs/` — Project Documentation

```
docs/
├── CONTEXT-INDEX.md          ← Peta semua artifact
├── CURRENT-STATE.md          ← State terakhir (resume point)
├── product/
│   └── vision.md
├── requirements/
│   ├── extracted/
│   └── validation/
├── specs/
│   └── srs/
├── design/
│   ├── system/
│   ├── technical/
│   └── security/
├── adr/                      ← Architecture Decision Records
├── learnings/                ← Bug learnings
├── tech-debt/                ← Tech debt tracking
├── retrospectives/           ← Sprint retrospectives
└── quality/
    ├── metrics-log.jsonl     ← Raw metrics (append-only)
    ├── sprint-N-scorecard.md ← Quality scorecard
    └── health-check-*.md     ← Health check reports
```

---

## 4. Cara Hooks Bekerja di Runtime

Hooks adalah **JSON files** yang mendefinisikan: kapan trigger, apa yang dilakukan.

### Anatomy Hook

```json
{
  "name": "Nama Hook",
  "version": "1.0.0",
  "description": "Deskripsi singkat",
  "when": {
    "type": "preTaskExecution | postToolUse | postTaskExecution | userTriggered",
    "toolTypes": ["write"]  // hanya untuk postToolUse
  },
  "then": {
    "type": "askAgent",
    "prompt": "Instruksi untuk AI Agent..."
  }
}
```

### Lifecycle Hook Execution

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   TRIGGER    │────▶│   PROMPT     │────▶│ AGENT ACTION │
│   (Event)    │     │   (Loaded)   │     │  (Executed)  │
└──────────────┘     └──────────────┘     └──────────────┘

Contoh: postTaskExecution
  1. User menyelesaikan task (event fired)
  2. Kiro load prompt dari qa-devops-post-task.json
  3. AI Agent eksekusi: lint → typecheck → test → commit → push
```

### Hook Trigger Types

| Type | Kapan Fire | Contoh Hook |
|------|-----------|-------------|
| `preTaskExecution` | Sebelum task dimulai | architect-gate |
| `postToolUse` (write) | Setelah file ditulis | code-quality-scan |
| `postTaskExecution` | Setelah task selesai | qa-devops, metrics-collector, bug-learning |
| `userTriggered` | User trigger manual | retrospective, scorecard, health-check |

### Smart Filtering (code-quality-scan)

Tidak semua file write perlu di-scan. Hook `code-quality-scan` menggunakan smart filter:

```
File ditulis
    ↓
┌─ SKIP jika:
│  • docs/**/* (documentation)
│  • *.md, *.json, *.yml (config)
│  • tests/**/* (test files)
│  • .kiro/**/* (tooling)
│  • src/core/domains/*/entities/* (pure logic)
│  • src/infrastructure/di/* (registration)
│
└─ JALANKAN jika:
   • src/core/domains/*/usecases/* → cek observability
   • src/infrastructure/repositories/* → cek security + observability
   • src/infrastructure/networking/* → cek security
   • src/presentation/features/*/viewModel/* → cek error handling
   • src/app/**/* → cek security + observability
```

---

## 5. Multi-Agent System

Framework ini mengimplementasikan multi-agent melalui 3 mekanisme:

### Mekanisme 1: Role-Based Prompting

AI Agent berganti "topi" (role) sesuai fase task:

```
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│Architect│──▶│ Backend │──▶│   QA    │──▶│Security │──▶│ DevOps  │
│  Agent  │   │  Agent  │   │  Agent  │   │  Agent  │   │  Agent  │
└─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘
  validate      implement     test          review        push
```

Setiap role mengubah:
- **Perspektif** — apa yang diperhatikan
- **Output** — apa yang dihasilkan
- **Constraints** — apa yang tidak boleh dilakukan
- **Reference** — steering files mana yang dibaca

### Mekanisme 2: Sub-Agent Delegation

Task besar (> 5 files) didelegasikan ke sub-agent dengan context terisolasi:

```
Main Agent (Orchestrator)
    │
    ├──▶ Sub-Agent 1 (Backend) → implement core + infra
    │         └── return: list files created
    │
    ├──▶ Sub-Agent 2 (QA) → write tests
    │         └── return: test results + coverage
    │
    └──▶ Sub-Agent 3 (Security) → review
              └── return: findings
```

### Mekanisme 3: Hook Chains (Agent Handoff)

Hooks yang ter-chain mensimulasikan agent handoff otomatis:

```
[preTaskExecution] ─── Architect Agent ───┐
                                          ▼
[Implementation] ─── Backend/Frontend ────┐
                                          ▼
[postToolUse:write] ─── Security Agent ───┐
                                          ▼
[postTaskExecution] ─── QA Agent ─────────┐
                                          ▼
[postTaskExecution] ─── DevOps Agent ─────┐
                                          ▼
[postTaskExecution] ─── Learning Agent ───┐
                                          ▼
[postTaskExecution] ─── Metrics Agent ────┘
```

### 7 Agent Roles

| Agent | Emoji | Tanggung Jawab |
|-------|-------|----------------|
| BA Agent | 📋 | Requirement extraction, user story |
| Architect Agent | 🏗️ | Design, ADR, architecture compliance |
| Backend Agent | ⚙️ | Use case, repository, API implementation |
| Frontend Agent | 🎨 | Component, screen, ViewModel |
| QA Agent | 🧪 | Test writing, coverage analysis |
| Security Agent | 🔒 | Threat model, vulnerability check |
| DevOps Agent | 🚀 | Git, CI/CD, deployment |
| Learning Agent | 📚 | Bug learning, retrospective |

### Kapan Sub-Agent vs Self (Role Switch)

| Situasi | Gunakan | Alasan |
|---------|---------|--------|
| Task besar (> 5 files) | Sub-Agent | Isolasi context |
| Code review | Sub-Agent | Perspektif terisolasi |
| Task kecil (1-2 files) | Self (role switch) | Overhead sub-agent tidak worth it |
| Sequential dependency | Self | Sub-agent tidak bisa baca output lain |

---

## 6. Sistem Metrics

### Collection Flow

```
[Task selesai]
       │
       ▼
[metrics-collector hook fires]
       │
       ▼
[Collect data dari:]
  • git diff → files_created, files_modified
  • test runner → tests_written, tests_pass, tests_fail
  • coverage-summary.json → coverage_pct
  • lint output → lint_errors_before_fix
  • typecheck → type_errors_before_fix
  • git log → rework_count
  • docs/specs/ → spec_exists
  • docs/design/ → design_exists
       │
       ▼
[Append 1 JSON line ke docs/quality/metrics-log.jsonl]
```

### Format metrics-log.jsonl

```jsonl
{"timestamp":"2026-05-10T14:30:00Z","task":"implement payment use case","type":"feat","metrics":{"files_created":3,"files_modified":1,"tests_written":8,"tests_pass":8,"tests_fail":0,"coverage_pct":87,"lint_errors_before_fix":2,"type_errors_before_fix":0,"rework_count":0,"security_findings":0,"observability_added":true,"spec_exists":true,"design_exists":true},"agent_sequence":["architect","backend","qa","security","devops"]}
```

### Dari Metrics ke Insight

```
metrics-log.jsonl (raw data, append-only)
       │
       ├──▶ [User trigger: "scorecard"]
       │         └── quality-scorecard hook
       │              └── docs/quality/sprint-N-scorecard.md
       │
       └──▶ [User trigger: "retrospective"]
                 └── sprint-retrospective hook
                      └── docs/retrospectives/sprint-N.md
```

### Hallucination Proxy

Framework menggunakan **rework_count** sebagai proxy untuk mendeteksi hallucination:
- Task yang butuh rework > 1x = kemungkinan AI salah paham requirement
- Task tanpa spec (`spec_exists=false`) yang butuh rework = **strong indicator** hallucination
- Di-track per sprint untuk trend analysis

---

## 7. Context Accumulation (Project Memory)

### Dua File Kunci

```
┌─────────────────────────────────────────────────────────┐
│  docs/CONTEXT-INDEX.md                                   │
│  "Peta navigasi" — daftar semua artifact di project      │
│                                                          │
│  • Product docs                                          │
│  • Requirements                                          │
│  • Specifications                                        │
│  • Design documents                                      │
│  • ADRs                                                  │
│  • Learnings                                             │
│  • Tech debt                                             │
│  • Retrospectives                                        │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  docs/CURRENT-STATE.md                                   │
│  "Save point" — state terakhir project                   │
│                                                          │
│  • Active sprint                                         │
│  • Current task + status                                 │
│  • Last action + next action                             │
│  • Progress (tasks, coverage, last commit)               │
│  • Blockers                                              │
└─────────────────────────────────────────────────────────┘
```

### Lifecycle Context

```
[Session dimulai]
       │
       ▼
[architect-gate hook: baca CURRENT-STATE + CONTEXT-INDEX]
       │
       ▼
[AI tahu: sprint aktif, task terakhir, next action]
       │
       ▼
[Lanjutkan dari "Next Action" — bukan mulai dari awal]
       │
       ▼
[Task selesai]
       │
       ▼
[qa-devops hook: update CURRENT-STATE + CONTEXT-INDEX]
       │
       ▼
[Commit state files bersama task commit]
```

### Rules Context

1. **SELALU baca** CURRENT-STATE.md di awal session (resume point)
2. **SELALU update** CURRENT-STATE.md setelah task (save point)
3. **SELALU baca** CONTEXT-INDEX.md sebelum buat artifact baru (cegah duplikasi)
4. **SELALU update** CONTEXT-INDEX.md setelah buat artifact (keep fresh)
5. **Kedua file WAJIB di-commit** ke repository

---

## 8. Integrasi CI/CD

### Pipeline Structure

```
┌─────────────────────────────────────────────────────────┐
│                  .gitlab-ci.yml                           │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  stages:                                                 │
│    - validate    (lint + typecheck)                       │
│    - test        (unit + coverage check)                 │
│    - security    (dependency audit)                       │
│    - build       (compile/bundle)                         │
│    - deploy      (staging/production)                     │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Quality Gates di CI

| Gate | Threshold | Blocking? |
|------|-----------|-----------|
| Lint | 0 errors | ✅ Yes |
| Typecheck | 0 errors | ✅ Yes |
| Unit tests | All pass | ✅ Yes |
| Coverage | >= 80% | ✅ Yes |
| Security audit | 0 critical | ✅ Yes |

### Local vs CI Validation

```
LOCAL (sebelum push — via hooks):        CI (setelah push — via pipeline):
┌────────────────────────┐               ┌────────────────────────┐
│ 1. npm run lint        │               │ 1. npm run lint        │
│ 2. npm run typecheck   │               │ 2. npm run typecheck   │
│ 3. npm run test:unit   │               │ 3. npm run test:unit   │
│ 4. Coverage >= 80%     │               │ 4. Coverage check      │
│ 5. git commit + push   │               │ 5. Security audit      │
└────────────────────────┘               │ 6. Build               │
                                         │ 7. Deploy (if main)    │
                                         └────────────────────────┘
```

---

## 8.5. GitLab Project Management (Otomatis via Hooks)

Framework ini otomatis mengelola work items, issue board, milestone, dan wiki di GitLab:

### Issue Board Automation

```
[Task dimulai — architect-gate hook]
    │
    ├── update_issue: labels → status::in-progress
    │   (Issue berpindah dari "Ready" ke "In Progress" di board)
    │
[Task selesai — qa-devops-post-task hook]
    │
    ├── update_issue: labels → status::review
    │   (Issue berpindah dari "In Progress" ke "Review" di board)
    │
    ├── create_issue_note: implementation summary
    │   (Comment otomatis: branch, tests, coverage)
    │
[MR merged — GitLab native]
    │
    └── Issue auto-closed via "Closes #N" di commit message
        (Issue berpindah ke "Done" di board)
```

### Milestone Management (Sprint Retrospective Hook)

```
[User trigger: "sprint selesai" / "retrospective"]
    │
    ├── List semua issues di milestone
    │
    ├── Semua done?
    │   ├── YA → close milestone + update description dengan summary
    │   └── TIDAK → carry-over:
    │       ├── Buat milestone Sprint N+1
    │       ├── Move incomplete issues ke milestone baru
    │       └── Tambah note: "Carried over from Sprint N"
    │
    └── Update milestone description: sprint summary
```

### Wiki Auto-Update (Sprint Retrospective Hook)

| Wiki Page | Kapan Update | Konten |
|-----------|-------------|--------|
| `Changelog` | Setiap sprint selesai | Features delivered, issues closed |
| `Architecture-Decisions` | Saat ADR baru dibuat | Link ke ADR files |
| `API-Documentation` | Saat endpoint baru | Endpoint list, request/response |

### GitLab Tools yang Digunakan

| Tool (GitLab MCP) | Kapan Dipanggil | Oleh Hook |
|-------------------|----------------|-----------|
| `update_issue` | Task mulai + selesai | architect-gate, qa-devops |
| `create_issue_note` | Task selesai | qa-devops |
| `create_merge_request` | Sprint selesai | qa-devops |
| `edit_milestone` | Sprint selesai | sprint-retrospective |
| `create_or_update_wiki_page` | Sprint selesai | sprint-retrospective |
| `create_issue` | Improvement/debt found | sprint-retrospective, bug-learning |

### Issue Board Columns (Label Mapping)

```
Backlog → Ready → In Progress → Review → Done
   ↓         ↓         ↓           ↓        ↓
status::   status::  status::    status::  (closed)
backlog    ready     in-progress  review
```

AI Agent memindahkan issue antar kolom dengan mengubah label `status::*` via `update_issue`.

---

## 9. Data Flow: Dari Request ke Deployed Code

```
┌──────────────────────────────────────────────────────────────────────┐
│                     END-TO-END DATA FLOW                              │
└──────────────────────────────────────────────────────────────────────┘

[User Request]
    │ "Implement fitur payment top-up"
    ▼
[Auto-Detect] ─── cek hooks, steering, state
    │
    ▼
[🏗️ Architect Agent] ─── preTaskExecution hook
    │ • Baca CURRENT-STATE.md
    │ • Baca CONTEXT-INDEX.md
    │ • Validate: spec ada? design ada?
    │ • Jika belum → buat spec/design dulu
    ▼
[📋 BA Agent] (jika perlu)
    │ • Extract requirements
    │ • Buat user stories
    ▼
[⚙️ Backend Agent] ─── implementation
    │ • Buat entity, use case, repository
    │ • Inject observability (logger + metrics)
    │ • Register di DI container
    ▼
[🔒 Security Agent] ─── postToolUse hook (setiap file write)
    │ • Quick scan: validation, auth, injection
    │ • CRITICAL → stop | clean → continue
    ▼
[🧪 QA Agent] ─── postTaskExecution hook
    │ • Tulis unit tests
    │ • npm run lint → fix
    │ • npm run typecheck → fix
    │ • npm run test:unit --coverage
    │ • Coverage >= 80%?
    ▼
[🚀 DevOps Agent] ─── postTaskExecution hook
    │ • git add (relevant files)
    │ • git commit (conventional)
    │ • git push ke feature branch
    ▼
[📚 Learning Agent] ─── postTaskExecution hook (jika bug fix)
    │ • Capture bug learning
    ▼
[📊 Metrics Agent] ─── postTaskExecution hook
    │ • Collect metrics
    │ • Append ke metrics-log.jsonl
    ▼
[Update State]
    │ • Update CURRENT-STATE.md
    │ • Update CONTEXT-INDEX.md
    ▼
[GitLab CI Pipeline] ─── triggered by push
    │ • validate → test → security → build → deploy
    ▼
[✅ DEPLOYED]
```

---

## 10. File System Layout Setelah Setup

```
project-root/
│
├── .kiro/
│   ├── steering/
│   │   ├── architecture-standards.md    ← Clean Architecture rules (always)
│   │   ├── test-writing-patterns.md     ← Test patterns per-layer (always)
│   │   └── coding-conventions.md        ← Commit, naming, lint (always)
│   │
│   └── hooks/
│       ├── architect-gate.json          ← preTaskExecution
│       ├── code-quality-scan.json       ← postToolUse (write)
│       ├── qa-devops-post-task.json     ← postTaskExecution
│       ├── bug-learning-capture.json    ← postTaskExecution
│       ├── metrics-collector.json       ← postTaskExecution
│       ├── sprint-retrospective.json    ← userTriggered
│       ├── quality-scorecard.json       ← userTriggered
│       └── health-check.json           ← userTriggered
│
├── docs/
│   ├── CONTEXT-INDEX.md                 ← Peta semua artifact
│   ├── CURRENT-STATE.md                 ← Resume state
│   ├── product/
│   │   └── vision.md
│   ├── requirements/
│   │   ├── extracted/
│   │   └── validation/
│   ├── specs/
│   │   └── srs/
│   │       └── [feature]-spec.md
│   ├── design/
│   │   ├── system/
│   │   ├── technical/
│   │   └── security/
│   ├── adr/
│   │   └── ADR-[N]-[title].md
│   ├── learnings/
│   │   └── BUG-[N]-[title].md
│   ├── tech-debt/
│   │   └── TD-[N]-[title].md
│   ├── retrospectives/
│   │   └── sprint-[N].md
│   └── quality/
│       ├── metrics-log.jsonl            ← Raw metrics (append-only)
│       ├── sprint-[N]-scorecard.md      ← Quality scorecard
│       └── health-check-[date].md       ← Health reports
│
├── src/
│   ├── core/                            ← Domain layer (entities, use cases)
│   │   ├── domains/
│   │   │   └── [domain]/
│   │   │       ├── entities/
│   │   │       ├── repositories/        ← Interfaces/protocols
│   │   │       ├── usecases/
│   │   │       └── mappers/
│   │   └── protocols/                   ← Shared interfaces
│   │
│   ├── infrastructure/                  ← Implementation layer
│   │   ├── repositories/
│   │   ├── networking/
│   │   └── di/
│   │       └── registry/
│   │
│   ├── presentation/                    ← UI layer
│   │   └── features/
│   │       └── [feature]/
│   │           ├── components/
│   │           └── viewModel/
│   │
│   └── app/                             ← Entry point / routes
│
├── tests/
│   ├── unit/
│   │   ├── core/
│   │   ├── infrastructure/
│   │   └── presentation/
│   └── integration/
│
├── .gitlab/
│   └── ci/                              ← CI templates (opsional)
│
├── .gitlab-ci.yml                       ← CI/CD pipeline
├── .gitignore
├── package.json
├── tsconfig.json
├── vitest.config.ts
└── README.md
```

---

## Ringkasan Interaksi Komponen

```
┌─────────────────────────────────────────────────────────────────┐
│                    INTERACTION MAP                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Steering Files ──────▶ AI Agent Context (rules & patterns)      │
│                                                                  │
│  Hooks ───────────────▶ AI Agent Actions (automated triggers)    │
│                                                                  │
│  MCP Servers ─────────▶ External Systems (GitLab, Git)           │
│                                                                  │
│  docs/ ───────────────▶ Project Memory (state & artifacts)       │
│                                                                  │
│  metrics-log.jsonl ───▶ Quality Insights (scorecard & retro)     │
│                                                                  │
│  .gitlab-ci.yml ──────▶ CI/CD Pipeline (automated gates)         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

Semua komponen bekerja bersama membentuk **AI Operating Layer** yang mengubah AI dari sekadar code generator menjadi engineering partner yang terstruktur dan accountable.
