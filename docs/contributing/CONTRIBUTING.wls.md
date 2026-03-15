<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Faka'apa'apa ki WIA SOOM</h1>
<p align="center"><strong>Te uhi mai a matou i ou faka'apa'apa!</strong></p>
<p align="center">Koe te fa'ahinga o se fa'ama'i, fa'ata'ita'iga fou, plugin, po'o le fa'alelei — e taua uma le fa'ama'i.</p>

---

## Lisi o Mea

- [Tulafono o le Amio](#code-of-conduct)
- [Fa'ailoa le Fa'ama'i](#-how-to-report-bugs)
- [Fa'ailoa le Fa'ata'ita'iga](#-how-to-suggest-features)
- [Fa'ailoa le Fa'ama'i o se Plugin](#-how-to-submit-a-plugin)
- [Fa'ailoa le Fa'ama'i o se Pull Request](#-how-to-submit-a-pull-request)
- [Faka'apa'apa i le Fa'alelei (254 Gagana)](#-translation-contributions-254-languages)
- [Seti le Atina'e](#-development-setup)

---

## Tulafono o le Amio

O le matou fa'amaoni e ofoina atu se fa'avae ma se fa'avae e aofia ai mo tagata uma.

- **Ia fa'amaoni.** Fa'ata'ita'i tagata uma i le agaga o le fa'amaoni.
- **Ia fa'atekinolosi.** Fa'amaonia le fesoasoani, e le fa'amaonia le fa'atekinolosi.
- **Ia aofia.** E matou te lagolagoina 254 gagana ma fa'afeiloa'i tagata fa'ama'i mai le atunuu uma o le lalolagi.
- **E leai ni fa'ata'iga.** E leai se fa'amaoni mo le fa'avae o so'o se ituaiga.

---

## 🐛 Fa'ailoa le Fa'ama'i

1. Alu i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kiliki **"New Issue"**
3. Filifili le **"Bug Report"** template
4. Fa'aopoopo:
   - WIA SOOM version (Settings → About)
   - OS ma le version (Windows/macOS/Linux)
   - La'asaga e toe fa'aaogaina
   - Fa'amoemoe vs. fa'ata'ita'iga
   - Ata e mafai ai po'o le fa'ama'i i le terminal

---

## 💡 Fa'ailoa le Fa'ata'ita'iga

1. Alu i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kiliki **"New Issue"**
3. Filifili le **"Feature Request"** template
4. Fa'amatala:
   - O le a le fa'afaigaluega e te fo'ia
   - E fa'apena i le fa'ama'i
   - So'o se isi fa'ata'ita'iga e te manatu i ai

---

## 🔌 Fa'ailoa le Fa'ama'i o se Plugin

O le WIA SOOM e iai se faiga plugin malosi — e mafai ona e fa'avae i le 5 minute.

### Fa'avae Vave
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Taiala Tumau

Fa'aoga le **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mo:
- Fa'amaoniga API atoa
- Fa'ata'ita'iga e galue
- Taiala i la'asaga
- Fa'avae lelei ma tulafono o le saogalemu

### Fa'ama'i Lau Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Fa'aopoopo lau plugin i `plugins/{your-plugin-name}/`
3. Fa'ama'i se Pull Request
4. A'o le'i fa'amaonia, o lau plugin e fa'aalia i le Plugin Store mo tagata uma!

---

## 🔀 Fa'ailoa le Fa'ama'i o se Pull Request

### Mo le fa'avae autu (wia-soom)

1. Fork le repository
2. Fa'avae se la'au fa'ata'ita'iga: `git checkout -b feat/my-feature`
3. Fa'ama'i au suiga
4. Fa'amaonia i le lotoifale:
   ```bash
   ```
5. Fa'ama'i ma se fa'amaonia manino:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ma fa'ailoa se PR i le `main`

### Fa'amaonia o le Fa'ama'i

| Prefix | Fa'aaoga mo |
|--------|---------|
| `feat:` | Fa'ata'ita'iga fou |
| `fix:` | Fa'ama'i fa'ama'i |
| `docs:` | Fa'amatalaga na'o le |
| `refactor:` | Fa'avae o le code (e leai se suiga i le fa'ata'ita'iga) |
| `i18n:` | Fa'amaonia o le fa'alelei |
| `plugin:` | Suiga e fa'atatau i le plugin |

### PR Checklist

- [ ] E galue le code e aunoa ma ni fa'ama'i
- [ ] E leai ni fa'ama'i i totonu (fa'aaoga i18n keys)
- [ ] E leai ni `console.log` i le code o le gaosiga
- [ ] E fa'amaonia le suiga i le fa'ama'i o le fa'avae

---

## 🌐 Faka'apa'apa i le Fa'alelei (254 Gagana)

O le WIA SOOM e lagolagoina **254 gagana** — mai le Amharic i le Zulu, e aofia ai le Braille ma gagana RTL.

### Fa'agaioiga o le Fa'alelei

- Fa'avae o le gagana: `src/renderer/src/i18n/en.json`
- O gagana uma e 254 o lo'o i le fa'avae tutusa
- O le fa'alelei e faia i le `scripts/translate-patch.js` (GPT-4o-mini API)

### Fa'ailoa le Faka'apa'apa i le Fa'alelei

#### Fa'ata'ita'iga 1: Fa'ama'i se fa'alelei fa'atekinolosi

1. Su'e le fa'avae o le gagana: `src/renderer/src/i18n/{lang-code}.json`
2. Fa'ama'i le fa'alelei sese
3. Fa'ama'i se PR ma le suiga

#### Fa'ata'ita'iga 2: Fa'aopoopo le ki e le o'o
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Fa'ata'ita'iga 3: Fa'amaonia le fa'alelei masini

E tele o le 254 gagana o lo'o fa'alelei i masini. O fa'amaonia a tagata e masani e taua tele!

1. Filifili lau fa'avae o le gagana
2. Fa'amaonia le fa'alelei
3. Fa'ama'i so'o se fa'alelei leaga po'o le sese
4. Fa'ama'i se PR

### Fa'ailoga o Gagana

E fa'aaoga e matou fa'ailoga masani ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ma fa'avae i le vaega e manaʻomia (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Seti le Atina'e

### Fa'avae

- Node.js 18+
- npm 9+
- Git

### Seti
```bash
```
### Fa'avae
```bash
```
> Fa'amolemole: O le 2GB heap masani e le o le aoga i le 254 gagana + Monaco editor bundle (~38MB renderer).

### Fa'avae o le Poloketi
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

Ko e fakaʻilonga kotoa e fai ai ke toe lelei ai te WIA SOOM ki he ngaahi fakafofonga ʻi he lalolagi.

Koeʻuhi ko e toe fakalelei e toe fakaʻilonga, fakaʻilonga e konga, hanga e plugin, pe fakaʻata e ngaahi ngaahi meʻangaue — **koe ʻoku ke kotoa i he tala ko ʻeni.**

---

<p align="center"><em>Fakaʻata mai ❤️ e SmileStory Inc. mo e ngaahi fakafofonga ʻi he lalolagi.</em></p>