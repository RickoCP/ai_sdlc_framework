# Layer 11 — AI Review & Validation

## Konsep

AI tidak hanya generate code. AI juga melakukan review.

## Yang Dicek AI

1. **Architecture violation** - Apakah dependency rule dilanggar?
2. **Security issue** - Apakah ada vulnerability?
3. **Duplicated logic** - Apakah ada code yang duplikat?
4. **Performance issue** - Apakah ada N+1, memory leak?
5. **Accessibility issue** - Apakah accessible?
6. **Anti-pattern** - Apakah ada anti-pattern?
7. **Clean architecture violation** - Apakah layer boundaries respected?

## Review Checklist

### Architecture Review
```markdown
- [ ] Dependency rule: inner layers tidak import outer layers
- [ ] Domain layer pure (no framework imports)
- [ ] Use cases hanya di application layer
- [ ] Infrastructure details tidak leak ke domain
- [ ] Proper use of dependency injection
- [ ] No circular dependencies
```

### Security Review
```markdown
- [ ] Input validation pada semua endpoints
- [ ] No hardcoded credentials
- [ ] Parameterized queries (no SQL injection)
- [ ] Output encoding (no XSS)
- [ ] Proper authentication checks
- [ ] Authorization on all protected resources
- [ ] Sensitive data not logged
- [ ] CORS properly configured
- [ ] Rate limiting implemented
- [ ] Error messages don't expose internals
```

### Code Quality Review
```markdown
- [ ] Single Responsibility Principle
- [ ] No god classes/functions
- [ ] Proper error handling
- [ ] No magic numbers/strings
- [ ] Meaningful variable names
- [ ] No dead code
- [ ] No commented-out code
- [ ] Proper logging
- [ ] No console.log in production code
```

### Performance Review
```markdown
- [ ] No N+1 queries
- [ ] Proper database indexing considered
- [ ] No unnecessary re-renders (frontend)
- [ ] Lazy loading where appropriate
- [ ] No memory leaks
- [ ] Async operations properly handled
- [ ] Caching strategy applied where needed
- [ ] No blocking operations in event loop
```

### Testing Review
```markdown
- [ ] Unit tests for business logic
- [ ] Integration tests for API endpoints
- [ ] Edge cases covered
- [ ] Error scenarios tested
- [ ] Mocks used appropriately
- [ ] Test naming is descriptive
- [ ] No test interdependencies
- [ ] Coverage meets threshold
```

## Implementasi dengan Kiro

### Pre-Commit Review Hook

```json
{
  "name": "AI Code Review",
  "version": "1.0.0",
  "when": {
    "type": "preToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Before writing this code, verify: 1) Architecture compliance (Clean Architecture layers), 2) Security (no vulnerabilities), 3) Code quality (naming, SRP, error handling). If any issues found, fix them before writing."
  }
}
```

### Post-Implementation Review

```
"Review code yang baru saja di-generate.
Check:
1. Architecture compliance - apakah sesuai Clean Architecture?
2. Security - apakah ada vulnerability?
3. Performance - apakah ada potential issues?
4. Testing - apakah tests adequate?

Reference:
- Architecture: docs/design/technical/clean-architecture.md
- Security: docs/design/security/threat-model.md
- Standards: docs/governance/code-review-policy.md"
```

### Self-Review Pattern

Sebelum membuat MR, minta AI melakukan self-review:

```
"Lakukan self-review pada semua perubahan di branch ini.
Bandingkan dengan:
1. Specification: [link ke spec]
2. Architecture: [link ke architecture]
3. Governance: docs/governance/code-review-policy.md

Report:
- Compliance issues
- Potential bugs
- Missing tests
- Improvement suggestions"
```

## GitLab Integration

### Automated Review di CI/CD

```yaml
ai-review:
  stage: review
  script:
    # Architecture compliance
    - echo "Checking architecture compliance..."
    - |
      # Domain should not import from infrastructure
      VIOLATIONS=$(grep -rn "from.*infrastructure\|from.*presentation" src/domain/ 2>/dev/null || true)
      if [ -n "$VIOLATIONS" ]; then
        echo "ARCHITECTURE VIOLATION:"
        echo "$VIOLATIONS"
        exit 1
      fi
    
    # No console.log in production
    - |
      CONSOLE_LOGS=$(grep -rn "console\.\(log\|debug\|info\)" src/ --include="*.ts" | grep -v "test\|spec\|\.d\.ts" || true)
      if [ -n "$CONSOLE_LOGS" ]; then
        echo "WARNING: console.log found in production code:"
        echo "$CONSOLE_LOGS"
      fi
    
    # Check for TODO/FIXME
    - |
      TODOS=$(grep -rn "TODO\|FIXME\|HACK\|XXX" src/ --include="*.ts" || true)
      if [ -n "$TODOS" ]; then
        echo "WARNING: TODOs found:"
        echo "$TODOS"
      fi
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

# Security review
security-review:
  stage: review
  script:
    - echo "Running security checks..."
    # Check for potential secrets
    - |
      SECRETS=$(grep -rn "password\s*=\|api_key\s*=\|secret\s*=\|token\s*=" src/ --include="*.ts" | grep -v "test\|spec\|\.d\.ts\|\.env" || true)
      if [ -n "$SECRETS" ]; then
        echo "SECURITY WARNING: Possible hardcoded secrets:"
        echo "$SECRETS"
        exit 1
      fi
    # Dependency audit
    - npm audit --production
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### MR Review Checklist (Auto-generated)

Tambahkan di `.gitlab/merge_request_templates/Default.md`:

```markdown
## AI Review Checklist

### Architecture
- [ ] Clean Architecture layers respected
- [ ] No dependency rule violations
- [ ] Proper use of interfaces/ports

### Security
- [ ] Input validation present
- [ ] No hardcoded credentials
- [ ] Proper error handling (no info leak)

### Quality
- [ ] Code follows naming conventions
- [ ] No duplicated logic
- [ ] Proper error handling

### Testing
- [ ] Unit tests added/updated
- [ ] Edge cases covered
- [ ] Coverage maintained

### Documentation
- [ ] API docs updated (if applicable)
- [ ] ADR created (if architecture decision)
- [ ] README updated (if applicable)
```

## Review Metrics

Track review effectiveness:

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Defect escape rate | < 5% | Bugs found in production / total bugs |
| Review coverage | 100% | MRs reviewed / total MRs |
| Review turnaround | < 4h | Time from MR creation to first review |
| False positive rate | < 10% | Invalid review comments / total comments |

## Best Practices

1. **Review before merge, always** - No exceptions
2. **Automated first** - Let CI/CD catch obvious issues
3. **AI review + human review** - AI catches patterns, humans catch logic
4. **Constructive feedback** - Review comments should suggest fixes
5. **Track patterns** - Recurring issues should become governance rules
6. **Continuous improvement** - Update review checklist based on findings


---

## Appendix: Review Checklist Lengkap (dari Review Checklist Standards)

### 1. Arsitektur
- [ ] Tidak ada pelanggaran dependency rule
- [ ] Core tidak mengakses infrastructure/presentation
- [ ] Tidak ada business logic di UI
- [ ] UseCase digunakan sebagai pusat business logic
- [ ] Tidak ada API call langsung di component

### 2. Code Pattern
- [ ] Entity bersih (tanpa dependency eksternal)
- [ ] Repository interface di core
- [ ] Repository implementation di infrastructure
- [ ] Mapper digunakan (tidak mapping di UI)
- [ ] DTO tidak bocor ke UI
- [ ] UseCase berbentuk factory function (DI-ready)

### 3. Dependency Injection
- [ ] Semua dependency menggunakan DI
- [ ] Tidak ada `new` manual untuk dependency utama
- [ ] Registration dilakukan di container
- [ ] Naming DI konsisten (camelCase)

### 4. Security
- [ ] Tidak ada hardcoded secret/token
- [ ] Tidak ada `dangerouslySetInnerHTML` tanpa sanitasi
- [ ] Redirect sudah divalidasi
- [ ] Input sudah divalidasi
- [ ] Output aman (tidak expose internal error)
- [ ] Tidak ada data sensitif di log
- [ ] Token tidak disimpan di localStorage

### 5. Testing
- [ ] UseCase memiliki unit test
- [ ] Mapper memiliki unit test
- [ ] Repository memiliki test
- [ ] ViewModel memiliki test
- [ ] Edge case dan error case diuji
- [ ] Coverage memenuhi minimum (>= 80%)

### 6. Konfigurasi dan Konvensi
- [ ] Menggunakan path alias
- [ ] Tidak ada penggunaan `any`
- [ ] Tidak ada `@ts-ignore`
- [ ] Lolos TypeScript strict mode
- [ ] Lolos ESLint tanpa warning
- [ ] Naming convention konsisten

### 7. UI dan UX
- [ ] Tidak ada hardcoded text (gunakan i18n)
- [ ] Loading state ditangani
- [ ] Error state ditangani
- [ ] Empty state ditangani
- [ ] Aksesibilitas dasar terpenuhi

### 8. Performance
- [ ] Tidak ada unnecessary re-render
- [ ] Tidak ada API call berulang tanpa kontrol
- [ ] Lazy loading digunakan jika perlu
- [ ] Tidak ada bundle bloat signifikan

### 9. Logging dan Observability
- [ ] Error dilog dengan aman
- [ ] Tidak expose stack trace ke UI
- [ ] Event penting tercatat
- [ ] Correlation ID digunakan

### 10. Security Critical (untuk fitur sensitif)
- [ ] Unauthorized access ditolak
- [ ] Invalid token ditangani dengan benar
- [ ] Redirect tidak bisa dimanipulasi
- [ ] Input abnormal tidak menyebabkan crash

### Anti-Pattern Check
- [ ] Tidak ada API call di component
- [ ] Tidak ada business logic di ViewModel
- [ ] Tidak ada import langsung infra ke core
- [ ] Tidak ada DTO langsung ke UI
- [ ] Tidak ada hardcoded config/URL
- [ ] Tidak ada side effect di tempat yang tidak semestinya