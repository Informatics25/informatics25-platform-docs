# 📜 RHS-012: Audit Log

> **Requirement Hardening Specification**
>
> **Reference:** PRD §12 – Data, Audit Log, Backup & Recovery
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical (Launch Blocking)

---

# 📖 Overview

Dokumen ini mendefinisikan aturan implementasi Audit Log pada Platform Informatika Angkatan 2025.

Audit Log berfungsi sebagai sumber pencatatan seluruh aktivitas penting yang dilakukan oleh pengguna maupun sistem. Audit Log digunakan untuk mendukung keamanan, akuntabilitas, investigasi insiden, troubleshooting, serta memenuhi kebutuhan operasional platform.

Audit Log **bukan** fitur yang dapat dimodifikasi oleh pengguna biasa. Seluruh data audit harus bersifat immutable (tidak dapat diubah) dan hanya dapat diakses oleh role yang memiliki kewenangan.

---

# 🎯 Objectives

- Menyediakan jejak aktivitas (audit trail) yang lengkap.
- Mendukung investigasi insiden keamanan.
- Mendukung proses troubleshooting operasional.
- Memenuhi prinsip Accountability dan Traceability.
- Menjamin seluruh aktivitas penting dapat ditelusuri.

---

# 📋 Business Rules

| ID | Rule |
|----|------|
| AUD-01 | Setiap aktivitas penting wajib menghasilkan Audit Log. |
| AUD-02 | Audit Log harus dibuat secara otomatis oleh sistem. |
| AUD-03 | Audit Log tidak boleh dapat diubah oleh pengguna biasa. |
| AUD-04 | Audit Log tidak boleh dapat dihapus oleh Administrator. |
| AUD-05 | Audit Log hanya dapat dibaca oleh role yang memiliki hak akses. |
| AUD-06 | Audit Log harus memiliki timestamp dalam UTC. |
| AUD-07 | Audit Log harus tetap tersedia walaupun proses bisnis utama gagal setelah log berhasil dicatat. |
| AUD-08 | Audit Log harus memiliki identitas actor yang jelas atau ditandai sebagai System. |
| AUD-09 | Audit Log harus dapat digunakan untuk proses investigasi. |
| AUD-10 | Audit Log keamanan disimpan minimal 1 tahun sesuai PRD. |

---

# 📚 Audit Event Categories

## Authentication

| Event | Trigger |
|---------|---------|
| Login berhasil | Pengguna berhasil melakukan autentikasi |
| Login gagal | Kredensial tidak valid |
| Logout | Pengguna keluar dari sistem |
| Password diubah | Password berhasil diperbarui |
| Password direset | Superadmin melakukan reset password |
| First Login | Pengguna pertama kali berhasil login |
| Onboarding selesai | Seluruh proses onboarding berhasil diselesaikan |

---

## Account

| Event | Trigger |
|---------|---------|
| Account Created | Superadmin membuat akun |
| Account Activated | Status akun menjadi Active |
| Account Suspended | Status akun menjadi Suspended |
| Account Reactivated | Suspended menjadi Active |
| Account Deactivated | Akun dinonaktifkan |
| Account Deleted | Akun dihapus sesuai kebijakan |
| Role Changed | Role pengguna berubah |

---

## Official Information

| Event | Trigger |
|---------|---------|
| Information Created | Draft dibuat |
| Information Published | Informasi dipublikasikan |
| Information Updated | Informasi diperbarui |
| Information Archived | Informasi diarsipkan |
| Information Deleted | Informasi dihapus |

---

## Schedule

| Event | Trigger |
|---------|---------|
| Schedule Created | Jadwal baru dibuat |
| Schedule Updated | Jadwal diperbarui |
| Schedule Exception Created | Exception dibuat |
| Schedule Cancelled | Jadwal dibatalkan |
| Replacement Class Created | Kuliah pengganti dibuat |

---

## Knowledge Hub

| Event | Trigger |
|---------|---------|
| Resource Uploaded | Mahasiswa mengunggah resource |
| Resource Approved | Reviewer menyetujui resource |
| Resource Rejected | Reviewer menolak resource |
| Resource Published | Resource dipublikasikan |
| Resource Deleted | Resource dihapus |

---

## Notification

| Event | Trigger |
|---------|---------|
| Notification Created | Notification dibuat |
| Notification Sent | Notification berhasil dikirim |
| Notification Failed | Pengiriman gagal |
| Notification Retried | Retry dijalankan |

---

## Governance

| Event | Trigger |
|---------|---------|
| Configuration Changed | Konfigurasi sistem berubah |
| Permission Changed | Permission diubah |
| Policy Updated | Kebijakan diperbarui |

---

# 📊 Audit Fields

Setiap Audit Log minimal memiliki atribut berikut.

| Field | Required |
|---------|----------|
| Audit ID | ✅ |
| Timestamp (UTC) | ✅ |
| Actor ID | ✅ |
| Actor Role | ✅ |
| Event Type | ✅ |
| Action | ✅ |
| Target Resource | ✅ |
| Resource ID | ✅ |
| IP Address | ✅ |
| User Agent | ✅ |
| Status | ✅ |

---

# ✅ Validation Rules

| ID | Rule |
|----|------|
| VAL-01 | Audit ID harus unik. |
| VAL-02 | Timestamp wajib menggunakan UTC. |
| VAL-03 | Event Type harus berasal dari daftar yang telah ditentukan. |
| VAL-04 | Actor wajib diketahui atau bernilai `System`. |
| VAL-05 | Audit tidak boleh kehilangan Resource ID apabila tersedia. |

---

# 🔐 Permission Rules

| Role | Permission |
|------|------------|
| Guest | Tidak memiliki akses |
| Student | Tidak memiliki akses |
| Administrator | Read Only (sesuai ruang lingkup operasional) |
| Superadmin | Full Read |

---

# 🔄 Audit Flow

```text
Business Event
      │
      ▼
Audit Service
      │
      ▼
Validation
      │
      ▼
Audit Storage
      │
      ▼
Monitoring / Investigation
```

---

# 📦 Data Retention Policy

| Jenis Audit | Retention |
|--------------|-----------|
| Authentication | Minimal 1 Tahun |
| Security | Minimal 1 Tahun |
| Official Information | Sesuai kebutuhan operasional |
| Schedule | Sesuai kebutuhan operasional |
| Knowledge Hub | Sesuai kebutuhan operasional |
| Notification | Minimal 180 Hari |

---

# ⚠ Edge Cases

| Scenario | Expected Behavior |
|-----------|------------------|
| Actor telah dihapus | Actor tetap direferensikan menggunakan ID historis |
| Resource telah dihapus | Audit tetap disimpan |
| Database gagal setelah audit dibuat | Audit tetap dipertahankan jika memungkinkan |
| Audit gagal ditulis | Sistem mencatat error internal |
| Duplicate Event | Audit tetap menggunakan Event ID unik |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | User login | Login berhasil | Audit Login dibuat |
| AC-02 | Password diubah | Password berhasil disimpan | Audit Password Changed dibuat |
| AC-03 | Informasi dipublikasikan | Publish berhasil | Audit Publish dibuat |
| AC-04 | Resource disetujui | Approval berhasil | Audit Approval dibuat |
| AC-05 | Role diubah | Perubahan disimpan | Audit Role Changed dibuat |

---

# 🔒 Security Requirements

- Audit Log bersifat immutable.
- Audit Log tidak boleh dapat dimodifikasi melalui aplikasi.
- Audit Log tidak boleh dihapus oleh Administrator.
- Seluruh akses terhadap Audit Log juga harus menghasilkan Audit Log.
- Audit Storage harus memiliki mekanisme backup.
- Audit tidak boleh menyimpan password, token, OTP, maupun credential.

---

# 📊 Data Model Reference

```typescript
interface AuditLog {
    id: string;
    actorId: string | null;
    actorRole: string;
    eventType: string;
    action: string;
    resourceType: string;
    resourceId: string | null;
    status: "success" | "failed";
    ipAddress: string;
    userAgent: string;
    createdAt: Date;
}
```

---

# 🌐 API Reference

## Get Audit Logs

```http
GET /api/v1/admin/audit-log
```

---

## Get Audit Detail

```http
GET /api/v1/admin/audit-log/{id}
```

---

## Filter Audit

```http
GET /api/v1/admin/audit-log?actor=uuid&event=login
```

---

# 🔗 Requirement Traceability Matrix (RTM)

| RHS ID | PRD Reference |
|---------|---------------|
| AUD-01 | PRD §12.1 |
| AUD-02 | PRD §12.1 |
| AUD-03 | PRD §12.1 |
| AUD-04 | PRD §12.1 |
| AUD-05 | PRD §10 |
| AUD-06 | PRD §12 |
| AUD-07 | PRD §12 |
| AUD-08 | PRD §12 |
| AUD-09 | PRD §12 |
| AUD-10 | PRD §12 |

---

# 🔗 Related Documents

- PRD §10 – Authentication, Authorization, Privacy & Security
- PRD §12 – Audit Log
- RHS-001 – Authentication & Onboarding
- RHS-002 – Official Information
- RHS-004 – Schedule
- RHS-005 – Knowledge Hub
- RHS-008 – RBAC
- RHS-009 – Security
- RHS-011 – Notification

---

# 📝 Notes

- Audit Log merupakan komponen **Launch Blocking**.
- Audit Log menjadi dasar untuk proses monitoring, troubleshooting, keamanan, dan investigasi insiden.
- Seluruh implementasi harus mengikuti prinsip **Immutable**, **Traceability**, **Least Privilege**, dan **Accountability**.
