<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contributing to WIA SOOM</h1>
<p align="center"><strong>We'd love your contributions!</strong></p>
<p align="center">Whether it's a bug fix, new feature, plugin, or translation — every contribution matters.</p>

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

We are committed to providing a welcoming and inclusive experience for everyone.

- **Be respectful.** Treat everyone with dignity.
- **Be constructive.** Offer helpful feedback, not destructive criticism.
- **Be inclusive.** We support 254 languages and welcome contributors from every country on Earth.
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
   - What problem you're solving
   - How you imagine it working
   - Any alternatives you've considered

---

## 🔌 How to Submit a Plugin

WIA SOOM has a powerful plugin system — you can build your own plugin in 5 minutes.

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
- All 254 language files are in the same directory
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

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

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

## 🙏 Tank Yuh

Every contribution make WIA SOOM better for developers 'round di world.

Whether yuh fix a typo, translate a string, build a plugin, or add a major feature — **yuh part a dis story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>