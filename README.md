# SILAUNDRY

Aplikasi desktop manajemen laundry berbasis **Flutter**. Repositori ini berisi hasil
**UI slicing** dari desain Figma SILAUNDRY untuk tugas *[Data Master] Feature Development*.

## Fitur yang sudah dikerjakan

| Kelompok | Fitur | Layar |
|----------|-------|-------|
| Autentikasi | Registrasi, Login, Logout | Register Form, Login Form, Log Out Caution |
| CRUD master data | Customer (tambah, lihat, cari, ubah, hapus) | Customers, New User, Edit User, Delete Caution |
| CRUD master data | Layanan laundry (tambah, lihat, cari, ubah, hapus, pagination) | Service List |
| Transaksi | Buat order, pilih layanan/customer, lihat detail, ubah status | New Order, Order, See Detail |
| Pembayaran | Checkout, pilih metode, lihat status dan ringkasan transaksi | Check Out, Payment |
| Profil | Lihat dan ubah informasi pengguna | Account Settings |
| Kerangka | Sidebar per role + Dashboard Admin/User | Dashboard |
| Pelaporan | Report Admin: query SQLite, filter (tanggal, status, kategori), grouping (periode, layanan/kategori/customer, status/metode bayar), line + bar + pie chart | Report |

Menu sidebar mengikuti desain: **Admin** (Dashboard, Customer, Order, Report, Service List) dan
**User** (Dashboard, Order, Payment). Data transaksi saat ini disimpan in-memory bersama data customer dan layanan.

## Modul pelaporan (Report)

- **Query database:** `lib/data/report_db.dart` membuat tabel SQLite sesuai PDM dan mengambil data
  dengan query `JOIN`, `WHERE`, `GROUP BY`, dan agregasi (`COUNT`, `SUM`).
  Isi tabel disalin dari `AppState` setiap ada perubahan data.
- **Filter:** rentang tanggal, status order, kategori layanan (tombol Reset filter tersedia).
- **Grouping:** tren per hari/bulan, pendapatan per layanan/kategori/customer, komposisi per status/metode bayar.
- **Chart (`fl_chart`):** line chart (tren pendapatan), bar chart (pendapatan per kelompok), pie chart (komposisi order),
  ditambah kartu ringkasan dan tabel rincian.
- **Data contoh:** `AppState._seedOrders()` membuat 36 order contoh (+-5 bulan) agar laporan langsung terisi.
  Hapus pemanggilan `_seedOrders()` di `_seed()` bila tidak diperlukan.

Setelah `git pull`/ekstrak, jalankan `flutter pub get` karena ada dependensi baru
(`fl_chart`, `sqflite_common_ffi`, `sqlite3_flutter_libs`).

## Akun demo

| Role | Username | Password |
|------|----------|----------|
| Admin | `admin` | `admin12345` |
| User | `user` | `user123456` |

Aturan: password 10-12 karakter, username harus unik.

## Cara menjalankan

Prasyarat: Flutter SDK dengan dukungan desktop (Windows / Linux / macOS).

```bash
git clone <url-repo-ini>
cd silaundry
flutter create . --platforms=windows,linux,macos   # membuat folder platform (sekali saja)
flutter pub get
flutter run -d windows      # atau: -d linux / -d macos
```

Menjalankan tes: `flutter test`

## Struktur proyek

```
lib/
  main.dart                    # entry point + pengalihan login/main
  theme.dart                   # warna dan tema
  models/                      # AppUser (tabel users), LaundryService (tabel services)
  data/app_state.dart          # data in-memory + method sesuai laporan mapping
  widgets/common.dart          # tombol, panel, field, dialog konfirmasi
  data/report_db.dart          # SQLite + query laporan (filter/grouping)
  screens/                     # login, register, main_shell, dashboard,
                               # customers, services, profile, report
assets/logo.png                # logo hasil slicing dari desain
docs/                          # laporan mapping UI-PDM dan skema SQL
```

## Hubungan dengan laporan mapping

Nama method di `lib/data/app_state.dart` sama dengan rencana method pada laporan
(`docs/Laporan_Mapping_UI_PDM_SILAUNDRY.pdf`): `login`, `register`, `isUsernameExist`,
`logout`, `getProfile`, `updateProfile`, `getAllCustomers`, `searchCustomer`, `addCustomer`,
`updateCustomer`, `deleteCustomer`, `getServices`, `searchService`, `addService`,
`updateService`, `deleteService`.

Saat ini data disimpan di memori (hilang saat aplikasi ditutup). Skema database sesuai PDM ada di
`docs/silaundry_schema.sql`, sehingga penyimpanan ke database dapat ditambahkan dengan mengganti
isi method di `app_state.dart` tanpa mengubah layar.

## Informasi tim

- **Nama clan:** (isi)
- **Hacker (pengumpul):** (nama) - (NPM)

| No | Nama | NPM | Peran | Pembagian tugas |
|----|------|-----|-------|-----------------|
| 1 | (isi) | (isi) | Hacker | (mis. autentikasi) |
| 2 | (isi) | (isi) | (isi) | (mis. CRUD Customer) |
| 3 | (isi) | (isi) | (isi) | (mis. CRUD Service dan profil) |
