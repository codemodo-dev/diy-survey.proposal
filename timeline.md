# Timeline: DIY Survey & Panel Platform

*v1.0.0 | 27 Februari 2026*

---

## Asumsi

- **Tim:** 3 role — Backend, Frontend, Designer
- **Kapasitas:** 20 jam/minggu per role (Senin–Jumat, 4 jam/hari)
- **Total kapasitas tim:** 60 jam/minggu

| Role | Kapasitas/Minggu |
|------|-----------------|
| Backend Developer | 20 jam |
| Frontend Developer | 20 jam |
| UI/UX Designer | 20 jam |
| **Total Tim** | **60 jam/minggu** |

---

## MVP (Phase 1–5)

| Phase | Durasi | Keterangan |
|-------|--------|------------|
| Phase 1 — Core System | 2 bulan | Auth, admin panel, audit log |
| Phase 2 — Survey Designer | 6–8 bulan | Questionnaire builder, logic, piping, quota, targeting, publish |
| Phase 3 — Survey Portal | 3–4 bulan | Panelist registration, survey rendering, eligibility, incentive, email invitation |
| Phase 4 — Analytics & Disbursement | 2–3 bulan | Analytics, fieldwork monitoring, data export, Xendit disbursement |
| Phase 5 — Fraud Detection | 2 bulan | Semua mekanisme fraud detection, audit log survey |
| **Total MVP** | **15–18 bulan** | **🚀 MVP Release** |

---

## Post-MVP

Dikerjakan setelah MVP berjalan stabil. Total durasi ~4 bulan.

**Urutan didasarkan pada:** isolasi dependency (yang tidak bergantung fitur lain dikerjakan duluan), kedekatan logic antar fitur (dikerjakan paralel), dan bobot kompleksitas (paling berat di akhir setelah base stabil).

### Bulan 1 — Question Types Lanjutan & Notifikasi

Dua fitur ini terisolasi — tidak ada dependency ke fitur Post-MVP lain, sehingga aman dikerjakan pertama.

| Fitur | Scope |
|-------|-------|
| **Question Types Lanjutan** | Auto-complete appearance (SA & MA); Grid question types: GRID_SA, GRID_MA, GRID_OE, GRID_NUMBER |
| **Notifikasi** | Sistem notifikasi in-app, email, dan push untuk seluruh role |

### Bulan 2 — Import Survey & Template Library

Keduanya share logic di questionnaire builder — Import Survey menghasilkan struktur survei, Template Library menyimpannya. Paling efisien dikerjakan bersamaan.

| Fitur | Scope |
|-------|-------|
| **Import Survey** | Import struktur survei dari file eksternal ke dalam platform |
| **Template Library** | Global template oleh Admin; private template per workspace Client |

### Bulan 3–4 — Client Dashboard & Multi-language Support

Client Dashboard adalah fitur terberat di Post-MVP — butuh data layer di atas analytics yang sudah ada, plus UI kompleks. Multi-language dikerjakan paralel di periode ini karena baru bisa dimulai setelah seluruh UI komponen sudah final.

| Fitur | Scope |
|-------|-------|
| **Client Dashboard** | Dashboard builder drag-and-drop per survei; Crosstab analysis (tabel matrix); NPS / Top2Box / Mean tagging & kalkulasi otomatis |
| **Multi-language Support** | Konten survei dalam beberapa bahasa; UI aplikasi tetap English |

### Summary

| Bulan | Fitur | Durasi |
|-------|-------|--------|
| 1 | Question Types Lanjutan + Notifikasi | 1 bulan |
| 2 | Import Survey + Template Library | 1 bulan |
| 3–4 | Client Dashboard + Multi-language Support | 2 bulan |
| | **Total Post-MVP** | **4 bulan** |

---

## Milestone

| Milestone | Estimasi |
|-----------|----------|
| 🚀 MVP Release | Bulan 15–18 |
| Post-MVP | +4 bulan setelah MVP stabil |
