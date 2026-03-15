<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kümeñ WIA SOOM</h1>
<p align="center"><strong>Ñi pu che ñi kümeñ!</strong></p>
<p align="center">Iñchiñ, ñi pu che ñi kümeñ, ñi pu che ñi pu züñ, plugin, o traducción — ñi kümeñ ñi pu che ñi ñi mapu.</p>

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

Ñi pu che ñi kümeñ ñi ñi mapu, ñi pu che ñi ñi ñi ñi.

- **Ñi pu che ñi züñ.** Ñi pu che ñi ñi ñi ñi.
- **Ñi pu che ñi kümeñ.** Ñi pu che ñi ñi ñi ñi ñi ñi.
- **Ñi pu che ñi ñi.** Ñi pu che ñi 254 ñi pu che ñi ñi ñi ñi.
- **Ñi pu che ñi ñi.** Ñi pu che ñi ñi ñi ñi ñi.

---

## 🐛 How to Report Bugs

1. Küyen [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Küyen **"New Issue"**
3. Küyen **"Bug Report"** template
4. Küyen:
   - WIA SOOM version (Settings → About)
   - OS y version (Windows/macOS/Linux)
   - Ñi pu che ñi ñi
   - Ñi pu che vs. ñi pu che ñi
   - Screenshots o terminal output ñi ñi

---

## 💡 How to Suggest Features

1. Küyen [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Küyen **"New Issue"**
3. Küyen **"Feature Request"** template
4. Küyen:
   - Ñi pu che ñi ñi
   - Ñi pu che ñi ñi
   - Ñi pu che ñi ñi

---

## 🔌 How to Submit a Plugin

WIA SOOM ñi ñi plugin ñi ñi — ñi pu che ñi ñi plugin ñi 5 minutos.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Küyen **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ñi:
- Ñi pu che API
- Ñi pu che ñi
- Ñi pu che ñi ñi
- Ñi pu che ñi ñi

### Submit Your Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Küyen ñi plugin ñi `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. Ñi pu che ñi ñi, ñi plugin ñi ñi Plugin Store ñi ñi ñi!

---

## 🔀 How to Submit a Pull Request

### Ñi ñi app (wia-soom)

1. Fork ñi repository
2. Küyen ñi feature branch: `git checkout -b feat/my-feature`
3. Küyen ñi ñi
4. Test ñi ñi:
   ```bash
   ```
5. Commit ñi ñi ñi:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push y open a PR ñi `main`

### Commit Message Convention

| Prefix | Use for |
|--------|---------|
| `feat:` | Ñi ñi |
| `fix:` | Ñi ñi |
| `docs:` | Ñi ñi |
| `refactor:` | Ñi ñi (ñi ñi ñi) |
| `i18n:` | Ñi ñi |
| `plugin:` | Ñi ñi ñi |

### PR Checklist

- [ ] Ñi ñi ñi ñi
- [ ] Ñi ñi ñi ñi (use i18n keys)
- [ ] Ñi ñi `console.log` ñi ñi ñi
- [ ] Ñi ñi ñi ñi

---

## 🌐 Translation Contributions (254 Languages)

WIA SOOM ñi **254 ñi pu che** — ñi Amharic ñi Zulu, ñi Braille y ñi RTL ñi.

### How Translation Works

- Base language file: `src/renderer/src/i18n/en.json`
- Ñi 254 ñi pu che ñi ñi ñi
- Ñi ñi ñi `scripts/translate-patch.js` (GPT-4o-mini API)

### How to Contribute Translations

#### Option 1: Fix a specific translation

1. Küyen ñi pu che file: `src/renderer/src/i18n/{lang-code}.json`
2. Küyen ñi ñi ñi
3. Submit a PR ñi ñi

#### Option 2: Add missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Ñi ñi ñi 254 ñi pu che ñi ñi ñi. Ñi pu che ñi ñi ñi ñi!

1. Küyen ñi pu che file
2. Küyen ñi ñi
3. Küyen ñi ñi ñi
4. Submit a PR

### Language Codes

Ñi ñi standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ��i ñi ñi ñi (e.g., `zh-CN`, `pt-BR`).

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
> Note: Ñi ñi 2GB heap ñi ñi ñi ñi 254 ñi pu che ñi + Monaco editor bundle (~38MB renderer).

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

Ñi pu zungun ñi WIA SOOM ñi pu peñi ñi pu mapu ñi pu ñi pu ñi.

Iñchiñ, iñchiñ ñi pu ñi, iñchiñ ñi pu ñi, iñchiñ ñi pu ñi, iñchiñ ñi pu ñi — **iñchiñ ñi pu ñi ñi.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>