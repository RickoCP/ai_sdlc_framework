# Git Workflow & Commit Convention

## Overview

Panduan lengkap untuk **git init**, **remote setup**, **commit convention**, dan **auto-push** setelah task/sprint selesai. Steering file ini memastikan AI Agent secara otomatis mengelola Git workflow tanpa menunggu instruksi manual dari user.

---

## 1. Commit Convention (Conventional Commits)

Standar commit message mengikuti [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

### Types (WAJIB)

| Type | Deskripsi | Contoh |
|------|-----------|--------|
| `feat` | Fitur baru | `feat(nfc): implement balance top up` |
| `fix` | Bug fix | `fix(gate): handle timeout on scan` |
| `docs` | Dokumentasi saja | `docs(prd): add user story STORY-005` |
| `style` | Formatting (bukan logic) | `style(ui): fix indentation` |
| `refactor` | Refactoring tanpa ubah behavior | `refactor(domain): simplify validator` |
| `test` | Tambah/ubah test | `test(nfc): add encryption test` |
| `chore` | Build, CI, tooling | `chore(deps): update vitest to 2.x` |
| `perf` | Performance improvement | `perf(codec): optimize binary encoding` |
| `ci` | CI/CD pipeline changes | `ci(gitlab): add security scan stage` |
| `build` | Build system changes | `build: update tsconfig paths` |
| `revert` | Revert commit sebelumnya | `revert: feat(nfc): implement top up` |

### Scope (RECOMMENDED)

Scope menunjukkan area yang terpengaruh:

| Category | Scopes |
|----------|--------|
| Domain | `nfc`, `gate`, `terminal`, `scout`, `station`, `card`, `member`, `transaction` |
| Layer | `domain`, `application`, `infrastructure`, `presentation`, `shared` |
| Docs | `prd`, `brd`, `srs`, `adr`, `design`, `governance`, `specs` |
| Tooling | `ci`, `deps`, `config`, `docker`, `gitlab`, `pipeline` |

### Subject Rules

1. **Lowercase** — Jangan capitalize huruf pertama
2. **No period** — Jangan akhiri dengan titik
3. **Imperative mood** — "add" bukan "added" atau "adds"
4. **Max 50 characters** — Singkat dan jelas
5. **Describe what, not how** — "add validation" bukan "add if-else check"

### ✅ Good
```
feat(nfc): implement card registration
fix(terminal): handle insufficient balance
test(gate): add simulation mode tests
```

### ❌ Bad
```
feat(nfc): Implemented card registration.
fix: fixed the bug
update code
WIP
```

### Body (OPTIONAL)

Gunakan body untuk menjelaskan **why** (bukan what):

```
fix(gate): prevent double tap detection

Previous implementation allowed rapid consecutive taps
which caused duplicate check-in entries. Added 3-second
cooldown between scans using timestamp comparison.

Closes #45
```

### Footer (OPTIONAL)

```
# Breaking Changes
feat(api): change response format for balance endpoint

BREAKING CHANGE: Balance response now returns amount in cents
instead of decimal. All clients must update parsing logic.

# Issue References
feat(nfc): implement card registration

Closes #12
Refs #10, #11
Covers: AC-001, AC-002, AC-003
```

### Commit per Task Pattern

```
Issue #12: NFC Card Registration
├── feat(nfc): implement card reader service          (TASK-001)
├── feat(nfc): add registration form component        (TASK-002)
├── feat(nfc): implement card data encryption         (TASK-003)
├── test(nfc): add registration unit tests            (TASK-004)
└── docs(srs): update NFC registration sequence       (TASK-005)
```

Rules:
- 1 Task = 1 commit (ideal)
- Jika task besar, boleh split menjadi beberapa commit
- Setiap commit harus atomic (bisa revert tanpa break)
- Jangan gabung multiple tasks dalam 1 commit

### AI Agent Commit Rules

1. **SELALU gunakan conventional commit format**
2. **SELALU sertakan scope yang relevan**
3. **SELALU referensikan issue number di footer** (jika ada)
4. **JANGAN commit file yang tidak terkait dengan task**
5. **JANGAN gunakan generic message** seperti "update files" atau "fix stuff"
6. **JANGAN commit secrets, credentials, atau .env files**

Template:
```
<type>(<scope>): <what was done>

<why it was done - 1-2 sentences>

Refs #<issue-number>
Covers: <AC-IDs>
```

### Commit Lint (CI/CD Validation)

```yaml
# .gitlab/ci/lint.yml
commitlint:
  stage: lint
  image: node:20-alpine
  script:
    - npm install --save-dev @commitlint/cli @commitlint/config-conventional
    - echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
    - npx commitlint --from $CI_MERGE_REQUEST_DIFF_BASE_SHA --to HEAD
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

```javascript
// commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [2, 'always', [
      'feat', 'fix', 'docs', 'style', 'refactor',
      'test', 'chore', 'perf', 'ci', 'build', 'revert'
    ]],
    'scope-case': [2, 'always', 'kebab-case'],
    'subject-case': [2, 'always', 'lower-case'],
    'subject-max-length': [2, 'always', 72],
    'body-max-line-length': [2, 'always', 100],
  }
};
```

---

## 2. Project Initialization (WAJIB di Awal)

### Kapan Dilakukan

AI Agent WAJIB melakukan git initialization saat:
- User konfirmasi membuat project baru
- Project belum memiliki `.git/` directory
- User minta setup repository

### Flow Initialization

```
User konfirmasi project baru
    ↓
[1] git init (lokal)
    ↓
[2] Buat .gitignore
    ↓
[3] Buat struktur folder framework
    ↓
[4] Initial commit: "chore: initial project setup"
    ↓
[5] Create GitLab project (via GitLab MCP)
    ↓
[6] git remote add origin <gitlab-url>
    ↓
[7] git push -u origin main
    ↓
[8] Buat branch develop dari main
    ↓
[9] git push -u origin develop
    ↓
[10] Setup branch protection (via GitLab MCP)
```

### Implementasi dengan MCP

```
# Step 1: Git Init
Tool: git_init
Arguments: { "path": "<project-path>" }

# Step 2-4: Buat files + Initial Commit
Tool: git_commit
Arguments: {
  "repo_path": "<project-path>",
  "message": "chore: initial project setup with enterprise-ai-sdlc framework"
}

# Step 5: Create GitLab Project
Tool: create_project (GitLab MCP)
Arguments: {
  "name": "<project-name>",
  "description": "<project-description>",
  "visibility": "private",
  "initialize_with_readme": false
}

# Step 6-7: Add Remote + Push
Tool: git_remote (jika tersedia) atau shell command
Command: git remote add origin <gitlab-url>

Tool: git_push
Arguments: {
  "repo_path": "<project-path>",
  "remote": "origin",
  "branch": "main"
}

# Step 8-9: Create develop branch
Tool: git_branch
Arguments: {
  "repo_path": "<project-path>",
  "branch_name": "develop"
}

Tool: git_push
Arguments: {
  "repo_path": "<project-path>",
  "remote": "origin",
  "branch": "develop"
}
```

### .gitignore Template (Node.js/Next.js)

```gitignore
# Dependencies
node_modules/
.pnp
.pnp.js

# Build
.next/
out/
dist/
build/

# Environment
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Testing
coverage/
.nyc_output/

# Debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Misc
*.tsbuildinfo
```

---

## 3. Branch Strategy

> **Untuk detail issue hierarchy (Epic → Feature → Task) dan labels**, lihat `layer-8-issue-driven-dev.md`.

### Quick Reference

```
main (production-ready)
├── develop (integration branch)
│   ├── feature/issue-{N}-{description}
│   ├── bugfix/issue-{N}-{description}
│   └── spike/issue-{N}-{description}
└── hotfix/issue-{N}-{description}
```

### Branch Creation per Issue (AI Agent WAJIB)

```
1. git checkout develop
2. git pull origin develop
3. git checkout -b feature/issue-{N}-{description}
4. Kerjakan task
5. Commit (conventional)
6. Push branch
7. Create MR
```

---

## 4. Auto-Push Rules (WAJIB)

### Kapan AI Agent WAJIB Push

| Trigger | Action | Branch Target |
|---------|--------|---------------|
| Task selesai (1 issue done) | Commit + Push | feature branch |
| Sprint selesai (semua issue done) | Push + Create MR | feature → develop |
| Hotfix selesai | Push + Create MR | hotfix → main |
| Documentation update | Commit + Push | develop |
| Initial setup selesai | Push | main + develop |

### Flow: Setelah Task Selesai

```
[Task Implementation Done]
    ↓
[AI Validation Pass]
    ↓
[Unit Tests Pass]
    ↓
git add <relevant-files>
    ↓
git commit -m "<conventional-commit-message>"
    ↓
git push origin <feature-branch>
    ↓
Informasikan user: "Task selesai. Code sudah di-push ke branch feature/issue-{N}."
```

### Flow: Setelah Sprint Selesai

```
[Semua Tasks dalam Sprint Done]
    ↓
[Coverage Check Pass]
    ↓
[AI Review Pass]
    ↓
git push origin <feature-branch> (jika belum)
    ↓
Create Merge Request (via GitLab MCP):
  - Source: feature branch / develop
  - Target: main (atau develop jika dari feature)
  - Title: "feat: [Sprint N] - [Feature Name]"
  - Description: (include AC coverage, test results)
  - Labels: sprint-N, ready-for-review
    ↓
Informasikan user: "Sprint selesai. MR sudah dibuat: [MR URL]"
    ↓
Tunggu human approval untuk merge
```

### Sprint Commit Flow

```
Sprint Start
    ↓
[Branch: feature/issue-12-nfc-registration]
    ↓
feat(nfc): implement card reader service
feat(nfc): add registration form
feat(nfc): implement encryption
test(nfc): add unit tests
    ↓
Push to branch ← AUTO
    ↓
Create MR ← AUTO
    ↓
Human review + approve
    ↓
Squash merge to develop
    ↓
Sprint End
```

### Implementasi Push dengan MCP

```
# Commit
Tool: git_commit
Arguments: {
  "repo_path": "<project-path>",
  "message": "feat(nfc): implement card registration\n\nCloses #12\nCovers: AC-001, AC-002, AC-003"
}

# Push
Tool: git_push
Arguments: {
  "repo_path": "<project-path>",
  "remote": "origin",
  "branch": "feature/issue-12-nfc-registration"
}

# Create MR (setelah sprint selesai)
Tool: create_merge_request (GitLab MCP)
Arguments: {
  "project_id": "<project-id>",
  "title": "feat: Sprint 1 - NFC Station Features",
  "source_branch": "feature/issue-12-nfc-registration",
  "target_branch": "develop",
  "description": "## Summary\n...\n\nCloses #12, #13, #14",
  "labels": "sprint-1,ready-for-review"
}
```

---

## 5. Pre-Push Checklist (AI Agent WAJIB Cek)

Sebelum push, AI Agent WAJIB memastikan:

### CI-Readiness (Push Pertama)
- [ ] `.eslintrc.json` atau `eslint.config.mjs` ada (WAJIB — tanpa ini lint stage GAGAL)
- [ ] `tsconfig.json` ada
- [ ] `.prettierrc` ada
- [ ] Test config (`vitest.config.ts`) ada
- [ ] `package.json` punya scripts: `lint`, `build`, `typecheck`, `test`
- [ ] `npm run lint` berjalan TANPA prompt interaktif
- [ ] `npm audit` tidak ada critical vulnerability

### Code Quality
- [ ] Tidak ada linting errors
- [ ] Tidak ada TypeScript errors
- [ ] Conventional commit message digunakan

### Security
- [ ] Tidak ada secrets/credentials di code
- [ ] .env TIDAK ter-commit
- [ ] Tidak ada hardcoded tokens

### Testing
- [ ] Unit tests passing
- [ ] Coverage tidak turun
- [ ] Tidak ada skipped tests tanpa alasan

### Files
- [ ] Hanya file yang relevan dengan task yang di-commit
- [ ] Tidak ada file temporary/debug
- [ ] .gitignore sudah benar

### Commit
- [ ] Message mengikuti conventional commit
- [ ] Scope sesuai dengan area perubahan
- [ ] Issue reference ada di footer

---

## 6. Sprint Completion Workflow

### Setelah Sprint Selesai, AI Agent WAJIB:

```
1. ✅ Pastikan semua task ter-commit dan ter-push
2. ✅ Jalankan full test suite
3. ✅ Generate coverage report
4. ✅ Generate AI validation report
5. ✅ Generate requirement traceability check
6. ✅ Push semua perubahan ke feature branch
7. ✅ Create Merge Request dengan:
   - Summary of changes
   - AC coverage table
   - Test results
   - Sprint reference
8. ✅ Assign reviewer (jika dikonfigurasi)
9. ✅ Update semua GitLab issues: status::review
10. ✅ Update milestone: sprint summary + close/carry-over
11. ✅ Update wiki: Changelog page
12. ✅ Informasikan user bahwa sprint deliverable sudah ready
```

### MR Description Template untuk Sprint

```markdown
## Sprint [N] Delivery: [Feature Name]

### Changes
- [List perubahan utama]

### Acceptance Criteria Coverage
| AC | Description | Status | Test |
|----|-------------|--------|------|
| AC-001 | [desc] | ✅ | test-file.test.ts |
| AC-002 | [desc] | ✅ | test-file.test.ts |

### Test Results
- Total tests: X
- Passing: X
- Coverage: X%

### Sprint Issues
- Closes #12
- Closes #13
- Closes #14

### AI Validation
- Architecture compliance: ✅
- Security check: ✅
- Governance compliance: ✅

### Ready for Review
- [ ] Code review
- [ ] Business logic review
- [ ] Security review
```

---

## 7. GitLab Project Management (WAJIB)

Selain git operations, AI Agent WAJIB mengelola work items di GitLab agar board, milestones, dan wiki selalu up-to-date.

> **Prerequisite:** Jalankan `discover_tools(category: "milestones")` dan `discover_tools(category: "wiki")` untuk mengaktifkan tools tambahan di awal session.

### 7.1 Work Items & Issue Board

AI Agent WAJIB update issue status di setiap tahap:

| Tahap | Label Update | Action |
|-------|-------------|--------|
| Task mulai dikerjakan | `status::in-progress` | `update_issue` → ubah labels |
| Task selesai + pushed | `status::review` | `update_issue` → ubah labels + tambah comment |
| MR merged | `status::done` (otomatis via `Closes #N`) | — |
| Task blocked | `status::blocked` | `update_issue` + `create_issue_note` (alasan) |
| Bug ditemukan saat review | Buat issue baru `type::bug` | `create_issue` |

#### Implementasi

```
# Saat mulai task
Tool: update_issue (GitLab MCP)
Arguments: {
  "project_id": "<project-id>",
  "issue_iid": "12",
  "labels": ["type::task", "status::in-progress", "domain::nfc"]
}

# Saat task selesai + pushed
Tool: update_issue
Arguments: {
  "project_id": "<project-id>",
  "issue_iid": "12",
  "labels": ["type::task", "status::review", "domain::nfc"]
}

Tool: create_issue_note
Arguments: {
  "project_id": "<project-id>",
  "noteable_type": "issue",
  "noteable_iid": "12",
  "body": "✅ Implementation complete.\n- Branch: `feature/issue-12-nfc-registration`\n- MR: !5\n- Tests: 8/8 passing\n- Coverage: 85%"
}
```

#### Issue Board Columns (Mapping ke Labels)

```
Backlog → Ready → In Progress → Review → Done
   ↓         ↓         ↓           ↓        ↓
status::   status::  status::    status::  status::
backlog    ready     in-progress  review    done
```

Agent WAJIB memindahkan issue antar kolom dengan mengubah label `status::*`.

### 7.2 Milestones (Sprint Management)

#### Saat Sprint Planning

```
# Buat milestone untuk sprint baru
Tool: discover_tools → category: "milestones"
Tool: create_milestone (setelah discover)
Arguments: {
  "project_id": "<project-id>",
  "title": "Sprint 2",
  "description": "NFC Gate & Terminal Features",
  "due_date": "2026-06-06",
  "start_date": "2026-05-23"
}

# Assign issues ke milestone
Tool: update_issue
Arguments: {
  "project_id": "<project-id>",
  "issue_iid": "15",
  "milestone_id": "<milestone-id>"
}
```

#### Saat Sprint Selesai

```
1. List semua issues di milestone → cek status
2. Generate sprint summary:
   - Total issues: X
   - Done: X
   - Carry-over: X
   - Coverage achieved: X%
3. Jika semua done → close milestone
4. Jika ada carry-over:
   - Buat milestone baru (Sprint N+1)
   - Move carry-over issues ke milestone baru
   - Tambah note di issue: "Carried over from Sprint N"
5. Tambah comment di milestone: sprint summary
```

#### Sprint Summary Template (sebagai milestone description update)

```markdown
## Sprint [N] Summary

### Delivered
- ✅ #12: NFC Card Registration
- ✅ #13: Balance Top Up
- ✅ #14: Card Encryption

### Carry-over
- 🔄 #15: Transaction Logs (70% done, moved to Sprint N+1)

### Metrics
- Velocity: 21 story points
- Coverage: 85%
- Pipeline success rate: 100%
- Issues closed: 3/4 (75%)

### Notes
- [Learnings atau blockers]
```

### 7.3 Wiki (Project Documentation)

#### Kapan Update Wiki

| Trigger | Wiki Page | Konten |
|---------|-----------|--------|
| Initial setup | `Home` | Project overview, tech stack, links |
| Layer 4 selesai | `Architecture` | High-level architecture, layer structure |
| Sprint selesai | `Changelog` | Sprint changelog (issues closed, features delivered) |
| Endpoint baru | `API-Documentation` | Endpoint list, request/response format |
| ADR dibuat | `Architecture-Decisions` | Link ke ADR files |
| Onboarding | `Getting-Started` | Setup guide, dev workflow |

#### Implementasi

```
# Discover wiki tools
Tool: discover_tools
Arguments: { "category": "wiki" }

# Create/Update wiki page
Tool: create_or_update_wiki_page (setelah discover)
Arguments: {
  "project_id": "<project-id>",
  "title": "Changelog",
  "content": "# Changelog\n\n## Sprint 1 (2026-05-09 → 2026-05-23)\n\n### Features\n- NFC Card Registration (#12)\n- Balance Top Up (#13)\n...",
  "format": "markdown"
}
```

#### Wiki Structure yang Direkomendasikan

```
Wiki Pages:
├── Home                    # Project overview + quick links
├── Getting-Started         # Dev setup, prerequisites, workflow
├── Architecture            # High-level architecture, C4 diagrams
├── Architecture-Decisions  # ADR index + summaries
├── API-Documentation       # Endpoints, contracts
├── Changelog               # Per-sprint changelog
├── Security                # Security model overview
└── Runbook                 # Operational procedures
```

#### Wiki Update Rules

1. **Home** — Update saat project info berubah (tech stack, team, links)
2. **Changelog** — WAJIB update setiap sprint selesai
3. **Architecture** — Update saat ada perubahan design signifikan (ADR baru)
4. **API-Documentation** — Update saat endpoint baru dibuat atau berubah
5. **Getting-Started** — Update saat dev workflow berubah

### 7.4 Complete Flow: Task → Sprint → GitLab Update

```
[Task Mulai]
    ├── update_issue: status::in-progress
    │
[Task Selesai]
    ├── git commit + push
    ├── update_issue: status::review
    ├── create_issue_note: implementation summary
    │
[Sprint Selesai]
    ├── Create MR
    ├── Update semua issues: status::review
    ├── Generate sprint summary
    │
[MR Merged]
    ├── Issues auto-closed (via "Closes #N")
    ├── Update milestone: close atau carry-over
    ├── Update wiki: Changelog page
    ├── Informasikan user
    │
[Sprint Baru]
    ├── Create milestone baru
    ├── Assign carry-over issues
    ├── Update wiki: Home (jika perlu)
```

---

## 8. Kiro Hooks

### Hook: Auto-Push After Task

```json
{
  "name": "Auto Push After Task Completion",
  "version": "1.0.0",
  "description": "Mengingatkan agent untuk commit, push, dan update GitLab setelah menyelesaikan task",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Task telah selesai. Lakukan langkah berikut secara OTOMATIS:\n\n**Git Operations:**\n1. Cek git status untuk melihat perubahan\n2. Stage HANYA file yang relevan dengan task ini (jangan git add .)\n3. Commit dengan conventional commit format: <type>(<scope>): <subject>\n4. Push ke current feature branch\n5. Jika ini task terakhir dalam sprint, buat Merge Request ke develop\n\n**GitLab Project Management:**\n6. Update issue status: ubah label dari status::in-progress ke status::review\n7. Tambah comment di issue: summary implementasi + branch + test results\n8. Jika sprint selesai: generate sprint summary, update milestone, update wiki Changelog\n\n**Tools:**\n- Git: git_status, git_commit, git_push\n- GitLab: update_issue, create_issue_note, create_merge_request\n- Discover: milestones, wiki (jika perlu update)\n\n**JANGAN:** commit .env, node_modules, secrets, temporary files.\n**SELALU:** sertakan issue reference di commit footer, update issue board status."
  }
}
```

### Hook: Git Init Check Before Task

```json
{
  "name": "Git Init Reminder",
  "version": "1.0.0",
  "description": "Mengingatkan agent untuk setup git jika project belum memiliki repository",
  "when": {
    "type": "preTaskExecution"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Sebelum mulai task, cek apakah project sudah memiliki git repository:\n1. Cek apakah .git/ directory ada\n2. Jika BELUM ada: jalankan git init, buat .gitignore, initial commit, create GitLab project, add remote, push\n3. Jika SUDAH ada: cek apakah remote origin sudah di-set. Jika belum, tanyakan user GitLab project URL\n4. Pastikan branch strategy sudah benar (main + develop)\n\nGunakan Git MCP dan GitLab MCP tools."
  }
}
```

---

## 9. Complete Git Flow Diagram

```
[Project Start]
    │
    ├── git init
    ├── Create GitLab project
    ├── Add remote origin
    ├── Initial commit + push to main
    ├── Create develop branch + push
    │
[Sprint Planning]
    │
    ├── Create feature branch from develop
    │
[Task Execution Loop]
    │
    ├── Implement task
    ├── AI validation
    ├── Unit test
    ├── Commit (conventional)
    ├── Push to feature branch ← AUTO
    │
[Sprint End]
    │
    ├── Final push
    ├── Create MR (feature → develop) ← AUTO
    ├── Human review + approve
    ├── Merge to develop
    │
[Release]
    │
    ├── Create MR (develop → main)
    ├── Final approval
    ├── Merge to main
    ├── Tag release
    └── Deploy
```

---

## 10. Error Handling

### Push Gagal: Authentication
```
Error: Authentication failed
→ Cek GITLAB_PERSONAL_ACCESS_TOKEN sudah di-set di environment
→ Cek token belum expired
→ Cek token punya scope: write_repository
→ Informasikan user untuk update token
```

### Push Gagal: Remote Not Found
```
Error: Remote 'origin' not found
→ Jalankan: git remote add origin <gitlab-url>
→ Retry push
→ Jika masih gagal, cek GitLab project sudah dibuat
```

### Push Gagal: Conflict
```
Error: Rejected (non-fast-forward)
→ git pull --rebase origin <branch>
→ Resolve conflicts jika ada
→ Retry push
→ Informasikan user jika conflict complex
```

### Push Gagal: Branch Protection
```
Error: Protected branch
→ Jangan push langsung ke main/develop
→ Buat feature branch
→ Push ke feature branch
→ Create MR
```

### MCP Server Tidak Tersedia

Jika Git MCP atau GitLab MCP tidak aktif, AI Agent HARUS:
1. Informasikan user bahwa MCP server perlu diaktifkan
2. Berikan instruksi manual git commands sebagai fallback
3. Jangan skip push — tetap lakukan via shell command jika MCP tidak tersedia

Fallback shell commands:
```bash
# Init
git init
git remote add origin https://gitlab.com/<namespace>/<project>.git

# Commit + Push
git add <files>
git commit -m "feat(scope): description"
git push -u origin feature/issue-N-description

# Create MR (via GitLab API)
curl --request POST \
  --header "PRIVATE-TOKEN: $GITLAB_PERSONAL_ACCESS_TOKEN" \
  --data "source_branch=feature/issue-N&target_branch=develop&title=feat: Sprint N delivery" \
  "$GITLAB_API_URL/projects/$PROJECT_ID/merge_requests"
```

---

## 11. Setup Environment Variables (.env)

### Kenapa Perlu .env

MCP servers (GitLab & Git) membutuhkan credentials. Credentials dibaca dari **system environment variables**. Gunakan `.env` file di root project sebagai referensi.

### .env Template

```env
# GitLab Configuration
GITLAB_PERSONAL_ACCESS_TOKEN=glpat-xxxxxxxxxxxxxxxxxxxx
GITLAB_API_URL=https://gitlab.com/api/v4

# Project Info (optional)
GITLAB_PROJECT_ID=12345678
GITLAB_NAMESPACE=your-username-or-group
```

### Cara Mendapatkan Token

1. Buka GitLab → User Settings → Access Tokens
2. Buat token: Name `kiro-power-sdlc`, Scopes: `api`, `read_repository`, `write_repository`
3. Copy token → paste ke `.env`

### Load .env ke System Environment

MCP config membaca dari system environment (`${VARIABLE_NAME}`), jadi perlu di-load:

**Windows (Permanent):**
```powershell
[Environment]::SetEnvironmentVariable("GITLAB_PERSONAL_ACCESS_TOKEN", "glpat-xxx", "User")
[Environment]::SetEnvironmentVariable("GITLAB_API_URL", "https://gitlab.com/api/v4", "User")
```

**Windows (Per-session dari .env):**
```powershell
Get-Content .env | ForEach-Object {
  if ($_ -match '^([^#][^=]+)=(.*)$') {
    [Environment]::SetEnvironmentVariable($matches[1], $matches[2], "Process")
  }
}
```

**Linux/Mac:**
```bash
# Di ~/.bashrc atau ~/.zshrc
export GITLAB_PERSONAL_ACCESS_TOKEN="glpat-xxxxxxxxxxxx"
export GITLAB_API_URL="https://gitlab.com/api/v4"

# Atau load dari .env
set -a; source .env; set +a
```

### PENTING

- `.env` WAJIB ada di `.gitignore`
- AI Agent JANGAN pernah commit `.env`
- AI Agent JANGAN baca/tampilkan isi `.env` di output
- Restart Kiro setelah set environment variables
- **Approve env vars di Kiro Settings** → cari "Mcp Approved Env Vars" → tambahkan `GITLAB_PERSONAL_ACCESS_TOKEN` dan `GITLAB_API_URL`

---

## Integration dengan Framework

| Layer | Relevansi |
|-------|-----------|
| Layer 5 (Governance) | Commit convention + git workflow sebagai standard |
| Layer 8 (Issue-Driven) | 1 issue → 1 branch → conventional commits → auto-push |
| Layer 9 (Orchestration) | DevOps Agent handles git operations |
| Layer 11 (AI Review) | AI validate commit message format |
| Layer 12 (Quality Gates) | commitlint + push triggers CI/CD pipeline |
