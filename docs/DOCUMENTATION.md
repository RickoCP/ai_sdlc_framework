# Enterprise AI-Native SDLC Framework — Dokumentasi Lengkap

**Version:** 1.5.0  
**Author:** ricko_c_putra@telkomsel.co.id  
**Last Updated:** 2026-05-10  
**Repository:** https://github.com/RickoCP/ai_sdlc_framework

---

## Daftar Isi

1. [Apa Itu Power Ini](#1-apa-itu-power-ini)
2. [Masalah yang Diselesaikan](#2-masalah-yang-diselesaikan)
3. [Arsitektur Framework](#3-arsitektur-framework)
4. [15 Layer — Penjelasan Lengkap](#4-15-layer--penjelasan-lengkap)
5. [Steering Files](#5-steering-files)
6. [Mode Operasi](#6-mode-operasi)
7. [Automated Workflow & Hooks](#7-automated-workflow--hooks)
8. [MCP Server Integration](#8-mcp-server-integration)
9. [Instalasi & Setup](#9-instalasi--setup)
10. [Cara Penggunaan](#10-cara-penggunaan)
11. [Quality & Governance](#11-quality--governance)
12. [Tech Stack Support](#12-tech-stack-support)
13. [FAQ](#13-faq)

---

## 1. Apa Itu Power Ini

Enterprise AI-Native SDLC Framework adalah **Kiro Power** yang mengubah AI dari sekadar code generator menjadi **AI Operating Layer** untuk seluruh Software Development Lifecycle.

Power ini menggabungkan:
- **Spec-Driven Development** — AI bekerja dari specification, bukan instruksi ambigu
- **AI Governance** — Mencegah hallucination dan technical debt at scale
- **Clean Architecture + DI** — Arsitektur enterprise-grade yang enforced
- **Issue-Driven Development** — 1 issue = 1 bounded context = less hallucination
- **Continuous Context** — AI tidak kehilangan memory antar session
- **Multi-Agent Orchestration** — Workflow terstruktur untuk AI agents
- **GitLab CI/CD** — Quality gates otomatis di setiap push

**Tagline:** *"Discipline at scale — making AI-generated code enterprise-ready."*

---

## 2. Masalah yang Diselesaikan

### Tanpa Framework Ini (AI Chaos)

| Masalah | Dampak |
|---------|--------|
| AI generate code tanpa arsitektur | Technical debt menumpuk |
| Tidak ada specification | AI hallucinate requirements |
| Tidak ada governance | Code quality tidak konsisten |
| Context hilang antar session | AI mulai dari nol setiap kali |
| Tidak ada quality gates | Bug masuk production |
| Tidak ada learning loop | Kesalahan yang sama berulang |
| Full-app generation | Hallucination tinggi, code tidak maintainable |

### Dengan Framework Ini (Engineering Acceleration)

| Solusi | Hasil |
|--------|-------|
| Clean Architecture enforced | Code maintainable dan testable |
| Spec-first approach | AI output sesuai requirement |
| AI Governance | Hallucination rate turun |
| Project Memory | AI resume dari session sebelumnya |
| Automated quality gates | Bug tertangkap sebelum production |
| Continuous Learning | Sistem makin baik setiap sprint |
| Issue-driven (small context) | AI output lebih stabil dan akurat |

---

## 3. Arsitektur Framework

### Paradigm Shift

```
Traditional SDLC:  Human → Code → Deploy

AI-Native SDLC:    Human + AI Agents
                   → Specifications
                   → Validation
                   → Architecture
                   → Tasks
                   → Code
                   → Testing
                   → Observability
                   → Continuous Learning
```

### Flow Lengkap

```
Vision → Requirement Intake → Requirement Validation → Spec-Driven Development
→ System Design → Security Design → Technical Design → Governance Layer
→ AI Skills → GitLab Planning → Issue-Driven Development
→ AI Agent Orchestration → Continuous Context Accumulation
→ AI Review → CI/CD → Observability → Continuous Learning
         ↑                                              │
         └──────────── Feedback Loop ───────────────────┘
```

### Layer Automation Status

```
Layer 0  [MANUAL]     Product Vision — keputusan bisnis manusia
Layer 1  [MANUAL]     Requirement Intake — input bervariasi
Layer 2  [AUTOMATED]  Requirement Validation — auto-validate saat file dibuat
Layer 3  [AUTOMATED]  Spec-Driven Dev — enforce spec-first gate
Layer 4  [AUTOMATED]  Design System — design compliance check
Layer 5  [PARTIAL]    AI Governance — enforced via CI/CD
Layer 6  [PARTIAL]    AI Skills — auto-loaded via fileMatch
Layer 7  [PARTIAL]    Team Extension — auto-loaded via fileMatch
Layer 8  [AUTOMATED]  Issue-Driven Dev — full GitLab integration
Layer 9  [MANUAL]     Agent Orchestration — conceptual
Layer 10 [AUTOMATED]  Continuous Context — auto-load + Context Index
Layer 11 [AUTOMATED]  AI Review — post-task validation
Layer 12 [AUTOMATED]  Quality Gates — CI/CD pipeline blocking
Layer 13 [AUTOMATED]  Observability — checklist + hook
Layer 14 [AUTOMATED]  Continuous Learning — bug capture + retro + ADR
```

**11 dari 15 layer terintegrasi ke workflow otomatis.**

---

## 4. 15 Layer — Penjelasan Lengkap

### Layer 0 — Product Vision
**Tujuan:** Menentukan arah produk, business value, dan roadmap.  
**Output (SEMUA WAJIB):**
- `docs/product/vision.md`
- `docs/product/roadmap.md`
- `docs/product/business-goals.md`
- `docs/product/success-metrics.md`
- `docs/product/ai-opportunities.md`

**Automation:** Manual (keputusan bisnis)

### Layer 1 — Requirement Intake
**Tujuan:** AI membaca berbagai input source (PRD, meeting notes, user feedback) dan mengekstrak requirements terstruktur.  
**Output (SEMUA WAJIB):**
- `docs/requirements/extracted/user-stories.md`
- `docs/requirements/extracted/functional-requirements.md`
- `docs/requirements/extracted/non-functional-requirements.md`

**Automation:** Manual (input terlalu bervariasi)

### Layer 2 — Requirement Validation
**Tujuan:** Validasi requirements sebelum eksekusi — cek ambiguity, completeness, conflicts, feasibility.  
**Output (SEMUA WAJIB):**
- `docs/requirements/validation/validation-report-[date].md`
- `docs/requirements/validation/ambiguity-report.md`
- `docs/requirements/validation/conflict-analysis.md`
- `docs/requirements/validation/risk-analysis.md`

**Automation:** ✅ Hook `fileCreated` pada `docs/requirements/**` — auto-validate  
**Gate:** BLOCK jika ada critical issue

### Layer 3 — Spec-Driven Development
**Tujuan:** Idea → Specification → Validation → Architecture → Tasks → Code  
**Output (SEMUA WAJIB):**
- `docs/specs/brd/business-flow.md` + `stakeholder-matrix.md`
- `docs/specs/prd/user-stories.md` + `acceptance-criteria.md` + `feature-matrix.md`
- `docs/specs/srs/[feature]-spec.md` + `architecture.md` + `sequence-diagram.md` + `api-contract.md` + `state-flow.md` + `failure-scenario.md` + `data-model.md`

**Automation:** ✅ Hook `preTaskExecution` — enforce spec-first untuk fitur kompleks  
**Gate:** BLOCK coding tanpa spec (fitur kompleks)

### Layer 4 — Design System
**Tujuan:** System Design, Technical Design, UI/UX Design, Security Design  
**Output (SEMUA WAJIB):**
- System: `high-level-architecture.md` + `sequence-diagram.md` + `deployment.md` + `c4-model.md` + `event-flow.md`
- Technical: `clean-architecture.md` + `folder-structure.md` + `error-handling.md` + `naming-convention.md` + `testing-pattern.md`
- Security: `threat-model.md` + `trust-boundary.md` + `attack-surface.md` + `mitigation-plan.md`
- UI/UX (jika ada UI): `wireframe.md` + `component-library.md` + `accessibility.md` + `i18n-strategy.md` + `theming.md` + `design-token.md`

**Design System Options (ditanyakan di pertanyaan inisiasi #5):**
- **Signal Design System** (built-in) — sudah tersedia di framework via `signal-design-system-complete.md`
- **Custom** — user punya design system sendiri (kirimkan file/detail)
- **Default** — generate default design system (Atomic Design + neutral tokens)

**Automation:** ✅ Hook `postTaskExecution` — validate code vs design document  
**Gate:** Informasikan deviasi, suggest update

### Layer 5 — AI Governance
**Tujuan:** Mencegah hallucination dan AI chaos melalui policy, standards, compliance.  
**Output (SEMUA WAJIB):**
- `docs/governance/ai-policy.md`
- `docs/governance/approved-tools.md`
- `docs/governance/security-policy.md`
- `docs/governance/code-review-policy.md`

**Automation:** Partial — enforced via CI/CD pipeline

### Layer 6 — AI Engineering Skills
**Tujuan:** Reusable workflow dan standards (create-api, create-usecase, create-test, dll).  
**Output (SEMUA WAJIB — 6 files):**
- `.kiro/skills/create-api.md`
- `.kiro/skills/create-usecase.md`
- `.kiro/skills/create-repository.md`
- `.kiro/skills/create-component.md`
- `.kiro/skills/create-test.md`
- `.kiro/skills/create-migration.md`

**Automation:** Partial — auto-loaded via fileMatch patterns

### Layer 7 — Team Extension Skills
**Tujuan:** Global + team-specific standards yang extend Layer 6.  
**Output (WAJIB minimal 1 per domain):**
- `.kiro/steering/team/[domain]-team.md`

**Automation:** Partial — auto-loaded via fileMatch patterns

### Layer 8 — Issue-Driven Development
**Tujuan:** 1 issue = 1 bounded context. Mengurangi hallucination dan context overflow.  
**Output (SEMUA WAJIB):**
- GitLab: Issues (Epic → Feature → Task) + Milestone + Labels + Board
- GitLab Wiki: Home, Changelog, API-Documentation, Architecture-Decisions
- `.gitlab/issue_templates/Feature.md` + `Task.md` + `Bug.md`
- `.gitlab/merge_request_templates/Default.md`

**Automation:** ✅ Full GitLab integration — branch per issue, auto-push, MR creation

### Layer 9 — AI Agent Orchestration
**Tujuan:** Multi-agent workflow — Backend Agent, Frontend Agent, DevOps Agent, QA Agent.  
**Output:** Orchestrated task execution (via hooks)  
**Automation:** ✅ Hook chains — role-based prompting + sub-agent delegation

### Layer 10 — Continuous Context
**Tujuan:** Artifact dari setiap layer menjadi context untuk layer berikutnya. AI tidak kehilangan memory.  
**Output (SEMUA WAJIB):**
- `docs/CONTEXT-INDEX.md`
- `docs/CURRENT-STATE.md`

**Automation:** ✅ Hook `preTaskExecution` — auto-load context + maintain index

### Layer 11 — AI Review & Validation
**Tujuan:** AI sebagai reviewer — cek architecture, security, performance, accessibility.  
**Output:** Review feedback (inline), compliance report  
**Automation:** ✅ Post-task validation — architecture + security + design compliance

### Layer 12 — Automated Quality Gates
**Tujuan:** GitLab CI/CD pipeline yang block merge jika quality tidak terpenuhi.  
**Output (SEMUA WAJIB):**
- `.gitlab-ci.yml`
- `.eslintrc.json` / `eslint.config.mjs`
- `tsconfig.json`
- `.prettierrc`
- `vitest.config.ts`
- `commitlint.config.js`

**Automation:** ✅ Full CI/CD — Lint → Typecheck → Test → Coverage → Security → Build → Deploy  
**Gate:** Coverage < 80% = BLOCK merge

### Layer 13 — Observability & Feedback
**Tujuan:** Mengukur system health, AI performance, engineering quality.  
**Output:** Logging + metrics di source code (use case, repository, handler)  
**Automation:** ✅ Hook `postToolUse` — remind observability saat fitur baru  
**Phased:** Day 1 (logger + Sentry) → Sprint 1 (metrics) → Sprint 2+ (tracing + alerts)

### Layer 14 — Continuous Learning
**Tujuan:** Belajar dari production — bugs, performance, review patterns, AI effectiveness.  
**Output (SEMUA WAJIB — auto-generated):**
- `docs/learnings/BUG-[N]-[title].md` (per bug fix)
- `docs/retrospectives/sprint-[N].md` (per sprint)
- `docs/adr/ADR-[N]-[title].md` (per keputusan arsitektur)
- `docs/tech-debt/TD-[N]-[title].md` (per debt detected)
- `docs/quality/metrics-log.jsonl` (auto-collected per task)
- `docs/quality/sprint-[N]-scorecard.md` (per sprint)

**Automation:** ✅ Hooks — Bug Learning Capture, Sprint Retro Generator, Metrics Collector, Compliance Validator

---

## 5. Steering Files

### Always Included (aktif di setiap session)

| File | Fungsi |
|------|--------|
| `architecture-standards.md` | Clean Architecture + DDD + Awilix DI — acuan arsitektur WAJIB |
| `test-writing-patterns.md` | Pattern testing per-layer dengan contoh TypeScript |
| `project-memory.md` | Context Index + Current State untuk resume antar session |
| `hooks-registry.md` | Definisi lengkap 9 hook files (JSON content, smart filtering, metrics) |

### Layer Files (15 files)

| File | Layer |
|------|-------|
| `layer-0-product-vision.md` | Product Vision |
| `layer-1-requirement-intake.md` | Requirement Intake |
| `layer-2-requirement-validation.md` | Requirement Validation |
| `layer-3-spec-driven-development.md` | Spec-Driven Development |
| `layer-4-design-system.md` | Design System |
| `layer-5-ai-governance.md` | AI Governance |
| `layer-6-ai-skills.md` | AI Engineering Skills |
| `layer-7-team-extension.md` | Team Extension |
| `layer-8-issue-driven-dev.md` | Issue-Driven Development |
| `layer-9-agent-orchestration.md` | Agent Orchestration |
| `layer-10-continuous-context.md` | Continuous Context |
| `layer-11-ai-review.md` | AI Review |
| `layer-12-quality-gates.md` | Quality Gates |
| `layer-13-observability.md` | Observability |
| `layer-14-continuous-learning.md` | Continuous Learning |

### Operational Files (8 files)

| File | Fungsi |
|------|--------|
| `git-workflow-automation.md` | Git init, commit convention, auto-push, branch strategy, MR |
| `gitlab-cicd-setup.md` | Setup lengkap GitLab CI/CD |
| `project-structure.md` | Folder structure, naming, quickstart |
| `requirement-traceability.md` | PRD → Code → Test traceability |
| `presentation-material.md` | Generate materi presentasi |
| `tech-stack-profiles.md` | Adaptasi ke Go, Python (FastAPI) |
| `fast-track-mode.md` | Fast Track, Quality Scorecard, Health Check |
| `signal-design-system-complete.md` | Signal Design System (built-in option untuk UI) |

---

## 6. Mode Operasi

### Enterprise Mode (Default)

Full ceremony untuk tim enterprise:
- Sprint planning dengan milestones
- Full issue board (Backlog → Ready → In Progress → Review → Done)
- Branch strategy: main + develop + feature (feature → develop → main)
- MR target: feature branch → `develop` (NEVER langsung ke `main`)
- Code review offer BEFORE creating MR
- MR approval wajib
- Full label taxonomy
- Wiki documentation (Home, Changelog, API-Documentation, Architecture-Decisions)
- Formal retrospectives
- Close issue dengan `state_event: "close"` (bukan hanya label update)

### Solo Mode

Simplified untuk developer tunggal:
- Tanpa branch `develop`
- Self-merge allowed
- Minimal labels (type:: saja)
- README.md cukup (tanpa wiki)
- Milestone opsional

**Aktivasi:** User bilang "saya bekerja solo"

### Fast Track Mode

Untuk deadline mendesak:
- Coverage minimum turun ke 60% (dari 80%)
- Spec dan design boleh di-skip
- Tapi SEMUA skip di-track sebagai tech debt
- Security dan architecture tetap enforced
- Auto-create GitLab issue untuk setiap skip

**Aktivasi:** User bilang "aktifkan fast track mode"

### Zero Touch Mode

AI jalankan semuanya otomatis tanpa konfirmasi per-layer:
- AI jalankan Layer 0-8 tanpa stop
- AI kerjakan semua sprint issues sequential
- AI auto-create MR saat semua task done
- STOP di merge point (user tetap harus merge di GitLab)
- Setelah merge → auto retro + scorecard + wiki update

**Aktivasi:** Pilih "Zero Touch" di pertanyaan #7 saat inisiasi project

---

## 7. Automated Workflow & Hooks

### 9 Hook Files (WAJIB Ada di `.kiro/hooks/`)

| File | Trigger | Agent/Fungsi |
|------|---------|--------------|
| `architect-gate.json` | preTaskExecution | 🏗️ Architect + GitLab (issue → in-progress) |
| `code-quality-scan.json` | postToolUse (write) | 🔒 Security + 📊 Observability (smart-filtered) |
| `qa-devops-post-task.json` | postTaskExecution | 🧪 QA + 🚀 DevOps + GitLab (issue → review + comment) |
| `bug-learning-capture.json` | postTaskExecution | 📚 Learning (capture saat bug fix) |
| `metrics-collector.json` | postTaskExecution | 📊 Metrics (auto-collect ke metrics-log.jsonl) |
| `sprint-end-auto-check.json` | postTaskExecution | 🏗️ Compliance Validator + Sprint Completion Offer |
| `sprint-retrospective.json` | userTriggered | 📚 Learning + GitLab (milestone close + wiki update) |
| `quality-scorecard.json` | userTriggered | 📊 Metrics (generate scorecard dari metrics) |
| `health-check.json` | userTriggered | 🏗️ Architect (framework compliance scan) |

### Hook Trigger Types

| Type | Kapan Fire | Hook Files |
|------|-----------|------------|
| `preTaskExecution` | Sebelum task dimulai | architect-gate |
| `postToolUse` (write) | Setelah file ditulis | code-quality-scan |
| `postTaskExecution` | Setelah task selesai | qa-devops, bug-learning, metrics-collector, sprint-end-auto-check |
| `userTriggered` | User trigger manual | sprint-retrospective, quality-scorecard, health-check |

---

## 8. MCP Server Integration

### GitLab MCP (`@zereight/mcp-gitlab`)

Mengelola project management di GitLab:
- Create/manage projects, issues, milestones, labels
- Create merge requests, branches
- Push files, manage wiki
- Pipeline management

### Git MCP (`@cyanheads/git-mcp-server`)

Mengelola git operations:
- Init, commit, push, pull, branch
- Status, log, diff

**⚠️ PENTING:** Git MCP memerlukan `repo_path` (absolute path) di SETIAP tool call.

### Fallback

Jika MCP server tidak tersedia, AI Agent fallback ke shell commands.

---

## 9. Instalasi & Setup

### Prerequisites

- Kiro IDE terinstall
- Node.js >= 20
- Git
- GitLab account (untuk CI/CD dan project management)

### Step 1: Install Power

1. Buka Kiro → Powers panel
2. Install "Enterprise AI-Native SDLC Framework"
3. Power otomatis tersedia

### Step 2: Set Environment Variables

**Windows:**
```powershell
[Environment]::SetEnvironmentVariable("GITLAB_PERSONAL_ACCESS_TOKEN", "glpat-xxx", "User")
[Environment]::SetEnvironmentVariable("GITLAB_API_URL", "https://gitlab.com/api/v4", "User")
```

**Linux/Mac:**
```bash
export GITLAB_PERSONAL_ACCESS_TOKEN="glpat-xxxxxxxxxxxx"
export GITLAB_API_URL="https://gitlab.com/api/v4"
```

### Step 3: Approve Env Vars di Kiro

- Kiro Settings → "Mcp Approved Env Vars"
- Tambahkan: `GITLAB_PERSONAL_ACCESS_TOKEN`, `GITLAB_API_URL`

### Step 4: Restart Kiro

### Step 5: Mulai Gunakan

Panggil power di chat, jawab 8 pertanyaan project, dan framework akan setup semuanya otomatis.

---

## 10. Cara Penggunaan

### Memulai Project Baru

1. Panggil power → jawab 8 pertanyaan
2. AI Agent otomatis: git init → create GitLab project → setup folder → push
3. Mulai dari Layer 0 (Product Vision) → sequential sampai Layer 8 → Sprint dimulai

### Development Cycle (Per Sprint)

```
1. Sprint Planning → buat issues + milestone + wiki pages (Home, Changelog, API-Documentation, Architecture-Decisions) di GitLab
2. Per Task:
   a. AI load context (spec, design, ADR)
   b. AI buat spec (jika belum ada)
   c. AI implement code (ikuti architecture)
   d. AI jalankan lint + typecheck + test
   e. AI commit + push
   f. AI update issue status
3. Sprint End:
   a. Push semua perubahan
   b. Code review offer (AI review sebelum MR)
   c. Create MR (feature → develop, NEVER ke main di Enterprise mode)
   d. Tunggu user merge di GitLab
   e. Sprint Completion Package: retro + scorecard + health check + wiki update
   f. Close issues (state_event: "close" + label status::done)
```

### Perintah Berguna

| Perintah | Fungsi |
|----------|--------|
| "Mulai sprint baru" | Setup milestone + issues |
| "Implement fitur [X]" | Spec → Design → Code → Test → Push |
| "Fix bug #[N]" | Fix + Learning capture |
| "Sprint retrospective" | Generate retro + scorecard |
| "Health check" | Scan framework compliance |
| "Aktifkan fast track" | Reduced ceremony mode |
| "Saya bekerja solo" | Solo mode |

---

## 11. Quality & Governance

### Quality Gates (CI/CD)

| Gate | Threshold | Action |
|------|-----------|--------|
| Lint | 0 errors | Block merge |
| Typecheck | 0 errors | Block merge |
| Unit Test | All pass | Block merge |
| Coverage | >= 80% | Block merge |
| Security (high) | 0 vulnerabilities | Block merge |
| Build | Success | Block merge |
| E2E | All pass | Block merge |

### Local Validation (Sebelum Push)

```bash
npm run lint          # 0 errors
npm run typecheck     # 0 errors
npm run test:unit -- --coverage  # all pass, >= 80%
```

### AI Quality Scorecard (Per Sprint)

Mengukur:
- Code Quality (coverage, lint, type errors, architecture violations)
- AI Effectiveness (tasks completed, rework rate, spec compliance)
- Governance Compliance (conventional commits, spec-first, observability)
- Debt (new debt, resolved debt, fast-track debt)

### Architecture Enforcement

- Clean Architecture + DDD (4 layers)
- Dependency Injection via Awilix
- Dependency rule: inner layer TIDAK BOLEH depend ke outer
- Impact analysis saat core/ berubah

---

## 12. Tech Stack Support

| Stack | Status | DI Approach | Test Runner |
|-------|--------|-------------|-------------|
| **Next.js (TypeScript)** | Default, fully supported | Awilix | Vitest |
| **Go (Golang)** | Supported via profile | Manual constructor injection | go test |
| **Python (FastAPI)** | Supported via profile | dependency-injector / Depends() | pytest |

Prinsip framework (dependency rule, spec-first, quality gates) berlaku di semua stack.

---

## 13. FAQ

### Q: Apakah power ini hanya untuk Next.js?
**A:** Tidak. Prinsipnya tech-agnostic. Next.js adalah default, tapi ada profile untuk Go dan Python. Lihat `tech-stack-profiles.md`.

### Q: Apakah harus pakai GitLab?
**A:** GitLab adalah default untuk CI/CD dan project management. Tapi git operations bisa pakai GitHub/Bitbucket. CI/CD pipeline perlu diadaptasi.

### Q: Bagaimana jika deadline mendesak?
**A:** Aktifkan Fast Track Mode. Beberapa gate boleh di-skip tapi semua skip di-track sebagai tech debt yang harus dibayar.

### Q: Apakah cocok untuk solo developer?
**A:** Ya. Aktifkan Solo Mode untuk mengurangi ceremony tanpa mengorbankan quality.

### Q: Bagaimana jika session Kiro terputus?
**A:** AI Agent membaca `docs/CURRENT-STATE.md` dan melanjutkan dari "Next Action". Tidak perlu jelaskan ulang.

### Q: Apakah coverage 80% terlalu tinggi?
**A:** Tidak. 80% adalah minimum. Untuk auth/payment/security, target 95%. Coverage tanpa meaningful assertion tidak dihitung — lihat `test-writing-patterns.md`.

### Q: Bagaimana mengukur efektivitas AI?
**A:** Gunakan AI Quality Scorecard (trigger: "sprint scorecard"). Mengukur acceptance rate, rework rate, spec compliance, dan governance compliance.

### Q: Apakah semua 15 layer harus dijalankan?
**A:** Layer 0-8 WAJIB dijalankan sequential — DILARANG skip. Setiap layer menghasilkan artifact yang dibutuhkan layer berikutnya. Untuk project existing yang sudah punya artifact, AI boleh validate artifact existing tanpa generate ulang, tapi layer tetap tidak boleh di-skip. Layer 9-14 adalah execution layers yang selalu aktif via hooks.

### Q: Bagaimana onboarding developer baru?
**A:** Developer baru cukup buka project → `.kiro/steering/` otomatis aktif → arsitektur dan conventions langsung tersedia tanpa briefing manual.

---

## Statistik Power

| Metric | Value |
|--------|-------|
| Version | 1.5.0 |
| Total Steering Files | 27 |
| Automated Layers | 11/15 (73%) |
| Always-Included Files | 4 |
| Kiro Hooks Defined | 9 |
| Tech Stacks Supported | 3 (Next.js, Go, Python) |
| Operating Modes | 4 (Enterprise, Solo, Fast Track, Zero Touch) |
| Integrated Standards | 27 |
| MCP Servers | 2 (GitLab, Git) |

---

## Key Principles

1. **Validate Before Execute** — AI wajib validasi sebelum coding
2. **Specification First** — AI bekerja lebih baik dengan spec lengkap
3. **Governance Over Chaos** — AI tanpa governance = technical debt at scale
4. **Context Is Everything** — AI butuh context terstruktur dan reusable
5. **Small Context Wins** — Issue-driven lebih stabil dari full-app generation

---

## Final Insight

> AI bukan pengganti software engineering. AI membutuhkan software engineering discipline yang jauh lebih kuat.
>
> Tanpa governance, specifications, architecture, validation, dan observability — AI hanya akan menghasilkan **Technical Debt at Scale**.
>
> Sedangkan AI + SDLC + Governance + Specifications + Observability akan menghasilkan **Enterprise Engineering Acceleration**.

---

*Enterprise AI-Native SDLC Framework v1.5.0 — Built with Kiro*
