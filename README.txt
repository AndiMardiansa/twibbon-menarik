TWIBBON PIXEL GENERATOR V3

Yang direvisi:
- Tutorial berada di HOME, tepat di atas Upload Foto.
- Video twibbon sudah bawaan developer: assets/twibbon.mp4
- Video tutorial sudah bawaan developer: assets/tutorial.mp4
- Chroma key hanya diatur developer.
- Chroma key memakai HSV + green dominance + feather + spill suppression.
- Hasil chroma ditempatkan di canvas sementara agar foto tidak menjadi blank.
- Audio asli dari video twibbon ikut direkam.
- Export menunggu event video ended sehingga mengikuti durasi penuh.
- Hasil akhir dikonversi ke MP4 dengan FFmpeg WASM.

PENTING:
Ganti file placeholder:
assets/twibbon.mp4
assets/tutorial.mp4

Atur PHOTO_AREA dan CHROMA di bagian DEVELOPER SETTINGS pada index.html.

Untuk penggunaan online, host melalui HTTPS/localhost agar import FFmpeg WASM dan MediaRecorder berjalan normal.
