# FormQu - Form Builder

FormQu adalah platform yang mudah digunakan bagi tim untuk membuat formulir kustom dengan cepat, mengatur logika kondisional yang cerdas, dan mengumpulkan file seperti foto dan video di satu tempat.

## ✨ Fitur Utama

- **Form Builder (Drag & Drop)**: Alat drag-and-drop sederhana untuk membuat formulir kustom.
- **Logika Kondisional (Conditional Logic)**: Atur aturan "jika/maka" untuk menampilkan atau menyembunyikan pertanyaan berdasarkan pilihan pengguna.
- **Dropdown Dinamis**: Memuat opsi langsung (live) secara otomatis dari database langsung ke menu dropdown.
- **Upload Media**: Mengumpulkan dan menyimpan foto serta video secara langsung dari formulir dengan mudah.
- **Pusat Pengiriman (Submissions Hub)**: Melihat semua jawaban formulir di dalam format tabel yang rapi.
- **Ramah Perangkat Seluler (Mobile Friendly)**: Berfungsi dengan sempurna di ponsel, tablet, dan komputer desktop.
- **Kustomisasi Tema**: Pilih warna tema dan warna teks sesuai identitas brand Anda.
- **Dua Tema Tampilan**: Pilihan antara tema "Default" (minimalis) dan "Google" (glassmorphism modern).
- **Deskripsi Field Opsional**: Tambahkan teks bantuan di bawah setiap pertanyaan untuk memperjelas instruksi.
- **Mini-Markdown**: Format teks deskripsi dan deskripsi form menggunakan syntax ringkas.

### 📝 Syntax Mini-Markdown

FormQu mendukung format teks sederhana pada deskripsi form dan deskripsi field:

| Syntax | Hasil | Contoh |
|---|---|---|
| `*teks*` | **Teks tebal** | `*Wajib diisi*` |
| `_teks_` | *Teks miring* | `_sesuai KTP_` |
| `~teks~` | ~~Teks coret~~ | `~tidak berlaku~` |
| `` `teks` `` | Kode (klik untuk copy) | `` `INV-001` `` |
| `[Label](url)` | Link yang bisa diklik | `[Hubungi CS](https://wa.me/628123)` |
| Baris baru (Enter) | Paragraf terpisah | Langsung tekan Enter |
| `1. item` | Daftar bernomor | `1. Siapkan CV` |

**Contoh deskripsi:**
```
Masukkan *nama lengkap* sesuai _KTP_.
Hubungi [CS Kami](https://wa.me/628123456) jika ada kendala.

Dokumen yang diperlukan:
1. CV terbaru
2. Foto ukuran 4x6
3. Fotocopy KTP
```

## 🛠️ Tech Stack (Teknologi yang Digunakan)

### Frontend
- **Framework**: Next.js 16 (App Router)
- **Bahasa**: TypeScript
- **Styling**: Tailwind CSS v4 dengan Base UI / Radix primitives
- **Drag & Drop**: `@dnd-kit`
- **Icons**: Lucide React

### Backend
- **Framework**: FastAPI (Python)
- **Database**: MongoDB (MongoDB Atlas)
- **Auth**: JWT (JSON Web Token) kustom
- **File Storage**: Local disk (`backend/public/files`)
- **Tools**: Pydantic, Motor (async MongoDB driver)

## 🏗️ Arsitektur & Database

FormQu menangani form yang scalable dan dapat disesuaikan menggunakan pendekatan berbasis JSON yang dinamis:

- **Schema Form Dinamis**: Formulir dibangun dan disimpan menggunakan dokumen JSON yang fleksibel di MongoDB.
- **Dynamic Renderer**: Frontend mencakup mesin rendering dinamis yang mengurai schema JSON. Secara otomatis menghasilkan komponen React yang tepat, menangani state form, menerapkan aturan validasi, dan mengelola logika kondisional.
- **Pengiriman yang Fleksibel**: Setiap catatan pengiriman disimpan secara dinamis, memetakan setiap `field_id` unik ke nilai yang dijawab pengguna.
- **CSS Variables Injection**: Warna tema dan warna teks di-inject sebagai variabel CSS (`--primary`, `--foreground`) untuk styling dinamis secara real-time.

## 🚀 Cara Menjalankan Proyek

Panduan langkah demi langkah untuk menjalankan kodingan ini di komputer Anda.

### 1. Prasyarat (Wajib diinstal dulu)

| Software | Versi Minimum | Cara Cek |
|---|---|---|
| **Node.js** | v18+ (rekomendasi v20+) | `node -v` |
| **Python** | v3.10+ | `python --version` |
| **pnpm** | v8+ | `pnpm -v` |

> Jika belum punya **pnpm**, instal dengan: `npm install -g pnpm`

Anda juga butuh akun **MongoDB Atlas** (gratis) untuk database cloud. Daftar di [mongodb.com/atlas](https://www.mongodb.com/atlas).

---

### 2. Konfigurasi Environment Variables (.env)

**⚠️ Lakukan langkah ini SEBELUM menjalankan server.**

#### Backend — buat file `backend/.env`:
```env
MONGO_URL="mongodb+srv://USERNAME:PASSWORD@cluster.mongodb.net/?appName=NamaApp"
JWT_SECRET="ganti_dengan_secret_key_bebas_anda"
```
- `MONGO_URL` → Connection string dari MongoDB Atlas (bisa dicopy dari dashboard Atlas > Connect > Drivers).
- `JWT_SECRET` → Kunci rahasia bebas untuk enkripsi token login admin. Bisa diisi apa saja, contoh: `"formqu_secret_123"`.

#### Frontend — buat file `frontend/.env`:
```env
NEXT_PUBLIC_APP_NAME="FormQu"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
NEXT_PUBLIC_API_URL="http://localhost:8000"
```

---

### 3. Jalankan Backend (Terminal 1)

```bash
cd backend

# Buat virtual environment
# Windows (gunakan "py", bukan "python"):
py -m venv venv
venv\Scripts\activate

# Mac/Linux:
python3 -m venv venv
source venv/bin/activate

# Instal dependencies
pip install -r requirements.txt

# Jalankan server
uvicorn src.main:app --reload
```

> **⚠️ Catatan Windows:** Jika `python` tidak dikenali, gunakan `py` sebagai gantinya. Jika `venv` sudah ada dari sebelumnya dan error path, hapus dulu folder `venv` lalu buat ulang.

✅ **Backend berjalan di:** http://localhost:8000  
📄 **Dokumentasi API (Swagger):** http://localhost:8000/docs

---

### 4. Jalankan Frontend (Terminal 2)

Buka terminal **BARU** (jangan tutup terminal backend).

```bash
cd frontend

# Instal dependencies
pnpm install

# Jalankan server
pnpm run dev
```

✅ **Frontend berjalan di:** http://localhost:3000

---

### 5. Login Admin

1. Buka http://localhost:3000/login di browser.
2. Masukkan **email** dan **password** bebas (contoh: `admin@formqu.com` / `admin123`).
3. Saat login pertama kali, akun admin akan **otomatis dibuat** di MongoDB.
4. Untuk login berikutnya, gunakan email dan password yang sama.

---

## 📁 Struktur Folder

```
FormQu/
├── backend/                  # API Server (FastAPI + Python)
│   ├── src/
│   │   └── main.py           # Entry point API & semua routes
│   ├── public/files/         # Folder upload file (gambar/video)
│   ├── .env                  # ← Konfigurasi MongoDB & JWT
│   └── requirements.txt
├── frontend/                 # Web App (Next.js + TypeScript)
│   ├── src/
│   │   ├── app/
│   │   │   ├── admin/        # Halaman builder (buat form baru)
│   │   │   ├── forms/        # Daftar form & halaman edit/view
│   │   │   ├── f/[slug]/     # Halaman form publik (short URL)
│   │   │   └── login/        # Halaman login admin
│   │   ├── components/
│   │   │   ├── admin/        # Komponen builder (canvas, panel, toolbar)
│   │   │   ├── renderer/     # Komponen renderer form (dynamic-field, dll)
│   │   │   └── ui/           # Komponen UI dasar (button, input, dll)
│   │   ├── types/            # TypeScript type definitions
│   │   ├── utils/            # Utilities (form-logic, mini-markdown)
│   │   ├── constants/        # Konstanta field types & labels
│   │   └── lib/              # Helpers (api, utils)
│   ├── .env                  # ← Konfigurasi URL
│   └── package.json
└── README.md
```

## 📌 Tipe Field yang Tersedia

| Tipe | Keterangann |
|---|---|
| `text` | Input teks pendek (Short Text) |
| `large_text` | Input teks panjang (Long Text / Textarea) |
| `email` | Input email dengan validasi format |
| `phone` | Input nomor telepon dengan validasi |
| `number` | Input angka |
| `date` | Input tanggal (Date Picker) |
| `select` | Dropdown pilihan tunggal |
| `radio_group` | Pilihan ganda (Radio Button) |
| `checkbox_group` | Pilihan checkbox (multi-select) |
| `multiple_choice_grid` | Grid pilihan (baris × kolom) |
| `image_block` | Blok gambar dekoratif (non-input) |
| `section` | Pemisah bagian/halaman form |
| `signature` | Input tanda tangan digital |
| `image_upload` | Upload gambar |
| `video_upload` | Upload video |
