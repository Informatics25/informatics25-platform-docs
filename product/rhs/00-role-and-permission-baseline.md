# 🔐 Role and Permission Baseline

## 📌 Overview

Dokumen ini mendefinisikan role dan permission baseline yang menjadi fondasi implementasi RHS. Role dan account status adalah dua konsep terpisah - status akun dapat membatasi akses tanpa mengubah role.

---

## 👤 Role Definitions

| Role | Deskripsi | Core Access |
|------|-----------|-------------|
| **Guest** | Pengunjung tidak login | Landing page dan konten publik yang disetujui |
| **Mahasiswa** | Mahasiswa Informatika 2025 terdaftar | Dashboard, informasi internal, jadwal, Knowledge Hub, upload resource/gallery |
| **Administrator** | Pengelola operasional harian | Operasional harian: pengumuman, jadwal, approval resource/gallery, notifikasi |
| **Superadmin** | Pemegang kendali penuh sistem | Kendali penuh sistem, akun, konfigurasi, policy, aset digital, dan governance |

---

## 🔑 Account Status

Account status adalah konsep terpisah dari role. Status dapat membatasi akses tanpa mengubah role.

| Status | Deskripsi | Dampak Akses |
|--------|-----------|--------------|
| **INVITED** | Akun dibuat, belum diaktivasi | Hanya dapat mengakses flow onboarding |
| **ACTIVE** | Akun aktif dan dapat digunakan | Akses penuh sesuai role |
| **SUSPENDED** | Akun ditangguhkan sementara | Akses dibatasi |
| **DEACTIVATED** | Akun dinonaktifkan | Tidak dapat login |
| **ALUMNI** | Mahasiswa telah lulus | Akses terbatas sesuai policy alumni |

---

## 🛡️ Security Principles

1. **Deny by default** - Permission tidak eksplisit berarti ditolak
2. **Least privilege** - Setiap role hanya memiliki akses minimum yang diperlukan
3. **Server-side enforcement** - Authorization ditegakkan di backend
4. **Separation of duties** - Role administratif dipisahkan berdasarkan tanggung jawab
5. **Individual accountability** - Akun administratif bersifat individual, bukan shared

---

## 📋 Permission Matrix Summary

| Capability | Guest | Mahasiswa | Administrator | Superadmin |
|------------|-------|-----------|---------------|------------|
| Landing page publik | ✅ Read | ✅ Read | ✅ Read | ✅ Read |
| Internal dashboard | ❌ | ✅ Read | ✅ Read | ✅ Read |
| Create announcement | ❌ | ❌ | ✅ | ✅ |
| Publish announcement | ❌ | ❌ | ✅* | ✅ |
| Manage schedule | ❌ | ❌ | ✅ | ✅ |
| Approve resource/gallery | ❌ | ❌ | ✅ | ✅ |
| Manage user accounts | ❌ | ❌ | Limited | Full |
| Change system configuration | ❌ | ❌ | ❌ | ✅ |
| Manage digital assets | ❌ | ❌ | ❌ | ✅ |

> *Administrator permission mengikuti policy operasional final yang ditetapkan dalam PRD/RHS.

---

## 🔒 Sensitive Data Access

| Data Type | Mahasiswa | Administrator | Superadmin |
|-----------|-----------|---------------|------------|
| Password hash | ❌ | ❌ | ❌ |
| TOTP Secret | ❌ | ❌ | ❌ |
| Backup Codes | ❌ | ❌ | ❌ |
| Full NIM (other users) | ❌ | ❌ | ✅ |
| Audit Log | ❌ | Limited | Full |
| Personal Data (other users) | ❌ | Limited* | Full |

> *Administrator hanya mengakses data pribadi yang diperlukan untuk tugas operasional.

---

## 📝 Implementation Notes

### Backend
- Authorization harus ditegakkan server-side
- Sensitive fields tidak boleh dikirim ke client jika tidak dibutuhkan
- Role dan permission check pada setiap endpoint

### Frontend
- UI hanya merefleksikan permission yang ada
- Tidak boleh mengandalkan client-side authorization saja
- Redirect ke halaman unauthorized jika permission tidak cukup

### Database
- Role dan status disimpan sebagai enum
- Permission dievaluasi berdasarkan role saat request diproses
- Audit log mencatat semua perubahan role dan status
