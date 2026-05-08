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
├── architecture.md
├── sequence-diagram.md
├── state-flow.md
├── failure-scenario.md
├── api-contract.md
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
Include: architecture, sequence diagram, state flow, failure scenarios, API contracts.
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
3. **Design** - Referensikan docs/specs/srs/architecture.md
4. **Tasks** - Break down menjadi implementable tasks

Contoh Kiro Spec:
```markdown
# Feature: User Authentication

## Requirements
#[[file:docs/specs/prd/user-stories.md]]

## Design
#[[file:docs/specs/srs/architecture.md]]
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