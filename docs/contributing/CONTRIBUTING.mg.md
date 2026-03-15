<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Mandraisa anjara amin'ny WIA SOOM</h1>
<p align="center"><strong>Maniry ny fandraisanao anjara izahay!</strong></p>
<p align="center>Na fanitsiana bibikely, endri-javatra vaovao, plugin, na fandikana — zava-dehibe ny fandraisana anjara rehetra.</p>

---

## Tabilao ao amin'ny Votoaty

- [Code of Conduct](#code-of-conduct)
- [Ahoana ny Fanaovana Tatitra Bibikely](#-how-to-report-bugs)
- [Ahoana ny Fanaovana Soso-kevitra Endri-javatra](#-how-to-suggest-features)
- [Ahoana ny Fanaovana Fampiharana Plugin](#-how-to-submit-a-plugin)
- [Ahoana ny Fanaovana Pull Request](#-how-to-submit-a-pull-request)
- [Fandraisana anjara amin'ny Fandikana (254 Fiteny)](#-translation-contributions-254-languages)
- [Fikirana ny Fampandrosoana](#-development-setup)

---

## Code of Conduct

Manolo-tena izahay hanome traikefa mandray sy manampy ho an'ny rehetra.

- **Mankasitraka.** Omeo voninahitra ny rehetra.
- **Mamorona.** Manolora fanehoan-kevitra mahasoa, fa tsy fanakianana manimba.
- **Mampiditra.** Manohana fiteny 254 izahay ary mandray mpandray anjara avy amin'ny firenena rehetra eto an-tany.
- **Tsy misy fanararaotana.** Tsy misy fandeferana amin'ny fanavakavahana amin'ny karazany rehetra.

---

## 🐛 Ahoana ny Fanaovana Tatitra Bibikely

1. Mandehana any amin'ny [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tsindrio ny **"New Issue"**
3. Safidio ny **"Bug Report"** template
4. Ampidiro:
   - WIA SOOM dikan-teny (Settings → About)
   - OS sy dikan-teny (Windows/macOS/Linux)
   - Dingana hamerenana
   - Fihetsika andrasana vs. tena fihetsika
   - Sary na vokatra terminal raha azo atao

---

## 💡 Ahoana ny Fanaovana Soso-kevitra Endri-javatra

1. Mandehana any amin'ny [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tsindrio ny **"New Issue"**
3. Safidio ny **"Feature Request"** template
4. Farito:
   - Inona no olana vahanao
   - Ahoana no eritreretinao fa hiasa izany
   - Na inona na inona safidy noheverinao

---

## 🔌 Ahoana ny Fanaovana Fampiharana Plugin

Manana rafitra plugin matanjaka ny WIA SOOM — afaka mamorona plugin anao manokana ao anatin'ny 5 minitra ianao.

### Fanombohana Haingana
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Torolàlana Feno

Vakio ny **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ho an'ny:
- Fampahalalana API feno
- Ohatra miasa
- Torolàlana tsikelikely
- Fomba fanao tsara indrindra sy fitsipika fiarovana

### Alefaso ny Plugin-nao

1. Fork [Plugin Store](https://wiasoom.com)
2. Ampidiro ny plugin-nao ao amin'ny `plugins/{your-plugin-name}/`
3. Alefaso ny Pull Request
4. Rehefa vita ny fanombanana, hiseho ny plugin-nao ao amin'ny Plugin Store ho an'ny mpampiasa rehetra!

---

## 🔀 Ahoana ny Fanaovana Pull Request

### Ho an'ny app lehibe (wia-soom)

1. Fork ny tahiry
2. Mamorona sampana endri-javatra: `git checkout -b feat/my-feature`
3. Ataovy ny fanovanao
4. Andramo eo an-toerana:
   ```bash
   ```
5. Ataovy commit miaraka amin'ny hafatra mazava:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ary sokafy ny PR manohitra ny `main`

### Fitsipika Hafatra Commit

| Prefix | Ampiasaina ho an'ny |
|--------|---------------------|
| `feat:` | Endri-javatra vaovao |
| `fix:` | Fanitsiana bibikely |
| `docs:` | Fampahalalana fotsiny |
| `refactor:` | Fanamboarana kaody (tsy misy fiovana amin'ny fihetsika) |
| `i18n:` | Fanavaozana fandikana |
| `plugin:` | Fanovàna mifandraika amin'ny plugin |

### Lisitry ny PR

- [ ] Mandeha tsy misy hadisoana ny kaody
- [ ] Tsy misy andian-teny voatokana (ampiasao ny lakilen'ny i18n)
- [ ] Tsy misy `console.log` sisa tavela ao amin'ny kaody famokarana
- [ ] Mbola mandalo ny fitsapana efa misy

---

## 🌐 Fandraisana anjara amin'ny Fandikana (254 Fiteny)

Manohana **254 fiteny** ny WIA SOOM — manomboka amin'ny Amharic ka hatramin'ny Zulu, anisan'izany ny Braille sy ny fiteny RTL.

### Ahoana no Fandehan'ny Fandikana

- Rakitra fiteny fototra: `src/renderer/src/i18n/en.json`
- Ny rakitra fiteny 254 rehetra dia ao amin'ny lahatahiry mitovy
- Ny fandikana dia atao amin'ny alalan'ny `scripts/translate-patch.js` (GPT-4o-mini API)

### Ahoana ny Fanaovana Fandraisana anjara amin'ny Fandikana

#### Safidy 1: Manamboatra fandikana manokana

1. Mitadiava ny rakitra fiteny: `src/renderer/src/i18n/{lang-code}.json`
2. Amboary ny fandikana diso
3. Alefaso ny PR miaraka amin'ny fanovàna

#### Safidy 2: Ampidiro ny lakile tsy ampy
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Safidy 3: Diniho ny fandikana nataon'ny milina

Marobe amin'ireo fiteny 254 anay no nadika tamin'ny alalan'ny milina. Ny fanombanana ataon'ny mpiteny teratany dia tena sarobidy!

1. Safidio ny rakitra fiteny
2. Diniho ny fandikana
3. Amboary ny fandikana izay tsy mety na mahakivy
4. Alefaso ny PR

### Kaody Fiteny

Ampiasainay ny kaody ISO 639-1 mahazatra (ohatra, `ko`, `en`, `ja`, `ar`, `hi`) miaraka amin'ny fanovàna ara-pirenena raha ilaina (ohatra, `zh-CN`, `pt-BR`).

---

## 🛠 Fikirana ny Fampandrosoana

### Fepetra Takiana

- Node.js 18+
- npm 9+
- Git

### Fikirana
```bash
```
### Fananganana
```bash
```
> Fanamarihana: Ny heap 2GB default dia tsy ampy noho ny rakitra fiteny 254 + bundle editor Monaco (~38MB renderer).

### Rafitra Tetikasa
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

## 🙏 Misaotra

Ny fanampiana rehetra dia mahatonga ny WIA SOOM ho tsara kokoa ho an'ny mpamorona manerana izao tontolo izao.

Na manamboatra hadisoana ianao, mandika andian-teny, manorina plugin, na manampy endri-javatra lehibe — **isan'ny tantara ity ianao.**

---

<p align="center"><em>Noforonin'ny ❤️ avy amin'ny SmileStory Inc. sy ny mpandray anjara manerana izao tontolo izao.</em></p>