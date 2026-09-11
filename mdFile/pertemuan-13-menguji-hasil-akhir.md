# 🧪 Pertemuan 13: Menguji Hasil Akhir
> **Kategori:** Uji, Rilis & Proyek Akhir · **Durasi:** 120 Menit  
> **Target Peserta:** Mahasiswa / Vibe Coder Non-IT  
> **Tools:** Antigravity IDE (AGY) · Browser DevTools  
> **Prasyarat:** Proyek Aplikasi Web/PWA/Flutter (Pertemuan 1–12) & Dokumen `prd.md`  
> **Output Utama:** Dokumen `test-plan.md` Terverifikasi + Log Bug + Aplikasi Bebas Bug Kritis

---

## 🎯 Tujuan Pembelajaran & Manfaat Pedagogis

> **Prinsip Utama:** *"Dalam Vibe Coding, AI menulis sintaks kode, namun manusia memvalidasi pengalaman pengguna (UX) dan integritas logika."*

Setelah menyelesaikan praktikum pada pertemuan ini, peserta mampu:
1. **Menyusun Matriks Skenario Uji Komprehensif** berbasis dokumen PRD menggunakan AI Prompt Builder (*Happy Path* & *Edge Cases*).
2. **Melakukan Pengujian Manual *Black-Box* Sistematis** mencakup 6 kategori kasus ekstrem (*empty inputs, rapid clicks, boundary limits, injection characters, network resilience, & state loss*).
3. **Mendokumentasikan Temuan Bug Berstandar Industri** ke dalam file `test-plan.md` lengkap dengan status severity dan langkah reproduksi.
4. **Mengeksekusi *Atomic Bug Fixing*** bersama Antigravity IDE tanpa merusak fitur lain yang sudah berjalan (*anti-regression*).
5. **Menghasilkan Metrik Kelayakan Rilis (*Release Readiness Score*)** sebelum melangkah ke tahap deployment produksi di Pertemuan 14.

---

## 🗺️ Peta Navigasi & Arsitektur Alur Pengujian

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                   ALUR PENGUJIAN & BUG HUNTING VIBE CODING                       │
└──────────────────────────────────────────────────────────────────────────────────┘
   [ Dokumen PRD / Fitur ]
              │
              ▼
   ┌──────────────────────┐
   │ FASE 1: TEST MATRIX  │ ──► Generate Matriks Uji via AGY Prompt (Happy + Edge)
   └──────────────────────┘     Simpan ke file: test-plan.md
              │
              ▼
   ┌──────────────────────┐
   │ FASE 2: MANUAL TEST  │ ──► Eksekusi Manual 6 Kategori Edge Case di Browser/HP
   └──────────────────────┘     Logging Temuan: BUG-01, BUG-02 (Severity & Steps)
              │
              ▼
   ┌──────────────────────┐
   │ FASE 3: ATOMIC FIX   │ ──► Perbaikan Satu per Satu via AI Agent Antigravity
   └──────────────────────┘     Regression Testing & Verifikasi Status [✅ LULUS]
              │
              ▼
   [ Aplikasi Siap Rilis (Release Ready) ] ──► Masuk ke Pertemuan 14 (Deployment)
```

> 💡 **Intisari Arsitektur:** Jangan pernah memperbaiki bug di tengah-tengah pengujian manual. Selesaikan seluruh skenario terlebih dahulu untuk mendapatkan gambaran kesehatan aplikasi secara utuh, baru lakukan perbaikan terisolasi (*atomic fix*).

---

## 🧭 Daftar Isi Modul

1. [Fase 1: Teori Pedagogis, Mental Model & Matriks Skenario Uji](#fase-1--part-a-teori-pedagogis-mental-model--matriks-skenario-uji-25-menit)
2. [Fase 2: Eksekusi Pengujian Manual & 6 Kategori Edge Cases Wajib](#fase-2--part-b-eksekusi-pengujian-manual--6-kategori-edge-cases-wajib-50-menit)
3. [Fase 3: Systematic Targeted Bug Fixing & Regression Verification](#fase-3--part-c-systematic-targeted-bug-fixing--regression-verification-35-menit)
4. [Kumpulan Master Prompt AI Siap Pakai (Prompt Builder K-T-B-H)](#-kumpulan-master-prompt-ai-siap-pakai-prompt-builder-k-t-b-h)
5. [Dosa Besar & Kesalahan Fatal Pengujian Vibe Coding](#-dosa-besar--kesalahan-fatal-pengujian-vibe-coding)
6. [Checklist Akhir & Rubrik Evaluasi Kelayakan Rilis](#-checklist-akhir--rubrik-evaluasi-kelayakan-rilis)

---

## FASE 1 / PART A: Teori Pedagogis, Mental Model & Matriks Skenario Uji (25 Menit)

### 1.1 Mental Model: Mengapa Pengujian Manual Mutlak Diperlukan?

Dalam pengembangan perangkat lunak berbasis AI (*Vibe Coding*), terjadi pergeseran peran:
* **Tradisional:** Manusia menulis sintaks baris demi baris $\rightarrow$ Kompiler memeriksa error sintaks.
* **Vibe Coding:** AI menulis seluruh implementasi kode $\rightarrow$ **Manusia berperan sebagai *Quality Assurance (QA) & Inspector Kelayakan Produk***.

> 🏠 **Analogi Konstruksi Rumah:**  
> AI adalah tukang bangunan cerdas yang memasang dinding, pintu, dan instalasi pipa sesuai gambar denah dalam hitungan detik. Namun, AI tidak pernah tinggal di rumah tersebut. **Pengujian manual adalah saat Anda masuk ke rumah tersebut, memutar kunci pintu, menyalakan keran air bersamaan dengan saklar lampu, dan memastikan pipa tidak bocor.**

### 1.2 Perbedaan Pendekatan: Unit Test vs Behavioral Testing

| Parameter | Pengujian Kode Tradisional (Unit Test) | Pengujian Perilaku Vibe Coding (User-Centric) |
| :--- | :--- | :--- |
| **Fokus Utama** | Menguji fungsi algoritma internal (`isEmailValid()`) | Menguji apa yang dilihat dan dirasakan pengguna di layar |
| **Pelaksana** | Kode skrip pengujian otomatis (*Jest, PyTest*) | Penguji manusia yang menjalankan skenario nyata (*Black-box*) |
| **Kecepatan** | Membutuhkan waktu berjam-jam menulis skrip tes | Sangat cepat dan intuitif menggunakan panduan tabel skenario |
| **Sasaran Vibe Coder** | Terlalu rumit untuk pemula non-IT | **Fokus pada hasil fungsional dan pencegahan crash aplikasi** |

---

### 1.3 Dua Pilar Skenario: Happy Path vs Edge Cases

```
┌─────────────────────────────────────────────────────────────────────────┐
│ 1. HAPPY PATH (Alur Bahagia / Normal)                                   │
│    Alur di mana pengguna bertindak sempurna sesuai ekspektasi pembuat.  │
│    Contoh: User login -> Input email valid -> Input password benar      │
│    -> Berhasil masuk ke dashboard.                                      │
├─────────────────────────────────────────────────────────────────────────┤
│ 2. EDGE CASES & DESTRUCTIVE TESTING (Kasus Tepi & Ekstrem)              │
│    Situasi tak terduga, kelalaian pengguna, atau manipulasi data.       │
│    Contoh: Form submit kosong -> Klik tombol 5x cepat -> Input script   │
│    -> Jaringan internet putus saat upload -> Aplikasi tetap aman & ramah│
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 1.4 Master Prompt 1: Generator Matriks Skenario Uji (`test-plan.md`)

Gunakan prompt terstruktur berbasis formula **K-T-B-H** (*Karakter - Tugas - Batasan - Hasil*) untuk memerintahkan Antigravity IDE menyusun rencana uji lengkap dari dokumen PRD Anda:

```markdown
### 📋 PROMPT BUILDER: GENERATE TEST PLAN DARI PRD
Bertindaklah sebagai Senior QA Lead & Security Tester profesional.
Saya memiliki dokumen PRD / daftar fitur aplikasi saya sebagai berikut:
[TEMPELKAN DAFTAR FITUR DARI prd.md ATAU DESKRIPSI APLIKASI ANDA DI SINI]

Tugas Anda:
Buatkan file komprehensif `test-plan.md` dalam format tabel Markdown berstandar industri yang mencakup:
1. Skenario Happy Path (alur normal setiap fitur)
2. Skenario Edge Cases (input kosong, batasan angka, karakter aneh, double click)
3. Skenario Penanganan Error & Keamanan Dasar

Format kolom tabel yang wajib digunakan:
| ID | Fitur | Kategori | Skenario Uji | Langkah Pengujian | Hasil yang Diharapkan | Status | Catatan Bug |

Instruksi Tambahan:
- Beri status default: "⚠️ BELUM DIUJI"
- Sertakan struktur bagian "Temuan Bug (Bug Log)" dan "Ringkasan Metrik Kelayakan Rilis" di bagian bawah file.
- Simpan file ini langsung ke root workspace dengan nama `test-plan.md`.
```

---

### 1.5 Struktur Standar File `test-plan.md` yang Dihasilkan

Setelah prompt dijalankan di Antigravity IDE, file `test-plan.md` yang terbentuk akan memiliki format berikut:

```markdown
# 📋 Rencana Pengujian Aplikasi (Test Plan) - [Nama Proyek]

## 📌 Informasi Rilis
- **Tanggal Pengujian:** 2026-09-11
- **Tester / Penguji:** [Nama Anda]
- **Target Platform:** Web Desktop / Mobile PWA / Android
- **Versi Aplikasi:** v1.0.0-rc1

---

## 🧪 Matriks Skenario Pengujian

| ID | Fitur | Kategori | Skenario Uji | Langkah Pengujian | Hasil yang Diharapkan | Status | Catatan Bug |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-01** | Autentikasi | Happy Path | Login email & password valid | 1. Isi email & pass benar<br>2. Klik 'Masuk' | Berhasil login & redirect ke Dashboard | ⚠️ BELUM DIUJI | - |
| **TC-02** | Autentikasi | Edge Case | Login dengan password salah | 1. Isi email benar, pass salah<br>2. Klik 'Masuk' | Muncul toast peringatan: "Password salah" | ⚠️ BELUM DIUJI | - |
| **TC-03** | Autentikasi | Edge Case | Submit form input kosong | 1. Kosongkan semua field<br>2. Klik 'Masuk' | Form tertahan, validasi merah "Wajib diisi" | ⚠️ BELUM DIUJI | - |
| **TC-04** | CRUD Data | Happy Path | Tambah data baru berhasil | 1. Isi form data lengkap<br>2. Klik 'Simpan' | Data baru muncul di tabel tanpa refresh | ⚠️ BELUM DIUJI | - |
| **TC-05** | CRUD Data | Edge Case | Rapid double-click tombol Simpan | 1. Klik 'Simpan' 2-3x sangat cepat | Hanya 1 data tersimpan (tidak duplikat) | ⚠️ BELUM DIUJI | - |
| **TC-06** | Keamanan | Edge Case | Input karakter tag HTML/Script | 1. Input `<script>alert(1)</script>`<br>2. Simpan data | Teks disimpan sebagai teks biasa (aman XSS) | ⚠️ BELUM DIUJI | - |

---

## 🪲 Log Temuan Bug (Bug Tracker)

### Format Pencatatan Bug:
#### BUG-01: [Judul Singkat Masalah]
- **Terkait Skenario:** TC-05
- **Tingkat Keparahan (Severity):** 🔴 Critical / 🟠 High / 🟡 Medium / 🔵 Low
- **Langkah Reproduksi:** Klik tombol Simpan 2x dengan cepat pada koneksi lambat.
- **Hasil Aktual (Kondisi Rusak):** Terbuat 2 data kembar di database Supabase.
- **Hasil yang Diharapkan:** Tombol otomatis disable saat request pertama berjalan.
- **Status Perbaikan:** ⏳ Menunggu Perbaikan / 🔧 Sedang Dikerjakan / ✅ Sudah Diperbaiki

---

## 📊 Ringkasan Metrik Pengujian
- **Total Skenario:** 0 Skenario
- **Lulus (Passed):** 0 (0%)
- **Gagal (Failed):** 0 (0%)
- **Belum Diuji:** 0 (100%)
- **Skor Kesiapan Rilis:** ⚠️ Belum Siap Rilis (Syarat Rilis: Pass Rate >= 95% & 0 Critical Bug)
```

---

## FASE 2 / PART B: Eksekusi Pengujian Manual & 6 Kategori Edge Cases Wajib (50 Menit)

### 2.1 Enam Kategori Kasus Ekstrem (*Edge Cases*) yang Wajib Diuji

Ketika melakukan pengujian mandiri, ujilah aplikasi Anda layaknya pengguna yang sedang bingung atau berniat jahat:

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                   6 KATEGORI PENGUJIAN KASUS EKSTREM (EDGE CASES)                │
├──────────────────────────┬───────────────────────────────────────────────────────┤
│ 1. Input Kosong & Spasi  │ Submit form dengan field kosong atau hanya berisi      │
│    (Empty & Whitespace)  │ spasi ("   "). Pastikan ditolak validasi.             │
├──────────────────────────┼───────────────────────────────────────────────────────┤
│ 2. Batas Angka & Ukuran  │ Input angka negatif (-5), 0, atau 999999999. Masukkan  │
│    (Boundary Limits)     │ teks 1000 karakter pada kolom nama/judul.             │
├──────────────────────────┼───────────────────────────────────────────────────────┤
│ 3. Karakter Berbahaya    │ Input kutip tunggal (`' OR '1'='1`), tag HTML         │
│    (Sanitization & XSS)  │ (`<h1>Test</h1>`), atau simbol aneh (`!@#$%^&*()`).   │
├──────────────────────────┼───────────────────────────────────────────────────────┤
│ 4. Klik Cepat / Ganda    │ Lakukan spam click (3-4 kali cepat) pada tombol       │
│    (Rapid Click/Debounce)│ 'Simpan', 'Bayar', atau 'Hapus'. Cegah *race condition*│
├──────────────────────────┼───────────────────────────────────────────────────────┤
│ 5. Refresh & Navigasi    │ Tekan F5 (Refresh) saat sedang mengisi form atau      │
│    (State Persistence)   │ saat sedang berada di halaman detail.                 │
├──────────────────────────┼───────────────────────────────────────────────────────┤
│ 6. Simulasi Offline      │ Matikan koneksi internet (DevTools -> Offline) lalu   │
│    (Network Resilience)  │ coba interaksi. Pastikan muncul pesan ramah.          │
└──────────────────────────┴───────────────────────────────────────────────────────┘
```

---

### 2.2 Panduan Langkah-demi-Langkah Pengujian Fitur Utama

#### A. Pengujian Modul Autentikasi (Login / Register)
1. **Tes Positif:** Masukkan email & password yang benar $\rightarrow$ Pastikan masuk dashboard & session tersimpan.
2. **Tes Password Salah:** Masukkan password acak $\rightarrow$ Pastikan muncul pesan *"Email atau password salah"*, bukan layar putih/crash.
3. **Tes Format Email:** Masukkan `bukanemail` $\rightarrow$ Form harus menolak sebelum request dikirim.

#### B. Pengujian Form Input & Operasi CRUD Database
1. **Tes Duplikasi Data:** Tambahkan item dengan nama/kode yang sama persis $\rightarrow$ Apakah database memvalidasi atau membuat data kembar?
2. **Tes Delete Safeguard:** Klik tombol 'Hapus' $\rightarrow$ **Wajib** muncul dialog konfirmasi *"Apakah Anda yakin ingin menghapus data ini?"*.
3. **Tes Edit Tanpa Perubahan:** Buka form edit, jangan ubah apapun, lalu klik simpan $\rightarrow$ Aplikasi tidak boleh melempar error.

#### C. Pengujian Tampilan Responsif (Mobile Viewport)
1. Buka aplikasi di Google Chrome pada Antigravity IDE.
2. Tekan `F12` $\rightarrow$ Klik ikon **Toggle Device Toolbar** (`Ctrl + Shift + M`).
3. Pilih perangkat **iPhone 14** atau **Samsung Galaxy S20**.
4. Periksa: Apakah ada teks yang terpotong? Apakah tabel meluap keluar layar (*overflow horizontal*)? Apakah tombol terlalu kecil untuk disentuh jari?

---

### 2.3 Master Prompt 2: Deep Edge-Case Discovery Generator

Jika Anda ragu apakah skenario uji Anda sudah lengkap, tanyakan pada AI:

```markdown
### 📋 PROMPT BUILDER: EKSPLORASI DEEP EDGE CASES
Aplikasi saya memiliki fitur: [JELASKAN FITUR, MISAL: "Form Checkout Pembayaran & Upload Bukti Transfer"].

Tolong daftarkan 8 kasus ekstrem (edge cases) paling berbahaya yang berpotensi merusak fitur ini atau membuat data di database korup.
Kategorikan berdasarkan:
1. Validasi Input (Format, Tipe File, Ukuran File)
2. Asinkron & Jaringan (Timeout, Double Submit)
3. Keamanan & Izin Akses (RLS Supabase / Token Expired)

Format jawaban dalam tabel siap tempel ke `test-plan.md`.
```

---

### 2.4 Klasifikasi Derajat Keparahan Bug (*Severity Matrix*)

Ketika Anda menemukan perilaku error selama pengetesan, tentukan tingkat keparahannya sebelum dicatat ke `test-plan.md`:

```
┌────────────────────────────────────────────────────────────────────────┐
│ 🔴 CRITICAL (Blokir Rilis)                                             │
│    Aplikasi crash layar putih, data hilang, celah keamanan terbuka,     │
│    atau alur utama (transaksi/login) tidak bisa diselesaikan sama sekali│
├────────────────────────────────────────────────────────────────────────┤
│ 🟠 HIGH (Mayor)                                                        │
│    Fitur penting tidak berfungsi normal, namun ada jalan pintas         │
│    (workaround) sementara yang bisa dilakukan pengguna.                 │
├────────────────────────────────────────────────────────────────────────┤
│ 🟡 MEDIUM (Minor / Logika)                                             │
│    Fungsi berjalan, tetapi pesan feedback salah, pagination lambat,     │
│    atau urutan data tidak sesuai sort filter.                          │
├────────────────────────────────────────────────────────────────────────┤
│ 🔵 LOW (Kosmetik / Polish)                                             │
│    Tombol bergeser 2px, warna teks kurang kontras, typo huruf,         │
│    atau animasi kurang halus pada perangkat mobile.                    │
└────────────────────────────────────────────────────────────────────────┘
```

---

## FASE 3 / PART C: Systematic Targeted Bug Fixing & Regression Verification (35 Menit)

### 3.1 Mental Model: Prinsip *Atomic Bug Fixing*

> ⚠️ **Peringatan Keras Vibe Coder:**  
> Jangan pernah memberikan prompt perbaikan yang menggabungkan 5 bug sekaligus! Misal: *"Perbaiki bug login, benerin juga tabel yang rusak, sama ubah warna tombol."*  
> **Dampaknya:** AI akan menulis ulang seluruh kode aplikasi Anda, menghapus logika yang sebelumnya sudah bekerja, dan menciptakan 10 bug baru (*Code Regression*).

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      ATOMIC BUG FIXING WORKFLOW                         │
└─────────────────────────────────────────────────────────────────────────┘
  1. Pilih 1 Bug Paling Kritis (Mulai dari Critical -> High)
     │
     ▼
  2. Susun Targeted Prompt dengan menyertakan Reproduksi & Kode Terkait
     │
     ▼
  3. AI Memperbaiki HANYA Bagian yang Rusak (Cek File Diff di IDE)
     │
     ▼
  4. Uji Ulang (Regression Test) Skenario Tersebut + Skenario Sekitarnya
     │
     ▼
  5. Update Status di test-plan.md menjadi [✅ SUDAH DIPERBAIKI]
     │
     ▼
  6. Lanjut ke Bug Berikutnya
```

---

### 3.2 Master Prompt 3: Targeted Atomic Bug Fixer

Gunakan template prompt presisi ini di Antigravity IDE setiap kali meminta AI memperbaiki bug temuan Anda:

```markdown
### 📋 PROMPT BUILDER: TARGETED ATOMIC BUG FIX
Saya menemukan bug pada aplikasi saya saat menjalankan test plan.
Berikut rincian tiket bug:
- ID Bug / Skenario: [BUG-01 / TC-05]
- Fitur: [Nama Fitur, misal: Tombol Tambah Transaksi]
- Langkah Reproduksi: [Tulis langkah detail cara memunculkan bug]
- Hasil Aktual: [Apa yang rusak / pesan error di konsol]
- Hasil yang Diharapkan: [Perilaku yang seharusnya terjadi]

Instruksi Perbaikan:
1. Perbaiki HANYA bagian fungsi atau file yang bertanggung jawab atas masalah ini.
2. JANGAN mengubah arsitektur database atau merombak styling komponen lain yang sudah berjalan.
3. Berikan penjelasan singkat 2 kalimat mengenai akar penyebab masalah dan solusi yang Anda terapkan.
4. Tuliskan kode perbaikannya secara lengkap dan bersih.
```

---

### 3.3 Verifikasi Komparasi: Sebelum vs Sesudah Perbaikan

Berikut adalah contoh studi kasus perbaikan bug *Double-Click Duplicate Entry*:

#### ❌ Kode Rusak (Sebelum Perbaikan):
```javascript
// Tombol langsung mengeksekusi tanpa debounce/disable state
async function handleSaveData(payload) {
  // Masalah: Jika user klik 3x cepat, fungsi ini jalan 3x bersamaan!
  showSpinner();
  const { data, error } = await supabase.from('products').insert(payload);
  hideSpinner();
  if (!error) showToast("Data berhasil disimpan!");
}
```

#### ✅ Kode Diperbaiki (Sesudah Perbaikan Terarah):
```javascript
let isSubmitting = false;

async function handleSaveData(payload) {
  // Solusi: Atomic lock & button disable guard
  if (isSubmitting) return; // Abaikan klik susulan
  isSubmitting = true;
  
  const submitBtn = document.getElementById('btn-submit');
  if (submitBtn) submitBtn.disabled = true;
  
  try {
    showSpinner();
    const { data, error } = await supabase.from('products').insert(payload);
    if (error) throw error;
    showToast("Data berhasil disimpan!");
    closeModal();
  } catch (err) {
    showToast("Gagal menyimpan: " + err.message, "error");
  } finally {
    isSubmitting = false;
    if (submitBtn) submitBtn.disabled = false;
    hideSpinner();
  }
}
```

---

### 3.4 Uji Regresi (*Regression Testing Checklist*)

Setelah AI menerapkan perbaikan kode:
1. Jalankan kembali skenario uji yang sebelumnya berstatus `[❌ GAGAL]`.
2. Pastikan sekarang menghasilkan `[✅ LULUS]`.
3. **Uji Regresi (Sanity Check):** Jalankan 2 skenario normal di sekitar fitur tersebut untuk memastikan perbaikan kode tidak merusak fitur lama.
4. Perbarui kolom status dan ringkasan metrik di file `test-plan.md`.

---

## 📚 Kumpulan Master Prompt AI Siap Pakai (Prompt Builder K-T-B-H)

Simpan dan manfaatkan template prompt berikut selama sesi praktikum:

```markdown
### 🧰 KARTU PROMPT 1: AUDIT FORM VALIDATION
"Tinjau kode form input pada file [nama_file.html / .dart / .jsx].
Periksa apakah sudah ada:
1. Validasi tipe data (angka tidak boleh huruf, email harus berformat valid).
2. Sanitasi terhadap input karakter berbahaya (XSS / SQLi).
3. Penanganan state saat input kosong.
Berikan saran perbaikan kode yang ramah pengguna (UI error message berwarna merah)."

---

### 🧰 KARTU PROMPT 2: SIMULASI PENANGANAN ERROR JARINGAN (SUPABASE / API)
"Periksa pemanggilan Supabase / API pada fungsi [nama_fungsi].
Pastikan ada blok `try ... catch` dan pesan error yang informatif ke pengguna jika:
1. Koneksi internet putus.
2. Token autentikasi habis (expired session).
3. Database menolak karena pelanggaran Row Level Security (RLS).
Tuliskan perbaikan kodenya."

---

### 🧰 KARTU PROMPT 3: GENERATE REKAP AKHIR TEST PLAN
"Baca seluruh isi tabel pada file `test-plan.md` di workspace saya.
Hitung total skenario, jumlah status LULUS, GAGAL, dan BELUM DIUJI.
Hitung persentase Pass Rate: (Lulus / Total) * 100%.
Perbarui bagian 'Ringkasan Metrik' di `test-plan.md` dengan kesimpulan apakah aplikasi ini sudah memenuhi standar kelayakan rilis ke tahap deployment atau belum."
```

---

## ⚠️ Dosa Besar & Kesalahan Fatal Pengujian Vibe Coding

Hindari 4 kebiasaan buruk yang sering menjebak pemula:

```
┌─────────────────────────────────────────────────────────────────────────┐
│ ❌ DOSA 1: "ASAL JALAN SEKALI DIANGGAP SELESAI"                         │
│    Hanya mengetes 1 alur normal, lalu langsung menganggap aplikasi      │
│    siap rilis tanpa mencoba skenario error dan kasus ekstrem.           │
├─────────────────────────────────────────────────────────────────────────┤
│ ❌ DOSA 2: LANGSUNG CODING FIX SAAT MENEMUKAN BUG PERTAMA               │
│    Menghentikan pengujian, membetulkan kode, lalu lupa bagian mana lagi │
│    yang belum sempat diuji. Selesaikan test plan terlebih dahulu!       │
├─────────────────────────────────────────────────────────────────────────┤
│ ❌ DOSA 3: MENYATUKAN BANYAK BUG DALAM SATU PROMPT                      │
│    Menyuruh AI memperbaiki 5 bug sekaligus menyebabkan halusinasi kode │
│    dan merusak fitur yang awalnya sudah berfungsi dengan baik.          │
├─────────────────────────────────────────────────────────────────────────┤
│ ❌ DOSA 4: PERCAYA PENUH PADA ASUMSI AI                                 │
│    Menganggap kode AI pasti bebas bug karena tidak ada error sintaks.    │
│    Ingat: AI tidak memahami konteks kenyamanan pengguna di dunia nyata. │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## ✅ Checklist Akhir & Rubrik Evaluasi Kelayakan Rilis

Sebelum mengakhiri sesi praktikum Pertemuan 13, pastikan seluruh item dalam daftar periksa berikut telah terpenuhi:

- [ ] File `test-plan.md` telah tersimpan rapi di root workspace proyek.
- [ ] Minimal **10 skenario uji** telah didefinisikan (mencakup minimal 4 Happy Path dan 6 Edge Cases).
- [ ] Seluruh skenario telah dieksekusi secara manual (tidak ada lagi status `⚠️ BELUM DIUJI`).
- [ ] Semua bug yang ditemukan telah tercatat di tabel log temuan bug lengkap dengan langkah reproduksi.
- [ ] **100% bug berstatus 🔴 Critical dan 🟠 High telah diperbaiki dan diuji ulang (*Pass*)**.
- [ ] Persentase kelulusan (*Pass Rate*) pada `test-plan.md` mencapai minimal **90%**.
- [ ] Aplikasi berjalan mulus pada orientasi layar mobile (*DevTools Responsive View*).

---

## 📝 Refleksi & Diskusi Kelas (10 Menit Terakhir)

Diskusikan bersama instruktur dan rekan kelas:
1. **Bug paling tak terduga apa** yang berhasil Anda temukan melalui pengujian *edge case* hari ini?
2. Mengapa pendekatan *Atomic Bug Fixing* jauh lebih aman dibandingkan meminta AI merombak seluruh kode sekaligus?
3. Mengapa dokumentasi `test-plan.md` sangat krusial sebelum kita melakukan deployment ke Vercel di **Pertemuan 14**?

> 🚀 **Langkah Selanjutnya:**  
> Selamat! Aplikasi Anda kini telah teruji secara fungsional dan memiliki ketahanan tinggi. Pada **Pertemuan 14**, kita akan mempublikasikan (*deploy*) aplikasi ini ke cloud menggunakan **Vercel** dan menghubungkan *custom domain* agar dapat diakses oleh seluruh dunia!
