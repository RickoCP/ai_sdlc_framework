# Layer 9 — AI Agent Orchestration

## Multi-Agent Workflow

```
BA Agent (Requirement Intake)
    ↓
Architect Agent (Design & Architecture)
    ↓
Security Agent (Threat Modeling & Review)
    ↓
Frontend Agent (UI Implementation)
    ↓
Backend Agent (API & Business Logic)
    ↓
QA Agent (Testing & Validation)
    ↓
DevOps Agent (CI/CD & Deployment)
```

## Agent Roles

### 1. Business Analyst Agent
**Responsibility:**
- Requirement extraction dari raw input
- Stakeholder mapping
- Feature identification
- Business process extraction

**Input:** Raw documents, meeting notes, emails
**Output:** Structured requirements di `docs/requirements/`

**Kiro Implementation:**
```
"Bertindak sebagai Business Analyst Agent.
Baca dokumen berikut dan ekstrak requirements.
Format output sesuai template di docs/requirements/intake/extracted/"
```

### 2. Architect Agent
**Responsibility:**
- System design
- Architecture decisions
- Technology selection
- Integration patterns

**Input:** Validated requirements, existing architecture
**Output:** Design documents di `docs/design/`

**Kiro Implementation:**
```
"Bertindak sebagai Architect Agent.
Berdasarkan requirements di docs/requirements/ dan existing architecture,
design solusi untuk [feature].
Ikuti patterns di docs/design/system/high-level-architecture.md"
```

### 3. Security Agent
**Responsibility:**
- Threat modeling
- Security review
- Vulnerability assessment
- Compliance check

**Input:** Architecture, code changes
**Output:** Security reports di `docs/design/security/`

**Kiro Implementation:**
```
"Bertindak sebagai Security Agent.
Review perubahan code berikut dari perspektif security.
Check: OWASP Top 10, input validation, auth/authz, data exposure.
Reference: docs/design/security/threat-model.md"
```

### 4. Frontend Agent
**Responsibility:**
- UI component implementation
- State management
- Accessibility
- Responsive design

**Input:** UI/UX design, component specs
**Output:** Frontend code di `src/presentation/`

**Kiro Implementation:**
```
"Bertindak sebagai Frontend Agent.
Implementasikan component berdasarkan design di docs/design/ui-ux/.
Ikuti standards di .kiro/skills/create-component.md.
Pastikan accessibility compliance."
```

### 5. Backend Agent
**Responsibility:**
- API implementation
- Business logic
- Data layer
- Integration

**Input:** API contracts, business rules, architecture
**Output:** Backend code di `src/`

**Kiro Implementation:**
```
"Bertindak sebagai Backend Agent.
Implementasikan endpoint berdasarkan spec di docs/specs/srs/api-contract.md.
Ikuti Clean Architecture pattern.
Gunakan skills: create-api, create-usecase, create-repository."
```

### 6. QA Agent
**Responsibility:**
- Test planning
- Test implementation
- Coverage analysis
- Bug detection

**Input:** Acceptance criteria, code changes
**Output:** Test files di `tests/`

**Kiro Implementation:**
```
"Bertindak sebagai QA Agent.
Tulis tests untuk [feature/code].
Cover: happy path, error scenarios, edge cases.
Target coverage: >= 80%.
Ikuti skill create-test.md."
```

### 7. DevOps Agent
**Responsibility:**
- CI/CD pipeline
- Infrastructure
- Deployment
- Monitoring setup

**Input:** Architecture, deployment requirements
**Output:** CI/CD configs, infrastructure code

**Kiro Implementation:**
```
"Bertindak sebagai DevOps Agent.
Setup CI/CD pipeline untuk [feature].
Platform: GitLab CI/CD.
Include: lint, test, build, security scan, deploy."
```

## Orchestration Patterns

### Sequential Orchestration
Agents bekerja berurutan, output satu menjadi input berikutnya.

```
[BA] → requirements → [Architect] → design → [Backend] → code → [QA] → tests
```

### Parallel Orchestration
Agents bekerja paralel untuk tasks yang independent.

```
[Architect] → design
                ├── [Frontend Agent] → UI code
                ├── [Backend Agent] → API code
                └── [QA Agent] → test plan
```

### Review Orchestration
Agent melakukan review terhadap output agent lain.

```
[Backend Agent] → code → [Security Agent] → review → [QA Agent] → tests
```

## Implementasi dengan Kiro

### Menggunakan Kiro Hooks untuk Orchestration

```json
{
  "name": "Security Review on Code Change",
  "version": "1.0.0",
  "when": {
    "type": "fileEdited",
    "patterns": ["src/**/*.ts"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Bertindak sebagai Security Agent. Review perubahan ini dari perspektif security. Check: input validation, auth, data exposure, injection risks."
  }
}
```

```json
{
  "name": "Architecture Check on New Files",
  "version": "1.0.0",
  "when": {
    "type": "fileCreated",
    "patterns": ["src/**/*.ts"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Bertindak sebagai Architect Agent. Verify file ini sesuai dengan Clean Architecture rules. Check: layer placement, dependency direction, naming convention."
  }
}
```

### Menggunakan Kiro Sub-Agents

Untuk complex tasks, gunakan sub-agents:

```
"Untuk feature Authentication:
1. Sebagai Architect Agent: review design di docs/design/
2. Sebagai Backend Agent: implementasikan berdasarkan spec
3. Sebagai QA Agent: tulis comprehensive tests
4. Sebagai Security Agent: review final implementation"
```

## GitLab Integration

### Agent-Specific CI/CD Jobs

```yaml
# Security Agent check di CI/CD
security-agent-review:
  stage: review
  script:
    - echo "Running automated security checks..."
    - npm run security:scan
    - npm run dependency:audit
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

# QA Agent check di CI/CD
qa-agent-review:
  stage: test
  script:
    - npm run test:coverage
    - |
      COVERAGE=$(cat coverage/coverage-summary.json | jq '.total.lines.pct')
      if (( $(echo "$COVERAGE < 80" | bc -l) )); then
        echo "Coverage below threshold: $COVERAGE%"
        exit 1
      fi
```

### Agent Labels di GitLab

```
agent::ba
agent::architect
agent::security
agent::frontend
agent::backend
agent::qa
agent::devops
```

## Orchestrator Options

| Tool | Use Case | Complexity |
|------|----------|-----------|
| Kiro (native) | Single developer workflow | Low |
| Kiro Hooks | Automated triggers | Medium |
| Kiro Sub-Agents | Parallel tasks | Medium |
| Custom MCP | Complex multi-step | High |

## Best Practices

1. **Clear role boundaries** - Setiap agent punya scope yang jelas
2. **Context passing** - Output agent A menjadi input agent B
3. **Human in the loop** - Manusia tetap decision maker
4. **Audit trail** - Track semua agent decisions
5. **Fail gracefully** - Jika satu agent gagal, jangan block yang lain
6. **Iterative** - Agent bisa di-invoke ulang untuk improvement
