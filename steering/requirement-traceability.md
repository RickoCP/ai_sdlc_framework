# Requirement Traceability & Implementation Flow

## Overview

Dokumen ini mendefinisikan **end-to-end flow** dari PRD User Story hingga Merge, memastikan setiap implementasi benar-benar sesuai dengan requirement yang didefinisikan.

**Flow Final:**

```
PRD User Story
    ↓
Acceptance Criteria
    ↓
Technical Tasks
    ↓
GitLab Issues
    ↓
AI Implementation
    ↓
AI Validation
    ↓
Unit Test
    ↓
Coverage Check
    ↓
Requirement Traceability
    ↓
Staging Validation
    ↓
Human Approval
    ↓
Merge
```

---

## 1. User Story di PRD

Setiap fitur HARUS dimulai dari User Story yang jelas.

### Format

```markdown
### STORY-[ID]: [Judul]

**As a** [role]
**I want to** [action]
**So that** [benefit]
```

### Contoh

```markdown
### STORY-001: Top Up Saldo NFC

**As a** admin koperasi
**I want to** melakukan top up saldo kartu NFC
**So that** anggota dapat menggunakan fasilitas koperasi
```

### Rules

- Setiap story HARUS punya ID unik (STORY-XXX)
- Story harus dari perspektif user/role yang jelas
- Benefit harus terukur dan spesifik
- Story disimpan di `docs/specs/prd/user-stories.md`

---

## 2. Acceptance Criteria (WAJIB)

Acceptance Criteria adalah **kontrak implementasi**. Ini menjadi:
- Kontrak implementasi untuk developer/AI
- Validation rule untuk AI reviewer
- Source untuk test generation
- Basis acceptance testing

### Format

```markdown
#### Acceptance Criteria untuk STORY-[ID]:

- [ ] AC-001: [Criteria 1]
- [ ] AC-002: [Criteria 2]
- [ ] AC-003: [Criteria 3]
```

### Contoh

```markdown
#### Acceptance Criteria untuk STORY-001:

- [ ] AC-001: Admin dapat scan kartu NFC
- [ ] AC-002: Admin dapat input nominal top up
- [ ] AC-003: Saldo kartu bertambah sesuai nominal
- [ ] AC-004: Maksimal transaction logs hanya 5
- [ ] AC-005: Data saldo terenkripsi
- [ ] AC-006: Jika write gagal, tampil error message
- [ ] AC-007: Top up tetap bisa dilakukan offline
```

### Rules

- Setiap story WAJIB punya acceptance criteria
- Criteria harus testable (bisa dibuktikan pass/fail)
- Criteria harus spesifik (bukan "berjalan dengan baik")
- Setiap criteria punya ID unik (AC-XXX)
- Criteria mencakup: happy path, error handling, edge cases, security, performance

---

## 3. Generate Technical Tasks

AI generate tasks berdasarkan acceptance criteria. Setiap AC harus ter-cover oleh minimal 1 task.

### Format

```markdown
## FEATURE: [Nama Feature]

### Tasks:
- [ ] TASK-001: [Task description] (covers: AC-001, AC-002)
- [ ] TASK-002: [Task description] (covers: AC-003)
- [ ] TASK-003: [Task description] (covers: AC-004)
- [ ] TASK-004: [Task description] (covers: AC-005)
- [ ] TASK-005: [Task description] (covers: AC-006, AC-007)
- [ ] TASK-006: Write unit tests (covers: ALL)
```

### Contoh

```markdown
## FEATURE: Top Up NFC Balance

### Tasks:
- [ ] TASK-001: Implement NFC card reader integration (covers: AC-001)
- [ ] TASK-002: Implement top up input form with validation (covers: AC-002)
- [ ] TASK-003: Implement encrypted balance storage (covers: AC-003, AC-005)
- [ ] TASK-004: Implement transaction log rotation (max 5) (covers: AC-004)
- [ ] TASK-005: Implement offline write with error handling (covers: AC-006, AC-007)
- [ ] TASK-006: Write unit tests for all acceptance criteria (covers: ALL)
```

### Rules

- Setiap task HARUS menyebutkan AC mana yang di-cover
- Tidak boleh ada AC yang tidak ter-cover oleh task
- Task harus cukup kecil (2-4 jam kerja)
- Task terakhir selalu: write tests

---

## 4. GitLab Issue Mapping

Setiap issue di GitLab HARUS menyimpan traceability information.

### Issue Template

```markdown
## Issue: [Title]

### Related PRD
- Story: STORY-[ID]
- PRD Location: docs/specs/prd/user-stories.md#STORY-[ID]

### Acceptance Criteria
- [ ] AC-001: [Criteria]
- [ ] AC-002: [Criteria]
- [ ] AC-003: [Criteria]

### Test Scenarios
- [ ] TEST-001: [Test description] (validates: AC-001)
- [ ] TEST-002: [Test description] (validates: AC-002)
- [ ] TEST-003: [Test description] (validates: AC-003)

### Technical Notes
[Implementation hints, architecture references]

### Definition of Done
- [ ] All acceptance criteria implemented
- [ ] All test scenarios passing
- [ ] AI validation passed
- [ ] Code review approved
- [ ] No governance violations

/label ~"type::task" ~"status::backlog"
```

### Contoh

```markdown
## Issue #22: Implement NFC Top Up

### Related PRD
- Story: STORY-001
- PRD Location: docs/specs/prd/user-stories.md#STORY-001

### Acceptance Criteria
- [ ] AC-001: Admin dapat scan kartu NFC
- [ ] AC-002: Admin dapat input nominal top up
- [ ] AC-003: Saldo kartu bertambah sesuai nominal
- [ ] AC-004: Maksimal transaction logs hanya 5
- [ ] AC-005: Data saldo terenkripsi
- [ ] AC-006: Jika write gagal, tampil error
- [ ] AC-007: Top up tetap bisa offline

### Test Scenarios
- [ ] TEST-001: should read NFC card successfully (validates: AC-001)
- [ ] TEST-002: should accept valid top up amount (validates: AC-002)
- [ ] TEST-003: should increase balance after successful top up (validates: AC-003)
- [ ] TEST-004: should keep only latest 5 transaction logs (validates: AC-004)
- [ ] TEST-005: should encrypt balance data (validates: AC-005)
- [ ] TEST-006: should show error when write fails (validates: AC-006)
- [ ] TEST-007: should complete top up in offline mode (validates: AC-007)

/label ~"type::task" ~"domain::payment" ~"priority::high"
```

### Rules

- Setiap issue WAJIB link ke PRD Story
- Setiap issue WAJIB list acceptance criteria
- Setiap issue WAJIB list test scenarios
- 1 AC = minimal 1 test scenario

---

## 5. AI Implementation Berdasarkan Issue

### JANGAN

```
"Buat feature top up"
```

### LAKUKAN

```
"Implement Issue #22: NFC Top Up.
Pastikan seluruh acceptance criteria terpenuhi:
- AC-001: Admin dapat scan kartu NFC
- AC-002: Admin dapat input nominal top up
- AC-003: Saldo kartu bertambah sesuai nominal
- AC-004: Maksimal transaction logs hanya 5
- AC-005: Data saldo terenkripsi
- AC-006: Jika write gagal, tampil error
- AC-007: Top up tetap bisa offline

Reference:
- PRD: docs/specs/prd/user-stories.md#STORY-001
- Architecture: docs/design/system/high-level-architecture.md
- Skills: .kiro/skills/create-usecase.md"
```

### Rules

- AI HARUS menerima acceptance criteria sebagai input
- AI HARUS mengimplementasikan SEMUA criteria
- AI HARUS mereferensikan spec dan architecture
- AI TIDAK BOLEH menambah scope di luar acceptance criteria
- AI HARUS mengikuti architecture dan coding standards

---

## 6. AI Validation Layer (WAJIB)

Setelah coding selesai, AI HARUS melakukan self-validation.

### Validation Prompt

```
Review implementasi ini.
Cek:
1. Apakah SEMUA acceptance criteria terpenuhi?
2. Apakah ada requirement PRD yang belum diimplementasikan?
3. Apakah ada edge case yang belum ditangani?
4. Apakah implementation sesuai architecture?
5. Apakah test coverage cukup?
6. Apakah ada business logic yang salah?

Acceptance Criteria:
[list semua AC]

PRD Reference:
[link ke PRD story]

Architecture Reference:
[link ke architecture doc]
```

### Validation Output Format

```markdown
## AI Validation Report

### Acceptance Criteria Coverage
| AC | Status | Evidence |
|----|--------|----------|
| AC-001 | Implemented | `nfcReader.scan()` in TopUpUseCase.ts:15 |
| AC-002 | Implemented | `validateAmount()` in TopUpForm.tsx:23 |
| AC-003 | Implemented | `updateBalance()` in BalanceRepository.ts:45 |
| AC-004 | Implemented | `rotateLog(5)` in TransactionLog.ts:12 |
| AC-005 | Implemented | `encrypt()` in BalanceStorage.ts:8 |
| AC-006 | Implemented | `catch` block in TopUpUseCase.ts:32 |
| AC-007 | Partial | Offline queue exists but no sync mechanism |

### Issues Found
- AC-007: Offline sync mechanism not implemented yet

### Recommendations
- Add offline sync worker for AC-007
- Add retry logic for failed syncs

### Architecture Compliance
- Clean Architecture layers respected
- DI pattern followed
- Error handling pattern followed
```

### Rules

- Validation WAJIB dilakukan sebelum commit
- Setiap AC harus punya status (Implemented/Partial/Missing)
- Evidence harus menunjuk ke file dan line number
- Issues harus di-resolve sebelum merge

---

## 7. Generate Test dari Acceptance Criteria

### Rule Utama

```
1 Acceptance Criteria = Minimal 1 Test
```

### Mapping AC ke Test

| Acceptance Criteria | Test |
|---|---|
| Saldo bertambah sesuai nominal | `should increase balance after successful top up` |
| Maksimal logs hanya 5 | `should keep only latest 5 transaction logs` |
| Data saldo terenkripsi | `should encrypt balance data before storage` |
| Jika write gagal tampil error | `should show error message when write fails` |
| Top up tetap bisa offline | `should complete top up without network connection` |

### Test Template

```typescript
describe('TopUpUseCase', () => {
  // AC-001: Admin dapat scan kartu NFC
  it('should read NFC card successfully', async () => {
    // Arrange
    const mockNfcReader = createMockNfcReader();
    // Act
    const result = await useCase.scanCard();
    // Assert
    expect(result.isSuccess).toBe(true);
    expect(result.value.cardId).toBeDefined();
  });

  // AC-003: Saldo kartu bertambah sesuai nominal
  it('should increase balance after successful top up', async () => {
    // Arrange
    const initialBalance = 100000;
    const topUpAmount = 50000;
    // Act
    const result = await useCase.execute({ cardId, amount: topUpAmount });
    // Assert
    expect(result.value.newBalance).toBe(initialBalance + topUpAmount);
  });

  // AC-004: Maksimal transaction logs hanya 5
  it('should keep only latest 5 transaction logs', async () => {
    // Arrange - create 6 transactions
    for (let i = 0; i < 6; i++) {
      await useCase.execute({ cardId, amount: 10000 });
    }
    // Act
    const logs = await logRepository.getByCardId(cardId);
    // Assert
    expect(logs.length).toBe(5);
  });

  // AC-005: Data saldo terenkripsi
  it('should encrypt balance data before storage', async () => {
    // Arrange & Act
    await useCase.execute({ cardId, amount: 50000 });
    // Assert
    const rawData = await storage.getRaw(cardId);
    expect(rawData).not.toContain('50000');
    expect(encryptSpy).toHaveBeenCalled();
  });

  // AC-006: Jika write gagal, tampil error
  it('should return error when write fails', async () => {
    // Arrange
    mockStorage.write.mockRejectedValue(new Error('Write failed'));
    // Act
    const result = await useCase.execute({ cardId, amount: 50000 });
    // Assert
    expect(result.isFailure).toBe(true);
    expect(result.error.code).toBe('NFC_WRITE_FAILED');
  });

  // AC-007: Top up tetap bisa offline
  it('should complete top up without network connection', async () => {
    // Arrange
    mockNetwork.isOnline.mockReturnValue(false);
    // Act
    const result = await useCase.execute({ cardId, amount: 50000 });
    // Assert
    expect(result.isSuccess).toBe(true);
    expect(offlineQueue.items.length).toBe(1);
  });
});
```

### Rules

- Setiap AC WAJIB punya minimal 1 test
- Test name harus deskriptif dan mereferensikan AC
- Test harus cover happy path DAN error path
- Comment di test harus menyebutkan AC-ID yang di-validate

---

## 8. Traceability Matrix

### Format

| PRD Story | Acceptance Criteria | GitLab Issue | Code File | Test File | Status |
|-----------|-------------------|--------------|-----------|-----------|--------|
| STORY-001 | AC-001 | #22 | nfcReader.ts | nfcReader.test.ts | Done |
| STORY-001 | AC-002 | #22 | topUpForm.tsx | topUpForm.test.tsx | Done |
| STORY-001 | AC-003 | #22 | topUpUseCase.ts | topUpUseCase.test.ts | Done |
| STORY-001 | AC-004 | #22 | transactionLog.ts | transactionLog.test.ts | Done |
| STORY-001 | AC-005 | #22 | balanceStorage.ts | balanceStorage.test.ts | Done |
| STORY-001 | AC-006 | #22 | topUpUseCase.ts | topUpUseCase.test.ts | Done |
| STORY-001 | AC-007 | #22 | offlineQueue.ts | offlineQueue.test.ts | Partial |

### Lokasi

```
docs/traceability/
├── traceability-matrix.md
└── coverage-report.md
```

### Kenapa Penting

Traceability matrix memungkinkan Anda mengetahui:
- Requirement mana yang **belum dikerjakan**
- Requirement mana yang **belum dites**
- Requirement mana yang **belum tervalidasi**
- Code mana yang **tidak punya requirement** (scope creep)

---

## 9. AI Requirement Coverage Check

AI bisa otomatis mengecek requirement coverage.

### Coverage Report Format

```markdown
# Requirement Coverage Report

## Summary
- PRD Stories: 24
- Acceptance Criteria: 87
- Implemented: 72 (83%)
- Tested: 65 (75%)
- Missing: 15 (17%)
- Partially Implemented: 5 (6%)

## Missing Requirements
| Story | AC | Description | Priority |
|-------|-----|-------------|----------|
| STORY-005 | AC-012 | Offline sync | High |
| STORY-008 | AC-023 | Rate limiting | Medium |
| STORY-008 | AC-024 | Retry logic | Medium |

## Untested Requirements
| Story | AC | Description | Code File |
|-------|-----|-------------|-----------|
| STORY-003 | AC-008 | Error boundary | errorBoundary.tsx |
| STORY-006 | AC-018 | Cache invalidation | cacheService.ts |

## Recommendations
1. Prioritaskan implementasi AC-012 (offline sync) - High priority
2. Tambahkan test untuk AC-008 dan AC-018
3. Review STORY-008 untuk completeness
```

### Automation

AI Agent HARUS menjalankan coverage check:
- Sebelum setiap MR/merge
- Setelah sprint selesai
- Saat diminta user

---

## 10. Staging Validation

Sebelum merge ke main, AI HARUS melakukan staging validation:

```
Review staging deployment:
1. PRD VS Implementation - Apakah semua story ter-implement?
2. Implementation VS Test - Apakah semua code ter-test?
3. Test VS Acceptance Criteria - Apakah semua AC ter-validate?
4. UI VS Design - Apakah UI sesuai design?
5. Security VS Threat Model - Apakah security terpenuhi?
```

### Staging Validation Checklist

```markdown
## Staging Validation: [Feature Name]

### PRD Compliance
- [ ] Semua user stories ter-implement
- [ ] Semua acceptance criteria terpenuhi
- [ ] Tidak ada scope creep (fitur di luar PRD)

### Test Compliance
- [ ] Semua AC punya test
- [ ] Semua test passing
- [ ] Coverage >= 80%
- [ ] No flaky tests

### Architecture Compliance
- [ ] Clean Architecture respected
- [ ] No dependency violations
- [ ] DI pattern followed
- [ ] Error handling consistent

### Security Compliance
- [ ] No hardcoded credentials
- [ ] Input validation present
- [ ] Output sanitized
- [ ] Auth/authz checked

### Performance
- [ ] No N+1 queries
- [ ] Proper caching
- [ ] Bundle size acceptable
```

---

## 11. Human Validation (WAJIB)

Walaupun AI powerful, **human approval tetap wajib**.

Karena AI bisa:
- Technically benar, tetapi **business-wise salah**
- Memenuhi semua AC, tetapi **UX buruk**
- Pass semua test, tetapi **edge case terlewat**

### Human Review Focus

1. **Business logic correctness** - Apakah flow bisnis benar?
2. **UX review** - Apakah user experience baik?
3. **Edge cases** - Apakah ada skenario yang terlewat?
4. **Security** - Apakah ada celah yang tidak terdeteksi AI?
5. **Performance** - Apakah acceptable di real-world usage?

### Approval Flow

```
AI Validation Pass
    ↓
MR Created
    ↓
Human Reviewer assigned
    ↓
Review (business + technical)
    ↓
Approve / Request Changes
    ↓
Merge (if approved)
```

---

## Workflow dengan Kiro

### Membuat User Story + AC

```
"Berdasarkan requirement berikut, buatkan User Story dengan Acceptance Criteria:
[paste requirement]

Format:
- User Story format: As a / I want to / So that
- Acceptance Criteria: testable, specific, complete
- Simpan di docs/specs/prd/user-stories.md"
```

### Generate Tasks dari AC

```
"Berdasarkan Acceptance Criteria untuk STORY-001,
generate technical tasks.
Setiap task harus menyebutkan AC mana yang di-cover.
Buat GitLab Issues untuk setiap task."
```

### Implement berdasarkan Issue

```
"Implement Issue #22: NFC Top Up.
Reference: docs/specs/prd/user-stories.md#STORY-001
Pastikan SEMUA acceptance criteria terpenuhi.
Gunakan skills yang sesuai."
```

### Validate setelah Implementation

```
"Validate implementasi terhadap acceptance criteria STORY-001.
Buat validation report.
Identifikasi AC yang belum terpenuhi."
```

### Generate Tests

```
"Generate unit tests berdasarkan acceptance criteria STORY-001.
Rule: 1 AC = minimal 1 test.
Setiap test harus menyebutkan AC-ID di comment."
```

### Coverage Check

```
"Jalankan requirement coverage check.
Bandingkan: PRD stories VS implemented VS tested.
Buat coverage report."
```

---

## GitLab CI/CD Integration

### Traceability Check di Pipeline

```yaml
traceability-check:
  stage: validate
  script:
    - echo "Checking requirement traceability..."
    - |
      # Check setiap AC punya test
      AC_COUNT=$(grep -c "AC-" docs/specs/prd/user-stories.md || echo 0)
      TEST_COUNT=$(grep -rc "AC-" tests/ || echo 0)
      echo "Acceptance Criteria: $AC_COUNT"
      echo "Tests referencing AC: $TEST_COUNT"
      if [ "$TEST_COUNT" -lt "$AC_COUNT" ]; then
        echo "WARNING: Not all acceptance criteria have tests!"
      fi
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

---

## Best Practices

1. **AC first, code later** - Jangan coding tanpa acceptance criteria
2. **1 AC = 1 Test minimum** - Tidak ada AC tanpa test
3. **Trace everything** - Setiap code harus bisa di-trace ke requirement
4. **AI validates, human approves** - AI cek completeness, human cek correctness
5. **Living documents** - Update traceability matrix seiring development
6. **Small stories** - Story yang terlalu besar harus dipecah
7. **Specific criteria** - "Berjalan dengan baik" bukan acceptance criteria
