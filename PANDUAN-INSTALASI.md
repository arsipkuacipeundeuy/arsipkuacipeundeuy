# Panduan pemasangan Arsip KUA Cipeundeuy

Paket ini merupakan aplikasi terpisah dari versi ChatGPT Site sebelumnya. Data pada versi sebelumnya tidak dipindahkan otomatis. Tidak ada password admin bawaan. Anda membuat akun admin sendiri di Supabase.

## 1. Siapkan proyek Supabase

1. Buka https://supabase.com/dashboard dan buat **New project** khusus arsip kantor. Gunakan proyek baru agar tidak bertabrakan dengan tabel website KUA.
2. Simpan password database di tempat aman. Password database tidak dimasukkan ke aplikasi.
3. Tunggu proyek selesai disiapkan.
4. Buka **SQL Editor → New query**.
5. Buka file `supabase/01_setup.sql` dari ZIP dengan Notepad/editor teks. Salin seluruh isinya ke SQL Editor, lalu **Run**.
6. Pastikan berhasil. Script ini dijalankan **sekali** pada proyek baru, bukan berulang-ulang. Jika sudah berhasil, lanjutkan ke tahap berikutnya.

SQL membuat tabel `profiles`, `records`, `attachments`, `audit_logs`, bucket privat `kua-arsip`, serta aturan hak akses di server.

## 2. Buat admin pertama

1. Buka **Authentication → Users → Add user → Create new user**.
2. Isi email admin dan kata sandi kuat minimal delapan karakter. Gunakan email yang dapat diakses untuk pemulihan kata sandi.
3. Konfirmasi email melalui opsi Auto Confirm jika tersedia, atau selesaikan verifikasi email.
4. Buka `supabase/02_admin_pertama.sql`.
5. Ganti `GANTI_EMAIL_ADMIN_ANDA` dengan email yang baru dibuat, tanpa menghapus tanda petik.
6. Jalankan script itu di **SQL Editor**.
7. Hasil harus menampilkan akun dengan `role=admin` dan `active=true`.
8. Jalankan `supabase/03_pemeriksaan.sql` bila ingin memeriksa RLS, bucket, dan daftar akun.

Akun baru selain admin pertama berstatus nonaktif. Aktivasi melalui menu **Akun & hak akses** setelah admin masuk.

## 3. Isi config.js

1. Cari **Project URL** dan **Publishable key** melalui **Connect** atau **Project Settings → API / API Keys**.
2. Buka `config.js` dan ubah dua nilai berikut:

```js
window.KUA_CONFIG = {
  supabaseUrl: 'https://PROJECT-ANDA.supabase.co',
  supabaseKey: 'sb_publishable_KUNCI_ANDA',
  officeName: 'KUA Cipeundeuy',
  region: 'Kabupaten Subang'
};
```

Publishable key memang dirancang untuk aplikasi browser; hak akses data ditentukan login dan Row Level Security. **Jangan menaruh secret key, service_role, password database, atau password akun pada config.js maupun GitHub.** Kunci `anon` lama juga didukung bila proyek masih menggunakannya.

## 4. Unggah ke GitHub

### Cara paling mudah: Deploy from a branch

1. Ekstrak ZIP, masuk ke folder `kua-arsip-full`.
2. Buat repository GitHub baru, misalnya **arsip-kua-cipeundeuy**. Untuk GitHub Free, gunakan repository Public jika memakai GitHub Pages.
3. Pilih **Add file → Upload files**.
4. Unggah **isi folder**, sehingga `index.html` dan `config.js` langsung berada di akar repository, bukan di dalam folder tambahan.
5. Berkas minimum yang wajib diunggah: `index.html`, `config.js`, dan seluruh folder `assets`. Panduan, SQL, dan tests boleh ikut diunggah; tidak mengandung data kantor atau kunci rahasia.
6. Commit ke branch `main`.
7. Buka **Settings → Pages**.
8. Pada **Build and deployment → Source**, pilih **Deploy from a branch**.
9. Pilih branch **main**, folder **/(root)**, lalu **Save**.
10. Tunggu proses selesai. GitHub menampilkan alamat situs, umumnya `https://USERNAME.github.io/arsip-kua-cipeundeuy/`.

Jika folder `.github` tidak terbawa saat unggah lewat browser, cara branch di atas tetap bekerja. Jika semua file disalin melalui Git, pilih salah satu cara deploy: branch atau Actions, bukan keduanya sekaligus.

### Alternatif: GitHub Actions

Jika `.github/workflows/deploy.yml` ikut diunggah, buka **Settings → Pages → Source → GitHub Actions**. Buka tab **Actions**, pilih workflow **Deploy Arsip KUA ke GitHub Pages**, lalu **Run workflow**. Workflow hanya menerbitkan file aplikasi (`index.html`, `config.js`, `assets`, `.nojekyll`).

Jika folder tersembunyi `.github` tidak terlihat saat ekstrak, aktifkan tampilan hidden files atau buat file workflow lewat **Add file → Create new file**, dengan nama `.github/workflows/deploy.yml`.

Tidak perlu menjalankan npm install atau memasang server Node.js untuk hosting.

## 5. Atur login dan pemulihan password

1. Di Supabase buka **Authentication → URL Configuration**.
2. Isi **Site URL** dengan alamat GitHub Pages lengkap, termasuk nama repository dan garis miring terakhir.
3. Tambahkan alamat yang sama pada **Redirect URLs**. Bila memakai domain khusus nanti, tambahkan alamat domain itu juga.
4. Pada pengaturan penyedia **Email**, pastikan login email/password aktif.
5. Nonaktifkan pendaftaran publik (**Allow new users to sign up**) karena akun dibuat oleh admin melalui dashboard Supabase. Aplikasi tidak menyediakan form pendaftaran publik.
6. Untuk pengiriman reset password operasional, konfigurasikan SMTP pada Supabase dan uji email pemulihannya.
7. Buka situs, masuk memakai email/password admin pertama.

Pengaturan nama menu dapat sedikit berubah di dashboard Supabase. Tujuan pengaturannya adalah login password aktif dan URL reset mengarah kembali ke situs yang benar.

## 6. Tambahkan petugas

1. Buat akun email/password petugas di **Supabase → Authentication → Users → Add user**.
2. Masuk ke aplikasi sebagai admin, buka **Akun & hak akses**.
3. Klik **Atur akses** pada akun baru.
4. Isi nama, pilih **petugas**, centang **Akun aktif**, dan pilih modul yang diperlukan.
5. Simpan.

Akses kepegawaian diberikan hanya kepada petugas yang memerlukannya. Admin dapat menonaktifkan petugas dari menu yang sama. Perubahan akses berlaku pada database segera; tampilan akun bersangkutan diperiksa ulang setiap satu menit.

Hanya admin yang dapat memindahkan arsip ke sampah, memulihkan, mengimpor, atau mengatur akses. Admin aktif terakhir tidak dapat dinonaktifkan/dijadikan petugas melalui aplikasi.

## 7. Uji sebelum memasukkan arsip asli

- Masuk sebagai admin dan buat satu catatan di setiap modul.
- Unggah PDF/foto contoh, lalu unduh kembali dan buka hasilnya.
- Muat ulang halaman dan pastikan data tetap tersedia.
- Uji pencarian nama, NIP, nomor surat, dan kode barang.
- Uji filter tanggal, ekspor CSV, dan cetak/simpan PDF.
- Pindahkan satu arsip ke sampah lalu pulihkan.
- Buat akun petugas khusus Buku tamu. Pastikan menu Kepegawaian tidak muncul dan arsip kepegawaian tidak terbaca oleh akun itu.
- Coba akun nonaktif: harus berhenti di halaman menunggu aktivasi.
- Uji reset password melalui email.

CSV dapat dibuka dengan Excel. Jika kolom tidak terpisah otomatis, gunakan **Data → From Text/CSV**, pilih UTF-8 dan delimiter titik koma (;).

## 8. Cadangan dan pemulihan

**CSV:** laporan data aktif sesuai filter; bukan cadangan lampiran.

**Cadangan JSON:** data aktif yang sesuai filter dan metadata lampiran. Isi PDF/foto tidak masuk ke JSON. Untuk cadangan data lengkap gunakan filter semua modul/tanggal. Untuk impor aplikasi, buat bagian cadangan maksimal 500 arsip per file, dengan mempersempit rentang tanggal bila diperlukan.

**Impor JSON:** hanya admin; penambahan data baru dengan ID baru, tanpa mengubah data lama dan tanpa memulihkan lampiran. Jangan mengimpor file yang sama berulang karena menambah duplikat. Jika terjadi impor keliru, arsip dapat dipindahkan ke sampah.

**Cadangan dokumen:** unduh lampiran dari detail arsip dan simpan di penyimpanan kantor terpisah. Untuk cadangan operasional skala besar, cadangkan database Supabase beserta seluruh isi bucket `kua-arsip` melalui mekanisme backup proyek/storage yang dikelola kantor. Backup database saja tidak mencakup isi file Storage.

Tempat sampah mempertahankan metadata dan lampiran. Setelah arsip dipulihkan, lampiran bisa diunduh kembali. Paket ini tidak menyediakan penghapusan permanen.

## 9. Domain khusus (opsional setelah uji berhasil)

Untuk memakai `arsip.kuacipeundeuy.my.id`, atur **Custom domain** di GitHub Pages dan DNS CNAME pada pengelola domain. Jangan mengganti DNS website utama. Tambahkan domain baru pada Site URL/Redirect URLs di Supabase dan aktifkan HTTPS setelah verifikasi domain selesai. Domain belum disambungkan di dalam paket ini.

## Penanganan kendala

| Kendala | Pemeriksaan |
|---|---|
| “Aplikasi siap dikonfigurasi” | Isi config.js, commit, tunggu deploy, muat ulang halaman |
| Gagal terhubung | Pastikan URL/key dari proyek yang sama dan status proyek aktif |
| Invalid login credentials | Pastikan akun ada di Authentication, email/password benar dan email terkonfirmasi |
| Akun menunggu aktivasi | Jalankan 02_admin_pertama.sql untuk admin atau aktifkan lewat menu admin |
| Tabel profiles tidak ditemukan | Pastikan 01_setup.sql berhasil pada proyek yang sama |
| Permission denied / RLS | Periksa role, active, dan daftar modules; jangan menonaktifkan RLS |
| Upload gagal | Periksa bucket privat kua-arsip, MIME PDF/JPG/PNG, ukuran di bawah 10 MB dan kuota proyek |
| Reset password salah halaman | Periksa Site URL, Redirect URLs, dan pengaturan SMTP |
| Situs 404 | index.html harus di akar branch main; periksa pengaturan Pages |
| Tampilan lama | Tunggu deployment selesai, lalu Ctrl+F5 |

## Rujukan teknis

- Supabase RLS: https://supabase.com/docs/guides/database/postgres/row-level-security
- Supabase Storage: https://supabase.com/docs/guides/storage/security/access-control
- Supabase password: https://supabase.com/docs/guides/auth/passwords
- API keys: https://supabase.com/docs/guides/getting-started/api-keys
- GitHub Pages: https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages
