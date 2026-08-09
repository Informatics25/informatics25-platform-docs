#  TSS-005: Development, Testing & Coding Standards

> **Technical Setup Specification (TSS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** Final — Technology Baseline
>
> **Focus:** Testing Technology & Coding Standards

---

# 📖 Overview

Dokumen ini mendefinisikan technology baseline dan standard development yang berkaitan dengan:

* Unit Testing.
* Integration Testing.
* End-to-End Testing.
* Naming Convention.
* Folder Structure.
* Git Branching Strategy.
* Commit Convention.
* Code Review Guideline.

Testing dan coding standards digunakan untuk menjaga:

* Correctness.
* Maintainability.
* Consistency.
* Security.
* Reproducibility.
* Quality.

---

# 1. Testing Technology Stack

Testing strategy terdiri atas:

```text
Testing
   │
   ├── Unit Test
   │
   ├── Integration Test
   │
   └── End-to-End Test
```

Masing-masing level memiliki fokus dan scope yang berbeda.

---

# 2. Unit Test

## 2.1 Backend

Backend menggunakan **Go testing ecosystem**.

Fokus pengujian:

* Business rules.
* State transitions.
* Validation.
* Permission decisions.
* Error mapping.
* Utility logic.

---

## 2.2 Frontend

Frontend menggunakan **Vitest** sebagai baseline unit testing.

Fokus pengujian tetap diarahkan pada logic yang dapat diverifikasi secara isolated, terutama:

* State.
* Validation.
* Utility logic.
* Component-related logic yang relevan.

---

# 3. Integration Test

Integration testing menggunakan:

* PostgreSQL.
* Dependency test environment yang realistis.
* Docker Compose sebagai environment yang ideal apabila diperlukan.

Fokus integration test mencakup:

* Transaction.
* Foreign key.
* Migration.
* Repository query.
* Object storage adapter.
* External integration boundaries.

---

## 3.1 Integration Environment

Secara konseptual:

```text
Integration Test
       │
       ├── Application
       │
       ├── PostgreSQL
       │
       ├── Object Storage Adapter
       │
       └── External Integration Boundary
```

Environment test harus merepresentasikan dependency yang cukup realistis agar integration issue dapat ditemukan sebelum production.

---

# 4. End-to-End Test

**Playwright** direkomendasikan sebagai browser E2E testing technology.

Skenario minimal mencakup:

* Onboarding.
* Login.
* Password change.
* Dashboard.
* Announcement publication.
* Schedule exception.
* Resource upload/approval.
* RBAC.
* Critical error paths.

---

## 4.1 E2E Principle

E2E test berfokus pada critical user journey dan system behavior dari perspektif browser/user.

```text
Browser
   │
   ▼
Frontend
   │
   ▼
API
   │
   ▼
Backend
   │
   ▼
Database / Dependencies
```

Tujuannya bukan menggantikan unit maupun integration test, tetapi memverifikasi alur sistem secara menyeluruh.

---

# 5. Testing Scope

| Test Type            | Primary Technology                  | Primary Focus                  |
| -------------------- | ----------------------------------- | ------------------------------ |
| Unit Test — Backend  | Go testing ecosystem                | Business rules & backend logic |
| Unit Test — Frontend | Vitest                              | Frontend logic & state         |
| Integration Test     | PostgreSQL + realistic dependencies | Integration boundaries         |
| E2E Test             | Playwright                          | Critical browser/user journeys |

---

# 6. Testing Principles

Testing harus memprioritaskan area yang memiliki risiko terhadap correctness dan security.

Prioritas utama meliputi:

* Business rules.
* Validation.
* Permission decisions.
* Authentication/authorization.
* Database transaction.
* Migration.
* External integration.
* Critical user journeys.
* Error handling.

---

# 7. Coding Standards

Coding standards mendefinisikan convention yang digunakan pada source code, database, API, environment configuration, dan project structure.

---

# 8. Naming Convention

## 8.1 General Convention

| Area                   | Convention         |
| ---------------------- | ------------------ |
| Go package             | lowercase, concise |
| Go exported identifier | PascalCase         |
| Go local variable      | camelCase          |
| TypeScript variable    | camelCase          |
| TypeScript function    | camelCase          |
| TypeScript type        | PascalCase         |
| TypeScript interface   | PascalCase         |
| Vue component          | PascalCase         |
| Database table         | snake_case plural  |
| Database column        | snake_case         |
| API JSON field         | camelCase          |
| Environment variable   | UPPER_SNAKE_CASE   |

---

## 8.2 Examples

### Go

```go
package announcement

type AnnouncementService struct {
    repository AnnouncementRepository
}

func (s *AnnouncementService) PublishAnnouncement() {}
```

### TypeScript

```ts
interface Announcement {
  publishedAt: string
}

const announcementList = []
```

### Vue

```text
AnnouncementCard.vue
ScheduleCard.vue
ResourceTable.vue
```

### Database

```text
announcements
announcement_recipients
created_at
published_at
```

### Environment

```text
DATABASE_URL
JWT_SECRET
OBJECT_STORAGE_BUCKET
```

> Contoh di atas hanya menggambarkan convention penamaan yang ditetapkan pada TSS.

---

# 9. Folder Structure

Struktur folder harus mencerminkan:

* Domain boundary.
* Layer responsibility.
* Separation of concerns.

Baseline struktur meliputi:

* Domain/module separation.
* Transport.
* Application/service.
* Repository/data access.
* Infrastructure adapters.
* Shared cross-cutting concerns.

Detail final struktur folder dikunci pada **Backend Technical Specification** dan **Frontend Technical Specification**.

Secara konseptual:

```text
Application
│
├── Domain / Module
│
├── Transport
│
├── Application / Service
│
├── Repository / Data Access
│
├── Infrastructure Adapter
│
└── Shared / Cross-Cutting
```

---

# 10. Git Branching Strategy

TSS merekomendasikan **trunk-oriented development**.

Strategi menggunakan:

* Branch pendek.
* Feature branch.
* Fix branch.
* Pull Request ke `main`.

Git Flow tidak dipilih karena overhead lebih tinggi untuk tim kecil.

Branch `main` harus:

* Protected.
* Selalu buildable.

---

## 10.1 Branch Flow

```text
main
 │
 ├──── feature/*
 │          │
 │          ▼
 │      Pull Request
 │          │
 │          ▼
 ├──────── main
 │
 └──── fix/*
            │
            ▼
        Pull Request
```

Branch sebaiknya berumur pendek agar divergence dari `main` tetap minimal.

---

# 11. Commit Convention

Project menggunakan **Conventional Commits**.

Prefix yang digunakan:

```text
feat
fix
refactor
docs
test
chore
build
ci
perf
```

Satu commit idealnya merepresentasikan **satu perubahan logis**.

Contoh:

```text
feat: add announcement publication workflow
fix: handle expired refresh token
refactor: simplify schedule service
docs: update API documentation
test: add RBAC permission tests
chore: update dependency configuration
ci: improve test workflow
perf: optimize announcement query
```

---

# 12. Code Review Guideline

Code review harus memeriksa:

* Correctness.
* Security.
* Maintainability.
* Test coverage.
* Performance impact.
* Consistency dengan TSS.

---

## 12.1 High-Risk Changes

Review yang lebih ketat diperlukan untuk perubahan:

* Authentication.
* Authorization.
* Database migration.
* Audit.
* Data retention.

---

## 12.2 Pull Request Merge Requirement

Pull Request tidak boleh digabung apabila required test gagal.

```text
Pull Request
     │
     ▼
Required Tests
     │
 ┌───┴────┐
 │        │
Pass    Fail
 │        │
 ▼        ▼
Review   Block Merge
 │
 ▼
Merge
```

---

# 13. Breaking Change Documentation

Breaking change terhadap:

* API.
* Database.
* Security policy.

harus memiliki dokumentasi keputusan.

Tujuannya agar perubahan penting tidak hanya tercermin dalam source code, tetapi juga dapat ditelusuri melalui dokumentasi engineering.

---

# 14. Dependency Review

Dependency baru harus memiliki **justifikasi** sebelum ditambahkan.

Justifikasi harus mempertimbangkan Technology Governance yang telah ditetapkan pada:

```text
01-tss-001-technology-governance.md
```

---

# 15. Development Quality Gate

Secara konseptual:

```text
Code Change
     │
     ▼
Lint / Static Checks
     │
     ▼
Unit Tests
     │
     ▼
Integration Tests
     │
     ▼
E2E / Relevant Checks
     │
     ▼
Code Review
     │
     ▼
Merge
```

Tidak seluruh perubahan harus menjalankan seluruh test suite secara identik; scope pengujian mengikuti risiko dan perubahan yang dilakukan.

Namun required tests tidak boleh gagal ketika Pull Request akan digabungkan.

---

# 16. Coding Standards Constraints

Implementasi harus mematuhi:

* Naming convention yang telah ditetapkan.
* Domain/module separation.
* Layer responsibility.
* Trunk-oriented development.
* Protected `main`.
* Conventional Commits.
* Logical commit principle.
* Code review.
* Required test gate.
* Documentation untuk breaking changes.
* Justifikasi untuk dependency baru.

---

# 17. Testing & Development Baseline

```text
┌────────────────────────────────────┐
│        Development Standards       │
├────────────────────────────────────┤
│ Naming Convention                  │
│ Folder Structure                   │
│ Git Branching                      │
│ Conventional Commits               │
│ Code Review                        │
├────────────────────────────────────┤
│             Testing                │
├────────────────────────────────────┤
│ Go Testing                         │
│ Vitest                             │
│ Integration Testing                │
│ Playwright                         │
└────────────────────────────────────┘
```

---

# 18. Relationship with Other TSS Documents

```text
TSS-001
Technology Governance
        │
        ▼
TSS-005
Development, Testing &
Coding Standards
        │
        ├── Testing
        │   ├── Unit
        │   ├── Integration
        │   └── E2E
        │
        └── Development
            ├── Naming
            ├── Structure
            ├── Git
            ├── Commit
            └── Code Review
```

TSS-005 menjadi pedoman development yang menerapkan technology governance pada aktivitas engineering sehari-hari.

---

# 📚 Related Documents

* [TSS README](./README.md)
* [Technology Governance](./01-tss-001-technology-governance.md)
* [Frontend Technology Stack](./02-tss-002-frontend-technology-stack.md)
* [Backend & Database Stack](./03-tss-003-backend-and-database-stack.md)
* [Infrastructure, Security & Observability](./04-tss-004-infrastructure-security-and-observability.md)
* [Technology Decision & Final Baseline](./06-tss-006-technology-decision-and-final-baseline.md)
* [SDS README](../SDS/README.md)
* [DDS README](../DDS/README.md)
* [AHS README](../AHS/README.md)
* [IDR README](../IDR/README.md)

---

# 🔄 Traceability Matrix

| Area                    | Baseline                            |
| ----------------------- | ----------------------------------- |
| Backend Unit Test       | Go testing ecosystem                |
| Frontend Unit Test      | Vitest                              |
| Integration Test        | PostgreSQL + realistic dependencies |
| Integration Environment | Docker Compose recommended          |
| Browser E2E             | Playwright                          |
| Go Naming               | Go conventions                      |
| TypeScript Naming       | TypeScript conventions              |
| Vue Naming              | PascalCase                          |
| Database Naming         | snake_case                          |
| API JSON Naming         | camelCase                           |
| Environment Naming      | UPPER_SNAKE_CASE                    |
| Git Strategy            | Trunk-oriented development          |
| Main Branch             | Protected & buildable               |
| Commit Convention       | Conventional Commits                |
| Code Review             | Required                            |
| Required Tests          | Must pass before merge              |
| Breaking Changes        | Must be documented                  |
| New Dependencies        | Must be justified                   |

---

# ✅ Review Checklist

## Testing

* [ ] Backend unit testing menggunakan Go testing ecosystem.
* [ ] Frontend unit testing menggunakan Vitest.
* [ ] Integration testing menggunakan realistic dependencies.
* [ ] PostgreSQL digunakan dalam integration testing.
* [ ] Docker Compose dapat digunakan untuk integration environment.
* [ ] Playwright digunakan untuk browser E2E.
* [ ] Critical user journeys memiliki E2E coverage yang relevan.

## Coding Standards

* [ ] Naming conventions telah ditetapkan.
* [ ] Folder structure mengikuti domain dan layer responsibility.
* [ ] Trunk-oriented development digunakan.
* [ ] `main` protected dan buildable.
* [ ] Conventional Commits digunakan.
* [ ] Satu commit idealnya satu perubahan logis.
* [ ] Code review mencakup correctness, security, maintainability, testing, performance, dan consistency.
* [ ] Perubahan high-risk mendapat review lebih ketat.
* [ ] Required tests harus pass sebelum merge.
* [ ] Breaking changes didokumentasikan.
* [ ] Dependency baru memiliki justifikasi.

---

# 📝 Revision History

| Version | Date       | Description                                                   | Author          |
| ------- | ---------- | ------------------------------------------------------------- | --------------- |
| 1.0     | 2026-07-21 | Initial Development, Testing & Coding Standards documentation | Abidzar Dzakwan |
