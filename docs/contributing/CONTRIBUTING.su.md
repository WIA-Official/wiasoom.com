<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Nyumbang ka WIA SOOM</h1>
<p align="center"><strong>Kami bakal senang pisan kana kontribusi anjeun!</strong></p>
<p align="center>Entong ragu, naha éta perbaikan bug, fitur anyar, plugin, atanapi tarjamahan — unggal kontribusi penting.</p>

---

## Daptar Eusi

- [Kode Etik](#kode-etik)
- [Kumaha Ngabéjaan Bug](#-kumaha-ngabéjaan-bug)
- [Kumaha Nyarankeun Fitur](#-kumaha-nyarankeun-fitur)
- [Kumaha Ngirim Plugin](#-kumaha-ngirim-plugin)
- [Kumaha Ngirim Pull Request](#-kumaha-ngirim-pull-request)
- [Kontribusi Tarjamahan (254 Basa)](#-kontribusi-tarjamahan-254-basa)
- [Setup Pembangunan](#-setup-pembangunan)

---

## Kode Etik

Kami komitmen pikeun nyayogikeun pangalaman anu ramah sareng inklusif pikeun sadayana.

- **Hormat.** Perlakukan sadayana kalayan martabat.
- **Konstruktif.** Tawarkan masukan anu mangpaat, sanés kritik anu ngancurkeun.
- **Inklusif.** Kami ngadukung 254 basa sareng nampi kontributor ti unggal nagara di Bumi.
- **Henteu aya pelecehan.** Nol toleransi pikeun diskriminasi naon waé.

---

## 🐛 Kumaha Ngabéjaan Bug

1. Buka [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"Isu Anyar"**
3. Pilih template **"Laporan Bug"**
4. Sertakan:
   - Versi WIA SOOM (Setélan → Ngeunaan)
   - OS sareng versi (Windows/macOS/Linux)
   - Léngkah-léngkah pikeun ngulang
   - Tingkah laku anu diarepkeun vs. anu sabenerna
   - Screenshot atanapi hasil terminal upami mungkin

---

## 💡 Kumaha Nyarankeun Fitur

1. Buka [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"Isu Anyar"**
3. Pilih template **"Pamundut Fitur"**
4. Jelaskeun:
   - Masalah naon anu anjeun rék selesaikan
   - Kumaha anjeun ngabayangkeun éta jalan
   - Sagala alternatif anu anjeun pertimbangkeun

---

## 🔌 Kumaha Ngirim Plugin

WIA SOOM gaduh sistem plugin anu kuat — anjeun tiasa ngawangun plugin anjeun sorangan dina 5 menit.

### Mimitian Gancang
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Panduan Lengkap

Baca **[Panduan Pamekar Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** pikeun:
- Réferénsi API lengkep
- Conto anu jalan
- Tutorial léngkah-demi-léngkah
- Prakték pangsaéna sareng aturan kaamanan

### Kirim Plugin Anjeun

1. Fork [Plugin Store](https://wiasoom.com)
2. Tambahkeun plugin anjeun ka `plugins/{ngaran-plugin-anjeun}/`
3. Kirim Pull Request
4. Saatos ditinjau, plugin anjeun bakal muncul di Plugin Store pikeun sadaya pangguna!

---

## 🔀 Kumaha Ngirim Pull Request

### Pikeun aplikasi utama (wia-soom)

1. Fork repositori
2. Jieun cabang fitur: `git checkout -b feat/my-feature`
3. Laksanakeun parobahan anjeun
4. Uji sacara lokal:
   ```bash
   ```
5. Komit sareng pesen anu jelas:
   ```
   feat: tambahkeun toggle mode poék ka setélan
   ```
6. Push sareng buka PR ngalawan `main`

### Konvensi Pesen Komit

| Prefix | Pamakéan pikeun |
|--------|-----------------|
| `feat:` | Fitur anyar |
| `fix:` | Perbaikan bug |
| `docs:` | Dokumentasi waé |
| `refactor:` | Réstrukturisasi kode (tanpa parobahan tingkah laku) |
| `i18n:` | Pembaruan tarjamahan |
| `plugin:` | Parobahan anu aya hubunganana sareng plugin |

### Daftar Periksa PR

- [ ] Kode jalan tanpa kasalahan
- [ ] Henteu aya string hardcoded (nganggo konci i18n)
- [ ] Henteu aya `console.log` anu ditinggalkeun dina kode produksi
- [ ] Uji anu aya masih lulus

---

## 🌐 Kontribusi Tarjamahan (254 Basa)

WIA SOOM ngadukung **254 basa** — ti Amharic dugi ka Zulu, kalebet Braille sareng basa RTL.

### Kumaha Tarjamahan Jalan

- File basa dasar: `src/renderer/src/i18n/en.json`
- Sadaya 254 file basa aya di diréktori anu sami
- Tarjamahan dilakukeun ngaliwatan `scripts/translate-patch.js` (GPT-4o-mini API)

### Kumaha Ngabantosan Tarjamahan

#### Pilihan 1: Perbaiki tarjamahan anu khusus

1. Panggihan file basa: `src/renderer/src/i18n/{kode-basa}.json`
2. Perbaiki tarjamahan anu salah
3. Kirim PR sareng parobahan

#### Pilihan 2: Tambahkeun konci anu hilang
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Pilihan 3: Tinjau tarjamahan mesin

Loba basa kami anu 254 ditarjamahkeun ku mesin. Tinjauan ti penutur asli pisan berharga!

1. Pilih file basa anjeun
2. Tinjau tarjamahan
3. Perbaiki tarjamahan anu aneh atanapi salah
4. Kirim PR

### Kode Basa

Kami ngagunakeun kode standar ISO 639-1 (contona, `ko`, `en`, `ja`, `ar`, `hi`) kalayan variasi régional dimana diperlukeun (contona, `zh-CN`, `pt-BR`).

---

## 🛠 Setup Pembangunan

### Sarat

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Ngawangun
```bash
```
> Catetan: Heap 2GB standar henteu cekap alatan 254 file basa + bundel editor Monaco (~38MB renderer).

### Struktur Proyék
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

## 🙏 Hatur Nuhun

Sakabéh kontribusi ngajadikeun WIA SOOM langkung saé pikeun pamekar di sakuliah dunya.

Naha anjeun ngalereskeun typo, narjamahkeun string, ngawangun plugin, atanapi nambahkeun fitur utama — **anjeun mangrupikeun bagian tina carita ieu.**

---

<p align="center"><em>Dibangun ku ❤️ ku SmileStory Inc. sareng kontributor di sakuliah dunya.</em></p>