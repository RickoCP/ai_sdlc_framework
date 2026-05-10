# Layer 10 — Continuous Context Accumulation

## Prinsip

Setiap stage menghasilkan artifact yang menjadi context untuk stage berikutnya.

## Context Flow

```
Requirement → Architecture → Tasks → Code → Tests → Logs → ADR
```

Semua artifact menjadi:
- **Project memory** - Histori keputusan dan evolusi
- **AI context** - Input untuk AI di setiap stage
- **Engineering knowledge base** - Referensi untuk tim

## Context Chain

| Stage | Produces | Consumed By |
|-------|----------|-------------|
| Product Vision | vision.md, roadmap.md | Requirement Intake |
| Requirement Intake | requirements/*.md | Requirement Validation |
| Requirement Validation | validation/*.md | Spec-Driven Development |
| Spec-Driven Development | specs/*.md | Design System |
| Design System | design/*.md | Implementation |
| Implementation | src/*.ts | Testing |
| Testing | tests/*.ts, coverage | Review |
| Review | review-notes.md | Improvement |
| Deployment | deploy-logs | Observability |
| Observability | metrics, logs | Continuous Learning |
| Continuous Learning | improvements.md | Next iteration |

## Implementasi dengan Kiro

### Steering Files sebagai Context

```markdown
---
inclusion: always
---

# Project Context

## Current Architecture
#[[file:docs/design/system/high-level-architecture.md]]

## Coding Standards
#[[file:docs/governance/coding-standards.md]]

## Active Specs
#[[file:docs/specs/srs/architecture.md]]
```

### Kiro Spec References

Dalam setiap Kiro Spec, referensikan context yang relevan:

```markdown
# Feature: Payment Processing

## Context
#[[file:docs/specs/prd/user-stories.md]]
#[[file:docs/design/system/sequence-diagram.md]]
#[[file:docs/design/security/threat-model.md]]
#[[file:.kiro/steering/team/payment-team.md]]

## Tasks
- [ ] Implement payment endpoint
- [ ] Add retry logic
- [ ] Write tests
```

## Architecture Decision Records (ADR)

Setiap keputusan arsitektur didokumentasikan sebagai ADR:

### Template: ADR

```markdown
# ADR-{number}: {Title}

## Status
[Proposed | Accepted | Deprecated | Superseded]

## Date
[YYYY-MM-DD]

## Context
[Situasi yang memerlukan keputusan]

## Decision
[Keputusan yang diambil]

## Rationale
[Alasan di balik keputusan]

## Alternatives Considered
1. [Alternative 1] - [Pro/Con]
2. [Alternative 2] - [Pro/Con]

## Consequences
### Positive
- [Consequence 1]

### Negative
- [Consequence 1]

### Risks
- [Risk 1]

## Related
- [Link ke ADR lain]
- [Link ke spec]
- [Link ke issue]
```

### Contoh ADR

```markdown
# ADR-001: Use Clean Architecture

## Status
Accepted

## Date
2024-01-15

## Context
Project membutuhkan architecture yang:
- Testable tanpa external dependencies
- Independent dari framework
- Independent dari database
- Scalable untuk tim yang growing

## Decision
Menggunakan Clean Architecture dengan 4 layers:
- Domain (entities, value objects)
- Application (use cases, ports)
- Infrastructure (database, external services)
- Presentation (controllers, middleware)

## Rationale
- Separation of concerns yang jelas
- Dependency rule memudahkan testing
- AI bisa generate code per layer tanpa conflict
- Tim bisa bekerja paralel di layer berbeda

## Alternatives Considered
1. Hexagonal Architecture - Terlalu complex untuk tim size saat ini
2. Layered Architecture - Kurang strict dependency rules
3. No specific architecture - Akan menyebabkan chaos dengan AI

## Consequences
### Positive
- Code lebih testable
- AI output lebih consistent
- Onboarding developer baru lebih mudah

### Negative
- Boilerplate lebih banyak
- Learning curve untuk tim yang belum familiar
```

## Context Accumulation Strategy

### Level 1: File-Level Context
Setiap file punya header yang menjelaskan context-nya:

```typescript
/**
 * @module CreateUserUseCase
 * @spec docs/specs/srs/api-contract.md#create-user
 * @design docs/design/system/sequence-diagram.md#user-creation
 * @adr ADR-001 (Clean Architecture)
 */
```

### Level 2: Folder-Level Context
Setiap folder punya README yang menjelaskan isinya:

```markdown
# src/application/use-cases/

## Purpose
Berisi semua use case implementations.

## Pattern
Setiap use case mengikuti pattern di .kiro/skills/create-usecase.md

## Dependencies
- Ports (interfaces) di src/application/ports/
- Domain entities di src/domain/entities/

## Related Specs
- docs/specs/srs/api-contract.md
```

### Level 3: Project-Level Context
Steering files memberikan project-wide context:

```markdown
# .kiro/steering/project-context.md

## Project Summary
[Ringkasan project]

## Current Phase
[Phase saat ini di roadmap]

## Active Features
- [Feature 1] - Status: In Progress
- [Feature 2] - Status: Planning

## Key Decisions
- ADR-001: Clean Architecture
- ADR-002: PostgreSQL as primary DB
- ADR-003: JWT for authentication
```

## GitLab Integration

### Wiki sebagai Knowledge Base

Gunakan GitLab Wiki untuk context yang sering berubah:
- Meeting notes
- Sprint retrospectives
- Technical decisions log
- Onboarding guide

### Snippets untuk Reusable Context

Gunakan GitLab Snippets untuk:
- Common code patterns
- Configuration templates
- Script templates

### MR sebagai Context Trail

Setiap MR menjadi bagian dari context trail:
- Linked ke Issue (requirement)
- Referensi ke Spec
- Review comments (knowledge)
- CI/CD results (quality)

## Best Practices

1. **Everything is documented** - Jika tidak didokumentasikan, AI tidak bisa mengaksesnya
2. **Structured format** - Gunakan template yang konsisten
3. **Cross-references** - Link antar dokumen
4. **Version controlled** - Semua context di Git
5. **Accessible** - AI harus bisa membaca semua context
6. **Up to date** - Context yang outdated lebih berbahaya dari tidak ada context

---

## Workflow Integration — Automated Context Accumulation

### Prinsip

Context accumulation BUKAN aktivitas manual. Setiap layer yang menghasilkan artifact HARUS otomatis menyimpannya sebagai context untuk layer berikutnya. AI Agent WAJIB membaca context yang tersedia sebelum mulai bekerja.

---

### 1. Context Chain (Otomatis)

Setiap layer menghasilkan output yang menjadi input layer berikutnya. AI Agent WAJIB mengikuti chain ini:

```
Layer 0: docs/product/vision.md
    ↓ (input untuk Layer 1)
Layer 1: docs/requirements/extracted/*.md
    ↓ (input untuk Layer 2)
Layer 2: docs/requirements/validation/validation-report.md
    ↓ (input untuk Layer 3)
Layer 3: docs/specs/srs/[feature]-spec.md
    ↓ (input untuk Layer 4)
Layer 4: docs/design/[type]/[feature].md
    ↓ (input untuk Layer 8)
Layer 8: GitLab Issues + Branch
    ↓ (input untuk coding)
Code: src/[layer]/[domain]/[file].ts
    ↓ (input untuk Layer 11)
Layer 11: AI Review feedback
    ↓ (input untuk Layer 12)
Layer 12: CI/CD results
    ↓ (input untuk Layer 13)
Layer 13: Production metrics
    ↓ (input untuk Layer 14)
Layer 14: docs/learnings/*.md → feeds back to Layer 3, 6, 5
```

---

### 2. Kiro Hook: Context Loading Before Task

```json
{
  "name": "Load Context Before Task",
  "version": "1.0.0",
  "description": "Otomatis load relevant context sebelum mulai task",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "SEBELUM mulai task, LOAD CONTEXT yang relevan:\n\n**1. Identifikasi domain/feature dari task:**\n- Dari issue title/description, tentukan domain (payment, auth, user, dll)\n- Tentukan feature yang sedang dikerjakan\n\n**2. Load context files (jika ada):**\n- docs/product/vision.md → untuk memahami big picture\n- docs/specs/srs/[feature]-spec.md → untuk memahami requirement\n- docs/design/[type]/[feature].md → untuk memahami design decisions\n- docs/learnings/BUG-*.md yang relevan → untuk menghindari kesalahan yang sama\n- docs/adr/ADR-*.md yang relevan → untuk memahami keputusan arsitektur\n\n**3. Load code context:**\n- src/core/domains/[domain]/ → entities dan interfaces yang sudah ada\n- src/infrastructure/di/registry/ → dependency yang sudah ter-register\n- tests/ yang relevan → pattern testing yang sudah digunakan\n\n**4. Informasikan user context yang di-load:**\n'Context loaded: [list files]. Saya akan mengerjakan task berdasarkan context ini.'\n\n**Rules:**\n- JANGAN mulai coding tanpa membaca spec (jika ada)\n- JANGAN buat entity/interface yang sudah ada\n- JANGAN buat pattern yang bertentangan dengan ADR\n- JANGAN ulangi bug yang sudah di-document di learnings"
  }
}
```

---

### 3. Context Accumulation Rules

AI Agent WAJIB menyimpan output setiap aktivitas sebagai context:

| Aktivitas | Output yang WAJIB Disimpan | Lokasi |
|-----------|---------------------------|--------|
| Requirement intake | Extracted requirements | `docs/requirements/extracted/` |
| Requirement validation | Validation report | `docs/requirements/validation/` |
| Spec creation | Feature spec | `docs/specs/srs/` |
| Design creation | Design document | `docs/design/[type]/` |
| Architecture decision | ADR | `docs/adr/` |
| Bug fix | Bug learning | `docs/learnings/` |
| Sprint completion | Retrospective | `docs/retrospectives/` |
| Incident resolution | Postmortem | `docs/incidents/` |

**Rules:**
- Setiap output HARUS berupa file markdown yang bisa dibaca AI di session berikutnya
- File HARUS di-commit ke repository (bukan hanya lokal)
- Naming convention HARUS konsisten agar mudah dicari
- JANGAN simpan context di chat history saja — harus persist di file

---

### 4. Context Index (Auto-Generated)

AI Agent WAJIB maintain file index yang merangkum semua context tersedia:

```markdown
# Context Index — [Project Name]

**Last Updated:** [date]

## Product
- [x] docs/product/vision.md — Product vision dan roadmap

## Requirements
- [x] docs/requirements/extracted/user-stories.md — 15 user stories
- [x] docs/requirements/validation/report-2026-05-10.md — All validated

## Specifications
- [x] docs/specs/srs/payment-spec.md — Payment feature (Approved)
- [x] docs/specs/srs/auth-spec.md — Authentication (Approved)
- [ ] docs/specs/srs/notification-spec.md — Notifications (Draft)

## Design
- [x] docs/design/system/high-level-architecture.md
- [x] docs/design/technical/payment-flow.md
- [x] docs/design/security/auth-threat-model.md

## Architecture Decisions
- [x] docs/adr/ADR-001-nextjs-app-router.md
- [x] docs/adr/ADR-002-awilix-di.md

## Learnings
- [x] docs/learnings/BUG-012-double-payment.md
- [x] docs/learnings/PERF-001-slow-query.md

## Tech Debt
- [ ] docs/tech-debt/TD-001-missing-validation.md (Open)
```

**Rules:**
- Index di-update setiap kali artifact baru dibuat
- Disimpan di `docs/CONTEXT-INDEX.md`
- AI Agent HARUS baca index ini di awal setiap session/task

---

### 5. Context Freshness

Context yang outdated bisa menyesatkan. AI Agent HARUS:

| Situasi | Action |
|---------|--------|
| Spec berubah setelah coding dimulai | Update spec, informasikan user |
| Design tidak match implementation | Flag sebagai deviasi, update design |
| ADR di-supersede | Mark old ADR sebagai Superseded, buat ADR baru |
| Learning sudah di-address | Mark learning sebagai Resolved |
| Tech debt sudah di-fix | Update status di docs/tech-debt/ |

---

### 6. Integration dengan Layer Lain

| Layer | Bagaimana Layer 10 Terintegrasi |
|-------|--------------------------------|
| Layer 0-4 (Planning) | Output planning → disimpan sebagai context |
| Layer 8 (Issue-Driven) | Issue reference context files yang relevan |
| Layer 11 (AI Review) | Review cek: apakah code sesuai dengan context (spec, design)? |
| Layer 14 (Learning) | Learning output → menjadi context untuk development berikutnya |
| Semua Layer | Context Index menjadi "memory" project yang persist antar session |

---

### 7. AI Agent Rules

1. **SELALU baca context sebelum mulai task** — jangan bekerja tanpa context
2. **SELALU simpan output sebagai file** — chat history hilang, file persist
3. **SELALU update Context Index** setelah membuat artifact baru
4. **JANGAN duplikasi** — cek dulu apakah entity/interface/pattern sudah ada
5. **JANGAN bertentangan dengan ADR** — baca ADR sebelum buat keputusan arsitektur
6. **JANGAN ulangi bug** — baca learnings sebelum implement area yang sama
7. **Context Index adalah entry point** — baca ini dulu untuk tahu apa yang tersedia
