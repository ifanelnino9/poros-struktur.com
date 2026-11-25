# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Poros Struktur Engineering

## Tujuan
- Website statis jasa perhitungan struktur "Poros Struktur Engineering".
- Dibuat dengan Claude chat, pemilik hanya meneruskan instruksi. Jelaskan semua hal dalam Bahasa Indonesia yang sederhana.

## Stack
- HTML, CSS, dan JS inline di dalam file HTML. Tanpa build, tanpa framework, tanpa dependensi.
- Font Google Fonts dan link `wa.me` (WhatsApp) bersifat eksternal.
- Hosting: Cloudflare Workers (static assets), deploy otomatis dari GitHub.

## Struktur file
- `index.html` – halaman utama (~110 KB).
- `alur-kerja.html` – alur kerja dan ketentuan pengerjaan (~12 KB).
- `images/` – `hasil-kerja-1.jpg` sampai `hasil-kerja-6.jpg`, dipakai galeri di `index.html`. Folder ini ikut dipublikasikan (tidak ada di `.assetsignore`).
- `wrangler.toml` – konfigurasi deploy: `name = "poros-struktur"`, `[assets] directory = "."`.
- `.assetsignore` – file yang tidak ikut dipublikasikan (wrangler.toml, .git, .wrangler, node_modules, README.md, CLAUDE.md, dll.).
- `.gitignore` – node_modules, .wrangler, .env*, .dev.vars, log, file sampah OS.
- `README.md` – deskripsi singkat, sudah menyebut Cloudflare Workers.

## Arsitektur halaman
- Tidak ada build, lint, atau test. Memeriksa perubahan berarti membuka halaman (`npx wrangler dev`) dan melihatnya.
- Setiap halaman berdiri sendiri: CSS di satu blok `<style>`, tidak ada file CSS atau JS terpisah. Kedua halaman punya dua blok `<script>` di awal `<head>`: tag Google (GTM-5CWZ8GB8) dan pelacak konversi Google Ads (lihat bagian Google Ads). `alur-kerja.html` tidak memakai JS lain. `index.html` punya satu blok `<script>` kecil di dekat akhir file, dan satu blok JSON-LD `ProfessionalService` (schema.org) di `<head>`.
- `index.html` adalah satu halaman panjang dengan bagian ber-`id`, berurutan: `paket`, `hasil-kerja`, `cocok`, `diterima`, `layanan`, `tentang`, `metode`, `contoh`, `alur`, `faq`. Menu dan tombol di atas mengacu ke id ini, jadi mengubah id berarti memeriksa semua tautan `#...`.
- Kontak hanya lewat WhatsApp. Nomor `6282143021058` tertulis langsung di banyak tautan `wa.me` (dengan teks pesan berbeda per tombol) di `index.html` dan `alur-kerja.html`. Mengganti nomor harus mengubah semua tautan itu, tidak ada satu tempat konfigurasi.
- Dua halaman saling terhubung lewat `alur-kerja.html` ↔ `index.html` (tautan relatif).
- Font Archivo dan Poppins dimuat dari Google Fonts. Ikon situs berupa SVG inline (data URI).

## Deploy dan Git
- Remote: `https://github.com/ifanelnino9/poros-struktur.com.git`, branch `main`. (Nama repositori lama `poros-struktur-R.1` sudah diganti; alamat lama masih dialihkan GitHub, tapi sebaiknya `git remote set-url origin` ke alamat baru.)
- Kalau ada perubahan yang di-commit lewat editor web GitHub, jalankan `git pull` dulu di folder lokal sebelum mengedit atau push lagi.
- Setiap `git push` ke `main` memicu deploy ulang otomatis (`npx wrangler deploy`).
- Situs live: https://poros-struktur.com (juga https://www.poros-struktur.com).
- Alamat cadangan: https://poros-struktur.ifanelnino.workers.dev
- Domain dibeli lewat Cloudflare Registrar dan dipasang ke Worker `poros-struktur` lewat dashboard (Workers & Pages, poros-struktur, Domains, Custom Domains). Tidak ada pengaturan domain di `wrangler.toml`.
- Identitas Git: `ifanelnino`.
- Claude tidak bisa login GitHub. Pemilik menjalankan `git push` sendiri di terminalnya.

## Keputusan penting
- Error awal "Could not detect a directory containing static files" disebabkan tidak adanya `wrangler.toml`. Solusinya `[assets] directory = "."`.
- `name` di wrangler.toml harus sama dengan nama Worker di Cloudflare (`poros-struktur`). Kalau beda, Workers Builds memberi peringatan.
- `.assetsignore` wajib ada, karena direktori aset adalah root. Tanpa itu `.git` dan `wrangler.toml` ikut terupload dan bisa diakses publik.
- Tambahkan setiap file internal baru ke `.assetsignore`.
- `/alur-kerja.html` dilayani sebagai `/alur-kerja` (redirect 307). Ini normal.
- Peringatan `workers_dev` dan `preview_urls` di log tidak berbahaya.

## Aturan kerja dari pemilik
- Sebelum mengubah file, tampilkan daftar file dan alasannya.
- Jangan hapus file tanpa izin.
- Jangan tulis atau tampilkan token, password, atau API key. Jika ada yang ditemukan, beri tahu pemilik dan jangan masukkan ke commit.
- Jangan push tanpa konfirmasi.
- Jangan ubah isi HTML, CSS, atau JS kecuali diminta.
- Akhiri tugas dengan ringkasan: penyebab, file yang diubah, hasil verifikasi, langkah berikutnya.

## Google Ads
- Akun Google Ads: `AW-18573158448`. Kampanye Search "Search - Jasa Hitung Struktur - Jatim Jabodetabek", 17 Nov sampai 30 Nov 2026.
- Konversi utama: "Klik WhatsApp" (label `Nj-ICL2e5LodELDQr5hF`), kategori Meminta penawaran harga.
- Pelacak: satu skrip di `<head>` kedua halaman, memakai satu pendengar klik untuk semua tautan `wa.me` dan mengirim `gtag('event','conversion',...)`. Tombol WhatsApp baru otomatis ikut terlacak, tidak perlu menambah atribut apa pun.
- Konversi lama "Kontak" sudah dihapus karena tidak pernah menerima data.
- Sitelink kampanye: Paket Harga (`/#paket`), Hasil Kerja (`/#hasil-kerja`), FAQ (`/#faq`), Alur Kerja (`/alur-kerja`). Kalau id bagian atau alamat halaman berubah, sitelink ikut diperbarui.

## Status
- Selesai: deploy berhasil, `/` dan `/alur-kerja` mengembalikan 200, file internal mengembalikan 404.
- Belum dicek: tampilan visual di browser dan HP, tombol WhatsApp, link antar-halaman.
- Selesai: tag Google dan pelacak klik WhatsApp online (dicek 18 Nov 2026). Belum dicek: pencatatan konversi "Klik WhatsApp" di Google Ads (butuh sampai 48 jam) dan pembayaran iklan.
- Selesai: domain `poros-struktur.com` dan `www.poros-struktur.com` terpasang, `/` dan `/alur-kerja` mengembalikan 200 di kedua alamat.

## Cara menjalankan lokal
```bash
npx wrangler dev
```
Atau buka `index.html` langsung di browser. Cek hasil deploy dengan `npx wrangler deploy --dry-run`.
