# Hasil pengujian paket 1.0.0

Tanggal: 16 November 2026.

## Berhasil diperiksa

- Pemeriksaan sintaks JavaScript aplikasi dan konfigurasi data.
- Delapan uji otomatis: tanggal kalender, enam jenis arsip, jumlah BMN, luas/nilai/tahun, impor tanpa membawa identitas/pemilik/lampiran lama, batas impor, ekspor CSV, dan escaping HTML.
- SQL setup dijalankan pada mesin PostgreSQL lokal berbasis PGlite dengan skema Auth/Storage uji.
- Tujuh belas pemeriksaan akses/validasi database: akun admin/petugas/nonaktif/anonim, pembatasan modul, larangan eskalasi role, perlindungan admin terakhir, unggah/lampiran privat, larangan penghapusan berkas terdaftar, pemindahan/pemulihan arsip, serta penolakan jumlah BMN nol.
- Uji alur antarmuka dengan mock Supabase: dashboard enam modul, pembukaan enam formulir, penyimpanan enam jenis arsip, filter laporan, dan formulir hak akses.
- Pemeriksaan visual di browser: dashboard dan formulir BMN/lampiran.

## Batas verifikasi

Konfigurasi proyek Supabase pengguna belum diberikan. Karena itu login, unggah/unduh file ke Storage asli, email reset password, deployment GitHub Pages, serta DNS domain pengguna belum diuji langsung. Uji tersebut dilakukan setelah URL dan publishable key diisi, SQL dipasang, dan akun admin diaktifkan.

Data uji dan mock tidak disertakan pada aplikasi produksi. Paket tidak mengandung akun admin, password, secret key, atau data kantor. File config.js masih berupa placeholder yang wajib diisi.

Cadangan JSON hanya metadata/data, tidak mencakup isi PDF/foto. Impor membuat arsip baru tanpa lampiran. Aplikasi tidak terhubung otomatis dengan versi ChatGPT Site sebelumnya atau sistem BMN eksternal.
