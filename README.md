# Arsip KUA Cipeundeuy

Aplikasi siap konfigurasi dan unggah ke GitHub Pages. Penyimpanan dan login menggunakan proyek Supabase milik kantor. Aplikasi ini tidak memerlukan login ChatGPT, Node.js, atau proses build untuk digunakan.

**Mulai dari [PANDUAN-INSTALASI.md](PANDUAN-INSTALASI.md).**

## Fitur

| Modul | Isi utama |
|---|---|
| Buku tamu | Tanggal, nama, alamat/instansi, kontak opsional, keperluan, petugas |
| Surat masuk | Agenda, nomor/tanggal, pengirim, perihal, disposisi, tenggat |
| Surat keluar | Agenda, nomor/tanggal, penerima, perihal, penandatangan, pengiriman |
| Kepegawaian | Nama, NIP, jabatan, pangkat/golongan, status, jenis dokumen |
| Bangunan & tanah | Lokasi, luas, kepemilikan, sertifikat, tahun, kondisi, nilai |
| BMN / SARPRAS | Kode barang, NUP, merek/seri, jumlah, satuan, kondisi, lokasi, nilai |

Semua modul: tambah/edit/detail, lampiran PDF/JPG/PNG privat, pencarian, filter tanggal/status, paginasi, ekspor CSV untuk Excel, cetak/simpan PDF. Lampiran maksimal 10 MB per berkas, lima berkas per penyimpanan; arsip dapat ditambah lampiran lagi lewat Edit.

Admin: semua modul, aktivasi akun, hak akses per modul, tempat sampah/pemulihan, riwayat aktivitas, impor JSON. Petugas: baca/tambah/edit/unduh pada modul yang diberikan; tidak dapat mengubah hak akses atau memindahkan arsip ke tempat sampah. Tidak ada fitur hapus permanen.

Cadangan JSON mencakup data aktif yang sesuai filter dan metadata lampiran, bukan isi file. Impor membuat arsip baru tanpa lampiran; bukan pemulihan database penuh. Petunjuk pencadangan berkas tersedia pada panduan.

## Berkas

- `index.html`, `config.js`, `assets/`: aplikasi web.
- `supabase/01_setup.sql`: tabel, fungsi, RLS, audit, dan bucket privat.
- `supabase/02_admin_pertama.sql`: aktivasi admin pertama.
- `supabase/03_pemeriksaan.sql`: pemeriksaan instalasi.
- `.github/workflows/deploy.yml`: deploy otomatis GitHub Pages.
- `.nojekyll`: dukungan hosting statis.
- `tests/core.test.js`: uji validasi/impor/ekspor.
- `LAPORAN-PENGUJIAN.md`: hasil pengujian dan batas verifikasi.

## Pengujian pengembang (opsional)

Dengan Node.js 20+:

```sh
npm test
npm run check
```

Aplikasi tidak memakai paket npm saat berjalan. Supabase JS sudah disertakan lokal, versi 2.49.8, lisensi MIT. Rujukan ada pada `THIRD-PARTY-NOTICES.md`.

Versi paket: 1.0.0 · 16 November 2026.
