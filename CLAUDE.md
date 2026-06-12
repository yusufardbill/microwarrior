# MicroWarrior - Microgreens Product Info System

## Overview
Single-page web app untuk menampilkan info produk microgreens dalam pot yang dijual hidup. Setiap pot punya QR code yang link ke halaman detail batch-nya. Fokus: tanaman hidup yang bisa dipanen bertahap oleh pembeli sendiri (bukan microgreens potong).

## Konsep Bisnis
**Jual tanaman microgreens hidup dalam pot**, bukan yang sudah dipotong:
- Pembeli bisa panen sendiri 2-4x dari satu pot
- Lebih fresh karena dipanen langsung sebelum makan
- Value proposition: dekorasi + edukasi + zero waste
- QR code di pot → scan → lihat info cara merawat & panen

## Tech Stack
- **Pure vanilla JS** - no framework, simple & lightweight
- **Static JSON files** - no database, mudah update manual
- **Responsive CSS** - mobile-first design
- **QR-code driven** - tiap pot punya URL unik via query param

## Struktur File

```
/public
├── index.html              # Single page app (product detail + index)
├── css/style.css           # All styling
├── data/
│   ├── config.json        # Brand info + master product catalog
│   ├── index.json         # List semua batch available
│   └── batches/
│       ├── {slug}_{date}.json    # Info per batch/pot
│       └── peas_20260612.json    # Example
└── qr-generator.html       # Tool untuk generate QR code
```

## Data Architecture

### config.json
Master data yang jarang berubah:
- Brand info (nama, lokasi, kontak)
- Product catalog (sunflower, radish, peas)
  - Deskripsi, emoji, warna
  - **Care** (cara merawat): penyiraman, cahaya, suhu, tips
  - **Harvesting** (cara panen): metode, frekuensi, yield, tips
  - Nutrisi
  - Usage (cocok untuk apa)

### batches/{slug}_{date}.json
Data per pot yang dijual (dibikin tiap kali tanam baru):
```json
{
  "productSlug": "peas",
  "batchId": "PS-2026-001",
  "plantedDate": "2026-06-12",
  "bestHarvestDays": "9-12",
  "potSize": "10cm",
  "growthMedium": "Cocopeat + kompos organik",
  "totalWeight": "250g (dengan pot & media)",
  "estimatedYield": "180-250g",
  "harvestDuration": "2-3 minggu (panen bertahap)",
  "currentStatus": "Siap panen",
  "notes": "Catatan opsional"
}
```

### index.json
List semua batch yang lagi dijual:
```json
{
  "batches": [
    {
      "id": "peas_20260612",
      "productSlug": "peas",
      "plantedDate": "2026-06-12"
    }
  ]
}
```

## URL Structure

- **Index/List**: `/?p=index` atau `/` → tampilkan semua batch available
- **Batch Detail**: `/?p={slug}_{date}` → detail satu pot
  - Example: `/?p=peas_20260612`
  - QR code di pot link ke URL ini

## Workflow: Tambah Batch Baru

1. **Tanam tanaman baru** (misal: sunflower, tanam 15 Juni 2026)

2. **Buat file batch**: `/public/data/batches/sunflower_20260615.json`
   ```json
   {
     "productSlug": "sunflower",
     "batchId": "SF-2026-002",
     "plantedDate": "2026-06-15",
     "bestHarvestDays": "8-12",
     "potSize": "12cm",
     "growthMedium": "Cocopeat + kompos organik",
     "totalWeight": "280g (dengan pot & media)",
     "estimatedYield": "150-200g",
     "harvestDuration": "1-2 minggu (2-3x panen)",
     "currentStatus": "Tumbuh sehat",
     "notes": "Batch musim kemarau"
   }
   ```

3. **Update index.json**: tambah entry baru
   ```json
   {
     "batches": [
       {
         "id": "sunflower_20260615",
         "productSlug": "sunflower",
         "plantedDate": "2026-06-15"
       },
       // ... batch lain
     ]
   }
   ```

4. **Generate QR code**:
   - Buka `qr-generator.html`
   - Input: `sunflower_20260615`
   - Download QR code
   - Print & tempel di pot

## Key Features

### Dynamic Info
- **Umur tanaman**: dihitung real-time dari `plantedDate` sampai hari ini
- **Waktu terbaik panen**: calculated dari `plantedDate` + `bestHarvestDays`
  - Format: "21 s/d 24 Juni 2026"
  
### No Expiry Date
Karena ini tanaman hidup (bukan yang sudah dipotong), gak ada expiry date. Pembeli urus sendiri kapan mau panen.

### Sections Displayed
1. **Info Batch**: umur, pot, media, berat, estimasi hasil
2. **Cara Merawat**: penyiraman, cahaya, suhu, tips
3. **Cara Panen**: metode, frekuensi, waktu, tips
4. **Nutrisi**: vitamin, mineral per 100g
5. **Cocok Untuk**: usage suggestions

## Design Principles

### Typography & Spacing
- Font size: 0.75-0.8rem untuk body (compact tapi readable)
- Line height: 1.45 (balance antara compact & legibility)
- Spacing: tight tapi gak cramped

### Colors
- Accent color per product (hijau variations)
- Consistent border & background colors
- Status badges dengan color coding

### Layout
- Mobile-first (max-width: 480px)
- Grid layout untuk batch info (2 columns)
- Card-based sections
- Sticky top bar

## CSS Classes Reference

### Batch Card
- `.batch-card` - main container
- `.batch-grid` - 2-col grid (6 items = 3 rows)
- `.batch-item` - each grid cell
- `.status-row` - current status with green gradient bg
- `.harvest-info` - harvest time & duration

### Care & Harvesting
- `.care-detail` - each subsection (penyiraman, cahaya, suhu)
- `.care-subtitle` - subsection title with emoji
- `.care-tips` - bulleted list
- `.harvest-detail` - info box (cream bg)
- `.harvest-tips` - bulleted list

## Dev Notes

### Vibing Development Style
User suka improvisasi & flexible, jadi:
- Struktur data bisa evolve sesuai kebutuhan
- Field baru bisa ditambah kapan aja
- Design iterate based on feel, bukan strict mockup
- "Gas langsung" > lengthy planning

### Common Operations
- **Update product info**: edit `config.json`
- **Add new batch**: buat file baru + update index.json
- **Change styling**: edit `style.css`
- **Hard refresh**: Cmd+Shift+R (biar CSS/JS reload)

### Important Gotchas
- File batch format: `{slug}_{YYYYMMDD}.json` (no dashes in date)
- Index.json MUST have matching IDs with batch filenames
- Emoji bisa jadi weird character (�) di editor - it's fine
- Always test dengan actual data, bukan placeholder

## Future Ideas (brainstorming)
- [ ] Photo upload per batch
- [ ] Customer review/rating system
- [ ] Harvest tracker (berapa kali udah dipanen)
- [ ] WhatsApp integration untuk order
- [ ] Admin panel untuk manage batches
- [ ] Analytics: view count per batch
- [ ] Multi-language (EN/ID)
- [ ] Dark mode

## Contact & Branding
- Brand: **Microgreens by Ucops**
- Location: Jagakarsa, Jakarta Selatan
- Instagram: @ucops.microgreens
- Konsep: Small batch, personal touch, edukasi customer

---

**Last updated**: 2026-06-12
**Version**: v1.0 - Living Plant Edition
