---
inclusion: always
---

# Test Writing Patterns — Clean Architecture + DI

## Overview

Dokumen ini adalah **panduan testing WAJIB** untuk seluruh pengembangan proyek yang menggunakan Enterprise AI-Native SDLC Framework.

Semua AI Agent dan Developer **WAJIB mengikuti pattern testing dalam dokumen ini**. Testing bukan formalitas — testing adalah **jaminan bahwa arsitektur bekerja sesuai kontrak**.

### Peran Dokumen Ini dalam Framework

| Layer | Bagaimana Dokumen Ini Digunakan |
|-------|--------------------------------|
| Layer 3 (Spec-Driven Dev) | Specification HARUS menyertakan test scenario |
| Layer 6 (AI Skills) | Skill `create-test` HARUS mengikuti pattern di sini |
| Layer 8 (Issue-Driven Dev) | Setiap task HARUS menyertakan test yang comply |
| Layer 9 (Agent Orchestration) | Agent HARUS generate test sesuai pattern ini |
| Layer 11 (AI Review) | Review HARUS validasi test quality dan coverage |
| Layer 12 (Quality Gates) | CI/CD enforce coverage threshold dari dokumen ini |

**AI Agent Rules:**
- Saat membuat fitur baru → WAJIB buat test untuk setiap layer yang terlibat
- Saat fix bug → WAJIB buat test yang mereproduksi bug SEBELUM fix
- Saat refactor → WAJIB pastikan existing test tetap pass
- **JANGAN PERNAH** menghasilkan test yang hanya mengejar coverage tanpa assertion bermakna

---

## Test Pyramid Strategy

```
        ╱╲
       ╱E2E╲         ← Sedikit: critical path (login, payment, binding)
      ╱──────╲
     ╱Integration╲   ← Sedang: boundary (repo+adapter, route handler)
    ╱──────────────╲
   ╱   Unit Tests    ╲ ← Mayoritas: use case, mapper, ViewModel, helper
  ╱════════════════════╲
```

### Distribusi Target

| Level | Proporsi | Target |
|-------|----------|--------|
| Unit | ~70% | Use case, mapper, ViewModel, helper, middleware logic |
| Integration | ~20% | Repository + HTTP adapter, route handler, DI container |
| E2E | ~10% | Login flow, payment flow, critical redirect |

### Prinsip

1. **Unit test cepat dan terisolasi** — tidak ada network, tidak ada database
2. **Integration test validasi boundary** — mock external service, test real adapter
3. **E2E test validasi user journey** — test dari perspektif user

---

## Per-Layer Test Patterns

### 1. Use Case Tests

Use case adalah **inti business logic**. Test HARUS mencakup: happy path, error path, edge case.

**Pattern:** Mock repository, inject via factory function parameter.

```typescript
// src/core/domains/payment/usecases/GetPaymentDetailUseCase.ts
export interface GetPaymentDetailUseCase {
  execute: (paymentId: string) => Promise<DomainResult<PaymentDetail>>;
}

export const GetPaymentDetailUseCaseImpl = ({
  paymentRepository,
}: {
  paymentRepository: PaymentRepository;
}): GetPaymentDetailUseCase => ({
  execute: async (paymentId) => {
    if (!paymentId) {
      return { success: false, error: new InvalidParamError('paymentId') };
    }
    return paymentRepository.getPaymentDetail(paymentId);
  },
});
```

```typescript
// tests/unit/core/domains/payment/usecases/GetPaymentDetailUseCase.test.ts
import { describe, it, expect, vi } from 'vitest';
import { GetPaymentDetailUseCaseImpl } from '@/core/domains/payment/usecases/GetPaymentDetailUseCase';
import type { PaymentRepository } from '@/core/domains/payment/repositories/PaymentRepository';

describe('GetPaymentDetailUseCase', () => {
  // === SETUP: Mock repository ===
  const mockPaymentRepository: PaymentRepository = {
    getPaymentDetail: vi.fn(),
    updatePayment: vi.fn(),
  };

  const useCase = GetPaymentDetailUseCaseImpl({
    paymentRepository: mockPaymentRepository,
  });

  beforeEach(() => {
    vi.clearAllMocks();
  });

  // === HAPPY PATH ===
  it('should return payment detail when paymentId is valid', async () => {
    const mockPayment = {
      id: 'PAY-001',
      amount: 150000,
      status: 'completed',
    };

    vi.mocked(mockPaymentRepository.getPaymentDetail).mockResolvedValue({
      success: true,
      data: mockPayment,
    });

    const result = await useCase.execute('PAY-001');

    expect(result.success).toBe(true);
    expect(result.data).toEqual(mockPayment);
    expect(mockPaymentRepository.getPaymentDetail).toHaveBeenCalledWith('PAY-001');
    expect(mockPaymentRepository.getPaymentDetail).toHaveBeenCalledTimes(1);
  });

  // === ERROR PATH ===
  it('should return error when repository fails', async () => {
    vi.mocked(mockPaymentRepository.getPaymentDetail).mockResolvedValue({
      success: false,
      error: new Error('Network timeout'),
    });

    const result = await useCase.execute('PAY-001');

    expect(result.success).toBe(false);
    expect(result.error).toBeDefined();
  });

  // === EDGE CASE ===
  it('should return InvalidParamError when paymentId is empty', async () => {
    const result = await useCase.execute('');

    expect(result.success).toBe(false);
    expect(result.error).toBeInstanceOf(InvalidParamError);
    expect(mockPaymentRepository.getPaymentDetail).not.toHaveBeenCalled();
  });
});
```

**Checklist Use Case Test:**
- [ ] Happy path — data valid, repository sukses
- [ ] Error path — repository gagal, network error
- [ ] Edge case — parameter kosong, null, format salah
- [ ] Dependency failure — repository throw exception
- [ ] Validasi bahwa repository dipanggil dengan parameter yang benar

---

### 2. Repository Tests

Repository adalah **boundary antara domain dan infrastructure**. Test HARUS mencakup: mapping response, error handling, header/payload.

**Pattern:** Mock HTTP adapter, test bahwa DTO di-map ke domain entity dengan benar.

```typescript
// src/infrastructure/repositories/payment/PaymentRepositoryImpl.ts
export const PaymentRepositoryImpl = ({
  httpProtocol,
  paymentMapper,
}: {
  httpProtocol: HttpProtocol;
  paymentMapper: PaymentMapper;
}): PaymentRepository => ({
  getPaymentDetail: async (paymentId) => {
    try {
      const response = await httpProtocol.get<PaymentDTO>(
        `/api/payments/${paymentId}`
      );
      return {
        success: true,
        data: paymentMapper.toDomain(response.data),
      };
    } catch (error) {
      return {
        success: false,
        error: mapHttpError(error),
      };
    }
  },
});
```

```typescript
// tests/unit/infrastructure/repositories/payment/PaymentRepositoryImpl.test.ts
import { describe, it, expect, vi } from 'vitest';
import { PaymentRepositoryImpl } from '@/infrastructure/repositories/payment/PaymentRepositoryImpl';
import type { HttpProtocol } from '@/core/protocols/HttpProtocol';
import type { PaymentMapper } from '@/core/domains/payment/mappers/PaymentMapper';

describe('PaymentRepositoryImpl', () => {
  const mockHttpProtocol: HttpProtocol = {
    get: vi.fn(),
    post: vi.fn(),
    put: vi.fn(),
    delete: vi.fn(),
  };

  const mockPaymentMapper: PaymentMapper = {
    toDomain: vi.fn(),
    toDTO: vi.fn(),
  };

  const repository = PaymentRepositoryImpl({
    httpProtocol: mockHttpProtocol,
    paymentMapper: mockPaymentMapper,
  });

  beforeEach(() => {
    vi.clearAllMocks();
  });

  // === SUCCESS RESPONSE ===
  it('should call correct endpoint and map response to domain', async () => {
    const mockDTO = { payment_id: 'PAY-001', total_amount: 150000, payment_status: 'completed' };
    const mockDomain = { id: 'PAY-001', amount: 150000, status: 'completed' };

    vi.mocked(mockHttpProtocol.get).mockResolvedValue({ data: mockDTO });
    vi.mocked(mockPaymentMapper.toDomain).mockReturnValue(mockDomain);

    const result = await repository.getPaymentDetail('PAY-001');

    expect(mockHttpProtocol.get).toHaveBeenCalledWith('/api/payments/PAY-001');
    expect(mockPaymentMapper.toDomain).toHaveBeenCalledWith(mockDTO);
    expect(result).toEqual({ success: true, data: mockDomain });
  });

  // === ERROR RESPONSE ===
  it('should return mapped error when HTTP call fails', async () => {
    vi.mocked(mockHttpProtocol.get).mockRejectedValue({
      status: 404,
      message: 'Not Found',
    });

    const result = await repository.getPaymentDetail('PAY-INVALID');

    expect(result.success).toBe(false);
    expect(result.error).toBeDefined();
  });

  // === NETWORK ERROR ===
  it('should handle network timeout gracefully', async () => {
    vi.mocked(mockHttpProtocol.get).mockRejectedValue(
      new Error('Network timeout')
    );

    const result = await repository.getPaymentDetail('PAY-001');

    expect(result.success).toBe(false);
    expect(result.error).toBeDefined();
  });
});
```

**Checklist Repository Test:**
- [ ] Endpoint URL benar (path, query params)
- [ ] Request payload/header sesuai kontrak API
- [ ] Response DTO di-map ke domain entity via mapper
- [ ] HTTP error di-handle dan di-map ke domain error
- [ ] Network error (timeout, connection refused) di-handle gracefully

---

### 3. ViewModel Tests

ViewModel adalah **orchestrator antara UI dan use case**. Test HARUS mencakup: state changes, action calls, loading/error states.

**Pattern:** Mock use case, test bahwa state berubah sesuai flow.

```typescript
// src/presentation/features/payment/viewModel/usePaymentViewModel.ts
export const PaymentViewModel = ({
  getPaymentDetailUseCase,
}: {
  getPaymentDetailUseCase: GetPaymentDetailUseCase;
}) => {
  const [state, setState] = useState<PaymentViewState>({
    isLoading: false,
    payment: null,
    error: null,
  });

  const fetchPayment = async (paymentId: string) => {
    setState(prev => ({ ...prev, isLoading: true, error: null }));

    const result = await getPaymentDetailUseCase.execute(paymentId);

    if (result.success) {
      setState({ isLoading: false, payment: result.data, error: null });
    } else {
      setState({ isLoading: false, payment: null, error: result.error.message });
    }
  };

  return { state, fetchPayment };
};
```

```typescript
// tests/unit/presentation/features/payment/viewModel/usePaymentViewModel.test.ts
import { describe, it, expect, vi } from 'vitest';
import { renderHook, act, waitFor } from '@testing-library/react';
import { PaymentViewModel } from '@/presentation/features/payment/viewModel/usePaymentViewModel';
import type { GetPaymentDetailUseCase } from '@/core/domains/payment/usecases/GetPaymentDetailUseCase';

describe('PaymentViewModel', () => {
  const mockGetPaymentDetailUseCase: GetPaymentDetailUseCase = {
    execute: vi.fn(),
  };

  // Helper: render ViewModel sebagai hook
  const renderPaymentViewModel = () => {
    return renderHook(() =>
      PaymentViewModel({
        getPaymentDetailUseCase: mockGetPaymentDetailUseCase,
      })
    );
  };

  beforeEach(() => {
    vi.clearAllMocks();
  });

  // === INITIAL STATE ===
  it('should have correct initial state', () => {
    const { result } = renderPaymentViewModel();

    expect(result.current.state).toEqual({
      isLoading: false,
      payment: null,
      error: null,
    });
  });

  // === LOADING STATE ===
  it('should set isLoading true when fetching', async () => {
    vi.mocked(mockGetPaymentDetailUseCase.execute).mockImplementation(
      () => new Promise(() => {}) // never resolves — test loading state
    );

    const { result } = renderPaymentViewModel();

    act(() => {
      result.current.fetchPayment('PAY-001');
    });

    expect(result.current.state.isLoading).toBe(true);
    expect(result.current.state.error).toBeNull();
  });

  // === SUCCESS STATE ===
  it('should update payment state on success', async () => {
    const mockPayment = { id: 'PAY-001', amount: 150000, status: 'completed' };

    vi.mocked(mockGetPaymentDetailUseCase.execute).mockResolvedValue({
      success: true,
      data: mockPayment,
    });

    const { result } = renderPaymentViewModel();

    await act(async () => {
      await result.current.fetchPayment('PAY-001');
    });

    expect(result.current.state.isLoading).toBe(false);
    expect(result.current.state.payment).toEqual(mockPayment);
    expect(result.current.state.error).toBeNull();
  });

  // === ERROR STATE ===
  it('should update error state on failure', async () => {
    vi.mocked(mockGetPaymentDetailUseCase.execute).mockResolvedValue({
      success: false,
      error: { message: 'Payment not found' },
    });

    const { result } = renderPaymentViewModel();

    await act(async () => {
      await result.current.fetchPayment('PAY-INVALID');
    });

    expect(result.current.state.isLoading).toBe(false);
    expect(result.current.state.payment).toBeNull();
    expect(result.current.state.error).toBe('Payment not found');
  });
});
```

**Checklist ViewModel Test:**
- [ ] Initial state benar
- [ ] Loading state aktif saat action dipanggil
- [ ] Success state — data ter-update, loading off, error null
- [ ] Error state — error ter-set, loading off, data null/unchanged
- [ ] Use case dipanggil dengan parameter yang benar
- [ ] Multiple action calls tidak corrupt state

---

### 4. Mapper Tests

Mapper adalah **translator antara DTO dan domain entity**. Test HARUS mencakup: field mapping, fallback values, abnormal data.

**Pattern:** Pure function test — input DTO, assert domain output.

```typescript
// src/core/domains/payment/mappers/PaymentMapper.ts
export interface PaymentMapper {
  toDomain: (dto: PaymentDTO) => PaymentDetail;
  toDTO: (domain: PaymentDetail) => PaymentDTO;
}

export const PaymentMapperImpl = (): PaymentMapper => ({
  toDomain: (dto) => ({
    id: dto.payment_id,
    amount: dto.total_amount ?? 0,
    status: dto.payment_status ?? 'unknown',
    createdAt: dto.created_at ? new Date(dto.created_at) : new Date(),
    merchantName: dto.merchant?.name ?? 'Unknown Merchant',
  }),
  toDTO: (domain) => ({
    payment_id: domain.id,
    total_amount: domain.amount,
    payment_status: domain.status,
    created_at: domain.createdAt.toISOString(),
    merchant: { name: domain.merchantName },
  }),
});
```

```typescript
// tests/unit/core/domains/payment/mappers/PaymentMapper.test.ts
import { describe, it, expect } from 'vitest';
import { PaymentMapperImpl } from '@/core/domains/payment/mappers/PaymentMapper';

describe('PaymentMapper', () => {
  const mapper = PaymentMapperImpl();

  describe('toDomain', () => {
    // === NORMAL MAPPING ===
    it('should map all fields correctly from DTO to domain', () => {
      const dto = {
        payment_id: 'PAY-001',
        total_amount: 250000,
        payment_status: 'completed',
        created_at: '2024-01-15T10:30:00Z',
        merchant: { name: 'Toko ABC' },
      };

      const result = mapper.toDomain(dto);

      expect(result.id).toBe('PAY-001');
      expect(result.amount).toBe(250000);
      expect(result.status).toBe('completed');
      expect(result.createdAt).toEqual(new Date('2024-01-15T10:30:00Z'));
      expect(result.merchantName).toBe('Toko ABC');
    });

    // === FALLBACK VALUES ===
    it('should use fallback values when fields are null/undefined', () => {
      const dto = {
        payment_id: 'PAY-002',
        total_amount: null,
        payment_status: undefined,
        created_at: null,
        merchant: null,
      };

      const result = mapper.toDomain(dto as any);

      expect(result.id).toBe('PAY-002');
      expect(result.amount).toBe(0);
      expect(result.status).toBe('unknown');
      expect(result.createdAt).toBeInstanceOf(Date);
      expect(result.merchantName).toBe('Unknown Merchant');
    });

    // === ABNORMAL DATA ===
    it('should handle empty merchant object gracefully', () => {
      const dto = {
        payment_id: 'PAY-003',
        total_amount: 100000,
        payment_status: 'pending',
        created_at: '2024-01-15T10:30:00Z',
        merchant: {},
      };

      const result = mapper.toDomain(dto as any);

      expect(result.merchantName).toBe('Unknown Merchant');
    });
  });

  describe('toDTO', () => {
    it('should map domain entity back to DTO format', () => {
      const domain = {
        id: 'PAY-001',
        amount: 250000,
        status: 'completed',
        createdAt: new Date('2024-01-15T10:30:00Z'),
        merchantName: 'Toko ABC',
      };

      const result = mapper.toDTO(domain);

      expect(result.payment_id).toBe('PAY-001');
      expect(result.total_amount).toBe(250000);
      expect(result.payment_status).toBe('completed');
      expect(result.merchant.name).toBe('Toko ABC');
    });
  });
});
```

**Checklist Mapper Test:**
- [ ] Semua field ter-map dengan benar (nama field DTO → domain)
- [ ] Fallback/default value bekerja saat field null/undefined
- [ ] Data abnormal (empty object, missing nested) tidak throw error
- [ ] Reverse mapping (toDTO) konsisten dengan toDomain

---

### 5. Middleware Tests

Middleware menangani **cross-cutting concerns** (auth, redirect, logging). Test HARUS mencakup: auth flow, redirect validation, safe fallback.

**Pattern:** Mock request/response objects, test routing decisions.

```typescript
// src/app/middleware.ts (simplified for testing)
export const createAuthMiddleware = ({
  validateTokenUseCase,
}: {
  validateTokenUseCase: ValidateTokenUseCase;
}) => {
  return async (request: NextRequest): Promise<NextResponse> => {
    const token = request.cookies.get('auth_token')?.value;

    if (!token) {
      return NextResponse.redirect(new URL('/login', request.url));
    }

    const result = await validateTokenUseCase.execute(token);

    if (!result.success) {
      const response = NextResponse.redirect(new URL('/login', request.url));
      response.cookies.delete('auth_token');
      return response;
    }

    return NextResponse.next();
  };
};
```

```typescript
// tests/unit/app/middleware.test.ts
import { describe, it, expect, vi } from 'vitest';
import { createAuthMiddleware } from '@/app/middleware';
import { NextRequest, NextResponse } from 'next/server';
import type { ValidateTokenUseCase } from '@/core/domains/auth/usecases/ValidateTokenUseCase';

// Mock Next.js server modules
vi.mock('next/server', () => ({
  NextRequest: vi.fn(),
  NextResponse: {
    next: vi.fn(() => ({ type: 'next' })),
    redirect: vi.fn((url) => ({
      type: 'redirect',
      url,
      cookies: { delete: vi.fn() },
    })),
  },
}));

describe('AuthMiddleware', () => {
  const mockValidateTokenUseCase: ValidateTokenUseCase = {
    execute: vi.fn(),
  };

  const middleware = createAuthMiddleware({
    validateTokenUseCase: mockValidateTokenUseCase,
  });

  const createMockRequest = (token?: string) => ({
    cookies: {
      get: vi.fn((name: string) =>
        name === 'auth_token' && token ? { value: token } : undefined
      ),
    },
    url: 'https://app.example.com/dashboard',
  } as unknown as NextRequest);

  beforeEach(() => {
    vi.clearAllMocks();
  });

  // === NO TOKEN → REDIRECT ===
  it('should redirect to login when no auth token', async () => {
    const request = createMockRequest(undefined);

    const result = await middleware(request);

    expect(result.type).toBe('redirect');
    expect(NextResponse.redirect).toHaveBeenCalled();
  });

  // === VALID TOKEN → NEXT ===
  it('should allow request when token is valid', async () => {
    const request = createMockRequest('valid-token-123');

    vi.mocked(mockValidateTokenUseCase.execute).mockResolvedValue({
      success: true,
      data: { userId: 'user-1', role: 'admin' },
    });

    const result = await middleware(request);

    expect(result.type).toBe('next');
    expect(mockValidateTokenUseCase.execute).toHaveBeenCalledWith('valid-token-123');
  });

  // === INVALID TOKEN → REDIRECT + CLEAR COOKIE ===
  it('should redirect and clear cookie when token is invalid', async () => {
    const request = createMockRequest('expired-token');

    vi.mocked(mockValidateTokenUseCase.execute).mockResolvedValue({
      success: false,
      error: { message: 'Token expired' },
    });

    const result = await middleware(request);

    expect(result.type).toBe('redirect');
    expect(result.cookies.delete).toHaveBeenCalledWith('auth_token');
  });

  // === USE CASE THROWS → SAFE FALLBACK ===
  it('should redirect safely when validateToken throws unexpected error', async () => {
    const request = createMockRequest('some-token');

    vi.mocked(mockValidateTokenUseCase.execute).mockRejectedValue(
      new Error('Unexpected service error')
    );

    // Middleware harus handle gracefully, bukan crash
    await expect(middleware(request)).resolves.toBeDefined();
  });
});
```

**Checklist Middleware Test:**
- [ ] No token → redirect ke login
- [ ] Valid token → request diteruskan (next)
- [ ] Invalid/expired token → redirect + clear cookie
- [ ] Unexpected error → safe fallback (tidak crash)
- [ ] Protected routes benar-benar terproteksi
- [ ] Public routes tidak kena middleware

---


## DI-Friendly Testing Patterns

### Prinsip Utama

Testing di Clean Architecture + DI **TIDAK bergantung pada container**. Dependency di-inject langsung via factory function parameter.

### Pattern 1: Direct Injection (Unit Test — UTAMA)

Inject mock langsung ke factory function. **Ini adalah pattern default untuk semua unit test.**

```typescript
// ✅ BENAR: Inject mock langsung via factory parameter
const useCase = GetPaymentDetailUseCaseImpl({
  paymentRepository: mockPaymentRepository,
});

// ✅ BENAR: Repository dengan multiple dependencies
const repository = PaymentRepositoryImpl({
  httpProtocol: mockHttpProtocol,
  paymentMapper: mockPaymentMapper,
});

// ✅ BENAR: ViewModel dengan use case mock
const viewModel = PaymentViewModel({
  getPaymentDetailUseCase: mockGetPaymentDetailUseCase,
  updatePaymentUseCase: mockUpdatePaymentUseCase,
});
```

```typescript
// ❌ SALAH: Jangan resolve dari container di unit test
import { container } from '@/infrastructure/di/container';
const useCase = container.resolve('getPaymentDetailUseCase'); // ❌
```

### Pattern 2: Test Fixtures / Factory Helpers

Buat helper untuk mengurangi boilerplate di test files.

```typescript
// tests/fixtures/payment.fixtures.ts
import type { PaymentRepository } from '@/core/domains/payment/repositories/PaymentRepository';
import type { PaymentDetail } from '@/core/domains/payment/entities/PaymentDetail';
import { vi } from 'vitest';

// === Mock Factory ===
export const createMockPaymentRepository = (
  overrides?: Partial<PaymentRepository>
): PaymentRepository => ({
  getPaymentDetail: vi.fn(),
  updatePayment: vi.fn(),
  listPayments: vi.fn(),
  ...overrides,
});

// === Data Factory ===
export const createPaymentDetail = (
  overrides?: Partial<PaymentDetail>
): PaymentDetail => ({
  id: 'PAY-001',
  amount: 150000,
  status: 'completed',
  createdAt: new Date('2024-01-15T10:30:00Z'),
  merchantName: 'Toko ABC',
  ...overrides,
});

// === Result Factory ===
export const createSuccessResult = <T>(data: T) => ({
  success: true as const,
  data,
});

export const createErrorResult = (message: string) => ({
  success: false as const,
  error: { message },
});
```

```typescript
// Penggunaan di test file
import {
  createMockPaymentRepository,
  createPaymentDetail,
  createSuccessResult,
} from '@tests/fixtures/payment.fixtures';

describe('GetPaymentDetailUseCase', () => {
  const mockRepo = createMockPaymentRepository();
  const useCase = GetPaymentDetailUseCaseImpl({ paymentRepository: mockRepo });

  it('should return payment detail', async () => {
    const payment = createPaymentDetail({ amount: 500000 });
    vi.mocked(mockRepo.getPaymentDetail).mockResolvedValue(
      createSuccessResult(payment)
    );

    const result = await useCase.execute('PAY-001');

    expect(result.data?.amount).toBe(500000);
  });
});
```

### Pattern 3: Container Override (Integration Test ONLY)

Container override **HANYA** untuk integration test yang perlu validasi wiring DI.

```typescript
// tests/integration/di/payment-wiring.test.ts
import { describe, it, expect } from 'vitest';
import { createContainer, asFunction } from 'awilix';
import { GetPaymentDetailUseCaseImpl } from '@/core/domains/payment/usecases/GetPaymentDetailUseCase';
import { PaymentRepositoryImpl } from '@/infrastructure/repositories/payment/PaymentRepositoryImpl';

describe('Payment DI Wiring', () => {
  it('should resolve use case with all dependencies', () => {
    const testContainer = createContainer();

    // Register real implementations dengan mock adapter
    testContainer.register({
      httpProtocol: asFunction(() => createMockHttpProtocol()).singleton(),
      paymentMapper: asFunction(PaymentMapperImpl).singleton(),
      paymentRepository: asFunction(PaymentRepositoryImpl).singleton(),
      getPaymentDetailUseCase: asFunction(GetPaymentDetailUseCaseImpl).singleton(),
    });

    const useCase = testContainer.resolve('getPaymentDetailUseCase');

    expect(useCase).toBeDefined();
    expect(useCase.execute).toBeInstanceOf(Function);
  });
});
```

### Kapan Menggunakan Pattern Mana

| Situasi | Pattern | Alasan |
|---------|---------|--------|
| Test use case logic | Direct Injection | Isolasi penuh, cepat |
| Test repository mapping | Direct Injection | Mock HTTP, test mapper |
| Test ViewModel state | Direct Injection | Mock use case |
| Test DI wiring benar | Container Override | Validasi registration |
| Test full flow (repo → API) | Container Override | Validasi integration |
| E2E test | Real container | Test real behavior |

---

## Anti-Pattern Testing (DILARANG)

### 1. ❌ Test yang Mengejar Coverage Tanpa Assertion Bermakna

```typescript
// ❌ SALAH: Test ini 100% coverage tapi tidak memvalidasi apa-apa
it('should call execute', async () => {
  await useCase.execute('PAY-001');
  // Tidak ada assertion! Apa yang divalidasi?
});

// ❌ SALAH: Assertion terlalu lemah
it('should return something', async () => {
  const result = await useCase.execute('PAY-001');
  expect(result).toBeDefined(); // Ini tidak membuktikan apa-apa
});
```

```typescript
// ✅ BENAR: Assertion spesifik dan bermakna
it('should return payment with correct amount after discount', async () => {
  const result = await useCase.execute('PAY-001');

  expect(result.success).toBe(true);
  expect(result.data.amount).toBe(135000); // 150000 - 10% discount
  expect(result.data.discountApplied).toBe(true);
});
```

### 2. ❌ Over-Mocking yang Tidak Memvalidasi Perilaku Nyata

```typescript
// ❌ SALAH: Mock terlalu dalam, test tidak membuktikan behavior
it('should process payment', async () => {
  vi.mocked(useCase.execute).mockResolvedValue({ success: true, data: {} });

  const result = await useCase.execute('PAY-001');

  expect(result.success).toBe(true);
  // Kita hanya test mock kita sendiri, bukan logic use case!
});
```

```typescript
// ✅ BENAR: Mock dependency (repository), test subject (use case) secara real
it('should process payment', async () => {
  vi.mocked(mockRepo.getPaymentDetail).mockResolvedValue({
    success: true,
    data: createPaymentDetail(),
  });

  // Use case REAL, repository MOCK
  const result = await useCase.execute('PAY-001');

  expect(result.success).toBe(true);
  expect(mockRepo.getPaymentDetail).toHaveBeenCalledWith('PAY-001');
});
```

### 3. ❌ Snapshot Berlebihan untuk Komponen Kompleks

```typescript
// ❌ SALAH: Snapshot besar, rapuh, sulit di-review
it('should render payment page', () => {
  const { container } = render(<PaymentScreen />);
  expect(container).toMatchSnapshot(); // 500+ lines snapshot
});
```

```typescript
// ✅ BENAR: Test behavior, bukan structure
it('should display payment amount formatted as currency', () => {
  render(<PaymentScreen payment={createPaymentDetail({ amount: 150000 })} />);

  expect(screen.getByText('Rp 150.000')).toBeInTheDocument();
});

it('should show error message when payment fails', () => {
  render(<PaymentScreen error="Payment gagal" />);

  expect(screen.getByRole('alert')).toHaveTextContent('Payment gagal');
});
```

### 4. ❌ Test Rapuh yang Bergantung pada Detail DOM

```typescript
// ❌ SALAH: Bergantung pada class name dan DOM structure
it('should render button', () => {
  const { container } = render(<PaymentButton />);
  const btn = container.querySelector('div.wrapper > button.btn-primary.mt-4');
  expect(btn).toBeTruthy();
});
```

```typescript
// ✅ BENAR: Gunakan accessible queries
it('should render submit payment button', () => {
  render(<PaymentButton />);

  expect(screen.getByRole('button', { name: /bayar sekarang/i })).toBeInTheDocument();
});
```

### 5. ❌ Shared Mutable State Antar Test

```typescript
// ❌ SALAH: State bocor antar test
let paymentData = { amount: 0 };

describe('PaymentUseCase', () => {
  it('test 1 — modifies shared state', () => {
    paymentData.amount = 100000; // Mengubah shared state
    expect(paymentData.amount).toBe(100000);
  });

  it('test 2 — depends on test 1 state', () => {
    // BAHAYA: Test ini bergantung pada urutan eksekusi!
    expect(paymentData.amount).toBe(100000);
  });
});
```

```typescript
// ✅ BENAR: Setiap test punya state sendiri
describe('PaymentUseCase', () => {
  let paymentData: PaymentDetail;

  beforeEach(() => {
    paymentData = createPaymentDetail(); // Fresh state setiap test
  });

  it('test 1', () => {
    paymentData.amount = 100000;
    expect(paymentData.amount).toBe(100000);
  });

  it('test 2 — independent dari test 1', () => {
    expect(paymentData.amount).toBe(150000); // Default dari factory
  });
});
```

---

## Test Naming Convention

### Format

```
[should/when] + [expected behavior] + [condition/context]
```

### Contoh

```typescript
// Use Case
it('should return payment detail when paymentId is valid')
it('should return InvalidParamError when paymentId is empty')
it('should propagate repository error when network fails')

// Repository
it('should call GET /api/payments/:id with correct path')
it('should map DTO to domain entity correctly')
it('should return NetworkError when request times out')

// ViewModel
it('should set isLoading true when fetchPayment is called')
it('should update payment state on successful fetch')
it('should clear previous error when retrying')

// Mapper
it('should map payment_id to id')
it('should use 0 as fallback when total_amount is null')
it('should handle missing merchant object gracefully')

// Middleware
it('should redirect to /login when auth token is missing')
it('should allow request when token is valid')
it('should clear cookie when token is expired')
```

### Rules

1. **Gunakan bahasa Inggris** untuk test name (konsistensi dengan code)
2. **Mulai dengan `should`** untuk behavior description
3. **Sertakan kondisi** (`when`, `if`, `given`) untuk clarity
4. **Hindari nama generik** seperti `it works`, `test 1`, `handles error`
5. **Satu assertion focus per test** — jika perlu banyak assertion, pastikan satu tema

---

## Coverage Rules

### Threshold Minimum (WAJIB)

| Metrik | Minimum | Enforcement |
|--------|---------|-------------|
| Statements | >= 80% | CI block merge |
| Branches | >= 80% | CI block merge |
| Functions | >= 80% | CI block merge |
| Lines | >= 80% | CI block merge |

### Threshold Lebih Tinggi (Domain Kritis)

| Domain | Target | Alasan |
|--------|--------|--------|
| Auth / Token | >= 95% | Security-critical, semua path harus tercover |
| Payment / Transaction | >= 95% | Financial-critical, edge case harus tertest |
| Redirect / Routing | >= 90% | User flow-critical, salah redirect = UX rusak |
| Middleware | >= 90% | Cross-cutting, impact luas |
| Mapper (financial) | >= 95% | Data integrity, salah map = data corrupt |

### Yang TIDAK Perlu 100% Coverage

- UI component styling (visual)
- Third-party library wrapper (sudah ditest oleh library)
- Generated code (types, constants)
- Development-only utilities

### Coverage Bukan Segalanya

Coverage tinggi **BUKAN jaminan** test berkualitas. Prioritaskan:

1. **Assertion bermakna** > coverage number
2. **Edge case tercover** > happy path berulang
3. **Error handling tertest** > success path saja
4. **Behavior validated** > implementation detail checked

---

## Vitest Configuration Best Practices

### Setup File (`vitest.setup.ts`)

```typescript
// vitest.setup.ts
import { afterEach, vi } from 'vitest';
import { cleanup } from '@testing-library/react';
import '@testing-library/jest-dom/vitest';

// Cleanup DOM setelah setiap test
afterEach(() => {
  cleanup();
});

// Reset semua mock setelah setiap test
afterEach(() => {
  vi.restoreAllMocks();
});
```

### Vitest Config (`vitest.config.ts`)

```typescript
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./vitest.setup.ts'],
    // Isolasi antar test file
    isolate: true,
    // Disable retry — test harus deterministic
    retry: 0,
    // Clear mock state
    clearMocks: true,
    restoreMocks: true,
    // Coverage config
    coverage: {
      provider: 'v8',
      reporter: ['text', 'json', 'json-summary', 'html', 'cobertura'],
      include: ['src/**/*.{ts,tsx}'],
      exclude: [
        'src/**/*.d.ts',
        'src/**/*.test.{ts,tsx}',
        'src/**/*.spec.{ts,tsx}',
        'src/**/index.ts', // barrel files
        'src/app/**/layout.tsx',
        'src/app/**/loading.tsx',
        'src/infrastructure/di/**', // DI registration
      ],
      thresholds: {
        statements: 80,
        branches: 80,
        functions: 80,
        lines: 80,
      },
    },
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@tests': path.resolve(__dirname, './tests'),
    },
  },
});
```

### Isolated QueryClient (React Query)

```typescript
// tests/utils/test-utils.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { render, type RenderOptions } from '@testing-library/react';
import type { ReactElement } from 'react';

// QueryClient BARU untuk setiap test — mencegah cache bocor
const createTestQueryClient = () =>
  new QueryClient({
    defaultOptions: {
      queries: {
        retry: false, // Disable retry di test
        gcTime: 0, // Immediate garbage collection
      },
      mutations: {
        retry: false,
      },
    },
  });

export const renderWithProviders = (
  ui: ReactElement,
  options?: Omit<RenderOptions, 'wrapper'>
) => {
  const testQueryClient = createTestQueryClient();

  const Wrapper = ({ children }: { children: React.ReactNode }) => (
    <QueryClientProvider client={testQueryClient}>
      {children}
    </QueryClientProvider>
  );

  return {
    ...render(ui, { wrapper: Wrapper, ...options }),
    queryClient: testQueryClient,
  };
};
```

### Reset Zustand State

```typescript
// tests/utils/zustand-reset.ts
import { act } from '@testing-library/react';

// Helper untuk reset Zustand store di test
export const resetZustandStore = (useStore: any) => {
  const initialState = useStore.getState();
  act(() => {
    useStore.setState(initialState, true);
  });
};

// Atau gunakan di afterEach
// afterEach(() => {
//   resetZustandStore(usePaymentStore);
// });
```

### Best Practices Ringkasan

| Setting | Value | Alasan |
|---------|-------|--------|
| `isolate: true` | Wajib | Mencegah state bocor antar file |
| `retry: 0` | Wajib | Test harus deterministic, bukan flaky |
| `clearMocks: true` | Wajib | Mock state bersih setiap test |
| `restoreMocks: true` | Wajib | Spy/stub di-restore otomatis |
| QueryClient per test | Wajib | Cache tidak bocor antar test |
| Zustand reset | Wajib | Global state bersih setiap test |
| `environment: 'jsdom'` | Untuk UI test | DOM simulation |
| `environment: 'node'` | Untuk logic test | Lebih cepat |

---

## Integration dengan Framework

### Referensi Silang

| Layer Framework | Relevansi Dokumen Ini |
|-----------------|----------------------|
| Layer 3 (Spec-Driven Dev) | Spec HARUS menyertakan test scenario per acceptance criteria |
| Layer 6 (AI Skills) | Skill `create-test` mengikuti pattern di dokumen ini |
| Layer 8 (Issue-Driven Dev) | Setiap task HARUS punya test yang comply |
| Layer 9 (Agent Orchestration) | Agent generate test sesuai pattern ini |
| Layer 11 (AI Review) | Review checklist mengecek test quality |
| Layer 12 (Quality Gates) | CI/CD enforce coverage threshold |
| Architecture Standards | Test pattern mengikuti dependency rule |

### AI Agent Workflow: Menulis Test

```
1. Identifikasi layer yang ditest (use case / repository / ViewModel / mapper / middleware)
2. Pilih pattern yang sesuai dari dokumen ini
3. Buat mock untuk dependency LANGSUNG (satu level)
4. Tulis test: happy path → error path → edge case
5. Pastikan naming convention sesuai
6. Jalankan test, pastikan pass
7. Cek coverage, pastikan >= threshold
```

### Hubungan dengan Architecture Standards

- **Dependency Rule berlaku di test** — test use case mock repository, BUKAN mock HTTP
- **Factory function pattern** memudahkan injection mock tanpa container
- **Layer boundary** menentukan apa yang di-mock dan apa yang di-test real
- **DI container TIDAK digunakan di unit test** — hanya di integration test

---

## Enforcement

Semua AI Agent dan Developer:

- **WAJIB** menulis test untuk setiap fitur/fix baru
- **WAJIB** mengikuti pattern per-layer di dokumen ini
- **WAJIB** menggunakan direct injection (bukan container) untuk unit test
- **WAJIB** mencapai coverage threshold minimum
- **WAJIB** menulis assertion bermakna, bukan sekedar coverage
- **DILARANG** menulis test yang bergantung pada shared mutable state
- **DILARANG** menggunakan snapshot untuk komponen kompleks
- **DILARANG** mock subject yang sedang ditest
- **DILARANG** skip test untuk "nanti saja"

Jika ada konflik antara kecepatan development vs kualitas test:
👉 **kualitas test harus diprioritaskan**

Test yang baik adalah **investasi**, bukan overhead.
