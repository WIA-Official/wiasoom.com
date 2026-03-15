<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kyns an WIA SOOM</h1>
<p align="center"><strong>Yma ni ow kelwel dhywgh an kyns!</strong></p>
<p align="center">Na bos a bug, nebon an gath, plugin, po treveth — yth esa an kyns ow kelwel.</p>

---

## Tabel an Kyns

- [Code of Conduct](#code-of-conduct)
- [Pyth a Vynn Dhiworth Bugs](#-pyth-a-vynn-dhiworth-bugs)
- [Pyth a Vynn Dhiworth Gathow](#-pyth-a-vynn-dhiworth-gathow)
- [Pyth a Vynn Dhiworth Plugin](#-pyth-a-vynn-dhiworth-plugin)
- [Pyth a Vynn Dhiworth Pull Request](#-pyth-a-vynn-dhiworth-pull-request)
- [Kyns Treveth (254 Yethow)](#-kyns-treveth-254-yethow)
- [Settyans Gweledh](#-settyans-gweledh)

---

## Code of Conduct

Yma ni ow kelwel dhywgh owth omdhal an gath a'n gath a'n gath.

- **Bledhyn owth.** Gwel an neb a'n gath gans dignit.
- **Bledhyn owth.** Gwel an neb a'n gath gans gath a'n gath.
- **Bledhyn owth.** Yma ni ow kelwel 254 yethow ha gwel an kyns a'n gath a'n gath.
- **Na haras.** Zero tolerance rag an disgwyl a'n gath.

---

## 🐛 Pyth a Vynn Dhiworth Bugs

1. Kynsa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Dewis an **"Bug Report"** template
4. Kynsa:
   - WIA SOOM version (Settings → About)
   - OS ha version (Windows/macOS/Linux)
   - Dros an gath
   - An gath a'n gath
   - Screenshots po terminal output ma's posibyl

---

## 💡 Pyth a Vynn Dhiworth Gathow

1. Kynsa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Dewis an **"Feature Request"** template
4. Discrif:
   - Pyth a'n gath a vynn dhywgh
   - Pyth a vynn dhywgh owth omdhal
   - An keth a vynn dhywgh

---

## 🔌 Pyth a Vynn Dhiworth Plugin

Yma WIA SOOM gans system plugin powrful — yth esa posibyl dhywgh owth omdhal owth 5 munud.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Dhewel an **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** rag:
- Kyns API a'n gath
- Omskans owth omdhal
- Tutorials a'n gath
- An gath a'n gath ha reol a'n gath

### Submit Your Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Addh an plugin dhywgh gans `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. Wosa an gath, an plugin a'n gath a'n gath!

---

## 🔀 Pyth a Vynn Dhiworth Pull Request

### Rag an app main (wia-soom)

1. Fork an repository
2. Kreya branch gath: `git checkout -b feat/my-feature`
3. Dhiworth an gath
4. Test locally:
   ```bash
   ```
5. Commit gans mesage clera:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ha omdhal a PR rag `main`

### Commit Message Convention

| Prefix | Usya rag |
|--------|---------|
| `feat:` | Gath new |
| `fix:` | Bug fix |
| `docs:` | Dokumentation onan |
| `refactor:` | Restructuring code (na gath a'n gath) |
| `i18n:` | Kyns treveth |
| `plugin:` | Kyns a'n plugin |

### PR Checklist

- [ ] Code a'n gath heb erredh
- [ ] Na strings hardcoded (usya i18n keys)
- [ ] Na `console.log` owth gans code production
- [ ] Tests a'n gath a'n gath

---

## 🌐 Kyns Treveth (254 Yethow)

Yma WIA SOOM gans **254 yethow** — a'n Amharic dhyworth Zulu, yn omdhal gans Braille ha yethow RTL.

### Pyth a'n Kyns Treveth

- Fyle yeth a'n gath: `src/renderer/src/i18n/en.json`
- An 254 yethow a'n gath a'n gath
- Kyns treveth owth omdhal gans `scripts/translate-patch.js` (GPT-4o-mini API)

### Pyth a Vynn Dhiworth Treveth

#### Option 1: Ffix an treveth spesifik

1. Kynsa an fyle yeth: `src/renderer/src/i18n/{lang-code}.json`
2. Ffix an treveth an gath
3. Submit a PR gans an gath

#### Option 2: Addh an keys a'n gath
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Gwel an treveth a'n gath

Myns an 254 yethow a'n gath a'n gath. An gath a'n gath a'n gath!

1. Dewis an fyle yeth
2. Gwel an treveth
3. Ffix an gath a'n gath
4. Submit a PR

### Codes Yeth

Usya ni an codes ISO 639-1 standard (e.g., `ko`, `en`, `ja`, `ar`, `hi`) gans an gath a'n gath (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Settyans Gweledh

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Settyans
```bash
```
### Build
```bash
```
> Note: An default 2GB heap na enough rag an 254 yethow a'n gath + Monaco editor bundle (~38MB renderer).

### Strucure an Project
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

## 🙏 Meur ras

Pyth a wra ow kelwel WIA SOOM gwell dhyworth an devs a-dhiworth an bys.

Pyth a wra dhis gwell an typo, treusgans an string, kerdh a plugin, po add a feth a vyth — **ty a wra ow kelwel an stori ma.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>