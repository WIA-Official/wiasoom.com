<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ekaraki e WIA SOOM</h1>
<p align="center"><strong>Ngara e aei ngara e aei!</strong></p>
<p align="center">Ekaraki n arung, e aei, plugin, n ekaraki — e aei ngara e aei.</p>

---

## Tabel e Taim

- [Kodo e Taim](#kodo-e-taim)
- [Ekaraki n Bugs](#-ekaraki-n-bugs)
- [Ekaraki n Features](#-ekaraki-n-features)
- [Ekaraki n Plugin](#-ekaraki-n-plugin)
- [Ekaraki n Pull Request](#-ekaraki-n-pull-request)
- [Ekaraki n Ekaraki (254 Languages)](#-ekaraki-n-ekaraki-254-languages)
- [Ekaraki n Development](#-ekaraki-n-development)

---

## Kodo e Taim

Ekaraki n e aei n e aei n e aei n e aei.

- **Ekaraki n aei.** Ekaraki n e aei n e aei.
- **Ekaraki n aei.** Ekaraki n e aei, n aei n e aei.
- **Ekaraki n aei.** Ekaraki n 254 languages n e aei n e aei.
- **No harassment.** Ekaraki n aei n e aei.

---

## 🐛 Ekaraki n Bugs

1. Ekaraki n [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Ekaraki **"New Issue"**
3. Ekaraki n **"Bug Report"** template
4. Ekaraki:
   - WIA SOOM version (Settings → About)
   - OS n version (Windows/macOS/Linux)
   - Steps n ekaraki
   - Expected vs. actual behavior
   - Screenshots n terminal output n e aei

---

## 💡 Ekaraki n Features

1. Ekaraki n [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Ekaraki **"New Issue"**
3. Ekaraki n **"Feature Request"** template
4. Ekaraki:
   - Ekaraki n e aei
   - Ekaraki n e aei
   - Ekaraki n e aei

---

## 🔌 Ekaraki n Plugin

WIA SOOM ekaraki n e aei n plugin — ekaraki n aei plugin n 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Ekaraki n **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** n:
- Ekaraki n API reference
- Ekaraki n e aei
- Ekaraki n e aei
- Ekaraki n e aei n e aei

### Ekaraki n Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Ekaraki n plugin n `plugins/{your-plugin-name}/`
3. Ekaraki n Pull Request
4. Ekaraki n review, ekaraki n plugin n e Plugin Store n e aei!

---

## 🔀 Ekaraki n Pull Request

### Ekaraki n main app (wia-soom)

1. Fork n repository
2. Ekaraki n feature branch: `git checkout -b feat/my-feature`
3. Ekaraki n e aei
4. Ekaraki n local:
   ```bash
   ```
5. Ekaraki n e aei n e aei:
   ```
   feat: add dark mode toggle to settings
   ```
6. Ekaraki n open n PR n `main`

### Ekaraki n Commit Message Convention

| Prefix | Ekaraki n |
|--------|---------|
| `feat:` | Ekaraki n e aei |
| `fix:` | Ekaraki n bug |
| `docs:` | Ekaraki n documentation n e aei |
| `refactor:` | Ekaraki n code restructuring (no behavior change) |
| `i18n:` | Ekaraki n translation updates |
| `plugin:` | Ekaraki n plugin-related changes |

### PR Checklist

- [ ] Ekaraki n e aei n e aei
- [ ] No hardcoded strings (use i18n keys)
- [ ] No `console.log` n production code
- [ ] Ekaraki n e aei n e aei

---

## 🌐 Ekaraki n Ekaraki (254 Languages)

WIA SOOM ekaraki n **254 languages** — n Amharic n Zulu, ekaraki n Braille n RTL languages.

### Ekaraki n Ekaraki

- Base language file: `src/renderer/src/i18n/en.json`
- Ekaraki n 254 language files n e aei
- Ekaraki n e aei n `scripts/translate-patch.js` (GPT-4o-mini API)

### Ekaraki n Ekaraki n Ekaraki

#### Option 1: Ekaraki n e aei

1. Ekaraki n language file: `src/renderer/src/i18n/{lang-code}.json`
2. Ekaraki n e aei
3. Ekaraki n PR n e aei

#### Option 2: Ekaraki n e aei
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Ekaraki n machine translations

Ekaraki n e aei n 254 languages ekaraki n machine-translated. Ekaraki n native speaker reviews n e aei!

1. Ekaraki n language file
2. Ekaraki n e aei
3. Ekaraki n e aei n e aei
4. Ekaraki n PR

### Language Codes

Ekaraki n standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) n regional variants n e aei (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Ekaraki n Development

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Ekaraki n
```bash
```
### Ekaraki n
```bash
```
> Note: Ekaraki n default 2GB heap ekaraki n e aei n e aei n 254 language files + Monaco editor bundle (~38MB renderer).

### Ekaraki n Project Structure
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

## 🙏 Ngi nmoa

Ekkar nmoa e WIA SOOM ebu raweiy e aibwanga e aibwanga.

Ekkar ngko ebu a typo, nmoa ebu a string, buid a plugin, o add a major feature — **ngko e part e nmoa.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>