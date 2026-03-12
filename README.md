# ERP Multi-Company System

Selamat datang di repositori utama proyek ERP Multi-Company. Sistem ini dirancang untuk menangani 10+ anak perusahaan dengan isolasi data tingkat tinggi melalui strategi *Database-per-Tenant* dan fitur rekonsiliasi otomatis.

---

## 📋 Dokumentasi Teknis

Untuk memastikan standarisasi pengembangan, seluruh arsitektur telah didokumentasikan "as-code". Silakan merujuk pada tautan di bawah ini:

### 1. Arsitektur Database & Multi-Tenancy
Menjelaskan struktur *Control Plane* (Main DB) untuk manajemen tenant dan *Data Plane* (Tenant DB) untuk operasional anak perusahaan.
* [**Lihat Rancangan ERD & Skema Database**](docs/database/schema.md)
  * *Isi: Skema tabel Companies, Invoices, Bank Statements, dan panduan Dynamic Connection Middleware.*

### 2. Arsitektur API (Clean Architecture)
Mendefinisikan standar komunikasi antar komponen dan pemisahan tanggung jawab (Layering) agar kode tetap modular dan mudah diuji.
* [**Lihat Arsitektur API & Rekonsiliasi**](docs/architecture/api.md)
  * *Isi: Sequence Diagram rekonsiliasi, implementasi Controller-Service-Repository, Model/DTO, dan standar Unit & API Testing.*

### 3. Strategi Infrastruktur (Caching & Queue)
Panduan mengenai penanganan komputasi berat, terutama untuk laporan konsolidasi lintas tenant agar performa API tetap responsif.
* [**Lihat Strategi Caching & Queue**](docs/architecture/system-flow.md)
  * *Isi: Background Processing (Queue), Distributed Cache dengan Redis, mekanisme Dead Letter Queue (DLQ), dan Data Chunking.*

---

## 🛠 Panduan Cepat (Quick Start)

### Identifikasi Tenant (Dynamic Connection)
Sistem tidak menggunakan koneksi statis. Identifikasi tenant dilakukan melalui middleware dengan dua metode:
1. **Subdomain (URL)**: Contoh `anak-a.erp-sistem.com`.
2. **Custom Header**: Menggunakan key `X-Tenant-Code`.

### Menjalankan Migrasi
Tim Backend wajib membedakan proses migrasi untuk menjaga integritas skema:
* **Main Migration**: Migrasi standar untuk tabel pusat di Main Database.
* **Tenant Migration**: Perintah kustom yang melakukan *looping* migrasi ke seluruh database anak perusahaan yang terdaftar.

---

## 👥 Tim Pengembang
* **Lead Developer**: [Isi Nama]
* **Backend Team**: [Isi Nama/Tim]

---
> **Catatan Penting**: Dokumentasi adalah bagian dari kode. Jika Anda melakukan perubahan pada struktur tabel, logika bisnis inti (Service), atau alur sistem asinkron, Anda **WAJIB** memperbarui file terkait di folder `docs/` menggunakan sintaks Mermaid atau Markdown sebelum mengajukan Pull Request.
