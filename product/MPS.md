# 📘 Master Project Specification (MPS)

**Platform Digital Informatika Angkatan 2025**

> **Dokumen Induk Resmi / Single Source of Truth**

---

## 📋 Document Metadata

| Atribut              | Nilai                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------- |
| **Status**           | FINAL — Referensi Induk Implementasi                                                      |
| **Bahasa**           | Bahasa Indonesia                                                                          |
| **Cakupan**          | Produk, requirement, arsitektur, keamanan, data, teknologi, operasi, QA, dan implementasi |
| **Basis**            | Product Discovery, PRD, RHS, IDR, SDS, AHS, DDS Final, dan TSS                            |
| **Tanggal**          | 21 Juli 2026                                                                              |
| **Aturan Prioritas** | Keputusan paling baru dan paling final menjadi acuan apabila terdapat perbedaan historis  |

Dokumen ini menyatukan keputusan final proyek ke dalam satu spesifikasi induk. Dokumen ini bukan sekadar penggabungan dokumen sumber; isi telah dinormalisasi untuk menghilangkan pengulangan, diskusi, opsi yang tidak dipilih, dan keputusan lama yang telah digantikan.

---

## 📑 Daftar Isi

1. [Executive Summary](#1-executive-summary)
2. [Product Vision](#2-product-vision)
3. [Product Goals](#3-product-goals)
4. [Success Metrics](#4-success-metrics)
5. [Product Principles](#5-product-principles)
6. [Stakeholders](#6-stakeholders)
7. [User Roles](#7-user-roles)
8. [RBAC Summary](#8-rbac-summary)
9. [MVP Scope](#9-mvp-scope)
10. [Future Scope](#10-future-scope)
11. [User Journey Summary](#11-user-journey-summary)
12. [Business Rules](#12-business-rules)
13. [Information Rules](#13-information-rules)
14. [Schedule Rules](#14-schedule-rules)
15. [Knowledge Hub Rules](#15-knowledge-hub-rules)
16. [Gallery Rules](#16-gallery-rules)
17. [Event Rules](#17-event-rules)
18. [Notification Rules](#18-notification-rules)
19. [Authentication](#19-authentication)
20. [Authorization](#20-authorization)
21. [Security Summary](#21-security-summary)
22. [Privacy Summary](#22-privacy-summary)
23. [Governance](#23-governance)
24. [Operational Ownership](#24-operational-ownership)
25. [High-Level Architecture](#25-high-level-architecture)
26. [Technology Stack Summary](#26-technology-stack-summary)
27. [Database Summary](#27-database-summary)
28. [External Integrations](#28-external-integrations)
29. [Non-Functional Requirements](#29-non-functional-requirements)
30. [Coding Standards Summary](#30-coding-standards-summary)
31. [Design Principles](#31-design-principles)
32. [Constraints](#32-constraints)
33. [Assumptions](#33-assumptions)
34. [Risks](#34-risks)
35. [Open Decisions](#35-open-decisions)
36. [Future Roadmap](#36-future-roadmap)
37. [AI Implementation Guidelines](#ai-implementation-guidelines)
38. [Lampiran A — Hierarki Referensi Dokumen](#lampiran-a--hierarki-referensi-dokumen)
39. [Lampiran B — Quick Reference](#lampiran-b--quick-reference)

---

# 1. Executive Summary

Platform Digital Informatika Angkatan 2025 adalah platform digital internal yang berfungsi sebagai pusat informasi resmi, jadwal akademik, Knowledge Hub, dan dokumentasi kegiatan bagi Mahasiswa Informatika Angkatan 2025.

Masalah utama yang diselesaikan adalah penyebaran informasi melalui banyak kanal yang tidak terpusat, sehingga mahasiswa dapat terlambat menerima informasi, menemukan informasi yang berbeda, atau kesulitan mencari kembali informasi yang telah disampaikan.

Produk ini menjadikan platform sebagai **Single Source of Truth**. Informasi resmi yang tampil di platform harus telah diverifikasi sesuai jenis informasinya. WhatsApp berfungsi sebagai kanal notifikasi pendukung, bukan sebagai sumber informasi resmi dan bukan dependency wajib untuk peluncuran MVP.

MVP memprioritaskan fondasi yang stabil dan sederhana:

* autentikasi;
* dashboard;
* pengumuman resmi;
* jadwal dengan exception;
* Knowledge Hub;
* administrasi;
* keamanan;
* auditability;
* backup;
* dokumentasi;
* keberlanjutan operasional.

Event dan Gallery merupakan bagian dari produk tetapi tidak bersifat launch-blocking apabila perlu dirilis setelah core MVP.

Platform dirancang sebagai aset digital bersama Angkatan Informatika 2025. Pengembang awal berperan sebagai steward/pengelola awal, bukan pemilik mutlak. Struktur governance dan dokumentasi harus memungkinkan serah terima kepada pengelola berikutnya.

---

# 2. Product Vision

Menjadi satu pusat digital yang terpercaya, sederhana, dan berkelanjutan bagi Mahasiswa Informatika Angkatan 2025 untuk memperoleh informasi resmi, memahami hal yang penting, mengakses pengetahuan, dan menjaga arsip digital angkatan.

Visi utama:

> Mahasiswa tidak perlu mencari informasi resmi di banyak tempat. Ketika membutuhkan informasi yang benar dan relevan, mereka dapat membuka satu platform yang menjadi referensi utama.

---

# 3. Product Goals

1. Menjadikan platform sebagai sumber informasi resmi utama bagi mahasiswa Informatika Angkatan 2025.
2. Menyampaikan informasi yang benar, penting, dan relevan pada waktu yang tepat.
3. Menyediakan jadwal kuliah yang mencerminkan jadwal reguler dan perubahan aktual.
4. Membangun Knowledge Hub yang terkurasi, dapat ditelusuri, dan memiliki atribusi.
5. Menyediakan fondasi operasional yang dapat bertahan melampaui pengembang pertama.
6. Menjaga kesederhanaan, keamanan, maintainability, dan efisiensi biaya.
7. Menyiapkan fondasi agar platform dapat berkembang mendukung alumni dan angkatan lain di masa depan tanpa mengubah fondasi secara besar.

---

# 4. Success Metrics

| Kategori               | Indikator                                                        | Makna Keberhasilan                                              |   |
| ---------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------- | - |
| Utama                  | Platform menjadi sumber informasi pertama yang dirujuk mahasiswa | Perubahan kebiasaan memperoleh informasi                        |   |
| Adopsi                 | Mayoritas mahasiswa mengaktifkan akun                            | Onboarding berhasil                                             |   |
| Engagement             | Mahasiswa mengakses platform secara rutin                        | Platform menjadi bagian dari aktivitas                          |   |
| Information Delivery   | Informasi resmi penting dibaca/diakses                           | Informasi tidak hanya dipublikasikan, tetapi sampai ke pengguna |   |
| Schedule               | Jadwal dan perubahan jadwal digunakan                            | Nilai operasional harian tercapai                               |   |
| Knowledge Hub          | Resource diunggah, disetujui, dan diakses                        | Knowledge sharing berjalan                                      |   |
| Operational Efficiency | Pengelola merasa distribusi informasi lebih efisien              | Masalah fragmentasi kanal berkurang                             |   |
| Reliability            | Target availability MVP 99% per bulan                            | Platform dapat diandalkan                                       |   |

---

# 5. Product Principles

1. **Single Source of Truth** — informasi resmi harus memiliki sumber kanonik.
2. **Trust First** — lebih baik terlambat beberapa menit tetapi benar daripada cepat tetapi salah.
3. **Community Contribution** — mahasiswa dapat berkontribusi pada Knowledge Hub dan dokumentasi dengan akuntabilitas.
4. **Scalable Governance** — sistem dan aset harus dapat diteruskan melalui governance yang jelas.
5. **Security by Default** — keamanan dan least privilege menjadi baseline.
6. **Data Minimization** — kumpulkan data pribadi yang benar-benar diperlukan.
7. **Stability over Feature Count** — stabilitas, keamanan, dan reliability didahulukan dari fitur baru.
8. **Platform First, External Channel Second** — integrasi eksternal mendukung, tetapi tidak menjadi fondasi.

---

# 6. Stakeholders

| Stakeholder                         | Peran/Kepentingan                                                                 |   |
| ----------------------------------- | --------------------------------------------------------------------------------- | - |
| Mahasiswa Informatika Angkatan 2025 | Pengguna utama dan penerima manfaat                                               |   |
| Administrator                       | Pengelola operasional harian                                                      |   |
| Superadmin                          | Pengelola dengan kendali penuh dan pemegang tanggung jawab operasional MVP        |   |
| Pengurus Angkatan                   | Sumber informasi kegiatan dan pihak governance                                    |   |
| Dosen                               | Sumber informasi akademik yang dapat diteruskan melalui platform                  |   |
| Program Studi                       | Stakeholder eksternal yang berkepentingan terhadap kredibilitas dan citra positif |   |
| Pengelola Masa Depan                | Penerima serah-terima aset, dokumentasi, dan tanggung jawab                       |   |

---

# 7. User Roles

| Role              | Deskripsi                                                                                                                          |   |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------- | - |
| **Guest**         | Pengunjung area publik; melihat landing page dan konten publik yang disetujui                                                      |   |
| **Mahasiswa**     | Pengguna internal; mengakses dashboard, informasi resmi, jadwal, Knowledge Hub, serta kontribusi resource dan galeri sesuai aturan |   |
| **Administrator** | Menjalankan operasional harian dengan hak terbatas                                                                                 |   |
| **Superadmin**    | Memiliki kendali penuh atas operasional, administrasi, konfigurasi, governance, dan aset digital                                   |   |

---

# 8. RBAC Summary

| Kemampuan                        | Guest | Mahasiswa | Administrator |                 Superadmin |
| -------------------------------- | ----: | --------: | ------------: | -------------------------: |
| Landing page publik              |     ✓ |         ✓ |             ✓ |                          ✓ |
| Dashboard internal               |     — |         ✓ |             ✓ |                          ✓ |
| Membaca informasi resmi internal |     — |         ✓ |             ✓ |                          ✓ |
| Membuat/mengelola pengumuman     |     — |         — |             ✓ |                          ✓ |
| Mengelola jadwal                 |     — |         — |             ✓ |                          ✓ |
| Upload resource/galeri           |     — |         ✓ |             ✓ |                          ✓ |
| Approval resource/galeri         |     — |         — |             ✓ |                          ✓ |
| Mengirim notifikasi WhatsApp     |     — |         — |             ✓ |                          ✓ |
| Mengelola akun pengguna          |     — |         — |      Terbatas |                      Penuh |
| Reset password                   |     — |         — |             — |                          ✓ |
| Mengubah konfigurasi inti        |     — |         — |             — |                          ✓ |
| Mengelola aset digital           |     — |         — |             — |                          ✓ |
| Mengubah policy/governance       |     — |         — |             — | Dengan governance angkatan |

**Prinsip penegakan:** permission wajib ditegakkan di backend. UI hanya merefleksikan permission dan tidak boleh dianggap sebagai kontrol keamanan.

---

# 9. MVP Scope

## 9.1 Launch-Blocking Core

* Authentication dan onboarding akun.
* Dashboard yang memprioritaskan informasi penting dan menampilkan jadwal hari ini.
* Official Information/Announcement.
* Schedule dengan jadwal reguler dan schedule exception.
* Knowledge Hub dengan upload, metadata, approval, dan akses resource.
* Admin Dashboard untuk Administrator dan Superadmin.
* Security baseline:

  * password policy;
  * RBAC;
  * TOTP untuk akun administratif;
  * rate limiting;
  * audit log.
* Backup, restore procedure, dokumentasi teknis, dokumentasi operasional, dan mekanisme handover.

## 9.2 Product Features yang Dapat Dirilis Setelah Core MVP

* Event.
* Gallery.
* Integrasi WhatsApp sebagai enhancement apabila provider dan biaya tersedia.

## 9.3 Out of Scope MVP

* Manajemen deadline tugas sebagai modul khusus.
* Manajemen jadwal ujian sebagai modul khusus.
* Self-service password reset.
* Video hosting langsung.
* Multi-angkatan sebagai fitur operasional penuh.
* Fitur alumni yang kompleks.
* Registrasi event, daftar peserta, sertifikat, dan absensi.
* 2FA wajib untuk akun mahasiswa.
* Informasi belum terverifikasi sebagai informasi resmi.

---

# 10. Future Scope

* Alumni sebagai status lifecycle dengan akses arsip yang sesuai.
* Self-service password reset melalui kanal verifikasi yang lebih otomatis.
* Pembatasan akses berdasarkan kelas, peminatan, atau kelompok.
* Multi-angkatan.
* Fitur event yang lebih lengkap.
* Video melalui provider eksternal.
* Integrasi eksternal tambahan dengan prinsip resilience.
* Analytics yang lebih matang untuk product measurement.
* Penguatan governance dan mekanisme pergantian pengelola.

---

# 11. User Journey Summary

1. Mahasiswa menerima akun yang dibuat oleh pengelola dengan username NIM dan password sementara acak.
2. Mahasiswa melakukan login pertama.
3. Sistem mewajibkan penggantian password.
4. Mahasiswa melengkapi data profil wajib dan opsional.
5. Mahasiswa diarahkan ke dashboard.
6. Dashboard menampilkan hal yang penting; jika tidak ada informasi penting baru, fokus utama adalah jadwal hari ini.
7. Mahasiswa membuka detail informasi, jadwal, Knowledge Hub, event, atau dokumentasi sesuai kebutuhan.
8. Mahasiswa dapat berkontribusi resource/galeri sesuai workflow approval.
9. Pengelola memverifikasi dan mengelola konten.
10. Informasi resmi dapat didistribusikan melalui kanal pendukung tanpa mengubah platform sebagai sumber kanonik.

---

# 12. Business Rules

1. Informasi resmi hanya boleh dipublikasikan setelah verifikasi sesuai jenis sumbernya.
2. Pada MVP, keputusan akhir kelayakan publikasi berada pada Superadmin; Administrator dapat menjalankan operasional sesuai permission.
3. Informasi dari sumber sangat terpercaya dapat diproses cepat, tetapi informasi yang belum cukup terverifikasi tidak boleh tampil sebagai informasi resmi.
4. Informasi memiliki lifecycle: aktif, berakhir otomatis, permanen sampai diperbarui/dicabut, atau diarsipkan.
5. Mahasiswa dapat mengakses seluruh informasi internal yang telah dipublikasikan pada MVP.
6. Versi terbaru adalah tampilan utama; riwayat perubahan disimpan untuk audit/internal traceability.
7. Data mahasiswa dan aset digital diperlakukan sebagai bagian dari governance angkatan, bukan aset pribadi permanen pengembang.
8. Perubahan policy mendasar melibatkan Superadmin dan tim pengelola/perwakilan angkatan.

---

# 13. Information Rules

1. Jenis utama MVP: pengumuman resmi, jadwal, event, dan Knowledge Hub.
2. Pengumuman wajib memiliki sumber/publisher dan metadata waktu publikasi serta pembaruan.
3. Informasi penting tetap terlihat sampai masa berlaku berakhir, dengan prioritas visual yang dapat menurun setelah dibaca.
4. Setelah berakhir, informasi masuk riwayat/arsip sesuai lifecycle.
5. Informasi darurat yang belum terverifikasi tidak boleh dipublikasikan sebagai informasi resmi.
6. WhatsApp, bila tersedia, hanya memberi tahu bahwa terdapat informasi penting atau perubahan; detail resmi tetap berada di platform.

---

# 14. Schedule Rules

1. Jadwal reguler menjadi baseline.
2. Perubahan jadwal dimodelkan sebagai exception terhadap jadwal reguler, bukan overwrite permanen.
3. Exception dapat menangani:

   * perubahan jam;
   * ruangan;
   * pembatalan;
   * kuliah pengganti.
4. Perubahan yang berdampak langsung pada aktivitas mahasiswa harus diprioritaskan untuk notifikasi pendukung bila integrasi tersedia.
5. Riwayat perubahan jadwal harus dapat ditelusuri.
6. Konflik exception harus diselesaikan secara deterministik sesuai aturan implementasi final: exception yang lebih spesifik/berlaku pada waktu yang sama tidak boleh menghasilkan dua jadwal aktif yang ambigu; konflik harus ditolak atau memerlukan resolusi administratif sesuai aturan IDR.

---

# 15. Knowledge Hub Rules

1. Resource harus memiliki metadata minimal:

   * judul;
   * kontributor;
   * mata kuliah;
   * kategori;
   * tanggal unggah;
   * atribusi/sumber bila relevan.
2. Upload anonim tidak diperbolehkan pada MVP.
3. Resource melalui lifecycle draft/pending review, approved, rejected, published, dan status terkait sesuai baseline RHS/DDS.
4. Sebelum disetujui, kontributor dapat mengubah atau menghapus resource.
5. Setelah dipublikasikan, kontributor tidak dapat mengubah/menghapus langsung; permintaan perubahan/penarikan ditinjau pengelola.
6. Resource publik hanya dapat ditampilkan jika telah melalui approval.
7. Duplicate detection menggunakan fingerprint/hash sebagai kontrol kualitas; resource yang terdeteksi duplikat ditangani sesuai aturan implementasi IDR.
8. Metadata PostgreSQL adalah sumber kebenaran; object storage menyimpan binary file. Konsistensi keduanya harus dijaga melalui lifecycle dan reconciliation.

---

# 16. Gallery Rules

1. Mahasiswa dapat mengunggah dokumentasi sesuai aturan platform.
2. Publikasi memerlukan approval Administrator atau Superadmin.
3. Konten publik harus menghormati:

   * privasi;
   * hak cipta;
   * permintaan pengurangan identitas.
4. Permintaan penghapusan identitas diprioritaskan melalui anonimisasi/pengurangan identitas bila memungkinkan sebelum penghapusan arsip penuh.

---

# 17. Event Rules

1. Event MVP minimal memuat:

   * daftar kegiatan;
   * detail;
   * tanggal;
   * waktu;
   * lokasi;
   * deskripsi;
   * gambar/poster.
2. Registrasi, daftar peserta, sertifikat, dan absensi bukan bagian dari core MVP.
3. Event publik harus melalui approval apabila ditampilkan di area publik.

---

# 18. Notification Rules

1. Platform adalah sumber informasi resmi.
2. WhatsApp tidak wajib untuk launch MVP.
3. Jika tersedia, WhatsApp menjadi kanal notifikasi pendukung.
4. Kegagalan WhatsApp tidak boleh menyebabkan informasi hilang atau menghambat fungsi inti.
5. Notifikasi gagal dapat masuk antrean retry sesuai policy IDR, dengan batas retry dan dead-letter/failure handling.
6. Informasi yang belum terverifikasi tidak boleh dikirim sebagai informasi resmi melalui WhatsApp.

---

# 19. Authentication

1. Username mahasiswa menggunakan NIM.
2. Password awal dihasilkan acak dan bersifat sementara.
3. Login pertama wajib mengganti password.
4. Password minimal 12 karakter sesuai IDR.
5. Password disimpan sebagai hash menggunakan Argon2id.
6. Access token dan refresh token menggunakan baseline JWT sesuai TSS.
7. Administrator dan Superadmin wajib menggunakan TOTP 2FA.
8. Backup code digunakan untuk pemulihan 2FA.
9. Jika backup code hilang, pemulihan dilakukan melalui verifikasi yang ditetapkan; pemulihan akun Superadmin memerlukan prosedur tambahan yang tidak bergantung pada satu orang.

---

# 20. Authorization

1. RBAC adalah baseline authorization.
2. Evaluasi permission dilakukan server-side pada setiap operasi terlindungi.
3. Administrator menerapkan least privilege dan tidak dapat mengubah:

   * Superadmin;
   * konfigurasi inti;
   * aset digital;
   * policy.
4. Superadmin memiliki kendali penuh operasional dan administrasi.
5. Data pribadi sensitif tidak otomatis terlihat oleh Administrator; visibility mengikuti kebutuhan tugas.
6. Alumni tetap memiliki akun dan dapat login untuk arsip sesuai policy; hak kontribusi baru dapat dibatasi.

---

# 21. Security Summary

* Password hashing: **Argon2id**.
* TOTP 2FA wajib untuk akun administratif.
* Rate limiting untuk endpoint sensitif.
* CSRF, XSS, dan CORS ditangani sesuai pola aplikasi dan deployment.
* Audit log untuk tindakan administratif dan keamanan penting.
* Least privilege dan akun administratif personal; akun bersama tidak digunakan.
* Secret tidak disimpan dalam source code.
* Validasi server-side wajib untuk aturan integritas, keamanan, upload, dan permission.
* File upload harus divalidasi tipe, ukuran, quota, dan status lifecycle.

---

# 22. Privacy Summary

* Data minimization.
* Default visibility profil adalah private/opt-in.
* Data yang dapat ditampilkan antar mahasiswa harus mengikuti pilihan pemilik.
* NIM ditampilkan masked kepada mahasiswa lain; Superadmin dapat melihat penuh untuk administrasi.
* Password, autentikasi, dan riwayat login bukan data publik.
* Alumni dapat meminta penghapusan data pribadi tertentu.
* Kontribusi historis dapat dipertahankan sebagai arsip dengan privacy-preserving traceability.
* Dokumentasi publik harus memperhatikan permintaan anonimisasi, hak cipta, dan alasan hukum.

---

# 23. Governance

1. Platform dipandang sebagai aset digital bersama Angkatan Informatika 2025.
2. Pengembang awal berperan sebagai steward/pengelola awal.
3. Domain, hosting, database, GitHub Organization, source code, data, dan aset terkait harus dapat dikelola secara kolektif.
4. Perubahan Superadmin dilakukan melalui mekanisme pengurus/perwakilan angkatan dan proses serah terima terdokumentasi.
5. Jika Superadmin dan Administrator tidak tersedia, pengurus inti/forum perwakilan angkatan menunjuk pengelola baru.
6. Perubahan policy mendasar melibatkan governance angkatan.

---

# 24. Operational Ownership

| Area                | MVP                                    | Jangka Panjang                             |
| ------------------- | -------------------------------------- | ------------------------------------------ |
| Operasional harian  | Superadmin dan Administrator           | Tim pengelola resmi                        |
| Aset digital        | Dikelola Superadmin sebagai steward    | Kepemilikan/governance kolektif            |
| Backup dan recovery | Tanggung jawab operasional pengelola   | Proses terdokumentasi dan diserahterimakan |
| Dokumentasi         | Wajib tersedia sejak MVP               | Menjadi handover package                   |
| Biaya               | Awalnya ditanggung pengembang          | Dialihkan melalui mekanisme resmi angkatan |
| Continuity          | Administrator dan dokumentasi cadangan | Regenerasi pengelola                       |

---

# 25. High-Level Architecture

Arsitektur menggunakan **modular monolith dengan batas modul yang eksplisit**.

Sistem terdiri atas:

* frontend web;
* backend application;
* PostgreSQL sebagai sumber data relasional;
* object storage untuk binary file;
* integrasi eksternal yang bersifat pendukung.

## Architecture Layers

| Layer                      | Teknologi / Komponen                                                                                                                    |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Presentation Layer         | Frontend Nuxt 4                                                                                                                         |
| Application/API Layer      | Backend Go dengan Echo                                                                                                                  |
| Domain/Application Modules | IAM, Official Information, Dashboard, Schedule, Knowledge Hub, Event, Gallery, Notification, Audit, Analytics/Observability, Governance |
| Data Layer                 | PostgreSQL untuk data kanonik dan metadata                                                                                              |
| Object Storage Layer       | S3-compatible object storage untuk file                                                                                                 |
| External Integration Layer | WhatsApp provider, GitHub bila digunakan, object storage provider, monitoring/analytics, hosting, dan domain                            |

Dependency direction harus menjaga domain/application tidak bergantung langsung pada detail provider eksternal.

Integrasi eksternal harus dapat gagal tanpa menjatuhkan fungsi inti.

---

# 26. Technology Stack Summary

| Lapisan            | Teknologi Final                       | Peran                         |
| ------------------ | ------------------------------------- | ----------------------------- |
| Frontend Framework | Nuxt 4                                | Web application dan rendering |
| Language           | TypeScript                            | Type safety                   |
| Styling            | Tailwind CSS v4                       | Styling utility-first         |
| UI                 | Nuxt UI                               | Komponen UI konsisten         |
| State              | Pinia                                 | State client                  |
| Validation         | VeeValidate + Zod                     | Form dan schema validation    |
| HTTP               | `$fetch` / ofetch                     | Komunikasi REST               |
| Icons/Font         | Lucide / Geist                        | UI consistency                |
| Backend            | Go + Echo                             | REST application backend      |
| Auth               | JWT Access + Refresh Token            | Authentication session model  |
| Database           | PostgreSQL                            | Relational source of truth    |
| DB Access          | SQLC                                  | Type-safe query access        |
| Storage            | S3-compatible Object Storage          | Binary files                  |
| API                | REST + OpenAPI 3.1                    | Contract dan integrasi        |
| Container          | Docker + Docker Compose               | Reproducible runtime          |
| CI/CD              | GitHub Actions                        | Automation                    |
| Security           | Argon2id + TOTP + RBAC                | Security baseline             |
| Observability      | Structured logging + health + metrics | Operational visibility        |
| Testing            | Unit + Integration + E2E              | Quality assurance             |

---

# 27. Database Summary

1. PostgreSQL menjadi sumber kebenaran untuk data inti, relasi, lifecycle, permission-related records, metadata resource, audit, dan data operasional.
2. UUID digunakan sebagai primary key baseline.
3. Foreign key dan constraint digunakan untuk menjaga integritas.
4. Business records menggunakan soft delete sesuai aturan domain.
5. Metadata file disimpan di PostgreSQL; binary file berada di object storage.
6. Audit table dilindungi dan memiliki retensi minimal sesuai kebijakan.
7. Account lifecycle mencakup `INVITED`, `ACTIVE`, `SUSPENDED`, dan `ALUMNI`.
8. Official information memiliki lifecycle termasuk `DRAFT`, `PUBLISHED`, `UPDATED`, `EXPIRED`, dan `ARCHIVED`.
9. Resource memiliki lifecycle sesuai RHS/DDS, termasuk status review dan publikasi.
10. Migration dilakukan berurutan dan dapat direproduksi.
11. Data retention harus membedakan data operasional, audit, dan data pribadi.

---

# 28. External Integrations

| Integrasi               | Status                  | Aturan                                                    |
| ----------------------- | ----------------------- | --------------------------------------------------------- |
| WhatsApp Provider       | Opsional untuk MVP      | Kanal notifikasi; failure tidak memengaruhi core platform |
| Object Storage Provider | Dependency storage      | Binary file; metadata kanonik tetap di PostgreSQL         |
| GitHub                  | Pendukung               | Platform tetap berjalan saat GitHub tidak tersedia        |
| Analytics/Monitoring    | Pendukung operasional   | Kumpulkan data secukupnya untuk product dan reliability   |
| Hosting/Infrastructure  | Dependency operasional  | Harus mendukung target availability dan recovery          |
| Domain Provider         | Dependency akses publik | Aset harus dapat ditransfer melalui governance            |

---

# 29. Non-Functional Requirements

| Area                    | Target / Rule                                                                                     |
| ----------------------- | ------------------------------------------------------------------------------------------------- |
| Availability            | Target 99% uptime per bulan untuk MVP                                                             |
| Recovery                | Gangguan serius ditargetkan pulih maksimal 24 jam; gangguan biasa diupayakan dalam hitungan jam   |
| RPO                     | Maksimal kehilangan data 24 jam terakhir                                                          |
| Performance             | Halaman utama dan dashboard sekitar ≤2 detik pada koneksi mobile normal sebagai target awal       |
| Security                | Admin TOTP, Argon2id, RBAC, least privilege, rate limiting, auditability                          |
| Scalability             | Awal sekitar 150–250 mahasiswa; arsitektur dapat berkembang ke lebih banyak pengguna/angkatan     |
| Maintainability         | Modular monolith, dokumentasi, coding standards, migration discipline                             |
| Resilience              | WhatsApp/GitHub/object storage failure tidak boleh merusak core platform secara tidak perlu       |
| Cost                    | Ideal Rp100.000–Rp200.000/bulan; maksimum realistis sekitar Rp300.000/bulan untuk MVP             |
| Accessibility/Usability | Mobile-first, responsif, sederhana, konsisten; aksesibilitas harus menjadi baseline desain dan QA |
| Observability           | Structured logs, health checks, metrics, alerting untuk kegagalan penting                         |
| Backup                  | Backup berkala dan prosedur restore yang terdokumentasi serta diuji                               |

---

# 30. Coding Standards Summary

1. Gunakan naming convention konsisten sesuai bahasa dan layer.
2. Struktur folder harus mengikuti batas modul dan dependency direction.
3. Branching menggunakan strategi yang konsisten dengan repository governance.
4. Commit mengikuti **Conventional Commits**.
5. Pull request harus memiliki scope jelas, test evidence, dan review.
6. Perubahan keamanan, permission, migration, dan business rule wajib mendapat perhatian review khusus.
7. Dokumentasi teknis harus diperbarui ketika perubahan memengaruhi operasi atau handover.

---

# 31. Design Principles

1. Mobile-first untuk pengalaman mahasiswa.
2. Dashboard menjawab pertanyaan: **“Apa yang penting?”**
3. Jika tidak ada informasi penting baru, jadwal hari ini menjadi fokus.
4. Informasi terbaru ditampilkan tanpa membanjiri pengguna dengan seluruh histori.
5. Admin UI harus mencerminkan permission, bukan menyembunyikan kontrol sebagai satu-satunya security mechanism.
6. Public branding dipisahkan secara jelas dari internal information system.
7. Desain harus mendukung trust, provenance, status, dan freshness informasi.
8. Kompleksitas UI dan arsitektur harus sebanding dengan kebutuhan MVP.

---

# 32. Constraints

1. Target pengguna awal terbatas pada satu angkatan.
2. Biaya operasional terbatas.
3. WhatsApp API tidak wajib dan provider dapat mengalami gangguan.
4. Video tidak disimpan langsung pada MVP.
5. Pengembang awal merupakan titik operasional penting sehingga handover wajib dirancang sejak awal.
6. Data pribadi harus diminimalkan.
7. Platform tidak boleh menjadi sumber informasi resmi yang belum diverifikasi.
8. Fitur baru tidak boleh mengorbankan stabilitas core platform.

---

# 33. Assumptions

1. Jumlah pengguna awal berada pada kisaran 150–250 mahasiswa, namun angka riil dapat berbeda.
2. Provider object storage, hosting, WhatsApp, dan monitoring dapat dipilih sesuai TSS/AHS tanpa mengubah kontrak arsitektur.
3. Mahasiswa memiliki akses internet mobile yang memadai untuk penggunaan normal.
4. Informasi akademik dapat diperoleh dari sumber resmi yang dapat diverifikasi.
5. Pengurus/perwakilan angkatan tersedia untuk governance dan succession.
6. Dokumentasi dan akses infrastruktur dapat diserahkan kepada pengelola berikutnya.
7. Detail physical schema, API contract, deployment topology, dan UI specification mengikuti dokumen teknis turunan dari MPS.

---

# 34. Risks

| Risiko                             | Dampak                     | Mitigasi                                                   |
| ---------------------------------- | -------------------------- | ---------------------------------------------------------- |
| Ketergantungan pada satu pengelola | Operasional berhenti       | Administrator, handover, dokumentasi, governance           |
| Informasi salah                    | Hilangnya kepercayaan      | Verification workflow dan Single Source of Truth           |
| Kompromi akun admin                | Akses data/operasi kritis  | TOTP, least privilege, audit log                           |
| Kegagalan object storage           | File tidak dapat diakses   | Metadata lifecycle, reconciliation, backup/restore         |
| Kegagalan WhatsApp                 | Notifikasi tidak terkirim  | Platform tetap menjadi sumber; retry policy                |
| Pertumbuhan scope                  | MVP terlambat/kompleks     | Launch-blocking scope dan stability-first                  |
| Biaya meningkat                    | Tidak berkelanjutan        | Managed service selektif, storage policy, monitoring biaya |
| Kehilangan data                    | Gangguan arsip dan operasi | Backup, RPO 24 jam, restore procedure                      |
| Privasi dokumentasi                | Keluhan/penghapusan        | Opt-in visibility, minimization, anonymization             |
| Provider lock-in                   | Migrasi sulit              | S3-compatible storage dan dependency abstraction           |

---

# 35. Open Decisions

Tidak terdapat open decision bisnis atau arsitektur inti yang boleh mengubah keputusan final dalam MPS.

Keputusan implementasi spesifik yang masih dapat ditentukan pada dokumen turunan tanpa mengubah baseline MPS meliputi pemilihan provider konkret untuk:

* hosting;
* object storage;
* WhatsApp;
* monitoring;
* analytics.

Keputusan tersebut harus tetap mematuhi kontrak teknologi, keamanan, biaya, resilience, dan governance.

Jika keputusan turunan tersebut mengubah:

* technology baseline;
* architecture decision;
* database decision;
* business rule;
* permission;

maka harus dibuat perubahan resmi terhadap dokumen induk melalui mekanisme governance/ADR.

---

# 36. Future Roadmap

| Tahap          | Fokus                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------- |
| **MVP**        | Core information system, schedule, Knowledge Hub, auth, RBAC, security, backup, audit, operations |
| **Post-MVP 1** | Event dan Gallery lengkap serta penguatan analytics/UX                                            |
| **Post-MVP 2** | Alumni lifecycle dan arsip yang lebih kaya                                                        |
| **Post-MVP 3** | Self-service recovery, segmentasi akses, dan fitur kolaborasi                                     |
| **Scale**      | Multi-angkatan, governance lebih formal, dan integrasi eksternal tambahan                         |

---

# AI Implementation Guidelines

> **Bagian ini bersifat normatif dan WAJIB dipatuhi oleh AI Coding Assistant, AI UI Generator, AI Design Assistant, dan AI lain yang menghasilkan artefak untuk proyek.**

Dokumen ini adalah **Single Source of Truth** untuk keputusan proyek.

AI tidak boleh:

1. Mengubah Product Vision.
2. Mengubah Product Goals.
3. Mengubah MVP Scope.
4. Mengubah Business Rules.
5. Mengubah Governance.
6. Mengubah Workflow.
7. Mengubah User Journey.
8. Mengubah Permission.
9. Mengubah Security Policy.
10. Mengubah Technology Stack.
11. Mengubah Architecture Decision.
12. Mengubah Database Decision.
13. Mengganti teknologi yang telah ditetapkan tanpa perubahan resmi terhadap TSS dan dokumen governance terkait.
14. Membuat workflow baru yang bertentangan dengan workflow resmi.
15. Memberikan permission tambahan hanya karena lebih mudah diimplementasikan.
16. Menganggap UI hiding sebagai authorization; permission harus ditegakkan di backend.
17. Membuat informasi belum terverifikasi tampil sebagai informasi resmi.
18. Menjadikan WhatsApp atau integrasi eksternal sebagai dependency yang membuat core platform gagal.
19. Mengubah model schedule exception menjadi overwrite permanen tanpa keputusan resmi.
20. Mengubah ownership, retention, privacy, auditability, atau lifecycle data tanpa keputusan resmi.
21. Membuat asumsi baru yang memengaruhi requirement, security, architecture, atau database tanpa menandainya sebagai Open Decision.

Jika terdapat konflik antara dokumen lama dan keputusan final, gunakan keputusan paling baru dan paling final.

Setiap perubahan yang berdampak pada MPS harus dapat ditelusuri, didokumentasikan, dan melalui proses review yang sesuai.

AI harus membedakan dengan jelas antara:

* keputusan final;
* constraint;
* assumption;
* proposal baru.

Jika requirement tidak cukup untuk menghasilkan implementasi yang aman dan konsisten, AI harus menandai gap tersebut daripada mengarang keputusan bisnis.

---

# Lampiran A — Hierarki Referensi Dokumen

| Prioritas | Dokumen                                                      | Fungsi                                                                |
| --------: | ------------------------------------------------------------ | --------------------------------------------------------------------- |
|         1 | **MPS Final**                                                | Single Source of Truth lintas domain                                  |
|         2 | **Dokumen final turunan yang tidak bertentangan dengan MPS** | Detail implementasi pada domain masing-masing                         |
|         3 | **ADR/keputusan governance terbaru**                         | Perubahan resmi terhadap keputusan sebelumnya                         |
|         4 | **Dokumen historis**                                         | Referensi konteks, bukan sumber keputusan final jika telah digantikan |

---

# Lampiran B — Quick Reference

| Area                  | Keputusan Final                                                                                 |
| --------------------- | ----------------------------------------------------------------------------------------------- |
| **Produk**            | Pusat informasi resmi, Knowledge Hub, jadwal, dokumentasi angkatan                              |
| **Prioritas**         | Single Source of Truth                                                                          |
| **MVP**               | Auth, dashboard, announcement, schedule exception, Knowledge Hub, admin, security, backup, docs |
| **Roles**             | Guest, Mahasiswa, Administrator, Superadmin                                                     |
| **Architecture**      | Modular monolith dengan batas modul eksplisit                                                   |
| **Frontend**          | Nuxt 4 + TypeScript + Tailwind CSS v4 + Nuxt UI + Pinia                                         |
| **Backend**           | Go + Echo                                                                                       |
| **Database**          | PostgreSQL + SQLC                                                                               |
| **Storage**           | S3-compatible Object Storage + metadata PostgreSQL                                              |
| **API**               | REST + OpenAPI 3.1                                                                              |
| **Auth**              | JWT Access/Refresh + Argon2id; TOTP admin                                                       |
| **Authorization**     | RBAC + least privilege + server-side enforcement                                                |
| **Availability**      | 99% per bulan                                                                                   |
| **RPO**               | Maksimal kehilangan data 24 jam                                                                 |
| **Recovery**          | Target pemulihan maksimal 24 jam untuk kegagalan serius                                         |
| **Cost**              | Ideal Rp100k–Rp200k/bulan; maksimum realistis sekitar Rp300k/bulan                              |
| **Governance**        | Aset dipandang sebagai aset digital bersama angkatan                                            |
| **Handover**          | Dokumentasi, backup, inventaris aset, dan proses serah-terima wajib                             |
| **External Services** | Pendukung; kegagalannya tidak boleh menjatuhkan core platform                                   |

---

# 📌 Final Document Statement

**MASTER PROJECT SPECIFICATION — Platform Digital Informatika Angkatan 2025**

**Status:** FINAL — Referensi Induk Implementasi

**Tanggal:** 21 Juli 2026

Dokumen ini menjadi referensi induk dan Single Source of Truth lintas domain untuk keputusan final proyek.
