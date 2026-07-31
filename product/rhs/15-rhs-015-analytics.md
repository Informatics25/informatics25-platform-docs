# 📊 RHS-015: Analytics & Observability

> **Requirement Hardening Specification**
>
> **Reference:** PRD §15 – Analytics & Product Measurement
>
> **Status:** ✅ Approved
>
> **Priority:** 🟡 Medium

---

# 📖 Overview

Dokumen ini mendefinisikan kebutuhan analitik (*analytics*) dan observabilitas (*observability*) untuk Platform Informatika Angkatan 2025.

Analytics digunakan untuk mengukur keberhasilan produk berdasarkan perilaku pengguna dan adopsi fitur, sedangkan observability digunakan untuk memantau kesehatan sistem selama operasional.

Dokumen ini **tidak** bertujuan mengumpulkan data pribadi secara berlebihan (*privacy by design*), melainkan memastikan keputusan pengembangan dapat didukung oleh data yang relevan.

---

# 🎯 Objective

- Mengukur tingkat adopsi platform.
- Mengetahui efektivitas penyampaian informasi resmi.
- Mengukur penggunaan setiap fitur utama.
- Membantu Product Owner mengambil keputusan berbasis data.
- Menyediakan metrik operasional untuk monitoring sistem.

---

# 📋 Business Rules

| ID | Rule |
|----|------|
| ANA-01 | Analytics hanya boleh mengumpulkan data yang diperlukan untuk evaluasi produk. |
| ANA-02 | Analytics tidak boleh menyimpan password, token autentikasi, atau data sensitif lainnya. |
| ANA-03 | Seluruh data analytics harus mengikuti kebijakan privasi platform. |
| ANA-04 | Analytics tidak boleh mengganggu performa aplikasi. |
| ANA-05 | Data analytics digunakan untuk peningkatan produk, bukan untuk memantau aktivitas pribadi mahasiswa. |

---

# 📊 Product Analytics

## Dashboard

| Event | Trigger |
|--------|---------|
| Dashboard dibuka | Pengguna berhasil membuka dashboard |
| Dashboard refresh | Pengguna melakukan refresh halaman |
| Widget dipilih | Pengguna membuka salah satu widget dashboard |

---

## Official Information

| Event | Trigger |
|--------|---------|
| Informasi dibuka | Pengguna membuka detail informasi |
| Informasi selesai dibaca | Pengguna membaca hingga akhir |
| Informasi dibagikan | Pengguna membagikan tautan informasi (jika fitur tersedia) |

---

## Schedule

| Event | Trigger |
|--------|---------|
| Jadwal dibuka | Pengguna membuka halaman jadwal |
| Detail jadwal dibuka | Pengguna memilih salah satu jadwal |
| Perubahan jadwal dilihat | Pengguna membuka detail exception/perubahan jadwal |

---

## Knowledge Hub

| Event | Trigger |
|--------|---------|
| Resource dibuka | Pengguna membuka detail resource |
| Resource diunduh | Pengguna mengunduh file |
| Resource diunggah | Mahasiswa mengirim resource baru |
| Resource disetujui | Administrator menyetujui resource |
| Resource ditolak | Administrator menolak resource |
| Resource dipublikasikan | Resource dipublikasikan ke area yang ditentukan |

---

## Event

| Event | Trigger |
|--------|---------|
| Event dibuka | Pengguna membuka halaman event |
| Detail event dibuka | Pengguna membuka detail kegiatan |

---

## Gallery

| Event | Trigger |
|--------|---------|
| Gallery dibuka | Pengguna membuka halaman galeri |
| Foto diperbesar | Pengguna membuka media dalam ukuran penuh |

---

## Authentication

| Event | Trigger |
|--------|---------|
| Login berhasil | Pengguna berhasil login |
| Login gagal | Kredensial tidak valid |
| Logout | Pengguna keluar dari sistem |
| Password diganti | Password berhasil diperbarui |
| Password direset | Superadmin melakukan reset password |
| Onboarding selesai | Pengguna menyelesaikan onboarding |

---

# 📈 Product Metrics

| Metric | Tujuan |
|---------|--------|
| Daily Active Users (DAU) | Mengukur pengguna aktif harian |
| Weekly Active Users (WAU) | Mengukur pengguna aktif mingguan |
| Monthly Active Users (MAU) | Mengukur pengguna aktif bulanan |
| Account Activation Rate | Persentase akun yang telah aktif |
| Onboarding Completion Rate | Tingkat penyelesaian onboarding |
| Official Information Read Rate | Tingkat keterbacaan informasi resmi |
| Knowledge Hub Usage | Tingkat penggunaan Knowledge Hub |
| Resource Upload Count | Jumlah resource yang diunggah |
| Dashboard Usage | Frekuensi akses dashboard |

---

# 📉 Operational Metrics

| Metric | Description |
|---------|-------------|
| API Response Time | Rata-rata waktu respons API |
| Error Rate | Persentase request gagal |
| Failed Login Rate | Tingkat login gagal |
| Upload Failure Rate | Tingkat kegagalan upload |
| Queue Length | Panjang antrean notifikasi |
| Notification Delivery Success | Tingkat keberhasilan pengiriman notifikasi |
| Database Connection Health | Status koneksi database |
| Storage Usage | Penggunaan penyimpanan file |

---

# 🔍 Observability Requirements

Sistem minimal harus mendukung monitoring terhadap:

- API latency
- API error rate
- CPU utilization
- Memory utilization
- Disk utilization
- Database availability
- Database connection pool
- Queue status
- Object storage health
- Background job status

---

# 🔒 Privacy Requirements

| ID | Rule |
|----|------|
| PRI-01 | Analytics tidak boleh menyimpan password atau credential. |
| PRI-02 | Analytics tidak boleh menyimpan TOTP secret atau backup code. |
| PRI-03 | Data analytics tidak boleh digunakan untuk profiling individu tanpa dasar yang sah. |
| PRI-04 | Event analytics harus menggunakan identifier internal yang sesuai kebijakan privasi. |
| PRI-05 | Pengumpulan data harus mengikuti prinsip data minimization. |

---

# 🧪 Analytics Validation

| Test | Expected Result |
|------|-----------------|
| Dashboard dibuka | Event analytics tercatat |
| Resource diunduh | Download event tercatat |
| Login berhasil | Login event tercatat |
| Login gagal | Failed login event tercatat |
| Resource disetujui | Approval event tercatat |
| Perubahan jadwal | Schedule update event tercatat |

---

# ✅ Acceptance Criteria

| ID | Given | When | Then |
|----|-------|------|------|
| AC-01 | Pengguna login | Login berhasil | Event login tercatat |
| AC-02 | Pengguna membuka dashboard | Halaman dimuat | Event dashboard view tercatat |
| AC-03 | Resource diunduh | Download berhasil | Event download tercatat |
| AC-04 | Informasi resmi dibaca | Detail dibuka | Event read information tercatat |
| AC-05 | Jadwal dibuka | Halaman dimuat | Event schedule view tercatat |

---

# 📊 Suggested Dashboard (Internal)

Dashboard analytics internal sebaiknya menampilkan:

- Daily Active Users (DAU)
- Weekly Active Users (WAU)
- Monthly Active Users (MAU)
- Account Activation Rate
- Onboarding Completion Rate
- Most Viewed Official Information
- Most Downloaded Resources
- Resource Approval Rate
- Login Success vs Failed Login
- Notification Delivery Rate
- System Health Summary

---

# 🔗 Related Documents

- PRD §15 – Analytics & Product Measurement
- RHS-001 – Authentication & Onboarding
- RHS-002 – Official Information
- RHS-003 – Dashboard
- RHS-004 – Schedule
- RHS-005 – Knowledge Hub
- RHS-011 – Notification & WhatsApp
- RHS-012 – Audit Log
- RHS-014 – Performance, Availability & Resilience
- RHS-016 – Governance & Operational Continuity

---

# 📝 Notes

- Analytics berfungsi sebagai alat evaluasi produk dan operasional, bukan sebagai mekanisme pemantauan individu.
- Implementasi analytics harus mengikuti prinsip **Privacy by Design**, **Data Minimization**, dan **Security by Default**.
- Metrik dapat diperluas pada iterasi berikutnya tanpa mengubah prinsip dasar yang telah ditetapkan dalam dokumen ini.
