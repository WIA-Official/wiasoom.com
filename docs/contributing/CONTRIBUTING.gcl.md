<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contributing to WIA SOOM</h1>
<p align="center"><strong>Wi a go love yuh contributions!</strong></p>
<p align="center">Whether it’s a bug fix, new feature, plugin, or translation — every contribution matters.</p>

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

Wi committed to providing a welcoming and inclusive experience for everybody.

- **Be respectful.** Treat everybody with dignity.
- **Be constructive.** Offer helpful feedback, not destructive criticism.
- **Be inclusive.** Wi support 254 languages and welcome contributors from every country pon Earth.
- **No harassment.** Zero tolerance for discrimination of any kind.

---

## 🐛 How to Report Bugs

1. Go to [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choose the **"Bug Report"** template
4. Include:
   - WIA SOOM version (Settings → About)
   - OS and version (Windows/macOS/Linux)
   - Steps to reproduce
   - Expected vs. actual behavior
   - Screenshots or terminal output if possible

---

## 💡 How to Suggest Features

1. Go to [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choose the **"Feature Request"** template
4. Describe:
   - What problem yuh solving
   - How yuh imagine it working
   - Any alternatives yuh consider

---

## 🔌 How to Submit a Plugin

WIA SOOM have a powerful plugin system — yuh can build yuh own plugin in 5 minutes.

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
- Best practices and security rules

### Submit Yuh Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Add yuh plugin to `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. After review, yuh plugin go appear in the Plugin Store for all users!

---

## 🔀 How to Submit a Pull Request

### For the main app (wia-soom)

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/my-feature`
3. Make yuh changes
4. Test locally:
   ```bash
   ```
5. Commit with a clear message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push and open a PR against `main`

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

WIA SOOM supports **254 languages** — from Amharic to Zulu, including Braille and RTL languages.

### How Translation Works

- Base language file: `src/renderer/src/i18n/en.json`
- All 254 language files dey in the same directory
- Translation done via `scripts/translate-patch.js` (GPT-4o-mini API)

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

Plenty of wi 254 languages was machine-translated. Native speaker reviews are incredibly valuable!

1. Pick yuh language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

Wi use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

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
> Note: The default 2GB heap not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Mesi

Chak kontribisyon fè WIA SOOM pi bon pou devlopè toutotou mond lan.

Si ou korije yon erè, tradwi yon chenn, bati yon plugin, oswa ajoute yon gwo karakteristik — **ou se yon pati nan istwa sa a.**

---

<p align="center"><em>Konstwi ak ❤️ pa SmileStory Inc. ak kontribitè atravè mond lan.</em></p>