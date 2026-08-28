# 📘 prdsupabase.md
## Laboratorium Supabase: Database, RLS, Auth & Realtime
### Product Requirements Document (PRD) — Modul Praktikum Vibe Coding

---

```
Versi    : v1.0.0 (LOCKED — Final)
Dibuat   : 2026-08-27
Pengajar : Ika Suhasmi
Platform : Antigravity IDE (Google AGY)
Stack    : Vite + React 18 + TypeScript + Supabase
Audiens  : Semua level — pemula s/d menengah
```

> **Catatan:** Dokumen ini bersifat **final dan locked** pada versi ini.
> Perubahan konten diterbitkan sebagai versi baru (v1.1.0, v2.0.0, dst).
> Cocok sebagai: (1) modul mandiri siswa, (2) bahan ajar kelas, (3) referensi permanen.

---

## Daftar Isi

| Lab | Topik |
|-----|-------|
| LAB 01 | SQL Schema & DDL |
| LAB 02 | Row Level Security (RLS) |
| LAB 03 | .env Setup & Supabase Client Singleton |
| LAB 04 | AuthContext & Route Guard |
| LAB 05 | Realtime Subscription |
| LAB 06 | CRUD Operations |
| LAB 07 | Storage (Upload File & Foto Profil) |
| LAB 08 | Edge Functions |
| LAB 09 | Deploy & Environment Variables |

---

## Arsitektur Sistem: Bagaimana Semua Lab Terhubung

```
APLIKASI REACT (Frontend)
  AuthContext (LAB 04) + Route Guard (LAB 04) + Realtime Hook (LAB 05)
                |
                | Supabase Client Singleton (LAB 03)
                |
        SUPABASE CLOUD
          Auth           (LAB 04) — JWT Token
          PostgreSQL DB  (LAB 01) — Schema & DDL
          RLS Policies   (LAB 02) — Keamanan per baris
          CRUD API       (LAB 06) — Baca/tulis data
          Storage Bucket (LAB 07) — File & gambar
          Edge Functions (LAB 08) — Serverless logic
          Realtime Engine(LAB 05) — WebSocket push
                |
        VERCEL / NETLIFY (LAB 09) — Deploy production
```

> **Insight:** Auth memberi identitas -> RLS menjaga privasi -> Database menyimpan ->
> Realtime menyiarkan -> Storage menyimpan file -> Edge Functions jalankan logika bisnis.

---

## Prerequisite

- [ ] Akun Supabase gratis di supabase.com
- [ ] Node.js v18+ terinstall
- [ ] Antigravity IDE / Google AGY terinstall dan login
- [ ] Akun GitHub (untuk Lab 09)

**Untuk Stack Lain:**
- Next.js: Konsep sama, ikuti konvensi App Router
- Vanilla JS: Hapus React hooks, pakai supabase-js langsung
- Vue.js: Ganti useEffect/useState dengan onMounted/ref dari Composition API

---

## LAB 01 — SQL Schema & DDL

### A. Teori Fundamental

**Database** adalah tempat penyimpanan data terstruktur dan permanen. Data tetap ada meskipun
server dimatikan atau browser ditutup.

**PostgreSQL** adalah mesin database relasional open-source yang digunakan Instagram, Spotify,
GitHub, dan Netflix. Supabase menggunakan PostgreSQL sebagai fondasinya.

#### Analogi: Minimarket dengan Sistem Gudang

```
GUDANG MINIMARKET = DATABASE
  RAK A = Tabel "profiles"    -> id, name, email
  RAK B = Tabel "categories"  -> id, name, color
  RAK C = Tabel "transactions"-> id, amount, user_id

Setiap "rak"        = TABEL
Setiap "baris produk" = RECORD (baris data)
Setiap "label (nama, harga, stok)" = KOLOM (field)
```

#### Konsep Kunci DDL

| Konsep | Definisi Sederhana | Analogi |
|--------|-------------------|---------|
| TABLE | Tempat simpan satu jenis data | Rak di gudang |
| PRIMARY KEY | Nomor unik setiap baris | Barcode produk |
| FOREIGN KEY | Kolom yang menunjuk ke tabel lain | Kode supplier di produk |
| TRIGGER | Fungsi otomatis saat event terjadi | Satpam otomatis beri kartu anggota |
| INDEX | Daftar pencarian cepat | Daftar isi buku |

#### Diagram Relasi Antar Tabel

```
auth.users (dikelola Supabase)
    |
    | TRIGGER: handle_new_user() — otomatis saat user sign-up
    v
profiles (id UUID = auth.users.id, full_name, avatar_url, created_at)
    |
    | user_id (FOREIGN KEY)
    v
transactions --- categories
  id UUID          id UUID
  user_id -> profiles.id
  category_id -> categories.id
  amount, type, description, date
```

#### Perbandingan: Dengan vs Tanpa DDL Terstruktur

| Aspek | Tanpa DDL | Dengan DDL + Trigger + Index |
|-------|-----------|------------------------------|
| Konsistensi data | Bisa tidak sinkron | Terjamin via trigger |
| Kecepatan query | Lambat untuk data besar | Cepat karena index |
| Keamanan data | Rentan orphaned data | Foreign key mencegah data yatim |

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap (Pengajar / Demo Kelas)

```
[ROLE]: Supabase Database Architect & PostgreSQL Security Specialist

[CONTEXT]:
Stack: Vite + React 18 (TypeScript) + @supabase/supabase-js v2.
Aplikasi: Manajemen keuangan pribadi multi-user ("FinTrack").

[TASK]:
Buatkan script SQL DDL lengkap untuk Supabase:

1. TABEL profiles:
   - id: UUID (referensi auth.users, cascade delete)
   - full_name: text nullable, avatar_url: text nullable
   - created_at, updated_at: timestamptz default now()

2. TABEL categories:
   - id: UUID primary key gen_random_uuid()
   - user_id: UUID -> profiles
   - name: text not null, color: text (hex), icon: text
   - type: enum('income','expense'), created_at: timestamptz

3. TABEL transactions:
   - id: UUID primary key gen_random_uuid()
   - user_id: UUID -> profiles (cascade delete)
   - category_id: UUID -> categories (set null on delete)
   - amount: numeric(12,2) check > 0, type: enum('income','expense')
   - description: text nullable, date: date not null
   - created_at, updated_at: timestamptz

4. TRIGGER handle_new_user():
   - Jalankan SETELAH INSERT pada auth.users
   - Otomatis buat baris di public.profiles

5. INDEX performa pada:
   - transactions.user_id
   - transactions.date (DESC)
   - categories.user_id

[CONSTRAINTS]:
- UUID (bukan integer) untuk semua primary key
- Semua timestamp: timestamptz (timezone-aware)
- Script idempotent (IF NOT EXISTS)
- Komentar penjelasan di setiap blok SQL
- Siap dijalankan di Supabase SQL Editor

[OUTPUT]: Script SQL siap copy-paste ke Supabase SQL Editor.
```

#### Template Prompt untuk Siswa (Adaptasi Sendiri)

```
[ROLE]: Supabase Database Architect

[CONTEXT]:
Stack: [SEBUTKAN STACK KAMU]
Aplikasi: [NAMA APLIKASI] — [DESKRIPSI SINGKAT]

[TASK]:
Buatkan SQL DDL untuk tabel: [DAFTAR TABEL YANG KAMU BUTUHKAN]

Sertakan:
- Trigger otomatis untuk profil user baru
- Index pada kolom yang sering di-query
- Komentar penjelasan di setiap blok
- Script idempotent (aman dijalankan ulang)
```

---

### C. Checklist Verifikasi Lab 01

- [ ] Table Editor: 3 tabel (profiles, categories, transactions) muncul
- [ ] Kolom id setiap tabel bertipe uuid
- [ ] Trigger handle_new_user muncul di Database -> Triggers
- [ ] Buat user test -> baris baru otomatis muncul di profiles
- [ ] Index muncul di Database -> Indexes
- [ ] SELECT * FROM profiles LIMIT 5; tidak error di SQL Editor

---

## LAB 02 — Row Level Security (RLS)

### A. Teori Fundamental

**Row Level Security (RLS)** adalah fitur PostgreSQL yang menentukan **siapa boleh melihat/
mengubah baris mana** langsung di level database — bukan hanya di kode aplikasi.

#### Analogi: Brankas Bank yang Cerdas

```
TANPA RLS — Brankas terbuka:
  SELECT * FROM transactions -> mengembalikan SEMUA baris semua user
  User A BISA melihat data User B! <- SANGAT BERBAHAYA

DENGAN RLS — Brankas pribadi per nasabah:
  User Ani query: SELECT * FROM transactions
  PostgreSQL OTOMATIS tambahkan: WHERE user_id = auth.uid()
  -> Hanya data Ani yang kembali. Data Budi tidak terlihat. [OK]
```

#### Mengapa RLS Lebih Kuat dari Filter di Kode?

```
[X] Filter hanya di kode (tidak aman):
    Developer lupa tambahkan filter -> semua data bocor ke semua user

[V] RLS di database (aman berlapis):
    Bahkan jika developer lupa filter di kode,
    PostgreSQL tetap memfilter otomatis -> tidak ada celah
```

#### Jenis Policy RLS

| Policy | SQL Syntax | Pertanyaan yang Dijawab |
|--------|-----------|------------------------|
| SELECT | USING (user_id = auth.uid()) | Baris mana yang boleh DIBACA? |
| INSERT | WITH CHECK (user_id = auth.uid()) | Data apa yang boleh DITAMBAHKAN? |
| UPDATE | USING + WITH CHECK | Baris mana yang boleh DIUBAH? |
| DELETE | USING (user_id = auth.uid()) | Baris mana yang boleh DIHAPUS? |

#### Diagram Alur: Bagaimana RLS Memproses Request

```
Browser: SELECT * FROM transactions + JWT Token di header
    |
    v Supabase API Layer
    Dekode JWT -> user_id = "uuid-user"
    |
    v PostgreSQL + RLS Engine
    Cek: RLS aktif? -> YA
    Terapkan: WHERE user_id = 'uuid-user' (otomatis, tersembunyi)
    |
    v Hasil
    Hanya baris milik user yang login dikembalikan [OK]
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap (Pengajar / Demo Kelas)

```
[ROLE]: Supabase Security Specialist & PostgreSQL DBA

[CONTEXT]:
Aplikasi: FinTrack (manajemen keuangan multi-user)
Tabel dari LAB 01: profiles, categories, transactions

[TASK]:
Buatkan script SQL RLS lengkap:

1. Tabel profiles:
   - Aktifkan RLS
   - SELECT: hanya profil sendiri
   - UPDATE: hanya profil sendiri
   - INSERT: DILARANG dari client (dibuat via trigger)
   - DELETE: DILARANG

2. Tabel categories:
   - Aktifkan RLS
   - SELECT/INSERT/UPDATE/DELETE: hanya kategori milik sendiri
   - INSERT: user_id harus = auth.uid()

3. Tabel transactions:
   - Aktifkan RLS
   - SELECT/INSERT/UPDATE/DELETE: hanya transaksi milik sendiri

[CONSTRAINTS]:
- Gunakan auth.uid() sebagai identifier
- Nama policy deskriptif (contoh: "profiles_select_own")
- Script idempotent (DROP POLICY IF EXISTS sebelum CREATE)
- Sertakan query verifikasi pg_policies di akhir

[OUTPUT]: Script SQL + tabel ringkasan policies yang dibuat
```

#### Template Prompt untuk Siswa

```
[ROLE]: Supabase Security Specialist

[CONTEXT]:
Aplikasi: [NAMA APLIKASI]
Tabel yang perlu dilindungi: [DAFTAR TABEL]

[TASK]:
Buat script SQL RLS:
Tabel [NAMA]: SELECT [siapa?], INSERT [siapa?], UPDATE [siapa?], DELETE [siapa?]

[CONSTRAINTS]:
- Gunakan auth.uid() sebagai identifier
- Script idempotent, nama policy deskriptif
```

---

### C. Checklist Verifikasi Lab 02

- [ ] Supabase -> Auth -> Policies: ketiga tabel menampilkan "RLS Enabled"
- [ ] Setiap tabel punya minimal 3-4 policies
- [ ] Test tanpa login: SELECT * FROM transactions -> 0 baris (bukan error)
- [ ] Test dengan login: hanya data milik user login yang muncul
- [ ] Test silang: User B tidak bisa lihat data User A
- [ ] SELECT * FROM pg_policies WHERE tablename = 'transactions'; berhasil

> **PERINGATAN:** Jangan pernah nonaktifkan RLS di production.
> Untuk akses admin, gunakan Service Role Key di backend saja.

---

## LAB 03 — .env Setup & Supabase Client Singleton

### A. Teori Fundamental

**Environment Variables (.env)** adalah variabel konfigurasi tersimpan di luar kode
agar API keys tidak terunggah ke GitHub.

#### Analogi: Kunci yang Tidak Ditulis di Tembok

```
[X] SALAH — Hard-coded:
  const supabase = createClient('https://abc.supabase.co', 'eyJ...')
  -> Semua orang yang akses GitHub bisa lihat kunci ini!

[V] BENAR — Dari environment variable:
  const supabase = createClient(
    import.meta.env.VITE_SUPABASE_URL,
    import.meta.env.VITE_SUPABASE_ANON_KEY
  )
```

#### Anon Key vs Service Role Key

| | Anon Key | Service Role Key |
|--|----------|-----------------|
| Digunakan di | Frontend (browser) | Backend / Edge Function SAJA |
| RLS berlaku? | Ya | Tidak (bypass semua RLS) |
| Jika bocor | Risiko rendah | SANGAT BERBAHAYA |

#### Singleton Pattern

```
[X] Tanpa Singleton (boros):
  CompA.tsx: const supabase = createClient(...) <- Koneksi #1
  CompB.tsx: const supabase = createClient(...) <- Koneksi #2 (duplikat!)

[V] Dengan Singleton (efisien):
  src/lib/supabase.ts: export const supabase = createClient(...) <- Dibuat SEKALI
  CompA.tsx: import { supabase } from '../lib/supabase'  <- Pakai yang sama
  CompB.tsx: import { supabase } from '../lib/supabase'  <- Pakai yang sama
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: Full-Stack TypeScript Developer & Supabase Integration Specialist

[CONTEXT]:
Project: fintrack — Vite + React 18 + TypeScript

[TASK]:
Setup awal Supabase di project ini:

1. Install: @supabase/supabase-js (versi terbaru)

2. Buat .env.local dengan:
   VITE_SUPABASE_URL=...
   VITE_SUPABASE_ANON_KEY=...
   (Sertakan komentar cara mendapatkan nilai dari Supabase Dashboard)

3. Update .gitignore: tambahkan .env.local

4. Buat src/lib/supabase.ts:
   - Singleton pattern
   - TypeScript strict (tidak ada 'any')
   - Throw error jika env variable tidak ada
   - Export named 'supabase' client

5. Buat src/lib/database.types.ts:
   - TypeScript types untuk tabel: profiles, categories, transactions

[CONSTRAINTS]:
- Gunakan import.meta.env (bukan process.env) karena Vite
- Sertakan cara generate types dengan Supabase CLI

[OUTPUT]: Setiap file lengkap dengan nama file sebagai header.
```

#### Template Prompt untuk Siswa

```
[ROLE]: Supabase Integration Developer

[CONTEXT]:
Project: [NAMA PROJECT] — [STACK]

[TASK]:
Setup Supabase:
1. Install @supabase/supabase-js
2. Buat .env.local dengan variabel yang benar untuk [STACK]
3. Buat Supabase client singleton
4. Pastikan .env.local di .gitignore

Tabel saya: [SEBUTKAN TABEL]
```

---

### C. Checklist Verifikasi Lab 03

- [ ] .env.local ada dengan nilai nyata dari Supabase Dashboard
- [ ] .gitignore mengandung .env.local
- [ ] src/lib/supabase.ts berhasil tanpa error TypeScript
- [ ] npm run dev berjalan tanpa error env variable
- [ ] git status: .env.local TIDAK muncul sebagai file yang akan di-commit

---

## LAB 04 — AuthContext & Route Guard

### A. Teori Fundamental

| Konsep | Definisi | Contoh |
|--------|----------|--------|
| Authentication | Verifikasi SIAPA kamu | Login email+password |
| Authorization | Apa yang BOLEH kamu lakukan | User tidak bisa hapus data user lain |

#### Analogi: Hotel dengan Kartu Kunci

```
AUTHENTICATION — Resepsionis (Supabase Auth):
  Verifikasi KTP -> Beri KARTU KUNCI (JWT Token)
  "Kartu valid 24 jam" = token expiry

AUTHORIZATION — Sensor Pintu (Route Guard + RLS):
  Kamar 301 -> Kartu sesuai -> Pintu terbuka  [OK]
  Dapur Hotel -> Tidak diizinkan -> Ditolak   [X]
```

#### AuthContext: Menghindari Prop Drilling

```
Tanpa Context (prop drilling - menyiksa):
  App -> Layout(user) -> Navbar(user) -> Avatar(user) <- baru dipakai

Dengan Context (bersih):
  App dibungkus AuthProvider
  Avatar langsung: useAuth() -> dapat user [OK]
```

#### Diagram: Lifecycle onAuthStateChange

```
App start -> AuthProvider mount
  -> supabase.auth.getSession()             <- restore session dari browser
  -> supabase.auth.onAuthStateChange(cb)   <- listener aktif terus

Event login  -> cb dipanggil, user = session.user
Event logout -> cb dipanggil, user = null
Token expired -> auto refresh atau logout
```

#### Route Guard

```
User akses /dashboard
  -> loading? -> tampilkan Spinner
  -> user ada (login)? -> render Dashboard [OK]
  -> user null (belum login)? -> redirect ke /login
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: React 18 Expert & Supabase Auth Integration Specialist

[CONTEXT]:
Project: FinTrack — Vite + React 18 + TypeScript + React Router v6
Supabase client: src/lib/supabase.ts (dari LAB 03)

[TASK]:
Implementasikan sistem autentikasi lengkap:

1. src/context/AuthContext.tsx:
   - State: user (User|null), session (Session|null), loading (boolean)
   - Mount: supabase.auth.getSession() untuk restore session
   - Setup onAuthStateChange listener
   - WAJIB: cleanup di useEffect return (cegah memory leak)
   - Export: AuthProvider + useAuth() hook

2. src/components/auth/LoginPage.tsx:
   - Form: email & password
   - Tombol "Login dengan Google" (OAuth)
   - Handle error dengan pesan ramah user
   - Loading state saat proses login
   - Redirect ke /dashboard setelah berhasil

3. src/components/auth/RegisterPage.tsx:
   - Form: email, password, confirm password
   - Validasi: password min 8 karakter
   - Setelah daftar: "Cek email Anda untuk verifikasi"

4. src/components/ProtectedRoute.tsx:
   - Loading -> spinner
   - User null -> Navigate ke /login
   - User ada -> render children

5. Update App.tsx:
   - Bungkus dengan AuthProvider
   - Routing: /login, /register, /dashboard (protected)

[CONSTRAINTS]:
- TypeScript strict (tidak ada 'as any')
- Semua useEffect HARUS ada cleanup function
- Gunakan Supabase v2 API

[OUTPUT]: Setiap file lengkap berurutan dari yang paling fundamental.
```

#### Template Prompt untuk Siswa

```
[ROLE]: React Auth Developer

[CONTEXT]:
Project: [NAMA PROJECT] — [STACK]
Supabase client: [PATH]

[TASK]:
Implementasikan login/logout Supabase Auth:
1. AuthContext menyimpan status login
2. Halaman Login: [METODE: email+password / Google / keduanya]
3. Route Guard untuk: [SEBUTKAN HALAMAN PRIVATE]

Setelah login -> arahkan ke: [HALAMAN TUJUAN]
Belum login + akses private -> arahkan ke: [HALAMAN LOGIN]
```

---

### C. Checklist Verifikasi Lab 04

- [ ] Test Login Email: daftar -> konfirmasi email -> login berhasil
- [ ] Test Login Google: OAuth berhasil -> redirect ke dashboard
- [ ] Test Route Guard: /dashboard tanpa login -> redirect ke /login
- [ ] Test Persist Session: login -> tutup browser -> buka lagi -> masih login
- [ ] Test Logout: redirect ke /login, session bersih
- [ ] Supabase -> Auth -> Users: user yang daftar muncul di sana

---

## LAB 05 — Realtime Subscription

### A. Teori Fundamental

Aplikasi biasa: Request -> Response -> selesai. Data berubah di server = browser tidak tahu
sampai refresh.

**Realtime (WebSocket)**: Koneksi dua arah selalu terbuka. Server bisa "push" data baru ke
browser kapanpun.

#### Analogi: DVD vs Siaran TV Langsung

```
HTTP (biasa) = DVD Player:
  Tekan Play -> selesai -> mau update? harus beli DVD baru (refresh halaman)

WebSocket (Realtime) = Siaran TV:
  TV menyala = koneksi terbuka terus
  Siaran terbaru langsung muncul tanpa "refresh" apapun
```

#### Bahaya: Memory Leak Tanpa Cleanup

```
[X] BERBAHAYA — Tanpa cleanup:
useEffect(() => {
  const channel = supabase.channel('tx').subscribe()
  // Tidak ada return! -> koneksi tidak pernah ditutup
  // Setiap navigasi menambah koneksi baru -> crash!
}, [])

[V] BENAR — Dengan cleanup:
useEffect(() => {
  const channel = supabase.channel('tx').subscribe()
  return () => {
    supabase.removeChannel(channel)  <- WAJIB!
  }
}, [])
```

#### Diagram: Siklus Hidup Realtime

```
Komponen mount -> useEffect berjalan
  -> Buat channel + subscribe() = WebSocket terbuka

INSERT ke tabel transactions
  -> Supabase push event via WebSocket
  -> callback(payload) dipanggil di browser
  -> setTransactions(prev => [payload.new, ...prev])
  -> React re-render -> UI terupdate [OK]

Komponen unmount -> cleanup:
  -> supabase.removeChannel(channel) = WebSocket ditutup rapi [OK]
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: React Performance Engineer & Supabase Realtime Specialist

[CONTEXT]:
Project: FinTrack — Vite + React 18 + TypeScript
Auth: useAuth() tersedia, Tabel: transactions (RLS aktif dari LAB 02)

[TASK]:
Implementasi Realtime Subscription yang aman (zero memory leak):

1. src/hooks/useRealtimeTransactions.ts:
   - Props: userId (string)
   - Return: { transactions, loading, error }
   - Logic:
     a. Fetch initial data saat mount
     b. Setup realtime channel tabel transactions
     c. INSERT -> tambahkan ke state (tanpa refetch)
     d. UPDATE -> update item yang sesuai di state
     e. DELETE -> hapus item dari state
     f. WAJIB: cleanup -> supabase.removeChannel(channel)

2. src/components/TransactionList.tsx:
   - Gunakan useRealtimeTransactions(user.id)
   - Loading skeleton saat loading
   - Badge "LIVE" untuk tunjukkan koneksi aktif

[CONSTRAINTS]:
- WAJIB cleanup di setiap useEffect yang buka koneksi
- Jangan gunakan polling (setInterval) — WebSocket murni
- Handle edge case: userId berubah (logout + login ulang)

[OUTPUT]: Kedua file lengkap + catatan "Cara test: buka 2 tab browser"
```

#### Template Prompt untuk Siswa

```
[ROLE]: Supabase Realtime Developer

[CONTEXT]:
Project: [NAMA PROJECT]
Tabel yang di-listen: [NAMA TABEL]

[TASK]:
Buat hook useRealtime[Nama](userId) yang:
1. Ambil data awal dari tabel [NAMA TABEL]
2. WebSocket listener event: [INSERT / UPDATE / DELETE]
3. Update state saat ada perubahan
4. WAJIB: cleanup WebSocket saat komponen unmount
```

---

### C. Checklist Verifikasi Lab 05

- [ ] 2 tab browser: tambah data Tab A -> muncul Tab B < 1 detik tanpa refresh
- [ ] DevTools -> Network -> WS: ada 1 koneksi WebSocket aktif ke Supabase
- [ ] Navigasi masuk-keluar 5x -> tidak ada memory leak (cek DevTools Memory)
- [ ] User A tidak dapat update dari aktivitas User B (RLS berfungsi)

---

## LAB 06 — CRUD Operations

### A. Teori Fundamental

**CRUD** = 4 operasi dasar pada data:

| Huruf | Operasi | SQL | Supabase JS |
|-------|---------|-----|------------|
| C | Create | INSERT | .insert({}) |
| R | Read | SELECT | .select() |
| U | Update | UPDATE | .update({}).eq() |
| D | Delete | DELETE | .delete().eq() |

#### Query Builder Pattern (Method Chaining)

```typescript
// "Dari transactions, ambil data + kategori, milik saya, Agustus 2026, terbaru dulu, 20 data"
const { data, error } = await supabase
  .from('transactions')
  .select('*, categories(name, color)')
  .eq('user_id', user.id)
  .gte('date', '2026-08-01')
  .order('date', { ascending: false })
  .limit(20)
```

#### Error Handling yang Benar

```typescript
// [X] SALAH — Tidak handle error:
const { data } = await supabase.from('transactions').select()
// Error -> data = null -> aplikasi crash diam-diam

// [V] BENAR — Selalu cek error:
const { data, error } = await supabase.from('transactions').select()
if (error) {
  setError('Gagal memuat transaksi. Silakan refresh.')
  return
}
setTransactions(data)
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: React Full-Stack Developer & Supabase CRUD Specialist

[CONTEXT]:
Project: FinTrack — Vite + React 18 + TypeScript
Auth: useAuth() tersedia
Tabel: transactions, categories (LAB 01 schema, LAB 02 RLS)

[TASK]:
Implementasikan CRUD untuk modul Transaksi:

1. src/hooks/useTransactions.ts:
   - fetchTransactions(month, year): filter bulan+tahun, sort date desc
   - addTransaction(data): dengan optimistic update
   - updateTransaction(id, data)
   - deleteTransaction(id): konfirmasi sebelum hapus
   - State: transactions[], loading, error per-operasi

2. src/components/transactions/TransactionForm.tsx:
   - Field: amount, type (income/expense), category, description, date
   - Dropdown categories dari database
   - Validasi: amount > 0, tanggal tidak future
   - Reset form setelah submit

3. src/components/transactions/TransactionItem.tsx:
   - Tampilkan: tanggal, deskripsi, kategori, amount
   - Income: hijau (+), Expense: merah (-)
   - Tombol Edit dan Delete

[CONSTRAINTS]:
- TypeScript generics untuk semua return types
- Optimistic update untuk UX smooth
- Format currency: Intl.NumberFormat untuk Rupiah (Rp 1.250.000)

[OUTPUT]: Setiap file lengkap + komentar "// WHY:" di setiap keputusan penting
```

#### Template Prompt untuk Siswa

```
[ROLE]: Supabase CRUD Developer

[CONTEXT]:
Project: [NAMA PROJECT]
Tabel utama: [NAMA TABEL], Kolom: [SEBUTKAN KOLOM]

[TASK]:
Buat hook useCRUD[Nama]() dengan:
1. Baca data (filter: [SEBUTKAN FILTER])
2. Tambah data baru
3. Update berdasarkan id
4. Hapus berdasarkan id

Komponen form dengan field: [SEBUTKAN FIELD]
```

---

### C. Checklist Verifikasi Lab 06

- [ ] CREATE: isi form -> submit -> data muncul di list tanpa refresh
- [ ] READ: hanya data milik user login (RLS berfungsi)
- [ ] UPDATE: edit amount -> simpan -> nilai baru tampil
- [ ] DELETE: hapus -> konfirmasi -> item hilang
- [ ] Validasi: amount = 0 -> muncul pesan error
- [ ] Format Rupiah: Rp 1.250.000 (bukan 1250000)

---

## LAB 07 — Storage (Upload File & Foto Profil)

### A. Teori Fundamental

**Supabase Storage** adalah layanan penyimpanan file (gambar, dokumen, video) terintegrasi
dengan Auth dan RLS Supabase.

#### Analogi: Lemari Dokumen dengan Kunci Personal

```
STORAGE BUCKET = Lemari Arsip
  "avatars" bucket:
    user-uuid-123/avatar.jpg  <- milik User A
    user-uuid-456/avatar.jpg  <- milik User B

Policy: User HANYA bisa upload ke folder nama user-id mereka sendiri
```

#### Public vs Private Bucket

| | Public Bucket | Private Bucket |
|--|--------------|----------------|
| Akses URL | Langsung via URL publik | Butuh signed URL (batas waktu) |
| Cocok untuk | Foto profil, asset publik | Dokumen pribadi sensitif |

#### Alur Upload File

```
Pilih file -> Validasi client-side (ukuran max, tipe file)
  -> Buat unique filename: userId/Date.now().ext
  -> supabase.storage.from('avatars').upload(path, file, {upsert: true})
  -> Dapatkan public URL: .getPublicUrl(path)
  -> Update profiles.avatar_url di database
  -> Tampilkan foto baru di UI [OK]
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: Supabase Storage & File Management Specialist

[CONTEXT]:
Project: FinTrack — Vite + React 18 + TypeScript
Auth: useAuth() tersedia, user.id tersedia
profiles sudah ada kolom avatar_url (text, nullable)

[TASK]:
Implementasikan upload foto profil:

1. SQL untuk Storage Bucket & Policy:
   - Bucket: avatars (public)
   - Policy INSERT: upload ke path diawali user.id sendiri
   - Policy SELECT: semua boleh lihat (public bucket)
   - Policy UPDATE/DELETE: hanya pemilik file

2. src/hooks/useAvatarUpload.ts:
   - uploadAvatar(file: File): Promise<string | null>
   - Validasi: max 2MB, tipe image/jpeg | image/png | image/webp
   - Filename: userId/Date.now().ext (unique)
   - Upload upsert: true (timpa foto lama)
   - Update profiles.avatar_url setelah upload
   - State: uploading (boolean), progress (0-100), error

3. src/components/profile/AvatarUpload.tsx:
   - Tampilkan foto saat ini (atau placeholder icon)
   - Klik/drag-drop pilih file baru
   - Preview sebelum upload (FileReader API)
   - Progress bar saat upload berlangsung
   - Tombol konfirmasi dan batal

[CONSTRAINTS]:
- Validasi di client SEBELUM upload ke server
- upsert: true agar foto lama tertimpa (tidak menumpuk)
- Feedback visual jelas: loading, success, error

[OUTPUT]: Kode lengkap + SQL Storage Policy + langkah buat bucket di Dashboard
```

#### Template Prompt untuk Siswa

```
[ROLE]: Supabase Storage Developer

[CONTEXT]:
Project: [NAMA PROJECT]
File yang diupload: [GAMBAR / PDF / VIDEO]
Bucket name: [NAMA BUCKET]

[TASK]:
Implementasikan upload [JENIS FILE]:
1. SQL policy bucket: siapa boleh upload? [DESKRIPSI]. Publik atau private?
2. Hook useUpload[Nama](): validasi max [UKURAN]MB, tipe [TIPE]
3. Komponen UI upload dengan preview dan progress bar
```

---

### C. Checklist Verifikasi Lab 07

- [ ] Bucket 'avatars' muncul di Supabase Dashboard -> Storage
- [ ] Storage Policies terkonfigurasi dengan benar
- [ ] Upload foto < 2MB: preview muncul -> upload berhasil -> foto tampil
- [ ] Foto baru muncul di profil tanpa reload halaman
- [ ] Upload > 2MB: muncul pesan error ukuran file
- [ ] File tersimpan di path: user-uuid/timestamp.jpg di bucket

---

## LAB 08 — Edge Functions

### A. Teori Fundamental

**Edge Functions** adalah fungsi serverless berjalan di server Supabase (bukan di browser),
ditulis dengan Deno runtime.

#### Analogi: Kasir vs Manajer Toko

```
FRONTEND (React) = Kasir:
  [OK] Terima dan tampilkan pesanan
  [X]  Tidak boleh pegang kunci gudang rahasia (service key)
  [X]  Tidak aman untuk logika bisnis sensitif

EDGE FUNCTION = Manajer di Ruang Belakang:
  [OK] Akses layanan premium (Midtrans, email, SMS, dll)
  [OK] Simpan secret key dengan aman (tidak expose ke browser)
  [OK] Jalankan logika yang tidak boleh dimanipulasi user
```

#### Kapan Harus Pakai Edge Function?

| Skenario | Alasan |
|----------|--------|
| Payment Gateway | Secret key Midtrans/Stripe tidak boleh di browser |
| Kirim Email | API key Resend/Sendgrid sensitif |
| Notifikasi Push | Firebase Server Key sangat sensitif |
| Kalkulasi sensitif | Tidak boleh dimanipulasi via DevTools browser |

#### Diagram Alur: Frontend -> Edge Function -> Service Eksternal

```
Browser: POST /functions/v1/monthly-summary + JWT Token
  |
  v Supabase Edge Function (Deno)
  Verifikasi JWT (auto) -> Ambil data user (SERVICE ROLE KEY — aman di server)
  -> Kalkulasi / proses data
  -> Hubungi service eksternal jika perlu (email, payment, dll)
  |
  v Return JSON response ke browser
Browser tampilkan hasil [OK]
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: Supabase Edge Functions Developer & Deno Runtime Specialist

[CONTEXT]:
Project: FinTrack — Vite + React 18 + TypeScript
Supabase CLI sudah terinstall

[TASK]:
Implementasikan Edge Function ringkasan keuangan bulanan:

1. Setup CLI:
   supabase init -> supabase login -> supabase link --project-ref [REF]

2. supabase/functions/monthly-summary/index.ts:
   - Endpoint: POST /functions/v1/monthly-summary
   - Body: { month: number, year: number }
   - Logic:
     a. Verifikasi JWT dari header (otomatis Supabase)
     b. Query semua transaksi bulan tersebut milik user
     c. Hitung: totalIncome, totalExpense, net, count, topCategory
   - Return: JSON ringkasan

3. supabase/functions/monthly-summary/types.ts:
   - TypeScript types untuk request & response

4. src/hooks/useMonthlySummary.ts (frontend):
   - supabase.functions.invoke('monthly-summary', { body: {month, year} })

[CONSTRAINTS]:
- Handle CORS headers agar bisa diakses dari Vite frontend
- Verifikasi JWT di setiap request
- SERVICE ROLE KEY HANYA di Edge Function, TIDAK di frontend

[OUTPUT]: Semua file + perintah CLI + panduan test lokal (supabase functions serve)
```

#### Template Prompt untuk Siswa

```
[ROLE]: Supabase Edge Functions Developer

[CONTEXT]:
Project: [NAMA PROJECT]
Kebutuhan: [JELASKAN, contoh: kirim email saat ada order baru]

[TASK]:
Buat Edge Function: [NAMA-FUNCTION]
1. Terima request dengan: [SEBUTKAN DATA]
2. Lakukan: [JELASKAN LOGIKA]
3. Kembalikan: [FORMAT RESPONSE]

Secret yang dibutuhkan: [NAMA=keterangan]
```

---

### C. Checklist Verifikasi Lab 08

- [ ] supabase --version menampilkan versi CLI
- [ ] Folder supabase/functions/monthly-summary/ ada
- [ ] supabase functions serve -> berjalan di localhost:54321
- [ ] Test curl berhasil mengembalikan JSON ringkasan
- [ ] supabase functions deploy monthly-summary -> berhasil
- [ ] Function muncul di Supabase Dashboard -> Edge Functions

---

## LAB 09 — Deploy & Environment Variables

### A. Teori Fundamental

**Deployment** = proses menerbitkan aplikasi dari laptop ke server publik agar bisa diakses
siapapun di seluruh dunia.

#### Analogi: Dari Dapur ke Restoran

```
DEVELOPMENT (localhost:5173) = Dapur Pribadi:
  Hanya kamu yang bisa akses. Bebas bereksperimen.
  Bahan rahasia (.env.local) di "kulkas pribadi"

PRODUCTION (domain publik) = Restoran Buka:
  Semua customer bisa akses. Harus stabil dan cepat.
  Bahan rahasia tersimpan di "dapur server" (Vercel env vars)
```

#### Mengapa .env.local Tidak Bisa Langsung di Vercel?

```
Laptop    : .env.local ADA
GitHub    : .env.local TIDAK ADA (di .gitignore)
Vercel    : .env.local TIDAK ADA (baca dari GitHub)

SOLUSI: Masukkan env vars di Vercel Dashboard -> Settings -> Environment Variables
  VITE_SUPABASE_URL = https://xxx.supabase.co
  VITE_SUPABASE_ANON_KEY = eyJ...
```

#### Diagram: GitHub -> Vercel -> Domain Publik

```
git push origin main
  -> GitHub Webhook -> Vercel CI/CD Pipeline
  -> npm install -> npm run build (Vite -> /dist)
  -> Inject env vars dari Vercel Dashboard
  -> Deploy ke CDN Vercel (edge nodes global)
  -> https://fintrack.vercel.app [OK]
```

---

### B. Prompt untuk Antigravity IDE

#### Prompt Lengkap

```
[ROLE]: DevOps Engineer & Full-Stack Deployment Specialist

[CONTEXT]:
Project: FinTrack — Vite + React 18 + TypeScript
Repository: ada di GitHub
Target: Vercel (free tier)

[TASK]:
Bantu deploy FinTrack ke Vercel:

1. Persiapan production:
   - Cek TypeScript: tsc --noEmit
   - Buat vercel.json untuk konfigurasi SPA routing React Router

2. Panduan deploy ke Vercel:
   - Connect GitHub repo ke Vercel
   - Konfigurasi Build & Output untuk Vite
   - Daftar semua Environment Variables yang dibutuhkan

3. Konfigurasi Supabase untuk production:
   - Update Site URL: https://fintrack.vercel.app
   - Update OAuth Redirect URLs untuk Google Login

4. Post-deployment checklist semua fitur

[CONSTRAINTS]:
- Free tier Vercel
- Sertakan catatan alternatif untuk Netlify

[OUTPUT]: Panduan langkah-demi-langkah + vercel.json siap pakai
```

#### Template Prompt untuk Siswa

```
[ROLE]: Deployment Guide

[CONTEXT]:
Project: [NAMA PROJECT] — [FRAMEWORK]
GitHub: [URL REPO], Platform target: [Vercel / Netlify]

[TASK]:
Deploy ke [PLATFORM]:
1. File konfigurasi yang dibutuhkan
2. Panduan langkah deploy
3. Semua env vars yang perlu dikonfigurasi
4. Update Supabase untuk domain: [DOMAIN KAMU]

Fitur Supabase yang dipakai: [Auth / Storage / Edge Functions / Realtime]
```

---

### C. Checklist Verifikasi Lab 09

- [ ] npm run build berhasil tanpa error
- [ ] vercel.json ada dengan konfigurasi SPA routing
- [ ] Deploy berhasil -> URL production tampil
- [ ] Test production URL:
  - [ ] Login email+password berhasil
  - [ ] Login Google berhasil
  - [ ] Data tersimpan & terbaca dari Supabase
  - [ ] Realtime berfungsi
  - [ ] Upload file berfungsi
- [ ] /dashboard tanpa login -> redirect ke /login (Route Guard OK)
- [ ] Tidak ada CORS error di browser console production

> **Catatan Netlify:**
> Tambahkan file public/_redirects berisi: /* /index.html 200
> Ini setara fungsi vercel.json untuk Netlify.

---

## Lampiran: Referensi Cepat Supabase JS v2

```typescript
// AUTH
await supabase.auth.signInWithPassword({ email, password })
await supabase.auth.signInWithOAuth({ provider: 'google' })
await supabase.auth.signUp({ email, password })
await supabase.auth.signOut()
const { data: { session } } = await supabase.auth.getSession()
supabase.auth.onAuthStateChange((event, session) => { ... })

// DATABASE — SELECT
const { data, error } = await supabase
  .from('transactions')
  .select('*, categories(name, color)')
  .eq('user_id', user.id)
  .order('date', { ascending: false })
  .limit(20)

// DATABASE — INSERT
const { data, error } = await supabase
  .from('transactions')
  .insert({ user_id: user.id, amount: 50000 })
  .select().single()

// DATABASE — UPDATE
await supabase.from('transactions').update({ amount: 75000 }).eq('id', id)

// DATABASE — DELETE
await supabase.from('transactions').delete().eq('id', id)

// STORAGE
await supabase.storage.from('avatars').upload(path, file, { upsert: true })
const { data } = supabase.storage.from('avatars').getPublicUrl(path)
await supabase.storage.from('avatars').remove([path])

// REALTIME
const channel = supabase.channel('changes')
  .on('postgres_changes', { event: 'INSERT', table: 'transactions' }, callback)
  .subscribe()
supabase.removeChannel(channel)  // <- SELALU cleanup!

// EDGE FUNCTIONS
const { data, error } = await supabase.functions.invoke('function-name', {
  body: { month: 8, year: 2026 }
})
```

### Error Codes yang Sering Ditemui

| Error Code | Arti | Solusi |
|------------|------|--------|
| PGRST116 | Row tidak ditemukan (.single()) | Gunakan .maybeSingle() |
| 42501 | Permission denied — RLS menolak | Cek policy RLS, pastikan user login |
| 23505 | Duplicate key | Cek unique constraint, pakai upsert |
| 23503 | Foreign key violation | Pastikan referenced data ada |
| auth/weak-password | Password terlalu lemah | Minimum 8 karakter |

---

## Tips Mengajar & Troubleshooting

### Urutan Lab yang Direkomendasikan

LAB 01 -> LAB 02 -> LAB 03 -> LAB 04 -> LAB 06 -> LAB 05 -> LAB 07 -> LAB 08 -> LAB 09

(LAB 06 CRUD sebelum LAB 05 Realtime: pahami cara biasa dulu, baru versi realtime-nya)

### Tips Mengajar per Lab

| Lab | Tips |
|-----|------|
| LAB 01 | Demo trigger live di SQL Editor: daftar user -> profil otomatis terbuat |
| LAB 02 | Demo "tanpa RLS" (data bocor) -> aktifkan RLS -> efek dramatis! |
| LAB 03 | Tunjukkan bahaya hard-coded key di git log |
| LAB 04 | Demo: /dashboard tanpa login -> redirect; dengan login -> masuk |
| LAB 05 | Demo 2 tab browser — efek "wow" terbaik untuk presentasi skripsi! |
| LAB 06 | Mulai dari SQL langsung, lalu tunjukkan versi elegant dengan supabase-js |
| LAB 07 | Tunjukkan beda public vs private bucket via URL di browser |
| LAB 08 | Fokus "mengapa tidak bisa di frontend" sebelum masuk ke kode |
| LAB 09 | Lakukan deploy live di kelas — momen paling memotivasi! |

### Troubleshooting Umum

| Masalah | Penyebab | Solusi |
|---------|----------|--------|
| Data tidak muncul setelah login | RLS terlalu ketat | Cek policy di Supabase -> Auth -> Policies |
| "User is not defined" error | useAuth() di luar AuthProvider | Pastikan AuthProvider bungkus seluruh app |
| Realtime tidak update | Channel tidak disubscribe | Cek filter channel, pastikan cleanup() dipanggil |
| Upload gagal 403 | Storage policy salah | Cek path file == user.id |
| Login Google error di production | Redirect URL belum diupdate | Update di Supabase Auth -> URL Configuration |
| CORS error di production | Domain belum dikenal Supabase | Tambahkan domain di Supabase -> API -> CORS |

---

prdsupabase.md | v1.0.0 | LOCKED
Pengajar: Ika Suhasmi | 2026-08-27
Bagian dari Kurikulum Vibe Coding 16 Pertemuan
Platform: Antigravity IDE — AI Coding untuk Pendidikan
