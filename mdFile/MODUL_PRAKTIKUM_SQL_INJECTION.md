# 📘 Modul Praktikum: Simulasi SQL Injection (Mode Rentan vs Mode Aman Supabase)

**Mata Kuliah / Topik:** Keamanan Aplikasi Web & Integrasi Database (OWASP Top 10)  
**Studi Kasus:** Proyek MyOrder - Portal Cek Pesanan Belanja  
**Teknologi:** Node.js, Express.js, Supabase (PostgreSQL), HTML5/CSS3  

---

## 🎯 1. Tujuan Pembelajaran

Setelah menyelesaikan modul praktikum ini, mahasiswa/peserta diharapkan mampu:
1. Memahami konsep dasar kerentanan **SQL Injection (OWASP A03: Injection)** pada aplikasi web.
2. Menganalisis perbedaan mendasar antara **Raw SQL String Concatenation** (Rentan) dan **Parameterized Query / Prepared Statement** (Aman).
3. Melakukan simulasi serangan injeksi boolean (`' OR '1'='1`) dan komentar (`--`) secara etis pada lingkungan *local lab*.
4. Memahami bagaimana **Supabase SDK / PostgreSQL Driver** melindungi aplikasi dari manipulasi query secara *default*.
5. Menggunakan teknik **Prompting AI** untuk mendeteksi celah dan menuliskan kode remediasi (*security patch*).

---

## 🧠 2. Konsep Teori & Perbandingan Arsitektur

### 2.1 Apa itu SQL Injection?
**SQL Injection (SQLi)** adalah celah keamanan di mana penyerang (*attacker*) menyisipkan perintah SQL berbahaya melalui input pengguna (form, search bar, parameter URL) yang kemudian dieksekusi langsung oleh interpreter database.

### 2.2 Mengapa Terjadi Kerentanan?
Kerentanan terjadi ketika pengembang membangun query SQL dengan cara **menggabungkan string secara manual (*string concatenation*)** tanpa melakukan validasi atau *escaping*.

```
[Input Pengguna] ➔ [Digabung Langsung ke Teks Query] ➔ [Struktur Sintaks SQL Rusak] ➔ [Data Bocor]
```

### 2.3 Tabel Komparasi: Mode Rentan vs Mode Aman Supabase

| Aspek | 🔴 Mode Rentan (Raw SQL Concatenation) | 🟢 Mode Aman (Supabase Parameterized) |
| :--- | :--- | :--- |
| **Metode Pembuatan Query** | `SELECT * FROM products WHERE name LIKE '%` + `q` + `%'` | `SELECT * FROM products WHERE name ILIKE $1` |
| **Penanganan Input `' OR '1'='1`** | Dianggap sebagai **perintah kode SQL** tambahan. | Dianggap murni sebagai **data teks (string)** pencarian. |
| **Hasil Eksekusi** | Kondisi `1=1` bernilai `TRUE` ➔ Seluruh tabel bocor. | Mencari teks harfiah `"' OR '1'='1"` ➔ 0 hasil (Aman). |
| **Standar Industri** | ❌ Sangat Berbahaya (Anti-pattern). | ✅ Sesuai standar OWASP & Supabase Default. |

---

## 🗄️ 3. Persiapan Database Supabase

Jika ingin menghubungkan langsung ke cloud Supabase, jalankan skrip berikut pada tab **SQL Editor** di Dashboard Supabase Anda:

```sql
-- 1. Buat Tabel Produk
CREATE TABLE IF NOT EXISTS products (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    category TEXT NOT NULL,
    price NUMERIC NOT NULL,
    rating NUMERIC DEFAULT 4.5,
    review_count INT DEFAULT 0,
    stock INT DEFAULT 10,
    image TEXT,
    description TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. Masukkan Data Sampel Produk
INSERT INTO products (id, name, category, price, rating, review_count, stock, image, description)
VALUES
('PRD-001', 'Wireless Headphones Pro', 'Elektronik', 849000, 4.8, 320, 25, 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=500', 'Headphone nirkabel ANC.'),
('PRD-002', 'Mechanical Gaming Keyboard', 'Komputer', 625000, 4.7, 185, 14, 'https://images.unsplash.com/photo-1587829741301-dc798b83add3?w=500', 'Keyboard mekanikal RGB.'),
('PRD-003', 'Sepatu Sneakers Pria', 'Fashion', 450000, 4.9, 540, 38, 'https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=500', 'Sneakers kasual empuk.'),
('PRD-004', 'Smartwatch Fitness Tracker', 'Elektronik', 799000, 4.6, 210, 19, 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=500', 'Jam tangan pintar OLED.')
ON CONFLICT (id) DO NOTHING;
```

---

## 🧪 4. Langkah Praktikum (Hands-on Lab)

### 4.1 Menjalankan Lingkungan Uji Coba
1. Buka Terminal pada folder proyek `MyOrder`.
2. Jalankan perintah server:
   ```bash
   npm start
   ```
3. Buka browser dan arahkan ke alamat:
   👉 **`http://localhost:3000/products.html`**

---

### 🔴 Skenario 1: Uji Coba Penyerangan (Mode Rentan)

1. Pastikan tombol di kanan atas banner aktif pada **🔴 Mode Rentan (Raw SQL)**.
2. Pada kolom input pencarian, masukkan payload:
   ```text
   ' OR '1'='1
   ```
3. **Observasi & Analisis:**
   - Amati kotak **Live SQL Query Interpreter**:
     ```sql
     SELECT * FROM products WHERE name ILIKE '%' OR '1'='1%' OR category ILIKE '%' OR '1'='1%';
     ```
   - **Pertanyaan Analisis:** Mengapa seluruh 8 produk muncul padahal tidak ada produk bernama `' OR '1'='1`?
   - **Jawaban:** Tanda kutip tunggal (`'`) menutup string pencarian lebih awal, dan operator `OR '1'='1'` menyuntikkan klausa kondisi yang **selalu bernilai BENAR (TRUE)**. Akibatnya database mengabaikan nama barang dan mengembalikan semua baris data.

---

### 🔴 Skenario 2: Uji Coba Injeksi Komentar (Comment-out Attack)

1. Tetap pada **Mode Rentan**.
2. Masukkan payload injeksi komentar SQL:
   ```text
   ' OR 1=1 --
   ```
3. **Observasi & Analisis:**
   - Tanda `--` dalam dialek SQL (PostgreSQL/SQLite/MySQL) berfungsi sebagai **komentar** (*comment*).
   - Seluruh karakter query SQL setelah tanda `--` akan diabaikan oleh database engine, memotong pengecekan logika selanjutnya.

---

### 🟢 Skenario 3: Uji Coba Pertahanan (Mode Aman Supabase)

1. Klik tombol **🟢 Mode Aman (Supabase SDK)**.
2. Masukkan kembali payload yang sama:
   ```text
   ' OR '1'='1
   ```
3. **Observasi & Analisis:**
   - Amati kotak **Live SQL Query Interpreter**:
     ```sql
     SELECT * FROM products WHERE name ILIKE $1 OR category ILIKE $1;
     -- Bound Parameter $1 = "%' OR '1'='1%"
     ```
   - **Hasil:** Muncul status **0 Produk Ditemukan (Aman)**.
   - **Penjelasan:** Mesin database memperlakukan seluruh teks `' OR '1'='1` sebagai parameter literal `$1`, sehingga tidak ada struktur sintaks SQL yang bisa dimanipulasi.

---

## 💻 5. Bedah Kode Backend (`server.js`)

### ❌ Kode Rentan (Vulnerable Code)
```javascript
// JANGAN DITIRU DI APLIKASI PRODUKSI!
app.get('/api/lab/search-vulnerable', (req, res) => {
  const { q } = req.query;
  
  // RENTAN: Menggabungkan string secara langsung dengan ${q}
  const rawQuery = `SELECT * FROM products WHERE name ILIKE '%${q}%' OR category ILIKE '%${q}%';`;
  
  // Eksekusi langsung ke database...
});
```

### ✅ Kode Aman (Secure / Parameterized Code)
```javascript
// STANDAR AMAN (Supabase SDK / PostgreSQL Parameterized)
app.get('/api/lab/search-secure', (req, res) => {
  const { q } = req.query;
  
  // AMAN: Menggunakan placeholder parameter ($1, $2, dst.)
  const secureQuery = `SELECT * FROM products WHERE name ILIKE $1 OR category ILIKE $1;`;
  const params = [`%${q}%`];
  
  // Atau menggunakan Supabase Client SDK bawaan:
  // const { data, error } = await supabase.from('products').select().ilike('name', `%${q}%`);
});
```

---

## 🤖 6. Panduan Prompting AI untuk Keamanan SQL

Gunakan pola *prompting* berikut saat bekerja dengan asisten AI (Vibe-Coding aman):

### 💬 Prompt 1: Audit Kerentanan SQL Injection
> *"Bertindaklah sebagai Senior Application Security Engineer. Tolong audit endpoint Express.js berikut apakah memiliki potensi celah SQL Injection. Jelaskan bagaimana penyerang dapat mengeksploitasinya dan berikan contoh payload-nya."*

### 💬 Prompt 2: Pembuatan Patch Parameterized Query
> *"Ubah query database mentah berikut ke dalam bentuk Parameterized Query / Prepared Statement menggunakan library `@supabase/supabase-js` atau `pg` PostgreSQL agar kebal dari serangan SQL Injection dan sesuai standar OWASP Top 10."*

---

## 📝 7. Lembar Kerja & Evaluasi Mandiri

1. **Jelaskan fungsi tanda kutip tunggal (`'`) pada serangan SQL Injection!**
2. **Apa fungsi tanda `--` dalam sintaks query SQL pada serangan injeksi?**
3. **Mengapa Supabase SDK secara default aman dari serangan SQL Injection tradisional?**
4. **Sebutkan 3 dampak bahaya jika sebuah aplikasi e-commerce memiliki celah SQL Injection!**

---

*Modul Praktikum MyOrder &copy; 2026 - Materi Pembelajaran Keamanan Web & Full-Stack Development.*
