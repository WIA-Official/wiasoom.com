<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kedul a WIA SOOM</h1>
<p align="center"><strong>Ng meral a uldes a kmo!</strong></p>
<p align="center">Ng di kmo a bug fix, ngeruul, plugin, a uldes — ng meral a uldes el chad.</p>

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

Ng meral a ikel a klungel a uldes a chad el ngii.

- **Mek a chad.** Chul a uldes el chad.
- **Mek a ngara.** Kmo a uldes el chad, a ng meral a chad.
- **Mek a klungel.** Ng meral a 254 languages a ng meral a uldes el chad el chad a uldes el chad.
- **Ng meral a chad.** Zero tolerance el chad a uldes el chad.

---

## 🐛 How to Report Bugs

1. Ng di [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Chul a **"Bug Report"** template
4. Include:
   - WIA SOOM version (Settings → About)
   - OS a version (Windows/macOS/Linux)
   - Steps a chad
   - Expected vs. actual behavior
   - Screenshots a terminal output ng meral

---

## 💡 How to Suggest Features

1. Ng di [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Chul a **"Feature Request"** template
4. Describe:
   - Ng di a uldes a uldes
   - Ng di a uldes a uldes
   - Ng meral a alternatives a uldes

---

## 🔌 How to Submit a Plugin

WIA SOOM a ikel a plugin system — ng meral a uldes a kmo a plugin el 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Read the **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** el:
- Complete API reference
- Working examples
- Step-by-step tutorials
- Best practices a security rules

### Submit Your Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Add a plugin a `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. Ng di review, a plugin a uldes a ikel el Plugin Store el chad a uldes!

---

## 🔀 How to Submit a Pull Request

### El main app (wia-soom)

1. Fork a repository
2. Create a feature branch: `git checkout -b feat/my-feature`
3. Mek a uldes
4. Test locally:
   ```bash
   ```
5. Commit a clear message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push a open a PR el `main`

### Commit Message Convention

| Prefix | Use for |
|--------|---------|
| `feat:` | Ngeruul |
| `fix:` | Bug fix |
| `docs:` | Documentation el chad |
| `refactor:` | Code restructuring (no behavior change) |
| `i18n:` | Translation updates |
| `plugin:` | Plugin-related changes |

### PR Checklist

- [ ] Code a uldes el chad a uldes
- [ ] No hardcoded strings (use i18n keys)
- [ ] No `console.log` a uldes el production code
- [ ] Existing tests a uldes el chad

---

## 🌐 Translation Contributions (254 Languages)

WIA SOOM a ikel **254 languages** — el Amharic a Zulu, a ikel Braille a RTL languages.

### How Translation Works

- Base language file: `src/renderer/src/i18n/en.json`
- All 254 language files a ikel el chad directory
- Translation a uldes el `scripts/translate-patch.js` (GPT-4o-mini API)

### How to Contribute Translations

#### Option 1: Fix a specific translation

1. Find a language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fix a incorrect translation
3. Submit a PR el a change

#### Option 2: Add missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Ng meral a 254 languages a ikel machine-translated. Native speaker reviews a ikel a uldes a uldes!

1. Pick a language file
2. Review a translations
3. Fix a awkward a incorrect translations
4. Submit a PR

### Language Codes

Ng meral a standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) a ikel regional variants el chad (e.g., `zh-CN`, `pt-BR`).

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
> Note: A default 2GB heap a ng meral a uldes el chad a 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Melekeu er a Ongerel

Ng diak a ngara WIA SOOM el mo er a klung a melem a ngara el mo er a kirel a diak a kirel.

Ng diak a mo er a ngara a ngara, a kirel a string, a chad a plugin, a mo er a klung a melem — **a diak a mo er a diak a kirel.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>