# Project Structure & Quickstart

## Quickstart — Memulai Framework

Panduan cepat memulai Enterprise AI-Native SDLC Framework di project baru atau existing.

### Prerequisites

- GitLab account dengan akses CI/CD
- Git repository (akan di-setup otomatis jika belum ada, lihat `git-workflow-automation.md`)
- Node.js / Python / bahasa pilihan Anda
- Docker (untuk CI/CD pipeline)

### 5 Langkah Memulai

```
1. Inisialisasi struktur folder (lihat section di bawah)
2. Setup GitLab CI/CD (lihat gitlab-cicd-setup.md)
3. Definisikan Product Vision (docs/product/vision.md)
4. Setup AI Governance (docs/governance/ai-policy.md)
5. Mulai Development Cycle (issue-driven)
```

### Development Cycle

Workflow development mengikuti pola:

```
1. Buat GitLab Issue (1 issue = 1 bounded context)
2. Buat branch dari issue
3. AI membaca specification terkait
4. AI generate code berdasarkan spec + skills
5. AI review code sendiri
6. Commit + Push ke branch (otomatis via hook)
7. CI/CD pipeline berjalan
8. Merge Request review
9. Merge ke develop/main
```

### Next Steps

- Baca `layer-0-product-vision.md` untuk detail Layer 0
- Baca `gitlab-cicd-setup.md` untuk setup CI/CD lengkap
- Baca `git-workflow-automation.md` untuk git init, push, dan branch strategy

---

## Struktur Folder yang Direkomendasikan

```
project-root/
│
├── .gitlab/
│   ├── ci/
│   │   ├── lint.yml              # Linting stage
│   │   ├── typecheck.yml         # Type checking stage
│   │   ├── test.yml              # Unit & integration test
│   │   ├── security.yml          # Security scanning
│   │   ├── build.yml             # Build stage
│   │   ├── deploy-preview.yml    # Preview deployment
│   │   ├── e2e.yml               # End-to-end testing
│   │   └── deploy.yml            # Production deployment
│   ├── issue_templates/
│   │   ├── Feature.md
│   │   ├── Task.md
│   │   ├── Bug.md
│   │   └── Spike.md
│   └── merge_request_templates/
│       ├── Default.md
│       ├── Feature.md
│       └── Hotfix.md
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
│   ├── traceability/
│   │   ├── traceability-matrix.md
│   │   └── coverage-report.md
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
│   ├── application/              # Application layer (use cases, ports)
│   ├── infrastructure/           # Infrastructure layer (DB, API clients)
│   ├── presentation/             # Presentation layer (controllers, UI)
│   └── shared/                   # Shared utilities, constants, types
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
├── .env                          # Environment variables (JANGAN COMMIT!)
├── .gitignore
└── README.md
```

---

## Penjelasan Setiap Folder

### `.gitlab/`
Konfigurasi GitLab: CI/CD stages, issue templates, MR templates, CODEOWNERS.

### `docs/`
Semua artifact dari setiap layer SDLC. Ini menjadi **project memory** dan **AI context**.

| Folder | Layer | Fungsi |
|--------|-------|--------|
| `docs/product/` | Layer 0 | Product vision, roadmap, business goals |
| `docs/requirements/` | Layer 1-2 | Raw input + validated requirements |
| `docs/specs/` | Layer 3 | BRD, PRD, SRS — specification lengkap |
| `docs/design/` | Layer 4 | System, technical, UI/UX, security design |
| `docs/governance/` | Layer 5 | AI policy, coding standards, review policy |
| `docs/traceability/` | Layer 11 | Requirement → Code → Test mapping |
| `docs/adr/` | Layer 14 | Architecture Decision Records |

### `.kiro/`
Kiro-specific configuration:
- `steering/` — Project standards dan AI guidelines
- `skills/` — Reusable AI engineering workflows (Layer 6)
- `settings/mcp.json` — MCP server configuration

### `src/`
Source code mengikuti **Clean Architecture** pattern:
- `domain/` — Entities, value objects, domain rules (no dependencies)
- `application/` — Use cases, ports/interfaces
- `infrastructure/` — Adapters, repositories, external services
- `presentation/` — Controllers, UI components, view models
- `shared/` — Constants, types, utilities

### `tests/`
Test files terpisah per level:
- `unit/` — Domain logic, use cases, pure functions
- `integration/` — API endpoints, database operations
- `e2e/` — Full user flow tests

---

## Naming Conventions

### Files
- Gunakan `kebab-case`: `user-service.ts`, `create-order.md`
- Test files: `{name}.test.ts` atau `{name}.spec.ts`
- Spec files: `{feature-name}-spec.md`
- ADR files: `{number}-{title}.md` (e.g., `001-framework-choice.md`)

### Folders
- Gunakan `kebab-case`: `user-management/`, `order-processing/`
- Domain folders mengikuti bounded context

### Branches

> Detail branch strategy lihat `layer-8-issue-driven-dev.md` dan `git-workflow-automation.md`.

```
feature/issue-{N}-{short-description}
bugfix/issue-{N}-{short-description}
hotfix/issue-{N}-{short-description}
spike/issue-{N}-{short-description}
```

---

## Context Accumulation

Setiap folder di `docs/` menghasilkan artifact yang menjadi context untuk layer berikutnya:

```
docs/product/vision.md → Input untuk requirement intake
docs/requirements/ → Input untuk specification
docs/specs/ → Input untuk design
docs/design/ → Input untuk coding
docs/governance/ → Constraint untuk semua layer
docs/adr/ → Historical context untuk keputusan
docs/traceability/ → Validation untuk completeness
```

Ini adalah implementasi dari Layer 10 (Continuous Context Accumulation).

---

## AI Agent: Scaffold Project

Saat membuat project baru, AI Agent WAJIB membuat minimal struktur berikut:

```
project-root/
├── .gitlab/issue_templates/Task.md
├── .gitlab-ci.yml
├── docs/product/vision.md
├── docs/governance/ai-policy.md
├── src/.gitkeep
├── tests/.gitkeep
├── .env (template, user isi sendiri)
├── .eslintrc.json (atau eslint.config.mjs)
├── .prettierrc
├── .gitignore
├── package.json (dengan scripts: lint, build, typecheck, test)
├── tsconfig.json
├── vitest.config.ts (atau jest.config.ts)
├── vercel.json (deployment config)
└── README.md
```

Folder lain dibuat seiring development berjalan (on-demand).

---

## CI-Readiness: Konfigurasi WAJIB Sebelum Push

**KRITIS:** File-file berikut WAJIB ada sebelum push pertama ke GitLab. Tanpa ini, CI/CD pipeline AKAN GAGAL karena tools membutuhkan konfigurasi non-interaktif.

### Masalah Umum

Banyak tools (ESLint, Prettier, dll) menampilkan **prompt interaktif** saat pertama kali dijalankan tanpa config file. Di CI/CD environment (non-interactive), prompt ini menyebabkan pipeline hang atau gagal.

### File Konfigurasi WAJIB per Tech Stack

#### Next.js / React

| File | Fungsi | Tanpa file ini → |
|------|--------|-----------------|
| `.eslintrc.json` atau `eslint.config.mjs` | ESLint config | `next lint` tampilkan prompt interaktif → CI gagal |
| `tsconfig.json` | TypeScript config | `tsc --noEmit` gagal |
| `.prettierrc` | Prettier config | Format check inconsistent |
| `next.config.ts` | Next.js config | Build gagal |
| `vitest.config.ts` / `jest.config.ts` | Test runner config | Test command gagal |

#### ESLint Config (WAJIB untuk Next.js)

**Option A: `.eslintrc.json`** (Next.js < 15 atau compatibility mode)
```json
{
  "extends": ["next/core-web-vitals", "next/typescript"]
}
```

**Option B: `eslint.config.mjs`** (Next.js 15+ flat config)
```javascript
import { dirname } from "path";
import { fileURLToPath } from "url";
import { FlatCompat } from "@eslint/eslintrc";

const __dirname = dirname(fileURLToPath(import.meta.url));
const compat = new FlatCompat({ baseDirectory: __dirname });

const eslintConfig = [
  ...compat.extends("next/core-web-vitals", "next/typescript")
];

export default eslintConfig;
```

#### TypeScript Config (`tsconfig.json`)
```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

#### Prettier Config (`.prettierrc`)
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

#### Vitest Config (`vitest.config.ts`)
```typescript
import { defineConfig } from 'vitest/config';
import path from 'path';

export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    coverage: {
      provider: 'v8',
      reporter: ['text', 'json', 'html', 'cobertura'],
      thresholds: {
        statements: 80,
        branches: 80,
        functions: 80,
        lines: 80,
      },
    },
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});
```

### package.json Scripts (WAJIB)

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "lint:fix": "next lint --fix",
    "format:check": "prettier --check .",
    "format:fix": "prettier --write .",
    "typecheck": "tsc --noEmit",
    "test": "vitest",
    "test:unit": "vitest run",
    "test:coverage": "vitest run --coverage"
  }
}
```

### AI Agent Rules: CI-Readiness

1. **WAJIB buat semua config files SEBELUM push pertama**
2. **JANGAN push tanpa ESLint config** — ini penyebab #1 pipeline gagal
3. **JANGAN gunakan tools yang membutuhkan interactive prompt di CI**
4. **SELALU test `npm run lint` dan `npm run build` secara lokal sebelum push**
5. **Jika menambahkan tool baru**, pastikan config file-nya sudah ada
6. **Jalankan `npm audit fix`** sebelum push jika ada vulnerability

### Checklist Sebelum Push Pertama

```markdown
- [ ] .eslintrc.json / eslint.config.mjs ada dan valid
- [ ] tsconfig.json ada dan valid
- [ ] .prettierrc ada
- [ ] vitest.config.ts / jest.config.ts ada (jika ada test)
- [ ] package.json punya scripts: lint, build, typecheck, test
- [ ] `npm run lint` berjalan tanpa prompt interaktif
- [ ] `npm run build` berhasil
- [ ] `npm run typecheck` berhasil
- [ ] `npm audit` tidak ada critical vulnerability
- [ ] Semua dependencies ter-install (`npm ci` berhasil)
```
