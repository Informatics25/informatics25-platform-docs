# TSS-003: Backend & Database Technology Stack

> **Technical Setup Specification (TSS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** Final — Technology Baseline
>
> **Focus:** Backend & Database Technology

---

# 📖 Overview

Dokumen ini mendefinisikan technology stack untuk backend dan database Platform Digital Informatika Angkatan 2025.

Backend bertanggung jawab terhadap:

* API.
* Business logic.
* Authentication.
* Authorization.
* Workflow.
* Integration.
* Operational services.

Database menjadi **system of record** untuk data relational dan transactional sistem.

Technology baseline terdiri atas:

```text
Backend
├── Go
├── Echo
├── JWT Access Token + Refresh Token
├── RBAC
├── File Upload Strategy
└── Logging & Error Handling

Database
├── PostgreSQL
├── Migration Strategy
└── SQLC
```

Pilihan tersebut merupakan technology baseline yang ditetapkan dalam TSS asli.

---

# 🏗️ Backend & Database Architecture

Secara konseptual:

```text
┌───────────────────────────────┐
│           Frontend            │
│          Nuxt 4 / TS          │
└───────────────┬───────────────┘
                │
                │ REST API
                ▼
┌───────────────────────────────┐
│            Backend            │
│              Go               │
│             Echo              │
├───────────────────────────────┤
│ Authentication / Authorization│
│ Business Logic / Workflow     │
│ Integration / Services        │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│          PostgreSQL           │
│        System of Record       │
└───────────────────────────────┘
```

Binary file menggunakan object storage dan tidak disimpan sebagai binary langsung di database. Detail storage dibahas pada TSS-004.

---

# 1. Go

## 1.1 Role

**Go** merupakan bahasa utama backend.

Go digunakan untuk:

* API.
* Business logic.
* Authentication.
* Authorization.
* Workflow.
* Integration.
* Operational services.

Go dipilih karena:

* Performance.
* Binary deployment sederhana.
* Concurrency.
* Runtime footprint.

---

## 1.2 Alternatives

Alternatif yang dipertimbangkan:

* Laravel / PHP.
* Node.js / NestJS.
* Java / Spring.

---

## 1.3 Trade-off

Keuntungan:

* Runtime footprint relatif kecil.
* Concurrency model yang sesuai untuk service backend.
* Deployment binary sederhana.
* Performance yang baik.

Trade-off:

* High-level CRUD abstraction lebih sedikit dibandingkan beberapa framework alternatif.

---

## 1.4 Technology Horizon

Go ditetapkan untuk penggunaan **jangka panjang**.

---

# 2. Echo

## 2.1 Role

**Echo** merupakan HTTP framework backend.

Digunakan untuk:

* Routing.
* Middleware.
* Request binding.
* Response handling.

Echo dipilih sebagai keseimbangan antara fitur dan simplicity.

---

## 2.2 Alternatives

Alternatif:

* Gin.
* Chi.
* Fiber.
* `net/http` murni.

---

## 2.3 Trade-off

Keuntungan:

* Menyediakan kebutuhan HTTP framework utama.
* Memiliki fitur middleware dan routing.
* Tetap relatif sederhana.

Trade-off:

* Convention internal tetap harus ditetapkan oleh project.

---

## 2.4 Technology Horizon

Echo ditetapkan untuk horizon **jangka panjang**.

---

# 3. JWT Access Token + Refresh Token

## 3.1 Role

Authentication menggunakan:

* JWT Access Token.
* Refresh Token.

Access token memiliki lifetime pendek.

Refresh token memiliki lifecycle yang mencakup:

* Rotation.
* Revocation.
* Controlled lifecycle.

---

## 3.2 Authentication Flow

```text
User
 │
 ▼
Login
 │
 ▼
Backend Authentication
 │
 ├──────────────► Access Token
 │
 └──────────────► Refresh Token
                    │
                    ▼
              Token Lifecycle
              Rotation / Revocation
```

---

## 3.3 Alternative

Alternatif:

* Server-side session.
* Opaque token.

Server-side session atau opaque token dapat memberikan revocation yang lebih mudah, tetapi menambah state management.

---

## 3.4 Critical Security Principle

**JWT bukan authorization source of truth.**

Permission tetap harus dievaluasi secara server-side.

```text
JWT
 │
 ▼
Identity / Claims
 │
 ▼
Server-side Authorization
 │
 ▼
Permission Decision
```

Dengan demikian, keberadaan JWT tidak secara otomatis memberikan akses terhadap resource.

---

## 3.5 Trade-off

Keuntungan:

* Stateless access token.
* Cocok untuk API architecture.
* Access token dapat memiliki lifetime pendek.

Trade-off:

* Refresh token security harus dikelola secara disiplin.
* Rotation dan revocation harus memiliki lifecycle yang jelas.

---

# 4. RBAC

## 4.1 Role

**Role-Based Access Control (RBAC)** digunakan untuk menegakkan role:

* Guest.
* Mahasiswa.
* Administrator.
* Superadmin.

RBAC menjadi mekanisme authorization utama pada baseline backend.

---

## 4.2 Authorization Principles

Authorization harus mengikuti:

* Default deny.
* Least privilege.
* Server-side enforcement.
* Role lifecycle management.
* Account status checking.
* Audit privilege changes.

---

## 4.3 Authorization Flow

```text
Authenticated User
        │
        ▼
Account Status
        │
        ▼
Role
        │
        ▼
Permission
        │
        ▼
Resource Access Decision
```

---

## 4.4 Future Consideration

ABAC atau policy engine dapat dipertimbangkan pada masa depan apabila authorization menjadi lebih granular.

Untuk baseline saat ini, RBAC tetap digunakan.

---

# 5. File Upload Strategy

## 5.1 Responsibility

Binary file **tidak disimpan di database**.

Backend bertanggung jawab terhadap:

* Authorization.
* Metadata.
* Validation.
* Lifecycle.
* Ownership.
* Reconciliation.

Object storage bertanggung jawab terhadap binary file.

---

## 5.2 Upload Flow

```text
Client
  │
  ▼
Backend
  │
  ├── Authorization
  ├── Validation
  ├── Metadata
  │
  ▼
Object Storage
  │
  ▼
Binary File
```

---

## 5.3 Required Validation

File upload harus mempertimbangkan:

* File size.
* MIME type.
* Extension.
* Ownership.
* Lifecycle.
* Reconciliation.

---

## 5.4 Storage Boundary

Database menyimpan metadata dan relationship.

Object storage menyimpan binary.

```text
PostgreSQL
   │
   ├── Ownership
   ├── Metadata
   ├── Lifecycle
   └── Object Reference
             │
             ▼
       Object Storage
             │
             └── Binary
```

Detail object storage architecture dibahas pada TSS-004.

---

# 6. Logging & Error Handling

## 6.1 Structured Logging

Backend menggunakan structured logging dengan level:

* `DEBUG`
* `INFO`
* `WARN`
* `ERROR`

---

## 6.2 Error Model

Error model membedakan setidaknya:

* Validation error.
* Authentication error.
* Authorization error.
* Not found.
* Conflict.
* Rate limit.
* Dependency failure.
* Internal error.

---

## 6.3 Sensitive Data Protection

Data berikut tidak boleh masuk log:

* Secret.
* Password.
* Token.
* Sensitive data.

Logging harus membantu troubleshooting tanpa menjadi sumber kebocoran informasi.

---

# 7. PostgreSQL

## 7.1 Role

**PostgreSQL** merupakan database utama dan **system of record**.

Digunakan untuk:

* Identity.
* RBAC.
* Schedules.
* Announcements.
* Resources.
* Audit.
* Governance.
* Metadata.

---

## 7.2 Why PostgreSQL

PostgreSQL dipilih karena:

* Relational integrity.
* Foreign keys.
* Transactions.
* Indexing.
* Mature ecosystem.

---

## 7.3 Alternatives

Alternatif yang dipertimbangkan:

* MySQL / MariaDB.
* MongoDB.

MongoDB tidak dipilih sebagai primary source karena domain memiliki banyak relational constraints dan governance requirements.

---

## 7.4 Database Responsibility

PostgreSQL menjadi sumber kebenaran untuk relational dan transactional data.

```text
Application
    │
    ▼
PostgreSQL
    │
    ├── Identity
    ├── RBAC
    ├── Schedule
    ├── Announcement
    ├── Resource
    ├── Audit
    ├── Governance
    └── Metadata
```

---

## 7.5 Technology Horizon

PostgreSQL ditetapkan untuk horizon **jangka panjang**.

---

# 8. Migration Strategy

## 8.1 Migration Principles

Database migration harus:

* Versioned.
* Deterministic.
* Repeatable.
* Tested.

Migration harus diuji terhadap:

1. Database kosong.
2. Database yang merepresentasikan kondisi production.

---

## 8.2 Destructive Migration

Destructive migration membutuhkan:

* Backup.
* Rollback plan.
* Recovery plan.

```text
Destructive Migration
        │
        ▼
Backup
        │
        ▼
Migration
        │
   ┌────┴────┐
   │         │
Success    Failure
   │         │
   ▼         ▼
Continue   Recovery
```

---

## 8.3 Schema Migration vs Data Migration

Schema migration dan data migration harus dapat dibedakan.

```text
Database Migration
       │
       ├── Schema Migration
       │
       └── Data Migration
```

Pemisahan tersebut membantu menjaga migration tetap dapat dipahami, diuji, dan dikontrol.

---

# 9. SQLC

## 9.1 Role

**SQLC** merupakan pendekatan utama database access.

SQL menjadi **source of truth**, kemudian SQLC menghasilkan type-safe code.

---

## 9.2 Alternative

Alternatif yang dipertimbangkan:

* GORM.

GORM dapat mempercepat CRUD, tetapi memiliki trade-off berupa kemungkinan menyembunyikan query behavior.

---

## 9.3 Why SQLC

SQLC dipilih untuk:

* Predictability.
* Query control.
* Maintainability.
* Type-safe generated code.

---

## 9.4 Trade-off

Trade-off utama:

* Developer harus menulis SQL.
* Terdapat generation step.
* Membutuhkan disiplin terhadap query dan schema.

Trade-off tersebut diterima demi kontrol query dan maintainability.

---

## 9.5 Technology Horizon

SQLC ditetapkan untuk horizon **jangka panjang**.

---

# 🧩 Backend Technology Summary

| Technology           | Role                  | Horizon                 |
| -------------------- | --------------------- | ----------------------- |
| Go                   | Backend language      | Jangka panjang          |
| Echo                 | HTTP framework        | Jangka panjang          |
| JWT + Refresh Token  | Authentication        | Jangka menengah-panjang |
| RBAC                 | Authorization         | Baseline                |
| File Upload Strategy | File management       | Baseline                |
| Structured Logging   | Logging               | Baseline                |
| PostgreSQL           | System of record      | Jangka panjang          |
| Migration Strategy   | Schema/data evolution | Baseline                |
| SQLC                 | Database access       | Jangka panjang          |

---

# 🔐 Security Baseline

Backend security baseline mencakup:

```text
Authentication
      │
      ▼
JWT + Refresh Token
      │
      ▼
Account Status
      │
      ▼
RBAC
      │
      ▼
Server-side Authorization
      │
      ▼
Resource Access
```

Security-sensitive information harus tetap berada pada trusted backend boundary.

Frontend tidak dianggap sebagai security enforcement boundary.

---

# 🚧 Backend & Database Constraints

Implementasi harus mematuhi constraint berikut:

* Go menjadi bahasa utama backend.
* Echo menjadi HTTP framework baseline.
* JWT access token memiliki lifetime pendek.
* Refresh token memiliki lifecycle dan revocation yang terkontrol.
* JWT bukan authorization source of truth.
* Authorization harus dievaluasi server-side.
* RBAC menggunakan default deny dan least privilege.
* Binary file tidak disimpan di PostgreSQL.
* File upload harus melalui authorization dan validation.
* Sensitive data tidak boleh masuk log.
* PostgreSQL menjadi system of record untuk relational/transactional data.
* Migration harus versioned, deterministic, repeatable, dan tested.
* Destructive migration membutuhkan backup dan recovery plan.
* Schema migration dan data migration harus dapat dibedakan.
* SQLC menjadi database access strategy utama.
* SQL menjadi source of truth untuk query SQLC.

---

# 🔗 Relationship with Other Documents

```text
TSS-001
Technology Governance
        │
        ▼
TSS-003
Backend & Database Stack
        │
        ├── Backend
        │   ├── Go
        │   ├── Echo
        │   ├── Authentication
        │   ├── RBAC
        │   └── Error Handling
        │
        └── Database
            ├── PostgreSQL
            ├── Migration
            └── SQLC
```

Backend dan database juga harus mengikuti architecture yang ditetapkan pada SDS dan hardening constraints pada AHS.

---

# 📚 Related Documents

* [TSS README](./README.md)
* [Technology Governance](./01-tss-001-technology-governance.md)
* [Frontend Technology Stack](./02-tss-002-frontend-technology-stack.md)
* [Infrastructure, Security & Observability](./04-tss-004-infrastructure-security-and-observability.md)
* [Development, Testing & Coding Standards](./05-tss-005-development-testing-and-coding-standards.md)
* [Technology Decision & Final Baseline](./06-tss-006-technology-decision-and-final-baseline.md)
* [SDS README](../SDS/README.md)
* [DDS README](../DDS/README.md)
* [AHS README](../AHS/README.md)
* [IDR README](../IDR/README.md)

---

# 🔄 Traceability Matrix

| Concern          | Technology / Strategy                  |
| ---------------- | -------------------------------------- |
| Backend Language | Go                                     |
| HTTP Framework   | Echo                                   |
| Authentication   | JWT + Refresh Token                    |
| Authorization    | RBAC                                   |
| File Upload      | Backend + S3-compatible Object Storage |
| Logging          | Structured Logging                     |
| Error Handling   | Structured Error Model                 |
| Database         | PostgreSQL                             |
| Migration        | Versioned Database Migration           |
| Database Access  | SQLC                                   |

---

# ✅ Review Checklist

* [ ] Go telah ditetapkan sebagai backend language.
* [ ] Echo telah ditetapkan sebagai HTTP framework.
* [ ] JWT access token dan refresh token telah didefinisikan.
* [ ] JWT bukan authorization source of truth.
* [ ] RBAC telah ditetapkan.
* [ ] Default deny telah ditetapkan.
* [ ] Least privilege telah ditetapkan.
* [ ] File upload boundary telah ditentukan.
* [ ] Binary file tidak disimpan di database.
* [ ] Structured logging telah ditentukan.
* [ ] Sensitive data logging restriction telah ditentukan.
* [ ] PostgreSQL telah ditetapkan sebagai system of record.
* [ ] Migration strategy telah ditentukan.
* [ ] Destructive migration requirement telah ditentukan.
* [ ] SQLC telah ditetapkan sebagai database access strategy.
* [ ] SQL menjadi source of truth untuk SQLC.
* [ ] Technology constraints telah didokumentasikan.

---

# 📝 Revision History

| Version | Date       | Description                                               | Author          |
| ------- | ---------- | --------------------------------------------------------- | --------------- |
| 1.0     | 2026-07-21 | Initial Backend & Database Technology Stack documentation | Abidzar Dzakwan |
