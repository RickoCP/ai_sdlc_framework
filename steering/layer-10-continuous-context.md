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
