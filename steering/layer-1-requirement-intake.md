# Layer 1 — Requirement Intake

## Tujuan

AI membaca berbagai sumber input dan mengekstrak requirements secara terstruktur.

## Input Sources

AI dapat membaca:
- PDF documents
- WhatsApp messages / chat logs
- Meeting notes
- Voice transcripts
- Screenshots
- Figma designs
- Jira / GitLab Issues
- Email threads
- Slack conversations

## AI Role

```
Business Analyst Agent
```

## AI Tasks

1. **Requirement Extraction** - Mengekstrak requirement dari raw input
2. **Stakeholder Mapping** - Mengidentifikasi stakeholder dan perannya
3. **Feature Identification** - Mengidentifikasi fitur dari requirement
4. **Business Process Extraction** - Mengekstrak business process flow
5. **Risk Detection** - Mendeteksi risiko awal

## Output yang Dihasilkan

```
docs/requirements/intake/
├── raw/
│   ├── meeting-notes-2024-01-15.md
│   ├── client-email-thread.md
│   └── whatsapp-discussion.md
├── extracted/
│   ├── functional-requirements.md
│   ├── non-functional-requirements.md
│   ├── stakeholder-map.md
│   ├── feature-list.md
│   └── business-process.md
```

## Template: functional-requirements.md

```markdown
# Functional Requirements

## Source
- [Sumber requirement: meeting notes, email, dll]
- Date: [Tanggal]
- Stakeholder: [Siapa yang request]

## Requirements

### FR-001: [Nama Requirement]
- **Description:** [Deskripsi lengkap]
- **Priority:** [High/Medium/Low]
- **Stakeholder:** [Siapa]
- **Acceptance Criteria:**
  - [ ] [Criteria 1]
  - [ ] [Criteria 2]
- **Dependencies:** [Dependency jika ada]
- **Notes:** [Catatan tambahan]

### FR-002: [Nama Requirement]
...
```

## Template: stakeholder-map.md

```markdown
# Stakeholder Map

## Primary Stakeholders
| Name | Role | Interest | Influence | Communication |
|------|------|----------|-----------|---------------|
| [Name] | [Role] | [Interest] | High/Medium/Low | [Channel] |

## Secondary Stakeholders
| Name | Role | Interest | Influence | Communication |
|------|------|----------|-----------|---------------|
| [Name] | [Role] | [Interest] | High/Medium/Low | [Channel] |

## Decision Matrix
| Decision Area | Decision Maker | Consulted | Informed |
|---------------|---------------|-----------|----------|
| [Area] | [Name] | [Names] | [Names] |
```

## Workflow dengan Kiro

### Step 1: Input Raw Data
Paste atau attach raw input ke Kiro chat:
- Drag & drop PDF
- Paste meeting notes
- Attach screenshots

### Step 2: AI Extraction
Minta Kiro mengekstrak requirements:

```
"Ekstrak semua functional dan non-functional requirements dari dokumen ini.
Format sesuai template di docs/requirements/intake/extracted/"
```

### Step 3: Organize
AI akan mengorganisir requirements ke dalam:
- Functional requirements
- Non-functional requirements
- Stakeholder map
- Feature list

### Step 4: Create GitLab Issues
Setiap requirement menjadi GitLab Issue:

```
"Buatkan GitLab Issues dari requirements yang sudah diekstrak.
Gunakan label: requirement, priority:{high|medium|low}"
```

## GitLab Integration

### Labels untuk Requirements
```
requirement::functional
requirement::non-functional
priority::high
priority::medium
priority::low
status::draft
status::validated
status::approved
```

### Issue Template untuk Requirement
```markdown
## Requirement: [ID]

### Description
[Deskripsi requirement]

### Source
- Document: [nama dokumen]
- Stakeholder: [nama]
- Date: [tanggal]

### Acceptance Criteria
- [ ] [Criteria 1]
- [ ] [Criteria 2]

### Priority
[High/Medium/Low]

### Dependencies
- [Dependency 1]

### Validation Status
- [ ] Ambiguity check passed
- [ ] Completeness check passed
- [ ] Conflict check passed

/label ~"requirement::functional" ~"priority::high" ~"status::draft"
```

## Best Practices

1. **Selalu simpan raw input** - Jangan buang sumber asli
2. **Trace requirement ke source** - Setiap requirement harus bisa di-trace ke sumbernya
3. **Gunakan ID yang konsisten** - FR-001, NFR-001, dll
4. **Validasi dengan stakeholder** - Requirement yang diekstrak AI harus dikonfirmasi
5. **Iterative extraction** - Jangan expect sempurna di iterasi pertama
