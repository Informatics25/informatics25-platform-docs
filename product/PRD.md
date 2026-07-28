# 📘 Product Requirements Document (PRD)

![Version](https://img.shields.io/badge/version-1.0-blue)
![Status](https://img.shields.io/badge/status-MVP-success)

**Platform Digital Angkatan Informatika 2025**  
*Pusat Informasi Resmi, Pembelajaran, dan Kolaborasi*

---

## Daftar Isi

1. [Ringkasan Eksekutif](#1-ringkasan-eksekutif)
2. [Latar Belakang dan Masalah](#2-latar-belakang-dan-masalah)
3. [Visi dan Prinsip Produk](#3-visi-dan-prinsip-produk)
4. [Tujuan Produk dan Definisi Keberhasilan](#4-tujuan-produk-dan-definisi-keberhasilan)
5. [Ruang Lingkup MVP](#5-ruang-lingkup-mvp)
6. [Pengguna, Stakeholder, dan Role](#6-pengguna-stakeholder-dan-role)
7. [Information Architecture dan Core User Journey](#7-information-architecture-dan-core-user-journey)
8. [Functional Requirements](#8-functional-requirements)
9. [Business Rules](#9-business-rules)
10. [Authentication, Authorization, Privacy, dan Security](#10-authentication-authorization-privacy-dan-security)
11. [Non-Functional Requirements](#11-non-functional-requirements)
12. [Data, Audit Log, Backup, dan Recovery](#12-data-audit-log-backup-dan-recovery)
13. [Integrasi Eksternal](#13-integrasi-eksternal)
14. [Governance dan Operational Continuity](#14-governance-dan-operational-continuity)
15. [Analytics dan Product Measurement](#15-analytics-dan-product-measurement)
16. [Deployment dan Operational Requirements](#16-deployment-dan-operational-requirements)
17. [Acceptance Criteria dan MVP Launch Readiness](#17-acceptance-criteria-dan-mvp-launch-readiness)
18. [Out of Scope dan Future Considerations](#18-out-of-scope-dan-future-considerations)
19. [Assumptions dan Open Decisions](#19-assumptions-dan-open-decisions)

---

## 1. Ringkasan Eksekutif

Platform Digital Angkatan Informatika 2025 adalah platform internal yang berfungsi sebagai pusat informasi akademik, pembelajaran, dan kolaborasi bagi Mahasiswa Informatika Angkatan 2025. Produk ini dibangun untuk mengatasi penyebaran informasi melalui banyak kanal yang menyebabkan informasi sulit ditemukan, berpotensi tidak konsisten, dan dapat terlewat.

**Nilai utama platform adalah menjadi Single Source of Truth:** satu tempat yang menjadi referensi utama untuk memperoleh informasi resmi yang benar, relevan, dan dapat dipercaya. Platform bukan sekadar kumpulan fitur, tetapi pusat aktivitas informasi yang membantu mahasiswa menjawab pertanyaan: *"Apa yang penting bagi saya saat ini?"*

MVP berfokus pada **Authentication**, **Dashboard**, **Announcement/Informasi Resmi**, **Schedule**, **Knowledge Hub**, serta **Admin Dashboard**. Event dan Gallery tetap dirancang sebagai bagian dari produk, tetapi bukan launch-blocking dan dapat dirilis setelah fungsi inti stabil.

---

## 2. Latar Belakang dan Masalah

### 2.1 Kondisi Saat Ini

- Informasi tersebar melalui grup WhatsApp angkatan, grup kelas, dosen, pengurus, dan komunikasi antar mahasiswa.
- Mahasiswa harus mencari informasi di banyak tempat.
- Informasi yang sama dapat memiliki versi berbeda atau tidak jelas mana yang terbaru.
- Perubahan jadwal dapat terlambat diketahui.
- Resource pembelajaran tersebar di berbagai media dan tidak memiliki katalog terpusat.
- Keberlangsungan pengelolaan informasi terlalu bergantung pada individu tertentu.

### 2.2 Masalah Utama yang Ingin Diselesaikan

Skenario utama yang ingin diselesaikan adalah: **"Apakah ada informasi atau perubahan terbaru yang perlu saya ketahui hari ini?"**

Platform harus membantu mahasiswa memperoleh jawaban tersebut dengan cepat, kemudian memungkinkan mereka mengakses detail jadwal, pengumuman, event, dan resource pembelajaran tanpa harus berpindah-pindah aplikasi.

---

## 3. Visi dan Prinsip Produk

**Platform Digital Angkatan Informatika 2025 adalah pusat informasi akademik, pembelajaran, dan kolaborasi yang menjadi sumber informasi resmi bagi seluruh mahasiswa Angkatan Informatika 2025.**

### Prinsip Produk:
1. **Kepercayaan (Trust)** - Setiap informasi yang dipublikasikan telah terverifikasi dan dapat diandalkan.
2. **Keberlanjutan (Sustainability)** - Platform dapat diwariskan dan dikelola oleh generasi berikutnya.
3. **Kesederhanaan (Simplicity)** - Fokus pada kebutuhan inti, menghindari kompleksitas yang tidak perlu.
4. **Skalabilitas (Scalability)** - Fondasi yang memungkinkan pengembangan fitur di masa depan.

---

## 4. Tujuan Produk dan Definisi Keberhasilan

### 4.1 Tujuan Produk

1. Menjadi sumber informasi resmi utama bagi Mahasiswa Informatika Angkatan 2025.
2. Mengurangi ketergantungan pada informasi yang tersebar di berbagai grup komunikasi.
3. Menyediakan akses cepat terhadap jadwal kuliah dan perubahan jadwal.
4. Menyediakan Knowledge Hub terpusat untuk resource pembelajaran.
5. Membangun fondasi platform yang dapat dipelihara dan diteruskan oleh pengelola berikutnya.

### 4.2 Indikator Keberhasilan

- Mahasiswa secara alami membuka platform ketika membutuhkan informasi resmi.
- Mayoritas mahasiswa telah mengaktifkan akun.
- Platform digunakan secara rutin untuk informasi resmi dan jadwal.
- Pengelola merasa penyampaian informasi lebih efisien dibanding hanya menggunakan grup WhatsApp.
- Knowledge Hub mulai digunakan sebagai pusat resource pembelajaran.
- Platform dapat beroperasi stabil, aman, terdokumentasi, dan dapat dipulihkan dari backup.

> **Catatan:** Keberhasilan MVP tidak diukur terutama dari jumlah fitur, tetapi dari perubahan kebiasaan pengguna dalam memperoleh informasi.

---

## 5. Ruang Lingkup MVP

### 5.1 Launch-Blocking Scope

| Modul | Deskripsi |
|-------|-----------|
| **Authentication & Account** | Login, password sementara, force change password, role-based access |
| **Dashboard** | Tampilan utama dengan informasi penting, jadwal hari ini, dan aktivitas terbaru |
| **Official Information** | Publikasi dan manajemen informasi resmi yang telah terverifikasi |
| **Schedule** | Jadwal reguler dan exception model untuk perubahan jadwal |
| **Knowledge Hub** | Upload, approval workflow, dan akses resource pembelajaran |
| **Admin Dashboard** | Manajemen konten, approval, dan pengelolaan akun sesuai role |
| **User Profile** | Pengelolaan data diri mahasiswa |

### 5.2 Non-Launch-Blocking / Dapat Dirilis Setelah Core MVP

- Event sebagai modul publikasi kegiatan
- Gallery sebagai dokumentasi yang melalui approval
- Integrasi WhatsApp API
- Fitur publikasi resource ke landing page publik
- Fitur lanjutan alumni
- Self-service password reset
- 2FA untuk akun mahasiswa

### 5.3 Out of Scope MVP

- Manajemen deadline tugas sebagai modul khusus
- Manajemen jadwal ujian sebagai modul khusus
- Pendaftaran event
- Absensi event
- Sertifikat event
- Sistem rekomendasi resource
- Video hosting langsung
- Role tambahan di luar Guest, Mahasiswa, Administrator, dan Superadmin
- Pembatasan informasi berdasarkan kelas atau kelompok
- Sistem multi-angkatan sebagai fitur operasional penuh

---

## 6. Pengguna, Stakeholder, dan Role

### 6.1 Stakeholder

| Stakeholder | Peran |
|-------------|-------|
| **Mahasiswa Informatika Angkatan 2025** | Pengguna utama dan penerima manfaat |
| **Superadmin** | Pemegang kendali penuh operasional dan administrasi |
| **Administrator** | Pengelola operasional harian |
| **Pengurus Angkatan** | Sumber informasi kegiatan dan pihak governance |
| **Dosen** | Sumber informasi akademik |
| **Program Studi** | Stakeholder eksternal yang berkepentingan terhadap kredibilitas dan citra positif |

### 6.2 Role MVP

| Role | Deskripsi | Hak Akses Utama |
|------|-----------|-----------------|
| **Guest** | Pengunjung tidak login | Akses landing page, informasi publik |
| **Mahasiswa** | Mahasiswa Angkatan 2025 yang telah login | Dashboard, informasi resmi, jadwal, Knowledge Hub, profil |
| **Administrator** | Pengelola harian | Publikasi informasi, approval resource, manajemen jadwal, manajemen event/gallery |
| **Superadmin** | Pemegang kendali penuh | Semua akses, manajemen admin, konfigurasi sistem, backup & recovery |

> **Catatan:** Role administratif menggunakan akun pribadi dengan RBAC. Akun bersama tidak digunakan untuk menjaga auditability.

---

## 7. Information Architecture dan Core User Journey

### 7.1 Area Publik
- Landing page
- Profil angkatan
- Galeri yang telah disetujui
- Event atau informasi umum yang dipilih untuk publik
- Resource publik yang telah melalui approval

### 7.2 Area Internal
- Dashboard
- Official Information
- Schedule
- Knowledge Hub
- Profile
- Notifikasi atau status informasi

### 7.3 Onboarding

1. Superadmin membuat akun menggunakan NIM sebagai username/identitas awal
2. Sistem menghasilkan password sementara secara acak dan aman
3. Mahasiswa login pertama kali
4. Mahasiswa wajib mengganti password
5. Mahasiswa melengkapi data profil wajib
6. Mahasiswa diarahkan ke dashboard

> **Onboarding dianggap berhasil setelah:** akun aktif, password diganti, profil dasar selesai, dan dashboard berhasil diakses.

---

## 8. Functional Requirements

### 8.1 Authentication & Account

| ID | Requirement |
|----|-------------|
| AUTH-01 | Sistem harus mendukung login menggunakan kredensial akun yang dibuat oleh pengelola |
| AUTH-02 | Password awal harus dihasilkan secara acak dan tidak menggunakan NIM |
| AUTH-03 | Pada login pertama, sistem wajib memaksa penggantian password |
| AUTH-04 | Password harus disimpan dalam bentuk hash, bukan plaintext |
| AUTH-05 | Mahasiswa dapat memperbarui profil yang diizinkan |
| AUTH-06 | Superadmin dapat mengelola akun mahasiswa |
| AUTH-07 | Reset password MVP dilakukan oleh Superadmin setelah verifikasi identitas |
| AUTH-08 | Password sementara hasil reset wajib diganti pada login berikutnya |

### 8.2 Dashboard

| ID | Requirement |
|----|-------------|
| DASH-01 | Dashboard harus menjawab pertanyaan utama: "Apa yang penting?" |
| DASH-02 | Menampilkan pengumuman penting yang masih aktif |
| DASH-03 | Menampilkan jadwal kuliah hari ini |
| DASH-04 | Menampilkan perubahan jadwal yang relevan |
| DASH-05 | Menampilkan aktivitas atau event terdekat apabila tersedia |
| DASH-06 | Menampilkan resource terbaru |
| DASH-07 | Menampilkan aktivitas terbaru |
| DASH-08 | Jika tidak ada informasi penting baru, fokus utama dashboard beralih ke jadwal hari ini |
| DASH-09 | Jika tidak ada jadwal hari ini, sistem dapat menampilkan jadwal terdekat atau informasi relevan lainnya |

### 8.3 Official Information / Announcement

| ID | Requirement |
|----|-------------|
| OFF-01 | Hanya informasi yang telah diverifikasi yang dapat dipublikasikan sebagai informasi resmi |
| OFF-02 | Superadmin dan Administrator dapat membuat dan mempublikasikan informasi sesuai kewenangannya |
| OFF-03 | Informasi harus memiliki judul, isi, status, prioritas, waktu publikasi, dan informasi pengelola |
| OFF-04 | Sistem harus mendukung informasi aktif, berakhir, dan permanen |
| OFF-05 | Informasi dapat memiliki tanggal mulai dan tanggal berakhir |
| OFF-06 | Informasi yang diperbarui menampilkan versi terbaru sebagai versi utama |
| OFF-07 | Riwayat perubahan disimpan dalam audit log internal |
| OFF-08 | Informasi yang diperbarui dapat diberi label "Diperbarui pada ..." |
| OFF-09 | Informasi resmi harus menampilkan metadata dasar: waktu publikasi, waktu pembaruan, dan pihak yang mempublikasikan atau memverifikasi |

### 8.4 Schedule

| ID | Requirement |
|----|-------------|
| SCH-01 | Sistem harus mendukung jadwal reguler |
| SCH-02 | Perubahan jadwal harus menggunakan Exception Model |
| SCH-03 | Exception dapat mengubah jam, ruangan, status kelas, atau detail lain yang relevan untuk tanggal/periode tertentu |
| SCH-04 | Sistem harus mendukung perubahan jam |
| SCH-05 | Sistem harus mendukung perubahan ruangan |
| SCH-06 | Sistem harus mendukung pembatalan kelas |
| SCH-07 | Sistem harus mendukung kuliah pengganti |
| SCH-08 | Jadwal terbaru harus menjadi sumber yang ditampilkan pada dashboard |
| SCH-09 | Perubahan jadwal penting dapat memicu notifikasi WhatsApp apabila integrasi tersedia |
| SCH-10 | Riwayat perubahan jadwal harus dapat diaudit secara internal |

### 8.5 Knowledge Hub

| ID | Requirement |
|----|-------------|
| KH-01 | Mahasiswa yang login dapat mengunggah resource |
| KH-02 | Resource baru masuk status Pending Review |
| KH-03 | Resource harus memiliki metadata minimum: judul, deskripsi, mata kuliah, kategori, sumber/atribusi, dan tanggal unggah |
| KH-04 | Resource dapat berupa file atau tautan eksternal |
| KH-05 | Format prioritas: PDF, DOCX, PPTX, XLSX, gambar, dan tautan |
| KH-06 | Video tidak disimpan langsung sebagai file video pada MVP |
| KH-07 | Resource yang disetujui dapat diakses oleh mahasiswa |
| KH-08 | Resource dapat dipublikasikan ke area publik hanya melalui approval |
| KH-09 | Resource yang ditolak harus memiliki alasan penolakan |
| KH-10 | Resource yang telah dipublikasikan tidak dapat diedit atau dihapus langsung oleh kontributor |
| KH-11 | Kontributor dapat mengajukan permintaan perubahan, penarikan, atau penghapusan |
| KH-12 | Identitas kontributor harus diketahui sistem, tetapi tidak harus ditampilkan secara mencolok |

### 8.6 Event

| ID | Requirement |
|----|-------------|
| EVT-01 | Mendukung daftar kegiatan |
| EVT-02 | Mendukung detail kegiatan (tanggal, waktu, lokasi, deskripsi, dan poster/gambar) |

> **Catatan:** Pendaftaran, daftar peserta, sertifikat, dan absensi berada di luar MVP.

### 8.7 Gallery

| ID | Requirement |
|----|-------------|
| GAL-01 | Mahasiswa dapat mengunggah dokumentasi apabila modul tersedia |
| GAL-02 | Konten harus melalui approval sebelum publikasi |
| GAL-03 | Publikasi ke landing page hanya untuk konten yang disetujui |
| GAL-04 | Permintaan pengurangan identitas diprioritaskan melalui anonimisasi atau pengurangan identitas jika memungkinkan |

### 8.8 Admin Dashboard

| ID | Requirement |
|----|-------------|
| ADM-01 | Mengelola informasi resmi |
| ADM-02 | Mengelola jadwal dan exception |
| ADM-03 | Melakukan approval resource |
| ADM-04 | Mengelola konten gallery/event sesuai role |
| ADM-05 | Mengelola akun sesuai kewenangan |
| ADM-06 | Melihat aktivitas audit yang relevan |
| ADM-07 | Melihat status operasional dasar |

---

## 9. Business Rules

| ID | Business Rule |
|----|---------------|
| BR-01 | Informasi resmi hanya dapat dipublikasikan setelah melalui verifikasi |
| BR-02 | Perubahan jadwal harus menggunakan exception model untuk menjaga integritas data jadwal reguler |
| BR-03 | Resource Knowledge Hub harus melalui approval workflow sebelum dapat diakses oleh mahasiswa lain |
| BR-04 | Akun administratif wajib menggunakan TOTP 2FA |
| BR-05 | Password sementara wajib diganti pada login pertama |
| BR-06 | Audit log harus disimpan untuk semua tindakan administratif kritis |
| BR-07 | Backup database harus dilakukan secara berkala dengan RPO maksimal 24 jam |
| BR-08 | Akun mahasiswa berubah menjadi Alumni setelah lulus |

---

## 10. Authentication, Authorization, Privacy, dan Security

### 10.1 RBAC

Hak akses harus ditentukan berdasarkan role dan diterapkan secara konsisten pada frontend maupun backend. Backend tetap menjadi sumber otoritatif untuk authorization.

### 10.2 Administrative Security

| ID | Requirement |
|----|-------------|
| SEC-01 | TOTP 2FA wajib untuk Superadmin dan Administrator |
| SEC-02 | Backup codes tersedia saat aktivasi 2FA |
| SEC-03 | Jika backup code hilang, pemulihan dilakukan melalui prosedur verifikasi |
| SEC-04 | Pemulihan akses Superadmin idealnya melibatkan minimal satu Administrator lain atau prosedur terkontrol |
| SEC-05 | Akun administratif harus bersifat individual |

### 10.3 Privacy

| ID | Requirement |
|----|-------------|
| PRV-01 | Data profil opsional default private |
| PRV-02 | Mahasiswa dapat memilih menampilkan foto, Instagram, LinkedIn, hobi, dan cita-cita |
| PRV-03 | Password, riwayat login, dan data autentikasi tidak boleh terlihat oleh mahasiswa lain |
| PRV-04 | Administrator hanya mengakses data pribadi yang diperlukan untuk tugasnya |
| PRV-05 | Superadmin memiliki akses lebih luas sesuai kebutuhan administrasi |
| PRV-06 | Kebijakan Privasi dan Kebijakan Penggunaan Platform tersedia sejak MVP |

### 10.4 Data Retention and Deletion

| ID | Requirement |
|----|-------------|
| RTN-01 | Akun mahasiswa berubah menjadi Alumni setelah lulus |
| RTN-02 | Alumni tetap dapat login dan mengakses arsip sesuai kebijakan |
| RTN-03 | Data pribadi tertentu dapat dihapus berdasarkan permintaan |
| RTN-04 | Kontribusi historis dapat dipertahankan sebagai arsip, dengan mempertimbangkan hukum, hak cipta, dan permintaan khusus |
| RTN-05 | Penghapusan harus dibedakan antara data pribadi, data kontribusi, dan audit log |

---

## 11. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-01 | **Performance** - Waktu muat halaman utama | < 2 detik |
| NFR-02 | **Performance** - API response time | < 500 ms |
| NFR-03 | **Availability** - Uptime platform | 99.5% |
| NFR-04 | **Scalability** - Mendukung jumlah pengguna | 200+ concurrent users |
| NFR-05 | **Security** - HTTPS enforcement | Selalu |
| NFR-06 | **Security** - Password hashing | Bcrypt / Argon2 |
| NFR-07 | **Reliability** - Backup RPO | Maksimal 24 jam |
| NFR-08 | **Reliability** - Recovery RTO | Maksimal 24 jam |
| NFR-09 | **Documentation** - Deployment guide | Tersedia |
| NFR-10 | **Documentation** - Database schema | Tersedia |
| NFR-11 | **Documentation** - API documentation | Tersedia |
| NFR-12 | **Documentation** - Operational procedures | Tersedia |

---

## 12. Data, Audit Log, Backup, dan Recovery

### 12.1 Audit Log

Event yang harus dicatat dalam audit log:

| Event | Detail |
|-------|--------|
| Login dan logout | Akun administratif |
| Perubahan role atau hak akses | Semua perubahan |
| Reset password | Termasuk inisiator |
| Publikasi, perubahan, penghapusan | Informasi resmi |
| Approval atau rejection | Resource Knowledge Hub |
| Perubahan konfigurasi penting | Sistem |
| Perubahan data jadwal penting | Exception dan update |

> **Retention:** Audit log keamanan dipertahankan minimal 1 tahun.

### 12.2 Backup

| ID | Requirement |
|----|-------------|
| BKP-01 | Backup database dilakukan secara berkala |
| BKP-02 | Backup harus mendukung proses pemulihan |
| BKP-03 | Target RPO maksimal 24 jam |
| BKP-04 | Prosedur restore harus terdokumentasi dan diuji secara berkala |
| BKP-05 | Backup harus dipisahkan dari sistem utama sejauh memungkinkan |

### 12.3 Recovery

| ID | Requirement |
|----|-------------|
| RCV-01 | Target pemulihan dari kegagalan serius adalah maksimal 24 jam |
| RCV-02 | Gangguan operasional biasa diharapkan pulih lebih cepat |

---

## 13. Integrasi Eksternal

### 13.1 WhatsApp

| ID | Requirement |
|----|-------------|
| WHA-01 | WhatsApp bukan dependency launch-blocking |
| WHA-02 | Platform tetap dapat launch tanpa WhatsApp API aktif |
| WHA-03 | WhatsApp berfungsi sebagai media distribusi/pemberitahuan |
| WHA-04 | Informasi lengkap dan sumber resmi tetap berada di platform |
| WHA-05 | Kegagalan pengiriman tidak boleh menyebabkan data informasi hilang |
| WHA-06 | Sistem idealnya mencatat status pengiriman dan menyediakan retry/queue |

### 13.2 GitHub

| ID | Requirement |
|----|-------------|
| GIT-01 | GitHub merupakan integrasi pendukung |
| GIT-02 | Gangguan GitHub tidak boleh mengganggu dashboard, announcement, schedule, atau Knowledge Hub |
| GIT-03 | Tautan atau metadata repository yang telah tersimpan dapat tetap ditampilkan |

### 13.3 Object Storage

| ID | Requirement |
|----|-------------|
| OBS-01 | File resource dan dokumentasi sebaiknya disimpan pada object storage yang sesuai |
| OBS-02 | Object storage digunakan untuk mengendalikan biaya dan memisahkan penyimpanan file dari database |

---

## 14. Governance dan Operational Continuity

### 14.1 Kepemilikan Aset

Platform dipandang sebagai **aset digital bersama** Angkatan Informatika 2025. Pengembang awal berperan sebagai steward/pengelola awal, bukan pemilik mutlak jangka panjang.

### 14.2 Superadmin

| Tanggung Jawab | Deskripsi |
|----------------|-----------|
| Kendali penuh sistem | Mengelola seluruh aspek platform |
| Mengelola Administrator | Menambah, mengubah, menghapus Administrator |
| Mengelola aset digital | Domain, infrastruktur, kode sumber |
| Menentukan kebijakan operasional | Sesuai governance |
| Mengelola akun, konten, jadwal | Tindakan administratif tingkat atas |

### 14.3 Administrator

| Tanggung Jawab | Deskripsi |
|----------------|-----------|
| Operasional harian | Menjalankan aktivitas sehari-hari |
| Publikasi informasi | Membuat dan mengelola informasi resmi |
| Pengelolaan jadwal | Memelihara jadwal dan exception |
| Approval resource dan gallery | Menyetujui atau menolak kontribusi |
| Pengiriman notifikasi | Mengirim pemberitahuan kepada mahasiswa |

> **Batasan:** Administrator tidak dapat mengubah Superadmin, aset digital, konfigurasi inti, atau kebijakan platform.

### 14.4 Succession

| ID | Requirement |
|----|-------------|
| SUC-01 | Pengelola baru ditunjuk melalui pengurus inti atau forum perwakilan angkatan yang disepakati |
| SUC-02 | Serah terima harus mencakup: akses, dokumentasi, aset digital, infrastruktur, dan prosedur operasional |
| SUC-03 | Inventaris aset digital harus dipelihara |
| SUC-04 | Platform harus dapat beroperasi meskipun pengembang awal tidak aktif |

---

## 15. Analytics dan Product Measurement

Analytics digunakan secara proporsional untuk mengukur keberhasilan produk tanpa mengumpulkan data berlebihan.

### Metrik Utama:

| Metrik | Tujuan |
|--------|--------|
| **Daily Active Users (DAU)** | Apakah mahasiswa membuka platform secara rutin |
| **Announcement read rate** | Apakah informasi resmi dibaca atau diakses |
| **Knowledge Hub usage** | Apakah Knowledge Hub digunakan |
| **Dashboard & schedule views** | Pola penggunaan dashboard dan jadwal |
| **Account activation rate** | Adopsi akun dan onboarding |

> **Catatan:** Analytics tidak ditujukan untuk mengukur WhatsApp sebagai kompetitor, karena WhatsApp berfungsi sebagai kanal distribusi pendukung.

---

## 16. Deployment dan Operational Requirements

| ID | Requirement |
|----|-------------|
| DEP-01 | Frontend dan backend dapat di-host pada infrastruktur cloud yang sesuai |
| DEP-02 | Database dapat menggunakan managed service apabila lebih andal dan efisien |
| DEP-03 | Object storage digunakan untuk file |
| DEP-04 | Backup harus tersedia |
| DEP-05 | Domain menjadi identitas resmi platform |
| DEP-06 | Dokumentasi deployment wajib tersedia |
| DEP-07 | Dokumentasi struktur database wajib tersedia |
| DEP-08 | Dokumentasi API wajib tersedia |
| DEP-09 | Dokumentasi proses operasional wajib tersedia |
| DEP-10 | Akses infrastruktur harus dapat diserahterimakan |

---

## 17. Acceptance Criteria dan MVP Launch Readiness

### 17.1 Authentication
- [ ] Akun mahasiswa dapat dibuat dan digunakan untuk login
- [ ] Password sementara tidak mudah ditebak
- [ ] Login pertama memaksa perubahan password
- [ ] Role dan permission berjalan sesuai RBAC
- [ ] TOTP wajib untuk akun administratif

### 17.2 Dashboard
- [ ] Dashboard menampilkan informasi penting aktif
- [ ] Jadwal hari ini dapat terlihat
- [ ] Jika tidak ada informasi penting baru, jadwal menjadi fokus utama
- [ ] Informasi terbaru dapat ditemukan tanpa berpindah banyak halaman

### 17.3 Official Information
- [ ] Admin berwenang dapat membuat dan mempublikasikan informasi
- [ ] Mahasiswa dapat melihat informasi resmi
- [ ] Informasi memiliki status lifecycle (aktif, berakhir, permanen)
- [ ] Perubahan dapat ditelusuri melalui audit log
- [ ] Metadata publikasi dan pembaruan tersedia

### 17.4 Schedule
- [ ] Jadwal reguler tersedia
- [ ] Exception dapat diterapkan untuk perubahan tertentu
- [ ] Perubahan jam, ruangan, pembatalan, dan kuliah pengganti dapat ditangani
- [ ] Jadwal terbaru muncul pada dashboard

### 17.5 Knowledge Hub
- [ ] Mahasiswa dapat mengunggah resource
- [ ] Resource masuk approval workflow
- [ ] Administrator dapat approve/reject
- [ ] Resource yang approved dapat diakses
- [ ] Atribusi dan metadata tersedia

### 17.6 Operational Readiness
- [ ] Backup berjalan
- [ ] Prosedur restore tersedia
- [ ] Audit log dasar tersedia
- [ ] Dokumentasi teknis dan operasional tersedia
- [ ] Kebijakan privasi dan penggunaan tersedia
- [ ] Minimal satu Administrator selain Superadmin tersedia untuk continuity

---

## 18. Out of Scope dan Future Considerations

### Future Features (Post-MVP):

| Fitur | Prioritas |
|-------|-----------|
| Self-service password reset | Tinggi |
| 2FA untuk mahasiswa | Sedang |
| Event management (pendaftaran, absensi, sertifikat) | Tinggi |
| Gallery dengan approval workflow | Sedang |
| Integrasi WhatsApp API | Tinggi |
| Public resource ecosystem | Rendah |
| Exam schedule management | Sedang |
| Deadline task management | Sedang |
| Recommendation system | Rendah |
| Multi-angkatan sebagai fitur penuh | Rendah |
| Pembatasan akses berdasarkan kelas/kelompok | Rendah |

### Out of Scope (Tidak akan dikerjakan):

- Video hosting langsung
- Role tambahan di luar Guest, Mahasiswa, Administrator, Superadmin
- Sistem multi-angkatan sebagai fitur operasional penuh
- Community-submitted official information workflow (tanpa admin approval)
- Advanced alumni features

---

## 19. Assumptions dan Open Decisions

Bagian ini tidak mengubah keputusan yang telah disepakati. Item berikut merupakan asumsi atau detail implementasi yang masih perlu ditentukan pada tahap desain teknis dan perencanaan implementasi.

| No | Asumsi / Open Decision | Status |
|----|----------------------|--------|
| 1 | Platform akan di-host di cloud provider (akan ditentukan) | Open |
| 2 | Framework/stack teknis yang akan digunakan (React/Next.js, Laravel/Django, dll) | Open |
| 3 | Database management system (PostgreSQL/MySQL/MongoDB) | Open |
| 4 | Object storage provider (AWS S3, Cloudflare R2, DigitalOcean Spaces) | Open |
| 5 | Domain name yang akan digunakan | Open |
| 6 | WhatsApp API provider yang akan diintegrasikan | Open |
| 7 | Detail struktur database untuk setiap modul | Open |
| 8 | Format dan frekuensi backup yang tepat | Open |
| 9 | UI/UX design system yang akan digunakan | Open |
| 10 | Timeline pengembangan per modul | Open |
| 11 | Prosedur verifikasi identitas untuk reset password | Open |
| 12 | Detail kebijakan privasi dan penggunaan | Open |

---

## Penutup

PRD ini mendefinisikan MVP sebagai platform yang memprioritaskan **kepercayaan terhadap informasi**, bukan jumlah fitur. Fondasi produk adalah:

1. **Official Information** sebagai Single Source of Truth
2. **Schedule** sebagai kebutuhan aktivitas harian
3. **Knowledge Hub** sebagai pusat resource pembelajaran

Seluruh keputusan teknis dan pengembangan selanjutnya harus dievaluasi terhadap **empat prinsip utama**: kepercayaan, keberlanjutan, kesederhanaan, dan skalabilitas. Fitur baru tidak boleh mengorbankan stabilitas, keamanan, kemudahan penggunaan, atau kemampuan platform untuk diwariskan kepada pengelola berikutnya.

---

*Dokumen ini adalah panduan resmi untuk pengembangan Platform Digital Angkatan Informatika 2025.*
