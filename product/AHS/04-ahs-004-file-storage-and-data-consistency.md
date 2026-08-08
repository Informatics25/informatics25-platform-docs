# AHS-004: File Storage & Data Consistency

> **Architecture Hardening Specification (AHS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini mendefinisikan strategi penyimpanan file dan konsistensi antara file dengan metadata yang tersimpan pada sistem Platform Digital Informatika Angkatan 2025.

File digunakan oleh beberapa fitur sistem, terutama **Knowledge Hub** dan **Gallery**. Karena file dan metadata dapat berada pada media penyimpanan yang berbeda, sistem harus memiliki aturan yang memastikan keduanya tetap konsisten dan dapat dipulihkan apabila terjadi kegagalan.

Dokumen ini berfokus pada keputusan arsitektur dan aturan yang harus dipatuhi selama implementasi.

---

# 🎯 Objectives

Dokumen ini bertujuan untuk:

* Menentukan tanggung jawab penyimpanan file dan metadata.
* Menjaga konsistensi antara file dan metadata.
* Mencegah orphan file dan orphan metadata.
* Mendefinisikan perilaku ketika proses penyimpanan gagal.
* Menentukan prinsip penghapusan dan penggantian file.
* Menjadi acuan implementasi file lifecycle.

---

# 📦 Scope

Dokumen ini mencakup:

* File Storage
* File Metadata
* File Lifecycle
* Upload Strategy
* Delete Strategy
* Replacement Strategy
* Consistency Handling
* Failure Handling

Dokumen ini tidak membahas konfigurasi storage provider secara spesifik.

---

# 🗂️ File & Metadata Model

Sistem membedakan antara:

```text
File Object
     │
     │
     ▼
Storage System
```

dan:

```text
File Metadata
     │
     ▼
Application Database
```

Secara konseptual:

```text
┌─────────────────────┐
│ Application Database│
│                     │
│ File Metadata       │
│ - ID                │
│ - Name              │
│ - Size              │
│ - MIME Type         │
│ - Storage Reference │
└──────────┬──────────┘
           │
           │ Reference
           ▼
┌─────────────────────┐
│ File Storage        │
│                     │
│ Actual File Object  │
└─────────────────────┘
```

Database menyimpan informasi yang dibutuhkan aplikasi untuk menemukan dan mengelola file, sedangkan file object disimpan pada media penyimpanan file.

---

# 🏷️ Metadata Ownership

Metadata file menjadi tanggung jawab domain yang menggunakan file tersebut.

Contoh:

```text
Knowledge Hub
      │
      └── Resource Metadata
             │
             └── File Reference

Gallery
      │
      └── Media Metadata
             │
             └── File Reference
```

Storage layer tidak mengambil alih business ownership dari metadata.

---

# 🔄 File Lifecycle

File harus memiliki lifecycle yang jelas.

```text
Created
   │
   ▼
Uploaded
   │
   ▼
Validated
   │
   ▼
Referenced
   │
   ├───────────────┐
   │               │
   ▼               ▼
Replaced         Deleted
   │               │
   ▼               ▼
Archived/Removed  Removed
```

Lifecycle aktual dapat berbeda berdasarkan domain, tetapi setiap perubahan status harus menghasilkan kondisi yang konsisten antara metadata dan file object.

---

# ⬆️ Upload Strategy

Proses upload harus memastikan bahwa file telah berhasil disimpan sebelum metadata dianggap valid.

Konseptual flow:

```text
User
 │
 ▼
Upload File
 │
 ▼
Validate File
 │
 ▼
Store File
 │
 ├── Failed ──► Stop
 │
 ▼
Create Metadata
 │
 ▼
Commit Reference
 │
 ▼
File Available
```

Metadata tidak boleh menunjukkan file yang tidak berhasil disimpan.

---

# 🔐 File Validation

Sebelum file diterima sistem, validasi harus dilakukan sesuai aturan domain.

Validasi dapat mencakup:

* File type
* MIME type
* File size
* File name
* File extension
* Domain-specific restrictions

File yang gagal validasi tidak boleh menjadi bagian dari persistent state sistem.

---

# 🔄 Replacement Strategy

Ketika file yang telah tersimpan diganti dengan file baru, sistem harus menghindari kondisi di mana metadata menunjuk pada file yang tidak tersedia.

Prinsip replacement:

```text
Existing File
      │
      ▼
Validate New File
      │
      ▼
Store New File
      │
      ▼
Update Metadata Reference
      │
      ▼
Remove / Retire Old File
```

File lama tidak boleh dihapus sebelum file baru berhasil disimpan dan metadata berhasil diperbarui.

---

# 🗑️ Delete Strategy

Penghapusan file harus mempertimbangkan hubungan antara metadata dan file object.

Prinsip:

```text
Metadata Reference
       │
       ▼
Delete / Disable Reference
       │
       ▼
Remove File Object
```

Sistem tidak boleh meninggalkan metadata aktif yang menunjuk pada file yang sudah tidak tersedia.

---

# ⚠️ Failure Scenarios

## Scenario 1 — Upload Failure

```text
Upload
  │
  ▼
Storage Failure
  │
  ▼
No Metadata Commit
```

Jika penyimpanan file gagal, metadata tidak boleh dianggap berhasil dibuat.

---

## Scenario 2 — Metadata Failure

```text
File Stored
    │
    ▼
Metadata Failure
    │
    ▼
Recovery / Cleanup
```

Apabila file telah tersimpan tetapi metadata gagal dibuat, sistem harus memiliki mekanisme untuk menangani file yang tidak memiliki reference aktif.

Tujuannya adalah mencegah **orphan file**.

---

## Scenario 3 — Delete Failure

```text
Delete Metadata
      │
      ▼
File Delete Failure
      │
      ▼
Inconsistent Storage State
```

Kegagalan penghapusan file harus dapat terdeteksi dan ditangani melalui mekanisme recovery atau cleanup.

---

# 🧹 Orphan File Handling

Orphan file adalah file yang terdapat pada storage tetapi tidak memiliki metadata/reference aktif pada database.

Contoh:

```text
Storage
 ├── file-a
 ├── file-b
 └── file-c

Database
 ├── file-a
 └── file-c
```

Pada contoh tersebut:

```text
file-b = orphan file
```

Sistem harus menyediakan mekanisme untuk mendeteksi dan menangani kondisi tersebut.

Cleanup tidak boleh dilakukan secara sembarangan karena file yang tampak tidak direferensikan dapat masih berada dalam proses transaksi atau recovery.

---

# 🗃️ Orphan Metadata Handling

Kondisi sebaliknya juga harus dicegah:

```text
Database
 ├── file-a
 ├── file-b
 └── file-c

Storage
 ├── file-a
 └── file-c
```

Dalam kondisi tersebut:

```text
file-b = orphan metadata
```

Metadata yang menunjuk pada file yang tidak tersedia harus dapat dideteksi dan ditangani.

---

# 🔄 Consistency Principle

Sistem harus berusaha mempertahankan invariant berikut:

```text
Active Metadata
      │
      ▼
Valid File Reference
      │
      ▼
Available File Object
```

Metadata aktif tidak boleh menunjuk pada file object yang diketahui tidak tersedia.

---

# 🛡️ Recovery Principle

Apabila terjadi ketidakkonsistenan antara metadata dan storage, sistem harus:

1. Mendeteksi kondisi.
2. Mencatat kejadian.
3. Menentukan status resource.
4. Melakukan recovery atau cleanup sesuai kasus.
5. Memastikan kondisi akhir dapat diverifikasi.

---

# 📊 Consistency Responsibility

| Component           | Responsibility             |
| ------------------- | -------------------------- |
| Domain Service      | Menjaga business state     |
| Metadata Repository | Menyimpan metadata         |
| Storage Layer       | Menyimpan file object      |
| Recovery Process    | Menangani inconsistency    |
| Audit               | Mencatat aktivitas penting |

Tidak ada satu komponen yang boleh mengabaikan kegagalan komponen lainnya ketika proses file lifecycle berlangsung.

---

# 🚧 Architecture Constraints

Implementasi harus mematuhi aturan berikut:

* Metadata tidak boleh dianggap valid sebelum file berhasil diproses.
* File tidak boleh dihapus sebelum replacement berhasil.
* Orphan file harus dapat dideteksi.
* Orphan metadata harus dapat dideteksi.
* File lifecycle harus dapat ditelusuri.
* Kegagalan storage harus dapat ditangani.
* Kegagalan metadata persistence harus dapat ditangani.
* File reference harus tetap konsisten dengan domain owner.

---

# 🔄 Change Management

Perubahan terhadap strategi file storage harus melalui architecture review apabila perubahan tersebut:

* Mengubah storage provider.
* Mengubah ownership metadata.
* Mengubah file lifecycle.
* Mengubah mekanisme consistency.
* Mengubah recovery strategy.
* Menambahkan tipe file yang memiliki kebutuhan khusus.

Perubahan signifikan harus ditelusuri kembali ke DDS dan dokumentasi requirement terkait.

---

# 📚 Related Documents

## Previous Documents

* [AHS README](./README.md)
* [AHS-001: Architecture Overview](./01-ahs-001-architecture-overview.md)
* [AHS-002: Module Boundaries](./02-ahs-002-module-boundaries.md)
* [AHS-003: Authentication & Authorization](./03-ahs-003-authentication-and-authorization.md)

## Related Design Documents

* [DDS-003: Domain Design](../DDS/03-dds-003-domain-design.md)
* [DDS-004: Data Design](../DDS/04-dds-004-data-design.md)
* [DDS-005: Interface Design](../DDS/05-dds-005-interface-design.md)
* [DDS-006: Security Design](../DDS/06-dds-006-security-design.md)

## Related Modules

* Knowledge Hub
* Gallery

## Next Document

* [AHS-005: Backup & Recovery](./05-ahs-005-backup-and-recovery.md)

---

# ✅ Review Checklist

* [ ] File storage responsibility telah didefinisikan.
* [ ] Metadata ownership telah ditentukan.
* [ ] File lifecycle telah dijelaskan.
* [ ] Upload strategy telah ditentukan.
* [ ] Replacement strategy telah ditentukan.
* [ ] Delete strategy telah ditentukan.
* [ ] Orphan file telah dipertimbangkan.
* [ ] Orphan metadata telah dipertimbangkan.
* [ ] Failure handling telah didefinisikan.
* [ ] Recovery principle telah ditetapkan.

---

# 🔄 Traceability Matrix

| Area              | Related Documentation |
| ----------------- | --------------------- |
| Knowledge Hub     | RHS                   |
| Gallery           | RHS                   |
| Data Design       | DDS-004               |
| Domain Ownership  | DDS-003               |
| Interface Design  | DDS-005               |
| Security          | DDS-006               |
| File Storage      | AHS-004               |
| Backup & Recovery | AHS-005               |
| Failure Recovery  | AHS-009               |

---

# 📝 Revision History

| Version | Date       | Description                                           | Author          |
| ------- | ---------- | ----------------------------------------------------- | --------------- |
| 1.0     | 2026-08-08 | Initial File Storage & Data Consistency documentation | Abidzar Dzakwan |
