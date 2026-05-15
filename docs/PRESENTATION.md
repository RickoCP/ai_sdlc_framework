# Enterprise AI-Native SDLC Framework
## Materi Presentasi — Slide by Slide

**Version:** 1.5.0
**Format:** Slide-by-slide content untuk PowerPoint / Google Slides
**Bahasa:** Indonesia

---

## ═══════════════════════════════════════
## OPENING (Slides 1-3)
## ═══════════════════════════════════════

---

## Slide 1 — Title Slide

### Enterprise AI-Native SDLC Framework

**Subtitle:** Discipline at Scale — Making AI-Generated Code Enterprise-Ready

- Mengubah AI dari code generator menjadi AI Operating Layer
- Spec-Driven Development + AI Governance + Multi-Agent Orchestration
- Built with Kiro IDE

**Visual:** Logo framework + tagline di center, background gelap profesional

---

## Slide 2 — Problem Statement: AI Chaos & Pilot Graveyard

### Masalah: AI Chaos di Enterprise

| Tanpa Framework | Dampak |
|----------------|--------|
| AI generate code tanpa arsitektur | Technical debt menumpuk |
| Tidak ada specification | AI hallucinate requirements |
| Tidak ada governance | Code quality tidak konsisten |
| Context hilang antar session | AI mulai dari nol setiap kali |
| Tidak ada quality gates | Bug masuk production |
| Full-app generation | Hallucination tinggi |
| Tidak ada learning loop | Kesalahan berulang |

**Key Message:** 70% AI pilot projects gagal masuk production — "Pilot Graveyard"

**Visual:** Ikon chaos (spaghetti code, error screens, frustrated developers)

---

## Slide 3 — Solution Overview: AI as Operating Layer

### Solusi: AI Sebagai Operating Layer

```
Traditional:  Human → Code → Deploy (AI = tool)

AI-Native:    Human + AI Agents
              → Specifications → Validation → Architecture
              → Tasks → Code → Testing
              → Observability → Continuous Learning
```

**Poin Utama:**
- AI bukan sekadar code generator
- AI menjadi **operating layer** untuk seluruh SDLC
- Dari chaos → engineering acceleration
- Dari pilot graveyard → production-ready at scale

**Visual:** Diagram transformasi: "AI as Tool" → "AI as Operating Layer"

---


## ═══════════════════════════════════════
## CORE CONCEPT (Slides 4-8)
## ═══════════════════════════════════════

---

## Slide 4 — Paradigm Shift: Traditional vs AI-Native SDLC

### Pergeseran Paradigma

| Aspek | Traditional SDLC | AI-Native SDLC |
|-------|-----------------|----------------|
| Peran AI | Code completion tool | Operating layer |
| Input | Instruksi ambigu | Specification terstruktur |
| Arsitektur | Opsional | Enforced (Clean Architecture) |
| Quality | Manual review | Automated gates |
| Context | Hilang tiap session | Persistent & accumulated |
| Learning | Ad-hoc | Systematic (per sprint) |
| Governance | Tidak ada | Policy-driven |

**Key Insight:**
> "AI bukan pengganti software engineering. AI membutuhkan software engineering discipline yang jauh lebih kuat."

**Visual:** Side-by-side comparison diagram dengan panah transformasi

---

## Slide 5 — 5 Key Principles

### 5 Prinsip Utama Framework

1. **🔍 Validate Before Execute**
   - AI wajib validasi sebelum coding
   - Requirement validation, spec review, architecture check

2. **📋 Specification First**
   - AI bekerja lebih baik dengan spec lengkap
   - Idea → Spec → Validation → Architecture → Tasks → Code

3. **🏛️ Governance Over Chaos**
   - AI tanpa governance = technical debt at scale
   - Policy, standards, compliance enforced

4. **🧠 Context Is Everything**
   - AI butuh context terstruktur dan reusable
   - Project Memory, Context Index, Current State

5. **🎯 Small Context Wins**
   - Issue-driven lebih stabil dari full-app generation
   - 1 issue = 1 bounded context = less hallucination

**Visual:** 5 ikon dalam pentagon/circle layout

---

## Slide 6 — 15 Layer Framework (Overview)

### 15 Layer — Saling Terhubung

```
┌─────────────────────────────────────────────────────┐
│  SETUP LAYERS (0-8) — Sequential, One-time          │
├─────────────────────────────────────────────────────┤
│  0. Product Vision          5. AI Governance        │
│  1. Requirement Intake      6. AI Engineering Skills│
│  2. Requirement Validation  7. Team Extension       │
│  3. Spec-Driven Dev         8. Issue-Driven Dev     │
│  4. Design System                                   │
├─────────────────────────────────────────────────────┤
│  RUNTIME LAYERS (9-14) — Always Active via Hooks    │
├─────────────────────────────────────────────────────┤
│  9.  Agent Orchestration   12. Quality Gates        │
│  10. Continuous Context    13. Observability        │
│  11. AI Review             14. Continuous Learning  │
└─────────────────────────────────────────────────────┘
```

**Key:** 13 dari 15 layer terintegrasi ke workflow (9 fully automated + 4 partial). Hanya Layer 0 dan 1 yang sepenuhnya manual.

**Visual:** Tower/pyramid diagram dengan 15 layer berwarna berbeda

---

## Slide 7 — Layer Categories: Setup vs Runtime

### Setup Layers (0-8) — Foundation

- Dijalankan **sequential** di awal project
- **DILARANG skip** — setiap layer menghasilkan artifact untuk layer berikutnya
- Output: 70+ dokumen mandatory
- Durasi: 1x setup, benefit sepanjang project

### Runtime Layers (9-14) — Always Active

- Aktif **otomatis** via hooks di setiap sprint
- Tidak perlu trigger manual
- Berjalan di background setiap task execution

| Layer | Automation Status |
|-------|------------------|
| 0-1 | MANUAL (keputusan bisnis) |
| 2-4 | AUTOMATED (hook-triggered) |
| 5-7 | PARTIAL (CI/CD + fileMatch) |
| 8 | AUTOMATED (full GitLab integration) |
| 9-14 | AUTOMATED (hooks + CI/CD) |

**Visual:** Timeline diagram: Setup phase → Sprint phase (runtime always on)

---

## Slide 8 — Context Accumulation Concept

### Continuous Context — AI Tidak Pernah Lupa

```
Layer 0 output → menjadi context untuk Layer 1
Layer 1 output → menjadi context untuk Layer 2
...
Layer 8 output → menjadi context untuk Sprint execution
Sprint N output → menjadi context untuk Sprint N+1
```

**Mekanisme:**
- `docs/CONTEXT-INDEX.md` — Peta semua artifact yang tersedia
- `docs/CURRENT-STATE.md` — State terakhir project (resume point)
- Project Memory — AI baca state sebelum mulai kerja
- ADR (Architecture Decision Records) — Keputusan tidak hilang

**Hasil:**
- Session terputus? AI resume dari titik terakhir
- Developer baru? Baca context, langsung produktif
- Sprint baru? AI tahu history dan learnings

**Visual:** Snowball effect diagram — context makin besar dan kaya setiap sprint

---


## ═══════════════════════════════════════
## ARCHITECTURE (Slides 9-13)
## ═══════════════════════════════════════

---

## Slide 9 — Clean Architecture + Dependency Injection (Awilix)

### Arsitektur Enterprise-Grade yang Enforced

```
┌─────────────────────────────────────────┐
│         PRESENTATION LAYER              │
│   (Controllers, ViewModels, Routes)     │
├─────────────────────────────────────────┤
│         APPLICATION LAYER               │
│   (Use Cases, DTOs, Interfaces)         │
├─────────────────────────────────────────┤
│           DOMAIN LAYER                  │
│   (Entities, Value Objects, Events)     │
├─────────────────────────────────────────┤
│       INFRASTRUCTURE LAYER              │
│   (Repositories, External Services)     │
└─────────────────────────────────────────┘
```

**Dependency Injection: Awilix**
- Container-based DI untuk TypeScript/Node.js
- Auto-resolve dependencies
- Testable — mock injection mudah
- Go: Manual constructor injection
- Python: dependency-injector / FastAPI Depends()

**Visual:** Concentric circles (Clean Architecture) + Awilix logo

---

## Slide 10 — Dependency Rule

### The Dependency Rule — Inner Layer TIDAK BOLEH Depend ke Outer

```
        ┌───────────────────┐
        │      Domain       │  ← Tidak depend ke siapapun
        │   (Entities)      │
        ├───────────────────┤
        │   Application     │  ← Hanya depend ke Domain
        │   (Use Cases)     │
        ├───────────────────┤
        │  Infrastructure   │  ← Depend ke Application & Domain
        │  (Repositories)   │
        ├───────────────────┤
        │   Presentation    │  ← Depend ke Application
        │   (Controllers)   │
        └───────────────────┘

        Dependency Direction: OUTSIDE → INSIDE (never reverse)
```

**Rules yang Enforced:**
- Domain layer: ZERO external dependencies
- Use Case: hanya import dari domain + interface
- Repository: implement interface dari application
- Controller: hanya panggil use case via DI container
- Impact analysis otomatis saat `core/` berubah

**Visual:** Arrows pointing inward only, red X on outward arrows

---

## Slide 11 — Multi-Agent System (7 Agents)

### 7 AI Agents — Masing-masing Punya Role

| Agent | Emoji | Tanggung Jawab |
|-------|-------|----------------|
| Architect Agent | 🏗️ | Architecture decisions, spec validation, impact analysis |
| Backend Agent | ⚙️ | Use cases, repositories, APIs, business logic |
| Frontend Agent | 🎨 | Components, pages, UI/UX implementation |
| QA Agent | 🧪 | Unit tests, integration tests, E2E, coverage |
| Security Agent | 🔒 | Threat model, vulnerability scan, secure coding |
| DevOps Agent | 🚀 | CI/CD, deployment, infrastructure, pipeline |
| BA Agent | 📋 | Requirements, specs, user stories, validation |
| Learning Agent | 📚 | Bug capture, retrospective, ADR, tech debt |

**Per-Task Agent Chain:**
```
🏗️ Architect → ⚙️ Backend → 🧪 QA → 🔒 Security → 🚀 DevOps
```

**Visual:** Agent icons in workflow chain, each with distinct color

---

## Slide 12 — Hook-Based Automation (9 Hooks)

### 9 Hooks — Otomatis Tanpa Manual Trigger

| Hook | Trigger | Fungsi |
|------|---------|--------|
| `architect-gate` | preTaskExecution | Validasi arsitektur + load context |
| `code-quality-scan` | postToolUse (write) | Security scan + observability check |
| `qa-devops-post-task` | postTaskExecution | Test + push + issue update |
| `bug-learning-capture` | postTaskExecution | Capture learning dari bug fix |
| `metrics-collector` | postTaskExecution | Collect metrics ke JSONL |
| `sprint-end-auto-check` | postTaskExecution | Compliance + sprint completion offer |
| `sprint-retrospective` | userTriggered | Generate retro + close milestone |
| `quality-scorecard` | userTriggered | Generate scorecard dari metrics |
| `health-check` | userTriggered | Framework compliance scan |

**Hook Lifecycle:**
```
preTaskExecution → [TASK EXECUTION] → postToolUse → postTaskExecution
                                                          ↓
                                              userTriggered (on-demand)
```

**Visual:** Timeline/pipeline diagram showing hook trigger points

---

## Slide 13 — Self-Healing Mechanism

### Auto-Detect & Self-Heal — Framework yang Memperbaiki Dirinya Sendiri

**Di Awal SETIAP Session, AI Agent Otomatis:**

```
[Session dimulai]
    ↓
[AUTO-DETECT 1] Project existing? → Cek .kiro/, docs/, src/
    ↓
[AUTO-DETECT 2] Hooks lengkap? → Jika kurang → GENERATE otomatis
    ↓
[AUTO-DETECT 3] Steering lengkap? → Jika kurang → GENERATE otomatis
    ↓
[AUTO-DETECT 4] CURRENT-STATE.md ada? → Jika tidak → BUAT
    ↓
[AUTO-DETECT 5] MCP config ada? → Jika tidak → GENERATE
    ↓
[Lanjut workflow normal — < 30 detik]
```

**Manfaat:**
- File terhapus? → Otomatis dibuat ulang
- Project lama tanpa hooks? → Otomatis di-upgrade
- Developer baru join? → Framework langsung ready
- Tidak perlu manual setup ulang

**Visual:** Self-healing loop diagram dengan checkmarks

---


## ═══════════════════════════════════════
## WORKFLOW (Slides 14-18)
## ═══════════════════════════════════════

---

## Slide 14 — Project Initialization (8 Questions)

### Memulai Project — 8 Pertanyaan Wajib

Saat power diaktifkan, AI Agent menanyakan:

| # | Pertanyaan | Contoh Jawaban |
|---|-----------|----------------|
| 1 | Nama project? | `payment-gateway` |
| 2 | Deskripsi singkat? | "Sistem pembayaran digital" |
| 3 | Tech stack? | Next.js / Go / Python |
| 4 | Project baru atau existing? | Baru |
| 5 | Ada UI? Design System? | Signal / Custom / Default |
| 6 | GitLab credentials ready? | Ya / Belum (dibantu setup) |
| 7 | Mode development? | Enterprise / Solo / Zero Touch |
| 8 | Mulai dari layer mana? | Layer 0 (default) |

**Setelah Dijawab:**
```
AI Agent otomatis:
→ git init
→ Create GitLab project
→ Setup folder structure
→ Generate hooks + steering
→ Push initial commit
→ Mulai Layer 0
```

**Visual:** Form/questionnaire mockup dengan 8 fields

---

## Slide 15 — Sprint Workflow

### Sprint Lifecycle: Planning → Execution → Completion

```
┌─────────────────────────────────────────────────────────┐
│  📋 SPRINT PLANNING                                     │
│  → Buat milestone di GitLab                             │
│  → Buat issues (Epic → Feature → Task)                  │
│  → Setup board (Backlog → Ready → In Progress → Done)   │
│  → Create wiki pages                                    │
├─────────────────────────────────────────────────────────┤
│  ⚙️ SPRINT EXECUTION (per task)                         │
│  → Load context (spec, design, ADR)                     │
│  → Implement code (Clean Architecture)                  │
│  → Run lint + typecheck + test                          │
│  → Commit + push                                        │
│  → Update issue status di GitLab                        │
├─────────────────────────────────────────────────────────┤
│  ✅ SPRINT COMPLETION                                    │
│  → Local validation (lint, typecheck, test, coverage)   │
│  → Code review offer                                    │
│  → Create MR (feature → develop)                        │
│  → User merge di GitLab                                 │
│  → Sprint Completion Package                            │
└─────────────────────────────────────────────────────────┘
```

**Visual:** 3-phase horizontal pipeline diagram

---

## Slide 16 — Per-Task Agent Chain

### Setiap Task Melewati Agent Chain

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ 🏗️       │    │ ⚙️       │    │ 🧪       │    │ 🔒       │    │ 🚀       │
│ ARCHITECT│ →  │ BACKEND  │ →  │ QA       │ →  │ SECURITY │ →  │ DEVOPS   │
│          │    │          │    │          │    │          │    │          │
│ • Spec   │    │ • Code   │    │ • Test   │    │ • Scan   │    │ • Push   │
│ • Design │    │ • Logic  │    │ • Cover  │    │ • Review │    │ • Deploy │
│ • Impact │    │ • API    │    │ • E2E    │    │ • Fix    │    │ • CI/CD  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

**Flow Detail:**
1. 🏗️ **Architect** — Validasi spec, cek impact, load context
2. ⚙️ **Backend/Frontend** — Implement sesuai architecture
3. 🧪 **QA** — Write tests, ensure coverage >= 80%
4. 🔒 **Security** — Scan vulnerabilities, secure coding check
5. 🚀 **DevOps** — Commit, push, update GitLab issue

**Setiap agent menampilkan banner saat aktif:**
```
━━━ 🏗️ ARCHITECT AGENT ━━━
[melakukan validasi...]
✅ Spec valid, architecture compliant
```

**Visual:** Pipeline dengan 5 agent boxes dan arrows

---

## Slide 17 — Sprint Completion Package

### Setelah MR Merged — 4 Deliverables Otomatis

| # | Package | Output |
|---|---------|--------|
| 📚 | **Sprint Retrospective** | `docs/retrospectives/sprint-[N].md` |
| 📊 | **Quality Scorecard** | `docs/quality/sprint-[N]-scorecard.md` |
| 🏗️ | **Health Check** | `docs/quality/health-check-[date].md` |
| 📖 | **Wiki Update** | GitLab Wiki: Changelog, API-Docs, ADR |

**Retrospective Berisi:**
- What went well / What didn't
- AI effectiveness metrics
- Improvement actions untuk sprint berikutnya

**Scorecard Berisi:**
- Code Quality (coverage, lint, type errors)
- AI Effectiveness (tasks completed, rework rate)
- Governance Compliance (conventional commits, spec-first)
- Technical Debt (new vs resolved)

**Visual:** 4 document icons dengan checkmarks

---

## Slide 18 — Compliance Validator

### Sprint-End Auto-Check — Tidak Ada yang Terlewat

**Hook `sprint-end-auto-check` otomatis validasi:**

```
✅ Checklist Compliance:
├── Semua issues punya label status::done?
├── Coverage >= 80% (atau 60% di Fast Track)?
├── Semua tests passing?
├── Conventional commits?
├── Spec-first compliance (fitur kompleks)?
├── Architecture violations = 0?
├── Security vulnerabilities (high) = 0?
├── Observability (logger) di setiap use case?
└── CONTEXT-INDEX.md up to date?
```

**Jika Ada Violation:**
- Informasikan user dengan detail
- Suggest fix
- Track sebagai tech debt jika di-skip (Fast Track only)

**Visual:** Checklist with green checkmarks and red X marks

---


## ═══════════════════════════════════════
## QUALITY & GOVERNANCE (Slides 19-22)
## ═══════════════════════════════════════

---

## Slide 19 — Quality Gates (CI/CD Pipeline)

### GitLab CI/CD — 7 Gates yang Block Merge

```
┌────────┐   ┌───────────┐   ┌────────┐   ┌──────────┐   ┌──────────┐   ┌───────┐   ┌────────┐
│  LINT  │ → │ TYPECHECK │ → │  TEST  │ → │ COVERAGE │ → │ SECURITY │ → │ BUILD │ → │  E2E   │
│        │   │           │   │        │   │          │   │          │   │       │   │        │
│ 0 err  │   │  0 err    │   │ All ✅  │   │  ≥ 80%   │   │  0 high  │   │  ✅    │   │ All ✅  │
└────────┘   └───────────┘   └────────┘   └──────────┘   └──────────┘   └───────┘   └────────┘
     │              │              │              │              │             │            │
     ▼              ▼              ▼              ▼              ▼             ▼            ▼
   BLOCK          BLOCK          BLOCK          BLOCK          BLOCK        BLOCK        BLOCK
  if fail        if fail        if fail        if < 80%       if high      if fail      if fail
```

**Setiap gate BLOCKING — MR tidak bisa di-merge jika gagal**

**Local Validation (sebelum push):**
```bash
npm run lint          # 0 errors
npm run typecheck     # 0 errors
npm run test:unit -- --coverage  # >= 80%
```

**Visual:** Pipeline stages with gate icons (red/green)

---

## Slide 20 — Test Pyramid + Coverage Rules

### Test Strategy — Pyramid + Minimum Coverage

```
          ┌─────────┐
          │   E2E   │  ← Sedikit, mahal, critical paths
          │  (10%)  │
          ├─────────┤
          │ Integr. │  ← Medium, API contracts
          │  (20%)  │
          ├─────────┤
          │  Unit   │  ← Banyak, cepat, murah
          │  (70%)  │
          └─────────┘
```

**Coverage Rules:**
| Area | Minimum Coverage |
|------|-----------------|
| Use Cases (business logic) | 90% |
| Repositories (data access) | 85% |
| Controllers/Handlers | 80% |
| Auth/Payment/Security | 95% |
| Utilities/Helpers | 80% |
| **Overall minimum** | **80%** |

**Testing Patterns (per layer):**
- Use Case → mock repository, test business rules
- Repository → integration test dengan test DB
- ViewModel/Mapper → pure function test
- Middleware → mock request/response
- E2E → happy path + critical error paths

**Visual:** Pyramid diagram with percentages

---

## Slide 21 — AI Quality Scorecard

### Mengukur Efektivitas AI — Bukan Hanya Code Quality

**4 Dimensi Pengukuran:**

| Dimensi | Metrics | Target |
|---------|---------|--------|
| 🔧 **Code Quality** | Coverage, lint errors, type errors, architecture violations | Coverage ≥ 80%, 0 violations |
| 🤖 **AI Effectiveness** | Tasks completed, rework rate, spec compliance | Rework < 15% |
| 📋 **Governance** | Conventional commits, spec-first, observability | 100% compliance |
| 💳 **Technical Debt** | New debt, resolved debt, fast-track debt | Net debt ≤ 0 |

**Scorecard di-generate otomatis setiap sprint:**
```
docs/quality/sprint-[N]-scorecard.md
```

**Data source:** `docs/quality/metrics-log.jsonl` (auto-collected per task)

**Visual:** Dashboard mockup dengan 4 quadrants dan gauges

---

## Slide 22 — Hallucination Detection

### Mendeteksi & Mencegah AI Hallucination

**Strategi Multi-Layer:**

1. **Spec-First Gate** (Layer 3)
   - AI WAJIB baca spec sebelum coding
   - Tanpa spec → coding diblokir (fitur kompleks)

2. **Issue-Driven (Small Context)** (Layer 8)
   - 1 issue = 1 bounded context
   - Konteks kecil = hallucination rendah

3. **Architecture Enforcement** (Layer 11)
   - Post-task review: apakah code sesuai spec?
   - Dependency rule violation = red flag

4. **Rework Count sebagai Proxy** (Layer 14)
   - `rework_count` di metrics-log.jsonl
   - Rework tinggi = indikasi hallucination
   - Target: rework rate < 15%

5. **Continuous Learning** (Layer 14)
   - Bug patterns di-capture
   - AI belajar dari kesalahan sebelumnya
   - ADR mencegah keputusan yang sama salah

**Visual:** Funnel diagram — hallucination berkurang di setiap layer

---


## ═══════════════════════════════════════
## GITLAB INTEGRATION (Slides 23-25)
## ═══════════════════════════════════════

---

## Slide 23 — Issue Board Automation

### GitLab Issue Board — Fully Automated

**Board Columns:**
```
┌──────────┐  ┌──────────┐  ┌─────────────┐  ┌──────────┐  ┌──────────┐
│ BACKLOG  │  │  READY   │  │ IN PROGRESS │  │  REVIEW  │  │   DONE   │
│          │  │          │  │             │  │          │  │          │
│ Issue #1 │→ │ Issue #2 │→ │  Issue #3   │→ │ Issue #4 │→ │ Issue #5 │
│ Issue #6 │  │ Issue #7 │  │             │  │          │  │ Issue #8 │
└──────────┘  └──────────┘  └─────────────┘  └──────────┘  └──────────┘
```

**Otomatis oleh AI Agent:**
- Task dimulai → label `status::in-progress`
- Task selesai + pushed → label `status::review` + comment summary
- Bug ditemukan → create issue `type::bug`
- Sprint selesai → create MR + update semua issues
- MR merged → close issues + update milestone

**Issue Hierarchy:**
```
Epic (big feature)
  └── Feature Issue (user story)
       └── Task Issue (implementable unit)
```

**Label Taxonomy:**
- `type::` — feature, task, bug, improvement, tech-debt
- `status::` — backlog, ready, in-progress, review, done
- `priority::` — critical, high, medium, low
- `sprint::` — sprint-1, sprint-2, ...

**Visual:** Kanban board screenshot/mockup

---

## Slide 24 — Milestone + Wiki Management

### Sprint = Milestone — Terstruktur & Tracked

**Milestone Management:**
- 1 Sprint = 1 Milestone di GitLab
- Semua issues di-assign ke milestone
- Progress tracking otomatis (% complete)
- Close milestone saat sprint selesai
- Carry-over issues ke milestone berikutnya

**Wiki Pages (Auto-generated):**

| Page | Konten | Update Kapan |
|------|--------|-------------|
| **Home** | Project overview, quick links | Sprint start |
| **Changelog** | Version history, what's new | Setiap MR merged |
| **API-Documentation** | Endpoint list, request/response | Endpoint baru dibuat |
| **Architecture-Decisions** | ADR summary + links | Keputusan arsitektur baru |

**Wiki Update Flow:**
```
Sprint selesai → MR merged → AI update wiki otomatis:
  → Changelog: tambah entry sprint ini
  → API-Docs: update jika ada endpoint baru
  → ADR: update jika ada keputusan baru
```

**Visual:** GitLab milestone progress bar + wiki page tree

---

## Slide 25 — Branch Strategy

### Git Flow — Feature → Develop → Main

```
main ─────────────────────────────────────────────── (production)
  │                                        ▲
  │                                        │ (merge develop → main)
  ▼                                        │
develop ──────────────────────────────────────────── (integration)
  │         ▲         │         ▲
  │         │         │         │
  ▼         │         ▼         │
feature/    │    feature/       │
issue-1  ───┘    issue-2  ──────┘
```

**Rules:**
- `main` — production-ready, protected
- `develop` — integration branch, MR target
- `feature/issue-[N]-[slug]` — per-issue branch
- MR: feature → develop (NEVER langsung ke main)
- AI Agent DILARANG merge sendiri — user harus approve

**Commit Convention:**
```
feat(scope): description     # fitur baru
fix(scope): description      # bug fix
docs(scope): description     # dokumentasi
test(scope): description     # testing
refactor(scope): description # refactoring
chore(scope): description    # maintenance
```

**Visual:** Git graph diagram dengan branch lines

---


## ═══════════════════════════════════════
## MODES & FLEXIBILITY (Slides 26-28)
## ═══════════════════════════════════════

---

## Slide 26 — 4 Operating Modes

### Satu Framework, Empat Mode — Sesuai Kebutuhan

| Mode | Ceremony | Target User | Key Difference |
|------|----------|-------------|----------------|
| 🏢 **Enterprise** | Full | Tim, production | Konfirmasi per-layer, full CI/CD, MR approval |
| 👤 **Solo** | Simplified | Developer tunggal | Tanpa develop branch, self-merge, minimal labels |
| ⚡ **Fast Track** | Reduced | Deadline mendesak | Coverage 60%, skip spec (tracked as debt) |
| 🤖 **Zero Touch** | Automated | PoC, prototype | AI jalankan semua, stop di merge point |

**Enterprise Mode (Default):**
- Sprint planning dengan milestones
- Full issue board + label taxonomy
- Branch: main + develop + feature
- Code review wajib sebelum merge
- Full retrospective + scorecard

**Zero Touch Mode:**
```
User jawab 8 pertanyaan → AI jalankan Layer 0-8 tanpa stop
→ AI kerjakan semua sprint issues sequential
→ AI create MR → STOP (user merge)
→ Auto retro + scorecard + wiki
```

**Visual:** 4 mode cards dengan icons dan key attributes

---

## Slide 27 — Design System Options

### 3 Pilihan Design System — Atau Tanpa UI

| Option | Deskripsi | Kapan Pakai |
|--------|-----------|-------------|
| 🎨 **Signal** | Built-in design system (sudah tersedia di framework) | Project baru tanpa existing design |
| 🎯 **Custom** | User punya design system sendiri | Enterprise dengan brand guidelines |
| 📐 **Default** | Generate default (Atomic Design + neutral tokens) | Project tanpa preferensi |

**Signal Design System (Built-in):**
- Color tokens: Primary (Red), Secondary (Navy), Valid (Green), Warning, Info, Neutral
- Typography: Signal font scale
- Spacing: Signal spacing scale
- Components: Signal component patterns
- Dark/Light theme support
- Atomic Design structure (atoms/molecules/organisms)

**Custom Design System:**
- AI extract dari file yang dikirim user (PDF, Figma, JSON tokens)
- Generate steering file + CSS tokens otomatis
- Enforce di semua UI development

**Visual:** 3 design system previews side by side

---

## Slide 28 — Tech Stack Support

### Multi-Stack — Prinsip Sama, Implementasi Adaptif

| Stack | Status | DI Approach | Test Runner | Default |
|-------|--------|-------------|-------------|---------|
| **Next.js (TypeScript)** | ✅ Fully supported | Awilix | Vitest | ✅ |
| **Go (Golang)** | ✅ Supported | Constructor injection | go test | |
| **Python (FastAPI)** | ✅ Supported | dependency-injector / Depends() | pytest | |

**Yang Sama di Semua Stack:**
- Clean Architecture (4 layers)
- Dependency Rule (inner ≠ depend outer)
- Spec-First approach
- Quality Gates (lint, test, coverage)
- Issue-Driven Development
- Continuous Context

**Yang Berbeda per Stack:**
- DI mechanism
- Test runner & assertion library
- Folder structure conventions
- CI/CD pipeline syntax
- Linting tools

**Visual:** 3 tech stack logos dengan shared principles in center

---


## ═══════════════════════════════════════
## RESULTS & COMPARISON (Slides 29-31)
## ═══════════════════════════════════════

---

## Slide 29 — Before vs After

### AI Chaos → Engineering Acceleration

| Aspek | ❌ BEFORE (AI Chaos) | ✅ AFTER (Framework) |
|-------|---------------------|---------------------|
| Architecture | Tidak ada, spaghetti code | Clean Architecture enforced |
| Specifications | Tidak ada, AI hallucinate | Spec-first, validated |
| Quality | Inconsistent, bugs ke prod | 80%+ coverage, 7 gates |
| Context | Hilang tiap session | Persistent, accumulated |
| Governance | Tidak ada | Policy-driven, auditable |
| Learning | Ad-hoc | Systematic per sprint |
| Collaboration | Chaotic | Issue-driven, traceable |
| Deployment | Manual, error-prone | CI/CD automated |
| Onboarding | Briefing panjang | Baca steering, langsung kerja |
| AI Output | Unpredictable | Consistent, governed |

**Hasil Nyata:**
- Technical debt: ↓ Berkurang signifikan
- Bug ke production: ↓ Tertangkap di quality gates
- Developer onboarding: ↓ Dari hari ke menit
- AI rework rate: ↓ Target < 15%
- Sprint predictability: ↑ Meningkat

**Visual:** Before/After split screen dengan red (chaos) vs green (order)

---

## Slide 30 — Comparison with BMAD Method

### Framework Ini vs BMAD Method

| Aspek | BMAD Method | Enterprise AI-Native SDLC |
|-------|-------------|--------------------------|
| **Scope** | Project generation | Full SDLC lifecycle |
| **Architecture** | Not enforced | Clean Architecture + DI (Awilix) |
| **Governance** | Minimal | Full (policy, compliance, audit) |
| **Quality Gates** | Basic | 7-stage CI/CD pipeline |
| **Context** | Session-based | Persistent + accumulated |
| **Learning** | None | Continuous (retro, ADR, bug capture) |
| **Project Mgmt** | None | Full GitLab integration |
| **Multi-Agent** | Sequential prompts | Structured agent chain + hooks |
| **Observability** | None | Built-in (logging, metrics, tracing) |
| **Modes** | Single | 4 modes (Enterprise, Solo, Fast Track, Zero Touch) |
| **Tech Stack** | Single | Multi-stack (Next.js, Go, Python) |
| **Self-Healing** | None | Auto-detect + auto-repair |

**Key Differentiator:**
- BMAD: Fokus pada **generating** project
- Framework ini: Fokus pada **governing** AI sepanjang lifecycle

**Visual:** Comparison table with highlight on differentiators

---

## Slide 31 — Statistics & Scale

### Framework dalam Angka

```
┌─────────────────────────────────────────────────┐
│                                                 │
│   27        9         70+        4              │
│  Steering   Hooks    Mandatory   Operating      │
│  Files      Defined  Documents   Modes          │
│                                                 │
├─────────────────────────────────────────────────┤
│                                                 │
│   15        7         3          13/15          │
│  Layers     AI        Tech       Automated      │
│  Framework  Agents    Stacks     Layers (87%)   │
│                                                 │
├─────────────────────────────────────────────────┤
│                                                 │
│   2         80%       8          4              │
│  MCP        Min       Init       Quality        │
│  Servers    Coverage  Questions  Dimensions     │
│                                                 │
└─────────────────────────────────────────────────┘
```

**Scale Indicators:**
- 27 steering files = comprehensive guidance
- 9 hooks = automated workflow tanpa manual trigger
- 70+ mandatory documents = full traceability
- 4 modes = flexibility untuk berbagai konteks
- 13/15 automated layers (9 fully + 4 partial) = minimal manual intervention
- 7 AI agents = specialized roles, not generic

**Visual:** Big number cards / infographic style

---


## ═══════════════════════════════════════
## CLOSING (Slide 32)
## ═══════════════════════════════════════

---

## Slide 32 — Final Insight & Call to Action

### Closing Quote

> *"AI bukan pengganti software engineering.*
> *AI membutuhkan software engineering discipline yang jauh lebih kuat.*
>
> *Tanpa governance, specifications, architecture, validation, dan observability —*
> *AI hanya akan menghasilkan Technical Debt at Scale.*
>
> *Sedangkan AI + SDLC + Governance + Specifications + Observability*
> *akan menghasilkan Enterprise Engineering Acceleration."*

---

### Call to Action

**Untuk Tim Engineering:**
- Install Kiro IDE + Enterprise AI-Native SDLC Framework Power
- Mulai dengan 1 project pilot (gunakan Enterprise Mode)
- Ukur: rework rate, coverage, sprint predictability

**Untuk Leadership:**
- AI tanpa governance = risk, bukan acceleration
- Framework ini = guardrails yang enable, bukan restrict
- ROI: less rework, faster onboarding, consistent quality

**Untuk Komunitas:**
- Open source: https://github.com/RickoCP/ai_sdlc_framework
- Kontribusi: steering files, tech stack profiles, hooks
- Feedback: issues & discussions di GitHub

---

### Terima Kasih

**Enterprise AI-Native SDLC Framework v1.5.0**

*Discipline at Scale — Making AI-Generated Code Enterprise-Ready*

Built with Kiro | Powered by Spec-Driven Development

---

## ═══════════════════════════════════════
## APPENDIX — Speaker Notes & Tips
## ═══════════════════════════════════════

### Tips Presentasi

1. **Slide 2 (Problem)** — Tanyakan audience: "Siapa yang pernah mengalami AI generate code yang tidak bisa di-maintain?" — biasanya 80%+ angkat tangan
2. **Slide 6 (15 Layers)** — Jangan jelaskan semua layer detail. Fokus ke 3-4 yang paling relevan untuk audience
3. **Slide 11 (Multi-Agent)** — Demo live jika memungkinkan: tunjukkan agent banner saat task execution
4. **Slide 19 (Quality Gates)** — Tunjukkan contoh MR yang di-block oleh pipeline — sangat impactful
5. **Slide 26 (4 Modes)** — Sesuaikan penekanan dengan audience: Enterprise untuk CTO, Solo untuk indie devs
6. **Slide 29 (Before/After)** — Gunakan contoh nyata dari project yang sudah pakai framework

### Durasi Estimasi

| Section | Slides | Durasi (menit) |
|---------|--------|----------------|
| Opening | 1-3 | 5 |
| Core Concept | 4-8 | 10 |
| Architecture | 9-13 | 10 |
| Workflow | 14-18 | 10 |
| Quality & Governance | 19-22 | 8 |
| GitLab Integration | 23-25 | 7 |
| Modes & Flexibility | 26-28 | 7 |
| Results & Comparison | 29-31 | 8 |
| Closing | 32 | 5 |
| **Total** | **32** | **~70 menit** |

**Untuk presentasi 30 menit:** Fokus ke slides 1-3, 5-6, 11, 15-16, 19, 26, 29, 32
**Untuk presentasi 15 menit:** Fokus ke slides 1-3, 6, 11, 19, 29, 32

---

*Generated from Enterprise AI-Native SDLC Framework v1.5.0*
*Format: Slide-by-slide content untuk PowerPoint / Google Slides*