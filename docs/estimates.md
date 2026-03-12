# 📊 Estimasi Detail Pengembangan Fitur (Mandays)

Dokumen ini merinci pembagian tugas spesifik untuk memastikan transparansi beban kerja di setiap lapisan pengembangan.

---

## 1. Ringkasan Alokasi Mandays

| Fitur | Riset/Analisis | Backend | Frontend | Testing/QA | Total (MD) |
|-------|----------------|---------|----------|------------|------------|
| **Custom PO** | 1.0 | 2.5 | 2.0 | 0.5 | **6.0** |
| **Deposit Payment** | 1.0 | 3.0 | 1.5 | 1.0 | **6.5** |
| **Payment Gateway** | 1.5 | 2.0 | 1.0 | 1.0 | **5.5** |
| **Chat System** | 1.0 | 2.5 | 2.5 | 1.0 | **7.0** |

---

## 2. Rincian Tugas Per Fitur

### A. Custom PO (Purchase Order) — **Total: 6.0 MD**

#### 📝 Riset & Analisis (1.0 MD)
- Analisis skema kalkulasi pajak (PPN/PPH) dan diskon bertingkat
- Perancangan skema database untuk *versioning* PO (menyimpan histori perubahan)

#### ⚙️ Backend (2.5 MD)
- Implementasi CRUD dengan *complex business logic* (validasi stok, limit budget per tenant)
- Integrasi library PDF (Snappy/DomPDF) dan pembuatan template blade untuk export
- Sistem penomoran otomatis (*auto-increment* unik per tenant)

#### 🎨 Frontend (2.0 MD)
- Pembuatan form dinamis (add/remove item row) dengan validasi *client-side*
- UI untuk *preview* PDF sebelum di-download

#### 🧪 Testing/QA (0.5 MD)
- Uji akurasi kalkulasi matematika pada berbagai skenario input

---

### B. Deposit Payment (Wallet System) — **Total: 6.5 MD**

#### 📝 Riset & Analisis (1.0 MD)
- Perancangan arsitektur *double-entry bookkeeping* untuk menjamin saldo tidak *corrupt*

#### ⚙️ Backend (3.0 MD)
- Pembuatan sistem ledger (tabel `transactions` dan `balances`)
- Implementasi *database locking* (pessimistic locking) untuk mencegah *race condition* saat top-up bersamaan
- Fitur *auto-refund* jika transaksi dibatalkan

#### 🎨 Frontend (1.5 MD)
- Dashboard saldo dan riwayat mutasi (debit/kredit)
- Form input nominal dengan format ribuan (*auto-masking*)

#### 🧪 Testing/QA (1.0 MD)
- *Stress testing* untuk memastikan integritas saldo saat ribuan request masuk serentak

---

### C. Payment Gateway Integration — **Total: 5.5 MD**

#### 📝 Riset & Analisis (1.5 MD)
- Studi dokumentasi API Midtrans/Xendit dan mapping status sukses/gagal
- Perancangan *security signature* untuk validasi callback

#### ⚙️ Backend (2.0 MD)
- Endpoint webhook untuk menerima notifikasi pembayaran otomatis
- Logika prioritas pengecekan status: `Expired` > `New` > `Ongoing`
- Integrasi otomatisasi rekonsiliasi ke database tenant masing-masing

#### 🎨 Frontend (1.0 MD)
- Integrasi SDK (Snap JS) dan halaman *checkout*
- Halaman "Menunggu Pembayaran" dengan timer *countdown*

#### 🧪 Testing/QA (1.0 MD)
- Simulasi berbagai status pembayaran (Pending, Settlement, Deny, Expire)

---

### D. Chat System (Real-time) — **Total: 7.0 MD**

#### 📝 Riset & Analisis (1.0 MD)
- Analisis infrastruktur WebSocket (Pusher/Socket.io) dan kebutuhan RAM server

#### ⚙️ Backend (2.5 MD)
- Setup WebSocket server dan *private channels* per tenant
- Logika penyimpanan pesan (chat history) dan status "Read/Unread"
- Broadcasting event notifikasi pesan masuk ke dashboard

#### 🎨 Frontend (2.5 MD)
- UI chat box yang *auto-scroll* ke bawah saat pesan baru masuk
- Indikator "Typing..." dan status "Online/Offline"
- Optimasi rendering pesan dalam jumlah banyak (*virtual scrolling*)

#### 🧪 Testing/QA (1.0 MD)
- Uji latensi pengiriman pesan dan sinkronisasi antar tab browser

---

## 💡 Catatan untuk Tim

| No | Catatan | Keterangan |
|----|---------|------------|
| 1 | **Buffer** | Estimasi di atas belum termasuk *buffer* 20% untuk kendala teknis tak terduga |
| 2 | **Update** | Jika ditemukan kendala saat fase riset yang berpotensi menambah mandays, segera laporkan pada saat *daily standup* |

---
