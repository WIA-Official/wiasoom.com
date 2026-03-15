<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribusi na WIA SOOM</h1>
<p align="center"><strong>Wi e lobi yu kontribusi!</strong></p>
<p align="center">Ofi a na wan bug fix, nanga feature, plugin, ofi translasi — ala kontribusi e mati.</p>

---

## Tabel fu Konten

- [Code of Conduct](#code-of-conduct)
- [Fa yu kan Reporti Bugs](#-fa-yu-kan-reporti-bugs)
- [Fa yu kan Sugesti Features](#-fa-yu-kan-sugesti-features)
- [Fa yu kan Submiti wan Plugin](#-fa-yu-kan-submiti-wan-plugin)
- [Fa yu kan Submiti wan Pull Request](#-fa-yu-kan-submiti-wan-pull-request)
- [Translation Contributions (254 Languages)](#-translation-contributions-254-languages)
- [Development Setup](#-development-setup)

---

## Code of Conduct

Wi e komit na fu gi wan welkom nanga inklusif ervaring fu ala.

- **Besi respetful.** Treati ala wan nanga digniteit.
- **Besi konstruktif.** Gi helpfu feedback, no destruktif kritik.
- **Besi inklusif.** Wi e supporti 254 languages nanga welkom kontribusanten fu ala kontry na Earth.
- **No harasment.** Zero tolerantie fu diskriminasi fu ala so.

---

## 🐛 Fa yu kan Reporti Bugs

1. Go na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Kieze di **"Bug Report"** template
4. Include:
   - WIA SOOM versie (Settings → About)
   - OS nanga versie (Windows/macOS/Linux)
   - Stappen fu reproduksi
   - Verwachte vs. werkelijke gedrag
   - Screenshots of terminal output als het mogelijk is

---

## 💡 Fa yu kan Sugesti Features

1. Go na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Kieze di **"Feature Request"** template
4. Beschrijvi:
   - Wat probleem yu e losi
   - Fa yu e imagine di e wroko
   - Any alternatieven yu e consideri

---

## 🔌 Fa yu kan Submiti wan Plugin

WIA SOOM abi wan poweful plugin systeem — yu kan buildi yu own plugin na 5 minuten.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Read di **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** fu:
- Kompleet API referensi
- Werkende voorbeelden
- Stap-voor-stap tutorials
- Best practices nanga security regels

### Submiti Yu Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Addi yu plugin na `plugins/{your-plugin-name}/`
3. Submiti wan Pull Request
4. Na review, yu plugin e kom na di Plugin Store fu ala gebruikers!

---

## 🔀 Fa yu kan Submiti wan Pull Request

### Fu di main app (wia-soom)

1. Fork di repository
2. Kreye wan feature branch: `git checkout -b feat/my-feature`
3. Maki yu changes
4. Testi lokal:
   ```bash
   ```
5. Commiti nanga wan klaro message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pushi nanga open wan PR tegen `main`

### Commit Message Convention

| Prefix | Use fu |
|--------|---------|
| `feat:` | Niu feature |
| `fix:` | Bug fix |
| `docs:` | Dokumentasi no |
| `refactor:` | Code restructuring (no behavior change) |
| `i18n:` | Translation updates |
| `plugin:` | Plugin-gerelateerde changes |

### PR Checklist

- [ ] Code run nanga no errors
- [ ] No hardcoded strings (use i18n keys)
- [ ] No `console.log` left na production code
- [ ] Bestaande tests e still passi

---

## 🌐 Translation Contributions (254 Languages)

WIA SOOM supporti **254 languages** — fu Amharic na Zulu, inklusi Braille nanga RTL languages.

### Fa Translation Wroko

- Base language file: `src/renderer/src/i18n/en.json`
- Ala 254 language files de na di same directory
- Translation e done via `scripts/translate-patch.js` (GPT-4o-mini API)

### Fa yu kan Kontribusi Translations

#### Optie 1: Fixi wan spesifieke translation

1. Findi di language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fixi di incorrect translation
3. Submiti wan PR nanga di change

#### Optie 2: Addi missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Optie 3: Reviewi machine translations

Many fu wi 254 languages ben machine-translated. Native speaker reviews e waardevol!

1. Picki yu language file
2. Reviewi di translations
3. Fixi any awkward of incorrect translations
4. Submiti wan PR

### Language Codes

Wi e use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) nanga regional variants whe yu nodi (e.g., `zh-CN`, `pt-BR`).

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
> Note: Di default 2GB heap no e naki fu di 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Tangi

Elk kontribusi mek WIA SOOM beter fu developers around di werld.

Of yu fix wan typo, translate wan string, build wan plugin, of add wan major feature — **yu de part fu dis stori.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>