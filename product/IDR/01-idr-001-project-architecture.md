# IDR-001: Project Architecture

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD v1.0 • RHS Baseline
>
> **Status:** 🟢 Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini menjelaskan arsitektur tingkat tinggi (High-Level Architecture) Platform Digital Informatika Angkatan 2025.

Dokumen ini menjadi acuan seluruh tim engineering mengenai bagaimana setiap komponen sistem saling berinteraksi.

Dokumen ini **tidak menjelaskan implementasi kode**, melainkan struktur sistem secara keseluruhan.

---

# 🎯 Objectives

- Menjadi acuan seluruh engineer.
- Menentukan batas tanggung jawab setiap komponen.
- Mempermudah scaling di masa depan.
- Menghindari tight coupling.
- Menjadi dasar seluruh IDR berikutnya.

---

# 🏛️ Architectural Principles

Platform mengikuti prinsip berikut.

| Principle | Description |
|-----------|-------------|
| Modular Architecture | Setiap modul memiliki tanggung jawab yang jelas. |
| Separation of Concerns | UI, Business Logic, dan Data dipisahkan. |
| API First | Seluruh komunikasi menggunakan API. |
| Security by Default | Keamanan menjadi bagian dari desain awal. |
| Least Privilege | Hak akses seminimal mungkin. |
| Auditability | Aktivitas penting dapat ditelusuri. |
| Scalability | Mudah dikembangkan di masa depan. |
| Maintainability | Mudah dipelihara oleh pengelola berikutnya. |

---

# 🏗️ High Level Architecture

```text
                   Internet
                       │
                       ▼
               Reverse Proxy / CDN
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼
Frontend (Vue/Nuxt)           Static Assets
        │
        ▼
 REST / HTTPS API
        │
        ▼
 Backend Application
        │
 ┌──────┼───────────┐
 │      │           │
 ▼      ▼           ▼
Database Object Storage External Services
(PostgreSQL) (S3) (WhatsApp, GitHub)
```

---

# 🧩 System Components

## Frontend

Responsibilities

- User Interface
- Authentication UI
- Dashboard
- Forms
- Routing
- API Communication

Tidak menyimpan business logic utama.

---

## Backend

Responsibilities

- Authentication
- Authorization
- Business Rules
- Validation
- File Management
- Notification
- Audit Log
- API

Backend merupakan source of truth.

---

## Database

Responsibilities

- Persistent Storage
- Transaction
- Constraint
- Relationship

Database tidak menangani business logic.

---

## Object Storage

Digunakan untuk:

- Resource
- Gallery
- Attachment
- Poster
- Images

Tidak digunakan untuk menyimpan data relasional.

---

## External Services

Contoh:

- WhatsApp API
- GitHub
- Email Service
- Monitoring
- Analytics

Seluruh integrasi harus bersifat optional kecuali ditentukan lain oleh PRD.

---

# 🔄 Request Flow

```text
User

↓

Frontend

↓

Authentication

↓

Authorization

↓

Validation

↓

Business Logic

↓

Database / Object Storage

↓

Response

↓

Frontend

↓

User
```

---

# 📦 Core Modules

| Module | Responsibility |
|----------|---------------|
| Authentication | Login, Session |
| User Management | Account |
| Dashboard | Dashboard |
| Official Information | Announcement |
| Schedule | Academic Schedule |
| Knowledge Hub | Resource |
| Gallery | Gallery |
| Event | Event |
| Notification | Notification |
| Audit Log | Logging |
| Analytics | Metrics |

---

# 🔐 Security Boundary

```
Public Internet
      │
      ▼
Reverse Proxy
      │
HTTPS Only
      │
Backend API
      │
Authorization
      │
Database
```

Seluruh komunikasi harus menggunakan HTTPS.

---

# 📂 Suggested Repository Structure

```text
apps/
    frontend/
    backend/

packages/
    shared/

docs/
    PRD/
    RHS/
    IDR/

infra/

database/

scripts/

.github/
```

---

# 📊 Deployment Overview

```text
Developer

↓

GitHub

↓

CI/CD

↓

Build

↓

Deploy

↓

Production
```

---

# 🔄 Future Scalability

Arsitektur harus memungkinkan:

- Horizontal Scaling
- Multi Instance Backend
- CDN
- Queue
- Cache
- Multiple Storage Provider

Tanpa perubahan besar pada business logic.

---

# 📋 Architecture Decision Summary

| Area | Decision |
|------|----------|
| Frontend | SPA / SSR sesuai kebutuhan |
| Backend | REST API |
| Authentication | Session / JWT |
| Database | PostgreSQL |
| Storage | Object Storage |
| File Upload | Backend Gateway |
| Notification | Event Driven (Future) |
| Monitoring | Planned |
| Audit | Mandatory |

---

# ✅ Acceptance Criteria

- [ ] Arsitektur dipahami seluruh engineer.
- [ ] Batas tanggung jawab tiap komponen jelas.
- [ ] Tidak terjadi business logic pada frontend.
- [ ] Seluruh komunikasi melalui API.
- [ ] Database hanya menangani persistence.
- [ ] Object Storage tidak digunakan sebagai database.
- [ ] Seluruh IDR berikutnya mengikuti arsitektur ini.

---

# 🔗 Related Documents

- PRD v1.0
- RHS-008 — RBAC
- RHS-009 — Security
- RHS-012 — Audit Log
- IDR-002 — Technology Stack
- IDR-003 — Repository Structure

---

# 📝 Notes

Dokumen ini merupakan fondasi seluruh dokumentasi implementasi.

Perubahan terhadap arsitektur harus melalui Architecture Decision Record (ADR) atau pembaruan IDR yang disetujui agar konsistensi implementasi tetap terjaga.
