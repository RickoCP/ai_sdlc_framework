# Layer 12 — Automated Quality Gates

## CI/CD Pipeline Flow

```
Lint → Typecheck → Unit Test → Coverage → Security Scan → SonarQube
→ Build → Preview Deploy → E2E Test → Merge
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

> **⚠️ PENTING:** Pastikan ESLint sudah dikonfigurasi (`.eslintrc.json` atau `eslint.config.mjs` ada di repo) SEBELUM push. Tanpa config file, `next lint` akan menampilkan prompt interaktif yang menyebabkan CI gagal. Lihat `project-structure.md` section "CI-Readiness" untuk detail.

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
    - npm run test:unit -- --coverage --reporter=junit --outputFile=junit.xml
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
  needs:
    - job: unit-test
      artifacts: true
  script:
    - |
      node -e "
        const fs = require('fs');
        const summaryPath = './coverage/coverage-summary.json';

        if (!fs.existsSync(summaryPath)) {
          console.error('ERROR: coverage-summary.json not found.');
          console.error('Pastikan vitest.config.ts punya reporter: [\"json-summary\"]');
          process.exit(1);
        }

        const data = JSON.parse(fs.readFileSync(summaryPath, 'utf8'));
        const total = data.total;
        const threshold = 80;

        console.log('=== Coverage Report ===');
        console.log('Statements:', total.statements.pct + '%');
        console.log('Branches:  ', total.branches.pct + '%');
        console.log('Functions: ', total.functions.pct + '%');
        console.log('Lines:     ', total.lines.pct + '%');
        console.log('=======================');

        const failures = [];
        if (total.statements.pct < threshold) failures.push('Statements: ' + total.statements.pct + '% < ' + threshold + '%');
        if (total.branches.pct < threshold) failures.push('Branches: ' + total.branches.pct + '% < ' + threshold + '%');
        if (total.functions.pct < threshold) failures.push('Functions: ' + total.functions.pct + '% < ' + threshold + '%');
        if (total.lines.pct < threshold) failures.push('Lines: ' + total.lines.pct + '% < ' + threshold + '%');

        if (failures.length > 0) {
          console.error('');
          console.error('FAILED: Coverage below ' + threshold + '% threshold:');
          failures.forEach(f => console.error('  - ' + f));
          process.exit(1);
        }

        console.log('');
        console.log('PASSED: All coverage metrics >= ' + threshold + '%');
      "
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

**⚠️ PENTING — Agar `coverage-check` Berjalan:**

1. **`unit-test` HARUS generate `coverage-summary.json`** — Pastikan vitest config punya:
   ```typescript
   // vitest.config.ts
   coverage: {
     provider: 'v8',
     reporter: ['text', 'json', 'json-summary', 'html', 'cobertura'],
     // 'json-summary' menghasilkan coverage/coverage-summary.json
   }
   ```

2. **`coverage-check` menggunakan `needs` + `artifacts: true`** — Ini mengambil folder `coverage/` dari job `unit-test` tanpa perlu install dependencies atau re-run tests.

3. **Tidak perlu `bc` atau `npm ci`** — Coverage check hanya baca JSON file dan validasi pakai Node.js yang sudah ada di image.

4. **Threshold dicek per-metrik** — Statements, Branches, Functions, dan Lines semua harus >= 80%.

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

sonarqube-analysis:
  stage: security
  image: 
    name: sonarsource/sonar-scanner-cli:latest
    entrypoint: [""]
  variables:
    SONAR_USER_HOME: "${CI_PROJECT_DIR}/.sonar"
    GIT_DEPTH: "0"
  cache:
    key: "${CI_JOB_NAME}"
    paths:
      - .sonar/cache
  script:
    - |
      sonar-scanner \
        -Dsonar.projectKey=${CI_PROJECT_PATH_SLUG} \
        -Dsonar.projectName="${CI_PROJECT_NAME}" \
        -Dsonar.sources=src/ \
        -Dsonar.tests=tests/ \
        -Dsonar.test.inclusions=**/*.test.ts,**/*.spec.ts \
        -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
        -Dsonar.coverage.exclusions=**/*.test.ts,**/*.spec.ts,**/index.ts \
        -Dsonar.qualitygate.wait=true
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_COMMIT_BRANCH == "develop"'
  allow_failure: false
```

**⚠️ SonarQube Setup (WAJIB untuk Enterprise Mode):**

**Prerequisites:**
1. SonarQube server tersedia (SonarCloud atau self-hosted)
2. Set GitLab CI/CD Variables:
   - `SONAR_HOST_URL`: URL SonarQube server (contoh: `https://sonarcloud.io`)
   - `SONAR_TOKEN`: Token autentikasi SonarQube

**File `sonar-project.properties` (di root project):**
```properties
sonar.projectKey=${CI_PROJECT_PATH_SLUG}
sonar.organization=your-org

# Source
sonar.sources=src/
sonar.tests=tests/
sonar.test.inclusions=**/*.test.ts,**/*.spec.ts

# Coverage
sonar.javascript.lcov.reportPaths=coverage/lcov.info

# Exclusions
sonar.coverage.exclusions=**/*.test.ts,**/*.spec.ts,**/index.ts,**/*.d.ts
sonar.exclusions=node_modules/**,coverage/**,.next/**,dist/**

# Quality Gate
sonar.qualitygate.wait=true
```

**Quality Gate Thresholds (SonarQube):**

| Metric | Threshold | Action |
|--------|-----------|--------|
| Coverage | >= 80% | Block merge |
| Duplicated Lines | < 3% | Block merge |
| Maintainability Rating | A | Block merge |
| Reliability Rating | A | Block merge |
| Security Rating | A | Block merge |
| Security Hotspots Reviewed | 100% | Block merge |
| Bugs | 0 (new code) | Block merge |
| Vulnerabilities | 0 (new code) | Block merge |
| Code Smells | < 10 (new code) | Warning |

**Vitest Coverage untuk SonarQube:**

Pastikan `vitest.config.ts` generate `lcov` report:
```typescript
coverage: {
  provider: 'v8',
  reporter: ['text', 'json', 'json-summary', 'html', 'cobertura', 'lcov'],
  // 'lcov' → generates coverage/lcov.info (WAJIB untuk SonarQube)
}
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

> **Deployment menggunakan Vercel.** Vercel otomatis deploy dari Git push, tapi kita tetap trigger via CLI di pipeline untuk kontrol penuh.

```yaml
deploy-preview:
  stage: deploy-preview
  image: $NODE_IMAGE
  environment:
    name: preview/$CI_COMMIT_REF_SLUG
    url: $VERCEL_PREVIEW_URL
    on_stop: stop-preview
    auto_stop_in: 1 week
  script:
    - npm i -g vercel
    - VERCEL_PREVIEW_URL=$(vercel deploy --token=$VERCEL_TOKEN --yes 2>&1 | grep "https://")
    - echo "Preview deployed to $VERCEL_PREVIEW_URL"
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

stop-preview:
  stage: deploy-preview
  script:
    - echo "Preview will auto-expire via Vercel"
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
  image: $NODE_IMAGE
  environment:
    name: staging
    url: https://staging.${VERCEL_PROJECT_NAME}.vercel.app
  script:
    - npm i -g vercel
    - vercel deploy --token=$VERCEL_TOKEN --yes
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'

deploy-production:
  stage: deploy-production
  image: $NODE_IMAGE
  environment:
    name: production
    url: https://${VERCEL_PROJECT_NAME}.vercel.app
  script:
    - npm i -g vercel
    - vercel deploy --prod --token=$VERCEL_TOKEN --yes
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
  when: manual
```

### Vercel CI/CD Variables (WAJIB di GitLab)

```
Settings → CI/CD → Variables:

- VERCEL_TOKEN: (protected, masked) — dari https://vercel.com/account/tokens
- VERCEL_ORG_ID: (protected) — dari .vercel/project.json
- VERCEL_PROJECT_ID: (protected) — dari .vercel/project.json
```

### Vercel Setup di Project

```bash
# Link project ke Vercel (sekali saja, lokal)
npx vercel link

# File yang dihasilkan (.vercel/project.json) — commit ke repo
# ATAU set via environment variables di GitLab CI
```

### vercel.json (opsional, di root project)

```json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "regions": ["sin1"],
  "env": {
    "NEXT_PUBLIC_APP_ENV": "production"
  }
}
```
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
| SonarQube Quality Gate | Pass | Block merge |
| Performance | < 3s response | Warning |

## GitLab MR Settings

> **Untuk detail setup branch protection dan MR approval**, lihat `gitlab-cicd-setup.md` Step 2.
> **Untuk branch strategy dan naming convention**, lihat `layer-8-issue-driven-dev.md` dan `git-workflow-automation.md`.

### Quick Reference

| Setting | Value |
|---------|-------|
| Minimum approvals | 1 |
| Pipeline must succeed | Yes |
| All discussions resolved | Yes |
| main branch | No push, merge only via MR |
| develop branch | No force push, merge via MR |

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

## Appendix: Release Standards

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