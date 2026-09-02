# MODUL PEMBELAJARAN VIBE CODING
## Sesi: Integrasi API Berbayar

| | |
|---|---|
| **Sasaran peserta** | Pemula non-IT (belum pernah menulis kode) |
| **Durasi** | 2 JP (90 menit) |
| **Tools** | Google Antigravity (IDE berbasis agen AI) |
| **Studi kasus** | Aplikasi "Asisten Aduan" — meringkas & mengklasifikasi teks aduan masyarakat |
| **Model biaya di kelas** | Free tier / kredit percobaan |
| **Prasyarat** | Sesi dasar vibe coding sudah dilalui (peserta pernah membuat 1 halaman web sederhana) |

---

## PERINGATAN UNTUK PENGAJAR — BACA SEBELUM APA PUN

Tiga hal berikut menentukan sesi ini berhasil atau gagal total. Jangan diperhalus saat perencanaan.

**1. 90 menit TIDAK cukup jika instalasi dilakukan di kelas.**
Instalasi Antigravity + login Google + pembuatan API key bisa menghabiskan 30–45 menit sendiri untuk peserta non-IT, terutama di jaringan kantor yang lambat atau diblokir proxy. Modul ini mengasumsikan **instalasi selesai H-1**. Jika Anda tidak bisa memastikan itu, ubah sesi menjadi 3–4 JP atau turunkan target ke "peserta melihat demo + mengisi lembar kalkulasi biaya" saja.

**2. Ada kontradiksi bawaan: mengajar "API berbayar" dengan free tier.**
Peserta tidak akan pernah melihat tagihan, dan itu justru bagian paling penting dari materi ini. Modul ini menutupinya dengan tiga cara — jangan salah satu pun dilewati:
- Pengajar **mendemokan dashboard tagihan asli** (akun berbayar pengajar) selama 3 menit. Jika tidak punya, siapkan tangkapan layar.
- Peserta **sengaja menabrak rate limit free tier** (error 429) sebagai pengganti pengalaman "kuota habis".
- Peserta **menghitung biaya di atas kertas** untuk skenario 1.000 pengguna. Ini yang membuat angka terasa nyata.

**3. Angka harga dan batas kuota berubah cepat.**
Semua harga dan limit di modul ini **wajib diverifikasi ulang maksimal H-3** dari halaman resmi. Lihat Lampiran G. Jangan mengajarkan angka dari modul ini apa adanya — ajarkan **cara membacanya**.

---

## A. TUJUAN PEMBELAJARAN

Setelah sesi ini, peserta mampu:

1. **Menjelaskan** lima komponen yang selalu ada pada setiap API berbayar (endpoint, kunci, permintaan-jawaban, kuota, tagihan) — tanpa istilah teknis.
2. **Membedakan** dua lapis biaya dalam vibe coding: biaya alat bantu ngoding vs biaya API di dalam aplikasi yang dibuat.
3. **Memperoleh** API key sendiri dan menyimpannya secara aman (tidak di dalam kode yang bisa dilihat publik).
4. **Menyusun prompt** yang membuat agen AI mengintegrasikan API berbayar lengkap dengan penanganan error dan pengaman biaya.
5. **Menghitung** perkiraan biaya operasional aplikasi dan memutuskan kapan free tier tidak lagi memadai.

**Tujuan yang SENGAJA TIDAK dikejar** (agar ekspektasi jujur): peserta tidak akan bisa membaca dokumentasi API secara mandiri, tidak akan paham HTTP/REST secara utuh, dan tidak akan bisa men-deploy aplikasinya ke internet. Itu materi sesi lain.

---

## B. STRUKTUR WAKTU (90 MENIT)

| Menit | Bagian | Kegiatan | Bentuk |
|---|---|---|---|
| 0–5 | Pembuka | Cek kesiapan alat (angkat tangan), pemantik | Klasikal |
| 5–35 | **Teori** | Konsep 1–7 | Ceramah interaktif + demo |
| 35–80 | **Praktikum** | Tahap 0–5 | Praktik terbimbing |
| 80–90 | Penutup | Lembar kalkulasi biaya, refleksi, tugas mandiri | Klasikal |

> **Catatan alokasi — dibuka jujur, bukan disamarkan:**
> Enam tahap praktikum di modul ini kalau dijumlahkan butuh **50 menit**, sedangkan jatahnya **45 menit**. Selisih 5 menit itu disengaja sebagai *katup pelepas*:
> - Peserta ≤ 15 orang dan ada asisten → kemungkinan besar 6 tahap muat.
> - Peserta > 20 orang atau tanpa asisten → **potong Tahap 5** (pengaman biaya) dan jadikan tugas mandiri. Sisa 42 menit.
>
> Tahap yang **tidak boleh dipotong dalam kondisi apa pun**: Tahap 2 (panggilan API pertama) dan Tahap 4 (menabrak rate limit). Keduanya adalah inti materi "berbayar"; tanpa keduanya sesi ini berubah menjadi sesi membuat halaman web biasa.

---

# BAGIAN 1 — TEORI (30 MENIT)

## Konsep 1 — Pemantik: "Kenapa aplikasi buatanmu tiba-tiba butuh uang?" (3 menit)

Tanyakan ke kelas:

> *"Di sesi lalu kita membuat halaman web. Gratis, kan? Sekarang saya minta halaman itu bisa meringkas surat aduan secara otomatis. Menurut Anda, siapa yang mengerjakan peringkasannya?"*

Jawaban yang dituju: **bukan komputer peserta**. Peringkasan dikerjakan komputer milik perusahaan lain, di tempat lain, dan mereka menagih untuk itu.

Inilah garis pemisah materi hari ini:

- Kode yang **berjalan di komputer sendiri** → gratis selamanya.
- Kode yang **meminta jasa komputer orang lain** → ada yang membayar.

## Konsep 2 — Apa itu API, versi paling sederhana (4 menit)

**Analogi warung fotokopi:**

Anda punya naskah, tapi tidak punya mesin fotokopi. Anda ke warung fotokopi:

| Di warung fotokopi | Di API |
|---|---|
| Alamat warungnya | **Endpoint** — alamat internet layanan |
| Kartu member / nama Anda di nota | **API Key** — identitas & penagihan |
| Naskah yang Anda serahkan | **Request** — data yang dikirim |
| Hasil fotokopi yang diterima | **Response** — jawaban yang kembali |
| "Maaf, antre, maksimal 50 lembar per orang" | **Rate limit** — batas pemakaian |
| Rp 500/lembar, bayar di akhir | **Billing** — tagihan |

**API = cara satu aplikasi memesan jasa ke aplikasi lain.** Itu saja. Selebihnya detail.

> **Poin yang harus tertanam:** API key bukan sekadar password. API key adalah **kartu identitas yang menempel pada tagihan Anda**. Kalau bocor, yang tertagih tetap Anda.

## Konsep 3 — Empat model biaya API (5 menit)

Peserta harus bisa mengenali mana yang sedang mereka hadapi sebelum mendaftar layanan apa pun.

| Model | Cara kerja | Contoh yang sering ditemui | Risiko utama bagi pemula |
|---|---|---|---|
| **Free tier permanen** | Gratis sampai batas tertentu per menit/hari, lalu ditolak | API AI untuk pengembangan | Aplikasi mati mendadak saat ramai, bukan "melambat" |
| **Bayar sesuai pakai** (*pay-as-you-go*) | Ditagih per satuan pemakaian, tanpa batas atas otomatis | Mayoritas API AI, SMS, peta | **Tagihan membengkak tanpa disadari** |
| **Langganan berkuota** | Bayar bulanan tetap, dapat jatah tertentu | Banyak layanan SaaS | Bayar penuh walau tidak dipakai |
| **Freemium + verifikasi** | Gratis, tapi wajib kartu kredit di depan | Beberapa gateway pembayaran | Terlanjur langganan tanpa sadar |

**Aturan praktis untuk pemula:**
Selalu cari tiga hal ini di halaman *pricing* sebelum mendaftar:
1. Apakah ada **batas pengeluaran** (*spend limit / budget cap*) yang bisa saya set sendiri?
2. Apa yang terjadi kalau kuota gratis habis — **ditolak** atau **langsung ditagih**?
3. Bagaimana cara **menghapus/menonaktifkan** kunci saya?

Kalau ketiganya tidak jelas, jangan pakai.

## Konsep 4 — Satuan biaya: cara membaca halaman harga (5 menit)

API berbayar tidak menagih "per aplikasi". Mereka menagih **per satuan pemakaian**, dan satuannya berbeda-beda:

| Jenis API | Satuan tagihan |
|---|---|
| API AI / model bahasa | **Token** (potongan kata; kasarnya 1 token ≈ 4 karakter Latin) |
| API peta, cuaca, data | **Request** (sekali panggil = 1) |
| API SMS/WhatsApp | **Pesan terkirim** |
| API transkrip audio | **Menit audio** |
| API pembayaran | **Persentase transaksi** |

**Khusus API AI, ada dua harga berbeda dalam satu panggilan:**

- **Token masuk (input)** — teks yang Anda kirim. Lebih murah.
- **Token keluar (output)** — teks yang dijawab. Biasanya **5–10× lebih mahal**.

**Konsekuensi praktis yang harus diingat peserta:**
> Menyuruh AI menjawab panjang lebar itu mahal. Menyuruh AI membaca dokumen panjang itu relatif murah. Karena itu, membatasi panjang jawaban adalah penghematan terbesar dan termudah.

**Demo pengajar (3 menit):** buka halaman harga resmi penyedia API, tunjukkan langsung mana angka input dan mana output. Lalu buka **dashboard tagihan/pemakaian akun asli Anda**. Biarkan peserta melihat grafik pemakaian naik-turun. Ini momen paling berkesan di seluruh sesi — jangan dilewatkan.

## Konsep 5 — DUA LAPIS BIAYA dalam vibe coding (4 menit)

Ini konsep paling sering disalahpahami peserta non-IT, dan paling penting di sesi ini.

```
┌─────────────────────────────────────────────────────────┐
│  LAPIS 1 — Biaya ALAT NGODING                           │
│  Antigravity memakai kuota AI dari akun Google Anda      │
│  untuk MENULISKAN kode.                                  │
│  → Habis kuota = Anda tidak bisa lanjut ngoding.         │
│  → Aplikasi yang sudah jadi TETAP JALAN.                 │
└─────────────────────────────────────────────────────────┘
                          ⬇  menghasilkan
┌─────────────────────────────────────────────────────────┐
│  LAPIS 2 — Biaya API DI DALAM APLIKASI                  │
│  Aplikasi hasil buatan memakai API key MILIK ANDA        │
│  setiap kali PENGGUNA menekan tombol.                    │
│  → Habis kuota = APLIKASI ANDA yang mati.                │
│  → Ini biaya yang jalan terus, 24 jam, selamanya.        │
└─────────────────────────────────────────────────────────┘
```

Poin penting yang sering bikin peserta bingung:
**Kunci di Lapis 1 dan Lapis 2 adalah dua hal yang berbeda dan tidak bisa saling menggantikan.** Kuota Antigravity untuk menulis kode tidak bisa dipakai aplikasi Anda, dan API key aplikasi Anda tidak bisa dipakai untuk menjalankan Antigravity.

**Analogi:** Lapis 1 = ongkos menyewa tukang untuk membangun warung. Lapis 2 = tagihan listrik warung setelah buka. Tukangnya sekali bayar; listriknya seumur hidup warung.

> **[Perlu diverifikasi H-3]** Per September 2026, dokumentasi resmi Antigravity menyatakan tidak mendukung *bring-your-own-key* untuk menambah kuota agennya. Artinya kuota alat ngoding memang terpisah dari API key aplikasi. Cek ulang di halaman Plans resmi sebelum mengajarkan ini sebagai fakta.

## Konsep 6 — Lima dosa besar pemula dengan API key (5 menit)

Sampaikan dengan tegas. Setiap butir pernah menimbulkan kerugian uang nyata.

| # | Kesalahan | Akibat |
|---|---|---|
| 1 | Menulis kunci langsung di dalam file yang dibuka browser | Siapa pun yang membuka aplikasi Anda bisa menyalin kunci dan memakai kuota/uang Anda |
| 2 | Mengunggah kode ke GitHub tanpa `.gitignore` | Ada robot yang khusus memindai GitHub mencari API key. Hitungan menit, bukan hari |
| 3 | Membagi kunci di grup WhatsApp/Telegram "biar cepat" | Kunci menyebar tak terkendali, tidak bisa ditarik kembali |
| 4 | Menampilkan kunci di layar saat *screen sharing* atau tangkapan layar | Sama saja menyiarkannya |
| 5 | Tidak pernah menghapus kunci lama yang sudah tidak dipakai | Pintu belakang yang tidak dijaga siapa pun |

**Kebiasaan yang benar (ajarkan sebagai ritual, bukan teori):**
- Kunci selalu di file `.env`, tidak pernah di file kode.
- File `.env` selalu masuk `.gitignore`.
- Selesai latihan → **hapus kunci** dari dashboard penyedia.
- Aktifkan notifikasi/batas pengeluaran kalau tersedia.

> **Aturan kelas hari ini:** kunci yang dibuat di sesi ini **wajib dihapus sebelum peserta pulang**. Ini masuk checklist penutup (bagian 3.3).

## Konsep 7 — Kapan API berbayar TIDAK perlu (4 menit)

Peserta yang baru belajar cenderung ingin menempelkan AI ke segala hal. Latih mereka menahan diri.

**Tidak perlu API berbayar bila:**
- Tugasnya berpola tetap → cukup logika biasa. Contoh: menghitung total, memformat tanggal, mengurutkan daftar, mencari kata kunci.
- Datanya sedikit dan jarang berubah → cukup tabel/daftar di dalam kode.
- Hanya dipakai sendiri, sekali-sekali → cukup buka ChatGPT/Gemini langsung, tidak usah dibangun jadi aplikasi.

**Baru perlu API berbayar bila ketiganya terpenuhi:**
1. Tugasnya menuntut pemahaman bahasa/gambar yang tidak berpola tetap, **dan**
2. Harus berjalan otomatis tanpa manusia menyalin-tempel, **dan**
3. Frekuensinya cukup sering sehingga mengerjakan manual jadi tidak masuk akal.

**Latihan cepat 90 detik — minta peserta menjawab "perlu API AI atau tidak":**

| Kasus | Jawaban |
|---|---|
| Menghitung sisa cuti pegawai | Tidak — aritmetika biasa |
| Mengelompokkan 500 aduan masyarakat per topik setiap hari | Ya — bahasa bebas, volume besar, rutin |
| Mengubah format tanggal di 1.000 baris data | Tidak — pola tetap |
| Merangkum notulen rapat 2 jam menjadi 1 halaman, sekali sebulan | Tidak — cukup salin-tempel manual ke chatbot |

---

# BAGIAN 2 — PRAKTIKUM (45 MENIT)

## Studi Kasus: Aplikasi "Asisten Aduan"

**Masalah nyata:** Petugas menerima puluhan teks aduan masyarakat setiap hari dalam bahasa bebas. Membaca satu per satu dan mengelompokkannya memakan waktu.

**Yang akan dibangun:** Halaman web sederhana. Petugas menempelkan teks aduan → menekan satu tombol → mendapat tiga keluaran: **ringkasan satu kalimat**, **kategori**, dan **tingkat urgensi**.

**Kenapa kasus ini dipilih:** ketiga keluaran itu tidak bisa dikerjakan dengan rumus biasa (Konsep 7 terpenuhi), tapi cukup sederhana untuk selesai dalam 45 menit.

---

## Aturan Emas Prompt untuk Pemula: Rumus **K–T–B–H**

Ajarkan satu rumus ini saja. Empat bagian, selalu urut:

| Huruf | Isi | Kenapa penting |
|---|---|---|
| **K — Konteks** | Siapa Anda, aplikasi apa, kondisi saat ini | Agen tidak bisa menebak; tanpa ini ia mengarang asumsi |
| **T — Tugas** | Apa persisnya yang diminta, dalam poin-poin | Kalimat panjang bertele-tele menghasilkan kode bertele-tele |
| **B — Batasan** | Yang TIDAK boleh dilakukan | Ini yang mencegah agen bikin kerusakan dan memboroskan biaya |
| **H — Hasil** | Bentuk keluaran yang diinginkan + cara mengujinya | Tanpa ini, agen berhenti sebelum Anda bisa memakainya |

**Tiga kalimat wajib yang selalu ditempel di setiap prompt pemula:**

```
- Saya BUKAN programmer. Gunakan bahasa Indonesia sederhana.
- Jelaskan singkat apa yang akan kamu lakukan SEBELUM menulis kode.
- Jangan menjalankan perintah yang menghapus file.
```

---

## TAHAP 0 — Mengambil API Key (5 menit)

**Peserta lakukan:**

1. Buka halaman API key penyedia (untuk Gemini: `aistudio.google.com/apikey`) dengan akun Google yang sama seperti Antigravity.
2. Klik buat kunci baru → salin.
3. **Tempel sementara ke Notepad**, bukan ke chat, bukan ke grup.
4. Di halaman yang sama, cari dan **catat di lembar kerja**: batas pemakaian gratis yang berlaku untuk akun Anda hari ini.

> **Instruksi untuk pengajar:** Angka batas free tier tidak lagi dipublikasikan sebagai angka tetap di dokumentasi — Google mengarahkan pengguna melihat dashboard masing-masing. Justru manfaatkan ini: suruh peserta **membaca dashboard sendiri**. Keterampilan itu jauh lebih awet daripada menghafal angka.

**Kesalahan yang akan terjadi & tanganinya:**

| Gejala | Penanganan cepat |
|---|---|
| Halaman minta buat proyek Cloud dulu | Terima saja opsi proyek default yang ditawarkan |
| Peserta salin kunci tidak lengkap | Minta salin ulang dengan tombol salin, bukan blok manual |
| Akun kantor diblokir kebijakan organisasi | Siapkan **kunci cadangan pengajar** untuk 2–3 peserta; catat namanya, cabut setelah sesi |

---

## TAHAP 1 — Bangun tampilan dulu, TANPA API (10 menit)

**Prinsip yang diajarkan:** *Jangan pernah menyambung API sebelum tampilannya jadi.* Setiap kali menyambung API sambil membetulkan tampilan, Anda membayar untuk kesalahan yang tidak ada hubungannya dengan API.

**PROMPT 1 — salin-tempel ke Antigravity:**

```
Saya BUKAN programmer. Gunakan bahasa Indonesia sederhana.
Jelaskan singkat apa yang akan kamu lakukan sebelum menulis kode.
Jangan menjalankan perintah yang menghapus file.

KONTEKS:
Saya membuat aplikasi web satu halaman untuk membantu petugas
mengolah teks aduan masyarakat.

TUGAS:
Buat satu file bernama index.html berisi HTML, CSS, dan JavaScript:
1. Judul aplikasi: "Asisten Aduan"
2. Kotak teks besar untuk menempelkan isi aduan
3. Tombol "Proses"
4. Area hasil yang menampilkan tiga baris: Ringkasan, Kategori, Tingkat Urgensi

BATASAN:
- TAHAP INI JANGAN memanggil API apa pun. Isi hasilnya dengan data
  palsu yang kamu tulis langsung di dalam kode.
- Jangan pakai library atau framework dari internet.
- Beri komentar berbahasa Indonesia pada baris yang nanti akan
  diganti dengan panggilan API sungguhan.

HASIL YANG SAYA INGINKAN:
Satu file index.html yang bisa langsung saya buka di browser dan
sudah terlihat rapi. Di akhir, beri tahu cara membukanya.
```

**Checkpoint:** peserta melihat halaman berfungsi dengan hasil palsu. Belum ada biaya sama sekali. Angkat tangan yang sudah berhasil sebelum lanjut.

---

## TAHAP 2 — Sambungkan ke API sungguhan (12 menit)

### Jalur Utama (disarankan) — kunci disimpan di server kecil

**PROMPT 2:**

```
Saya BUKAN programmer. Gunakan bahasa Indonesia sederhana.
Jelaskan singkat apa yang akan kamu lakukan sebelum menulis kode.

KONTEKS:
Lanjutkan aplikasi index.html yang tadi. Sekarang saya ingin data
palsu diganti dengan panggilan ke API AI yang sesungguhnya.

BATASAN KEAMANAN (ini yang paling penting):
- API key TIDAK BOLEH ada di file yang dibuka browser.
- Simpan API key di file .env
- Buat file .gitignore yang mengabaikan .env
- Sebelum menulis kode, periksa dulu nama model yang MASIH AKTIF di
  dokumentasi resmi. Jangan mengarang nama model.

TUGAS:
1. Buat server kecil dengan Node.js yang:
   - menyajikan index.html
   - punya satu alamat POST /api/proses
   - alamat itu menerima teks dari halaman, memanggil API AI, lalu
     mengembalikan JSON berisi tiga isian: ringkasan, kategori, urgensi
2. Minta API menjawab dalam format JSON supaya mudah ditampilkan.
3. Ubah index.html agar tombol "Proses" memanggil /api/proses,
   bukan data palsu.

HASIL YANG SAYA INGINKAN:
- Tuliskan langkah persis yang harus saya ketik di terminal untuk
  menjalankannya, satu per satu.
- Beri tahu persis di file mana dan baris mana saya menempelkan API key saya.
- Beri tahu cara menguji bahwa sambungannya sudah berhasil.
```

### Jalur Cadangan (Plan B) — bila Node.js gagal jalan

Jika lebih dari 5 menit peserta tersangkut di instalasi Node, hentikan dan pindah ke Plan B: minta agen memanggil API langsung dari browser.

**Sampaikan peringatannya dengan jelas dan jangan diperhalus:**

> Cara ini **menempelkan kunci Anda di halaman yang bisa dibaca siapa pun**. Boleh dipakai HANYA untuk latihan di komputer sendiri. Aplikasi seperti ini **tidak boleh diunggah ke internet**, dan kuncinya **wajib dihapus di akhir sesi**.

**PROMPT 2-B:**

```
Node.js tidak bisa saya jalankan. Untuk latihan hari ini saja,
ubah agar index.html memanggil API AI langsung dari browser,
dengan API key saya tempel di satu variabel di baris paling atas file.

Saya sadar cara ini tidak aman dan hanya untuk latihan lokal.
Tambahkan komentar peringatan besar di atas variabel itu bahwa
file ini tidak boleh dibagikan atau diunggah ke internet.

Sisanya sama seperti sebelumnya: jawaban dalam format JSON berisi
ringkasan, kategori, dan urgensi.
```

**Checkpoint:** satu aduan asli diproses, hasil nyata muncul. **Inilah panggilan API berbayar pertama peserta.** Beri jeda satu menit — suruh mereka kembali ke dashboard dan melihat angka pemakaiannya bertambah. Momen ini yang mengubah "biaya" dari konsep menjadi kenyataan.

---

## TAHAP 3 — Penanganan error & biaya yang terlihat (8 menit)

**PROMPT 3:**

```
Sekarang buat aplikasi ini layak dipakai orang lain.

TUGAS 1 — PESAN ERROR YANG DIMENGERTI ORANG AWAM
Ganti semua pesan error mentah dengan kalimat bahasa Indonesia
yang jelas, untuk situasi berikut:
- API key salah, kosong, atau kedaluwarsa
- Kuota atau batas kecepatan habis (error 429)
- Tidak ada koneksi internet
- Jawaban dari API tidak berbentuk JSON yang benar
- Kotak teks masih kosong saat tombol ditekan
Setiap pesan harus menyebutkan apa yang harus saya lakukan berikutnya.

TUGAS 2 — INDIKATOR BIAYA
Setelah setiap pemrosesan, tampilkan di bawah hasil:
jumlah token masuk, token keluar, dan perkiraan biaya dalam Rupiah.
Letakkan harga per 1 juta token dan kurs Rupiah sebagai variabel
di paling atas file, dengan komentar bahwa saya harus mengisinya sendiri.
JANGAN mengarang angka harga.

TUGAS 3 — STATUS TUNGGU
Selama menunggu jawaban, tampilkan "Sedang memproses..." dan
nonaktifkan tombol Proses, supaya saya tidak menekan berkali-kali.
Jelaskan singkat kenapa menekan tombol dua kali berarti membayar dua kali.
```

**Poin yang harus ditekankan pengajar:** angka Rupiah yang muncul di layar adalah **perkiraan berdasarkan harga yang peserta isi sendiri** — bukan tagihan resmi. Tagihan resmi hanya ada di dashboard penyedia. Jangan biarkan peserta pulang mengira aplikasinya bisa membaca tagihan.

---

## TAHAP 4 — Sengaja dirusak: belajar dari error (7 menit)

Ini bagian yang paling sering dilewati pengajar dan paling banyak diingat peserta.

**Minta peserta melakukan tiga sabotase berurutan:**

| # | Sabotase | Yang seharusnya terlihat | Pelajarannya |
|---|---|---|---|
| 1 | Hapus 3 huruf terakhir API key, jalankan | Pesan "kunci tidak valid" | Kunci salah ≠ aplikasi rusak |
| 2 | Tekan tombol Proses **secepat mungkin 15–20 kali berturut-turut** | Muncul error 429 / batas kecepatan | **Inilah rasanya kuota habis.** Di akun berbayar, yang habis bukan kuota — tapi uang |
| 3 | Kosongkan kotak teks lalu tekan Proses | Pesan "teks masih kosong" | Validasi mencegah panggilan API sia-sia yang tetap ditagih |

**PROMPT 4 — pola baku memperbaiki error (ajarkan sebagai template seumur hidup):**

```
Saya mendapat pesan error berikut saat menekan tombol Proses:

[TEMPEL PESAN ERROR PERSIS SEPERTI YANG MUNCUL DI LAYAR]

Tolong:
1. Jelaskan dalam bahasa Indonesia sederhana apa arti error ini.
2. Sebutkan kemungkinan penyebabnya, urut dari yang paling sering terjadi.
3. Perbaiki kodenya.
4. Beri tahu cara saya memastikan perbaikannya sudah berhasil.
```

> **Tekankan:** menempelkan pesan error **apa adanya** jauh lebih berguna daripada menulis "kok error ya". Agen AI tidak bisa melihat layar peserta.

---

## TAHAP 5 — Pengaman biaya (8 menit)

Jika waktu tersisa < 8 menit, jadikan ini tugas mandiri dan langsung ke penutup.

**PROMPT 5:**

```
Tambahkan pengaman biaya berikut, dan jelaskan singkat setiap
pengaman itu menghemat di bagian mana:

1. Batasi teks masukan maksimal 2000 karakter. Tampilkan penghitung
   sisa karakter di bawah kotak teks.
2. Batasi panjang jawaban dari API (maksimal sekitar 300 token keluar).
3. Simpan hasil di memori: kalau teks yang sama diproses lagi, ambil
   dari simpanan dan JANGAN panggil API. Beri label "diambil dari
   simpanan (tanpa biaya)".
4. Batasi maksimal 20 panggilan API per sesi. Setelah itu kunci tombol
   dan tampilkan "Batas latihan tercapai".
5. Tampilkan total biaya kumulatif sejak halaman dibuka.
```

**Diskusi 2 menit setelah selesai:** dari lima pengaman itu, mana yang paling besar penghematannya? *(Jawaban: nomor 2 dan 3 — membatasi token keluar memotong sisi yang paling mahal, dan simpanan memotong panggilan berulang sampai nol.)*

---

# BAGIAN 3 — PENUTUP (10 MENIT)

## 3.1 Lembar Kalkulasi Biaya (5 menit) — WAJIB

Ini pengganti pengalaman "melihat tagihan" yang tidak didapat peserta karena memakai free tier. Isi bersama-sama di papan tulis.

**Skenario:** aplikasi Asisten Aduan dipakai satu kantor. **1.000 aduan per bulan.**

| Langkah | Cara hitung | Isi |
|---|---|---|
| a. Token masuk per aduan | ± panjang aduan ÷ 4 karakter | ~ 250 token |
| b. Token keluar per aduan | ± panjang jawaban ÷ 4 karakter | ~ 100 token |
| c. Total token masuk/bulan | a × 1.000 | 250.000 |
| d. Total token keluar/bulan | b × 1.000 | 100.000 |
| e. Harga input per 1 juta token | **[isi dari halaman harga hari ini, USD]** | ......... |
| f. Harga output per 1 juta token | **[isi dari halaman harga hari ini, USD]** | ......... |
| g. Biaya input | (c ÷ 1.000.000) × e | ......... |
| h. Biaya output | (d ÷ 1.000.000) × f | ......... |
| i. Total USD/bulan | g + h | ......... |
| j. Kurs hari ini | **[isi kurs USD→IDR hari ini]** | ......... |
| **k. Total Rupiah/bulan** | i × j | **.........** |

**Tiga pertanyaan diskusi setelah angka keluar:**
1. Berapa biaya kalau volumenya naik 10×? *(Jawaban: naik hampir persis 10× — biaya API bersifat linear, tidak ada diskon volume otomatis. Ini beda dengan langganan bulanan.)*
2. Kalau setiap petugas iseng menekan tombol 5× untuk aduan yang sama, biaya naik berapa persen? *(400%. Ini kenapa Tahap 5 nomor 3 penting.)*
3. Pada volume berapa free tier akan jebol dan Anda harus mulai membayar?

## 3.2 Refleksi (3 menit)

Minta setiap peserta menuliskan satu kalimat:
> *"Hal yang paling mengejutkan saya hari ini tentang API berbayar adalah ..."*

Baca 2–3 secara acak. Ini pengganti evaluasi formal yang cepat dan cukup jujur.

## 3.3 Checklist Penutup — JANGAN ADA YANG PULANG SEBELUM INI (2 menit)

- [ ] API key sudah **dihapus/dicabut** dari dashboard penyedia
- [ ] File `.env` **tidak** ikut tersalin ke folder yang dibagikan
- [ ] Peserta bisa menyebutkan di mana melihat pemakaian & tagihan
- [ ] Lembar kalkulasi biaya sudah terisi

## 3.4 Tugas Mandiri

Pilih **satu**:

**A. Tingkat dasar** — Ganti studi kasus dengan kebutuhan pekerjaan Anda sendiri (contoh: mengklasifikasi disposisi surat, meringkas notulen, menilai kelengkapan berkas). Kumpulkan: prompt yang Anda pakai + tangkapan layar hasil + perkiraan biaya per bulan.

**B. Tingkat lanjut** — Pindahkan API key dari browser ke server kecil (bila tadi memakai Plan B), lalu tambahkan pencatatan: setiap pemanggilan API tersimpan ke sebuah file beserta waktu dan jumlah tokennya.

---

# LAMPIRAN

## Lampiran A — Bank Prompt Siap Pakai

| Kode | Untuk apa | Kapan dipakai |
|---|---|---|
| PROMPT 1 | Membuat tampilan dengan data palsu | Selalu, sebelum menyentuh API |
| PROMPT 2 | Menyambungkan API dengan kunci aman di server | Jalur utama |
| PROMPT 2-B | Menyambungkan API langsung dari browser | Hanya jika Node.js gagal; kunci wajib dihapus |
| PROMPT 3 | Pesan error ramah + indikator biaya | Setelah sambungan berhasil |
| PROMPT 4 | Memperbaiki error apa pun | Setiap kali muncul error |
| PROMPT 5 | Pengaman biaya | Sebelum aplikasi dipakai orang lain |

**Prompt tambahan yang sering dibutuhkan:**

**A-1. Memaksa agen menjelaskan, bukan langsung menulis kode**
```
Sebelum menulis kode apa pun, jelaskan dulu dalam maksimal 5 kalimat
bahasa Indonesia sederhana: apa yang akan kamu ubah, di file mana,
dan apa risikonya. Tunggu saya bilang "lanjut" baru menulis kode.
```

**A-2. Kembali ke kondisi yang tadi berfungsi**
```
Perubahan terakhir membuat aplikasi rusak. Kembalikan ke kondisi
seperti sebelum perubahan terakhir, lalu jelaskan apa yang salah tadi.
Jangan menambah fitur baru apa pun.
```

**A-3. Memeriksa kebocoran kunci sebelum membagikan kode**
```
Periksa seluruh file di proyek ini. Apakah ada API key, kata sandi,
atau data rahasia yang tertulis langsung di dalam kode atau di file
yang akan ikut terbagi? Sebutkan nama file dan nomor barisnya.
Jangan tampilkan isi kuncinya di jawabanmu.
```

**A-4. Menerjemahkan dokumentasi API ke bahasa awam**
```
Saya menempelkan potongan dokumentasi API di bawah ini. Saya bukan
programmer. Jelaskan dalam bahasa Indonesia sederhana:
1. Apa yang harus saya kirim
2. Apa yang akan saya terima
3. Bagaimana layanan ini menghitung tagihan
4. Batas pemakaian apa saja yang ada

[TEMPEL DOKUMENTASINYA DI SINI]
```

## Lampiran B — Tabel Troubleshooting

| Yang terlihat peserta | Kemungkinan penyebab | Tindakan tercepat |
|---|---|---|
| `401` / `403` / "invalid key" | Kunci salah salin, ada spasi, atau sudah dicabut | Salin ulang kunci dengan tombol salin |
| `429` / "rate limit" / "quota exceeded" | Batas kecepatan atau kuota harian tercapai | Tunggu 1 menit; kurangi frekuensi klik |
| `404` / "model not found" | Nama model sudah tidak berlaku | Suruh agen cek nama model aktif di dokumentasi resmi |
| Halaman kosong / tombol tidak bereaksi | Kesalahan JavaScript | Buka Console browser (F12), tempel isinya ke PROMPT 4 |
| "Failed to fetch" / CORS | Browser menolak panggilan lintas alamat | Ini alasan Jalur Utama pakai server kecil; pindah ke Jalur Utama |
| Server Node tidak mau jalan | Node.js belum terpasang / port terpakai | Cek `node -v`; jika gagal, pindah ke Plan B |
| Jawaban AI tidak beraturan | Format JSON tidak dipaksakan | Minta agen menambahkan validasi & percobaan ulang sekali |
| Tagihan/pemakaian tidak bertambah | Aplikasi masih memakai data palsu | Pastikan Tahap 2 benar-benar selesai |

## Lampiran C — Kamus Istilah (bahasa awam)

| Istilah | Arti sederhana |
|---|---|
| **API** | Cara satu aplikasi memesan jasa ke aplikasi lain |
| **Endpoint** | Alamat internet tempat pesanan dikirim |
| **API Key** | Kartu identitas yang menempel pada tagihan Anda |
| **Request / Response** | Pesanan yang dikirim / jawaban yang kembali |
| **Token** | Potongan kata; satuan tagihan API AI (kasarnya 1 token ≈ 4 huruf) |
| **Rate limit** | Batas berapa kali boleh memanggil dalam waktu tertentu |
| **Kuota** | Jatah pemakaian dalam periode tertentu |
| **Free tier** | Jatah gratis permanen sampai batas tertentu |
| **Pay-as-you-go** | Bayar sesuai pemakaian, tanpa batas atas otomatis |
| **`.env`** | File khusus tempat menyimpan rahasia, tidak ikut dibagikan |
| **`.gitignore`** | Daftar file yang sengaja tidak ikut diunggah |
| **JSON** | Format tulisan rapi agar data mudah dibaca program |
| **Error 429** | "Terlalu sering, pelan-pelan" |
| **Error 401/403** | "Saya tidak mengenali/mengizinkan Anda" |
| **Backend / server** | Bagian aplikasi yang berjalan tersembunyi, tempat aman menyimpan kunci |

## Lampiran D — Rubrik Penilaian

| Aspek | Bobot | 1 — Belum | 2 — Cukup | 3 — Baik | 4 — Sangat Baik |
|---|---|---|---|---|---|
| **Aplikasi berfungsi** | 25% | Tidak jalan | Jalan dengan data palsu | Terhubung API, hasil muncul | Terhubung + tampilan rapi |
| **Keamanan kunci** | 30% | Kunci di kode & dibagikan | Kunci di kode, disadari risikonya | Kunci di `.env` + `.gitignore` | Di `.env` + dicabut setelah sesi |
| **Penanganan error** | 20% | Tidak ada | Ada 1–2 pesan | Semua situasi ditangani | Pesan menyebutkan tindak lanjut |
| **Kesadaran biaya** | 25% | Tidak bisa menjelaskan | Tahu ada biaya | Lembar kalkulasi terisi benar | Menerapkan pengaman biaya + bisa argumentasikan |

**Batas kelulusan sesi:** total ≥ 2,5 **dan** aspek Keamanan Kunci minimal 3. Alasannya: peserta yang membuat aplikasi bagus tapi membocorkan kuncinya menimbulkan kerugian nyata; yang aplikasinya sederhana tapi kuncinya aman tidak merugikan siapa pun.

## Lampiran E — Checklist Persiapan Pengajar

**H-7**
- [ ] Uji coba seluruh alur sendiri, dari nol, di komputer sebersih mungkin — catat waktu aslinya
- [ ] Verifikasi harga & batas kuota dari halaman resmi (Lampiran G)
- [ ] Siapkan 2–3 API key cadangan untuk peserta yang akunnya bermasalah

**H-1 — kirim ke peserta, wajib selesai sebelum masuk kelas**
- [ ] Antigravity sudah terpasang dan sudah bisa login dengan akun Google
- [ ] Sudah pernah membuat 1 proyek/folder di Antigravity
- [ ] Node.js terpasang (uji: buka terminal, ketik `node -v`, harus muncul angka versi)
- [ ] Akun Google **pribadi**, bukan akun kantor yang dibatasi kebijakan
- [ ] Laptop membawa charger; jaringan diuji

**Hari-H, 30 menit sebelum mulai**
- [ ] Uji jaringan ruangan dengan 1 panggilan API sungguhan
- [ ] Buka tab dashboard tagihan akun berbayar Anda, siap didemokan
- [ ] Siapkan 3 contoh teks aduan (pendek, sedang, panjang) untuk ditempel peserta
- [ ] Tulis di papan: alamat halaman API key + link modul ini

**Setelah sesi**
- [ ] Pastikan seluruh peserta sudah mencabut kuncinya
- [ ] Cabut API key cadangan yang tadi dipinjamkan

## Lampiran F — Contoh Teks Aduan untuk Latihan

**Pendek:**
> Lampu jalan di Jl. Melati RT 04 mati sudah dua minggu, rawan kejahatan malam hari.

**Sedang:**
> Selamat pagi, saya warga Perumahan Griya Asri Blok C. Sejak awal bulan, air PDAM di wilayah kami hanya mengalir pukul 11 malam sampai 3 pagi. Warga terpaksa membeli air galon untuk mandi. Sudah dilaporkan ke petugas kelurahan tapi belum ada tindak lanjut. Mohon segera ditangani karena ada balita dan lansia di lingkungan kami.

**Panjang / bercampur:**
> Saya mau mengadu sekaligus bertanya. Pertama, trotoar depan pasar rusak parah dan sudah memakan korban dua orang jatuh minggu lalu. Kedua, saya sudah mengurus surat keterangan domisili sejak tiga minggu lalu tapi katanya masih diproses terus, padahal saya butuh untuk urusan sekolah anak yang batas waktunya minggu depan. Ketiga, apakah benar ada program bantuan untuk pedagang kecil? Saya dengar dari tetangga tapi tidak ada informasi resminya di mana pun. Tolong dijawab ya Pak/Bu, terima kasih.

*(Teks panjang ini sengaja mengandung tiga aduan sekaligus — bagus untuk mendiskusikan keterbatasan aplikasi satu-kategori.)*

## Lampiran G — Catatan Verifikasi & Tingkat Keyakinan

**Wajib diverifikasi ulang maksimal H-3 sebelum mengajar:**

| Klaim dalam modul | Status | Sumber untuk verifikasi |
|---|---|---|
| Antigravity tidak mendukung *bring-your-own-key* untuk kuota agennya | **[Keyakinan sedang]** — tertulis di dokumentasi resmi per September 2026, tapi produk ini berubah cepat | Halaman Plans resmi Antigravity |
| Pengguna standar (non-langganan) tetap dapat kuota agen | **[Keyakinan sedang]** — dokumentasi menyebut "kuota berarti, diperbarui mingguan" tanpa angka pasti | Halaman Plans resmi Antigravity |
| Batas free tier API AI tidak lagi diterbitkan sebagai angka tetap | **[Keyakinan tinggi]** — dokumentasi resmi mengarahkan ke dashboard masing-masing pengguna | Halaman Rate Limits resmi penyedia |
| Token keluar 5–10× lebih mahal dari token masuk | **[Keyakinan tinggi]** untuk pola umum, **[rendah]** untuk model tertentu — rasionya berbeda per model | Halaman Pricing resmi penyedia |
| Harga per 1 juta token | **HARUS DIISI SENDIRI** — jangan pakai angka hafalan | Halaman Pricing resmi penyedia |
| Kurs USD→IDR | **HARUS DIISI SENDIRI** — berubah harian | Kurs Bank Indonesia |
| Nama model yang aktif | **HARUS DICEK** — nama model sering dipensiunkan | Dokumentasi model resmi penyedia |

**Yang TIDAK diverifikasi dan sengaja dibiarkan generik:** langkah antarmuka Antigravity (nama tombol, letak menu). Antarmuka IDE berubah antar versi. Karena itu modul ini menuliskan *apa yang harus dicapai*, bukan *tombol mana yang diklik*. Lakukan uji coba sendiri H-7 dan sisipkan tangkapan layar versi yang akan dipakai di kelas.

---

## Catatan Penutup untuk Pengajar

Materi ini akan terasa berhasil kalau di akhir sesi peserta bisa menjawab satu pertanyaan ini dengan yakin:

> *"Kalau besok aplikasi buatan saya dipakai 1.000 orang, siapa yang membayar, berapa, dan bagaimana saya menghentikannya kalau kebablasan?"*

Peserta yang aplikasinya jalan tapi tidak bisa menjawab pertanyaan itu **belum lulus sesi ini**. Kemampuan menyuruh AI menulis kode sudah murah dan makin murah. Yang mahal — dan yang membuat orang dipercaya memegang proyek — adalah kemampuan menahan diri dan mengerti konsekuensi biayanya.
