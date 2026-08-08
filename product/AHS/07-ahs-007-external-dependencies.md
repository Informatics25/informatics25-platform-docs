# AHS-007: External Dependencies

> **Architecture Hardening Specification (AHS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** ✅ Approved
>
> **Priority:** 🟠 High

---

# 📖 Overview

Dokumen ini mendefinisikan aturan arsitektural untuk penggunaan **external dependencies** pada Platform Digital Informatika Angkatan 2025.

External dependency adalah layanan, sistem, library, platform, atau komponen di luar business domain utama aplikasi yang dibutuhkan untuk menjalankan fungsi tertentu.

Penggunaan dependency eksternal harus dikendalikan agar kegagalan atau perubahan pada dependency tidak secara tidak terkontrol merusak keseluruhan sistem.

---

# 🎯 Objectives

Dokumen ini bertujuan untuk:

* Mengidentifikasi kategori dependency eksternal.
* Menentukan batas integrasi dengan sistem eksternal.
* Mengurangi coupling terhadap external service.
* Mendefinisikan prinsip dependency isolation.
* Menentukan perilaku sistem ketika dependency mengalami failure.
* Memastikan external dependency memiliki ownership dan lifecycle yang jelas.

---

# 📦 Scope

Dokumen ini mencakup:

* External Service
* Third-Party Provider
* Infrastructure Dependency
* Integration Boundary
* Dependency Isolation
* Failure Handling
* Dependency Availability
* Dependency Change Management

Dokumen ini tidak mendefinisikan konfigurasi teknis provider secara rinci.

---

# 🔗 External Dependency Categories

External dependency dapat dikategorikan sebagai berikut:

| Category            | Description                                         |
| ------------------- | --------------------------------------------------- |
| Infrastructure      | Komponen infrastruktur yang dibutuhkan sistem       |
| Storage             | Layanan penyimpanan file atau object                |
| Communication       | Layanan komunikasi eksternal                        |
| Authentication      | Layanan authentication apabila digunakan            |
| Monitoring          | Layanan observability eksternal                     |
| Development         | Tool atau service yang digunakan selama development |
| Third-Party Library | Library yang digunakan aplikasi                     |

Tidak semua dependency memiliki tingkat kritikalitas yang sama.

---

# 🧩 Dependency Boundary

External service harus diakses melalui boundary yang jelas.

```text
Application
     │
     ▼
Integration Boundary
     │
     ▼
External Service
```

Business logic tidak boleh bergantung langsung pada detail implementasi provider eksternal apabila dependency tersebut dapat diisolasi melalui abstraction atau integration layer.

---

# 🛡️ Dependency Isolation

Dependency eksternal sebaiknya diisolasi dari domain utama aplikasi.

Contoh:

```text
Knowledge Hub
      │
      ▼
File Storage Interface
      │
      ▼
Storage Provider
```

Dengan pendekatan tersebut, domain tidak perlu mengetahui detail internal provider.

Tujuannya adalah mengurangi dampak apabila provider diganti atau mengalami perubahan.

---

# 📊 Dependency Criticality

Setiap external dependency harus dapat diklasifikasikan berdasarkan dampaknya terhadap sistem.

| Level    | Meaning                                                        |
| -------- | -------------------------------------------------------------- |
| Critical | Sistem atau fungsi utama tidak dapat berjalan tanpa dependency |
| High     | Fungsi penting mengalami gangguan                              |
| Medium   | Sebagian fitur terganggu tetapi core system tetap berjalan     |
| Low      | Dampak terbatas dan dapat ditoleransi                          |

Criticality harus dipertimbangkan ketika menentukan failure handling dan recovery strategy.

---

# ⚠️ Dependency Failure

External dependency dapat mengalami:

* Timeout.
* Service unavailable.
* Network failure.
* Authentication failure.
* Rate limitation.
* Invalid response.
* Service degradation.
* Provider maintenance.

Sistem tidak boleh menganggap external service selalu tersedia.

Konseptual flow:

```text
Application
     │
     ▼
External Dependency
     │
     ├── Success ──► Continue
     │
     └── Failure
            │
            ▼
       Failure Handling
            │
            ├── Retry
            ├── Fallback
            └── Graceful Degradation
```

Mekanisme yang digunakan bergantung pada karakteristik dependency.

---

# 🔄 Retry Principle

Retry dapat digunakan ketika kegagalan bersifat sementara.

Contoh:

```text
Request
  │
  ▼
External Service
  │
  ▼
Temporary Failure
  │
  ▼
Retry
  │
  ├── Success ──► Continue
  │
  └── Failed ──► Failure Handling
```

Retry tidak boleh dilakukan tanpa batas karena dapat memperburuk kondisi dependency yang sedang mengalami masalah.

---

# 🧯 Graceful Degradation

Apabila external dependency tidak tersedia dan fungsi tersebut bukan bagian dari critical path, sistem harus mempertimbangkan graceful degradation.

Contoh:

```text
External Notification Service
          │
          ▼
       Failure
          │
          ▼
Core Application Continues
```

Kegagalan sebuah fitur pendukung tidak boleh menyebabkan seluruh aplikasi berhenti apabila fungsi utama masih dapat berjalan.

---

# 🚫 External Dependency as Single Point of Failure

Dependency eksternal yang menjadi bagian dari critical path harus diidentifikasi sebagai potential **Single Point of Failure (SPOF)**.

Contoh:

```text
Application
     │
     ▼
Critical External Service
     │
     ▼
Service Failure
     │
     ▼
Application Impact
```

Untuk dependency yang bersifat critical, architecture review harus mempertimbangkan:

* Availability.
* Recovery strategy.
* Fallback.
* Replacement strategy.
* Operational ownership.

---

# 🔐 External Dependency Security

External integration harus memperhatikan keamanan.

Prinsip:

* Credential tidak boleh disimpan di source code.
* Secret tidak boleh ditulis ke log.
* Access terhadap external service harus dibatasi.
* Communication harus menggunakan mekanisme yang sesuai.
* External response tidak boleh dipercaya tanpa validasi.
* Dependency credential harus memiliki lifecycle yang jelas.

---

# 🧪 External Response Validation

Data yang berasal dari external service harus dianggap sebagai **untrusted input** sampai divalidasi.

```text
External Service
      │
      ▼
External Response
      │
      ▼
Validation
      │
      ├── Invalid ──► Reject / Handle Error
      │
      ▼
Application Logic
```

Aplikasi tidak boleh mengasumsikan external provider selalu mengembalikan data sesuai ekspektasi.

---

# 📦 Third-Party Libraries

Third-party library merupakan external dependency pada level application code.

Penggunaan library harus mempertimbangkan:

* Compatibility.
* Maintenance status.
* Security.
* License.
* Community or vendor support.
* Dependency impact.
* Upgrade strategy.

Library yang tidak lagi dipelihara atau memiliki risiko keamanan harus dievaluasi kembali.

---

# 🔄 Dependency Lifecycle

Setiap dependency memiliki lifecycle:

```text
Evaluate
   │
   ▼
Adopt
   │
   ▼
Monitor
   │
   ▼
Upgrade
   │
   ├── Continue
   │
   └── Replace / Remove
```

Dependency tidak boleh dianggap sebagai keputusan permanen.

---

# 📝 Dependency Documentation

Dependency penting harus terdokumentasi sekurang-kurangnya berdasarkan:

* Nama dependency.
* Tujuan penggunaan.
* Modul yang menggunakannya.
* Criticality.
* Dampak apabila unavailable.
* Ownership.
* Recovery atau fallback strategy apabila diperlukan.

Detail technology stack tetap dikelola pada dokumentasi IDR.

---

# 👥 Dependency Ownership

Setiap critical external dependency harus memiliki pihak yang bertanggung jawab terhadap:

* Credential.
* Configuration.
* Availability monitoring.
* Failure handling.
* Upgrade.
* Replacement.

Tidak boleh terdapat critical dependency yang tidak memiliki ownership yang jelas.

---

# 🔄 Provider Replacement

Arsitektur harus memungkinkan dependency tertentu diganti apabila diperlukan.

Contoh:

```text
Application
     │
     ▼
Storage Interface
     │
     ├── Provider A
     │
     └── Provider B
```

Tujuan abstraction bukan berarti seluruh dependency harus dapat diganti dengan mudah, tetapi memastikan business domain tidak terikat secara tidak perlu pada detail provider.

---

# 🚧 Architecture Constraints

Implementasi harus mematuhi aturan berikut:

* External dependency harus memiliki integration boundary.
* Business domain tidak boleh bergantung langsung pada detail provider apabila abstraction diperlukan.
* Dependency failure harus dapat ditangani.
* Critical dependency harus memiliki ownership.
* External response harus divalidasi.
* Credential tidak boleh disimpan di source code.
* Retry tidak boleh dilakukan tanpa batas.
* Critical dependency harus dievaluasi sebagai potential SPOF.
* Dependency yang sudah tidak aman atau tidak terpelihara harus dievaluasi ulang.

---

# 🔄 Change Management

Perubahan external dependency harus melalui review apabila perubahan tersebut:

* Menambahkan critical dependency baru.
* Menghapus dependency yang digunakan oleh critical feature.
* Mengganti provider.
* Mengubah integration boundary.
* Mengubah authentication mechanism.
* Mengubah recovery strategy.
* Meningkatkan coupling terhadap provider tertentu.

Perubahan signifikan harus ditelusuri kembali ke SDS, IDR, dan requirement yang terdampak.

---

# 📚 Related Documents

## Previous Documents

* [AHS README](./README.md)
* [AHS-001: Architecture Overview](./01-ahs-001-architecture-overview.md)
* [AHS-002: Module Boundaries](./02-ahs-002-module-boundaries.md)
* [AHS-003: Authentication & Authorization](./03-ahs-003-authentication-and-authorization.md)
* [AHS-004: File Storage & Data Consistency](./04-ahs-004-file-storage-and-data-consistency.md)
* [AHS-005: Backup & Recovery](./05-ahs-005-backup-and-recovery.md)
* [AHS-006: Audit & Observability](./06-ahs-006-audit-and-observability.md)

## Related Documents

* [IDR-002: Technology Stack](../IDR/02-idr-002-technology-stack.md)
* [IDR-007: API Design Guidelines](../IDR/07-idr-007-api-design-guidelines.md)
* [DDS-005: Interface Design](../DDS/05-dds-005-interface-design.md)

## Next Document

* [AHS-008: Operational Governance](./08-ahs-008-operational-governance.md)

---

# ✅ Review Checklist

* [ ] External dependency categories telah diidentifikasi.
* [ ] Integration boundary telah ditentukan.
* [ ] Dependency isolation telah ditetapkan.
* [ ] Criticality telah dipertimbangkan.
* [ ] Failure handling telah didefinisikan.
* [ ] Retry principle telah ditentukan.
* [ ] Graceful degradation telah dipertimbangkan.
* [ ] Critical dependency telah dievaluasi sebagai potential SPOF.
* [ ] External response validation telah ditentukan.
* [ ] Dependency ownership telah ditentukan.
* [ ] Dependency lifecycle telah didefinisikan.
* [ ] Provider replacement telah dipertimbangkan.

---

# 🔄 Traceability Matrix

| Area                   | Related Documentation |
| ---------------------- | --------------------- |
| Technology Selection   | IDR-002               |
| API Integration        | IDR-007               |
| Interface Design       | DDS-005               |
| File Storage           | AHS-004               |
| External Dependencies  | AHS-007               |
| Backup & Recovery      | AHS-005               |
| Operational Governance | AHS-008               |
| Failure Recovery       | AHS-009               |

---

# 📝 Revision History

| Version | Date       | Description                                 | Author          |
| ------- | ---------- | ------------------------------------------- | --------------- |
| 1.0     | 2026-08-08 | Initial External Dependencies documentation | Abidzar Dzakwan |
