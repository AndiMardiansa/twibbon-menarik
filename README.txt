TWIBBON PIXEL GENERATOR — "MENARIK" Season 5
============================================

Generator twibbon (bingkai foto bernyawa) berbasis web. Pengguna unggah foto,
foto disisipkan ke dalam video twibbon hijau (green screen), lalu hasilnya
direkam jadi video MP4/WebM atau PNG. Semua berjalan di browser — tanpa server,
tanpa build step.

Fitur
-----
- Pemilihan & penempatan foto di atas video twibbon (zoom/geser).
- Chroma key otomatis: HSV + green dominance + feather + spill suppression.
- Audio asli video twibbon ikut direkam.
- Preview mini (PNG) bagi pengguna yang belum memuat video besar.
- Export video 30 detik dengan durasi penuh (menunggu event ended).
- Export MP4 lewat FFmpeg WASM; di perangkat mobile langsung memakai blob WebM
  agar tidak berat.
- Download PNG hasil frame twibbon.
- Caption yang bisa dibuka/disalin, tombol tutorial di halaman utama.

Struktur file
-------------
- index.html        — seluruh aplikasi (HTML + CSS + JS), satu file, tanpa bundler.
- assets/twibbon.mp4   — video twibbon hijau (placeholder developer).
- assets/tutorial.mp4  — video tutorial di halaman utama (placeholder).
- assets/twibbon4.mp4  — fallback bila twibbon.mp4 gagal dimuat.
- assets/twibbon1.png  — preview PNG twibbon (pratinjau cepat).
- assets/overlay.png   — overlay/bingkai yang digambar di atas video.
- assets/twibbon.mp3   — audio twibbon.
- font/                — font pixel lokal via @font-face.
- text.txt             — file coretan (snippet), bukan bagian dari aplikasi.

Kustomisasi (untuk developer)
-----------------------------
Semua pengaturan di "DEVELOPER SETTINGS", konstanta di awal <script>
index.html (sekitar baris 911):

- TWIBBON_VIDEO / FALLBACK_TWIBBON_VIDEO / TWIBBON_PNG_PREVIEW / TWIBBON_OVERLAY_PNG
  — path aset yang dipakai.
- CHROMA — parameter keying hijau (r,g,b, intensity, shadow, feather, cleanup).
- PHOTO_AREA — { x, y, w, h } area foto di dalam canvas 1080×1350.
- EXPORT_DURATION — durasi target video export (30 detik).
- IS_MOBILE — mendeteksi perangkat mobile; bila true, export memakai blob WebM
  langsung (tanpa konversi FFmpeg).

PENTING: ganti file placeholder assets/twibbon.mp4 dan assets/tutorial.mp4
dengan video asli (green screen) milik pihak pengelola.

Cara menjalankan / men-deploy
-----------------------------
Aplikasi hanya bekerja lewat HTTPS atau localhost (bukan file://), karena
FFmpeg WASM diimpor dari CDN (jsdelivr) dan MediaRecorder butuh konteks aman.

Format static host sudah disiapkan di tiga tempat (nilai cache bisa berbeda,
jaga tetap sinkron):
- vercel.json  — Vercel
- _headers     — Netlify
- .htaccess    — Apache (mod_brotli/expires)

HTML disajikan dengan revalidate (max-age=0, must-revalidate); aset statis
dengan cache immutable. Aset yang diubah biasanya diberi nama versi baru agar
cache lama tidak menampilkan konten basi.

Catatan teknis
--------------
- Seluruh kode ada di index.html — jangan dipecah menjadi modul (tanpa bundler).
- Teks antarmuka memakai bahasa Indonesia.
- Penghentian rekaman memakai timer dinding (EXPORT_DURATION × 1000) plus event
  ended video — ini perbaikan khusus iPhone (hang & durasi salah), jangan disederhanakan.