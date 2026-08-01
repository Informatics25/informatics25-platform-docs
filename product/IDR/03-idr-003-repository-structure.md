# IDR-003: Repository Structure & Development Standards

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD v1.0 • RHS Baseline • IDR-001 Project Architecture • IDR-002 Technology Stack
>
> **Status:** 🟢 Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini mendefinisikan struktur repository resmi Platform Digital Informatika Angkatan 2025 beserta standar organisasi source code, dokumentasi, aset, dan konfigurasi proyek.

Repository Structure bertujuan menjaga konsistensi pengembangan sehingga setiap engineer dapat memahami lokasi setiap komponen proyek tanpa bergantung pada pengetahuan individu.

Dokumen ini tidak menjelaskan implementasi kode, melainkan standar organisasi repository.

---

# 🎯 Objectives

- Menetapkan struktur repository resmi proyek.
- Memisahkan source code, dokumentasi, dan infrastruktur secara jelas.
- Mempermudah onboarding developer baru.
- Mengurangi inkonsistensi struktur folder.
- Mendukung skalabilitas proyek jangka panjang.

---

# 🏛️ Repository Organization Principles

| Principle | Description |
|-----------|-------------|
| Separation of Concerns | Source code, documentation, database, dan infrastructure dipisahkan. |
| Discoverability | Struktur mudah dipahami tanpa dokumentasi tambahan. |
| Consistency | Penamaan folder mengikuti standar yang sama. |
| Scalability | Mudah berkembang tanpa reorganisasi besar. |
| Maintainability | Mudah dipelihara oleh pengelola berikutnya. |

---

# 📂 Recommended Repository Structure

```text
project-root/
│
├── apps/
│   ├── frontend/
│   └── backend/
│
├── packages/
│   └── shared/
│
├── database/
│   ├── migrations/
│   ├── seeds/
│   └── schema/
│
├── docs/
│   ├── PRD/
│   ├── RHS/
│   ├── IDR/
│   ├── SDS/
│   ├── DDS/
│   ├── AHS/
│   ├── TSS/
│   └── MPS/
│
├── infra/
│
├── scripts/
│
├── .github/
│   ├── workflows/
│   └── ISSUE_TEMPLATE/
│
├── .vscode/
│
├── .editorconfig
├── .gitignore
├── README.md
├── LICENSE
└── docker-compose.yml
```

---

# 📁 Directory Responsibilities

| Directory | Responsibility |
|-----------|----------------|
| `apps/frontend` | Frontend Application |
| `apps/backend` | Backend Application |
| `packages/shared` | Shared library dan utilities |
| `database` | Database migration, seed, dan schema |
| `docs` | Dokumentasi proyek |
| `infra` | Infrastruktur dan deployment |
| `scripts` | Automation script |
| `.github` | GitHub configuration dan CI/CD |

---

# 📚 Documentation Structure

```text
docs/
│
├── PRD/
├── RHS/
├── IDR/
├── SDS/
├── DDS/
├── AHS/
├── TSS/
└── MPS/
```

Setiap folder dokumentasi memiliki `README.md` sebagai index.

---

# 📄 File Naming Convention

## Documentation

```text
01-rhs-001-authentication.md
02-rhs-002-official-information.md
03-rhs-003-dashboard.md

01-idr-001-project-architecture.md
02-idr-002-technology-stack.md
03-idr-003-repository-structure.md
```

---

## Source Code

Gunakan penamaan yang konsisten sesuai bahasa pemrograman.

Contoh:

```text
user_service.go
schedule_controller.go
knowledge_hub_service.go

DashboardView.vue
AnnouncementCard.vue
LoginPage.vue
```

---

# 🌿 Git Branch Strategy

| Branch | Purpose |
|---------|---------|
| `main` | Production-ready code |
| `develop` | Integration branch |
| `feature/*` | Pengembangan fitur baru |
| `fix/*` | Perbaikan bug |
| `hotfix/*` | Perbaikan kritis pada production |
| `release/*` | Persiapan rilis |

---

# 💬 Commit Message Convention

Gunakan format berikut.

```text
feat(auth): implement first login onboarding

fix(schedule): resolve exception validation

docs(rhs): update authentication requirement

refactor(api): simplify middleware

test(notification): add retry test

chore(deps): update dependencies
```

---

# 🏷️ Naming Convention

| Item | Convention |
|------|------------|
| Folder | kebab-case |
| Markdown File | kebab-case |
| Vue Component | PascalCase |
| Go File | snake_case |
| API Endpoint | kebab-case |
| Database Table | snake_case |
| Environment Variable | UPPER_SNAKE_CASE |

---

# 📦 Binary Files

Repository tidak boleh menyimpan:

- build output
- executable
- cache
- dependency cache
- node_modules
- vendor cache
- temporary file

Seluruh file tersebut harus dikecualikan melalui `.gitignore`.

---

# 🔐 Sensitive Files

File berikut tidak boleh dikomit ke repository.

- `.env`
- private key
- secret
- credential
- token
- database dump production

Contoh file yang boleh tersedia:

```text
.env.example
```

---

# 📋 Repository Standards

Repository harus memiliki minimal:

- README.md
- LICENSE
- .gitignore
- .editorconfig
- Docker Compose (jika digunakan)
- Dokumentasi pada folder `docs`

---

# 🚫 Anti-Patterns

Hal berikut harus dihindari.

- Menyimpan dokumentasi di luar folder `docs`.
- Menyimpan migration bersama source code backend.
- Menyimpan file hasil build pada repository.
- Menggunakan penamaan folder yang tidak konsisten.
- Menyimpan credential pada Git.

---

# ✅ Acceptance Criteria

- [ ] Struktur repository mengikuti standar proyek.
- [ ] Dokumentasi berada pada folder `docs`.
- [ ] Source code frontend dan backend dipisahkan.
- [ ] Database memiliki direktori khusus.
- [ ] File sensitif tidak berada di repository.
- [ ] Penamaan file mengikuti konvensi proyek.
- [ ] Repository siap digunakan oleh developer baru tanpa reorganisasi struktur.

---

# 🔗 Related Documents

- PRD v1.0
- RHS Baseline
- IDR-001 — Project Architecture
- IDR-002 — Technology Stack
- SDS — Software Design Specification
- AHS — Application Hosting Specification

---

# 📝 Notes

Repository Structure merupakan standar organisasi proyek yang harus dipatuhi oleh seluruh kontributor. Perubahan struktur utama repository hanya dilakukan melalui persetujuan tim dan pembaruan dokumentasi agar konsistensi proyek tetap terjaga.
