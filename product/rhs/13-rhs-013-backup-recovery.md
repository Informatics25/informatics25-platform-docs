# 💾 RHS-013: Backup, Recovery & Availability

> **Requirement Hardening Specification**
>
> **Reference:** PRD §12.2 – Backup, PRD §12.3 – Recovery, PRD §16 – Deployment & Operational Requirements
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical (Launch Blocking)

---

# 📖 Overview

Dokumen ini mendefinisikan aturan implementasi proses Backup, Restore, Disaster Recovery, dan Availability untuk Platform Informatika Angkatan 2025.

Tujuan utama adalah memastikan bahwa kehilangan data dapat diminimalkan dan layanan dapat dipulihkan dalam waktu yang dapat diterima apabila terjadi kegagalan sistem.

Backup merupakan bagian dari strategi Business Continuity dan bukan sekadar proses penyalinan database.

---

# 🎯 Objectives

- Melindungi data dari kehilangan.
- Menjamin proses restore dapat dilakukan.
- Memastikan backup dilakukan secara otomatis.
- Memenuhi target Recovery Point Objective (RPO).
- Memenuhi target Recovery Time Objective (RTO).

---

# 📋 Business Rules

| ID | Rule |
|----|------|
| BAK-01 | Database wajib dibackup secara berkala. |
| BAK-02 | Backup dilakukan secara otomatis tanpa intervensi manual. |
| BAK-03 | Backup harus disimpan pada lokasi yang terpisah dari server utama apabila memungkinkan. |
| BAK-04 | Backup wajib diuji melalui proses restore secara berkala. |
| BAK-05 | Backup tidak boleh mengganggu operasional sistem. |
| BAK-06 | Seluruh proses backup dan restore harus menghasilkan Audit Log. |
| BAK-07 | Backup harus dienkripsi apabila disimpan di media eksternal atau cloud. |
| BAK-08 | Restore hanya boleh dilakukan oleh Superadmin atau personel yang berwenang. |
| BAK-09 | Target RPO maksimum adalah 24 jam sesuai PRD. |
| BAK-10 | Target pemulihan kegagalan serius (RTO) maksimum adalah 24 jam sesuai PRD. |

---

# 🏗 Architecture Notes

Backup mencakup:

- Database
- Object Storage Metadata
- Konfigurasi sistem
- File konfigurasi deployment
- Audit Log (sesuai kebijakan)

Object Storage tidak selalu harus dibackup apabila menggunakan layanan yang telah menyediakan redundansi bawaan, tetapi metadata dan referensi file tetap harus dapat dipulihkan.

---

# 📊 Backup Scope

| Resource | Included |
|----------|----------|
| PostgreSQL Database | ✅ |
| User Data | ✅ |
| Official Information | ✅ |
| Schedule | ✅ |
| Knowledge Hub Metadata | ✅ |
| Audit Log | ✅ |
| Configuration | ✅ |
| Uploaded Files | Sesuai kebijakan storage |
| Application Source Code | Melalui Git Repository |

---

# ✅ Validation Rules

| ID | Rule |
|----|------|
| VAL-01 | Backup harus selesai tanpa error. |
| VAL-02 | File backup wajib lolos integrity check. |
| VAL-03 | Restore harus berhasil pada lingkungan pengujian. |
| VAL-04 | Backup tidak boleh overwrite backup aktif secara tidak sengaja. |
| VAL-05 | Metadata backup wajib tercatat. |

---

# 🔐 Permission Rules

| Role | Permission |
|------|------------|
| Student | Tidak memiliki akses |
| Administrator | Melihat status backup (opsional sesuai kebijakan) |
| Superadmin | Full Backup & Restore |

---

# 📊 Service Level Objectives (SLO)

| Metric | Target |
|---------|--------|
| Recovery Point Objective (RPO) | ≤ 24 Jam |
| Recovery Time Objective (RTO) | ≤ 24 Jam |
| Backup Success Rate | ≥ 99% |
| Restore Success Rate | 100% pada pengujian berkala |

---

# 🔄 Backup Lifecycle

```text
Scheduled
     │
     ▼
Backup Started
     │
     ▼
Backup Completed
     │
     ▼
Integrity Validation
     │
     ▼
Archive
```

---

# 🚨 Failure Scenarios

| Scenario | Expected Behavior |
|-----------|------------------|
| Backup gagal | Sistem mencatat audit dan mengirim alert |
| Storage penuh | Backup dihentikan secara aman dan administrator diberi notifikasi |
| File backup rusak | Restore dibatalkan dan backup lain digunakan |
| Restore gagal | Sistem mempertahankan data aktif dan mencatat audit |
| Server utama gagal | Disaster Recovery Procedure dijalankan |

---

# ⚠ Edge Cases

| Scenario | Expected Behavior |
|-----------|------------------|
| Backup berjalan saat traffic tinggi | Backup tetap berjalan tanpa mengganggu layanan secara signifikan |
| Backup dijalankan dua kali | Tidak menghasilkan data korup atau konflik |
| Restore ke versi lama | Dilakukan hanya melalui prosedur yang disetujui |
| Backup terhapus | Menggunakan backup cadangan sesuai retensi |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | Jadwal backup | Waktu backup tiba | Backup otomatis dijalankan |
| AC-02 | Backup selesai | Integrity check | Backup dinyatakan valid |
| AC-03 | Restore diminta | Restore selesai | Data berhasil dipulihkan |
| AC-04 | Backup gagal | Monitoring berjalan | Alert dikirim |
| AC-05 | Audit diperiksa | Backup dijalankan | Audit Log tersedia |

---

# 🔒 Security Requirements

- Backup harus dienkripsi saat disimpan di luar server utama.
- Backup tidak boleh dapat diakses publik.
- Credential backup tidak boleh disimpan dalam source code.
- Restore harus memerlukan otorisasi.
- Seluruh aktivitas backup menghasilkan Audit Log.

---

# 📈 Observability

Sistem harus memonitor minimal:

- Backup Success Rate
- Backup Duration
- Restore Duration
- Restore Success Rate
- Backup Size
- Last Successful Backup
- Backup Failure Count

---

# 📊 Backup Metadata

| Field | Description |
|---------|-------------|
| Backup ID | Identifier unik |
| Started At | Waktu mulai |
| Finished At | Waktu selesai |
| Backup Size | Ukuran backup |
| Status | Success / Failed |
| Storage Location | Lokasi backup |
| Checksum | Validasi integritas |

---

# 🌐 API Reference

## Manual Backup

```http
POST /api/v1/admin/backup
```

---

## Restore Backup

```http
POST /api/v1/admin/restore
```

---

## Backup Status

```http
GET /api/v1/admin/backup/status
```

---

# 🔗 Requirement Traceability Matrix (RTM)

| RHS ID | PRD Reference |
|---------|---------------|
| BAK-01 | PRD §12.2 |
| BAK-02 | PRD §12.2 |
| BAK-03 | PRD §12.2 |
| BAK-04 | PRD §12.2 |
| BAK-05 | PRD §12.2 |
| BAK-06 | PRD §12 |
| BAK-07 | PRD §12.2 |
| BAK-08 | PRD §12.3 |
| BAK-09 | PRD §12.2 |
| BAK-10 | PRD §12.3 |

---

# 🔗 Related Documents

- PRD §12 – Backup & Recovery
- PRD §16 – Deployment & Operational Requirements
- RHS-009 – Security
- RHS-012 – Audit Log
- RHS-014 – Performance, Availability & Resilience

---

# 📝 Notes

- Backup bukan pengganti High Availability.
- Restore harus diuji secara berkala, bukan hanya diasumsikan dapat berjalan.
- Disaster Recovery Plan harus terdokumentasi dan dapat dijalankan oleh pengelola yang berwenang.
- Target RPO dan RTO pada dokumen ini mengikuti baseline MVP yang telah ditetapkan dalam PRD.
