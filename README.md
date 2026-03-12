# ERP Multi-Company System

Selamat datang di repositori utama proyek ERP Multi-Company. Sistem ini dirancang untuk menangani 10+ anak perusahaan dengan isolasi data tingkat tinggi dan fitur rekonsiliasi otomatis.

---

## 📋 Dokumentasi Teknis

Untuk memastikan standarisasi pengembangan, seluruh arsitektur telah didokumentasikan "as-code". Silakan merujuk pada tautan di bawah ini:

### 1. Arsitektur Database & Multi-Tenancy
Kami menggunakan strategi **Database-per-Tenant** untuk menjamin isolasi data antar anak perusahaan secara fisik.
* [**Lihat Rancangan ERD & Skema Database**](docs/database/schema.md)
  * *Isi: Main DB Control Plane, Tenant DB Data Plane, dan Panduan Middleware.*

### 2. Arsitektur API & Pola Layanan (Service Pattern)
Standar komunikasi antar komponen dan pemisahan logika bisnis menggunakan Service Layer agar kode tetap modular.
* [**Lihat Arsitektur API & Rekonsiliasi**](docs/architecture/api.md)
  * *Isi: Sequence Diagram, Layering (Controller, Service, Repository), Model/DTO, dan Standar Testing.*

### 3. Strategi Infrastruktur (Caching & Queue)
Panduan mengenai penanganan beban kerja berat untuk laporan konsolidasi dan sinkronisasi data asinkron.
* [**Lihat Strategi Caching & Queue**](docs/architecture/system-flow.md)
  * *Isi: Background Processing, Redis Caching Strategy, dan Dead Letter Queue (DLQ).*

---

## 🛠 Panduan Cepat (Quick Start)

### Identifikasi Tenant
Sistem mengidentifikasi tenant berdasarkan dua metode utama:
1. **URL Subdomain**: `namaanakperusahaan.erp-sistem.com`
2. **Header HTTP**: `X-Tenant-Code: CMP_XXX`

### Menjalankan Migrasi
Karena sistem menggunakan multi-database, tim Backend wajib menggunakan perintah yang sesuai:
* **Main DB**: `command-migrate-main` (Untuk tabel Companies & Config)
* **Semua Tenant**: `command-migrate-tenants` (Iterasi ke seluruh database anak perusahaan)

---

## 👥 Tim Pengembang
* **Lead Developer**:
* **Backend Team**:

---
> **Catatan Penting**: Dokumentasi adalah bagian dari kode. Jika Anda melakukan perubahan pada struktur tabel, logika bisnis inti, atau alur sistem, Anda **WAJIB** memperbarui file terkait di folder `docs/` menggunakan sintaks Mermaid atau Markdown sebelum mengajukan Pull Request.
