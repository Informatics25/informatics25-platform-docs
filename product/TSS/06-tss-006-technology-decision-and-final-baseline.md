# TSS-006: Technology Decision & Final Baseline

> **Technical Setup Specification (TSS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** Final — Official Technology Baseline
>
> **Purpose:** Technology Decision Matrix & Final Stack Reference

---

# 📖 Overview

Dokumen ini merupakan **penutup Technical Setup Specification (TSS)** dan mendefinisikan baseline teknologi final untuk Platform Digital Informatika Angkatan 2025.

Dokumen ini merangkum:

* Technology Decision Matrix.
* Final Technology Stack.
* Technology Horizon.
* Major Trade-off.
* Final Consistency Statement.
* Governance terhadap perubahan technology stack.

Dokumen ini tidak menggantikan detail teknis pada TSS-001 sampai TSS-005. Dokumen ini berfungsi sebagai **single reference** terhadap technology stack yang telah diputuskan.

---

# 1. Technology Decision Matrix

Technology Decision Matrix merangkum pilihan teknologi final, alternatif utama, trade-off, dan horizon penggunaannya.

| Layer              | Pilihan Final       | Alternatif Utama    | Trade-off Utama                                         | Horizon                 |
| ------------------ | ------------------- | ------------------- | ------------------------------------------------------- | ----------------------- |
| Frontend Framework | Nuxt 4              | Vue + Vite; Next.js | Convention lebih kuat, tetapi boilerplate lebih sedikit | Jangka panjang          |
| Language           | TypeScript          | JavaScript          | Type safety membutuhkan disiplin tambahan               | Jangka panjang          |
| Styling            | Tailwind CSS v4     | CSS Modules; UnoCSS | Utility markup lebih panjang                            | Jangka panjang          |
| UI                 | Nuxt UI             | shadcn-vue          | Lebih cepat, tetapi ecosystem dependency                | Jangka menengah         |
| State              | Pinia               | Composables; Vuex   | Global state harus dibatasi                             | Jangka panjang          |
| Form               | VeeValidate + Zod   | Yup; Valibot        | Schema maintenance diperlukan                           | Jangka menengah         |
| HTTP               | `$fetch` / ofetch   | Axios               | Abstraction convention diperlukan                       | Mudah diubah            |
| Backend            | Go                  | Laravel; NestJS     | CRUD abstraction lebih sedikit                          | Jangka panjang          |
| HTTP Framework     | Echo                | Gin; Chi; Fiber     | Convention internal diperlukan                          | Jangka panjang          |
| Authentication     | JWT + Refresh Token | Server Session      | Revocation lebih kompleks                               | Jangka menengah-panjang |
| Database           | PostgreSQL          | MySQL; MongoDB      | Migration discipline diperlukan                         | Jangka panjang          |
| DB Access          | SQLC                | GORM                | SQL harus eksplisit                                     | Jangka panjang          |
| Storage            | S3-compatible       | MinIO               | Vendor dependency atau self-hosting trade-off           | Jangka panjang          |
| API                | REST + OpenAPI 3.1  | GraphQL; gRPC       | Kurang fleksibel dari GraphQL                           | Jangka panjang          |
| Containers         | Docker + Compose    | Native; Kubernetes  | Compose tidak untuk orchestration skala besar           | Jangka menengah-panjang |
| CI/CD              | GitHub Actions      | Jenkins; GitLab CI  | Terkait ecosystem GitHub                                | Jangka menengah         |
| Password           | Argon2id            | bcrypt              | Parameter tuning diperlukan                             | Jangka panjang          |
| Admin 2FA          | TOTP                | SMS/Email OTP       | Recovery procedure harus kuat                           | Jangka panjang          |
| E2E                | Playwright          | Cypress             | Test suite tetap membutuhkan maintenance                | Jangka menengah         |

## Decision matrix tersebut berasal dari Technology Stack Specification dan menjadi dasar baseline final.

# 2. Final Stack Reference

Bagian ini merupakan **referensi utama** untuk teknologi final yang digunakan pada project.

| Komponen             | Teknologi Final                                        |
| -------------------- | ------------------------------------------------------ |
| Frontend Framework   | Nuxt 4                                                 |
| Frontend Language    | TypeScript                                             |
| Styling              | Tailwind CSS v4                                        |
| UI Component Library | Nuxt UI                                                |
| State Management     | Pinia                                                  |
| Form Validation      | VeeValidate + Zod                                      |
| HTTP Client          | `$fetch` / ofetch                                      |
| Icons                | Lucide                                                 |
| Font                 | Geist                                                  |
| Backend Language     | Go                                                     |
| Backend Framework    | Echo                                                   |
| Authentication       | JWT Access Token + Refresh Token                       |
| Authorization        | RBAC — Guest, Mahasiswa, Administrator, Superadmin     |
| Password Hashing     | Argon2id                                               |
| Administrative 2FA   | TOTP + Backup Codes                                    |
| Database             | PostgreSQL                                             |
| Database Access      | SQLC                                                   |
| Migration            | Versioned, deterministic migration system              |
| File Storage         | S3-compatible Object Storage                           |
| Metadata             | PostgreSQL                                             |
| API                  | REST                                                   |
| API Contract         | OpenAPI 3.1                                            |
| API Versioning       | Major version path, baseline `/api/v1`                 |
| Containerization     | Docker                                                 |
| Local Orchestration  | Docker Compose                                         |
| CI/CD                | GitHub Actions                                         |
| Logging              | Structured logging                                     |
| Monitoring           | Health checks + metrics + actionable alerts            |
| Unit Testing         | Go test + Vitest                                       |
| Integration Testing  | Dockerized PostgreSQL and dependency test environments |
| E2E Testing          | Playwright                                             |
| Git Strategy         | Trunk-oriented short-lived branches + PR               |
| Commit Convention    | Conventional Commits                                   |

---

# 3. Final Technology Architecture

Secara konseptual, final technology stack membentuk architecture berikut:

```text
┌──────────────────────────────────────────────────────┐
│                    FRONTEND                          │
│                                                      │
│  Nuxt 4 + TypeScript                                 │
│  Tailwind CSS v4 + Nuxt UI                           │
│  Pinia + VeeValidate/Zod                             │
│  $fetch/ofetch + Lucide + Geist                      │
└────────────────────────┬─────────────────────────────┘
                         │
                         │ REST / OpenAPI 3.1
                         │ /api/v1
                         ▼
┌──────────────────────────────────────────────────────┐
│                     BACKEND                          │
│                                                      │
│  Go + Echo                                           │
│  JWT + Refresh Token                                 │
│  RBAC                                                │
│  Business Logic                                      │
└───────────────┬───────────────────────┬──────────────┘
                │                       │
                ▼                       ▼
┌────────────────────────┐   ┌────────────────────────┐
│      PostgreSQL        │   │   S3-Compatible        │
│                        │   │   Object Storage       │
│ Identity               │   │                        │
│ RBAC                   │   │ Binary Files           │
│ Schedules              │   │                        │
│ Announcements          │   │                        │
│ Resources              │   │                        │
│ Audit                  │   │                        │
│ Metadata               │   │                        │
└────────────────────────┘   └────────────────────────┘

              Infrastructure
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
     Docker     GitHub       Monitoring
     Compose    Actions      & Logging
```

---

# 4. Technology Baseline by Layer

## 4.1 Frontend Layer

```text
Nuxt 4
   │
   ├── TypeScript
   ├── Tailwind CSS v4
   ├── Nuxt UI
   ├── Pinia
   ├── VeeValidate + Zod
   ├── $fetch / ofetch
   ├── Lucide
   └── Geist
```

---

## 4.2 Backend Layer

```text
Go
 │
 └── Echo
      │
      ├── REST API
      ├── Authentication
      ├── Authorization
      └── Business Logic
```

---

## 4.3 Data Layer

```text
PostgreSQL
 │
 └── SQLC
      │
      └── Explicit SQL
```

Binary file dipisahkan dari relational metadata:

```text
PostgreSQL
    │
    └── Metadata

S3-Compatible Storage
    │
    └── Binary
```

---

## 4.4 Infrastructure Layer

```text
Docker
   │
   └── Docker Compose
          │
          └── Local / Integration Environment

GitHub Actions
   │
   └── CI/CD

Monitoring
   │
   ├── Health Checks
   ├── Metrics
   └── Actionable Alerts
```

---

# 5. Security Baseline

Technology baseline final juga menetapkan security components:

```text
Authentication
      │
      ├── JWT Access Token
      └── Refresh Token
             │
             ▼
Authorization
      │
      └── RBAC
             │
             ├── Guest
             ├── Mahasiswa
             ├── Administrator
             └── Superadmin

Password
      │
      └── Argon2id

Administrative 2FA
      │
      ├── TOTP
      └── Backup Codes
```

Baseline tersebut merupakan bagian dari final stack reference.

---

# 6. API Baseline

API final menggunakan:

```text
REST
 │
 └── OpenAPI 3.1
        │
        └── /api/v1
```

Kontrak API digunakan sebagai reference bersama untuk:

* Frontend.
* Backend.
* QA.
* Integration.

Major version digunakan pada path.

---

# 7. Development Baseline

Development workflow menggunakan:

```text
Developer
    │
    ▼
Short-Lived Branch
    │
    ▼
Conventional Commit
    │
    ▼
Pull Request
    │
    ▼
CI / Testing
    │
    ▼
Code Review
    │
    ▼
main
```

Git strategy yang digunakan adalah **trunk-oriented development dengan short-lived branches dan Pull Request**.

Commit menggunakan **Conventional Commits**.

---

# 8. Testing Baseline

Final testing stack:

```text
Testing
   │
   ├── Unit
   │     ├── Go test
   │     └── Vitest
   │
   ├── Integration
   │     └── Dockerized PostgreSQL
   │
   └── E2E
         └── Playwright
```

---

# 9. Technology Horizon

Technology tidak seluruhnya memiliki horizon yang sama.

### Long-term

* Nuxt 4.
* TypeScript.
* Tailwind CSS v4.
* Pinia.
* Go.
* Echo.
* PostgreSQL.
* SQLC.
* S3-compatible storage.
* REST + OpenAPI 3.1.
* Argon2id.
* TOTP.

### Medium-term

* Nuxt UI.
* VeeValidate + Zod.
* JWT + Refresh Token.
* GitHub Actions.
* Playwright.
* Docker + Compose.

### Easily Replaceable

* `$fetch` / ofetch.

Penggantian technology tidak boleh dilakukan hanya berdasarkan preferensi individual apabila perubahan tersebut memengaruhi architecture boundary atau security model.

---

# 10. Major Trade-offs

Technology baseline menerima sejumlah trade-off yang telah diputuskan.

| Decision              | Accepted Trade-off                   |
| --------------------- | ------------------------------------ |
| Nuxt 4                | Convention lebih kuat                |
| TypeScript            | Membutuhkan disiplin type            |
| Tailwind CSS          | Utility markup dapat lebih panjang   |
| Nuxt UI               | Dependency terhadap ecosystem        |
| Pinia                 | Global state harus dibatasi          |
| VeeValidate + Zod     | Schema maintenance                   |
| `$fetch` / ofetch     | Memerlukan convention abstraction    |
| Go                    | CRUD abstraction lebih sedikit       |
| Echo                  | Internal convention perlu ditetapkan |
| JWT + Refresh Token   | Revocation lebih kompleks            |
| PostgreSQL            | Migration discipline                 |
| SQLC                  | SQL harus eksplisit                  |
| S3-compatible storage | Vendor/self-hosting trade-off        |
| REST                  | Kurang fleksibel dibanding GraphQL   |
| Docker Compose        | Bukan orchestration skala besar      |
| GitHub Actions        | Terikat dengan ecosystem GitHub      |
| Argon2id              | Parameter tuning                     |
| TOTP                  | Recovery procedure harus kuat        |
| Playwright            | Test suite membutuhkan maintenance   |

## Trade-off tersebut merupakan bagian dari decision matrix sumber.

# 11. Final Consistency Statement

Technology Stack Specification dinyatakan konsisten dengan prinsip dan keputusan proyek.

Baseline mempertahankan prinsip berikut:

1. Platform tetap menjadi **Single Source of Truth**.
2. Authorization menggunakan **RBAC dengan least privilege**.
3. Binary file dipisahkan dari relational metadata.
4. Audit dan privacy memiliki lifecycle terkontrol.
5. External dependency tidak menjadi single point of failure.
6. Stack dipilih dengan mempertimbangkan:

   * Skala satu angkatan.
   * Biaya operasional.
   * Maintainability.
   * Security.
   * Kemungkinan ekspansi ke beberapa angkatan.

---

# 12. Technology Change Governance

Dokumen ini menjadi **baseline resmi**.

Perubahan teknologi yang memengaruhi salah satu boundary berikut wajib melalui proses keputusan arsitektur yang terdokumentasi:

* Architecture boundary.
* Security model.
* Database consistency.
* API contract.
* Operational model.

Secara konseptual:

```text
Technology Change
       │
       ▼
Impact Assessment
       │
       ▼
Architecture Decision
       │
       ▼
Documentation
       │
       ▼
Approved Change
       │
       ▼
Updated Baseline
```

---

# 13. Final Technology Baseline

Sebagai reference singkat:

```text
┌────────────────────────────────────────────┐
│        INFORMATIKA25 FINAL STACK           │
├────────────────────────────────────────────┤
│ Frontend                                   │
│ Nuxt 4 + TypeScript                        │
│ Tailwind CSS v4 + Nuxt UI                  │
│ Pinia + VeeValidate/Zod                    │
│ $fetch/ofetch + Lucide + Geist             │
├────────────────────────────────────────────┤
│ Backend                                    │
│ Go + Echo                                  │
│ JWT + Refresh Token + RBAC                 │
├────────────────────────────────────────────┤
│ Database                                   │
│ PostgreSQL + SQLC                          │
├────────────────────────────────────────────┤
│ Storage                                    │
│ S3-Compatible Object Storage               │
├────────────────────────────────────────────┤
│ API                                        │
│ REST + OpenAPI 3.1 + /api/v1              │
├────────────────────────────────────────────┤
│ Infrastructure                             │
│ Docker + Docker Compose                    │
│ GitHub Actions                             │
├────────────────────────────────────────────┤
│ Security                                   │
│ Argon2id + TOTP + Backup Codes             │
├────────────────────────────────────────────┤
│ Observability                              │
│ Structured Logs + Health + Metrics         │
│ + Actionable Alerts                        │
├────────────────────────────────────────────┤
│ Testing                                    │
│ Go test + Vitest + Integration + Playwright│
├────────────────────────────────────────────┤
│ Development                                │
│ Trunk-oriented + PR + Conventional Commits│
└────────────────────────────────────────────┘
```

---

# 14. Relationship with TSS Documentation

TSS-006 menjadi summary/reference dari keseluruhan TSS:

```text
TSS
│
├── 01 Technology Governance
│
├── 02 Frontend Technology Stack
│
├── 03 Backend & Database Stack
│
├── 04 Infrastructure, Security & Observability
│
├── 05 Development, Testing & Coding Standards
│
└── 06 Technology Decision & Final Baseline
                         │
                         ▼
                  FINAL BASELINE
```

TSS-006 **tidak menggantikan** file TSS sebelumnya. Detail implementation tetap dirujuk pada dokumen masing-masing.

---

# 📚 Related Documents

* [TSS README](./README.md)
* [Technology Governance](./01-tss-001-technology-governance.md)
* [Frontend Technology Stack](./02-tss-002-frontend-technology-stack.md)
* [Backend & Database Stack](./03-tss-003-backend-and-database-stack.md)
* [Infrastructure, Security & Observability](./04-tss-004-infrastructure-security-and-observability.md)
* [Development, Testing & Coding Standards](./05-tss-005-development-testing-and-coding-standards.md)
* [SDS README](../SDS/README.md)
* [DDS README](../DDS/README.md)
* [AHS README](../AHS/README.md)
* [IDR README](../IDR/README.md)

---

# 🔄 Traceability Matrix

| Area                | Final Baseline                              |
| ------------------- | ------------------------------------------- |
| Frontend            | Nuxt 4 + TypeScript                         |
| Styling             | Tailwind CSS v4                             |
| UI                  | Nuxt UI                                     |
| State               | Pinia                                       |
| Form                | VeeValidate + Zod                           |
| HTTP Client         | `$fetch` / ofetch                           |
| Icons               | Lucide                                      |
| Font                | Geist                                       |
| Backend             | Go + Echo                                   |
| Authentication      | JWT + Refresh Token                         |
| Authorization       | RBAC                                        |
| Password            | Argon2id                                    |
| Admin 2FA           | TOTP + Backup Codes                         |
| Database            | PostgreSQL                                  |
| DB Access           | SQLC                                        |
| Migration           | Versioned, deterministic                    |
| Storage             | S3-compatible Object Storage                |
| API                 | REST                                        |
| API Contract        | OpenAPI 3.1                                 |
| API Versioning      | Major version path / `/api/v1`              |
| Containers          | Docker + Docker Compose                     |
| CI/CD               | GitHub Actions                              |
| Logging             | Structured Logging                          |
| Monitoring          | Health Checks + Metrics + Actionable Alerts |
| Unit Testing        | Go test + Vitest                            |
| Integration Testing | Dockerized PostgreSQL + Dependencies        |
| E2E                 | Playwright                                  |
| Git                 | Trunk-oriented                              |
| Commits             | Conventional Commits                        |

---

# ✅ Final Review Checklist

* [ ] Technology Decision Matrix telah didokumentasikan.
* [ ] Final Stack Reference telah ditetapkan.
* [ ] Frontend stack telah dikunci.
* [ ] Backend stack telah dikunci.
* [ ] Database stack telah dikunci.
* [ ] Storage strategy telah dikunci.
* [ ] API stack dan contract telah dikunci.
* [ ] Containerization telah dikunci.
* [ ] CI/CD telah dikunci.
* [ ] Security baseline telah dikunci.
* [ ] Monitoring baseline telah dikunci.
* [ ] Testing baseline telah dikunci.
* [ ] Git strategy telah dikunci.
* [ ] Commit convention telah dikunci.
* [ ] Major trade-offs telah terdokumentasi.
* [ ] Technology horizon telah dicatat.
* [ ] Technology change governance telah ditetapkan.
* [ ] Final consistency statement telah didokumentasikan.

---

# 📝 Revision History

| Version | Date       | Description                                                | Author          |
| ------- | ---------- | ---------------------------------------------------------- | --------------- |
| 1.0     | 2026-07-21 | Initial Technology Decision & Final Baseline documentation | Abidzar Dzakwan |
