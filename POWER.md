---
name: "enterprise-ai-sdlc"
displayName: "Enterprise AI-Native SDLC Framework"
description: "Framework SDLC berbasis AI untuk enterprise yang menggabungkan Spec-Driven Development, AI Governance, Multi-Agent Orchestration, dan GitLab CI/CD. Mencegah AI chaos dan pilot graveyard dengan pendekatan terstruktur."
keywords: ["sdlc", "ai-native", "spec-driven", "governance", "gitlab", "ci-cd", "enterprise", "multi-agent"]
author: "ricko_c_putra@telkomsel.co.id"
---

# Enterprise AI-Native SDLC Framework

## ⚠️ CRITICAL RULES — BACA INI DULU (AI Agent WAJIB Patuh)

**10 rules terpenting yang TIDAK BOLEH dilanggar:**

1. **DEFAULT MODE: Enterprise.** DILARANG assume Solo. Cek mode dari CURRENT-STATE.md sebelum SETIAP layer.
2. **SEBELUM PUSH/MR:** Jalankan `npm run lint` + `npm run typecheck` + `npm run test:unit -- --coverage`. Semua HARUS pass, coverage >= 80%. **Gagal = BLOCK. FIX DULU.**
3. **MR TARGET:** `feature/*` → `develop` (BUKAN main). Solo Mode saja: `feature/*` → `main`.
4. **MERGE:** HANYA oleh user di GitLab. AI Agent **DILARANG** merge sendiri.
5. **SETIAP TASK SELESAI:** Update GitLab issue (label `status::review` + comment) + collect metrics. Setelah MR merged: **close issue** (`state_event: "close"`).
6. **SPRINT END:** Tawarkan retro + scorecard + health check + wiki update. WAJIB update milestone + wiki Changelog.
7. **SETELAH PROJECT FOLDER DIBUAT:** LANGSUNG generate `.kiro/hooks/` (9 files) + `.kiro/steering/` (3 files) + `.kiro/settings/mcp.json` + `docs/CURRENT-STATE.md`. **JANGAN tunggu Layer 8.**
8. **LAYER 0-8:** Sequential, DILARANG skip. Cek document completeness setiap layer selesai. Semua docs WAJIB (tidak ada opsional).
9. **SPRINT PLANNING:** WAJIB buat milestone (`create_milestone`) + wiki pages (`create_or_update_wiki_page`). Verify response. JANGAN lanjut tanpa milestone + wiki.
10. **CODE REVIEW:** Tawarkan AI review SEBELUM buat MR. Jika user setuju → review architecture + security.

---

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

27 steering files tersedia. Lihat `docs/DOCUMENTATION.md` section 5 untuk list lengkap.

**Always-included (4 files):** `architecture-standards.md`, `test-writing-patterns.md`, `project-memory.md`, `hooks-registry.md`

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
    → Jika YA (minimal 1 ada) → ini project existing, jalankan detect 2-5
    → Jika TIDAK → ini project baru, lanjut ke "Tanyakan Project"
      **TAPI setelah user jawab pertanyaan dan project folder dibuat → WAJIB kembali jalankan detect 2-5**
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
    → Jika TIDAK ADA → GENERATE 3 steering files (architecture-standards, test-writing-patterns, coding-conventions) — generate FULL content
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

**⚠️ CRITICAL — Untuk Project BARU:**

Auto-detect step 2-5 WAJIB dijalankan **SEGERA SETELAH folder project dibuat** (setelah `git init` atau setelah user jawab pertanyaan). JANGAN tunggu sampai Layer 8 selesai.

**Urutan untuk project baru:**
```
1. User jawab 8 pertanyaan
2. AI buat folder project (git init, mkdir .kiro, docs, src)
3. ⚠️ LANGSUNG jalankan auto-detect 2-5:
   → Generate .kiro/hooks/ (9 files)
   → Generate .kiro/steering/ (3 files)
   → Generate docs/CURRENT-STATE.md
   → Generate .kiro/settings/mcp.json
4. Baru lanjut ke Layer 0
```

**JANGAN:**
- ❌ Tunggu sampai Layer 8 untuk generate hooks
- ❌ Tunggu sampai sprint untuk generate MCP config
- ❌ Skip generate karena "nanti saja"
- ❌ Hanya generate untuk project existing

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

**Trigger untuk Manual Setup (jika auto-detect gagal):**
```
"setup framework"
"generate hooks"
"generate steering"
"generate MCP config"
"setup lengkap"
```
→ AI Agent WAJIB jalankan: generate `.kiro/hooks/` (9 files) + `.kiro/steering/` (3 files) + `.kiro/settings/mcp.json` + `docs/CURRENT-STATE.md`

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

(Contoh lain: lihat docs/USER-GUIDE.md section "Help Command")

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
   a. Design System mana yang ingin dipakai?
      - **Signal Design System** (built-in — sudah tersedia di framework ini)
      - **Custom** — punya design system sendiri (kirimkan file/detail)
      - **Default** — generate default design system (Atomic Design + neutral tokens)
   b. Jika pilih **Custom** — kirimkan detail berikut:
      - Color palette (primary, secondary, neutral, semantic colors)
      - Typography (font family, sizes, weights)
      - Spacing scale
      - Component library yang dipakai (shadcn, MUI, Ant Design, custom, dll)
      - Figma/design file link (opsional)
   c. Jika pilih **Signal** — saya akan gunakan `signal-design-system-complete.md` sebagai acuan:
      - Color tokens: Primary (Red), Secondary (Navy), Valid (Green), Warning, Info, Neutral
      - Typography: Signal font scale
      - Spacing: Signal spacing scale
      - Components: mengikuti Signal component patterns
      - File `signal-design-system-complete.md` akan menjadi steering untuk semua UI development
6. Apakah GitLab credentials sudah di-setup?
   - GITLAB_PERSONAL_ACCESS_TOKEN (dari GitLab → User Settings → Access Tokens, scope: api, read_repository, write_repository)
   - GITLAB_API_URL (contoh: https://gitlab.com/api/v4)
   
   Jika BELUM:
   → Saya akan bantu set environment variable di OS Anda.
   → Windows: [Environment]::SetEnvironmentVariable('GITLAB_PERSONAL_ACCESS_TOKEN', 'glpat-xxx', 'User')
   → Linux/Mac: export GITLAB_PERSONAL_ACCESS_TOKEN='glpat-xxx' (di ~/.bashrc)
   → Setelah set, restart Kiro.
   
   Jika SUDAH: lanjut.
   
   **→ LANGSUNG generate `.kiro/settings/mcp.json` dengan autoApprove list** (lihat Step 4.6 di bawah). JANGAN tunggu sampai akhir setup.

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

**Jika user pilih Signal Design System:**

AI Agent WAJIB:
1. Gunakan `steering/signal-design-system-complete.md` sebagai **sumber kebenaran** untuk semua UI development
2. Generate `.kiro/steering/design-system.md` dengan konten dari `signal-design-system-complete.md` (`inclusion: always`)
3. Generate `src/presentation/styles/tokens/` berdasarkan Signal color/typography/spacing tokens
4. Semua component yang dibuat HARUS menggunakan Signal CSS variables (`--signal-color-*`, `--signal-spacing-*`, dll)
5. DILARANG menggunakan warna/font/spacing yang tidak ada di Signal Design System

**Rules Signal Design System:**
- Semua component → gunakan `--signal-color-*` variables
- Typography → gunakan Signal font scale
- Spacing → gunakan Signal spacing scale
- Dark/Light theme → gunakan Signal theme tokens
- **Atomic Design TETAP WAJIB** — Signal tokens dipakai di dalam struktur atoms/molecules/organisms
- Jika component baru dibutuhkan → ikuti Signal component patterns + Atomic Design hierarchy
- `signal-design-system-complete.md` adalah **single source of truth** untuk UI
- Struktur folder tetap: `src/presentation/components/{atoms,molecules,organisms}/`

---

**Jika user pilih Custom (punya Design System sendiri):**

AI Agent WAJIB:
1. Terima detail design system dari user (colors, typography, spacing, component library)
2. **Jika user mengirimkan FILE** (PDF, gambar, Figma export, JSON tokens, atau dokumen lain):
   - **Baca dan extract** seluruh informasi design system dari file tersebut
   - Extract: color palette, typography, spacing, border radius, shadows, breakpoints, component list
   - Jika file adalah gambar → describe visual elements dan extract warna/layout
   - Jika file adalah JSON/code → parse tokens langsung
   - Jika file adalah PDF/dokumen → extract semua design specifications
3. Generate `.kiro/steering/design-system.md` dengan **FULL extracted content** (`inclusion: always`)
4. Generate `src/presentation/styles/tokens/` berdasarkan extracted data
5. Sesuaikan `docs/design/ui-ux/theming.md` dengan tokens yang di-extract
6. Sesuaikan `docs/design/ui-ux/component-library.md` dengan library/components yang di-extract

**Jika user kirim file design system, extract SEMUA informasi berikut:**
- Color palette (primary, secondary, neutral, semantic, dark/light variants)
- Typography (font family, sizes, line heights, weights, letter spacing)
- Spacing scale (4px, 8px, 12px, 16px, dll)
- Border radius values
- Shadow values (elevation levels)
- Breakpoints (mobile, tablet, desktop)
- Component list + variants (button sizes, input states, dll)
- Icon set/library yang dipakai
- Animation/transition values

**Template `.kiro/steering/design-system.md` (jika user punya design system):**
```markdown
---
inclusion: always
---

# Design System — [Project Name]

## Source
[Dari file: [nama file yang user kirim] — extracted [tanggal]]

## Color Tokens
### Light Theme
- Primary: [hex]
- Secondary: [hex]
- Neutral: [scale 50-900]
- Semantic: success [hex], error [hex], warning [hex], info [hex]

### Dark Theme
- Primary: [hex]
- Secondary: [hex]
- Background: [hex]
- Surface: [hex]
- Text: [hex]

## Typography
- Font Family: [name]
- Heading: [sizes H1-H6 + weights]
- Body: [sizes + line heights]
- Caption/Small: [size]

## Spacing
[Scale: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96]

## Border Radius
- Small: [value]
- Medium: [value]
- Large: [value]
- Full: 9999px

## Shadows
- sm: [value]
- md: [value]
- lg: [value]

## Breakpoints
- Mobile: [value]
- Tablet: [value]
- Desktop: [value]

## Component Library
[Library: shadcn / MUI / Ant Design / custom]
[Component list + variants]

## Icons
[Icon set: Lucide / Heroicons / custom]

## Rules
- Semua component WAJIB menggunakan tokens di atas
- DILARANG hardcode warna/font/spacing
- Jika membuat component baru → ikuti pattern library yang dipilih
- Dark/Light theme WAJIB menggunakan token variants
- Semua values di atas adalah SUMBER KEBENARAN — jangan deviate
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
| **8 — Issue-Driven** | GitLab: issues + milestone + labels + board + issue templates (Feature, Task, Bug) + MR templates + **Wiki pages (Home, Changelog, API-Documentation, Architecture-Decisions)** + **`.kiro/settings/mcp.json` dengan autoApprove** + **`.kiro/hooks/` (9 files)** | ✅ semua harus ada |
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

**⚠️ MODE PERSISTENCE (WAJIB CEK SETIAP LAYER):**

AI Agent **WAJIB** mengecek mode yang dipilih user **SEBELUM mengerjakan setiap layer**. Mode yang dipilih di pertanyaan #7 saat inisiasi TIDAK BOLEH berubah tanpa user eksplisit minta.

```
[Sebelum mulai Layer N]
    ↓
[CEK: Mode apa yang dipilih user?]
    → Baca dari docs/CURRENT-STATE.md field "Mode"
    → Jika CURRENT-STATE belum ada → default Enterprise
    → GUNAKAN mode tersebut untuk layer ini
    ↓
[DILARANG:]
    ❌ Mengubah mode tanpa user minta
    ❌ Assume Solo karena "project kecil" atau "1 developer"
    ❌ Switch ke mode lain di tengah layer execution
    ❌ Mengabaikan mode yang sudah dipilih
```

**AI Agent WAJIB simpan mode di `docs/CURRENT-STATE.md`:**
```markdown
## Mode
- **Mode:** Enterprise (dipilih saat inisiasi)
- **Diubah:** Tidak (atau: diubah ke Solo pada [tanggal] atas permintaan user)
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

```
"sprint selesai" / "sprint done" / "semua task sudah selesai" / "sprint complete" / "selesaikan sprint ini"
```

**Flow Sprint Completion (detail lengkap: `steering/git-workflow-automation.md`):**

1. **LOCAL VALIDATION (BLOCKING):** lint + typecheck + test (coverage >= 80%). Jika gagal → fix dulu.
2. **Push** semua perubahan yang belum di-push.
3. **CODE REVIEW OFFER:** Tanyakan user apakah mau AI review sebelum MR (architecture, security, best practices).
4. **Create MR** (feature → develop). JANGAN auto-merge. **Description WAJIB formatted markdown** (lihat template di bawah).
5. **Update issues** (status::review) + milestone description.
6. **Informasikan user:** MR URL + instruksi merge di GitLab.

**⚠️ MR Description Format (WAJIB — jangan kirim sebagai 1 baris):**

Saat memanggil `create_merge_request`, field `description` HARUS menggunakan **proper markdown** dengan newlines yang benar. JANGAN gunakan `\n` literal.

```
Tool: create_merge_request
Arguments: {
  "project_id": "<id>",
  "source_branch": "feature/issue-N-description",
  "target_branch": "develop",
  "title": "feat: Sprint [N] — [Feature Name]",
  "description": "## Summary\n\n[1-2 kalimat apa yang dibangun]\n\n## Related Issues\n\nCloses #[N], #[N]\n\n## Local Validation\n\n- ✅ Lint: 0 errors\n- ✅ Typecheck: 0 errors\n- ✅ Tests: [N]/[N] pass\n- ✅ Coverage: [X%]\n\n## Changes\n\n- [Perubahan utama 1]\n- [Perubahan utama 2]\n- [Perubahan utama 3]\n\n## Acceptance Criteria\n\n| AC | Description | Status |\n|-----|-------------|--------|\n| AC-001 | [desc] | ✅ |\n| AC-002 | [desc] | ✅ |\n\n## Checklist\n\n- [x] Architecture compliance\n- [x] Tests written\n- [x] Coverage >= 80%\n- [x] No security findings\n- [x] Observability added\n- [x] Conventional commits"
}
```

**JANGAN seperti ini (SALAH):**
```
description: "Summary\n\nAdd language switcher...\n\n## Related Issue\n\nCloses #33..."
```
↑ Ini menghasilkan `\n` literal di GitLab, bukan newline.

**HARUS seperti ini (BENAR):**
Gunakan actual newline characters dalam string, atau pastikan GitLab MCP tool meng-interpret `\n` sebagai newline.
7. **Setelah user bilang "sudah merged"** → Tawarkan Sprint Completion Package:
   - 📚 Retrospective (docs/retrospectives/sprint-[N].md)
   - 📊 Scorecard (docs/quality/sprint-[N]-scorecard.md)
   - 🏗️ Health Check (docs/quality/health-check-[date].md)
   - 📖 GitLab Wiki Update (Changelog, API-Documentation, Architecture-Decisions)
8. **Close/carry-over milestone** + update CURRENT-STATE.md (sprint = N+1)

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
| **CLOSE semua sprint issues** | `update_issue` | **state_event: "close"** + Label → `status::done` |
| Milestone | `edit_milestone` | Close (jika semua done) atau update description |
| Improvement issues | `create_issue` | Dari retrospective action items |

**⚠️ CRITICAL — Close Issue vs Update Label:**

Label `status::done` dan state `closed` adalah **2 hal BERBEDA** di GitLab:
- Label `status::done` = visual indicator di board (TIDAK close issue)
- State `closed` = actual issue state (issue benar-benar selesai)

**AI Agent WAJIB melakukan KEDUANYA:**
```
# WAJIB: Update label DAN close issue
Tool: update_issue
Arguments: {
  "project_id": "<project-id>",
  "issue_iid": "<issue-number>",
  "labels": ["status::done", ...other labels],
  "state_event": "close"    ← INI YANG SERING TERLEWAT!
}
```

**DILARANG:**
- ❌ Hanya update label tanpa close issue
- ❌ Biarkan issue state "opened" meskipun sudah `status::done`
- ❌ Mengandalkan "Closes #N" di commit saja (hanya bekerja saat MR merged, bukan per-task)

**WAJIB:**
- ✅ Setiap task selesai + pushed → update label `status::review`
- ✅ Setelah MR merged → **close issue** (`state_event: "close"`) + label `status::done`
- ✅ Verify: issue state = "closed" (bukan hanya label berubah)

**Rules:**
- Wiki update WAJIB — bukan opsional
- Issue **CLOSE** WAJIB — bukan hanya label update
- Milestone close/carry-over WAJIB — jangan biarkan milestone open tanpa alasan
- Jika GitLab MCP gagal → retry 2x → informasikan user untuk manual update
- **Setelah close issues → verify** bahwa state benar-benar "closed"

---

### 3. WAJIB Ikuti Workflow Power di Setiap Sprint

Setiap sprint yang dijalankan **WAJIB** mengikuti workflow lengkap framework ini. AI Agent DILARANG "shortcut" atau skip step.

**Sprint Workflow WAJIB (tidak boleh di-skip):**

```
[SPRINT PLANNING]
1. Buat/update milestone di GitLab (title, start_date, due_date, description)
   Tool: create_milestone { project_id, title: "Sprint [N]", start_date, due_date }
   **VERIFIKASI: milestone berhasil dibuat (cek response)**
2. Breakdown fitur → issues (Epic → Feature → Task)
3. Assign issues ke milestone + label sprint::[N]
4. Buat labels jika ada domain baru
5. **Buat/update GitLab Wiki pages (WAJIB):**
   Tool: create_or_update_wiki_page { project_id, title, content }
   - "Home": project overview, tech stack, team, links (jika belum ada)
   - "Changelog": tambah section Sprint [N] (kosong, akan diisi saat sprint end)
   - "API-Documentation": (jika belum ada, buat page kosong)
   - "Architecture-Decisions": (jika belum ada, buat page kosong)
   **VERIFIKASI: semua 4 wiki pages berhasil dibuat (cek response)**
6. Buat branch dari develop (atau main di Solo Mode)

**⚠️ JIKA MILESTONE ATAU WIKI GAGAL DIBUAT:**
- Retry 2x
- Jika masih gagal → cek: apakah MCP config ada? Token valid? API URL benar?
- Informasikan user: "Milestone/Wiki gagal dibuat. Cek GitLab credentials."
- JANGAN lanjut ke sprint execution tanpa milestone + wiki

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
- ❌ **Push atau Create MR jika test coverage < 80%** — FIX DULU, tambah test
- ❌ **Push atau Create MR jika ada test yang fail** — FIX DULU
- ❌ **Push atau Create MR jika lint/typecheck error** — FIX DULU
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
   → Labels: type::feat/fix/chore, priority::*, domain::*, **sprint::[N]**
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
3. **SELALU tambahkan label `sprint::[N]`** — setiap issue harus jelas masuk sprint berapa
4. **SELALU ikuti per-task workflow** — tidak ada shortcut
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

27 engineering standards terintegrasi. Lihat `docs/DOCUMENTATION.md` section 5 untuk mapping lengkap.

## Getting Started & MCP Setup

- Lihat `docs/USER-GUIDE.md` untuk panduan instalasi dan setup lengkap
- Lihat `docs/DOCUMENTATION.md` section 5 untuk integrated standards (27 items)
- MCP Servers: GitLab (`@zereight/mcp-gitlab`) + Git (`@cyanheads/git-mcp-server`)
- Credentials: set via system environment variables (`${GITLAB_PERSONAL_ACCESS_TOKEN}`, `${GITLAB_API_URL}`)
- Setup: Windows `[Environment]::SetEnvironmentVariable(...)` / Linux `export ...` → restart Kiro

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

**Template:** Lihat `steering/project-structure.md` section "CI-Readiness" untuk template lengkap coding-conventions.md.

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

**Hook File Contents:**

Untuk detail lengkap JSON content setiap hook, lihat `steering/hooks-registry.md` (inclusion: always — selalu di-load ke context).

AI Agent WAJIB generate 9 hook files sesuai definisi di hooks-registry.md.

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
