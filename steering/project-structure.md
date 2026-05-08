# Project Structure

## Struktur Folder yang Direkomendasikan

```
project-root/
│
├── .gitlab/
│   └── ci/
│       ├── lint.yml              # Linting stage
│       ├── typecheck.yml         # Type checking stage
│       ├── test.yml              # Unit & integration test
│       ├── security.yml          # Security scanning
│       ├── build.yml             # Build stage
│       ├── deploy-preview.yml    # Preview deployment
│       ├── e2e.yml               # End-to-end testing
│       └── deploy.yml            # Production deployment
│
├── .gitlab-ci.yml                # Main CI/CD config
│
├── docs/
│   ├── product/
│   │   ├── vision.md             # Product vision
│   │   ├── roadmap.md            # Product roadmap
│   │   ├── business-goals.md     # Business goals & KPI
│   │   ├── success-metrics.md    # Success metrics
│   │   └── ai-opportunities.md   # AI opportunity mapping
│   │
│   ├── requirements/
│   │   ├── intake/
│   │   │   ├── raw/              # Raw input (PDF, notes, etc)
│   │   │   └── extracted/        # AI-extracted requirements
│   │   └── validation/
│   │       ├── ambiguity-report.md
│   │       ├── missing-requirements.md
│   │       ├── conflict-analysis.md
│   │       └── risk-analysis.md
│   │
│   ├── specs/
│   │   ├── brd/                  # Business Requirements
│   │   │   ├── business-flow.md
│   │   │   └── stakeholder-matrix.md
│   │   ├── prd/                  # Product Requirements
│   │   │   ├── user-stories.md
│   │   │   ├── acceptance-criteria.md
│   │   │   └── feature-matrix.md
│   │   └── srs/                  # System Requirements
│   │       ├── architecture.md
│   │       ├── sequence-diagram.md
│   │       ├── state-flow.md
│   │       └── failure-scenario.md
│   │
│   ├── design/
│   │   ├── system/
│   │   │   ├── high-level-architecture.md
│   │   │   ├── c4-model.md
│   │   │   ├── sequence-diagram.md
│   │   │   ├── state-diagram.md
│   │   │   ├── event-flow.md
│   │   │   └── deployment.md
│   │   ├── technical/
│   │   │   ├── clean-architecture.md
│   │   │   ├── folder-structure.md
│   │   │   ├── naming-convention.md
│   │   │   ├── testing-pattern.md
│   │   │   └── error-handling.md
│   │   ├── ui-ux/
│   │   │   ├── wireframe.md
│   │   │   ├── component-library.md
│   │   │   ├── accessibility.md
│   │   │   └── design-token.md
│   │   └── security/
│   │       ├── threat-model.md
│   │       ├── trust-boundary.md
│   │       ├── attack-surface.md
│   │       └── mitigation-plan.md
│   │
│   ├── governance/
│   │   ├── ai-policy.md          # AI usage policy
│   │   ├── approved-tools.md     # Approved AI tools
│   │   ├── prompt-standard.md    # Prompt engineering standards
│   │   ├── security-policy.md    # Security policy
│   │   └── code-review-policy.md # Code review policy
│   │
│   └── adr/                      # Architecture Decision Records
│       ├── 001-framework-choice.md
│       └── template.md
│
├── .kiro/
│   ├── steering/
│   │   ├── project-standards.md  # Project-wide standards
│   │   ├── coding-conventions.md # Coding conventions
│   │   └── ai-guidelines.md     # AI interaction guidelines
│   ├── skills/
│   │   ├── create-api.md
│   │   ├── create-usecase.md
│   │   ├── create-repository.md
│   │   ├── create-component.md
│   │   ├── create-test.md
│   │   └── create-migration.md
│   └── settings/
│       └── mcp.json
│
├── src/
│   ├── domain/                   # Domain layer (entities, value objects)
│   ├── application/              # Application layer (use cases)
│   ├── infrastructure/           # Infrastructure layer (DB, API clients)
│   ├── presentation/             # Presentation layer (controllers, UI)
│   └── shared/                   # Shared utilities
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── scripts/
│   ├── setup.sh
│   └── seed.sh
│
└── README.md
```

## Penjelasan Setiap Folder

### `.gitlab/ci/`
Berisi konfigurasi CI/CD yang di-split per stage. Memudahkan maintenance dan reusability.

### `docs/`
Semua artifact dari setiap layer SDLC disimpan di sini. Ini menjadi **project memory** dan **AI context**.

### `docs/product/`
Output dari Layer 0 (Product Vision). Menjadi north star untuk seluruh development.

### `docs/requirements/`
Output dari Layer 1 (Intake) dan Layer 2 (Validation). Raw input dan hasil validasi AI.

### `docs/specs/`
Output dari Layer 3 (SDD). Specification lengkap yang menjadi basis coding.

### `docs/design/`
Output dari Layer 4 (Design System). Mencakup system, technical, UI/UX, dan security design.

### `docs/governance/`
Output dari Layer 5 (AI Governance). Policy dan standards yang harus diikuti AI.

### `docs/adr/`
Architecture Decision Records. Setiap keputusan arsitektur didokumentasikan di sini.

### `.kiro/steering/`
Steering files untuk Kiro. Memberikan context dan instruksi ke AI saat development.

### `.kiro/skills/`
AI Engineering Skills (Layer 6). Reusable workflow yang bisa dipanggil AI.

### `src/`
Source code mengikuti Clean Architecture pattern.

### `tests/`
Test files terpisah per level (unit, integration, e2e).

## Naming Conventions

### Files
- Gunakan `kebab-case` untuk semua file: `user-service.ts`, `create-order.md`
- Spec files: `{feature-name}-spec.md`
- ADR files: `{number}-{title}.md`

### Folders
- Gunakan `kebab-case`: `user-management/`, `order-processing/`
- Domain folders mengikuti bounded context

### Branches
- Feature: `feature/{issue-number}-{short-description}`
- Bugfix: `bugfix/{issue-number}-{short-description}`
- Hotfix: `hotfix/{issue-number}-{short-description}`

### Commits
```
type(scope): description

feat(auth): implement JWT token refresh
fix(payment): handle timeout on binding flow
docs(specs): add payment SRS
chore(ci): update security scan config
```

## Context Accumulation

Setiap folder di `docs/` menghasilkan artifact yang menjadi context untuk layer berikutnya:

```
docs/product/vision.md → Input untuk requirement intake
docs/requirements/ → Input untuk specification
docs/specs/ → Input untuk design
docs/design/ → Input untuk coding
docs/governance/ → Constraint untuk semua layer
docs/adr/ → Historical context untuk keputusan
```

Ini adalah implementasi dari Layer 10 (Continuous Context Accumulation).
