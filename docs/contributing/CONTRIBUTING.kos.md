<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Koneh WIA SOOM</h1>
<p align="center"><strong>We'd love your contributions!</strong></p>
<p align="center">Sōmōn kīr, kīr, plugin, o kīr kīr — kīr kīr sōmōn.</p>

---

## Tōbōn o Kōntents

- [Kōd o Kōndakt](#kōd-o-kōndakt)
- [Kōmōn kīr Bug](#-kōmōn-kīr-bug)
- [Kōmōn kīr Kīr](#-kōmōn-kīr-kīr)
- [Kōmōn kīr Plugin](#-kōmōn-kīr-plugin)
- [Kōmōn kīr Pull Request](#-kōmōn-kīr-pull-request)
- [Kīr Kōntrībūshōn (254 Kōl)](#-kīr-kōntrībūshōn-254-kōl)
- [Dēvōlōpment Setup](#-dēvōlōpment-setup)

---

## Kōd o Kōndakt

Mōn kīr kīr kīr nēnōn o kīr sōmōn.

- **Kīr respect.** Kīr sōmōn kīr kīr.
- **Kīr constructive.** Kōmōn kīr kīr kīr, nēnōn kīr kīr.
- **Kīr inclusive.** Mōn kīr sōmōn 254 kōl o kīr kīr kīr kīr kīr.
- **No harassment.** Zero tolerance kīr discrimination o kīr.

---

## 🐛 Kōmōn kīr Bug

1. Kōmōn kīr [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kōmōn **"New Issue"**
3. Kōmōn kīr **"Bug Report"** template
4. Kōmōn:
   - WIA SOOM version (Settings → About)
   - OS o version (Windows/macOS/Linux)
   - Steps kīr reproduce
   - Expected vs. actual behavior
   - Screenshots o terminal output sōmōn

---

## 💡 Kōmōn kīr Kīr

1. Kōmōn kīr [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kōmōn **"New Issue"**
3. Kōmōn kīr **"Feature Request"** template
4. Kōmōn:
   - Kīr problem kīr sōmōn
   - Kīr kīr kīr kīr
   - Kīr alternatives kīr kīr

---

## 🔌 Kōmōn kīr Plugin

WIA SOOM mōn kīr plugin system — kīr kīr kīr plugin kīr 5 minutes.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Kōmōn kīr **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** kīr:
- Complete API reference
- Working examples
- Step-by-step tutorials
- Best practices o security rules

### Kōmōn kīr Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Kōmōn kīr plugin kīr `plugins/{your-plugin-name}/`
3. Kōmōn kīr Pull Request
4. Kīr review, kīr plugin kīr kīr kīr Plugin Store kīr kīr!

---

## 🔀 Kōmōn kīr Pull Request

### Kīr main app (wia-soom)

1. Fork kīr repository
2. Kōmōn kīr feature branch: `git checkout -b feat/my-feature`
3. Kōmōn kīr changes
4. Test locally:
   ```bash
   ```
5. Commit kīr kīr message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push o open kīr PR kīr `main`

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

## 🌐 Kīr Kōntrībūshōn (254 Kōl)

WIA SOOM mōn **254 kōl** — kīr Amharic kīr Zulu, kīr Braille o RTL languages.

### Kīr Kōntrībūshōn Kīr

- Base language file: `src/renderer/src/i18n/en.json`
- Kīr 254 language files kīr kīr kīr kīr
- Kīr kīr kīr `scripts/translate-patch.js` (GPT-4o-mini API)

### Kōmōn kīr Kōntrībūshōn Kīr

#### Option 1: Fix a specific translation

1. Kōmōn kīr language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fix kīr incorrect translation
3. Kōmōn kīr PR kīr kīr

#### Option 2: Add missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Kīr kīr 254 kōl kīr machine-translated. Native speaker reviews kīr incredibly valuable!

1. Kōmōn kīr language file
2. Review kīr translations
3. Fix kīr awkward o incorrect translations
4. Kōmōn kīr PR

### Language Codes

Mōn kīr standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) kīr regional variants kīr kīr (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Dēvōlōpment Setup

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
> Note: Kīr default 2GB heap kīr nēnōn kīr 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Kasei

Ewe toso WIA SOOM meke soom for developers ian ekke. 

Mwe toso e soom, toso e string, wia a plugin, o toso e major feature — **kasei ian e story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>