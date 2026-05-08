# Quickstart Guide

## Memulai Enterprise AI-Native SDLC Framework

Panduan ini membantu Anda memulai framework dalam project baru atau existing.

## Prerequisites

- GitLab account dengan akses CI/CD
- Git repository yang sudah di-setup
- Node.js / Python / bahasa pilihan Anda
- Docker (untuk CI/CD pipeline)

## Step 1: Inisialisasi Struktur Project

Buat struktur folder berikut di root project:

```
project-root/
├── .gitlab/
│   └── ci/
│       ├── lint.yml
│       ├── test.yml
│       ├── build.yml
│       ├── security.yml
│       └── deploy.yml
├── .gitlab-ci.yml
├── docs/
│   ├── product/
│   │   ├── vision.md
│   │   ├── roadmap.md
│   │   └── business-goals.md
│   ├── requirements/
│   │   ├── intake/
│   │   └── validation/
│   ├── specs/
│   │   ├── brd/
│   │   ├── prd/
│   │   └── srs/
│   ├── design/
│   │   ├── system/
│   │   ├── technical/
│   │   ├── ui-ux/
│   │   └── security/
│   ├── governance/
│   │   ├── ai-policy.md
│   │   ├── coding-standards.md
│   │   └── review-policy.md
│   └── adr/
├── .kiro/
│   ├── steering/
│   │   └── project-standards.md
│   └── skills/
│       ├── create-api.md
│       ├── create-usecase.md
│       ├── create-repository.md
│       └── create-component.md
├── src/
├── tests/
└── README.md
```

## Step 2: Setup GitLab CI/CD

Buat file `.gitlab-ci.yml` di root project:

```yaml
stages:
  - lint
  - typecheck
  - test
  - security
  - build
  - deploy-preview
  - e2e
  - deploy

include:
  - local: '.gitlab/ci/lint.yml'
  - local: '.gitlab/ci/test.yml'
  - local: '.gitlab/ci/build.yml'
  - local: '.gitlab/ci/security.yml'
  - local: '.gitlab/ci/deploy.yml'

variables:
  NODE_IMAGE: node:20-alpine
  DOCKER_IMAGE: docker:24.0.5
```

## Step 3: Definisikan Product Vision

Buat `docs/product/vision.md`:

```markdown
# Product Vision

## Problem Statement
[Masalah yang ingin diselesaikan]

## Target Users
[Siapa pengguna utama]

## Business Value
[Nilai bisnis yang dihasilkan]

## Success Metrics
- [Metric 1]
- [Metric 2]

## AI Opportunities
[Dimana AI bisa memberikan value]
```

## Step 4: Setup AI Governance

Buat `docs/governance/ai-policy.md`:

```markdown
# AI Policy

## Approved AI Tools
- Kiro (primary development)
- [Other tools]

## AI Usage Rules
1. AI WAJIB bekerja berdasarkan specification
2. AI WAJIB melalui review sebelum merge
3. AI TIDAK BOLEH mengubah architecture tanpa approval
4. AI WAJIB mengikuti coding standards

## Security Rules
1. AI TIDAK BOLEH mengakses production data
2. AI TIDAK BOLEH menyimpan credentials
3. Semua AI output WAJIB melalui security scan
```

## Step 5: Buat Kiro Skills

Buat `.kiro/skills/create-api.md`:

```markdown
# Skill: Create API Endpoint

## Prerequisites
- Specification sudah ada di docs/specs/
- Architecture sudah di-review

## Steps
1. Baca specification terkait
2. Buat route handler sesuai pattern yang ada
3. Implementasi use case / service layer
4. Buat repository jika perlu
5. Tulis unit test
6. Tulis integration test
7. Update API documentation

## Standards
- Gunakan Clean Architecture
- Validasi input dengan schema
- Handle error dengan pattern yang konsisten
- Logging di setiap layer
```

## Step 6: Mulai Development Cycle

Workflow development mengikuti pola:

```
1. Buat GitLab Issue (1 issue = 1 bounded context)
2. Buat branch dari issue
3. AI membaca specification terkait
4. AI generate code berdasarkan spec + skills
5. AI review code sendiri
6. Push ke branch
7. CI/CD pipeline berjalan
8. Merge Request review
9. Merge ke main
```

## Next Steps

- Baca `layer-0-product-vision.md` untuk detail Layer 0
- Baca `gitlab-cicd-setup.md` untuk setup CI/CD lengkap
- Baca `project-structure.md` untuk detail struktur project
