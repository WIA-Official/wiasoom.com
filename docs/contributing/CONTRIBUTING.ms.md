<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Menyumbang kepada WIA SOOM</h1>
<p align="center"><strong>Kami sangat menghargai sumbangan anda!</strong></p>
<p align="center">Sama ada ia adalah pembetulan pepijat, ciri baru, plugin, atau terjemahan — setiap sumbangan adalah penting.</p>

---

## Jadual Kandungan

- [Kod Tingkah Laku](#code-of-conduct)
- [Cara Melaporkan Pepijat](#-how-to-report-bugs)
- [Cara Mencadangkan Ciri](#-how-to-suggest-features)
- [Cara Menghantar Plugin](#-how-to-submit-a-plugin)
- [Cara Menghantar Permintaan Tarik](#-how-to-submit-a-pull-request)
- [Sumbangan Terjemahan (254 Bahasa)](#-translation-contributions-254-languages)
- [Persediaan Pembangunan](#-development-setup)

---

## Kod Tingkah Laku

Kami komited untuk menyediakan pengalaman yang mesra dan inklusif untuk semua orang.

- **Hargai orang lain.** Layan semua orang dengan maruah.
- **Bersikap membina.** Berikan maklum balas yang berguna, bukan kritikan yang merosakkan.
- **Inklusif.** Kami menyokong 254 bahasa dan mengalu-alukan penyumbang dari setiap negara di Bumi.
- **Tiada gangguan.** Toleransi sifar terhadap diskriminasi dalam apa jua bentuk.

---

## 🐛 Cara Melaporkan Pepijat

1. Pergi ke [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"Isu Baru"**
3. Pilih templat **"Laporan Pepijat"**
4. Sertakan:
   - Versi WIA SOOM (Tetapan → Mengenai)
   - OS dan versi (Windows/macOS/Linux)
   - Langkah-langkah untuk menghasilkan semula
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
   - Sebarang alternatif yang anda pertimbangkan

---

## 🔌 Cara Menghantar Plugin

WIA SOOM mempunyai sistem plugin yang kuat — anda boleh membina plugin anda sendiri dalam 5 minit.

### Permulaan Pantas
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
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
4. Setelah disemak, plugin anda akan muncul di Plugin Store untuk semua pengguna!

---

## 🔀 Cara Menghantar Permintaan Tarik

### Untuk aplikasi utama (wia-soom)

1. Fork repositori
2. Buat cawangan ciri: `git checkout -b feat/my-feature`
3. Lakukan perubahan anda
4. Uji secara tempatan:
   ```bash
   ```
5. Komit dengan mesej yang jelas:
   ```
   feat: tambah togol mod gelap ke tetapan
   ```
6. Push dan buka PR terhadap `main`

### Konvensyen Mesej Komit

| Prefix | Digunakan untuk |
|--------|----------------|
| `feat:` | Ciri baru |
| `fix:` | Pembetulan pepijat |
| `docs:` | Dokumentasi sahaja |
| `refactor:` | Penstrukturan semula kod (tiada perubahan tingkah laku) |
| `i18n:` | Kemas kini terjemahan |
| `plugin:` | Perubahan berkaitan plugin |

### Senarai Semak PR

- [ ] Kod berjalan tanpa ralat
- [ ] Tiada rentetan keras (gunakan kunci i18n)
- [ ] Tiada `console.log` ditinggalkan dalam kod pengeluaran
- [ ] Ujian sedia ada masih lulus

---

## 🌐 Sumbangan Terjemahan (254 Bahasa)

WIA SOOM menyokong **254 bahasa** — dari Amharic hingga Zulu, termasuk Braille dan bahasa RTL.

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
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Pilihan 3: Semak terjemahan mesin

Banyak daripada 254 bahasa kami diterjemahkan oleh mesin. Semakan oleh penutur asli adalah sangat berharga!

1. Pilih fail bahasa anda
2. Semak terjemahan
3. Betulkan sebarang terjemahan yang janggal atau tidak betul
4. Hantar PR

### Kod Bahasa

Kami menggunakan kod ISO 639-1 standard (contohnya, `ko`, `en`, `ja`, `ar`, `hi`) dengan variasi serantau jika perlu (contohnya, `zh-CN`, `pt-BR`).

---

## 🛠 Persediaan Pembangunan

### Prasyarat

- Node.js 18+
- npm 9+
- Git

### Persediaan
```bash
```
### Pembinaan
```bash
```
> Nota: Heap 2GB lalai tidak mencukupi disebabkan oleh 254 fail bahasa + bundel editor Monaco (~38MB renderer).

### Struktur Projek
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

## 🙏 Terima Kasih

Setiap sumbangan menjadikan WIA SOOM lebih baik untuk pembangun di seluruh dunia.

Sama ada anda membetulkan kesalahan ejaan, menterjemah satu rentetan, membina plugin, atau menambah ciri utama — **anda adalah sebahagian daripada cerita ini.**

---

<p align="center"><em>Dibina dengan ❤️ oleh SmileStory Inc. dan penyumbang di seluruh dunia.</em></p>