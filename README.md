# Google AI Pro (5 TB) 2 Tahun Gratis ($0.00) - Full Otomatisasi

Repositori ini menyediakan sistem otomasi lengkap berbasis **Python & Playwright** dengan antarmuka **Web Dashboard (FastAPI)** untuk mengklaim paket **Google AI Pro (5 TB)** secara gratis (US$0.00) selama **2 Tahun (24 Bulan)** dan langsung mengunci masa aktifnya hingga tahun 2028 dengan pembatalan perpanjangan otomatis (*cancel auto-renew*).

---

## 🌟 Fitur Utama

- **100% Autopilot 5-Langkah**:
  - **Step 1**: Login Google otomatis, bypass verifikasi email pemulihan, penanganan otomatis 2FA (TOTP), dan bypass penawaran Passkey (*Faster sign-in*).
  - **Step 2**: Pengikatan kartu pembayaran bebas pajak (*Tax-Free ZIP 97201 Oregon*) + Injeksi paket promo lapis ke-1 (12 Bulan gratis $0.00).
  - **Step 3**: Stacking/penumpukan paket promo lapis ke-2 (tambahan 6 Bulan gratis -> akumulasi 18 Bulan).
  - **Step 4**: Stacking/penumpukan paket promo lapis ke-3 (tambahan 6 Bulan gratis -> akumulasi 24 Bulan penuh).
  - **Step 5**: Pembatalan otomatis langganan di Google Play untuk mengunci masa aktif 2 tahun tanpa risiko terkena tagihan perpanjangan, lengkap dengan tangkapan layar verifikasi (*screenshot audit*).
- **Perlindungan Keuangan Ketat ($0.00 Safety Guard)**: Bot hanya akan mengklik konfirmasi langganan jika total tagihan di layar terkonfirmasi **US$0.00**.
- **Human-Like Interaction**: Pengetikan nomor kartu, tanggal kedaluwarsa, CVC, email, dan password dilengkapi jeda pengetikan manusiawi (*human delay 85-95ms*) untuk mencegah deteksi bot Google Pay.
- **Dukungan Proxy Lengkap**: Kompatibel dengan proxy residensial (FloppyData, SOCKS5, HTTP Proxy dengan autentikasi otomatis).
- **Web Dashboard Interaktif**: Kelola akun, impor massal (*bulk import*), pantau log secara langsung (*live stream*), dan jalankan akun secara tunggal maupun antrean massal (*batch mode*).

---

## 📋 Persyaratan Sistem

1. **Sistem Operasi**: Windows 10 / 11 (64-bit).
2. **Browser**: Google Chrome versi terbaru (terpasang di lokasi standar).
3. **Python**: Python 3.10 atau versi lebih baru (pastikan mencentang opsi **"Add Python to PATH"** saat instalasi).
4. **Jaringan / Proxy**: Koneksi menggunakan IP Amerika Serikat (US) baik via VPN lokal maupun template proxy di dashboard.

---

## 🚀 Panduan Memulai Cepat

### Cara 1: Menggunakan File Batch (Paling Praktis)
1. **Pertama kali menjalankan**: Klik dua kali file `1_首次运行_安装依赖并启动.bat`. File ini akan otomatis menginstal library yang dibutuhkan (`requirements.txt`), browser Playwright, dan menyalakan server.
2. **Penggunaan sehari-hari**: Cukup klik dua kali file `2_启动控制台.bat`.
3. Browser akan otomatis membuka dashboard di: **`http://127.0.0.1:8765`**.

### Cara 2: Manual lewat Terminal / CMD
```bash
# 1. Masuk ke folder proyek
cd C:\Rivaldi\mygithub\Google_AI_Pro_2Year_ZeroCost_Auto

# 2. Instal dependensi python
pip install -r requirements.txt

# 3. Instal browser playwright
playwright install chromium

# 4. Jalankan aplikasi
python app.py
```
Buka browser dan akses alamat: `http://127.0.0.1:8765`.

---

## 💳 Konfigurasi Kartu Pembayaran Bebas Pajak

Di panel kiri Web Dashboard (**⚙️ Global Plan & Card Config**), isi data kartu Anda:
* **Nomor Kartu**: 16 digit Visa atau Mastercard luar negeri.
* **Masa Berlaku**: Bulan dan Tahun (contoh: `08` / `29`).
* **CVC**: 3 digit kode keamanan di belakang kartu.
* **Nama Pemegang Kartu**: Nama latin (contoh: `JOHN DOE`).
* **ZIP Code Bebas Pajak**: Ditetapkan secara default ke `97201` (Portland, Oregon - Negara Bagian Bebas Pajak di AS).
* Klik tombol **💾 Simpan Konfigurasi**.

> ℹ️ **Catatan**: Kartu hanya digunakan sebagai syarat pendaftaran uji coba Google Pay. Sepanjang alur otomasi, saldo kartu tidak akan terpotong karena bot memvalidasi transaksi berbiaya **US$0.00**.

---

## 📥 Panduan Impor Akun Massal (Batch Import)

Di panel kanan Web Dashboard (**📥 Area Impor Akun Massal**), Anda dapat memasukkan banyak akun sekaligus.
* Aturan: **1 baris = 1 akun**.
* Karakter pemisah yang didukung: `----` *(disarankan)*, `|`, `/`, koma, spasi, atau Tab.

### Contoh Format Baris:

#### 1. Akun Lengkap dengan 2FA TOTP (100% Autopilot - Paling Direkomendasikan)
```text
dwildandi1@gmail.com----Password123----recovery1@outlook.com----JBSWY3DPEHPK3PXP
dwildandi2@gmail.com----Password456----recovery2@outlook.com----KRSXG5CTMVRXEZLU
```

#### 2. Akun dengan Email Pemulihan Saja (Tanpa 2FA)
```text
dwildandi1@gmail.com----Password123----recovery1@gmail.com
dwildandi2@gmail.com----Password456----recovery2@gmail.com
```

#### 3. Akun dengan Proxy Khusus per Akun
```text
dwildandi1@gmail.com----Password123----recovery1@gmail.com----JBSWY3DPEHPK3PXP----http://user:pass@ip:port
```

Setelah ditempel ke kolom teks, klik tombol hijau **➕ Impor Akun**.

---

## 🔐 Penanganan 2FA (Two-Factor Authentication / TOTP)

Sistem ini mendukung penanganan verifikasi 2 langkah secara cerdas:

### 1. Apa itu `totp_secret`?
`totp_secret` adalah kunci rahasia berupa teks (16–32 karakter Base32) yang diberikan Google saat mengaktifkan **Aplikasi Authenticator**. Bot menggunakan kunci ini untuk menghitung secara real-time kode 6 digit verifikasi tanpa perlu melihat handphone.

### 2. Cara Mengambil `totp_secret` di Akun Google:
1. Kunjungi [Google Account Security](https://myaccount.google.com/security) -> **Verifikasi 2 Langkah**.
2. Pilih **Aplikasi Authenticator** -> klik **+ Siapkan authenticator**.
3. Di bawah QR Code, klik tulisan: **"Tidak dapat memindai? / Can't scan it?"**.
4. Salin kode teks rahasia yang muncul (contoh: `JBSWY3DPEHPK3PXP`). Masukkan kode ini pada kolom `totp_secret` akun Anda.

### 3. Alur Kerja Bot saat Menemui 2FA:
* **Jika akun memiliki `totp_secret`**:
  Bot akan otomatis memilih metode Google Authenticator, menghitung 6 angka yang valid di detik itu juga, mengisikannya ke form, dan langsung lolos masuk (*100% autopilot*).
* **Jika akun TIDAK memiliki `totp_secret` (Notifikasi HP / Ketuk Angka)**:
  Bot secara otomatis mendeteksi ketiadaan kunci, tidak membatalkan notifikasi HP, dan memberi jeda waktu 30 detik agar Anda dapat mengetuk angka konfirmasi di handphone Anda secara manual.

---

## 🛡️ Fitur Bypass Layar Tambahan Google

Skrip ini dilengkapi penanganan otomatis untuk rintangan keamanan Google yang sering muncul:
* **Google Passkey / Kunci Sandi (`更快登录`)**: Otomatis mendeteksi tawaran Passkey dan memilih **"以后再说" / "Not now"** agar tidak memicu pop-up Windows Hello PIN.
* **Halaman Recovery Options (`gds.google.com`)**: Otomatis menekan tombol Lewati/Batal untuk melanjutkan pengalihan sesi login.
* **Halaman Confirm Identifier**: Otomatis menekan tombol Next untuk melanjutkan ke form password.

---

## ⚙️ Alur Eksekusi 5 Langkah (Step 1 - Step 5)

| Langkah | Nama Tahapan | Deskripsi Teknis |
| :---: | :--- | :--- |
| **Step 1** | **Login & 2FA** | Autentikasi akun Google, auto-fill TOTP 2FA / email pemulihan, bypass Passkey & layar bantuan. |
| **Step 2** | **Layer 1 (12 Bulan)** | Buka Google One, pasang kartu bebas pajak, klaim paket 12 bulan gratis pertama ($0.00). |
| **Step 3** | **Layer 2 (6 Bulan)** | Tumpuk promo kedua melalui injeksi token offer Google One ($0.00 -> 18 bulan). |
| **Step 4** | **Layer 3 (6 Bulan)** | Tumpuk promo ketiga hingga maksimal 24 bulan penuh ($0.00 -> 2028). |
| **Step 5** | **Cancel & Lock** | Buka Google Play Subscription, batalkan perpanjangan otomatis (*cancel auto-renew*), kunci hak akses 5 TB sampai 2028, dan simpan screenshot audit. |

---

## 📁 Struktur Direktori Proyek

```text
Google_AI_Pro_2Year_ZeroCost_Auto/
├── app.py                     # Server backend FastAPI & antarmuka Web Dashboard
├── automation_engine.py       # Engine otomasi Playwright (5-Step Pipeline & Auto-handling)
├── config.json                # Konfigurasi aktif kartu & daftar akun (diabaikan git)
├── config.example.json        # Template konfigurasi contoh
├── templates/
│   └── index.html             # Antarmuka dashboard web
├── profiles/                  # Direktori cache sesi browser Chrome per akun
├── screenshots/               # Bukti tangkapan layar keberhasilan Step 1 - Step 5
├── requirements.txt           # Daftar dependensi Python
├── 1_首次运行_安装依赖并启动.bat  # Script inisialisasi & instalasi otomatis
└── 2_启动控制台.bat             # Script peluncur dashboard instan
```

---

## ⚠️ Catatan & Penafian (Disclaimer)

Proyek ini dibuat untuk keperluan penelitian, pembelajaran otomasi peramban (*browser automation*), serta pengujian integrasi Playwright. Gunakan secara bijak dan patuhi syarat dan ketentuan penggunaan layanan Google. Penulis tidak bertanggung jawab atas penyalahgunaan skrip ini.
