# IDR-009: Authentication Architecture

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD §8.1 Authentication & Account, RHS-001 Authentication & Onboarding, RHS-008 RBAC, RHS-009 Security
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical (Launch Blocking)

---

# 📖 Overview

Dokumen ini mendefinisikan arsitektur autentikasi yang digunakan pada Platform Digital Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh proses autentikasi dirancang secara aman, konsisten, mudah dipelihara, dan mampu mendukung pengembangan fitur di masa depan tanpa mengubah fondasi keamanan sistem.

Dokumen ini hanya menjelaskan keputusan arsitektur autentikasi. Detail implementasi endpoint API dijelaskan pada **API Documentation**, sedangkan detail skema database dijelaskan pada **Database Design Specification (DDS)**.

---

# 🎯 Objectives

- Menyediakan mekanisme autentikasi yang aman.
- Mendukung Role-Based Access Control (RBAC).
- Memastikan kredensial pengguna terlindungi.
- Mendukung audit terhadap aktivitas autentikasi.
- Menyediakan fondasi untuk pengembangan fitur keamanan lanjutan.

---

# 📦 Scope

Dokumen ini mencakup:

- Login
- Logout
- Session Management
- Access Token
- Refresh Token
- Password Hashing
- Password Reset
- First Login Onboarding
- Two-Factor Authentication (Administrator)
- Authorization Integration

---

# 🏗 Authentication Architecture

Platform menggunakan arsitektur **Token-Based Authentication**.

Komponen utama:

- Frontend Client
- Backend API
- Authentication Service
- PostgreSQL Database
- Audit Log Service

Seluruh proses autentikasi dilakukan melalui Backend API.

Frontend tidak menyimpan informasi sensitif selain token yang memang diperlukan.

---

# 🔑 Authentication Flow

Alur autentikasi:

1. Pengguna memasukkan NIM dan password.
2. Backend melakukan validasi kredensial.
3. Password diverifikasi menggunakan hash.
4. Sistem memeriksa status akun.
5. Jika login pertama, pengguna diarahkan ke proses onboarding.
6. Jika berhasil, sistem menerbitkan Access Token dan Refresh Token.
7. Aktivitas login dicatat pada Audit Log.

---

# 👤 User Identifier

Identitas utama pengguna:

| Item | Value |
|------|-------|
| Username | NIM |
| Primary Key | UUID |
| Display Name | Nama Mahasiswa |

NIM digunakan sebagai username, sedangkan relasi database menggunakan UUID.

---

# 🔒 Password Policy

Password harus memenuhi ketentuan berikut:

- Minimum 12 karakter.
- Tidak boleh menggunakan NIM.
- Tidak boleh menggunakan nama lengkap.
- Tidak boleh menggunakan tanggal lahir.
- Tidak boleh sama dengan password sebelumnya (jika riwayat password diterapkan).

Password hanya disimpan dalam bentuk hash.

---

# 🔐 Password Hashing

Algoritma yang direkomendasikan:

| Priority | Algorithm |
|----------|-----------|
| Preferred | Argon2id |
| Alternative | BCrypt |

Password plaintext tidak boleh disimpan maupun dikirim kembali ke client.

---

# 🎫 Token Strategy

Platform menggunakan dua jenis token.

| Token | Purpose |
|--------|----------|
| Access Token | Mengakses API |
| Refresh Token | Memperbarui Access Token |

Karakteristik:

- Access Token berumur pendek.
- Refresh Token memiliki masa berlaku lebih panjang.
- Refresh Token dapat dicabut apabila akun logout atau dinonaktifkan.

---

# ⏳ Session Management

Setiap login menghasilkan session baru.

Session harus dapat:

- dicabut,
- kedaluwarsa,
- diaudit.

Logout mengakhiri session aktif dan mencabut Refresh Token.

---

# 🚪 First Login Onboarding

Pada login pertama:

1. Sistem mendeteksi akun belum menyelesaikan onboarding.
2. Pengguna diwajibkan mengganti password.
3. Pengguna melengkapi data profil wajib.
4. Setelah onboarding selesai, akun berubah menjadi aktif penuh.

Selama proses onboarding, akses ke fitur internal dibatasi.

---

# 🔐 Two-Factor Authentication

TOTP wajib diterapkan untuk:

- Superadmin
- Administrator

Mahasiswa belum diwajibkan menggunakan 2FA pada MVP.

Backup codes harus tersedia saat aktivasi TOTP.

---

# 🛡 Authorization Integration

Autentikasi hanya memastikan identitas pengguna.

Hak akses ditentukan melalui RBAC sebagaimana dijelaskan pada RHS-008.

Backend menjadi sumber otoritatif dalam proses authorization.

---

# 🚨 Account Status Validation

Sebelum menerbitkan token, sistem wajib memeriksa status akun.

Status yang dikenali:

- Invited
- Active
- Suspended
- Alumni
- Deactivated

Akun selain **Active** hanya dapat mengakses fitur sesuai kebijakan.

---

# 📊 Audit Requirements

Aktivitas berikut wajib dicatat:

- Login berhasil
- Login gagal
- Logout
- Password berubah
- Password reset
- Aktivasi akun
- Suspended
- Reactivated
- Deactivated
- Aktivasi atau penonaktifan 2FA

---

# 🔒 Security Principles

Seluruh implementasi harus mengikuti prinsip:

- Security by Default
- Least Privilege
- Defense in Depth
- Zero Trust
- Fail Secure

---

# 🚫 Security Restrictions

Tidak diperbolehkan:

- Menyimpan password plaintext.
- Mengirim password melalui email atau API response.
- Menampilkan password hash.
- Menyimpan credential pada source code.
- Menonaktifkan validasi token pada endpoint privat.

---

# 🧪 Validation Checklist

- [ ] Password menggunakan hash.
- [ ] Access Token memiliki masa berlaku terbatas.
- [ ] Refresh Token dapat dicabut.
- [ ] Login pertama memicu onboarding.
- [ ] RBAC diterapkan pada backend.
- [ ] Aktivitas autentikasi tercatat pada Audit Log.
- [ ] 2FA aktif untuk akun administratif.
- [ ] Session dapat diakhiri melalui logout.

---

# 🔄 Traceability Matrix

| IDR Section | Related Requirement |
|-------------|---------------------|
| Login | RHS-001 |
| Password Policy | RHS-001 |
| RBAC Integration | RHS-008 |
| 2FA | RHS-009 |
| Audit | RHS-012 |
| Account Lifecycle | RHS-007 |

---

# 🔗 References

## Product Documents

- [PRD](../PRD.md)

## Requirement Hardening

- [RHS README](../RHS/README.md)
- [RHS-001 Authentication](../RHS/01-rhs-001-authentication.md)
- [RHS-007 Account Lifecycle](../RHS/07-rhs-007-account-lifecycle.md)
- [RHS-008 RBAC](../RHS/08-rhs-008-rbac.md)
- [RHS-009 Security](../RHS/09-rhs-009-security-2fa.md)
- [RHS-012 Audit Log](../RHS/12-rhs-012-audit-log.md)

## Previous IDR

- [IDR-002 Technology Stack](02-idr-002-technology-stack.md)
- [IDR-007 API Design Guidelines](07-idr-007-api-design-guidelines.md)
- [IDR-008 Database Design Guidelines](08-idr-008-database-design-guidelines.md)

## Future Documents

- `docs/DDS/README.md`
- `docs/api/README.md`

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-02 | Initial Authentication Architecture documentation |
