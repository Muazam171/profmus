# Portofolio Prof Mustang | MA CNWK

Website portofolio statis dengan latar bintang, judul kaca (WebGL), viewer model 3D, pemutar musik, form login demo, dan kolom komentar komunitas.

## Struktur Folder

```
.
├── index.html              # Halaman utama (markup saja)
├── css/
│   └── style.css           # Seluruh gaya tampilan
├── js/
│   ├── stars.js            # Animasi latar bintang (canvas)
│   ├── model-assets.js     # Data model 3D + tekstur (base64, ±5,8 MB)
│   ├── viewer-3d.js        # Viewer 3D (Three.js + OBJLoader)
│   ├── glass-headline.js   # Efek judul kaca WebGL2
│   ├── neuro-noise.js      # Shader latar hero (Neuro Noise)
│   ├── auth.js             # Login / Sign Up demo (localStorage)
│   └── lightbox.js         # Perbesar foto finalis DBL
├── assets/
│   ├── community.js        # Komentar komunitas (memanggil backend) + navbar kaca
│   ├── dbl-photo.jpg       # Foto finalis DBL
│   ├── basketball-event-history.png
│   ├── certificate-empty.svg
│   └── project-*.svg       # Gambar proyek
├── netlify.toml            # Konfigurasi Netlify
└── .netlify/               # Migrasi database (tabel portfolio_comments)
```

## Menjalankan Secara Lokal

Cukup buka `index.html` di browser, atau jalankan server statis:

```bash
npx serve .
# atau
python3 -m http.server 8000
```

Butuh koneksi internet untuk memuat Google Fonts dan Three.js (CDN).

## Urutan Script

Urutan `<script>` di akhir `index.html` penting dan tidak boleh diubah:

1. `stars.js`
2. `model-assets.js` (mendefinisikan `window.__ASSETS`)
3. Three.js dan OBJLoader (CDN)
4. `viewer-3d.js` (memakai `window.__ASSETS` dan `THREE`)
5. `glass-headline.js`, `neuro-noise.js`, `auth.js`, `lightbox.js`
6. `assets/community.js` (`defer`)

## Deploy ke Netlify

- Build command: `node scripts/prepare-site.mjs` (sesuai `netlify.toml`)
- Node: 24
- Fitur komentar memakai Netlify Database. Migrasi ada di `.netlify/internal/db/migrations/`:

```sql
CREATE TABLE "portfolio_comments" (
  "id" serial PRIMARY KEY,
  "name" varchar(60) NOT NULL,
  "body" varchar(1000) NOT NULL,
  "created_at" timestamp with time zone DEFAULT now() NOT NULL
);
```

> Catatan: `netlify.toml` merujuk ke `scripts/prepare-site.mjs` dan folder `public`, yang tidak ada di ZIP ini. Sesuaikan atau hapus bagian itu jika deploy dari folder ini langsung.

## Catatan

- Login/Sign Up hanya demo: akun disimpan di `localStorage` (kata sandi di-hash PBKDF2), tanpa server.
- Foto DBL dan aset 3D sebelumnya tertanam di `index.html` (±6,3 MB). Sekarang dipisah supaya HTML hanya ±21 KB.
