# Dokumentasi Skema Database ERP (Multi-Company)

Dokumen ini menjelaskan struktur data untuk mendukung fitur **Multi-tenancy** menggunakan strategi *Database-per-Tenant*, Rekonsiliasi Payment, dan Laporan Konsolidasi.

---

## 1. Main Database (Control Plane)
Database ini berfungsi sebagai pusat kendali untuk mengidentifikasi dan mengarahkan koneksi ke database anak perusahaan yang tepat.

```mermaid
erDiagram
    COMPANIES ||--o{ DATABASE_CONFIGS : "manages"
    COMPANIES {
        uuid id PK
        string company_name
        string domain_url "Identifikasi via URL"
        string tenant_code "Identifikasi via Header"
        boolean is_active
    }
    DATABASE_CONFIGS {
        uuid id PK
        uuid company_id FK
        string db_host
        string db_name "Nama DB Fisik Tenant"
        string db_user
        string db_password
        int db_port
    }
```

## 2. Tenant Database (Data Plane)
Database ini diduplikasi untuk setiap anak perusahaan. Tidak terdapat kolom company_id karena data sudah terisolasi secara fisik di level database.

```mermaid
erDiagram
    CHART_OF_ACCOUNTS ||--o{ INVOICES : "categorizes"
    INVOICES ||--o{ PAYMENT_RECONCILIATIONS : "matched_to"
    BANK_STATEMENTS ||--o{ PAYMENT_RECONCILIATIONS : "links_to"

    CHART_OF_ACCOUNTS {
        uuid id PK
        string account_code
        string account_name
        string category "Asset/Revenue/Expense/Liability"
    }

    INVOICES {
        uuid id PK
        string invoice_number
        decimal total_amount
        string status "Unpaid/Partial/Paid"
        datetime due_date
    }

    BANK_STATEMENTS {
        uuid id PK
        string bank_account
        decimal transaction_amount
        string reference_code "Auto-match reference"
        boolean is_reconciled
    }

    PAYMENT_RECONCILIATIONS {
        uuid id PK
        uuid invoice_id FK
        uuid bank_statement_id FK
        decimal matched_amount
        string match_method "System/Manual"
        datetime reconciled_at
    }
```

## 3. Panduan Implementasi untuk Backend Developer

Kepada tim Backend, mohon perhatikan logika berikut agar implementasi sesuai dengan standar arsitektur:

### A. Mekanisme Dynamic Connection
Aplikasi tidak boleh menggunakan koneksi statis. Setiap request harus melalui middleware untuk menentukan database target sebelum logika bisnis dijalankan:

1. **Identifikasi**: Ambil identitas perusahaan melalui **Subdomain (URL)** (misal: `anak-a.erp.com`) atau **Custom Header** (`X-Tenant-Code`).
2. **Lookup**: Lakukan query ke **Main Database** pada tabel `COMPANIES` dan `DATABASE_CONFIGS` menggunakan identitas yang didapat.
3. **Switch**: Gunakan konfigurasi (host, db_name, username, password) tersebut untuk mengubah koneksi database *default* aplikasi secara dinamis (on-the-fly).



### B. Migrasi Database
Untuk menjaga integritas skema di seluruh lingkungan, tim harus membedakan proses migrasi:

* **Main Migration**: Gunakan perintah migrasi standar (misal: `php artisan migrate` atau `npx prisma migrate`) yang hanya ditujukan untuk tabel di **Main Database**.
* **Tenant Migration**: Gunakan perintah kustom yang melakukan *looping* ke semua database yang terdaftar di `DATABASE_CONFIGS`. Perintah ini memastikan skema di setiap database anak perusahaan tetap sinkron (homogen).

### C. Strategi Konsolidasi Laporan
Karena data terpisah secara fisik, penarikan laporan konsolidasi dilakukan dengan:
1. Iterasi query ke masing-masing database tenant yang aktif.
2. Melakukan agregasi data (Collection/Array Map) di level aplikasi.
3. Gunakan **Caching** jika data yang ditarik melibatkan kalkulasi berat dari 10 anak perusahaan sekaligus.
