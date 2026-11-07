# 🛡️ SIMBANA - Sistem Informasi Monitoring Bencana Terpadu

> **Early Warning System (EWS) Multi-Hazard Berbasis Multi-Sensor IoT dan Web Dashboard Real-Time**  
> Sistem Informasi Pemantauan Bencana Terpadu Berbasis IoT Multi-Node.

---

## 📋 Tentang Proyek

**SIMBANA** (*Sistem Informasi Monitoring Bencana Terpadu*) adalah sistem peringatan dini terintegrasi yang memantau **4 jenis potensi ancaman bencana** secara simultan dalam satu kesatuan sistem:

1. 🌋 **Gempa Bumi**: Deteksi akselerasi seismik 3-axis ($X, Y, Z$) menggunakan sensor **MPU-6050** (I2C Digital).
2. 🔥 **Kebakaran / Api**: Deteksi radiasi inframerah lidah api menggunakan **Flame Sensor** (Digital Out).
3. 💨 **Kebocoran Gas Beracun & Asap**: Pemantauan konsentrasi gas mudah terbakar (LPG/Propana/Asap) menggunakan sensor **MQ-2 / MQ-135** (Analog Out).
4. 🌊 **Luapan Banjir**: Deteksi genangan/kontak cairan air menggunakan **Sensor Kelembapan / Raindrop Module** dengan komparator LM393 (Digital Out).

Sistem ini dirancang dengan pendekatan arsitektur *multi-tenant* dan *multi-device* yang menghubungkan node instrumen fisik berbasis mikrokontroler **NodeMCU ESP8266** menuju Web Server Backend (**Laravel & MySQL**) secara *real-time*.

---

## 🚀 Fitur Utama

### 📊 Dashboard & Monitoring Multi-Node Terpusat
- **Global Overview Multi-Node**: Pemantauan status darurat agregat pada seluruh titik node perangkat secara tersentralisasi.
- **Live Telemetry Zero-Refresh**: Pembaruan data 4 sensor secara *real-time* via *asynchronous polling* tanpa *reload* halaman.
- **Peta Spasial GIS (Leaflet.js)**: Pemetaan sebaran node instrumen dengan penanda status interaktif (*Aman / Siaga / Bahaya*).
- **Grafik Seismogram & Tren Gas (Chart.js)**: Visualisasi fluktuasi getaran dan konsentrasi gas secara dinamis.
- **Peringatan Darurat Otomatis**: Banner darurat visual dan aktivasi sirene audio lokal (*Active Buzzer fail-safe*) di lokasi kejadian.

### 👥 Multi-User & Device Sharing
- **Role-Based Access Control (RBAC)**: Pengelolaan hak akses berjenjang (Administrator, Petugas/Dosen, Civitas Akademika).
- **Device Claiming & Ownership**: Pendaftaran dan pengelolaan kepemilikan alat berbasis identitas UUID perangkat.
- **Multi-User Access Sharing**: Pemilik utama (*Owner*) dapat membagikan izin pemantauan alat kepada pengguna lain (*Viewer / Operator*) melalui email.

### 📜 Manajemen Insiden & Pelaporan
- **Pencatatan Insiden Otomatis**: Deteksi pelanggaran ambang batas (*threshold*) yang langsung dibukukan ke riwayat insiden.
- **Ekspor Dokumen Laporan**: Rekapitulasi catatan kejadian bencana yang dapat diekspor ke format dokumen (PDF/CSV) untuk kebutuhan manajerial.

## 📦 Panduan Instalasi & Menjalankan Proyek

### 1. Prasyarat Sistem
- PHP >= 8.2
- Composer >= 2.x
- Node.js >= 18.x & NPM
- MySQL Server / MariaDB

### 2. Kloning & Pengaturan Dependensi
```bash
# Salin file konfigurasi environment
cp .env.example .env

# Pasang dependensi PHP (Composer)
composer install

# Pasang dependensi Javascript/CSS (NPM)
npm install
```

### 3. Konfigurasi Basis Data & Kunci Aplikasi
Buka file `.env`, lalu sesuaikan konfigurasi basis data Anda:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=simbana_db
DB_USERNAME=root
DB_PASSWORD=
```

Jalankan pembuatan application key dan migrasi basis data:
```bash
# Generate application encryption key
php artisan key:generate

# Jalankan migrasi dan seeder
php artisan migrate --seed

# Buat symbolic link untuk penyimpanan berkas
php artisan storage:link
```

### 4. Menjalankan Server Lokal
```bash
# Terminal 1: Menjalankan Laravel Backend Server
php artisan serve

# Terminal 2: Menjalankan Vite Asset Bundler
npm run dev
```
Akses sistem melalui peramban pada alamat: `http://localhost:8000`

---

## 📡 Dokumentasi Endpoint REST API Telemetri

| Method | Endpoint | Fungsi |
| :---: | :--- | :--- |
| `POST` | `/api/v1/devices/{uuid}/data` | Menerima paket payload telemetri 4 sensor dari ESP8266 |
| `POST` | `/api/v1/devices/{uuid}/heartbeat` | Pembaruan status *last seen* dan konektivitas perangkat |
| `GET` | `/api/v1/devices/{uuid}/info` | Memeriksa konfigurasi dan status registrasi perangkat |
| `GET` | `/api/v1/thresholds` | Mengambil data konfigurasi ambang batas (*threshold*) bahaya |

---

## 👥 Tim Pengembang & Kontributor
Proyek ini dikembangkan sebagai bagian dari **Capstone Project Program Studi Sistem Informasi**, Program Studi Sistem Informasi.

---

## 📄 Acknowledgement & Referensi
Sistem informasi ini dikembangkan lebih lanjut (*upgraded baseline*) dari arsitektur dasar proyek monitoring gempa sebelumnya:
- **Basis Repositori Acuan:** [dkrpl/Monitoring-Gempa](https://github.com/dkrpl/Monitoring-Gempa) (SIPEGA UNP Kediri)
- **Peningkatan (*Novelty*):** Ekspansi dari sistem monolitik gempa tunggal menjadi arsitektur terpadu *Multi-Hazard Disaster Monitoring* (Gempa MPU-6050, Api, Gas MQ-2, Banjir Kelembapan), integrasi *WiFiManager*, serta implementasi *Multi-Tenant Device Sharing*.

---

## 📝 Lisensi
Proyek ini dilindungi di bawah lisensi [MIT License](LICENSE).
