# OWASP Top 10 untuk Vibe Coder Pemula (Non-IT)

> Materi ini dibuat untuk kamu yang belajar coding dengan bantuan AI ("vibe coding") dan belum punya latar belakang IT. Tujuannya bukan membuatmu jadi expert security, tapi membuatmu **tahu apa yang harus diminta ke AI** supaya aplikasi yang kamu buat tidak gampang dibobol.

## Cara Pakai Materi Ini

Setiap bab punya 3 bagian:

1. **Teori** — penjelasan sederhana, tanpa jargon berlebihan.
2. **Kenapa ini bahaya buat vibe coder** — karena AI sering menulis kode yang *jalan*, tapi belum tentu *aman*, kecuali kamu memintanya secara eksplisit.
3. **Praktikum (Prompt)** — contoh prompt yang bisa kamu copy-paste ke Claude/ChatGPT/Cursor/dll, baik untuk membangun fitur dengan aman maupun untuk mengecek kode yang sudah ada.

Catatan: OWASP Top 10 di bawah ini mengikuti versi resmi OWASP Top 10:2021 (versi terbaru yang dipublikasikan OWASP). [Medium confidence] — OWASP kadang merilis update, jadi kalau kamu perlu kepastian versi terbaru sebelum audit resmi, cek langsung di owasp.org.

---

## A01: Broken Access Control (Kontrol Akses yang Rusak)

### Teori

Ini soal "siapa boleh lihat/ubah apa". Contoh gagal: user A bisa melihat data pesanan milik user B hanya dengan mengganti angka `id=101` menjadi `id=102` di URL. Aplikasi tidak mengecek "apakah data ini benar-benar milik yang sedang login?" — dia cuma mengecek "apakah ID ini ada di database?"

Ini konsisten menjadi kategori risiko nomor 1 di OWASP Top 10:2021 dan paling sering ditemukan di aplikasi nyata.

### Kenapa Bahaya Buat Vibe Coder

AI sangat sering membuat endpoint seperti `GET /api/order/:id` yang berfungsi sempurna secara logika, tapi lupa menambahkan pengecekan "apakah order ini milik user yang sedang login". Karena aplikasinya "kelihatan jalan" saat kamu tes sendiri (login sebagai satu user), bug ini baru ketahuan kalau ada orang lain yang iseng ganti angka di URL.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Buatkan endpoint untuk melihat detail pesanan berdasarkan order_id.
Syarat wajib:
1. Ambil user yang sedang login dari session/token, JANGAN dari parameter yang dikirim client.
2. Sebelum mengembalikan data, cek apakah order_id tersebut memang milik user_id yang login.
3. Kalau bukan miliknya, kembalikan error 403 Forbidden, bukan data orang lain.
4. Tulis juga 1 contoh test case yang mencoba mengakses order milik user lain untuk membuktikan proteksinya jalan.
```

**Prompt mengecek kode yang sudah ada:**
```
Ini kode endpoint saya: [paste kode].
Tolong cek khusus dari sisi Broken Access Control (OWASP A01):
- Apakah ada endpoint yang mengambil data berdasarkan ID dari user tanpa memverifikasi kepemilikan data tersebut?
- Apakah ada fitur admin yang bisa diakses user biasa kalau dia tahu URL-nya?
Jelaskan tiap temuan dengan bahasa sederhana, lalu kasih perbaikannya.
```

---

## A02: Cryptographic Failures (Kegagalan Kriptografi)

### Teori

Ini soal data sensitif (password, nomor kartu, data pribadi) yang seharusnya dilindungi tapi disimpan atau dikirim dalam bentuk "polos" (plaintext), atau dilindungi dengan metode enkripsi yang lemah/ketinggalan zaman.

### Kenapa Bahaya Buat Vibe Coder

Saat kamu minta AI "buatkan sistem login", AI kadang menyimpan password langsung ke database tanpa di-hash, atau pakai algoritma hash lama seperti MD5 yang mudah dibobol. Ini tidak kelihatan errornya sama sekali sampai database-nya bocor.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Buatkan sistem registrasi dan login user.
Syarat wajib:
1. JANGAN simpan password dalam bentuk plaintext.
2. Gunakan algoritma hashing yang direkomendasikan untuk password saat ini (misalnya bcrypt atau argon2), bukan MD5/SHA1 biasa.
3. Data sensitif lain (contoh: token reset password) harus di-generate secara acak dan aman, bukan angka berurutan.
4. Jangan pernah kirim password asli lewat email atau log aplikasi.
Jelaskan juga di komentar kode, kenapa pilihan algoritma itu yang dipakai.
```

**Prompt mengecek kode yang sudah ada:**
```
Cek kode ini dari sisi Cryptographic Failures (OWASP A02): [paste kode]
- Apakah ada password atau data sensitif yang disimpan tanpa enkripsi/hashing yang layak?
- Apakah ada data sensitif yang dikirim lewat HTTP biasa (bukan HTTPS) atau muncul di log?
Kasih daftar temuan dan cara perbaikannya, urutkan dari yang paling berbahaya.
```

---

## A03: Injection

### Teori

Injection terjadi kalau input dari user "dipercaya begitu saja" dan digabung langsung ke perintah (misalnya query database). Contoh paling terkenal: **SQL Injection** — user mengetik `' OR '1'='1` di form login dan berhasil masuk tanpa password yang benar.

### Kenapa Bahaya Buat Vibe Coder

AI kadang menulis query database dengan cara menggabungkan string secara langsung (string concatenation) karena itu cara paling "cepat kelihatan jalan". Kalau kamu tidak eksplisit minta pakai parameterized query, ini rawan sekali.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Buatkan fitur pencarian produk berdasarkan nama yang diketik user.
Syarat wajib:
1. Gunakan parameterized query / prepared statement untuk semua query ke database, JANGAN gabungkan input user langsung ke string SQL.
2. Validasi juga jenis dan panjang input sebelum diproses.
3. Tunjukkan versi kode yang SALAH (rawan injection) dan versi yang BENAR, supaya saya paham bedanya.
```

**Prompt mengecek kode yang sudah ada:**
```
Cek kode ini khusus untuk kerentanan Injection (OWASP A03): [paste kode]
- Cari semua tempat yang menggabungkan input user langsung ke query database, command shell, atau HTML tanpa sanitasi.
- Untuk tiap temuan, kasih contoh input jahat yang bisa dipakai menyerangnya, dan perbaikannya.
```

---

## A04: Insecure Design (Desain yang Tidak Aman)

### Teori

Ini beda dari bug biasa — ini soal fitur yang *dari awal dirancang* tanpa mempertimbangkan risiko keamanan. Contoh: fitur "lupa password" yang tidak membatasi berapa kali orang boleh mencoba menebak kode OTP.

### Kenapa Bahaya Buat Vibe Coder

Karena vibe coding sering fokus ke "apakah fiturnya jalan", bukan "apa yang terjadi kalau orang jahat memakai fitur ini dengan cara yang tidak wajar". AI tidak akan otomatis memikirkan skenario penyalahgunaan kecuali kamu memintanya.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Saya mau membuat fitur "reset password lewat OTP 6 digit yang dikirim ke email".
Sebelum menulis kode, tolong analisa dulu:
1. Apa saja skenario penyalahgunaan yang mungkin (contoh: orang mencoba semua kombinasi OTP)?
2. Bagaimana desain yang aman untuk mencegah tiap skenario itu (rate limiting, masa berlaku OTP, jumlah percobaan maksimal, dll)?
Baru setelah itu, tuliskan kodenya berdasarkan desain aman tersebut.
```

**Prompt mengecek kode yang sudah ada:**
```
Ini alur fitur [nama fitur] di aplikasi saya: [jelaskan alurnya / paste kode].
Sebagai reviewer keamanan, coba pikirkan seperti penyerang: skenario penyalahgunaan apa saja yang bisa terjadi pada alur ini yang tidak dicegah oleh desainnya? Kasih rekomendasi perbaikan desain, bukan cuma tambal kode.
```

---

## A05: Security Misconfiguration (Konfigurasi yang Salah)

### Teori

Aplikasi jadi rentan bukan karena kodenya salah, tapi karena *pengaturannya* longgar: mode debug masih menyala di production, pesan error menampilkan detail teknis lengkap ke user, folder admin bisa diakses tanpa login, dll.

### Kenapa Bahaya Buat Vibe Coder

Banyak template/boilerplate hasil AI defaultnya dikonfigurasi untuk kemudahan development (misalnya `DEBUG=True`, CORS mengizinkan semua domain `*`), dan pengaturan itu sering lupa diganti sebelum aplikasi dipakai publik (deploy ke production).

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Saya mau deploy aplikasi [nama framework, misal: Express/Flask/Next.js] ke production.
Buatkan checklist konfigurasi keamanan sebelum go-live, mencakup minimal:
1. Mode debug/error detail harus mati di production.
2. CORS hanya mengizinkan domain yang saya tentukan, bukan '*'.
3. Header keamanan HTTP standar (misalnya Content-Security-Policy, X-Frame-Options) sudah diset.
4. Tidak ada endpoint/dashboard admin default yang masih terbuka tanpa autentikasi.
Setelah itu, cek file konfigurasi saya ini terhadap checklist itu: [paste config]
```

**Prompt mengecek kode yang sudah ada:**
```
Cek project ini dari sisi Security Misconfiguration (OWASP A05): [paste file config/env/settings]
Cari pengaturan yang aman untuk development tapi berbahaya kalau dipakai di production, dan jelaskan kenapa masing-masing berbahaya.
```

---

## A06: Vulnerable and Outdated Components (Komponen Rentan/Usang)

### Teori

Aplikasi modern dibangun dari banyak library pihak ketiga (npm package, pip package, dll). Kalau salah satu library itu punya celah keamanan yang sudah diketahui publik (CVE) dan tidak diupdate, penyerang tinggal cari aplikasi mana saja yang masih memakainya.

### Kenapa Bahaya Buat Vibe Coder

Vibe coder biasanya `npm install` atau `pip install` apa saja yang disarankan AI tanpa mengecek reputasi atau versi library tersebut, dan jarang melakukan update rutin setelah aplikasi jadi.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Saya mau menambahkan fitur upload gambar dan resize otomatis.
Sebelum sarankan library, tolong:
1. Sarankan library yang masih aktif di-maintain dan populer (bukan yang sudah lama tidak diupdate).
2. Jelaskan cara saya mengecek riwayat kerentanan/vulnerability library tersebut sebelum saya pakai.
```

**Prompt mengecek kode yang sudah ada:**
```
Ini isi file dependency saya (package.json / requirements.txt): [paste isi file]
Tolong:
1. Tunjukkan cara saya menjalankan audit keamanan otomatis untuk dependency ini (contoh perintah command line yang relevan untuk stack saya).
2. Jelaskan langkah apa yang harus saya lakukan kalau audit menemukan kerentanan (upgrade versi, cari alternatif, dll).
```

---

## A07: Identification and Authentication Failures (Kegagalan Autentikasi)

### Teori

Ini soal proses login, session, dan identitas user yang lemah: tidak ada batas percobaan login (brute force bebas), session tidak pernah expired, tidak ada opsi multi-factor authentication (MFA), atau session ID mudah ditebak.

### Kenapa Bahaya Buat Vibe Coder

Sistem login "dasar" yang dibuat AI biasanya fokus ke fungsi inti (bisa login/logout) dan sering melewatkan hal-hal seperti rate limiting percobaan login atau kebijakan session yang aman, kecuali diminta.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Buatkan sistem login yang aman.
Syarat wajib:
1. Batasi jumlah percobaan login yang gagal (contoh: maksimal 5x dalam 15 menit, lalu di-lock sementara).
2. Session/token punya masa berlaku (expiry), tidak berlaku selamanya.
3. Session di-invalidate saat user logout atau ganti password.
4. Jangan bocorkan informasi "email tidak terdaftar" vs "password salah" secara spesifik di pesan error (gunakan pesan generik) untuk mencegah user enumeration.
```

**Prompt mengecek kode yang sudah ada:**
```
Cek alur login dan session di kode ini dari sisi Authentication Failures (OWASP A07): [paste kode]
- Apakah ada rate limiting untuk percobaan login?
- Apakah session bisa expired?
- Apakah pesan error login membocorkan informasi yang seharusnya dirahasiakan?
```

---

## A08: Software and Data Integrity Failures (Kegagalan Integritas)

### Teori

Ini soal mempercayai sesuatu (kode, update, data) tanpa memverifikasi keasliannya. Contoh: aplikasi otomatis menjalankan update dari sumber luar tanpa mengecek "apakah file ini benar-benar dari sumber resmi dan tidak diubah orang lain di tengah jalan (man-in-the-middle)".

### Kenapa Bahaya Buat Vibe Coder

Ini kategori yang lebih jarang terasa buat pemula, tapi relevan kalau kamu memakai fitur seperti auto-update, CI/CD pipeline, atau menerima file/plugin dari pihak ketiga di aplikasimu.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Saya mau fitur di aplikasi saya yang mengunduh file konfigurasi/plugin dari server eksternal secara otomatis.
Sebelum menulis kode, jelaskan bagaimana cara memverifikasi integritas file tersebut (misalnya checksum/signature) sebelum dieksekusi, lalu tuliskan kodenya.
```

**Prompt mengecek kode yang sudah ada:**
```
Ini pipeline/proses deployment saya: [jelaskan/paste config CI-CD]
Apakah ada tahap yang mempercayai kode/dependency dari luar tanpa verifikasi (signature, checksum, source terpercaya)? Kasih rekomendasi perbaikan.
```

---

## A09: Security Logging and Monitoring Failures (Kegagalan Logging & Monitoring)

### Teori

Kalau aplikasi diserang tapi tidak ada catatan (log) tentang siapa melakukan apa dan kapan, tim tidak akan pernah tahu ada serangan sampai kerugiannya besar. Ini soal "kalau terjadi insiden, apakah kita bisa mendeteksi dan menelusurinya?"

### Kenapa Bahaya Buat Vibe Coder

Vibe coder biasanya tidak memikirkan logging sama sekali di awal karena tidak terlihat sebagai "fitur". Padahal tanpa log yang layak, kamu baru sadar ada kebocoran data setelah semuanya sudah terjadi.

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Tambahkan logging keamanan dasar ke aplikasi saya.
Syarat wajib:
1. Catat setiap percobaan login (berhasil maupun gagal) beserta waktu dan IP (tanpa mencatat password).
2. Catat aktivitas sensitif seperti perubahan password, perubahan role/permission user.
3. JANGAN pernah mencatat data sensitif (password, token, nomor kartu) di dalam log.
4. Jelaskan juga bagaimana saya bisa memantau log ini untuk mendeteksi aktivitas mencurigakan (misalnya banyak percobaan login gagal dari satu IP).
```

**Prompt mengecek kode yang sudah ada:**
```
Cek kode ini dari sisi Logging & Monitoring (OWASP A09): [paste kode]
- Aktivitas sensitif apa saja yang tidak tercatat sama sekali?
- Apakah ada data sensitif yang malah ikut tercatat di log (ini juga masalah)?
```

---

## A10: Server-Side Request Forgery / SSRF

### Teori

SSRF terjadi kalau aplikasi bisa "disuruh" mengambil/request ke URL yang dikontrol penyerang dari sisi server. Contoh: fitur "masukkan URL gambar profil dari internet", lalu penyerang memasukkan URL yang mengarah ke sistem internal perusahaan (misalnya `http://localhost:internal-admin-panel`), dan server tanpa sadar mengaksesnya seolah dari dalam jaringan sendiri.

### Kenapa Bahaya Buat Vibe Coder

Fitur seperti "import dari URL", "preview link", atau "webhook" sangat umum diminta ke AI, dan AI sering membuatnya tanpa validasi/whitelist domain tujuan, karena secara fungsi memang "jalan".

### Praktikum (Prompt)

**Prompt membangun fitur dengan aman:**
```
Buatkan fitur "ambil gambar profil dari URL yang dimasukkan user".
Syarat wajib:
1. Buat whitelist domain/protokol yang diizinkan (misalnya hanya https, dan bukan alamat internal seperti localhost, 127.0.0.1, atau IP privat 10.x/172.16.x/192.168.x).
2. Validasi ini dilakukan di server, bukan cuma di frontend.
3. Jelaskan kenapa validasi ini penting (kasih contoh skenario serangan SSRF singkat).
```

**Prompt mengecek kode yang sudah ada:**
```
Cek kode ini dari sisi SSRF (OWASP A10): [paste kode]
Cari semua fitur yang membuat server melakukan request ke URL yang berasal dari input user (fetch URL, webhook, import link, dll), lalu cek apakah ada validasi/whitelist tujuannya. Kasih rekomendasi perbaikan.
```

---

## Latihan Akhir: "AI Security Reviewer" Universal Prompt

Setelah paham 10 kategori di atas, kamu bisa pakai satu prompt "sapu jagat" ini setiap kali selesai membuat fitur baru dengan AI:

```
Kamu berperan sebagai security reviewer berpengalaman. Review kode berikut ini terhadap OWASP Top 10:2021, satu per satu kategori (A01 sampai A10):

[paste seluruh kode/fitur]

Untuk setiap kategori:
- Kalau tidak relevan/tidak ditemukan masalah, tulis singkat "tidak relevan/aman" beserta alasannya.
- Kalau ditemukan masalah, jelaskan: apa masalahnya, bagaimana cara penyerang bisa mengeksploitasinya (dengan bahasa sederhana), dan bagaimana cara memperbaikinya.

Di akhir, buatkan ringkasan prioritas: mana yang harus saya perbaiki DULUAN karena paling berbahaya.
```

### Tips Praktis Buat Vibe Coder

- Jangan cuma minta AI "buatkan fitur X" — selalu tambahkan kalimat keamanan spesifik seperti contoh di atas (validasi kepemilikan data, parameterized query, hashing, rate limiting, dll), karena AI akan menulis jalan pintas yang "kelihatan berfungsi" kalau tidak diminta secara eksplisit.
- Selalu lakukan review keamanan sebelum aplikasi dipakai orang lain (production/publik), bukan cuma sebelum di-deploy sekali di awal.
- Kalau aplikasimu menyimpan data pribadi orang lain (nama, email, alamat, dsb), anggap standar keamanannya harus lebih tinggi — pertimbangkan minta bantuan orang dengan latar belakang security untuk review sebelum go-live, terutama untuk aplikasi yang menangani data finansial atau kesehatan.
- Materi ini adalah titik awal, bukan pengganti audit keamanan profesional untuk aplikasi yang menangani data sensitif atau bernilai tinggi.
