<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">U shumisele WIA SOOM</h1>
<p align="center"><strong>Ri funza u shumisela!</strong></p>
<p align="center">Na u bug fix, feature itsha, plugin, kana u fhedza — u shumisela hu na ndeme.</p>

---

## Tafela ya Zwiitisi

- [Code of Conduct](#code-of-conduct)
- [U itela Bug](#-how-to-report-bugs)
- [U sugela Zwiitisi](#-how-to-suggest-features)
- [U itela Plugin](#-how-to-submit-a-plugin)
- [U itela Pull Request](#-how-to-submit-a-pull-request)
- [U shumisela Zwiitisi (254 Languages)](#-translation-contributions-254-languages)
- [U setup wa Development](#-development-setup)

---

## Code of Conduct

Ri khou shumisa u fana na muya wa u amba na u shandukela vhathu vhoṱhe.

- **Khou vha na u rabela.** Tenda vhathu vhoṱhe nga u shuma na u vhonala.
- **Khou vha na u shuma.** Fana na ndeme, a si u vhuya.
- **Khou vha na u shandukela.** Ri amba 254 languages na u amba na vhashumeli vha u bva kha shango ḽoṱhe.
- **A hu na u vhuya.** U sa amba na u vhuya ha ndeme dzoṱhe.

---

## 🐛 U itela Bug

1. Fhira kha [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Khetha **"New Issue"**
3. Khetha **"Bug Report"** template
4. Fhira:
   - WIA SOOM version (Settings → About)
   - OS na version (Windows/macOS/Linux)
   - Mavhisi a u itela
   - Zwiitisi zwo ralo na zwo itwa
   - Mifananndo kana terminal output arali zwo itwa

---

## 💡 U sugela Zwiitisi

1. Fhira kha [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Khetha **"New Issue"**
3. Khetha **"Feature Request"** template
4. Dzhia:
   - Ndi mini u sa vhuya
   - U funza hani u shuma
   - Zwiṅwe zwoṱhe zwo amba

---

## 🔌 U itela Plugin

WIA SOOM i na system ya plugin ine ya khou shuma zwavhuḓi — u nga bvela phanda u bvela plugin ya hau nga minwaha 5.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Funda **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** u:
- U amba API ya u fhela
- Mifananndo ya u shuma
- Tutorials dza nga u bvela phanda
- Zwiitisi zwoṱhe na maphu a u shuma

### Itela Plugin Ya Hau

1. Fork [Plugin Store](https://wiasoom.com)
2. Dzhia plugin ya hau kha `plugins/{your-plugin-name}/`
3. Itela Pull Request
4. Kha u vhona, plugin ya hau i tshi bva kha Plugin Store u itela vhathu vhoṱhe!

---

## 🔀 U itela Pull Request

### Kha app ya nḓa (wia-soom)

1. Fork repository
2. Vhala feature branch: `git checkout -b feat/my-feature`
3. Ita shanduko dzau
4. Test kha ndila:
   ```bash
   ```
5. Commit na uthuli u fanela:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push na u vhona PR kha `main`

### Uthuli wa Commit Message

| Prefix | U shuma na |
|--------|---------|
| `feat:` | Feature itsha |
| `fix:` | Bug fix |
| `docs:` | Documentation fhedzi |
| `refactor:` | U shandula code (a si u shandula u shuma) |
| `i18n:` | U fhedza zwiitisi |
| `plugin:` | U shandula plugin-related changes |

### PR Checklist

- [ ] Code i khou shuma na u si na maphu
- [ ] A hu na strings dzo hardcoded (shuma i18n keys)
- [ ] A hu na `console.log` i sa si na production code
- [ ] Zwiitisi zwo vhuya zwoṱhe zwo shuma

---

## 🌐 U shumisela Zwiitisi (254 Languages)

WIA SOOM i amba **254 languages** — u bva kha Amharic u ya kha Zulu, hu na Braille na RTL languages.

### U shuma ha Zwiitisi

- File ya u bva: `src/renderer/src/i18n/en.json`
- Zwiitisi zwo 254 zwo bva kha directory yeo
- U fhedza zwiitisi u itwa nga `scripts/translate-patch.js` (GPT-4o-mini API)

### U itela U Fhedza Zwiitisi

#### Option 1: Fhedza u fhedza
 
1. Fhira file ya luambo: `src/renderer/src/i18n/{lang-code}.json`
2. Fhedza u fhedza u sa vhuya
3. Itela PR na shanduko

#### Option 2: Dzhia maphu a u vhuya
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Vhona u fhedza ha machine

Zwiitisi zwo 254 zwo fhedzwiwa nga machine. Vhathu vha u bva kha luambo vha na ndeme!

1. Khetha file ya luambo
2. Vhona zwiitisi
3. Fhedza zwiitisi zwo sa vhuya kana zwo vhuya
4. Itela PR

### Codes dza Luambo

Ri shuma codes dza ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) na regional variants arali zwo itwa (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 U setup wa Development

### Zwiitisi Zwo Fhela

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Note: The default 2GB heap a i si na vhukuma nga u bva kha 254 language files + Monaco editor bundle (~38MB renderer).

### U Vhumbula ha Project
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

## 🙏 Ndavhuya

Uṱali uṱhuwedza WIA SOOM u itela vhadavhidzi vha shango. 

Na u tshi khou sika phanda, u fhedza mutheo, u sika plugin, kana u ṱanganya mvelele ya ndeme — **ni part ya nyambo iyi.**

---

<p align="center"><em>U sika na ❤️ nga SmileStory Inc. na vhadavhidzi vha shango.</em></p>