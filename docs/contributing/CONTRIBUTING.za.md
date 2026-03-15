<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Yiengh WIA SOOM</h1>
<p align="center"><strong>Roengz daih raemx gwn!</strong></p>
<p align="center">Baeq goj a bug fix, new feature, plugin, raeuj translation — raemx gwn goj mbouj daih.</p>

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

Roengz raemx gwn goj cungj raeuj yiengh raemx gwn raeuj.

- **Be respectful.** Cungj raemx gwn goj mbouj.
- **Be constructive.** Yiengh raemx gwn goj raemx gwn gwnz, mbouj raemx gwn gwnz.
- **Be inclusive.** Roengz raemx gwn goj 254 languages raeuj raemx gwn goj mbouj raeuj.
- **No harassment.** Mbouj raemx gwn goj discrimination goj mbouj.

---

## 🐛 How to Report Bugs

1. Go to [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choose the **"Bug Report"** template
4. Include:
   - WIA SOOM version (Settings → About)
   - OS raeuj version (Windows/macOS/Linux)
   - Steps to reproduce
   - Expected vs. actual behavior
   - Screenshots raeuj terminal output if possible

---

## 💡 How to Suggest Features

1. Go to [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choose the **"Feature Request"** template
4. Describe:
   - What problem you're solving
   - How you imagine it working
   - Any alternatives you've considered

---

## 🔌 How to Submit a Plugin

WIA SOOM yiengh raemx gwn goj plugin system — roengz raemx gwn goj build raeuj own plugin raeuj 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Read the **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** for:
- Complete API reference
- Working examples
- Step-by-step tutorials
- Best practices raeuj security rules

### Submit Your Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Add your plugin to `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. After review, your plugin appears in the Plugin Store for all users!

---

## 🔀 How to Submit a Pull Request

### For the main app (wia-soom)

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/my-feature`
3. Make your changes
4. Test locally:
   ```bash
   ```
5. Commit with a clear message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push raeuj open a PR against `main`

### Commit Message Convention

| Prefix | Use for |
|--------|---------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation only |
| `refactor:` | Code restructuring (no behavior change) |
| `i18n:` | Translation updates |
| `plugin:` | Plugin-related changes |

### PR Checklist

- [ ] Code runs without errors
- [ ] No hardcoded strings (use i18n keys)
- [ ] No `console.log` left in production code
- [ ] Existing tests still pass

---

## 🌐 Translation Contributions (254 Languages)

WIA SOOM yiengh raemx gwn goj **254 languages** — raeuj Amharic raeuj Zulu, gwnz Braille raeuj RTL languages.

### How Translation Works

- Base language file: `src/renderer/src/i18n/en.json`
- All 254 language files raeuj in the same directory
- Translation is done via `scripts/translate-patch.js` (GPT-4o-mini API)

### How to Contribute Translations

#### Option 1: Fix a specific translation

1. Find the language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fix the incorrect translation
3. Submit a PR with the change

#### Option 2: Add missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Many of our 254 languages raeuj machine-translated. Native speaker reviews raeuj incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward raeuj incorrect translations
4. Submit a PR

### Language Codes

Roengz raemx gwn goj standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) raeuj regional variants where needed (e.g., `zh-CN`, `pt-BR`).

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
> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Doj mbouj

Gwnz goengq daih WIA SOOM gwnz raemx gwn daih boux gwnz raemx doengh gwnz raemx.

Gwnz goengq raemx a typo, gwnz goengq raemx a string, gwnz goengq raemx a plugin, raemx gwnz goengq raemx a major feature — **neix gwnz boux raemx gwnz raemx.**

---

<p align="center"><em>Gwnz raemx ❤️ doengh SmileStory Inc. raemx gwnz boux gwnz raemx doengh gwnz raemx.</em></p>