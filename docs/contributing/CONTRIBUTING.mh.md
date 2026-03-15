<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Jab in WIA SOOM</h1>
<p align="center"><strong>Jab in aolep in jorren!</strong></p>
<p align="center">Kwoj in jab in, eok, plugin, ak jorren — aolep in jab in eok.</p>

---

## Kaji in Kōmān

- [Kōmān in Kōmān](#code-of-conduct)
- [Kōj in Kōmān](#-how-to-report-bugs)
- [Kōj in Jorren](#-how-to-suggest-features)
- [Kōj in Plugin](#-how-to-submit-a-plugin)
- [Kōj in Pull Request](#-how-to-submit-a-pull-request)
- [Jorren in Jab (254 Kōmān)](#-translation-contributions-254-languages)
- [Kōmān in Kōmān](#-development-setup)

---

## Kōmān in Kōmān

Jab in aolep in jorren ak jorren in kōmān in aolep.

- **Kōmān in jorren.** Jab in aolep in jorren.
- **Kōmān in kōmān.** Jab in aolep in jorren, eok jab in kōmān.
- **Kōmān in jorren.** Kōmān in 254 kōmān ak jab in aolep in jorren in aolep.
- **Ejjab in jorren.** Zero tolerance in kōmān in aolep.

---

## 🐛 Kōj in Kōmān

1. Kōj in [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kōj **"New Issue"**
3. Kōj in **"Bug Report"** template
4. Kōj in:
   - WIA SOOM version (Settings → About)
   - OS ak version (Windows/macOS/Linux)
   - Kōj in kōmān
   - Kōj in kōmān ak kōmān in kōmān
   - Screenshots ak terminal output in kōmān

---

## 💡 Kōj in Jorren

1. Kōj in [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kōj **"New Issue"**
3. Kōj in **"Feature Request"** template
4. Kōj in:
   - Ejjab in kōmān in kōmān
   - Ejjab in kōmān in kōmān
   - Kōj in kōmān in kōmān

---

## 🔌 Kōj in Plugin

WIA SOOM ej jab in plugin system — kwoj in jab in plugin in 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Kōj in **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** in:
- Kōmān API reference
- Kōmān in kōmān
- Kōj in kōmān
- Best practices ak security rules

### Kōj in Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Kōj in plugin in `plugins/{your-plugin-name}/`
3. Kōj in Pull Request
4. Kōj in review, plugin in kwoj in Plugin Store in aolep in jab in!

---

## 🔀 Kōj in Pull Request

### For the main app (wia-soom)

1. Fork in repository
2. Kōj in feature branch: `git checkout -b feat/my-feature`
3. Kōj in kōmān
4. Test in local:
   ```bash
   ```
5. Commit in kōmān:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ak kōj in PR against `main`

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

## 🌐 Jorren in Jab (254 Kōmān)

WIA SOOM ej jab in **254 kōmān** — from Amharic to Zulu, including Braille ak RTL languages.

### Kōj in Jorren

- Base language file: `src/renderer/src/i18n/en.json`
- Aolep 254 language files ej in the same directory
- Jorren ej kōj in `scripts/translate-patch.js` (GPT-4o-mini API)

### Kōj in Jorren

#### Option 1: Kōj in jorren in kōmān

1. Kōj in language file: `src/renderer/src/i18n/{lang-code}.json`
2. Kōj in kōmān in kōmān
3. Kōj in PR in kōmān

#### Option 2: Kōj in missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Kōj in review machine translations

Aolep in 254 kōmān ej machine-translated. Native speaker reviews ej valuable in aolep!

1. Kōj in language file
2. Kōj in jorren
3. Kōj in kōmān in kōmān ak kōmān in kōmān
4. Kōj in PR

### Language Codes

Jab in standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ak regional variants in kōmān (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Kōmān in Kōmān

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Kōmān
```bash
```
### Build
```bash
```
> Note: The default 2GB heap ej jab in kōmān in aolep in 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Komol tata

Ejjab in wōt in WIA SOOM kōmṃan in developers in wōt in jikin.

Kōmṃan in eṃṃan a typo, translate a string, build a plugin, ak add a major feature — **kōmṃan in ej part in jikin.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>