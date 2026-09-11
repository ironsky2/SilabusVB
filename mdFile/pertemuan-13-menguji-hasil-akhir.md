# Pertemuan 13: Menguji Hasil Akhir
> **Kategori:** Uji, Rilis & Proyek Akhir · **Durasi:** 120 menit  
> **Tools:** Antigravity IDE (AGY)

---

## 🎯 Tujuan Pertemuan

> Peserta **menguji apa yang dilihat pengguna**, bukan sekadar percaya kode sudah benar.

Setelah pertemuan ini, peserta mampu:
- Membuat daftar skenario uji dari fitur aplikasi yang telah dibangun
- Menjalankan pengujian manual secara sistematis
- Mencatat temuan bug dan memperbaikinya dengan bantuan AI
- Menghasilkan file `test-plan.md` sebagai dokumentasi pengujian

---

## 📖 Teori Singkat (15 menit)

### Apa itu Testing dalam Vibe Coding?

Dalam vibe coding, kita menggunakan AI untuk *menulis kode*. Tapi **AI tidak bisa merasakan pengalaman pengguna**. Pengujian adalah tanggung jawab kita sebagai manusia.

> **Analogi:** AI seperti tukang bangunan yang membangun rumah sesuai gambar denah. Testing adalah saat kita masuk ke rumah itu dan coba buka setiap pintu, nyalakan setiap lampu, dan pastikan kamar mandinya berfungsi.

### Dua Jenis Pengujian yang Perlu Dilakukan

| Jenis | Penjelasan | Contoh |
|---|---|---|
| **Happy Path** | Alur normal seperti yang diharapkan | Login dengan username & password benar |
| **Edge Case** | Situasi ekstrem/tidak terduga | Login dengan password kosong, karakter khusus `!@#` |

### Mengapa Edge Case Penting?

Pengguna sungguhan **tidak selalu menggunakan aplikasi seperti yang kita bayangkan**. Mereka akan:
- Mengklik tombol dua kali dengan cepat (*double-click*)
- Mengosongkan input yang seharusnya diisi
- Memasukkan angka negatif di form harga
- Copy-paste teks panjang yang tidak terduga

---

## 🧠 Konsep Kunci

### 1. Fokus pada Hasil Fungsional, Bukan Sintaks Kode

```
❌ Salah: "Kode ini pasti benar karena AI yang membuatnya"
✅ Benar: "Fitur ini berfungsi sesuai yang diharapkan pengguna?"
```

Biarkan AI mengurus sintaks dan logika kode. Tugas Anda adalah **memverifikasi pengalaman pengguna**.

### 2. Skenario Uji Berbasis PRD

> **Apa itu `prd.md`?**  
> `prd.md` adalah file daftar fitur aplikasi Anda — seperti "menu" dari apa saja yang bisa dilakukan aplikasi Anda. Biasanya dibuat di awal proyek. Jika Anda belum punya, cukup tulis daftar fitur aplikasi Anda di kertas atau di file teks biasa.

Setiap fitur yang tertulis di `prd.md` harus punya minimal satu skenario uji.

**Contoh:**  
Jika `prd.md` memiliki fitur: *"Pengguna dapat menambahkan item ke keranjang belanja"*, maka skenario ujinya adalah:

| # | Skenario | Langkah | Hasil yang Diharapkan |
|---|---|---|---|
| T01 | Tambah item ke keranjang | Klik tombol "Tambah" pada produk | Item muncul di keranjang, jumlah +1 |
| T02 | Tambah item yang sama dua kali | Klik "Tambah" dua kali pada produk yang sama | Jumlah item menjadi 2, bukan duplikat |
| T03 | Tambah item saat keranjang kosong | Keranjang kosong, lalu klik "Tambah" | Item pertama berhasil masuk keranjang |

### 3. Struktur File `test-plan.md`

Output dari pertemuan ini adalah file `test-plan.md` dengan format:

```markdown
# Test Plan - [Nama Aplikasi]

## Informasi
- Tanggal: [tanggal]
- Penguji: [nama]
- Kondisi Saat Uji: [misal: "setelah fitur login selesai"]

## Skenario Uji

| ID | Fitur | Skenario | Langkah | Hasil Diharapkan | Status | Catatan |
|----|-------|----------|---------|-----------------|--------|---------|
| T01 | Login | Login sukses | 1. Buka halaman login 2. Isi username & password 3. Klik Login | Masuk ke dashboard | ✅ LULUS | - |
| T02 | Login | Password salah | 1. Isi username benar 2. Isi password salah 3. Klik Login | Muncul pesan error | ❌ GAGAL | Tidak ada pesan error |
| T03 | Login | Password kosong | 1. Isi username 2. Kosongkan password 3. Klik Login | Form tidak terkirim | ⚠️ BELUM DIUJI | - |

## Temuan Bug

### BUG-01: Tidak ada pesan error saat password salah
- **Skenario:** T02
- **Langkah Reproduksi:** Login dengan password salah
- **Hasil Aktual:** Halaman loading terus, tidak ada feedback
- **Hasil Diharapkan:** Muncul pesan "Password salah, silakan coba lagi"
- **Status:** 🔧 Sedang diperbaiki / ✅ Sudah diperbaiki

## Ringkasan
- Total Skenario: X
- Lulus: X
- Gagal: X
- Belum Diuji: X
```

---

## 📚 Kamus Istilah (Baca Sebelum Mulai)

Berikut istilah yang akan sering muncul di pertemuan ini:

| Istilah | Artinya dalam Bahasa Sehari-hari |
|---|---|
| **Bug** | Kesalahan atau kerusakan pada aplikasi — sesuatu yang seharusnya berjalan tapi tidak |
| **Testing / Pengujian** | Proses mencoba aplikasi sendiri untuk menemukan bug sebelum pengguna menemukannya |
| **Happy Path** | Skenario ideal — pengguna melakukan hal yang "benar" sesuai harapan kita |
| **Edge Case** | Skenario ekstrem atau tidak biasa — hal yang jarang terjadi tapi bisa menyebabkan masalah |
| **Reproduksi Bug** | Langkah-langkah untuk membuat bug tersebut muncul kembali, supaya bisa diperbaiki |
| **Sintaks Kode** | "Tata bahasa" dari kode — aturan penulisan yang harus diikuti agar kode bisa berjalan |
| **Feedback** | Respons dari aplikasi kepada pengguna — misal pesan error, notifikasi, atau perubahan tampilan |

---

## 🛠️ Praktik di Kelas (90 menit)

### Persiapan (5 menit)

Sebelum mulai, pastikan Anda memiliki:
- [ ] Antigravity IDE terbuka
- [ ] Proyek aplikasi yang sudah dibangun di pertemuan sebelumnya
- [ ] File `prd.md` (atau catatan fitur yang ingin dibangun)

> **Jika belum punya `prd.md`:** Buat daftar fitur aplikasi Anda terlebih dahulu. Contoh: *"Aplikasi saya bisa: (1) tambah item, (2) hapus item, (3) simpan data"*

---

### Langkah 1: Buat Daftar Skenario Uji dengan AGY (25 menit)

**Tujuan:** Gunakan AI untuk membantu membuat skenario uji yang komprehensif.

#### Prompt yang Digunakan di AGY:

```
Saya punya aplikasi [nama aplikasi] dengan fitur-fitur berikut:
[tempel daftar fitur dari prd.md]

Tolong buatkan saya daftar skenario uji dalam format tabel markdown yang mencakup:
1. Happy path (alur normal)
2. Edge cases (kasus tepi) seperti input kosong, karakter khusus, klik cepat ganda
3. Setiap baris berisi: ID, Fitur, Skenario, Langkah, Hasil yang Diharapkan, Status (isi dengan "⚠️ BELUM DIUJI"), Catatan

Simpan hasilnya ke file test-plan.md
```

#### Yang Harus Dilakukan:
1. Buka Antigravity IDE
2. Ketik prompt di atas, sesuaikan dengan aplikasi Anda
3. Review dan tambahkan skenario yang menurut Anda penting tapi belum ada
4. Simpan file `test-plan.md`

---

### Langkah 2: Jalankan Pengujian Manual (45 menit)

**Tujuan:** Eksekusi setiap skenario, catat semua yang gagal.

#### Cara Pengujian yang Baik:

```
Untuk setiap baris di test-plan.md:
1. Baca skenarionya
2. Ikuti langkah-langkahnya PERSIS seperti yang tertulis
3. Bandingkan hasil aktual dengan "Hasil yang Diharapkan"
4. Update kolom Status: ✅ LULUS / ❌ GAGAL
5. Jika GAGAL → tulis Catatan apa yang terjadi
```

#### ⚠️ Aturan Penting Saat Testing:
- **Jangan skip edge case** — justru di situlah bug tersembunyi
- **Catat SEMUA yang aneh**, walau bukan error besar
- **Jangan perbaiki dulu** saat testing — selesaikan semua skenario terlebih dahulu

#### Contoh Edge Case yang Wajib Dicoba:

| Kategori | Yang Dicoba |
|---|---|
| **Input Kosong** | Submit form tanpa mengisi apapun |
| **Angka Negatif** | Masukkan `-1` atau `-999` di field angka |
| **Karakter Khusus** | Ketik `<script>`, `' OR 1=1`, `!@#$%^` |
| **Klik Cepat Ganda** | Double-click tombol simpan/kirim |
| **Teks Sangat Panjang** | Paste 1000 karakter di satu input field |
| **Refresh Halaman** | Refresh saat sedang mengisi form |

---

### Langkah 3: Perbaiki Temuan Satu per Satu (20 menit)

**Tujuan:** Perbaiki bug secara sistematis, verifikasi setelah setiap perbaikan.

#### Alur Perbaikan dengan AGY:

```
1. Pilih satu bug dari daftar (mulai dari yang paling kritis)
2. Di AGY, prompt:
   "Saya menemukan bug: [jelaskan bug dari catatan test-plan.md]
    Langkah reproduksi: [tempel dari test-plan]
    Hasil aktual: [apa yang terjadi]
    Hasil diharapkan: [seharusnya apa]
    Tolong perbaiki bug ini."
3. Uji ulang skenario yang sama setelah diperbaiki
4. Update status di test-plan.md menjadi ✅ SUDAH DIPERBAIKI
5. Ulangi untuk bug berikutnya
```

> **🔑 Aturan Emas:** Perbaiki **satu bug per satu**, lalu **uji ulang** sebelum lanjut ke bug berikutnya. Memperbaiki banyak bug sekaligus membuat kita tidak tahu perbaikan mana yang berhasil.

---

## 📦 Output Pertemuan

Di akhir kelas, Anda harus memiliki file `test-plan.md` yang berisi:

- ✅ Daftar lengkap skenario uji (happy path + edge cases)
- ✅ Status setiap skenario (Lulus / Gagal / Belum Diuji)
- ✅ Catatan temuan bug
- ✅ Status perbaikan setiap bug

---

## ❌ Kesalahan yang Sering Terjadi

### 1. Hanya Menguji Alur Ideal

```
❌ "Saya sudah test, bisa login dan tambah data. Selesai."
✅ "Saya test login sukses, login gagal, login kosong, login karakter aneh..."
```

Pengguna asli **akan menemukan kasus tepi lebih dulu** daripada Anda. Jangan beri mereka kesempatan itu.

### 2. Langsung Perbaiki Saat Menemukan Bug

```
❌ Temukan bug → langsung perbaiki → lanjut test → temukan bug lagi → perbaiki lagi...
✅ Selesaikan semua skenario dulu → catat semua bug → perbaiki satu per satu
```

### 3. Percaya Penuh pada AI

```
❌ "AI sudah bilang kodenya benar, pasti tidak ada bug."
✅ "AI menulis kode, tapi saya yang memvalidasi pengalaman pengguna."
```

---

## 💬 Prompt Referensi untuk AGY

Simpan prompt-prompt ini untuk digunakan selama praktikum:

```markdown
### Prompt 1: Generate Test Plan
"Buatkan test-plan.md untuk aplikasi [nama] dengan fitur [daftar fitur].
Sertakan happy path dan edge cases. Format: tabel markdown."

### Prompt 2: Perbaiki Bug
"Perbaiki bug berikut di aplikasi saya:
- Deskripsi: [deskripsi bug]
- Langkah reproduksi: [langkah]
- Hasil aktual: [apa yang terjadi]
- Hasil diharapkan: [seharusnya apa]"

### Prompt 3: Tambah Edge Case
"Dari fitur [nama fitur] di aplikasi saya, edge case apa lagi yang
belum saya uji? Tambahkan ke test-plan.md yang sudah ada."

### Prompt 4: Update Status Test Plan
"Update test-plan.md: skenario T[XX] statusnya ubah menjadi ✅ LULUS.
Bug BUG-[XX] statusnya ubah menjadi ✅ Sudah Diperbaiki."
```

---

## ✅ Checklist Akhir Pertemuan

Sebelum kelas selesai, pastikan:

- [ ] File `test-plan.md` sudah dibuat dan tersimpan di proyek
- [ ] Semua fitur utama sudah ada skenario ujinya
- [ ] Edge case sudah diuji (minimal: input kosong, klik cepat ganda, karakter khusus)
- [ ] Semua bug yang ditemukan sudah dicatat
- [ ] Minimal 50% bug yang ditemukan sudah diperbaiki dan diuji ulang
- [ ] Status setiap skenario di `test-plan.md` sudah diperbarui

---

## 📝 Refleksi (5 menit terakhir)

Diskusikan bersama kelas:

1. **Bug apa yang paling mengejutkan** yang Anda temukan hari ini?
2. **Edge case mana** yang tidak terpikirkan sebelumnya?
3. **Apa yang berubah** dari cara Anda melihat "aplikasi yang sudah selesai"?

---

> *"Kode yang benar secara sintaks bukan berarti pengalaman pengguna yang benar. Testing adalah jembatan antara keduanya."*
