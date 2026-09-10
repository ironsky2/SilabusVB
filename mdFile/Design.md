# 🎨 Spesifikasi Sistem Desain & Panduan UI/UX (Real Project)
**Proyek: Vibe Coding Masterclass — 16 Pertemuan & Laboratorium Interaktif**

Dokumen ini adalah **Design System & UI/UX Guidelines resmi** yang merefleksikan 100% implementasi nyata pada seluruh halaman web (HTML/CSS/JS) di repositori ini.

---

## 1. 📌 Ikhtisar & Status Kesesuaian

Seluruh modul dan halaman web dalam proyek ini menerapkan standar desain **Notion-meets-Stripe Clean Aesthetic** dengan struktur pedagogis modular 3-bagian (*Tripartite Architecture*). 

### 🗂️ Cakupan Halaman Aktif (12 File HTML):
1. [index.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/index.html) / [silabus-vibe-coding-16-pertemuan.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/silabus-vibe-coding-16-pertemuan.html) — *Master Syllabus & Dashboard 16 Pertemuan*
2. [kurikulum_vibe_coding_pwa_supabase.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/kurikulum_vibe_coding_pwa_supabase.html) — *Kurikulum PWA & Supabase*
3. [praktikum_git_sebelum_coding.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/praktikum_git_sebelum_coding.html) — *Modul 01: Git & GitHub Setup*
4. [panduan_push_github.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/panduan_push_github.html) — *Modul 02: Panduan Praktis Push GitHub*
5. [teori_integrasi_api_eksternal.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/teori_integrasi_api_eksternal.html) — *Modul 03: Teori & Mental Model API Eksternal*
6. [praktikum_integrasi_api_eksternal.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/praktikum_integrasi_api_eksternal.html) — *Modul 04: Praktikum Integrasi API Eksternal*
7. [panduan_keamanan_data.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/panduan_keamanan_data.html) — *Modul 05: Keamanan Data, Env Vars & API Key*
8. [laboratorium_lengkap_supabase.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/laboratorium_lengkap_supabase.html) — *Modul 06: Fullstack Database, Auth & PWA Supabase*
9. [praktikum_flutter_supabase.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/praktikum_flutter_supabase.html) — *Modul 07: Vibe Coding Mobile App Flutter + Supabase*
10. [panduan_deploy_vercel.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/panduan_deploy_vercel.html) — *Modul 08: Deploy Web App ke Vercel*
11. [panduan_vibecoding_owasp.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/panduan_vibecoding_owasp.html) — *Modul 09: Standar Keamanan OWASP Top 10*
12. [favicon.svg](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/favicon.svg) & [_redirects](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/_redirects) — *Aset Favicon & Konfigurasi Hosting / Routing*

---

## 2. 🎨 Palet Warna & CSS Design Tokens (`:root`)

Sistem warna dirancang harmonis dengan basis Teal/Seafoam yang segar, dikombinasikan dengan warna semantik untuk status, peringatan, dan diferensiasi materi.

```css
:root {
  /* Brand Primary Colors */
  --teal: #028090;          /* Warna aksen utama / tombol / highlight */
  --seafoam: #00A896;       /* Sub-aksen / dot indicator / badge aktif */
  --mint: #02C39A;          /* Selection highlight & success tint */
  --deep: #01656F;          /* Hover state tautan & aksen gelap */

  /* Semantic Alerts & Badges */
  --amber: #B45309;         /* Peringatan / catatan penting */
  --amber-bg: #FEF3C7;      /* Background alert peringatan */
  --rose: #BE123C;          /* Bahaya / anti-pattern / error */
  --rose-bg: #FFF1F2;       /* Background alert bahaya */
  --emerald: #047857;       /* Sukses / output valid / praktik benar */
  --emerald-bg: #ECFDF5;    /* Background alert sukses */
  --indigo: #4F46E5;        /* Insight arsitektur / prompt AI */
  --indigo-bg: #EEF2FF;     /* Background prompt & syntax info */
  --purple: #7E22CE;        /* Topik khusus / otentikasi */
  --purple-bg: #FAF5FF;     /* Background topik khusus */

  /* Neutrals (Notion Typography & Surface) */
  --ink: #37352F;           /* Teks utama (High contrast dark gray) */
  --ink-mid: #5A5852;       /* Teks sekunder / keterangan */
  --ink-soft: #787774;      /* Teks tersier / metadata / placeholder */
  --line: #EDECE9;          /* Border & divider utama */
  --line-soft: #F1F0ED;     /* Border kartu sekunder */
  --bg: #FFFFFF;            /* Canvas background */
  --bg-soft: #F7F6F3;       /* Card background / sidebar tint */
  --bg-tint: #EAF6F4;       /* Primary light tint */

  /* Border Radii */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 14px;
}
```

---

## 3. 🔤 Tipografi & Skala Font (Typography Stack)

Menggunakan kombinasi 3 keluarga font Google Fonts berkelas editorial:

| Kategori | Font Family | Penggunaan di Web |
| :--- | :--- | :--- |
| **Editorial Serif** | `'Newsreader', Georgia, serif` | `h1`, `h2`, `h3`, `.hero-title`, `.brand-title`, `.section-heading` |
| **Clean UI Sans** | `'Inter', -apple-system, sans-serif` | `body`, navigasi, tombol, badge, deskripsi, tabel |
| **Monospace Code** | `'JetBrains Mono', Consolas, monospace` | `code`, `pre`, diagram ASCII/arsitektur, terminal snippet |

```html
<!-- Import Font Standar -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,600;0,6..72,700;1,6..72,400&display=swap" rel="stylesheet">
```

---

## 4. 🧭 Navigasi Terpadu (Notion-Style Header & Popover Menu)

Header dibuat *sticky* dengan efek *glassmorphism* (`backdrop-filter: blur(12px)`) setinggi `56px` dengan lebar kontainer maksimal `1160px`.

### Elemen Utama Header:
1. **Brand**:
   - `.brand .dot`: Indikator status berwarna teal/seafoam dengan animasi *pulse ring*.
   - `.brand-title`: Teks serif berbobot 700 (*Vibe Coding*).
   - `.brand-pill`: Badge versi/status kecil (`Modul X`, `16 Pertemuan`, atau `Masterclass`).
2. **Dropdown Menu Navigasi Modul (`.nav-dropdown`)**:
   - Menu *popover hover/focus* berisi seluruh tautan silabus dan modul praktikum lengkap dengan ikon dan status `.active`.
3. **CTA / Quick Action (`.nav-cta-btn`)**:
   - Tombol cepat ke repositori, modul berikutnya, atau materi praktikum utama.

---

## 5. 🧱 Struktur Desain Modular (Pedagogical Tripartite Model)

Setiap modul praktikum disusun dengan struktur **3-Bagian Terstandarisasi** untuk memudahkan mahasiswa non-IT memahami materi:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. HERO HEADER                                              │
│    • .eyebrow (Kategori/Pertemuan)                          │
│    • h1.hero-title (Judul Serif)                            │
│    • p.lede (Penjelasan Manfaat)                            │
│    • .meta-badge-row (Waktu, Level, Prasyarat)              │
├─────────────────────────────────────────────────────────────┤
│ 2. QUICK NAV & ARCHITECTURE ROADMAP                         │
│    • .lab-nav-sticky (Pill Navigasi Cepat Horizontal)       │
│    • .arch-card & .arch-diagram (Peta Konsep Monospace)     │
├─────────────────────────────────────────────────────────────┤
│ 3. TRI-PART PERTEMUAN / FASE:                               │
│    ├── PART A · Teori Pedagogis & Mental Model               │
│    │   (Analogi Sederhana, Tabel Komparasi, Dosa Besar)     │
│    ├── PART B · Master Prompts AI & Source Code             │
│    │   (.prompt-card, .code-card, Tombol .btn-copy)         │
│    └── PART C · Output, Verifikasi & Pitfalls               │
│        (.res-output Hijau, .res-pitfall Merah, Checklist)   │
└─────────────────────────────────────────────────────────────┘
```

### Detail Komponen Spesifik:
* **Analogy Box (`.analogy-box`)**: Kotak perumpamaan dunia nyata berlatar kuning/krem lembut (`--amber-bg`) dengan ikon khusus.
* **Architecture Diagram (`.arch-card` & `.arch-diagram`)**: Visual alur data monospace (`JetBrains Mono`) dengan kotak penjelasan intisari (`.arch-insight`).
* **Prompt Card (`.prompt-card`)**: Kartu prompt AI dengan tag prompt builder (K-T-B-H / 5 Elemen), format monospace, dan tombol `.btn-copy` interaktif.
* **Code Card (`.code-card`)**: Blok kode dengan header filename/bahasa dan styling syntax yang kontras.
* **Output vs Pitfall Grid (`.result-grid`)**:
  - `.res-output` (Border hijau `--emerald`, ikon checklist, output terminal/UI yang benar).
  - `.res-pitfall` (Border merah `--rose`, ikon warning, jebakan umum dan solusinya).

---

## 6. ⚡ Fitur Interaktif & Script Standar

1. **Clipboard Copy with Toast Notification**:
   ```javascript
   function copySnippet(elementId) {
     const codeText = document.getElementById(elementId).innerText;
     navigator.clipboard.writeText(codeText).then(() => {
       showToast("Snippet berhasil disalin ke clipboard!");
     });
   }
   ```
2. **Toast Feedback Element (`#toast`)**:
   Popup melayang di sudut kanan bawah dengan transisi *fade & slide up* saat tombol salin ditekan.
3. **Smooth Scroll Offset**:
   `html { scroll-behavior: smooth; scroll-padding-top: 100px; }` memastikan judul seksi tidak tertutup sticky header saat tautan navigasi diklik.

---

## 7. 📱 Responsivitas & Standar Cetak (Print Stylesheet)

* **Mobile Breakpoint (`@media (max-width: 768px)`)**:
  - Grid kolom otomatis menjadi *single-column*.
  - `.lab-nav-sticky` mendukung *horizontal scroll* dengan *scrollbar-hidden*.
  - Ukuran `h1.hero-title` menyesuaikan skala ke `26px - 32px`.
* **Print Optimization (`@media print`)**:
  - Sticky nav disembunyikan.
  - Warna diubah ke profil cetak monokrom hemat tinta tanpa mengurangi keterbacaan teks dan diagram.

---

> [!TIP]
> Semua halaman baru yang ditambahkan ke repositori ini **wajib mematuhi design tokens, tipografi, dan struktur tripartit** di atas demi menjaga konsistensi pengalaman belajar pengguna.
