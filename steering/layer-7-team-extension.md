# Layer 7 — Team Extension Skills

## Konsep

```
Global Standards + Team Extensions
```

Setiap tim memiliki domain knowledge yang spesifik. Layer ini memungkinkan tim menambahkan skills khusus di atas global standards.

## Struktur

```
.kiro/
├── skills/                    # Global skills (semua tim)
│   ├── create-api.md
│   ├── create-usecase.md
│   └── create-test.md
└── steering/
    ├── project-standards.md   # Global standards
    └── team/                  # Team-specific
        ├── payment-team.md
        ├── ai-team.md
        └── platform-team.md
```

## Contoh: Global Standards

```markdown
# Global Standards

## Architecture
- Clean Architecture
- TypeScript strict mode
- Jest for testing
- Zustand for state management (frontend)

## Coding
- ESLint + Prettier
- No any type
- All functions typed
- Error handling with Result pattern

## Git
- Conventional commits
- Feature branches
- Squash merge
- MR review required
```

## Contoh: Payment Team Extension

```markdown
---
inclusion: fileMatch
fileMatchPattern: "src/payment/**/*"
---

# Payment Team Standards

## Domain Knowledge
- PCI DSS compliance required
- All payment data must be encrypted at rest
- Tokenization for card data
- Audit trail for all transactions

## Specific Patterns

### OTP Flow
```typescript
// OTP harus expire dalam 5 menit
// Max 3 retry attempts
// Lock account setelah 5 failed attempts
const OTP_EXPIRY = 5 * 60 * 1000; // 5 minutes
const MAX_RETRY = 3;
const LOCK_THRESHOLD = 5;
```

### Payment Security
- NEVER log full card numbers
- ALWAYS mask PAN: `****-****-****-1234`
- Use payment gateway tokenization
- Implement idempotency keys for all payment operations

### Retry Policy
```typescript
// Payment retry configuration
const RETRY_CONFIG = {
  maxRetries: 3,
  initialDelay: 1000,    // 1 second
  maxDelay: 30000,       // 30 seconds
  backoffMultiplier: 2,
  retryableErrors: ['TIMEOUT', 'NETWORK_ERROR', 'GATEWAY_UNAVAILABLE'],
  nonRetryableErrors: ['INSUFFICIENT_FUNDS', 'CARD_DECLINED', 'FRAUD_DETECTED'],
};
```

### Binding Flow
- Binding = menghubungkan payment method ke user
- Harus melalui 3DS verification
- Store token, NEVER store raw card data
- Implement soft delete untuk unbinding

## Testing Requirements
- All payment flows harus punya integration test
- Mock payment gateway di unit test
- E2E test dengan sandbox environment
- Load test untuk concurrent transactions
```

## Contoh: AI Team Extension

```markdown
---
inclusion: fileMatch
fileMatchPattern: "src/ai/**/*"
---

# AI Team Standards

## Domain Knowledge
- RAG (Retrieval Augmented Generation) patterns
- Embedding strategies
- Prompt registry management
- Model versioning

## Specific Patterns

### RAG Strategy
```typescript
// RAG Pipeline
// 1. Query → Embedding
// 2. Embedding → Vector Search
// 3. Results → Context Assembly
// 4. Context + Query → LLM
// 5. LLM Response → Post-processing

interface RAGPipeline {
  embed(query: string): Promise<number[]>;
  search(embedding: number[], topK: number): Promise<Document[]>;
  assembleContext(documents: Document[], maxTokens: number): string;
  generate(context: string, query: string): Promise<string>;
  postProcess(response: string): string;
}
```

### Embedding Strategy
- Model: [text-embedding-ada-002 / custom]
- Chunk size: 512 tokens
- Overlap: 50 tokens
- Storage: Vector database (Pinecone/Weaviate/pgvector)

### Prompt Registry
```typescript
// Semua prompts harus di-register
// Versioning untuk A/B testing
// Metrics tracking per prompt version

interface PromptRegistry {
  register(name: string, version: string, template: string): void;
  get(name: string, version?: string): PromptTemplate;
  track(name: string, version: string, metrics: PromptMetrics): void;
}
```

## Testing Requirements
- Mock LLM responses di unit test
- Evaluation metrics untuk RAG quality
- Latency benchmarks
- Cost tracking per request
```

## Contoh: Platform Team Extension

```markdown
---
inclusion: fileMatch
fileMatchPattern: "src/platform/**/*"
---

# Platform Team Standards

## Domain Knowledge
- Infrastructure as Code
- Multi-tenancy patterns
- Rate limiting
- Service mesh

## Specific Patterns

### Multi-tenancy
- Tenant isolation via schema separation
- Tenant context propagation via middleware
- Resource quotas per tenant

### Rate Limiting
```typescript
const RATE_LIMITS = {
  free: { requests: 100, window: '1h' },
  pro: { requests: 1000, window: '1h' },
  enterprise: { requests: 10000, window: '1h' },
};
```

### Observability
- All services must emit structured logs
- Distributed tracing with correlation IDs
- Custom metrics for business KPIs
```

## Cara Setup Team Extensions

### Step 1: Buat Global Standards
```
.kiro/steering/project-standards.md
```

### Step 2: Buat Team-Specific Files
```
.kiro/steering/team/{team-name}.md
```

### Step 3: Gunakan fileMatch untuk Auto-Loading
```yaml
---
inclusion: fileMatch
fileMatchPattern: "src/{team-domain}/**/*"
---
```

Dengan ini, saat AI bekerja di folder tertentu, team-specific standards otomatis di-load.

## GitLab Integration

### Team-Specific CI/CD

```yaml
# .gitlab/ci/team-payment.yml
payment-security-scan:
  stage: security
  script:
    - echo "Running payment-specific security checks..."
    - |
      # Check no raw card data in code
      if grep -rn "cardNumber\|cvv\|pan" src/payment/ | grep -v "mask\|token\|test"; then
        echo "ERROR: Possible raw card data in payment code!"
        exit 1
      fi
  rules:
    - changes:
        - src/payment/**/*
```

### Team-Specific MR Templates

Setiap tim bisa punya MR template sendiri di `.gitlab/merge_request_templates/`:

```
.gitlab/merge_request_templates/
├── Default.md
├── Payment.md
├── AI.md
└── Platform.md
```

## Best Practices

1. **Global first** - Mulai dengan global standards, extend per tim
2. **Don't duplicate** - Team extension hanya untuk yang spesifik
3. **Auto-load** - Gunakan fileMatch agar tidak perlu manual
4. **Evolve** - Team extensions berkembang seiring domain knowledge bertambah
5. **Share knowledge** - Team extensions bisa jadi referensi cross-team
