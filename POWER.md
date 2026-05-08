---
name: "enterprise-ai-sdlc"
displayName: "Enterprise AI-Native SDLC Framework"
description: "Framework SDLC berbasis AI untuk enterprise yang menggabungkan Spec-Driven Development, AI Governance, Multi-Agent Orchestration, dan GitLab CI/CD. Mencegah AI chaos dan pilot graveyard dengan pendekatan terstruktur."
keywords: ["sdlc", "ai-native", "spec-driven", "governance", "gitlab", "ci-cd", "enterprise", "multi-agent"]
author: "Enterprise AI Engineering Team"
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
- **project-structure.md** - Struktur folder project yang direkomendasikan
- **quickstart.md** - Panduan cepat memulai framework
- **requirement-traceability.md** - End-to-end flow dari PRD User Story hingga Merge dengan traceability matrix

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

## Getting Started

Baca steering file `quickstart.md` untuk panduan cepat memulai framework ini di project Anda.

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

**PENTING:** Power ini membaca credentials dari environment variable system Anda. Set variable berikut sebelum menggunakan power:

- **`GITLAB_PERSONAL_ACCESS_TOKEN`**: GitLab Personal Access Token Anda.
  - **Cara mendapatkan:**
    1. Buka GitLab - User Settings - Access Tokens
    2. Buat token baru dengan scope: `api`, `read_repository`, `write_repository`
    3. Copy token yang dihasilkan
  - **Cara set:**
    - **Linux/Mac:** Tambahkan di `~/.bashrc` atau `~/.zshrc`:
      ```bash
      export GITLAB_PERSONAL_ACCESS_TOKEN="glpat-xxxxxxxxxxxx"
      ```
    - **Windows:** Set di System Environment Variables atau PowerShell profile:
      ```powershell
      [Environment]::SetEnvironmentVariable("GITLAB_PERSONAL_ACCESS_TOKEN", "glpat-xxxxxxxxxxxx", "User")
      ```

- **`GITLAB_API_URL`**: URL API GitLab instance Anda.
  - **Untuk gitlab.com:** `https://gitlab.com/api/v4`
  - **Untuk self-hosted:** `https://your-gitlab-domain.com/api/v4`
  - **Cara set:**
    - **Linux/Mac:**
      ```bash
      export GITLAB_API_URL="https://gitlab.com/api/v4"
      ```
    - **Windows:**
      ```powershell
      [Environment]::SetEnvironmentVariable("GITLAB_API_URL", "https://gitlab.com/api/v4", "User")
      ```

**Keuntungan pendekatan ini:**
- Tidak perlu edit file power
- Setiap anggota tim pakai token masing-masing
- Token tidak tersimpan di repository
- Power bisa di-share tanpa risiko credential leak

## Automated Project Setup

Saat user konfirmasi membuat project baru, AI Agent akan otomatis:

1. **`git init`** - Inisialisasi Git repository lokal
2. **`create_project`** (GitLab) - Buat repository di GitLab
3. **Buat struktur folder** sesuai framework (docs/, .kiro/, src/, tests/, .gitlab/)
4. **Buat `.gitignore`** sesuai tech stack
5. **Buat `.gitlab-ci.yml`** dengan pipeline dasar
6. **Push initial commit** ke GitLab
7. **Setup branch protection** (main, develop)
8. **Buat labels** sesuai framework (type::*, status::*, priority::*, team::*)
9. **Buat issue templates** di repository

Semua ini dilakukan otomatis menggunakan Git MCP dan GitLab MCP setelah user menjawab pertanyaan project di awal.
