# 🔔 RHS-011: Notification & WhatsApp Resilience

> **Requirement Hardening Specification**
>
> **Reference:** PRD §13.1 – WhatsApp Integration, PRD §8.2 Dashboard, PRD §8.3 Official Information, PRD §8.4 Schedule
>
> **Status:** ✅ Approved
>
> **Priority:** 🟡 High (Non Launch Blocking)

---

# 📖 Overview

Dokumen ini mendefinisikan aturan implementasi sistem notifikasi pada Platform Informatika Angkatan 2025.

Notification merupakan mekanisme distribusi informasi, **bukan sumber informasi utama**. Seluruh informasi resmi tetap berada di dalam platform (Single Source of Truth), sedangkan notifikasi berfungsi sebagai media pemberitahuan agar pengguna mengetahui adanya perubahan atau informasi penting.

Seluruh implementasi harus tetap menjamin bahwa kegagalan sistem notifikasi tidak mempengaruhi integritas data maupun akses terhadap informasi resmi.

---

# 🎯 Objectives

- Memberikan pemberitahuan terhadap informasi penting.
- Mengurangi risiko mahasiswa terlambat mengetahui perubahan.
- Menjamin informasi resmi tetap berada di platform.
- Memastikan kegagalan pengiriman tidak menyebabkan kehilangan data.
- Mendukung retry dan monitoring pengiriman notifikasi.

---

# 📋 Business Rules

| ID | Rule |
|----|------|
| NOTIF-01 | Notification hanya berfungsi sebagai media pemberitahuan, bukan sumber informasi utama. |
| NOTIF-02 | Seluruh informasi resmi harus tetap tersedia di dalam platform. |
| NOTIF-03 | WhatsApp bukan dependency untuk peluncuran MVP. |
| NOTIF-04 | Platform tetap dapat beroperasi walaupun layanan WhatsApp tidak tersedia. |
| NOTIF-05 | Notification hanya dikirim untuk event yang memenuhi aturan bisnis. |
| NOTIF-06 | Pengiriman notification tidak boleh mengubah data utama sistem. |
| NOTIF-07 | Seluruh proses pengiriman harus dapat diaudit. |
| NOTIF-08 | Notification dapat dikirim ulang (retry) apabila gagal. |
| NOTIF-09 | Duplicate notification harus dicegah apabila request dikirim lebih dari satu kali. |
| NOTIF-10 | Pengguna dapat menerima lebih dari satu jenis notification sesuai preferensi sistem. |

---

# ✅ Notification Events

## Official Information

| Event | Trigger |
|---------|---------|
| Informasi baru | Official Information dipublikasikan |
| Informasi diperbarui | Official Information direvisi |
| Informasi prioritas tinggi | Priority = Critical |

---

## Schedule

| Event | Trigger |
|---------|---------|
| Perubahan jadwal | Schedule Exception dibuat atau diperbarui |
| Perubahan ruangan | Room pada Schedule Exception berubah |
| Pembatalan kelas | Status Schedule Exception menjadi `Cancelled` |
| Kuliah pengganti | Schedule Exception bertipe `Replacement Class` dibuat |


---

## Knowledge Hub

| Event | Trigger |
|---------|---------|
| Resource diunggah | Mahasiswa berhasil mengunggah resource baru |
| Resource masuk review | Resource berhasil disimpan dengan status `Pending Review` |
| Resource disetujui | Reviewer mengubah status menjadi `Approved` |
| Resource ditolak | Reviewer mengubah status menjadi `Rejected` |
| Resource dipublikasikan | Resource `Approved` dipublikasikan |
| Resource diperbarui | Metadata atau file resource diperbarui |
| Resource dihapus | Resource dihapus oleh Administrator atau Superadmin |

---

## Account

| Event | Trigger |
|---------|---------|
| Password direset | Superadmin melakukan reset password |
| Akun diaktifkan | Status akun berubah menjadi `Active` |
| Akun dinonaktifkan | Status akun berubah menjadi `Suspended` atau `Deactivated` |

---

# ✅ Validation Rules

| ID | Rule |
|----|------|
| VAL-01 | Notification hanya boleh dikirim untuk event yang valid. |
| VAL-02 | Duplicate notification harus dicegah menggunakan event identifier. |
| VAL-03 | Retry tidak boleh menghasilkan pengiriman ganda. |
| VAL-04 | Notification harus memiliki timestamp. |
| VAL-05 | Status pengiriman wajib disimpan. |
| VAL-06 | Payload notification harus tervalidasi sebelum dikirim. |

---

# 🔐 Permission Rules

| Role | Permission |
|------|------------|
| Guest | Tidak menerima notification internal |
| Student | Menerima notification sesuai hak akses |
| Administrator | Mengirim notification operasional |
| Superadmin | Konfigurasi notification dan monitoring |

---

# 🔄 Notification Lifecycle

```text
Created
   │
   ▼
Queued
   │
   ▼
Sending
   │
   ├──────────────► Failed
   │                    │
   │                    ▼
   │                 Retry Queue
   │                    │
   ▼                    │
Delivered ◄─────────────┘
   │
   ▼
Archived
```

---

# 📊 Notification Status

| Status | Description |
|---------|-------------|
| Created | Notification dibuat |
| Queued | Masuk antrean pengiriman |
| Sending | Sedang dikirim |
| Delivered | Berhasil diterima provider |
| Failed | Pengiriman gagal |
| Retrying | Menunggu retry |
| Archived | Riwayat notification |

---

# ⚠ Edge Cases

| Scenario | Expected Behavior |
|-----------|------------------|
| WhatsApp API down | Notification masuk retry queue |
| Timeout | Retry sesuai kebijakan |
| Duplicate request | Hanya satu notification dikirim |
| User tidak memiliki nomor | Notification dilewati tanpa mempengaruhi proses utama |
| Queue penuh | Sistem tetap menyimpan notification |
| Provider mengembalikan error | Error dicatat pada audit log |
| Notification terlambat | Informasi tetap tersedia pada dashboard |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | Official Information dipublikasikan | Notification dibuat | Notification masuk queue |
| AC-02 | WhatsApp gagal | Retry dijalankan | Notification dikirim ulang |
| AC-03 | Retry berhasil | Delivery selesai | Status menjadi Delivered |
| AC-04 | Retry melebihi batas | Retry dihentikan | Status menjadi Failed |
| AC-05 | Dashboard dibuka | Notification gagal | Informasi tetap tersedia |

---

# 🔒 Security Requirements

- Notification tidak boleh memuat informasi sensitif.
- Credential provider tidak boleh muncul pada log.
- API Key harus disimpan menggunakan Secret Manager.
- Endpoint webhook harus tervalidasi.
- Signature webhook harus diverifikasi.
- Queue harus terlindungi dari duplicate processing.
- Payload notification harus disanitasi.

---

# 📝 Audit Requirements

Sistem wajib mencatat:

- Notification Created
- Notification Queued
- Notification Sent
- Notification Delivered
- Notification Failed
- Retry Executed
- Retry Exhausted
- Notification Cancelled

Audit minimal mencatat:

- Event ID
- Actor/System
- Timestamp
- Channel
- Target User
- Delivery Status
- Retry Count
- Error Message (jika ada)

---

# 📊 Data Model Reference

```typescript
interface Notification {
    id: string;
    userId: string;
    channel: "whatsapp" | "in_app";
    eventType: string;
    title: string;
    message: string;
    status:
        | "created"
        | "queued"
        | "sending"
        | "delivered"
        | "failed"
        | "retrying"
        | "archived";
    retryCount: number;
    createdAt: Date;
    deliveredAt?: Date;
}
```

---

# 🔌 Queue Flow

```text
Business Event
      │
      ▼
Notification Service
      │
      ▼
Queue
      │
      ▼
Worker
      │
      ▼
WhatsApp Provider
      │
      ├────────► Success
      │
      └────────► Retry Queue
```

---

# 🔗 Requirement Traceability Matrix (RTM)

| RHS ID | PRD Reference |
|---------|---------------|
| NOTIF-01 | PRD §13.1 |
| NOTIF-02 | PRD §13.1 |
| NOTIF-03 | PRD §13.1 |
| NOTIF-04 | PRD §13.1 |
| NOTIF-05 | PRD §8.3 |
| NOTIF-06 | PRD §8.3 |
| NOTIF-07 | PRD §12 |
| NOTIF-08 | PRD §13.1 |
| NOTIF-09 | PRD §13.1 |
| NOTIF-10 | PRD §15 |

---

# 🔗 Related Documents

- PRD §8.2 – Dashboard
- PRD §8.3 – Official Information
- PRD §8.4 – Schedule
- PRD §12 – Audit Log
- PRD §13.1 – WhatsApp Integration
- RHS-002 – Official Information
- RHS-003 – Dashboard
- RHS-004 – Schedule
- RHS-005 – Knowledge Hub
- RHS-012 – Audit Log

---

# 📝 Notes

- Notification bukan sumber informasi utama.
- Dashboard tetap menjadi pusat informasi resmi.
- WhatsApp hanyalah media distribusi.
- Sistem harus tetap berjalan tanpa layanan WhatsApp.
- Retry mechanism harus bersifat idempotent.
- Seluruh aktivitas notification wajib dapat diaudit.
