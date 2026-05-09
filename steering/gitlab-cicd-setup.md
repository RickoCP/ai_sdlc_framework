# GitLab CI/CD Setup Guide

## Overview

Panduan setup **infrastruktur** GitLab CI/CD. File ini fokus pada konfigurasi GitLab (settings, runner, templates), bukan pipeline stages.

> **Untuk pipeline stages dan quality gate thresholds**, lihat `layer-12-quality-gates.md`.
> **Untuk branch strategy dan commit convention**, lihat `git-workflow-automation.md`.
> **Untuk issue hierarchy dan labels**, lihat `layer-8-issue-driven-dev.md`.

## Prerequisites

- GitLab account (self-hosted atau gitlab.com)
- GitLab Runner (shared atau dedicated)
- Docker installed pada runner
- Node.js project (atau sesuaikan untuk bahasa lain)

---

## Step 1: Repository Settings

### Enable CI/CD
```
Settings → CI/CD → General pipelines → Enable
```

### Setup Container Registry
```
Settings → Packages & Registries → Container Registry → Enable
```

### Setup Variables
```
Settings → CI/CD → Variables:

# Required
- CI_REGISTRY_USER: $CI_REGISTRY_USER (predefined)
- CI_REGISTRY_PASSWORD: $CI_REGISTRY_PASSWORD (predefined)

# Application
- NODE_ENV: production (protected, production only)
- DATABASE_URL: (protected, masked)
- REDIS_URL: (protected, masked)

# Vercel Deployment (WAJIB)
- VERCEL_TOKEN: (protected, masked) — dari https://vercel.com/account/tokens
- VERCEL_ORG_ID: (protected) — dari .vercel/project.json
- VERCEL_PROJECT_ID: (protected) — dari .vercel/project.json

# Monitoring
- SENTRY_DSN: (protected, masked)
- SENTRY_AUTH_TOKEN: (protected, masked)
```

---

## Step 2: Branch Protection

### Protected Branches
```
Settings → Repository → Protected Branches:

main:
  - Allowed to merge: Maintainers
  - Allowed to push: No one
  - Require approval: Yes (1 minimum)

develop:
  - Allowed to merge: Developers + Maintainers
  - Allowed to push: No one
  - Require approval: Yes (1 minimum)
```

### Merge Request Settings
```
Settings → Merge Requests:
  - Merge method: Squash merge
  - Merge checks:
    - Pipeline must succeed: Yes
    - All discussions resolved: Yes
  - Approval rules:
    - Minimum approvals: 1
    - Code owners: Required
```

---

## Step 3: File Structure

```
.gitlab/
├── ci/
│   ├── lint.yml
│   ├── typecheck.yml
│   ├── test.yml
│   ├── security.yml
│   ├── build.yml
│   └── deploy.yml
├── issue_templates/
│   ├── Feature.md
│   ├── Task.md
│   ├── Bug.md
│   └── Spike.md
├── merge_request_templates/
│   ├── Default.md
│   ├── Feature.md
│   └── Hotfix.md
└── CODEOWNERS

.gitlab-ci.yml
```

---

## Step 4: CODEOWNERS

### `.gitlab/CODEOWNERS`

```
# Global owners
* @tech-lead

# Architecture
docs/design/ @architect-team
docs/specs/srs/ @architect-team

# Security
docs/design/security/ @security-team
.gitlab/ci/security.yml @security-team

# CI/CD
.gitlab-ci.yml @devops-team
.gitlab/ci/ @devops-team

# Frontend
src/presentation/ @frontend-team

# Backend
src/domain/ @backend-team
src/application/ @backend-team
src/infrastructure/ @backend-team

# Governance
docs/governance/ @tech-lead @security-team
```

---

## Step 5: Issue Templates

### `.gitlab/issue_templates/Feature.md`

```markdown
## Feature: [Title]

### Epic
<!-- Link ke parent epic -->

### Description
<!-- Deskripsi fitur -->

### User Stories
- As a [role], I want to [action], so that [benefit]

### Acceptance Criteria
- [ ] 
- [ ] 

### Technical Considerations
<!-- Architecture, security, performance notes -->

### Specification References
- PRD: 
- SRS: 
- Design: 

### Tasks
- [ ] Task 1
- [ ] Task 2

/label ~"type::feature" ~"status::backlog"
```

### `.gitlab/issue_templates/Task.md`

```markdown
## Task: [Title]

### Parent Feature
<!-- Link ke parent feature -->

### Specification
- API Contract: 
- Business Rules: 
- Design: 

### Acceptance Criteria
- [ ] 
- [ ] 

### Technical Notes
<!-- Implementation hints -->

### Skills to Use
<!-- Kiro skills reference -->

### Branch Name
`feature/issue-{number}-{description}`

### Estimated Time
<!-- 1-4 hours ideal -->

/label ~"type::task" ~"status::backlog"
```

### `.gitlab/issue_templates/Bug.md`

```markdown
## Bug: [Title]

### Environment
- Browser/OS: 
- Version: 
- Environment: [dev/staging/production]

### Steps to Reproduce
1. 
2. 
3. 

### Expected Behavior
<!-- Apa yang seharusnya terjadi -->

### Actual Behavior
<!-- Apa yang terjadi -->

### Screenshots/Logs
<!-- Attach evidence -->

### Severity
<!-- Critical/High/Medium/Low -->

### Root Cause (to be filled after investigation)

### Learning (to be filled after fix)

/label ~"type::bug" ~"status::backlog"
```

---

## Step 6: Merge Request Templates

### `.gitlab/merge_request_templates/Default.md`

```markdown
## Summary
<!-- Ringkasan perubahan -->

## Related Issue
Closes #

## Specification Reference
- Spec: 
- Design: 

## Changes Made
- 

## How to Test
1. 
2. 

## Checklist

### Code Quality
- [ ] Follows project coding standards
- [ ] No linting errors
- [ ] Type-safe (no `any`)
- [ ] Proper error handling

### Architecture
- [ ] Clean Architecture layers respected
- [ ] No dependency rule violations
- [ ] Proper use of interfaces

### Security
- [ ] Input validation present
- [ ] No hardcoded credentials
- [ ] Proper authorization checks

### Testing
- [ ] Unit tests added/updated
- [ ] Coverage maintained >= 80%

### Documentation
- [ ] Code comments where needed
- [ ] API docs updated (if applicable)
- [ ] ADR created (if architecture decision)

## Screenshots (if UI changes)
```

---

## Step 7: GitLab Runner Configuration

### Shared Runner (gitlab.com)
Sudah tersedia, tidak perlu setup tambahan.

### Self-Hosted Runner

```bash
# Install GitLab Runner
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt-get install gitlab-runner

# Register runner
sudo gitlab-runner register \
  --url "https://gitlab.example.com/" \
  --registration-token "PROJECT_REGISTRATION_TOKEN" \
  --executor "docker" \
  --docker-image "node:20-alpine" \
  --description "project-runner" \
  --tag-list "docker,node" \
  --run-untagged="true"
```

### Runner Configuration (`/etc/gitlab-runner/config.toml`)

```toml
[[runners]]
  name = "project-runner"
  executor = "docker"
  [runners.docker]
    image = "node:20-alpine"
    privileged = true
    volumes = ["/cache", "/certs/client"]
    shm_size = 268435456
  [runners.cache]
    Type = "s3"
    Shared = true
```

---

## Step 8: Scheduled Pipelines

### Weekly Metrics Collection
```
CI/CD → Schedules → New Schedule:
- Description: Weekly Metrics
- Interval: 0 9 * * 1 (Monday 9 AM)
- Target branch: main
- Variables: SCHEDULE_TYPE=weekly-metrics
```

### Daily Security Scan
```
CI/CD → Schedules → New Schedule:
- Description: Daily Security Scan
- Interval: 0 2 * * * (Daily 2 AM)
- Target branch: main
- Variables: SCHEDULE_TYPE=security-scan
```

---

## Step 9: Notifications

### GitLab Integrations
```
Settings → Integrations:
- Slack: Pipeline failures, MR events
- Email: Critical pipeline failures
```

### Notification Job (tambahkan di pipeline)
```yaml
notify-failure:
  stage: .post
  script:
    - |
      curl -X POST "$SLACK_WEBHOOK" \
        -H 'Content-type: application/json' \
        -d "{\"text\": \"❌ Pipeline failed on $CI_COMMIT_BRANCH by $GITLAB_USER_NAME\"}"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: on_failure
```

---

## Troubleshooting

### Pipeline Stuck
- Check runner availability: `Settings → CI/CD → Runners`
- Check runner tags match job tags
- Check runner disk space

### Cache Issues
- Clear cache: `CI/CD → Pipelines → Clear Runner Caches`
- Verify cache key matches

### Docker-in-Docker Issues
- Ensure runner has `privileged: true`
- Check Docker socket permissions
- Verify TLS certificates

### Slow Pipelines
- Enable `FF_USE_FASTZIP`
- Use `--prefer-offline` for npm
- Parallelize independent jobs
- Use smaller Docker images (alpine)

---

## Best Practices

1. **Keep pipeline fast** - Target < 10 minutes for MR pipeline
2. **Use includes** - Split CI config into manageable files
3. **Cache everything** - node_modules, Docker layers, build artifacts
4. **Fail fast** - Cheap checks first
5. **Secure variables** - Use protected + masked for secrets
6. **Review pipeline regularly** - Optimize based on metrics
7. **Document pipeline** - Team should understand each stage
