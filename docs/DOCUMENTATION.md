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
5. [26 Steering Files](#5-26-steering-files)
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
**Output:** `docs/product/vision.md`, `docs/product/roadmap.md`  
**Automation:** Manual (keputusan bisnis)

### Layer 1 — Requirement Intake
**Tujuan:** AI membaca berbagai input source (PRD, meeting notes, user feedback) dan mengekstrak requirements terstruktur.  
**Output:** `docs/requirements/extracted/*.md`  
**Automation:** Manual (input terlalu bervariasi)

### Layer 2 — Requirement Validation
**Tujuan:** Validasi requirements sebelum eksekusi — cek ambiguity, completeness, conflicts, feasibility.  
**Output:** `docs/requirements/validation/validation-report.md`  
**Automation:** ✅ Hook `fileCreated` pada `docs/requirements/**` — auto-validate  
**Gate:** BLOCK jika ada critical issue

### Layer 3 — Spec-Driven Development
**Tujuan:** Idea → Specification → Validation → Architecture → Tasks → Code  
**Output:** `docs/specs/srs/[feature]-spec.md`  
**Automation:** ✅ Hook `preTaskExecution` — enforce spec-first untuk fitur kompleks  
**Gate:** BLOCK coding tanpa spec (fitur kompleks)

### Layer 4 — Design System
**Tujuan:** System Design, Technical Design, UI/UX Design, Security Design  
**Output:** `docs/design/[type]/[feature].md`  
**Automation:** ✅ Hook `postTaskExecution` — validate code vs design document  
**Gate:** Informasikan deviasi, suggest update

### Layer 5 — AI Governance
**Tujuan:** Mencegah hallucination dan AI chaos melalui policy, standards, compliance.  
**Output:** `docs/governance/ai-policy.md`, approved tools, prompt standards  
**Automation:** Partial — enforced via CI/CD pipeline

### Layer 6 — AI Engineering Skills
**Tujuan:** Reusable workflow dan standards (create-api, create-usecase, create-test, dll).  
**Output:** `.kiro/skills/*.md`  
**Automation:** Partial — auto-loaded via fileMatch patterns

### Layer 7 — Team Extension Skills
**Tujuan:** Global + team-specific standards yang extend Layer 6.  
**Output:** `.kiro/steering/team-*.md`  
**Automation:** Partial — auto-loaded via fileMatch patterns

### Layer 8 — Issue-Driven Development
**Tujuan:** 1 issue = 1 bounded context. Mengurangi hallucination dan context overflow.  
**Output:** GitLab Issues (Epic → Feature → Task), branches, MRs  
**Automation:** ✅ Full GitLab integration — branch per issue, auto-push, MR creation

### Layer 9 — AI Agent Orchestration
**Tujuan:** Multi-agent workflow — Backend Agent, Frontend Agent, DevOps Agent, QA Agent.  
**Output:** Orchestrated task execution  
**Automation:** Conceptual (tergantung evolusi Kiro multi-agent)

### Layer 10 — Continuous Context
**Tujuan:** Artifact dari setiap layer menjadi context untuk layer berikutnya. AI tidak kehilangan memory.  
**Output:** `docs/CONTEXT-INDEX.md`, `docs/CURRENT-STATE.md`  
**Automation:** ✅ Hook `preTaskExecution` — auto-load context + maintain index

### Layer 11 — AI Review & Validation
**Tujuan:** AI sebagai reviewer — cek architecture, security, performance, accessibility.  
**Output:** Review feedback, compliance report  
**Automation:** ✅ Post-task validation — architecture + security + design compliance

### Layer 12 — Automated Quality Gates
**Tujuan:** GitLab CI/CD pipeline yang block merge jika quality tidak terpenuhi.  
**Output:** Pipeline results, coverage reports  
**Automation:** ✅ Full CI/CD — Lint → Typecheck → Test → Coverage → Security → Build → Deploy  
**Gate:** Coverage < 80% = BLOCK merge

### Layer 13 — Observability & Feedback
**Tujuan:** Mengukur system health, AI performance, engineering quality.  
**Output:** Logs, metrics, traces, alerts, dashboards  
**Automation:** ✅ Hook `postToolUse` — remind observability saat fitur baru  
**Phased:** Day 1 (logger + Sentry) → Sprint 1 (metrics) → Sprint 2+ (tracing + alerts)

### Layer 14 — Continuous Learning
**Tujuan:** Belajar dari production — bugs, performance, review patterns, AI effectiveness.  
**Output:** `docs/learnings/*.md`, `docs/retrospectives/*.md`, `docs/adr/*.md`  
**Automation:** ✅ 3 hooks — Bug Learning Capture, Sprint Retro Generator, ADR Reminder

---

## 5. 26 Steering Files

### Always Included (aktif di setiap session)

| File | Fungsi |
|------|--------|
| `architecture-standards.md` | Clean Architecture + DDD + Awilix DI — acuan arsitektur WAJIB |
| `test-writing-patterns.md` | Pattern testing per-layer dengan contoh TypeScript |
| `project-memory.md` | Context Index + Current State untuk resume antar session |

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

### Operational Files (7 files)

| File | Fungsi |
|------|--------|
| `git-workflow-automation.md` | Git init, commit convention, auto-push, branch strategy, MR |
| `gitlab-cicd-setup.md` | Setup lengkap GitLab CI/CD |
| `project-structure.md` | Folder structure, naming, quickstart |
| `requirement-traceability.md` | PRD → Code → Test traceability |
| `presentation-material.md` | Generate materi presentasi |
| `tech-stack-profiles.md` | Adaptasi ke Go, Python (FastAPI) |
| `fast-track-mode.md` | Fast Track, Quality Scorecard, Health Check |

---

## 6. Mode Operasi

### Enterprise Mode (Default)

Full ceremony untuk tim enterprise:
- Sprint planning dengan milestones
- Full issue board (Backlog → Ready → In Progress → Review → Done)
- Branch strategy: main + develop + feature
- MR approval wajib
- Full label taxonomy
- Wiki documentation
- Formal retrospectives

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

### Pre-Task Execution Hooks

| Hook | Fungsi |
|------|--------|
| Git Init & Working Directory Check | Pastikan git repo ready |
| Spec-First Gate | Block coding tanpa spec (fitur kompleks) |
| Load Context Before Task | Load CURRENT-STATE + CONTEXT-INDEX |

### Post-Task Execution Hooks

| Hook | Fungsi |
|------|--------|
| Auto Push After Task | Lint → Typecheck → Test → Commit → Push |
| Design Compliance Check | Validate code vs design document |
| Bug Learning Capture | Auto-generate learning doc untuk bug fix |
| Sprint End Auto Check | Auto health check + offer retro saat sprint task terakhir |
| Save Project State | Update CURRENT-STATE + CONTEXT-INDEX |

### Post-Tool-Use Hooks

| Hook | Fungsi |
|------|--------|
| Observability Check | Remind tambah logging/metrics di fitur baru |
| Impact Analysis | Detect impacted files saat core/ berubah |

### File Event Hooks

| Hook | Trigger | Fungsi |
|------|---------|--------|
| Requirement Validation Gate | `docs/requirements/**` created | Auto-validate requirements |

### User-Triggered Hooks

| Hook | Fungsi |
|------|--------|
| Sprint Retrospective Generator | Generate retro document |
| Sprint Quality Scorecard | Generate quality metrics |
| Framework Health Check | Scan project compliance |

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
3. Mulai development dengan Layer 3 (Spec-Driven)

### Development Cycle (Per Sprint)

```
1. Sprint Planning → buat issues di GitLab
2. Per Task:
   a. AI load context (spec, design, ADR)
   b. AI buat spec (jika belum ada)
   c. AI implement code (ikuti architecture)
   d. AI jalankan lint + typecheck + test
   e. AI commit + push
   f. AI update issue status
3. Sprint End → MR + Retrospective + Scorecard
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
**A:** Tidak harus sequential. Layer 0-4 (planning) bisa di-skip untuk project existing. Layer 8-14 (execution) adalah inti yang selalu aktif.

### Q: Bagaimana onboarding developer baru?
**A:** Developer baru cukup buka project → `.kiro/steering/` otomatis aktif → arsitektur dan conventions langsung tersedia tanpa briefing manual.

---

## Statistik Power

| Metric | Value |
|--------|-------|
| Version | 1.5.0 |
| Total Steering Files | 26 |
| Automated Layers | 11/15 (73%) |
| Always-Included Files | 3 |
| Kiro Hooks Defined | 9 |
| Tech Stacks Supported | 3 (Next.js, Go, Python) |
| Operating Modes | 4 (Enterprise, Solo, Fast Track, Zero Touch) |
| Integrated Standards | 26 |
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
