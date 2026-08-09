# TSS-002: Frontend Technology Stack

> **Technical Setup Specification (TSS)**
>
> **Platform Digital Informatika Angkatan 2025**
>
> **Status:** Final — Technology Baseline
>
> **Focus:** Frontend Technology

---

# 📖 Overview

Dokumen ini mendefinisikan technology stack yang digunakan untuk pengembangan frontend Platform Digital Informatika Angkatan 2025.

Frontend bertanggung jawab terhadap:

* Public landing page.
* Authenticated dashboard.
* Administrator interface.
* Routing.
* Rendering.
* User interface.
* Client-side state.
* Form interaction.
* Frontend validation.
* API communication.

Technology baseline frontend menggunakan **Nuxt 4 + TypeScript** sebagai fondasi utama, dengan Tailwind CSS v4, Nuxt UI, Pinia, VeeValidate + Zod, `$fetch` / ofetch, serta Lucide + Geist sebagai teknologi pendukung.

---

# 🏗️ Frontend Stack Overview

```text
┌─────────────────────────────────────┐
│             Nuxt 4                  │
│          TypeScript                 │
├─────────────────────────────────────┤
│ Tailwind CSS v4     │   Nuxt UI     │
├─────────────────────┴───────────────┤
│ Pinia                               │
├─────────────────────────────────────┤
│ VeeValidate + Zod                   │
├─────────────────────────────────────┤
│ $fetch / ofetch                     │
├─────────────────────────────────────┤
│ Lucide + Geist                      │
└─────────────────────────────────────┘
```

---

# 1. Nuxt 4

## 1.1 Role

**Nuxt 4** merupakan framework utama frontend.

Nuxt digunakan untuk:

* Public landing page.
* Authenticated dashboard.
* Administrator interface.
* Routing.
* Rendering.
* Web application structure.

Nuxt dipilih karena memiliki integrasi yang kuat dengan Vue, file-based routing, TypeScript support, serta dukungan SSR/SSG apabila diperlukan.

---

## 1.2 Alternatives

Alternatif yang dipertimbangkan:

* Vue 3 + Vite murni.
* Next.js.

Vue 3 + Vite lebih minimal, tetapi membutuhkan lebih banyak keputusan integrasi.

Next.js tidak dipilih karena akan mengubah baseline framework frontend dari Vue menjadi React.

---

## 1.3 Trade-off

Keuntungan:

* Integrasi Vue yang kuat.
* Routing berbasis file.
* TypeScript support.
* SSR/SSG tersedia apabila diperlukan.
* Boilerplate lebih sedikit.

Trade-off:

* Abstraksi framework lebih besar dibandingkan setup Vue + Vite yang lebih minimal.
* Terdapat dependency terhadap ecosystem Nuxt.

---

## 1.4 Risk & Mitigation

**Risk:**

Perubahan major version dapat memengaruhi compatibility aplikasi.

**Mitigation:**

* Dependency dikunci melalui lockfile.
* Upgrade dilakukan secara terencana.
* Perubahan major version harus melalui evaluasi.

---

## 1.5 Technology Horizon

Nuxt 4 ditetapkan untuk horizon **jangka menengah-panjang**.

---

# 2. TypeScript

## 2.1 Role

**TypeScript** merupakan bahasa utama frontend.

TypeScript digunakan untuk:

* Type safety.
* API data contracts.
* Composables.
* Stores.
* Components.

TypeScript dipilih untuk mengurangi risiko yang dapat muncul dari domain aplikasi yang berkembang.

---

## 2.2 Alternative

Alternatif:

* JavaScript.

JavaScript lebih sederhana, tetapi memberikan lebih sedikit compile-time type safety dibandingkan TypeScript.

---

## 2.3 Trade-off

Keuntungan:

* Type safety.
* Kontrak data lebih eksplisit.
* Lebih mudah menjaga consistency pada aplikasi yang berkembang.

Trade-off:

* Membutuhkan type definitions.
* Membutuhkan disiplin tambahan dalam development.

---

## 2.4 Technology Horizon

TypeScript ditetapkan untuk horizon **jangka panjang**.

---

# 3. Tailwind CSS v4

## 3.1 Role

**Tailwind CSS v4** digunakan sebagai technology baseline untuk styling frontend.

Penggunaan utamanya:

* Utility-first styling.
* Design token implementation.
* Responsive design.
* Mobile-first implementation.
* UI consistency.

Tailwind CSS v4 dipilih untuk mendukung konsistensi dan produktivitas development.

---

## 3.2 Alternatives

Alternatif yang dipertimbangkan:

* CSS Modules.
* UnoCSS.

---

## 3.3 Trade-off

Keuntungan:

* Utility-first workflow.
* Responsive/mobile-first support.
* Produktivitas development.
* Mendukung konsistensi styling.

Trade-off:

* Markup dapat menjadi lebih panjang.
* Membutuhkan convention penggunaan yang konsisten.

---

## 3.4 Technology Horizon

Tailwind CSS v4 ditetapkan untuk horizon **jangka panjang**.

---

# 4. Nuxt UI

## 4.1 Role

**Nuxt UI** merupakan component library utama frontend.

Digunakan untuk:

* UI components.
* Form controls.
* Overlays.
* Feedback components.
* Interface patterns.

Nuxt UI dipilih karena memiliki integrasi dengan Nuxt dan membantu mempercepat implementasi interface.

---

## 4.2 Alternative

Alternatif yang dipertimbangkan:

* shadcn-vue.
* Custom component library.

---

## 4.3 Trade-off

Keuntungan:

* Integrasi dengan Nuxt.
* Mempercepat implementasi UI.
* Menyediakan component patterns yang konsisten.

Trade-off:

* Menambah dependency terhadap ecosystem Nuxt UI.

---

## 4.4 Technology Horizon

Nuxt UI ditetapkan untuk horizon **jangka menengah**.

---

# 5. Pinia

## 5.1 Role

**Pinia** digunakan sebagai state management untuk state global frontend.

Contoh state yang relevan:

* Session state.
* User context.
* Global application state yang memang membutuhkan centralized state.

Pinia dipilih karena merupakan state management yang modern dan idiomatik untuk Vue.

---

## 5.2 Alternative

Alternatif yang dipertimbangkan:

* Composables-only.
* Vuex.

---

## 5.3 State Management Principle

State global harus dibatasi hanya untuk state yang memang membutuhkan centralized management.

Tidak semua state harus dimasukkan ke Pinia.

```text
Component Local State
        │
        └──► Keep Local

Shared Application State
        │
        ▼
      Pinia
```

---

## 5.4 Trade-off

Keuntungan:

* Modern Vue state management.
* Idiomatik untuk ecosystem Vue.
* Mendukung centralized state ketika diperlukan.

Trade-off:

* State global harus dikontrol agar tidak berkembang secara tidak terstruktur.

---

## 5.5 Technology Horizon

Pinia ditetapkan untuk horizon **jangka panjang**.

---

# 6. VeeValidate + Zod

## 6.1 Role

**VeeValidate + Zod** digunakan untuk:

* Form validation.
* Schema validation.
* Declarative validation.
* Consistency validation.

Kombinasi ini dipilih untuk mendukung validasi deklaratif dan konsistensi schema.

---

## 6.2 Alternatives

Alternatif yang dipertimbangkan:

* Yup.
* Valibot.
* Custom validation.

---

## 6.3 Trade-off

Keuntungan:

* Declarative validation.
* Schema-based validation.
* Consistency.

Trade-off:

* Menambah dependency.
* Schema harus dijaga agar tetap sinkron.

---

## 6.4 Backend Validation Boundary

Validasi frontend **tidak menggantikan backend validation**.

```text
User Input
    │
    ▼
Frontend Validation
    │
    ▼
API Request
    │
    ▼
Backend Validation
    │
    ▼
Business Logic
```

Frontend validation terutama memberikan feedback dan membantu user experience.

Backend tetap menjadi trusted validation boundary.

---

# 7. `$fetch` / ofetch

## 7.1 Role

`$fetch` / **ofetch** digunakan sebagai HTTP client utama frontend.

Digunakan untuk komunikasi frontend dengan backend API.

Pemilihannya didasarkan pada integrasi Nuxt dan API ergonomics.

---

## 7.2 Alternatives

Alternatif:

* Axios.
* Native `fetch`.

---

## 7.3 Trade-off

Keuntungan:

* Integrasi dengan Nuxt.
* API ergonomics.
* Abstraksi HTTP yang sesuai dengan frontend stack.

Trade-off:

* Abstraction harus memiliki convention error handling yang konsisten.

---

## 7.4 API Contract

HTTP client tidak menjadi sumber kebenaran terhadap API.

Frontend tetap mengikuti API contract yang telah ditentukan.

```text
Nuxt Application
       │
       ▼
$fetch / ofetch
       │
       ▼
REST API
       │
       ▼
Backend
```

API contract tetap menjadi boundary komunikasi frontend dan backend.

---

## 7.5 Technology Horizon

`$fetch` / ofetch dianggap mudah untuk diganti selama **API contract tetap dipertahankan**.

---

# 8. Lucide + Geist

## 8.1 Lucide

**Lucide** digunakan sebagai icon system frontend.

Tujuan:

* Konsistensi icon.
* Developer experience.
* Unified visual language.

---

## 8.2 Geist

**Geist** digunakan sebagai font utama.

Fallback font wajib disediakan untuk menjaga typography tetap usable apabila font utama tidak tersedia.

---

# 🧩 Frontend Technology Summary

| Technology        | Role                   | Horizon                                 |
| ----------------- | ---------------------- | --------------------------------------- |
| Nuxt 4            | Frontend framework     | Jangka menengah-panjang                 |
| TypeScript        | Frontend language      | Jangka panjang                          |
| Tailwind CSS v4   | Styling                | Jangka panjang                          |
| Nuxt UI           | Component library      | Jangka menengah                         |
| Pinia             | State management       | Jangka panjang                          |
| VeeValidate + Zod | Form/schema validation | —                                       |
| `$fetch` / ofetch | HTTP client            | Mudah diganti selama API contract tetap |
| Lucide            | Icon system            | —                                       |
| Geist             | Primary font           | —                                       |

Technology choices tersebut merupakan bagian dari frontend technology baseline yang ditetapkan dalam TSS.

---

# 🚧 Frontend Technology Constraints

Implementasi frontend harus mematuhi constraint berikut:

* Nuxt 4 menjadi framework utama.
* TypeScript menjadi bahasa utama frontend.
* Tailwind CSS v4 menjadi styling baseline.
* Nuxt UI menjadi component library utama.
* Pinia digunakan untuk shared/global state yang memang membutuhkan centralized state.
* VeeValidate + Zod digunakan untuk form dan schema validation.
* Frontend validation tidak menggantikan backend validation.
* `$fetch` / ofetch digunakan sebagai HTTP client utama.
* API communication harus mengikuti API contract.
* Lucide digunakan sebagai icon system.
* Geist digunakan sebagai font utama dengan fallback font.
* Dependency tambahan harus mengikuti Technology Governance.
* Dependency version harus dikunci melalui lockfile.

---

# 🔗 Relationship with Other Documents

```text
TSS-001
Technology Governance
       │
       ▼
TSS-002
Frontend Technology Stack
       │
       ├── Nuxt 4
       ├── TypeScript
       ├── Tailwind CSS v4
       ├── Nuxt UI
       ├── Pinia
       ├── VeeValidate + Zod
       ├── $fetch / ofetch
       └── Lucide + Geist
```

Frontend implementation juga harus mengikuti:

* SDS untuk architecture.
* AHS untuk architecture hardening.
* API Specification untuk API contract.
* IDR untuk implementation guidelines.

---

# 📚 Related Documents

* [TSS README](./README.md)
* [Technology Governance](./01-tss-001-technology-governance.md)
* [Backend & Database Stack](./03-tss-003-backend-and-database-stack.md)
* [Infrastructure, Security & Observability](./04-tss-004-infrastructure-security-and-observability.md)
* [Development, Testing & Coding Standards](./05-tss-005-development-testing-and-coding-standards.md)
* [Technology Decision & Final Baseline](./06-tss-006-technology-decision-and-final-baseline.md)
* [SDS README](../SDS/README.md)
* [AHS README](../AHS/README.md)
* [IDR README](../IDR/README.md)

---

# 🔄 Traceability Matrix

| Concern            | Technology        |
| ------------------ | ----------------- |
| Frontend Framework | Nuxt 4            |
| Language           | TypeScript        |
| Styling            | Tailwind CSS v4   |
| Component Library  | Nuxt UI           |
| State Management   | Pinia             |
| Form Validation    | VeeValidate + Zod |
| HTTP Client        | `$fetch` / ofetch |
| Icon System        | Lucide            |
| Typography         | Geist             |

---

# ✅ Review Checklist

* [ ] Nuxt 4 telah ditetapkan sebagai frontend framework.
* [ ] TypeScript telah ditetapkan sebagai frontend language.
* [ ] Tailwind CSS v4 telah ditetapkan sebagai styling baseline.
* [ ] Nuxt UI telah ditetapkan sebagai component library.
* [ ] Pinia telah ditetapkan sebagai state management.
* [ ] VeeValidate + Zod telah ditetapkan sebagai validation stack.
* [ ] `$fetch` / ofetch telah ditetapkan sebagai HTTP client.
* [ ] Lucide telah ditetapkan sebagai icon system.
* [ ] Geist telah ditetapkan sebagai primary font.
* [ ] Frontend validation boundary telah dijelaskan.
* [ ] API contract boundary telah dipertahankan.
* [ ] Technology constraints telah didokumentasikan.
* [ ] Technology governance telah dirujuk.

---

# 📝 Revision History

| Version | Date       | Description                                     | Author          |
| ------- | ---------- | ----------------------------------------------- | --------------- |
| 1.0     | 2026-07-21 | Initial Frontend Technology Stack documentation | Abidzar Dzakwan |
