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
    → Jika TIDAK ADA → GENERATE semua 9 hook files otomatis
    → Jika ADA → cek apakah lengkap (9 files)?
    → Jika kurang → generate yang missing
    → Informasikan user: "Hooks framework sudah di-setup/updated."
    ↓
[AUTO-DETECT 3: Apakah .kiro/steering/ sudah ada dan lengkap?]
    → Cek: apakah folder .kiro/steering/ ada?
    → Jika TIDAK ADA → GENERATE 3 steering files (architecture-standards, test-writing-patterns, coding-conventions) — generate compact version, BUKAN copy dari power
    → Jika ADA → cek apakah lengkap (3 files minimum)?
    → Jika kurang → generate yang missing
    ↓
[AUTO-DETECT 4: Apakah docs/CURRENT-STATE.md ada?]
    → Jika ADA → baca dan resume dari last state
    → Jika TIDAK ADA → buat dengan template kosong
    ↓
[AUTO-DETECT 5: Apakah .kiro/settings/mcp.json ada dengan autoApprove?]
    → Jika TIDAK ADA → GENERATE mcp.json dengan autoApprove list lengkap
    → Jika ADA tapi tanpa autoApprove → tambahkan autoApprove list
    → Jika ADA dan lengkap → skip
    ↓
[Lanjut ke workflow normal]
```

**Hook Files yang WAJIB Ada (9 files):**

| File | Cek Keberadaan |
|------|---------------|
| `.kiro/hooks/architect-gate.json` | ✅ harus ada |
| `.kiro/hooks/code-quality-scan.json` | ✅ harus ada |
| `.kiro/hooks/qa-devops-post-task.json` | ✅ harus ada |
| `.kiro/hooks/bug-learning-capture.json` | ✅ harus ada |
| `.kiro/hooks/metrics-collector.json` | ✅ harus ada |
| `.kiro/hooks/sprint-end-auto-check.json` | ✅ harus ada (auto health check) |
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

### Help Command — "What's Next?" (Kapan Saja)

User bisa bertanya **kapan saja** untuk mendapat guidance tentang apa yang harus dilakukan selanjutnya. AI Agent WAJIB merespons dengan context-aware guidance.

**Trigger phrases:**
```
"help"
"what's next"
"apa selanjutnya"
"saya bingung harus ngapain"
"status"
"di mana saya sekarang"
```

**AI Agent WAJIB merespons dengan format:**

```
━━━ 📍 STATUS & NEXT STEPS ━━━

📊 Current State:
- Project: [nama project]
- Mode: [Enterprise / Solo / Fast Track]
- Sprint: [N] (atau "belum mulai sprint")
- Layer progress: [X/8 setup layers complete]
- Current task: [issue #N — title] (atau "tidak ada task aktif")
- Branch: [current branch]

✅ Yang Sudah Selesai:
- [Layer/task yang sudah done]

📋 Yang Harus Dilakukan Selanjutnya:
1. [Next action — paling prioritas]
2. [Action setelahnya]
3. [Action opsional]

💡 Tips:
- [Saran kontekstual berdasarkan state]

Mau saya kerjakan item #1?
```

**Contoh Response Berdasarkan State:**

#### Jika belum mulai (project baru):
```
━━━ 📍 STATUS & NEXT STEPS ━━━

📊 Current State:
- Project: belum di-setup
- Layer progress: 0/8

📋 Yang Harus Dilakukan Selanjutnya:
1. Jawab 5 pertanyaan project (nama, deskripsi, tech stack, baru/existing, layer awal)
2. Setelah dijawab → AI akan setup project otomatis

Silakan jawab pertanyaan project untuk memulai.
```

#### Jika di tengah layer setup:
```
━━━ 📍 STATUS & NEXT STEPS ━━━

📊 Current State:
- Project: payment-gateway
- Mode: Enterprise
- Layer progress: 4/8 (Layer 0-3 done, Layer 4 in progress)

✅ Yang Sudah Selesai:
- Layer 0: vision.md ✅
- Layer 1: requirements extracted ✅
- Layer 2: validation report ✅
- Layer 3: payment-spec.md ✅

📋 Yang Harus Dilakukan Selanjutnya:
1. Selesaikan Layer 4 — Design System (12 dokumen tersisa)
2. Lanjut Layer 5 — AI Governance
3. Lanjut Layer 6 — AI Skills

💡 Tips:
- Layer 4 akan generate 14-16 design documents sekaligus
- Bilang "lanjutkan Layer 4" untuk melanjutkan

Mau saya lanjutkan Layer 4?
```

#### Jika di tengah sprint:
```
━━━ 📍 STATUS & NEXT STEPS ━━━

📊 Current State:
- Project: payment-gateway
- Mode: Enterprise
- Sprint: 2
- Current task: #18 — implement refund use case (in-progress)
- Branch: feature/issue-18-refund
- Coverage: 85%

✅ Sprint Progress: 3/5 tasks done
- #15: create payment entity ✅
- #16: implement top-up ✅
- #17: implement transfer ✅
- #18: implement refund ← IN PROGRESS
- #19: implement notification (next)

📋 Yang Harus Dilakukan Selanjutnya:
1. Selesaikan issue #18 (refund use case)
2. Setelah #18 done → kerjakan #19 (notification)
3. Setelah semua done → sprint completion (MR + retro + scorecard)

💡 Tips:
- Spec untuk refund ada di docs/specs/srs/refund-spec.md
- Design ada di docs/design/technical/refund-flow.md
- Bilang "kerjakan issue #18" untuk lanjut

Mau saya lanjutkan issue #18?
```

#### Jika sprint selesai:
```
━━━ 📍 STATUS & NEXT STEPS ━━━

📊 Current State:
- Project: payment-gateway
- Sprint: 2 (ALL TASKS DONE ✅)
- Coverage: 88%
- MR: belum dibuat

📋 Yang Harus Dilakukan Selanjutnya:
1. Create Merge Request (feature → develop)
2. Setelah MR merged (oleh Anda di GitLab) → Sprint Completion Package:
   - 📚 Sprint Retrospective
   - 📊 Quality Scorecard
   - 🏗️ Framework Health Check
3. Plan Sprint 3 (jika ada fitur berikutnya)

💡 Tips:
- Bilang "sprint selesai" untuk trigger MR creation
- Merge HARUS dilakukan oleh Anda di GitLab (bukan AI)

Mau saya buat MR untuk sprint ini?
```

**Rules Help Command:**
1. **Selalu baca CURRENT-STATE.md** sebelum merespons help
2. **Context-aware** — response berbeda tergantung state project
3. **Actionable** — selalu berikan next step yang jelas
4. **Tawarkan eksekusi** — "Mau saya kerjakan item #1?"
5. **Jangan verbose** — ringkas, to the point, fokus ke action
6. **Jika CURRENT-STATE.md tidak ada** → buat dulu, lalu respond

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
5. Apakah project ini punya UI? Jika ya:
   a. Apakah Anda sudah punya Design System sendiri? (color tokens, typography, component library)
   b. Jika ya — kirimkan detail berikut agar saya integrasikan ke project:
      - Color palette (primary, secondary, neutral, semantic colors)
      - Typography (font family, sizes, weights)
      - Spacing scale
      - Component library yang dipakai (shadcn, MUI, Ant Design, custom, dll)
      - Figma/design file link (opsional)
   c. Jika belum punya — saya akan generate default design system (Atomic Design + tokens)
6. Apakah GitLab credentials sudah di-setup?
   - GITLAB_PERSONAL_ACCESS_TOKEN (dari GitLab → User Settings → Access Tokens, scope: api, read_repository, write_repository)
   - GITLAB_API_URL (contoh: https://gitlab.com/api/v4)
   
   Jika BELUM:
   → Saya akan bantu set environment variable di OS Anda.
   → Windows: [Environment]::SetEnvironmentVariable('GITLAB_PERSONAL_ACCESS_TOKEN', 'glpat-xxx', 'User')
   → Linux/Mac: export GITLAB_PERSONAL_ACCESS_TOKEN='glpat-xxx' (di ~/.bashrc)
   → Setelah set, restart Kiro.
   
   Jika SUDAH: lanjut.

7. Mode development yang diinginkan?
   a. **Enterprise** (default) — konfirmasi setiap layer, full ceremony, kontrol penuh
   b. **Solo** — simplified workflow untuk developer tunggal
   c. **Zero Touch** — AI jalankan semuanya otomatis tanpa konfirmasi per-layer
      (hanya stop di merge point — user tetap harus approve merge di GitLab)
   
   → Default: Enterprise (jika tidak dijawab)

8. Layer mana yang ingin dikerjakan terlebih dahulu?
   → Default: Layer 0 (mulai dari awal)

Silakan jawab pertanyaan di atas agar saya bisa menyesuaikan framework dengan kebutuhan project Anda."
```

**Mode Development:**

| Mode | Behavior | Kapan Pakai |
|------|----------|-------------|
| **Enterprise** (default) | Konfirmasi setiap layer, full ceremony | Tim, production project |
| **Solo** | Simplified (tanpa develop branch, self-merge) | Developer tunggal |
| **Zero Touch** | AI jalankan semua otomatis, hanya stop di merge | Prototype cepat, PoC |

**Zero Touch Mode — Apa yang Terjadi:**
```
[User jawab pertanyaan → pilih Zero Touch]
    ↓
AI jalankan TANPA STOP (Layer 0-8):
- Generate semua docs (vision, requirements, specs, design, governance, skills)
- Setup GitLab (project, issues, milestone, labels)
- Setup hooks + steering + MCP config
    ↓
[Informasikan user: "Setup selesai. Sprint 1 ready dengan [N] issues."]
    ↓
AI kerjakan SEMUA issues sequential (tanpa user bilang per-issue):
- Issue #1 → full agent chain → push
- Issue #2 → full agent chain → push
- ...
- Issue #N → full agent chain → push
    ↓
[Auto health check → Create MR]
    ↓
[STOP — "MR dibuat: [URL]. Silakan merge di GitLab."]
    ↓
[Setelah user merge → auto retro + scorecard + wiki update]
```

**Zero Touch Rules:**
1. **HANYA aktif jika user eksplisit pilih** di pertanyaan #7
2. **Tetap enforce quality** — lint, typecheck, test, coverage >= 80%
3. **Tetap enforce architecture** — Clean Architecture, DI, observability
4. **STOP di merge point** — AI DILARANG merge sendiri
5. **Jika ada error/failure** → stop, informasikan user, tunggu instruksi
6. **User bisa interrupt kapan saja** — bilang "stop" atau "pause"

**Jika user punya Design System sendiri:**

AI Agent WAJIB:
1. Terima detail design system dari user (colors, typography, spacing, component library)
2. Generate `.kiro/steering/design-system.md` dengan detail tersebut (`inclusion: always`)
3. Generate `src/presentation/styles/tokens/` berdasarkan input user
4. Sesuaikan `docs/design/ui-ux/theming.md` dengan tokens user
5. Sesuaikan `docs/design/ui-ux/component-library.md` dengan library yang dipilih user

**Template `.kiro/steering/design-system.md` (jika user punya design system):**
```markdown
---
inclusion: always
---

# Design System — [Project Name]

## Color Tokens
[Dari input user — primary, secondary, neutral, semantic]

## Typography
[Dari input user — font family, sizes, weights]

## Spacing
[Dari input user — spacing scale]

## Component Library
[Library yang dipakai: shadcn / MUI / Ant Design / custom]

## Rules
- Semua component WAJIB menggunakan tokens di atas
- DILARANG hardcode warna/font/spacing
- Jika membuat component baru → ikuti pattern library yang dipilih
- Dark/Light theme WAJIB menggunakan token variants
```

**Jika user TIDAK punya Design System:**

AI Agent akan generate default design system saat Layer 4:
- Default color tokens (neutral + primary blue)
- Default typography (Inter/system font)
- Default spacing (4px scale)
- Atomic Design structure (atoms/molecules/organisms)

**Rules:**
- JANGAN langsung eksekusi layer tanpa mengetahui context project
- Tunggu jawaban user sebelum memulai pengerjaan
- Sesuaikan output setiap layer dengan context project yang diberikan user
- Jika user hanya menjawab sebagian, tanyakan yang belum dijawab
- **Jika user punya design system** → integrasikan ke steering + tokens SEBELUM mulai coding

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
    ↓
[CEK KELENGKAPAN DOKUMEN — WAJIB sebelum konfirmasi]
    ↓
[Jika ada dokumen yang belum lengkap → lengkapi dulu]
    ↓
[Tampilkan checklist ke user:]
    "Layer [N] - [Nama Layer] selesai.
    
    📋 Document Completeness Check:
    ✅ [dokumen 1] — created
    ✅ [dokumen 2] — created
    ❌ [dokumen 3] — MISSING → sedang dilengkapi...
    
    Semua dokumen lengkap. Apakah hasilnya sudah sesuai,
    atau ada yang perlu direvisi sebelum lanjut?"
    ↓
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

### Document Completeness Check (WAJIB Setelah Setiap Layer)

AI Agent **WAJIB** mengecek kelengkapan dokumen setelah menyelesaikan setiap layer. Jika ada yang belum lengkap, **WAJIB dilengkapi SEBELUM** konfirmasi ke user.

**Checklist per Layer (SEMUA WAJIB — tidak ada yang opsional):**

| Layer | Dokumen WAJIB | Cek |
|-------|--------------|-----|
| **0 — Vision** | `docs/product/vision.md` + `roadmap.md` + `business-goals.md` + `success-metrics.md` + `ai-opportunities.md` | ✅ semua harus ada |
| **1 — Intake** | `docs/requirements/extracted/user-stories.md` + `functional-requirements.md` + `non-functional-requirements.md` | ✅ semua harus ada |
| **2 — Validation** | `docs/requirements/validation/validation-report-*.md` + `ambiguity-report.md` + `conflict-analysis.md` + `risk-analysis.md` | ✅ semua harus ada |
| **3 — Spec-Driven** | `docs/specs/brd/business-flow.md` + `stakeholder-matrix.md` | ✅ semua harus ada |
| | `docs/specs/prd/user-stories.md` + `acceptance-criteria.md` + `feature-matrix.md` | ✅ semua harus ada |
| | `docs/specs/srs/[feature]-spec.md` + `architecture.md` + `sequence-diagram.md` + `api-contract.md` + `state-flow.md` + `failure-scenario.md` + `data-model.md` | ✅ semua harus ada |
| **4 — Design** | System: `high-level-architecture.md` + `sequence-diagram.md` + `deployment.md` + `c4-model.md` + `event-flow.md` | ✅ semua harus ada |
| | Technical: `clean-architecture.md` + `folder-structure.md` + `error-handling.md` + `naming-convention.md` + `testing-pattern.md` | ✅ semua harus ada |
| | Security: `threat-model.md` + `trust-boundary.md` + `attack-surface.md` + `mitigation-plan.md` | ✅ semua harus ada |
| | UI/UX (jika ada UI): `wireframe.md` + `component-library.md` + `accessibility.md` + `i18n-strategy.md` + `theming.md` + `design-token.md` | ✅ semua harus ada |
| **5 — Governance** | `docs/governance/ai-policy.md` + `approved-tools.md` + `security-policy.md` + `code-review-policy.md` | ✅ semua harus ada |
| **6 — Skills** | `.kiro/skills/create-api.md` + `create-usecase.md` + `create-repository.md` + `create-component.md` + `create-test.md` + `create-migration.md` | ✅ semua harus ada (6 files) |
| **7 — Team Extension** | `.kiro/steering/team/[domain]-team.md` (minimal 1 per domain project) | ✅ harus ada |
| **8 — Issue-Driven** | GitLab: issues + milestone + labels + board + issue templates (Feature, Task, Bug) + MR templates | ✅ semua harus ada |
| **10 — Context** | `docs/CONTEXT-INDEX.md` + `docs/CURRENT-STATE.md` | ✅ semua harus ada |
| **12 — Quality Gates** | `.gitlab-ci.yml` + `.eslintrc.json`/`eslint.config.mjs` + `tsconfig.json` + `.prettierrc` + `vitest.config.ts` + `commitlint.config.js` | ✅ semua harus ada |
| **14 — Learning** | `docs/learnings/` folder + `docs/retrospectives/` folder + `docs/adr/` folder + `docs/tech-debt/` folder + `docs/quality/metrics-log.jsonl` | ✅ semua harus ada |

**Flow:**
```
[Layer N selesai]
    ↓
[AI Agent scan: semua dokumen WAJIB untuk layer ini ada?]
    ↓
├── SEMUA ADA → tampilkan checklist ✅ + konfirmasi user
└── ADA YANG MISSING → 
    ├── Generate dokumen yang missing
    ├── Informasikan user: "Dokumen [X] belum ada, sedang dilengkapi..."
    ├── Setelah lengkap → tampilkan checklist ✅ + konfirmasi user
    └── JANGAN lanjut ke layer berikutnya sampai SEMUA lengkap
```

**Rules:**
1. **JANGAN konfirmasi layer selesai** jika ada dokumen yang missing
2. **JANGAN lanjut ke layer berikutnya** jika dokumen belum lengkap
3. **Auto-generate** dokumen yang missing — jangan tanya user "mau buat?"
4. **Tampilkan checklist** ke user agar transparan apa yang sudah/belum ada
5. **Update CONTEXT-INDEX.md** setelah dokumen baru di-generate

---

### Layer Sequence (DILARANG Skip)

Layer 0-8 **WAJIB dijalankan sequential**. AI Agent **DILARANG** menawarkan skip atau loncat layer.

**Urutan WAJIB:**
```
Layer 0 → 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → [Sprint dimulai]
```

**Rules:**
1. **DILARANG skip layer manapun** dari 0-8 — setiap layer menghasilkan artifact yang dibutuhkan layer berikutnya
2. **DILARANG menawarkan "mau skip ke Layer X?"** — ini bukan opsional
3. **DILARANG mulai Sprint** sebelum Layer 0-8 selesai semua
4. **Layer 6 (Skills) WAJIB** — tanpa skills, AI tidak punya recipe untuk coding konsisten
5. **Layer 7 (Team Extension) WAJIB** — minimal buat 1 team steering file sesuai domain project
6. **Jika user minta skip** → informasikan: "Layer [N] diperlukan karena [alasan]. Saya akan mengerjakannya secara ringkas."

**Pengecualian (HANYA jika user eksplisit bilang):**
- Project existing yang sudah punya artifact → AI boleh **validate** artifact existing tanpa generate ulang
- Fast Track Mode aktif → Layer 6 dan 7 boleh **simplified** (minimal 3 skills + 1 team file) tapi TIDAK boleh di-skip total

**Kenapa Layer 6 & 7 Tidak Boleh Di-skip:**
- Layer 6 (Skills) = "recipe" untuk AI membuat code. Tanpa ini, output AI **inconsistent** antar fitur
- Layer 7 (Team Extension) = domain knowledge. Tanpa ini, AI tidak tahu constraint spesifik (PCI compliance, encryption rules, dll)
- Keduanya dipakai SETIAP KALI AI coding di sprint — bukan one-time setup yang bisa di-skip

---

### GitLab Project Management (WAJIB di Setiap Aktivitas)

Setiap aktivitas yang berkaitan dengan issue/task/sprint, AI Agent **WAJIB** update GitLab:

| Aktivitas | GitLab Update WAJIB |
|-----------|-------------------|
| Task dimulai | `update_issue`: label → `status::in-progress` |
| Task selesai + pushed | `update_issue`: label → `status::review` + `create_issue_note`: summary |
| Bug ditemukan | `create_issue`: type::bug + assign ke milestone |
| Sprint selesai | `create_merge_request` + update semua issues + update milestone |
| MR merged (oleh user) | `edit_milestone`: close/carry-over + `create_or_update_wiki_page`: Changelog |
| Improvement identified | `create_issue`: type::improvement |
| Tech debt detected | `create_issue`: type::tech-debt |

**Rules:**
1. **SETIAP task yang terkait issue → WAJIB update issue status + comment**
2. **SETIAP sprint end → WAJIB update milestone + wiki**
3. **MERGE di GitLab HANYA oleh user** — AI Agent DILARANG merge MR sendiri
4. **Setelah sprint MR merged → WAJIB tawarkan retro + scorecard + health-check**
5. **JANGAN skip GitLab updates** — ini bukan opsional, ini bagian dari workflow

**Sprint Completion Package (WAJIB ditawarkan setelah MR merged):**
```
"MR sudah merged! 🎉 Saya rekomendasikan:
1. 📚 Sprint Retrospective — review performance + learnings
2. 📊 Quality Scorecard — metrics objektif sprint ini
3. 🏗️ Framework Health Check — cek compliance
4. 📖 Update GitLab Wiki — Changelog + API docs (jika ada endpoint baru)

Mau jalankan semua, sebagian, atau skip?"
```

**Trigger Phrases untuk Sprint Completion (AI Agent WAJIB recognize):**

Saat user bilang salah satu dari frasa berikut, AI Agent WAJIB menjalankan Sprint Completion flow:
```
"sprint selesai"
"sprint done"
"semua task sudah selesai"
"sprint complete"
"selesaikan sprint ini"
```

**Flow yang WAJIB dijalankan saat trigger di atas:**
```
[User: "sprint selesai"]
    ↓
━━━ 🚀 DEVOPS AGENT ━━━
1. **LOCAL VALIDATION (WAJIB SEBELUM MR — BLOCKING):**
   - `npm run lint` → HARUS 0 errors
   - `npm run typecheck` → HARUS 0 errors
   - `npm run test:unit -- --coverage` → HARUS all pass, coverage >= 80%
   - Jika SALAH SATU gagal → FIX DULU, JANGAN buat MR
2. Push semua perubahan yang belum di-push
3. Create Merge Request (feature → develop)
   - JANGAN auto-merge — WAJIB user approval di GitLab
3. Update semua sprint issues: status::review
4. Update milestone description: sprint progress summary
    ↓
Informasikan user:
  "MR dibuat: [URL]. Silakan review dan merge di GitLab.
   Setelah merged, bilang 'sudah merged' untuk lanjut."
    ↓
[User: "sudah merged" / "merged" / "done"]
    ↓
━━━ 📚 LEARNING AGENT ━━━
WAJIB TAWARKAN Sprint Completion Package:
  "Sprint merged! 🎉 Saya akan jalankan:
  1. 📚 Sprint Retrospective — review AI performance, what worked/didn't
  2. 📊 Quality Scorecard — metrics objektif (coverage, rework rate, compliance)
  3. 🏗️ Framework Health Check — scan compliance project
  4. 📖 GitLab Wiki Update — Changelog + API Documentation
  
  Mau jalankan semua? (recommended) Atau pilih sebagian?"
    ↓
[Jika user bilang "jalankan semua" / "semua" / "ya":]
    ↓
a. Generate docs/retrospectives/sprint-[N].md (dari metrics-log.jsonl)
b. Generate docs/quality/sprint-[N]-scorecard.md
c. Jalankan health check → docs/quality/health-check-[date].md
d. Update GitLab Wiki:
   - Page "Changelog": tambah sprint [N] delivery summary
   - Page "API-Documentation": update jika ada endpoint baru
   - Page "Architecture-Decisions": update jika ada ADR baru
e. Close/carry-over milestone di GitLab
f. Update docs/CURRENT-STATE.md: sprint = N+1
g. Update docs/CONTEXT-INDEX.md: tambah retro + scorecard
    ↓
Informasikan user: summary + next sprint recommendation
```

**Rules:**
1. **WAJIB recognize trigger phrases** — jangan tunggu user bilang persis "jalankan retrospective"
2. **WAJIB tawarkan SETELAH merge** — bukan sebelum merge
3. **WAJIB include wiki update** — Changelog selalu, API docs jika ada endpoint baru
4. **Jika user skip** → informasikan bisa dijalankan nanti ("bilang 'retrospective' kapan saja")
5. **JANGAN jalankan tanpa konfirmasi** — tawarkan dulu, tunggu user approve

---

## Solo Mode (Opsional — HANYA Jika User Eksplisit Minta)

**⚠️ DEFAULT MODE ADALAH ENTERPRISE MODE.**

AI Agent **DILARANG** mengaktifkan Solo Mode kecuali user **EKSPLISIT** bilang salah satu dari:
- "Saya bekerja solo"
- "Ini project personal"
- "Solo mode"
- "Saya sendiri"

**DILARANG assume Solo Mode** berdasarkan:
- ❌ Jumlah orang di session (selalu 1 di Kiro — bukan indikator)
- ❌ Ukuran project (project kecil tetap bisa Enterprise)
- ❌ Tech stack (apapun stack-nya, default Enterprise)
- ❌ "Sepertinya ini project kecil" (AI DILARANG assume)

**Jika user tidak menyebutkan mode → SELALU gunakan Enterprise Mode (full ceremony).**

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

## Strict Rules (TIDAK BOLEH DILANGGAR)

### 1. DILARANG Merge Tanpa Konfirmasi User

AI Agent **DILARANG KERAS** melakukan merge — baik di GitLab maupun di local git — tanpa konfirmasi eksplisit dari user.

**DILARANG:**
```
❌ git merge feature/xxx ke main/develop (local)
❌ Merge MR via GitLab MCP tanpa user approve
❌ Set auto-merge pada MR
❌ Squash merge tanpa user bilang "merge"
❌ Create MR dari feature branch ke main (SALAH TARGET!)
```

**WAJIB:**
```
✅ Buat MR di GitLab → informasikan URL → tunggu user merge manual di GitLab
✅ Jika user bilang "merge" → konfirmasi dulu: "Merge [branch] ke [target]? (y/n)"
✅ Setelah user konfirmasi → baru eksekusi merge
```

**⚠️ BRANCH TARGET RULES (WAJIB DIIKUTI):**

| Source Branch | Target Branch | Kapan |
|--------------|---------------|-------|
| `feature/*` | **`develop`** | Sprint delivery (SELALU ke develop, BUKAN main!) |
| `bugfix/*` | **`develop`** | Bug fix non-urgent |
| `hotfix/*` | **`main`** | HANYA untuk hotfix production critical |
| `develop` | **`main`** | Release ke production (setelah QA di develop) |

**DILARANG KERAS:**
- ❌ `feature/*` → `main` (SALAH! Harus ke `develop` dulu)
- ❌ `bugfix/*` → `main` (SALAH! Harus ke `develop` dulu)
- ❌ Skip `develop` branch (kecuali hotfix)

**Flow yang BENAR:**
```
feature/issue-N → MR ke develop → merge → test di develop → MR ke main → release
```

**Jika Solo Mode (tanpa develop):**
```
feature/issue-N → MR ke main (Solo Mode SAJA — karena tidak ada develop)
```

**Alasan:** Merge adalah operasi irreversible yang mengubah branch utama. Feature branch HARUS melalui `develop` dulu untuk integration testing sebelum ke `main` (production).

---

### 2. WAJIB Update GitLab Wiki + Issues Setiap Sprint Selesai

Setelah sprint selesai (MR merged), AI Agent **WAJIB** melakukan update berikut — TANPA perlu user minta:

| Update | Tool | Konten |
|--------|------|--------|
| Wiki "Changelog" | `create_or_update_wiki_page` | Sprint [N]: features delivered, issues closed, carry-over |
| Wiki "API-Documentation" | `create_or_update_wiki_page` | Endpoint baru (jika ada) |
| Wiki "Architecture-Decisions" | `create_or_update_wiki_page` | ADR baru (jika ada) |
| Semua sprint issues | `update_issue` | Label → `status::done` (atau carry-over) |
| Milestone | `edit_milestone` | Close (jika semua done) atau update description |
| Improvement issues | `create_issue` | Dari retrospective action items |

**Rules:**
- Wiki update WAJIB — bukan opsional
- Issue status update WAJIB — board harus reflect reality
- Milestone close/carry-over WAJIB — jangan biarkan milestone open tanpa alasan
- Jika GitLab MCP gagal → retry 2x → informasikan user untuk manual update

---

### 3. WAJIB Ikuti Workflow Power di Setiap Sprint

Setiap sprint yang dijalankan **WAJIB** mengikuti workflow lengkap framework ini. AI Agent DILARANG "shortcut" atau skip step.

**Sprint Workflow WAJIB (tidak boleh di-skip):**

```
[SPRINT PLANNING]
1. Buat/update milestone di GitLab
2. Breakdown fitur → issues (Epic → Feature → Task)
3. Assign issues ke milestone
4. Buat branch dari develop (atau main di Solo Mode)

[PER TASK — WAJIB untuk setiap task]
5. 🏗️ Architect Gate: load context + validate spec/design
6. ⚙️/🎨 Implementation: code sesuai architecture standards
7. 🔒 Security scan: setiap file write di-scan
8. 📊 Observability: logging + metrics ditambahkan
9. 🧪 QA: lint + typecheck + test (coverage >= 80%)
10. 🚀 DevOps: commit + push + update issue status
11. 📊 Metrics: collect ke metrics-log.jsonl
12. 📚 Learning: capture jika bug fix

[SPRINT END — WAJIB setelah semua task done]
13. Push semua perubahan
14. Create MR → TUNGGU user merge di GitLab
15. Setelah merged → TAWARKAN Sprint Completion Package:
    - 📚 Retrospective
    - 📊 Scorecard
    - 🏗️ Health Check
    - 📖 Wiki Update
16. Update milestone (close/carry-over)
17. Update wiki Changelog
18. Update semua issues status
19. Update CURRENT-STATE.md (sprint = N+1)
```

**DILARANG:**
- ❌ Skip Architect Gate (langsung coding tanpa cek spec)
- ❌ Skip QA (push tanpa lint/typecheck/test)
- ❌ Skip issue update (push tanpa update GitLab)
- ❌ Skip wiki update setelah sprint
- ❌ Skip metrics collection
- ❌ Merge tanpa konfirmasi user
- ❌ Mulai sprint baru tanpa close sprint sebelumnya

---

### 4. WAJIB Ikuti Workflow Saat Tambah Issue / Sprint / Fitur Baru

Saat user ingin menambahkan issue baru, sprint baru, atau fitur baru — AI Agent **WAJIB** mengikuti workflow framework ini. DILARANG langsung coding tanpa melalui proses yang benar.

**Saat user bilang "tambah issue baru" / "ada fitur baru" / "buat task baru":**

```
[User minta tambah issue/fitur]
    ↓
1. Tentukan: apakah ini masuk sprint aktif atau sprint baru?
   → Jika sprint aktif masih berjalan → tambahkan ke sprint aktif
   → Jika sprint sudah selesai → buat sprint baru
    ↓
2. Jalankan Spec-First Gate (Layer 3):
   → Fitur kompleks? → WAJIB buat spec dulu
   → Bug fix / task kecil? → boleh langsung
    ↓
3. Buat GitLab Issue:
   → Title, description, acceptance criteria
   → Labels: type::feat/fix/chore, priority::*, domain::*
   → Assign ke milestone aktif
    ↓
4. Buat branch dari develop (atau main di Solo Mode):
   → Naming: feature/issue-[N]-[description]
    ↓
5. Ikuti per-task workflow (step 5-12 dari Sprint Workflow):
   → Architect Gate → Implementation → Security → QA → DevOps → Metrics
    ↓
6. Update GitLab: issue status + comment + milestone progress
```

**Saat user bilang "mulai sprint baru" / "plan sprint [N]":**

```
[User minta sprint baru]
    ↓
1. Cek: apakah sprint sebelumnya sudah di-close?
   → Jika BELUM → close dulu (carry-over issues, update wiki, retro)
   → Jika SUDAH → lanjut
    ↓
2. Buat milestone baru di GitLab:
   → Title: Sprint [N]
   → Start date + due date
    ↓
3. Breakdown fitur menjadi issues (BA Agent + Architect Agent):
   → User stories → technical tasks → GitLab issues
    ↓
4. Assign semua issues ke milestone
    ↓
5. Buat labels jika ada domain baru
    ↓
6. Update CURRENT-STATE.md: sprint = N, tasks = [list]
    ↓
7. Informasikan user: "Sprint [N] ready. [X] issues created. Mau mulai task pertama?"
```

**Saat user bilang "tambah hotfix" / "ada bug urgent":**

```
[User report bug urgent]
    ↓
1. Buat GitLab issue: type::bug, priority::critical
    ↓
2. Buat branch: hotfix/issue-[N]-[description] (dari main)
    ↓
3. Fix → Test → Push (ikuti per-task workflow)
    ↓
4. Create MR ke main → TUNGGU user merge
    ↓
5. Setelah merged → bug learning capture (Layer 14)
    ↓
6. Update issue status + wiki (jika perlu)
```

**Rules:**
1. **SELALU buat GitLab issue** — tidak ada task tanpa issue (traceability)
2. **SELALU assign ke milestone** — tidak ada issue orphan
3. **SELALU ikuti per-task workflow** — tidak ada shortcut
4. **SELALU update CURRENT-STATE.md** — state harus reflect reality
5. **Sprint baru HARUS close sprint lama dulu** — tidak boleh overlap tanpa carry-over
6. **Hotfix tetap ikuti workflow** — hanya branch strategy yang berbeda (dari main, bukan develop)

---

### 5. Pertanyaan Inisiasi Sprint & Task (WAJIB Ditanyakan)

Saat user minta mulai sprint baru atau task baru, AI Agent **WAJIB** menanyakan pertanyaan berikut. Jika user tidak menjawab semua, berikan **rekomendasi default** dan konfirmasi.

#### Inisiasi Sprint Baru

```
"Sprint baru akan dimulai. Saya perlu informasi berikut:

1. Nama/nomor sprint? 
   → Default: Sprint [N+1] (increment dari sprint terakhir)

2. Durasi sprint?
   → Default: 2 minggu

3. Fitur/epic apa yang akan dikerjakan?
   → WAJIB dijawab user (tidak ada default — ini menentukan scope)

4. Apakah ada carry-over dari sprint sebelumnya?
   → Default: Saya cek dari milestone sebelumnya (auto-detect issues yang belum done)

5. Apakah ada deadline khusus atau dependency external?
   → Default: Tidak ada (standard sprint timeline)

6. Prioritas utama sprint ini?
   → Default: Delivery fitur baru
   → Opsi: delivery fitur / fix bugs / tech debt payback / mixed

Jika Anda hanya jawab nomor 3 (fitur), saya akan gunakan default untuk sisanya.
Konfirmasi sebelum saya mulai planning?"
```

**Rekomendasi Default Sprint:**

| Pertanyaan | Default | Kapan Berbeda |
|-----------|---------|---------------|
| Nama | Sprint [N+1] | User mau custom naming |
| Durasi | 2 minggu | Deadline mendesak → 1 minggu |
| Fitur | — (WAJIB user jawab) | — |
| Carry-over | Auto-detect dari milestone sebelumnya | User mau reset clean |
| Deadline | Tidak ada | Ada demo/release date |
| Prioritas | Delivery fitur | Banyak bugs → fix bugs. Banyak debt → payback |

**Setelah user jawab (minimal pertanyaan #3):**
```
AI: "Sprint [N+1] akan saya setup dengan:
- Durasi: 2 minggu ([start] — [end])
- Fitur: [yang user bilang]
- Carry-over: [N] issues dari Sprint [N]
- Prioritas: Delivery fitur

Konfirmasi? Atau ada yang mau diubah?"
```

---

#### Inisiasi Task/Issue Baru

```
"Task baru akan dibuat. Saya perlu informasi berikut:

1. Deskripsi task (apa yang harus dilakukan)?
   → WAJIB dijawab user

2. Tipe task?
   → Default: feat (fitur baru)
   → Opsi: feat / fix / refactor / docs / chore / test

3. Domain/area?
   → Default: Auto-detect dari deskripsi (payment, auth, notification, dll)

4. Sprint assignment?
   → Default: Sprint aktif saat ini

5. Acceptance criteria?
   → Default: AI generate berdasarkan deskripsi, user review
   → Rekomendasi: minimal 3 AC per task

6. Dependencies?
   → Default: Tidak ada
   → AI cek: apakah ada entity/interface yang belum dibuat?

Jika Anda hanya bilang deskripsi task, saya akan generate sisanya dan konfirmasi."
```

**Rekomendasi Default Task:**

| Pertanyaan | Default | Kapan Berbeda |
|-----------|---------|---------------|
| Deskripsi | — (WAJIB user jawab) | — |
| Tipe | `feat` | Bug → `fix`. Cleanup → `refactor`. Docs → `docs` |
| Domain | Auto-detect dari deskripsi | User specify jika ambigu |
| Sprint | Sprint aktif | Standalone (di luar sprint) |
| AC | AI generate 3-5 AC, user review | User punya AC sendiri |
| Dependencies | Tidak ada (AI cek otomatis) | User tahu ada blocker |

**Contoh: User hanya bilang deskripsi**
```
User: "Buat fitur export data ke CSV"

AI: "Task akan saya buat dengan detail berikut:
- Title: feat: implement data export to CSV
- Type: feat
- Domain: data (auto-detected)
- Sprint: Sprint 3 (aktif)
- Acceptance Criteria (generated):
  1. User bisa export data dari halaman list ke file CSV
  2. File CSV berisi semua kolom yang ditampilkan di tabel
  3. Export handle data > 10.000 rows tanpa timeout
  4. Nama file: [entity]-export-[timestamp].csv
  5. Error handling: informasikan user jika export gagal
- Dependencies: Tidak ada

Konfirmasi? Atau ada yang mau diubah?"
```

**Rules:**
1. **Minimal pertanyaan #1 (deskripsi) WAJIB dijawab user** — sisanya boleh default
2. **SELALU konfirmasi** sebelum create issue di GitLab
3. **SELALU generate AC** jika user tidak provide — user bisa revisi
4. **Auto-detect domain** dari kata kunci di deskripsi
5. **Jika user jawab singkat** → AI lengkapi dengan default, konfirmasi
6. **JANGAN create issue tanpa konfirmasi** — user harus approve detail

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

| File Target (di project user) | Cara Generate | Inclusion |
|-------------------------------|--------------|-----------|
| `.kiro/steering/architecture-standards.md` | **Generate FULL content** (100% isi dari power steering) | `always` |
| `.kiro/steering/test-writing-patterns.md` | **Generate FULL content** (100% isi dari power steering) | `always` |
| `.kiro/steering/coding-conventions.md` | Generate dari template di bawah | `always` |

**Cara generate (AI Agent WAJIB ikuti):**

1. **`architecture-standards.md`** — Generate **100% FULL content** yang berisi:
   - Front-matter: `inclusion: always`
   - Overview + Tujuan Arsitektur
   - Prinsip Utama (Dependency Rule, Separation of Concerns, DI via Awilix, Protocol-based Abstraction, Framework as Detail, Composition Root)
   - Struktur Layer LENGKAP (folder tree src/core, infrastructure, presentation, app)
   - Tanggung Jawab Layer (CORE, INFRASTRUCTURE, PRESENTATION, APP — boleh/dilarang)
   - Server-First Rendering + Atomic Design rules
   - Aturan Dependency (tabel strict)
   - Data Flow + Aturan Mapping Data (DTO → Entity → ViewModel → UI)
   - DI section LENGKAP (struktur, factory function pattern, registration pattern, lifetime rules, resolve pattern)
   - DI di Presentation Layer
   - DI dan Clean Architecture relationship
   - Naming Convention (tabel lengkap)
   - Anti-Pattern (Architecture + DI) dengan contoh code ❌/✅
   - DI untuk Testing
   - Workflow Membuat Fitur Baru (8 steps)
   - Container Bootstrapping
   - Enforcement rules
   - Best Practices (Arsitektur + DI)
   - Kesimpulan
   - Integration dengan Framework
   
   **GENERATE SELURUH ISI** — bukan ringkasan. File ini adalah sumber kebenaran arsitektur di project user. Jika terlalu besar untuk 1 operasi, gunakan multiple write operations (fs_write + fs_append).

2. **`test-writing-patterns.md`** — Generate **100% FULL content** yang berisi:
   - Front-matter: `inclusion: always`
   - Overview + Peran Dokumen
   - Test Pyramid Strategy (70% unit, 20% integration, 10% E2E)
   - Per-Layer Test Patterns LENGKAP dengan contoh code TypeScript:
     - Use Case tests (mock repository, happy/error/edge)
     - Repository tests (mock HTTP, mapping, error handling)
     - ViewModel tests (mock use case, state changes)
     - Mapper tests (field mapping, fallback, abnormal data)
     - Middleware tests (auth flow, redirect, safe fallback)
   - DI-Friendly Testing Patterns (direct injection, fixtures, container override)
   - Anti-Pattern Testing (5 items dengan contoh ❌/✅)
   - Test Naming Convention
   - Coverage Rules (80% minimum, 95% untuk auth/payment)
   - Vitest Configuration Best Practices (setup file, config, QueryClient, Zustand reset)
   - Integration dengan Framework
   - Enforcement rules
   
   **GENERATE SELURUH ISI** — termasuk contoh code. File ini adalah panduan testing lengkap di project user.

3. **`coding-conventions.md`** — Generate dari template di bawah

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
| `.kiro/hooks/code-quality-scan.json` | postToolUse (write) | Security + Observability (merged, smart-filtered) |
| `.kiro/hooks/qa-devops-post-task.json` | postTaskExecution | Lint + test + push setelah task |
| `.kiro/hooks/bug-learning-capture.json` | postTaskExecution | Capture learning saat bug fix |
| `.kiro/hooks/metrics-collector.json` | postTaskExecution | Auto-collect AI quality metrics |
| `.kiro/hooks/sprint-end-auto-check.json` | postTaskExecution | Auto health check + offer retro saat sprint task terakhir |
| `.kiro/hooks/sprint-retrospective.json` | userTriggered | Generate retrospective (data-driven) |
| `.kiro/hooks/quality-scorecard.json` | userTriggered | Generate scorecard dari metrics |
| `.kiro/hooks/health-check.json` | userTriggered | Framework compliance scan |

**Untuk detail lengkap setiap hook file (JSON content, smart filtering logic, metrics system), lihat `steering/hooks-registry.md`.**

**Perubahan v1.5.0:**
- `security-review.json` + `observability-check.json` → **MERGED** menjadi `code-quality-scan.json` dengan smart filtering (skip docs/config/test/entity)
- **NEW:** `metrics-collector.json` — auto-collect metrics ke `docs/quality/metrics-log.jsonl` setiap task selesai
- Retrospective dan Scorecard sekarang **data-driven** (baca dari metrics-log, bukan manual count)

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

### Step 4.6: Generate MCP Config dengan autoApprove (WAJIB)

AI Agent **WAJIB** membuat `.kiro/settings/mcp.json` di project user agar MCP tools bisa berjalan **tanpa approval popup** untuk operasi rutin.

**Tanpa step ini, user akan mendapat popup approval setiap kali AI Agent melakukan git commit, push, create issue, dll — sangat mengganggu workflow.**

**File yang WAJIB di-generate: `.kiro/settings/mcp.json`**

```json
{
  "mcpServers": {
    "gitlab": {
      "command": "npx",
      "args": ["-y", "@zereight/mcp-gitlab"],
      "env": {
        "GITLAB_PERSONAL_ACCESS_TOKEN": "${GITLAB_PERSONAL_ACCESS_TOKEN}",
        "GITLAB_API_URL": "${GITLAB_API_URL}"
      },
      "autoApprove": [
        "create_repository",
        "create_project",
        "create_label",
        "create_issue",
        "create_milestone",
        "create_branch",
        "create_merge_request",
        "create_issue_note",
        "create_or_update_wiki_page",
        "update_issue",
        "edit_milestone",
        "push_files",
        "create_or_update_file",
        "discover_tools",
        "search_repositories",
        "get_file_contents",
        "list_issues"
      ]
    },
    "git": {
      "command": "npx",
      "args": ["-y", "@cyanheads/git-mcp-server"],
      "autoApprove": [
        "git_init",
        "git_add",
        "git_commit",
        "git_remote",
        "git_push",
        "git_pull",
        "git_branch",
        "git_checkout",
        "git_merge",
        "git_status",
        "git_log",
        "git_diff"
      ]
    }
  }
}
```

**Rules:**
- WAJIB generate saat project setup
- Jika `.kiro/settings/mcp.json` sudah ada → **merge** autoApprove list (jangan overwrite seluruh file)
- Credentials WAJIB tetap sebagai `${VARIABLE_NAME}` — JANGAN hardcode token
- File ini HARUS di-commit ke repository (agar tim lain juga dapat autoApprove)
- Informasikan user: "MCP config dengan autoApprove sudah di-setup. Pastikan environment variables sudah di-set."

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
