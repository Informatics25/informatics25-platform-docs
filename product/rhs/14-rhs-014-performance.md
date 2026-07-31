# ⚡ RHS-014: Performance, Availability & Resilience

> **Requirement Hardening Specification**
>
> **Reference:** PRD §11 – Non-Functional Requirements, PRD §12 – Backup & Recovery, PRD §16 – Deployment & Operational Requirements
>
> **Status:** ✅ Approved
>
> **Priority:** 🟠 High

---

# 📖 Overview

Dokumen ini mendefinisikan kebutuhan non-fungsional terkait performa sistem, ketersediaan layanan (*availability*), ketahanan sistem (*resilience*), serta target operasional yang harus dipenuhi oleh Platform Informatika Angkatan 2025.

Requirement ini memastikan bahwa sistem tetap responsif, stabil, dan mampu menangani gangguan tanpa mengorbankan integritas data maupun pengalaman pengguna.

---

# 🎯 Objective

- Menjamin performa aplikasi tetap responsif.
- Menetapkan target availability sistem.
- Mendefinisikan batas downtime yang dapat diterima.
- Mengurangi risiko kegagalan layanan.
- Menjadi baseline untuk pengujian performa dan operasional.

---

# 📋 Business Rules

| ID | Rule |
|----|------|
| PERF-01 | Platform harus tetap dapat digunakan oleh seluruh mahasiswa selama periode operasional normal. |
| PERF-02 | Gangguan pada layanan pendukung tidak boleh menyebabkan kegagalan fungsi inti platform. |
| PERF-03 | Dashboard harus tetap dapat diakses selama database tersedia. |
| PERF-04 | Kegagalan integrasi eksternal tidak boleh menghentikan proses bisnis utama. |
| PERF-05 | Sistem harus mendukung proses pemulihan sesuai target recovery yang telah ditetapkan. |

---

# ⚙️ Performance Requirements

## API Response Time

| Endpoint Type | Target |
|---------------|--------|
| Authentication | ≤ 2 detik |
| Dashboard | ≤ 3 detik |
| Official Information | ≤ 2 detik |
| Schedule | ≤ 2 detik |
| Knowledge Hub | ≤ 3 detik |
| Search | ≤ 5 detik |
| File Upload | Bergantung ukuran file, progress harus ditampilkan |

---

## Frontend Performance

| Requirement | Target |
|------------|--------|
| Initial Page Load | ≤ 3 detik |
| Dashboard Rendering | ≤ 2 detik |
| Route Navigation | ≤ 1 detik |
| Lazy Loading | Wajib untuk halaman besar |
| Code Splitting | Direkomendasikan |

---

## File Upload Performance

| Requirement | Value |
|------------|-------|
| Upload Progress | Harus tersedia |
| Retry Upload | Didukung |
| Resume Upload | Opsional (Future Enhancement) |
| Maximum Upload Size | Mengikuti kebijakan sistem |

---

# 🌐 Availability Requirements

| Requirement | Target |
|------------|--------|
| Target Availability MVP | ≥ 99% |
| Planned Maintenance | Di luar jam operasional jika memungkinkan |
| Downtime Notification | Harus diumumkan sebelumnya jika direncanakan |
| Graceful Shutdown | Wajib |

---

# 🔄 Resilience Requirements

| ID | Rule |
|----|------|
| RES-01 | Sistem harus tetap berjalan ketika layanan GitHub tidak tersedia. |
| RES-02 | Sistem tetap dapat berjalan ketika WhatsApp API gagal. |
| RES-03 | Kegagalan Object Storage tidak boleh menyebabkan database menjadi korup. |
| RES-04 | Error pada satu request tidak boleh memengaruhi request pengguna lain. |
| RES-05 | Timeout layanan eksternal harus ditangani dengan mekanisme retry atau fallback yang sesuai. |

---

# 🚨 Failure Handling

| Scenario | Expected Behavior |
|----------|-------------------|
| Database Restart | Sistem melakukan reconnect otomatis |
| WhatsApp API Down | Notifikasi masuk antrean (*queue*) dan dapat dicoba kembali |
| GitHub API Down | Repository tetap ditampilkan dari metadata terakhir |
| Object Storage Lambat | Upload menampilkan progres dan dapat diulang |
| Request Timeout | Mengembalikan error yang informatif tanpa menghentikan layanan |

---

# 📊 Scalability Guidelines

| Requirement | Description |
|------------|-------------|
| Horizontal Scaling | Direkomendasikan |
| Vertical Scaling | Didukung |
| Stateless API | Direkomendasikan |
| Reverse Proxy | Direkomendasikan |
| CDN | Direkomendasikan untuk aset publik |

---

# 🧪 Performance Testing

Minimal pengujian berikut harus dilakukan sebelum MVP dinyatakan siap diluncurkan.

| Test | Target |
|------|--------|
| Load Test | Lulus |
| Stress Test | Lulus |
| Smoke Test | Lulus |
| API Benchmark | Memenuhi target response time |
| Database Performance Test | Tidak ditemukan bottleneck kritis |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | Pengguna membuka dashboard | Request diproses | Dashboard dimuat sesuai target waktu |
| AC-02 | WhatsApp API gagal | Sistem mengirim notifikasi | Proses bisnis utama tetap berjalan |
| AC-03 | GitHub tidak tersedia | Repository ditampilkan | Metadata terakhir tetap tersedia |
| AC-04 | Server direstart | Aplikasi aktif kembali | Sistem dapat melayani pengguna tanpa kehilangan data |
| AC-05 | Beban normal | Pengguna mengakses sistem | Response time memenuhi target |

---

# 📈 Monitoring Requirements

Monitoring minimal harus mencakup:

- API response time
- Error rate
- CPU utilization
- Memory utilization
- Disk utilization
- Database connection pool
- Failed login rate
- Upload failure rate
- Queue length
- Application uptime

---

# 📊 Suggested Metrics

| Metric | Target |
|---------|--------|
| Error Rate | < 1% |
| API Availability | ≥ 99% |
| Failed Request | < 2% |
| Average Response Time | ≤ 2 detik |
| Database Error | Mendekati 0 |
| Queue Failure | Mendekati 0 |

---

# 🔗 Related Documents

- PRD §11 – Non-Functional Requirements
- PRD §12 – Data, Audit Log, Backup & Recovery
- PRD §13 – External Integration
- PRD §16 – Deployment & Operational Requirements
- RHS-011 – Notification & WhatsApp Resilience
- RHS-013 – Backup, Recovery & Availability
- RHS-015 – Analytics & Observability

---

# 📝 Notes

- Target performa merupakan baseline MVP dan dapat ditingkatkan pada iterasi berikutnya.
- Seluruh pengukuran harus dilakukan pada lingkungan yang mendekati kondisi produksi.
- Requirement ini menjadi acuan bagi Backend Engineer, Frontend Engineer, DevOps, dan QA selama proses implementasi serta pengujian.
