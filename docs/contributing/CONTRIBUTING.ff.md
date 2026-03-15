<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ndeer to WIA SOOM</h1>
<p align="center"><strong>Mi yidi no ndiyam!</strong></p>
<p align="center">So tawii ko bug fix, new feature, plugin, walla translation — kadi ndiyam fota.</p>

---

## Tabbiti Ndeer

- [Code of Conduct](#code-of-conduct)
- [Ko Hakkil Bugji](#-ko-hakkil-bugji)
- [Ko Hakkil Features](#-ko-hakkil-features)
- [Ko Ndeer Plugin](#-ko-ndeer-plugin)
- [Ko Ndeer Pull Request](#-ko-ndeer-pull-request)
- [Ndeer Translation (254 Languages)](#-ndeer-translation-254-languages)
- [Development Setup](#-development-setup)

---

## Code of Conduct

Min ngoni ko ɓe naatnude e jamma e nder ɓe.

- **Ndeer ko feere.** Ndeer kadi ɓe e jamma.
- **Ndeer ko jooni.** Ndeer ɓe e jamma, walaa ɓe ɓuri.
- **Ndeer ko ɓe naatnude.** Min ngoni 254 languages e ɓe naatnude ɓe e nder fow.
- **Alaa harasment.** Zero tolerance ko diskriminasyon e nder fow.

---

## 🐛 Ko Hakkil Bugji

1. Ndeer e [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Ndeer **"Bug Report"** template
4. Ndeer:
   - WIA SOOM version (Settings → About)
   - OS e version (Windows/macOS/Linux)
   - Ndeer e hakki
   - Ndeer e jooni e hakki
   - Screenshots walla terminal output so tawii

---

## 💡 Ko Hakkil Features

1. Ndeer e [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Ndeer **"Feature Request"** template
4. Ndeer:
   - Ko hakki maa njahdi
   - Ko ɓe njahdi e hakki
   - Ndeer e alternatifs ɓe njahdi

---

## 🔌 Ko Ndeer Plugin

WIA SOOM ngoni ko plugin system ɓuri — a waawi nder 5 minit e nder plugin maa.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Ndeer **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ko:
- API reference fota
- Ndeer e hakki
- Tutorials e hakki
- Best practices e security rules

### Ndeer Plugin Maa

1. Fork [Plugin Store](https://wiasoom.com)
2. Ndeer plugin maa e `plugins/{your-plugin-name}/`
3. Ndeer Pull Request
4. So tawii review, plugin maa ngoni e Plugin Store ko fow!

---

## 🔀 Ko Ndeer Pull Request

### Ko app main (wia-soom)

1. Fork repository
2. Ndeer feature branch: `git checkout -b feat/my-feature`
3. Ndeer hakki maa
4. Test locally:
   ```bash
   ```
5. Commit e message fota:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push e nder PR e `main`

### Commit Message Convention

| Prefix | Ndeer |
|--------|---------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation only |
| `refactor:` | Code restructuring (no behavior change) |
| `i18n:` | Translation updates |
| `plugin:` | Plugin-related changes |

### PR Checklist

- [ ] Code ngoni e jamma
- [ ] Ala hardcoded strings (nawni i18n keys)
- [ ] Ala `console.log` e production code
- [ ] Existing tests ngoni e jamma

---

## 🌐 Ndeer Translation (254 Languages)

WIA SOOM ngoni **254 languages** — e Amharic to Zulu, e nder Braille e RTL languages.

### Ko Ndeer Translation

- Base language file: `src/renderer/src/i18n/en.json`
- Fow 254 language files ngoni e nder ɓe
- Ndeer translation e `scripts/translate-patch.js` (GPT-4o-mini API)

### Ko Ndeer Translation

#### Option 1: Fix a specific translation

1. Ndeer e language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fix e translation ɓuri
3. Ndeer PR e hakki

#### Option 2: Ndeer keys ɓe njahdi
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Review machine translations

Fow ɓe 254 languages ngoni machine-translated. Reviews e native speaker ngoni ɓuri!

1. Ndeer e language file maa
2. Review e translations
3. Fix e translations ɓe njahdi walla ɓuri
4. Ndeer PR

### Language Codes

Min ngoni standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) e regional variants so tawii (e.g., `zh-CN`, `pt-BR`).

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
> Note: The default 2GB heap ngoni walaa e 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 A jaaraama

Kadi fowru naatnude WIA SOOM jooni e nder jeyaaɓe e nder leydi fow.

So a yidi hokkude kañum, jokkude nder laawol, jeyde plugin, walla aɗa waɗi gite maɓɓe — **aɗa jooɗi e nder ngol laawol.**

---

<p align="center"><em>Jeyaaɗo e ❤️ e SmileStory Inc. e jeyaaɓe fow e nder leydi.</em></p>