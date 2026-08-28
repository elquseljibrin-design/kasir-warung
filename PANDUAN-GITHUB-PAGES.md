# Panduan Hosting Frontend ke GitHub Pages (PWA Offline Sungguhan)

## Kenapa pindah dari Blogger?
Blogger tidak bisa menghosting file `manifest.json` dan `service-worker.js` sebagai file mentah di domain sendiri (same-origin), padahal service worker **wajib** satu origin dengan halamannya. GitHub Pages bisa, gratis, dan mendukung HTTPS otomatis — jadi aplikasi bisa benar-benar diinstal dan tetap **terbuka saat offline** (tampilan/app shell-nya; transaksi tetap butuh internet karena datanya live di Google Sheets).

## Isi paket ini
```
index.html          -> aplikasi (hasil konversi dari template Blogger, sudah tanpa syntax khusus Blogger)
manifest.json        -> identitas PWA (nama, ikon, warna, mode standalone)
service-worker.js    -> caching app shell supaya bisa dibuka offline
icons/icon-192.png   -> ikon aplikasi
icons/icon-512.png   -> ikon aplikasi (resolusi tinggi)
icons/apple-touch-icon.png -> ikon untuk iOS "Add to Home Screen"
.nojekyll            -> supaya GitHub Pages tidak memproses file lewat Jekyll
```

## Langkah Setup

### 1. Buat Repository GitHub
1. Buka [github.com](https://github.com), login/daftar akun (gratis).
2. Klik **+ > New repository**.
3. Nama repo bebas, misal `kasir-warung`. Set **Public**. Centang "Add a README" boleh dicentang atau tidak. Klik **Create repository**.

### 2. Upload File
**Cara termudah (tanpa command line):**
1. Di halaman repo, klik **Add file > Upload files**.
2. Drag & drop semua file dari paket ini (termasuk folder `icons` — GitHub akan otomatis membuat foldernya saat Anda drag folder, atau upload isinya satu-satu ke path `icons/`).
3. Scroll bawah, klik **Commit changes**.

**Atau pakai command line (kalau familiar dengan git):**
```bash
git clone https://github.com/USERNAME/kasir-warung.git
cd kasir-warung
# salin semua file dari paket ini ke folder ini
git add .
git commit -m "Setup aplikasi kasir warung"
git push
```

### 3. Aktifkan GitHub Pages
1. Di repo, buka tab **Settings > Pages** (menu kiri).
2. Di bagian **Build and deployment > Source**, pilih **Deploy from a branch**.
3. Branch: pilih **main** (atau `master`), folder **/ (root)**. Klik **Save**.
4. Tunggu 1-2 menit, refresh halaman — akan muncul URL seperti:
   `https://USERNAME.github.io/kasir-warung/`

### 4. Sambungkan ke Backend Apps Script
1. Buka file `index.html` di repo GitHub (klik file-nya, lalu ikon pensil **Edit**).
2. Cari baris:
   ```js
   const API_URL = 'PASTE_URL_WEB_APP_APPS_SCRIPT_ANDA_DI_SINI';
   ```
3. Ganti dengan URL Web App Apps Script Anda (yang sama seperti sebelumnya di Blogger).
4. Klik **Commit changes**.

### 5. Coba Buka & Instal
1. Buka `https://USERNAME.github.io/kasir-warung/` di HP (Chrome/Android atau Safari/iOS).
2. Login seperti biasa.
3. Di Android/Chrome: akan muncul tombol **⬇️ Pasang Aplikasi** (pojok kanan bawah) — tap untuk instal ke layar utama, aplikasi akan terbuka fullscreen seperti app asli.
4. Di iPhone/Safari: tap ikon **Share (kotak dengan panah ke atas) > Add to Home Screen**.
5. Coba matikan internet sebentar setelah membuka aplikasi sekali — halaman tetap bisa terbuka (app shell dari cache), meskipun data produk/transaksi baru tetap perlu online.

## Setelah ini, mana yang jadi frontend utama?
Anda bisa pakai **salah satu** (GitHub Pages atau Blogger) atau keduanya sekaligus — backend Apps Script yang sama bisa dipanggil dari mana saja. Kalau ingin fokus satu saja, GitHub Pages lebih disarankan karena mendukung PWA offline sungguhan.

## Update aplikasi di kemudian hari
Setiap kali saya (atau Anda) mengubah `index.html`, cukup upload ulang file yang berubah ke repo (replace file yang sama) lalu commit — GitHub Pages otomatis publish versi terbaru dalam 1-2 menit. Kalau ada perubahan berarti pada file, saya akan sarankan menaikkan angka `CACHE_NAME` di `service-worker.js` (misal dari `kasir-warung-v1` jadi `kasir-warung-v2`) supaya pengguna yang sudah install otomatis dapat versi terbaru, bukan versi lama dari cache.
