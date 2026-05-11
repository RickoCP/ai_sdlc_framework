---
inclusion: always
---

# Hooks Registry — Auto-Generated Hook Files

## Overview

Dokumen ini mendefinisikan **semua hook files** yang WAJIB di-generate di `.kiro/hooks/` project user. Hooks ini mengaktifkan automation Layer 9, 13, dan 14.

**Tanpa hooks ini, automation framework TIDAK akan tertrigger.**

---

## Hook Files (8 files)

| File | Trigger | Fungsi | Version |
|------|---------|--------|---------|
| `architect-gate.json` | preTaskExecution | Validate spec + design + update issue status::in-progress | 1.1.0 |
| `code-quality-scan.json` | postToolUse (write) | Security + Observability (merged, smart-filtered) | 2.0.0 |
| `qa-devops-post-task.json` | postTaskExecution | Lint + test + push + WAJIB update issue/board/milestone/wiki | 2.1.0 |
| `bug-learning-capture.json` | postTaskExecution | Capture learning saat bug fix | 1.0.0 |
| `metrics-collector.json` | postTaskExecution | Auto-collect AI quality metrics | 1.0.0 |
| `sprint-retrospective.json` | userTriggered | Generate retro + close milestone + update wiki + offer scorecard | 2.1.0 |
| `quality-scorecard.json` | userTriggered | Generate scorecard dari metrics | 1.1.0 |
| `health-check.json` | userTriggered | Framework compliance scan | 1.0.0 |

**Perubahan dari v1.4.0:**
- `security-review.json` + `observability-check.json` → **MERGED** menjadi `code-quality-scan.json` (v2.0.0) dengan smart filtering
- **NEW:** `metrics-collector.json` — auto-collect metrics setiap task ke `docs/quality/metrics-log.jsonl`
- `sprint-retrospective.json` dan `quality-scorecard.json` sekarang **data-driven** (baca dari metrics-log)

---

## Smart Filtering (Mengurangi Hook Overhead)

`code-quality-scan.json` menggunakan smart filtering agar TIDAK trigger di setiap file write:

**SKIP (tidak perlu scan):**
- `docs/**/*` — documentation
- `*.md` — markdown files
- `*.json`, `*.yml`, `*.yaml` — config files
- `tests/**/*` — test files
- `.kiro/**/*` — kiro config
- `.gitlab/**/*` — CI config
- `src/core/domains/*/entities/*` — pure entities
- `src/core/domains/*/mappers/*` — pure mappers
- `src/infrastructure/di/*` — DI registration only

**JALANKAN (perlu scan):**
- `src/core/domains/*/usecases/*` → observability check
- `src/infrastructure/repositories/*` → observability + security
- `src/infrastructure/networking/*` → security
- `src/presentation/features/*/viewModel/*` → error handling
- `src/app/**/*` (route handlers) → security + observability

---

## Hook File Contents

### 1. `architect-gate.json`

```json
{
  "name": "Architect Gate",
  "version": "1.1.0",
  "description": "Validate spec/design + update GitLab issue status sebelum mulai task",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: Architect Agent\n\nSebelum mulai task:\n1. Baca docs/CURRENT-STATE.md — resume dari session sebelumnya\n2. Baca docs/CONTEXT-INDEX.md — apa saja artifact yang tersedia\n3. Cek apakah spec ada untuk fitur ini (docs/specs/srs/)\n4. Cek apakah design document ada (docs/design/)\n5. Jika fitur kompleks dan spec/design belum ada → buat dulu, JANGAN langsung coding\n6. Jika sudah ada → validate bahwa task sesuai arsitektur\n7. Load relevant context (ADR, learnings) untuk menghindari kesalahan yang sama\n\n**GITLAB PROJECT MANAGEMENT:**\n8. Jika task terkait GitLab issue → update issue label: status::in-progress\n   Tool: update_issue (GitLab MCP)\n   Arguments: { project_id, issue_iid, labels: ['status::in-progress', ...existing labels] }\n9. Jika milestone aktif → pastikan issue ter-assign ke milestone\n\nInformasikan user: 'Task dimulai. Issue #[N] status: in-progress.'"
  }
}
```

### 2. `code-quality-scan.json` (MERGED: Security + Observability + Smart Filter)

```json
{
  "name": "Code Quality Scan",
  "version": "2.0.0",
  "description": "Security + Observability scan dengan smart filtering — skip non-source files",
  "when": {
    "type": "postToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "SMART FILTER — cek file yang baru ditulis:\n\n**SKIP jika file adalah:**\n- docs/, *.md, *.json, *.yml, *.yaml (config/docs)\n- tests/**/* (test files)\n- .kiro/**/* atau .gitlab/**/* (tooling config)\n- src/core/domains/*/entities/* atau */mappers/* (pure logic)\n- src/infrastructure/di/* (registration only)\n\n**JALANKAN jika file adalah source code di:**\n- src/core/domains/*/usecases/* → cek observability\n- src/infrastructure/repositories/* → cek observability + security\n- src/infrastructure/networking/* → cek security\n- src/presentation/features/*/viewModel/* → cek error handling\n- src/app/**/* → cek security + observability\n\n**SECURITY (🔒):**\n1. Input validation ada?\n2. Sensitive data TIDAK di-log?\n3. Auth check ada (jika protected)?\n4. Parameterized queries?\n5. CRITICAL finding → STOP\n\n**OBSERVABILITY (📊):**\n1. Structured logging di entry + error path?\n2. Metrics untuk operasi penting?\n3. Logger/metrics via DI injection?\n4. Jika belum → tambahkan\n\n**OUTPUT:** Hanya jika ada finding. Silent skip untuk clean/non-source files."
  }
}
```

### 3. `qa-devops-post-task.json`

```json
{
  "name": "QA + DevOps Post Task",
  "version": "2.1.0",
  "description": "Lint, typecheck, test, commit, push + WAJIB update GitLab (issue/board/milestone/wiki)",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: QA Agent → DevOps Agent\n\n**QA Phase:**\n1. npm run lint (jika error → fix dulu)\n2. npm run typecheck (jika error → fix dulu)\n3. npm run test:unit -- --coverage (semua harus pass, coverage >= 80%)\n4. Jika task docs-only → skip test, tetap lint+typecheck\n\n**DevOps Phase (hanya jika QA pass):**\n5. git add (relevant files only, BUKAN git add .)\n6. git commit (conventional format, sertakan 'Refs #[issue-number]' di footer)\n7. git push ke feature branch\n8. Update docs/CURRENT-STATE.md\n9. Update docs/CONTEXT-INDEX.md jika ada artifact baru\n\n**GITLAB PROJECT MANAGEMENT (WAJIB SETIAP TASK):**\n10. Update issue label: status::in-progress → status::review\n    Tool: update_issue { project_id, issue_iid, labels: ['status::review', ...] }\n11. Tambah comment di issue:\n    Tool: create_issue_note { project_id, issue_iid, body: '✅ Task complete.\\n- Branch: `[branch]`\\n- Commit: `[hash]`\\n- Tests: [N]/[N] pass\\n- Coverage: [X%]' }\n12. Jika milestone aktif → cek progress milestone (berapa issue done vs total)\n13. Jika ini task TERAKHIR dalam sprint:\n    a. Push semua perubahan\n    b. Create MR (feature → develop) — JANGAN auto-merge, WAJIB user approval di GitLab\n    c. Update semua sprint issues: status::review\n    d. Update milestone description: sprint progress\n    e. Update wiki 'Changelog': tambah sprint delivery entry\n    f. TAWARKAN Sprint Completion Package (retro + scorecard + health check)\n\n**RULES:**\n- SETIAP task yang terkait issue → WAJIB update issue status + comment\n- SETIAP push → WAJIB cek milestone progress\n- MERGE di GitLab HANYA oleh user (AI DILARANG merge)\n- Setelah sprint MR merged → WAJIB tawarkan retro/scorecard/health-check\n\nInformasikan user: Lint ✅/❌ | Typecheck ✅/❌ | Tests X/X | Coverage X% | Pushed to [branch] | Issue #[N]: review"
  }
}
```

### 4. `bug-learning-capture.json`

```json
{
  "name": "Bug Learning Capture",
  "version": "1.0.0",
  "description": "Capture learning setiap kali bug di-fix",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Cek apakah task yang baru selesai adalah BUG FIX (commit message 'fix:' atau issue type::bug).\n\nJika YA:\n1. Buat docs/learnings/BUG-[issue-number]-[short-title].md\n2. Isi: What Happened, Root Cause, Why Not Detected Earlier, Prevention actions\n3. Identifikasi: perlu update skill/test/CI?\n4. Informasikan user: 'Bug learning captured. Rekomendasi: [list]'\n\nJika BUKAN bug fix → skip."
  }
}
```

### 5. `metrics-collector.json` (NEW — Auto-Collect AI Quality Metrics)

```json
{
  "name": "AI Metrics Collector",
  "version": "1.0.0",
  "description": "Otomatis collect dan append metrics setiap task selesai",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: Metrics Agent\n\nSetelah task selesai, COLLECT dan APPEND metrics ke docs/quality/metrics-log.jsonl:\n\nData per task (1 JSON line):\n{\"timestamp\":\"ISO-8601\",\"task\":\"description\",\"type\":\"feat|fix|refactor|docs\",\"metrics\":{\"files_created\":N,\"files_modified\":N,\"tests_written\":N,\"tests_pass\":N,\"tests_fail\":N,\"coverage_pct\":N,\"lint_errors_before_fix\":N,\"type_errors_before_fix\":N,\"rework_count\":N,\"security_findings\":N,\"observability_added\":bool,\"spec_exists\":bool,\"design_exists\":bool},\"agent_sequence\":[\"list\"]}\n\nCara collect:\n1. files: dari git diff\n2. tests: dari test run output\n3. coverage: dari coverage-summary.json\n4. lint/type errors: berapa yang di-fix\n5. rework: cek git log untuk issue yang sama (>1 fix commit = rework)\n6. spec/design: cek docs/specs/ dan docs/design/\n\nAppend ke docs/quality/metrics-log.jsonl (JANGAN overwrite).\nJika file belum ada → buat baru.\nJangan output ke user — silent collection."
  }
}
```

### 6. `sprint-retrospective.json` (v2.1.0 — Data-Driven + GitLab + Offer Scorecard)

```json
{
  "name": "Sprint Retrospective",
  "version": "2.1.0",
  "description": "Generate retrospective + close milestone + update wiki + offer scorecard/health-check",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Generate sprint retrospective + full GitLab management:\n\n**PHASE 1 — Generate Retrospective:**\n1. Parse docs/quality/metrics-log.jsonl — filter entries untuk sprint ini\n2. Calculate: total tasks, rework rate, average coverage, spec compliance, observability compliance, security findings\n3. Generate docs/retrospectives/sprint-[N].md\n4. Buat GitLab issues untuk improvement actions (type::improvement)\n\n**PHASE 2 — GitLab Milestone Management (WAJIB):**\n5. List semua issues di milestone sprint ini\n6. Cek status setiap issue:\n   - Semua done? → close milestone\n   - Ada yang belum done? → carry-over:\n     a. Buat milestone baru (Sprint N+1) jika belum ada\n     b. Move carry-over issues ke milestone baru\n     c. Tambah note di issue: 'Carried over from Sprint [N]'\n7. Update milestone description dengan sprint summary\n\n**PHASE 3 — GitLab Wiki Update (WAJIB):**\n8. Update wiki page 'Changelog': Sprint [N] — features delivered, issues closed, carry-over\n9. Jika ada ADR baru sprint ini → update wiki page 'Architecture-Decisions'\n10. Jika ada endpoint baru → update wiki page 'API-Documentation'\n\n**PHASE 4 — Tawarkan Scorecard + Health Check:**\n11. Informasikan user:\n    'Retrospective selesai. Mau saya juga generate:\n    - 📊 Quality Scorecard (metrics objektif)\n    - 🏗️ Framework Health Check (compliance scan)\n    Atau skip?'\n\n**PHASE 5 — Update State:**\n12. Update docs/CURRENT-STATE.md: sprint = N+1, reset tasks\n13. Update docs/CONTEXT-INDEX.md: tambah retrospective file\n14. Commit state files"
  }
}
```

### 7. `quality-scorecard.json` (UPDATED — Data-Driven)

```json
{
  "name": "Quality Scorecard",
  "version": "1.1.0",
  "description": "Generate scorecard dari collected metrics",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Generate quality scorecard dari docs/quality/metrics-log.jsonl:\n\n1. Parse metrics — filter sprint aktif\n2. Calculate per kategori:\n   - Code Quality: avg coverage, total lint/type errors, security findings\n   - AI Effectiveness: tasks completed, rework rate (<20%), spec compliance (>=90%)\n   - Hallucination Indicators: tasks tanpa spec yang butuh rework\n3. Generate docs/quality/sprint-[N]-scorecard.md\n4. Compare dengan sprint sebelumnya (trend ↑/↓/→)\n5. Overall score: (metrics met / total) × 100%\n\nInformasikan user: score + trend + top 3 recommendations"
  }
}
```

### 8. `health-check.json`

```json
{
  "name": "Framework Health Check",
  "version": "1.0.0",
  "description": "Scan project compliance terhadap framework",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Scan project compliance:\n1. Structure: folder sesuai? (src/core, infrastructure, presentation, app)\n2. Architecture: ada import violation? (core → infra, presentation → infra)\n3. Testing: coverage >= 80%? Ada file tanpa test?\n4. Docs: CONTEXT-INDEX up-to-date? Spec outdated?\n5. CI/CD: .gitlab-ci.yml valid? ESLint config ada?\n6. Governance: conventional commits? Tech debt tracked?\n\nGenerate docs/quality/health-check-[date].md\nInformasikan user: overall health score + violations + recommendations"
  }
}
```

---

## Metrics Collection System

### Bagaimana Metrics Bekerja

```
[Task selesai]
    ↓
[metrics-collector hook] → append 1 line ke docs/quality/metrics-log.jsonl
    ↓
[Sprint selesai — user trigger "retrospective"]
    ↓
[sprint-retrospective hook] → baca metrics-log.jsonl → generate retro
    ↓
[User trigger "scorecard"]
    ↓
[quality-scorecard hook] → baca metrics-log.jsonl → generate scorecard
```

### Format: `docs/quality/metrics-log.jsonl`

```jsonl
{"timestamp":"2026-05-10T14:30:00Z","task":"implement payment use case","type":"feat","metrics":{"files_created":3,"files_modified":1,"tests_written":8,"tests_pass":8,"tests_fail":0,"coverage_pct":87,"lint_errors_before_fix":2,"type_errors_before_fix":0,"rework_count":0,"security_findings":0,"observability_added":true,"spec_exists":true,"design_exists":true},"agent_sequence":["architect","backend","qa","security","devops"]}
{"timestamp":"2026-05-10T16:45:00Z","task":"fix token expiry bug","type":"fix","metrics":{"files_created":1,"files_modified":2,"tests_written":3,"tests_pass":3,"tests_fail":0,"coverage_pct":89,"lint_errors_before_fix":0,"type_errors_before_fix":0,"rework_count":0,"security_findings":0,"observability_added":false,"spec_exists":false,"design_exists":false},"agent_sequence":["backend","qa","devops","learning"]}
```

### Metrics yang Di-track

| Metric | Sumber | Digunakan Untuk |
|--------|--------|----------------|
| `files_created/modified` | git diff | Productivity |
| `tests_written/pass/fail` | test runner output | Quality |
| `coverage_pct` | coverage-summary.json | Quality gate |
| `lint_errors_before_fix` | lint output | Code hygiene |
| `type_errors_before_fix` | typecheck output | Type safety |
| `rework_count` | git log (same issue, multiple fix commits) | AI effectiveness / hallucination proxy |
| `security_findings` | code-quality-scan output | Security posture |
| `observability_added` | code-quality-scan output | Observability compliance |
| `spec_exists` | docs/specs/ check | Spec-first compliance |
| `design_exists` | docs/design/ check | Design compliance |
| `agent_sequence` | agent banners used | Orchestration tracking |

### Hallucination Proxy

Framework ini menggunakan **rework_count** sebagai proxy untuk hallucination:
- Task yang butuh rework > 1x = kemungkinan AI salah paham requirement
- Task tanpa spec yang butuh rework = **strong indicator** hallucination
- Tracked per sprint untuk trend analysis

---

## Auto-Detect: Hook Completeness Check

Saat auto-detect jalan (awal session), cek 8 files ini:

```
.kiro/hooks/architect-gate.json       → harus ada
.kiro/hooks/code-quality-scan.json    → harus ada (menggantikan security-review + observability-check)
.kiro/hooks/qa-devops-post-task.json  → harus ada
.kiro/hooks/bug-learning-capture.json → harus ada
.kiro/hooks/metrics-collector.json    → harus ada
.kiro/hooks/sprint-retrospective.json → harus ada
.kiro/hooks/quality-scorecard.json    → harus ada
.kiro/hooks/health-check.json         → harus ada
```

Jika ada yang missing → generate. Jika ada versi lama (`security-review.json`, `observability-check.json`) → hapus dan ganti dengan `code-quality-scan.json`.
