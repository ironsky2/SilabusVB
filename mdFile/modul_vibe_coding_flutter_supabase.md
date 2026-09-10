# 🎵 Modul Praktikum Vibe Coding
# Membuat Aplikasi Android dengan Flutter + Supabase
# Menggunakan Antigravity IDE

> **Target Peserta:** Profesional non-IT yang ingin membuat aplikasi sendiri  
> **Tool AI:** Antigravity IDE (dengan AI Agent)  
> **Metode:** Vibe Coding — kombinasi prompt AI + kode minimal  
> **Estimasi Waktu:** 3–4 jam  
> **Output:** Aplikasi Flutter lengkap dengan database Supabase

---

## 🤔 Apa itu Vibe Coding?

**Vibe Coding** adalah pendekatan pembuatan aplikasi di mana Anda **tidak perlu menulis kode dari nol**. Anda cukup:

1. **Menjelaskan** apa yang Anda inginkan dalam bahasa sehari-hari
2. **AI menghasilkan** kode yang dibutuhkan
3. **Anda mereview** dan menjalankan hasilnya
4. **Iterasi** — perbaiki atau tambah fitur via prompt lagi

```
Anda (non-IT)          Antigravity AI           Hasil
─────────────          ──────────────           ──────
"Buatkan halaman   →   Generate kode   →   Aplikasi berjalan
 login dengan          Dart/Flutter         di HP Android
 email & password"     lengkap + tes
```

> 💡 **Analogi:** Vibe coding itu seperti punya **developer pribadi** yang siap 24 jam. Anda cukup bilang apa yang mau dibuat, dia yang kerjakan.

---

## 📋 Daftar Isi

1. [Persiapan Environment](#bagian-1-persiapan-environment)
2. [Mengenal Antigravity IDE](#bagian-2-mengenal-antigravity-ide)
3. [Teknik Menulis Prompt yang Efektif](#bagian-3-teknik-menulis-prompt-yang-efektif)
4. [Setup Project Flutter via Prompting](#bagian-4-setup-project-flutter-via-prompting)
5. [Membuat UI Aplikasi via Prompting](#bagian-5-membuat-ui-aplikasi-via-prompting)
6. [Koneksi ke Supabase via Prompting](#bagian-6-koneksi-ke-supabase-via-prompting)
7. [Fitur CRUD via Prompting](#bagian-7-fitur-crud-via-prompting)
8. [Autentikasi Pengguna via Prompting](#bagian-8-autentikasi-pengguna-via-prompting)
9. [Test di HP Android & Browser](#bagian-9-test-di-hp-android--browser)
10. [Tips Iterasi & Perbaikan via Prompt](#bagian-10-tips-iterasi--perbaikan-via-prompt)
11. [Troubleshooting via Prompting](#bagian-11-troubleshooting-via-prompting)

---

## BAGIAN 1: Persiapan Environment

> ℹ️ Bagian ini adalah satu-satunya bagian yang membutuhkan **perintah manual**. Setelah setup selesai, semua pengembangan dilakukan via prompting.

### 1.1 Software yang Dibutuhkan

| Software | Download | Keterangan |
|---|---|---|
| Flutter SDK | https://flutter.dev | Framework aplikasi |
| Android Command Line Tools | https://developer.android.com/studio#command-line-tools-only | Android SDK ringan |
| Antigravity IDE | Sudah terinstall | Editor + AI Agent |
| Akun Supabase | https://supabase.com | Database cloud gratis |

### 1.2 Setup Cepat (Jalankan di Command Prompt)

**Step 1 — Ekstrak Flutter ke folder tanpa spasi, lalu tambahkan ke PATH:**
```
D:\Projects\flutter\bin
```

**Step 2 — Buat struktur folder Android SDK:**
```
D:\Projects\Android\cmdline-tools\latest\
```
Ekstrak Command Line Tools ke folder `latest\` tersebut.

**Step 3 — Install Android SDK:**
```bash
D:\Projects\Android\cmdline-tools\latest\bin\sdkmanager.bat ^
  --sdk_root=D:\Projects\Android ^
  "platform-tools" "platforms;android-34" "build-tools;34.0.0"
```

**Step 4 — Konfigurasi Flutter:**
```bash
flutter config --android-sdk D:\Projects\Android
flutter doctor --android-licenses
```
*(Ketik `y` untuk semua pertanyaan)*

**Step 5 — Verifikasi:**
```bash
flutter doctor
```
Pastikan `[✓] Android toolchain` muncul.

---

> ✅ **Setup selesai!** Mulai sekarang, semua pengembangan dilakukan via **prompt ke Antigravity IDE**.

---

## BAGIAN 2: Mengenal Antigravity IDE

### 2.1 Area Kerja Antigravity IDE

```
┌─────────────────────────────────────────────────────┐
│  Antigravity IDE                                    │
├──────────────┬──────────────────────┬───────────────┤
│              │                      │               │
│   Explorer   │    Editor Kode       │  Sidebar Chat │
│   (File      │    (main.dart,       │  ← Di sinilah │
│    Tree)     │     dll)             │    Anda        │
│              │                      │    prompting! │
│              │                      │               │
├──────────────┴──────────────────────┴───────────────┤
│  Terminal (untuk flutter run, dll)                  │
└─────────────────────────────────────────────────────┘
```

### 2.2 Cara Menggunakan Sidebar Chat

1. Klik ikon chat di sidebar kanan **atau** tekan `Ctrl+Shift+I`
2. Ketik prompt Anda di kotak teks di bawah
3. Tekan **Enter** atau klik tombol kirim
4. AI akan:
   - Menganalisis project Anda
   - Menulis/memodifikasi file kode
   - Menjelaskan apa yang dilakukan

### 2.3 Fitur Penting untuk Vibe Coding

| Fitur | Cara Pakai | Kegunaan |
|---|---|---|
| **Agent Mode** | Default saat chat | AI bisa baca & tulis file project |
| **Inline Edit** | `Ctrl+I` di editor | Edit kode tertentu langsung |
| **Auto Accept** | Klik "Accept All" | Terapkan perubahan AI ke file |
| **Terminal** | `Ctrl+J` | Jalankan `flutter run` |

---

## BAGIAN 3: Teknik Menulis Prompt yang Efektif

### 3.1 Lima Prinsip Prompt Vibe Coding

#### 🎯 Prinsip 1: Spesifik, Bukan Umum

❌ **Buruk:**
```
Buatkan halaman login
```

✅ **Baik:**
```
Buatkan halaman login Flutter dengan:
- Field email dan password
- Tombol "Login" berwarna biru
- Link "Lupa Password?" di bawah tombol
- Validasi: email harus format valid, password minimal 6 karakter
- Tampilkan loading spinner saat proses login
```

---

#### 📐 Prinsip 2: Sertakan Konteks

❌ **Buruk:**
```
Tambahkan fitur hapus data
```

✅ **Baik:**
```
Di aplikasi Flutter saya yang menggunakan Supabase,
tambahkan fitur hapus data di halaman daftar produk (product_list.dart).
Saat user tekan tombol hapus, tampilkan dialog konfirmasi dulu.
Jika dikonfirmasi, hapus dari tabel "products" di Supabase.
```

---

#### 🔢 Prinsip 3: Satu Fitur Per Prompt

❌ **Buruk:**
```
Buatkan halaman login, register, dashboard, daftar produk, 
tambah produk, edit produk, hapus produk, dan laporan
```

✅ **Baik:**
```
Prompt 1: Buatkan halaman login dulu
Prompt 2: Setelah berhasil, buatkan halaman register
Prompt 3: Buatkan halaman dashboard setelah login
... dan seterusnya
```

---

#### 🎨 Prinsip 4: Jelaskan Tampilan yang Diinginkan

❌ **Buruk:**
```
Buatkan UI yang bagus
```

✅ **Baik:**
```
Buatkan UI dengan tema warna biru tua (#1565C0) dan putih.
Gunakan Material Design 3.
Card product harus menampilkan: nama, harga (format Rupiah), stok, gambar.
Gunakan ListView untuk daftar, bukan GridView.
```

---

#### 🔄 Prinsip 5: Iterasi, Jangan Takut Salah

Vibe coding adalah proses **iterasi**. Jika hasilnya tidak sesuai, perbaiki via prompt:

```
Hasilnya sudah baik, tapi ada yang perlu diubah:
1. Warna tombol ubah dari biru ke hijau
2. Ukuran font judul terlalu besar, kurangi sedikit
3. Tambahkan icon di sebelah kiri setiap item list
```

### 3.2 Template Prompt Siap Pakai

#### Template untuk Membuat Halaman Baru:
```
Buatkan halaman Flutter bernama [nama_halaman].dart dengan:
- Judul halaman: "[Judul]"
- Konten: [deskripsi isi halaman]
- Navigasi: [dari mana/ke mana navigasi]
- Warna tema: [warna]
- Data yang ditampilkan: [daftar data]
```

#### Template untuk Koneksi Database:
```
Di halaman [nama_file].dart, tambahkan fungsi untuk:
- [baca/tambah/edit/hapus] data dari tabel "[nama_tabel]" di Supabase
- Tampilkan loading indicator saat proses berlangsung
- Tampilkan pesan error jika gagal
- Refresh data setelah operasi berhasil
```

#### Template untuk Perbaikan Error:
```
Saya mendapat error berikut di [nama_file].dart:
[paste error message di sini]

Tolong perbaiki dan jelaskan apa penyebabnya.
```

---

## BAGIAN 4: Setup Project Flutter via Prompting

### 4.1 Buat Project Baru

Buka **Terminal** di Antigravity IDE (`Ctrl+J`), jalankan:

```bash
flutter create my_app
cd my_app
```

Lalu buka folder project di Antigravity IDE:
**File → Open Folder → pilih folder `my_app`**

### 4.2 Prompt: Setup Struktur Project

Setelah project terbuka, gunakan prompt berikut di **Sidebar Chat**:

---

📋 **PROMPT 4.2 — Setup Struktur Folder:**
```
Saya baru membuat project Flutter bernama my_app.
Tolong atur struktur folder yang rapi untuk aplikasi dengan fitur:
- Autentikasi (login, register)
- CRUD data (tambah, lihat, edit, hapus)
- Halaman profil pengguna

Buat folder-folder yang dibutuhkan di dalam lib/:
- screens/ (untuk halaman-halaman)
- widgets/ (untuk komponen UI yang reusable)
- services/ (untuk koneksi Supabase)
- models/ (untuk struktur data)

Buat juga file kosong placeholder di setiap folder agar struktur terlihat jelas.
```

---

💡 **Apa yang diharapkan AI lakukan:**
- Membuat folder `lib/screens/`, `lib/widgets/`, `lib/services/`, `lib/models/`
- Membuat file placeholder di setiap folder
- Menjelaskan fungsi setiap folder

### 4.3 Prompt: Tambahkan Package Supabase

📋 **PROMPT 4.3 — Install Supabase:**
```
Tambahkan package Supabase ke project Flutter saya.
Edit file pubspec.yaml dan tambahkan:
- supabase_flutter versi terbaru yang stabil

Setelah itu jalankan flutter pub get.
Juga setup inisialisasi Supabase di main.dart dengan placeholder URL dan key
(saya akan isi sendiri nanti).
```

---

💡 **Output yang diharapkan di `pubspec.yaml`:**
```yaml
dependencies:
  flutter:
    sdk: flutter
  supabase_flutter: ^2.0.0
```

💡 **Output yang diharapkan di `main.dart`:**
```dart
await Supabase.initialize(
  url: 'YOUR_SUPABASE_URL',       // ← Anda ganti ini
  anonKey: 'YOUR_SUPABASE_KEY',   // ← Anda ganti ini
);
```

---

## BAGIAN 5: Membuat UI Aplikasi via Prompting

### 5.1 Prompt: Halaman Splash Screen

📋 **PROMPT 5.1 — Splash Screen:**
```
Buatkan file lib/screens/splash_screen.dart untuk halaman splash screen dengan:
- Background warna biru tua (#1565C0)
- Logo/ikon aplikasi di tengah (gunakan icon Flutter built-in: Icons.apps)
- Nama aplikasi di bawah ikon dengan font putih, ukuran 24
- Loading indicator di bagian bawah
- Setelah 2 detik, otomatis navigasi ke halaman login

Gunakan StatefulWidget dan Timer untuk delay 2 detik.
```

---

### 5.2 Prompt: Halaman Login

📋 **PROMPT 5.2 — Halaman Login:**
```
Buatkan file lib/screens/login_screen.dart dengan tampilan:
- AppBar tidak ada (fullscreen)
- Background putih
- Logo/ikon di bagian atas (Icons.lock_outline, ukuran 80, warna biru)
- Judul "Selamat Datang" font besar
- Subjudul "Masuk ke akun Anda"
- TextField untuk email dengan icon email
- TextField untuk password dengan icon lock, ada toggle show/hide password
- Tombol "MASUK" warna biru, lebar penuh
- Teks "Belum punya akun? Daftar di sini" di bawah
- Semua elemen di dalam SingleChildScrollView agar tidak overflow
- Validasi form menggunakan GlobalKey<FormState>

Untuk sementara, fungsi login dikosongkan dulu (akan diisi di bagian Supabase).
```

---

### 5.3 Prompt: Halaman Dashboard / Home

📋 **PROMPT 5.3 — Halaman Dashboard:**
```
Buatkan file lib/screens/home_screen.dart sebagai halaman utama setelah login:
- AppBar dengan judul "Dashboard" dan tombol logout di kanan
- Greeting text: "Halo, [nama pengguna]!"
- Grid 2 kolom berisi menu card dengan icon dan label:
  * Card 1: icon list, label "Data"
  * Card 2: icon add_circle, label "Tambah"  
  * Card 3: icon bar_chart, label "Laporan"
  * Card 4: icon person, label "Profil"
- Setiap card bisa ditekan (onTap) tapi navigasi dikosongkan dulu
- Gunakan GridView.count

Gunakan dummy nama "Pengguna" untuk greeting sementara.
```

---

### 5.4 Prompt: Halaman Daftar Data

📋 **PROMPT 5.4 — Halaman List:**
```
Buatkan file lib/screens/data_list_screen.dart untuk menampilkan daftar data:
- AppBar judul "Daftar Data"
- FloatingActionButton dengan icon + untuk tambah data baru
- ListView.builder untuk menampilkan daftar
- Setiap item ditampilkan sebagai Card berisi:
  * Judul item (bold)
  * Deskripsi singkat
  * Tanggal dibuat
  * Tombol Edit (icon pensil, warna oranye)
  * Tombol Hapus (icon trash, warna merah)
- Tampilkan EmptyState (ikon dan teks) jika data kosong
- Gunakan data dummy List<Map> untuk sementara:
  [
    {'judul': 'Item Pertama', 'deskripsi': 'Ini adalah item pertama', 'tanggal': '2026-09-01'},
    {'judul': 'Item Kedua', 'deskripsi': 'Ini adalah item kedua', 'tanggal': '2026-09-02'},
  ]
```

---

## BAGIAN 6: Koneksi ke Supabase via Prompting

### 6.1 Persiapan Supabase Dashboard

Sebelum prompting, lakukan ini di **supabase.com**:

1. Login → klik **"New Project"**
2. Isi nama project dan password database → klik **Create**
3. Tunggu ~2 menit hingga project siap
4. Buka **Settings → API**, catat:
   - **Project URL**: `https://xxxx.supabase.co`
   - **anon public**: `eyJhbGci...`
5. Buka **Table Editor → New Table**, buat tabel `items` dengan kolom:
   - `id` (int8, primary key, auto-increment)
   - `judul` (text, not null)
   - `deskripsi` (text)
   - `created_at` (timestamptz, default: now())
   - `user_id` (uuid) — untuk filter data per pengguna

### 6.2 Prompt: Isi Konfigurasi Supabase

📋 **PROMPT 6.2 — Setup Supabase Config:**
```
Saya sudah punya akun Supabase dengan:
- URL: https://XXXX.supabase.co
- Anon Key: eyJhbGci...

Tolong:
1. Buat file lib/services/supabase_service.dart berisi konfigurasi dan helper Supabase
2. Update main.dart untuk inisialisasi Supabase dengan URL dan key di atas
3. Buat constant file lib/config/supabase_config.dart untuk menyimpan URL dan key
   (agar mudah diubah di masa depan)
```

---

💡 **Output yang diharapkan di `supabase_config.dart`:**
```dart
class SupabaseConfig {
  static const String url = 'https://XXXX.supabase.co';
  static const String anonKey = 'eyJhbGci...';
}
```

### 6.3 Prompt: Buat Service Layer

📋 **PROMPT 6.3 — Data Service:**
```
Di file lib/services/data_service.dart, buatkan class DataService dengan fungsi:

1. getAllItems() → ambil semua data dari tabel "items" di Supabase
   - Urutkan berdasarkan created_at terbaru
   - Return List<Map<String, dynamic>>

2. addItem(String judul, String deskripsi) → tambah data baru ke Supabase
   - Return true jika berhasil, false jika gagal

3. updateItem(int id, String judul, String deskripsi) → edit data
   - Return true jika berhasil

4. deleteItem(int id) → hapus data berdasarkan id
   - Return true jika berhasil

Setiap fungsi harus ada try-catch untuk handle error.
Gunakan supabase = Supabase.instance.client untuk akses Supabase.
```

---

## BAGIAN 7: Fitur CRUD via Prompting

### 7.1 Prompt: Sambungkan List dengan Supabase

📋 **PROMPT 7.1 — List dari Supabase:**
```
Update file lib/screens/data_list_screen.dart:
- Ganti data dummy dengan data asli dari Supabase
- Import DataService dari lib/services/data_service.dart
- Tambahkan initState() yang memanggil DataService().getAllItems()
- Tampilkan CircularProgressIndicator saat loading
- Tampilkan SnackBar merah jika error
- Refresh list setelah ada perubahan data (tambah/edit/hapus)
- Ubah struktur menjadi StatefulWidget jika belum
```

---

### 7.2 Prompt: Form Tambah Data

📋 **PROMPT 7.2 — Form Tambah:**
```
Buatkan file lib/screens/add_item_screen.dart untuk halaman tambah data:
- AppBar judul "Tambah Data Baru"
- Form dengan validasi menggunakan GlobalKey<FormState>
- TextField "Judul" (required, min 3 karakter)
- TextField "Deskripsi" (optional, multiline 3 baris)
- Tombol "SIMPAN" di bagian bawah
- Saat tombol ditekan:
  * Validasi form
  * Panggil DataService().addItem()
  * Tampilkan loading saat proses
  * Jika berhasil: kembali ke halaman list dan kirim signal refresh
  * Jika gagal: tampilkan SnackBar error
```

---

### 7.3 Prompt: Form Edit Data

📋 **PROMPT 7.3 — Form Edit:**
```
Buatkan file lib/screens/edit_item_screen.dart mirip dengan add_item_screen.dart, 
tapi untuk edit data yang sudah ada:
- Terima parameter: id (int), judul (String), deskripsi (String)
- Pre-fill form dengan data yang sudah ada
- AppBar judul "Edit Data"
- Saat simpan, panggil DataService().updateItem() bukan addItem()
- Setelah berhasil kembali ke list dengan signal refresh
```

---

### 7.4 Prompt: Sambungkan Tombol di List Screen

📋 **PROMPT 7.4 — Hubungkan Navigasi:**
```
Update lib/screens/data_list_screen.dart:
1. Tombol FAB (+) → navigasi ke AddItemScreen, 
   setelah kembali refresh data jika ada perubahan

2. Tombol Edit di setiap item → navigasi ke EditItemScreen 
   dengan data item tersebut, setelah kembali refresh data

3. Tombol Hapus di setiap item → tampilkan AlertDialog konfirmasi:
   - Judul: "Hapus Data?"
   - Isi: "Data yang dihapus tidak dapat dikembalikan."
   - Tombol "Batal" dan "Hapus" (merah)
   - Jika konfirmasi, panggil DataService().deleteItem(id)
   - Refresh list setelah hapus
```

---

## BAGIAN 8: Autentikasi Pengguna via Prompting

### 8.1 Aktifkan Auth di Supabase

Di **Supabase Dashboard**:
1. Klik **Authentication** di sidebar
2. Klik **"Providers"**
3. Pastikan **Email** provider sudah aktif (default sudah aktif)

### 8.2 Prompt: Service Autentikasi

📋 **PROMPT 8.2 — Auth Service:**
```
Buatkan file lib/services/auth_service.dart dengan class AuthService berisi:

1. signUp(String email, String password) 
   → daftar user baru via Supabase Auth
   → Return: null jika berhasil, String error message jika gagal

2. signIn(String email, String password)
   → login user via Supabase Auth  
   → Return: null jika berhasil, String error message jika gagal

3. signOut()
   → logout user

4. getCurrentUser()
   → return User? (null jika belum login)

5. isLoggedIn() → bool
   → cek apakah user sudah login

Gunakan supabase.auth untuk semua operasi autentikasi.
```

---

### 8.3 Prompt: Sambungkan Login Screen dengan Auth

📋 **PROMPT 8.3 — Fungsi Login:**
```
Update lib/screens/login_screen.dart:
- Import AuthService
- Fungsi login: panggil AuthService().signIn(email, password)
- Tampilkan loading indicator di tombol saat proses
- Jika berhasil: navigasi ke HomeScreen (hapus semua history navigasi)
- Jika gagal: tampilkan SnackBar dengan pesan error
- Tambahkan navigasi ke RegisterScreen saat link "Daftar di sini" ditekan
```

---

### 8.4 Prompt: Halaman Register

📋 **PROMPT 8.4 — Register Screen:**
```
Buatkan lib/screens/register_screen.dart untuk halaman daftar akun:
- Mirip dengan login_screen.dart tapi tambahkan:
  * Field "Konfirmasi Password"
  * Validasi: password dan konfirmasi harus sama
- Gunakan AuthService().signUp() untuk proses registrasi
- Setelah berhasil daftar: tampilkan dialog sukses dan kembali ke halaman login
- Ada link "Sudah punya akun? Masuk" di bawah
```

---

### 8.5 Prompt: Auto-Login & Route Guard

📋 **PROMPT 8.5 — Auth Guard:**
```
Update main.dart dan SplashScreen:
- Di SplashScreen, setelah 2 detik cek apakah user sudah login:
  * Jika sudah login → navigasi ke HomeScreen
  * Jika belum → navigasi ke LoginScreen
- Gunakan AuthService().isLoggedIn() untuk pengecekan
- Pastikan HomeScreen tidak bisa diakses jika belum login
```

---

## BAGIAN 9: Test di HP Android & Browser

### 9.1 Persiapan HP Android

**Aktifkan Developer Mode di HP:**
1. Buka **Pengaturan → Tentang Ponsel**
2. Tap **Nomor Build** sebanyak **7 kali**
3. Masukkan PIN jika diminta
4. Muncul notifikasi: *"Anda sekarang seorang developer"* ✅

**Aktifkan USB Debugging:**
1. Buka **Pengaturan → Opsi Pengembang**
2. Aktifkan **USB Debugging** ✅

**Sambungkan ke Komputer:**
1. Hubungkan HP via kabel USB (kabel data, bukan charger)
2. Di HP, pilih mode **"Transfer File (MTP)"**
3. Tap **"Izinkan"** pada dialog USB Debugging yang muncul di HP

### 9.2 Verifikasi HP Terdeteksi

Buka Terminal di Antigravity IDE (`Ctrl+J`):

```bash
adb devices
```

Output yang diharapkan:
```
List of devices attached
XXXXXXXX    device
```

Jika muncul `unauthorized`, pastikan Anda sudah tap "Allow" di HP.

### 9.3 Prompt: Pastikan App Siap Dijalankan

📋 **PROMPT 9.3 — Final Check:**
```
Sebelum saya jalankan aplikasi, tolong lakukan review menyeluruh:
1. Cek apakah semua import sudah benar di setiap file
2. Cek apakah semua navigasi antar halaman sudah terhubung dengan benar
3. Cek apakah ada fungsi yang belum diimplementasi (masih TODO/placeholder)
4. Pastikan pubspec.yaml sudah include package supabase_flutter
5. Pastikan AndroidManifest.xml sudah ada permission INTERNET

Tampilkan daftar file yang perlu diperbaiki jika ada.
```

---

### 9.4 Jalankan di HP Android

Di Terminal Antigravity IDE:

```bash
flutter run
```

Jika ada beberapa device:
```bash
flutter devices        # lihat daftar device
flutter run -d [ID]    # jalankan di device tertentu
```

**Saat app berjalan, Anda bisa:**
| Tekan | Fungsi |
|---|---|
| `r` | Hot Reload — update tampilan tanpa restart |
| `R` | Hot Restart — restart app dari awal |
| `q` | Keluar / stop app |

### 9.5 Jalankan di Browser

```bash
flutter run -d chrome
```

Browser Chrome akan otomatis terbuka dengan app Anda.

> ⚠️ **Catatan Browser:** Beberapa fitur seperti kamera dan notifikasi mungkin berbeda perilakunya di browser vs HP Android.

---

## BAGIAN 10: Tips Iterasi & Perbaikan via Prompt

### 10.1 Cara Perbaiki Tampilan

Jika tampilan tidak sesuai ekspektasi:

📋 **PROMPT Perbaikan UI:**
```
Di halaman [nama_halaman], ubah:
1. [Elemen X]: [perubahan yang diinginkan]
2. [Elemen Y]: [perubahan yang diinginkan]

Contoh:
1. Warna tombol MASUK: ubah dari biru ke hijau emerald (#2E7D32)
2. Ukuran teks judul: terlalu besar, kurangi dari 28 ke 22
3. Spacing antar form field: tambah jarak jadi 16px
```

### 10.2 Cara Tambah Fitur Baru

📋 **PROMPT Tambah Fitur:**
```
Tambahkan fitur [nama fitur] ke aplikasi:
- Lokasi: [di halaman mana]
- Fungsi: [apa yang dilakukan]
- UI: [seperti apa tampilannya]
- Data: [dari mana datanya / disimpan ke mana]
```

### 10.3 Cara Optimasi Performa

📋 **PROMPT Optimasi:**
```
Aplikasi terasa lambat saat [situasi tertentu].
Tolong analisis dan optimalkan:
- Apakah ada call API yang tidak perlu?
- Apakah widget rebuild terlalu sering?
- Apakah ada yang bisa di-cache?
```

### 10.4 Cara Tambah Validasi

📋 **PROMPT Validasi:**
```
Di form [nama form], tambahkan validasi:
- [Field X]: [aturan validasi]
- [Field Y]: [aturan validasi]
Tampilkan pesan error yang ramah pengguna dalam Bahasa Indonesia.
```

---

## BAGIAN 11: Troubleshooting via Prompting

> 💡 **Prinsip utama:** Jika ada error, **paste error message langsung ke chat** Antigravity IDE. AI akan mendiagnosis dan memperbaikinya.

### 11.1 Error Saat Build

📋 **PROMPT Error Build:**
```
Saya mendapat error saat menjalankan flutter run:
[paste seluruh error message di sini]

Tolong:
1. Jelaskan apa penyebab error ini
2. Perbaiki kode yang bermasalah
3. Jelaskan cara mencegah error ini di masa depan
```

### 11.2 Error Koneksi Supabase

📋 **PROMPT Error Supabase:**
```
Aplikasi saya tidak bisa terhubung ke Supabase.
Error yang muncul: [paste error]

Hal yang sudah saya cek:
- URL Supabase: sudah benar
- Anon key: sudah benar
- Internet: ada koneksi

Tolong cek apakah:
1. Permission internet sudah ada di AndroidManifest.xml
2. Inisialisasi Supabase di main.dart sudah benar
3. Ada masalah lain yang mungkin
```

### 11.3 Data Tidak Muncul

📋 **PROMPT Data Kosong:**
```
Halaman daftar data saya kosong padahal di Supabase sudah ada data.
Tolong debug fungsi getAllItems() di DataService:
1. Tambahkan print/log untuk melihat response dari Supabase
2. Cek apakah query sudah benar
3. Cek apakah ada filter yang salah (misalnya filter user_id)
```

### 11.4 HP Tidak Terdeteksi

📋 **PROMPT HP Tidak Terdeteksi:**
```
Saya jalankan adb devices tapi HP saya tidak muncul.
HP saya: [merk dan model HP]
OS HP: Android [versi]

Tolong berikan langkah troubleshooting untuk:
1. Cek apakah USB Debugging sudah aktif
2. Install driver yang diperlukan
3. Solusi alternatif jika masih bermasalah
```

---

## 📊 Ringkasan: Alur Vibe Coding

```
MULAI
  │
  ▼
Setup Environment (manual, 1x saja)
  │
  ▼
Buka Antigravity IDE + Sidebar Chat
  │
  ▼
Prompt: "Buatkan [fitur/halaman] dengan [spesifikasi]"
  │
  ▼
AI Generate Kode ──→ Review sebentar
  │                      │
  ▼                      ▼ (Ada yang kurang?)
Accept Changes      Prompt lagi untuk perbaikan
  │
  ▼
flutter run → Test di HP / Browser
  │
  ▼ (Ada error?)
Paste error ke chat → AI perbaiki → Test lagi
  │
  ▼ (Semua OK)
Prompt: "Tambahkan fitur selanjutnya..."
  │
  ▼
SELESAI: Aplikasi jadi! 🎉
```

---

## 📝 Daftar Prompt Siap Pakai (Quick Reference)

| # | Kebutuhan | Prompt Singkat |
|---|---|---|
| 1 | Buat halaman baru | "Buatkan [nama].dart dengan [spesifikasi UI]" |
| 2 | Ambil data dari Supabase | "Sambungkan [halaman] ke tabel [nama] di Supabase" |
| 3 | Tambah data ke Supabase | "Buat fungsi insert ke tabel [nama]" |
| 4 | Edit data | "Tambah fungsi update di tabel [nama] berdasarkan id" |
| 5 | Hapus data | "Tambah konfirmasi hapus dan delete di tabel [nama]" |
| 6 | Login/Register | "Implementasi login menggunakan Supabase Auth" |
| 7 | Validasi form | "Tambah validasi: [aturan] di form [nama]" |
| 8 | Perbaiki tampilan | "Ubah [elemen] menjadi [deskripsi]" |
| 9 | Debug error | "Ada error: [paste error]. Tolong perbaiki." |
| 10 | Tambah fitur | "Tambahkan fitur [nama] ke halaman [nama]" |

---

## 🎯 Checklist Penyelesaian Modul

### Setup
- [ ] Flutter doctor menampilkan Android toolchain ✅
- [ ] Akun Supabase sudah dibuat
- [ ] Tabel `items` sudah dibuat di Supabase
- [ ] Project Flutter berhasil dibuat

### UI via Vibe Coding
- [ ] Splash Screen selesai
- [ ] Halaman Login selesai
- [ ] Halaman Register selesai
- [ ] Halaman Dashboard/Home selesai
- [ ] Halaman Daftar Data selesai
- [ ] Halaman Tambah Data selesai
- [ ] Halaman Edit Data selesai

### Koneksi Supabase
- [ ] Supabase berhasil diinisialisasi
- [ ] Data berhasil ditampilkan dari Supabase
- [ ] Tambah data berfungsi
- [ ] Edit data berfungsi
- [ ] Hapus data berfungsi
- [ ] Login/Register berfungsi

### Testing
- [ ] App berjalan di HP Android tanpa error
- [ ] App berjalan di Browser Chrome
- [ ] Semua fitur CRUD berfungsi
- [ ] Login dan logout berfungsi

---

*Modul Vibe Coding — Flutter + Supabase + Antigravity IDE*  
*Untuk Profesional Non-IT yang Ingin Membuat Aplikasi Sendiri*  
*September 2026*
