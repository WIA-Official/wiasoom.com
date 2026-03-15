<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Fakaʻilonga ki he WIA SOOM</h1>
<p align="center"><strong>Te ke fiafia mo e ngaahi fakaʻilonga!</strong></p>
<p align="center">Ko e meʻa pe ko e fakaʻilonga, ngaahi meʻa foʻou, plugin, pe fakaʻilonga — ko e fakaʻilonga kotoa pē e ngaahi meʻa ko ia.</p>

---

## Tohi ʻo e Konga

- [Kau o e Fakaʻilonga](#code-of-conduct)
- [Fakaʻilonga ki he ngaahi Fakaʻilonga](#-how-to-report-bugs)
- [Fakaʻilonga ki he ngaahi Meʻa Foʻou](#-how-to-suggest-features)
- [Fakaʻilonga ki he Plugin](#-how-to-submit-a-plugin)
- [Fakaʻilonga ki he Pull Request](#-how-to-submit-a-pull-request)
- [Fakaʻilonga ki he ngaahi Fakaʻilonga (254 Languages)](#-translation-contributions-254-languages)
- [Fakaʻilonga ki he Development Setup](#-development-setup)

---

## Kau o e Fakaʻilonga

Kuo tau fakakaukau ki he tuʻuina atu ʻo e ngaahi meʻa fakaʻilonga lelei mo e ngaahi meʻa fakaʻilonga.

- **Fakaʻapaʻapa.** Tuku ke fakaʻapaʻapa ki he tangata kotoa.
- **Fakaʻilonga lelei.** Fakaʻilonga mai e ngaahi manatu lelei, ʻa e ngaahi fakaʻilonga kovi.
- **Fakaʻilonga kotoa.** Tautoko ʻo e 254 ngaahi lea mo e fakaʻilonga ki he ngaahi tagata mai he ngaahi fonua kotoa ʻo e lalolagi.
- **Tuku ke lele.** Zero tolerance ki he ngaahi fakaʻilonga kovi.

---

## 🐛 Fakaʻilonga ki he ngaahi Fakaʻilonga

1. Kuo ki he [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikia **"New Issue"**
3. Fai e **"Bug Report"** template
4. Kuo fakaʻilonga:
   - WIA SOOM version (Settings → About)
   - OS mo e version (Windows/macOS/Linux)
   - Ngaahi laumālie ki he toe toe
   - Ngaahi meʻa e fakakaukau vs. e meʻa tonu
   - Ngaahi ata pe terminal output kehekehe

---

## 💡 Fakaʻilonga ki he ngaahi Meʻa Foʻou

1. Kuo ki he [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikia **"New Issue"**
3. Fai e **"Feature Request"** template
4. Fakaʻilonga:
   - Ko e meʻa e ke fakaʻilonga
   - Pehea e ke fakakaukau ai
   - Ngaahi meʻa kehekehe e ke fakaʻilonga

---

## 🔌 Fakaʻilonga ki he Plugin

Kuo iai e WIA SOOM e ngaahi plugin malosi — e mafai ke ke fakaʻilonga e plugin ko e 5 miniti.

### Fakaʻata
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tohi Katoa

Fakakau e **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ki he:
- API reference kotoa
- Ngaahi meʻa e fai
- Ngaahi tohi fakakaukau
- Ngaahi ngaahi meʻa lelei mo e ngaahi tulafono fakapisinisi

### Fakaʻilonga e Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Tuku e plugin ki `plugins/{your-plugin-name}/`
3. Fakaʻilonga e Pull Request
4. Ka toe fakamālohi, e hoko e plugin ki he Plugin Store ki he ngaahi tagata kotoa!

---

## 🔀 Fakaʻilonga ki he Pull Request

### Ki he app matua (wia-soom)

1. Fork e repository
2. Fakaʻata e feature branch: `git checkout -b feat/my-feature`
3. Fai e ngaahi meʻa kehekehe
4. Fakaʻilonga ki he fakamālohi:
   ```bash
   ```
5. Fakaʻilonga mo e fakaʻilonga e fakamatala:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push mo e fakaʻilonga e PR ki he `main`

### Fakaʻilonga e Fakamatala

| Prefix | Fakaʻilonga ki he |
|--------|-------------------|
| `feat:` | Meʻa foʻou |
| `fix:` | Fakaʻilonga kovi |
| `docs:` | Fakaʻilonga e ngaahi tohi |
| `refactor:` | Fakaʻilonga e koloa (ʻa e meʻa e toe toe) |
| `i18n:` | Fakaʻilonga e ngaahi fakaʻilonga |
| `plugin:` | Ngaahi meʻa e plugin |

### PR Checklist

- [ ] E nofo e code ʻa e ngaahi fakaʻilonga
- [ ] ʻIkai e ngaahi string ko e kovi (fai e i18n keys)
- [ ] ʻIkai e `console.log` e nofo ki he code production
- [ ] E toe fakamālohi e ngaahi tohi

---

## 🌐 Fakaʻilonga ki he ngaahi Fakaʻilonga (254 Languages)

Kuo tautoko e WIA SOOM e **254 ngaahi lea** — mai he Amharic ki he Zulu, e aofia ai e Braille mo e ngaahi lea RTL.

### Pehea e Fakaʻilonga ai

- Fakaʻilonga lea tuʻukimu: `src/renderer/src/i18n/en.json`
- Kotoa e 254 ngaahi lea ko e nofo ki he konga kotoa
- E fai e fakaʻilonga ki he `scripts/translate-patch.js` (GPT-4o-mini API)

### Pehea e Fakaʻilonga e ngaahi Fakaʻilonga

#### Fakaʻilonga 1: Fakaʻilonga e fakaʻilonga ko e

1. Fakaʻilonga e lea file: `src/renderer/src/i18n/{lang-code}.json`
2. Fakaʻilonga e fakaʻilonga ko e
3. Fakaʻilonga e PR mo e ngaahi meʻa

#### Fakaʻilonga 2: Tuku e ngaahi key ko e
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Fakaʻilonga 3: Fakaʻilonga e ngaahi fakaʻilonga masini

Kuo iai e 254 ngaahi lea ko e e fakaʻilonga masini. E lelei e ngaahi toe fakamālohi ʻo e ngaahi tagata ʻi he lea!

1. Fakaʻilonga e lea file
2. Fakaʻilonga e ngaahi fakaʻilonga
3. Fakaʻilonga e ngaahi fakaʻilonga ko e kovi
4. Fakaʻilonga e PR

### Ngaahi Koodu o e Lea

Kuo tau fakaʻilonga e ngaahi koodu ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) mo e ngaahi variante regional kehekehe (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Fakaʻilonga ki he Development Setup

### Ngaahi Meʻa Fakaʻilonga

- Node.js 18+
- npm 9+
- Git

### Fakaʻilonga
```bash
```
### Fakaʻilonga
```bash
```
> Fakaʻilonga: Ko e 2GB heap ko e maʻu ʻikai ke lava ke toe fakamālohi e 254 lea files + Monaco editor bundle (~38MB renderer).

### Koloa ʻo e Project
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

## 🙏 Mālo ni

Ko e ngaahi faingamālie kotoa pē e fakakoloa ai a WIA SOOM ki he kau devlopā ʻo e lalolagi.

Koeʻuhi ko e ke toe fakaʻosi e toe, fakaʻilonga e string, hanga e plugin, pe fakaʻata e ngaahi meʻa mahuʻinga — **ko e ke part of this story.**

---

<p align="center"><em>Hanga mo e ❤️ mai he SmileStory Inc. mo e kau faingamālie ʻo e lalolagi.</em></p>