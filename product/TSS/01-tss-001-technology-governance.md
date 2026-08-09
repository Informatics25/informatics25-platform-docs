# TSS-001: Technology Governance

> **Technical Setup Specification (TSS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** Final — Technology Baseline
>
> **Tanggal:** 21 Juli 2026

---

# 📖 Overview

Dokumen ini mendefinisikan aturan **Technology Governance** untuk Platform Digital Informatika Angkatan 2025.

Technology Governance memastikan bahwa teknologi yang digunakan dalam implementasi tetap konsisten dengan technology baseline yang telah ditetapkan dan tidak berubah hanya berdasarkan preferensi individual.

TSS merupakan dokumen referensi resmi implementasi dan menjadi acuan bagi berbagai aktivitas teknis, termasuk Physical Database Schema, API Specification, Backend Technical Specification, Frontend Technical Specification, UI/UX Specification, Deployment Guide, CI/CD, testing, dan implementasi aplikasi.

Technology Stack Specification tidak mengubah keputusan bisnis maupun arsitektur yang telah disepakati. Technology dipilih untuk mendukung prinsip utama proyek:

* Single Source of Truth
* Security
* Maintainability
* Scalability
* Simplicity
* Reliability
* Operational Sustainability

---

# 🎯 Purpose

Technology Governance bertujuan untuk:

* Menjaga konsistensi technology baseline.
* Mencegah penambahan dependency berdasarkan preferensi personal.
* Memastikan dependency baru dievaluasi secara sistematis.
* Menjaga dependency version tetap terkontrol.
* Memisahkan pengembangan frontend dan backend melalui API contract.
* Menetapkan sumber data utama berdasarkan domain.
* Menyediakan dasar untuk perubahan technology stack yang terkontrol.

---

# 📌 Technology Baseline

Technology yang telah ditetapkan dalam TSS merupakan **baseline resmi implementasi**.

Baseline tersebut harus digunakan dalam implementasi kecuali terdapat:

1. ADR (*Architecture Decision Record*); atau
2. Perubahan resmi terhadap TSS.

Secara konseptual:

```text
Technology Baseline
        │
        ▼
Implementation
        │
        ▼
Need for Technology Change?
        │
    ┌───┴───┐
    │       │
   No      Yes
    │       │
    ▼       ▼
 Continue  ADR / Official TSS Change
                │
                ▼
          Approved Baseline
```

Perubahan technology stack tidak boleh dilakukan secara informal.

---

# 🔒 Baseline Enforcement

Technology baseline harus diperlakukan sebagai acuan bersama bagi engineering team.

Penggunaan teknologi alternatif tidak diperbolehkan hanya karena:

* Preferensi personal.
* Familiaritas individual.
* Kebiasaan dari project sebelumnya.
* Keinginan menambahkan library tambahan.

Apabila teknologi resmi yang telah ditetapkan sudah memenuhi kebutuhan, library tambahan tidak seharusnya ditambahkan hanya berdasarkan preferensi personal.

---

# 📦 Dependency Governance

Setiap dependency baru harus dievaluasi sebelum ditambahkan ke project.

Evaluasi minimal mempertimbangkan:

| Evaluation Area      | Consideration                                   |
| -------------------- | ----------------------------------------------- |
| Security             | Risiko keamanan dependency                      |
| Maintenance Activity | Aktivitas maintenance dan keberlanjutan project |
| License              | Kesesuaian lisensi                              |
| Bundle Size          | Dampak terhadap ukuran aplikasi/bundle          |
| Compatibility        | Kompatibilitas dengan technology stack          |
| Operational Cost     | Dampak terhadap biaya operasional               |

Dependency baru harus memberikan manfaat yang jelas dan tidak sekadar menambah kompleksitas.

---

# 🧩 Dependency Selection Principle

Penambahan dependency harus mempertimbangkan apakah kebutuhan tersebut sebenarnya telah dapat dipenuhi oleh teknologi yang sudah menjadi bagian dari baseline.

Secara konseptual:

```text
New Requirement
       │
       ▼
Existing Baseline Technology
       │
       ├── Sufficient ──► Use Existing Technology
       │
       └── Insufficient
               │
               ▼
        Evaluate New Dependency
               │
               ▼
      Security / Maintenance /
      License / Bundle / Compatibility /
      Operational Cost
               │
               ▼
            Decision
```

Tujuannya adalah menjaga dependency tetap terkendali dan menghindari unnecessary complexity.

---

# 🔢 Dependency Version Governance

Versi dependency harus dikunci menggunakan **lockfile**.

Lockfile berfungsi memastikan environment dan dependency resolution tetap konsisten.

```text
Dependency Definition
        │
        ▼
Version Resolution
        │
        ▼
Lockfile
        │
        ▼
Reproducible Installation
```

Dependency tidak boleh diperbarui secara sembarangan.

Upgrade dependency harus dilakukan secara terkontrol dengan mempertimbangkan dampaknya terhadap compatibility dan implementation.

---

# 🔄 Dependency Update

Dependency update harus mempertimbangkan:

* Perubahan major version.
* Compatibility.
* Security update.
* Maintenance status.
* Dampak terhadap application behaviour.
* Dampak terhadap technology baseline.

Upgrade yang memiliki dampak signifikan harus melalui review yang sesuai.

---

# 🌐 Frontend & Backend Independence

Frontend dan backend dikembangkan secara independen.

Keduanya berkomunikasi melalui **API contract yang terdokumentasi**.

```text
┌─────────────────────┐
│      Frontend       │
│  Nuxt / TypeScript  │
└──────────┬──────────┘
           │
           │ API Contract
           ▼
┌─────────────────────┐
│       Backend       │
│       Go / Echo     │
└─────────────────────┘
```

Independensi ini tidak berarti frontend dan backend boleh berkembang tanpa koordinasi.

Perubahan terhadap API contract harus dikelola melalui dokumentasi dan governance yang sesuai.

---

# 🔌 API Contract as Boundary

API contract menjadi boundary komunikasi antara frontend dan backend.

Prinsipnya:

* Frontend tidak bergantung pada implementasi internal backend.
* Backend tidak bergantung pada implementasi internal frontend.
* Komunikasi dilakukan melalui contract yang terdokumentasi.
* Perubahan contract harus dipertimbangkan dampaknya terhadap consumer.

Detail mengenai API technology dan contract akan dibahas pada dokumen TSS yang relevan serta dokumentasi API Specification.

---

# 🗄️ Data Source Governance

TSS menetapkan:

* **PostgreSQL** sebagai sumber data utama untuk domain relational/transactional.
* **Object Storage** sebagai sumber data utama untuk binary files.

Secara konseptual:

```text
Application
    │
    ├───────────────┐
    │               │
    ▼               ▼
PostgreSQL      Object Storage
    │               │
    ▼               ▼
Relational      Binary Files
Data / Metadata
```

Database dan object storage memiliki tanggung jawab yang berbeda.

Binary file tidak seharusnya diperlakukan sama dengan transactional relational data.

---

# 🧭 Technology Responsibility

Technology Governance membedakan tanggung jawab masing-masing teknologi berdasarkan domain penggunaannya.

| Area           | Primary Responsibility                                           |
| -------------- | ---------------------------------------------------------------- |
| Frontend       | Web application dan user interface                               |
| Backend        | Business logic, authentication, authorization, workflow, dan API |
| PostgreSQL     | Relational dan transactional data                                |
| Object Storage | Binary files dan media                                           |
| API            | Communication contract                                           |
| DevOps         | Reproducible environment dan automation                          |
| Security       | Identity, session, dan administrative access                     |
| Observability  | Reliability dan operational visibility                           |
| Testing        | Verification                                                     |

Detail teknologi masing-masing area dibahas pada dokumen TSS berikutnya.

---

# 🚫 Prohibited Technology Changes

Perubahan berikut tidak boleh dilakukan tanpa governance yang sesuai:

* Mengganti framework utama tanpa review.
* Menambahkan library yang sebenarnya tidak diperlukan.
* Mengganti database utama tanpa evaluasi.
* Mengubah storage strategy tanpa mempertimbangkan data architecture.
* Mengubah API technology tanpa mempertimbangkan contract.
* Menambahkan dependency besar tanpa evaluasi.
* Mengubah dependency version secara tidak terkontrol.

---

# 🔄 Technology Change Governance

Apabila terdapat kebutuhan untuk mengubah technology baseline:

```text
Technology Change Request
          │
          ▼
Impact Evaluation
          │
          ▼
Technology Decision
          │
          ▼
ADR / Official TSS Change
          │
          ▼
Approved Baseline
          │
          ▼
Implementation
```

Perubahan resmi harus tercermin dalam dokumentasi technology baseline.

---

# 🧱 Governance Principles

Technology Governance mengikuti prinsip berikut:

### 1. Baseline First

Gunakan teknologi yang telah ditetapkan sebelum mempertimbangkan teknologi baru.

### 2. Minimize Unnecessary Dependencies

Dependency baru harus memiliki alasan yang jelas.

### 3. Evaluate Before Adoption

Technology dan dependency baru harus dievaluasi sebelum digunakan.

### 4. Lock Versions

Dependency version harus dikunci melalui lockfile.

### 5. Controlled Updates

Dependency update dilakukan secara terkontrol.

### 6. Contract-Based Integration

Frontend dan backend berkomunikasi melalui API contract yang terdokumentasi.

### 7. Domain-Appropriate Data Storage

PostgreSQL dan object storage digunakan sesuai karakteristik data masing-masing.

---

# 🚧 Governance Constraints

Implementasi harus mematuhi constraint berikut:

* Technology baseline wajib digunakan kecuali terdapat ADR atau perubahan resmi terhadap TSS.
* Library tambahan tidak boleh ditambahkan hanya karena preferensi personal apabila technology resmi sudah memenuhi kebutuhan.
* Dependency baru harus dievaluasi.
* Dependency version harus dikunci melalui lockfile.
* Dependency update harus dilakukan secara terkontrol.
* Frontend dan backend harus berkomunikasi melalui API contract yang terdokumentasi.
* PostgreSQL menjadi sumber data utama untuk relational data.
* Object storage menjadi sumber data utama untuk binary files.

---

# 🔗 Relationship with Other TSS Documents

Technology Governance menjadi dasar bagi seluruh dokumen TSS lainnya.

```text
01 Technology Governance
          │
          ├────────► 02 Frontend Stack
          │
          ├────────► 03 Backend & Database Stack
          │
          ├────────► 04 Infrastructure, Security & Observability
          │
          ├────────► 05 Development, Testing & Coding Standards
          │
          └────────► 06 Technology Decision & Final Baseline
```

---

# 📚 Related Documents

## Project Documentation

* [PRD](../PRD.md)
* [RHS README](../rhs/README.md)
* [IDR README](../IDR/README.md)
* [SDS README](../SDS/README.md)
* [DDS README](../DDS/README.md)
* [AHS README](../AHS/README.md)

## TSS Documents

* [TSS README](./README.md)
* [Frontend Technology Stack](./02-tss-002-frontend-technology-stack.md)
* [Backend & Database Stack](./03-tss-003-backend-and-database-stack.md)
* [Infrastructure, Security & Observability](./04-tss-004-infrastructure-security-and-observability.md)
* [Development, Testing & Coding Standards](./05-tss-005-development-testing-and-coding-standards.md)
* [Technology Decision & Final Baseline](./06-tss-006-technology-decision-and-final-baseline.md)

---

# 🔄 Traceability Matrix

| Governance Area               | Source / Related Documentation |
| ----------------------------- | ------------------------------ |
| Technology Baseline           | TSS                            |
| Technology Decision           | TSS                            |
| Dependency Governance         | TSS                            |
| Version Governance            | TSS                            |
| Frontend/Backend Boundary     | SDS / TSS                      |
| Database Responsibility       | DDS / TSS                      |
| Object Storage Responsibility | AHS / TSS                      |
| API Contract                  | AHS / API Specification        |
| Implementation                | IDR / TSS                      |

---

# ✅ Review Checklist

* [ ] Technology baseline telah didefinisikan.
* [ ] Aturan penggunaan baseline telah ditentukan.
* [ ] Dependency governance telah ditentukan.
* [ ] Dependency evaluation criteria telah ditentukan.
* [ ] Version locking telah ditentukan.
* [ ] Dependency update governance telah ditentukan.
* [ ] Frontend/backend independence telah ditentukan.
* [ ] API contract boundary telah ditentukan.
* [ ] PostgreSQL responsibility telah ditentukan.
* [ ] Object storage responsibility telah ditentukan.
* [ ] Technology change governance telah ditentukan.
* [ ] Governance constraints telah didokumentasikan.
* [ ] Traceability telah ditentukan.

---

# 📝 Revision History

| Version | Date       | Description                                 | Author          |
| ------- | ---------- | ------------------------------------------- | --------------- |
| 1.0     | 2026-07-21 | Initial Technology Governance documentation | Abidzar Dzakwan |
