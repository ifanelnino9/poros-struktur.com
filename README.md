# Poros Struktur Engineering

Website statis jasa perhitungan struktur. Tanpa build, tanpa dependensi.

- `index.html` – halaman utama
- `alur-kerja.html` – alur kerja dan ketentuan pengerjaan
- `images/` – gambar galeri hasil kerja

Situs live: https://poros-struktur.com

Deploy: Cloudflare Workers (static assets), konfigurasi di `wrangler.toml`. Setiap `git push` ke `main` memicu deploy ulang otomatis. Jalankan lokal dengan `npx wrangler dev`.
