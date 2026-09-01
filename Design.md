# Walkthrough: Penyelarasan Desain PRD Modul Deploy Vercel

Desain antarmuka modul praktikum baru [panduan_deploy_vercel.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/panduan_deploy_vercel.html) telah diselaraskan **100% mengikuti standar Product Requirements Document (PRD)** yang dipakai pada [laboratorium_lengkap_supabase.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/laboratorium_lengkap_supabase.html) dan [silabus-vibe-coding-16-pertemuan.html](file:///d:/IKA2026/Jurnal%20Conference/MateriLengkap%20VB/silabus-vibe-coding-16-pertemuan.html).

> [!NOTE]
> Sesuai instruksi pengguna, seluruh perubahan ini **BELUM di-commit dan BELUM di-push** ke GitHub.

---

## 🎨 Arsitektur Desain PRD yang Distandarisasi

1. **Header & Navigation**:
   - `header.nav` dengan `.brand`, status dot, brand title (*Newsreader Serif*), brand pill, dan dropdown terpadu ke seluruh modul masterclass.
2. **Hero Header**:
   - `.hero` gradient, `.eyebrow` badge uppercase, `h1.hero-title`, `.lede` deskriptif, dan `.meta-badge-row` ringkas.
3. **Sticky Quick-Filter Bar**:
   - `.lab-nav-sticky` dengan `.lab-nav-pill` pill navigasi horizontal yang menempel saat di-scroll.
4. **Architecture Roadmap Callout**:
   - `.arch-card` dengan visual `.arch-diagram` monospace & `.arch-insight` untuk mental model pemula.
5. **Struktur Bagian Modul Standar PRD (Per Fase)**:
   - **Part A · Teori Pedagogis & Mental Model** (`.part-box`, `.teori-title`, `.analogy-box`, perbandingan fitur).
   - **Part B · Master Prompts AI Siap Pakai** (`.prompt-title`, `.prompt-card`, `.code-card` header + tombol copy `.btn-copy`).
   - **Part C · Output & Verifikasi Kesiapan** (`.check-title`, `.result-grid`, `.res-output` hijau & `.res-pitfall` merah).
6. **Toast Notification System**:
   - Skrip `copySnippet(id)` dengan feedback popup `#toast` di pojok kanan bawah.

---

## 🔍 Hasil Validasi DOM Tree

Seluruh 8 file HTML dalam repositori telah divalidasi dan dinyatakan **100% seimbang & valid**:
- `silabus-vibe-coding-16-pertemuan.html` ✅
- `index.html` ✅
- `laboratorium_lengkap_supabase.html` ✅
- `panduan_push_github.html` ✅
- `panduan_vibecoding_owasp.html` ✅
- `praktikum_git_sebelum_coding.html` ✅
- `kurikulum_vibe_coding_pwa_supabase.html` ✅
- `panduan_deploy_vercel.html` ✅
