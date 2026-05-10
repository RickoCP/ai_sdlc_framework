# Layer 13 — Observability & Feedback Loop

## Tujuan

Mengukur:
- System health
- AI performance
- Engineering quality

## Observability Areas

1. **Logs** - Structured logging
2. **Metrics** - Business & technical metrics
3. **Tracing** - Distributed tracing
4. **Analytics** - User behavior
5. **Error Tracking** - Error monitoring
6. **AI Usage Metrics** - AI tool effectiveness
7. **Hallucination Metrics** - AI accuracy tracking

## Recommended Stack

| Area | Tool | Purpose |
|------|------|---------|
| Error Tracking | Sentry | Error monitoring & alerting |
| Tracing | OpenTelemetry | Distributed tracing |
| Dashboards | Grafana | Visualization |
| Logs | Loki | Log aggregation |
| Traces | Tempo | Trace storage |
| Metrics | Prometheus | Metrics collection |
| APM | Grafana Cloud | Application performance |

## Structured Logging

### Log Format

```typescript
interface LogEntry {
  timestamp: string;      // ISO-8601
  level: 'debug' | 'info' | 'warn' | 'error' | 'fatal';
  message: string;
  service: string;
  traceId: string;
  spanId: string;
  userId?: string;
  requestId: string;
  context: Record<string, unknown>;
  error?: {
    name: string;
    message: string;
    stack: string;
  };
}
```

### Logging Standards

```typescript
// Good - structured, contextual
logger.info('User created', {
  userId: user.id,
  email: user.email,
  source: 'registration',
  duration: elapsed,
});

// Good - error with context
logger.error('Payment failed', {
  orderId: order.id,
  amount: order.amount,
  gateway: 'stripe',
  error: {
    name: err.name,
    message: err.message,
    code: err.code,
  },
});

// Bad - unstructured
logger.info(`User ${user.email} created`);

// Bad - sensitive data
logger.info('Login', { password: user.password }); // NEVER!
```

### Log Levels

| Level | When to Use | Example |
|-------|-------------|---------|
| debug | Development only | Variable values, flow tracing |
| info | Normal operations | User actions, business events |
| warn | Potential issues | Retry attempts, deprecation |
| error | Failures | Exceptions, failed operations |
| fatal | System crash | Unrecoverable errors |

## Metrics

### Business Metrics

```typescript
// Track business KPIs
metrics.counter('user.registration', { source: 'web' });
metrics.counter('order.created', { type: 'subscription' });
metrics.histogram('payment.duration', elapsed, { gateway: 'stripe' });
metrics.gauge('active.users', count);
```

### Technical Metrics

```typescript
// Track system health
metrics.histogram('http.request.duration', elapsed, {
  method: 'POST',
  path: '/api/users',
  status: '201',
});
metrics.counter('http.request.total', { method, path, status });
metrics.gauge('db.connection.pool', poolSize);
metrics.counter('cache.hit', { key: 'user-profile' });
metrics.counter('cache.miss', { key: 'user-profile' });
```

### AI Metrics

```typescript
// Track AI performance
metrics.counter('ai.generation.total', { agent: 'backend', task: 'create-api' });
metrics.histogram('ai.generation.duration', elapsed, { agent: 'backend' });
metrics.counter('ai.review.issues_found', { severity: 'high' });
metrics.counter('ai.hallucination.detected', { type: 'architecture-violation' });
metrics.gauge('ai.code.acceptance_rate', rate); // % of AI code that passes review
```

## Distributed Tracing

### OpenTelemetry Setup

```typescript
import { NodeSDK } from '@opentelemetry/sdk-node';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';

const sdk = new NodeSDK({
  traceExporter: new OTLPTraceExporter({
    url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT,
  }),
  instrumentations: [
    // Auto-instrument HTTP, database, etc.
  ],
});

sdk.start();
```

### Custom Spans

```typescript
import { trace } from '@opentelemetry/api';

const tracer = trace.getTracer('payment-service');

async function processPayment(order: Order) {
  return tracer.startActiveSpan('process-payment', async (span) => {
    span.setAttribute('order.id', order.id);
    span.setAttribute('order.amount', order.amount);
    
    try {
      const result = await gateway.charge(order);
      span.setAttribute('payment.status', 'success');
      return result;
    } catch (error) {
      span.recordException(error);
      span.setStatus({ code: SpanStatusCode.ERROR });
      throw error;
    } finally {
      span.end();
    }
  });
}
```

## Error Tracking (Sentry)

### Setup

```typescript
import * as Sentry from '@sentry/node';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  release: process.env.CI_COMMIT_SHA,
  tracesSampleRate: 0.1, // 10% of transactions
  integrations: [
    new Sentry.Integrations.Http({ tracing: true }),
  ],
});
```

### Error Context

```typescript
Sentry.withScope((scope) => {
  scope.setUser({ id: user.id, email: user.email });
  scope.setTag('feature', 'payment');
  scope.setContext('order', { id: order.id, amount: order.amount });
  Sentry.captureException(error);
});
```

## GitLab Integration

### CI/CD Observability

```yaml
# .gitlab/ci/observability.yml
deploy-with-release:
  stage: deploy-production
  script:
    # Create Sentry release
    - npx @sentry/cli releases new $CI_COMMIT_SHA
    - npx @sentry/cli releases set-commits $CI_COMMIT_SHA --auto
    - npx @sentry/cli releases finalize $CI_COMMIT_SHA
    # Deploy
    - ./deploy.sh
    # Mark deploy in Sentry
    - npx @sentry/cli releases deploys $CI_COMMIT_SHA new -e production
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

### GitLab Metrics Dashboard

Gunakan GitLab built-in metrics:
- Pipeline duration trends
- Deployment frequency
- Lead time for changes
- Change failure rate
- Mean time to recovery (MTTR)

## Alerting

### Alert Rules

| Metric | Threshold | Severity | Action |
|--------|-----------|----------|--------|
| Error rate | > 1% | Critical | Page on-call |
| Response time P95 | > 3s | High | Notify team |
| CPU usage | > 80% | Medium | Auto-scale |
| Memory usage | > 85% | High | Investigate |
| Disk usage | > 90% | Critical | Expand storage |
| Failed deployments | > 0 | High | Rollback |

### Alert Template

```yaml
# Grafana alert rule example
alert: HighErrorRate
expr: rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) > 0.01
for: 5m
labels:
  severity: critical
annotations:
  summary: "High error rate detected"
  description: "Error rate is {{ $value | humanizePercentage }} (threshold: 1%)"
```

## Feedback Loop

```
Production Metrics
    ↓
Analyze Patterns
    ↓
Identify Issues
    ↓
Create GitLab Issues
    ↓
Improve (Code/Specs/Skills)
    ↓
Deploy Fix
    ↓
Verify Improvement
```

## Best Practices

1. **Observe everything** - Jika tidak di-observe, tidak bisa di-improve
2. **Structured logs** - Selalu gunakan structured logging
3. **Correlation IDs** - Trace request across services
4. **Alert on symptoms** - Alert pada user impact, bukan internal metrics
5. **Dashboard per team** - Setiap tim punya dashboard sendiri
6. **SLO-based** - Define SLOs, alert saat SLO terancam
7. **Cost-aware** - Monitor observability costs


---

## Appendix: Observability Standards (dari Observability Guideline)

### Prinsip Observability

1. **Observable by Default** - Semua fitur baru HARUS bisa di-observe
2. **Correlation First** - Semua log, trace, metric HARUS bisa dihubungkan
3. **No Blind Spot** - Tidak boleh ada bagian sistem yang tidak bisa dilacak
4. **Low Noise, High Signal** - Log harus informatif, bukan spam

### Structured Logging (WAJIB)

```typescript
logger.info({
  message: 'Payment success',
  userId,
  orderId,
  correlationId,
});
```

### Log Levels

- `info` - Event normal
- `warn` - Kondisi abnormal
- `error` - Failure
- `debug` - Dev only

### DILARANG Log

- Token
- Password
- OTP
- CVV
- PII sensitif tanpa masking

### Context Wajib di Setiap Log

- correlationId
- feature/module
- timestamp (auto)

### Metrics (RED Method)

- **Rate** - Jumlah request
- **Error** - Jumlah error
- **Duration** - Waktu response

### Tracing

- End-to-end trace: UI - BFF - API - DB
- Correlation ID dibuat di entry point, propagate ke seluruh layer
- Span minimal: API call, use case execution, external service call

### Dashboard Monitoring Minimal

- Request rate
- Error rate
- Latency
- Top endpoints
- Top errors

### Alerting Rules

- Error spike - alert
- Latency tinggi - alert
- Service down - alert
- Alert harus actionable, tidak spam

---

## Appendix: Incident Management (dari Incident Management Standards)

### Severity Classification

| Severity | Deskripsi | Response |
|----------|-----------|----------|
| Sev-1 (Critical) | Total outage, dampak luas | Immediate response |
| Sev-2 (High) | Fitur utama terganggu | < 30 min response |
| Sev-3 (Medium) | Fitur tertentu terganggu, ada workaround | < 2 hour response |
| Sev-4 (Low) | Minor bug, tidak mengganggu flow utama | Next sprint |

### Incident Lifecycle

```
Detection - Triage - Response - Mitigation - Recovery - Postmortem - Follow-up
```

### Peran dalam Incident

1. **Incident Commander** - Memimpin respons, menentukan prioritas
2. **Tech Lead / Resolver** - Investigasi teknis, implementasi perbaikan
3. **Communication Lead** - Update status ke stakeholder

### Mitigation Strategy

- Rollback release
- Disable fitur (feature flag)
- Fallback ke service lain
- Rate limit sementara

### Postmortem Template (WAJIB untuk Sev-1 dan Sev-2)

```markdown
# Incident Postmortem: [Judul]

## Ringkasan
## Timeline
## Dampak
## Root Cause
## Contributing Factors
## What Went Well
## What Went Wrong
## Action Items
- [ ] Perbaikan teknis
- [ ] Improvement process
- [ ] Monitoring tambahan
## Owner
```

### Metrics Incident

- MTTR (Mean Time To Recovery)
- MTTD (Mean Time To Detect)
- Jumlah incident per bulan
- Jumlah Sev-1/Sev-2


---

## Workflow Integration — Menjadikan Observability Bagian dari Development Flow

### Prinsip

Observability BUKAN afterthought. Observability adalah **bagian dari Definition of Done** untuk setiap fitur.

---

### 1. Observability Checklist per Fitur (WAJIB)

Setiap fitur baru HARUS memenuhi checklist observability sebelum dianggap "done":

```markdown
## Observability Checklist — [Feature Name]

### Logging
- [ ] Structured logging ditambahkan di entry point (API handler/route)
- [ ] Error logging dengan context (userId, requestId, correlationId)
- [ ] Tidak ada sensitive data di log (password, token, OTP, CVV)
- [ ] Log level sesuai (info untuk normal, error untuk failure)

### Metrics
- [ ] Business metric ditambahkan (counter untuk event utama)
- [ ] Technical metric ditambahkan (duration untuk operasi penting)
- [ ] Error counter ditambahkan per endpoint/use case

### Tracing
- [ ] Correlation ID di-propagate dari entry point ke semua layer
- [ ] Custom span ditambahkan untuk operasi kritis (DB call, external API)

### Error Handling
- [ ] Error di-capture ke Sentry/error tracker
- [ ] Error context lengkap (user, request, stack trace)
- [ ] Graceful degradation jika observability service down

### Alerting
- [ ] Alert rule didefinisikan untuk failure scenario
- [ ] Alert threshold realistis (tidak spam)
```

AI Agent WAJIB mengecek checklist ini saat:
- Membuat fitur baru (Layer 8 task completion)
- AI Review (Layer 11)
- Pre-push validation

---

### 2. AI Agent Rules: Kapan Menambahkan Observability

| Situasi | Action AI Agent |
|---------|----------------|
| Membuat API endpoint baru | WAJIB tambah: structured log di handler, duration metric, error counter |
| Membuat Use Case baru | WAJIB tambah: log di entry/exit, error capture |
| Membuat Repository baru | WAJIB tambah: log untuk external call, duration metric |
| Membuat Middleware baru | WAJIB tambah: request log, auth event log |
| Fix bug | WAJIB tambah: log/metric yang bisa mendeteksi bug ini di masa depan |
| Performance improvement | WAJIB tambah: before/after metric untuk validasi improvement |

### Pattern: Observability di Setiap Layer

```typescript
// USE CASE — log business event
export const CreatePaymentUseCaseImpl = ({
  paymentRepository,
  logger,
  metrics,
}: {
  paymentRepository: PaymentRepository;
  logger: Logger;
  metrics: MetricsClient;
}): CreatePaymentUseCase => ({
  execute: async (params) => {
    const startTime = Date.now();
    logger.info('Payment creation started', { userId: params.userId, amount: params.amount });

    try {
      const result = await paymentRepository.create(params);
      metrics.counter('payment.created', { status: 'success' });
      metrics.histogram('payment.creation.duration', Date.now() - startTime);
      logger.info('Payment created successfully', { paymentId: result.id });
      return { success: true, data: result };
    } catch (error) {
      metrics.counter('payment.created', { status: 'error' });
      logger.error('Payment creation failed', { error: error.message, userId: params.userId });
      return { success: false, error };
    }
  },
});
```

```typescript
// REPOSITORY — log external call
export const PaymentRepositoryImpl = ({
  httpProtocol,
  logger,
  metrics,
}: {
  httpProtocol: HttpProtocol;
  logger: Logger;
  metrics: MetricsClient;
}): PaymentRepository => ({
  create: async (params) => {
    const startTime = Date.now();
    try {
      const response = await httpProtocol.post('/api/payments', params);
      metrics.histogram('http.external.duration', Date.now() - startTime, {
        endpoint: '/api/payments',
        method: 'POST',
        status: 'success',
      });
      return response.data;
    } catch (error) {
      metrics.histogram('http.external.duration', Date.now() - startTime, {
        endpoint: '/api/payments',
        method: 'POST',
        status: 'error',
      });
      logger.error('External API call failed', {
        endpoint: '/api/payments',
        error: error.message,
        duration: Date.now() - startTime,
      });
      throw error;
    }
  },
});
```

---

### 3. DI Registration untuk Observability

Logger dan Metrics WAJIB di-register di DI container dan di-inject ke setiap layer yang membutuhkan:

```typescript
// src/infrastructure/di/registry/loggingContainer.ts
import { asFunction } from 'awilix';

export const registerLogging = (container) => {
  container.register({
    logger: asFunction(createStructuredLogger).singleton(),
    metrics: asFunction(createMetricsClient).singleton(),
    errorTracker: asFunction(createSentryClient).singleton(),
  });
};
```

**Rules:**
- Logger dan Metrics adalah **singleton** (stateless, shared)
- JANGAN import logger langsung — selalu via DI injection
- JANGAN log di core/domain entities — log di use case dan infrastructure

---

### 4. Kiro Hook: Observability Reminder

```json
{
  "name": "Observability Check on New Feature",
  "version": "1.0.0",
  "description": "Mengingatkan agent untuk menambahkan observability saat membuat fitur baru",
  "when": {
    "type": "postToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Jika file yang baru ditulis adalah source code (bukan docs/config), cek apakah observability sudah ditambahkan:\n\n1. Apakah ada structured logging di entry point?\n2. Apakah ada error capture dengan context?\n3. Apakah ada metrics (counter/histogram) untuk operasi penting?\n4. Apakah correlation ID di-propagate?\n\nJika BELUM ada dan file ini adalah handler/usecase/repository → TAMBAHKAN sekarang.\nJika file ini adalah entity/mapper/component UI → SKIP (tidak perlu observability langsung).\n\nPattern: inject logger dan metrics via DI, log di entry/exit/error, tambah duration metric untuk operasi I/O."
  }
}
```

---

### 5. Integration dengan Layer Lain

| Layer | Bagaimana Layer 13 Terintegrasi |
|-------|--------------------------------|
| Layer 6 (AI Skills) | Skill `create-api`, `create-usecase`, `create-repository` HARUS include observability pattern |
| Layer 8 (Issue-Driven) | Observability checklist masuk ke Definition of Done per task |
| Layer 11 (AI Review) | Review checklist: "Apakah fitur ini observable?" |
| Layer 12 (Quality Gates) | CI/CD cek: apakah logger/metrics di-import di file baru |
| Layer 14 (Continuous Learning) | Production metrics menjadi input untuk improvement |

---

### 6. Incident Response Integration

Saat incident terjadi, AI Agent HARUS:

```
[Incident Detected — dari alert/user report]
    ↓
1. Cek severity (Sev-1 s/d Sev-4)
    ↓
2. Jika Sev-1/Sev-2:
   - Buat GitLab issue: type::incident, priority::critical
   - Suggest immediate mitigation (rollback, feature flag off)
   - Generate postmortem template
    ↓
3. Setelah resolved:
   - Buat postmortem document (docs/incidents/INC-XXX.md)
   - Identifikasi: "Kenapa observability tidak mendeteksi lebih awal?"
   - Tambah alert rule baru jika perlu
   - Update monitoring dashboard
   - Create follow-up issues (type::tech-debt atau type::improvement)
    ↓
4. Feed ke Layer 14 (Continuous Learning):
   - Update skill jika pattern berulang
   - Update governance jika ada gap
```

---

### 7. Minimum Viable Observability (untuk Project Baru)

Untuk project yang baru dimulai, implementasi observability minimal:

**Phase 1 (Day 1 — saat project setup):**
- Structured logger (console JSON di dev, service di prod)
- Global error handler + Sentry
- Request logging middleware (method, path, status, duration)

**Phase 2 (Sprint 1 — saat fitur pertama):**
- Business metrics (counter untuk event utama)
- Correlation ID middleware
- Basic dashboard (request rate, error rate, latency)

**Phase 3 (Sprint 2+ — seiring fitur bertambah):**
- Custom spans (OpenTelemetry)
- Alert rules per fitur kritis
- SLO definition

**AI Agent WAJIB** implement Phase 1 saat project setup (Automated Project Setup step).
