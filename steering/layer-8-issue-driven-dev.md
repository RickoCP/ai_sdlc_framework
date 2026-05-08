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
# Epic: User Authentication System

## Objective
Implementasi sistem autentikasi lengkap dengan JWT, refresh token, dan social login.

## Scope
- Login/Register
- Password reset
- Social login (Google, GitHub)
- Session management
- 2FA

## Success Criteria
- [ ] User bisa register dan login
- [ ] Token refresh berjalan otomatis
- [ ] Social login functional
- [ ] 2FA optional untuk user

## Timeline
- Start: [date]
- Target: [date]

## Related Specs
- docs/specs/prd/user-stories.md#authentication
- docs/specs/srs/api-contract.md#auth-endpoints
```

### Feature
Bagian dari Epic yang bisa di-deliver independently.

```markdown
# Feature: JWT Authentication

## Parent Epic
[Link ke Epic]

## Description
Implementasi JWT-based authentication dengan access token dan refresh token.

## User Stories
- US-001: As a user, I want to login with email/password
- US-002: As a user, I want my session to persist
- US-003: As a user, I want to logout

## Tasks
- [ ] #task-001: Create auth controller
- [ ] #task-002: Create login use case
- [ ] #task-003: Create token service
- [ ] #task-004: Create auth middleware
- [ ] #task-005: Write tests

/label ~"feature" ~"authentication"
/milestone %"Sprint 1"
```

### Task
Unit kerja yang bisa diselesaikan dalam 1 session.

```markdown
# Task: Create Login Use Case

## Parent Feature
[Link ke Feature]

## Specification
- API Contract: docs/specs/srs/api-contract.md#login
- Business Rules: docs/specs/brd/business-rules.md#authentication

## Acceptance Criteria
- [ ] Validate email format
- [ ] Check password against hash
- [ ] Generate access token (15min expiry)
- [ ] Generate refresh token (7d expiry)
- [ ] Return tokens in response
- [ ] Handle invalid credentials error
- [ ] Handle locked account error

## Technical Notes
- Use bcrypt for password comparison
- Use jsonwebtoken for token generation
- Follow create-usecase skill pattern

## Branch
`feature/task-002-login-usecase`

## Estimated Time
2-4 hours

/label ~"task" ~"authentication" ~"backend"
/assign @developer
```

## Branch Strategy

```
main (production)
├── develop (integration)
│   ├── feature/task-001-auth-controller
│   ├── feature/task-002-login-usecase
│   ├── feature/task-003-token-service
│   └── bugfix/issue-045-token-expiry
└── hotfix/issue-099-security-patch
```

### Naming Convention
```
{type}/{issue-number}-{short-description}

Examples:
feature/task-001-auth-controller
bugfix/issue-045-token-expiry
hotfix/issue-099-security-patch
```

## Merge Request Flow

### MR Template

```markdown
## Summary
[Ringkasan perubahan]

## Related Issue
Closes #[issue-number]

## Specification Reference
- Spec: [link ke spec]
- Design: [link ke design]

## Changes Made
- [Change 1]
- [Change 2]

## How to Test
1. [Step 1]
2. [Step 2]

## Checklist
- [ ] Code follows project standards
- [ ] Tests written and passing
- [ ] No governance violations
- [ ] Documentation updated
- [ ] Spec compliance verified

## Screenshots (if UI)
[Screenshots]
```

### MR Rules
1. Setiap MR harus linked ke Issue
2. Setiap MR harus punya reviewer
3. CI/CD pipeline harus pass
4. Coverage tidak boleh turun
5. No merge tanpa approval

## GitLab Configuration

### Issue Templates

Buat `.gitlab/issue_templates/`:

```
.gitlab/issue_templates/
├── Feature.md
├── Task.md
├── Bug.md
└── Spike.md
```

### Task Template (`.gitlab/issue_templates/Task.md`)

```markdown
## Task: [Title]

### Parent Feature
<!-- Link ke parent feature issue -->

### Specification
<!-- Link ke relevant spec documents -->
- API Contract: 
- Business Rules: 
- Design: 

### Acceptance Criteria
- [ ] 
- [ ] 

### Technical Notes
<!-- Implementation hints, patterns to follow -->

### Skills to Use
<!-- Reference relevant Kiro skills -->
- .kiro/skills/create-api.md
- .kiro/skills/create-test.md

### Branch Name
`feature/task-{number}-{description}`

### Estimated Time
<!-- 1-4 hours ideal -->

/label ~"task"
```

### Labels

```
# Type
type::epic
type::feature
type::task
type::bug
type::spike

# Status
status::backlog
status::ready
status::in-progress
status::review
status::done

# Priority
priority::critical
priority::high
priority::medium
priority::low

# Team
team::backend
team::frontend
team::platform
team::ai

# Domain
domain::auth
domain::payment
domain::notification
```

### Milestones = Sprints

```
Sprint 1 (2 weeks)
Sprint 2 (2 weeks)
Sprint 3 (2 weeks)
```

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
Branch: feature/task-002-login-usecase"
```

### Creating MR
```
"Buat Merge Request untuk branch feature/task-002-login-usecase.
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
