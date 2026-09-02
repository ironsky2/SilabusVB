# Modul Vibe Coding untuk Pemula Non-IT
## Topik: Integrasi API Eksternal

---

## Tentang Modul Ini

Modul ini dirancang untuk peserta **tanpa latar belakang IT** yang ingin belajar membuat aplikasi sederhana dengan bantuan AI (pendekatan *vibe coding* — kamu menjelaskan apa yang kamu mau dengan bahasa manusia, AI yang menuliskan kodenya). Fokus modul ini adalah memahami **apa itu API eksternal** dan **bagaimana menyambungkannya ke aplikasi** menggunakan prompt yang tepat ke AI coding assistant (misalnya Claude, ChatGPT, Cursor, atau Claude Code).

**Durasi:** ± 2-3 jam (1 jam teori, 1-2 jam praktikum)

**Prasyarat:**
- Laptop dengan koneksi internet
- Akun di salah satu AI coding assistant (Claude.ai, ChatGPT, atau sejenisnya)
- Tidak perlu bisa coding sebelumnya

**Tujuan Pembelajaran**

Setelah menyelesaikan modul ini, peserta mampu:

1. Menjelaskan apa itu API dan mengapa aplikasi butuh "menyambung" ke API eksternal.
2. Mengenali istilah dasar: endpoint, request, response, API key, JSON.
3. Menulis prompt yang jelas dan terstruktur untuk meminta AI membangun integrasi API.
4. Membuat aplikasi sederhana yang mengambil data dari API publik dan menampilkannya.
5. Melakukan debugging dasar ketika integrasi API tidak berjalan, dengan bantuan AI.

---

## BAGIAN 1 — TEORI

### 1.1 Apa itu API?

API adalah singkatan dari **Application Programming Interface**. Bayangkan API seperti **pelayan restoran**:

- Kamu (aplikasi) tidak masuk ke dapur (server) untuk mengambil makanan sendiri.
- Kamu memberi pesanan ke pelayan (API) dengan format tertentu (menu).
- Pelayan membawa pesanan ke dapur, dapur memasak, lalu pelayan membawakan makanan (data) kembali ke kamu.

Jadi, **API eksternal** adalah "pelayan" milik pihak lain (bukan aplikasi kita sendiri) yang mengizinkan aplikasi kita meminta data atau layanan dari sistem mereka. Contoh: aplikasi cuaca di HP kamu tidak punya satelit sendiri — ia meminta data cuaca dari API milik BMKG atau OpenWeatherMap.

Contoh API eksternal yang sering dipakai:

| API | Fungsi |
|---|---|
| OpenWeatherMap | Data cuaca |
| Google Maps API | Peta dan lokasi |
| Payment Gateway (Midtrans, Xendit) | Pembayaran online |
| WhatsApp Business API | Kirim pesan otomatis |
| Exchange Rate API | Kurs mata uang |
| Open Trivia DB | Soal kuis gratis |

### 1.2 Istilah Dasar yang Wajib Dipahami

- **Endpoint** — alamat URL spesifik tempat kita "memesan" data. Contoh: `https://api.exchangerate-api.com/v4/latest/USD`
- **Request** — permintaan yang dikirim aplikasi kita ke API (misalnya "tolong berikan data cuaca Jakarta hari ini").
- **Response** — jawaban yang dikirim balik oleh API, biasanya dalam format **JSON**.
- **JSON (JavaScript Object Notation)** — format data berupa pasangan `"kunci": "nilai"`, mudah dibaca manusia maupun komputer.
- **API Key** — semacam "kata sandi" yang membuktikan aplikasi kita berhak mengakses API tersebut. Biasanya didaftarkan gratis di situs penyedia API.
- **HTTP Method** — cara permintaan dikirim. Yang paling umum:
  - `GET` — mengambil data (misalnya lihat data cuaca)
  - `POST` — mengirim data baru (misalnya membuat pesanan)
- **Status Code** — kode angka yang menunjukkan hasil request. Contoh: `200` (berhasil), `401` (API key salah), `404` (endpoint tidak ditemukan), `500` (server API bermasalah).
- **Rate Limit** — batas jumlah request yang boleh dikirim dalam periode waktu tertentu (misalnya 60 request/menit).

### 1.3 Contoh Sederhana Response JSON

```json
{
  "kota": "Jakarta",
  "suhu": 31,
  "kondisi": "Cerah Berawan"
}
```

Cara membacanya: "kota" adalah kunci, "Jakarta" adalah nilainya. Sesederhana itu.

### 1.4 Kenapa Non-IT Perlu Paham Konsep Ini (Meski Tak Menulis Kode)?

Dalam *vibe coding*, kamu tidak menulis kode baris demi baris — AI yang menuliskannya. Tapi kamu tetap perlu:

1. **Tahu apa yang harus diminta** (API mana, data apa yang dibutuhkan).
2. **Bisa membaca dokumentasi API secara garis besar** untuk memberi info ke AI (endpoint, cara dapat API key).
3. **Bisa mengenali error** dan menjelaskannya ke AI supaya AI bisa membantu memperbaiki.

Tanpa pemahaman dasar ini, kamu hanya bisa "menebak-nebak" prompt tanpa tahu kenapa hasilnya gagal.

### 1.5 Anatomi Prompt yang Baik untuk Integrasi API

Prompt yang efektif untuk meminta AI membuat integrasi API sebaiknya memuat 5 elemen:

1. **Konteks aplikasi** — aplikasi seperti apa yang sedang dibuat.
2. **Nama & endpoint API** — API mana yang ingin digunakan (sertakan link dokumentasi jika ada).
3. **Data yang ingin ditampilkan** — field apa saja yang dibutuhkan dari response.
4. **Cara menampilkan hasil** — di halaman web, tabel, kartu, dsb.
5. **Batasan teknis** — bahasa/tools yang dipakai (misalnya HTML+JavaScript sederhana), dan permintaan agar AI menjelaskan tiap bagian kode dengan bahasa awam.

Contoh kerangka prompt:

```
Saya sedang membuat [jenis aplikasi], menggunakan [bahasa/tools].
Saya ingin mengintegrasikan API [nama API] dengan endpoint [URL/dokumentasi].
Saya ingin menampilkan data berikut: [daftar field].
Tampilkan hasilnya dalam bentuk [format tampilan].
Tolong buatkan kodenya lengkap dengan komentar penjelasan yang mudah dipahami
oleh saya yang bukan programmer, dan jelaskan langkah menjalankannya.
```

---

## BAGIAN 2 — PRAKTIKUM

### Studi Kasus: Membuat Halaman Web "Kurs Mata Uang Hari Ini"

Kita akan membuat halaman web sederhana yang mengambil data kurs mata uang dari API gratis **ExchangeRate-API** (tidak wajib API key untuk versi dasar) dan menampilkannya.

### Langkah 0 — Persiapan

1. Buka situs [https://www.exchangerate-api.com](https://www.exchangerate-api.com) atau gunakan endpoint publik gratis: `https://open.er-api.com/v6/latest/USD`
2. Buka aplikasi AI coding assistant pilihanmu (Claude.ai, ChatGPT, dsb).
3. Siapkan folder kosong di komputer untuk menyimpan file hasil (misalnya folder bernama `kurs-mata-uang`).

### Langkah 1 — Prompt Awal: Meminta AI Membuatkan Kerangka Aplikasi

Salin dan kirim prompt berikut ke AI:

```
Saya pemula, belum pernah coding sama sekali. Saya ingin membuat halaman
web sederhana bernama "Kurs Mata Uang Hari Ini" menggunakan HTML,
CSS, dan JavaScript polos (tanpa framework), dalam satu file HTML saja.

Halaman ini harus:
1. Mengambil data kurs mata uang secara real-time dari API gratis ini:
   https://open.er-api.com/v6/latest/USD
   (tidak perlu API key)
2. Menampilkan kurs USD terhadap IDR, EUR, JPY, dan SGD dalam bentuk
   kartu-kartu yang rapi.
3. Ada tombol "Refresh" untuk mengambil data terbaru.
4. Menampilkan pesan yang ramah jika terjadi error (misalnya koneksi
   internet bermasalah).

Tolong:
- Tulis kode lengkap dalam satu file HTML.
- Beri komentar di setiap bagian kode dengan bahasa yang mudah dipahami
  orang non-programmer.
- Jelaskan langkah demi langkah cara menyimpan dan membuka file ini
  di browser.
```

**Yang perlu diperhatikan peserta:** AI akan memberi kode HTML lengkap. Simpan sebagai `index.html`, lalu buka dengan double-click atau drag ke browser.

### Langkah 2 — Menguji Hasilnya

1. Buka file `index.html` di browser.
2. Amati apakah data kurs muncul.
3. Klik tombol refresh, lihat apakah data berubah/update.

### Langkah 3 — Prompt Lanjutan: Menambahkan Fitur

Setelah versi dasar berhasil, coba prompt lanjutan berikut untuk melatih kemampuan iterasi:

```
Aplikasi kurs mata uang saya sudah berjalan. Sekarang saya ingin
menambahkan fitur:
1. Dropdown untuk memilih mata uang asal (bukan hanya USD), pilihannya:
   USD, IDR, EUR, JPY.
2. Saat mata uang asal diganti, data kurs otomatis diambil ulang dari
   API dengan endpoint: https://open.er-api.com/v6/latest/{KODE_MATA_UANG}
3. Tampilkan waktu terakhir data di-update (ambil dari field
   "time_last_update_utc" pada response API).

Tolong ubah kode saya (saya lampirkan di bawah) dan jelaskan bagian mana
saja yang berubah.

[TEMPEL KODE index.html KAMU DI SINI]
```

### Langkah 4 — Prompt Debugging (Simulasi Error)

Bagian penting dari *vibe coding* adalah tahu cara meminta bantuan saat terjadi error. Latih peserta dengan skenario berikut:

**Skenario:** Data tidak muncul, hanya tulisan "Loading..." yang tidak hilang.

Prompt debugging yang baik:

```
Aplikasi kurs mata uang saya tidak menampilkan data, hanya tulisan
"Loading..." yang tidak pernah berubah.

Yang sudah saya coba:
- Saya sudah cek koneksi internet, normal.
- Saya buka file dengan cara double-click dari File Explorer.

Ini pesan error yang muncul di console browser (saya buka lewat
klik kanan > Inspect > tab Console):
[TEMPEL PESAN ERROR DI SINI]

Ini kode saya saat ini:
[TEMPEL KODE index.html KAMU DI SINI]

Tolong jelaskan kemungkinan penyebabnya dan perbaiki kodenya.
```

> **Catatan Pengajar:** Ajarkan peserta cara membuka Console browser (klik kanan pada halaman → Inspect → tab Console) karena ini adalah sumber informasi error paling penting saat integrasi API gagal — misalnya error CORS, 404, atau format URL salah.

### Langkah 5 — Prompt untuk API yang Butuh API Key

Sebagai perbandingan, ajak peserta mencoba API yang **mewajibkan API key** (misalnya OpenWeatherMap), agar memahami alur pendaftaran dan keamanan API key.

```
Saya sudah mendaftar akun gratis di OpenWeatherMap dan mendapatkan
API key: [API_KEY_SAYA]

Saya ingin membuat halaman web sederhana yang menampilkan cuaca hari ini
untuk kota Jakarta, menggunakan endpoint:
https://api.openweathermap.org/data/2.5/weather?q=Jakarta&appid=API_KEY&units=metric

Tolong:
1. Buatkan kode HTML+JavaScript lengkap.
2. PENTING: jelaskan ke saya cara terbaik menyimpan API key ini supaya
   tidak sembarangan terlihat publik jika saya upload kode ini ke internet
   (misalnya GitHub).
3. Tampilkan nama kota, suhu, dan kondisi cuaca dalam bentuk kartu.
```

> **Poin edukasi keamanan:** Tekankan bahwa API key sebaiknya tidak ditaruh langsung di kode yang diunggah publik. Untuk pemula, cukup jelaskan konsepnya (API key = kunci rumah, jangan sebar sembarangan); solusi teknis seperti environment variable bisa disampaikan sebagai wawasan lanjutan.

---

## BAGIAN 3 — LATIHAN MANDIRI

Pilih salah satu tantangan berikut dan kerjakan menggunakan pendekatan prompt di atas:

1. **Kutipan Motivasi Harian** — gunakan API `https://api.quotable.io/random` untuk menampilkan kutipan acak dengan tombol "Kutipan Baru".
2. **Fakta Acak** — gunakan API `https://uselessfacts.jsph.pl/api/v2/facts/random` untuk menampilkan fakta unik.
3. **Cek Nama Negara** — gunakan API `https://restcountries.com/v3.1/name/{nama_negara}` untuk menampilkan info dasar suatu negara (bendera, ibu kota, populasi) saat pengguna mengetik nama negara di kolom pencarian.

**Instruksi latihan:**

1. Tulis prompt awal sendiri (gunakan kerangka di bagian 1.5).
2. Jalankan hasilnya, catat apakah berhasil di percobaan pertama.
3. Jika gagal, tulis prompt debugging sendiri.
4. Simpan hasil akhir dan bandingkan dengan teman.

---

## BAGIAN 4 — CHECKLIST EVALUASI PESERTA

Gunakan checklist ini untuk menilai pemahaman peserta setelah praktikum:

- [ ] Peserta dapat menjelaskan perbedaan request dan response dengan kata-kata sendiri.
- [ ] Peserta dapat menyebutkan fungsi API key.
- [ ] Peserta berhasil menjalankan minimal satu aplikasi yang mengambil data dari API eksternal.
- [ ] Peserta mampu menulis prompt integrasi API menggunakan kerangka 5 elemen (konteks, nama/endpoint, data, tampilan, batasan teknis).
- [ ] Peserta mampu membuka Console browser dan menyalin pesan error untuk prompt debugging.
- [ ] Peserta menyelesaikan minimal satu tantangan di Bagian 3 secara mandiri.

---

## Lampiran: Kumpulan Istilah (Glosarium Singkat)

| Istilah | Penjelasan Singkat |
|---|---|
| API | Penghubung yang memungkinkan aplikasi saling bertukar data |
| Endpoint | Alamat URL spesifik untuk mengakses data/layanan tertentu |
| Request | Permintaan yang dikirim ke API |
| Response | Jawaban yang dikembalikan oleh API |
| JSON | Format data berupa pasangan kunci-nilai |
| API Key | Kode identitas/izin akses ke suatu API |
| GET / POST | Jenis permintaan: ambil data / kirim data |
| Status Code | Kode angka penanda hasil request (200, 404, 500, dst) |
| Rate Limit | Batas jumlah request dalam periode waktu tertentu |
| CORS | Aturan keamanan browser terkait akses data lintas domain, sering jadi sumber error saat integrasi API |

---

*Modul ini dapat dikembangkan lebih lanjut dengan menambahkan sesi integrasi API berbayar, autentikasi OAuth, atau studi kasus API internal perusahaan sesuai kebutuhan kelas.*
