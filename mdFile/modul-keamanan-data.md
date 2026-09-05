# 🔐 Modul: Keamanan Data & Validasi Input
### Program Vibe Coding | Untuk Pengusaha | Durasi: 90 Menit

---

> **Untuk siapa modul ini?**
> Untuk kamu yang baru mulai membuat website/aplikasi bisnis menggunakan AI, dan ingin memastikan data pelangganmu **aman dan tidak mudah dimanipulasi**.

---

## 🗺️ Peta Perjalanan 90 Menit

| Segmen | Aktivitas | Durasi |
|---|---|---|
| 🧠 **Teori Singkat** | Konsep keamanan data dalam bahasa sehari-hari | 20 menit |
| 💻 **Praktikum 1** | Copy-paste prompt, lihat hasilnya | 25 menit |
| 💻 **Praktikum 2** | Modifikasi prompt sesuai bisnis kamu | 25 menit |
| 🎯 **Review & Tanya Jawab** | Diskusi dan evaluasi hasil | 20 menit |

---

# BAGIAN 1: TEORI (20 Menit)

## 🤔 Apa Itu Keamanan Data & Validasi Input?

### Analogi yang Mudah Dipahami

Bayangkan kamu punya **toko fisik**:

- 🏪 **Pintu masuk toko** = Form input di website (form pendaftaran, form pemesanan, form login)
- 👤 **Pelanggan yang masuk** = Data yang diisi pengguna
- 💂 **Satpam di pintu** = Validasi input
- 🏦 **Brankas toko** = Database / penyimpanan data

**Tanpa satpam** → Siapa saja bisa masuk, termasuk orang jahat.
**Tanpa validasi input** → Data apa saja bisa masuk, termasuk data berbahaya.

---

## 📦 4 Topik Utama yang Wajib Kamu Tahu

### 1️⃣ Validasi Form Input — "Satpam yang Periksa KTP"

**Apa itu?**
Proses memastikan data yang diisi pengguna **sesuai format yang benar** sebelum disimpan.

**Contoh kasus nyata:**
- Kolom email → harus ada karakter `@` dan domain (`.com`, `.id`, dll)
- Kolom nomor HP → hanya boleh angka, minimal 10 digit
- Kolom nama → tidak boleh kosong, tidak boleh berisi simbol aneh

**Kenapa penting buat pengusaha?**
> Tanpa validasi, database kamu bisa penuh dengan data sampah, dan kamu tidak bisa menghubungi pelanggan karena email/nomor HP yang tersimpan salah.

---

### 2️⃣ Keamanan Password & Enkripsi — "Gembok yang Tidak Bisa Dibaca"

**Apa itu?**
Password pelanggan **tidak boleh disimpan apa adanya** di database. Harus diubah menjadi kode acak (enkripsi/hashing) sehingga walaupun seseorang mencuri data, mereka tidak bisa membaca passwordnya.

**Analogi:**
- Password asli: `toko123`
- Setelah di-hash: `$2b$10$Xk9mL2...` (tidak bisa dibaca manusia)

**Yang wajib kamu minta ke AI:**
- Gunakan bcrypt atau metode hashing yang aman
- Jangan pernah simpan password dalam bentuk teks biasa (plaintext)

---

### 3️⃣ Proteksi dari Input Berbahaya — "Mencegah Penjahat Masuk Lewat Celah Tersembunyi"

**Dua ancaman utama (dijelaskan tanpa jargon):**

#### 🔴 SQL Injection — "Perintah Palsu yang Menyusup"
Bayangkan form login kamu. Biasanya pengguna mengisi:
- Username: `budi`
- Password: `rahasia123`

Tapi seorang penyerang mengisi:
- Username: `admin' OR '1'='1`

Kalau tidak ada proteksi, sistem bisa "tertipu" dan membiarkan penyerang masuk **tanpa password yang benar**.

**Solusi:** Minta AI selalu menggunakan "parameterized query" atau "prepared statement" — ini cara aman menyimpan dan memproses data dari pengguna.

#### 🔴 XSS (Cross-Site Scripting) — "Pesan yang Berubah Jadi Jebakan"
Bayangkan pelanggan bisa menulis komentar di tokomu. Kalau tidak ada proteksi, seseorang bisa menulis:
```
<script>alert('Website kamu kena hack!')</script>
```
Dan semua pengunjung website yang melihat komentar itu akan terkena dampaknya.

**Solusi:** Minta AI untuk selalu "sanitasi" (membersihkan) input sebelum ditampilkan di halaman.

---

### 4️⃣ Validasi Data Transaksi — "Cek Ganda Sebelum Uang Berpindah"

**Kenapa ini penting?**
Untuk form pemesanan dan pembayaran, validasi lebih ketat diperlukan:
- Harga tidak boleh bernilai negatif atau nol
- Jumlah barang tidak boleh melebihi stok
- Konfirmasi pembayaran harus diverifikasi dari dua sisi (server + klien)

---

## 💡 Prinsip Emas Keamanan Data untuk Pengusaha

> **"Jangan percaya input dari pengguna. Selalu validasi, selalu bersihkan."**

| Prinsip | Artinya |
|---|---|
| Validasi di sisi server | Jangan hanya mengandalkan validasi di browser |
| Enkripsi data sensitif | Password, data KTP, nomor rekening |
| Prinsip "least privilege" | Setiap bagian sistem hanya boleh mengakses yang diperlukan |
| Selalu update | Keamanan bukan sekali jadi, perlu pemeliharaan rutin |

---

# BAGIAN 2: PRAKTIKUM (50 Menit)

## 🛠️ Persiapan Sebelum Mulai

1. Buka **Antigravity IDE** di komputer kamu
2. Buat folder baru bernama `toko-online-saya`
3. Siapkan ide bisnis kamu (nama toko, produk yang dijual)

---

## 💻 PRAKTIKUM 1: Copy-Paste & Lihat Hasilnya (25 Menit)

> **Tujuan:** Memahami hasil nyata dari validasi input dengan melihat langsung output dari AI

---

### 🔵 Latihan 1A — Form Pendaftaran Pelanggan dengan Validasi

**Instruksi:** Copy prompt di bawah ini, paste ke Antigravity AI, dan tekan Enter.

```
Prompt 1A — Salin ini ke Antigravity AI:

Buatkan saya form pendaftaran pelanggan untuk toko online dengan HTML dan JavaScript.
Form ini harus memiliki validasi untuk:
- Nama lengkap: wajib diisi, minimal 3 karakter, hanya huruf dan spasi
- Email: wajib diisi, format email yang valid
- Nomor HP: wajib diisi, hanya angka, panjang 10-13 digit
- Password: minimal 8 karakter, harus ada huruf besar, huruf kecil, dan angka
- Konfirmasi password: harus sama dengan password

Tampilkan pesan error yang jelas dan ramah dalam Bahasa Indonesia.
Buat tampilannya menarik dengan CSS sederhana berwarna biru dan putih.
```

**Setelah AI merespons:**
- ✅ Lihat kode yang dihasilkan — kamu tidak perlu memahami semua barisnya!
- ✅ Klik tombol **Preview** untuk melihat tampilan form
- ✅ Coba isi form dengan data yang salah dan lihat pesan errornya
- ✅ Coba isi form dengan data yang benar dan lihat hasilnya

**Pertanyaan refleksi:**
> Coba isi kolom email tanpa tanda `@`. Apa yang terjadi? Inilah validasi bekerja!

---

### 🔵 Latihan 1B — Form Login dengan Proteksi

```
Prompt 1B — Salin ini ke Antigravity AI:

Tambahkan halaman login ke proyek toko online saya dengan keamanan berikut:
- Form login dengan email dan password
- Batasi percobaan login maksimal 5 kali (setelah itu akun terkunci 15 menit)
- Tampilkan pesan yang TIDAK memberitahu apakah email atau password yang salah
  (cukup tulis "Email atau password tidak valid" bukan "Password salah")
- Tambahkan tombol "Tampilkan/Sembunyikan Password"
- Validasi: email harus terisi dan format valid, password tidak boleh kosong

Gunakan Bahasa Indonesia untuk semua teks.
```

**Yang perlu diperhatikan:**
- 🔍 Perhatikan bahwa pesan error tidak spesifik — ini disengaja untuk keamanan!
- 🔍 Coba klik login 5 kali dengan data salah — lihat apa yang terjadi

---

### 🔵 Latihan 1C — Form Pemesanan dengan Validasi Transaksi

```
Prompt 1C — Salin ini ke Antigravity AI:

Buatkan form pemesanan produk untuk toko online saya dengan validasi lengkap:
- Nama produk: dropdown pilihan (isi dengan 3 produk contoh bebas)
- Jumlah: hanya angka positif, minimal 1, maksimal 100
- Nama penerima: wajib diisi, minimal 3 karakter
- Alamat pengiriman: wajib diisi, minimal 10 karakter
- Nomor HP penerima: format nomor HP Indonesia (diawali 08 atau +62)
- Catatan tambahan: opsional, maksimal 200 karakter

Tambahkan ringkasan pesanan yang muncul di samping form (simulasi harga bebas).
Tampilkan semua validasi dalam Bahasa Indonesia.
```

---

### 🔵 Latihan 1D — Form Kontak dengan Anti-Spam

```
Prompt 1D — Salin ini ke Antigravity AI:

Buatkan form kontak pelanggan untuk website bisnis saya dengan:
- Nama: wajib diisi
- Email: wajib diisi, format valid
- Subjek: wajib diisi, minimal 5 karakter
- Pesan: wajib diisi, minimal 20 karakter, maksimal 500 karakter
- Tambahkan penghitung karakter real-time di kolom pesan
- Tambahkan simple CAPTCHA (pertanyaan matematika sederhana, contoh: "Berapa 3 + 5?")
  untuk mencegah spam dari robot

Buat tampilan yang profesional dan ramah pengguna.
```

**Apa itu CAPTCHA?**
> CAPTCHA = pertanyaan atau tantangan yang hanya bisa dijawab manusia, bukan robot/program otomatis. Ini melindungi form kamu dari spam.

---

### 🔵 Latihan 1E — Form Pembayaran Sederhana

```
Prompt 1E — Salin ini ke Antigravity AI:

Buatkan halaman ringkasan pembayaran untuk toko online saya dengan validasi:
- Pilihan metode pembayaran: Transfer Bank, QRIS, COD (pilih salah satu)
- Jika Transfer Bank: tampilkan field upload bukti transfer (hanya file JPG/PNG, maks 2MB)
- Jika QRIS: tampilkan gambar QR code simulasi
- Jika COD: tampilkan konfirmasi alamat

Tambahkan tombol konfirmasi dengan:
- Checkbox persetujuan syarat & ketentuan (wajib dicentang)
- Konfirmasi ulang sebelum submit: "Apakah kamu yakin ingin melanjutkan pembayaran?"

Semua teks dalam Bahasa Indonesia.
```

---

## 💻 PRAKTIKUM 2: Modifikasi Prompt Sesuai Bisnismu (25 Menit)

> **Tujuan:** Belajar mengadaptasi prompt untuk kebutuhan bisnis spesifik kamu

---

### 🟡 Cara Memodifikasi Prompt

Lihat **template** berikut. Bagian yang ditandai `[GANTI INI]` adalah yang perlu kamu sesuaikan:

```
Template Dasar — Modifikasi sesuai bisnismu:

Buatkan form [GANTI: jenis form apa?] untuk bisnis [GANTI: nama bisnis kamu]
yang bergerak di bidang [GANTI: bidang bisnis kamu].

Form ini harus memvalidasi:
- [GANTI: field 1 dan aturan validasinya]
- [GANTI: field 2 dan aturan validasinya]
- [GANTI: field 3 dan aturan validasinya]

Keamanan yang harus diterapkan:
- [GANTI: pilih dari daftar keamanan di bawah]

Warna tema bisnis saya adalah [GANTI: warna utama] dan [GANTI: warna sekunder].
Semua teks dalam Bahasa Indonesia.
```

**Daftar keamanan yang bisa dipilih dan ditambahkan:**
- `Validasi format email yang ketat`
- `Validasi nomor HP format Indonesia`
- `Password strength indicator (indikator kekuatan password)`
- `Batasi upload file hanya [GANTI: tipe file] maksimal [GANTI: ukuran]`
- `Sanitasi input untuk mencegah karakter berbahaya`
- `Batas maksimal karakter untuk mencegah data berlebihan`
- `Verifikasi ulang data sebelum submit`

---

### 🟡 Latihan 2A — Buat Form Versi Bisnismu

**Tugas:** Buat **satu form** yang benar-benar relevan dengan bisnis kamu.

Gunakan template di atas, ganti semua bagian `[GANTI INI]`.

**Contoh hasil modifikasi untuk bisnis catering:**
```
Contoh Prompt yang Sudah Dimodifikasi:

Buatkan form pemesanan katering untuk bisnis "Dapur Bu Sari"
yang bergerak di bidang jasa katering pernikahan dan acara.

Form ini harus memvalidasi:
- Nama pemesan: wajib diisi, minimal 3 karakter, hanya huruf dan spasi
- Tanggal acara: wajib diisi, tidak boleh tanggal yang sudah lewat,
  minimal 7 hari dari sekarang (untuk persiapan)
- Jumlah tamu: hanya angka, minimal 50, maksimal 1000 orang
- Jenis menu: dropdown pilihan (Nasi Box, Prasmanan, Set Menu)
- Lokasi acara: wajib diisi, minimal 20 karakter
- Budget per orang: hanya angka, minimal Rp 25.000

Keamanan yang harus diterapkan:
- Validasi format email yang ketat untuk konfirmasi pesanan
- Validasi nomor HP format Indonesia untuk koordinasi
- Verifikasi ulang data sebelum submit
- Batas maksimal 500 karakter untuk catatan tambahan

Warna tema bisnis saya adalah hijau tua dan emas.
Semua teks dalam Bahasa Indonesia.
```

---

### 🟡 Latihan 2B — Uji Keamanan dengan Prompt "Hacker Baik"

Setelah form bisnismu selesai, gunakan prompt berikut untuk menguji keamanannya:

```
Prompt Pengujian Keamanan:

Saya sudah membuat form [GANTI: nama form kamu] di atas.
Sekarang tolong:

1. Simulasikan 5 cara pengguna jahat bisa mencoba menyalahgunakan form ini
2. Untuk setiap cara, tunjukkan apakah validasi yang sudah ada berhasil mencegahnya
3. Jika ada celah keamanan yang ditemukan, perbaiki kodenya
4. Berikan laporan singkat: "Form ini sudah aman dari: [daftar]"
   dan "Form ini masih perlu ditingkatkan untuk: [daftar]"

Jelaskan semuanya dalam bahasa yang mudah dipahami oleh pemilik bisnis non-teknis.
```

---

### 🟡 Latihan 2C — Prompt Lanjutan: Panduan Keamanan Rutin

```
Prompt Perawatan Keamanan:

Sebagai pemilik bisnis online non-teknis, buatkan saya:

1. Checklist keamanan bulanan yang bisa saya lakukan sendiri
   (tanpa perlu mengerti coding) untuk website toko online saya
2. Tanda-tanda bahaya (red flags) yang harus saya waspadai
   yang menandakan website saya mungkin sedang diserang
3. Apa yang harus saya lakukan PERTAMA KALI jika saya mencurigai
   ada kebocoran data pelanggan?
4. Rekomendasi tools gratis yang bisa membantu menjaga keamanan website
   (jelaskan cara pakainya dengan sederhana)

Sampaikan dalam format yang bisa saya cetak dan tempel di kantor.
```

---

# BAGIAN 3: REVIEW & TANYA JAWAB (20 Menit)

## 📊 Checklist Evaluasi Diri

Sebelum sesi berakhir, periksa hasil kerjamu:

| ✅ | Kemampuan | Sudah Bisa? |
|---|---|---|
| ☐ | Memahami mengapa validasi input penting untuk bisnis | |
| ☐ | Membuat prompt untuk form dengan validasi lengkap | |
| ☐ | Memodifikasi prompt sesuai kebutuhan bisnis spesifik | |
| ☐ | Menggunakan AI untuk menguji keamanan form | |
| ☐ | Memahami ancaman SQL Injection & XSS tanpa jargon teknis | |
| ☐ | Tahu apa yang harus dilakukan jika ada masalah keamanan | |

---

## 🔑 Ringkasan Kunci yang Wajib Diingat

> **1. Validasi selalu.** Jangan percaya data yang diisi pengguna begitu saja.

> **2. Password harus dienkripsi.** Jangan simpan password dalam bentuk aslinya.

> **3. Pesan error jangan terlalu spesifik.** Jangan kasih tahu penyerang apa yang salah.

> **4. AI adalah tim kamu.** Selalu minta AI untuk menambahkan keamanan di setiap form.

> **5. Keamanan adalah proses, bukan produk.** Perlu dicek secara rutin.

---

## 💬 Prompt Andalan untuk Diingat

Simpan prompt-prompt ini sebagai "senjata" kamu untuk proyek ke depan:

```
🛡️ Prompt Keamanan Standar (Gunakan untuk Setiap Form Baru):

Buatkan [jenis form] dengan keamanan standar berikut:
- Validasi semua field sebelum data dikirim
- Sanitasi input untuk mencegah karakter berbahaya
- Pesan error yang informatif tapi tidak membocorkan informasi teknis
- Password (jika ada) harus di-hash menggunakan bcrypt
- Batasi ukuran dan format file upload (jika ada)
- Konfirmasi sebelum submit untuk form penting
Jelaskan dalam komentar kode apa yang dilakukan setiap bagian keamanan.
```

---

## 📚 Kamus Istilah (Bahasa Manusia)

| Istilah Teknis | Artinya untuk Pengusaha |
|---|---|
| **Validasi Input** | Proses memeriksa apakah data yang diisi benar dan sesuai format |
| **Enkripsi / Hashing** | Mengubah data penting menjadi kode acak agar tidak bisa dibaca orang lain |
| **SQL Injection** | Cara penyerang "menipu" sistem dengan memasukkan perintah jahat lewat form |
| **XSS** | Cara penyerang menyisipkan kode berbahaya yang bisa menyerang pengunjung website |
| **CAPTCHA** | Pertanyaan/tantangan untuk memastikan yang mengisi form adalah manusia, bukan robot |
| **Sanitasi** | Membersihkan input dari karakter berbahaya sebelum disimpan |
| **Plaintext** | Data dalam bentuk aslinya, tidak dienkripsi (berbahaya untuk password) |
| **Bcrypt** | Cara aman untuk menyimpan password (diubah menjadi kode acak) |
| **Parameterized Query** | Cara aman menyimpan data ke database yang mencegah SQL Injection |

---

## 🏆 Tantangan Bonus (Untuk yang Sudah Selesai Lebih Awal)

Gunakan prompt berikut untuk membuat **dashboard keamanan sederhana**:

```
Prompt Bonus:

Buatkan halaman admin sederhana untuk toko online saya yang menampilkan:
1. Log aktivitas login (siapa, kapan, dari mana - tampilkan sebagai data dummy)
2. Daftar percobaan login yang gagal (lebih dari 3 kali)
3. Status keamanan form (hijau = aman, kuning = perlu perhatian, merah = bahaya)
4. Tombol "Kunci Sementara" untuk memblokir pengguna yang mencurigakan

Buat tampilan yang bersih dan mudah dibaca oleh pemilik bisnis non-teknis.
Data bisa berupa simulasi/dummy untuk keperluan latihan.
```

---

## 📌 Tugas Lanjutan (Dikerjakan di Rumah)

1. **Terapkan** validasi pada salah satu form yang sudah kamu buat untuk bisnis nyata kamu
2. **Screenshot** hasil form dan validasinya
3. **Kirim** ke grup WhatsApp/Telegram kelas dengan format:
   - Nama bisnis kamu
   - Jenis form yang dibuat
   - 3 validasi utama yang kamu tambahkan
   - Tantangan yang kamu temui

---

*📅 Modul ini dibuat untuk Program Vibe Coding — Keamanan Data & Validasi Input*
*🛠️ Menggunakan: Antigravity AI & Antigravity IDE*
*📖 Bahasa: Indonesia | Level: Pemula (Non-IT)*
