# Layer 8 — Issue-Driven Development

## Prinsip

```
1 issue = 1 bounded context
```

Mengurangi:
- Hallucination
- Context overflow
- Architecture inconsistency

## Workflow

```
Epic
└── Feature
    └── Task
        └── Subtask
            └── Branch
                └── Merge Request
```

## GitLab Issue Hierarchy

### Epic
Representasi dari major feature atau initiative.

```markdown
# Epic: [Title]

## Objective
[Tujuan utama epic ini]

## Scope
- [Feature 1]
- [Feature 2]

## Success Criteria
- [ ] [Criteria 1]
- [ ] [Criteria 2]

## Timeline
- Start: [date]
- Target: [date]

## Related Specs
- docs/specs/prd/user-stories.md#[section]
- docs/specs/srs/[relevant-spec].md
```

### Feature
Bagian dari Epic yang bisa di-deliver independently.

```markdown
# Feature: [Title]

## Parent Epic
[Link ke Epic]

## Description
[Deskripsi fitur]

## User Stories
- US-001: As a [role], I want to [action], so that [benefit]

## Tasks
- [ ] #task-001: [Task title]
- [ ] #task-002: [Task title]

/label ~"type::feature" ~"status::backlog"
/milestone %"Sprint N"
```

### Task
Unit kerja yang bisa diselesaikan dalam 1 session (2-4 jam).

```markdown
# Task: [Title]

## Parent Feature
[Link ke Feature]

## Specification
- API Contract: docs/specs/srs/[spec].md
- Business Rules: docs/specs/brd/business-rules.md#[section]

## Acceptance Criteria
- [ ] AC-001: [Criteria]
- [ ] AC-002: [Criteria]

## Technical Notes
- [Implementation hints, patterns to follow]

## Skills to Use
- .kiro/skills/[relevant-skill].md

## Branch
`feature/task-{number}-{description}`

## Estimated Time
2-4 hours

/label ~"type::task" ~"status::backlog"
/assign @developer
```

## Branch Strategy

```
main (production)
├── develop (integration)
│   ├── feature/issue-{N}-{description}
│   ├── bugfix/issue-{N}-{description}
│   └── spike/issue-{N}-{description}
└── hotfix/issue-{N}-{description}
```

### Naming Convention
```
{type}/issue-{number}-{short-description}

Examples:
feature/issue-12-nfc-registration
bugfix/issue-45-token-expiry
hotfix/issue-99-security-patch
```

> **Untuk detail git init, commit convention, auto-push, dan MR automation**, lihat `git-workflow-automation.md`.

## GitLab Configuration

### Labels

```
# Type
type::epic, type::feature, type::task, type::bug, type::spike

# Status
status::backlog, status::ready, status::in-progress, status::review, status::done

# Priority
priority::critical, priority::high, priority::medium, priority::low

# Team
team::backend, team::frontend, team::platform, team::ai

# Domain (sesuaikan per project)
domain::auth, domain::payment, domain::notification
```

### Milestones = Sprints

```
Sprint 1 (2 weeks)
Sprint 2 (2 weeks)
Sprint 3 (2 weeks)
```

### Issue Templates

Simpan di `.gitlab/issue_templates/`:

```
.gitlab/issue_templates/
├── Feature.md
├── Task.md
├── Bug.md
└── Spike.md
```

> **Untuk isi lengkap templates**, lihat `gitlab-cicd-setup.md` Step 6.

## Merge Request Rules

1. Setiap MR harus linked ke Issue
2. Setiap MR harus punya reviewer
3. CI/CD pipeline harus pass
4. Coverage tidak boleh turun
5. No merge tanpa approval

> **Untuk MR templates dan checklist**, lihat `gitlab-cicd-setup.md` Step 7.

## Workflow dengan Kiro

### Creating Issues dari Spec
```
"Berdasarkan user stories di docs/specs/prd/user-stories.md,
buatkan GitLab Issues dengan hierarchy Epic → Feature → Task.
Gunakan template di .gitlab/issue_templates/"
```

### Working on an Issue
```
"Kerjakan Task #002: Create Login Use Case.
Baca spec di docs/specs/srs/api-contract.md#login.
Gunakan skill create-usecase.
Branch: feature/issue-002-login-usecase"
```

### Creating MR
```
"Buat Merge Request untuk branch feature/issue-002-login-usecase.
Gunakan MR template. Link ke Issue #002."
```

## Kenapa 1 Issue = 1 Bounded Context?

1. **Reduced hallucination** - AI fokus pada 1 context kecil
2. **Better code quality** - Scope kecil = lebih mudah di-review
3. **Easier rollback** - Jika ada masalah, rollback 1 MR saja
4. **Clear ownership** - 1 issue = 1 developer/AI session
5. **Traceable** - Setiap perubahan bisa di-trace ke requirement

## Best Practices

1. **Small issues** - Idealnya 2-4 jam kerja per task
2. **Clear acceptance criteria** - AI tahu kapan "done"
3. **Spec reference** - Setiap issue link ke spec
4. **Branch per issue** - 1 branch = 1 issue = 1 MR
5. **Close via MR** - Issue otomatis close saat MR merged
