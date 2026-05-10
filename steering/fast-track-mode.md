---
inclusion: manual
---

# Fast Track Mode & AI Quality Scorecard

## Overview

Dokumen ini menyediakan **mekanisme formal** untuk situasi deadline mendesak dimana full ceremony tidak feasible, SERTA **scorecard** untuk mengukur kualitas output AI secara objektif.

**Prinsip:** Boleh potong jalan, tapi HARUS bayar hutangnya.

---

## BAGIAN 1: Fast Track Mode

### Kapan Digunakan

Fast Track Mode diaktifkan saat:
- Deadline bisnis mendesak (< 3 hari)
- Hotfix production yang harus segera deploy
- Demo/presentation yang membutuhkan working prototype
- Proof of concept yang tidak akan langsung ke production

### Aktivasi

User menyatakan:
```
"Aktifkan fast track mode" atau "Ini urgent, deadline [tanggal]"
```

AI Agent akan menyesuaikan workflow.

### Yang Boleh Di-skip di Fast Track Mode

| Gate | Normal Mode | Fast Track Mode | Syarat Skip |
|------|-------------|-----------------|-------------|
| Spec-first | WAJIB untuk fitur kompleks | BOLEH skip | Buat spec retroactively dalam 1 sprint |
| Design document | WAJIB untuk fitur kompleks | BOLEH skip | Buat design retroactively |
| Coverage threshold | >= 80% | >= 60% (minimum) | HARUS kembali ke 80% dalam 2 sprint |
| Requirement validation | WAJIB | BOLEH simplified | Minimal completeness check |
| AI Review full | Full checklist | Abbreviated (security + architecture only) | Full review di sprint berikutnya |
| Context Index update | WAJIB | BOLEH delay | Update sebelum sprint end |

### Yang TIDAK BOLEH Di-skip (Bahkan di Fast Track)

| Gate | Alasan |
|------|--------|
| Lint + Typecheck | Mencegah broken build — 0 effort to run |
| Unit test (minimal) | Minimal happy path — mencegah obvious bugs |
| Security check | Security debt terlalu mahal untuk di-skip |
| Conventional commits | Traceability tetap penting |
| Architecture compliance | Debt arsitektur paling mahal untuk di-fix |
| .gitignore / no secrets | Security non-negotiable |

### Accountability: Tech Debt Auto-Tracking

Setiap kali gate di-skip di Fast Track Mode, AI Agent **WAJIB OTOMATIS:**

1. **Buat GitLab issue** untuk setiap skip:
   ```
   Title: [FAST-TRACK DEBT] Spec belum dibuat untuk fitur [X]
   Labels: type::tech-debt, priority::high, fast-track-debt
   Description: 
   - Gate yang di-skip: [spec-first / coverage / design]
   - Alasan: [deadline / hotfix / demo]
   - Deadline payback: [tanggal — max 2 sprint]
   - Assigned to: [developer]
   ```

2. **Tambahkan ke docs/tech-debt/FT-[N]-[title].md:**
   ```markdown
   # FT-001: [Apa yang di-skip]
   
   ## Date: [YYYY-MM-DD]
   ## Reason: [Kenapa fast track]
   ## What was skipped: [Gate yang di-skip]
   ## Payback plan: [Apa yang harus dilakukan]
   ## Deadline: [Kapan harus selesai]
   ## Status: Open
   ```

3. **Informasikan user:**
   ```
   "⚠️ Fast Track: [gate] di-skip. Issue #[N] dibuat untuk tracking.
   Payback deadline: [tanggal]. Jangan lupa bayar hutangnya."
   ```

### Fast Track Workflow

```
[User aktifkan Fast Track Mode]
    ↓
[AI Agent acknowledge + set constraints:]
    - Coverage minimum: 60%
    - Spec: skip, tapi buat issue
    - Design: skip, tapi buat issue
    - Security + Architecture: tetap enforce
    ↓
[Development — reduced ceremony]
    ↓
[Setiap skip → auto-create tech debt issue]
    ↓
[Task selesai → push (dengan reduced gates)]
    ↓
[Sprint berikutnya → payback semua fast-track debt]
```

### Deactivation

Fast Track Mode otomatis nonaktif setelah:
- Task/sprint yang urgent selesai
- User bilang "kembali ke normal mode"
- Semua fast-track debt sudah di-payback

### AI Agent Rules di Fast Track

1. **SELALU informasikan user** bahwa fast track mode aktif
2. **SELALU buat issue** untuk setiap gate yang di-skip
3. **JANGAN skip security dan architecture** — ini non-negotiable
4. **JANGAN biarkan fast track jadi permanent** — ingatkan user untuk payback
5. **Track semua debt** — tidak ada "silent skip"
6. **Coverage MINIMAL 60%** — di bawah ini tetap BLOCK
7. **Setelah fast track selesai** → generate summary: apa yang di-skip, berapa debt

---

## BAGIAN 2: AI Quality Scorecard

### Tujuan

Mengukur kualitas output AI secara objektif per sprint. Tanpa measurement, governance hanya aspirational.

### Scorecard Template: `docs/quality/sprint-[N]-scorecard.md`

```markdown
# AI Quality Scorecard — Sprint [N]

**Period:** [start] — [end]
**Generated:** [date]

## Metrics

### Code Quality
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Test coverage | [X%] | >= 80% | ✅/❌ |
| Lint errors (pre-fix) | [N] | 0 | ✅/❌ |
| Type errors (pre-fix) | [N] | 0 | ✅/❌ |
| Architecture violations | [N] | 0 | ✅/❌ |

### AI Effectiveness
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Tasks completed | [N/total] | 100% | ✅/❌ |
| Tasks requiring rework | [N] | < 20% | ✅/❌ |
| Rework cycles average | [N] | < 2 | ✅/❌ |
| Spec compliance | [X%] | >= 95% | ✅/❌ |

### Governance Compliance
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Conventional commits | [X%] | 100% | ✅/❌ |
| Spec-first compliance | [X%] | >= 90% | ✅/❌ |
| Design doc for complex features | [X%] | >= 80% | ✅/❌ |
| Observability added | [X%] | >= 90% | ✅/❌ |

### Debt
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| New tech debt items | [N] | < 3 | ✅/❌ |
| Tech debt resolved | [N] | >= new | ✅/❌ |
| Fast-track debt open | [N] | 0 | ✅/❌ |
| Outdated specs | [N] | 0 | ✅/❌ |

## Overall Score
[Calculate: (metrics met / total metrics) * 100]%

## Trend
- Sprint N-1: [X%]
- Sprint N: [X%]
- Direction: ↑/↓/→

## Action Items
1. [Improvement needed based on failed metrics]
2. [Specific action to address]
```

### Kapan Generate Scorecard

| Trigger | Action |
|---------|--------|
| Sprint selesai | AI Agent WAJIB generate scorecard |
| Monthly review | AI Agent generate monthly aggregate |
| User minta | Generate on-demand |

### Kiro Hook: Sprint Scorecard Generator

```json
{
  "name": "Sprint Quality Scorecard",
  "version": "1.0.0",
  "description": "Generate AI quality scorecard saat sprint selesai",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "User meminta quality scorecard. Generate docs/quality/sprint-[N]-scorecard.md:\n\n1. Kumpulkan metrics:\n   - Coverage: dari coverage-summary.json\n   - Tasks: dari git log / GitLab issues\n   - Rework: dari issues yang di-reopen\n   - Commits: cek conventional format\n   - Debt: count docs/tech-debt/ yang open\n\n2. Calculate scores per kategori\n3. Determine overall score\n4. Compare dengan sprint sebelumnya (trend)\n5. Generate action items untuk metrics yang gagal\n6. Informasikan user: summary + recommendations"
  }
}
```

### Scoring Thresholds

| Score | Rating | Action |
|-------|--------|--------|
| >= 90% | Excellent | Maintain, celebrate |
| 70-89% | Good | Minor improvements needed |
| 50-69% | Needs Improvement | Focused improvement sprint |
| < 50% | Critical | Stop features, fix quality |

---

## BAGIAN 3: Dependency & Impact Analysis

### Tujuan

Saat mengubah file di `core/` (entities, interfaces), AI Agent HARUS mengidentifikasi semua file yang terdampak sebelum commit.

### Kapan Dilakukan

| Perubahan | Impact Analysis Required |
|-----------|------------------------|
| Ubah entity di `core/domains/` | ✅ WAJIB |
| Ubah interface/protocol di `core/protocols/` | ✅ WAJIB |
| Ubah DI registration | ✅ WAJIB |
| Ubah shared types | ✅ WAJIB |
| Ubah implementation di `infrastructure/` | ⚠️ Cek consumers |
| Ubah UI component props | ⚠️ Cek parent components |
| Tambah file baru | ❌ Tidak perlu |
| Bug fix tanpa ubah interface | ❌ Tidak perlu |

### Impact Analysis Flow

```
[File di core/ diubah]
    ↓
[AI Agent scan: siapa yang import/depend pada file ini?]
    ↓
[List semua impacted files:]
    ├── infrastructure/repositories/ yang implement interface
    ├── application/usecases/ yang pakai entity
    ├── presentation/viewModel/ yang consume use case
    └── tests/ yang mock interface ini
    ↓
[Untuk setiap impacted file:]
    ├── Apakah masih compatible? → OK
    └── Apakah perlu update? → Update sekarang
    ↓
[Jalankan typecheck: npm run typecheck]
    ↓
[Jika ada error → fix sebelum commit]
```

### Kiro Hook: Impact Analysis on Core Changes

```json
{
  "name": "Impact Analysis on Core Changes",
  "version": "1.0.0",
  "description": "Analisis dampak saat file core diubah",
  "when": {
    "type": "postToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Jika file yang baru ditulis/diubah berada di `src/core/` (domains, protocols, entities, interfaces):\n\n1. IDENTIFIKASI semua file yang depend pada file ini:\n   - Cari semua import/reference ke file ini di seluruh project\n   - List: infrastructure implementations, use cases, viewModels, tests\n\n2. CEK COMPATIBILITY:\n   - Apakah perubahan breaking? (rename field, ubah type, hapus method)\n   - Jika breaking → update semua dependent files SEKARANG\n   - Jika non-breaking (tambah optional field) → OK, tidak perlu update\n\n3. JALANKAN typecheck:\n   - `npm run typecheck`\n   - Jika ada error → fix sebelum lanjut\n\n4. INFORMASIKAN user:\n   - 'Perubahan di core/ berdampak pada [N] files: [list]. Semua sudah di-update.'\n\nJika file BUKAN di src/core/ → skip analysis."
  }
}
```

### Anti-Pattern

❌ Ubah interface tanpa update implementasi
❌ Ubah entity tanpa update mapper
❌ Ubah type tanpa jalankan typecheck
❌ Commit dengan TypeScript errors
❌ Assume "nanti saja update yang lain"

---

## BAGIAN 4: Framework Health Check

### Tujuan

Mendeteksi "drift" dari framework — saat project mulai menyimpang dari standar yang didefinisikan.

### Kiro Hook: Framework Health Check

```json
{
  "name": "Framework Health Check",
  "version": "1.0.0",
  "description": "Scan project untuk compliance terhadap framework standards",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "User meminta framework health check. Scan project dan report:\n\n**1. Structure Compliance:**\n- Apakah folder structure sesuai (src/core, src/infrastructure, src/presentation, src/app)?\n- Apakah docs/ folder lengkap (product, requirements, specs, design, adr, learnings)?\n- Apakah .kiro/steering/ ada dan up-to-date?\n\n**2. Architecture Compliance:**\n- Apakah ada import dari infrastructure ke core? (VIOLATION)\n- Apakah ada import dari presentation ke infrastructure langsung? (VIOLATION)\n- Apakah semua dependency di-register di DI container?\n- Apakah ada 'new' manual untuk dependency yang seharusnya via DI?\n\n**3. Testing Compliance:**\n- Apakah coverage >= 80%?\n- Apakah ada source file tanpa corresponding test?\n- Apakah test naming convention diikuti?\n\n**4. Documentation Compliance:**\n- Apakah CONTEXT-INDEX.md up-to-date?\n- Apakah CURRENT-STATE.md up-to-date?\n- Apakah ada spec yang outdated (status: implemented tapi code sudah berubah)?\n- Apakah ada ADR yang perlu di-review?\n\n**5. CI/CD Compliance:**\n- Apakah .gitlab-ci.yml ada dan valid?\n- Apakah ESLint config ada?\n- Apakah vitest config ada dengan json-summary reporter?\n- Apakah package.json punya semua required scripts?\n\n**6. Governance Compliance:**\n- Apakah semua commits mengikuti conventional format?\n- Apakah ada fast-track debt yang belum di-payback?\n- Apakah tech debt items di-track?\n\n**Output:**\nGenerate `docs/quality/health-check-[date].md` dengan:\n- Overall health score (0-100%)\n- Per-category scores\n- Violations found (with file paths)\n- Recommendations\n\nInformasikan user: 'Framework health: [X%]. [N] violations found. Top priority: [recommendation].'"
  }
}
```

### Health Score Calculation

```
Structure:     [X/Y checks passed] × weight 15%
Architecture:  [X/Y checks passed] × weight 25%
Testing:       [X/Y checks passed] × weight 20%
Documentation: [X/Y checks passed] × weight 15%
CI/CD:         [X/Y checks passed] × weight 15%
Governance:    [X/Y checks passed] × weight 10%
─────────────────────────────────────────────────
Overall:       weighted average
```

### Recommended Frequency

| Trigger | Frequency |
|---------|-----------|
| Sprint end | WAJIB (otomatis via sprint completion) |
| Monthly | Recommended |
| After major refactor | WAJIB |
| New team member joins | Recommended |
| User request | On-demand |

---

## Integration dengan Framework

| Layer | Relevansi |
|-------|-----------|
| Layer 5 (Governance) | Fast Track Mode = formal exception to governance |
| Layer 10 (Context) | Project Memory = implementasi context accumulation |
| Layer 12 (Quality Gates) | Scorecard measures gate effectiveness |
| Layer 14 (Learning) | Health Check feeds into improvement cycle |
