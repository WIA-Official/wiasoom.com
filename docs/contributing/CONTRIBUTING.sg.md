<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kondé WIA SOOM</h1>
<p align="center"><strong>O yé na nganga na yo!</strong></p>
<p align="center">Soki ezali likambo ya bug, fonctionnalité ya sika, plugin, to traduction — nyonso ezali na ntina.</p>

---

## Tabela ya Ndakisa

- [Code ya Comportement](#code-of-conduct)
- [Ndenge ya Koyebisa Bux](#-how-to-report-bugs)
- [Ndenge ya Koyebisa Fonctionnalités](#-how-to-suggest-features)
- [Ndenge ya Koyebisa Plugin](#-how-to-submit-a-plugin)
- [Ndenge ya Koyebisa Pull Request](#-how-to-submit-a-pull-request)
- [Kondé Traduction (254 Langues)](#-translation-contributions-254-languages)
- [Mokili ya Développement](#-development-setup)

---

## Code ya Comportement

Tozali na esengo ya kopesa expérience ya malamu mpe ya kolanda nyonso.

- **Sunga respect.** Landa nyonso na esengo.
- **Sunga constructive.** Pesa feedback ya mabe, te critique ya mabe.
- **Sunga inclusive.** Tozali na soutien ya 254 langues mpe tozali na esengo ya koyamba ba contributeurs nyonso ya mboka nyonso ya mokili.
- **Te harassment.** Tolanda zéro tolérance na discrimination ya ndenge nyonso.

---

## 🐛 Ndenge ya Koyebisa Bux

1. Yaka na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kóndá **"New Issue"**
3. Séléctionne **"Bug Report"** template
4. Kanga:
   - WIA SOOM version (Settings → About)
   - OS mpe version (Windows/macOS/Linux)
   - Bato ya kopesa
   - Kombo ya sika vs. comportement ya solo
   - Screenshots to terminal output soki ezali na possibilité

---

## 💡 Ndenge ya Koyebisa Fonctionnalités

1. Yaka na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kóndá **"New Issue"**
3. Séléctionne **"Feature Request"** template
4. Kanga:
   - Likambo oyo ozali kokanga
   - Ndenge ozali komona yango ekozala
   - Ba alternatives oyo olingi

---

## 🔌 Ndenge ya Koyebisa Plugin

WIA SOOM ezali na système ya plugin ya mabe — okoki kosala plugin na yo na miniti 5.

### Koti ya Mbala
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guide ya Pona

Landa **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mpo na:
- Referens ya API ya kokamwa
- Ba exemples ya mosala
- Ba tutoriels ya étape par étape
- Ba pratiques ya malamu mpe ba règles ya sécurité

### Koyebisa Plugin na Yo

1. Fork [Plugin Store](https://wiasoom.com)
2. Kanga plugin na yo na `plugins/{your-plugin-name}/`
3. Koyebisa Pull Request
4. Na nsima ya kukanga, plugin na yo ekokoma na Plugin Store mpo na ba utilisateurs nyonso!

---

## 🔀 Ndenge ya Koyebisa Pull Request

### Mpo na application principale (wia-soom)

1. Fork repository
2. Salá branche ya fonctionnalité: `git checkout -b feat/my-feature`
3. Salá ba changements na yo
4. Teste na ndakisa:
   ```bash
   ```
5. Commit na message ya malamu:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push mpe open PR na `main`

### Convention ya Message ya Commit

| Prefix | Koyeba mpo na |
|--------|---------------|
| `feat:` | Fonctionnalité sika |
| `fix:` | Bug fix |
| `docs:` | Documentation kaka |
| `refactor:` | Restructuration ya code (te changement ya comportement) |
| `i18n:` | Ba mise à jour ya traduction |
| `plugin:` | Ba changements liés na plugin |

### Checklist ya PR

- [ ] Code ebandi na mabe
- [ ] Te ba chaînes ya hardcoded (sunga i18n keys)
- [ ] Te `console.log` eza na code ya production
- [ ] Ba tests oyo ezali kuna ezali kokoma

---

## 🌐 Kondé Traduction (254 Langues)

WIA SOOM ezali na soutien ya **254 langues** — na Amharic tii na Zulu, na kati ya Braille mpe ba langues ya RTL.

### Ndenge Traduction Emasaka

- Lingala ya base: `src/renderer/src/i18n/en.json`
- Ba fichiers ya 254 langues nyonso ezali na esika moko
- Traduction esalema na `scripts/translate-patch.js` (GPT-4o-mini API)

### Ndenge ya Koyebisa Traductions

#### Option 1: Kanga traduction moko

1. Zwa fichier ya langue: `src/renderer/src/i18n/{lang-code}.json`
2. Kanga traduction oyo ezali mabe
3. Koyebisa PR na changement

#### Option 2: Kanga ba clés oyo ezali mpona
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Koyeba ba traductions ya machine

Bokoki na ba 254 langues na biso ezalaka na ba traductions ya machine. Ba reviews ya ba locuteurs natifs ezali na ntina mingi!

1. Zwa fichier ya langue na yo
2. Koyeba ba traductions
3. Kanga ba traductions oyo ezali na mabe to oyo ezali mabe
4. Koyebisa PR

### Ba Codes ya Langue

Tosalela ba codes ya ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) na ba variantes régionales soki ezali na ntina (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Mokili ya Développement

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
> Note: Heap ya 2GB ya default ezali te ebele mpo na ba fichiers ya 254 langues + Monaco editor bundle (~38MB renderer).

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

## 🙏 Mbotu na yo

Bongani na yo nyonso eko WIA SOOM malamu mpo na ba développeurs na mokili mobimba.

Soki otya motuna, otranslate motuna, omonisa plugin, to oongeza fonction ya ntina — **oyebi na lisanga ya histoire oyo.**

---

<p align="center"><em>Ezali na ❤️ na SmileStory Inc. mpe ba contributeurs na mokili mobimba.</em></p>