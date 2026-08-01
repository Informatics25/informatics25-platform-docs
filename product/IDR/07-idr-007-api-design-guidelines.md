# IDR-007: API Design Guidelines

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD, RHS Series
>
> **Status:** ✅ Approved
>
> **Version:** 1.0

---

# 📖 Overview

Dokumen ini mendefinisikan standar perancangan REST API yang digunakan pada Platform Digital Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh endpoint API memiliki struktur, perilaku, dan format yang konsisten sehingga mudah digunakan, dipelihara, didokumentasikan, dan diuji.

Seluruh backend service wajib mengikuti standar yang didefinisikan pada dokumen ini.

---

# 🎯 Objectives

- Menyediakan standar REST API yang konsisten.
- Mempermudah integrasi Frontend dan Backend.
- Menjamin interoperabilitas antar layanan.
- Mempermudah pembuatan dokumentasi OpenAPI.
- Mendukung versioning tanpa mengganggu kompatibilitas.

---

# 🏗️ API Architecture Principles

Seluruh API harus mengikuti prinsip berikut.

- RESTful Design
- Resource-Oriented
- Stateless Communication
- Consistent Response
- Secure by Default
- Versioned API
- Idempotent Operations
- Least Privilege Access

---

# 🌐 Base URL

Seluruh endpoint menggunakan prefix berikut.

```text
/api/v1
```

Contoh:

```text
GET    /api/v1/users
GET    /api/v1/profile
GET    /api/v1/announcements
POST   /api/v1/resources
```

---

# 📦 Endpoint Naming Convention

Gunakan resource dalam bentuk jamak (*plural noun*).

## ✅ Benar

```text
/users
/resources
/schedules
/announcements
/events
/gallery
```

## ❌ Hindari

```text
/getUsers
/createAnnouncement
/deleteResource
```

---

# 🔗 HTTP Methods

| Method | Purpose |
|---------|----------|
| GET | Mengambil data |
| POST | Membuat data |
| PUT | Mengganti seluruh data |
| PATCH | Memperbarui sebagian data |
| DELETE | Menghapus data |

---

# 📄 Request Format

Gunakan JSON sebagai format pertukaran data.

Contoh:

```http
POST /api/v1/resources
```

```json
{
  "title": "Algoritma Dasar",
  "description": "Materi Minggu 1",
  "course_id": "uuid"
}
```

---

# 📥 Response Format

Seluruh endpoint menggunakan struktur berikut.

## Success

```json
{
  "success": true,
  "message": "Resource berhasil dibuat.",
  "data": {}
}
```

---

## Error

```json
{
  "success": false,
  "code": "RESOURCE_NOT_FOUND",
  "message": "Resource tidak ditemukan.",
  "errors": [],
  "trace_id": "af239abce12"
}
```

---

# 📑 Pagination Standard

Collection endpoint wajib mendukung pagination.

Contoh:

```http
GET /api/v1/resources?page=1&limit=20
```

Response:

```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total_items": 150,
    "total_pages": 8
  }
}
```

---

# 🔍 Filtering

Filtering menggunakan query parameter.

Contoh:

```text
GET /api/v1/resources?course=algoritma
```

```text
GET /api/v1/announcements?priority=critical
```

---

# ↕ Sorting

Gunakan parameter:

```text
sort=
order=
```

Contoh:

```text
GET /api/v1/resources?sort=created_at&order=desc
```

---

# 🔎 Searching

Gunakan parameter:

```text
search=
```

Contoh:

```text
GET /api/v1/resources?search=golang
```

---

# 🔐 Authentication

Endpoint privat menggunakan Bearer Token.

Contoh:

```http
Authorization: Bearer <access_token>
```

Endpoint publik tidak memerlukan autentikasi.

---

# 🔒 Authorization

Hak akses ditentukan berdasarkan RBAC.

Contoh:

| Endpoint | Student | Admin | Superadmin |
|----------|---------|-------|------------|
| GET /announcements | ✅ | ✅ | ✅ |
| POST /announcements | ❌ | ✅ | ✅ |
| DELETE /announcements | ❌ | ❌ | ✅ |

---

# 📊 HTTP Status Codes

| Status | Meaning |
|---------|----------|
| 200 | Success |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

---

# 🛡️ Validation Rules

- Semua input harus divalidasi di backend.
- Frontend validation tidak menggantikan backend validation.
- Input tidak valid harus menghasilkan HTTP 422.
- Error validasi harus menyebutkan field yang bermasalah.

Contoh:

```json
{
  "success": false,
  "code": "VALIDATION_ERROR",
  "message": "Validasi gagal.",
  "errors": [
    {
      "field": "title",
      "message": "Title wajib diisi."
    }
  ]
}
```

---

# 🚦 Rate Limiting

Endpoint autentikasi wajib memiliki pembatasan request.

| Endpoint | Recommendation |
|----------|----------------|
| Login | 5 request / menit |
| Reset Password | 3 request / 15 menit |
| OTP / 2FA | 5 request / 10 menit |

---

# 🔁 Idempotency

Operasi berikut harus bersifat idempotent.

- PUT
- DELETE

POST dapat menggunakan **Idempotency-Key** untuk operasi yang berpotensi dikirim ulang.

---

# 📚 API Versioning

Seluruh endpoint menggunakan versioning.

```text
/api/v1
```

Perubahan yang tidak kompatibel (*breaking changes*) harus menggunakan versi baru.

Contoh:

```text
/api/v2
```

---

# 📖 OpenAPI Documentation

Seluruh endpoint wajib didokumentasikan menggunakan OpenAPI Specification (OAS) 3.x.

Dokumentasi minimal mencakup:

- Endpoint
- Method
- Parameter
- Request Body
- Response
- Error Response
- Authentication
- Authorization
- Example Request
- Example Response

---

# 🧪 API Testing Requirements

Setiap endpoint minimal diuji untuk:

- Success response
- Validation error
- Unauthorized access
- Forbidden access
- Resource not found
- Conflict
- Unexpected error

---

# ✅ Review Checklist

- [ ] Endpoint mengikuti RESTful convention.
- [ ] Menggunakan versioning `/api/v1`.
- [ ] Request menggunakan JSON.
- [ ] Response mengikuti standar.
- [ ] HTTP status code sesuai.
- [ ] Authentication diterapkan.
- [ ] Authorization mengikuti RBAC.
- [ ] Pagination tersedia pada collection endpoint.
- [ ] Filtering dan sorting konsisten.
- [ ] OpenAPI diperbarui.

---

# 🔄 Traceability Matrix

| IDR Section | Related Requirement |
|-------------|---------------------|
| Authentication | RHS-001 |
| Official Information | RHS-002 |
| Dashboard | RHS-003 |
| Schedule API | RHS-004 |
| Knowledge Hub API | RHS-005 |
| RBAC | RHS-008 |
| Security | RHS-009 |
| Notification | RHS-011 |
| Audit Log | RHS-012 |

---

# 🔗 References

## Product Documents

- [Product Requirements Document](../PRD.md)

## Requirement Hardening

- [RHS README](../RHS/README.md)
- [RHS-001 Authentication](../RHS/01-rhs-001-authentication.md)
- [RHS-002 Official Information](../RHS/02-rhs-002-official-information.md)
- [RHS-004 Schedule](../RHS/04-rhs-004-schedule.md)
- [RHS-005 Knowledge Hub](../RHS/05-rhs-005-knowledge-hub.md)
- [RHS-008 RBAC](../RHS/08-rhs-008-rbac.md)
- [RHS-009 Security](../RHS/09-rhs-009-security-2fa.md)
- [RHS-011 Notification](../RHS/11-rhs-011-notification.md)
- [RHS-012 Audit Log](../RHS/12-rhs-012-audit-log.md)

## Previous IDR

- [IDR-001 Project Architecture](01-idr-001-project-architecture.md)
- [IDR-002 Technology Stack](02-idr-002-technology-stack.md)
- [IDR-003 Repository Structure](03-idr-003-repository-structure.md)
- [IDR-004 Development Workflow](04-idr-004-development-workflow.md)
- [IDR-005 Coding Standards](05-idr-005-coding-standards.md)
- [IDR-006 Error Handling Guidelines](06-idr-006-error-handling-guidelines.md)

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-01 | Initial API Design Guidelines documentation |
