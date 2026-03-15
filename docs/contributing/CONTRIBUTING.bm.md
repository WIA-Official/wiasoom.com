<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Sikuru WIA SOOM</h1>
<p align="center"><strong>Yerew ka a fɔlɔ!</strong></p>
<p align="center">N'i bɛ bug fix, fɛɛrɛ kelen, plugin, an ka fɔlɔ — kɛlɛ fɔlɔ bɛ yɛrɛ.</p>

---

## Kɛnɛya Kɔrɔ

- [Kɔdɔ Kɛlɛ](#code-of-conduct)
- [Bugs bɛ bɔ](#-how-to-report-bugs)
- [Fɛɛrɛ kelen bɛ bɔ](#-how-to-suggest-features)
- [Plugin bɛ bɔ](#-how-to-submit-a-plugin)
- [Pull Request bɛ bɔ](#-how-to-submit-a-pull-request)
- [Fɔlɔ bɛ bɔ (254 Kelen)](#-translation-contributions-254-languages)
- [Nɔgɔya Kɛlɛ](#-development-setup)

---

## Kɔdɔ Kɛlɛ

N'ka fɔlɔ ka bɛ dɔɔnin kɛlɛ ni fɔlɔ kɛlɛ.

- **Bɛ fɔlɔ.** Kɛlɛ bɛ fɔlɔ kɛlɛ.
- **Bɛ kɛlɛ.** Bɛ bɔ fɔlɔ ka bɛ dɔɔnin, a bɛ fɔlɔ kɛlɛ.
- **Bɛ kɛlɛ.** N'ka fɔlɔ 254 kelen ni n'ka bɔ fɔlɔ kɛlɛ kɛlɛ.
- **Ala bɛ fɔlɔ.** N'i bɛ fɔlɔ kɛlɛ ka bɛ dɔɔnin.

---

## 🐛 Bugs bɛ bɔ

1. Kɔ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kɔ **"New Issue"**
3. Sɛbɛ **"Bug Report"** template
4. Kɔ:
   - WIA SOOM version (Nɔgɔya → Kɛnɛya)
   - OS ni version (Windows/macOS/Linux)
   - Kɛnɛya ka bɔ
   - Kɛnɛya bɛ kɛlɛ ni kɛnɛya bɛ yɛrɛ
   - Screenshots an terminal output sisan

---

## 💡 Fɛɛrɛ kelen bɛ bɔ

1. Kɔ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kɔ **"New Issue"**
3. Sɛbɛ **"Feature Request"** template
4. Kɔ:
   - Kɛnɛya bɛ kɛlɛ
   - N'i bɛ fɔlɔ kɛlɛ
   - Kɛnɛya bɛ fɔlɔ kɛlɛ

---

## 🔌 Plugin bɛ bɔ

WIA SOOM bɛ fɔlɔ plugin system — i bɛ fɔlɔ plugin kan sisan 5 minit.

### Kɛnɛya Kɛlɛ
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kɛnɛya Kɛlɛ Kɛnɛya

Bɔ **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ka:
- API reference kɛlɛ
- Kɛnɛya bɛ yɛrɛ
- Kɛnɛya bɛ kɛnɛya
- Kɛlɛ bɛ fɔlɔ ni bɔlɔ kɛlɛ

### I bɛ fɔlɔ Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Fɔlɔ plugin kan `plugins/{your-plugin-name}/`
3. Bɔ Pull Request
4. Kɔ fɔlɔ, plugin kan bɛ yɛrɛ Plugin Store ka bɛ fɔlɔ!

---

## 🔀 Pull Request bɛ bɔ

### Kɛnɛya app (wia-soom)

1. Fork repository
2. Fɔlɔ feature branch: `git checkout -b feat/my-feature`
3. Fɔlɔ kɛnɛya
4. Test sisan:
   ```bash
   ```
5. Commit ka kɛnɛya:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push an bɔ PR ka `main`

### Commit Kɛnɛya Kɔrɔ

| Prefix | Kɛlɛ bɛ fɔlɔ |
|--------|---------|
| `feat:` | Fɛɛrɛ kelen |
| `fix:` | Bug fix |
| `docs:` | Documentation kɛlɛ |
| `refactor:` | Kɔdɔ kɛlɛ (a bɛ fɔlɔ kɛlɛ) |
| `i18n:` | Fɔlɔ bɛ fɔlɔ |
| `plugin:` | Plugin bɛ fɔlɔ |

### PR Checklist

- [ ] Kɔdɔ bɛ yɛrɛ a bɛ fɔlɔ
- [ ] Ala hardcoded strings (i bɛ fɔlɔ i18n keys)
- [ ] Ala `console.log` bɛ fɔlɔ production code
- [ ] Kɛnɛya bɛ yɛrɛ

---

## 🌐 Fɔlɔ bɛ bɔ (254 Kelen)

WIA SOOM bɛ fɔlɔ **254 kelen** — Amharic kɔ Zulu, Braille ni RTL kelen kɛlɛ.

### Fɔlɔ bɛ kɛlɛ

- Base language file: `src/renderer/src/i18n/en.json`
- Kelen 254 bɛ fɔlɔ ka kɛlɛ
- Fɔlɔ bɛ kɛlɛ sisan `scripts/translate-patch.js` (GPT-4o-mini API)

### I bɛ fɔlɔ Fɔlɔ

#### Kɛnɛya 1: Fɔlɔ kelen kɛlɛ

1. Fɔlɔ kelen file: `src/renderer/src/i18n/{lang-code}.json`
2. Fɔlɔ kelen kɛlɛ
3. Bɔ PR ka kɛnɛya

#### Kɛnɛya 2: Fɔlɔ kelen bɛ bɔ
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kɛnɛya 3: Fɔlɔ machine translations

Kelen 254 bɛ machine-translated. Native speaker reviews bɛ fɔlɔ kɛlɛ!

1. Kɔ kelen file
2. Fɔlɔ translations
3. Fɔlɔ kelen kɛlɛ an kelen kɛlɛ
4. Bɔ PR

### Kelen Kɔdɔ

N'ka fɔlɔ standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ka regional variants sisan (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Nɔgɔya Kɛlɛ

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Kɛnɛya
```bash
```
### Build
```bash
```
> Note: Default 2GB heap bɛ a bɛ fɔlɔ 254 kelen files + Monaco editor bundle (~38MB renderer).

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

## 🙏 I ni ce

Kɔrɔkɔrɔ ye WIA SOOM bɛɛ fɔlɔ kɛlɛ ye kɛlɛbaw ka fɔlɔ ye.

I bɛ tɔgɔ fɔ, i bɛ fɔ a string, i bɛ bɔ plugin, an ka i bɛ sɔrɔ kɛlɛ fɔlɔ — **i ye n'i kɔrɔ.**

---

<p align="center"><em>❤️ ka SmileStory Inc. ni kɔrɔbaw bɛ fɔlɔ.</em></p>