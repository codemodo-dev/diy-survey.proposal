# Scope of Work: DIY Survey & Panel Platform

*v1.0.0 | 27 Februari 2026*

---

## Authentication

Semua area (Admin Panel, Survey Designer, Survey Portal) menggunakan mekanisme autentikasi yang sama:
- Login via email/password dan SSO Google
- Forgot password — reset via email
- Sesi dikelola secara stateful di server-side

---

## Scope MVP & Post-MVP

**MVP = Phase 1–5** (15–18 bulan) — mencakup Core System, Survey Designer, Survey Portal, Analytics & Disbursement, dan Fraud Detection. Platform sudah dapat berjalan end-to-end secara penuh termasuk kualitas response data.

---

### Phase 1 — Core System

**Focus:** Foundation platform — authentication, user management, dan infrastruktur dasar sebelum fitur survei dibangun.

#### 1. Authentication
- Login email/password dan SSO Google
- Forgot password — reset via email
- Stateful session — sesi dikelola server-side, memungkinkan revoke akses secara real-time (logout paksa, invalidasi sesi aktif)
- Device awareness — mendeteksi dan mencatat perangkat yang digunakan saat login (device type, OS, browser)
- IP tracking — mencatat dan memonitor IP address setiap sesi
- Location tracking — mencatat lokasi akses berdasarkan data yang tersedia

#### 2. Admin Panel — Admin Area
- User management — create, edit, deactivate user (Super Admin & Admin)
- Permission management — Super Admin mengonfigurasi permission per role atau per individu Admin
- Client data management — create, edit client account

#### 3. Admin Panel — Client Area
- Project list — Client dapat melihat dan mengelola project milik sendiri
- Member management — Client dapat mengundang dan mengelola member dalam workspace

#### 4. Audit Log
- System activity — login, logout, session revoke, failed attempts
- User activity — create/edit/deactivate user, permission change

---

### Phase 2 — Survey Designer

**Focus:** Core produk — membangun, mengonfigurasi, dan mempublikasikan survei. Pada phase ini panelist belum tersedia — targeting dan quota dikonfigurasi di sini, namun pencocokan dengan panelist baru berjalan setelah Survey Portal dibangun.

#### 1. Survey Designer
- Questionnaire builder dengan reorder pertanyaan via drag-and-drop; tambah pertanyaan via klik/button
- **Randomization** — random urutan answer options per pertanyaan
- **Validation rules** — konfigurasi per pertanyaan:
  - Required / Optional
  - Min / Max karakter untuk OE
  - Min / Max value untuk NUMBER
  - Min / Max selection untuk MA
  - Format validation untuk OE Email appearance
- **Quota** — set batas maksimum response per survei; response tidak dapat melebihi quota yang ditentukan
- **Panelist targeting & eligibility** — client set kriteria demografis saat setup survei; system match panelist berdasarkan kriteria tersebut saat survei dipublish

| Field | Deskripsi |
|-------|-----------|
| `gender` | Gender panelist |
| `dateOfBirth` | Tanggal lahir, dapat digunakan untuk targeting berdasarkan age range |
| `maritalStatus` | Status pernikahan |
| `lastEducation` | Pendidikan terakhir |
| `province` | Provinsi |
| `district` | Kabupaten / Kota |
| `subdistrict` | Kecamatan |
| `village` | Kelurahan / Desa |

Targeting lokasi hanya dapat dipilih di satu level — jika memilih Province, maka District, Subdistrict, dan Village tidak dapat dipilih, begitu pula sebaliknya. Dalam satu level, client dapat memilih multiple item dengan maksimum 10 item.

- **Survey status lifecycle:**

| Status | Deskripsi |
|--------|-----------|
| `DRAFT` | Survei belum dipublish, masih dalam tahap penyusunan |
| `TEST` | Mode testing sebelum live; hanya 5 panelist yang di-assign yang dapat mengisi |
| `PUBLISHED` | Survei aktif dan menerima response dari panel |
| `CLOSED` | Survei selesai, tidak menerima response baru |
| `CANCELED` | Survei dibatalkan |

- **Survey distribution mode:**

| Mode | Deskripsi |
|------|-----------|
| `AUTO` | Survei didistribusikan otomatis ke seluruh panelist yang terdaftar |
| `CRITERIA` | Survei hanya dikirim ke panelist yang secara manual di-assign |

- **Delete** — project dan survei dapat dihapus permanen; tidak ada soft delete
- **Shareable link** — dapat dikonfigurasi public (siapa saja bisa akses) atau restricted (harus terdaftar sebagai panelist)

**Question Types & Appearance**

| Question Type | Code | Appearance Options |
|---------------|------|--------------------|
| Single Answer | `SA` | Radio, Dropdown |
| Multiple Answer | `MA` | Checkbox, Dropdown |
| Open-ended | `OE` | Single line, Medium, Large, Email |
| Date | `DATE` | Date & Time, Date only |
| Number | `NUMBER` | Number |
| Blank / Info | `BLANK` | Text (display only) |
| Ranking | `RANK` | Drag-and-drop |

**Logic**

| Kategori | Options |
|----------|---------|
| **Logic Type** | Jump to question (hanya bisa maju, tidak bisa jump ke pertanyaan sebelumnya), Disqualified, Completed |
| **Condition — String** | Equal, Not equal, In, Not in |
| **Condition — Number** | Equal, Not equal, Less than, Less than or equal, More than, More than or equal |
| **Logic Process** | Pre (sebelum pertanyaan ditampilkan), Post (setelah jawaban diisi) |
| **Question Index** | Insert before, Insert after, New |

**Piping**

Piping memungkinkan jawaban dari pertanyaan sebelumnya ditampilkan otomatis di dalam teks pertanyaan berikutnya. Piping dapat digunakan langsung di dalam konten pertanyaan:

- **Single answer (SA)** — nilai jawaban langsung di-inject ke teks
- **Multiple answer (MA) / Grid** — beberapa jawaban digabung dengan pemisah koma (`,`)

| Kondisi | Deskripsi |
|---------|-----------|
| `IS_ANSWERED` | Tampilkan pertanyaan berikutnya jika pertanyaan referensi sudah dijawab |
| `NOT_ANSWERED` | Tampilkan pertanyaan berikutnya jika pertanyaan referensi belum dijawab |

**Answer Option Flags**

| Flag | Deskripsi |
|------|-----------|
| `exclusive` | Jika opsi ini dipilih, semua pilihan lain otomatis di-deselect (e.g. "None of the above") |
| `otherSpecify` | Jika opsi ini dipilih, memunculkan input teks bebas untuk jawaban tambahan (e.g. "Other, please specify") |

---

### Phase 3 — Survey Portal

**Focus:** Panelist-facing — registrasi, pengisian survei, eligibility enforcement, dan pencatatan insentif.

#### 1. Survey Portal
- Panelist registration & profile management. Field profil demografis:
  - Gender, date of birth, marital status, last education
  - Lokasi: province, district, subdistrict, village
- Survey list — panelist dapat melihat daftar survei yang tersedia dan relevan untuk mereka
- Survey invitation via email — panelist menerima undangan survei melalui email
- Eligibility check berdasarkan targeting criteria demografis yang sudah diset di Phase 2
- Survey rendering & response submission
- Response validation — memvalidasi kelengkapan dan format jawaban sesuai validation rules yang dikonfigurasi di Phase 2 (required field, min/max karakter, format email, dll)

#### 2. Behavior-based Targeting
Sebagai tambahan dari demographic targeting, client dapat men-target panelist berdasarkan aktivitas mereka di platform:
- Pernah mengisi survei kategori tertentu
- Terakhir aktif dalam rentang waktu tertentu
- Sudah complete minimal X survei
- Belum pernah mengisi survei dari client tertentu

#### 3. Incentive
- Setiap survei memiliki nominal insentif yang ditentukan oleh Admin
- Panelist yang berhasil complete survei mendapatkan insentif ke balance akun mereka secara otomatis
- Balance tercatat secara internal — pencairan ke rekening tersedia di Phase 4

#### 4. Admin Panel — Panel List
- Panel list management — Admin dan Super Admin dapat melihat dan mengelola seluruh data panelist yang terdaftar

---

### Phase 4 — Analytics, Fieldwork & Disbursement

**Focus:** Data & insights untuk Client, monitoring fieldwork, dan pencairan insentif panelist.

#### 1. Fieldwork Monitoring
Built-in di Survey Designer:

- Progress response secara real-time
- Quota status per segment

#### 2. Survey Analytics
Built-in di Survey Designer, dengan layout fixed per default:

- Summary: total responses, completion rate, incidence rate (persentase panelist yang eligible dan berhasil complete survei dari total yang mencoba), average completion time
- Response trend per hari (chart)
- Quota progress per segment
- Chart & frequency table per pertanyaan (otomatis sesuai question type)
- Dropout rate per pertanyaan — drop-off funnel untuk mengidentifikasi pertanyaan mana yang paling banyak ditinggalkan responden
- Filter by segment — mendukung multi-filter (kombinasi beberapa kondisi sekaligus); semua chart dan frequency table terupdate secara otomatis sesuai filter yang dipilih

#### 3. Admin Panel — Global Overview
- Global overview: total client, project, dan survei aktif di platform
- Client & project performance monitoring

#### 4. Data Export
Tersedia dua tipe export dalam format CSV:

| Tipe | Deskripsi |
|------|-----------|
| **Raw data** | Satu baris per respondent, kolom horizontal per pertanyaan, include data respondent |
| **Summary** | Hasil agregasi sesuai tampilan analytics (frequency, percentage, dll) |

#### 5. Disbursement — Xendit Integration
- Integrasi payment gateway menggunakan Xendit
- Panelist dapat mencairkan balance ke rekening
- Minimum withdrawal dapat dikonfigurasi oleh Super Admin dan Admin (sesuai permission); default Rp 50.000

---

### Phase 5 — Fraud Detection

**Focus:** Peningkatan kualitas response — semua mekanisme deteksi fraud dibangun sekaligus di phase ini.

| Mekanisme | Deskripsi |
|-----------|-----------|
| **Duplicate response** | Satu panelist tidak dapat mengisi survei yang sama lebih dari satu kali |
| **Speeder detection** | Response yang selesai jauh di bawah average completion time dianggap tidak valid |
| **Straight-lining** | Pola jawaban yang selalu memilih opsi di posisi yang sama secara berurutan dianggap tidak valid |
| **IP & device duplicate** | Satu IP atau device tidak dapat mengisi survei yang sama lebih dari satu kali |

#### Audit Log — Survey Activity
- Mencatat seluruh aktivitas survei: create, publish, close, delete, dan perubahan status survei

---

## Post-MVP

Fitur-fitur berikut tidak masuk scope MVP, dan akan dikerjakan setelah platform MVP berjalan stabil (~4 bulan). Urutan fase di bawah sudah mencerminkan urutan pengerjaan yang direkomendasikan.

### 1. Question Types Lanjutan & Notifikasi
- Auto-complete appearance untuk SA dan MA
- Grid question types: GRID_SA, GRID_MA, GRID_OE, GRID_NUMBER
Sistem notifikasi untuk seluruh role — definisi trigger, channel (in-app, email, push), dan target penerima akan ditentukan setelah platform MVP berjalan.

### 2. Import Survey & Template Library
- Import struktur survei d
- Admin dapat membuat global template yang visible ke semua client
- Client dapat menyimpan survei sebagai private template, hanya visible di workspace sendiri
- Template dapat digunakan sebagai starting point saat membuat survei baru

### 3. Client Dashboard (extension dari Survey Designer)
Fitur kustomisasi analytics lanjutan — dikembangkan sebagai extension dari Survey Designer setelah MVP stabil:
- **Dashboard builder** — kustomisasi layout widget per survei (drag-and-drop), tambah, hapus, dan resize widget secara bebas
- **Crosstab analysis** — analisis silang antara dua pertanyaan; hasil ditampilkan dalam bentuk tabel matrix
- **NPS, Top2Box, dan Mean** — client men-tag pertanyaan mana yang dihitung, system kalkulasi otomatis berdasarkan tag tersebut

### 4. Multi-language Support
- Admin dan Client dapat menambahkan konten survei dalam beberapa bahasa; UI aplikasi menggunakan English only
