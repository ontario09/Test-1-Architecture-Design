# ERP Multi-Company System

Selamat datang di repositori utama proyek ERP Multi-Company. Sistem ini dirancang untuk menangani 10+ anak perusahaan dengan isolasi data tingkat tinggi dan fitur rekonsiliasi otomatis.

---

## 📋 Dokumentasi Teknis

Untuk memastikan standarisasi pengembangan, seluruh arsitektur telah didokumentasikan "as-code". Silakan merujuk pada tautan di bawah ini:

### 1. Arsitektur Database & Multi-Tenancy
Kami menggunakan strategi **Database-per-Tenant** untuk menjamin isolasi data antar anak perusahaan.
* [**Lihat Rancangan ERD & Skema Database**](docs/database/schema.md)
  * *Isi: Main DB Control Plane, Tenant DB Data Plane, dan Panduan Middleware.*

### 2. Arsitektur API (Coming Soon)
Dokumentasi kontrak API menggunakan standar OpenAPI/Swagger.
* [Desain Endpoint Rekonsiliasi](docs/api/v1-contract.md)

### 3. Strategi Infrastruktur
Panduan mengenai penanganan beban kerja berat dan sinkronisasi data.
* [Strategi Caching & Queue](docs/architecture/system-flow.md)

---

## 🛠 Panduan Cepat (Quick Start)

### Identifikasi Tenant
Sistem mengidentifikasi tenant berdasarkan dua metode:
1. **URL Subdomain**: `namaanakperusahaan.erp-sistem.com`
2. **Header HTTP**: `X-Tenant-Code: CMP_XXX`

### Menjalankan Migrasi
Karena kita menggunakan multi-database, pastikan Anda menjalankan perintah yang benar:
* **Main DB**: `command-migrate-main`
* **Semua Tenant**: `command-migrate-tenants`

---

## 👥 Tim Pengembang
* **Lead Developer**: @YourName
* **Backend Team**: @BackendTeam

---
> **Catatan**: Jika Anda melakukan perubahan pada struktur tabel, Anda **WAJIB** memperbarui file [schema.md](docs/database/schema.md) menggunakan sintaks Mermaid sebelum melakukan Pull Request.
