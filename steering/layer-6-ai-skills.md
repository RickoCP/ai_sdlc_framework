# Layer 6 — AI Engineering Skills

## Konsep

```
Skill is the new package
```

AI Skill bukan reusable code. Tetapi:
- Reusable workflow
- Reusable engineering standards
- Reusable architecture knowledge

## Output yang Dihasilkan

```
.kiro/skills/
├── create-api.md
├── create-usecase.md
├── create-repository.md
├── create-component.md
├── create-test.md
├── create-migration.md
├── create-event-handler.md
└── testing-standard.md
```

## Contoh Skills

- Create API endpoint
- Create React component
- Create use case
- Create repository
- Create unit test
- Create integration test
- Create database migration
- Create event handler
- Create CI/CD pipeline

---

## Template: create-api.md

```markdown
# Skill: Create API Endpoint

## Prerequisites
- Specification ada di docs/specs/srs/api-contract.md
- Architecture sudah di-review
- Security design sudah ada

## Input Required
- Endpoint path dan method
- Request/response schema
- Business rules
- Error scenarios

## Steps

### 1. Baca Specification
Baca API contract di docs/specs/srs/api-contract.md untuk endpoint yang diminta.

### 2. Create Controller
Location: `src/presentation/controllers/{resource}.controller.ts`

Pattern:
```typescript
import { Controller, Post, Body, HttpCode } from '@nestjs/common';
import { CreateResourceUseCase } from '@/application/use-cases/create-resource';
import { CreateResourceDto } from '@/application/dto/create-resource.dto';

@Controller('resources')
export class ResourceController {
  constructor(private readonly createResource: CreateResourceUseCase) {}

  @Post()
  @HttpCode(201)
  async create(@Body() dto: CreateResourceDto) {
    const result = await this.createResource.execute(dto);
    if (result.isFailure) {
      throw result.error;
    }
    return result.value;
  }
}
```

### 3. Create DTO
Location: `src/application/dto/{action}-{resource}.dto.ts`

Pattern:
```typescript
import { IsString, IsEmail, IsNotEmpty } from 'class-validator';

export class CreateResourceDto {
  @IsString()
  @IsNotEmpty()
  name: string;

  @IsEmail()
  email: string;
}
```

### 4. Create Use Case
Location: `src/application/use-cases/{action}-{resource}.ts`

Ikuti skill `create-usecase.md`

### 5. Create Repository (if needed)
Ikuti skill `create-repository.md`

### 6. Write Tests
Ikuti skill `create-test.md`

### 7. Update API Documentation
Update docs/specs/srs/api-contract.md jika ada perubahan.

## Validation Checklist
- [ ] Input validation ada
- [ ] Error handling lengkap
- [ ] Unit test ada
- [ ] Integration test ada
- [ ] API contract sesuai spec
- [ ] Security checks (auth, authz)
- [ ] Logging ada
```

## Template: create-usecase.md

```markdown
# Skill: Create Use Case

## Prerequisites
- Business rules sudah didefinisikan di spec
- Domain entities sudah ada
- Repository interfaces sudah ada

## Steps

### 1. Create Use Case File
Location: `src/application/use-cases/{action}-{resource}.ts`

Pattern:
```typescript
import { Injectable } from '@nestjs/common';
import { Result } from '@/shared/result';
import { IResourceRepository } from '@/application/ports/resource.repository';

export interface CreateResourceInput {
  name: string;
  email: string;
}

@Injectable()
export class CreateResourceUseCase {
  constructor(
    private readonly resourceRepo: IResourceRepository,
  ) {}

  async execute(input: CreateResourceInput): Promise<Result<Resource>> {
    // 1. Validate business rules
    const existingResource = await this.resourceRepo.findByEmail(input.email);
    if (existingResource) {
      return Result.fail(new ConflictError('Resource already exists'));
    }

    // 2. Create domain entity
    const resource = Resource.create({
      name: input.name,
      email: input.email,
    });

    // 3. Persist
    await this.resourceRepo.save(resource);

    // 4. Return result
    return Result.ok(resource);
  }
}
```

### 2. Create Port (Interface)
Location: `src/application/ports/{resource}.repository.ts`

```typescript
export interface IResourceRepository {
  findById(id: string): Promise<Resource | null>;
  findByEmail(email: string): Promise<Resource | null>;
  save(resource: Resource): Promise<void>;
  delete(id: string): Promise<void>;
}
```

## Rules
- Use case HANYA berisi business logic
- TIDAK BOLEH depend ke infrastructure langsung
- HARUS menggunakan ports (interfaces)
- HARUS return Result type
- HARUS handle semua error scenarios dari spec
```

## Template: create-test.md

```markdown
# Skill: Create Tests

## Test Structure
```
tests/
├── unit/
│   ├── use-cases/
│   │   └── create-resource.spec.ts
│   └── domain/
│       └── resource.spec.ts
├── integration/
│   ├── controllers/
│   │   └── resource.controller.spec.ts
│   └── repositories/
│       └── resource.repository.spec.ts
└── e2e/
    └── resource.e2e.spec.ts
```

## Unit Test Pattern
```typescript
describe('CreateResourceUseCase', () => {
  let useCase: CreateResourceUseCase;
  let mockRepo: jest.Mocked<IResourceRepository>;

  beforeEach(() => {
    mockRepo = {
      findByEmail: jest.fn(),
      save: jest.fn(),
    } as any;
    useCase = new CreateResourceUseCase(mockRepo);
  });

  it('should create resource successfully', async () => {
    mockRepo.findByEmail.mockResolvedValue(null);
    
    const result = await useCase.execute({
      name: 'Test',
      email: 'test@example.com',
    });

    expect(result.isSuccess).toBe(true);
    expect(mockRepo.save).toHaveBeenCalled();
  });

  it('should fail if resource already exists', async () => {
    mockRepo.findByEmail.mockResolvedValue(existingResource);
    
    const result = await useCase.execute({
      name: 'Test',
      email: 'existing@example.com',
    });

    expect(result.isFailure).toBe(true);
    expect(result.error).toBeInstanceOf(ConflictError);
  });
});
```

## Rules
- Setiap use case HARUS punya unit test
- Test semua happy path DAN error scenarios
- Mock external dependencies
- Test naming: `should [expected behavior] when [condition]`
- Coverage target: >= 80%
```

## Cara Menggunakan Skills di Kiro

### Sebagai Steering File
Simpan skills di `.kiro/skills/` agar Kiro bisa mengaksesnya:

```
"Gunakan skill create-api untuk membuat endpoint POST /users
berdasarkan spec di docs/specs/srs/api-contract.md#create-user"
```

### Sebagai Reference di Spec
```markdown
## Implementation Guide
Gunakan skills berikut:
- #[[file:.kiro/skills/create-api.md]]
- #[[file:.kiro/skills/create-usecase.md]]
- #[[file:.kiro/skills/create-test.md]]
```

## Best Practices

1. **Skills = Standards** - Skill mendefinisikan HOW, bukan WHAT
2. **Consistent patterns** - Semua code yang di-generate mengikuti pattern yang sama
3. **Composable** - Skills bisa saling referensi
4. **Evolving** - Update skills seiring project mature
5. **Team-owned** - Skills disepakati dan di-maintain oleh tim


---

## Appendix: Dependency Injection Pattern (dari DI Standards)

### DI Container: Awilix

Struktur DI:
```
src/infrastructure/di/
├── container.ts
└── registry/
    ├── moduleContainer.ts
    ├── repositoryContainer.ts
    ├── useCasesContainer.ts
    ├── viewModelContainer.ts
    └── loggingContainer.ts
```

### Factory Function Pattern (WAJIB)

```typescript
export const NamaUseCaseImpl = ({
  namaRepository,
}: {
  namaRepository: NamaRepository;
}): NamaUseCase => ({
  execute: async params => {
    return namaRepository.getData(params);
  },
});
```

### Registration Pattern

```typescript
// Module/Protocol
container.register({
  httpProtocol: asFunction(HttpAdapter).singleton(),
});

// Repository
container.register({
  paymentRepository: asFunction(PaymentRepositoryImpl).singleton(),
});

// Use Case
container.register({
  updatePaymentUseCase: asFunction(UpdatePaymentUseCaseImpl).singleton(),
});

// ViewModel
container.register({
  usePaymentViewModel: asFunction(PaymentViewModel),
});
```

### Lifetime Rules

- **Singleton** - Dependency stateless (config, logger, repository, use case)
- **Scoped** - Request-bound dependency
- **Transient** - Object dengan mutable state lokal

### DI Rules

- Core TIDAK BOLEH import container
- Semua dependency HARUS lewat DI (tidak boleh `new` manual)
- Registration key: camelCase dengan makna jelas
- Resolve sedekat mungkin dengan composition boundary

---

## Appendix: BFF Pattern (dari BFF Standards)

### Kapan Menggunakan BFF

Gunakan BFF jika:
- UI perlu menggabungkan banyak backend
- Perlu normalisasi contract untuk frontend
- Perlu boundary auth/security yang kuat
- Perlu menyembunyikan kompleksitas backend internal

### BFF Request Flow

```
Incoming Request → Validate Input → Resolve Auth Context
→ Resolve UseCase/Orchestrator → Call backend(s)
→ Aggregate/Map Response → Return safe UI contract
```

### BFF Rules

- BFF adalah orchestration layer, BUKAN tempat business logic domain
- Route handler hanya menjadi HTTP boundary
- Gunakan use case/service orchestration yang testable
- Auth diambil di boundary (server), bukan dipercaya dari client
- Header forwarding via whitelist saja
- Semua external call HARUS punya timeout
- Error normalization WAJIB (jangan expose internal)

### Skill: Create BFF Route Handler

```typescript
// src/app/api/[feature]/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  // 1. Validate input
  const body = await request.json();
  const validation = validateInput(body);
  if (!validation.success) {
    return NextResponse.json(
      { code: 'COMMON_VALIDATION_FAILED', fields: validation.errors },
      { status: 400 }
    );
  }

  // 2. Resolve auth context
  const token = request.headers.get('authorization');
  if (!token) {
    return NextResponse.json(
      { code: 'AUTH_UNAUTHORIZED', message: 'Unauthorized' },
      { status: 401 }
    );
  }

  // 3. Execute use case
  const useCase = container.resolve('namaUseCase');
  const result = await useCase.execute({ token, ...body });

  // 4. Return safe response
  if (result.isFailure) {
    return NextResponse.json(
      { code: result.error.code, message: result.error.userMessage },
      { status: mapErrorToStatus(result.error) }
    );
  }

  return NextResponse.json(result.value, { status: 200 });
}
```

---

## Appendix: Code Pattern Reference (dari Code Pattern Standards)

### Entity (Domain Model)

```typescript
// Pure TypeScript, no dependency
export interface PaymentEntity {
  id: string;
  amount: number;
  status: 'pending' | 'success' | 'failed';
}
```

### Repository Interface (CORE)

```typescript
export interface PaymentRepository {
  getPayment: (auth: string, id: string) => Promise<DomainResult<PaymentEntity>>;
}
```

### Repository Implementation (INFRASTRUCTURE)

```typescript
export const PaymentRepositoryImpl = ({
  apiRequest,
}: { apiRequest: HttpProtocol }): PaymentRepository => ({
  getPayment: (auth, id) =>
    handleApiRequest({
      apiRequest,
      endpoint: PaymentEndpoint.get(id),
      method: 'get',
      headers: { Authorization: auth },
      mapper: mapPaymentResponse,
      errorMessage: 'Get Payment Failed',
    }),
});
```

### Mapper (WAJIB)

```typescript
// DTO → Domain Entity (1 file = 1 mapper)
export function mapPaymentResponse(res: PaymentResponseDto): PaymentEntity {
  return {
    id: res.transaction_id,
    amount: res.total_amount,
    status: normalizeStatus(res.payment_status),
  };
}
```

### ViewModel (PRESENTATION)

```typescript
const PaymentViewModel = ({ getPaymentUseCase }: Props) => {
  const { data, isPending } = useQuery({
    queryKey: ['payment', id],
    queryFn: () => getPaymentUseCase.execute({ token, id }),
  });
  return { data, isPending };
};
```

---

## Appendix: CX Platform API Skills (dari CX API Playbook)

### Go Project Structure

```
/cmd/server
/internal/handler
/internal/service
/internal/repository
/internal/model
/internal/middleware
/pkg/config
/pkg/logger
```

### Mandatory Headers

- `Authorization` - Bearer token
- `x-correlation-id` - Traceability
- `x-device-id` - Device context
- `x-app-version` - Client version
- `Idempotency-Key` - Wajib untuk operasi kritikal

### Resilience Pattern (WAJIB)

- **Timeouts** - WAJIB untuk semua external calls
- **Retry** - Max 3 attempts, exponential backoff, hanya untuk transient failures
- **Circuit Breaker** - Cegah cascading failures

### Rate Limiting

| Type | Use Case | Example |
|------|----------|---------|
| Per User | Logged-in APIs | 100 req/min |
| Per Device | Mobile apps | 60 req/min |
| Per IP | Public APIs | 100 req/min |
| Per API | Critical endpoints | 10-50 req/min |

### Caching Rules

- Boleh cache: catalog, offers, dashboard aggregation (short TTL)
- DILARANG cache: payments, balance, real-time transactional data