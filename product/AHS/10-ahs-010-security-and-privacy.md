# AHS-010: Security & Privacy

> **Architecture Hardening Specification (AHS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** ✅ Approved
>
> **Priority:** 🔴 Critical

---

# 📖 Overview

Dokumen ini mendefinisikan prinsip dan constraint arsitektural terkait **Security & Privacy** pada Platform Digital Informatika Angkatan 2025.

Security berfokus pada perlindungan sistem, akun, data, dan resource dari akses atau tindakan yang tidak sah.

Privacy berfokus pada bagaimana data pengguna dikumpulkan, digunakan, disimpan, diakses, dan dilindungi sesuai dengan kebutuhan sistem.

Dokumen ini berada pada tingkat arsitektur dan tidak menggantikan detail implementasi security, database security, maupun konfigurasi deployment.

---

# 🎯 Objectives

Dokumen ini bertujuan untuk:

* Melindungi akun dan resource sistem.
* Mencegah unauthorized access.
* Menerapkan prinsip least privilege.
* Melindungi data pengguna.
* Meminimalkan data yang dikumpulkan.
* Menjaga confidentiality, integrity, dan availability.
* Mendukung auditability terhadap aktivitas penting.
* Mengurangi risiko security incident.
* Menjadi baseline security architecture untuk implementasi.

---

# 📦 Scope

Dokumen ini mencakup:

* Authentication Security
* Authorization Security
* Access Control
* Data Protection
* Privacy
* Secret Management
* Input Security
* Output Security
* File Security
* Audit Security
* Dependency Security
* Security Incident Handling

---

# 🛡️ Security Principles

Arsitektur sistem mengikuti prinsip:

### 1. Least Privilege

Setiap actor hanya mendapatkan permission yang diperlukan untuk menjalankan tugasnya.

### 2. Defense in Depth

Security tidak bergantung pada satu mekanisme saja.

```text id="4g3z8n"
Authentication
      │
      ▼
Authorization
      │
      ▼
Input Validation
      │
      ▼
Business Rules
      │
      ▼
Data Protection
      │
      ▼
Audit / Monitoring
```

### 3. Secure by Default

Default behaviour sistem harus mengutamakan keamanan.

### 4. Fail Secure

Ketika terjadi kondisi yang tidak diketahui atau failure tertentu, sistem tidak boleh memberikan privilege tambahan secara otomatis.

### 5. Minimize Data Exposure

Data hanya boleh diekspos sesuai kebutuhan.

---

# 🔐 Authentication Security

Authentication harus memastikan bahwa hanya pengguna yang valid yang dapat memperoleh access ke sistem.

Prinsip:

* Credential harus dilindungi.
* Password tidak boleh disimpan dalam plaintext.
* Authentication failure tidak boleh memberikan informasi sensitif.
* Session harus memiliki lifecycle yang jelas.
* Credential management harus mengikuti security policy.
* Perubahan credential harus dapat diaudit apabila diperlukan.

Detail authentication flow mengikuti:

**AHS-003: Authentication & Authorization**

---

# 👥 Authorization Security

Authentication tidak secara otomatis memberikan permission terhadap resource.

```text id="m3p7k8"
User
 │
 ▼
Authentication
 │
 ▼
Authenticated Identity
 │
 ▼
Authorization
 │
 ▼
Permission Check
 │
 ▼
Resource Access
```

Setiap protected action harus melewati authorization yang sesuai.

---

# 🔑 Role & Permission Security

Role harus digunakan untuk mengontrol capability pengguna.

Contoh konseptual:

```text id="u7gq6f"
Student
  │
  └── Student Permissions

Backup Administrator
  │
  └── Limited Administrative Permissions

Superadmin
  │
  └── Full Administrative Permissions
```

Role tidak boleh memberikan privilege yang tidak diperlukan.

Administrative privilege harus dibatasi dan dapat diaudit.

---

# 🚪 Access Control

Access control harus diterapkan pada resource dan action yang membutuhkan perlindungan.

Contoh:

```text id="5jskzi"
Request
   │
   ▼
Identity
   │
   ▼
Role / Permission
   │
   ▼
Resource Ownership / Policy
   │
   ▼
Allow / Deny
```

Access control tidak boleh hanya bergantung pada tampilan frontend.

Security enforcement harus dilakukan pada trusted backend boundary.

---

# 🧹 Data Minimization

Sistem harus menerapkan prinsip **data minimization**.

Hanya data yang dibutuhkan untuk:

* Menjalankan fungsi sistem.
* Memenuhi kebutuhan operasional.
* Mendukung governance.
* Menyediakan pengalaman pengguna yang diperlukan.

yang seharusnya dikumpulkan.

Data yang tidak diperlukan tidak seharusnya dikumpulkan hanya karena tersedia.

---

# 🔒 Data Protection

Data penting harus dilindungi selama lifecycle-nya.

```text id="l6f4c2"
Collect
  │
  ▼
Process
  │
  ▼
Store
  │
  ▼
Access
  │
  ▼
Retain
  │
  ▼
Delete
```

Setiap tahap harus mempertimbangkan security dan privacy.

---

# 🔐 Credential & Secret Management

Secret dan credential harus dipisahkan dari source code.

Contoh secret:

* Database credential.
* API key.
* External service credential.
* Authentication secret.
* Deployment credential.

Prinsip:

```text id="5m1my9"
Source Code
     │
     X
Secret

Secure Configuration
     │
     ▼
Secret
```

Secret tidak boleh:

* Di-commit ke repository.
* Ditulis langsung dalam source code.
* Ditampilkan pada log.
* Diberikan kepada pengguna yang tidak berwenang.

---

# 🧪 Input Validation

Semua input yang berasal dari pengguna atau external system harus dianggap sebagai **untrusted input**.

```text id="m9tylq"
User Input
    │
    ▼
Validation
    │
    ├── Invalid ──► Reject
    │
    ▼
Sanitization / Normalization
    │
    ▼
Business Logic
```

Validasi harus mempertimbangkan:

* Format.
* Type.
* Length.
* Range.
* Required fields.
* Business constraints.

Detail error handling mengikuti IDR terkait.

---

# 📤 Output Security

Data yang dikembalikan kepada client harus dibatasi berdasarkan kebutuhan.

Sistem tidak boleh mengembalikan:

* Password.
* Secret.
* Internal credential.
* Data pengguna lain yang tidak berhak diakses.
* Internal implementation detail yang tidak diperlukan.

API response harus mengikuti prinsip **minimum necessary exposure**.

---

# 📁 File Security

File yang diunggah ke sistem harus diperlakukan sebagai untrusted content.

Security consideration meliputi:

* Authorization.
* File type validation.
* File size validation.
* Storage access control.
* Safe file retrieval.
* File metadata validation.

File private tidak boleh dapat diakses hanya dengan mengetahui identifier atau URL apabila resource tersebut membutuhkan authorization.

Detail file storage architecture mengikuti:

**AHS-004: File Storage & Data Consistency**

---

# 🗃️ Database Security

Database merupakan trusted infrastructure yang harus dilindungi dari unauthorized access.

Prinsip:

* Database tidak boleh diekspos tanpa kebutuhan.
* Credential harus dilindungi.
* Application access harus menggunakan permission yang sesuai.
* Data access harus mengikuti authorization model.
* Backup database harus dilindungi.

Detail struktur database dibahas dalam DDS.

---

# 🌐 Network Security

Communication antar komponen harus menggunakan mekanisme komunikasi yang sesuai dengan tingkat sensitivitas data.

Security consideration mencakup:

* Secure transport.
* Access restriction.
* Network boundary.
* Service exposure.
* External communication.

Tidak semua service harus diekspos ke public network.

---

# 🔗 External Dependency Security

External dependency harus dianggap sebagai potential security boundary.

```text id="txa6sm"
Application
     │
     ▼
Integration Boundary
     │
     ▼
External Service
```

Credential dan data yang dikirim ke external service harus dibatasi sesuai kebutuhan.

External response juga harus divalidasi sebelum digunakan.

Detail dependency governance mengikuti:

**AHS-007: External Dependencies**

---

# 📝 Audit & Security Events

Security-relevant activity harus dapat ditelusuri.

Contoh:

* Login failure.
* Unauthorized access attempt.
* Permission change.
* Administrative action.
* Credential change.
* Security configuration change.

Audit event harus menyediakan konteks yang cukup untuk investigation tanpa menyimpan secret secara langsung.

Detail audit architecture mengikuti:

**AHS-006: Audit & Observability**

---

# 🔎 Privacy

Privacy architecture harus mengikuti prinsip:

* Data minimization.
* Purpose limitation.
* Access limitation.
* Appropriate retention.
* Controlled disclosure.
* Secure deletion apabila diperlukan.

Data pengguna tidak boleh digunakan untuk tujuan yang tidak berkaitan dengan kebutuhan sistem tanpa dasar yang sesuai.

---

# 👤 User Data Access

Pengguna hanya boleh mengakses data yang menjadi haknya.

```text id="gqj7t5"
User
 │
 ▼
Authenticated Identity
 │
 ▼
Authorization Policy
 │
 ▼
Allowed User Data
```

Administrative access terhadap data pengguna harus dibatasi berdasarkan role dan kebutuhan operasional.

---

# 🗑️ Data Retention & Deletion

Data tidak seharusnya disimpan tanpa batas apabila sudah tidak diperlukan.

Lifecycle konseptual:

```text id="m9d7qd"
Active Data
    │
    ▼
Retention Period
    │
    ▼
Review
    │
    ├── Required ──► Continue Retention
    │
    └── No Longer Required
               │
               ▼
            Deletion
```

Retention period harus mengikuti kebutuhan bisnis, operational requirement, dan kebijakan yang berlaku.

---

# 🚨 Security Incident

Security incident harus diperlakukan sebagai operational incident yang membutuhkan investigation.

Contoh:

* Credential compromise.
* Unauthorized access.
* Privilege escalation.
* Data exposure.
* Suspicious administrative activity.
* Malicious file upload.

Konseptual flow:

```text id="a2p5c7"
Detection
    │
    ▼
Containment
    │
    ▼
Investigation
    │
    ▼
Remediation
    │
    ▼
Recovery
    │
    ▼
Post-Incident Review
```

Detail failure recovery mengikuti AHS-009.

---

# 🧯 Security Failure

Apabila security check mengalami failure atau kondisi tidak dapat ditentukan, sistem harus memilih behaviour yang aman.

Contoh:

```text id="0okyq7"
Authorization Check
       │
       ▼
Unknown / Error
       │
       ▼
Deny Access
```

Sistem tidak boleh memberikan access hanya karena security verification gagal.

---

# 🔄 Security by Change

Perubahan sistem harus mempertimbangkan security impact.

Security review diperlukan terutama untuk perubahan yang:

* Mengubah authentication.
* Mengubah authorization.
* Menambah privilege.
* Mengubah data sensitive.
* Menambah external integration.
* Mengubah file access.
* Mengubah network exposure.
* Mengubah credential mechanism.

---

# 🚧 Architecture Constraints

Implementasi harus mematuhi aturan berikut:

* Authentication dan authorization harus dipisahkan.
* Protected action harus memiliki authorization enforcement.
* Least privilege harus diterapkan.
* Secret tidak boleh berada di source code.
* Password tidak boleh disimpan plaintext.
* User input harus divalidasi.
* External input harus dianggap untrusted.
* Sensitive data tidak boleh diekspos tanpa authorization.
* File private harus memiliki access control.
* Security event penting harus dapat diaudit.
* Data harus dikumpulkan berdasarkan kebutuhan.
* Security failure harus fail secure.
* Security impact harus dipertimbangkan pada perubahan critical.

---

# 🔄 Change Management

Perubahan security architecture harus melalui review apabila:

* Mengubah authentication mechanism.
* Mengubah authorization model.
* Mengubah role atau permission.
* Mengubah data protection mechanism.
* Mengubah privacy boundary.
* Mengubah external integration.
* Mengubah storage security.
* Mengubah audit mechanism.

Perubahan harus ditelusuri kembali ke requirement, SDS, IDR, DDS, dan AHS terkait.

---

# 📚 Related Documents

## Previous Documents

* [AHS README](./README.md)
* [AHS-003: Authentication & Authorization](./03-ahs-003-authentication-and-authorization.md)
* [AHS-004: File Storage & Data Consistency](./04-ahs-004-file-storage-and-data-consistency.md)
* [AHS-006: Audit & Observability](./06-ahs-006-audit-and-observability.md)
* [AHS-007: External Dependencies](./07-ahs-007-external-dependencies.md)
* [AHS-009: Failure Recovery](./09-ahs-009-failure-recovery.md)

## Related Documents

* [RHS README](../rhs/README.md)
* [IDR README](../IDR/README.md)
* [DDS README](../DDS/README.md)
* [DDS-006: Security Design](../DDS/06-dds-006-security-design.md)
* [IDR-005: Coding Standards](../IDR/05-idr-005-coding-standards.md)
* [IDR-006: Error Handling Guidelines](../IDR/06-idr-006-error-handling-guidelines.md)

---

# ✅ Review Checklist

* [ ] Authentication security telah ditentukan.
* [ ] Authorization security telah ditentukan.
* [ ] Least privilege telah diterapkan.
* [ ] Access control boundary telah ditentukan.
* [ ] Data minimization telah dipertimbangkan.
* [ ] Data protection telah ditentukan.
* [ ] Secret management telah ditentukan.
* [ ] Input validation telah ditentukan.
* [ ] Output exposure telah dibatasi.
* [ ] File security telah dipertimbangkan.
* [ ] Database security telah dipertimbangkan.
* [ ] External dependency security telah ditentukan.
* [ ] Audit security event telah ditentukan.
* [ ] Privacy principles telah ditentukan.
* [ ] Data retention telah dipertimbangkan.
* [ ] Security incident handling telah dipertimbangkan.
* [ ] Security failure telah menggunakan fail-secure principle.

---

# 🔄 Traceability Matrix

| Area                         | Related Documentation |
| ---------------------------- | --------------------- |
| Authentication               | AHS-003               |
| Authorization                | AHS-003               |
| File Security                | AHS-004               |
| Audit & Observability        | AHS-006               |
| External Dependency Security | AHS-007               |
| Failure Recovery             | AHS-009               |
| Security Design              | DDS-006               |
| Coding Security              | IDR-005               |
| Error Handling               | IDR-006               |
| Security & Privacy           | AHS-010               |

---

# 📝 Revision History

| Version | Date       | Description                              | Author          |
| ------- | ---------- | ---------------------------------------- | --------------- |
| 1.0     | 2026-08-08 | Initial Security & Privacy documentation | Abidzar Dzakwan |
