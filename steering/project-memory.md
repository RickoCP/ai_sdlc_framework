---
inclusion: always
---

# Project Memory & Resume State

## Overview

Dokumen ini memastikan AI Agent **SELALU tahu state project** di awal setiap session — tanpa user harus menjelaskan ulang. Ini menyelesaikan masalah "context loss" antar session.

**Dua mekanisme utama:**
1. **Context Index** (`docs/CONTEXT-INDEX.md`) — Peta semua artifact yang tersedia
2. **Current State** (`docs/CURRENT-STATE.md`) — State terakhir project (sprint, task, branch)

Kedua file ini WAJIB di-maintain oleh AI Agent dan WAJIB dibaca di awal setiap task.

---

## 1. Context Index — Project Memory

### Tujuan

Context Index adalah **peta navigasi** untuk AI Agent. Berisi daftar semua artifact yang tersedia di project, sehingga AI tahu apa yang sudah ada dan tidak perlu membuat ulang.

### Template: `docs/CONTEXT-INDEX.md`

```markdown
# Context Index — [Project Name]

**Last Updated:** [YYYY-MM-DD]
**Updated By:** AI Agent / [Developer Name]

## Product
- [ ] docs/product/vision.md — [status: draft/approved]
- [ ] docs/product/roadmap.md — [status]

## Requirements
- [ ] docs/requirements/extracted/[file].md — [N user stories]
- [ ] docs/requirements/validation/[report].md — [status: passed/blocked]

## Specifications
- [ ] docs/specs/srs/[feature]-spec.md — [status: draft/approved/implemented]

## Design
- [ ] docs/design/system/[file].md
- [ ] docs/design/technical/[file].md
- [ ] docs/design/security/[file].md

## Architecture Decisions
- [ ] docs/adr/ADR-[N]-[title].md — [status: accepted/superseded]

## Learnings
- [ ] docs/learnings/BUG-[N]-[title].md
- [ ] docs/learnings/PERF-[N]-[title].md

## Tech Debt
- [ ] docs/tech-debt/TD-[N]-[title].md — [status: open/resolved]

## Retrospectives
- [ ] docs/retrospectives/sprint-[N].md
```

### Rules

1. **AI Agent WAJIB update Context Index** setiap kali artifact baru dibuat
2. **AI Agent WAJIB baca Context Index** di awal setiap task (sebelum coding)
3. **Context Index WAJIB di-commit** ke repository
4. **Jika Context Index belum ada** → AI Agent WAJIB buat saat pertama kali bekerja di project

---

## 2. Current State — Resume Capability

### Tujuan

Current State memungkinkan AI Agent **melanjutkan pekerjaan** setelah session terputus (Kiro crash, user close, context limit). AI tahu persis "terakhir sampai mana".

### Template: `docs/CURRENT-STATE.md`

```markdown
# Current State — [Project Name]

**Last Updated:** [YYYY-MM-DD HH:MM]

## Active Sprint
- **Sprint:** [N]
- **Milestone:** [GitLab milestone name]
- **Start:** [date]
- **Target End:** [date]

## Current Task
- **Issue:** #[N] — [title]
- **Branch:** feature/issue-[N]-[description]
- **Status:** [not-started / in-progress / blocked / review / done]
- **Last Action:** [apa yang terakhir dilakukan]
- **Next Action:** [apa yang harus dilakukan selanjutnya]

## Progress
- Tasks completed this sprint: [N/total]
- Coverage: [X%]
- Last commit: [hash] — [message]
- Last push: [branch] → [remote] at [time]

## Blockers (jika ada)
- [Blocker description + what's needed to unblock]

## Context Files Loaded
- [List file yang sudah dibaca di session ini]
```

### Kapan Update Current State

| Event | Update |
|-------|--------|
| Task dimulai | Update: current task, status = in-progress |
| Task selesai | Update: status = done, next action, progress |
| Push berhasil | Update: last commit, last push |
| Sprint selesai | Update: active sprint, reset tasks |
| Blocker ditemukan | Update: blockers section |
| Session dimulai | Baca file ini, lanjutkan dari last action |

### Rules

1. **AI Agent WAJIB update Current State** di setiap state change
2. **AI Agent WAJIB baca Current State** di awal setiap session
3. **Jika session terputus** → AI baca Current State dan lanjutkan dari "Next Action"
4. **Current State WAJIB di-commit** setiap kali di-update (sebagai bagian dari task commit)
5. **Jika Current State belum ada** → AI Agent WAJIB buat saat pertama kali bekerja

---

## 3. Kiro Hook: Session Start — Load Memory

```json
{
  "name": "Load Project Memory on Session Start",
  "version": "1.0.0",
  "description": "Otomatis load project memory dan resume state di awal setiap task",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "DI AWAL TASK, load project memory:\n\n**1. Baca docs/CURRENT-STATE.md (jika ada):**\n- Apa sprint aktif?\n- Apa task terakhir yang dikerjakan?\n- Apa next action yang harus dilakukan?\n- Apakah ada blocker?\n\n**2. Baca docs/CONTEXT-INDEX.md (jika ada):**\n- Apa saja artifact yang tersedia?\n- Spec mana yang relevan untuk task ini?\n- ADR mana yang perlu diperhatikan?\n- Learning mana yang relevan?\n\n**3. Jika file belum ada:**\n- Buat docs/CURRENT-STATE.md dengan template\n- Buat docs/CONTEXT-INDEX.md dengan scan folder docs/\n\n**4. Informasikan user:**\n'Project state loaded. Sprint [N], last task: [X], next action: [Y].'\n\n**5. Lanjutkan dari next action** — jangan mulai dari awal jika sudah ada progress."
  }
}
```

---

## 4. Kiro Hook: Session End — Save State

```json
{
  "name": "Save Project State on Task Completion",
  "version": "1.0.0",
  "description": "Otomatis save state setelah task selesai untuk resume di session berikutnya",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "SETELAH task selesai, UPDATE project state:\n\n**1. Update docs/CURRENT-STATE.md:**\n- Current task status → done\n- Update progress (tasks completed, coverage)\n- Update last commit dan last push\n- Set next action (task berikutnya atau 'sprint complete')\n- Clear blockers jika sudah resolved\n\n**2. Update docs/CONTEXT-INDEX.md:**\n- Jika ada artifact baru (spec, design, learning, ADR) → tambahkan ke index\n- Update status artifact yang berubah\n- Update 'Last Updated' timestamp\n\n**3. Commit state files:**\n- Stage: docs/CURRENT-STATE.md, docs/CONTEXT-INDEX.md\n- Commit: 'docs: update project state after [task description]'\n- Push bersama dengan task commit\n\n**JANGAN lupa update state** — ini adalah 'memory' project untuk session berikutnya."
  }
}
```

---

## 5. Integration dengan Layer Lain

| Layer | Bagaimana Project Memory Terintegrasi |
|-------|--------------------------------------|
| Layer 8 (Issue-Driven) | Current State track active issue dan branch |
| Layer 10 (Context) | Context Index = implementasi konkret dari context accumulation |
| Layer 12 (Quality Gates) | Progress track coverage dan test results |
| Layer 14 (Learning) | Learnings otomatis masuk ke Context Index |

---

## 6. AI Agent Rules

1. **SELALU baca CURRENT-STATE.md di awal session** — ini adalah "resume point"
2. **SELALU update CURRENT-STATE.md setelah task** — ini adalah "save point"
3. **SELALU baca CONTEXT-INDEX.md sebelum membuat artifact baru** — cegah duplikasi
4. **SELALU update CONTEXT-INDEX.md setelah membuat artifact** — keep index fresh
5. **Kedua file WAJIB di-commit** — jangan hanya lokal
6. **Jika file belum ada → buat** — jangan skip karena belum ada
7. **Jika session terputus → baca Current State → lanjutkan dari Next Action**
