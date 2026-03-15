<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribueting long WIA SOOM</h1>
<p align="center"><strong>Yumi lavem yufala kontribuson!</strong></p>
<p align="center">Sapos hem wan bug fix, niu fetja, plugin, o translason — evri kontribuson i metta.</p>

---

## Teibl blong Kontents

- [Kode blong Kondukt](#kode-blong-kondukt)
- [Olsem blong Reportim Bugs](#-olsem-blong-reportim-bugs)
- [Olsem blong Sugestem Fetja](#-olsem-blong-sugestem-fetja)
- [Olsem blong Submitim wan Plugin](#-olsem-blong-submitim-wan-plugin)
- [Olsem blong Submitim wan Pull Request](#-olsem-blong-submitim-wan-pull-request)
- [Translason Kontribuson (254 Languages)](#-translason-kontribuson-254-languages)
- [Development Setup](#-development-setup)

---

## Kode blong Kondukt

Yumi i komit long givim wan welcome mo inclusive experience blong evriwan.

- **Bikpela respekte.** Tritim evriwan long dignity.
- **Bikpela konstruktiv.** Offerim helpfull feedback, no destruktiv kritik.
- **Bikpela inclusive.** Yumi sapotem 254 languages mo welcome contributors from evri kantri long Earth.
- **No harassment.** Zero tolerance blong diskriminason blong eni kaen.

---

## 🐛 Olsem blong Reportim Bugs

1. Go long [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikim **"Niu Isiu"**
3. Chusim **"Bug Report"** template
4. Inkludim:
   - WIA SOOM version (Settings → About)
   - OS mo version (Windows/macOS/Linux)
   - Steps blong reproducem
   - Ekspekted vs. actual behavior
   - Screenshots o terminal output sapos i posibol

---

## 💡 Olsem blong Sugestem Fetja

1. Go long [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikim **"Niu Isiu"**
3. Chusim **"Feature Request"** template
4. Deskraib:
   - Wetem problem yu i solvem
   - Olsem yu i imagine hem i wok
   - Eny alternatives yu i konsidarem

---

## 🔌 Olsem blong Submitim wan Plugin

WIA SOOM i gat wan paoaful plugin sistem — yu ken bildim yufala plugin long 5 minit.

### Kwik Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ful Gaid

Ridim the **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** blong:
- Komplit API reference
- Wokim eksampol
- Step-by-step tutorials
- Best practices mo security rul

### Submitim Yufala Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Addim yufala plugin long `plugins/{your-plugin-name}/`
3. Submitim wan Pull Request
4. Afta review, yufala plugin i kamap long Plugin Store blong ol yusers!

---

## 🔀 Olsem blong Submitim wan Pull Request

### Blong the main app (wia-soom)

1. Fork the repository
2. Kreate wan feature branch: `git checkout -b feat/my-feature`
3. Mekim yufala changes
4. Testim lokali:
   ```bash
   ```
5. Commitim wetem wan klia message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push mo openim wan PR against `main`

### Commit Message Convention

| Prefix | Yus blong |
|--------|-----------|
| `feat:` | Niu fetja |
| `fix:` | Bug fix |
| `docs:` | Dokumentason onli |
| `refactor:` | Kode restructuring (no behavior change) |
| `i18n:` | Translason updates |
| `plugin:` | Plugin-related changes |

### PR Checklist

- [ ] Kode i ron wetemaut errors
- [ ] No hardcoded strings (yusim i18n keys)
- [ ] No `console.log` left long production kode
- [ ] Existing tests i stil pas

---

## 🌐 Translason Kontribuson (254 Languages)

WIA SOOM i sapotem **254 languages** — from Amharic go long Zulu, inkludim Braille mo RTL languages.

### Olsem Translason i Wok

- Base language file: `src/renderer/src/i18n/en.json`
- Ol 254 language files i stap long sem directory
- Translason i mekem long `scripts/translate-patch.js` (GPT-4o-mini API)

### Olsem blong Kontribueting Translason

#### Option 1: Fixim wan spesifik translason

1. Faendem the language file: `src/renderer/src/i18n/{lang-code}.json`
2. Fixim the incorrect translason
3. Submitim wan PR wetem the change

#### Option 2: Addim missing keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Reviewim machine translations

Plenti blong yumi 254 languages i bin machine-translated. Native speaker reviews i valuble tumas!

1. Pikim yufala language file
2. Reviewim the translations
3. Fixim eni awkward o incorrect translations
4. Submitim wan PR

### Language Codes

Yumi yusim standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) wetem regional variants we i nidim (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Bild
```bash
```
> Nots: The default 2GB heap i no inap long 254 language files + Monaco editor bundle (~38MB renderer).

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

## 🙏 Tank Yu

Evri kontribuson i mekem WIA SOOM i betta fo ol developa raon the world.

Sapos yu fiks wan typo, translit wan string, bildim wan plugin, o adim wan major feature — **yu i parte blong dis stori.**

---

<p align="center"><em>Bilum wet ❤️ blong SmileStory Inc. mo ol kontributa raon the world.</em></p>