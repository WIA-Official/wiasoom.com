<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contributing to WIA SOOM</h1>
<p align="center"><strong>Yumi laikim yupla kontribuson!</strong></p>
<p align="center">Sapos em i wanpela bug fix, nupela feature, plugin, o translation — olgeta kontribuson i gat bikpela samting.</p>

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Report Bugs](#-how-to-report-bugs)
- [How to Suggest Features](#-how-to-suggest-features)
- [How to Submit a Plugin](#-how-to-submit-a-plugin)
- [How to Submit a Pull Request](#-how-to-submit-a-pull-request)
- [Translation Contributions (254 Languages)](#-translation-contributions-254-languages)
- [Development Setup](#-development-setup)

---

## Code of Conduct

Yumi i stap long givim wanpela welcome na inclusive experience long olgeta.

- **Respectim olgeta.** Treatim olgeta long dignity.
- **Bikpela helpim.** Offerim helpim feedback, no destruktiv kritik.
- **Inclusive.** Yumi suportim 254 languages na welcomeim contributors long olgeta kantri long graun.
- **No harassment.** Zero tolerance long diskriminasyon bilong wanpela kain.

---

## 🐛 How to Report Bugs

1. Go long [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikim **"New Issue"**
3. Chusim **"Bug Report"** template
4. Include:
   - WIA SOOM version (Settings → About)
   - OS na version (Windows/macOS/Linux)
   - Steps bilong reproducem
   - Expected vs. actual behavior
   - Screenshots o terminal output sapos i possible

---

## 💡 How to Suggest Features

1. Go long [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikim **"New Issue"**
3. Chusim **"Feature Request"** template
4. Deskraibim:
   - Wanem problem yu i solvim
   - Hau yu i tingim em i wok
   - Ol narapela alternatives yu i tingim

---

## 🔌 How to Submit a Plugin

WIA SOOM i gat wanpela strong plugin system — yu inap bildim yu own plugin long 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Readim the **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** bilong:
- Kompleit API reference
- Wokim examples
- Step-by-step tutorials
- Best practices na security rul

### Submit Your Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Addim yu plugin long `plugins/{your-plugin-name}/`
3. Submitim wanpela Pull Request
4. After review, yu plugin i kamap long Plugin Store bilong olgeta yuser!

---

## 🔀 How to Submit a Pull Request

### For the main app (wia-soom)

1. Forkim the repository
2. Kreitim wanpela feature branch: `git checkout -b feat/my-feature`
3. Mekim yu changes
4. Testim long lokal:
   ```bash
   ```
5. Commitim wantaim wanpela klia message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pushim na openim wanpela PR agains `main`

### Commit Message Convention

| Prefix | Use for |
|--------|---------|
| `feat:` | Nupela feature |
| `fix:` | Bug fix |
| `docs:` | Documentation only |
| `refactor:` | Code restructuring (no behavior change) |
| `i18n:` | Translation updates |
| `plugin:` | Plugin-related changes |

### PR Checklist

- [ ] Code i ranim wantaim no errors
- [ ] No hardcoded strings (yusim i18n keys)
- [ ] No `console.log` i stap long production code
- [ ] Existing tests i stap pass

---

## 🌐 Translation Contributions (254 Languages)

WIA SOOM i suportim **254 languages** — long Amharic go long Zulu, i klia long Braille na RTL languages.

### Hau Translation I Wok

- Base language file: `src/renderer/src/i18n/en.json`
- Ol 254 language files i stap long sem directory
- Translation i wok long `scripts/translate-patch.js` (GPT-4o-mini API)

### Hau Long Kontribusim Translations

#### Option 1: Fixim wanpela spesifik translation

1. Painim language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fixim wanpela rong translation
3. Submitim wanpela PR wantaim change

#### Option 2: Addim missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Reviewim machine translations

Plenti bilong yumi 254 languages i bin machine-translated. Native speaker reviews i gat bikpela value!

1. Pikim yu language file
2. Reviewim ol translations
3. Fixim ol awkward o rong translations
4. Submitim wanpela PR

### Language Codes

Yumi yusim standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) wantaim regional variants sapos i nidim (e.g., `zh-CN`, `pt-BR`).

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
> Note: The default 2GB heap i no inap long 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Tenkyu

Ol kontribusen i mekim WIA SOOM i gutpela moa long ol developa long olgeta hap bilong wol.

Sapos yu lukautim wanpela typo, tansletim wanpela string, bildim wanpela plugin, o addim wanpela bikpela fitur — **yu i wanpela part bilong dispela stori.**

---

<p align="center"><em>Bilong ❤️ long SmileStory Inc. na ol kontribusa long olgeta hap bilong wol.</em></p>