---
inclusion: manual
---

# Tech Stack Profiles — Adaptasi Multi-Stack

## Overview

Dokumen ini menyediakan **konfigurasi spesifik per tech stack** untuk mengadaptasi prinsip Clean Architecture + DI dari framework ini ke berbagai bahasa dan ekosistem.

Prinsip framework ini **tech-agnostic** — separation of concerns, dependency rule, dan DI tetap berlaku. Yang berubah adalah tooling, konvensi, dan implementasi teknis.

**Kapan digunakan:** Saat memulai proyek non-Next.js, migrasi stack, atau membuat service baru dalam polyglot architecture.

### Mapping Konsep Universal
| Konsep Framework | Next.js (TS) | Go | Python (FastAPI) |
|-----------------|--------------|-----|------------------|
| DI Container | Awilix | Wire / manual | dependency-injector |
| Repository Interface | TypeScript interface | Go interface | ABC / Protocol |
| Use Case | Factory function | Struct + method | Class dengan `execute()` |
| ViewModel | Hook-based | N/A (API only) | N/A (API only) |
| Entry Point | `app/` router | `cmd/` | `main.py` |
| Package Manager | npm/pnpm | go mod | pip/poetry |
| Linter | ESLint | golangci-lint | ruff |
| Formatter | Prettier | gofmt | ruff format |
| Test Runner | Vitest/Jest | go test | pytest |

---

## Profile 1: Next.js (TypeScript) — Default

Stack default, sudah didetailkan di `architecture-standards.md`. Ringkasan sebagai baseline:

**Structure:** `src/{core,infrastructure,presentation,app}`
**DI:** Awilix (asFunction, singleton/transient)
**Test:** Vitest + React Testing Library
**Lint:** ESLint + Prettier
**Commands:** `pnpm lint` | `pnpm typecheck` | `pnpm test` | `pnpm build`

---

## Profile 2: Go (Golang) — Clean Architecture
### Folder Structure

```
project-root/
├── cmd/api/main.go                 # Entry point (composition root)
├── internal/
│   ├── domain/[domain]/            # CORE — entity, interface, errors
│   │   ├── entity.go
│   │   ├── repository.go          # Interface only
│   │   └── errors.go
│   ├── usecase/[domain]/           # Use case implementations
│   │   ├── usecase.go
│   │   └── usecase_test.go
│   ├── infrastructure/             # INFRA — adapters
│   │   ├── repository/[domain]/    # DB implementations
│   │   ├── http/                   # HTTP client adapters
│   │   └── config/
│   └── delivery/                   # PRESENTATION — transport
│       ├── http/handler/           # HTTP handlers
│       ├── http/middleware/
│       ├── http/dto/               # Request/Response DTOs
│       └── grpc/                   # (optional)
├── pkg/                            # Public shared libraries
├── migrations/
├── go.mod                          # = package.json
├── Makefile
└── .golangci.yml
```

### Mapping Layer

| Framework Layer | Go Equivalent |
|----------------|---------------|
| `core/` | `internal/domain/` — Interface + entity, zero dependency |
| `infrastructure/` | `internal/infrastructure/` — Implementasi konkret |
| `presentation/` | `internal/delivery/` — HTTP/gRPC handlers |
| `app/` | `cmd/` — Composition root, wiring |

### DI Approach

Go tidak memerlukan DI container yang berat. **Manual Constructor Injection** direkomendasikan (Google Wire untuk proyek besar).

```go
// internal/domain/payment/repository.go — Interface di domain
type PaymentRepository interface {
    GetByID(ctx context.Context, id string) (*Payment, error)
}

// internal/usecase/payment/usecase.go — Inject via constructor
type PaymentUseCase struct {
    repo   domain.PaymentRepository
    logger pkg.Logger
}

func NewPaymentUseCase(repo domain.PaymentRepository, logger pkg.Logger) *PaymentUseCase {
    return &PaymentUseCase{repo: repo, logger: logger}
}

// cmd/api/main.go — Composition Root (= DI registration)
func main() {
    db := postgres.NewConnection(cfg.DatabaseURL)
    paymentRepo := repository.NewPostgresPaymentRepo(db)
    paymentUC := usecase.NewPaymentUseCase(paymentRepo, logger.New(cfg.LogLevel))
    paymentHandler := handler.NewPaymentHandler(paymentUC)
    router := setupRouter(paymentHandler)
    server.Start(router, cfg.Port)
}
```

### Testing

Mock cukup implement interface — tidak perlu framework berat.

```go
func TestGetPayment_Success(t *testing.T) {
    repo := &mockPaymentRepo{payments: map[string]*domain.Payment{
        "123": {ID: "123", Amount: 50000},
    }}
    uc := usecase.NewPaymentUseCase(repo, logger.Noop())

    result, err := uc.GetByID(context.Background(), "123")
    assert.NoError(t, err)
    assert.Equal(t, int64(50000), result.Amount)
}
```

**Tools:** `go test` (built-in), `testify` (assertions), `mockery`/`gomock` (generated mocks), `testcontainers-go` (integration)

### Naming Convention

| Jenis | Format | Contoh |
|-------|--------|--------|
| Package | lowercase, singular | `payment`, `user` |
| Interface | PascalCase | `PaymentRepository` |
| Struct/Impl | PascalCase | `PostgresPaymentRepo` |
| Constructor | `New` + Name | `NewPaymentUseCase` |
| File | snake_case + `_test.go` | `payment_repository.go` |
| Error var | `Err` prefix | `ErrPaymentNotFound` |

### CI/CD & Commands

```yaml
# .gitlab-ci.yml — stages: lint, test, build, deploy
lint:
  script: golangci-lint run ./...
test:
  script: go test -race -coverprofile=coverage.out ./...
build:
  script: CGO_ENABLED=0 go build -o bin/api ./cmd/api
```

```makefile
# Makefile
lint:       golangci-lint run ./...
test:       go test -race -cover ./...
build:      CGO_ENABLED=0 go build -o bin/api ./cmd/api
run:        go run ./cmd/api
migrate-up: goose -dir migrations postgres "$(DATABASE_URL)" up
```

---

## Profile 3: Python (FastAPI) — Clean Architecture

### Folder Structure

```
project-root/
├── src/
│   ├── domain/[domain]/            # CORE — business rules
│   │   ├── entity.py               # Pydantic/dataclass
│   │   ├── repository.py           # ABC interface
│   │   ├── usecase.py              # Use case interface
│   │   └── errors.py               # Domain exceptions
│   ├── usecase/[domain]/           # Use case implementations
│   │   └── usecase.py
│   ├── infrastructure/             # INFRA — adapters
│   │   ├── repository/[domain]/    # SQLAlchemy implementations
│   │   ├── http/                   # External API clients
│   │   ├── config.py               # pydantic-settings
│   │   └── di/container.py         # DI container
│   └── delivery/                   # PRESENTATION — transport
│       ├── api/v1/[domain].py      # Route handlers
│       ├── api/deps.py             # Depends() definitions
│       ├── dto/[domain].py         # Request/Response schemas
│       └── middleware/
├── tests/{unit,integration}/
├── migrations/                     # Alembic
├── main.py                         # Entry point
├── pyproject.toml                  # = package.json
└── ruff.toml
```

### Mapping Layer

| Framework Layer | Python Equivalent |
|----------------|-------------------|
| `core/` | `src/domain/` — ABC/Protocol + entity, zero dependency |
| `infrastructure/` | `src/infrastructure/` — SQLAlchemy, httpx, dll |
| `presentation/` | `src/delivery/` — FastAPI routes + DTOs |
| `app/` | `main.py` — Composition root |

### DI Approach

**Option A: `dependency-injector` (proyek besar)**

```python
# src/infrastructure/di/container.py
from dependency_injector import containers, providers

class Container(containers.DeclarativeContainer):
    config = providers.Configuration()
    db_session = providers.Singleton(create_session, url=config.database_url)
    payment_repository = providers.Singleton(SqlAlchemyPaymentRepo, session=db_session)
    get_payment_usecase = providers.Factory(GetPaymentUseCase, repository=payment_repository)
```

**Option B: FastAPI `Depends()` (proyek kecil-menengah)**

```python
# src/delivery/api/deps.py
def get_payment_repo(session: AsyncSession = Depends(get_db_session)) -> PaymentRepository:
    return SqlAlchemyPaymentRepo(session)

def get_payment_usecase(repo: PaymentRepository = Depends(get_payment_repo)) -> GetPaymentUseCase:
    return GetPaymentUseCase(repository=repo)
```

### Domain Layer Pattern

```python
# src/domain/payment/entity.py
@dataclass(frozen=True)
class Payment:
    id: str
    amount: Decimal
    status: str

# src/domain/payment/repository.py
class PaymentRepository(ABC):
    @abstractmethod
    async def get_by_id(self, payment_id: str) -> Payment | None: ...

# src/domain/payment/usecase.py
class GetPaymentUseCase(ABC):
    @abstractmethod
    async def execute(self, payment_id: str) -> Payment: ...
```

### Testing

```python
import pytest
from unittest.mock import AsyncMock

@pytest.fixture
def mock_repo():
    repo = AsyncMock(spec=PaymentRepository)
    repo.get_by_id.return_value = Payment(id="123", amount=Decimal("50000"), status="paid")
    return repo

@pytest.mark.asyncio
async def test_get_payment_success(mock_repo):
    uc = GetPaymentUseCaseImpl(repository=mock_repo)
    result = await uc.execute("123")
    assert result.amount == Decimal("50000")
    mock_repo.get_by_id.assert_called_once_with("123")
```

**Tools:** `pytest` + `pytest-asyncio`, `unittest.mock`/`pytest-mock`, `factory-boy`, `testcontainers`, `httpx` (test client)

### Naming Convention

| Jenis | Format | Contoh |
|-------|--------|--------|
| Module | snake_case | `payment`, `user_auth` |
| Class | PascalCase | `PaymentRepository` |
| Implementation | PascalCase + suffix | `SqlAlchemyPaymentRepo` |
| Function | snake_case | `get_by_id`, `execute` |
| File | snake_case / `test_` prefix | `payment_repository.py` |
| Exception | PascalCase + Error | `PaymentNotFoundError` |
| Constants | UPPER_SNAKE | `MAX_RETRY_COUNT` |

### CI/CD & Commands

```yaml
# .gitlab-ci.yml — stages: lint, test, build, deploy
lint:
  script:
    - ruff check src/ tests/
    - ruff format --check src/ tests/
    - mypy src/
test:
  script: pytest --cov=src --cov-report=term-missing tests/
build:
  script: docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
```

```bash
ruff check src/ tests/       # Lint
ruff format src/ tests/      # Format
mypy src/                    # Type check
pytest --cov=src             # Test + coverage
uvicorn main:app --reload    # Dev server
alembic upgrade head         # Migrations
```

### pyproject.toml (= package.json)

```toml
[tool.poetry.dependencies]
python = "^3.11"
fastapi = "^0.104.0"
uvicorn = "^0.24.0"
sqlalchemy = "^2.0"
pydantic-settings = "^2.0"
dependency-injector = "^4.41"

[tool.poetry.group.dev.dependencies]
pytest = "^7.4"
pytest-asyncio = "^0.23"
ruff = "^0.1"
mypy = "^1.7"
```

---

## Panduan Memilih Stack

| Kriteria | Next.js (TS) | Go | Python (FastAPI) |
|----------|-------------|-----|------------------|
| Full-stack web app | ✅ Ideal | ❌ | ⚠️ Backend only |
| High-performance API | ⚠️ | ✅ Ideal | ✅ Good |
| Microservices | ⚠️ | ✅ Ideal | ✅ Good |
| ML/Data pipeline | ❌ | ⚠️ | ✅ Ideal |
| Rapid prototyping | ✅ Good | ⚠️ | ✅ Ideal |

### Prinsip yang TETAP Berlaku di Semua Stack

1. **Dependency Rule** — inner layer tidak boleh depend ke outer layer
2. **Interface/Protocol** — core hanya mengenal abstraksi
3. **Composition Root** — wiring terpusat di satu tempat
4. **Testability** — mock via interface injection
5. **CI/CD Quality Gates** — lint + typecheck + test wajib pass

---

## Catatan

- Dokumen ini adalah **panduan adaptasi**, bukan pengganti `architecture-standards.md`
- Setiap stack boleh memiliki idiom sendiri selama **tidak melanggar dependency rule**
- Jika ragu: "Apakah inner layer saya bebas dari dependency ke outer layer?"
