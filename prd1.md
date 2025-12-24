# Nama kelompok : Nurul Huda (221240001330)

#Product Requirement Document (PRD) — v2 (dengan Fitur Premium)

## MyKasir - Aplikasi Kasir Mobile (Android, SQLite)

---

### 1. Latar Belakang & Tujuan
MyKasir adalah aplikasi kasir mobile Android berbasis Flutter dengan database lokal (SQLite/Drift). Aplikasi membantu UMKM/toko retail mencatat dan mengelola produk, transaksi, dan laporan secara cepat, akurat, serta berjalan offline.

**Tujuan:**
- Menyediakan solusi kasir offline yang mudah digunakan dan terjangkau.
- Mendukung operasional modern: transaksi cepat, laporan ringkas, cetak struk Bluetooth.
- Menawarkan paket Premium (Pro) untuk kebutuhan lanjutan seperti manajemen kadaluarsa dan scan Barcode/QR.

---

### 2. Kebutuhan Bisnis
- Pencatatan produk dan transaksi yang andal tanpa internet.
- Monitoring stok, diskon, pajak, dan kembalian secara otomatis.
- Cetak struk via Bluetooth printer.
- Pelacakan barang perishable (kedaluwarsa) untuk mengurangi kerugian.
- Input super-cepat via barcode/QR untuk kasir.

---

### 3. Target Pengguna
- UMKM: warung, minimarket, toko retail kecil/menengah.
- Peran pengguna: Kasir (operasional harian), Pemilik/Manager (monitor & laporan).

---

### 4. Fitur Utama (Basic)
1. Manajemen Produk: CRUD (kode unik, nama, harga, stok, kategori, gambar ≤ 2MB)
2. Transaksi Kasir: keranjang, diskon (nominal/%), pajak (PPN), pembayaran & kembalian
3. Validasi Stok: cegah jual melebihi stok, update stok saat checkout
4. Struk & Printing: template struk, preview, cetak via Bluetooth
5. Dashboard & Laporan: penjualan harian, produk terlaris, grafik 7 hari, export (PDF)
6. Pengaturan: profil toko, pajak, printer, backup & restore, reset data (testing)
7. Offline Support: seluruh operasi inti tanpa internet

---

### 5. Fitur Premium (Pro)
1. Manajemen Kadaluarsa (Perishable/Expiry)
   - Tandai produk sebagai perishable (punya tanggal kadaluarsa)
   - Restock per batch dengan tanggal kadaluarsa
   - Penandaan status: normal, near-expiry (H−N), expired
   - Penjualan FIFO by expiry (batch dengan kadaluarsa terdekat diprioritaskan)
   - Kebijakan barang expired: blokir atau izinkan dengan konfirmasi (diatur di Pengaturan)
   - Laporan near-expiry dan expired (nilai potensi rugi), export PDF/Excel

2. Scan Barcode & QR Code
   - Tombol Scan pada halaman Transaksi (AppBar/Search/FAB)
   - Baca Barcode/QR → cocokkan ke kode produk → tambah/increase item di keranjang
   - Validasi stok & peringatan jika produk tidak ditemukan
   - Opsi QR lanjutan: kode dapat memuat info batch (untuk masa depan)

3. Gating & Monetisasi
   - Badge “Pro” pada tombol/halaman premium (Scan, Laporan Kadaluarsa)
   - Dialog “Upgrade ke Pro” saat diakses tanpa lisensi (ringkas benefit + CTA)

---

### 6. Spesifikasi Teknis Utama
- Platform: Android (API 21–34)
- Teknologi: Flutter, SQLite (Drift), Provider, Bluetooth printing
- Database inti: Products, Transactions, TransactionItems, Settings
- Bahasa: Indonesia; MD3 compliance

Tambahan Premium (konseptual DB):
- Batch Stok dengan tanggal kadaluarsa terasosiasi dengan produk (perishable only)

---

### 7. User Flow Utama
- Dashboard → Produk → CRUD/Restock → Transaksi → Checkout → Cetak Struk → Laporan → Pengaturan
- Premium: Restock per batch (kadaluarsa) → Transaksi (FIFO by expiry) → Laporan Kadaluarsa
- Premium: Transaksi → Scan (Barcode/QR) → Tambah item → Checkout

---

### 8. Detail Penempatan di UI
- Transaksi:
  - AppBar: tombol “Scan (Barcode/QR)” [Pro]
  - Alternatif: ikon kamera di kanan field pencarian atau FAB
  - Peringatan near-expiry/expired pada item di keranjang (sesuai pengaturan)
- Produk:
  - Form Produk: toggle “punya tanggal kadaluarsa” + field kode (Barcode/QR)
  - Restock: input tanggal kadaluarsa per batch
  - Daftar: badge warna (Near-expiry/Expired), filter by status
- Laporan:
  - Menu “Laporan Kadaluarsa” [Pro]: near-expiry, expired, filter, export
- Pengaturan:
  - Ambang near-expiry (H−7/H−14/H−30)
  - Kebijakan penjualan expired (blokir/konfirmasi)
  - Bagian “Premium/Pro”: upgrade & manajemen lisensi

---

### 9. Acceptance Criteria
Basic:
- CRUD Produk tervalidasi, kode unik, gambar ≤ 2MB
- Transaksi: keranjang, diskon, PPN, pembayaran & kembalian akurat
- Stok ter-update setelah checkout; validasi stok berjalan
- Struk tercetak rapi via Bluetooth; preview tersedia
- Dashboard & laporan berjalan; export PDF
- Performa ≤ 3 detik; offline penuh; UI MD3; bahasa Indonesia

Premium:
- Produk dapat ditandai perishable dan restock per batch dengan tanggal kadaluarsa
- Penandaan near-expiry/expired muncul sesuai ambang pengaturan
- Transaksi otomatis memilih batch dengan kadaluarsa terdekat (FIFO by expiry)
- Kebijakan expired dijalankan sesuai setting (blokir/konfirmasi)
- Scan Barcode/QR menambahkan produk ke keranjang dengan akurasi tinggi
- Laporan near-expiry & expired dapat difilter dan diekspor (PDF/Excel)
- Gating “Pro” tampil jelas; dialog upgrade muncul saat diperlukan

---

### 10. Skema Database Inti (Ringkas)
- Products: id, kode_barang (unik), nama, harga, stok, kategori, image_url
- Transactions: id, nomor_transaksi, tanggal, total, diskon, pajak, pembayaran, kembalian
- TransactionItems: id, transaction_id, product_id, harga_satuan, jumlah, subtotal
- Settings: id, key, value, updated_at

Catatan Premium (konseptual):
- StockBatches: id, product_id, qty, tanggal_kadaluarsa, created_at (untuk perishable)

---

### 11. Risiko & Mitigasi
- Salah input tanggal kadaluarsa → validasi format & konfirmasi ringkas
- Performa filter/sort expiry dengan data besar → indexing & pagination
- Printer Bluetooth kompatibilitas bervariasi → daftar merk teruji & fallback
- Akses kamera/izin ditolak untuk Scan → dialog izin ulang & fallback pencarian manual

---

### 12. Timeline & Deliverables (Ringkas)
- Mengikuti roadmap MVP 7 sprint; Premium ditambahkan setelah Sprint 2 (Transaksi):
  - Sprint 2.1: Scan Barcode/QR
  - Sprint 2.2: Manajemen Kadaluarsa (batch & FIFO by expiry) + Laporan

---

### 13. Dokumentasi & Compliance
- Mini-README per fitur utama, kebijakan privasi (kamera/Bluetooth), target API 34, ukuran bundle < 50MB, permissions minimal.

---

Dokumen ini memperbarui PRD sebelumnya dengan penambahan Fitur Premium (Pro): Manajemen Kadaluarsa dan Scan Barcode/QR. Untuk detail implementasi teknis, mengacu pada arsitektur yang ada tanpa mengubah tujuan operasional offline dan kemudahan penggunaan.



