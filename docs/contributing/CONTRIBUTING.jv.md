<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribusi kanggo WIA SOOM</h1>
<p align="center"><strong>Kami seneng kontribusi sampeyan!</strong></p>
<p align="center">Apa iku perbaikan bug, fitur anyar, plugin, utawa terjemahan — saben kontribusi iku penting.</p>

---

## Daftar Isi

- [Kode Etik](#kode-etik)
- [Cara Ng lapor Bug](#-cara-ng-lapor-bug)
- [Cara Nyaranake Fitur](#-cara-nyaranake-fitur)
- [Cara Ngirim Plugin](#-cara-ngirim-plugin)
- [Cara Ngirim Pull Request](#-cara-ngirim-pull-request)
- [Kontribusi Terjemahan (254 Basa)](#-kontribusi-terjemahan-254-basa)
- [Pengaturan Pengembangan](#-pengaturan-pengembangan)

---

## Kode Etik

Kita komitmen kanggo nyedhiyakake pengalaman sing ramah lan inklusif kanggo kabeh wong.

- **Dadi hormat.** Perlakukan kabeh wong kanthi martabat.
- **Dadi konstruktif.** Tawarkan umpan balik sing migunani, ora kritik sing ngrusak.
- **Dadi inklusif.** Kita ndhukung 254 basa lan nyambut tamu kontributor saka saben negara ing Bumi.
- **Ora ana pelecehan.** Nol toleransi kanggo diskriminasi saka jinis apa wae.

---

## 🐛 Cara Ng lapor Bug

1. Bukak [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Pilih template **"Bug Report"**
4. Kalebu:
   - Versi WIA SOOM (Setelan → Bab)
   - OS lan versi (Windows/macOS/Linux)
   - Langkah-langkah kanggo ngasilake
   - Perilaku sing diarepake vs. nyata
   - Tangkapan layar utawa output terminal yen bisa

---

## 💡 Cara Nyaranake Fitur

1. Bukak [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Pilih template **"Feature Request"**
4. Deskripsikan:
   - Masalah apa sing sampeyan atasi
   - Kepiye sampeyan mbayangkan cara kerjane
   - Alternatif apa wae sing wis sampeyan pertimbangkan

---

## 🔌 Cara Ngirim Plugin

WIA SOOM nduweni sistem plugin sing kuat — sampeyan bisa nggawe plugin dhewe ing 5 menit.

### Mulai Cepet
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Pandhuan Lengkap

Waca **[Pandhuan Pengembang Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** kanggo:
- Referensi API lengkap
- Conto kerja
- Tutorial langkah demi langkah
- Praktik paling apik lan aturan keamanan

### Kirim Plugin Sampeyan

1. Fork [Plugin Store](https://wiasoom.com)
2. Tambahake plugin sampeyan menyang `plugins/{your-plugin-name}/`
3. Kirim Pull Request
4. Sawise ditinjau, plugin sampeyan bakal muncul ing Plugin Store kanggo kabeh pangguna!

---

## 🔀 Cara Ngirim Pull Request

### Kanggo aplikasi utama (wia-soom)

1. Fork repositori
2. Gawe cabang fitur: `git checkout -b feat/my-feature`
3. Ganti sing dibutuhake
4. Uji lokal:
   ```bash
   ```
5. Commit nganggo pesen sing jelas:
   ```
   feat: tambahake saklar mode peteng menyang setelan
   ```
6. Push lan buka PR nglawan `main`

### Konvensi Pesen Commit

| Prefix | Gunakake kanggo |
|--------|----------------|
| `feat:` | Fitur anyar |
| `fix:` | Perbaikan bug |
| `docs:` | Dokumentasi wae |
| `refactor:` | Restrukturisasi kode (ora ana owah-owahan perilaku) |
| `i18n:` | Pembaruan terjemahan |
| `plugin:` | Owahan sing ana gandhengane karo plugin |

### Checklist PR

- [ ] Kode mlaku tanpa kesalahan
- [ ] Ora ana string hardcoded (gunakake kunci i18n)
- [ ] Ora ana `console.log` sing ditinggalake ing kode produksi
- [ ] Tes sing ana isih lulus

---

## 🌐 Kontribusi Terjemahan (254 Basa)

WIA SOOM ndhukung **254 basa** — saka Amharic nganti Zulu, kalebu Braille lan basa RTL.

### Cara Kerja Terjemahan

- File basa dhasar: `src/renderer/src/i18n/en.json`
- Kabeh 254 file basa ana ing direktori sing padha
- Terjemahan ditindakake liwat `scripts/translate-patch.js` (GPT-4o-mini API)

### Cara Kontribusi Terjemahan

#### Opsi 1: Perbaiki terjemahan sing spesifik

1. Temokake file basa: `src/renderer/src/i18n/{lang-code}.json`
2. Perbaiki terjemahan sing salah
3. Kirim PR kanthi owah-owahan

#### Opsi 2: Tambah kunci sing ilang
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsi 3: Tinjau terjemahan mesin

Akeh saka 254 basa kita diterjemahake kanthi mesin. Tinjauan saka penutur asli iku banget berharga!

1. Pilih file basa sampeyan
2. Tinjau terjemahan
3. Perbaiki terjemahan sing aneh utawa salah
4. Kirim PR

### Kode Basa

Kita nggunakake kode standar ISO 639-1 (contone, `ko`, `en`, `ja`, `ar`, `hi`) kanthi varian regional yen perlu (contone, `zh-CN`, `pt-BR`).

---

## 🛠 Pengaturan Pengembangan

### Prasyarat

- Node.js 18+
- npm 9+
- Git

### Pengaturan
```bash
```
### Build
```bash
```
> Cathetan: Heap 2GB standar ora cukup amarga 254 file basa + bundel editor Monaco (~38MB renderer).

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

## 🙏 Matur Nuwun

Saben kontribusi nggawe WIA SOOM luwih apik kanggo para pangembang ing saindenging jagad.

Apa sampeyan ndandani kesalahan ketik, nerjemahake string, mbangun plugin, utawa nambahake fitur utama — **sampeyan minangka bagian saka crita iki.**

---

<p align="center"><em>Dibangun kanthi ❤️ dening SmileStory Inc. lan para kontributor ing saindenging jagad.</em></p>