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
- **presentation-material.md** - Generate materi presentasi project (UI/UX, Design, Construction, Quality, Deployment, Security)
- **tech-stack-profiles.md** - Adaptasi framework ke multi-stack: Next.js, Go, Python (FastAPI)

## Agent Workflow Rules

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
4. **Stage + Initial commit** — `git add` + `git commit` (repo_path WAJIB di-pass)
5. **`create_project`** (GitLab MCP) - Buat repository di GitLab
6. **`git remote add origin`** — via shell command (CWD = repo_path), karena Git MCP tidak punya tool remote add
7. **`git push -u origin main`** — Push initial commit
8. **Buat branch `develop`** + push ke remote
9. **Setup branch protection** (main, develop)
10. **Buat labels** sesuai framework (type::*, status::*, priority::*, team::*)
11. **Buat `.gitlab-ci.yml`** dengan pipeline dasar

Semua ini dilakukan otomatis menggunakan Git MCP dan GitLab MCP setelah user menjawab pertanyaan project di awal.

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
