<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kendalc'h da WIA SOOM</h1>
<p align="center"><strong>Meur a c'hendalc'h a c'houlenn!</strong></p>
<p align="center>Pe c'hwi a zo ur bug, ur feur, ur plugin pe ur frazenn — pep c'hendalc'h a zo pouezus.</p>

---

## Toupin D'ar C'hont

- [Kod d'ober](#code-of-conduct)
- [Penaos da adkavout bug](#-how-to-report-bugs)
- [Penaos da c'houlenn feurioù](#-how-to-suggest-features)
- [Penaos da submit ur plugin](#-how-to-submit-a-plugin)
- [Penaos da submit ur Pull Request](#-how-to-submit-a-pull-request)
- [Kendalc'hioù frazennoù (254 yezh)](#-translation-contributions-254-languages)
- [Setu ar Raktres](#-development-setup)

---

## Kod d'ober

Emaomp o c'houlenn ur c'hinnig a-fet a-berzh an holl.

- **Be respectful.** Grit ur c'houlenn a-fet d'an holl.
- **Be constructive.** Roit ur c'houlenn a-fet, n'eo ket ur c'hritik destrus.
- **Be inclusive.** Grit ur c'houlenn a-fet d'an holl, e vez kinniget 254 yezh ha degemeret an holl a zo e pep bro er Bearth.
- **No harassment.** N'eus ket a c'houlenn a-fet evit an diskriminadur ebet.

---

## 🐛 Penaos da adkavout bug

1. Kemerit da [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikit **"New Issue"**
3. Dibabit ar c'houlenn **"Bug Report"**
4. Enklasit:
   - WIA SOOM verz (Settin → Abo)
   - OS ha verz (Windows/macOS/Linux)
   - Derañser da adkavout
   - Behadur a c'houlenn ha gwir
   - Skeudenn pe disoc'h terminal ma'z eo posubl

---

## 💡 Penaos da c'houlenn feurioù

1. Kemerit da [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikit **"New Issue"**
3. Dibabit ar c'houlenn **"Feature Request"**
4. Deskrivit:
   - Peseurt problem e c'houlennit
   - Peseurt doare e c'houlennit
   - Pep all a c'houlennit a zo bet goulennet

---

## 🔌 Penaos da submit ur plugin

WIA SOOM a zo ur sistem plugin galloudus — gallout a rit sevel ho plugin e 5 munut.

### Start Dihun
§§§CHUNK_SEPARATOR§§§
### Derc'hel Kinnig

Lec'hiañ ar **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** evit:
- Derc'hel API komplez
- Skouerioù o labourat
- Tutoioù e steps
- Best practices ha reolennoù surentez

### Submitit ho Plugin

1. Forkit [Plugin Store](https://wiasoom.com)
2. Ouzhpennañ ho plugin da `plugins/{your-plugin-name}/`
3. Submitit ur Pull Request
4. Goude ar c'hontrol, e vo ho plugin e Plijadur Store evit an holl implijourien!

---

## 🔀 Penaos da submit ur Pull Request

### Evit ar programm pennañ (wia-soom)

1. Forkit ar repo
2. Krouit ur branch feur: `git checkout -b feat/my-feature`
3. Derc'hel ho changes
4. Testit en lokal:
   ```bash
   ```
5. Commitit gant ur messaj sklaer:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pushit ha digoret ur PR a-enep `main`

### Kodenn Messaj

| Prefix | Use for |
|--------|---------|
| `feat:` | Feur nevez |
| `fix:` | Adkav bug |
| `docs:` | Dokumentation hepken |
| `refactor:` | Adsevel kod (n'eo ket ur c'houlenn a-bezh) |
| `i18n:` | Adkas frazennoù |
| `plugin:` | Changement a-fet plugin |

### PR Checklist

- [ ] Kode a ra en ur c'houlenn hep bug
- [ ] N'eus ket a frazennoù hardcoded (use i18n keys)
- [ ] N'eus ket `console.log` chomet en kod produiñ
- [ ] Testoù a zo bet a-raok c'hoari

---

## 🌐 Kendalc'hioù frazennoù (254 yezh)

WIA SOOM a zo kinniget **254 yezh** — eus Amharic da Zulu, o c'hopret Braille ha yezhoù RTL.

### Peseurt Penaos e labour ar frazennoù

- Fichieroù yezh baz: `src/renderer/src/i18n/en.json`
- An holl 254 fichieroù yezh a zo en ur meneger
- E labourer ar frazennoù dre `scripts/translate-patch.js` (GPT-4o-mini API)

### Peseurt Penaos da c'hendalc'h frazennoù

#### Dalc'h 1: Adkav ur frazenn bennañ

1. Klaskit ar fichieroù yezh: `src/renderer/src/i18n/{lang-code}.json`
2. Adkavit ar frazenn fall
3. Submitit ur PR gant ar c'hangement

#### Dalc'h 2: Ouzhpennañ klavioù fall
§§§CHUNK_SEPARATOR§§§
#### Dalc'h 3: Adkav ar frazennoù madoù

Meur a ziezh eus an 254 yezh a zo bet adkavet gant ar madoù. Adkav a ra ar re a zo ur c'houlenn a-fet!

1. Derc'hel ho fichieroù yezh
2. Adkavit ar frazennoù
3. Adkavit ar frazennoù fall pe fall
4. Submitit ur PR

### Kodennoù Yezh

Grit a raomp gant kodennoù ISO 639-1 standard (e.g., `ko`, `en`, `ja`, `ar`, `hi`) gant variantioù reol d'ar c'houlenn (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Setu ar Raktres

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setu
§§§CHUNK_SEPARATOR§§§
### Krouiñ
§§§CHUNK_SEPARATOR§§§
> Nota: Ar heap 2GB a zo nebeud a-walc'h abalamour d'ar 254 fichieroù yezh + bundle Monaco (~38MB renderer).

### Strukture ar Raktres
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Trugarez

Pep kemm a ra WIA SOOM gwelloc'h evit ar c'hendivizien er bed.

Pe e teurel ur fazi, pe e traduis ur c'homz, pe e sevel ur plugin, pe e ouzhpennañ ur brasañ feur — **c'hwi a zo un partez eus ar c'hont.**

---

<p align="center"><em>Sevel a ra ❤️ gant SmileStory Inc. ha kenderc'hiennoù er bed a-bezh.</em></p>
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
