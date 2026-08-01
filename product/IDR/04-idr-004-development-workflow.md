# IDR-004: Development Workflow

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD, RHS Series
>
> **Status:** ✅ Approved
>
> **Version:** 1.0

---

# 📖 Overview

Dokumen ini mendefinisikan alur kerja pengembangan (*Development Workflow*) yang digunakan pada Platform Digital Informatika Angkatan 2025.

Tujuan utama workflow ini adalah memastikan seluruh perubahan source code dilakukan secara konsisten, terdokumentasi, dapat ditinjau (*reviewable*), serta dapat ditelusuri (*traceable*) hingga ke requirement yang mendasarinya.

Workflow ini berlaku untuk seluruh anggota tim pengembang dan menjadi acuan pada proses implementasi, code review, pengujian, serta deployment.

---

# 🎯 Objectives

- Menyediakan workflow pengembangan yang konsisten.
- Menjamin kualitas kode melalui proses review.
- Mengurangi risiko konflik saat kolaborasi.
- Menjaga histori perubahan tetap bersih dan mudah ditelusuri.
- Memastikan setiap implementasi memiliki keterkaitan dengan requirement yang telah disetujui.

---

# 🏗️ Branching Strategy

Repository menggunakan pendekatan **Git Feature Branch Workflow**.

Setiap perubahan dilakukan pada branch terpisah sebelum digabungkan ke branch utama.

---

## Branch Structure

| Branch | Purpose |
|---------|---------|
| `main` | Production-ready source code |
| `develop` *(opsional)* | Integration branch apabila diperlukan |
| `feature/*` | Implementasi fitur baru |
| `fix/*` | Bug fixing |
| `hotfix/*` | Perbaikan kritikal pada production |
| `docs/*` | Perubahan dokumentasi |
| `refactor/*` | Refactoring tanpa perubahan behavior |

---

## Naming Convention

| Type | Example |
|--------|---------|
| Feature | `feature/authentication` |
| Feature | `feature/knowledge-hub` |
| Fix | `fix/login-validation` |
| Hotfix | `hotfix/security-patch` |
| Docs | `docs/prd-update` |
| Refactor | `refactor/dashboard-service` |

---

# 🌱 Development Flow

```text
Issue
    │
    ▼
Create Branch
    │
    ▼
Implementation
    │
    ▼
Local Testing
    │
    ▼
Commit
    │
    ▼
Push
    │
    ▼
Pull Request
    │
    ▼
Code Review
    │
    ▼
Approval
    │
    ▼
Merge
```

---

# 📝 Commit Convention

Project menggunakan standar **Conventional Commits**.

## Format

```text
type(scope): description
```

---

## Allowed Types

| Type | Purpose |
|--------|----------|
| feat | Fitur baru |
| fix | Bug fixing |
| docs | Dokumentasi |
| refactor | Refactoring |
| style | Formatting |
| test | Testing |
| chore | Maintenance |
| perf | Performance improvement |
| build | Build configuration |
| ci | CI/CD |

---

## Examples

```text
feat(auth): implement first login onboarding

feat(schedule): add exception schedule

fix(auth): validate temporary password

docs(prd): update dashboard requirements

refactor(notification): simplify queue service
```

---

# 🔀 Pull Request Rules

Setiap Pull Request harus memenuhi persyaratan berikut.

| Rule | Requirement |
|------|-------------|
| Target Branch | main / develop |
| CI | Seluruh pipeline harus berhasil |
| Build | Tidak boleh gagal |
| Documentation | Wajib diperbarui jika requirement berubah |
| Review | Minimal satu approval (jika tim lebih dari satu orang) |

---

## Pull Request Checklist

- [ ] Branch sesuai naming convention
- [ ] Tidak ada merge conflict
- [ ] Build berhasil
- [ ] Test berhasil
- [ ] Dokumentasi diperbarui
- [ ] Tidak terdapat credential
- [ ] Tidak terdapat secret
- [ ] Tidak terdapat file sementara

---

# 🧪 Development Checklist

Sebelum membuat Pull Request, developer harus memastikan:

- Code berhasil di-build.
- Linter tidak menghasilkan error.
- Formatter telah dijalankan.
- Unit test berhasil.
- Manual testing selesai.
- Dokumentasi diperbarui bila diperlukan.

---

# 🔍 Code Review Guidelines

Reviewer harus mengevaluasi:

- Kesesuaian dengan PRD.
- Kesesuaian dengan RHS.
- Kesesuaian dengan IDR terkait.
- Keamanan implementasi.
- Kualitas kode.
- Konsistensi style.
- Potensi bug.
- Potensi technical debt.

---

# 📚 Documentation Update Rules

Dokumentasi harus diperbarui apabila terjadi perubahan pada:

- Requirement.
- API.
- Database Schema.
- Deployment.
- Infrastruktur.
- Security Policy.
- Architecture Decision.

Perubahan implementasi tanpa pembaruan dokumentasi tidak direkomendasikan.

---

# 🚫 Forbidden Practices

Hal-hal berikut tidak diperbolehkan:

- Commit langsung ke `main`.
- Menggabungkan beberapa fitur berbeda dalam satu Pull Request.
- Menyimpan password atau credential pada repository.
- Commit file hasil build.
- Commit folder dependency (`node_modules`, `vendor`, dan sejenisnya).
- Menonaktifkan security check tanpa persetujuan.
- Mengubah requirement tanpa persetujuan Product Owner.

---

# 📊 Workflow Traceability

| Development Activity | Reference |
|----------------------|-----------|
| Authentication | [RHS-001](./rhs/01-rhs-001-authentication.md) |
| Official Information | [RHS-002](./rhs/02-rhs-002-official-information.md) |
| Dashboard | [RHS-003](./rhs/03-rhs-003-dashboard.md) |
| Schedule | [RHS-004](./rhs/04-rhs-004-schedule.md) |
| Knowledge Hub | [RHS-005](./rhs/05-rhs-005-knowledge-hub.md) |
| Security | [RHS-009](./rhs/09-rhs-009-security-2fa.md) |
| Audit Log | [RHS-012](./rhs/12-rhs-012-audit-log.md) |

---

# 📄 Related Documents

- `PRD.md`
- `rhs/README.md`
- `01-idr-001-project-architecture.md`
- `02-idr-002-technology-stack.md`
- `03-idr-003-repository-structure.md`

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-01 | Initial Development Workflow documentation |
