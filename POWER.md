---
name: "enterprise-ai-sdlc"
displayName: "Enterprise AI-Native SDLC Framework"
description: "Framework SDLC berbasis AI untuk enterprise yang menggabungkan Spec-Driven Development, AI Governance, Multi-Agent Orchestration, dan GitLab CI/CD. Mencegah AI chaos dan pilot graveyard dengan pendekatan terstruktur."
keywords: ["sdlc", "ai-native", "spec-driven", "governance", "gitlab", "ci-cd", "enterprise", "multi-agent"]
author: "ricko_c_putra@telkomsel.co.id"
---

# Enterprise AI-Native SDLC Framework

## Overview

Framework ini merupakan gabungan dari:
- AI-Native SDLC
- Spec-Driven Development (SDD)
- AI Governance Framework
- AI-Powered Developer Experience (DevEx)
- Enterprise Engineering Workflow
- Continuous Context Accumulation
- Issue-Driven Development
- Multi-Agent Engineering Workflow

**Tujuan:**
- Menghindari AI chaos
- Menghindari pilot graveyard
- Membuat AI development scalable
- Menjaga engineering quality
- Meningkatkan developer experience
- Menjadikan AI sebagai operating layer dalam SDLC

## Paradigm Shift

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

AI bukan sekadar code generator. AI menjadi **AI Operating Layer for SDLC**.

## Framework Layers

Framework ini terdiri dari 15 layer yang saling terhubung:

| Layer | Nama | Deskripsi |
|-------|------|-----------|
| 0 | Product Vision | Arah produk, business value, roadmap |
| 1 | Requirement Intake | AI membaca berbagai input source |
| 2 | Requirement Validation | Validate before execute |
| 3 | Spec-Driven Development | Idea → Spec → Validation → Architecture → Tasks → Code |
| 4 | Design System | System, Technical, UI/UX, Security Design |
| 5 | AI Governance | Mencegah hallucination dan AI chaos |
| 6 | AI Engineering Skills | Reusable workflow dan standards |
| 7 | Team Extension Skills | Global + team-specific standards |
| 8 | Issue-Driven Development | 1 issue = 1 bounded context |
| 9 | AI Agent Orchestration | Multi-agent workflow |
| 10 | Continuous Context | Artifact sebagai context untuk stage berikutnya |
| 11 | AI Review & Validation | AI sebagai reviewer |
| 12 | Automated Quality Gates | GitLab CI/CD Pipeline |
| 13 | Observability & Feedback | System health dan AI performance |
| 14 | Continuous Learning | Belajar dari production |

## Available Steering Files

Setiap layer memiliki steering file tersendiri untuk panduan detail:

- **layer-0-product-vision.md** - Menentukan arah produk, business value, dan roadmap
- **layer-1-requirement-intake.md** - AI sebagai Business Analyst Agent
- **layer-2-requirement-validation.md** - Validasi requirement sebelum eksekusi
- **layer-3-spec-driven-development.md** - Specification-first approach
- **layer-4-design-system.md** - System, Technical, UI/UX, dan Security Design
- **layer-5-ai-governance.md** - Policy, standards, dan compliance
- **layer-6-ai-skills.md** - Reusable engineering skills
- **layer-7-team-extension.md** - Team-specific standards dan extensions
- **layer-8-issue-driven-dev.md** - Issue-driven development dengan GitLab
- **layer-9-agent-orchestration.md** - Multi-agent workflow
- **layer-10-continuous-context.md** - Context accumulation strategy
- **layer-11-ai-review.md** - AI review dan validation
- **layer-12-quality-gates.md** - GitLab CI/CD pipeline dan quality gates
- **layer-13-observability.md** - Monitoring, metrics, dan feedback loop
- **layer-14-continuous-learning.md** - Continuous improvement dari production
- **gitlab-cicd-setup.md** - Setup lengkap GitLab CI/CD untuk framework ini
- **project-structure.md** - Struktur folder project, naming conventions, dan quickstart guide
- **git-workflow-automation.md** - Git init, remote setup, commit convention, auto-push, branch strategy, dan MR automation
- **requirement-traceability.md** - End-to-end flow dari PRD User Story hingga Merge dengan traceability matrix
- **architecture-standards.md** - **(ALWAYS INCLUDED)** Clean Architecture + DDD + Dependency Injection (Awilix) — acuan arsitektur WAJIB untuk semua layer
- **test-writing-patterns.md** - **(ALWAYS INCLUDED)** Pattern testing per-layer dengan contoh konkret (Use Case, Repository, ViewModel, Mapper, Middleware)
- **project-memory.md** - **(ALWAYS INCLUDED)** Context Index + Current State — memastikan AI selalu tahu state project antar session
- **presentation-material.md** - Generate materi presentasi project (UI/UX, Design, Construction, Quality, Deployment, Security)
- **tech-stack-profiles.md** - Adaptasi framework ke multi-stack: Next.js, Go, Python (FastAPI)
- **fast-track-mode.md** - Fast Track Mode, AI Quality Scorecard, Dependency Analysis, Framework Health Check

## Agent Workflow Rules

### Auto-Detect & Self-Heal (WAJIB — Jalankan di Awal SETIAP Session)

Saat power ini diaktifkan atau AI Agent mulai bekerja di sebuah project, AI Agent **WAJIB** menjalankan auto-detect berikut **SEBELUM** melakukan apapun (termasuk sebelum tanya project):

```
[Power diaktifkan / Session dimulai]
    ↓
[AUTO-DETECT 1: Apakah ini project existing yang sudah pakai framework?]
    → Cek: apakah ada .kiro/ folder?
    → Cek: apakah ada docs/ folder?
    → Cek: apakah ada src/ folder?
    → Jika YA (minimal 1 ada) → ini project existing, jalankan detect 2-4
    → Jika TIDAK → ini project baru, lanjut ke "Tanyakan Project"
    ↓
[AUTO-DETECT 2: Apakah .kiro/hooks/ sudah ada dan lengkap?]
    → Cek: apakah folder .kiro/hooks/ ada?
    → Jika TIDAK ADA → GENERATE semua 8 hook files otomatis
    → Jika ADA → cek apakah lengkap (8 files)?
    → Jika kurang → generate yang missing
    → Informasikan user: "Hooks framework sudah di-setup/updated."
    ↓
[AUTO-DETECT 3: Apakah .kiro/steering/ sudah ada dan lengkap?]
    → Cek: apakah folder .kiro/steering/ ada?
    → Jika TIDAK ADA → GENERATE steering files (architecture-standards, test-writing-patterns, coding-conventions)
    → Jika ADA → cek apakah lengkap (3 files minimum)?
    → Jika kurang → generate yang missing
    ↓
[AUTO-DETECT 4: Apakah docs/CURRENT-STATE.md ada?]
    → Jika ADA → baca dan resume dari last state
    → Jika TIDAK ADA → buat dengan template kosong
    ↓
[Lanjut ke workflow normal]
```

**Hook Files yang WAJIB Ada (8 files):**

| File | Cek Keberadaan |
|------|---------------|
| `.kiro/hooks/architect-gate.json` | ✅ harus ada |
| `.kiro/hooks/security-review.json` | ✅ harus ada |
| `.kiro/hooks/observability-check.json` | ✅ harus ada |
| `.kiro/hooks/qa-devops-post-task.json` | ✅ harus ada |
| `.kiro/hooks/bug-learning-capture.json` | ✅ harus ada |
| `.kiro/hooks/sprint-retrospective.json` | ✅ harus ada |
| `.kiro/hooks/quality-scorecard.json` | ✅ harus ada |
| `.kiro/hooks/health-check.json` | ✅ harus ada |

**Rules:**
- Auto-detect WAJIB jalan di awal SETIAP session (bukan hanya project baru)
- Jika hooks missing → generate TANPA tanya user (self-heal)
- Jika steering missing → generate TANPA tanya user (self-heal)
- Informasikan user apa yang di-generate: "Framework hooks/steering sudah di-setup."
- JANGAN block workflow — detect + heal harus cepat (< 30 detik)
- Setelah auto-detect selesai → lanjut ke workflow normal (tanya project / resume state)

**Alasan:**
- Project existing yang di-setup sebelum fitur hooks ditambahkan akan otomatis ter-update
- User tidak perlu ingat "generate hooks" secara manual
- Framework self-healing — jika ada file yang terhapus, otomatis dibuat ulang
- Memastikan Layer 9, 13, 14 SELALU aktif di setiap project

---

### Tampilkan Agent Aktif (WAJIB)

Saat mengerjakan task, AI Agent **WAJIB** menampilkan agent mana yang sedang bekerja menggunakan format banner:

```
━━━ 🏗️ ARCHITECT AGENT ━━━
━━━ ⚙️ BACKEND AGENT ━━━
━━━ 🎨 FRONTEND AGENT ━━━
━━━ 🧪 QA AGENT ━━━
━━━ 🔒 SECURITY AGENT ━━━
━━━ 🚀 DEVOPS AGENT ━━━
━━━ 📋 BA AGENT ━━━
━━━ 📚 LEARNING AGENT ━━━
```

**Rules:**
- WAJIB tampilkan banner saat mulai bekerja sebagai agent tertentu
- WAJIB tampilkan banner saat berganti role
- Sertakan summary apa yang dilakukan + status (✅/❌/⚠️) di akhir phase
- Format ini WAJIB di setiap task execution — user harus selalu tahu siapa yang bekerja
- Lihat `layer-9-agent-orchestration.md` section "Format Display Agent" untuk detail lengkap

---

### Tanyakan Project Terlebih Dahulu (WAJIB)

Saat power ini diaktifkan atau dipanggil, AI Agent **WAJIB** menanyakan ke user terlebih dahulu:

**Pertanyaan wajib sebelum eksekusi:**
```
"Selamat datang di Enterprise AI-Native SDLC Framework!

Sebelum kita mulai, saya perlu memahami project yang akan dikerjakan:

1. Apa nama project yang ingin dibuat/dikerjakan?
2. Deskripsi singkat project (apa yang ingin diselesaikan)?
3. Tech stack yang diinginkan? (contoh: Next.js, Go, Python, dll)
4. Apakah ini project baru atau project existing?
5. Layer mana yang ingin dikerjakan terlebih dahulu?

Silakan jawab pertanyaan di atas agar saya bisa menyesuaikan framework dengan kebutuhan project Anda."
```

**Rules:**
- JANGAN langsung eksekusi layer tanpa mengetahui context project
- Tunggu jawaban user sebelum memulai pengerjaan
- Sesuaikan output setiap layer dengan context project yang diberikan user
- Jika user hanya menjawab sebagian, tanyakan yang belum dijawab

**Alasan:**
- Framework ini bersifat generic, perlu disesuaikan dengan project spesifik
- Mencegah output yang tidak relevan
- Memastikan semua artifact yang dihasilkan sesuai kebutuhan user
- Sesuai prinsip "Context Is Everything"

---

### Konfirmasi Setelah Setiap Layer (WAJIB)

Saat mengerjakan framework ini secara bertahap (layer by layer), AI Agent **WAJIB** menanyakan konfirmasi ke user setelah menyelesaikan setiap layer sebelum melanjutkan ke layer berikutnya.

**Pattern:**
```
[Selesaikan pengerjaan Layer N]
→ Tanyakan: "Layer [N] - [Nama Layer] sudah selesai. Apakah hasilnya sudah sesuai, atau ada yang perlu direvisi sebelum lanjut ke layer berikutnya?"
→ Tunggu konfirmasi user
→ Jika user setuju → lanjut ke Layer N+1
→ Jika user minta revisi → revisi dulu, lalu tanya lagi
```

**Alasan:**
- Mencegah pengerjaan yang salah arah berlanjut ke layer berikutnya
- Memberikan user kontrol atas output di setiap tahap
- Memungkinkan iterasi cepat per layer
- Sesuai prinsip "Validate Before Execute"

---

## Solo Mode (Opsional)

Untuk developer yang bekerja sendiri (solo), beberapa ceremony enterprise bisa di-simplify tanpa mengorbankan quality.

### Aktivasi

User cukup menyatakan di awal:
```
"Saya bekerja solo / ini project personal"
```

AI Agent akan otomatis menyesuaikan workflow.

### Yang Berubah di Solo Mode

| Aspek | Enterprise Mode | Solo Mode |
|-------|----------------|-----------|
| MR Approval | Wajib 1 reviewer | Self-merge allowed |
| Sprint Planning | Formal milestone + issues | Simplified task list |
| Issue Board | Full board (Backlog→Done) | Minimal (Todo→Done) |
| Branch Strategy | main + develop + feature | main + feature (tanpa develop) |
| Labels | Full taxonomy (type::, status::, priority::, team::) | Minimal (type:: saja) |
| Wiki | Full documentation | README.md cukup |
| Milestone | Per-sprint formal | Opsional |
| Code Review | AI Review + Human Review | AI Review saja |
| Commit Convention | Wajib (enforced via CI) | Wajib (tapi tanpa commitlint CI) |

### Yang TETAP SAMA (Tidak Boleh Di-skip)

Meskipun solo, quality engineering tetap dijaga:

- ✅ Clean Architecture + DI pattern (WAJIB)
- ✅ Lint + Typecheck + Test sebelum push (WAJIB)
- ✅ Coverage >= 80% (WAJIB)
- ✅ Conventional Commits (WAJIB, meskipun tanpa CI enforcement)
- ✅ .gitignore + security (WAJIB)
- ✅ Spec-Driven Development (WAJIB untuk fitur kompleks)
- ✅ AI Governance (WAJIB)

### Solo Mode Branch Strategy

```
main (production-ready)
└── feature/short-description
    └── commit → push → self-merge to main
```

- Tidak perlu branch `develop`
- Feature branch langsung merge ke `main`
- Tidak perlu MR approval (self-merge)
- Tetap gunakan feature branch (jangan push langsung ke main)

### Solo Mode Workflow

```
1. Buat feature branch dari main
2. Implement (ikuti architecture standards)
3. Lint + Typecheck + Test (WAJIB)
4. Commit (conventional)
5. Push ke feature branch
6. Merge ke main (tanpa approval)
7. Delete feature branch
```

### AI Agent Rules di Solo Mode

- JANGAN buat milestone kecuali user minta
- JANGAN buat issue board kompleks
- JANGAN enforce MR approval
- TETAP enforce architecture, testing, dan quality
- TETAP gunakan conventional commits
- TETAP jalankan lint/typecheck/test sebelum push
- Simplify output — kurangi ceremony, fokus ke delivery

---

## Key Principles

### 1. Validate Before Execute
AI wajib melakukan validasi sebelum coding.

### 2. Specification First
AI bekerja jauh lebih baik dengan specification lengkap.

### 3. Governance Over Chaos
AI tanpa governance akan menghasilkan technical debt.

### 4. Context Is Everything
AI membutuhkan context yang terstruktur dan reusable.

### 5. Small Context Wins
Issue-driven development lebih stabil dibanding full-app generation.

## Final Flow

```
Vision → Requirement Intake → Requirement Validation → Spec-Driven Development
→ System Design → Security Design → Technical Design → Governance Layer
→ AI Skills → GitLab Planning → Issue-Driven Development
→ AI Agent Orchestration → Continuous Context Accumulation
→ AI Review → CI/CD → Observability → Continuous Learning
```

## Final Insight

AI bukan pengganti software engineering. AI membutuhkan software engineering discipline yang jauh lebih kuat.

Tanpa governance, specifications, architecture, validation, dan observability — AI hanya akan menghasilkan **Technical Debt at Scale**.

Sedangkan AI + SDLC + Governance + Specifications + Observability akan menghasilkan **Enterprise Engineering Acceleration**.

## Integrated Standards

Power ini sudah mengintegrasikan konten dari 19 engineering steering files:

| # | Steering | Terintegrasi di |
|---|----------|-----------------|
| 01 | Project Architecture | layer-4-design-system.md |
| 02 | Code Pattern | layer-6-ai-skills.md |
| 03 | Configuration Convention | layer-5-ai-governance.md |
| 04 | Testing Pattern | layer-12-quality-gates.md |
| 05 | Security Guideline | layer-4-design-system.md |
| 06 | DI Pattern | layer-6-ai-skills.md |
| 07 | Review Checklist | layer-11-ai-review.md |
| 08 | Observability | layer-13-observability.md |
| 09 | Performance | layer-12-quality-gates.md |
| 10 | Release & CI/CD | layer-12-quality-gates.md, gitlab-cicd-setup.md |
| 11 | BFF Pattern | layer-6-ai-skills.md |
| 12 | Error Handling | layer-4-design-system.md |
| 13 | API Contract | layer-3-spec-driven-development.md |
| 14 | Specs Workflow | layer-3-spec-driven-development.md |
| 15 | ADR | layer-14-continuous-learning.md |
| 16 | Technical Debt | layer-14-continuous-learning.md |
| 17 | Incident Management | layer-13-observability.md |
| 18 | Architecture Evolution | layer-14-continuous-learning.md |
| 19 | CX API Playbook | layer-6-ai-skills.md |
| 20 | Commit Convention | git-workflow-automation.md |
| 21 | Git Workflow Automation | git-workflow-automation.md |
| 22 | Clean Architecture + DI Pattern | architecture-standards.md |
| 23 | Test Writing Patterns | test-writing-patterns.md |
| 24 | Tech Stack Profiles | tech-stack-profiles.md |
| 25 | Project Memory & Resume | project-memory.md |
| 26 | Fast Track Mode & Quality Scorecard | fast-track-mode.md |

## Getting Started

Baca steering file `project-structure.md` untuk panduan cepat memulai framework ini di project Anda.

---

## MCP Servers

Power ini menggunakan 2 MCP server:

### 1. GitLab MCP (`@zereight/mcp-gitlab`)

Menyediakan tools untuk:
- **create_project** - Membuat repository GitLab baru
- **create_issue** - Membuat issues (Epic, Feature, Task)
- **create_merge_request** - Membuat Merge Request
- **create_branch** - Membuat branch baru
- **push_files** - Push files ke repository
- **create_or_update_file** - Buat/update file di repository
- **get_file_contents** - Baca file dari repository
- **search_repositories** - Cari project
- Dan banyak lagi (pipelines, wiki, releases, milestones, labels)

### 2. Git MCP (`@cyanheads/git-mcp-server`)

Menyediakan tools untuk:
- **git_init** - Inisialisasi repository Git baru
- **git_clone** - Clone repository
- **git_commit** - Commit changes
- **git_push** - Push ke remote
- **git_pull** - Pull dari remote
- **git_branch** - Manage branches
- **git_status** - Cek status
- **git_log** - Lihat history
- **git_diff** - Lihat perubahan

## MCP Config Placeholders

**PENTING:** Power ini membaca credentials dari **system environment variable** menggunakan syntax `${VARIABLE_NAME}`.

### Setup Cepat

1. **Set environment variables di OS:**

   - **Windows (permanent):**
     ```powershell
     [Environment]::SetEnvironmentVariable("GITLAB_PERSONAL_ACCESS_TOKEN", "glpat-xxx", "User")
     [Environment]::SetEnvironmentVariable("GITLAB_API_URL", "https://gitlab.com/api/v4", "User")
     ```
   - **Linux/Mac:** Tambahkan di `~/.bashrc` atau `~/.zshrc`:
     ```bash
     export GITLAB_PERSONAL_ACCESS_TOKEN="glpat-xxxxxxxxxxxx"
     export GITLAB_API_URL="https://gitlab.com/api/v4"
     ```

2. **Approve env vars di Kiro:**
   - Buka Kiro Settings (Ctrl+,)
   - Cari **"Mcp Approved Env Vars"**
   - Tambahkan: `GITLAB_PERSONAL_ACCESS_TOKEN`, `GITLAB_API_URL`
   - Atau approve langsung dari popup security warning saat pertama kali

3. **Restart Kiro** agar environment variables terbaca oleh MCP servers.

4. Opsional: Buat `.env` di root project sebagai dokumentasi (JANGAN commit!)

### Variable yang Dibutuhkan

- **`GITLAB_PERSONAL_ACCESS_TOKEN`**: GitLab Personal Access Token
  - Scope: `api`, `read_repository`, `write_repository`
  - Buat di: GitLab → User Settings → Access Tokens

- **`GITLAB_API_URL`**: URL API GitLab instance
  - gitlab.com: `https://gitlab.com/api/v4`
  - Self-hosted: `https://your-gitlab-domain.com/api/v4`

**Keuntungan pendekatan ini:**
- Tidak perlu edit file power
- Setiap anggota tim pakai token masing-masing
- Token tidak tersimpan di repository
- Power bisa di-share tanpa risiko credential leak

## Automated Project Setup

Saat user konfirmasi membuat project baru, AI Agent akan otomatis:

0. **Tentukan `repo_path`** — Absolute path ke project directory (WAJIB untuk semua Git MCP calls)
1. **`git_init`** - Inisialisasi Git repository lokal (path = repo_path)
2. **Buat `.gitignore`** sesuai tech stack
3. **Buat struktur folder** sesuai framework (docs/, .kiro/, src/, tests/, .gitlab/)
4. **Generate workspace steering files** — Copy architecture & test standards ke `.kiro/steering/` project user
5. **Generate Kiro hooks** — Buat hook files di `.kiro/hooks/` agar automation aktif (lihat Step 4.5)
6. **Stage + Initial commit** — `git add` + `git commit` (repo_path WAJIB di-pass)
7. **`create_project`** (GitLab MCP) - Buat repository di GitLab
8. **`git remote add origin`** — via shell command (CWD = repo_path)
9. **`git push -u origin main`** — Push initial commit
10. **Buat branch `develop`** + push ke remote
11. **Setup branch protection** (main, develop)
12. **Buat labels** sesuai framework (type::*, status::*, priority::*, team::*)
13. **Buat `.gitlab-ci.yml`** dengan pipeline dasar

Semua ini dilakukan otomatis menggunakan Git MCP dan GitLab MCP setelah user menjawab pertanyaan project di awal.

### Step 4: Generate Workspace Steering Files (WAJIB)

AI Agent **WAJIB** membuat file steering di `.kiro/steering/` project user agar arsitektur dan test patterns menjadi **acuan permanen** — aktif di setiap chat session tanpa perlu memanggil power lagi.

**Files yang WAJIB di-generate:**

| File Target (di project user) | Source (dari power ini) | Inclusion |
|-------------------------------|------------------------|-----------|
| `.kiro/steering/architecture-standards.md` | `steering/architecture-standards.md` | `always` |
| `.kiro/steering/test-writing-patterns.md` | `steering/test-writing-patterns.md` | `always` |
| `.kiro/steering/coding-conventions.md` | Generated (ringkasan dari power) | `always` |

**Cara generate:**
1. Baca konten `architecture-standards.md` dari power ini
2. Tulis ke `<repo_path>/.kiro/steering/architecture-standards.md`
3. Baca konten `test-writing-patterns.md` dari power ini
4. Tulis ke `<repo_path>/.kiro/steering/test-writing-patterns.md`
5. Generate `coding-conventions.md` ringkas (commit convention, naming, lint rules)

**Template `coding-conventions.md`:**
```markdown
---
inclusion: always
---

# Coding Conventions

## Commit Convention
- Format: `<type>(<scope>): <subject>`
- Types: feat, fix, docs, style, refactor, test, chore, perf, ci, build, revert
- Subject: lowercase, imperative, max 50 chars, no period

## Naming Convention
- Files: kebab-case (user-service.ts)
- Classes/Types: PascalCase (PaymentRepository)
- Functions/Variables: camelCase (getPaymentDetail)
- Constants: UPPER_SNAKE_CASE (MAX_RETRY)
- DI Keys: camelCase (paymentRepository)

## Pre-Push Validation (WAJIB)
Sebelum push, WAJIB jalankan dan SEMUA harus pass:
1. `npm run lint` — 0 errors
2. `npm run typecheck` — 0 errors
3. `npm run test:unit -- --coverage` — all pass, coverage >= 80%

## Architecture Rules
- Ikuti Clean Architecture: core → infrastructure → presentation → app
- Semua dependency via DI container (Awilix)
- Core TIDAK BOLEH import dari infrastructure/presentation
- Lihat `.kiro/steering/architecture-standards.md` untuk detail lengkap
```

**Keuntungan:**
- User tidak perlu panggil power setiap kali untuk mendapat guidance arsitektur
- Setiap kali user chat dengan Kiro di project tersebut, arsitektur standards otomatis aktif
- Planning dan coding selalu mengacu pada arsitektur yang benar
- Tim baru yang join project langsung mendapat context arsitektur

**Rules:**
- WAJIB generate saat project setup (step 4)
- Jika `.kiro/steering/` sudah ada file dengan nama sama → tanyakan user apakah mau overwrite
- File yang di-generate HARUS di-commit ke repository (bukan di .gitignore)
- Jika user update arsitektur di power → informasikan bahwa project steering perlu di-sync

### Step 4.5: Generate Kiro Hooks (WAJIB)

AI Agent **WAJIB** membuat hook files di `.kiro/hooks/` project user agar automation framework (Layer 9, 13, 14) **benar-benar aktif** — bukan hanya dokumentasi.

**Tanpa step ini, hooks hanya ada sebagai teks di steering files dan TIDAK akan tertrigger.**

**Files yang WAJIB di-generate:**

| File Target | Trigger | Fungsi |
|-------------|---------|--------|
| `.kiro/hooks/architect-gate.json` | preTaskExecution | Validate spec + design sebelum coding |
| `.kiro/hooks/security-review.json` | postToolUse (write) | Security scan saat file ditulis |
| `.kiro/hooks/observability-check.json` | postToolUse (write) | Remind tambah logging/metrics |
| `.kiro/hooks/qa-devops-post-task.json` | postTaskExecution | Lint + test + push setelah task |
| `.kiro/hooks/bug-learning-capture.json` | postTaskExecution | Capture learning saat bug fix |
| `.kiro/hooks/sprint-retrospective.json` | userTriggered | Generate retrospective |
| `.kiro/hooks/quality-scorecard.json` | userTriggered | Generate quality scorecard |
| `.kiro/hooks/health-check.json` | userTriggered | Framework compliance scan |

**Hook File Contents (WAJIB persis seperti ini):**

#### `.kiro/hooks/architect-gate.json`
```json
{
  "name": "Architect Gate",
  "version": "1.0.0",
  "description": "Validate spec dan design sebelum mulai task",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: Architect Agent\n\nSebelum mulai task:\n1. Baca docs/CURRENT-STATE.md — resume dari session sebelumnya\n2. Baca docs/CONTEXT-INDEX.md — apa saja artifact yang tersedia\n3. Cek apakah spec ada untuk fitur ini (docs/specs/srs/)\n4. Cek apakah design document ada (docs/design/)\n5. Jika fitur kompleks dan spec/design belum ada → buat dulu, JANGAN langsung coding\n6. Jika sudah ada → validate bahwa task sesuai arsitektur\n7. Load relevant context (ADR, learnings) untuk menghindari kesalahan yang sama"
  }
}
```

#### `.kiro/hooks/security-review.json`
```json
{
  "name": "Security Review",
  "version": "1.0.0",
  "description": "Quick security scan saat file source ditulis",
  "when": {
    "type": "postToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: Security Agent\n\nJika file yang baru ditulis adalah source code di src/ (bukan docs/config/test):\n1. Quick scan: input validation ada? Sensitive data tidak di-log? Auth check ada?\n2. Jika finding CRITICAL → informasikan user, JANGAN lanjut\n3. Jika clean atau non-source file → skip silently"
  }
}
```

#### `.kiro/hooks/observability-check.json`
```json
{
  "name": "Observability Check",
  "version": "1.0.0",
  "description": "Remind tambah logging dan metrics di fitur baru",
  "when": {
    "type": "postToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Jika file yang baru ditulis adalah use case atau repository di src/ (bukan entity/mapper/test):\n- Apakah ada structured logging (logger.info/error) di entry dan error path?\n- Apakah ada metrics (counter/histogram) untuk operasi penting?\n- Jika BELUM → tambahkan sekarang. Inject logger dan metrics via DI.\n- Jika sudah ada atau file bukan use case/repository → skip."
  }
}
```

#### `.kiro/hooks/qa-devops-post-task.json`
```json
{
  "name": "QA + DevOps Post Task",
  "version": "1.2.0",
  "description": "Lint, typecheck, test, commit, push setelah task selesai",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: QA Agent → DevOps Agent\n\n**QA Phase:**\n1. npm run lint (jika error → fix dulu)\n2. npm run typecheck (jika error → fix dulu)\n3. npm run test:unit -- --coverage (semua harus pass, coverage >= 80%)\n4. Jika task docs-only → skip test, tetap lint+typecheck\n\n**DevOps Phase (hanya jika QA pass):**\n5. git add (relevant files only, BUKAN git add .)\n6. git commit (conventional format, reference issue di footer)\n7. git push ke feature branch\n8. Update docs/CURRENT-STATE.md\n9. Update docs/CONTEXT-INDEX.md jika ada artifact baru\n\nInformasikan user: Lint ✅/❌ | Typecheck ✅/❌ | Tests X/X | Coverage X% | Pushed to [branch]"
  }
}
```

#### `.kiro/hooks/bug-learning-capture.json`
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

#### `.kiro/hooks/sprint-retrospective.json`
```json
{
  "name": "Sprint Retrospective",
  "version": "1.0.0",
  "description": "Generate retrospective saat user trigger",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Generate sprint retrospective:\n1. Kumpulkan data: commits, tasks completed, coverage trend, issues reopened\n2. Buat docs/retrospectives/sprint-[N].md\n3. Isi: AI Performance, What Worked, What Didn't, Recurring Issues, Improvement Actions\n4. Buat GitLab issues untuk improvement actions (type::improvement)\n5. Update docs/CONTEXT-INDEX.md"
  }
}
```

#### `.kiro/hooks/quality-scorecard.json`
```json
{
  "name": "Quality Scorecard",
  "version": "1.0.0",
  "description": "Generate AI quality scorecard",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Generate quality scorecard:\n1. Collect metrics: coverage, lint errors, type errors, tasks completed, rework rate\n2. Buat docs/quality/sprint-[N]-scorecard.md\n3. Calculate overall score\n4. Compare dengan sprint sebelumnya (trend)\n5. Generate action items untuk metrics yang gagal\n6. Informasikan user: overall score + top recommendations"
  }
}
```

#### `.kiro/hooks/health-check.json`
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

**Cara Generate:**
1. Buat folder `<repo_path>/.kiro/hooks/` (jika belum ada)
2. Tulis setiap file JSON di atas ke folder tersebut
3. Commit bersama initial setup

**Rules:**
- WAJIB generate saat project setup
- Jika `.kiro/hooks/` sudah ada → tanyakan user apakah mau overwrite atau merge
- Hook files HARUS di-commit ke repository (agar tim lain juga dapat)
- Jika user mau disable hook tertentu → hapus file-nya atau rename ke `.disabled`

**⚠️ CRITICAL — Working Directory:**
- Git MCP (`@cyanheads/git-mcp-server`) MEMERLUKAN `repo_path` (absolute path) di SETIAP tool call
- Tanpa `repo_path`, semua git operations akan GAGAL
- Jika Git MCP tidak tersedia, fallback ke shell commands dengan CWD = repo_path
- Lihat steering file `git-workflow-automation.md` Section 2 untuk detail lengkap

**PENTING:** Jika project sudah ada tapi belum punya git repository, AI Agent WAJIB mendeteksi ini dan menjalankan setup di atas sebelum mulai development. Lihat steering file `git-workflow-automation.md` untuk detail lengkap.

## Auto-Push After Task/Sprint

AI Agent WAJIB melakukan push otomatis pada kondisi berikut:

| Trigger | Pre-condition | Action |
|---------|---------------|--------|
| Task selesai | Tests pass + Coverage >= 80% | Commit + Push ke feature branch |
| Sprint selesai | Full test suite pass + Coverage >= 80% | Push + Create Merge Request |
| Hotfix selesai | Tests pass + Coverage >= 80% | Push + Create MR ke main |
| Initial setup | — | Push ke main + develop |

**⚠️ BLOCKING RULES:**
- Test failure = **BLOCK push** — fix tests dulu
- Coverage < 80% = **BLOCK push** — tambah test dulu
- Coverage check di-skip HANYA untuk task docs-only atau config-only

Commit message WAJIB mengikuti Conventional Commits. Lihat steering file `git-workflow-automation.md` untuk format lengkap.

---

## Changelog

### v1.4.0 (2026-05-10)

**New Features:**
- ✅ **Project Memory** (`project-memory.md`, always-included) — Context Index + Current State untuk resume antar session
- ✅ **Fast Track Mode** (`fast-track-mode.md`) — Escape hatch formal untuk deadline pressure dengan debt tracking
- ✅ **AI Quality Scorecard** — Mengukur AI output quality per sprint (code quality, effectiveness, governance, debt)
- ✅ **Dependency & Impact Analysis** — Auto-detect impacted files saat core/ berubah
- ✅ **Framework Health Check** — Scan project compliance terhadap framework standards
- ✅ **Layer 2 Automation** — Requirement validation gate (auto-validate saat file dibuat)
- ✅ **Layer 3 Automation** — Spec-first enforcement (block coding tanpa spec)
- ✅ **Layer 4 Automation** — Design compliance check (validate code vs design)
- ✅ **Layer 10 Automation** — Context loading + Context Index auto-maintained
- ✅ **Layer 13 Automation** — Observability checklist + hook + phased approach
- ✅ **Layer 14 Automation** — Bug learning capture + sprint retro + ADR reminder

**Improvements:**
- 🔧 11/15 layers sekarang terintegrasi ke workflow (dari 5/15)
- 🔧 Total 25 steering files (dari 21)
- 🔧 8 Kiro hooks baru didefinisikan

### v1.3.0 (2026-05-10)

**New Features:**
- ✅ **Solo Mode** — Simplified workflow untuk solo developer (tanpa develop branch, tanpa MR approval, tanpa milestone formal)
- ✅ **Test Writing Patterns** (`test-writing-patterns.md`) — Pattern testing per-layer dengan contoh konkret TypeScript
- ✅ **Tech Stack Profiles** (`tech-stack-profiles.md`) — Adaptasi framework ke Go dan Python (FastAPI)
- ✅ **Error Recovery Flows** — 6 recovery scenario untuk stuck-state (MCP crash, push gagal, partial sprint)

**Improvements:**
- 🔧 Fix Git MCP working directory — `repo_path` (absolute path) wajib di setiap call
- 🔧 Fix CI `coverage-check` — gunakan `needs` + artifacts, hapus dependency `bc`, validasi 4 metrik
- 🔧 Add Windows long path fix — `esbuild-wasm` override di package.json
- 🔧 Add mandatory local validation (lint + typecheck + test) sebelum push
- 🔧 Architecture-standards.md sebagai always-included steering file
- 🔧 Post-task hook v1.2.0 — full validation pipeline (lint → typecheck → test → push)

**Breaking Changes:**
- ❌ `architecture-and-di.md` dihapus (merged ke `architecture-standards.md`)
- ❌ `quickstart.md` dihapus (merged ke `project-structure.md`)

### v1.2.0 (2026-05-09)

- Initial public release
- 15 layer framework
- GitLab + Git MCP integration
- 21 steering files
