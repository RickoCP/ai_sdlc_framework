# Panduan: Enterprise AI-SDLC Power + Kiro Spec

## Overview

Dokumen ini menjelaskan cara menggunakan **Enterprise AI-Native SDLC Framework (Power)** bersamaan dengan **Kiro Spec** untuk mendapatkan workflow development yang optimal.

---

## Apa Bedanya

| Aspek | Power (Framework) | Kiro Spec |
|-------|-------------------|-----------|
| Scope | Seluruh project lifecycle | Per-fitur/feature |
| Output | 70+ docs, GitLab, hooks, CI/CD | `.kiro/specs/[feature]/` (3 files) |
| Fokus | Governance, architecture, quality | Requirements → Design → Tasks |
| Kapan | Sekali setup + always-on hooks | Setiap fitur baru |
| Automation | Hooks + CI/CD + GitLab | Task execution via Kiro |

**Keduanya saling melengkapi:**
- Power = **infrastruktur & governance** (rules, gates, automation)
- Kiro Spec = **per-feature workflow** (breakdown fitur menjadi implementable tasks)

---

## Workflow Gabungan

```
┌─────────────────────────────────────────────────────────────┐
│  PHASE 1: Project Setup (Power — sekali di awal)            │
│                                                             │
│  Layer 0-8: Vision → Requirements → Spec → Design →        │
│  Governance → Skills → Team → Issues                        │
│  + Generate hooks, steering, MCP config                     │
└─────────────────────────────┬───────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  PHASE 2: Feature Development (Kiro Spec — per fitur)       │
│                                                             │
│  Untuk setiap fitur baru:                                   │
│  1. Buat Kiro Spec (requirements → design → tasks)          │
│  2. Kiro Spec tasks = GitLab issues                         │
│  3. Execute tasks (hooks Power tetap aktif)                 │
└─────────────────────────────┬───────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  PHASE 3: Quality & Learning (Power — otomatis)             │
│                                                             │
│  Layer 9-14 aktif via hooks:                                │
│  - Architect Gate (baca spec + wireframe)                   │
│  - Code Quality Scan (security + observability)             │
│  - QA + DevOps (lint + test + push)                         │
│  - Metrics Collection                                       │
│  - Sprint Completion (retro + scorecard + wiki)             │
└─────────────────────────────────────────────────────────────┘
```

---

## Step-by-Step: Cara Pakai

### Step 1: Setup Project dengan Power (Sekali)

```
1. Aktifkan power → jawab 8 pertanyaan
2. Power setup: git, GitLab, hooks, steering, folder structure
3. Jalankan Layer 0-8 (sequential)
4. Project ready untuk development
```

### Step 2: Buat Kiro Spec untuk Fitur Baru

Saat mau implement fitur baru, buat Kiro Spec:

**Di Kiro IDE:** Klik "New Spec" atau gunakan command palette → "Create Spec"

**Kiro Spec akan generate 3 files:**
```
.kiro/specs/[feature-name]/
├── requirements.md    ← User stories + acceptance criteria
├── design.md          ← Technical design + data flow
└── tasks.md           ← Breakdown menjadi implementable tasks
```

### Step 3: Hubungkan Kiro Spec dengan Framework Docs

Di Kiro Spec, referensikan docs dari framework:

```markdown
# .kiro/specs/payment-topup/requirements.md

## Context
#[[file:docs/specs/srs/payment-topup-spec.md]]
#[[file:docs/design/technical/payment-flow.md]]
#[[file:docs/design/security/threat-model.md]]

## User Stories
- As a user, I want to top up my balance...
```

```markdown
# .kiro/specs/payment-topup/design.md

## Architecture Reference
#[[file:.kiro/steering/architecture-standards.md]]

## Design
- Layer: core/domains/payment/
- Use Case: CreateTopUpUseCase
- Repository: PaymentRepositoryImpl
```

### Step 4: Execute Kiro Spec Tasks

Saat Kiro Spec tasks dieksekusi, **hooks dari Power tetap aktif**:

```
[Kiro Spec Task: "Implement CreateTopUpUseCase"]
    ↓
[Hook: architect-gate] → Baca spec + wireframe + requirements
    ↓
[AI implement code] → Ikuti architecture-standards
    ↓
[Hook: code-quality-scan] → Security + observability check
    ↓
[Hook: qa-devops-post-task] → Lint + test + push + update GitLab
    ↓
[Hook: metrics-collector] → Collect metrics
```

### Step 5: Sprint Completion (Power)

Setelah semua Kiro Spec tasks selesai:

```
User: "sprint selesai"
    ↓
Power hooks: compliance check → MR → retro → scorecard → wiki
```

---

## Mapping: Kiro Spec ↔ Framework Layers

| Kiro Spec Phase | Framework Layer | Apa yang Terjadi |
|----------------|-----------------|------------------|
| **Requirements** | Layer 1-3 | Kiro Spec baca dari `docs/specs/srs/` dan `docs/requirements/` |
| **Design** | Layer 4 | Kiro Spec baca dari `docs/design/` dan `architecture-standards.md` |
| **Tasks** | Layer 8 | Setiap task = 1 GitLab issue + 1 branch |
| **Task Execution** | Layer 9-14 | Hooks aktif: architect-gate, QA, security, metrics |

---

## Contoh Lengkap: Fitur "Payment Top Up"

### 1. Framework sudah setup (Layer 0-8 done)

Docs yang sudah ada:
- `docs/specs/srs/payment-topup-spec.md` (dari Layer 3)
- `docs/design/technical/payment-flow.md` (dari Layer 4)
- `docs/design/security/threat-model.md` (dari Layer 4)

### 2. Buat Kiro Spec

```markdown
# .kiro/specs/payment-topup/requirements.md

## Feature: Payment Top Up

## Context
#[[file:docs/specs/srs/payment-topup-spec.md]]

## Requirements
1. User bisa top up saldo via transfer bank
2. Minimum top up: Rp 10.000
3. Maximum top up: Rp 10.000.000 per hari
4. Notifikasi setelah top up berhasil

## Acceptance Criteria
- [ ] AC-001: Form top up dengan validasi amount
- [ ] AC-002: Integrasi payment gateway
- [ ] AC-003: Update saldo real-time
- [ ] AC-004: Kirim notifikasi (push + email)
```

```markdown
# .kiro/specs/payment-topup/design.md

## Architecture
#[[file:.kiro/steering/architecture-standards.md]]

## Components
- Entity: `src/core/domains/payment/entities/TopUp.ts`
- Use Case: `src/core/domains/payment/usecases/CreateTopUpUseCase.ts`
- Repository: `src/infrastructure/repositories/payment/TopUpRepositoryImpl.ts`
- ViewModel: `src/presentation/features/topup/viewModel/useTopUpViewModel.ts`

## Data Flow
```
UI (TopUpForm) → ViewModel → UseCase → Repository → Payment Gateway API
                                                   → Update Balance
                                                   → Send Notification
```
```

```markdown
# .kiro/specs/payment-topup/tasks.md

## Tasks

- [ ] 1. Create TopUp entity + validation rules
- [ ] 2. Create CreateTopUpUseCase with business logic
- [ ] 3. Create TopUpRepositoryImpl (payment gateway integration)
- [ ] 4. Create useTopUpViewModel (UI state management)
- [ ] 5. Create TopUpForm component (Atomic Design)
- [ ] 6. Write unit tests (coverage >= 80%)
- [ ] 7. Add observability (logger + metrics)
```

### 3. Execute Tasks (Power hooks aktif)

```
━━━ 🏗️ ARCHITECT AGENT ━━━
Loading context from Kiro Spec + framework docs...
- Spec: .kiro/specs/payment-topup/requirements.md ✅
- Design: .kiro/specs/payment-topup/design.md ✅
- Architecture: .kiro/steering/architecture-standards.md ✅
Status: ✅ Ready to implement

━━━ ⚙️ BACKEND AGENT ━━━
Implementing Task 1: Create TopUp entity...
- Created: src/core/domains/payment/entities/TopUp.ts
Status: ✅ Done

━━━ 🧪 QA AGENT ━━━
- Lint: ✅ | Typecheck: ✅ | Tests: 8/8 ✅ | Coverage: 91%
Status: ✅ Pushed to feature/issue-20-payment-topup
```

---

## Tips Kombinasi

### 1. Gunakan Kiro Spec untuk Fitur Kompleks

```
Fitur sederhana (1-2 files) → langsung coding (tanpa Kiro Spec)
Fitur kompleks (3+ files, multiple layers) → buat Kiro Spec dulu
```

### 2. Reference Framework Docs di Kiro Spec

Selalu gunakan `#[[file:...]]` untuk link ke framework docs:
```markdown
#[[file:docs/specs/srs/payment-topup-spec.md]]
#[[file:docs/design/ui-ux/wireframe.md]]
#[[file:.kiro/steering/architecture-standards.md]]
```

### 3. Kiro Spec Tasks = GitLab Issues

Setiap task di Kiro Spec sebaiknya correspond ke 1 GitLab issue:
```
Kiro Spec Task 1 → GitLab Issue #20
Kiro Spec Task 2 → GitLab Issue #21
...
```

### 4. Hooks Tetap Aktif

Saat Kiro Spec task dieksekusi:
- `preTaskExecution` → architect-gate (baca context)
- `postToolUse` → code-quality-scan (security + observability)
- `postTaskExecution` → QA + DevOps + metrics

### 5. Sprint = Kumpulan Kiro Specs

```
Sprint 1:
├── Kiro Spec: Payment Top Up (5 tasks)
├── Kiro Spec: Notification Service (3 tasks)
└── Kiro Spec: User Profile (4 tasks)
    = 12 GitLab issues total
```

---

## Kapan Pakai Kiro Spec vs Langsung Coding

| Situasi | Gunakan |
|---------|---------|
| Fitur baru kompleks (3+ layers) | Kiro Spec |
| Bug fix sederhana | Langsung coding (hooks tetap aktif) |
| Refactoring | Langsung coding |
| Fitur dengan UI + API + DB | Kiro Spec |
| Config/tooling changes | Langsung coding |
| Fitur dengan acceptance criteria banyak | Kiro Spec |

---

## Keuntungan Kombinasi

| Benefit | Dari Power | Dari Kiro Spec |
|---------|-----------|----------------|
| Architecture enforced | ✅ | |
| Per-feature breakdown | | ✅ |
| Quality gates (CI/CD) | ✅ | |
| Task tracking (visual) | | ✅ |
| GitLab integration | ✅ | |
| Context accumulation | ✅ | |
| Spec-first approach | ✅ | ✅ |
| Design reference | ✅ | ✅ |
| Hooks automation | ✅ | |
| Sprint management | ✅ | |

---

## Quick Start

```
1. Install power + jawab 8 pertanyaan → project setup
2. Untuk setiap fitur baru:
   a. Buat Kiro Spec (requirements → design → tasks)
   b. Reference framework docs di spec
   c. Execute tasks (hooks otomatis aktif)
3. Sprint end → retro + scorecard + wiki (otomatis ditawarkan)
```

---

*Power + Kiro Spec = Governance + Per-Feature Workflow = Enterprise-Grade AI Development*
