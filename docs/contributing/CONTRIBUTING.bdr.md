<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Menyumbang kepada WIA SOOM</h1>
<p align="center"><strong>Kami sangat menghargai sumbangan anda!</strong></p>
<p align="center">Sama ada ia adalah pembetulan bug, ciri baru, plugin, atau terjemahan — setiap sumbangan adalah penting.</p>

---

## Jadual Kandungan

- [Kod Etika](#code-of-conduct)
- [Cara Melaporkan Bug](#-how-to-report-bugs)
- [Cara Mencadangkan Ciri](#-how-to-suggest-features)
- [Cara Menghantar Plugin](#-how-to-submit-a-plugin)
- [Cara Menghantar Permintaan Tarik](#-how-to-submit-a-pull-request)
- [Sumbangan Terjemahan (254 Bahasa)](#-translation-contributions-254-languages)
- [Persediaan Pembangunan](#-development-setup)

---

## Kod Etika

Kami komited untuk menyediakan pengalaman yang mesra dan inklusif untuk semua orang.

- **Hormati.** Layan semua orang dengan maruah.
- **Bersifat membina.** Berikan maklum balas yang membantu, bukan kritikan yang merosakkan.
- **Inklusif.** Kami menyokong 254 bahasa dan mengalu-alukan penyumbang dari setiap negara di Bumi.
- **Tiada gangguan.** Toleransi sifar terhadap diskriminasi dalam apa jua bentuk.

---

## 🐛 Cara Melaporkan Bug

1. Pergi ke [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"Isu Baru"**
3. Pilih templat **"Laporan Bug"**
4. Sertakan:
   - Versi WIA SOOM (Tetapan → Mengenai)
   - OS dan versi (Windows/macOS/Linux)
   - Langkah untuk menghasilkan semula
   - Tingkah laku yang dijangka vs. sebenar
   - Tangkapan skrin atau output terminal jika boleh

---

## 💡 Cara Mencadangkan Ciri

1. Pergi ke [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"Isu Baru"**
3. Pilih templat **"Permintaan Ciri"**
4. Huraikan:
   - Masalah yang anda selesaikan
   - Bagaimana anda membayangkan ia berfungsi
   - Sebarang alternatif yang telah anda pertimbangkan

---

## 🔌 Cara Menghantar Plugin

WIA SOOM mempunyai sistem plugin yang kuat — anda boleh membina plugin anda sendiri dalam 5 minit.

### Permulaan Pantas
§§§CHUNK_SEPARATOR§§§
### Panduan Penuh

Baca **[Panduan Pembangun Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** untuk:
- Rujukan API lengkap
- Contoh kerja
- Tutorial langkah demi langkah
- Amalan terbaik dan peraturan keselamatan

### Hantar Plugin Anda

1. Fork [Plugin Store](https://wiasoom.com)
2. Tambah plugin anda ke `plugins/{your-plugin-name}/`
3. Hantar Permintaan Tarik
4. Selepas semakan, plugin anda akan muncul di Plugin Store untuk semua pengguna!

---

## 🔀 Cara Menghantar Permintaan Tarik

### Untuk aplikasi utama (wia-soom)

1. Fork repositori
2. Buat cabang ciri: `git checkout -b feat/my-feature`
3. Lakukan perubahan anda
4. Uji secara tempatan:
   ```bash
   ```
5. Komit dengan mesej yang jelas:
   ```
   feat: tambah suis mod gelap ke tetapan
   ```
6. Push dan buka PR terhadap `main`

### Konvensyen Mesej Komit

| Prefix | Digunakan untuk |
|--------|----------------|
| `feat:` | Ciri baru |
| `fix:` | Pembetulan bug |
| `docs:` | Dokumentasi sahaja |
| `refactor:` | Penstrukturan semula kod (tiada perubahan tingkah laku) |
| `i18n:` | Kemas kini terjemahan |
| `plugin:` | Perubahan berkaitan plugin |

### Senarai Semak PR

- [ ] Kod berjalan tanpa ralat
- [ ] Tiada rentetan keras (gunakan kunci i18n)
- [ ] Tiada `console.log` ditinggalkan dalam kod pengeluaran
- [ ] Ujian yang sedia ada masih lulus

---

## 🌐 Sumbangan Terjemahan (254 Bahasa)

WIA SOOM menyokong **254 bahasa** — dari Amharik ke Zulu, termasuk Braille dan bahasa RTL.

### Cara Terjemahan Berfungsi

- Fail bahasa asas: `src/renderer/src/i18n/en.json`
- Semua 254 fail bahasa berada dalam direktori yang sama
- Terjemahan dilakukan melalui `scripts/translate-patch.js` (API GPT-4o-mini)

### Cara Menyumbang Terjemahan

#### Pilihan 1: Betulkan terjemahan tertentu

1. Cari fail bahasa: `src/renderer/src/i18n/{lang-code}.json`
2. Betulkan terjemahan yang tidak betul
3. Hantar PR dengan perubahan

#### Pilihan 2: Tambah kunci yang hilang
§§§CHUNK_SEPARATOR§§§
#### Pilihan 3: Semak terjemahan mesin

Banyak daripada 254 bahasa kami diterjemahkan oleh mesin. Semakan oleh penutur asli adalah sangat berharga!

1. Pilih fail bahasa anda
2. Semak terjemahan
3. Betulkan sebarang terjemahan yang janggal atau tidak betul
4. Hantar PR

### Kod Bahasa

Kami menggunakan kod standard ISO 639-1 (contohnya, `ko`, `en`, `ja`, `ar`, `hi`) dengan varian serantau di mana perlu (contohnya, `zh-CN`, `pt-BR`).

---

## 🛠 Persediaan Pembangunan

### Prasyarat

- Node.js 18+
- npm 9+
- Git

### Persediaan
§§§CHUNK_SEPARATOR§§§
### Pembinaan
§§§CHUNK_SEPARATOR§§§
> Nota: Heap 2GB lalai tidak mencukupi disebabkan oleh 254 fail bahasa + bundel editor Monaco (~38MB renderer).

### Struktur Projek
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Terima Kasih

Setiap sumbangan menjadikan WIA SOOM lebih baik untuk para pengembang di seluruh dunia.

Sama ada anda membetulkan kesalahan ejaan, menterjemahkan satu string, membina plugin, atau menambah ciri utama — **anda adalah sebahagian daripada cerita ini.**

---

<p align="center"><em>Dibina dengan ❤️ oleh SmileStory Inc. dan penyumbang di seluruh dunia.</em></p>
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```

#### Option 3: Review machine translations

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup

```bash
```

### Build

```bash
```

> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

### Project Structure

```
wia-soom/
├── src/
│   ├── main/          # Electron main process
│   ├── renderer/      # React frontend
│   └── preload/       # Preload scripts
├── docs/              # Documentation
├── scripts/           # Build & automation scripts
└── prompts/           # AI prompt engineering
```

---

## 🙏 Thank You

Every contribution makes WIA SOOM better for developers around the world.

Whether you fix a typo, translate a string, build a plugin, or add a major feature — **you are part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>
