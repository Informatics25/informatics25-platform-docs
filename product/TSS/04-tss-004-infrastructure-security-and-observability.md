# TSS-004: Infrastructure, Security & Observability

> **Technical Setup Specification (TSS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** Final — Technology Baseline
>
> **Focus:** Infrastructure, Storage, API, DevOps, Security & Observability

---

# 📖 Overview

Dokumen ini mendefinisikan technology baseline yang berkaitan dengan:

* Storage Technology
* API Technology
* DevOps Technology
* Environment Configuration
* Security Technology
* Monitoring & Observability

Dokumen ini melengkapi TSS-002 yang membahas frontend dan TSS-003 yang membahas backend serta database.

Secara konseptual:

```text
Application
    │
    ├───────────────┐
    │               │
    ▼               ▼
   API           Storage
    │               │
    ▼               ▼
 Backend       Object Storage
    │
    ├── DevOps
    ├── Security
    └── Observability
```

---

# 1. Storage Technology

## 1.1 S3-Compatible Object Storage

Binary file disimpan menggunakan **S3-compatible Object Storage**.

Production awal dapat menggunakan managed provider seperti:

* Cloudflare R2.
* Layanan S3-compatible lain yang sesuai dengan kebutuhan biaya.

**MinIO self-hosted** menjadi alternatif apabila kontrol terhadap infrastructure lebih penting dibandingkan operational simplicity.

---

## 1.2 Storage Abstraction

Storage abstraction harus menggunakan interface yang kompatibel dengan S3.

Tujuannya adalah memungkinkan provider storage diganti tanpa mengubah business logic secara signifikan.

```text
Application
     │
     ▼
Storage Interface
     │
     ├──────────────► Cloudflare R2
     │
     ├──────────────► S3-Compatible Provider
     │
     └──────────────► MinIO
```

Dengan abstraction tersebut, implementation tidak dikunci pada satu provider.

---

# 2. Metadata & Synchronization

## 2.1 Database Metadata

PostgreSQL menyimpan business metadata yang berkaitan dengan object.

Metadata meliputi:

* Ownership.
* Lifecycle.
* Content hash.
* Size.
* MIME metadata.
* Provider abstraction.
* Object key.
* Reference relation.

---

## 2.2 Source of Truth

Pembagian responsibility:

```text
PostgreSQL
    │
    ├── Ownership
    ├── Lifecycle
    ├── Content Hash
    ├── Size
    ├── MIME Metadata
    ├── Provider
    ├── Object Key
    └── Reference Relation

Object Storage
    │
    └── Binary File
```

Database menjadi **source of truth untuk business metadata**, sedangkan object storage menyimpan binary.

---

## 2.3 Partial Failure

Partial failure antara database dan object storage harus ditangani.

Strategi yang ditetapkan meliputi:

* Lifecycle state.
* Reconciliation.

Tujuannya agar kondisi database dan object storage dapat diperiksa dan diselaraskan kembali ketika terjadi kegagalan sebagian.

---

## 2.4 Logical vs Physical Deletion

Physical deletion harus dipisahkan dari logical removal.

Physical deletion mengikuti:

* Retention period.
* Grace period.

Secara konseptual:

```text
Active
  │
  ▼
Logical Removal
  │
  ▼
Grace / Retention Period
  │
  ▼
Physical Deletion
```

---

# 3. API Technology Stack

## 3.1 REST API

**REST API** menjadi baseline API technology.

REST dipilih karena:

* Sederhana.
* Interoperable.
* Mudah diintegrasikan dengan Nuxt.
* Sesuai dengan skala proyek.

GraphQL dan gRPC tidak dipilih untuk baseline karena kompleksitas tambahan belum diperlukan untuk kebutuhan MVP.

---

## 3.2 OpenAPI 3.1

**OpenAPI 3.1** menjadi kontrak resmi untuk:

* Backend.
* Frontend.
* QA.
* Integration.

Breaking change harus mengikuti versioning policy.

```text
OpenAPI 3.1
     │
     ├── Backend
     ├── Frontend
     ├── QA
     └── Integration
```

---

## 3.3 API Versioning

Baseline menggunakan **major version pada path**.

Contoh:

```text
/api/v1
```

Backward-compatible changes tetap berada pada major version yang sama.

Breaking change membutuhkan major version baru.

---

## 3.4 Error Response Standard

API error response menggunakan struktur berikut:

| Field        | Purpose                            |
| ------------ | ---------------------------------- |
| `code`       | Stable machine-readable error code |
| `message`    | Pesan aman untuk user              |
| `details`    | Informasi validation yang aman     |
| `request_id` | Korelasi dengan log                |

Response error **tidak boleh membocorkan**:

* Stack trace.
* SQL query.
* Token.
* Credential.
* Informasi security-sensitive.

---

# 4. DevOps Technology Stack

## 4.1 Docker

**Docker** digunakan untuk:

* Reproducible development.
* Packaging.
* Environment consistency.

Docker menjadi dasar containerization untuk menjaga konsistensi environment.

---

## 4.2 Docker Compose

**Docker Compose** digunakan untuk:

* Local development.
* Integration environment.

Kubernetes tidak dipilih untuk MVP karena operational overhead dianggap terlalu besar untuk skala proyek saat ini.

---

## 4.3 GitHub Actions

**GitHub Actions** digunakan sebagai CI/CD technology.

Workflow mencakup:

* Linting.
* Testing.
* Build.
* Security checks.
* Deployment workflow.

---

## 4.4 CI/CD Security

Secret CI/CD harus dikelola berdasarkan prinsip:

* Least privilege.
* Environment protection.

Production workflow tidak boleh memiliki akses lebih besar daripada yang dibutuhkan.

---

# 5. Environment Configuration

## 5.1 Configuration Separation

Configuration harus dipisahkan dari source code.

```text
Source Code
    │
    └── Application Logic

Environment Configuration
    │
    ├── Development
    ├── Test / CI
    ├── Staging
    └── Production
```

---

## 5.2 Secret Management

Secret **tidak boleh disimpan di repository**.

Production secret hanya boleh dapat diakses oleh:

* Workflow yang berwenang.
* Operator yang berwenang.

---

## 5.3 Environment Baseline

Environment minimal:

1. Development.
2. Test / CI.
3. Staging, bila tersedia.
4. Production.

Configuration harus divalidasi saat startup.

---

# 6. Security Technology Stack

## 6.1 Password Hashing — Argon2id

**Argon2id** menjadi password hashing algorithm utama.

Password plaintext:

* Tidak pernah disimpan.
* Tidak pernah dilog.

Bcrypt merupakan alternatif yang matang, tetapi Argon2id dipilih sebagai baseline modern memory-hard hashing.

```text
Password
    │
    ▼
 Argon2id
    │
    ▼
Password Hash
    │
    ▼
PostgreSQL
```

---

# 7. TOTP 2FA

## 7.1 Administrative 2FA

**TOTP 2FA wajib untuk:**

* Administrator.
* Superadmin.

Backup codes diberikan saat enrollment.

Recovery mengikuti verification dan governance yang telah ditentukan.

---

## 7.2 SMS OTP

SMS OTP **tidak menjadi primary administrative 2FA**.

Baseline administrative authentication menggunakan TOTP.

---

# 8. Audit Log

Audit log mencatat tindakan yang bersifat:

* Security-sensitive.
* Business-critical.

Contoh:

* Role changes.
* Account lifecycle.
* Publication.
* Approval.
* Password reset.
* Sensitive configuration.
* Governance event.

---

## 8.1 Audit Log Principles

Audit log:

* Bersifat logically append-only.
* Harus disanitasi.
* Tidak boleh menyimpan secret yang tidak diperlukan.
* Tidak boleh menyimpan PII yang tidak diperlukan.

---

# 9. Rate Limiting

Rate limiting diterapkan pada endpoint sensitif, termasuk:

* Login.
* Refresh token.
* Password reset.
* 2FA verification.
* Upload initiation.
* Endpoint sensitif lainnya.

---

## 9.1 Rate Limit Implementation

Untuk development:

```text
In-Memory Rate Limiting
```

dapat digunakan.

Jika deployment berkembang, shared/distributed mechanism dapat dipertimbangkan.

---

# 10. CSRF, XSS & CORS

## 10.1 XSS

Mitigasi XSS mencakup:

* Framework escaping.
* Sanitization terhadap user-generated content.
* Content Security Policy apabila sesuai.

---

## 10.2 CSRF

Mitigasi CSRF mengikuti token transport strategy.

Apabila credential menggunakan cookie-based transport, CSRF protection harus diterapkan.

---

## 10.3 CORS

CORS menggunakan **explicit origin allowlist**.

Wildcard tidak digunakan untuk credentialed production requests.

```text
Allowed Origins
       │
       ▼
Explicit Allowlist
       │
       ▼
Production API
```

---

## 10.4 Security Headers

Security headers diterapkan sesuai application/deployment boundary.

---

# 11. Monitoring & Observability

Monitoring dan observability digunakan untuk memberikan visibility terhadap kondisi runtime sistem.

Baseline observability meliputi:

* Structured logging.
* Request/correlation identifier.
* Health checks.
* Metrics.
* Actionable monitoring.

---

# 12. Production Logging

Production menggunakan **structured machine-readable logs**.

Setiap request perlu memiliki:

* Request identifier.
* Correlation identifier.

Tujuannya adalah membantu korelasi antara request, service behavior, dan operational events.

---

# 13. Health Check

Minimal terdapat dua semantics:

* **Liveness**
* **Readiness**

Health check tidak boleh membocorkan:

* Credential.
* Sensitive topology.
* Informasi internal yang tidak diperlukan.

Secara konseptual:

```text
Health Check
     │
     ├── Liveness
     │
     └── Readiness
```

---

# 14. Metrics

Metrics minimal mencakup:

### HTTP

* HTTP request count.
* Latency.
* Error rate.
* Status code distribution.

### Authentication

* Authentication success/failure.
* Rate-limit events.

### Database

* Database connection.
* Database latency.
* Database error.

### Object Storage

* Upload failure.
* Download failure.

### Notification Queue

* Success.
* Failure.
* Retry.

### Backup

* Backup success/failure.
* Restore verification.

---

# 15. Monitoring Strategy

Monitoring berorientasi pada **actionable alert**.

Prioritas tinggi mencakup:

* Service unavailable.
* Repeated backup failure.
* Database failure.
* Storage inconsistency.
* Authentication anomaly.
* Recovery target yang terancam.

Tujuan monitoring bukan sekadar mengumpulkan metric, tetapi memastikan kondisi yang membutuhkan tindakan dapat diketahui.

---

# 🧩 Technology Summary

| Area                            | Technology / Strategy                                              |
| ------------------------------- | ------------------------------------------------------------------ |
| Object Storage                  | S3-compatible Object Storage                                       |
| Production Storage Example      | Cloudflare R2 / S3-compatible provider                             |
| Self-hosted Alternative         | MinIO                                                              |
| API                             | REST                                                               |
| API Contract                    | OpenAPI 3.1                                                        |
| API Versioning                  | Major version pada path                                            |
| Containerization                | Docker                                                             |
| Local / Integration Environment | Docker Compose                                                     |
| CI/CD                           | GitHub Actions                                                     |
| Password Hashing                | Argon2id                                                           |
| Administrative 2FA              | TOTP                                                               |
| Audit                           | Append-only logical audit log                                      |
| Rate Limiting                   | In-memory untuk development; distributed mechanism bila berkembang |
| XSS Protection                  | Framework escaping + sanitization                                  |
| CSRF Protection                 | Berdasarkan token transport strategy                               |
| CORS                            | Explicit origin allowlist                                          |
| Logging                         | Structured machine-readable logs                                   |
| Health Check                    | Liveness + Readiness                                               |
| Metrics                         | HTTP, authentication, database, storage, queue, backup             |
| Monitoring                      | Actionable alert                                                   |

---

# 🚧 Infrastructure & Security Constraints

Implementasi harus mematuhi constraint berikut:

* Binary file disimpan di S3-compatible object storage.
* PostgreSQL menjadi source of truth untuk business metadata.
* Storage abstraction harus S3-compatible.
* Partial failure harus ditangani melalui lifecycle state dan reconciliation.
* Physical deletion mengikuti retention/grace period.
* REST menjadi API baseline.
* OpenAPI 3.1 menjadi API contract resmi.
* Breaking API change membutuhkan major version baru.
* Error response tidak boleh membocorkan informasi sensitif.
* Docker digunakan untuk reproducible development dan packaging.
* Docker Compose digunakan untuk local dan integration environment.
* Kubernetes tidak menjadi baseline MVP.
* CI/CD menggunakan GitHub Actions.
* Secret tidak boleh disimpan di repository.
* Production secret hanya dapat diakses oleh pihak yang berwenang.
* Configuration harus divalidasi saat startup.
* Argon2id menjadi password hashing baseline.
* TOTP wajib untuk Administrator dan Superadmin.
* Audit log harus logically append-only.
* Rate limiting diterapkan pada endpoint sensitif.
* Production CORS menggunakan explicit origin allowlist.
* Production logging menggunakan structured machine-readable logs.
* Health check menyediakan liveness dan readiness semantics.
* Monitoring berorientasi pada actionable alert.

---

# 🔗 Relationship with Other Documents

```text
TSS-001
Technology Governance
        │
        ▼
TSS-004
Infrastructure, Security & Observability
        │
        ├── Storage
        ├── API
        ├── DevOps
        ├── Environment
        ├── Security
        └── Observability
```

Hubungan dengan technology stack lain:

```text
TSS-002 Frontend
        │
        │ REST / OpenAPI
        ▼
TSS-003 Backend
        │
        ├────────► PostgreSQL
        │
        └────────► Object Storage
```

---

# 📚 Related Documents

* [TSS README](./README.md)
* [Technology Governance](./01-tss-001-technology-governance.md)
* [Frontend Technology Stack](./02-tss-002-frontend-technology-stack.md)
* [Backend & Database Stack](./03-tss-003-backend-and-database-stack.md)
* [Development, Testing & Coding Standards](./05-tss-005-development-testing-and-coding-standards.md)
* [Technology Decision & Final Baseline](./06-tss-006-technology-decision-and-final-baseline.md)
* [SDS README](../SDS/README.md)
* [DDS README](../DDS/README.md)
* [AHS README](../AHS/README.md)
* [IDR README](../IDR/README.md)

---

# 🔄 Traceability Matrix

| Concern                  | Baseline                         |
| ------------------------ | -------------------------------- |
| Binary Storage           | S3-compatible Object Storage     |
| Metadata Source of Truth | PostgreSQL                       |
| API Style                | REST                             |
| API Contract             | OpenAPI 3.1                      |
| API Versioning           | `/api/v1` style major version    |
| Containerization         | Docker                           |
| Local Environment        | Docker Compose                   |
| CI/CD                    | GitHub Actions                   |
| Password Hashing         | Argon2id                         |
| Administrative 2FA       | TOTP                             |
| Audit                    | Logical append-only audit log    |
| Rate Limiting            | Endpoint-sensitive rate limiting |
| CORS                     | Explicit origin allowlist        |
| Production Logging       | Structured machine-readable logs |
| Health                   | Liveness + Readiness             |
| Metrics                  | Operational metrics              |
| Alerting                 | Actionable alerts                |

---

# ✅ Review Checklist

* [ ] S3-compatible object storage telah ditetapkan.
* [ ] Storage abstraction telah ditentukan.
* [ ] Metadata dan binary storage responsibility telah dipisahkan.
* [ ] Partial failure handling telah ditentukan.
* [ ] Logical dan physical deletion telah dipisahkan.
* [ ] REST API telah ditetapkan.
* [ ] OpenAPI 3.1 telah ditetapkan sebagai contract.
* [ ] API versioning telah ditentukan.
* [ ] Error response standard telah ditentukan.
* [ ] Docker telah ditetapkan.
* [ ] Docker Compose telah ditetapkan.
* [ ] GitHub Actions telah ditetapkan.
* [ ] Environment configuration telah ditentukan.
* [ ] Secret management constraint telah ditentukan.
* [ ] Argon2id telah ditetapkan.
* [ ] TOTP 2FA telah ditetapkan untuk Administrator dan Superadmin.
* [ ] Audit log telah ditentukan.
* [ ] Rate limiting telah ditentukan.
* [ ] CSRF/XSS/CORS requirements telah ditentukan.
* [ ] Security headers telah ditentukan.
* [ ] Structured logging telah ditentukan.
* [ ] Liveness dan readiness telah ditentukan.
* [ ] Operational metrics telah ditentukan.
* [ ] Actionable monitoring telah ditentukan.

---

# 📝 Revision History

| Version | Date       | Description                                                    | Author          |
| ------- | ---------- | -------------------------------------------------------------- | --------------- |
| 1.0     | 2026-07-21 | Initial Infrastructure, Security & Observability documentation | Abidzar Dzakwan |
