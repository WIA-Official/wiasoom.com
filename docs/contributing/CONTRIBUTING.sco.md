<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contributin tae WIA SOOM</h1>
<p align="center"><strong>We'd love yer contributions!</strong></p>
<p align="center">Whether it's a bug fix, new feature, plugin, or translation — every contribution matters.</p>

---

## Table o Contents

- [Code o Conduct](#code-of-conduct)
- [Hoo tae Report Bugs](#-hoo-tae-report-bugs)
- [Hoo tae Suggest Features](#-hoo-tae-suggest-features)
- [Hoo tae Submit a Plugin](#-hoo-tae-submit-a-plugin)
- [Hoo tae Submit a Pull Request](#-hoo-tae-submit-a-pull-request)
- [Translation Contributions (254 Languages)](#-translation-contributions-254-languages)
- [Development Setup](#-development-setup)

---

## Code o Conduct

We are committed tae providin a welcomin an inclusive experience for ilkae.

- **Be respectful.** Treat ilkae wi dignity.
- **Be constructive.** Offer helpful feedback, no destructive criticism.
- **Be inclusive.** We support 254 languages an welcome contributors frae ilkae country on Earth.
- **No harassment.** Zero tolerance for discrimination o ony kind.

---

## 🐛 Hoo tae Report Bugs

1. Go tae [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choose the **"Bug Report"** template
4. Include:
   - WIA SOOM version (Settings → About)
   - OS an version (Windows/macOS/Linux)
   - Steps tae reproduce
   - Expected vs. actual behavior
   - Screenshots or terminal output if possible

---

## 💡 Hoo tae Suggest Features

1. Go tae [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choose the **"Feature Request"** template
4. Describe:
   - Whit problem ye're solvins
   - Hoo ye imagine it workin
   - Ony alternatives ye've considered

---

## 🔌 Hoo tae Submit a Plugin

WIA SOOM has a powerful plugin system — ye can build yer ain plugin in 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Read the **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** for:
- Complete API reference
- Workin examples
- Step-by-step tutorials
- Best practices an security rules

### Submit Yer Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Add yer plugin tae `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. After review, yer plugin appears in the Plugin Store for a' users!

---

## 🔀 Hoo tae Submit a Pull Request

### For the main app (wia-soom)

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/my-feature`
3. Mak yer changes
4. Test locally:
   ```bash
   ```
5. Commit wi a clear message:
   ```
   feat: add dark mode toggle tae settings
   ```
6. Push an open a PR against `main`

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

WIA SOOM supports **254 languages** — frae Amharic tae Zulu, includin Braille an RTL languages.

### Hoo Translation Works

- Base language file: `src/renderer/src/i18n/en.json`
- A' 254 language files are in the same directory
- Translation is done via `scripts/translate-patch.js` (GPT-4o-mini API)

### Hoo tae Contribute Translations

#### Option 1: Fix a specific translation

1. Find the language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fix the incorrect translation
3. Submit a PR wi the change

#### Option 2: Add missin keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Many o our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick yer language file
2. Review the translations
3. Fix ony awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) wi regional variants where needed (e.g., `zh-CN`, `pt-BR`).

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
> Note: The default 2GB heap is no enough due tae the 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Thank Ye

Every contribution maks WIA SOOM better fur developers aroond the warld.

Whether ye fix a typo, translate a string, build a plugin, or add a major feature — **ye are pairt o this story.**

---

<p align="center"><em>Built wi ❤️ by SmileStory Inc. an contributors worldwide.</em></p>