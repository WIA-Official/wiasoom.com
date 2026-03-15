<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kaselehlie i WIA SOOM</h1>
<p align="center"><strong>Ongosou a kosei i mwet a kosei!</strong></p>
<p align="center">Soun a kosei, a kosei, plugin, o a kosei — a kosei a kosei ehu.</p>

---

## Kaselehlie i Kaselehlie

- [Kaselehlie i Kaselehlie](#code-of-conduct)
- [Kaselehlie i Kasei](#-how-to-report-bugs)
- [Kaselehlie i Kosei](#-how-to-suggest-features)
- [Kaselehlie i Plugin](#-how-to-submit-a-plugin)
- [Kaselehlie i Pull Request](#-how-to-submit-a-pull-request)
- [Kaselehlie i Kosei (254 Languages)](#-translation-contributions-254-languages)
- [Kaselehlie i Development Setup](#-development-setup)

---

## Kaselehlie i Kaselehlie

Ongosou a kosei i mwet a kosei i mwet.

- **Kaselehlie.** Kaselehlie a mwet a kosei.
- **Kaselehlie i mwet.** Kaselehlie a mwet a kosei, a kosei a mwet.
- **Kaselehlie i mwet.** Ongosou a kosei i 254 languages a kosei a mwet a kosei.
- **A kosei a mwet.** A kosei a mwet a kosei i mwet.

---

## 🐛 Kaselehlie i Kasei

1. Kaselehlie i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kaselehlie **"New Issue"**
3. Kaselehlie i **"Bug Report"** template
4. Kaselehlie:
   - WIA SOOM version (Settings → About)
   - OS i version (Windows/macOS/Linux)
   - Kaselehlie i mwet
   - Kaselehlie i mwet i mwet
   - Screenshots o terminal output soun a kosei

---

## 💡 Kaselehlie i Kosei

1. Kaselehlie i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kaselehlie **"New Issue"**
3. Kaselehlie i **"Feature Request"** template
4. Kaselehlie:
   - A kosei a mwet a kosei
   - A mwet a kosei a mwet
   - A kosei a mwet a kosei

---

## 🔌 Kaselehlie i Plugin

WIA SOOM a kosei i mwet a plugin — a kosei a mwet a plugin i 5 minutes.

### Kaselehlie i mwet
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kaselehlie i mwet

Kaselehlie i **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** i:
- Kaselehlie i API
- Kaselehlie i mwet
- Kaselehlie i mwet i mwet
- Kaselehlie i mwet a mwet

### Kaselehlie i Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Kaselehlie i plugin i `plugins/{your-plugin-name}/`
3. Kaselehlie i Pull Request
4. Soun a mwet, a plugin a kosei i Plugin Store i mwet a kosei!

---

## 🔀 Kaselehlie i Pull Request

### I mwet app (wia-soom)

1. Fork i repository
2. Kaselehlie i feature branch: `git checkout -b feat/my-feature`
3. Kaselehlie i mwet
4. Kaselehlie i mwet:
   ```bash
   ```
5. Kaselehlie i mwet i mwet:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push i kosei i PR i `main`

### Kaselehlie i Kaselehlie i mwet

| Prefix | Kaselehlie i |
|--------|---------|
| `feat:` | Kosei a mwet |
| `fix:` | Kaselehlie i mwet |
| `docs:` | Kaselehlie i mwet |
| `refactor:` | Kaselehlie i mwet (a kosei a mwet) |
| `i18n:` | Kaselehlie i mwet |
| `plugin:` | Kaselehlie i mwet a plugin |

### PR Checklist

- [ ] Kosei a mwet i mwet
- [ ] A kosei a mwet a mwet (kaselehlie i18n keys)
- [ ] A `console.log` a kosei i production code
- [ ] Kaselehlie i mwet a mwet

---

## 🌐 Kaselehlie i Kosei (254 Languages)

WIA SOOM a kosei **254 languages** — soun a Amharic a Zulu, a kosei a Braille o RTL languages.

### Kaselehlie i Kosei

- Base language file: `src/renderer/src/i18n/en.json`
- A kosei 254 language files a mwet i mwet
- Kaselehlie i mwet i `scripts/translate-patch.js` (GPT-4o-mini API)

### Kaselehlie i Kosei i Kosei

#### Option 1: Kaselehlie i mwet a kosei

1. Kaselehlie i language file: `src/renderer/src/i18n/{lang-code}.json`
2. Kaselehlie i mwet a kosei
3. Kaselehlie i PR i mwet

#### Option 2: Kaselehlie i mwet a mwet
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Kaselehlie i machine translations

A kosei a kosei 254 languages a kosei i machine-translated. A mwet a mwet a mwet a mwet!

1. Kaselehlie i language file
2. Kaselehlie i mwet
3. Kaselehlie i mwet a mwet o a kosei a mwet
4. Kaselehlie i PR

### Kaselehlie i Language Codes

Ongosou a kosei i standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) a kosei i regional variants soun a kosei (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Kaselehlie i Development Setup

### Kaselehlie i mwet

- Node.js 18+
- npm 9+
- Git

### Kaselehlie
```bash
```
### Kaselehlie
```bash
```
> Kaselehlie: A default 2GB heap a kosei i mwet a kosei 254 language files + Monaco editor bundle (~38MB renderer).

### Kaselehlie i Project Structure
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

## 🙏 Kinisou

Ewe eko eko WIA SOOM pein a kisin a developers an pweini.

Ike eko a kisin a typo, eko a kisin a string, eko a kisin a plugin, or eko a kisin a major feature — **kini eko a part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>