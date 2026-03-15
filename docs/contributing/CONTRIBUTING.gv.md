<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Cur ad WIA SOOM</h1>
<p align="center"><strong>Ta mee er ny choyrt dy vel do choyrt er ny chione!</strong></p>
<p align="center">Ny s'cosoylagh eh dy bee'n bug, feer ny s'ghoo, plug-in, ny targhyss — ta'n chooilley choyrt ynsagh.</p>

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

Ta shin er ny choyrt dy vel cur er ny chione dy chooilley duillag.

- **Bee respecktil.** Darragh dy vel yindyssagh.
- **Bee constructif.** Cur er ny chione dy vel yn choyrt shickyr, cha nel ny choyrt shickyr.
- **Bee inclusive.** Ta shin er ny choyrt dy vel 254 gailckyn as ta shin cur er ny chione dy choyrt er ny chione veih ny h-ynnyd.
- **Cha nel harassagh.** Ny chaghlaa dy vel ny chaghlaa er ny chione.

---

## 🐛 How to Report Bugs

1. Goll gys [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choyr dy chooilley **"Bug Report"** template
4. Cur er:
   - WIA SOOM version (Settings → About)
   - OS as version (Windows/macOS/Linux)
   - Steatyn dy vel cur er ny chione
   - Behaviour aaght vs. ynsagh
   - Screenshots ny terminal output myr shickyr

---

## 💡 How to Suggest Features

1. Goll gys [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Choyr dy chooilley **"Feature Request"** template
4. Cur er:
   - Cre ta'n phobble ta's cur er ny chione
   - Cre ta'n choyrt ta's cur er ny chione
   - Ny h-ailt er ny choyrt ta's cur er ny chione

---

## 🔌 How to Submit a Plugin

Ta WIA SOOM er ny choyrt dy vel system plug-in shickyr — ta's dooinney dy choyrt do plug-in ayns 5 mynt.

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
2. Cur do plug-in gys `plugins/{your-plugin-name}/`
3. Submit a Pull Request
4. Ny chaghlaa, ta do plug-in er ny chione ayns y Plugin Store son ny h-ynnyd!

---

## 🔀 How to Submit a Pull Request

### Son y app main (wia-soom)

1. Fork y repository
2. Cur branch feer: `git checkout -b feat/my-feature`
3. Cur do chaghlaa
4. Test locally:
   ```bash
   ```
5. Commit gys message shickyr:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push as goll er PR gys `main`

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
- [ ] Cha nel strings hardcoded (use i18n keys)
- [ ] Cha nel `console.log` er ny chione ayns code production
- [ ] Ta'n tests er ny chione shickyr

---

## 🌐 Translation Contributions (254 Languages)

Ta WIA SOOM er ny choyrt dy vel **254 gailckyn** — veih Amharic gys Zulu, goaill in Braille as gailckyn RTL.

### Cre Ta Translation Works

- Faiyl gailckyn bun: `src/renderer/src/i18n/en.json`
- Ta'n 254 gailckyn ooilley ayns y dirree shoh
- Ta translation er ny choyrt gys `scripts/translate-patch.js` (GPT-4o-mini API)

### Cre Ta Cur er Translations

#### Option 1: Fix a specific translation

1. Find y faiyl gailckyn: `src/renderer/src/i18n/{lang-code}.json`
2. Fix y translation shickyr
3. Submit a PR gys y chaghlaa

#### Option 2: Add missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Ta'n 254 gailckyn shoh er ny choyrt dy vel machine-translated. Ta reviews veih ny h-ynnyd shickyr!

1. Pick do faiyl gailckyn
2. Review y translations
3. Fix ny translations shickyr ny shickyr
4. Submit a PR

### Language Codes

Ta shin er ny choyrt dy vel codes ISO 639-1 shickyr (e.g., `ko`, `en`, `ja`, `ar`, `hi`) gys ny h-ailt er ny choyrt ta's cur er ny chione (e.g., `zh-CN`, `pt-BR`).

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
> Note: Ta'n default 2GB heap cha nel doshyn dy vel 254 gailckyn + Monaco editor bundle (~38MB renderer).

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

## 🙏 Gura mie eu

Ta kione erbee aym WIA SOOM ny smoo dy chooilley devs er y theihll.

Ny s'jerrey dy vel oo goaill er cooish, troggal er strinj, cur magh plugin, ny geddyn yn feer chooilley — **ta oo ny phart jeh'n chaarj shoh.**

---

<p align="center"><em>Cur er ny geddyn ❤️ veih SmileStory Inc. as ny contribuitors er y theihll.</em></p>