# IDR-002: Technology Stack

> **Implementation Detail Record (IDR)**
>
> **Reference:** PRD v1.0 • RHS Baseline • IDR-001 Project Architecture
>
> **Status:** 🟢 Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini mendefinisikan teknologi yang digunakan pada Platform Digital Informatika Angkatan 2025 beserta alasan pemilihannya.

Technology Stack menjadi acuan resmi seluruh proses pengembangan agar implementasi tetap konsisten, mudah dipelihara, dan tidak menghasilkan variasi teknologi yang tidak diperlukan.

Dokumen ini menjelaskan **apa yang digunakan** dan **mengapa dipilih**, bukan cara instalasi maupun konfigurasi teknis.

---

# 🎯 Objectives

- Menetapkan technology stack resmi proyek.
- Menjaga konsistensi implementasi antar engineer.
- Mengurangi technical debt akibat penggunaan teknologi yang tidak terstandarisasi.
- Menjadi referensi untuk seluruh dokumen implementasi berikutnya.

---

# 🏛️ Technology Selection Principles

Pemilihan teknologi mengikuti prinsip berikut.

| Principle | Description |
|-----------|-------------|
| Stability First | Mengutamakan teknologi yang stabil dan memiliki dukungan komunitas yang baik. |
| Long-Term Support | Memprioritaskan versi LTS atau versi yang telah matang. |
| Open Source | Mengutamakan teknologi open-source yang aktif dikembangkan. |
| Maintainability | Mudah dipelihara dan dipahami oleh pengembang berikutnya. |
| Scalability | Mampu berkembang seiring bertambahnya kebutuhan sistem. |
| Security | Memiliki praktik keamanan yang baik dan pembaruan yang aktif. |

---

# 🖥️ Frontend Stack

| Technology | Purpose |
|------------|---------|
| Vue 3 | Frontend Framework |
| Vite | Build Tool |
| TypeScript | Static Typing |
| Pinia | State Management |
| Vue Router | Routing |
| Tailwind CSS v4 | Styling |
| Axios / Fetch API | HTTP Client |
| VueUse | Utility Composition |
| Lucide Icons | Icon Library |

---

# ⚙️ Backend Stack

| Technology | Purpose |
|------------|---------|
| Golang | Backend Application |
| Gin / Fiber *(sesuai keputusan proyek)* | REST API Framework |
| GORM / SQL Driver | Database Access |
| JWT / Session | Authentication |
| Validator | Request Validation |

---

# 🗄️ Database Stack

| Technology | Purpose |
|------------|---------|
| PostgreSQL | Primary Relational Database |
| UUID | Primary Identifier |
| SQL Migration Tool | Schema Migration |

---

# 📦 Object Storage

| Technology | Purpose |
|------------|---------|
| S3 Compatible Storage | Resource Storage |
| Local Storage *(Development Only)* | Local Development |

Object Storage digunakan untuk menyimpan:

- Resource
- Gallery
- Images
- Poster
- Attachment

Database tidak digunakan sebagai media penyimpanan file.

---

# 🌐 External Services

| Service | Purpose |
|---------|---------|
| WhatsApp API | Notification |
| GitHub | Source Code Management |
| GitHub Actions | CI/CD |
| Email Service *(Future)* | Email Notification |

Seluruh integrasi eksternal harus bersifat **loosely coupled** sehingga kegagalan layanan tidak menghentikan fungsi utama platform.

---

# 🧪 Development Tools

| Tool | Purpose |
|------|---------|
| Git | Version Control |
| GitHub | Repository Hosting |
| Docker | Development Environment |
| Postman / Bruno | API Testing |
| Markdown | Documentation |

---

# 📂 Package Management

| Environment | Tool |
|------------|------|
| Frontend | npm |
| Backend | Go Modules |

Dependency harus dikelola menggunakan package manager resmi masing-masing platform.

---

# 🔐 Security Dependencies

Teknologi keamanan minimum yang digunakan:

- HTTPS
- BCrypt atau Argon2id
- JWT atau Session Authentication
- TOTP 2FA (Administrator & Superadmin)
- Rate Limiting
- Secure HTTP Headers
- CORS Configuration

---

# 📊 Version Management

Seluruh dependency mengikuti prinsip berikut.

| Rule | Description |
|------|-------------|
| Major Version | Perubahan hanya setelah evaluasi. |
| Minor Version | Dapat diperbarui setelah pengujian. |
| Patch Version | Direkomendasikan untuk menjaga keamanan dan stabilitas. |

Dependency tidak boleh diperbarui langsung pada production tanpa proses pengujian.

---

# 🔄 Dependency Management Policy

- Hindari dependency yang tidak aktif dipelihara.
- Hindari library dengan lisensi yang tidak sesuai.
- Hindari duplicate dependency.
- Dependency baru harus memiliki alasan implementasi yang jelas.
- Seluruh dependency harus tercatat pada repository.

---

# 📈 Future Technology Considerations

Teknologi berikut dapat dipertimbangkan pada fase berikutnya apabila diperlukan.

| Technology | Potential Usage |
|------------|-----------------|
| Redis | Cache & Session |
| Message Queue | Asynchronous Processing |
| Elasticsearch | Full-text Search |
| Prometheus | Metrics Collection |
| Grafana | Monitoring Dashboard |
| OpenTelemetry | Distributed Tracing |

Teknologi di atas **bukan bagian dari MVP** dan hanya diimplementasikan apabila terdapat kebutuhan yang jelas.

---

# 📋 Technology Decision Summary

| Area | Selected Technology |
|------|----------------------|
| Frontend | Vue 3 + TypeScript + Vite |
| Styling | Tailwind CSS |
| State Management | Pinia |
| Backend | Golang |
| API | REST |
| Database | PostgreSQL |
| File Storage | Object Storage |
| Authentication | JWT / Session |
| Version Control | Git + GitHub |
| CI/CD | GitHub Actions |

---

# ✅ Acceptance Criteria

- [ ] Seluruh engineer menggunakan technology stack yang telah ditetapkan.
- [ ] Tidak terdapat penggunaan framework utama di luar standar proyek.
- [ ] Dependency terdokumentasi dengan baik.
- [ ] Versi teknologi dikelola secara konsisten.
- [ ] Integrasi eksternal tidak menjadi single point of failure.
- [ ] Dokumentasi stack selalu diperbarui ketika terjadi perubahan teknologi.

---

# 🔗 Related Documents

- PRD v1.0
- RHS-009 — Security, 2FA & Password Reset
- IDR-001 — Project Architecture
- IDR-003 — Repository Structure
- SDS — Software Design Specification
- DDS — Database Design Specification

---

# 📝 Notes

Technology Stack merupakan keputusan implementasi tingkat proyek dan menjadi standar yang harus diikuti oleh seluruh tim engineering. Perubahan terhadap teknologi inti (framework, database, atau bahasa pemrograman) harus melalui proses evaluasi teknis dan pembaruan dokumentasi yang sesuai agar konsistensi arsitektur tetap terjaga.
