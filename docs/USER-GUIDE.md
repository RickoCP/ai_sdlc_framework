# Panduan Pengguna — Enterprise AI-Native SDLC Framework

Panduan lengkap untuk menggunakan framework ini dalam pengembangan sehari-hari.

---

## 1. Prerequisites

Sebelum memulai, pastikan Anda memiliki:

### Software

| Kebutuhan | Minimum Version | Cek |
|-----------|----------------|-----|
| Kiro IDE | Latest | Sudah terinstall |
| Node.js | 18+ | `node --version` |
| Git | 2.30+ | `git --version` |
| npm/yarn | npm 9+ / yarn 1.22+ | `npm --version` |

### Akun & Token

| Kebutuhan | Cara Mendapatkan |
|-----------|-----------------|
| GitLab Account | Daftar di gitlab.com atau instance perusahaan |
| GitLab Personal Access Token | GitLab → User Settings → Access Tokens → Scope: `api`, `read_repository`, `write_repository` |

### Environment Variables

Set di OS Anda:

**Windows (PowerShell — permanent):**
```powershell
[Environment]::SetEnvironmentVariable("GITLAB_PERSONAL_ACCESS_TOKEN", "glpat-xxxx", "User")
[Environment]::SetEnvironmentVariable("GITLAB_API_URL", "https://gitlab.com/api/v4", "User")
```

**Linux/Mac (tambahkan di ~/.bashrc atau ~/.zshrc):**
```bash
export GITLAB_PERSONAL_ACCESS_TOKEN="glpat-xxxxxxxxxxxx"
export GITLAB_API_URL="https://gitlab.com/api/v4"
```

---

## 2. Instalasi

### Step 1: Install Power di Kiro

1. Buka Kiro IDE
2. Buka panel **Powers** (sidebar kiri atau Ctrl+Shift+P → "Powers")
3. Cari "Enterprise AI-Native SDLC Framework"
4. Klik **Install**
5. Tunggu hingga status berubah menjadi "Installed"

### Step 2: Approve Environment Variables

1. Buka Kiro Settings (Ctrl+,)
2. Cari **"Mcp Approved Env Vars"**
3. Tambahkan:
   - `GITLAB_PERSONAL_ACCESS_TOKEN`
   - `GITLAB_API_URL`
4. Atau: approve langsung dari popup security warning saat pertama kali

### Step 3: Restart Kiro

Restart Kiro agar environment variables terbaca oleh MCP servers.

### Step 4: Verifikasi

Buka chat dan ketik:
```
Aktifkan Enterprise AI-SDLC Framework
```

Jika berhasil, AI Agent akan menampilkan pesan selamat datang dan 8 pertanyaan project.

---

## 3. First Time Setup

### Menjawab 8 Pertanyaan

Saat pertama kali mengaktifkan power, AI Agent akan bertanya:

```
Selamat datang di Enterprise AI-Native SDLC Framework!

1. Apa nama project yang ingin dibuat/dikerjakan?
2. Deskripsi singkat project (apa yang ingin diselesaikan)?
3. Tech stack yang diinginkan? (contoh: Next.js, Go, Python, dll)
4. Apakah ini project baru atau project existing?
5. Apakah project ini punya UI? Jika ya, apakah Anda sudah punya Design System sendiri?
6. Apakah GitLab credentials sudah di-setup?
7. Mode development yang diinginkan? (Enterprise / Solo / Zero Touch)
8. Layer mana yang ingin dikerjakan terlebih dahulu?
```

**Contoh jawaban:**
```
1. payment-gateway
2. Microservice untuk payment processing (top-up, transfer, bill payment)
3. TypeScript + Express + PostgreSQL
4. Project baru
5. Tidak ada UI
6. Sudah di-setup
7. Enterprise
8. Mulai dari Layer 0 (Product Vision)
```

### Apa yang Terjadi Setelah Jawab

Untuk **project baru**, AI Agent akan otomatis:

1. ✅ Inisialisasi Git repository
2. ✅ Buat `.gitignore` sesuai tech stack
3. ✅ Buat struktur folder (docs/, src/, tests/, .kiro/, .gitlab/)
4. ✅ Generate workspace steering files (3 files di `.kiro/steering/`)
5. ✅ Generate Kiro hooks (9 files di `.kiro/hooks/`)
6. ✅ Initial commit + push
7. ✅ Buat repository di GitLab
8. ✅ Setup branch protection
9. ✅ Buat labels framework
10. ✅ Buat `.gitlab-ci.yml`

Untuk **project existing**, AI Agent akan:

1. ✅ Detect file yang sudah ada
2. ✅ Generate hooks yang missing (self-heal)
3. ✅ Generate steering yang missing
4. ✅ Buat CURRENT-STATE.md jika belum ada
5. ✅ Resume dari state terakhir

---

## 4. Daily Workflow

### Memulai Hari Kerja

Cukup buka Kiro dan mulai chat. AI Agent akan otomatis:

```
━━━ 🏗️ ARCHITECT AGENT ━━━
Loading project state...
- Sprint: 2
- Last task: #15 — implement payment validation
- Status: done
- Next action: #16 — implement notification service
Status: ✅ State loaded. Ready to continue.
```

### Mengerjakan Task

Katakan apa yang ingin dikerjakan:

```
User: "Kerjakan issue #16 — implement notification service"
```

AI Agent akan menjalankan full agent chain:
1. 🏗️ Architect → validate spec & design
2. ⚙️ Backend → implement code
3. 🧪 QA → write tests
4. 🔒 Security → review
5. 🚀 DevOps → lint + test + commit + push

### Mengakhiri Hari Kerja

Tidak perlu melakukan apa-apa khusus. Setiap task yang selesai otomatis:
- Update `docs/CURRENT-STATE.md` (save point)
- Update `docs/CONTEXT-INDEX.md` (jika ada artifact baru)
- Commit + push
- **Update GitLab issue** → label berubah ke `status::review`
- **Comment di issue** → summary implementasi (branch, tests, coverage)
- **Collect metrics** → append ke `docs/quality/metrics-log.jsonl`

Besok, AI Agent akan resume dari titik terakhir.

### GitLab Board Otomatis Terupdate

Anda tidak perlu manual pindahkan issue di board. Framework otomatis:

| Event | GitLab Action |
|-------|--------------|
| Task dimulai | Issue → `status::in-progress` (pindah ke kolom "In Progress") |
| Task selesai + pushed | Issue → `status::review` + comment (pindah ke "Review") |
| MR merged | Issue auto-close (pindah ke "Done") |
| Sprint selesai | Milestone closed/carry-over + wiki Changelog updated |

---

## 5. Common Commands / Requests

### Feature Development

| Anda Bilang | AI Agent Lakukan |
|-------------|-----------------|
| "Implement fitur [X]" | Full agent chain: architect → backend → QA → security → devops |
| "Buat spec untuk fitur [X]" | BA Agent: extract requirements → buat SRS |
| "Design arsitektur untuk [X]" | Architect Agent: buat technical design + ADR |
| "Tulis test untuk [file]" | QA Agent: unit tests sesuai pattern |

### Bug Fix

| Anda Bilang | AI Agent Lakukan |
|-------------|-----------------|
| "Fix bug #23" | Backend → QA (regression test) → DevOps → Learning capture |
| "Ada error di [file]" | Diagnose → fix → test → push |

### Review & Quality

| Anda Bilang | AI Agent Lakukan |
|-------------|-----------------|
| "Review code di branch [X]" | Security + Architect review → unified report |
| "Generate scorecard" | Parse metrics-log → generate scorecard |
| "Jalankan retrospective" | Parse metrics-log → generate retro + improvement issues |
| "Health check" | Scan compliance → generate report |

### Sprint Management

| Anda Bilang | AI Agent Lakukan |
|-------------|-----------------|
| "Plan sprint [N]" | BA breakdown → Architect tasks → create GitLab issues |
| "Sprint selesai" | Push all → create MR → generate retro + scorecard |
| "Buat milestone sprint [N]" | Create GitLab milestone + issues |

### Mode Switching

| Anda Bilang | AI Agent Lakukan |
|-------------|-----------------|
| "Saya bekerja solo" | Aktifkan Solo Mode (simplified workflow) |
| "Aktifkan fast track mode" | Reduced ceremony + auto-track debt |
| "Kembali ke normal mode" | Deactivate fast track |

---

## 6. Memahami Agent Banners

Saat AI Agent bekerja, Anda akan melihat banner yang menunjukkan agent aktif:

### Banner Reference

```
━━━ 📋 BA AGENT ━━━
Mengekstrak requirements, membuat user stories.
Aktif saat: upload dokumen, minta breakdown fitur.

━━━ 🏗️ ARCHITECT AGENT ━━━
Validasi arsitektur, buat design, tulis ADR.
Aktif saat: awal task (gate), review arsitektur.

━━━ ⚙️ BACKEND AGENT ━━━
Implement business logic di core/ dan infrastructure/.
Aktif saat: coding fitur, fix bug.

━━━ 🎨 FRONTEND AGENT ━━━
Implement UI components dan ViewModels.
Aktif saat: task UI/presentation layer.

━━━ 🧪 QA AGENT ━━━
Tulis test, jalankan lint/typecheck/test.
Aktif saat: setelah implementation (postTaskExecution).

━━━ 🔒 SECURITY AGENT ━━━
Review security: validation, auth, injection.
Aktif saat: setelah file ditulis (postToolUse).

━━━ 🚀 DEVOPS AGENT ━━━
Git operations: commit, push, create MR.
Aktif saat: setelah QA pass.

━━━ 📚 LEARNING AGENT ━━━
Capture bug learning, generate retrospective.
Aktif saat: setelah bug fix, sprint end.
```

### Status Indicators

| Symbol | Arti |
|--------|------|
| ✅ | Phase selesai sukses |
| ❌ | Phase gagal — perlu fix |
| ⚠️ | Warning — ada catatan |

---

## 7. Sprint Workflow

### Phase 1: Planning

```
User: "Plan sprint 3 untuk fitur payment dan notification"

━━━ 📋 BA AGENT ━━━
Breakdown fitur menjadi user stories...
- US-01: Sebagai user, saya bisa top-up saldo
- US-02: Sebagai user, saya dapat notifikasi setelah transaksi
...

━━━ 🏗️ ARCHITECT AGENT ━━━
Technical task breakdown...
- Task 1: Create Payment entity + use case
- Task 2: Create PaymentRepository implementation
- Task 3: Create NotificationService
...

━━━ 🚀 DEVOPS AGENT ━━━
Creating GitLab issues + milestone...
- Milestone: Sprint 3 (2 weeks)
- Issues: #20, #21, #22, #23, #24
- Labels assigned: type::feat, agent::backend
Status: ✅ Sprint planned
```

### Phase 2: Execution (Daily)

Kerjakan task satu per satu:
```
User: "Kerjakan issue #20"
```

AI Agent menjalankan full chain per task. Setiap task selesai → auto push.

### Phase 3: Retrospective

```
User: "Sprint selesai, jalankan retrospective"

━━━ 📚 LEARNING AGENT ━━━
Generating retrospective dari metrics-log.jsonl...

Sprint 3 Summary:
- Tasks completed: 5/5 ✅
- Average coverage: 88%
- Rework rate: 10% (target < 20%) ✅
- Spec compliance: 100% ✅
- Security findings: 0 ✅

What Worked:
- Spec-first approach mengurangi rework
- Observability otomatis via hooks

What Didn't:
- Task #22 butuh 2x rework (requirement ambigu)

Improvement Actions:
- Issue #30: Improve requirement clarity checklist

Status: ✅ Retrospective generated → docs/retrospectives/sprint-3.md
```

---

## 8. Manual Hooks (User-Triggered)

Tiga hooks yang bisa Anda trigger kapan saja:

### Sprint Retrospective

```
User: "Generate retrospective" atau "Jalankan retrospective"
```

Output: `docs/retrospectives/sprint-[N].md`
- AI Performance metrics
- What Worked / What Didn't
- Recurring issues
- Improvement actions (+ GitLab issues)

### Quality Scorecard

```
User: "Generate scorecard" atau "Tampilkan quality scorecard"
```

Output: `docs/quality/sprint-[N]-scorecard.md`
- Code Quality (coverage, lint, type errors)
- AI Effectiveness (tasks, rework rate, spec compliance)
- Governance Compliance
- Overall score + trend

### Framework Health Check

```
User: "Health check" atau "Cek compliance framework"
```

Output: `docs/quality/health-check-[date].md`
- Structure compliance
- Architecture violations
- Testing coverage
- Documentation freshness
- CI/CD validity
- Governance adherence
- Overall health score

---

## 9. Solo Mode

### Kapan Menggunakan

- Project personal / side project
- Bekerja sendiri tanpa tim
- Prototype yang tidak perlu full ceremony

### Cara Mengaktifkan

```
User: "Saya bekerja solo" atau "Ini project personal"
```

### Apa yang Berubah

| Aspek | Normal | Solo |
|-------|--------|------|
| Branch strategy | main + develop + feature | main + feature |
| MR Approval | Wajib reviewer | Self-merge |
| Sprint planning | Formal milestone | Simplified task list |
| Labels | Full taxonomy | type:: saja |
| Code review | AI + Human | AI saja |

### Apa yang TETAP (Non-Negotiable)

- ✅ Clean Architecture + DI
- ✅ Lint + Typecheck + Test sebelum push
- ✅ Coverage >= 80%
- ✅ Conventional Commits
- ✅ Security checks
- ✅ Spec-first untuk fitur kompleks

### Solo Workflow

```
1. Buat feature branch dari main
2. Implement (ikuti architecture standards)
3. Lint + Typecheck + Test (otomatis via hooks)
4. Commit (conventional format)
5. Push ke feature branch
6. Merge ke main (tanpa approval)
7. Delete feature branch
```

---

## 10. Fast Track Mode

### Kapan Menggunakan

- Deadline bisnis mendesak (< 3 hari)
- Hotfix production
- Demo/presentation yang butuh working prototype
- Proof of concept

### Cara Mengaktifkan

```
User: "Aktifkan fast track mode" atau "Ini urgent, deadline besok"
```

### Yang Boleh Di-skip

| Gate | Fast Track | Syarat |
|------|-----------|--------|
| Spec-first | Skip | Buat retroactively dalam 1 sprint |
| Design document | Skip | Buat retroactively |
| Coverage threshold | 60% (dari 80%) | Kembali ke 80% dalam 2 sprint |
| Full AI Review | Abbreviated | Full review sprint berikutnya |

### Yang TIDAK BOLEH Di-skip

- ❌ Lint + Typecheck (0 effort)
- ❌ Unit test minimal (happy path)
- ❌ Security check
- ❌ Architecture compliance
- ❌ Conventional commits

### Accountability

Setiap gate yang di-skip → otomatis:
1. Buat GitLab issue `[FAST-TRACK DEBT]`
2. Buat file `docs/tech-debt/FT-[N]-[title].md`
3. Informasikan user tentang payback deadline

### Deactivation

Fast Track otomatis nonaktif setelah:
- Task urgent selesai
- User bilang "kembali ke normal mode"
- Semua fast-track debt sudah di-payback

---

## 10.5 Zero Touch Mode

### Kapan Menggunakan

- Prototype cepat
- Proof of concept
- Ingin AI jalankan semuanya otomatis

### Cara Mengaktifkan

Pilih "Zero Touch" di pertanyaan #7 saat inisiasi project.

### Apa yang Terjadi

- AI jalankan Layer 0-8 tanpa stop (tanpa konfirmasi per layer)
- AI kerjakan semua sprint issues sequential
- AI auto-create MR saat semua task done
- STOP di merge point (user tetap harus merge di GitLab)
- Setelah merge → auto retro + scorecard + wiki update

### Yang TETAP Enforced

- ✅ Lint + Typecheck + Test (coverage >= 80%)
- ✅ Architecture compliance
- ✅ Security checks
- ✅ Conventional commits
- ✅ User harus merge MR (AI tidak boleh merge)

---

## 11. Troubleshooting

### MCP Server Tidak Connect

**Gejala:** Error "MCP server not available" atau GitLab operations gagal.

**Solusi:**
1. Cek environment variables sudah di-set: `echo $GITLAB_PERSONAL_ACCESS_TOKEN`
2. Cek sudah di-approve di Kiro Settings → "Mcp Approved Env Vars"
3. Restart Kiro
4. Cek token belum expired di GitLab → User Settings → Access Tokens

### Hooks Tidak Trigger

**Gejala:** Tidak ada agent banner muncul, tidak ada auto-push.

**Solusi:**
1. Cek folder `.kiro/hooks/` ada di project
2. Cek 9 file JSON ada dan valid
3. Bilang ke AI: "Cek hooks framework" — akan auto-heal jika missing
4. Restart Kiro jika hooks baru di-generate

### Push Gagal (Coverage < 80%)

**Gejala:** DevOps Agent block push karena coverage rendah.

**Solusi:**
1. Tulis test tambahan: "Tulis test untuk [file] sampai coverage >= 80%"
2. Atau aktifkan Fast Track Mode jika urgent (minimum 60%)
3. Cek coverage: `npm run test:unit -- --coverage`

### Context Loss (AI Lupa State)

**Gejala:** AI mulai dari awal, tidak ingat task sebelumnya.

**Solusi:**
1. Cek `docs/CURRENT-STATE.md` ada dan up-to-date
2. Bilang: "Baca CURRENT-STATE.md dan lanjutkan"
3. Jika file hilang: "Generate ulang CURRENT-STATE dari git log"

### Git MCP Error "repo_path required"

**Gejala:** Git operations gagal dengan error path.

**Solusi:**
- AI Agent harus selalu pass absolute path (`repo_path`) ke Git MCP
- Jika error persist, AI akan fallback ke shell commands
- Pastikan working directory benar

### Architecture Violation Detected

**Gejala:** Architect Agent block task karena violation.

**Solusi:**
1. Baca error message — biasanya import direction salah
2. Fix: core TIDAK BOLEH import dari infrastructure
3. Gunakan DI (dependency injection) untuk dependency
4. Baca `.kiro/steering/architecture-standards.md` untuk rules lengkap

### Fast Track Debt Menumpuk

**Gejala:** Banyak issue `[FAST-TRACK DEBT]` yang belum resolved.

**Solusi:**
1. "Tampilkan semua fast-track debt yang open"
2. Alokasikan 1 sprint khusus untuk payback
3. Prioritaskan: spec → coverage → design

---

## 12. Tips & Tricks

### Produktivitas

1. **Mulai dengan spec** — Task dengan spec memiliki rework rate 3x lebih rendah
2. **Satu issue = satu task** — Jangan gabung multiple fitur dalam satu request
3. **Gunakan issue number** — "Kerjakan issue #15" lebih presisi dari deskripsi panjang
4. **Biarkan hooks bekerja** — Jangan skip QA/Security phase, mereka menangkap bug lebih awal

### Quality

5. **Review scorecard tiap sprint** — Trend menurun = early warning
6. **Baca learnings sebelum fitur serupa** — Cegah bug yang sama terulang
7. **Health check setelah refactor besar** — Pastikan tidak ada drift
8. **Coverage 80% bukan ceiling** — Untuk auth/payment, target 95%

### Workflow

9. **Gunakan Fast Track dengan bijak** — Selalu bayar debt-nya
10. **Solo Mode untuk prototype** — Switch ke normal saat production-ready
11. **Trigger retrospective rutin** — Data-driven improvement lebih efektif
12. **Update CONTEXT-INDEX manual jika perlu** — AI kadang miss artifact external

### Communication dengan AI Agent

13. **Spesifik lebih baik** — "Implement CreatePaymentUseCase" > "buat payment"
14. **Sebutkan layer jika ambigu** — "Di infrastructure layer, buat repository untuk..."
15. **Minta role tertentu** — "Sebagai Security Agent, review file ini"
16. **Konfirmasi sebelum lanjut** — AI akan tanya, jawab "lanjut" atau minta revisi

### Advanced

17. **Custom steering** — Tambah file di `.kiro/steering/` untuk rules project-specific
18. **Disable hook sementara** — Rename file ke `.disabled` extension
19. **Multi-stack** — Framework support Next.js, Go, Python (lihat tech-stack-profiles)
20. **Presentasi** — "Generate materi presentasi" untuk demo ke stakeholder

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────┐
│           ENTERPRISE AI-SDLC QUICK REFERENCE             │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  START SESSION:                                          │
│    → Otomatis load state + validate hooks                │
│                                                          │
│  IMPLEMENT FITUR:                                        │
│    → "Implement fitur [X]" atau "Kerjakan issue #N"      │
│                                                          │
│  FIX BUG:                                                │
│    → "Fix bug #N" atau "Ada error di [file]"             │
│                                                          │
│  REVIEW:                                                 │
│    → "Review branch [X]"                                 │
│                                                          │
│  QUALITY:                                                │
│    → "Generate scorecard"                                │
│    → "Health check"                                      │
│    → "Jalankan retrospective"                            │
│                                                          │
│  MODE:                                                   │
│    → "Saya bekerja solo"                                 │
│    → "Aktifkan fast track mode"                          │
│    → "Kembali ke normal mode"                            │
│                                                          │
│  SPRINT:                                                 │
│    → "Plan sprint [N]"                                   │
│    → "Sprint selesai"                                    │
│                                                          │
│  BLOCKING RULES:                                         │
│    → Test fail = BLOCK push                              │
│    → Coverage < 80% = BLOCK push                         │
│    → Security critical = BLOCK continue                  │
│                                                          │
└─────────────────────────────────────────────────────────┘
```
