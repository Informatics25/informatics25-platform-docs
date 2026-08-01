# IDR-008: Database Design Guidelines

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD, RHS, IDR-002 Technology Stack, IDR-007 API Design Guidelines
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 High

---

# 📖 Overview

Dokumen ini mendefinisikan standar dan pedoman desain database yang digunakan pada Platform Informatika Angkatan 2025.

Tujuan utama dokumen ini adalah memastikan seluruh struktur database memiliki desain yang konsisten, mudah dipelihara, aman, dan mampu berkembang seiring bertambahnya kebutuhan sistem.

Dokumen ini tidak mendefinisikan Entity Relationship Diagram (ERD) ataupun skema tabel secara rinci. Detail tersebut akan dijelaskan pada **Database Design Specification (DDS)**.

---

# 🎯 Objectives

- Menetapkan standar desain database.
- Menjaga konsistensi struktur data.
- Mengurangi redundansi data.
- Mendukung skalabilitas sistem.
- Mempermudah proses maintenance dan migrasi database.
- Menjadi acuan implementasi seluruh database engineer.

---

# 📦 Scope

Pedoman ini berlaku untuk seluruh database yang digunakan oleh Platform Informatika Angkatan 2025, termasuk:

- Authentication
- User Management
- Official Information
- Schedule
- Knowledge Hub
- Event
- Gallery
- Notification
- Audit Log
- Analytics

---

# 🛠 Database Management System

Platform menggunakan:

| Item | Value |
|------|-------|
| Database | PostgreSQL |
| ORM | Prisma ORM |
| Migration | Prisma Migration |
| UUID | UUID v4 |
| Charset | UTF-8 |
| Timezone | UTC |

---

# 📐 General Design Principles

Seluruh desain database harus mengikuti prinsip berikut:

- Normalize before optimize.
- Hindari redundansi data.
- Gunakan foreign key untuk menjaga integritas data.
- Seluruh tabel harus memiliki primary key.
- Seluruh data penting harus dapat diaudit.
- Desain harus mendukung ekspansi fitur di masa depan.

---

# 🗂 Table Naming Convention

Gunakan format:

```
snake_case
```

Contoh:

```
users
official_information
knowledge_hub_resources
resource_categories
schedule_exceptions
audit_logs
```

---

# 🔑 Primary Key Convention

Seluruh tabel menggunakan:

```
id UUID
```

Contoh:

```sql
id UUID PRIMARY KEY
```

---

# 🔗 Foreign Key Convention

Foreign key menggunakan format:

```
xxx_id
```

Contoh:

```
user_id
publisher_id
resource_id
announcement_id
```

---

# 📅 Timestamp Convention

Seluruh tabel minimal memiliki field berikut:

```text
created_at
updated_at
```

Jika mendukung soft delete:

```text
deleted_at
```

Gunakan tipe data:

```sql
TIMESTAMP WITH TIME ZONE
```

---

# 📝 Enum Strategy

Gunakan enum database atau enum pada Prisma untuk data yang memiliki nilai tetap.

Contoh:

- UserRole
- AnnouncementPriority
- AnnouncementStatus
- ResourceStatus
- NotificationChannel

Hindari penggunaan string bebas untuk nilai yang bersifat tetap.

---

# 🗃 Soft Delete Strategy

Data penting tidak langsung dihapus secara permanen.

Gunakan:

```text
deleted_at
```

Data dianggap aktif apabila:

```
deleted_at IS NULL
```

Hard delete hanya dilakukan untuk kebutuhan administrasi tertentu sesuai kebijakan platform.

---

# 📚 Normalization Rules

Seluruh desain tabel minimal memenuhi Third Normal Form (3NF), kecuali terdapat alasan performa yang terdokumentasi.

Denormalisasi hanya diperbolehkan apabila:

- terdapat kebutuhan performa,
- telah dianalisis dampaknya,
- serta didokumentasikan pada dokumen implementasi terkait.

---

# 📊 Indexing Guidelines

Index dibuat pada kolom yang sering digunakan untuk:

- pencarian,
- filtering,
- sorting,
- relasi (foreign key),
- unique constraint.

Contoh:

```text
nim
email
status
created_at
published_at
```

Hindari membuat index yang tidak memiliki manfaat terhadap query.

---

# 🔒 Data Integrity

Gunakan:

- Primary Key
- Foreign Key
- Unique Constraint
- Check Constraint

untuk menjaga konsistensi data.

Validasi pada aplikasi tidak menggantikan validasi pada database.

---

# 🔐 Security Guidelines

Database tidak boleh menyimpan:

- password plaintext,
- access token aktif,
- credential pihak ketiga,
- API key dalam bentuk terbuka.

Password wajib menggunakan algoritma hashing yang aman (Argon2id atau BCrypt).

---

# 📈 Auditability

Seluruh perubahan penting harus dapat ditelusuri melalui Audit Log.

Minimal mencakup:

- create,
- update,
- delete,
- publish,
- approval,
- permission change.

Audit log disimpan pada tabel khusus dan tidak boleh dimodifikasi oleh pengguna biasa.

---

# 🚀 Migration Strategy

Seluruh perubahan struktur database dilakukan melalui migration.

Perubahan manual langsung pada database produksi tidak diperbolehkan kecuali untuk kebutuhan darurat yang telah disetujui.

Migration harus:

- versioned,
- reversible (jika memungkinkan),
- terdokumentasi,
- melalui proses review.

---

# 📦 Backup Compatibility

Desain database harus mendukung:

- full backup,
- incremental backup (jika digunakan),
- restore,
- disaster recovery.

Perubahan skema tidak boleh menghambat proses backup dan restore.

---

# 🧪 Validation Checklist

Sebelum implementasi tabel baru, pastikan:

- [ ] Menggunakan UUID sebagai Primary Key.
- [ ] Memiliki Foreign Key yang sesuai.
- [ ] Menggunakan snake_case.
- [ ] Memiliki created_at.
- [ ] Memiliki updated_at.
- [ ] Soft delete dipertimbangkan jika diperlukan.
- [ ] Constraint telah ditentukan.
- [ ] Index telah dianalisis.
- [ ] Mendukung audit jika diperlukan.

---

# 🔗 References

- [PRD](../PRD.md)
- [RHS Documentation](../rhs/README.md)
- [02-idr-002-technology-stack.md](./02-idr-002-technology-stack.md)
- [07-idr-007-api-design-guidelines.md](./07-idr-007-api-design-guidelines.md)
- `docs/DDS/README.md` *(Database Design Specification — Planned)*

---

# 📝 Notes

Dokumen ini hanya mendefinisikan standar desain database.

Implementasi tabel, Entity Relationship Diagram (ERD), relasi antar entitas, normalisasi spesifik, indeks aktual, dan skema database akan dijelaskan secara rinci pada **Database Design Specification (DDS)**.

Seluruh implementasi database wajib mengacu pada pedoman yang ditetapkan dalam dokumen ini.
