# 🤖 Prompt Deploy ke Vercel — Edisi Pemula Non-IT
## Tinggal Copy-Paste ke AI, Biarkan AI yang Pandu!

---

> [!NOTE]
> **Cara menggunakan halaman ini:**
> Temukan situasimu → Copy prompt yang sesuai → Paste ke AI (Antigravity/ChatGPT/Claude) → Ikuti panduan AI langkah demi langkah!
>
> 🟢 = Prompt wajib (harus digunakan)
> 🟡 = Prompt opsional (jika butuh)
> 🔴 = Prompt darurat (jika ada masalah)

---

## 🗺️ Peta Perjalanan Deploy

![Peta Arsitektur 5 Fase Perjalanan Deploy Vercel dan Supabase](peta_deploy_vercel_5fase.jpg)

```
FASE A          FASE B            FASE C           FASE D          FASE E
Siapkan       Upload ke          Deploy ke         Update        Supabase
Laptop    →   GitHub      →      Vercel      →    Aplikasi  →   (Database)
(Prompt 1-3)  (Prompt 4-6)      (Prompt 7-9)    (Prompt 10-11) (Prompt 12-18)
```

> [!NOTE]
> **Kapan perlu Supabase?**
> Jika aplikasimu butuh menyimpan data (login pengguna, formulir, daftar produk, dll), kamu butuh **database**. Supabase adalah database gratis yang sangat mudah digunakan dan bekerja sempurna bersama Vercel.

---

---

# 🅰️ FASE A — Siapkan Laptop
### *"Sebelum mengirim, pastikan barangnya sudah siap"*

---

## 🟢 PROMPT A-1 — Cek Kesiapan Laptop

**Kapan digunakan:** Di awal sesi, sebelum mulai apapun.

**Penjelasan:**
> Ibarat sebelum masak, kita cek dulu bahan-bahannya ada atau tidak. Prompt ini meminta AI untuk memandu kamu mengecek apakah semua alat yang dibutuhkan sudah terinstal di laptopmu.

```
Halo! Saya adalah pemula yang sama sekali belum pernah coding atau deploy 
website. Hari ini saya ingin deploy website HTML saya ke internet 
menggunakan Vercel.

Tolong panduan saya untuk mengecek apakah laptop saya sudah siap.
Saya perlu tahu cara cek apakah Git sudah terinstall atau belum.

Sistem operasi saya: [Windows/Mac/Linux]

Berikan instruksi langkah demi langkah yang sangat detail.
Jelaskan setiap langkah dalam bahasa yang mudah dipahami orang 
yang belum pernah coding sama sekali. 
Sertakan gambar/emoji untuk membantu saya memahami.
```

**Apa yang akan AI lakukan:**
- Minta kamu buka Command Prompt / Terminal
- Minta kamu ketik perintah untuk cek Git
- Beritahu apakah perlu install atau tidak
- Berikan link download jika perlu install

---

## 🟢 PROMPT A-2 — Siapkan Folder Proyek

**Kapan digunakan:** Setelah kode dari AI sudah jadi dan siap disimpan.

**Penjelasan:**
> Ini seperti menyiapkan amplop sebelum mengirim surat. Kita butuh "wadah" (folder) yang rapi sebelum mengirim kode ke GitHub. Prompt ini memandu kamu membuat folder proyek yang benar.

```
Saya sudah punya kode website dalam bentuk file HTML yang saya dapatkan 
dari AI. Sekarang saya perlu menyimpannya dengan benar di laptop saya 
sebelum upload ke GitHub.

Tolong panduan saya:
1. Cara membuat folder baru untuk proyek saya
2. Cara menyimpan file HTML saya di folder tersebut
3. Cara membuka folder tersebut di VS Code

Nama proyek saya: [TULIS NAMA PROYEKMU, contoh: website-portofolio]
Sistem operasi saya: [Windows/Mac/Linux]

Berikan panduan yang sangat detail dengan langkah bernomor.
Sertakan apa yang harus saya lihat di layar setelah setiap langkah
agar saya tahu apakah berhasil atau tidak.
```

**Apa yang akan AI lakukan:**
- Panduan buat folder di lokasi yang tepat
- Cara simpan file `index.html` di folder
- Cara buka folder di VS Code

---

## 🟡 PROMPT A-3 — Cek Kode di Browser Lokal Dulu

**Kapan digunakan:** Sebelum upload, untuk memastikan kode berfungsi.

**Penjelasan:**
> Sebelum pamerkan ke semua orang, cek dulu di kacamu sendiri! Kalau website terlihat benar di laptopmu, baru kita kirim ke Vercel. Ini menghemat banyak waktu dan kebingungan.

```
Saya punya file index.html di folder [NAMA FOLDER] di laptop saya.
Saya ingin melihat tampilan website saya di browser sebelum upload 
ke internet, untuk memastikan semuanya sudah benar.

Tolong jelaskan cara paling mudah untuk membuka file HTML lokal 
di browser (Chrome/Firefox/Edge).

Saya tidak punya keahlian teknis apapun, jadi tolong berikan 
panduan yang sangat sederhana. Jelaskan juga apa yang normal 
dan apa yang tidak normal saat melihat website lokal.
```

---

---

# 🅱️ FASE B — Upload ke GitHub
### *"Menitipkan kodeму di 'gudang' cloud bernama GitHub"*

---

## 🟢 PROMPT B-1 — Buat Akun & Repository GitHub

**Kapan digunakan:** Jika belum punya akun GitHub atau belum tahu cara buat repository.

**Penjelasan:**
> GitHub itu seperti Google Drive tapi khusus untuk kode. Sebelum bisa "menaruh" kode di sana, kamu perlu daftar akun dan membuat "folder cloud" yang disebut **repository**. Prompt ini memandu kamu dari nol.

```
Saya belum pernah menggunakan GitHub sebelumnya. Saya perlu membuat 
akun GitHub dan menyiapkan sebuah repository (folder cloud) untuk 
menyimpan proyek website saya.

Tolong panduan saya langkah demi langkah untuk:
1. Mendaftar akun GitHub (jika belum punya)
2. Membuat repository baru yang bernama [NAMA PROYEK]
3. Mengatur repository sebagai Public (bukan Private)

Jelaskan setiap istilah teknis yang muncul dalam bahasa sederhana.
Beritahu saya apa yang harus saya klik, apa yang harus saya isi,
dan apa yang harus saya lihat setelah setiap langkah.

Saya menggunakan browser [Chrome/Firefox/Edge].
```

**Apa yang akan AI lakukan:**
- Panduan daftar di github.com
- Cara buat repository baru
- Penjelasan pilihan Public vs Private
- Screenshot deskripsi tiap tombol

---

## 🟢 PROMPT B-2 — Setup Git di Laptop (Konfigurasi Pertama Kali)

**Kapan digunakan:** Pertama kali menggunakan Git di laptop ini.

**Penjelasan:**
> Git itu seperti petugas pengiriman di laptopmu. Sebelum dia bisa kirim kode, kita perlu kasih tahu siapa namanya dan emailnya (agar GitHub tahu siapa yang mengirim). Ini hanya perlu dilakukan satu kali!

```
Ini adalah pertama kalinya saya menggunakan Git di laptop saya.
Git sudah terinstall, tapi saya perlu mengkonfigurasinya dengan 
identitas saya sebelum bisa menggunakannya.

Tolong panduan saya untuk:
1. Membuka Command Prompt (atau Terminal) dengan benar
2. Memasukkan nama dan email saya ke Git
3. Memverifikasi bahwa konfigurasi berhasil

Nama saya: [NAMA KAMU]
Email GitHub saya: [EMAIL YANG DIDAFTARKAN DI GITHUB]
Sistem operasi: [Windows/Mac/Linux]

Jelaskan setiap perintah yang harus saya ketik dan apa artinya
dalam bahasa yang sangat sederhana. Saya tidak perlu hafal, 
hanya perlu mengerti garis besarnya.
```

---

## 🟢 PROMPT B-3 — Upload Kode ke GitHub (Push)

**Kapan digunakan:** Setelah folder proyek siap dan repository GitHub sudah dibuat.

**Penjelasan:**
> Ini adalah momen "pengiriman"! Kita akan menggunakan perintah Git untuk mengirim file dari laptopmu ke repository di GitHub. Prosesnya seperti mengirim email dengan lampiran — ada beberapa langkah, tapi setelah paham, terasa mudah.

```
Saya sudah punya:
✅ Folder proyek di laptop dengan file index.html di dalamnya
✅ Akun GitHub
✅ Repository GitHub kosong bernama [NAMA REPOSITORY]
✅ Git sudah terinstall dan dikonfigurasi

Sekarang saya perlu mengirim (upload) file saya dari laptop ke 
GitHub. Saya tahu ini menggunakan perintah Git tapi saya tidak 
tahu cara menggunakannya.

Tolong berikan panduan langkah demi langkah yang sangat detail untuk:
1. Membuka Terminal/Command Prompt di folder proyek saya
2. Menginisialisasi Git di folder tersebut
3. Menandai semua file untuk dikirim
4. Membuat "snapshot" pertama (commit)
5. Menghubungkan laptop ke GitHub
6. Mengirim kode ke GitHub (push)

Lokasi folder proyek saya: [TULIS PATH FOLDER, contoh: C:\Users\Nama\proyek]
Username GitHub saya: [USERNAME GITHUB]
Nama repository: [NAMA REPOSITORY]

Sertakan perintah yang harus saya ketik persis sama, dan 
jelaskan setiap perintah dengan analogi sehari-hari.
Beritahu juga apa yang akan saya lihat di layar jika berhasil.
```

**Apa yang akan AI lakukan:**
- Perintah `git init`, `git add .`, `git commit`, `git push` satu per satu
- Penjelasan tiap perintah dengan analogi
- Panduan jika diminta username/password
- Cara verifikasi bahwa file sudah masuk GitHub

---

## 🔴 PROMPT B-DARURAT — Diminta Password saat Push

**Kapan digunakan:** Saat muncul permintaan password dan password GitHub tidak diterima.

**Penjelasan:**
> GitHub sudah tidak menerima password biasa untuk keamanan. Mereka pakai sistem "token" khusus. Jangan panik — ini sangat umum dialami semua orang pertama kali!

```
Saya baru saja mencoba git push ke GitHub tapi muncul pesan error
atau diminta memasukkan password dan password GitHub saya tidak berhasil.

Pesan yang muncul di layar saya:
[PASTE PESAN ERROR ATAU DESKRIPSI YANG MUNCUL]

Saya tahu bahwa GitHub menggunakan "Personal Access Token" 
sebagai pengganti password. Tolong panduan saya cara:
1. Membuat Personal Access Token di GitHub
2. Menggunakannya sebagai pengganti password saat git push
3. Menyimpannya agar tidak perlu diketik berulang kali

Jelaskan dalam bahasa yang sangat sederhana karena saya 
tidak punya latar belakang IT.
```

---

---

# 🅲 FASE C — Deploy ke Vercel
### *"Membuka 'toko' ke publik di internet!"*

---

## 🟢 PROMPT C-1 — Setup Akun & Hubungkan GitHub ke Vercel

**Kapan digunakan:** Pertama kali menggunakan Vercel.

**Penjelasan:**
> Vercel adalah platform yang akan "menjalankan" websitemu di internet. Kita perlu mendaftarkan akun Vercel dan menghubungkannya ke GitHub agar Vercel bisa "membaca" kode yang kita upload tadi. Ini hanya dilakukan sekali!

```
Saya baru pertama kali menggunakan Vercel untuk deploy website.
Kode saya sudah ada di GitHub di repository bernama [NAMA REPOSITORY].

Tolong panduan saya untuk:
1. Membuat akun Vercel menggunakan akun GitHub saya
   (bukan email/password biasa)
2. Memberikan izin kepada Vercel untuk membaca repository GitHub saya
3. Memahami tampilan dashboard Vercel pertama kali

Jelaskan kenapa kita perlu menghubungkan Vercel ke GitHub
dan apa yang terjadi "di balik layar" dalam bahasa yang 
mudah dipahami orang awam.

Saya menggunakan browser [Chrome/Firefox/Edge].
```

---

## 🟢 PROMPT C-2 — Import Proyek & Deploy

**Kapan digunakan:** Setelah akun Vercel siap dan terhubung ke GitHub.

**Penjelasan:**
> Ini adalah momen besar! Kita akan "memberitahu" Vercel repository mana yang mau di-deploy, lalu Vercel akan bekerja secara otomatis untuk membuat websitemu bisa diakses dari mana saja di seluruh dunia. Prosesnya hanya butuh beberapa klik!

```
Akun Vercel saya sudah terhubung ke GitHub. Sekarang saya ingin 
men-deploy repository GitHub saya bernama [NAMA REPOSITORY] yang 
berisi file index.html (website HTML biasa, bukan framework apapun).

Tolong panduan saya langkah demi langkah untuk:
1. Menemukan tombol untuk menambah proyek baru di Vercel
2. Memilih repository yang tepat
3. Mengatur konfigurasi yang benar untuk website HTML sederhana
   (bukan Next.js, bukan React — hanya HTML biasa)
4. Menekan tombol deploy
5. Memahami proses yang terjadi saat Vercel sedang deploy
6. Menemukan link website saya setelah deploy selesai

Beritahu saya pengaturan apa yang TIDAK perlu saya ubah
dan pengaturan apa yang penting untuk diperhatikan.
Saya tidak paham istilah teknis seperti "build command" 
atau "output directory" — tolong jelaskan dalam bahasa sederhana.
```

**Apa yang akan AI lakukan:**
- Panduan klik "New Project" di Vercel
- Cara pilih repository dari GitHub
- Pengaturan untuk HTML biasa (Framework: Other)
- Cara baca status deploy
- Cara temukan link `.vercel.app`

---

## 🟡 PROMPT C-3 — Custom Nama Domain Vercel (Bonus)

**Kapan digunakan:** Setelah berhasil deploy, ingin nama link yang lebih bagus.

**Penjelasan:**
> Vercel otomatis memberi nama link yang agak random seperti `proyek-abc123.vercel.app`. Kita bisa menggantinya jadi nama yang lebih keren dan mudah diingat seperti `portofolio-budi.vercel.app`. Ini gratis!

```
Website saya sudah berhasil deploy di Vercel dan saya mendapat link:
[LINK VERCEL KAMU SEKARANG]

Saya ingin mengganti nama subdomain (bagian sebelum .vercel.app) 
menjadi nama yang lebih mudah diingat dan personal.

Nama yang saya inginkan: [NAMA YANG KAMU MAU]
Contoh: portofolio-budi.vercel.app

Tolong panduan saya cara mengubah nama subdomain Vercel
tanpa merusak website yang sudah berjalan.
Apakah ada batasan nama yang boleh digunakan?
```

---

---

# 🅳 FASE D — Update Aplikasi
### *"Mengirim 'edisi baru' dari aplikasimu"*

---

## 🟢 PROMPT D-1 — Update Kode & Re-Deploy Otomatis

**Kapan digunakan:** Setiap kali kamu edit kode dan mau publish perubahan ke internet.

**Penjelasan:**
> Ini adalah salah satu fitur terbaik Vercel — setiap kali kamu kirim perubahan ke GitHub, Vercel OTOMATIS memperbarui websitemu di internet! Tidak perlu deploy ulang secara manual. Prompt ini memandu cara "mengirim" update tersebut.

```
Saya sudah berhasil deploy website ke Vercel sebelumnya.
Sekarang saya sudah mengubah/mengedit file index.html saya
dan ingin perubahan tersebut langsung tampil di website online saya.

Perubahan yang saya buat: [DESKRIPSI SINGKAT PERUBAHANNYA]

Tolong panduan saya cara mengirim perubahan ini ke GitHub
agar Vercel otomatis memperbarui website saya.

Langkah-langkah yang saya butuhkan:
1. Cara menyimpan file yang sudah diedit
2. Perintah Git untuk mengirim perubahan
3. Cara memverifikasi bahwa Vercel sudah mendeteksi perubahan
4. Berapa lama biasanya butuh waktu untuk update selesai

Tolong ingatkan saya juga apa arti setiap perintah Git 
yang saya jalankan — saya sedang dalam proses belajar.
```

---

## 🟡 PROMPT D-2 — Lihat Riwayat Deploy

**Kapan digunakan:** Jika ingin melihat semua versi deploy atau "mundur" ke versi sebelumnya.

**Penjelasan:**
> Vercel menyimpan riwayat setiap deploy yang pernah kamu lakukan. Kalau update terakhirmu merusak website, kamu bisa kembali ke versi yang masih bagus. Ini seperti fitur "Undo" tapi untuk website!

```
Saya ingin melihat daftar semua versi deploy website saya di Vercel.
Saya juga ingin tahu cara kembali ke versi sebelumnya jika 
update terbaru saya ternyata merusak tampilan website.

Tolong jelaskan:
1. Di mana saya bisa melihat riwayat deployment di dashboard Vercel
2. Cara membaca informasi di halaman deployment history
3. Cara "rollback" (kembali) ke versi yang lebih lama jika perlu
4. Apakah rollback aman dilakukan dan apa risikonya

Jelaskan dalam bahasa yang mudah dipahami orang awam.
```

---

---

# 🆘 PROMPT DARURAT — Untuk Situasi Panik

---

## 🔴 PROMPT PANIK-1 — Layar Error Penuh, Tidak Tahu Harus Apa

**Kapan digunakan:** Saat muncul tulisan merah/pesan aneh dan kamu bingung total.

```
TOLONG BANTU SAYA! Saya sedang mencoba deploy website ke Vercel 
tapi layar saya penuh dengan pesan yang tidak saya mengerti.

Ini pesan/error yang saya lihat:
[COPY-PASTE SEMUA PESAN ERROR YANG MUNCUL DI SINI]

Saya sedang ada di tahap: [PILIH SALAH SATU]
- Mencoba menjalankan perintah Git di Command Prompt
- Mencoba upload ke GitHub
- Mencoba deploy di Vercel

Sistem operasi saya: [Windows/Mac/Linux]

Tolong:
1. Jelaskan apa yang salah dalam bahasa yang SANGAT sederhana
2. Berikan solusi langkah demi langkah yang bisa saya ikuti sekarang
3. Beritahu cara mencegah masalah ini terulang

Saya tidak punya latar belakang IT sama sekali.
```

---

## 🔴 PROMPT PANIK-2 — Website Deploy tapi Tampil Kosong/Error

**Kapan digunakan:** Deploy berhasil, ada link Vercel, tapi website tidak tampil dengan benar.

```
Website saya sudah berhasil deploy ke Vercel dan saya punya linknya:
[LINK VERCEL KAMU]

Tapi ketika saya buka link tersebut, yang saya lihat adalah:
[PILIH/DESKRIPSI]
- Halaman putih kosong
- Tulisan "404 Not Found"  
- Tampilan rusak/berantakan
- Error message: [TULIS ERRORNYA]

Kode saya adalah file index.html yang berisi [DESKRIPSI SINGKAT WEBSITEMU].

Tolong bantu saya:
1. Mendiagnosis apa penyebab masalah ini
2. Memberikan solusi yang bisa saya lakukan sekarang
3. Cara mencegah masalah ini terulang

Berikan jawaban yang sangat mudah dipahami pemula non-IT.
```

---

## 🔴 PROMPT PANIK-3 — Lupa/Bingung Sudah Sampai Mana

**Kapan digunakan:** Setelah istirahat atau bingung sudah ada di tahap mana.

```
Saya sedang dalam proses deploy website ke Vercel tapi saya 
kehilangan jejak sudah sampai tahap mana.

Yang saya ingat sudah berhasil lakukan:
[CENTANG YANG SUDAH SELESAI]
☐ Buat akun GitHub
☐ Buat repository GitHub
☐ Install Git di laptop
☐ Konfigurasi Git (nama & email)
☐ Simpan kode di folder proyek
☐ Berhasil git push ke GitHub
☐ Buat akun Vercel
☐ Hubungkan Vercel ke GitHub
☐ Deploy proyek di Vercel
☐ Punya link .vercel.app

Berdasarkan checklist di atas, tolong beritahu saya:
1. Tahap mana yang harus saya lakukan selanjutnya
2. Cara cepat memverifikasi tahap mana yang sudah benar-benar berhasil
3. Panduan untuk melanjutkan dari titik ini

Nama repository GitHub saya: [NAMA REPO, jika sudah ada]
Link Vercel saya: [LINK, jika sudah ada]
```

---

---

---

# 🅴 FASE E — Supabase (Database Online)
### *"Tempat menyimpan data aplikasimu — seperti Google Sheets tapi untuk aplikasi!"*

---

## 🧠 Apa itu Supabase & Kenapa Perlu?

> **Analogi:** Bayangkan websitemu adalah sebuah warung. Vercel = bangunan warungnya. Supabase = buku catatan pesanan di dalam warung.
>
> Tanpa Supabase → setiap kali warung tutup (browser ditutup), semua catatan hilang.
> Dengan Supabase → semua data tersimpan permanen & bisa diakses dari mana saja.

**Kapan kamu butuh Supabase?**

| Jika aplikasimu punya fitur ini... | Perlu Supabase? |
|------------------------------------|-----------------|
| Form pendaftaran / login | ✅ Ya |
| Menyimpan data dari pengguna | ✅ Ya |
| Daftar produk / konten yang bisa diubah | ✅ Ya |
| Hanya menampilkan informasi statis | ❌ Tidak perlu |
| Kalkulator / game sederhana | ❌ Tidak perlu |

---

## 🔗 Bagaimana Supabase Bekerja dengan Vercel?

```
[Pengguna buka website]        [Supabase]
         ↓                          ↑  ↓
[Vercel tampilkan website]  ←→  [Simpan & ambil data]

Vercel = "Etalase toko"  |  Supabase = "Gudang stok barang"
```

**Yang penting dipahami:**
- Supabase punya **URL** dan **API Key** (seperti alamat & kata sandi gudang)
- Kode di Vercel pakai URL + Key itu untuk bicara ke Supabase
- Key harus disimpan **rahasia** → tidak boleh ada di kode publik → pakai **Environment Variable**

---

## 🟢 PROMPT E-1 — Buat Akun Supabase & Proyek Pertama

**Kapan digunakan:** Pertama kali menggunakan Supabase.

**Penjelasan:**
> Seperti mendaftar Google Drive baru khusus untuk data aplikasimu. Kamu cukup daftar akun, lalu buat "proyek" (wadah database) untuk aplikasimu.

```
Saya ingin menggunakan Supabase sebagai database untuk website saya
yang sudah di-deploy di Vercel. Saya sama sekali belum pernah
menggunakan Supabase atau database apapun sebelumnya.

Tolong panduan saya langkah demi langkah untuk:
1. Mendaftar akun Supabase (gratis)
2. Membuat proyek baru di Supabase
3. Memilih region server yang tepat untuk Indonesia
4. Memahami tampilan dashboard Supabase pertama kali
5. Menemukan URL dan API Key proyek saya

Nama proyek yang ingin saya buat: [NAMA PROYEK]

Jelaskan setiap istilah teknis dalam bahasa sederhana.
Beritahu apa yang harus saya simpan/catat setelah setiap langkah.
Saya tidak punya latar belakang IT sama sekali.
```

**Apa yang akan AI lakukan:**
- Panduan daftar di supabase.com
- Cara buat proyek baru + pilih region Asia (Singapore)
- Cara temukan `Project URL` dan `anon public key`
- Peringatan apa yang harus disalin dan disimpan

---

## 🟢 PROMPT E-2 — Buat Tabel Database (Tanpa Coding!)

**Kapan digunakan:** Setelah akun Supabase siap, perlu menyiapkan struktur data.

**Penjelasan:**
> Database itu seperti spreadsheet Excel — ada kolom dan baris. Supabase punya antarmuka visual (Table Editor) yang bisa kamu pakai untuk membuat tabel **tanpa perlu coding** sama sekali. Ini semudah membuat tabel di Word!

```
Saya sudah punya akun Supabase dan proyek bernama [NAMA PROYEK].
Sekarang saya perlu membuat tabel database untuk menyimpan data
aplikasi saya.

Aplikasi saya adalah: [DESKRIPSI APLIKASI, contoh: "sistem pendaftaran peserta seminar"]
Data yang perlu disimpan: [DESKRIPSI DATA, contoh: "nama, email, nomor HP, pilihan sesi"]

Tolong panduan saya cara:
1. Membuka Table Editor di dashboard Supabase
2. Membuat tabel baru dengan kolom yang sesuai
3. Memilih tipe data yang tepat untuk setiap kolom
   (saya tidak tahu apa itu "integer", "text", "boolean" — tolong jelaskan)
4. Menyimpan dan memverifikasi tabel sudah terbuat

Gunakan bahasa yang sangat sederhana. Analogikan tabel database
seperti spreadsheet biasa agar mudah saya pahami.
```

**Apa yang akan AI lakukan:**
- Panduan masuk ke Table Editor Supabase
- Cara tambah kolom + pilih tipe data (dengan penjelasan awam)
- Tips penamaan kolom yang baik
- Cara cek data sudah masuk dengan benar

---

## 🟢 PROMPT E-3 — Hubungkan Kode Website ke Supabase

**Kapan digunakan:** Setelah tabel siap, perlu menghubungkan kode HTML ke Supabase.

**Penjelasan:**
> Ini seperti memasukkan "nomor telepon gudang" ke dalam kode toko kamu, agar tokomu bisa menelepon gudang kapanpun butuh kirim atau ambil data. Kita akan meminta AI untuk menambahkan koneksi Supabase ke kode HTML yang sudah ada.

```
Saya punya website HTML (file index.html) yang sudah berjalan di Vercel.
Sekarang saya ingin menghubungkan website tersebut ke Supabase agar
bisa menyimpan dan menampilkan data secara dinamis.

Berikut kode website saya saat ini:
[PASTE KODE HTML KAMU DI SINI]

Detail Supabase saya:
- Project URL: [URL DARI SUPABASE DASHBOARD]
- Anon/Public Key: [KEY DARI SUPABASE DASHBOARD]
- Nama tabel: [NAMA TABEL YANG SUDAH DIBUAT]
- Kolom tabel: [DAFTAR NAMA KOLOM]

Fitur yang ingin saya tambahkan:
[PILIH YANG SESUAI]
- Menyimpan data dari form ke database Supabase
- Menampilkan data dari database di halaman website
- Keduanya (simpan dan tampilkan)

Tolong modifikasi kode saya agar terhubung ke Supabase.
Gunakan Supabase JavaScript Client (CDN) agar tidak perlu install apapun.
Simpan URL dan Key sebagai variabel yang mudah saya temukan dan ganti.
Jelaskan setiap perubahan yang kamu buat dalam komentar di kode.
```

**Apa yang akan AI lakukan:**
- Tambah Supabase CDN ke HTML
- Buat koneksi ke database
- Hubungkan form ke fungsi insert/select data
- Beri komentar penjelasan di setiap baris penting

---

## 🟢 PROMPT E-4 — Pasang Environment Variable di Vercel (PENTING!)

**Kapan digunakan:** Sebelum atau sesudah deploy ulang setelah menambahkan Supabase.

**Penjelasan:**
> Ini adalah langkah yang paling sering dilewatkan dan menjadi penyebab error! URL dan Key Supabase itu seperti kata sandi — tidak boleh ditulis langsung di kode yang bisa dilihat umum. Kita simpan di "brankas rahasia" Vercel yang disebut **Environment Variable**.
>
> **Tanpa langkah ini → website deploy tapi tidak bisa konek ke database!**

```
Website saya menggunakan Supabase untuk database dan sudah saya
deploy di Vercel. Saya tahu bahwa URL dan API Key Supabase
harus disimpan sebagai Environment Variable di Vercel,
bukan langsung di kode (karena alasan keamanan).

Detail yang perlu saya simpan sebagai Environment Variable:
- SUPABASE_URL = [URL SUPABASE KAMU]
- SUPABASE_ANON_KEY = [KEY SUPABASE KAMU]

Tolong panduan saya langkah demi langkah untuk:
1. Menemukan menu Environment Variables di dashboard Vercel
2. Menambahkan kedua variabel tersebut dengan benar
3. Memilih environment mana yang perlu diisi (Production/Preview/Development)
4. Men-trigger ulang deploy setelah menambahkan variabel
5. Memverifikasi bahwa variabel sudah aktif

Juga tolong jelaskan:
- Apa bedanya Environment Variable dengan menulis langsung di kode?
- Apakah saya perlu mengubah kode saya juga setelah ini?

Jelaskan dalam bahasa yang mudah dipahami orang awam.
```

**Apa yang akan AI lakukan:**
- Panduan buka Settings → Environment Variables di Vercel
- Cara isi Key dan Value dengan benar
- Cara centang semua environment (Production, Preview, Development)
- Cara redeploy agar perubahan aktif
- Penjelasan cara update kode agar baca env var (jika perlu)

---

## 🟡 PROMPT E-5 — Cek Koneksi Supabase Berhasil atau Tidak

**Kapan digunakan:** Setelah deploy ulang dengan konfigurasi Supabase, untuk verifikasi.

**Penjelasan:**
> Seperti mencoba menelepon gudang setelah memasukkan nomornya — kita perlu pastikan sambungannya berhasil sebelum mulai operasional. Prompt ini membantu kamu menguji apakah website sudah benar-benar terhubung ke Supabase.

```
Saya sudah:
✅ Membuat proyek dan tabel di Supabase
✅ Menambahkan kode koneksi Supabase di index.html
✅ Menambahkan Environment Variable di Vercel
✅ Re-deploy website

Sekarang saya ingin memverifikasi apakah koneksi website ke 
Supabase sudah berhasil.

Link website Vercel saya: [LINK VERCEL]
Nama tabel Supabase: [NAMA TABEL]

Tolong panduan saya cara:
1. Mengetes koneksi Supabase dari browser (tanpa coding tambahan)
2. Melihat apakah data berhasil masuk ke tabel Supabase saat saya isi form
3. Menggunakan Supabase Table Editor untuk melihat data yang masuk
4. Membaca pesan error di browser jika koneksi gagal

Saya tidak familiar dengan developer tools browser,
jadi tolong berikan panduan yang sangat sederhana.
```

---

## 🔴 PROMPT E-DARURAT-1 — Data Tidak Tersimpan ke Supabase

**Kapan digunakan:** Website berjalan, form bisa diisi, tapi data tidak masuk ke tabel Supabase.

**Penjelasan:**
> Ini adalah masalah paling umum! Biasanya karena URL/Key salah, nama tabel salah, atau ada error yang tidak terlihat. Jangan panik — ada beberapa tempat yang perlu dicek satu per satu.

```
Saya punya masalah: website saya berjalan normal di Vercel,
Form bisa diisi dan tombol submit bisa diklik,
Tapi saat saya cek di Supabase Table Editor, tidak ada data yang masuk.

Detail setup saya:
- Link website: [LINK VERCEL]
- Nama proyek Supabase: [NAMA PROYEK]
- Nama tabel: [NAMA TABEL]
- Saya [sudah/belum] menambahkan Environment Variable di Vercel

Kode yang saya gunakan untuk koneksi Supabase:
[PASTE BAGIAN KODE KONEKSI DAN FUNGSI SUBMIT FORM]

Tolong bantu saya:
1. Mendiagnosis kemungkinan penyebab data tidak tersimpan
2. Cara mengecek apakah ada error yang tersembunyi di browser
3. Cara membaca pesan error tersebut (dalam bahasa sederhana)
4. Solusi langkah demi langkah untuk setiap kemungkinan penyebab

Saya tidak punya latar belakang IT sama sekali.
```

---

## 🔴 PROMPT E-DARURAT-2 — Error: Environment Variable Tidak Terbaca

**Kapan digunakan:** Website di Vercel tidak bisa konek Supabase padahal env var sudah diisi.

**Penjelasan:**
> Environment Variable di Vercel punya aturan cara penggunaannya. Salah satu jebakan tersering: **nama variabel yang tidak sesuai** atau **lupa redeploy** setelah menambahkan variabel. Prompt ini membantu mendiagnosis masalah tersebut.

```
Saya sudah menambahkan Environment Variable di Vercel:
- Nama variabel: [NAMA VARIABEL YANG KAMU BUAT]
- Value: [DESKRIPSI VALUE, jangan paste key asli di sini]

Tapi website saya masih error dan sepertinya tidak membaca
variabel tersebut. Website lokal di laptop berjalan normal,
tapi di Vercel tidak bisa konek ke Supabase.

Pesan error yang saya lihat (jika ada):
[PASTE ERROR ATAU TULIS "tidak ada pesan error, hanya tidak berfungsi"]

Kode koneksi Supabase saya:
[PASTE BAGIAN KODE YANG MENGGUNAKAN SUPABASE URL DAN KEY]

Tolong bantu saya:
1. Cara memeriksa apakah Vercel benar-benar membaca env var saya
2. Kesalahan umum dalam penulisan nama Environment Variable
3. Apakah perlu redeploy setelah menambah env var?
4. Cara debug masalah env var di Vercel

Berikan panduan yang sangat mudah dipahami untuk orang non-IT.
```

---

## 🟡 PROMPT E-6 — Atur Keamanan Data Supabase (Row Level Security)

**Kapan digunakan:** Setelah koneksi berhasil, untuk memastikan data aman dari akses tidak sah.

**Penjelasan:**
> Supabase secara default membuka akses data untuk semua orang (karena kita pakai anon key yang publik). Kita perlu mengatur "pintu penjaga" agar hanya orang yang berhak yang bisa baca/tulis data. Di Supabase, ini disebut **Row Level Security (RLS)**.

```
Website saya sudah terhubung ke Supabase dan berjalan dengan baik.
Namun saya khawatir tentang keamanan data — apakah semua orang
bisa mengakses atau memanipulasi data di database saya?

Aplikasi saya: [DESKRIPSI SINGKAT APLIKASI]
Tabel yang ada: [NAMA-NAMA TABEL]
Siapa yang boleh akses data: [contoh: "siapa saja bisa submit form, tapi hanya admin yang bisa lihat semua data"]

Tolong jelaskan:
1. Apa itu Row Level Security (RLS) dalam bahasa awam
2. Apakah database saya saat ini aman atau tidak?
3. Bagaimana cara mengaktifkan keamanan dasar di Supabase
4. Pengaturan yang paling cocok untuk aplikasi saya

Saya tidak punya keahlian IT — gunakan analogi sehari-hari
dan panduan klik-per-klik melalui dashboard Supabase
(bukan lewat kode).
```

---

## 📋 Checklist: Supabase + Vercel Siap 100%

```
☐ Akun Supabase sudah dibuat
☐ Proyek Supabase sudah dibuat (catat URL & Key!)
☐ Tabel database sudah dibuat sesuai kebutuhan aplikasi
☐ Kode HTML sudah ditambahkan koneksi Supabase
☐ Website lokal sudah dicek — data berhasil masuk ke Supabase
☐ Environment Variable SUPABASE_URL sudah ditambah di Vercel
☐ Environment Variable SUPABASE_ANON_KEY sudah ditambah di Vercel  
☐ Re-deploy sudah dilakukan setelah tambah env var
☐ Website Vercel sudah dicek — data berhasil masuk ke Supabase
☐ Supabase Table Editor menampilkan data yang dikirim dari website
☐ Row Level Security sudah dikonfigurasi (opsional tapi disarankan)
```

---

## 💬 Tips Berkomunikasi dengan AI

> [!TIP]
> **Gunakan tips ini agar AI bisa membantu kamu dengan lebih baik:**

### ✅ Lakukan ini:
- **Paste pesan error persis sama** — jangan ditulis ulang, langsung copy-paste
- **Sebutkan sistem operasi** — Windows/Mac berbeda caranya
- **Bilang kalau tidak mengerti** — minta AI jelaskan ulang dengan kata berbeda
- **Tanya satu langkah dulu** — jangan terburu-buru ke langkah berikutnya

### ❌ Hindari ini:
- Jangan bilang "tidak berhasil" tanpa menjelaskan apa yang terjadi
- Jangan skip langkah meski terlihat tidak penting
- Jangan tutup Command Prompt di tengah proses

### 💡 Kalimat Ajaib untuk Meminta Penjelasan Ulang:
```
Saya masih bingung dengan langkah [NOMOR]. 
Bisakah kamu jelaskan ulang dengan analogi sehari-hari 
yang lebih mudah dipahami? 
Dan tunjukkan contoh konkretnya.
```

---

## 📊 Ringkasan Semua Prompt

### Fase A–D: Deploy ke Vercel

| Kode | Nama Prompt | Kapan Pakai | Wajib? |
|------|-------------|-------------|--------|
| A-1 | Cek Kesiapan Laptop | Di awal sesi | 🟢 Ya |
| A-2 | Siapkan Folder Proyek | Sebelum upload | 🟢 Ya |
| A-3 | Cek di Browser Lokal | Sebelum upload | 🟡 Disarankan |
| B-1 | Buat Akun & Repo GitHub | Jika belum punya | 🟢 Ya |
| B-2 | Setup Git Pertama Kali | Pertama kali pakai Git | 🟢 Ya |
| B-3 | Upload Kode ke GitHub | Wajib di setiap proyek | 🟢 Ya |
| B-🚨 | Darurat Password Push | Jika error password | 🔴 Jika perlu |
| C-1 | Setup Akun Vercel | Pertama kali pakai Vercel | 🟢 Ya |
| C-2 | Import & Deploy | Inti dari deploy | 🟢 Ya |
| C-3 | Custom Nama Link | Setelah berhasil | 🟡 Bonus |
| D-1 | Update & Re-Deploy | Setiap ada perubahan | 🟢 Ya |
| D-2 | Lihat Riwayat Deploy | Jika perlu rollback | 🟡 Opsional |
| 🆘-1 | Panik: Error Penuh | Darurat | 🔴 Jika perlu |
| 🆘-2 | Panik: Website Kosong | Darurat | 🔴 Jika perlu |
| 🆘-3 | Panik: Lupa Posisi | Darurat | 🔴 Jika perlu |

### Fase E: Supabase (Database)

| Kode | Nama Prompt | Kapan Pakai | Wajib? |
|------|-------------|-------------|--------|
| E-1 | Buat Akun Supabase | Pertama kali pakai Supabase | 🟢 Ya |
| E-2 | Buat Tabel Database | Setelah akun siap | 🟢 Ya |
| E-3 | Hubungkan Kode ke Supabase | Setelah tabel siap | 🟢 Ya |
| E-4 | Pasang Env Variable di Vercel | Sebelum deploy ulang | 🟢 **Kritis!** |
| E-5 | Cek Koneksi Supabase | Setelah deploy ulang | 🟡 Disarankan |
| E-🚨1 | Darurat: Data Tidak Masuk | Jika data hilang | 🔴 Jika perlu |
| E-🚨2 | Darurat: Env Var Tidak Terbaca | Jika koneksi gagal di Vercel | 🔴 Jika perlu |
| E-6 | Atur Keamanan RLS | Setelah semua berjalan | 🟡 Disarankan |

---

*Materi Praktikum Vibe Coding — Sesi 3: Deploy ke Vercel + Supabase | September 2026*
*"Tidak perlu tahu caranya, cukup tahu apa yang mau ditanya." 💡*

---

> [!TIP]
> **Urutan yang disarankan untuk pemula yang baru pertama kali:**
> A-1 → A-2 → A-3 → B-1 → B-2 → B-3 → C-1 → C-2 → D-1 → E-1 → E-2 → E-3 → E-4 → E-5
