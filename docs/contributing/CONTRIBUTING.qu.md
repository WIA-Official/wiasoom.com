<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-ta Chaylla</h1>
<p align="center"><strong>Yukuykiyki chaylla!</strong></p>
<p align="center">Kawsaykuyki chaylla, huk bug chaylla, nuyku chaylla, o traducción — cada chaylla importante kanki.</p>

---

## Chaylla Qhichwa

- [Conducta de Código](#code-of-conduct)
- [Imanata Bug-nin Qhawasqa](#-how-to-report-bugs)
- [Imanata Nuyku-nin Qhawasqa](#-how-to-suggest-features)
- [Imanata Plugin-nin Qhawasqa](#-how-to-submit-a-plugin)
- [Imanata Pull Request-nin Qhawasqa](#-how-to-submit-a-pull-request)
- [Traducción Chaylla (254 Simikuna)](#-translation-contributions-254-languages)
- [Desarrollo Qhichwa](#-development-setup)

---

## Conducta de Código

Ñukaka chaylla kawsaykuyki chaylla, allinmi chaylla, chaylla kawsaykuyki.

- **Respeto kachkani.** Kawsaykuyki chaylla, kawsaykuyki.
- **Constructivo kachkani.** Chaylla allin chaylla, mana chaylla chaylla.
- **Inclusivo kachkani.** Ñukaka 254 simikuna apoyanichu, chaylla kawsaykuyki kawsaykuyki.
- **Mana chaylla kachkani.** Mana chaylla kachkani chaylla kawsaykuyki.

---

## 🐛 Imanta Bug-nin Qhawasqa

1. Rikhuy [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chaylla **"New Issue"**-ta chaylla
3. Chaylla **"Bug Report"**-ta chaylla
4. Rikhuy:
   - WIA SOOM version (Ajustes → Ayllu)
   - OS y version (Windows/macOS/Linux)
   - Rikhuykuyki
   - Rikhuyki chaylla vs. rikhuyki
   - Screenshot o terminal output chaylla

---

## 💡 Imanta Nuyku-nin Qhawasqa

1. Rikhuy [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chaylla **"New Issue"**-ta chaylla
3. Chaylla **"Feature Request"**-ta chaylla
4. Rikhuy:
   - Imata chaylla rikhuyki
   - Imata chaylla rikhuyki
   - Imata chaylla chaylla

---

## 🔌 Imanta Plugin-nin Qhawasqa

WIA SOOM huk plugin sistema kachkani — 5 minutos nisqayki plugin chaylla.

### Chaylla Qhichwa
§§§CHUNK_SEPARATOR§§§
### Chaylla Qhichwa Kawsay

Rikhuy **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)**-ta chaylla:
- Completo API referencia
- Chaylla rikhuyki
- Pasu-pasu tutoriales
- Allin prácticas y seguridad reglas

### Plugin-nikuyki

1. Fork [Plugin Store](https://wiasoom.com)
2. Plugin-nikuyki `plugins/{your-plugin-name}/`
3. Imanta Pull Request-nikuyki
4. Rikhuykuyki, plugin-nikuyki Plugin Store-ta chaylla kawsaykuyki!

---

## 🔀 Imanta Pull Request-nin Qhawasqa

### WIA-SOOM-ta (wia-soom)

1. Fork chaylla repository
2. Huk nuyku branch qhawasqa: `git checkout -b feat/my-feature`
3. Chaylla rikhuyki
4. Localmente rikhuyki:
   ```bash
   ```
5. Chaylla allin mensaje-nikuyki:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push chaylla, PR open chaylla `main`-ta

### Commit Mensaje Convención

| Prefijo | Usar para |
|--------|---------|
| `feat:` | Huk nuyku |
| `fix:` | Bug chaylla |
| `docs:` | Documentación chaylla |
| `refactor:` | Código restructuring (mana rikhuyki chaylla) |
| `i18n:` | Traducción updates |
| `plugin:` | Plugin-ñikuyki chaylla |

### PR Checklist

- [ ] Código rikhuyki mana errores
- [ ] Mana hardcoded strings (i18n keys-llata rikhuyki)
- [ ] Mana `console.log` chaylla producción código
- [ ] Chaylla rikhuyki chaylla

---

## 🌐 Traducción Chaylla (254 Simikuna)

WIA SOOM **254 simikuna** apoyanichu — Amharic-man Zulu-man, Braille y RTL simikuna chaylla.

### Imanta Traducción Kawsay

- Base simi file: `src/renderer/src/i18n/en.json`
- Huk 254 simikuna files chaylla chaylla
- Traducción chaylla `scripts/translate-patch.js` (GPT-4o-mini API)

### Imanta Traducción Chaylla

#### Opción 1: Huk chaylla traducción chaylla

1. Rikhuy simi file: `src/renderer/src/i18n/{lang-code}.json`
2. Chaylla chaylla traducción chaylla
3. Imanta PR-nikuyki

#### Opción 2: Chaylla chaylla keys
§§§CHUNK_SEPARATOR§§§
#### Opción 3: Rikhuy machine traducciones

Ñukaka 254 simikuna chaylla machine-translated kachkani. Ñukaka rikhuyki nativa hablantes chaylla chaylla kachkani!

1. Chaylla simi file-nikuyki
2. Rikhuy traducciones
3. Chaylla chaylla o chaylla traducciones chaylla
4. Imanta PR-nikuyki

### Simi Kódigos

Ñukaka estándar ISO 639-1 kódigos (e.g., `ko`, `en`, `ja`, `ar`, `hi`) regional variantes chaylla (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Desarrollo Qhichwa

### Prerequisitos

- Node.js 18+
- npm 9+
- Git

### Qhichwa
§§§CHUNK_SEPARATOR§§§
### Kawsay
§§§CHUNK_SEPARATOR§§§
> Nota: Huk default 2GB heap mana chaylla chaylla huk 254 simikuna files + Monaco editor bundle (~38MB renderer).

### Proyecto Estructura
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Yuspay

Tukuy yachaykuykuna WIA SOOM nisqayki, yachayniykichikta allinmi kawsaykuykuna. 

Ima chayllapaqa, yachaykuyki, plugin nisqayki, o chayllapaqa huk ñawpaq rikhuykuyki — **kikinka chay rimanakuykitaqa.**

---

<p align="center"><em>❤️ chayllapaqa SmileStory Inc. nisqaykuna, yachaykuykuna allin kawsaykuykuna.</em></p>
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

## 🙏 Thank You

Every contribution makes WIA SOOM better for developers around the world.

Whether you fix a typo, translate a string, build a plugin, or add a major feature — **you are part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>
