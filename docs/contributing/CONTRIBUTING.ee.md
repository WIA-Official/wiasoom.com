<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Nye WIA SOOM</h1>
<p align="center"><strong>Yɛpɛ wo nyɛyɛ!</strong></p>
<p align="center">Sɛ ɛyɛ bɔne a wɔyɛ no, nsɛm foforɔ, plugin anaa kɔkɔbɔ — nyɛyɛ biara yɛ ho hia.</p>

---

## Nsɛm a Ɛda Ho Adwene

- [Code of Conduct](#code-of-conduct)
- [Sɛnea Wobɛka Bɔne Ho Asɛm](#-how-to-report-bugs)
- [Sɛnea Wobɛda Nsɛm Foforɔ Ho Adwene](#-how-to-suggest-features)
- [Sɛnea Wobɛma Plugin](#-how-to-submit-a-plugin)
- [Sɛnea Wobɛma Pull Request](#-how-to-submit-a-pull-request)
- [Kɔkɔbɔ Nye (254 Languages)](#-translation-contributions-254-languages)
- [Nkɔsoɔ Setup](#-development-setup)

---

## Code of Conduct

Yɛyɛ adwuma sɛ yɛbɛma ɔdɔ ne ɔmanfoɔ nyinaa anigye.

- **Yɛyɛ ɔdɔ.** Fa ɔdɔ bɔ nkɔmɔ.
- **Yɛyɛ ɔkɛse.** Ma nsɛm a ɛyɛ mmerɛ ne nsɛm a ɛnyɛ mmerɛ.
- **Yɛyɛ ɔmanfoɔ.** Yɛyɛ adwuma wɔ nsɛm 254 mu na yɛfrɛ ɔmanfoɔ fi ɔman biara.
- **Nni abufuw.** Mmerɛ a ɛyɛ den wɔ abufuw a ɛyɛ bɔne biara.

---

## 🐛 Sɛnea Wobɛka Bɔne Ho Asɛm

1. Kɔ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pɛ **"New Issue"**
3. Pɛ **"Bug Report"** template
4. Ka ho:
   - WIA SOOM version (Settings → About)
   - OS ne version (Windows/macOS/Linux)
   - Nsɛm a ɛda ho adi
   - Nkyerɛkyerɛ a ɛda ho adi ne nea ɛyɛ nokware
   - Screenshots anaa terminal output sɛ ɛyɛ a

---

## 💡 Sɛnea Wobɛda Nsɛm Foforɔ Ho Adwene

1. Kɔ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pɛ **"New Issue"**
3. Pɛ **"Feature Request"** template
4. Kyerɛ:
   - Dɛn na wopɛ sɛ wopɛ
   - Sɛnea wopɛ sɛ ɛyɛ
   - Nsɛm a wopɛ sɛ wopɛ

---

## 🔌 Sɛnea Wobɛma Plugin

WIA SOOM wɔ plugin system a ɛyɛ den — wubetumi ayɛ wo plugin wɔ minit 5 mu.

### Ntɔkwaw
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kɛse Kɔkɔbɔ

Kenkan **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** no fa:
- API a ɛyɛ nokware
- Nsɛm a ɛyɛ nokware
- Nkyerɛkyerɛ a ɛyɛ nokware
- Nkyerɛkyerɛ a ɛyɛ nokware ne ɔmanfoɔ mmara

### Ma Wo Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Ka wo plugin kɔ `plugins/{your-plugin-name}/`
3. Ma Pull Request
4. Akyiri no, wo plugin bɛda Plugin Store mu ma ɔmanfoɔ nyinaa!

---

## 🔀 Sɛnea Wobɛma Pull Request

### Fa app a ɛyɛ kɛse (wia-soom)

1. Fork repository no
2. Bɔ feature branch: `git checkout -b feat/my-feature`
3. Yɛ nsɛm a ɛyɛ nokware
4. Test wɔ ha:
   ```bash
   ```
5. Commit fa nsɛm a ɛyɛ nokware:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push na bue PR kɔ `main`

### Commit Message Convention

| Prefix | Fa yɛ |
|--------|---------|
| `feat:` | Nsɛm foforɔ |
| `fix:` | Bɔne a wɔyɛ no |
| `docs:` | Nsɛm a ɛyɛ nokware nko |
| `refactor:` | Code a wɔyɛ no (nni nsɛm a ɛyɛ nokware) |
| `i18n:` | Kɔkɔbɔ a wɔyɛ no |
| `plugin:` | Plugin ho nsɛm |

### PR Checklist

- [ ] Code no yɛ nokware a nni nsɛm a ɛyɛ bɔne
- [ ] Nni nsɛm a ɛyɛ den (fa i18n keys)
- [ ] Nni `console.log` a ɛda ho adi wɔ production code mu
- [ ] Nsɛm a ɛda ho adi yɛ nokware

---

## 🌐 Kɔkɔbɔ Nye (254 Languages)

WIA SOOM yɛ **254 languages** — fi Amharic kɔ Zulu, ka Braille ne RTL languages ho.

### Sɛnea Kɔkɔbɔ Yɛ

- Base language file: `src/renderer/src/i18n/en.json`
- Nsɛm 254 nyinaa wɔ baabi koro mu
- Kɔkɔbɔ yɛ wɔ `scripts/translate-patch.js` (GPT-4o-mini API)

### Sɛnea Wobɛyɛ Kɔkɔbɔ

#### Ɔkwan 1: Sɛɛ kɔkɔbɔ a ɛyɛ bɔne

1. Hwehwɛ language file no: `src/renderer/src/i18n/{lang-code}.json`
2. Sɛɛ kɔkɔbɔ a ɛyɛ bɔne
3. Ma PR fa nsɛm no

#### Ɔkwan 2: Ka nsɛm a ɛyɛ bɔne
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Ɔkwan 3: Sɔ machine translations ho nsɛm

Nnipa a wɔyɛ 254 languages no yɛ machine-translated. Nnipa a wɔyɛ ɔmanfoɔ yɛ nsɛm a ɛyɛ nokware!

1. Pɛ wo language file no
2. Sɔ nsɛm a ɛda ho adi
3. Sɛɛ nsɛm a ɛyɛ bɔne anaa a ɛyɛ bɔne
4. Ma PR

### Language Codes

Yɛde standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) a wɔde regional variants a ɛyɛ a ɛho hia (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Nkɔsoɔ Setup

### Nsɛm a Ɛho Hia

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Nsɛm: Default 2GB heap no nni hia kɔ 254 language files + Monaco editor bundle (~38MB renderer). 

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

## 🙏 Meda Wo Akpe

Mia nyuie nyuie le WIA SOOM me na wòle agbe nyuie kple wòkɔwo ƒe nyateƒe.

Esi wòyɔ a typo, wòtrɔ a string, wòbɔ a plugin, anaa wòkɔ a major feature — **wòyɛ part of this story.**

---

<p align="center"><em>Wɔbɔe na ❤️ le SmileStory Inc. kple wòkɔwo ƒe nyateƒe.</em></p>