# Layer 3 — Spec-Driven Development (SDD)

## Prinsip

```
Idea → Specification → Validation → Architecture → Tasks → Code
```

Tanpa specification: **AI = random output**
Dengan specification: **AI = engineering accelerator**

## Generated Specifications

### Business Requirements Document (BRD)
```
docs/specs/brd/
├── business-flow.md
├── stakeholder-matrix.md
└── business-rules.md
```

### Product Requirements Document (PRD)
```
docs/specs/prd/
├── user-stories.md
├── acceptance-criteria.md
├── feature-matrix.md
└── priority-matrix.md
```

### System Requirements Specification (SRS)
```
docs/specs/srs/
├── [feature]-spec.md
├── api-contract.md
├── state-flow.md
├── failure-scenario.md
└── data-model.md
```

## Template: user-stories.md

```markdown
# User Stories

## Epic: [Nama Epic]

### US-001: [Judul Story]
**As a** [role]
**I want to** [action]
**So that** [benefit]

**Acceptance Criteria:**
- Given [context]
- When [action]
- Then [expected result]

**Priority:** [Must/Should/Could/Won't]
**Story Points:** [1/2/3/5/8/13]
**Dependencies:** [US-xxx]

### US-002: [Judul Story]
...
```

## Template: api-contract.md

```markdown
# API Contract

## Endpoint: [Method] [Path]

### Request
```json
{
  "field": "type - description"
}
```

### Response (200)
```json
{
  "field": "type - description"
}
```

### Error Responses
| Code | Description | Body |
|------|-------------|------|
| 400 | Bad Request | `{"error": "message"}` |
| 401 | Unauthorized | `{"error": "message"}` |
| 404 | Not Found | `{"error": "message"}` |

### Business Rules
1. [Rule 1]
2. [Rule 2]

### Validation Rules
| Field | Rule | Message |
|-------|------|---------|
| email | Required, valid email | "Email is required" |
```

## Template: failure-scenario.md

```markdown
# Failure Scenarios

## FS-001: [Nama Scenario]
- **Trigger:** [Apa yang menyebabkan failure]
- **Impact:** [Dampak ke user/system]
- **Detection:** [Bagaimana mendeteksi]
- **Recovery:** [Bagaimana recover]
- **Prevention:** [Bagaimana mencegah]

## FS-002: Network Timeout
- **Trigger:** API call melebihi timeout threshold
- **Impact:** User melihat error, data tidak tersimpan
- **Detection:** Timeout exception, monitoring alert
- **Recovery:** Retry dengan exponential backoff, show retry button
- **Prevention:** Set reasonable timeout, implement circuit breaker
```

## Workflow dengan Kiro

### Step 1: Generate BRD dari Requirements
```
"Berdasarkan validated requirements di docs/requirements/,
generate Business Requirements Document.
Simpan di docs/specs/brd/"
```

### Step 2: Generate PRD
```
"Berdasarkan BRD di docs/specs/brd/,
generate Product Requirements Document dengan user stories dan acceptance criteria.
Simpan di docs/specs/prd/"
```

### Step 3: Generate SRS
```
"Berdasarkan PRD di docs/specs/prd/,
generate System Requirements Specification.
Include: state flow, failure scenarios, API contracts, data model.
Simpan di docs/specs/srs/"
```

### Step 4: Validate Specs
```
"Review semua specs di docs/specs/.
Check: completeness, consistency, feasibility, testability.
Report any issues."
```

## Kiro Spec Integration

Gunakan Kiro Spec feature untuk setiap fitur:

1. **Create Spec** di Kiro untuk fitur yang akan dibangun
2. **Requirements** - Referensikan docs/specs/prd/user-stories.md
3. **Design** - Referensikan docs/design/system/high-level-architecture.md
4. **Tasks** - Break down menjadi implementable tasks

Contoh Kiro Spec:
```markdown
# Feature: User Authentication

## Requirements
#[[file:docs/specs/prd/user-stories.md]]

## Design
#[[file:docs/design/system/high-level-architecture.md]]
#[[file:docs/design/security/threat-model.md]]

## Tasks
- [ ] Implement login endpoint
- [ ] Implement registration endpoint
- [ ] Implement JWT token generation
- [ ] Implement token refresh
- [ ] Write unit tests
- [ ] Write integration tests
```

## GitLab Integration

### Spec sebagai Merge Request Description

Setiap MR harus mereferensikan spec:

```markdown
## Related Specification

- BRD: docs/specs/brd/business-flow.md
- PRD: docs/specs/prd/user-stories.md#US-001
- SRS: docs/specs/srs/api-contract.md#endpoint-name

## Implementation Notes
[Catatan implementasi]

## Deviation from Spec
[Jika ada perbedaan dari spec, jelaskan alasannya]
```

### CI/CD Check untuk Spec Compliance

```yaml
spec-compliance:
  stage: validate
  script:
    - echo "Checking spec compliance..."
    - |
      # Pastikan setiap file baru punya referensi ke spec
      CHANGED_FILES=$(git diff --name-only origin/main)
      for file in $CHANGED_FILES; do
        if [[ "$file" == src/* ]]; then
          echo "Checking $file has spec reference in MR..."
        fi
      done
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

## Quality Gates

Spec TIDAK BOLEH masuk ke implementation jika:
- User stories tidak punya acceptance criteria
- API contract tidak lengkap
- Failure scenarios belum didefinisikan
- Architecture belum di-review
- Security considerations belum dianalisis

## Best Practices

1. **Spec first, code later** - Jangan coding tanpa spec
2. **Living documents** - Spec harus di-update saat ada perubahan
3. **Traceable** - Setiap code harus bisa di-trace ke spec
4. **Testable** - Acceptance criteria harus bisa dijadikan test
5. **Versioned** - Spec disimpan di Git, bisa di-track perubahannya
6. **Referenced** - Gunakan `#[[file:]]` di Kiro Spec untuk referensi


---

## Appendix: API Contract Guideline (dari API Contract Standards)

### Prinsip API Contract

1. **Contract is a Product** - Kontrak API adalah janji antar sistem
2. **Stable, Explicit, Predictable** - API harus stabil dan dapat diprediksi
3. **UI-friendly, Not Backend-leaky** - Contract untuk UI dirancang sesuai kebutuhan konsumen
4. **Backward Compatibility by Default** - Perubahan harus kompatibel ke belakang
5. **Validation at the Boundary** - Semua request/response divalidasi di boundary

### Struktur Dasar Kontrak

```
Request DTO ≠ Domain Entity ≠ Response DTO
```

### Request Contract Rules

- Semua field input harus jelas: nama, tipe, required/optional, format
- Gunakan tipe yang ketat (hindari `any`)
- Request harus minimal dan sesuai tujuan endpoint
- Validasi WAJIB di boundary: type, required, format, enum, range, length

### Response Contract Rules

- Response harus konsisten antar endpoint
- Kembalikan hanya data yang dibutuhkan
- Gunakan shape yang ramah konsumen
- Gunakan `null` dan `optional` dengan disiplin

### Error Response Contract

```typescript
export interface ErrorResponseDto {
  code: string;          // WAJIB stabil
  message: string;       // Aman untuk konsumen
  correlationId?: string;
  fields?: Record<string, string>; // Untuk validation error
}
```

### Versioning Rules

- Breaking change HARUS ada versioning (`/api/v1/` → `/api/v2/`)
- Additive change lebih diutamakan (tambah field optional)
- Jangan mengubah makna field lama diam-diam
- Deprecation harus jelas dengan timeline migrasi

### Backward Compatibility

**Perubahan aman:**
- Tambah field optional
- Tambah endpoint baru

**Perubahan berisiko breaking:**
- Rename/hapus field
- Ubah type field
- Ubah makna enum
- Ubah struktur nested

---

## Appendix: Specs Workflow (dari Specs Workflow Standards)

### End-to-End Workflow

```
Idea → Problem Statement → Functional Spec → Technical Design
→ API Contract → Task Breakdown → Implementation → Testing
→ Review → CI/CD → Release → Monitoring → Feedback & Iteration
```

### Artefak Utama per Fitur

1. **Problem Statement** - Masalah yang ingin diselesaikan
2. **Functional Spec** - Use case, user flow, acceptance criteria
3. **Technical Design** - Arsitektur, komponen, data flow
4. **API Contract** - Request/response DTO, error response
5. **Test Plan** - Unit, integration, e2e
6. **Release Plan** - Deployment strategy

### Template Ringkas Spec

```markdown
# Feature Name

## Problem
[Latar belakang dan pain point]

## Objective
[Tujuan yang ingin dicapai]

## User Flow
[Alur user step by step]

## Acceptance Criteria
- [ ] [Criteria 1]
- [ ] [Criteria 2]

## Technical Design
[Arsitektur, komponen, data flow]

## API Contract
[Request/response DTO]

## Edge Case
[Skenario edge case]

## Test Plan
[Strategi testing]

## Release Plan
[Deployment strategy]
```

### Prinsip Specs Workflow

1. **Spec Before Code** - Tidak boleh coding tanpa spesifikasi minimal
2. **Shift Left** - Security, testing, performance dipikirkan di fase awal
3. **Single Source of Truth** - Satu fitur = satu dokumen spec utama
4. **Small, Iterative Delivery** - Fitur dipecah scope kecil
5. **Traceability** - Setiap perubahan bisa ditelusuri dari spec → code → release


---

## Workflow Integration — Enforce Spec-First Before Coding

### Prinsip

Spec-Driven Development bukan saran — ini adalah **gate wajib**. AI Agent DILARANG mulai coding fitur kompleks tanpa specification yang sudah divalidasi.

---

### 1. Spec-First Gate (WAJIB)

```
[User minta implement fitur baru]
    ↓
[AI Agent cek: Apakah spec sudah ada untuk fitur ini?]
    ↓
├── SPEC ADA + VALIDATED → Lanjut coding
├── SPEC ADA + BELUM VALIDATED → Jalankan Layer 2 validation dulu
└── SPEC BELUM ADA → Buat spec dulu, JANGAN langsung coding
    ↓
[Buat spec → Validate → User approve → Baru coding]
```

**Kapan spec WAJIB ada:**
- Fitur baru yang melibatkan > 1 layer (use case + repository + UI)
- Fitur yang melibatkan external service/API
- Fitur yang melibatkan data model baru
- Fitur yang punya > 3 acceptance criteria
- Fitur yang melibatkan auth/payment/security

**Kapan spec BOLEH di-skip:**
- Bug fix sederhana (1 file, 1 function)
- Refactoring tanpa perubahan behavior
- Config/tooling changes
- Documentation updates

---

### 2. Kiro Hook: Spec-First Enforcement

```json
{
  "name": "Spec-First Gate",
  "version": "1.0.0",
  "description": "Memastikan spec ada sebelum mulai coding fitur baru",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "SEBELUM mulai task, cek apakah ini FITUR BARU yang membutuhkan spec:\n\n**Kriteria butuh spec:**\n- Melibatkan > 1 layer (use case + repository + UI)\n- Melibatkan external service/API baru\n- Melibatkan data model/entity baru\n- Punya > 3 acceptance criteria\n- Melibatkan auth/payment/security\n\n**Jika BUTUH spec:**\n1. Cek apakah file spec sudah ada di docs/specs/ untuk fitur ini\n2. Jika BELUM ada → JANGAN mulai coding. Buat spec dulu:\n   - docs/specs/srs/[feature-name]-spec.md\n   - Isi: Overview, Acceptance Criteria, Data Flow, API Contract, Error Scenarios\n   - Tanyakan user untuk review spec sebelum implement\n3. Jika SUDAH ada → cek apakah sudah validated (ada validation report)\n4. Jika belum validated → jalankan validation (Layer 2)\n\n**Jika TIDAK butuh spec (bug fix, refactor, config):**\n→ Skip, lanjut langsung ke implementation\n\n**JANGAN PERNAH:**\n- Mulai coding fitur kompleks tanpa spec\n- Buat spec setelah coding selesai (spec-after = documentation, bukan spec-driven)\n- Skip spec karena 'sudah jelas di kepala'"
  }
}
```

---

### 3. Spec Template (Auto-Generate)

Saat AI Agent perlu membuat spec, gunakan template ini:

```markdown
# Specification: [Feature Name]

## Status
Draft | Reviewed | Approved | Implemented

## Overview
[1-2 paragraf menjelaskan fitur]

## User Stories
- As a [actor], I want to [action], so that [benefit]

## Acceptance Criteria
| ID | Criteria | Priority |
|----|----------|----------|
| AC-001 | [Specific, measurable criteria] | Must |
| AC-002 | [Specific, measurable criteria] | Must |
| AC-003 | [Specific, measurable criteria] | Should |

## Data Flow
```
[Input] → [Process] → [Output]
```

## API Contract (jika ada)
### Endpoint
- Method: POST/GET/PUT/DELETE
- Path: /api/v1/[resource]
- Request Body: { ... }
- Response: { ... }
- Error Responses: { ... }

## Error Scenarios
| Scenario | Expected Behavior |
|----------|------------------|
| [Error case] | [How system should respond] |

## Dependencies
- [External service/API]
- [Other features that must exist first]

## Out of Scope
- [What this feature does NOT include]

## Technical Notes
- [Architecture decisions, constraints]
```

---

### 4. Spec Lifecycle

```
[Draft] → AI Agent buat berdasarkan requirement
    ↓
[Review] → User review, berikan feedback
    ↓
[Approved] → User approve, siap implement
    ↓
[Implemented] → Code selesai, spec menjadi documentation
    ↓
[Updated] → Jika ada perubahan saat development, spec di-update
```

**Rules:**
- Spec HARUS di-update jika implementation berbeda dari spec
- Spec yang outdated = tech debt (buat issue)
- Spec disimpan di `docs/specs/srs/[feature-name]-spec.md`

---

### 5. Traceability: Spec → Code → Test

Setiap acceptance criteria di spec HARUS bisa di-trace ke:
- Code yang mengimplementasikannya
- Test yang memvalidasinya

```
AC-001 → src/core/domains/payment/usecases/CreatePayment.ts
       → tests/unit/core/domains/payment/usecases/CreatePayment.test.ts

AC-002 → src/infrastructure/repositories/payment/PaymentRepositoryImpl.ts
       → tests/unit/infrastructure/repositories/payment/PaymentRepositoryImpl.test.ts
```

AI Agent WAJIB menyertakan referensi AC di commit message:
```
feat(payment): implement card registration

Covers: AC-001, AC-002, AC-003
Refs #12
```

---

### 6. Integration dengan Layer Lain

| Layer | Bagaimana Layer 3 Terintegrasi |
|-------|-------------------------------|
| Layer 2 (Validation) | Requirement HARUS validated sebelum spec dibuat |
| Layer 4 (Design) | Spec menjadi input untuk technical design |
| Layer 8 (Issue-Driven) | Setiap issue HARUS reference spec (jika ada) |
| Layer 11 (AI Review) | Review cek: apakah code sesuai spec? |
| Layer 14 (Learning) | Spec yang sering berubah = signal untuk improve intake |

---

### 7. AI Agent Rules

1. **Spec-first untuk fitur kompleks** — WAJIB, bukan opsional
2. **Spec sebelum coding** — bukan setelah (spec-after = documentation)
3. **User HARUS approve spec** sebelum implementation dimulai
4. **Spec HARUS di-update** jika implementation berubah
5. **Setiap AC HARUS traceable** ke code dan test
6. **JANGAN buat spec yang terlalu detail** untuk fitur sederhana — proportional effort
7. **Spec adalah living document** — update seiring development, bukan freeze
