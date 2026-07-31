# 🚨 Global Error Handling Rules

> **Requirement Hardening Specification**
>
> **Reference:** PRD §8 – Functional Requirements, PRD §10 – Security, PRD §11 – Non-Functional Requirements
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini mendefinisikan standar global penanganan error (*Global Error Handling*) untuk seluruh Platform Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh komponen sistem memberikan respons yang konsisten, aman, mudah dipahami oleh pengguna, dan mudah ditelusuri oleh tim Engineering.

Standar ini berlaku untuk seluruh layanan backend, frontend, API, background jobs, dan integrasi eksternal.

---

# 🎯 Objective

- Menstandarkan format error di seluruh sistem.
- Menghindari kebocoran informasi sensitif.
- Mempermudah proses debugging.
- Memastikan pengalaman pengguna tetap konsisten ketika terjadi kegagalan.
- Menjadi acuan implementasi dan pengujian QA.

---

# 📋 Business Rules

| ID | Rule |
|----|------|
| ERR-01 | Seluruh endpoint harus menggunakan format error yang konsisten. |
| ERR-02 | Error tidak boleh menampilkan stack trace kepada pengguna. |
| ERR-03 | Error tidak boleh mengungkapkan informasi sensitif seperti password, token, secret, maupun query database. |
| ERR-04 | Seluruh unexpected error wajib dicatat pada application log. |
| ERR-05 | Error yang berasal dari layanan eksternal tidak boleh menyebabkan aplikasi berhenti beroperasi. |

---

# 📦 Standard Error Response

Seluruh API harus menggunakan struktur berikut.

```json
{
  "success": false,
  "error": {
    "code": "AUTH_INVALID_CREDENTIAL",
    "message": "Username atau password tidak valid."
  },
  "request_id": "b82f17d4",
  "timestamp": "2026-07-31T10:00:00Z"
}
```

---

# 🔢 Error Categories

| Category | Description |
|----------|-------------|
| Validation Error | Input tidak valid |
| Authentication Error | Login gagal |
| Authorization Error | Tidak memiliki hak akses |
| Business Rule Error | Melanggar aturan bisnis |
| Resource Error | Data tidak ditemukan |
| Conflict Error | Konflik data |
| External Service Error | Layanan eksternal gagal |
| System Error | Kesalahan internal sistem |

---

# 🌐 HTTP Status Code Mapping

| HTTP Status | Usage |
|------------|-------|
| 400 | Bad Request |
| 401 | Authentication Failed |
| 403 | Forbidden |
| 404 | Resource Not Found |
| 409 | Conflict |
| 422 | Validation Failed |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 502 | External Service Failure |
| 503 | Service Unavailable |

---

# 📋 Standard Error Codes

## Authentication

| Code | Description |
|------|-------------|
| AUTH_INVALID_CREDENTIAL | Username atau password salah |
| AUTH_ACCOUNT_DISABLED | Akun dinonaktifkan |
| AUTH_ACCOUNT_SUSPENDED | Akun ditangguhkan |
| AUTH_PASSWORD_EXPIRED | Password harus diganti |
| AUTH_ONBOARDING_REQUIRED | Onboarding belum selesai |

---

## Authorization

| Code | Description |
|------|-------------|
| PERMISSION_DENIED | Hak akses tidak mencukupi |
| ROLE_REQUIRED | Role tertentu diperlukan |

---

## Validation

| Code | Description |
|------|-------------|
| VALIDATION_FAILED | Validasi gagal |
| DUPLICATE_NIM | NIM sudah digunakan |
| INVALID_PASSWORD | Password tidak memenuhi aturan |
| INVALID_REQUEST | Request tidak valid |

---

## Resource

| Code | Description |
|------|-------------|
| RESOURCE_NOT_FOUND | Data tidak ditemukan |
| RESOURCE_ALREADY_EXISTS | Data sudah ada |

---

## External Integration

| Code | Description |
|------|-------------|
| WHATSAPP_UNAVAILABLE | WhatsApp API tidak tersedia |
| STORAGE_UNAVAILABLE | Object Storage tidak tersedia |
| GITHUB_UNAVAILABLE | GitHub API tidak tersedia |

---

## System

| Code | Description |
|------|-------------|
| INTERNAL_SERVER_ERROR | Kesalahan sistem |
| DATABASE_ERROR | Database gagal diakses |
| UNKNOWN_ERROR | Error tidak diketahui |

---

# 🔒 Security Rules

Seluruh error harus memenuhi aturan berikut.

- Tidak menampilkan stack trace.
- Tidak menampilkan SQL Query.
- Tidak menampilkan credential.
- Tidak menampilkan password hash.
- Tidak menampilkan access token.
- Tidak menampilkan refresh token.
- Tidak menampilkan TOTP secret.
- Tidak menampilkan backup code.

---

# 🔄 Retry Policy

| Scenario | Retry |
|----------|-------|
| Database Timeout | Ya |
| WhatsApp API Timeout | Ya |
| GitHub API Timeout | Ya |
| Storage Timeout | Ya |
| Validation Error | Tidak |
| Authentication Failed | Tidak |
| Authorization Failed | Tidak |

---

# ⚠️ Graceful Degradation

| Scenario | Expected Behavior |
|----------|-------------------|
| WhatsApp API Down | Informasi tetap tersimpan, notifikasi masuk antrean |
| GitHub API Down | Metadata repository terakhir tetap ditampilkan |
| Object Storage Down | Upload gagal dengan pesan yang jelas tanpa memengaruhi layanan lain |
| Background Job Gagal | Dicatat pada log dan dapat diproses ulang |
| Cache Gagal | Sistem mengambil data langsung dari database |

---

# 📊 Logging Requirements

Minimal informasi berikut harus dicatat.

- Timestamp
- Request ID
- User ID (jika tersedia)
- Endpoint
- HTTP Method
- Error Code
- Error Category
- Severity
- Stack Trace (internal saja)

---

# 🚦 Error Severity

| Severity | Description |
|----------|-------------|
| INFO | Informasi umum |
| WARNING | Kesalahan ringan |
| ERROR | Gangguan proses |
| CRITICAL | Gangguan yang memerlukan tindakan segera |

---

# 🧪 Error Validation

| Test | Expected Result |
|------|-----------------|
| Password salah | AUTH_INVALID_CREDENTIAL |
| NIM duplikat | DUPLICATE_NIM |
| Endpoint tidak ditemukan | HTTP 404 |
| Hak akses kurang | HTTP 403 |
| Database mati | HTTP 500 dengan pesan umum |
| WhatsApp gagal | Queue tetap berjalan |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | Input tidak valid | Request diproses | HTTP 422 dikembalikan |
| AC-02 | Password salah | Login | AUTH_INVALID_CREDENTIAL |
| AC-03 | User tanpa permission | Mengakses endpoint admin | HTTP 403 |
| AC-04 | Database gagal | API dipanggil | HTTP 500 tanpa informasi sensitif |
| AC-05 | WhatsApp API gagal | Notifikasi dikirim | Queue tetap berjalan |

---

# 🔗 Related Documents

- PRD §8 – Functional Requirements
- PRD §10 – Security
- PRD §11 – Non-Functional Requirements
- RHS-001 – Authentication & Onboarding
- RHS-008 – RBAC & Least Privilege
- RHS-009 – Security, 2FA & Password Reset
- RHS-011 – Notification & WhatsApp Resilience
- RHS-012 – Audit Log
- RHS-014 – Performance, Availability & Resilience

---

# 📝 Notes

- Seluruh endpoint baru wajib mengikuti standar error yang ditetapkan pada dokumen ini.
- Format respons error harus konsisten di seluruh layanan untuk memudahkan integrasi frontend dan pengujian QA.
- Pesan yang ditampilkan kepada pengguna harus bersifat informatif tanpa mengungkapkan detail implementasi internal.
- Setiap penambahan error code baru harus didokumentasikan agar tetap konsisten dengan standar platform.
