# Microgreens by Ucops — QR Product Info Website

Website info produk microgreens, diakses lewat QR code di packaging.

## Struktur Proyek

```
ucops-microgreens/
├── data/
│   └── products.json          ← Semua data produk & batch di sini
├── public/
│   ├── index.html             ← Halaman produk (user-facing)
│   ├── qr-generator.html      ← Tool admin generate QR
│   └── css/
│       └── style.css
└── README.md
```

## Cara Pakai

### 1. Edit data produk

Buka `data/products.json`. Ini satu-satunya file yang perlu diedit untuk:
- Tambah produk baru
- Update info batch (tanggal tanam, panen, kadaluarsa)
- Ganti info nutrisi, tips penyimpanan, dll

Untuk **tambah batch baru** (misal panen baru), update field `batches[0]`:
```json
{
  "batchId": "SF-2025-003",
  "plantedDate": "2025-01-20",
  "harvestDate": "2025-01-30",
  "expiryDate": "2025-02-04",
  "daysToHarvest": 10,
  "weight": "100g",
  "notes": "catatan opsional"
}
```

### 2. URL produk

Format URL: `https://[domain]/?p=[slug]`

Contoh:
- `https://ucops.id/?p=sunflower` → halaman Sunflower
- `https://ucops.id/?p=radish` → halaman Radish
- `https://ucops.id/` → halaman index semua produk

### 3. Generate QR Code

Buka `qr-generator.html` di browser (butuh server lokal atau setelah deploy).
- Set base URL sesuai domain
- Pilih produk → klik QR → download PNG
- Bisa print semua sekaligus

---

## Deploy (Pilih Salah Satu)

### Option A: GitHub Pages (Gratis, Rekomen)

1. Buat repo di GitHub (misal: `ucops-microgreens`)
2. Upload semua file
3. Settings → Pages → Source: `main`, folder `/` atau `/public`
4. URL jadi: `https://[username].github.io/ucops-microgreens/?p=sunflower`

Kalau punya domain sendiri (ucops.id), tinggal set CNAME di GitHub Pages.

### Option B: Vercel / Netlify (Gratis, Lebih Mudah Custom Domain)

1. Connect repo GitHub ke Vercel/Netlify
2. Build command: kosong (static site)
3. Publish directory: `public`
4. Custom domain: ucops.id

### Option C: Lokal dulu (test)

```bash
cd public
python3 -m http.server 8080
# Buka: http://localhost:8080/?p=sunflower
```

---

## Tambah Produk Baru

1. Buka `data/products.json`
2. Copy salah satu entry di array `products`
3. Ganti `slug`, `name`, `emoji`, warna, info nutrisi, dll
4. Save → deploy ulang (kalau pakai GitHub, push aja)
5. Generate QR baru di `qr-generator.html`

---

## Roadmap / Ide Pengembangan

- [ ] Tambah halaman `/admin` simpel untuk edit batch tanpa buka JSON
- [ ] Email/WA reminder kalau produk hampir kadaluarsa
- [ ] Analytics — track berapa kali QR discan
- [ ] Multiple bahasa (id/en)
- [ ] Foto produk per batch

---

Made with 🌱 by Ucop · Jagakarsa, Jakarta Selatan
