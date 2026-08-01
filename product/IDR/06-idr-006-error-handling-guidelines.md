# IDR-006: Error Handling Guidelines

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD, RHS Series
>
> **Status:** ✅ Approved
>
> **Version:** 1.0

---

# 📖 Overview

Dokumen ini mendefinisikan standar penanganan kesalahan (*Error Handling Guidelines*) yang digunakan pada Platform Digital Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh aplikasi memiliki mekanisme penanganan error yang konsisten, aman, mudah dipahami oleh pengguna, serta dapat ditelusuri melalui logging dan audit.

Dokumen ini berlaku untuk seluruh komponen aplikasi, termasuk Frontend, Backend, API, Background Worker, dan Integrasi Eksternal.

---

# 🎯 Objectives

- Menyediakan standar penanganan error yang konsisten.
- Menghindari kebocoran informasi sensitif.
- Mempermudah proses debugging.
- Memastikan seluruh error dapat ditelusuri.
- Memberikan pengalaman pengguna yang baik ketika terjadi kegagalan.

---

# 🏗️ Error Handling Principles

Seluruh implementasi wajib mengikuti prinsip berikut.

- Fail securely.
- Never expose internal implementation.
- Log everything important.
- Return meaningful error messages.
- Use standardized error responses.
- Keep user messages simple.
- Separate user-facing errors from system errors.

---

# 📂 Error Categories

| Category | Description | Example |
|----------|-------------|----------|
| Validation Error | Input tidak valid | Password terlalu pendek |
| Authentication Error | Login gagal | Password salah |
| Authorization Error | Hak akses ditolak | Mahasiswa mengakses halaman admin |
| Resource Error | Resource tidak ditemukan | Announcement tidak ada |
| Conflict Error | Konflik data | Duplicate NIM |
| Business Rule Error | Melanggar aturan bisnis | Publish informasi yang belum diverifikasi |
| External Service Error | Layanan eksternal gagal | WhatsApp API timeout |
| Infrastructure Error | Gangguan sistem | Database connection failed |
| Unexpected Error | Error tidak terduga | Null pointer exception |

---

# 🌐 HTTP Status Code Standard

| Status | Meaning |
|---------|---------|
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
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

# 📦 Standard API Error Response

Seluruh endpoint API harus menggunakan struktur respons yang konsisten.

```json
{
  "success": false,
  "code": "AUTH_INVALID_PASSWORD",
  "message": "Username atau password tidak valid.",
  "errors": [],
  "trace_id": "3d71c6b4d52f4db6"
}
```

---

## Response Fields

| Field | Description |
|--------|-------------|
| success | Status keberhasilan request |
| code | Error code yang unik |
| message | Pesan yang aman ditampilkan kepada pengguna |
| errors | Detail validasi (jika ada) |
| trace_id | ID untuk keperluan tracing dan debugging |

---

# 🏷️ Error Code Convention

Gunakan format berikut.

```text
MODULE_ERROR_NAME
```

Contoh:

```text
AUTH_INVALID_PASSWORD

AUTH_ACCOUNT_SUSPENDED

AUTH_ONBOARDING_REQUIRED

ANNOUNCEMENT_NOT_FOUND

SCHEDULE_CONFLICT

RESOURCE_NOT_APPROVED

PERMISSION_DENIED

DATABASE_CONNECTION_FAILED

WHATSAPP_TIMEOUT
```

---

# 📝 User-Friendly Error Messages

Pesan yang ditampilkan kepada pengguna tidak boleh mengandung informasi internal sistem.

| Internal Error | User Message |
|----------------|--------------|
| SQL Exception | Terjadi kesalahan sistem. Silakan coba beberapa saat lagi. |
| Database Timeout | Layanan sedang sibuk. Silakan coba kembali. |
| Invalid Password | Username atau password tidak valid. |
| Unauthorized | Anda tidak memiliki izin untuk melakukan tindakan ini. |

---

# 📊 Logging Guidelines

Error penting wajib dicatat pada sistem logging.

Informasi minimum yang harus dicatat:

- Timestamp
- User ID (jika tersedia)
- IP Address
- Request ID
- Trace ID
- Endpoint
- HTTP Method
- Error Code
- Error Category
- Stack Trace (internal only)

---

# 🚫 Sensitive Information

Informasi berikut **tidak boleh** ditulis ke log maupun dikirim ke client.

- Password
- Password Hash
- JWT Secret
- API Key
- Session Token
- Refresh Token
- OTP
- TOTP Secret
- Backup Code
- Database Credential

---

# 🔁 Retry Strategy

Retry hanya dilakukan pada error yang bersifat sementara (*transient*).

| Scenario | Retry |
|----------|-------|
| Database timeout | ✅ |
| WhatsApp API timeout | ✅ |
| Object Storage timeout | ✅ |
| Validation error | ❌ |
| Authentication error | ❌ |
| Authorization error | ❌ |

Gunakan **Exponential Backoff** untuk seluruh mekanisme retry.

---

# 🔐 Security Considerations

- Seluruh exception internal harus disembunyikan dari pengguna.
- Stack trace hanya tersedia pada log internal.
- Error tidak boleh mengungkap struktur database, framework, atau detail server.
- Semua endpoint autentikasi wajib memiliki rate limiting.

---

# 🧪 Testing Requirements

QA harus menguji minimal skenario berikut.

- Input tidak valid.
- Login gagal.
- Hak akses ditolak.
- Duplicate resource.
- Database tidak tersedia.
- Integrasi WhatsApp gagal.
- Object Storage gagal.
- Unexpected exception.

---

# ✅ Review Checklist

- [ ] Error response mengikuti standar.
- [ ] Tidak ada informasi sensitif yang bocor.
- [ ] Logging lengkap.
- [ ] Error code konsisten.
- [ ] HTTP status sesuai standar.
- [ ] Retry hanya dilakukan untuk transient error.
- [ ] Pesan kepada pengguna mudah dipahami.

---

# 🔄 Traceability Matrix

| IDR Section | Related Requirement |
|-------------|---------------------|
| API Error Response | RHS-001 |
| Authentication Error | RHS-001 |
| Authorization Error | RHS-008 |
| Security Handling | RHS-009 |
| Notification Retry | RHS-011 |
| Audit Logging | RHS-012 |

---

# 🔗 References

## Product Documents

- [Product Requirements Document](../PRD.md)

## Requirement Hardening

- [RHS README](../RHS/README.md)
- [RHS-001 Authentication](../RHS/01-rhs-001-authentication.md)
- [RHS-008 RBAC](../RHS/08-rhs-008-rbac.md)
- [RHS-009 Security](../RHS/09-rhs-009-security-2fa.md)
- [RHS-011 Notification](../RHS/11-rhs-011-notification.md)
- [RHS-012 Audit Log](../RHS/12-rhs-012-audit-log.md)

## Implementation Details

- [Project Architecture](01-idr-001-project-architecture.md)
- [Technology Stack](02-idr-002-technology-stack.md)
- [Repository Structure](03-idr-003-repository-structure.md)
- [Development Workflow](04-idr-004-development-workflow.md)
- [Coding Standards](05-idr-005-coding-standards.md)

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-01 | Initial Error Handling Guidelines documentation |
