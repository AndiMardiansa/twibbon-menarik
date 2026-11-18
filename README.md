# Twibbon Pixel Generator — "MENARIK" Season 5

Generator twibbon (bingkai foto bernyawa) berbasis web. Pengguna mengunggah foto, foto disisipkan ke dalam video twibbon hijau (green screen), lalu hasilnya direkam menjadi video MP4/WebM atau PNG. Semua berjalan di browser — tanpa server, tanpa build step.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)

## ✨ Fitur

- Pemilihan & penempatan foto di atas video twibbon (zoom / geser)
- Chroma key otomatis: HSV + green dominance + feather + spill suppression
- Audio asli video twibbon ikut direkam
- Preview mini (PNG) bagi pengguna yang belum memuat video besar
- Export video 30 detik dengan durasi penuh
- Export MP4 lewat FFmpeg WASM; di perangkat mobile langsung memakai blob WebM agar tidak berat
- Download PNG hasil frame twibbon
- Caption yang bisa dibuka / disalin, tombol tutorial di halaman utama

## 🚀 Cara Menjalankan

Aplikasi hanya bekerja lewat **HTTPS atau localhost** (bukan `file://`), karena FFmpeg WASM diimpor dari CDN dan MediaRecorder butuh konteks aman.

Buka `index.html` lewat static host, misalnya:

```bash
npx serve .
# atau deploy ke Vercel / Netlify
```

Konfigurasi static host sudah disiapkan:

| Host | File |
| ---- | ---- |
| Vercel | `vercel.json` |
| Netlify | `_headers` |
| Apache | `.htaccess` |

> Nilai cache di ketiga file bisa berbeda — jaga tetap sinkron. HTML disajikan dengan revalidate (`max-age=0, must-revalidate`); aset statis dengan cache immutable. Aset yang diubah biasanya diberi nama versi baru agar cache lama tidak menampilkan konten basi.

## 📁 Struktur File

```
index.html            — seluruh aplikasi (HTML + CSS + JS), satu file, tanpa bundler
assets/twibbon.mp4    — video twibbon hijau (placeholder developer)
assets/tutorial.mp4   — video tutorial di halaman utama (placeholder)
assets/twibbon4.mp4   — fallback bila twibbon.mp4 gagal dimuat
assets/twibbon1.png   — preview PNG twibbon (pratinjau cepat)
assets/overlay.png    — overlay / bingkai yang digambar di atas video
assets/twibbon.mp3    — audio twibbon
font/                 — font pixel lokal via @font-face
text.txt              — file coretan (snippet), bukan bagian dari aplikasi
```

## 🛠️ Kustomisasi (untuk Developer)

Semua pengaturan ada di **"DEVELOPER SETTINGS"**, konstanta di awal `<script>` `index.html` (sekitar baris 911):

- `TWIBBON_VIDEO` / `FALLBACK_TWIBBON_VIDEO` / `TWIBBON_PNG_PREVIEW` / `TWIBBON_OVERLAY_PNG` — path aset yang dipakai
- `CHROMA` — parameter keying hijau (r, g, b, intensity, shadow, feather, cleanup)
- `PHOTO_AREA` — `{ x, y, w, h }` area foto di dalam canvas 1080×1350
- `EXPORT_DURATION` — durasi target video export (30 detik)
- `IS_MOBILE` — mendeteksi perangkat mobile; bila `true`, export memakai blob WebM langsung (tanpa konversi FFmpeg)

> **PENTING:** ganti file placeholder `assets/twibbon.mp4` dan `assets/tutorial.mp4` dengan video asli (green screen) milik pihak pengelola.

## 📝 Catatan Teknis

- Seluruh kode ada di `index.html` — jangan dipecah menjadi modul (tanpa bundler).
- Teks antarmuka memakai bahasa Indonesia.
- Penghentian rekaman memakai timer dinding (`EXPORT_DURATION × 1000`) plus event `ended` video — ini perbaikan khusus iPhone (hang & durasi salah), jangan disederhanakan.

## 👤 Author

**Andi Mardiansa** — [@andimardiansa](https://www.instagram.com/andimardiansa) · [LinkedIn](https://www.linkedin.com/in/andi-mardiansa-90661532b) · [GitHub](https://github.com/AndiMardiansa)
