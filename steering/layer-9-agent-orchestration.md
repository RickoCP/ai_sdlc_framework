# Layer 9 — AI Agent Orchestration (Practical Implementation)

## Overview

Multi-agent di Kiro saat ini diimplementasikan melalui 3 mekanisme:
1. **Role-Based Prompting** — AI berganti "topi" sesuai task
2. **Sub-Agent Delegation** — Task didelegasikan ke sub-agent dengan context terisolasi
3. **Hook Chains** — Hooks yang simulate agent handoff otomatis

Ini BUKAN true parallel multi-agent, tapi menghasilkan output yang **setara** untuk kebanyakan use case.

---

## Agent Registry

### 7 Agent Roles

| Agent | Tanggung Jawab | Trigger | Output |
|-------|---------------|---------|--------|
| **BA Agent** | Requirement extraction, user story | User upload dokumen | `docs/requirements/` |
| **Architect Agent** | Design, ADR, architecture compliance | Fitur baru / review | `docs/design/`, `docs/adr/` |
| **Security Agent** | Threat model, vulnerability check | Code change / review | Security report |
| **Backend Agent** | Use case, repository, API | Task implementation | `src/core/`, `src/infrastructure/` |
| **Frontend Agent** | Component, screen, ViewModel | UI task | `src/presentation/` |
| **QA Agent** | Test writing, coverage analysis | Post-implementation | `tests/` |
| **DevOps Agent** | Git, CI/CD, deployment | Post-task / sprint end | Pipeline, MR |

### Agent → Skill Mapping

| Agent | Steering Files yang Digunakan | Skills |
|-------|------------------------------|--------|
| BA Agent | layer-1, layer-2 | Requirement extraction |
| Architect Agent | architecture-standards, layer-4, layer-10 | Design, ADR |
| Security Agent | layer-4 (security appendix), layer-5 | Threat model, review |
| Backend Agent | architecture-standards, layer-6, layer-13 | create-usecase, create-repository, create-api |
| Frontend Agent | architecture-standards, layer-4 (UI/UX) | create-component, Atomic Design |
| QA Agent | test-writing-patterns, layer-12 | create-test, coverage |
| DevOps Agent | git-workflow-automation, layer-12 | CI/CD, git ops |

---

## Implementasi 1: Role-Based Prompting

### Cara Kerja

AI Agent mengadopsi role tertentu yang mengubah:
- **Perspektif** — apa yang diperhatikan
- **Output format** — apa yang dihasilkan
- **Constraints** — apa yang tidak boleh dilakukan
- **Reference** — steering files mana yang dibaca

### Role Activation Patterns

```
# Eksplisit — user minta role tertentu
"Sebagai Security Agent, review code ini"
"Sebagai QA Agent, tulis test untuk payment use case"
"Sebagai Architect Agent, apakah design ini sudah benar?"

# Implisit — AI auto-detect role dari task type
"Implement fitur X"        → Backend Agent (primary) + QA Agent (secondary)
"Review PR ini"            → Security Agent + Architect Agent
"Fix bug #23"              → Backend Agent + QA Agent + Learning (Layer 14)
"Setup monitoring"         → DevOps Agent + Observability (Layer 13)
"Buat wireframe payment"   → Frontend Agent
```

### Role Prompt Templates

#### Backend Agent Prompt
```
Kamu adalah Backend Agent. Tanggung jawabmu:
- Implement business logic di src/core/ dan src/infrastructure/
- Ikuti Clean Architecture (architecture-standards.md)
- Gunakan DI pattern (Awilix factory function)
- Tambahkan observability (logger + metrics) di setiap use case dan repository
- JANGAN sentuh src/presentation/ atau src/app/

Context yang HARUS dibaca:
- docs/specs/srs/[feature]-spec.md (requirement)
- src/core/domains/[domain]/ (existing entities/interfaces)
- src/infrastructure/di/registry/ (existing registrations)
```

#### QA Agent Prompt
```
Kamu adalah QA Agent. Tanggung jawabmu:
- Tulis test sesuai test-writing-patterns.md
- Cover: happy path, error path, edge case
- Mock dependency langsung (bukan container)
- Target coverage >= 80% (95% untuk auth/payment)
- JANGAN ubah source code — hanya tulis test

Context yang HARUS dibaca:
- Source file yang akan ditest
- test-writing-patterns.md (pattern per layer)
- Existing tests di tests/ (untuk consistency)
```

#### Security Agent Prompt
```
Kamu adalah Security Agent. Tanggung jawabmu:
- Review code dari perspektif security
- Check: input validation, auth/authz, data exposure, injection
- Reference: OWASP Top 10, layer-4 security appendix
- JANGAN fix code — hanya report findings
- Severity: Critical / High / Medium / Low

Output format:
| Finding | Severity | File | Line | Recommendation |
```

#### Architect Agent Prompt
```
Kamu adalah Architect Agent. Tanggung jawabmu:
- Validate architecture compliance
- Check: dependency rule, layer placement, DI usage
- Detect: circular dependencies, layer violations
- Reference: architecture-standards.md
- Jika ada keputusan arsitektur baru → suggest ADR

Output: compliance report + recommendations
```

---

## Implementasi 2: Sub-Agent Delegation

### Cara Kerja

Kiro punya `invokeSubAgent` yang mendelegasikan task ke context terisolasi. Ini memungkinkan:
- Main agent tetap fokus di orchestration
- Sub-agent bekerja di context terpisah (tidak polusi main context)
- Hasil dikembalikan ke main agent untuk review

### Delegation Patterns

#### Pattern A: Sequential Delegation (Fitur Baru)

```
Main Agent (Orchestrator):
  "User minta implement fitur Payment Top Up"
  
  1. [Self — Architect role]
     → Baca spec, validate design exists
     
  2. [Delegate ke Sub-Agent — Backend role]
     → "Implement entity, use case, repository untuk Payment Top Up
        berdasarkan spec di docs/specs/srs/payment-topup-spec.md.
        Ikuti architecture-standards.md."
     → Sub-agent return: list files created
     
  3. [Delegate ke Sub-Agent — QA role]
     → "Tulis test untuk files berikut: [list dari step 2].
        Ikuti test-writing-patterns.md.
        Target coverage >= 80%."
     → Sub-agent return: test files + coverage report
     
  4. [Self — DevOps role]
     → Lint + Typecheck + Test + Commit + Push
```

#### Pattern B: Review Delegation (Code Review)

```
Main Agent (Orchestrator):
  "User minta review code di branch feature/issue-15"
  
  1. [Delegate ke Sub-Agent — Security Agent]
     → "Review semua file yang berubah dari perspektif security.
        Check OWASP Top 10, input validation, auth."
     → Return: security findings
     
  2. [Delegate ke Sub-Agent — Architect Agent]
     → "Review semua file yang berubah dari perspektif architecture.
        Check dependency rule, layer placement, DI usage."
     → Return: architecture findings
     
  3. [Self — Synthesize]
     → Gabungkan findings dari kedua sub-agent
     → Present ke user sebagai unified review report
```

#### Pattern C: Parallel-like Delegation (Sprint Planning)

```
Main Agent (Orchestrator):
  "User minta breakdown fitur besar menjadi tasks"
  
  1. [Delegate ke Sub-Agent — BA Agent]
     → "Breakdown fitur ini menjadi user stories + acceptance criteria"
     → Return: user stories
     
  2. [Self — Architect role]
     → Dari user stories, tentukan technical tasks per layer
     
  3. [Self — Create GitLab issues]
     → Buat issues dengan labels agent:: yang sesuai
```

### Kapan Gunakan Sub-Agent vs Self

| Situasi | Gunakan | Alasan |
|---------|---------|--------|
| Task besar (> 5 files) | Sub-Agent | Isolasi context, tidak polusi main |
| Review code | Sub-Agent | Perspektif terisolasi per role |
| Task kecil (1-2 files) | Self (role switch) | Overhead sub-agent tidak worth it |
| Sequential dependency | Self | Sub-agent tidak bisa baca output sub-agent lain |
| Independent tasks | Sub-Agent | Bisa "paralel" (sequential tapi isolated) |

---

## Implementasi 3: Hook Chains (Agent Handoff)

### Cara Kerja

Hooks yang ter-chain simulate "agent handoff" — output satu hook menjadi trigger untuk hook berikutnya.

### Chain: Feature Implementation

```
[preTaskExecution] — Architect Agent
  "Cek spec ada? Design ada? Context loaded?"
      ↓ (task mulai)
[Implementation] — Backend/Frontend Agent
  AI implement code sesuai role
      ↓ (file written)
[postToolUse:write] — Observability Agent
  "Cek: apakah logging/metrics sudah ditambahkan?"
      ↓ (task selesai)
[postTaskExecution] — QA Agent + DevOps Agent
  "Jalankan lint → typecheck → test → commit → push"
      ↓ (jika bug fix)
[postTaskExecution] — Learning Agent
  "Capture bug learning jika ini fix: commit"
```

### Chain: Sprint Completion

```
[User trigger: "sprint selesai"]
      ↓
[DevOps Agent] — Push semua, create MR
      ↓
[QA Agent] — Generate coverage report
      ↓
[Architect Agent] — Check architecture compliance
      ↓
[Learning Agent] — Generate retrospective
      ↓
[BA Agent] — Update Context Index + Current State
```

### Hook Definitions untuk Agent Chain

```json
{
  "name": "Agent Chain: Architect Gate",
  "version": "1.0.0",
  "description": "Architect Agent validates before task starts",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: Architect Agent\n\nSebelum mulai task:\n1. Baca spec untuk fitur ini (docs/specs/srs/)\n2. Baca design document (docs/design/)\n3. Baca ADR yang relevan (docs/adr/)\n4. Validate: apakah task ini sesuai dengan arsitektur yang sudah diputuskan?\n5. Jika ada conflict → informasikan user sebelum lanjut\n6. Jika OK → set context untuk Backend/Frontend Agent"
  }
}
```

```json
{
  "name": "Agent Chain: Security Review Post-Write",
  "version": "1.0.0",
  "description": "Security Agent reviews setiap file source yang ditulis",
  "when": {
    "type": "postToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: Security Agent\n\nJika file yang baru ditulis adalah source code di src/:\n1. Quick security scan:\n   - Input validation ada?\n   - Sensitive data tidak di-log?\n   - Auth check ada (jika endpoint protected)?\n   - SQL injection safe (parameterized)?\n2. Jika ada finding CRITICAL → STOP, informasikan user\n3. Jika ada finding MEDIUM/LOW → catat, lanjut\n4. Jika clean → skip\n\nJika file bukan source code → skip entirely."
  }
}
```

```json
{
  "name": "Agent Chain: QA + DevOps Post-Task",
  "version": "1.0.0",
  "description": "QA Agent validates tests, DevOps Agent handles git",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "ROLE: QA Agent → DevOps Agent (sequential)\n\n**QA Agent Phase:**\n1. Cek: apakah test sudah ditulis untuk code baru?\n2. Jika BELUM → tulis test sekarang (ikuti test-writing-patterns.md)\n3. Jalankan: npm run lint → npm run typecheck → npm run test:unit -- --coverage\n4. Semua HARUS pass. Coverage >= 80%.\n5. Jika gagal → fix sebelum lanjut ke DevOps phase\n\n**DevOps Agent Phase (hanya jika QA pass):**\n6. git add (relevant files only)\n7. git commit (conventional format)\n8. git push ke feature branch\n9. Update GitLab issue status\n10. Jika sprint task terakhir → create MR\n\nInformasikan user: 'QA: ✅ [N] tests, [X%] coverage | DevOps: pushed to [branch]'"
  }
}
```

---

## Orchestration Patterns

### Pattern 1: Full Feature (Sequential)

```
User: "Implement fitur [X]"

Orchestrator decides:
  1. Architect Agent → validate spec + design
  2. Backend Agent → implement core + infrastructure
  3. Frontend Agent → implement presentation (jika ada UI)
  4. QA Agent → write tests
  5. Security Agent → review
  6. DevOps Agent → lint + test + push
```

### Pattern 2: Bug Fix (Compact)

```
User: "Fix bug #23"

Orchestrator decides:
  1. Backend Agent → fix code
  2. QA Agent → add regression test
  3. DevOps Agent → push
  4. Learning Agent → capture bug learning
```

### Pattern 3: Code Review (Parallel-like)

```
User: "Review branch feature/issue-15"

Orchestrator delegates:
  Sub-Agent 1 (Security) → security findings
  Sub-Agent 2 (Architect) → architecture findings
  Main Agent → synthesize + present unified report
```

### Pattern 4: Sprint Planning (BA-led)

```
User: "Plan sprint 4"

Orchestrator decides:
  1. BA Agent → breakdown features into stories
  2. Architect Agent → technical task breakdown
  3. DevOps Agent → create GitLab issues + milestone
```

---

## Agent Communication Protocol

### Via File Artifacts (Primary)

Agents communicate melalui file yang di-commit ke repository:

```
Architect Agent writes:  docs/design/technical/payment-flow.md
Backend Agent reads:     docs/design/technical/payment-flow.md → implements
QA Agent reads:          src/core/domains/payment/ → writes tests
Security Agent reads:    src/ changes → writes findings
Learning Agent reads:    git log + issues → writes retrospective
```

### Via Context Files (State)

```
docs/CURRENT-STATE.md    — "Siapa yang terakhir bekerja, apa yang dilakukan"
docs/CONTEXT-INDEX.md    — "Apa saja artifact yang tersedia"
```

### Via GitLab (Tracking)

```
Issue labels:  agent::backend, agent::qa, agent::security
Issue notes:   "Backend Agent: implementation complete. Ready for QA."
MR comments:   "Security Agent: 0 findings. Approved."
```

---

## AI Agent Rules untuk Orchestration

1. **Orchestrator SELALU tentukan role sequence** sebelum mulai — jangan langsung coding
2. **Setiap role switch HARUS ditampilkan dengan format banner** (lihat format di bawah)
3. **Sub-agent untuk task besar** — jika > 5 files, delegate ke sub-agent
4. **Self untuk task kecil** — jika 1-2 files, role switch cukup
5. **Security Agent SELALU jalan** — setiap code change harus di-review security
6. **QA Agent SELALU jalan** — setiap implementation harus punya test
7. **DevOps Agent SELALU di akhir** — lint + test + push adalah closing ritual
8. **Jangan skip agent** — setiap role punya value, skip = risk
9. **WAJIB tampilkan agent aktif** — user harus selalu tahu agent mana yang sedang bekerja
10. **Jika satu "agent" gagal** — stop, informasikan user, jangan lanjut ke agent berikutnya

---

## Format Display Agent (WAJIB)

Saat AI Agent berganti role atau mulai bekerja sebagai agent tertentu, **WAJIB** menampilkan banner berikut:

### Format Banner

```
━━━ 🏗️ ARCHITECT AGENT ━━━
[Apa yang sedang dilakukan]

━━━ ⚙️ BACKEND AGENT ━━━
[Apa yang sedang dilakukan]

━━━ 🎨 FRONTEND AGENT ━━━
[Apa yang sedang dilakukan]

━━━ 🧪 QA AGENT ━━━
[Apa yang sedang dilakukan]

━━━ 🔒 SECURITY AGENT ━━━
[Apa yang sedang dilakukan]

━━━ 🚀 DEVOPS AGENT ━━━
[Apa yang sedang dilakukan]

━━━ 📋 BA AGENT ━━━
[Apa yang sedang dilakukan]

━━━ 📚 LEARNING AGENT ━━━
[Apa yang sedang dilakukan]
```

### Emoji per Agent (WAJIB konsisten)

| Agent | Emoji | Banner |
|-------|-------|--------|
| Architect Agent | 🏗️ | `━━━ 🏗️ ARCHITECT AGENT ━━━` |
| Backend Agent | ⚙️ | `━━━ ⚙️ BACKEND AGENT ━━━` |
| Frontend Agent | 🎨 | `━━━ 🎨 FRONTEND AGENT ━━━` |
| QA Agent | 🧪 | `━━━ 🧪 QA AGENT ━━━` |
| Security Agent | 🔒 | `━━━ 🔒 SECURITY AGENT ━━━` |
| DevOps Agent | 🚀 | `━━━ 🚀 DEVOPS AGENT ━━━` |
| BA Agent | 📋 | `━━━ 📋 BA AGENT ━━━` |
| Learning Agent | 📚 | `━━━ 📚 LEARNING AGENT ━━━` |

### Rules Display

1. **SELALU tampilkan banner** saat mulai bekerja sebagai agent tertentu
2. **SELALU tampilkan banner** saat berganti role ke agent lain
3. **Sertakan summary** di bawah banner: apa yang akan/sedang dilakukan
4. **Di akhir setiap agent phase**, tampilkan status: ✅ Done / ❌ Failed / ⚠️ Warning
5. **Jangan gabung multiple agent** dalam satu output tanpa banner pemisah
6. **Format ini WAJIB** — bukan opsional, bukan "nice to have"

### Contoh Output yang BENAR

```
━━━ 🏗️ ARCHITECT AGENT ━━━
Validating architecture compliance...
- Spec: docs/specs/srs/payment-spec.md ✅
- Design: docs/design/technical/payment-flow.md ✅
- ADR-002 (Awilix DI): compatible ✅
Status: ✅ Architecture validated

━━━ ⚙️ BACKEND AGENT ━━━
Implementing payment use case...
- Created: src/core/domains/payment/entities/Payment.ts
- Created: src/core/domains/payment/usecases/CreatePaymentUseCase.ts
- Created: src/infrastructure/repositories/payment/PaymentRepositoryImpl.ts
- Registered: DI container updated
- Observability: logger + metrics injected
Status: ✅ Implementation complete (3 files)

━━━ 🧪 QA AGENT ━━━
Writing tests...
- Created: tests/unit/.../CreatePaymentUseCase.test.ts (8 tests)
- Created: tests/unit/.../PaymentRepositoryImpl.test.ts (6 tests)
- Coverage: 91%
Status: ✅ 14 tests pass, coverage 91%

━━━ 🔒 SECURITY AGENT ━━━
Reviewing security...
- Input validation: ✅
- No sensitive data in logs: ✅
- Amount manipulation check: ✅
Status: ✅ 0 findings

━━━ 🚀 DEVOPS AGENT ━━━
Running quality gates + push...
- Lint: ✅ | Typecheck: ✅ | Tests: 14/14 ✅ | Coverage: 91%
- Committed: feat(payment): implement create payment use case
- Pushed: feature/issue-15-payment → origin
Status: ✅ Pushed successfully
```

### Contoh Output yang SALAH (JANGAN seperti ini)

```
❌ SALAH — tidak ada banner, user tidak tahu agent mana yang bekerja:

"Saya akan implement payment use case. Berikut file yang dibuat:
- Payment.ts
- CreatePaymentUseCase.ts
Lalu saya tulis test dan push."

❌ SALAH — banner tidak konsisten:

"## Backend
Implementing...

## Testing
Writing tests..."
```

---

## Contoh Lengkap: End-to-End Feature Implementation

```
User: "Implement fitur NFC Card Registration berdasarkan spec"

AI: "Saya akan mengerjakan ini dengan multi-agent approach:

━━━ 🏗️ ARCHITECT AGENT ━━━
Membaca context...
- Spec: docs/specs/srs/nfc-registration-spec.md ✅
- Design: docs/design/technical/nfc-flow.md ✅
- ADR: ADR-003 (Clean Architecture) ✅
Status: ✅ Architecture validated. Proceeding.

━━━ ⚙️ BACKEND AGENT ━━━
Implementing core + infrastructure...
- Created: src/core/domains/nfc/entities/Card.ts
- Created: src/core/domains/nfc/repositories/CardRepository.ts
- Created: src/core/domains/nfc/usecases/RegisterCardUseCase.ts
- Created: src/infrastructure/repositories/nfc/CardRepositoryImpl.ts
- Registered: DI container updated
- Observability: logger + metrics added
Status: ✅ Implementation complete (4 files)

━━━ 🧪 QA AGENT ━━━
Writing tests...
- Created: tests/unit/core/domains/nfc/usecases/RegisterCardUseCase.test.ts
- Created: tests/unit/infrastructure/repositories/nfc/CardRepositoryImpl.test.ts
- Results: 14 tests pass, coverage 89%
Status: ✅ All tests pass, coverage 89%

━━━ 🔒 SECURITY AGENT ━━━
Reviewing...
- Input validation: ✅
- Card data encryption: ✅
- No sensitive data in logs: ✅
Status: ✅ 0 findings (critical: 0, high: 0)

━━━ 🚀 DEVOPS AGENT ━━━
Running quality gates + push...
- Lint: ✅ | Typecheck: ✅ | Tests: 14/14 ✅ | Coverage: 89%
- Committed: feat(nfc): implement card registration
- Pushed: feature/issue-12-nfc-registration
- GitLab issue #12: status → review
Status: ✅ Pushed successfully

All agents complete. Summary:
- Files created: 6 (4 source + 2 test)
- Tests: 14 pass | Coverage: 89%
- Security: 0 findings
- Branch: feature/issue-12-nfc-registration"
```

---

## Integration dengan Framework

| Layer | Bagaimana Layer 9 Terintegrasi |
|-------|-------------------------------|
| Layer 3 (Spec) | Architect Agent validate spec sebelum implementation |
| Layer 4 (Design) | Architect Agent validate design compliance |
| Layer 8 (Issue-Driven) | DevOps Agent manage GitLab issues + branches |
| Layer 11 (AI Review) | Security + Architect Agent = AI Review |
| Layer 12 (Quality Gates) | QA + DevOps Agent = quality enforcement |
| Layer 13 (Observability) | Backend Agent otomatis tambah observability |
| Layer 14 (Learning) | Learning Agent capture dari bug fix + sprint end |
