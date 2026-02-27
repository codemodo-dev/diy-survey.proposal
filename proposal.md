# Project Proposal: DIY Survey & Panel Platform

*v1.0.0 | 27 Februari 2026*

---

## Overview

Proyek ini bertujuan membangun platform survei berbasis web yang memungkinkan client menjalankan survei secara mandiri — mulai dari menyusun kuesioner, mendistribusikannya ke panel responden, hingga memantau dan menganalisis hasilnya secara real-time.

---

## User Roles

| Role | Deskripsi |
|------|-----------|
| **Super Admin** | Akses penuh ke seluruh Admin Panel — user management, client data, panel list, dan konfigurasi permission Admin. Tidak dapat membuat project atau mengakses Survey Designer. |
| **Admin** | Akses ke Admin Panel sesuai permission yang dikonfigurasi oleh Super Admin. Permission bersifat configurable per role atau per individu. Tidak dapat membuat project atau mengakses Survey Designer. |
| **Client** | Akses ke Admin Panel untuk mengelola project milik sendiri (terisolasi per tenant) dan Survey Designer untuk membangun serta mempublikasikan survei. |
| **Panelist** | Akses ke Survey Portal untuk registrasi, melengkapi profil, dan mengisi survei yang tersedia. |

---

## System Overview

Platform dibangun sebagai satu sistem monolith terintegrasi dengan tampilan yang berbeda berdasarkan role pengguna.

**Admin Panel**
Diakses oleh Super Admin, Admin, dan Client dalam satu platform — dengan sidebar, menu, dan permission yang berbeda per role.

| Role | Yang Terlihat di Admin Panel |
|------|------------------------------|
| Super Admin | User management, client data, panel list, permission management |
| Admin | Sesuai permission yang dikonfigurasi Super Admin |
| Client | Project milik sendiri saja (terisolasi per tenant), invite & manage member workspace |

**Survey Designer**
Hanya dapat diakses oleh Client. Digunakan untuk membangun, mengonfigurasi, dan mempublikasikan survei, serta memonitor fieldwork dan menganalisis hasil.

Survey Designer adalah tool utama Client — seluruh siklus survei dari build hingga analytics berjalan di sini. Ke depannya, fitur **Client Dashboard** (visualisasi analytics dengan layout kustomisasi dinamis) akan dikembangkan sebagai extension dari Survey Designer.

Admin yang ingin mengakses Survey Designer harus memiliki akun Client tersendiri.

**Survey Portal**
Hanya dapat diakses oleh Panelist. Digunakan untuk registrasi, melengkapi profil demografis, dan mengisi survei yang tersedia.

---

## User Flows

### Super Admin & Admin
```
Login → Admin Panel
         ↓
Manage Users & Permission (Super Admin only)
         ↓
Manage Client Data
         ↓
View & Manage Panel List
         ↓
Monitor Platform Activity & Audit Log
```

### Client
```
Login → Admin Panel
         ↓
Create & Manage Project
         ↓
Enter Survey Designer
         ↓
Build Questionnaire → Set Logic, Quota, Targeting
         ↓
Publish Survey
         ↓
Monitor Fieldwork (real-time)
         ↓
Analyze Results & Export Data
```

### Panelist
```
Register & Complete Profile (Survey Portal)
         ↓
Browse Available Surveys
         ↓
Check Eligibility
         ↓
Fill Survey
         ↓
Submit Response → Receive Incentive
```

---

## Deliverables

### Per Phase
Diserahkan setiap akhir phase untuk keperluan review dan acceptance:

| # | Deliverable | Keterangan |
|---|-------------|------------|
| 1 | **Backend API** | REST API untuk seluruh fitur per phase |
| 2 | **Frontend Web** | Aplikasi web sesuai scope phase |
| 3 | **API Documentation** | API spec untuk seluruh endpoint per phase — versi draft untuk review |
| 4 | **Figma Design** | Akses view mode file Figma per phase — untuk keperluan review UI |

### Final (Setelah MVP Complete)
Diserahkan setelah seluruh phase MVP selesai dan diterima:

| # | Deliverable | Keterangan |
|---|-------------|------------|
| 1 | **Source Code** | Repository Git fresh — tanpa commit history pengerjaan |
| 2 | **API Documentation** | Versi final lengkap seluruh endpoint |
| 3 | **User Guide** | Panduan penggunaan aplikasi lengkap dari sisi user |
| 4 | **FAQ** | Frequently asked questions seputar penggunaan platform |
| 5 | **Figma Design** | File Figma final yang dapat di-import secara mandiri |
| 6 | **Production Deployment** | Deploy aplikasi ke server yang telah disiapkan oleh client |

---

## Timeline Ringkas

| Phase | Durasi | Fokus |
|-------|--------|-------|
| Phase 1 — Core System | 2 bulan | Auth, admin panel, audit log |
| Phase 2 — Survey Designer | 6–8 bulan | Questionnaire builder, logic, quota, publish |
| Phase 3 — Survey Portal | 3–4 bulan | Panelist, eligibility, insentif |
| Phase 4 — Analytics & Disbursement | 2–3 bulan | Analytics, export, Xendit |
| Phase 5 — Fraud Detection | 2 bulan | Fraud detection, audit log survei |
| **Total MVP (Phase 1–5)** | **15–18 bulan** | **🚀 MVP Release** |
| Post-MVP | 4 bulan | Question types, client dashboard, notifikasi, import survey, template, multi-language |

---

## Out of Scope

- **Mobile app** — platform hanya tersedia dalam versi web; tidak ada native iOS atau Android app
- **Landing page** — tidak termasuk halaman marketing atau landing page publik
- **Third-party integrations** — tidak ada integrasi dengan tools atau platform eksternal di luar yang sudah didefinisikan dalam proposal ini (e.g. Salesforce, Tableau)
- **Public API** — tidak tersedia public API untuk di-consume oleh client secara langsung
- **Pricing & billing** — tidak ada kalkulasi harga, invoicing, atau billing system
- **Server production cost** — biaya server dan infrastruktur production ditanggung langsung oleh client, tidak termasuk dalam biaya pengerjaan
- **Data migration** — pemindahan data existing milik client ke platform baru di luar scope pengerjaan
- **Ongoing maintenance** — setelah periode warranty selesai, maintenance dan operasional platform sepenuhnya menjadi tanggung jawab client

---

## Post-Handover

### Support Period
Setelah MVP delivered dan diterima, tim pengembang akan aktif membantu client dalam menjalankan project pertama di platform. Support period berakhir ketika **salah satu kondisi berikut terpenuhi lebih dulu:**
- Project pertama client selesai, atau
- 3 bulan sejak project pertama dimulai

Support mencakup: konsultasi penggunaan platform, guidance operasional, dan penyesuaian konfigurasi.

### Warranty Period
Setelah support period selesai, warranty period berjalan selama **3 bulan**. Selama periode ini, tim pengembang akan memperbaiki bug yang ditemukan tanpa biaya tambahan.

Ketentuan warranty:
- Berlaku untuk bug yang berasal dari kode yang diserahkan tim pengembang
- **Hangus** jika client melakukan modifikasi pada source code
- Tidak mencakup penambahan fitur baru — akan dikenakan biaya terpisah
- Setelah warranty period selesai, seluruh maintenance menjadi tanggung jawab client
