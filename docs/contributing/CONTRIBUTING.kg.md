<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kuzala na WIA SOOM</h1>
<p align="center"><strong>Tulenda ku zala na yo!</strong></p>
<p align="center">Soki ezali likambo ya kosala, feature ya sika, plugin, to traduction — nyonso ezali na ntina.</p>

---

## Table ya Mibeko

- [Mibeko ya Conduct](#code-of-conduct)
- [Ndenge ya Koyebisa Biyoko](#-how-to-report-bugs)
- [Ndenge ya Koyebisa Features](#-how-to-suggest-features)
- [Ndenge ya Koyebisa Plugin](#-how-to-submit-a-plugin)
- [Ndenge ya Koyebisa Pull Request](#-how-to-submit-a-pull-request)
- [Kuzala na Traductions (254 Langues)](#-translation-contributions-254-languages)
- [Mokano ya Development](#-development-setup)

---

## Mibeko ya Conduct

Tozali na esengo ya kopesa esengo mpe experience ya kolanda nyonso.

- **Kokamwa.** Tika nyonso na esengo.
- **Kokamwa na ndenge ya malamu.** Pesa feedback ya malamu, te critique ya mabe.
- **Kokamwa.** Tozali na lisungi ya 254 langues mpe tozali na esengo ya kozala na ba contributeurs nyonso ya mokili.
- **Te na harassment.** Zero tolerance na discrimination ya ndenge nyonso.

---

## 🐛 Ndenge ya Koyebisa Biyoko

1. Yaka na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Koma **"New Issue"**
3. Kanga **"Bug Report"** template
4. Kanga:
   - WIA SOOM version (Settings → About)
   - OS mpe version (Windows/macOS/Linux)
   - Bato ya koyeba
   - Kombo ya sika vs. pona
   - Screenshots to terminal output soki ezali na possibilité

---

## 💡 Ndenge ya Koyebisa Features

1. Yaka na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Koma **"New Issue"**
3. Kanga **"Feature Request"** template
4. Kanga:
   - Likambo oyo ozali kosala
   - Ndenge ozali komona yango
   - Ba alternatives oyo olingi

---

## 🔌 Ndenge ya Koyebisa Plugin

WIA SOOM ezali na système ya plugin ya makasi — okoki kosala plugin na yo na miniti 5.

### Ndenge ya Koyebisa
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guide ya Malamu

Landa **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mpo na:
- Referens ya API ya complete
- Ba exemples ya mosala
- Ba tutorials ya étape par étape
- Ba pratiques ya malamu mpe mibeko ya sécurité

### Koyebisa Plugin na Yo

1. Fork [Plugin Store](https://wiasoom.com)
2. Kanga plugin na yo na `plugins/{your-plugin-name}/`
3. Koyebisa Pull Request
4. Na nsima ya review, plugin na yo ekokisi na Plugin Store mpo na ba utilisateurs nyonso!

---

## 🔀 Ndenge ya Koyebisa Pull Request

### Mpo na application principale (wia-soom)

1. Fork repository
2. Salela branch ya feature: `git checkout -b feat/my-feature`
3. Salela ba changements na yo
4. Teste na ndako:
   ```bash
   ```
5. Koma na message ya malamu:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push mpe buka PR na `main`

### Mibeko ya Komit

| Prefix | Koyebisa mpo na |
|--------|-----------------|
| `feat:` | Feature ya sika |
| `fix:` | Bug fix |
| `docs:` | Documentation kaka |
| `refactor:` | Réorganisation ya code (te changement ya comportement) |
| `i18n:` | Mise à jour ya traduction |
| `plugin:` | Ba changements ya plugin |

### PR Checklist

- [ ] Code ezali kosala na mabe
- [ ] Te na ba strings ya hardcoded (salela i18n keys)
- [ ] Te na `console.log` oyo ezalaka na production code
- [ ] Ba tests oyo ezalaka na libanda ezali kokoma

---

## 🌐 Kuzala na Traductions (254 Langues)

WIA SOOM ezali na lisungi ya **254 langues** — banda na Amharic ti na Zulu, na kati ya Braille mpe ba langues ya RTL.

### Ndenge Traduction Ekozala

- Base language file: `src/renderer/src/i18n/en.json`
- Ba fichiers ya 254 langues nyonso ezali na esika moko
- Traduction ekozala na `scripts/translate-patch.js` (GPT-4o-mini API)

### Ndenge ya Kuzala na Traductions

#### Option 1: Koyebisa traduction moko

1. Zwa fichier ya langue: `src/renderer/src/i18n/{lang-code}.json`
2. Koyebisa traduction oyo ezali mabe
3. Koyebisa PR na changement

#### Option 2: Koyebisa ba clés oyo ezali te
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Koyeba ba traductions ya machine

Boko ya 254 langues na biso ezalaka na ba traductions ya machine. Ba reviews ya ba locuteurs natifs ezali na ntina mingi!

1. Zwa fichier ya langue na yo
2. Koyeba ba traductions
3. Koyebisa ba traductions oyo ezali mabe to oyo ezali na mabe
4. Koyebisa PR

### Ba Codes ya Langue

Tozali kosalela ba codes ya ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) na ba variantes régionales soki ezali na besoin (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Mokano ya Development

### Ba Prérequis

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Note: Heap ya 2GB ya default ezali te tozali na ba fichiers ya 254 langues + bundle ya Monaco editor (~38MB renderer).

### Structure ya Projet
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

## 🙏 Melesi

Biso nyonso ya kotalisa ebandaka WIA SOOM na malamu mpo na ba développeurs na mokili mobimba.

Soki otyaka motuna, otranslate moko, omonisa plugin, to oongeza fonction ya ntina — **ozali na kati ya lisolo oyo.**

---

<p align="center"><em>Ekangami na ❤️ na SmileStory Inc. mpe ba contributeurs na mokili mobimba.</em></p>