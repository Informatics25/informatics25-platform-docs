# 📝 IDR-005: Coding Standards & Naming Convention

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD, RHS Series
>
> **Status:** ✅ Approved
>
> **Version:** 1.0

---

# 📖 Overview

Dokumen ini mendefinisikan standar penulisan kode (*Coding Standards*) dan konvensi penamaan (*Naming Convention*) yang digunakan pada Platform Digital Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh source code memiliki gaya penulisan yang konsisten, mudah dipahami, mudah dipelihara (*maintainable*), serta mendukung kolaborasi jangka panjang.

Dokumen ini berlaku untuk seluruh repository dalam proyek.

---

# 🎯 Objectives

- Menjaga konsistensi source code.
- Mempermudah proses code review.
- Mengurangi technical debt.
- Mempermudah onboarding developer baru.
- Meningkatkan maintainability.

---

# 📁 General Principles

Seluruh implementasi harus mengikuti prinsip berikut.

- Readability over cleverness.
- Consistency over personal preference.
- Explicit is better than implicit.
- Simplicity over complexity.
- Security by default.
- Least privilege.
- Single Responsibility Principle (SRP).
- DRY (Don't Repeat Yourself).
- KISS (Keep It Simple, Stupid).

---

# 📦 File Naming Convention

Gunakan nama file yang deskriptif dan konsisten.

| Item | Convention | Example |
|------|------------|---------|
| Vue Component | PascalCase | `DashboardCard.vue` |
| Layout | PascalCase | `AdminLayout.vue` |
| Page | kebab-case | `official-information.vue` |
| Store | camelCase | `useAuthStore.ts` |
| Utility | camelCase | `formatDate.ts` |
| Composable | camelCase | `useNotification.ts` |
| Type | PascalCase | `UserProfile.ts` |
| Interface | PascalCase | `Dashboard.ts` |
| Enum | PascalCase | `UserRole.ts` |
| Middleware | kebab-case | `auth-guard.ts` |

---

# 🔤 Variable Naming

Gunakan nama yang jelas dan mudah dipahami.

## Variables

```ts
const currentUser

const dashboardStatistics

const activeAnnouncement
```

Hindari:

```ts
const data

const temp

const x

const obj
```

---

## Boolean

Gunakan awalan yang menunjukkan kondisi.

```ts
isActive

isVerified

hasPermission

canPublish

shouldRefresh
```

---

## Functions

Gunakan kata kerja.

```ts
login()

logout()

publishAnnouncement()

approveResource()

calculateStatistics()

resetPassword()
```

---

## Async Functions

Gunakan nama yang tetap deskriptif.

```ts
async function fetchAnnouncements()

async function createUser()

async function updateSchedule()
```

---

# 🏛️ Class Naming

Gunakan PascalCase.

```ts
UserService

AuthenticationService

ScheduleRepository

DashboardController
```

---

# 📦 Interface Naming

Tidak menggunakan prefix "I".

Gunakan:

```ts
User

DashboardStatistics

Announcement

KnowledgeResource
```

Bukan:

```ts
IUser

IAnnouncement
```

---

# 📚 Enum Naming

```ts
UserRole

AnnouncementStatus

ScheduleType

NotificationChannel
```

---

# 🧩 Component Naming

Komponen UI menggunakan PascalCase.

```text
DashboardHeader.vue

NotificationCard.vue

ScheduleCalendar.vue

KnowledgeHubCard.vue
```

---

# 🗂️ Folder Structure Convention

```text
components/

pages/

layouts/

stores/

composables/

services/

repositories/

types/

utils/

middleware/

plugins/

assets/
```

Folder harus dikelompokkan berdasarkan tanggung jawab (*responsibility*).

---

# 📄 API Naming Convention

Menggunakan RESTful convention.

```text
GET    /api/v1/users

POST   /api/v1/users

PUT    /api/v1/users/{id}

PATCH  /api/v1/users/{id}

DELETE /api/v1/users/{id}
```

Gunakan bentuk jamak (*plural resource*).

Benar:

```text
/users

/resources

/schedules

/announcements
```

Hindari:

```text
/getUsers

/createUser

/userData
```

---

# 🗄️ Database Naming Convention

## Table

Gunakan snake_case.

```text
users

announcements

knowledge_resources

schedule_exceptions
```

---

## Column

```text
created_at

updated_at

deleted_at

published_at

is_active
```

---

## Primary Key

```text
id
```

---

## Foreign Key

```text
user_id

announcement_id

resource_id
```

---

# 📝 Comment Guidelines

Komentar hanya digunakan apabila benar-benar diperlukan.

Komentar harus menjelaskan **mengapa**, bukan **apa**.

Baik:

```ts
// Retry diperlukan karena WhatsApp API bersifat asynchronous.
```

Kurang baik:

```ts
// Increment i
i++
```

---

# 📋 Formatting Rules

Gunakan formatter proyek secara konsisten.

- Indentasi: 2 spasi.
- Gunakan UTF-8.
- Akhiri file dengan newline.
- Hindari trailing whitespace.

Formatting dilakukan secara otomatis menggunakan formatter yang disepakati tim.

---

# 🚫 Forbidden Practices

Tidak diperbolehkan:

- Penamaan variabel satu huruf (`x`, `y`, `z`).
- Magic number tanpa konstanta.
- Hardcoded credential.
- Hardcoded URL production.
- Fungsi dengan banyak tanggung jawab.
- File dengan ukuran berlebihan tanpa alasan yang jelas.
- Commented-out code yang tidak digunakan.
- Duplicate code.

---

# 📏 Code Quality Guidelines

Setiap implementasi diharapkan memenuhi prinsip berikut.

- Function memiliki satu tanggung jawab.
- Hindari nested condition yang berlebihan.
- Hindari duplicate logic.
- Gunakan early return jika memungkinkan.
- Pisahkan business logic dari presentation layer.
- Tangani error secara eksplisit.

---

# 🔍 Review Checklist

Reviewer perlu memverifikasi:

- [ ] Naming convention konsisten.
- [ ] Struktur folder sesuai standar.
- [ ] Tidak ada hardcoded credential.
- [ ] Tidak ada duplicate code.
- [ ] Function memiliki tanggung jawab tunggal.
- [ ] Formatter telah dijalankan.
- [ ] Kode mudah dipahami.

---

# 📊 Traceability

| Area | Reference |
|------|-----------|
| Authentication | [RHS-001](./rhs/01-rhs-001-authentication.md) |
| Official Information | [RHS-002](./rhs/02-rhs-002-official-information.md) |
| Dashboard | [RHS-003](./rhs/03-rhs-003-dashboard.md) |
| Schedule | [RHS-004](./rhs/04-rhs-004-schedule.md) |
| Knowledge Hub | [RHS-005](./rhs/05-rhs-005-knowledge-hub.md) |
| Security | [RHS-009](./rhs/09-rhs-009-security-2fa.md) |
| Development Workflow | [IDR-004](./04-idr-004-development-workflow.md) |

---

# 📄 Related Documents

- `PRD.md`
- `RHS/README.md`
- `01-idr-001-project-architecture.md`
- `02-idr-002-technology-stack.md`
- `03-idr-003-repository-structure.md`
- `04-idr-004-development-workflow.md`

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-01 | Initial Coding Standards & Naming Convention documentation |
