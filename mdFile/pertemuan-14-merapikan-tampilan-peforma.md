# Pertemuan 14: Merapikan Tampilan & Performa
> **Kategori:** Uji, Rilis & Proyek Akhir · **Durasi:** 90 menit  
> **Tools:** Antigravity IDE (AGY) + Chrome Browser (DevTools)

---

## 🎯 Tujuan Pertemuan

> Aplikasi terasa **nyaman dan layak dipakai** oleh orang lain di berbagai ukuran layar.

Setelah pertemuan ini, peserta mampu:
- Mendeteksi dan memperbaiki kesalahan tampilan di layar ponsel dan laptop
- Menjalankan audit Lighthouse di Chrome DevTools dan membaca skornya
- Memprioritaskan 3 rekomendasi Lighthouse yang paling berdampak
- Memperbaiki tampilan dan performa menggunakan prompt spesifik di AGY

---

## 📖 Teori Singkat (15 menit)

### Mengapa Tampilan Responsif Itu Penting?

Lebih dari **60% pengguna** mengakses web dari ponsel. Jika aplikasi Anda hanya terlihat bagus di laptop Anda sendiri, banyak pengguna yang akan pergi.

> **Analogi:** Bayangkan membuat baju yang hanya muat untuk satu ukuran badan. Tampilan responsif adalah "baju" yang menyesuaikan diri dengan ukuran layar pengguna.

### Dua Hal yang Diperiksa di Pertemuan Ini

| Aspek | Yang Diperiksa | Tools |
|---|---|---|
| **Tampilan Responsif** | Apakah layout tetap rapi di ponsel (375px) dan laptop (1440px)? | Chrome DevTools — Toggle Device |
| **Performa Lighthouse** | Seberapa cepat, accessible, dan teroptimasi aplikasi Anda? | Chrome DevTools — Lighthouse |

---

### Kesalahan Tampilan Paling Umum

Sebelum praktik, kenali dulu masalah yang sering muncul:

| Masalah | Penjelasan | Contoh |
|---|---|---|
| **Teks terpotong** | Kotaknya terlalu sempit, teks tidak muat | Tombol dengan teks "Simpan Data Pengguna" terpotong jadi "Simpan D..." |
| **Overflow horizontal** | Ada elemen yang lebih lebar dari layar, jadi keluar ke kanan | Tabel atau gambar menonjol ke kanan, muncul scrollbar horizontal |
| **Kontras warna rendah** | Warna teks terlalu mirip dengan warna latar — susah dibaca | Teks abu-abu di latar putih |
| **Tombol terlalu kecil** | Tombol terlalu kecil untuk disentuh dengan jari di layar ponsel | Tombol berukuran sangat kecil — susah diklik |
| **Layout berantakan di mobile** | Susunan halaman hancur saat ukuran layar mengecil | Kolom yang harusnya turun ke bawah, malah saling tindih |

---

### Mengenal Skor Lighthouse

Lighthouse memberi **skor 0–100** untuk empat kategori:

```
🟢 90–100  → Baik (Good)
🟡 50–89   → Perlu diperbaiki (Needs Improvement)  
🔴 0–49    → Buruk (Poor)
```

| Kategori | Yang Diukur |
|---|---|
| **Performance** | Seberapa cepat halaman dimuat |
| **Accessibility** | Seberapa mudah diakses (termasuk pengguna disabilitas) |
| **Best Practices** | Apakah mengikuti standar web modern |
| **SEO** | Seberapa mudah ditemukan di mesin pencari |

> **⚠️ Fokus pertemuan ini:** Performance + Accessibility. Jangan mengejar skor 100 dengan mengorbankan fitur yang dibutuhkan pengguna.

---

## 📚 Kamus Istilah (Baca Sebelum Mulai)

| Istilah | Artinya dalam Bahasa Sehari-hari |
|---|---|
| **Responsif** | Tampilan yang menyesuaikan diri otomatis di berbagai ukuran layar (ponsel, tablet, laptop) |
| **Viewport** | Area tampilan layar yang terlihat pengguna — berbeda ukurannya di ponsel vs laptop |
| **375px / 1440px** | Satuan lebar layar dalam piksel. 375px = lebar layar ponsel standar. 1440px = lebar laptop besar |
| **Overflow** | Ketika isi halaman "meluber" keluar dari batas layar, muncul scrollbar yang tidak diinginkan |
| **Layout** | Susunan/tata letak elemen-elemen di halaman web (tombol, gambar, teks, kolom, dll.) |
| **Chrome DevTools** | Alat bawaan browser Chrome untuk menganalisis dan menginspeksi halaman web (tekan F12) |
| **Lighthouse** | Fitur di Chrome DevTools yang memberi nilai/skor kualitas aplikasi web Anda secara otomatis |
| **Audit** | Proses pemeriksaan menyeluruh — di sini berarti Lighthouse memeriksa aplikasi Anda |
| **localhost** | Alamat web sementara di komputer Anda sendiri saat aplikasi sedang dikembangkan (belum online) |
| **Alt text** | Teks deskripsi untuk gambar — dibaca oleh pembaca layar untuk pengguna tunanetra |

---

## 🛠️ Praktik di Kelas (70 menit)

### Persiapan (5 menit)

Pastikan Anda memiliki:
- [ ] Aplikasi berjalan di browser Chrome  
  *(Cara buka: jalankan aplikasi dari AGY terlebih dahulu, lalu buka Chrome — biasanya alamatnya `localhost:3000` atau alamat yang diberikan AGY saat aplikasi dijalankan)*
- [ ] Antigravity IDE terbuka di proyek yang sama
- [ ] Chrome DevTools siap dibuka (`F12` atau `Ctrl+Shift+I` — ini tombol shortcut untuk buka alat developer)

---

### Langkah 1: Cek Tampilan di Ponsel dan Laptop (20 menit)

**Tujuan:** Temukan dan catat semua layout yang patah di dua ukuran layar.

#### Cara Membuka Mode Responsif di Chrome DevTools:

```
1. Buka aplikasi di Chrome
2. Tekan F12 (atau klik kanan → Inspect)
3. Klik ikon 📱 "Toggle device toolbar" (atau Ctrl+Shift+M)
4. DevTools akan masuk ke mode responsive
```

#### Dua Ukuran yang WAJIB Dicek:

**Ukuran Ponsel — 375px (iPhone SE)**
```
Di toolbar DevTools:
→ Klik dropdown "Dimensions"
→ Pilih "iPhone SE" ATAU
→ Ketik manual: Width = 375, Height = 667
```

**Ukuran Laptop — 1440px**
```
Di toolbar DevTools:
→ Ketik manual: Width = 1440
→ Atau pilih "Laptop L" jika tersedia
```

#### Yang Dicatat Selama Cek Tampilan:

Buat catatan di file baru `ui-audit.md`:

```markdown
## Audit Tampilan - [Nama Aplikasi]
**Tanggal:** [tanggal]

### Masalah yang Ditemukan

| # | Halaman/Komponen | Masalah | Ukuran Layar | Catatan |
|---|---|---|---|---|
| U01 | Header | Teks terpotong | 375px | Nama aplikasi terpotong |
| U02 | Tabel data | Overflow horizontal | 375px | Tabel menonjol ke kanan |
| U03 | Form input | Tombol terlalu kecil | 375px | Susah diklik di ponsel |
```

#### Checklist Masalah yang Perlu Diperiksa:

- [ ] Apakah ada elemen yang keluar dari batas layar (overflow)?
- [ ] Apakah semua teks terbaca tanpa terpotong?
- [ ] Apakah semua tombol cukup besar untuk diklik di ponsel?
- [ ] Apakah gambar menyesuaikan ukuran layar (tidak terlalu besar/kecil)?
- [ ] Apakah navigasi/menu masih bisa diakses di layar kecil?

---

### Langkah 2: Jalankan Audit Lighthouse (20 menit)

**Tujuan:** Dapatkan skor objektif dan ambil 3 rekomendasi dengan dampak terbesar.

#### Cara Menjalankan Lighthouse:

```
1. Di Chrome DevTools (F12), klik tab "Lighthouse"
   (Jika tidak muncul, klik ">>" untuk lihat tab tersembunyi)

2. Pilih kategori yang akan diaudit:
   ✅ Performance
   ✅ Accessibility
   ✅ Best Practices
   ✅ SEO

3. Pilih device:
   → Mobile (untuk cek versi ponsel)
   → Desktop (untuk cek versi laptop)
   Lakukan dua kali — untuk mobile DAN desktop

4. Klik "Analyze page load"
   (Tunggu 30–60 detik, jangan klik apapun selama proses)
```

#### Cara Membaca Hasil Lighthouse:

Setelah selesai, Anda akan melihat halaman dengan skor dan rekomendasi. Ini cara membacanya:

```
📊 SKOR DI ATAS (bulatan warna)
→ Catat skornya untuk setiap kategori

📋 DAFTAR AUDIT DI BAWAH
→ Scroll ke bawah, ada dua bagian:
   "Passed audits" → yang sudah baik (tidak perlu diperbaiki)
   "Failed audits" / yang berwarna merah/kuning → yang perlu diperbaiki

🎯 PILIH 3 YANG PALING BERDAMPAK
→ Lihat angka "Potential savings" atau "Impact"
→ Pilih 3 masalah dengan dampak terbesar
```

#### Catat di `ui-audit.md`:

```markdown
## Hasil Lighthouse

### Skor Awal
| Kategori | Mobile | Desktop |
|---|---|---|
| Performance | [skor] | [skor] |
| Accessibility | [skor] | [skor] |
| Best Practices | [skor] | [skor] |
| SEO | [skor] | [skor] |

### 3 Rekomendasi yang Dipilih
| # | Masalah dari Lighthouse | Kategori | Dampak |
|---|---|---|---|
| L01 | [nama masalah] | Performance | [potensi penghematan] |
| L02 | [nama masalah] | Accessibility | - |
| L03 | [nama masalah] | Best Practices | - |
```

---

### Langkah 3: Perbaiki dengan AGY, Ukur Ulang Skor (30 menit)

**Tujuan:** Gunakan prompt spesifik di AGY untuk perbaiki masalah, lalu cek apakah skor naik.

#### Cara Perbaikan yang Efektif:

```
Untuk setiap masalah yang ditemukan:
1. Buka AGY
2. Buat prompt spesifik (lihat panduan di bawah)
3. Terapkan perubahan kode dari AGY
4. Refresh browser, cek tampilan / jalankan Lighthouse ulang
5. Catat perubahan skor di ui-audit.md
6. Lanjut ke masalah berikutnya
```

> **🔑 Ingat:** Perbaiki **satu masalah per satu**, lalu ukur ulang. Jangan perbaiki semuanya sekaligus karena kita tidak akan tahu mana yang efektif.

---

## 💬 Prompt Referensi untuk AGY

> **Cara pakai:** Salin prompt di bawah, ganti bagian dalam `[kurung kotak]` dengan kondisi aplikasi Anda, lalu kirim ke AGY.

### Prompt untuk Perbaikan Tampilan Responsif

```
### Perbaiki Tampilan di Layar Ponsel
"Di aplikasi saya, saat dilihat di layar ponsel (ukuran kecil),
[nama bagian halaman, misal: header / tabel / form] terlihat berantakan.
[Jelaskan masalahnya: misal 'ada bagian yang keluar dari layar ke kanan'
atau 'tombolnya terlalu kecil' atau 'teksnya terpotong']
Tolong perbaiki agar tampilannya rapi di layar ponsel maupun laptop."

### Perbaiki Teks Terpotong
"Teks di [nama elemen, misal: tombol / judul / label] terpotong
saat dilihat di layar kecil. Tolong perbaiki agar teksnya tidak terpotong
dan tetap terbaca di semua ukuran layar."

### Perbaiki Tombol Susah Diklik di Ponsel
"Tombol [nama tombol] terlalu kecil dan susah diklik saat di ponsel.
Tolong perbesar ukuran tombolnya agar nyaman disentuh dengan jari."

### Perbaiki Layout Berantakan di Mobile
"Halaman [nama halaman] tampilannya berantakan saat di ponsel.
[Deskripsikan masalahnya dengan kata-kata sendiri]
Tolong perbaiki agar susunannya rapi di layar kecil."
```

### Prompt untuk Perbaikan dari Hasil Lighthouse

```
### Salin Langsung dari Lighthouse
"Lighthouse memberi saya rekomendasi berikut:
[Tempel / copy-paste teks rekomendasi yang muncul di Lighthouse]

Tolong perbaiki masalah ini di aplikasi saya
dan jelaskan dengan bahasa sederhana apa yang sudah diubah."

### Perbaiki Kontras Warna
"Lighthouse melaporkan warna teks di [lokasi elemen]
terlalu mirip dengan warna latarnya, jadi susah dibaca.
Tolong perbaiki warnanya agar lebih mudah dibaca."

### Perbaiki Gambar Tanpa Keterangan
"Lighthouse melaporkan ada gambar di aplikasi saya yang tidak punya
keterangan teks (alt text). Tolong tambahkan keterangan yang sesuai
pada setiap gambar di aplikasi saya."

### Optimalkan Gambar yang Berat
"Lighthouse melaporkan gambar di aplikasi saya terlalu berat/lambat dimuat.
Tolong optimalkan semua gambar agar halaman lebih cepat terbuka."
```

---

## 📦 Output Pertemuan

Di akhir kelas, Anda harus memiliki file `ui-audit.md` yang berisi:

- ✅ Daftar masalah tampilan yang ditemukan (ponsel 375px + laptop 1440px)
- ✅ Skor Lighthouse **sebelum** perbaikan (mobile + desktop)
- ✅ 3 rekomendasi Lighthouse yang dipilih untuk diperbaiki
- ✅ Skor Lighthouse **setelah** perbaikan
- ✅ Catatan perubahan apa yang dilakukan untuk setiap perbaikan

---

## ❌ Kesalahan yang Sering Terjadi

### 1. Mengejar Skor 100 dengan Mengorbankan Fitur

```
❌ "Saya hapus semua gambar supaya score Performance-nya 100."
✅ "Saya optimalkan gambar agar ukurannya lebih kecil tanpa menghapusnya."
```

Skor Lighthouse adalah **alat bantu**, bukan tujuan akhir. Aplikasi dengan skor 72 yang semua fiturnya berjalan lebih baik dari skor 100 tapi setengah fiturnya rusak.

### 2. Hanya Cek di Layar Sendiri

```
❌ "Di laptop saya terlihat bagus, berarti sudah responsif."
✅ "Saya cek di 375px (ponsel) DAN 1440px (laptop) via DevTools."
```

Layar laptop Anda mungkin 1366px atau 1920px. Itu bukan ukuran representatif semua pengguna.

### 3. Perbaiki Banyak Hal Sekaligus

```
❌ Perbaiki 5 masalah sekaligus → jalankan Lighthouse → skor naik → tidak tahu mana yang berpengaruh
✅ Perbaiki 1 masalah → ukur → catat → perbaiki 1 lagi → ukur lagi
```

### 4. Tidak Membaca Pesan Error Lighthouse dengan Teliti

```
❌ "Ada tulisan merah, pasti harus diperbaiki semua sekarang."
✅ Pilih 3 yang paling berdampak (lihat "Potential savings" atau dampaknya)
```

---

## ✅ Checklist Akhir Pertemuan

Sebelum kelas selesai, pastikan:

- [ ] Sudah cek tampilan di **375px** (mobile) dan **1440px** (desktop)
- [ ] Semua masalah tampilan sudah dicatat di `ui-audit.md`
- [ ] Lighthouse sudah dijalankan minimal untuk **Mobile**
- [ ] Skor awal Lighthouse sudah dicatat (sebelum perbaikan)
- [ ] Minimal **3 rekomendasi Lighthouse** sudah dipilih
- [ ] Minimal **1 perbaikan** sudah dilakukan dan diukur ulang skornya
- [ ] Skor Lighthouse sesudah perbaikan sudah dicatat

---

## 📝 Refleksi (5 menit terakhir)

Diskusikan bersama kelas:

1. **Masalah tampilan apa** yang paling mengejutkan saat dilihat di ponsel?
2. **Skor Lighthouse** Anda sebelum dan sesudah — naik berapa poin?
3. **Rekomendasi mana** yang paling mudah diperbaiki? Mana yang paling sulit?

---

> *"Aplikasi yang 'selesai' di laptop bukan berarti selesai untuk semua pengguna. Cek di layar mereka, bukan layar Anda."*
