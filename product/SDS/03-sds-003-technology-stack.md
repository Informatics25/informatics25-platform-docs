# SDS-003: Technology Stack

> **Software Design Specification (SDS)**
>
> **Reference:** PRD, RHS, IDR
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini mendefinisikan teknologi yang digunakan pada Platform Digital Informatika Angkatan 2025 beserta alasan pemilihannya.

Pemilihan teknologi dilakukan berdasarkan kebutuhan MVP, kemudahan pengembangan, maintainability, performa, keamanan, dan keberlanjutan proyek dalam jangka panjang.

Dokumen ini tidak membahas implementasi teknis setiap teknologi, melainkan menjadi baseline keputusan teknologi yang digunakan pada Software Design Specification Tahap 1.

---

# 🎯 Objectives

Technology Stack bertujuan untuk:

- Menetapkan standar teknologi proyek.
- Menjaga konsistensi implementasi.
- Mengurangi kompleksitas pemilihan teknologi selama pengembangan.
- Mendukung maintainability dan scalability.
- Menjadi referensi bagi seluruh Engineering Team.

---

# 📦 Scope

Dokumen ini mencakup:

- Frontend Technology
- Backend Technology
- Database
- Object Storage
- Authentication
- Infrastructure
- Development Tools
- Deployment
- Technology Decision Matrix

---

# 🖥️ Frontend Technology

| Component | Technology | Purpose |
|-----------|------------|---------|
| Framework | Nuxt.js | Full-stack web framework berbasis Vue.js. |
| UI Framework | Vue.js | Membangun antarmuka pengguna yang reaktif. |
| Styling | Tailwind CSS | Utility-first CSS framework untuk pengembangan antarmuka. |
| State Management | Pinia | Pengelolaan state aplikasi. |
| HTTP Client | Native Fetch API | Komunikasi antara frontend dan backend. |

---

# ⚙️ Backend Technology

| Component | Technology | Purpose |
|-----------|------------|---------|
| Language | Go (Golang) | Bahasa pemrograman utama backend. |
| Architecture | RESTful API | Komunikasi antara client dan server. |
| Authentication | JWT | Mekanisme autentikasi berbasis token. |

---

# 🗄️ Database

| Component | Technology | Purpose |
|-----------|------------|---------|
| Database | PostgreSQL | Penyimpanan data utama aplikasi. |
| ORM / Query Builder | Sesuai implementasi | Abstraksi akses database apabila diperlukan. |

---

# 📂 File Storage

| Component | Technology | Purpose |
|-----------|------------|---------|
| Object Storage | S3-Compatible Object Storage | Penyimpanan file dan media. |

---

# 🔐 Security Technology

| Component | Technology | Purpose |
|-----------|------------|---------|
| Password Hashing | Argon2id / BCrypt | Penyimpanan password secara aman. |
| HTTPS | TLS | Enkripsi komunikasi client dan server. |
| Authorization | RBAC | Pengelolaan hak akses pengguna. |

---

# 🚀 Development Tools

| Component | Technology | Purpose |
|-----------|------------|---------|
| Version Control | Git | Manajemen source code. |
| Repository | GitHub | Kolaborasi dan penyimpanan repository. |
| Containerization | Docker | Konsistensi environment pengembangan dan deployment. |

---

# 🌐 Infrastructure

| Component | Purpose |
|-----------|---------|
| VPS / Cloud Server | Menjalankan backend dan layanan aplikasi. |
| Reverse Proxy | Mengelola akses HTTP/HTTPS. |
| Object Storage | Menyimpan file statis dan media. |

---

# 📊 Technology Decision Matrix

| Technology | Selected | Primary Consideration |
|------------|----------|-----------------------|
| Nuxt.js | ✅ | SSR, struktur proyek, produktivitas pengembangan. |
| Vue.js | ✅ | Reaktif, mudah dipelajari, ekosistem matang. |
| Tailwind CSS | ✅ | Konsistensi UI dan efisiensi pengembangan. |
| Go | ✅ | Performa tinggi, concurrency, deployment sederhana. |
| PostgreSQL | ✅ | Relational database yang stabil dan andal. |
| Docker | ✅ | Konsistensi environment lintas pengembang. |
| GitHub | ✅ | Version control dan kolaborasi proyek. |

---

# ⚖️ Technology Selection Principles

Pemilihan teknologi mempertimbangkan prinsip berikut:

- Stabil dan telah digunakan secara luas.
- Memiliki dokumentasi yang baik.
- Mendukung kebutuhan MVP.
- Mudah dipelihara.
- Memiliki komunitas yang aktif.
- Mendukung pengembangan jangka panjang.

---

# 🔄 Relationship with Other Documents

Technology Stack menjadi dasar implementasi pada dokumen berikutnya.

```text
SDS-002 Architecture Principles
              │
              ▼
SDS-003 Technology Stack
              │
              ├──────── SDS-004 High-Level Architecture
              ├──────── IDR-002 Technology Stack
              ├──────── IDR-012 Deployment Strategy
              └──────── IDR-013 CI/CD Pipeline
```

---

# 📚 Related Documents

## Previous Documents

- SDS-001: System Overview
- SDS-002: Architecture Principles

## Related IDR

- IDR-002: Technology Stack
- IDR-012: Deployment Strategy
- IDR-013: CI/CD Pipeline
- IDR-015: Security Architecture

---

# ✅ Review Checklist

- [ ] Seluruh teknologi utama telah didefinisikan.
- [ ] Pemilihan teknologi konsisten dengan Architecture Principles.
- [ ] Technology Decision Matrix telah diperbarui.
- [ ] Tidak terdapat teknologi yang bertentangan dengan ruang lingkup MVP.
- [ ] Seluruh Engineering Team menggunakan baseline teknologi yang sama.

---

# 🔄 Traceability Matrix

| Technology Area | Related Document |
|-----------------|------------------|
| Frontend | IDR-002 Technology Stack |
| Backend | IDR-002 Technology Stack |
| Database | IDR-008 Database Design Guidelines |
| Authentication | IDR-009 Authentication Architecture |
| File Storage | IDR-010 File Storage Strategy |
| Deployment | IDR-012 Deployment Strategy |
| CI/CD | IDR-013 CI/CD Pipeline |
| Security | IDR-015 Security Architecture |

---

# 🔗 References

## Product Documentation

- [Product Requirements Document (PRD)](../PRD.md)
- [Requirements Hardening Specification (RHS)](../rhs/README.md)
- [Implementation Detail Records (IDR)](../IDR/README.md)

## Related SDS

- [SDS README](./README.md)
- [SDS-002: Architecture Principles](./02-sds-002-architecture-principles.md)
- [SDS-004: High-Level Architecture](./04-sds-004-high-level-architecture.md)

## Related IDR

- [IDR-002: Technology Stack](../IDR/02-idr-002-technology-stack.md)
- [IDR-012: Deployment Strategy](../IDR/12-idr-012-deployment-strategy.md)
- [IDR-013: CI/CD Pipeline](../IDR/13-idr-013-ci-cd-pipeline.md)
- [IDR-015: Security Architecture](../IDR/15-idr-015-security-architecture.md)

---

# 📝 Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-08-02 | Initial Technology Stack documentation |
