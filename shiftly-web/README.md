# Shiftly AI - Hospital Staff Scheduling & Financial Optimization System

**Shiftly AI** adalah platform sistem manajemen penjadwalan pegawai rumah sakit berbasis web yang mengintegrasikan algoritma Machine Learning dan kecerdasan buatan (**K-Means Clustering**, **Genetic Algorithm**, dan **Random Forest**) untuk mengoptimalkan pembagian jadwal kerja teknis medis sekaligus mengevaluasi dampak finansial operasional rumah sakit.

Sistem ini dikembangkan memanfaatkan **Healthcare Staff Dataset (Kaggle)** dengan memperhitungkan kompleksitas dunia kerja medis yang dinamis, seperti keragaman sertifikasi pegawai, spesialisasi medis, batas maksimum jam kerja, hingga kalkulasi lembur (*overtime*) dan biaya operasional penggajian.

---

## AI Features & Implementation

1. **Komposisi Spesialisasi & Sertifikasi (K-Means Clustering)**
   * **Masalah:** Setiap unit/shift di rumah sakit membutuhkan komposisi perawat dan dokter dengan kualifikasi khusus (contoh: ICU, IGD, Bedah).
   * **Solusi:** Algoritma *K-Means* mengelompokkan pegawai berdasarkan tingkat keahlian, pengalaman, dan jenis sertifikasi agar distribusi staf per-shift seimbang dan sesuai standar kualifikasi.

2. **Optimasi Penjadwalan Beban Kerja (Genetic Algorithm)**
   * **Masalah:** Menyusun jadwal ratusan pegawai secara manual sering memicu *burnout*, bentrok shift, atau melanggar batasan regulasi jam kerja.
   * **Solusi:** *Genetic Algorithm* (GA) secara otomatis memunculkan kombinasi jadwal kerja yang optimal (meminimalkan *hard constraints violation* & *soft constraints violation*).

3. **Evaluasi & Prediksi Efisiensi Finansial (Random Forest)**
   * **Masalah:** Biaya operasional tinggi akibat pembengkakan uang lembur atau alokasi staf berlebih di jam-jam sepi.
   * **Solusi:** Model *Random Forest* menganalisis performa historis dan memprediksi kebutuhan finansial/anggaran gaji, sehingga manajemen bisa mengevaluasi efisiensi biaya jadwal yang dibuat sebelum diterapkan.

---

## 🛠 Tech Stack

* **Web Architecture & Backend:** Laravel (PHP) — `Port: 8001`
* **AI Engine & Microservice:** FastAPI (Python) — `Port: 8000`
* **Database:** MySQL (Laragon / phpMyAdmin)
* **Dataset:** Kaggle Healthcare Employee / Staff Dataset

---

## First-Time Setup Guide

Ikuti langkah-langkah di bawah ini untuk melakukan instalasi dan konfigurasi pertama kali pada environment lokal kamu.

### 1. Clone Repository

Buka terminal/PowerShell, lalu jalankan perintah berikut:

```bash
git clone https://github.com/jonathanchristian21/shiftly_aiml.git
cd shiftly-aiml
```

### 2. Setup Web Application (Laravel)

Masuk ke direktori `shiftly-web` dan install dependencies PHP:

```bash
cd shiftly-web
composer install
```

Copy file contoh environment dan generate application key:

```bash
copy .env.example .env
php artisan key:generate
```

Buka file `.env` yang baru dibuat di `shiftly-web`, lalu sesuaikan konfigurasi databasenya sebagai berikut:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=shiftly_aiml
DB_USERNAME=root
DB_PASSWORD=
```

Buka **Laragon / phpMyAdmin**, lalu buat database baru dengan nama **`shiftly_aiml`**.

Jalankan migrasi database:

```bash
php artisan migrate:fresh
```

### 3. Setup AI Service (FastAPI)

Kembalikan direktori ke folder utama dan masuk ke folder `shiftly-ai`:

```bash
cd ..\shiftly-ai
```

Buat dan aktifkan Virtual Environment Python:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install seluruh library yang dibutuhkan:

```bash
pip install -r requirements.txt
```

---

## How to Run the Application

Untuk menjalankan project ini, kamu perlu membukanya dalam 2 terminal berbeda (PowerShell).

### Terminal 1: FastAPI (AI Engine)

Jalankan perintah berikut:

```powershell
cd D:\laragon\www\Projects\shiftly-aiml\shiftly-ai
.\.venv\Scripts\Activate.ps1
python -B -m uvicorn app.main:app --host 127.0.0.1 --port 8000
```

**Cek Ketersediaan API:**
* **Health Check:** `http://127.0.0.1:8000/health`
* **API Docs (Swagger):** `http://127.0.0.1:8000/docs`

### Terminal 2: Laravel (Web Service)

Buka terminal baru, lalu jalankan perintah berikut:

```powershell
cd D:\laragon\www\Projects\shiftly-aiml\shiftly-web
php artisan serve --port=8001
```

**Akses Web Application:**
* Buka browser dan akses: `http://127.0.0.1:8001`

---

## Useful Commands

* **Menghentikan Process Service (FastAPI / Laravel):**
  Tekan `Ctrl + C` pada terminal tempat service berjalan.

* **Menghentikan Process jika Port Terkunci (misal port 8001):**
  Jika terjadi kendala *Port in use*, jalankan command PowerShell berikut untuk kill process:

  ```powershell
  Get-NetTCPConnection -LocalPort 8001 | ForEach-Object { Stop-Process -Id $_.OwningProcess -Force }
  ```

---

## Troubleshooting & Important Notes

### Note Login Akun Manager

Jika kamu ingin mendemokan atau login menggunakan akun **Manager**:

1. Buka database **`shiftly_aiml`** di **phpMyAdmin**.
2. Cari data user dengan role manager pada tabel `users`.
3. Edit data manager tersebut pada kolom `password`:
   * Pilih fungsi **MD5** pada opsi drop-down.
   * Masukkan password biasa (misal: `password123`).
   * Klik **Save / Go**.
4. Coba login menggunakan password tersebut.
5. **Penting:** Jika setelah berhasil login terjadi error saat masuk/navigasi ke dalam dashboard account manager, kembalikan (*change back*) password tersebut menjadi password hash standar Laravel.