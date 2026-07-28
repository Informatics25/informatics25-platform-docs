# 🛡️ RHS-001: Authentication and Onboarding

**Tujuan:** Memastikan akun mahasiswa dapat diaktifkan dengan aman dan onboarding memiliki hasil yang terukur.

---

## 📋 Business Rules

| ID | Rule |
|----|------|
| AUTH-01 | Akun mahasiswa dibuat oleh Superadmin menggunakan NIM sebagai username/identifier |
| AUTH-02 | Password awal berupa temporary credential acak yang aman dan tidak menggunakan NIM atau data pribadi |
| AUTH-03 | Pada login pertama, mahasiswa wajib mengganti password |
| AUTH-04 | Mahasiswa belum dianggap selesai onboarding sampai password diganti, profil wajib dilengkapi, dan dashboard berhasil diakses |
| AUTH-05 | Akun yang belum selesai onboarding tidak boleh mengakses fitur internal normal selain flow onboarding yang diperlukan |

---

## ✅ Validation Rules

| ID | Rule |
|----|------|
| VAL-01 | NIM wajib unik |
| VAL-02 | Password baru minimum 12 karakter |
| VAL-03 | Password baru tidak boleh sama dengan temporary password |
| VAL-04 | Password tidak boleh menggunakan nilai yang mudah ditebak (NIM, nama, tanggal lahir) |
| VAL-05 | Data wajib profil: nama, NIM, angkatan, asal daerah |

---

## 🔐 Permission Rules

| ID | Rule |
|----|------|
| PER-01 | Mahasiswa hanya dapat menyelesaikan onboarding untuk akun sendiri |
| PER-02 | Superadmin dapat membuat dan mengelola akun |
| PER-03 | Administrator tidak dapat melihat password, password hash, TOTP secret, atau backup codes |

---


# 🔄 State Transition

```mermaid
stateDiagram-v2

    [*] --> Invited : Account Created

    Invited --> Active : Complete Onboarding

    Active --> Suspended : Suspend Account
    Active --> Alumni : Graduate
    Active --> Deactivated : Delete Account

    Suspended --> Active : Reactivate
    Suspended --> Alumni : Graduation Policy
    Suspended --> Deactivated : Delete Account

    Alumni --> Deactivated : Archive / Delete
```

---

# 📊 State Transition Summary

| State | Allowed Transition |
|------|--------------------|
| **Invited** | → Active |
| **Active** | → Suspended, Alumni, Deactivated |
| **Suspended** | → Active, Alumni, Deactivated |
| **Alumni** | → Deactivated |

---

# ⚠️ Edge Cases & Error Handling

| Scenario | Expected Behavior |
|----------|-------------------|
| Login menggunakan temporary password setelah password berhasil diganti | Ditolak |
| Password change gagal | Akun tetap berada pada status onboarding sebelumnya |
| NIM sudah digunakan | Pembuatan akun dibatalkan |
| Request onboarding dikirim ulang | Sistem harus idempotent |
| Temporary password kedaluwarsa | Superadmin melakukan reset password |
| Browser ditutup saat onboarding | Progress tetap tersimpan sesuai kebijakan sistem |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | Akun baru | Mahasiswa login pertama kali | Sistem memaksa perubahan password |
| AC-02 | Password baru valid | Password disimpan | Temporary password tidak dapat digunakan kembali |
| AC-03 | Profil wajib belum lengkap | Mahasiswa membuka dashboard | Sistem mengarahkan ke onboarding |
| AC-04 | Onboarding selesai | Mahasiswa login kembali | Dashboard dapat diakses |
| AC-05 | Password tidak memenuhi kebijakan | Mahasiswa menyimpan password | Sistem menolak dan menampilkan validasi |

---

# 🔒 Security Requirements

| Requirement | Implementation |
|------------|----------------|
| Password Storage | Argon2id atau BCrypt |
| Password Transmission | HTTPS Only |
| Password Hash | Tidak pernah dikirim kembali ke client |
| Credential Logging | Dilarang muncul pada log |
| Session | HttpOnly Cookie atau Secure Token |
| Rate Limiting | Wajib pada endpoint login |
| Brute Force Protection | Account Lockout / Delay |
| CSRF Protection | Wajib untuk session-based authentication |

---

# 📝 Audit Requirements

Sistem harus mencatat minimal aktivitas berikut:

- Account Created
- First Login
- Password Changed
- Password Reset
- Account Activated
- Account Suspended
- Account Reactivated
- Account Deactivated

Audit log minimal berisi:

- Timestamp
- User ID
- Actor
- Action
- IP Address (opsional sesuai kebijakan)
- Device Information (opsional)

---

# 🌐 API Contract Reference

## Login

```http
POST /api/v1/auth/login
```

```json
{
  "nim": "20251337037",
  "password": "temporary_password"
}
```

---

## Complete Onboarding

```http
POST /api/v1/auth/onboarding
```

```json
{
  "new_password": "Kaizer_Anti_Error86!",
  "profile": {
    "name": "Abidzar Dzakwan",
    "origin": "Surabaya"
  }
}
```

---

## Reset Password (Superadmin)

```http
POST /api/v1/auth/reset-password
```

```json
{
  "user_id": "uuid",
  "reason": "User requested reset"
}
```

---

# 📚 Related Documents

- PRD §8.1 Authentication & Account
- RHS-009 — Authorization & RBAC
- RHS-010 — Security & Password Policy
- RHS-011 — Audit Log
- API Specification (Authentication)
- Database Schema (Users)

---

# 📌 Notes

- Authentication merupakan **Launch Blocking Feature**.
- Onboarding wajib selesai sebelum mahasiswa memperoleh akses ke fitur internal.
- Seluruh implementasi harus mengikuti kebijakan keamanan platform dan prinsip **least privilege**.
