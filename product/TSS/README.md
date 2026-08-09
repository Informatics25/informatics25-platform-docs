# ⚙️ Technical Setup Specification (TSS)

**Platform Digital Informatika Angkatan 2025**
*Technical Setup Specification*

---

# 📌 Overview

Dokumentasi **Technical Setup Specification (TSS)** mendefinisikan technology baseline yang digunakan dalam pengembangan dan operasional Platform Digital Informatika Angkatan 2025.

TSS mendokumentasikan:

* Technology Governance
* Frontend Technology Stack
* Backend Technology Stack
* Database Technology Stack
* Storage Technology
* API Technology Stack
* DevOps Technology Stack
* Security Technology Stack
* Monitoring & Observability
* Testing Technology Stack
* Coding Standards
* Technology Decision Matrix
* Final Technology Baseline

TSS berfungsi sebagai acuan teknis untuk memastikan teknologi yang digunakan dalam implementasi tetap konsisten dengan keputusan arsitektur yang telah ditetapkan.

---

# 🎯 Purpose

Technical Setup Specification bertujuan untuk:

* Mendokumentasikan technology baseline sistem.
* Menetapkan teknologi utama yang digunakan dalam implementasi.
* Menjelaskan aturan penggunaan dan governance teknologi.
* Menjadi referensi bagi engineering team.
* Menjaga konsistensi antara architecture design dan implementation.
* Mendokumentasikan alasan pemilihan teknologi dan trade-off yang relevan.
* Menjadi referensi terhadap perubahan technology stack di masa mendatang.

---

# 📦 Scope

TSS mencakup technology setup dan baseline untuk:

```text
Technology Governance
        │
        ├── Frontend
        │
        ├── Backend
        │
        ├── Database
        │
        ├── Storage
        │
        ├── API
        │
        ├── DevOps
        │
        ├── Security
        │
        ├── Observability
        │
        └── Testing & Development
```

TSS tidak menggantikan:

* PRD untuk product requirements.
* RHS untuk requirement hardening.
* IDR untuk implementation detail.
* SDS untuk software architecture.
* DDS untuk database design.
* AHS untuk architecture hardening.

---

# 🧭 Technology Baseline Principle

Technology stack yang telah ditetapkan dalam TSS merupakan **baseline teknologi resmi** untuk implementasi.

Perubahan terhadap technology baseline tidak dilakukan secara informal.

Perubahan yang signifikan harus melalui review dan governance yang sesuai sebelum digunakan sebagai bagian dari implementation baseline.

Secara konseptual:

```text
Technology Baseline
        │
        ▼
Implementation
        │
        ▼
Technology Change?
        │
   ┌────┴────┐
   │         │
  No        Yes
   │         │
   ▼         ▼
Continue   Review
              │
              ▼
           Approval
              │
              ▼
      Baseline Update
```

---

# 📂 Document Structure

| No | Document                                                                                              | Description                                                                                        |
| -- | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 1  | [Technology Governance](./01-tss-001-technology-governance.md)                                        | Aturan governance, baseline, dependency, dan perubahan technology stack.                           |
| 2  | [Frontend Technology Stack](./02-tss-002-frontend-technology-stack.md)                                | Teknologi dan tooling utama yang digunakan pada frontend.                                          |
| 3  | [Backend & Database Stack](./03-tss-003-backend-and-database-stack.md)                                | Teknologi backend dan database yang digunakan sebagai core application stack.                      |
| 4  | [Infrastructure, Security & Observability](./04-tss-004-infrastructure-security-and-observability.md) | Infrastructure, storage, API, DevOps, security, monitoring, dan observability technology baseline. |
| 5  | [Development, Testing & Coding Standards](./05-tss-005-development-testing-and-coding-standards.md)   | Technology untuk testing serta standar development dan coding.                                     |
| 6  | [Technology Decision & Final Baseline](./06-tss-006-technology-decision-and-final-baseline.md)        | Technology decision matrix, final stack reference, dan final technology baseline.                  |

---

# 🏗️ Technology Stack Overview

Technology baseline sistem dikelompokkan menjadi:

| Layer               | Primary Technology              |
| ------------------- | ------------------------------- |
| Frontend            | Nuxt 4                          |
| Language            | TypeScript                      |
| UI                  | Tailwind CSS v4, Nuxt UI        |
| State Management    | Pinia                           |
| Validation          | VeeValidate + Zod               |
| Backend             | Go                              |
| Backend Framework   | Echo                            |
| Authentication      | JWT + Refresh Token             |
| Authorization       | RBAC                            |
| Database            | PostgreSQL                      |
| Database Access     | SQLC                            |
| Migration           | Database Migration              |
| Object Storage      | S3-compatible Object Storage    |
| API                 | REST API                        |
| API Documentation   | OpenAPI 3.1                     |
| Containerization    | Docker                          |
| Local Orchestration | Docker Compose                  |
| CI/CD               | GitHub Actions                  |
| Password Hashing    | Argon2id                        |
| MFA                 | TOTP                            |
| Monitoring          | Logging, Health Check & Metrics |
| Testing             | Unit, Integration & E2E Testing |

> Detail penggunaan, alasan pemilihan, trade-off, dan konfigurasi masing-masing teknologi dijelaskan pada dokumen TSS terkait.

---

# 🔗 Technology Governance

Technology governance memastikan bahwa technology stack tidak berkembang secara tidak terkendali.

Governance mencakup:

* Technology baseline.
* Dependency management.
* Version management.
* Configuration management.
* Technology change.
* Technology review.
* Documentation consistency.

Setiap perubahan teknologi yang memiliki dampak terhadap architecture atau implementation baseline harus ditinjau terhadap dokumentasi terkait.

---

# 🔄 Relationship with Other Documents

TSS merupakan bagian dari dokumentasi teknis proyek:

```text
PRD
 │
 ▼
RHS
 │
 ▼
IDR
 │
 ▼
SDS
 │
 ▼
DDS
 │
 ▼
AHS
 │
 ▼
TSS
 │
 ▼
Implementation
```

Hubungan utama:

* **PRD** → kebutuhan dan tujuan produk.
* **RHS** → requirement yang telah di-hardening.
* **IDR** → implementation decisions.
* **SDS** → software architecture.
* **DDS** → database design.
* **AHS** → architecture hardening.
* **TSS** → technology setup dan technology baseline.
* **Implementation** → penerapan seluruh keputusan dan baseline tersebut.

---

# 🔍 Traceability

Technology decision harus dapat ditelusuri kembali ke architecture dan requirement yang relevan.

```text
Business Need
      │
      ▼
PRD
      │
      ▼
RHS
      │
      ▼
SDS / AHS
      │
      ▼
TSS
      │
      ▼
Implementation
```

Apabila technology choice menyebabkan perubahan terhadap architecture atau requirement, dokumentasi terkait harus ditinjau kembali.

---

# 🧩 Technology Change Governance

Technology baseline dapat berubah apabila terdapat alasan yang valid.

Contoh alasan:

* Technology tidak lagi memenuhi kebutuhan.
* Security concern.
* Compatibility issue.
* Maintenance concern.
* Performance requirement.
* Operational requirement.
* Dependency issue.

Perubahan harus mempertimbangkan:

* Architecture impact.
* Implementation impact.
* Security impact.
* Operational impact.
* Migration effort.
* Documentation impact.

---

# 📊 Document Status

| Status    | Description                           |
| --------- | ------------------------------------- |
| 🔴 Draft  | Masih dalam penyusunan                |
| 🟡 Review | Sedang dalam proses review            |
| ✅ Final   | Disetujui sebagai technology baseline |

**Current Status:** 🟡 Review

---

# 📝 Revision History

| Date       | Version | Description                     | Author          |
| ---------- | ------- | ------------------------------- | --------------- |
| 2026-08-08 | 1.0     | Initial TSS documentation index | Abidzar Dzakwan |

---

# 📚 References

## Product Documentation

* [Product Requirements Document (PRD)](../PRD.md)
* [Requirements Hardening Specification (RHS)](../rhs/README.md)

## Implementation Documentation

* [Implementation Detail Records (IDR)](../IDR/README.md)

## Design Documentation

* [Software Design Specification (SDS)](../SDS/README.md)
* [Database Design Specification (DDS)](../DDS/README.md)
* [Architecture Hardening Specification (AHS)](../AHS/README.md)

---

# 🗂️ TSS Directory

```text
TSS/
│
├── README.md
│
├── 01-tss-001-technology-governance.md
├── 02-tss-002-frontend-technology-stack.md
├── 03-tss-003-backend-and-database-stack.md
├── 04-tss-004-infrastructure-security-and-observability.md
├── 05-tss-005-development-testing-and-coding-standards.md
└── 06-tss-006-technology-decision-and-final-baseline.md
```

---

# 📖 Recommended Reading Order

Untuk memahami technology baseline secara bertahap:

1. **Technology Governance**
2. **Frontend Technology Stack**
3. **Backend & Database Stack**
4. **Infrastructure, Security & Observability**
5. **Development, Testing & Coding Standards**
6. **Technology Decision & Final Baseline**

Dengan urutan tersebut, pembaca memahami terlebih dahulu aturan governance, kemudian technology stack, infrastructure concerns, development standards, dan akhirnya keputusan serta baseline teknologi final.

---

# ✅ Completion Criteria

TSS dianggap selesai apabila:

* [ ] Technology governance telah didokumentasikan.
* [ ] Frontend technology stack telah didokumentasikan.
* [ ] Backend technology stack telah didokumentasikan.
* [ ] Database technology stack telah didokumentasikan.
* [ ] Storage technology telah didokumentasikan.
* [ ] API technology stack telah didokumentasikan.
* [ ] DevOps technology stack telah didokumentasikan.
* [ ] Security technology stack telah didokumentasikan.
* [ ] Monitoring & observability telah didokumentasikan.
* [ ] Testing technology stack telah didokumentasikan.
* [ ] Coding standards telah didokumentasikan.
* [ ] Technology decision matrix telah didokumentasikan.
* [ ] Final technology baseline telah ditetapkan.
* [ ] Seluruh dokumen TSS memiliki traceability yang sesuai.

---

# 🔖 Final Statement

Technical Setup Specification menjadi **technology baseline** bagi implementasi Platform Digital Informatika Angkatan 2025.

Seluruh implementation harus mengacu pada technology baseline yang telah ditetapkan dalam dokumentasi TSS.

Perubahan terhadap baseline harus melalui proses review dan governance yang sesuai untuk menjaga konsistensi antara architecture, technology, dan implementation.
