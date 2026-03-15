<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">E fesoasoani i le WIA SOOM</h1>
<p align="center"><strong>Matou te fiafia i au fesoasoani!</strong></p>
<p align="center">E whether o se fa'asa'o, fa'avae fou, plugin, po'o se fa'atekinolosi — e taua uma le fesoasoani.</p>

---

## Taaloga o Mea

- [Tulaga o le Fa'avae](#code-of-conduct)
- [Fa'ailoa le Fa'aletonu](#-how-to-report-bugs)
- [Fa'ailoa Fa'avae](#-how-to-suggest-features)
- [Fa'ailoa se Plugin](#-how-to-submit-a-plugin)
- [Fa'ailoa se Pull Request](#-how-to-submit-a-pull-request)
- [Fa'ailoa Fa'atekinolosi (254 Gagana)](#-translation-contributions-254-languages)
- [Fa'avae le Atina'e](#-development-setup)

---

## Tulaga o le Fa'avae

O le matou fa'amaonia e ofoina atu se avanoa malamalama ma aofia ai mo tagata uma.

- **Ia fa'amaoni.** Fa'ata'ita'i tagata uma i le agaga o le fa'amaoni.
- **Ia fa'avae.** Fa'amolemole ofoina mai ni fa'amatalaga fesoasoani, e le o ni fa'amaonia fa'aleaga.
- **Ia aofia.** E lagolagoina e matou le 254 gagana ma talia tagata e fesoasoani mai i le atunuu uma i le lalolagi.
- **E leai se fa'aleaga.** E leai se fa'amaonia mo le fa'aleaga o so'o se ituaiga.

---

## 🐛 Fa'ailoa le Fa'aletonu

1. O i le [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kiliki **"New Issue"**
3. Filifili le **"Bug Report"** template
4. Fa'aopoopo:
   - WIA SOOM version (Settings → About)
   - OS ma le version (Windows/macOS/Linux)
   - Laasaga e toe fa'aaogaina
   - Fa'amoemoe vs. fa'ata'ita'iga
   - Ata e fa'atauina atu po'o le fa'amaoniga i le terminal pe a mafai

---

## 💡 Fa'ailoa Fa'avae

1. O i le [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kiliki **"New Issue"**
3. Filifili le **"Feature Request"** template
4. Fa'amatala:
   - O le a le fa'afitauli e te fo'ia
   - E fa'apena ona e fa'amoemoe e galue
   - So'o se isi filifiliga e te manatu i ai

---

## 🔌 Fa'ailoa se Plugin

O le WIA SOOM e iai se faiga plugin malosi — e mafai ona e fa'avae lau plugin i le 5 minute.

### Amata Vave
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ta'iala Tumau

Fa'amolemole faitau le **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mo:
- Fa'amatalaga API atoa
- Fa'ata'ita'iga galue
- Ta'iala i laasaga
- Fa'avae lelei ma tulafono saogalemu

### Fa'ailoa Lau Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Fa'aopoopo lau plugin i le `plugins/{your-plugin-name}/`
3. Fa'ailoa se Pull Request
4. A'o le'i fa'amaonia, o le a fa'aalia lau plugin i le Plugin Store mo tagata uma!

---

## 🔀 Fa'ailoa se Pull Request

### Mo le fa'avae autu (wia-soom)

1. Fork le fa'avae
2. Fa'avae se laupapa fa'avae: `git checkout -b feat/my-feature`
3. Fa'amaonia au suiga
4. Fa'ata'ita'iga i le lotoifale:
   ```bash
   ```
5. Fa'amaonia ma se fa'amatalaga manino:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ma fa'ailoa se PR i le `main`

### Fa'avae o le Fa'amatalaga

| Fa'ailoga | Fa'aaoga mo |
|-----------|-------------|
| `feat:`   | Fa'avae fou |
| `fix:`    | Fa'asa'o fa'aletonu |
| `docs:`   | Fa'amatalaga na'o le |
| `refactor:` | Fa'avae o le code (e le o se suiga i le fa'ata'ita'iga) |
| `i18n:`   | Fa'atekinolosi fa'atekinolosi |
| `plugin:` | Suiga e fa'avae i le plugin |

### PR Fa'amaonia

- [ ] O le code e galue e aunoa ma ni fa'aletonu
- [ ] E leai ni fa'ailoga e fa'avae i le code (fa'aaoga i18n keys)
- [ ] E leai ni `console.log` i le code fa'avae
- [ ] O su'esu'ega o lo'o i ai e fa'amaonia pea

---

## 🌐 Fa'atekinolosi Fa'avae (254 Gagana)

O le WIA SOOM e lagolagoina **254 gagana** — mai le Amharic i le Zulu, e aofia ai le Braille ma gagana RTL.

### E Fa'avae le Fa'atekinolosi

- Fa'avae gagana: `src/renderer/src/i18n/en.json`
- O fa'avae gagana e 254 o lo'o i le fa'avae tutusa
- E fa'atekinolosi i le `scripts/translate-patch.js` (GPT-4o-mini API)

### E Fa'avae i le Fa'atekinolosi

#### Filifiliga 1: Fa'asa'o se fa'atekinolosi

1. Fa'afa'ailoa le fa'avae gagana: `src/renderer/src/i18n/{lang-code}.json`
2. Fa'asa'o le fa'atekinolosi sese
3. Fa'ailoa se PR ma le suiga

#### Filifiliga 2: Fa'aopoopo fa'amaoniga e le o i ai
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Filifiliga 3: Fa'amaonia fa'atekinolosi masini

E tele o a matou gagana 254 na fa'atekinolosi i le masini. O le fa'amaonia a tagata e masani e taua tele!

1. Filifili lau fa'avae gagana
2. Fa'amaonia le fa'atekinolosi
3. Fa'asa'o so'o se fa'atekinolosi leaga po'o le sese
4. Fa'ailoa se PR

### Fa'ailoga Gagana

E fa'aaoga e matou fa'ailoga ISO 639-1 masani (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ma fa'avae i le itulagi e manaʻomia (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Fa'avae le Atina'e

### Fa'avae

- Node.js 18+
- npm 9+
- Git

### Fa'avae
```bash
```
### Fa'avae
```bash
```
> Fa'amolemole: O le 2GB heap masani e le o lava ona o le 254 fa'avae gagana + Monaco editor bundle (~38MB renderer).

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

## 🙏 Fakafetai

O le fa'atupega uma e fa'aleleia ai le WIA SOOM mo tagata atina'e i le lalolagi atoa.

E fa'avae i luga o le fa'asa'oina o se fa'ailoga, fa'aluaina o se fa'ata'ita'iga, fa'avae i se plugin, po'o le fa'aopoopoina o se vaega taua — **o le vaega o lenei tala.**

---

<p align="center"><em>Fa'avae i le ❤️ e le SmileStory Inc. ma tagata fa'atau i le lalolagi.</em></p>