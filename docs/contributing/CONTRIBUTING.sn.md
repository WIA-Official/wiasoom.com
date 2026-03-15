<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kupa Mukana kuWIA SOOM</h1>
<p align="center"><strong>Tinoda mipiro yenyu!</strong></p>
<p align="center">Kunyange zvazvo iri kugadzirisa bug, chimiro chitsva, plugin, kana kushandura — mipiro yese ine basa.</p>

---

## Tafura yeZviri Mukati

- [Code of Conduct](#code-of-conduct)
- [Maitiro Ekushandisa Bug](#-how-to-report-bugs)
- [Maitiro Ekusuma Zvimiro](#-how-to-suggest-features)
- [Maitiro Ekutumira Plugin](#-how-to-submit-a-plugin)
- [Maitiro Ekutumira Pull Request](#-how-to-submit-a-pull-request)
- [Mipiro yeKushandura (254 Mitauro)](#-translation-contributions-254-languages)
- [Kugadzirisa Development](#-development-setup)

---

## Code of Conduct

Takatendeka kupa chiitiko chinogamuchirwa uye chinosanganisira munhu wese.

- **Iva nekuremekedza.** Ipa munhu wese kukudzwa.
- **Iva nekunzwisisa.** Ipa mhinduro inobatsira, kwete kutuka.
- **Iva nesanganiswa.** Tinotsigira mitauro ye254 uye tinogamuchira vanobatsira kubva munyika dzese dzepasi.
- **Hapana kutyisidzirwa.** Hapana kutolerwa kwekusarura kwechero rudzi.

---

## 🐛 Maitiro Ekushandisa Bug

1. Enda ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Dzvanya **"New Issue"**
3. Sarudza **"Bug Report"** template
4. Sanganisira:
   - WIA SOOM version (Settings → About)
   - OS uye version (Windows/macOS/Linux)
   - Matanho ekudzokorora
   - Zvinotarisirwa vs. maitiro chaiwo
   - Screenshots kana terminal output kana zvichibvira

---

## 💡 Maitiro Ekusuma Zvimiro

1. Enda ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Dzvanya **"New Issue"**
3. Sarudza **"Feature Request"** template
4. Tsanangura:
   - Chinetso chauri kugadzirisa
   - Maitiro aunofungidzira kuti zvichashanda
   - Chero dzimwe nzira dzawakafunga nezvadzo

---

## 🔌 Maitiro Ekutumira Plugin

WIA SOOM ine sisitimu ine simba yeplugin — unogona kugadzira plugin yako mumaminitsi mashanu.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Verenga **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ye:
- API reference yakazara
- Mienzaniso inoshanda
- Tutorials nhanho-nhanho
- Maitiro akanakisisa uye mitemo yekuchengetedza

### Tumira Plugin Yako

1. Fork [Plugin Store](https://wiasoom.com)
2. Wedzera plugin yako ku `plugins/{your-plugin-name}/`
3. Tumira Pull Request
4. Mushure mekutarisa, plugin yako inoratidzwa muPlugin Store kune vashandisi vese!

---

## 🔀 Maitiro Ekutumira Pull Request

### Kune main app (wia-soom)

1. Fork repository
2. Gadzira feature branch: `git checkout -b feat/my-feature`
3. Ita shanduko dzako
4. Edza locally:
   ```bash
   ```
5. Commit ne meseji yakajeka:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push uye uvhure PR ku `main`

### Commit Message Convention

| Prefix | Shandisa kuti |
|--------|---------------|
| `feat:` | Chimwe chitsva |
| `fix:` | Kugadzirisa bug |
| `docs:` | Zvinyorwa chete |
| `refactor:` | Kudzokorora kodhi (hapana shanduko yemaitiro) |
| `i18n:` | Kushandura kugadzirisa |
| `plugin:` | Shanduko dzine chekuita neplugin |

### PR Checklist

- [ ] Kodhi inomhanya pasina zvikanganiso
- [ ] Hapana tambo dzakaumbwa (shandisa i18n keys)
- [ ] Hapana `console.log` yakasara mukodhi yekugadzira
- [ ] Kuedzwa kwekare kuchiri kupfuura

---

## 🌐 Mipiro yeKushandura (254 Mitauro)

WIA SOOM inotsigira **254 mitauro** — kubva kuAmharic kusvika kuZulu, kusanganisira Braille nemitauro yeRTL.

### Maitiro Ekushandura

- Base language file: `src/renderer/src/i18n/en.json`
- Mafaira emutauro ese 254 ari mudhairekitori rimwe chete
- Kushandura kunoitwa kuburikidza ne `scripts/translate-patch.js` (GPT-4o-mini API)

### Maitiro Ekupa Mipiro yeKushandura

#### Sarudzo 1: Gadzirisa kushandura kwakakanganisika

1. Tsvaga faira remutauro: `src/renderer/src/i18n/{lang-code}.json`
2. Gadzirisa kushandura kwakakanganisika
3. Tumira PR ine shanduko

#### Sarudzo 2: Wedzera makiyi asipo
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Sarudzo 3: Ongorora kushandurwa kwemichina

Mazhinji emitauro yedu ye254 akashandurwa nemichina. Ongororo dzevatongi vechivanhu dzakakosha zvikuru!

1. Sarudza faira remutauro rako
2. Ongorora kushandurwa
3. Gadzirisa chero kushandurwa kusina kujairika kana kwakakanganisika
4. Tumira PR

### Makodhi eMitauro

Tinoshandisa makodhi eISO 639-1 akajairika (semuenzaniso, `ko`, `en`, `ja`, `ar`, `hi`) ane shanduko dzemuno panodiwa (semuenzaniso, `zh-CN`, `pt-BR`).

---

## 🛠 Kugadzirisa Development

### Zvinodiwa

- Node.js 18+
- npm 9+
- Git

### Kugadzirisa
```bash
```
### Kuvaka
```bash
```
> Cherechedzo: Iyo default 2GB heap haina kukwana nekuda kwemafaira emutauro 254 + Monaco editor bundle (~38MB renderer).

### Chimiro cheProjekiti
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

## 🙏 Ndatenda

Chipo chese chinobatsira WIA SOOM kuti ive nani kune vanogadzira pasi rese.

Kunyange ukagadzirisa typo, kuturikira mutsara, kuvaka plugin, kana kuwedzera chimiro chikuru — **uri chikamu chenyaya iyi.**

---

<p align="center"><em>Yakagadzirwa ne ❤️ neSmileStory Inc. uye vanobatsira pasi rese.</em></p>