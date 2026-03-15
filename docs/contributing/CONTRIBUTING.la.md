<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contributio ad WIA SOOM</h1>
<p align="center"><strong>Amamus tuae contributiones!</strong></p>
<p align="center">Sive sit emendatio erroris, nova functio, plugin, aut translatio — omnis contributio momenti est.</p>

---

## Index Contentorum

- [Codex Conductus](#code-of-conduct)
- [Quomodo Referre Erroribus](#-how-to-report-bugs)
- [Quomodo Suggere Functiones](#-how-to-suggest-features)
- [Quomodo Submittere Plugin](#-how-to-submit-a-plugin)
- [Quomodo Submittere Pull Request](#-how-to-submit-a-pull-request)
- [Translation Contributions (254 Linguarum)](#-translation-contributions-254-languages)
- [Configuratio Evolutionis](#-development-setup)

---

## Codex Conductus

Nos dedicati sumus ad praebendum experientiam amicae et inclusivae pro omnibus.

- **Esto reverens.** Omnes cum dignitate tractare.
- **Esto constructivus.** Offer auxilium feedback, non destructivam criticam.
- **Esto inclusivus.** Sustinemus 254 linguas et accipimus contributores ex omni terra in Terra.
- **Nulla molestia.** Nulla tolerantia pro discriminatione cuiusvis generis.

---

## 🐛 Quomodo Referre Erroribus

1. I ad [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"Nova Quaestio"**
3. Elige **"Bug Report"** exemplar
4. Includere:
   - Versio WIA SOOM (Optiones → De)
   - OS et versio (Windows/macOS/Linux)
   - Gradus ad reproducendum
   - Exspectata vs. actualis actio
   - Screenshots aut output terminalis si fieri potest

---

## 💡 Quomodo Suggere Functiones

1. I ad [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"Nova Quaestio"**
3. Elige **"Feature Request"** exemplar
4. Describere:
   - Quod problema solvis
   - Quomodo imaginari vis id operari
   - Quaevis alternativa considerata

---

## 🔌 Quomodo Submittere Plugin

WIA SOOM habet potentem systema plugin — potes construere tuum plugin in 5 minutis.

### Celeris Initium
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Dux Completa

Legere **[Ducem Plugin Developer](docs/PLUGIN_DEVELOPER_GUIDE.md)** pro:
- Completa API referentia
- Exempla operantia
- Tutoriales gradus-per-gradus
- Optimae practicae et regulas securitatis

### Submittere Tuum Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Adde tuum plugin ad `plugins/{your-plugin-name}/`
3. Submittere Pull Request
4. Post recognitionem, tuum plugin apparet in Plugin Store pro omnibus usoribus!

---

## 🔀 Quomodo Submittere Pull Request

### Pro principali app (wia-soom)

1. Fork repositorium
2. Crea ramum functionis: `git checkout -b feat/my-feature`
3. Fac tuas mutationes
4. Testa localiter:
   ```bash
   ```
5. Committe cum clara nuntiatione:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push et aperi PR contra `main`

### Conventio Nuntiationis Commit

| Praefixum | Usus pro |
|-----------|----------|
| `feat:`   | Nova functio |
| `fix:`    | Emendatio erroris |
| `docs:`   | Solum documentatio |
| `refactor:` | Restructuratio codicis (nulla mutatio actionis) |
| `i18n:`   | Updates translationis |
| `plugin:` | Mutationes ad plugin pertinentes |

### PR Checklist

- [ ] Codex sine erroribus currit
- [ ] Nulla strings hardcoded (utere i18n claves)
- [ ] Nulla `console.log` relicta in codice productionis
- [ ] Testes existentes adhuc transiunt

---

## 🌐 Translation Contributions (254 Linguarum)

WIA SOOM sustinet **254 linguas** — ab Amharico ad Zulum, inclusis Braille et linguis RTL.

### Quomodo Translatio Operatur

- Lingua fundamentalis file: `src/renderer/src/i18n/en.json`
- Omnes 254 linguae files in eadem directory
- Translatio fit per `scripts/translate-patch.js` (GPT-4o-mini API)

### Quomodo Contribuere Translationes

#### Optio 1: Emendare specificam translationem

1. Invenire linguam file: `src/renderer/src/i18n/{lang-code}.json`
2. Emenda translationem incorrectam
3. Submittere PR cum mutatione

#### Optio 2: Addere claves desunt
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Optio 3: Recensere translationes machinales

Multae ex nostris 254 linguis machinaliter translatae sunt. Recensiones locutorum indigenarum valde pretiosae sunt!

1. Elige tuum linguam file
2. Recensere translationes
3. Emenda quaecumque awkward aut incorrectas translationes
4. Submittere PR

### Codices Linguarum

Utimur standardibus ISO 639-1 codicibus (exempli gratia, `ko`, `en`, `ja`, `ar`, `hi`) cum variantibus regionalibus ubi necesse est (exempli gratia, `zh-CN`, `pt-BR`).

---

## 🛠 Configuratio Evolutionis

### Prae-requisita

- Node.js 18+
- npm 9+
- Git

### Configuratio
```bash
```
### Constructio
```bash
```
> Nota: Default 2GB heap non satis est propter 254 linguas files + Monaco editor bundle (~38MB renderer).

### Structura Proiecti
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

## 🙏 Gratias Tibi Agimus

Quotidiana contributio WIA SOOM melius facit pro programmatoribus per orbem terrarum.

Sive typo corrigas, stringam transferas, plugin construas, sive maiorem functionem addas — **pars huius fabulae es.**

---

<p align="center"><em>Constructum cum ❤️ a SmileStory Inc. et contributoribus toto orbe.</em></p>