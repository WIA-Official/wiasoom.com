<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribusi untuk WIA SOOM</h1>
<p align="center"><strong>Kami sangat menghargai kontribusi Anda!</strong></p>
<p align="center">Apakah itu perbaikan bug, fitur baru, plugin, atau terjemahan — setiap kontribusi sangat berarti.</p>

---

## Daftar Isi

- [Kode Etik](#kode-etik)
- [Cara Melaporkan Bug](#-cara-melaporkan-bug)
- [Cara Mengusulkan Fitur](#-cara-mengusulkan-fitur)
- [Cara Mengirimkan Plugin](#-cara-mengirimkan-plugin)
- [Cara Mengirimkan Pull Request](#-cara-mengirimkan-pull-request)
- [Kontribusi Terjemahan (254 Bahasa)](#-kontribusi-terjemahan-254-bahasa)
- [Pengaturan Pengembangan](#-pengaturan-pengembangan)

---

## Kode Etik

Kami berkomitmen untuk memberikan pengalaman yang ramah dan inklusif bagi semua orang.

- **Hormati.** Perlakukan semua orang dengan martabat.
- **Konstruktif.** Berikan umpan balik yang membantu, bukan kritik yang merusak.
- **Inklusif.** Kami mendukung 254 bahasa dan menyambut kontributor dari setiap negara di Bumi.
- **Tidak ada pelecehan.** Tidak ada toleransi untuk diskriminasi dalam bentuk apapun.

---

## 🐛 Cara Melaporkan Bug

1. Kunjungi [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Pilih template **"Bug Report"**
4. Sertakan:
   - Versi WIA SOOM (Pengaturan → Tentang)
   - OS dan versi (Windows/macOS/Linux)
   - Langkah-langkah untuk mereproduksi
   - Perilaku yang diharapkan vs. aktual
   - Tangkapan layar atau keluaran terminal jika memungkinkan

---

## 💡 Cara Mengusulkan Fitur

1. Kunjungi [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Pilih template **"Feature Request"**
4. Deskripsikan:
   - Masalah apa yang Anda selesaikan
   - Bagaimana Anda membayangkan cara kerjanya
   - Alternatif apa yang telah Anda pertimbangkan

---

## 🔌 Cara Mengirimkan Plugin

WIA SOOM memiliki sistem plugin yang kuat — Anda dapat membangun plugin Anda sendiri dalam 5 menit.

### Memulai dengan Cepat
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Panduan Lengkap

Baca **[Panduan Pengembang Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** untuk:
- Referensi API lengkap
- Contoh kerja
- Tutorial langkah demi langkah
- Praktik terbaik dan aturan keamanan

### Kirim Plugin Anda

1. Fork [Plugin Store](https://wiasoom.com)
2. Tambahkan plugin Anda ke `plugins/{nama-plugin-anda}/`
3. Kirim Pull Request
4. Setelah ditinjau, plugin Anda akan muncul di Plugin Store untuk semua pengguna!

---

## 🔀 Cara Mengirimkan Pull Request

### Untuk aplikasi utama (wia-soom)

1. Fork repositori
2. Buat cabang fitur: `git checkout -b feat/my-feature`
3. Lakukan perubahan Anda
4. Uji secara lokal:
   ```bash
   ```
5. Commit dengan pesan yang jelas:
   ```
   feat: tambahkan toggle mode gelap ke pengaturan
   ```
6. Dorong dan buka PR terhadap `main`

### Konvensi Pesan Commit

| Prefix | Digunakan untuk |
|--------|----------------|
| `feat:` | Fitur baru |
| `fix:` | Perbaikan bug |
| `docs:` | Hanya dokumentasi |
| `refactor:` | Restrukturisasi kode (tanpa perubahan perilaku) |
| `i18n:` | Pembaruan terjemahan |
| `plugin:` | Perubahan terkait plugin |

### Daftar Periksa PR

- [ ] Kode berjalan tanpa kesalahan
- [ ] Tidak ada string yang dikodekan keras (gunakan kunci i18n)
- [ ] Tidak ada `console.log` yang tersisa dalam kode produksi
- [ ] Tes yang ada masih lulus

---

## 🌐 Kontribusi Terjemahan (254 Bahasa)

WIA SOOM mendukung **254 bahasa** — dari Amharik hingga Zulu, termasuk Braille dan bahasa RTL.

### Cara Kerja Terjemahan

- File bahasa dasar: `src/renderer/src/i18n/en.json`
- Semua 254 file bahasa berada di direktori yang sama
- Terjemahan dilakukan melalui `scripts/translate-patch.js` (API GPT-4o-mini)

### Cara Berkontribusi Terjemahan

#### Opsi 1: Memperbaiki terjemahan tertentu

1. Temukan file bahasa: `src/renderer/src/i18n/{kode-bahasa}.json`
2. Perbaiki terjemahan yang salah
3. Kirim PR dengan perubahan tersebut

#### Opsi 2: Tambahkan kunci yang hilang
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsi 3: Tinjau terjemahan mesin

Banyak dari 254 bahasa kami diterjemahkan oleh mesin. Tinjauan oleh penutur asli sangat berharga!

1. Pilih file bahasa Anda
2. Tinjau terjemahan
3. Perbaiki terjemahan yang canggung atau salah
4. Kirim PR

### Kode Bahasa

Kami menggunakan kode standar ISO 639-1 (misalnya, `ko`, `en`, `ja`, `ar`, `hi`) dengan varian regional jika diperlukan (misalnya, `zh-CN`, `pt-BR`).

---

## 🛠 Pengaturan Pengembangan

### Prasyarat

- Node.js 18+
- npm 9+
- Git

### Pengaturan
```bash
```
### Bangun
```bash
```
> Catatan: Heap default 2GB tidak cukup karena 254 file bahasa + bundle editor Monaco (~38MB renderer).

### Struktur Proyek
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

Setiap kontribusi membuat WIA SOOM lebih baik untuk pengembang di seluruh dunia.

Apakah Anda memperbaiki kesalahan ketik, menerjemahkan sebuah string, membangun sebuah plugin, atau menambahkan fitur besar — **Anda adalah bagian dari cerita ini.**

---

<p align="center"><em>Dibangun dengan ❤️ oleh SmileStory Inc. dan kontributor di seluruh dunia.</em></p>