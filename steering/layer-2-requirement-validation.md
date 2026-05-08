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
