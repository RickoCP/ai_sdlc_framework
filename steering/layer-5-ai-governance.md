# Layer 5 — AI Governance Framework

## Tujuan

Mencegah:
- Hallucination
- Inconsistent architecture
- Security vulnerability
- AI chaos
- Technical debt

## Output yang Dihasilkan

```
docs/governance/
├── ai-policy.md
├── approved-tools.md
├── prompt-standard.md
├── security-policy.md
└── code-review-policy.md
```

## Governance Areas

1. **Security** - Apa yang boleh dan tidak boleh dilakukan AI terkait security
2. **Architecture** - Rules arsitektur yang harus diikuti AI
3. **Coding Standards** - Standards coding yang wajib
4. **AI Policy** - Kebijakan penggunaan AI
5. **Review Policy** - Bagaimana AI output di-review
6. **Compliance** - Regulatory compliance
7. **Audit Trail** - Tracking AI decisions

---

## Template: ai-policy.md

```markdown
# AI Policy

## Approved AI Tools
| Tool | Purpose | Scope | Restrictions |
|------|---------|-------|-------------|
| Kiro | Development | All code | Must follow specs |
| [Tool] | [Purpose] | [Scope] | [Restrictions] |

## AI Usage Rules

### WAJIB (Must Do)
1. AI WAJIB bekerja berdasarkan specification yang sudah divalidasi
2. AI WAJIB mengikuti architecture yang sudah didefinisikan
3. AI WAJIB melalui code review sebelum merge
4. AI WAJIB menulis tests untuk setiap code yang di-generate
5. AI WAJIB mengikuti coding standards dan naming conventions
6. AI WAJIB mendokumentasikan setiap keputusan arsitektur

### DILARANG (Must Not)
1. AI TIDAK BOLEH mengubah architecture tanpa approval
2. AI TIDAK BOLEH mengakses production data
3. AI TIDAK BOLEH menyimpan credentials di code
4. AI TIDAK BOLEH skip security checks
5. AI TIDAK BOLEH generate code tanpa specification
6. AI TIDAK BOLEH mengubah shared libraries tanpa review

### SEBAIKNYA (Should)
1. AI sebaiknya menggunakan existing patterns yang sudah ada
2. AI sebaiknya memberikan penjelasan untuk keputusan yang diambil
3. AI sebaiknya mengidentifikasi risiko sebelum implementasi
4. AI sebaiknya menyarankan improvement pada existing code

## Escalation Policy
| Situation | Action | Escalate To |
|-----------|--------|-------------|
| Architecture change | Stop, request review | Tech Lead |
| Security concern | Flag immediately | Security Team |
| Unclear requirement | Ask for clarification | Product Owner |
| Performance impact | Document and flag | Tech Lead |
```

## Template: prompt-standard.md

```markdown
# Prompt Engineering Standards

## Context Loading
Setiap prompt ke AI HARUS include:
1. Relevant specification reference
2. Architecture constraints
3. Coding standards reference
4. Existing patterns to follow

## Prompt Structure
```
[CONTEXT]
- Spec: #[[file:docs/specs/...]]
- Architecture: #[[file:docs/design/...]]
- Standards: #[[file:docs/governance/coding-standards.md]]

[TASK]
[Deskripsi task yang jelas dan spesifik]

[CONSTRAINTS]
- Must follow [pattern]
- Must not [restriction]
- Must include [requirement]

[OUTPUT]
- Format: [expected format]
- Location: [where to save]
- Tests: [test requirements]
```

## Anti-Patterns
- ❌ "Buatkan fitur login" (terlalu vague)
- ❌ "Fix this bug" (tanpa context)
- ❌ "Refactor everything" (scope terlalu besar)

## Good Patterns
- ✅ "Berdasarkan spec US-001, implementasikan login endpoint mengikuti pattern di src/presentation/controllers/"
- ✅ "Fix bug #123: timeout pada payment API. Lihat error log di [link]. Gunakan retry pattern yang sudah ada."
- ✅ "Refactor UserService.createUser() untuk menggunakan Result pattern sesuai docs/design/technical/error-handling.md"
```

## Template: code-review-policy.md

```markdown
# Code Review Policy

## AI-Generated Code Review Checklist

### Architecture Compliance
- [ ] Mengikuti Clean Architecture layers
- [ ] Dependency rule tidak dilanggar
- [ ] Tidak ada circular dependencies
- [ ] Menggunakan dependency injection

### Security
- [ ] Input validation ada
- [ ] No hardcoded credentials
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] Proper error handling (no info leak)

### Code Quality
- [ ] Naming conventions diikuti
- [ ] No duplicated logic
- [ ] Single Responsibility Principle
- [ ] Proper error handling
- [ ] Logging yang adequate

### Testing
- [ ] Unit tests ada
- [ ] Edge cases di-cover
- [ ] Error scenarios di-test
- [ ] Coverage >= threshold

### Performance
- [ ] No N+1 queries
- [ ] Proper indexing considered
- [ ] No memory leaks
- [ ] Async operations handled correctly

### Documentation
- [ ] Public APIs documented
- [ ] Complex logic explained
- [ ] ADR dibuat jika ada keputusan arsitektur baru
```

## Kiro Steering Integration

Buat `.kiro/steering/ai-guidelines.md`:

```markdown
---
inclusion: always
---

# AI Development Guidelines

## Architecture Rules
- Ikuti Clean Architecture di docs/design/technical/clean-architecture.md
- Semua business logic di application layer (use cases)
- Infrastructure details TIDAK BOLEH leak ke domain layer

## Coding Standards
- TypeScript strict mode
- No any type
- All functions must have return type
- Error handling dengan Result pattern

## Before Coding
1. Baca specification terkait
2. Baca existing patterns di codebase
3. Identifikasi dependencies

## After Coding
1. Tulis unit tests
2. Run linter
3. Check type errors
4. Verify against acceptance criteria
```

## GitLab Integration

### Governance sebagai CI/CD Check

```yaml
governance-check:
  stage: validate
  script:
    - echo "Running governance checks..."
    # Check no credentials in code
    - |
      if grep -rn "password\|secret\|api_key\|token" src/ --include="*.ts" | grep -v "test" | grep -v ".d.ts"; then
        echo "ERROR: Possible credentials in code!"
        exit 1
      fi
    # Check architecture compliance
    - |
      # Domain layer should not import from infrastructure
      if grep -rn "from.*infrastructure" src/domain/; then
        echo "ERROR: Domain layer importing from infrastructure!"
        exit 1
      fi
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### MR Template dengan Governance

```markdown
## Governance Checklist

- [ ] Code follows AI policy
- [ ] Architecture rules respected
- [ ] Security policy followed
- [ ] Coding standards met
- [ ] Review policy satisfied
- [ ] No governance violations in CI/CD
```

## Best Practices

1. **Governance bukan blocker** - Tujuannya guide, bukan block
2. **Automate checks** - Sebanyak mungkin governance di-automate via CI/CD
3. **Living document** - Update governance seiring project evolve
4. **Team buy-in** - Governance harus disepakati tim
5. **Proportional** - Governance sesuai risk level project


---

## Appendix: Configuration & Convention Standards

### Path Aliases (WAJIB)

| Alias | Target |
|-------|--------|
| `@core/*` | `src/core/*` |
| `@infrastructure/*` | `src/infrastructure/*` |
| `@features/*` | `src/presentation/features/*` |
| `@components/*` | `src/presentation/components/*` |
| `@presentation/*` | `src/presentation/*` |
| `@di/*` | `src/infrastructure/di/*` |
| `@app/*` | `src/app/*` |

DILARANG menggunakan relative path panjang (`../../..`).

### TypeScript Rules (STRICT)

- `strict: true`
- `noImplicitAny: true`
- `strictNullChecks: true`
- `noUncheckedIndexedAccess: true`
- DILARANG: `any`, `@ts-ignore`

### Naming Convention

| Item | Format | Contoh |
|------|--------|--------|
| Component | PascalCase | `PaymentCard.tsx` |
| Hook/ViewModel | camelCase | `usePaymentViewModel.ts` |
| UseCase | PascalCase + UseCase | `GetPaymentUseCase.ts` |
| Repository | PascalCase | `PaymentRepository.ts` |
| Impl | PascalCase + Impl | `PaymentRepositoryImpl.ts` |
| Entity | PascalCase | `Payment.ts` |
| Mapper | PascalCase + Mapper | `PaymentMapper.ts` |
| DI Key | camelCase | `paymentUseCase` |

### Commit Convention (WAJIB)

- `feat:` fitur baru
- `fix:` bug fix
- `refactor:` refactor
- `docs:` dokumentasi
- `test:` testing
- `chore:` maintenance

### Environment Variables

- Client env WAJIB prefix `NEXT_PUBLIC_`
- Tidak boleh expose secret ke client
- DILARANG hardcode URL

### Internationalization

- Semua text HARUS pakai `t()` (i18n)
- Tidak boleh hardcode text di UI