<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Njàngaan ci WIA SOOM</h1>
<p align="center"><strong>Nu bëgg nga xamle!</strong></p>
<p align="center">Bu njëkk, bu jàpp, plugin, walla tarjam — bu njëkk xamle la.</p>

---

## Tàggat bu Ndaw

- [Kood bu Jàmm](#code-of-conduct)
- [Naka lañuy Jàpp Bug](#-how-to-report-bugs)
- [Naka lañuy Jàpp Fitur](#-how-to-suggest-features)
- [Naka lañuy Jàpp Plugin](#-how-to-submit-a-plugin)
- [Naka lañuy Jàpp Pull Request](#-how-to-submit-a-pull-request)
- [Tarjam Contributions (254 Languages)](#-translation-contributions-254-languages)
- [Njàngaan ci Development](#-development-setup)

---

## Kood bu Jàmm

Nuy jàpp ci jàmm ak njaxlaf, ngir keneen.

- **Jàmm.** Jàppale keneen ak xam-xam.
- **Jàmm ci jàmm.** Jàppale ak xam-xam, waaye bu jàmmul.
- **Jàmm ci jàmm.** Nuy jàpp 254 languages ak jàppale keneen ci bés bu nekk ci àdduna.
- **Dama jàmmul.** Jàmm bu nekk ci jàmm.

---

## 🐛 Naka lañuy Jàpp Bug

1. Nanu ci [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tànn **"New Issue"**
3. Tànn template **"Bug Report"**
4. Nanu jëfandikoo:
   - WIA SOOM version (Settings → About)
   - OS ak version (Windows/macOS/Linux)
   - Njàngaan ngir jàpp
   - Jàmm ak jàmm bu xam
   - Screenshots walla terminal output bu am

---

## 💡 Naka lañuy Jàpp Fitur

1. Nanu ci [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tànn **"New Issue"**
3. Tànn template **"Feature Request"**
4. Jàppale:
   - Naka la jàmm bi nga jàpp
   - Naka la nga xamle ni it jàpp
   - Kàddu yu amul ci jàmm

---

## 🔌 Naka lañuy Jàpp Plugin

WIA SOOM am na plugin system bu mag — nga jàpp plugin la ci 5 minit.

### Njàngaan bu Njàng
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Njàngaan bu Topp

Jàppale **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ngir:
- API bu jëm
- Njàngaan yu jëm
- Njàngaan ci jàmm
- Jàmm ak jàmm ci jàmm

### Jàpp Plugin Sa

1. Fork [Plugin Store](https://wiasoom.com)
2. Jàpp plugin sa ci `plugins/{your-plugin-name}/`
3. Jàpp Pull Request
4. Ba jàpp, plugin sa dina am ci Plugin Store ngir nit ñi!

---

## 🔀 Naka lañuy Jàpp Pull Request

### Ngir app bu jëm (wia-soom)

1. Fork repository bi
2. Jàpp branch bu fitur: `git checkout -b feat/my-feature`
3. Jàppale sa yeneen
4. Test ci ndaje:
   ```bash
   ```
5. Commit ak kàddu bu jàmm:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ak jàpp PR ci `main`

### Kàddu bu Commit

| Prefix | Jàpp ngir |
|--------|---------|
| `feat:` | Fitur bu jëm |
| `fix:` | Jàpp bug |
| `docs:` | Documentation ci jàmm |
| `refactor:` | Jàppale code (bu jàmmul) |
| `i18n:` | Tarjam yu am |
| `plugin:` | Jàppale ci plugin |

### PR Checklist

- [ ] Code bi jëm ci jàmm
- [ ] Nuy amul strings bu hardcoded (jefandikoo i18n keys)
- [ ] Nuy amul `console.log` ci production code
- [ ] Tests yu am jëm ci jàmm

---

## 🌐 Tarjam Contributions (254 Languages)

WIA SOOM jàpp **254 languages** — ci Amharic ak Zulu, jàppale Braille ak languages RTL.

### Naka Tarjam bi Jëm

- File bu base language: `src/renderer/src/i18n/en.json`
- Nuy am 254 language files ci sama directory
- Tarjam bi jëm ci `scripts/translate-patch.js` (GPT-4o-mini API)

### Naka lañuy Jàpp Tarjam

#### Option 1: Jàpp tarjam bu amul

1. Jàpp file bu language: `src/renderer/src/i18n/{lang-code}.json`
2. Jàppale tarjam bu amul
3. Jàpp PR ak jàmm

#### Option 2: Jàpp keys bu amul
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Jàpp reviews ci tarjam bu machine

Doomi njàngaan yu 254 languages yi jàpp ci tarjam bu machine. Reviews yu jëm ci doom yu jëm nañu!

1. Jàpp file bu language sa
2. Jàppale tarjam yi
3. Jàppale kàddu yu amul walla bu amul
4. Jàpp PR

### Codes bu Language

Nuy jëfandikoo codes ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ak regional variants bu am (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Njàngaan ci Development

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Njàngaan
```bash
```
### Build
```bash
```
> Nota: Heap bu jëmm 2GB amul ci 254 language files + Monaco editor bundle (~38MB renderer).

### Structure bu Project
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

## 🙏 Jërëjëf

Benn jëfandikoo am na WIA SOOM bu baax ci jàngalekat yi ak jàngalekat yu am ci atum.

Su nu jàppale ci jàmm, jàppale ci xel, jëfandikoo plugin, walla jàppale ci benn feex bu mag — **ñu ngi ci biir sa jàmm.**

---

<p align="center"><em>Jëfandikoo ak ❤️ ci SmileStory Inc. ak jëfandikoo yu am ci atum.</em></p>