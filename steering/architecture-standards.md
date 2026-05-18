---
inclusion: always
---

# Architecture Standards — Clean Architecture + DI Pattern

## Overview

Dokumen ini adalah **engineering guideline WAJIB** untuk seluruh pengembangan proyek yang menggunakan Enterprise AI-Native SDLC Framework.

Arsitektur yang digunakan adalah **Clean Architecture** dengan pendekatan **Domain-Driven Design (DDD)** pada **Next.js App Router**, menggunakan **Awilix** sebagai DI container.

Semua AI Agent dan Developer **WAJIB mengikuti aturan dalam dokumen ini**. Jika ada konflik dengan dokumen lain, dokumen ini adalah **sumber kebenaran utama** untuk keputusan arsitektur dan DI.

### Peran Dokumen Ini dalam Framework

Dokumen ini berfungsi sebagai **acuan arsitektur utama** yang WAJIB dirujuk saat mengerjakan **setiap layer** dalam Enterprise AI-Native SDLC Framework:

| Layer | Bagaimana Dokumen Ini Digunakan |
|-------|--------------------------------|
| Layer 3 (Spec-Driven Dev) | Specification HARUS mengikuti struktur layer dan naming convention di sini |
| Layer 4 (Design System) | Technical design HARUS comply dengan dependency rule dan data flow |
| Layer 6 (AI Skills) | Setiap skill/workflow HARUS menghasilkan code yang sesuai arsitektur ini |
| Layer 7 (Team Extension) | Team standards TIDAK BOLEH bertentangan dengan aturan di sini |
| Layer 8 (Issue-Driven Dev) | Setiap task/issue HARUS menghasilkan code yang comply |
| Layer 9 (Agent Orchestration) | Semua agent HARUS mengikuti pattern ini saat generate code |
| Layer 11 (AI Review) | Review checklist HARUS mengecek compliance terhadap dokumen ini |
| Layer 12 (Quality Gates) | CI/CD HARUS enforce dependency rule dan structure |

**AI Agent Rules:**
- Saat membuat fitur baru → ikuti "Workflow Membuat Fitur Baru" di bawah
- Saat membuat file baru → ikuti "Naming Convention" dan letakkan di layer yang benar
- Saat register dependency → ikuti "Registration Pattern" dan "Lifetime Rules"
- Saat review code → validasi terhadap "Aturan Dependency" dan "Anti-Pattern"
- **JANGAN PERNAH** menghasilkan code yang melanggar dependency rule, meskipun user meminta shortcut

---

## Tujuan Arsitektur

* Menjaga **separation of concerns**
* Memastikan **dependency flow satu arah**
* Meningkatkan **testability dan maintainability**
* Mempermudah **scalability dan extensibility**
* Mencegah **tight coupling terhadap framework / library**
* Memisahkan kontrak dari implementasi
* Mempermudah mocking dalam test

---

## Prinsip Utama

### 1. Dependency Rule (WAJIB)

Layer dalam **TIDAK BOLEH** bergantung ke layer luar.

```
app → presentation → core
             ↘
        infrastructure
```

### 2. Separation of Concerns

Setiap layer memiliki tanggung jawab spesifik dan tidak boleh overlap.

### 3. Dependency Injection via Awilix

Semua dependency **WAJIB di-resolve melalui DI container (Awilix)**.

❌ Dilarang:
```ts
import axios from 'axios'
```

✅ Wajib:
```ts
const http = DI.resolve('HttpProtocol')
```

### 4. Protocol-based Abstraction

Core hanya mengenal **interface (protocol)**, bukan implementasi.

### 5. Framework as Detail

Next.js, Axios, Zustand, dsb adalah **detail**, bukan pusat arsitektur.

### 6. Composition Root Terpusat

Semua registration dependency HARUS dilakukan di `src/infrastructure/di/`.

---

## Struktur Layer

```
src/
├── core/                               # LAYER 1: Business Logic (paling dalam)
│   ├── domains/
│   │   └── [domain]/
│   │       ├── entities/               # Domain entities / value objects
│   │       ├── repositories/           # Repository interface ONLY
│   │       ├── usecases/               # Business logic utama
│   │       ├── errors/                 # Domain error
│   │       └── mappers/                # Mapping logic (optional)
│   │
│   ├── protocols/                      # Interface global (http, storage, dll)
│   └── utils/                          # Pure utilities (no external deps)
│
├── infrastructure/                     # LAYER 2: Implementation
│   ├── di/
│   │   ├── container.ts               # Root container (Awilix)
│   │   └── registry/
│   │       ├── moduleContainer.ts      # Shared modules/protocol implementation
│   │       ├── repositoryContainer.ts  # Repository implementation registration
│   │       ├── useCasesContainer.ts    # Use case registration
│   │       ├── viewModelContainer.ts   # ViewModel registration
│   │       ├── loggingContainer.ts     # Logger/analytics registration
│   │       └── index.ts               # Orchestration register semua module
│   │
│   ├── networking/                     # HTTP adapter (axios/fetch)
│   ├── repositories/                   # Repository implementation
│   │   └── [domain]/
│   ├── storage/                        # localStorage/cookie adapter
│   ├── devices/                        # Device adapter
│   ├── logging/                        # Logger + metrics
│   ├── stateManagement/                # Zustand (shared app state)
│   └── utils/                          # Infrastructure utilities
│
├── presentation/                       # LAYER 3: UI
│   ├── features/
│   │   └── [featureName]/
│   │       ├── screens/                # Page-level UI
│   │       ├── components/             # Feature components
│   │       ├── viewModel/              # UI state orchestration
│   │       ├── mappers/                # View data mapping
│   │       └── validation/             # Form validation
│   │
│   ├── components/                     # Shared components (Atomic Design)
│   │   ├── atoms/
│   │   ├── molecules/
│   │   └── organisms/
│   │
│   ├── hooks/
│   ├── providers/
│   └── locales/                        # i18n translations
│   ├── locales/
│   └── utils/
│
└── app/                                # Next.js App Router (composition layer)
    ├── layout.tsx
    ├── page.tsx
    └── [route]/
        └── page.tsx
```

---

## Tanggung Jawab Layer

### CORE (Business Rules)

✅ Boleh: entity, value object, use case, repository interface, domain rules

❌ Dilarang: React, Next.js, API call, localStorage, analytics, library eksternal, import container, resolve dependency

### INFRASTRUCTURE (Implementation)

✅ Boleh: HTTP client, repository implementation, storage adapter, analytics, Zustand store, register dependency, bind contract → implementation

❌ Dilarang: business rule inti, logic domain

### PRESENTATION (UI)

✅ Boleh: component, screen, view model, UI state, form validation, resolve ViewModel dari DI

❌ Dilarang: API call langsung, business logic inti, repository implementation, resolve repository/http adapter langsung, import implementation infra langsung

#### Server-First Rendering (Next.js)
- Default: gunakan **Server Component**
- Gunakan Client Component (`use client`) hanya jika ada interaksi atau state lokal
- Minimalkan `use client` untuk menjaga bundle size

#### Atomic Design (Shared Components)
- `atoms/` → elemen dasar (button, input, label)
- `molecules/` → kombinasi atoms (form field, search bar)
- `organisms/` → komponen kompleks (header, sidebar, card list)
- Shared component harus reusable dan tidak mengandung business logic

### APP (Routing Layer)

✅ Boleh: routing, layout, loading, error boundary

❌ Dilarang: business logic, data mapping kompleks

---

## Aturan Dependency (STRICT)

| From | Boleh Akses |
|------|-------------|
| core | ❌ tidak boleh ke mana pun |
| infrastructure | core |
| presentation | core |
| app | presentation |

❌ DILARANG:
- core → infrastructure
- core → presentation
- presentation → infrastructure (langsung)

---

## Data Flow (WAJIB)

```
UI (Screen)
  → ViewModel
  → UseCase
  → Repository (interface)
  → Repository Implementation
  → API / Storage
  → DomainResult<T>
  → UI
```

### Aturan Mapping Data

- API response = **DTO**
- DTO tidak boleh langsung ke UI
- Mapping dilakukan di repository / mapper

```
DTO → Domain Entity → View Model → UI
```

---

## Dependency Injection (Awilix)

### Struktur DI

```
src/infrastructure/di/
├── container.ts               # Root container
└── registry/
    ├── moduleContainer.ts     # Protocol implementations
    ├── repositoryContainer.ts # Repository implementations
    ├── useCasesContainer.ts   # Use case registrations
    ├── viewModelContainer.ts  # ViewModel registrations
    ├── loggingContainer.ts    # Logger/analytics
    └── index.ts              # Orchestration
```

### Factory Function Pattern (WAJIB)

Semua dependency utama HARUS berbentuk **factory function**:

```ts
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

Rules:
- Dependency diterima via parameter object
- Jangan ambil dependency dari global scope
- Jangan import container ke dalam use case/repository

### Registration Pattern

```ts
// Module / Protocol
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

### Registration Rules

1. **Semua dependency baru WAJIB diregistrasikan** di registry yang sesuai
2. **Nama registration konsisten** — gunakan camelCase: `httpProtocol`, `paymentRepository`, `validateTokenUseCase`
3. **Satu registration, satu tanggung jawab** — jangan nama ambigu (`service`, `utils`, `handler`)

### Lifetime Rules

| Lifetime | Kapan Digunakan | Contoh |
|----------|----------------|--------|
| **Singleton** | Dependency stateless, aman dibagi | config, logger, repository impl, use case |
| **Scoped** | Per-request / per-scope | request-bound context, header metadata |
| **Transient** | Instance baru setiap resolve | object dengan mutable state lokal |

Default aman: **singleton** untuk dependency stateless.

### Resolve Pattern

```ts
// Screen resolve ViewModel
const vm = DI.resolve('usePaymentViewModel');
```

Rules:
- Resolve sedekat mungkin dengan composition boundary
- Jangan resolve di core
- Jangan resolve berulang tanpa perlu
- Component kecil/reusable: terima props dari parent, jangan resolve sendiri

### DI di Presentation Layer

**Screen** ✅ Boleh: resolve ViewModel, panggil method ViewModel, render state

**Screen** ❌ Dilarang: resolve repository, resolve http adapter, resolve utility infra

**Component kecil** → terima props dari parent/screen/ViewModel, jangan resolve sendiri

### DI dan Clean Architecture (Relationship)

DI adalah alat untuk **menegakkan arsitektur**, bukan untuk membypass arsitektur.

❌ Salah:
- Component presentation resolve `paymentRepository`
- Core use case import `container`
- Mapper resolve logger dari DI

✅ Benar:
- Repository di-inject ke use case
- Use case di-inject ke ViewModel
- ViewModel di-resolve di screen/container

---

## Naming Convention (WAJIB)

| Jenis | Format | Contoh |
|-------|--------|--------|
| Use Case | PascalCase + UseCase | `GetPaymentUseCase.ts` |
| Repository Interface | PascalCase + Repository | `PaymentRepository.ts` |
| Repository Impl | PascalCase + RepositoryImpl | `PaymentRepositoryImpl.ts` |
| ViewModel | camelCase + ViewModel | `usePaymentViewModel.ts` |
| Screen | PascalCase + Screen | `PaymentScreen.tsx` |
| Component | PascalCase | `PaymentCard.tsx` |
| DI Key | camelCase | `paymentRepository`, `validateTokenUseCase` |
| Factory/Impl | PascalCase + Impl | `ValidateTokenUseCaseImpl` |

---

## Anti-Pattern (DILARANG KERAS)

### Architecture Anti-Pattern

❌ API call di component:
```ts
useEffect(() => { fetch('/api/data') }, [])
```

❌ Business logic di ViewModel:
```ts
if (amount > 1000000) { ... }
```

❌ Import infra ke core:
```ts
import axios from 'axios'
```

❌ Pakai response API langsung di UI:
```ts
data.response.amount
```

❌ Folder `services` tanpa batasan jelas

### DI Anti-Pattern

❌ Import container ke `core`
❌ Resolve dependency di entity/mapper/use case
❌ Import implementation konkret langsung ke UI (harus via DI)
❌ Registration key ambigu (`service`, `helper`, `handler`)
❌ Menggunakan DI untuk menyembunyikan pelanggaran arsitektur
❌ Mendaftarkan dependency tanpa lifecycle yang dipikirkan
❌ Side effect berat saat container bootstrap
❌ Component kecil resolve terlalu banyak dependency sendiri
❌ Membuat instance manual (`new`) untuk dependency yang seharusnya dikelola container

---

## DI untuk Testing

1. **Mock dependency, bukan implementation yang diuji**
   - Test use case → mock repository
   - Test ViewModel → mock use case
   - Test screen → mock resolved ViewModel

2. **Jangan bergantung pada container real** untuk unit test — inject mock langsung

3. **Container override boleh** untuk integration test (registration override eksplisit)

---

## Workflow Membuat Fitur Baru

```
1. Buat Entity di core/domains/[domain]/entities/
2. Buat Repository Interface di core/domains/[domain]/repositories/
3. Buat UseCase di core/domains/[domain]/usecases/
4. Implementasi Repository di infrastructure/repositories/[domain]/
5. Register di DI container (registry yang sesuai)
6. Buat ViewModel di presentation/features/[feature]/viewModel/
7. Buat Screen & Component di presentation/features/[feature]/
8. Hubungkan di app/[route]/page.tsx
```

---

## Container Bootstrapping

### Urutan Registration (WAJIB konsisten)

1. module/protocol
2. repository
3. use case
4. viewModel
5. logging/other support

### Rules

- Container initialization HARUS terpusat
- Registration tidak boleh memicu network call atau side effect berat
- Container dibuat dan di-bootstrap sekali di tempat yang jelas

---

## Enforcement

Semua AI Agent dan Developer:

- **WAJIB** mengikuti struktur layer ini
- **WAJIB** mengikuti dependency rule
- **WAJIB** menggunakan DI untuk seluruh dependency utama
- **WAJIB** mendaftarkan dependency baru ke registry yang sesuai
- **WAJIB** menjaga consistency antara DI pattern dan Clean Architecture
- **DILARANG** melanggar boundary layer
- **DILARANG** import implementation konkret lintas layer jika seharusnya di-inject
- **DILARANG** menggunakan container di `core`

Jika ada konflik antara kemudahan implementasi vs disiplin arsitektur/DI:
👉 **disiplin arsitektur dan DI harus diprioritaskan**

Jika ada konflik dengan dokumen lain:
👉 gunakan dokumen ini sebagai **sumber kebenaran utama untuk keputusan arsitektur dan DI**

---

## Best Practices

### Arsitektur

- Semua business logic → use case
- Semua external access → repository
- Semua dependency → DI container
- Semua mapping → mapper
- Page di `app/` harus tipis (routing + layout only)
- Gunakan Server Component sebagai default
- Minimalkan `use client` — hanya untuk interaksi/state lokal

### Dependency Injection

- Gunakan composition root terpusat (`src/infrastructure/di/`)
- Register dependency sesuai layer
- Gunakan factory function dengan object injection
- Pilih lifecycle dengan sadar (default: singleton untuk stateless)
- Resolve dependency sedekat mungkin ke boundary komposisi
- Mock dependency via injection saat testing
- Jaga registration naming tetap konsisten dan eksplisit
- Gunakan DI untuk memperkuat Clean Architecture, bukan melemahkannya

---

## Kesimpulan

Arsitektur Clean Architecture + DDD yang diperkuat dengan Dependency Injection memastikan:

- Code lebih rapi dan konsisten
- Dependency terkontrol dan terisolasi
- Logic bisnis terisolasi dari framework
- Testing lebih mudah dan reliable
- Implementasi bisa diganti tanpa mengubah caller
- Boundary layer tetap bersih
- Proyek siap untuk skala enterprise

---

## Integration dengan Framework

| Layer | Relevansi |
|-------|-----------|
| Layer 4 (Design System) | Arsitektur ini adalah standar technical design |
| Layer 6 (AI Skills) | Workflow fitur baru mengikuti pattern ini |
| Layer 8 (Issue-Driven) | Setiap task harus comply dengan arsitektur |
| Layer 11 (AI Review) | Review checklist mengecek compliance arsitektur |
| Layer 12 (Quality Gates) | CI/CD bisa enforce dependency rule |
