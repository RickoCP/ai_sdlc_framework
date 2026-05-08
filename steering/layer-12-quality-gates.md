# Layer 12 — Automated Quality Gates

## CI/CD Pipeline Flow

```
Lint → Typecheck → Unit Test → Coverage → Build → Security Scan
→ Performance Check → Preview Deploy → E2E Test → Merge
```

## Tujuan
- Menjaga quality
- Menjaga consistency
- Mencegah broken deployment

## GitLab CI/CD Pipeline Lengkap

### Main Pipeline (`.gitlab-ci.yml`)

```yaml
stages:
  - lint
  - typecheck
  - test
  - coverage
  - security
  - build
  - deploy-preview
  - e2e
  - deploy-staging
  - deploy-production

variables:
  NODE_IMAGE: node:20-alpine
  DOCKER_IMAGE: docker:24.0.5
  DOCKER_DIND: docker:24.0.5-dind
  FF_USE_FASTZIP: "true"
  CACHE_COMPRESSION_LEVEL: "fast"

# Cache node_modules across jobs
default:
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: pull

include:
  - local: '.gitlab/ci/lint.yml'
  - local: '.gitlab/ci/typecheck.yml'
  - local: '.gitlab/ci/test.yml'
  - local: '.gitlab/ci/security.yml'
  - local: '.gitlab/ci/build.yml'
  - local: '.gitlab/ci/deploy.yml'
```

---

### Lint Stage (`.gitlab/ci/lint.yml`)

```yaml
lint:
  stage: lint
  image: $NODE_IMAGE
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: pull-push
  script:
    - npm ci --prefer-offline
    - npm run lint
    - npm run format:check
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_COMMIT_BRANCH == "develop"'

lint:commit-message:
  stage: lint
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npx commitlint --from $CI_MERGE_REQUEST_DIFF_BASE_SHA --to HEAD
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Typecheck Stage (`.gitlab/ci/typecheck.yml`)

```yaml
typecheck:
  stage: typecheck
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npx tsc --noEmit
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_COMMIT_BRANCH == "develop"'
```

### Test Stage (`.gitlab/ci/test.yml`)

```yaml
unit-test:
  stage: test
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npm run test:unit -- --coverage
  artifacts:
    when: always
    paths:
      - coverage/
    reports:
      junit: junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'

integration-test:
  stage: test
  image: $NODE_IMAGE
  services:
    - postgres:15-alpine
    - redis:7-alpine
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: test_user
    POSTGRES_PASSWORD: test_pass
    DATABASE_URL: "postgresql://test_user:test_pass@postgres:5432/test_db"
    REDIS_URL: "redis://redis:6379"
  script:
    - npm ci --prefer-offline
    - npm run test:integration
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'

coverage-check:
  stage: coverage
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npm run test:unit -- --coverage
    - |
      COVERAGE=$(cat coverage/coverage-summary.json | node -e "
        const data = require('./coverage/coverage-summary.json');
        console.log(data.total.lines.pct);
      ")
      echo "Coverage: $COVERAGE%"
      if (( $(echo "$COVERAGE < 80" | bc -l) )); then
        echo "ERROR: Coverage $COVERAGE% is below threshold 80%"
        exit 1
      fi
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Security Stage (`.gitlab/ci/security.yml`)

```yaml
dependency-scan:
  stage: security
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npm audit --production --audit-level=high
  allow_failure: false
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'

sast:
  stage: security
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npx eslint src/ --config .eslintrc.security.js --format json --output-file gl-sast-report.json || true
  artifacts:
    reports:
      sast: gl-sast-report.json
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

secret-detection:
  stage: security
  image: $NODE_IMAGE
  script:
    - |
      echo "Scanning for secrets..."
      SECRETS_FOUND=0
      
      # Check for common secret patterns
      PATTERNS="password=|api_key=|secret_key=|private_key|AWS_SECRET|GITHUB_TOKEN"
      
      RESULTS=$(grep -rn "$PATTERNS" src/ --include="*.ts" --include="*.js" | grep -v "test\|spec\|mock\|\.d\.ts" || true)
      
      if [ -n "$RESULTS" ]; then
        echo "CRITICAL: Possible secrets detected!"
        echo "$RESULTS"
        SECRETS_FOUND=1
      fi
      
      if [ $SECRETS_FOUND -eq 1 ]; then
        exit 1
      fi
      
      echo "No secrets detected."
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

license-scan:
  stage: security
  image: $NODE_IMAGE
  script:
    - npm ci --prefer-offline
    - npx license-checker --production --failOn "GPL-3.0;AGPL-3.0"
  allow_failure: true
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

### Build Stage (`.gitlab/ci/build.yml`)

```yaml
build:
  stage: build
  image: $DOCKER_IMAGE
  services:
    - $DOCKER_DIND
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - |
      docker build \
        --cache-from $CI_REGISTRY_IMAGE:latest \
        --tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA \
        --tag $CI_REGISTRY_IMAGE:latest \
        .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker push $CI_REGISTRY_IMAGE:latest
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_COMMIT_BRANCH == "develop"'
```

### Deploy Stage (`.gitlab/ci/deploy.yml`)

```yaml
deploy-preview:
  stage: deploy-preview
  image: $NODE_IMAGE
  environment:
    name: preview/$CI_COMMIT_REF_SLUG
    url: https://$CI_COMMIT_REF_SLUG.preview.example.com
    on_stop: stop-preview
    auto_stop_in: 1 week
  script:
    - echo "Deploying preview environment..."
    # Deploy to preview (adjust based on your infrastructure)
    - npm ci --prefer-offline
    - npm run build
    - echo "Preview deployed to https://$CI_COMMIT_REF_SLUG.preview.example.com"
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

stop-preview:
  stage: deploy-preview
  script:
    - echo "Stopping preview environment..."
  environment:
    name: preview/$CI_COMMIT_REF_SLUG
    action: stop
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: manual

e2e-test:
  stage: e2e
  image: mcr.microsoft.com/playwright:v1.40.0-focal
  script:
    - npm ci --prefer-offline
    - npx playwright install
    - npm run test:e2e
  artifacts:
    when: always
    paths:
      - playwright-report/
    reports:
      junit: e2e-results.xml
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

deploy-staging:
  stage: deploy-staging
  image: $DOCKER_IMAGE
  environment:
    name: staging
    url: https://staging.example.com
  script:
    - echo "Deploying to staging..."
    # kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'

deploy-production:
  stage: deploy-production
  image: $DOCKER_IMAGE
  environment:
    name: production
    url: https://example.com
  script:
    - echo "Deploying to production..."
    # kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
  when: manual
```

## Quality Gate Thresholds

| Gate | Threshold | Action on Fail |
|------|-----------|----------------|
| Lint | 0 errors | Block merge |
| Typecheck | 0 errors | Block merge |
| Unit Test | All pass | Block merge |
| Coverage | >= 80% | Block merge |
| Security (high) | 0 vulnerabilities | Block merge |
| Security (medium) | Report only | Warning |
| Build | Success | Block merge |
| E2E | All pass | Block merge |
| Performance | < 3s response | Warning |

## GitLab MR Settings

### Merge Request Approval Rules

```
Settings → Merge Requests → Approval Rules:
- Minimum approvals: 1
- Code owners approval: Required
- Pipeline must succeed: Yes
- All discussions resolved: Yes
- No broken pipelines: Yes
```

### Branch Protection

```
Settings → Repository → Protected Branches:
- main: No push, merge only via MR
- develop: No force push, merge via MR
```

## Best Practices

1. **Fast feedback** - Lint dan typecheck harus cepat (< 2 min)
2. **Parallel where possible** - Run independent jobs in parallel
3. **Cache aggressively** - Cache node_modules, Docker layers
4. **Fail fast** - Cheap checks first, expensive checks later
5. **Clear error messages** - Developer harus tahu apa yang salah
6. **No manual gates for dev** - Hanya production yang manual
7. **Preview environments** - Setiap MR punya preview


---

## Appendix: Testing Pattern Standards

### Test Pyramid (WAJIB)

1. **Unit Test** - Mayoritas (use case, mapper, helper, ViewModel)
2. **Integration Test** - Boundary penting (repository + adapter, route handler)
3. **E2E Test** - Happy path + critical path (login, payment, binding)

### Coverage Policy

- Statements: >= 80%
- Branches: >= 80%
- Functions: >= 80%
- Lines: >= 80%
- Coverage lebih tinggi WAJIB untuk: auth, payment, redirect, middleware

### Yang WAJIB Diuji

| Layer | Target Test |
|-------|-------------|
| Use Case | Happy path, error path, edge case, dependency failure |
| Mapper | Field mapping, fallback/default, abnormal response |
| Repository | Success response, error response, header/payload, mapper call |
| ViewModel | Action/mutation, state change, success/error path |
| Middleware | Auth flow, redirect validation, safe fallback |

### Testing Rules

- Mock dependency, bukan implementation yang diuji
- Gunakan QueryClient terisolasi per test
- Nonaktifkan retry di test
- Bersihkan cache antar test
- Jangan gunakan network nyata
- Reset Zustand state per test

### Anti-Pattern Testing

- Test yang hanya mengejar coverage tanpa assertion bermakna
- Mock berlebihan sampai test tidak memvalidasi perilaku nyata
- Snapshot berlebihan untuk komponen kompleks
- Test rapuh yang bergantung pada detail DOM
- Shared mutable state antar test

---

## Appendix: Performance Standards

### Target KPI

- LCP < 2.5s
- INP < 200ms
- CLS < 0.1
- TTFB < 500ms

### Rendering Strategy (Next.js)

- Default: React Server Components (RSC)
- SSR: Data sangat dinamis, per-request personalization
- SSG/ISR: Data relatif statis
- CSR: Interaksi kompleks di client

### Bundle Optimization

- Minimalkan `use client`
- Dynamic import untuk komponen berat
- Tree shaking (import spesifik)
- Gunakan `next/image` dengan lazy loading

### Data Fetching

- Gunakan fetch caching (`force-cache`, `revalidate`)
- Hindari duplicate fetch
- React Query untuk client state server
- Debounce/throttle untuk input search

### Performance Anti-Pattern

- Terlalu banyak `use client`
- Fetch berulang tanpa cache
- Bundle besar tanpa splitting
- Render blocking component berat
- Waterfall API call
- Perhitungan berat di render

---

## Appendix: Release dan CI/CD Standards

### Pipeline Stages (WAJIB)

1. Install
2. Lint
3. Type Check
4. Unit Test
5. Coverage Check
6. Build
7. Security Scan
8. Deploy

### Quality Gate (STRICT)

Pipeline HARUS gagal jika:
- Lint error
- TypeScript error
- Test gagal
- Coverage < 80%
- Build gagal

### Branching Strategy

- `main` - production
- `develop` - staging
- `feature/*` - development
- `hotfix/*` - urgent fix
- Tidak boleh commit langsung ke `main`

### Versioning (SemVer)

- MAJOR - breaking change
- MINOR - fitur baru
- PATCH - bug fix

### Deployment Strategy

- Standard deploy setelah merge
- Blue-Green / Canary (disarankan)
- Rollback HARUS tersedia dan cepat

### Release Validation Checklist

- [ ] Semua test lulus
- [ ] Tidak ada critical bug
- [ ] Tidak ada security issue
- [ ] Performance tidak menurun signifikan
- [ ] Observability aktif