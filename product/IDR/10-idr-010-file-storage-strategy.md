# 📁 IDR-010: File Storage Strategy

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD §13.3 Object Storage, RHS-005 Knowledge Hub, RHS-006 Event & Gallery
>
> **Status:** ✅ Approved
>
> **Priority:** 🟡 High

---

# 📖 Overview

Dokumen ini mendefinisikan strategi penyimpanan file yang digunakan oleh Platform Digital Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh file pengguna dikelola secara aman, efisien, mudah dipelihara, dan tidak membebani database utama.

Seluruh file seperti dokumen, gambar, poster, dan media lainnya disimpan pada Object Storage, sedangkan database hanya menyimpan metadata file.

---

# 🎯 Objectives

- Memisahkan penyimpanan file dari database.
- Menjamin skalabilitas penyimpanan.
- Mempermudah proses backup dan recovery.
- Mendukung pengelolaan file dalam jumlah besar.
- Menyediakan standar penyimpanan file yang konsisten.

---

# 📦 Scope

Dokumen ini mencakup penyimpanan:

- Knowledge Hub Resources
- Gallery
- Event Poster
- Profile Picture
- Landing Page Assets
- Attachment lainnya

Dokumen ini tidak membahas penyimpanan log aplikasi maupun backup database.

---

# 🏗 Storage Architecture

Platform menggunakan arsitektur berikut:

```text
Frontend
      │
      ▼
Backend API
      │
      ├──────────────► PostgreSQL
      │                   │
      │                   └── Metadata File
      │
      ▼
Object Storage
      │
      └── File Binary
```

Database hanya menyimpan informasi mengenai file, sedangkan isi file disimpan pada Object Storage.

---

# 📂 Supported Storage

Implementasi direkomendasikan menggunakan layanan yang kompatibel dengan Amazon S3 API.

Contoh:

- Cloudflare R2
- MinIO
- Amazon S3
- DigitalOcean Spaces

Pemilihan penyedia bergantung pada kebutuhan operasional dan biaya.

---

# 📑 Supported File Types

## Knowledge Hub

- PDF
- DOCX
- PPTX
- XLSX
- ZIP
- Gambar
- External Link

---

## Gallery

- JPG
- JPEG
- PNG
- WEBP

---

## Event

- Poster
- Banner
- Dokumentasi

---

## Profile

- JPG
- PNG
- WEBP

---

# 🚫 Unsupported Files

MVP tidak mendukung penyimpanan langsung untuk:

- Video
- Executable (.exe)
- Script yang dapat dieksekusi
- File berbahaya

Video direkomendasikan menggunakan layanan eksternal dan hanya menyimpan tautannya.

---

# 📋 Metadata Strategy

Database hanya menyimpan metadata.

Minimal meliputi:

| Field | Description |
|---------|-------------|
| id | UUID |
| filename | Nama file asli |
| object_key | Lokasi file pada Object Storage |
| mime_type | MIME Type |
| size | Ukuran file |
| uploaded_by | User ID |
| uploaded_at | Timestamp |
| checksum | Verifikasi integritas |
| visibility | Public / Internal |
| status | Pending / Approved / Rejected |

---

# 🗂 Object Key Convention

Gunakan struktur direktori berikut.

```text
knowledge-hub/

gallery/

profile/

events/

landing-page/
```

Contoh:

```text
knowledge-hub/2026/algoritma/file.pdf

gallery/2026/event-1/photo-01.webp

profile/user-uuid/avatar.webp
```

---

# 🔒 Access Control

File mengikuti hak akses sesuai RBAC.

| Resource | Guest | Student | Admin | Superadmin |
|-----------|-------|----------|-------|------------|
| Public Gallery | ✅ | ✅ | ✅ | ✅ |
| Internal Resource | ❌ | ✅ | ✅ | ✅ |
| Pending Resource | ❌ | ❌ | ✅ | ✅ |

Backend menjadi otoritas dalam proses validasi akses.

---

# 🛡 Security Requirements

Seluruh file harus:

- Diverifikasi tipe filenya.
- Diverifikasi ukuran file.
- Memiliki nama file yang disanitasi.
- Disimpan menggunakan object key yang aman.
- Tidak dapat diakses langsung apabila bersifat privat.

Executable file tidak diperbolehkan diunggah.

---

# 📏 File Size Policy

Ukuran maksimum ditentukan berdasarkan jenis file.

| Resource | Recommendation |
|-----------|----------------|
| Profile Picture | 5 MB |
| Gallery | 10 MB |
| Poster Event | 10 MB |
| Knowledge Hub Document | 50 MB |

Nilai tersebut dapat disesuaikan sesuai kapasitas infrastruktur.

---

# 🧪 File Validation

Sebelum penyimpanan dilakukan:

- Validasi MIME Type.
- Validasi ekstensi.
- Validasi ukuran.
- Validasi hak akses pengguna.
- Pemeriksaan duplikasi (opsional menggunakan checksum).

---

# 🔄 Lifecycle

Lifecycle file:

```text
Upload

↓

Pending Review

↓

Approved / Rejected

↓

Published

↓

Archived

↓

Deleted (Jika diperlukan)
```

File yang ditolak tidak dipublikasikan kepada pengguna.

---

# 📦 Backup Strategy

Backup Object Storage mengikuti kebijakan backup infrastruktur.

Metadata database dan file harus tetap konsisten.

Restore metadata tanpa file, atau sebaliknya, dianggap sebagai kondisi inkonsisten dan harus ditangani melalui prosedur recovery.

---

# 🚀 Future Considerations

Fitur berikut berada di luar MVP namun dapat dipertimbangkan:

- Antivirus Scanning
- Automatic Image Compression
- Thumbnail Generation
- CDN Integration
- File Versioning
- Lifecycle Policy
- Signed URL
- Multi-region Replication

---

# ✅ Review Checklist

- [ ] Database hanya menyimpan metadata.
- [ ] File disimpan pada Object Storage.
- [ ] MIME Type divalidasi.
- [ ] Ukuran file divalidasi.
- [ ] RBAC diterapkan.
- [ ] Object Key mengikuti standar.
- [ ] File privat tidak dapat diakses langsung.
- [ ] Strategi backup terdokumentasi.

---

# 🔄 Traceability Matrix

| IDR Section | Related Requirement |
|-------------|---------------------|
| Knowledge Hub | RHS-005 |
| Event | RHS-006 |
| Gallery | RHS-006 |
| RBAC | RHS-008 |
| Backup | RHS-013 |
| Security | RHS-009 |

---

# 🔗 References

## Product Documents

- [PRD](../PRD.md)

## Requirement Hardening

- [RHS README](../RHS/README.md)
- [RHS-005 Knowledge Hub](../RHS/05-rhs-005-knowledge-hub.md)
- [RHS-006 Event & Gallery](../RHS/06-rhs-006-event-gallery.md)
- [RHS-008 RBAC](../RHS/08-rhs-008-rbac.md)
- [RHS-009 Security](../RHS/09-rhs-009-security-2fa.md)
- [RHS-013 Backup & Recovery](../RHS/13-rhs-013-backup-recovery.md)

## Previous IDR

- [IDR-002 Technology Stack](02-idr-002-technology-stack.md)
- [IDR-008 Database Design Guidelines](08-idr-008-database-design-guidelines.md)
- [IDR-009 Authentication Architecture](09-idr-009-authentication-architecture.md)

## Future Documents

- `docs/DDS/README.md`
- `docs/SDS/README.md`

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-02 | Initial File Storage Strategy documentation |
