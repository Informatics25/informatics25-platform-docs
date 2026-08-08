# AHS-006: Audit & Observability

> **Architecture Hardening Specification (AHS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 High

---

# 📖 Overview

Dokumen ini mendefinisikan prinsip **Audit & Observability** untuk Platform Digital Informatika Angkatan 2025.

Audit dan observability digunakan untuk memastikan aktivitas penting dalam sistem dapat ditelusuri dan kondisi operasional sistem dapat diketahui oleh pihak yang bertanggung jawab.

Dokumen ini mencakup dua kebutuhan yang saling berkaitan tetapi memiliki tujuan berbeda:

* **Audit** berfokus pada keterlacakan aktivitas dan perubahan penting.
* **Observability** berfokus pada kondisi dan perilaku sistem secara operasional.

Keduanya menjadi bagian dari architecture baseline sistem.

---

# 🎯 Objectives

Dokumen ini bertujuan untuk:

* Memastikan aktivitas penting dapat ditelusuri.
* Menyediakan audit trail untuk perubahan kritis.
* Membantu identifikasi masalah sistem.
* Menyediakan informasi yang dibutuhkan untuk monitoring.
* Mendukung proses investigasi incident.
* Membantu evaluasi kesehatan sistem.
* Menjaga agar aktivitas penting tidak menjadi *untraceable operation*.

---

# 📦 Scope

Dokumen ini mencakup:

* Audit Logging
* Application Logging
* Monitoring
* Observability
* Error Tracking
* Security Events
* Operational Events
* Log Retention
* Log Access
* Incident Investigation

---

# 📝 Audit Logging

Audit log digunakan untuk mencatat aktivitas penting yang memengaruhi data, akses, konfigurasi, atau keamanan sistem.

Contoh aktivitas yang dapat memerlukan audit:

* Login.
* Logout.
* Perubahan credential.
* Perubahan role atau permission.
* Pembuatan announcement.
* Perubahan announcement.
* Publishing announcement.
* Perubahan schedule.
* Pengelolaan event.
* Pengelolaan resource.
* Penghapusan data penting.
* Perubahan konfigurasi sistem.

Tidak semua aktivitas pengguna harus menjadi audit event.

Audit event harus berfokus pada aktivitas yang memiliki nilai keamanan, governance, atau accountability.

---

# 👤 Audit Event Context

Audit event idealnya menyediakan konteks yang cukup untuk menjawab:

> **Siapa melakukan apa, terhadap resource apa, dan kapan?**

Secara konseptual:

```text
Audit Event
 │
 ├── Actor
 ├── Action
 ├── Resource
 ├── Timestamp
 ├── Result
 └── Context
```

Contoh konseptual:

```text
Actor:
Superadmin

Action:
Publish Announcement

Resource:
Announcement #123

Timestamp:
2026-08-08T10:00:00

Result:
Success
```

Struktur detail audit record mengikuti desain data yang telah ditentukan pada DDS.

---

# 🔐 Security Audit Events

Aktivitas yang berkaitan dengan keamanan harus dapat ditelusuri apabila dianggap relevan.

Contohnya:

```text
Authentication
      │
      ├── Login Success
      ├── Login Failure
      ├── Logout
      └── Session Failure

Authorization
      │
      ├── Access Granted
      └── Access Denied
```

Audit terhadap security event membantu proses investigasi apabila terjadi aktivitas yang tidak diharapkan.

---

# 📊 Application Logging

Application log digunakan untuk memberikan informasi mengenai perilaku aplikasi.

Log dapat digunakan untuk:

* Mengetahui error.
* Mengetahui warning.
* Melacak proses penting.
* Membantu debugging.
* Mengetahui kegagalan dependency.
* Mendukung incident investigation.

Logging tidak boleh digunakan sebagai pengganti audit trail.

---

# 🔍 Audit vs Application Log

Audit log dan application log memiliki tujuan berbeda.

| Aspect          | Audit Log          | Application Log      |
| --------------- | ------------------ | -------------------- |
| Primary Purpose | Accountability     | Troubleshooting      |
| Focus           | User/system action | Application behavior |
| Security        | High               | Supporting           |
| Business Action | Important          | Optional             |
| Debugging       | Limited            | Primary              |
| Retention       | Governance-based   | Operational-based    |

Satu event dapat menghasilkan audit record dan application log apabila keduanya dibutuhkan.

---

# 📈 Observability

Observability digunakan untuk mengetahui kondisi internal sistem melalui informasi yang dihasilkan oleh sistem.

Area observability meliputi:

```text
Observability
      │
      ├── Logs
      ├── Metrics
      ├── Errors
      └── Operational Signals
```

Tujuan utamanya adalah memungkinkan engineering team memahami:

* Apakah sistem berjalan normal.
* Apakah terjadi error.
* Komponen mana yang mengalami masalah.
* Apakah dependency mengalami degradation.
* Apakah terjadi perubahan perilaku sistem.

---

# 📡 Monitoring

Monitoring digunakan untuk mengamati indikator operasional sistem.

Contoh indikator:

* Application availability.
* Error rate.
* Request failure.
* Database availability.
* Storage availability.
* Resource utilization.
* Dependency availability.

Monitoring harus berfokus pada indikator yang memiliki nilai operasional.

---

# 🚨 Alerting

Monitoring dapat menghasilkan alert ketika kondisi tertentu menunjukkan potensi masalah.

Konseptual flow:

```text
System
  │
  ▼
Metric / Log / Error
  │
  ▼
Monitoring
  │
  ├── Normal
  │
  └── Abnormal
          │
          ▼
        Alert
          │
          ▼
      Investigation
```

Alert harus digunakan untuk kondisi yang membutuhkan perhatian.

Terlalu banyak alert yang tidak actionable dapat mengurangi efektivitas monitoring.

---

# ❌ Error Tracking

Error penting harus dapat diidentifikasi dan ditelusuri.

Error tracking harus membantu menjawab:

* Apa yang gagal?
* Kapan terjadi?
* Komponen mana yang terlibat?
* Seberapa sering terjadi?
* Apakah error memengaruhi pengguna?
* Apakah error masih berlangsung?

Error yang diketahui tetapi tidak dapat ditelusuri kembali akan menyulitkan proses incident handling.

---

# 🔗 Correlation & Context

Informasi log dan audit sebaiknya memiliki konteks yang memungkinkan event berkaitan satu sama lain.

Contoh:

```text
Request
   │
   ├── Application Log
   │
   ├── Error
   │
   └── Audit Event
```

Correlation identifier dapat digunakan apabila diperlukan untuk menghubungkan beberapa event dalam satu execution flow.

Detail mekanisme implementasi berada pada dokumentasi teknis terkait.

---

# 🔒 Log Security

Log dan audit record dapat mengandung informasi sensitif.

Karena itu:

* Log tidak boleh dapat diakses oleh pengguna umum.
* Access terhadap audit log harus dibatasi.
* Credential tidak boleh dicatat secara langsung.
* Secret tidak boleh ditulis ke log.
* Data sensitif harus diperlakukan sesuai kebijakan privacy dan security.

Logging tidak boleh menjadi sumber kebocoran credential atau secret.

---

# 🗃️ Log Retention

Log dan audit record harus memiliki kebijakan retention yang sesuai dengan tujuan masing-masing.

Audit retention mempertimbangkan:

* Governance.
* Accountability.
* Security investigation.

Application log retention mempertimbangkan:

* Troubleshooting.
* Operational monitoring.
* Storage capacity.

Nilai retention spesifik ditetapkan pada konfigurasi operasional apabila belum dikunci oleh requirement.

---

# 👥 Log Access

Access terhadap audit dan operational logs harus dibatasi berdasarkan kebutuhan.

Contoh:

```text
Student
   │
   └── No Direct Audit Access

Administrator
   │
   └── Limited Operational Access

Superadmin / Authorized Operator
   │
   └── Administrative Access
```

Hak akses aktual harus mengikuti role dan permission yang ditentukan oleh sistem.

---

# 🕵️ Incident Investigation

Audit dan observability harus mendukung investigasi incident.

Contoh alur:

```text
Incident
   │
   ▼
Detection
   │
   ▼
Logs / Metrics / Audit
   │
   ▼
Investigation
   │
   ▼
Root Cause Analysis
   │
   ▼
Remediation
```

Data observability dan audit harus dipertahankan secukupnya untuk memungkinkan proses tersebut.

---

# 🧩 Observability Boundary

Observability bukan bagian dari business logic domain.

```text
Business Modules
       │
       ├── Application Events
       ├── Errors
       └── Operational Signals
                │
                ▼
        Observability Layer
```

Observability harus membantu sistem dipahami tanpa mengambil alih tanggung jawab domain.

---

# 🚧 Architecture Constraints

Implementasi harus mematuhi aturan berikut:

* Aktivitas penting harus dapat diaudit.
* Audit log harus memiliki actor dan timestamp.
* Security event penting harus dapat ditelusuri.
* Application error harus dapat diidentifikasi.
* Log tidak boleh menyimpan secret secara langsung.
* Audit log harus dilindungi dari unauthorized access.
* Monitoring harus berfokus pada indikator yang actionable.
* Alert harus dapat ditindaklanjuti.
* Audit log tidak boleh digantikan hanya dengan application log.
* Observability tidak boleh mengubah business ownership domain.

---

# 🔄 Change Management

Perubahan terhadap audit dan observability harus melalui architecture review apabila perubahan tersebut:

* Menghapus audit event penting.
* Mengubah ownership audit data.
* Mengubah security logging.
* Mengubah observability architecture.
* Menghilangkan monitoring terhadap critical component.
* Mengubah akses terhadap audit log.

Perubahan signifikan harus ditelusuri kembali ke requirement dan desain terkait.

---

# 📚 Related Documents

## Previous Documents

* [AHS README](./README.md)
* [AHS-001: Architecture Overview](./01-ahs-001-architecture-overview.md)
* [AHS-002: Module Boundaries](./02-ahs-002-module-boundaries.md)
* [AHS-003: Authentication & Authorization](./03-ahs-003-authentication-and-authorization.md)
* [AHS-004: File Storage & Data Consistency](./04-ahs-004-file-storage-and-data-consistency.md)
* [AHS-005: Backup & Recovery](./05-ahs-005-backup-and-recovery.md)

## Related Documents

* [DDS-006: Security Design](../DDS/06-dds-006-security-design.md)
* [IDR-006: Error Handling Guidelines](../IDR/06-idr-006-error-handling-guidelines.md)
* [IDR-011: Observability & Monitoring](../IDR/11-idr-011-observability-and-monitoring.md)

## Next Document

* [AHS-007: External Dependencies](./07-ahs-007-external-dependencies.md)

---

# ✅ Review Checklist

* [ ] Audit logging strategy telah ditentukan.
* [ ] Audit event penting telah diidentifikasi.
* [ ] Security events telah dipertimbangkan.
* [ ] Application logging telah dibedakan dari audit logging.
* [ ] Observability scope telah ditentukan.
* [ ] Monitoring telah didefinisikan.
* [ ] Alerting principle telah ditentukan.
* [ ] Error tracking telah dipertimbangkan.
* [ ] Log security telah ditentukan.
* [ ] Log access telah ditentukan.
* [ ] Retention telah dipertimbangkan.
* [ ] Incident investigation telah didukung.

---

# 🔄 Traceability Matrix

| Area                         | Related Documentation |
| ---------------------------- | --------------------- |
| Audit Requirements           | RHS                   |
| Audit Architecture           | SDS                   |
| Security Design              | DDS-006               |
| Error Handling               | IDR-006               |
| Observability Implementation | IDR-011               |
| Audit & Observability        | AHS-006               |
| Operational Governance       | AHS-008               |
| Failure Recovery             | AHS-009               |

---

# 📝 Revision History

| Version | Date       | Description                                 | Author          |
| ------- | ---------- | ------------------------------------------- | --------------- |
| 1.0     | 2026-08-08 | Initial Audit & Observability documentation | Abidzar Dzakwan |
