# Layer 2 — Requirement Validation

## Prinsip Utama

```
Validate Before Execute
```

Mayoritas AI project gagal karena:
- Requirement ambigu
- Requirement tidak lengkap
- Requirement konflik
- Architecture tidak dipikirkan sejak awal

## Yang Dicek AI

1. **Ambiguity** - Apakah ada requirement yang ambigu?
2. **Missing Requirements** - Apakah ada yang terlewat?
3. **Conflicting Requirements** - Apakah ada yang saling bertentangan?
4. **Scalability Risk** - Apakah bisa scale?
5. **Security Risk** - Apakah ada celah keamanan?
6. **Offline-First Risk** - Apakah perlu offline support?
7. **UX Risk** - Apakah UX-nya feasible?
8. **Operational Risk** - Apakah bisa di-operate?

## Output yang Dihasilkan

```
docs/requirements/validation/
├── ambiguity-report.md
├── missing-requirements.md
├── conflict-analysis.md
└── risk-analysis.md
```

## Template: ambiguity-report.md

```markdown
# Ambiguity Report

## Summary
- Total requirements reviewed: [N]
- Ambiguous requirements found: [N]
- Severity: [High/Medium/Low]

## Ambiguous Requirements

### AMB-001: [Requirement ID]
- **Original Text:** "[teks asli requirement]"
- **Ambiguity Type:** [Vague/Incomplete/Multiple Interpretations]
- **Why Ambiguous:** [Penjelasan]
- **Possible Interpretations:**
  1. [Interpretasi 1]
  2. [Interpretasi 2]
- **Recommended Clarification:** [Saran perbaikan]
- **Action Required:** [Tanya stakeholder / Revise / Accept risk]

### AMB-002: [Requirement ID]
...

## Recommendations
1. [Rekomendasi 1]
2. [Rekomendasi 2]
```

## Template: conflict-analysis.md

```markdown
# Conflict Analysis

## Summary
- Total conflicts found: [N]
- Critical conflicts: [N]
- Resolvable conflicts: [N]

## Conflicts

### CONF-001
- **Requirement A:** [ID] - "[teks]"
- **Requirement B:** [ID] - "[teks]"
- **Conflict Type:** [Logical/Resource/Timeline/Technical]
- **Description:** [Penjelasan konflik]
- **Impact:** [Dampak jika tidak diselesaikan]
- **Resolution Options:**
  1. [Option 1 - pro/con]
  2. [Option 2 - pro/con]
- **Recommended Resolution:** [Rekomendasi]

## Resolution Matrix
| Conflict | Priority | Owner | Status | Resolution |
|----------|----------|-------|--------|------------|
| CONF-001 | High | [Name] | Open | [Resolution] |
```

## Template: risk-analysis.md

```markdown
# Risk Analysis

## Risk Matrix

| Risk | Probability | Impact | Score | Mitigation |
|------|-------------|--------|-------|------------|
| [Risk 1] | High | High | Critical | [Mitigation] |
| [Risk 2] | Medium | High | High | [Mitigation] |

## Detailed Risks

### RISK-001: [Nama Risk]
- **Category:** [Security/Scalability/UX/Operational/Technical]
- **Probability:** [High/Medium/Low]
- **Impact:** [High/Medium/Low]
- **Description:** [Deskripsi lengkap]
- **Affected Requirements:** [IDs]
- **Mitigation Strategy:** [Strategi]
- **Contingency Plan:** [Plan B]
- **Owner:** [Siapa yang responsible]

## Security Risks
[Detail security risks]

## Scalability Risks
[Detail scalability risks]

## Operational Risks
[Detail operational risks]
```

## Workflow dengan Kiro

### Step 1: Load Requirements
```
"Baca semua requirements di docs/requirements/intake/extracted/ 
dan lakukan validasi lengkap"
```

### Step 2: Ambiguity Check
```
"Identifikasi semua requirement yang ambigu.
Untuk setiap ambiguity, berikan possible interpretations dan recommended clarification."
```

### Step 3: Completeness Check
```
"Analisis apakah ada requirement yang missing.
Pertimbangkan: error handling, edge cases, security, performance, accessibility."
```

### Step 4: Conflict Check
```
"Cari requirement yang saling bertentangan.
Berikan resolution options untuk setiap conflict."
```

### Step 5: Risk Assessment
```
"Lakukan risk assessment untuk semua requirements.
Kategorikan: security, scalability, UX, operational, technical."
```

## GitLab Integration

### Validation sebagai Pipeline Stage

Tambahkan validation check di `.gitlab-ci.yml`:

```yaml
validate-requirements:
  stage: validate
  script:
    - echo "Checking requirement documents..."
    - |
      # Check semua requirement punya acceptance criteria
      for file in docs/requirements/intake/extracted/*.md; do
        if ! grep -q "Acceptance Criteria" "$file"; then
          echo "WARNING: $file missing acceptance criteria"
        fi
      done
    - echo "Validation complete"
  rules:
    - changes:
        - docs/requirements/**/*
```

### Labels untuk Validation Status
```
validation::pending
validation::passed
validation::failed
validation::needs-clarification
```

### Merge Request Template
```markdown
## Requirement Validation Checklist

- [ ] Ambiguity report reviewed
- [ ] No critical ambiguities remaining
- [ ] Missing requirements addressed
- [ ] No unresolved conflicts
- [ ] Risk analysis completed
- [ ] All high risks have mitigation plans
- [ ] Stakeholder sign-off obtained
```

## Quality Gates

Requirement TIDAK BOLEH masuk ke Layer 3 (Spec) jika:
- Ada ambiguity dengan severity High yang belum resolved
- Ada conflict yang belum ada resolution
- Ada risk Critical tanpa mitigation plan
- Stakeholder belum sign-off

## Best Practices

1. **Validate early, validate often** - Jangan tunggu semua requirement selesai
2. **Involve stakeholders** - AI bisa detect, tapi manusia yang decide
3. **Document decisions** - Setiap resolution harus didokumentasikan
4. **Track validation status** - Gunakan GitLab labels
5. **Automate where possible** - Gunakan CI/CD untuk basic checks

---

## Workflow Integration — Automated Validation Gates

### Prinsip

Requirement Validation BUKAN step opsional. Validation adalah **gate wajib** antara intake dan development. Tanpa validation, AI akan bekerja berdasarkan requirement yang ambigu, incomplete, atau conflicting.

---

### 1. Automated Validation Trigger

AI Agent WAJIB menjalankan validation **OTOMATIS** setelah requirement intake selesai (Layer 1 output tersedia).

```
[Layer 1: Requirement Intake selesai]
    ↓
[TRIGGER: Auto-validate requirements]
    ↓
[AI Agent jalankan 4 validation checks:]
    ├── Ambiguity Check — apakah ada requirement yang bisa diinterpretasi ganda?
    ├── Completeness Check — apakah ada informasi yang hilang?
    ├── Conflict Check — apakah ada requirement yang bertentangan?
    └── Feasibility Check — apakah requirement bisa diimplementasi dengan tech stack yang dipilih?
    ↓
[Generate Validation Report]
    ↓
[Jika ada issue → BLOCK lanjut ke Layer 3, minta klarifikasi user]
[Jika clean → lanjut ke Layer 3 (Spec-Driven Development)]
```

---

### 2. Kiro Hook: Requirement Validation Gate

```json
{
  "name": "Requirement Validation Gate",
  "version": "1.0.0",
  "description": "Auto-validate requirements sebelum lanjut ke spec/development",
  "when": {
    "type": "fileCreated",
    "patterns": ["docs/requirements/**/*.md", "docs/specs/prd/**/*.md"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "File requirement/PRD baru dibuat. Jalankan VALIDATION OTOMATIS:\n\n**1. Ambiguity Check:**\n- Scan setiap user story/requirement\n- Tandai yang menggunakan kata ambigu: 'cepat', 'mudah', 'baik', 'sesuai', 'dll'\n- Tandai yang tidak punya acceptance criteria spesifik\n- Tandai yang tidak punya definisi measurable\n\n**2. Completeness Check:**\n- Apakah setiap user story punya: Actor, Action, Benefit?\n- Apakah setiap requirement punya acceptance criteria?\n- Apakah ada dependency yang belum didefinisikan?\n- Apakah error/edge case sudah dipertimbangkan?\n\n**3. Conflict Check:**\n- Apakah ada requirement yang bertentangan satu sama lain?\n- Apakah ada requirement yang bertentangan dengan constraint teknis?\n- Apakah ada requirement yang bertentangan dengan governance/security policy?\n\n**4. Feasibility Check:**\n- Apakah requirement bisa diimplementasi dengan tech stack yang dipilih?\n- Apakah ada requirement yang membutuhkan third-party service yang belum tersedia?\n- Apakah timeline realistis untuk scope yang diminta?\n\n**Output:**\nGenerate file `docs/requirements/validation/validation-report-[date].md` dengan:\n- Summary: X requirements validated, Y issues found\n- Detail per issue: requirement ID, issue type, severity, suggestion\n- Recommendation: proceed / needs clarification\n\n**Rules:**\n- Jika ada issue CRITICAL → BLOCK, minta klarifikasi user sebelum lanjut\n- Jika ada issue WARNING → informasikan user, boleh lanjut dengan catatan\n- Jika clean → informasikan user, lanjut ke Layer 3"
  }
}
```

---

### 3. Validation Report Template

```markdown
# Requirement Validation Report

**Date:** [YYYY-MM-DD]
**Source:** [file yang divalidasi]
**Status:** ✅ PASSED / ⚠️ PASSED WITH WARNINGS / ❌ BLOCKED

## Summary
- Total requirements: [N]
- Valid: [N]
- Warnings: [N]
- Critical issues: [N]

## Issues Found

### Critical (MUST fix before proceeding)
| # | Requirement | Issue Type | Description | Suggestion |
|---|-------------|-----------|-------------|------------|
| 1 | US-003 | Ambiguity | "sistem harus cepat" — tidak ada definisi measurable | Definisikan: response time < 2s |

### Warnings (Can proceed, but should address)
| # | Requirement | Issue Type | Description | Suggestion |
|---|-------------|-----------|-------------|------------|
| 1 | US-007 | Completeness | Tidak ada error scenario | Tambahkan: apa yang terjadi jika payment gagal? |

## Recommendation
[Proceed to Layer 3 / Needs clarification from stakeholder]
```

---

### 4. Integration dengan Layer Lain

| Layer | Bagaimana Layer 2 Terintegrasi |
|-------|-------------------------------|
| Layer 1 (Intake) | Output Layer 1 → input validation otomatis |
| Layer 3 (Spec-Driven) | Validation HARUS pass sebelum spec dibuat |
| Layer 5 (Governance) | Validation cek compliance terhadap governance rules |
| Layer 8 (Issue-Driven) | Issues tidak boleh dibuat dari requirement yang belum validated |

---

### 5. AI Agent Rules

1. **JANGAN mulai spec/coding tanpa validation** — requirement yang belum validated = risiko rework
2. **JANGAN skip validation karena "requirement sudah jelas"** — selalu jalankan minimal completeness check
3. **SELALU generate validation report** — sebagai audit trail
4. **SELALU informasikan user** tentang hasil validation sebelum lanjut
5. **Jika user push untuk skip** → informasikan risiko, catat sebagai accepted risk di docs/governance/
