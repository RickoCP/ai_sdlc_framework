---
inclusion: always
---

# Hooks Generator — Enterprise AI-SDLC

## Overview

Steering ini berisi **blueprint lengkap** untuk men-generate semua agent hooks yang digunakan di project. Gunakan dokumen ini untuk mereproduksi seluruh hook system di project baru atau untuk restore hooks yang hilang.

## Hook Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    HOOK LIFECYCLE                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  promptSubmit ──→ [spec-context-loader]                      │
│                                                              │
│  preTaskExecution ──→ [pre-task-pipeline]                    │
│                                                              │
│  postTaskExecution ──→ [post-task-pipeline]                  │
│                       ──→ [sprint-completion]                │
│                                                              │
│  userTriggered ──→ [health-check]                           │
│                 ──→ [quality-scorecard]                      │
│                 ──→ [sprint-retrospective]                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Total: 7 Hooks

| # | ID | Event | Kondisi Aktif |
|---|-----|-------|---------------|
| 1 | spec-context-loader | promptSubmit | Saat user minta buat spec (requirements/design/tasks) |
| 2 | pre-task-pipeline | preTaskExecution | Setiap task dimulai |
| 3 | post-task-pipeline | postTaskExecution | Setiap task selesai |
| 4 | sprint-completion | postTaskExecution | Bug fix task + task terakhir sprint |
| 6 | health-check | userTriggered | Manual |
| 7 | quality-scorecard | userTriggered | Manual |
| 8 | sprint-retrospective | userTriggered | Manual |

---

## Hook 1: Spec Context Loader

**File:** `.kiro/hooks/spec-context-loader.kiro.hook`

```json
{
  "enabled": true,
  "name": "Spec Context Loader",
  "version": "1.0.0",
  "description": "Unified context loader untuk spec creation — auto-detect phase (requirements/design/tasks) dan load dokumen referensi yang sesuai",
  "when": {
    "type": "promptSubmit"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Detect apakah user sedang meminta membuat spec document. Cek kata kunci di pesan user dan tentukan PHASE mana yang diminta:\n\n━━━ PHASE DETECTION ━━━\n- REQUIREMENTS: kata kunci 'spec', 'requirements', 'buat requirements', 'create spec', 'generate requirements'\n- DESIGN: kata kunci 'design', 'buat design', 'generate design', 'create design', 'design.md', 'technical design'\n- TASKS: kata kunci 'tasks', 'task list', 'buat tasks', 'generate tasks', 'tasks.md', 'implementation plan'\n\nJika TIDAK match phase manapun → skip (tidak perlu output).\n\n═══════════════════════════════════════════\nJika REQUIREMENTS PHASE → load 3 layers (11 documents):\n═══════════════════════════════════════════\n\nLAYER 1 — EXISTING REQUIREMENTS (basis utama):\n- docs/requirements/extracted/functional-requirements.md\n- docs/requirements/extracted/non-functional-requirements.md\n- docs/requirements/extracted/user-stories.md\n\nLAYER 2 — BUSINESS CONTEXT (why + business rules):\n- docs/product/vision.md\n- docs/product/business-goals.md\n- docs/specs/brd/business-flow.md\n- docs/specs/brd/stakeholder-matrix.md\n\nLAYER 3 — TECHNICAL DETAILS (precise acceptance criteria):\n- docs/specs/srs/[project]-spec.md\n- docs/specs/srs/data-model.md\n- docs/specs/srs/state-flow.md\n- docs/specs/srs/failure-scenario.md\n\nTampilkan: \"📚 Requirements context loaded (3 layers, 11 documents)\"\n\n═══════════════════════════════════════════\nJika DESIGN PHASE → load 4 layers (17 documents):\n═══════════════════════════════════════════\n\nLAYER 1 — REQUIREMENTS SPEC (apa yang harus di-design):\n- .kiro/specs/[feature-name]/requirements.md\n\nLAYER 2 — SRS DOCS (keputusan teknis yang sudah ada):\n- docs/specs/srs/architecture.md\n- docs/specs/srs/api-contract.md\n- docs/specs/srs/data-model.md\n- docs/specs/srs/[project]-spec.md\n- docs/specs/srs/sequence-diagram.md\n- docs/specs/srs/state-flow.md\n- docs/specs/srs/failure-scenario.md\n\nLAYER 3 — STEERING FILES (aturan yang harus dipatuhi):\n- .kiro/steering/architecture-standards.md\n- .kiro/steering/coding-conventions.md\n- .kiro/steering/test-writing-patterns.md\n\nLAYER 4 — DESIGN DOCS (UI/UX decisions):\n- docs/design/technical/folder-structure.md\n- docs/design/technical/error-handling.md\n- docs/design/technical/naming-convention.md\n- docs/design/ui-ux/component-library.md\n- docs/design/ui-ux/design-token.md\n- docs/design/ui-ux/wireframe.md\n\nTampilkan: \"📐 Design context loaded (4 layers, 17 documents)\"\n\n═══════════════════════════════════════════\nJika TASKS PHASE → load 3 layers (12 documents):\n═══════════════════════════════════════════\n\nLAYER 1 — SPEC FILES (APA yang harus diimplementasi):\n- .kiro/specs/[feature-name]/requirements.md\n- .kiro/specs/[feature-name]/design.md\n\nLAYER 2 — STEERING FILES (BAGAIMANA mengimplementasi):\n- .kiro/steering/architecture-standards.md\n- .kiro/steering/coding-conventions.md\n- .kiro/steering/test-writing-patterns.md\n\nLAYER 3 — SRS + DESIGN DOCS (DI MANA dan DETAIL):\n- docs/design/technical/folder-structure.md\n- docs/specs/srs/api-contract.md\n- docs/specs/srs/data-model.md\n- docs/specs/srs/[project]-spec.md\n- docs/specs/srs/failure-scenario.md\n- docs/specs/srs/state-flow.md\n- docs/design/ui-ux/component-library.md\n\nTampilkan: \"📋 Tasks context loaded (3 layers, 12 documents)\"\n\nGunakan semua dokumen yang di-load sebagai referensi untuk pembuatan spec document."
  }
}
```

---

## Hook 2: Pre-Task Pipeline

**File:** `.kiro/hooks/pre-task-pipeline.kiro.hook`

```json
{
  "enabled": true,
  "name": "Pre-Task Pipeline",
  "version": "2.3.0",
  "description": "CURRENT-STATE check + Architect gate + GitLab issue create + Branch + Milestone + Wiki sprint plan (task pertama) — unified pre-task hook",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "SEBELUM mulai coding task ini, jalankan Pre-Task Pipeline:\n\n⚠️ FORMATTING RULE: Saat membuat/update issue, wiki, MR, atau milestone di GitLab — SELALU gunakan proper multiline markdown. JANGAN gunakan escaped newlines (\\\\n). Tulis description sebagai multiline string dengan heading (##), bullet points (-), dan checklist (- [ ]).\n\n━━━ PHASE 0: CURRENT-STATE UPDATE ━━━\n1. Baca docs/CURRENT-STATE.md\n2. Update field 'Current Task' dengan task yang akan dikerjakan sekarang\n3. Update status task di tabel Sprint Issues: task ini → 'In Progress'\n4. Simpan perubahan ke docs/CURRENT-STATE.md\n5. Tampilkan: \"📍 CURRENT-STATE updated: task [title] → In Progress\"\n\n━━━ PHASE 1: ARCHITECT GATE ━━━\n6. Cek apakah spec/design sudah ada untuk fitur ini (.kiro/specs/)\n7. Jika belum ada → STOP, buat spec dulu\n8. Jika sudah ada → load spec + design + wireframe + requirements sebagai context\n9. Validate: apakah task ini sesuai dengan architecture standards?\n10. Tampilkan banner: ━━━ 🏗️ ARCHITECT GATE PASSED ━━━\n\n━━━ PHASE 2: GITLAB ISSUE ━━━\n11. Baca task title dan description dari context\n12. Cari issue di GitLab project yang match dengan task title\n13. Jika SUDAH ADA issue → catat issue IID, lanjut tanpa buat baru\n14. Jika BELUM ADA → buat issue baru di GitLab:\n    - Title: \"[Sprint N] {task title}\"\n    - Description: WAJIB proper markdown\n    - Labels: [\"status::in-progress\", \"sprint::N\"]\n15. Tampilkan: \"📋 GitLab Issue #{IID} — {title}\"\n\n━━━ PHASE 3: SPRINT KICKOFF (task pertama saja) ━━━\n16. Cek apakah ini TASK PERTAMA di sprint\n17. Jika TASK PERTAMA:\n    a. GIT BRANCH: Buat branch baru dari develop (atau main di Solo Mode)\n    b. GITLAB MILESTONE: Buat milestone 'Sprint [N]' jika belum ada\n    c. WIKI SPRINT PLAN: Create/update wiki page 'Sprint-[N]-Plan'\n    d. Tampilkan: \"📖 Wiki Sprint Plan + 🏁 Milestone created\"\n18. Jika BUKAN task pertama:\n    - Checkout ke sprint branch jika belum\n    - Assign issue ke milestone existing\n\nBaru lanjut ke implementation setelah semua phase selesai."
  }
}
```

---

---

## Hook 3: Post-Task Pipeline

**File:** `.kiro/hooks/post-task-pipeline.kiro.hook`

```json
{
  "enabled": true,
  "name": "Post-Task Pipeline",
  "version": "2.1.0",
  "description": "QA (lint/test/typecheck) + git push + close GitLab issue + collect metrics + update CURRENT-STATE — unified post-task hook",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Task selesai! Jalankan Post-Task Pipeline:\n\n⚠️ FORMATTING RULE: Saat membuat/update issue, wiki, MR, atau milestone di GitLab — SELALU gunakan proper multiline markdown. JANGAN gunakan escaped newlines (\\\\n).\n\n━━━ PHASE 1: QA VALIDATION ━━━\n1. npm run lint (HARUS 0 errors)\n2. npm run typecheck (HARUS 0 errors)\n3. npm run test:unit -- --coverage (HARUS >= 80%)\n4. Jika GAGAL → FIX DULU, jangan lanjut ke phase berikutnya\n\n━━━ PHASE 2: GIT PUSH ━━━\n5. git add (stage specific files yang diubah)\n6. git commit (conventional commits format)\n7. git push ke feature branch\n\n━━━ PHASE 3: GITLAB ISSUE UPDATE ━━━\n8. Cari issue di GitLab yang match dengan task yang baru selesai\n9. Jika DITEMUKAN issue:\n   - Update labels: hapus 'status::in-progress', tambah 'status::done'\n   - Tambah comment (proper markdown): implementation summary\n   - Close issue (state_event: 'close')\n10. Jika TIDAK ditemukan → skip\n\n━━━ PHASE 4: METRICS COLLECTION ━━━\n11. Collect metrics dan APPEND ke docs/quality/metrics-log.jsonl\n12. Jika file belum ada → buat dulu\n\n━━━ PHASE 5: UPDATE CURRENT-STATE ━━━\n13. Update status task di CURRENT-STATE.md → 'Done'\n14. Update 'Current Task' ke task berikutnya"
  }
}
```

---

## Hook 4: Sprint Completion

**File:** `.kiro/hooks/sprint-completion.kiro.hook`

```json
{
  "enabled": true,
  "name": "Sprint Completion",
  "version": "2.1.0",
  "description": "Bug learning capture (setiap bug fix) + Sprint end health check + close milestone + Wiki summary (task terakhir)",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Jalankan Sprint Completion checks (2 conditional branches):\n\n⚠️ FORMATTING RULE: Saat membuat/update content di GitLab — SELALU gunakan proper multiline markdown. JANGAN gunakan escaped newlines (\\\\n).\n\n━━━ BRANCH A: BUG LEARNING (setiap bug fix task) ━━━\n1. Cek apakah task yang baru selesai adalah BUG FIX\n2. Jika YA → capture learning ke docs/learnings/bug-[issue-number].md\n3. Jika BUKAN bug fix → skip Branch A\n\n━━━ BRANCH B: SPRINT END (task terakhir saja) ━━━\n4. Baca docs/CURRENT-STATE.md → cek apakah SEMUA task done\n5. Jika BUKAN task terakhir → skip Branch B\n6. Jika ini TASK TERAKHIR:\n   a. HEALTH CHECK: tests pass? coverage >= 80%? issues closed? npm audit?\n   b. CLOSE MILESTONE: close milestone 'Sprint [N]' di GitLab\n   c. WIKI SPRINT SUMMARY: create/update wiki page + update Changelog\n   d. INFORM USER: tawarkan retro + scorecard + health check"
  }
}
```

---

## Hook 5: Framework Health Check

**File:** `.kiro/hooks/health-check.kiro.hook`

```json
{
  "enabled": true,
  "name": "Framework Health Check",
  "version": "1.0.0",
  "description": "Framework compliance scan",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "━━━ 🏗️ ARCHITECT AGENT ━━━\nJalankan Framework Health Check:\n\n1. Architecture Compliance: Clean Architecture layers respected? DI consistent?\n2. Documentation: Semua required docs ada? CURRENT-STATE up to date?\n3. Quality: Test coverage >= 80%? Lint clean? Typecheck clean?\n4. GitLab Sync: Issues status match? Milestone up to date? Wiki current?\n5. Generate report: docs/quality/health-check-[date].md\n6. Score: Healthy / Warning / Critical"
  }
}
```

---

## Hook 6: Quality Scorecard

**File:** `.kiro/hooks/quality-scorecard.kiro.hook`

```json
{
  "enabled": true,
  "name": "Quality Scorecard",
  "version": "1.5.0",
  "description": "Generate scorecard dari metrics-log",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "━━━ 🧪 QA AGENT ━━━\nGenerate Quality Scorecard (data-driven):\n\n1. Baca docs/quality/metrics-log.jsonl untuk sprint aktif\n2. Calculate scores (0-100): Code Quality, Test Coverage, Architecture Compliance, Security, Delivery, Rework Rate\n3. Generate docs/quality/sprint-[N]-scorecard.md\n4. Overall Score = weighted average\n5. Grade: A (90+), B (80+), C (70+), D (60+), F (<60)\n6. Recommendations berdasarkan lowest scores"
  }
}
```

---

## Hook 7: Sprint Retrospective

**File:** `.kiro/hooks/sprint-retrospective.kiro.hook`

```json
{
  "enabled": true,
  "name": "Sprint Retrospective",
  "version": "1.5.0",
  "description": "Generate retrospective data-driven dari metrics",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "━━━ 📚 LEARNING AGENT ━━━\nGenerate Sprint Retrospective (data-driven):\n\n1. Baca docs/quality/metrics-log.jsonl untuk sprint aktif\n2. Hitung: total tasks, avg coverage, total rework, duration\n3. Generate docs/retrospectives/sprint-[N].md:\n   - Sprint Summary (dates, goals, delivery)\n   - Metrics Overview\n   - What Went Well\n   - What Didn't Go Well\n   - Action Items\n   - Learnings\n4. Update GitLab wiki 'Changelog' (proper markdown)\n5. Informasikan user hasilnya"
  }
}
```

---

## Cara Menggunakan Steering Ini

### Generate Semua Hooks

Untuk men-generate semua 8 hooks di project baru:

1. Buat folder `.kiro/hooks/` jika belum ada
2. Copy-paste setiap JSON block di atas ke file yang sesuai
3. Atau minta agent: "Generate semua hooks dari steering hooks-generator"

### Customization Points

| Parameter | Default | Ubah Jika |
|-----------|---------|-----------|
| Branch naming | `feature/sprint-[N]-[project]` | Project name berbeda |
| Sprint labels | `sprint::1`, `status::in-progress` | Label scheme berbeda |
| Coverage threshold | 80% | Target berbeda |
| Metrics file | `docs/quality/metrics-log.jsonl` | Path berbeda |
| Wiki page names | `Sprint-[N]-Plan`, `Sprint-[N]-Summary` | Naming convention berbeda |

### Dependencies

Hooks ini mengasumsikan:
- GitLab MCP server terkonfigurasi (untuk issue/milestone/wiki operations)
- `npm run lint`, `npm run typecheck`, `npm run test:unit` tersedia di package.json
- `docs/CURRENT-STATE.md` ada dan mengikuti format yang diharapkan
- Git remote sudah dikonfigurasi
