<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM irratti Hirmaachuu</h1>
<p align="center"><strong>Hirmaannaa keessan ni jaalanna!</strong></p>
<p align="center">Rakkoo sirreessuu, amaloota haaraa, plugin, yookiin hiikkaa — hirmaannaan kamiyyuu barbaachisaa dha.</p>

---

## Teessuma Qabiyyee

- [Seera Hirmaannaa](#code-of-conduct)
- [Rakkoolee Akka Itti Himamu](#-how-to-report-bugs)
- [Amaloota Akka Itti Fakkatan](#-how-to-suggest-features)
- [Plugin Akka Itti Galu](#-how-to-submit-a-plugin)
- [Pull Request Akka Itti Galu](#-how-to-submit-a-pull-request)
- [Hirmaannaa Hiikkaa (254 Afaan)](#-translation-contributions-254-languages)
- [Qophaa'ina Hojii](#-development-setup)

---

## Seera Hirmaannaa

Namoota hundaaf muuxannoo gammachiisaa fi hirmaachisaa kennuuf waadaa galuudhaan jira.

- **Kabaja qabaadhu.** Namoota hundumaa kabajaan ilaali.
- **Gargaarsa ta'i.** Yaada gargaaraa dhiheessi, yaada miidhaa hin kennine.
- **Hirmaachisaa ta'i.** Afaan 254 deeggarra, hirmaattota biyya hundumaa simanna.
- **Hiriyaa hin ta'in.** Madaa'aa kamiyyuu irratti hiriyaa hin qabnu.

---

## 🐛 Rakkoolee Akka Itti Himamu

1. Gara [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) deemi
2. **"Rakkoo Haaraa"** tuqi
3. **"Rakkoo"** template filadhu
4. Dabalata:
   - WIA SOOM version (Settings → About)
   - OS fi version (Windows/macOS/Linux)
   - Tarkaanfiiwwan deebi'uu
   - Hojii eegamaa fi dhugaa
   - Suuraawwan yookiin baafata terminal yoo danda'ame

---

## 💡 Amaloota Akka Itti Fakkatan

1. Gara [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) deemi
2. **"Rakkoo Haaraa"** tuqi
3. **"Amaloota Itti Fakkatan"** template filadhu
4. Ibsa:
   - Rakkoo maal furuuf jirtu
   - Akkamitti hojjachuu yaadu
   - Filannoon kamiyyuu yaadatte

---

## 🔌 Plugin Akka Itti Galu

WIA SOOM sirna plugin cimaa qaba — plugin ofii keetiin daqiiqaa 5 keessatti ijaaru dandeessa.

### Ka'umsaa
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Qajeelfama Guutuu

**[Qajeelfama Hojjetaa Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** dubbisi:
- Qajeelfama API guutuu
- Fakkeenya hojjataman
- Qajeelfama tarkaanfii-tarkaanfii
- Kallattiiwwan gaarii fi seerota nageenya

### Plugin Kee Gali

1. [Plugin Store](https://wiasoom.com) fork godhi
2. Plugin kee `plugins/{your-plugin-name}/` keessatti dabalata
3. Pull Request dhiheessi
4. Iddoowwan qorannoo booda, plugin kee Plugin Store keessatti hundaaf mul'ata!

---

## 🔀 Pull Request Akka Itti Galu

### App guddaa (wia-soom)

1. Repository fork godhi
2. Branch amaloota uumi: `git checkout -b feat/my-feature`
3. Jijjiirraa kee gochuu
4. Naannoo keessatti qoradhu:
   ```bash
   ```
5. Ergaa ifa ta'e waliin commit godhi:
   ```
   feat: dark mode toggle settings irratti dabaluu
   ```
6. Push gochuu fi PR `main` irratti banaa

### Seera Ergaa Commit

| Prefix | Fayyadama |
|--------|-----------|
| `feat:` | Amaloota haaraa |
| `fix:` | Rakkoo sirreessuu |
| `docs:` | Dokumentii qofa |
| `refactor:` | Qoodinsa koodii (bifa jijjiiruu hin qabu) |
| `i18n:` | Hiikkaa haaromsuu |
| `plugin:` | Jijjiirraa plugin waliin walqabatee |

### PR Checklist

- [ ] Koodiin dogoggora malee hojjata
- [ ] Barruulee cimaan hin jirre (i18n keys fayyadami)
- [ ] `console.log` koodii oomisha keessatti hin hafne
- [ ] Qorannoon duraan jiru ni darbu

---

## 🌐 Hirmaannaa Hiikkaa (254 Afaan)

WIA SOOM **254 afaan** deeggarra — Afaan Amhaariik irraa hanga Zulu, Braille fi afaanota RTL dabalatee.

### Akkamitti Hiikkaan Hojjata

- Faayila afaan bu'uuraa: `src/renderer/src/i18n/en.json`
- Faayiloonni afaan 254 hundi bakka tokkotti jiru
- Hiikkaan `scripts/translate-patch.js` (GPT-4o-mini API) irratti hojjatama

### Hiikkaa Dhiheessi

#### Filannoo 1: Hiikkaa sirrii gochuu

1. Faayila afaan barbaadi: `src/renderer/src/i18n/{lang-code}.json`
2. Hiikkaa dogoggora sirreessi
3. PR jijjiirraa dhiheessi

#### Filannoo 2: Keywwan dhaban dabaluu
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Filannoo 3: Hiikkaa makiinaa qorachuu

Afaanota keenya 254 keessaa hedduun hiikkaa makiinaa ta'aniiru. Qorannoon namoota afaan sana dubbatan baay'ee barbaachisaa dha!

1. Faayila afaan keeti filadhu
2. Hiikkaa qoradhu
3. Hiikkaa dogoggora yookiin hin malle sirreessi
4. PR dhiheessi

### Koodii Afaan

Koodii ISO 639-1 kan sirna qajeelfama fayyadamna (fakkeenyaaf, `ko`, `en`, `ja`, `ar`, `hi`) bakka barbaachisaa ta'etti (fakkeenyaaf, `zh-CN`, `pt-BR`).

---

## 🛠 Qophaa'ina Hojii

### Barbaachisummaa

- Node.js 18+
- npm 9+
- Git

### Qophaa'ina
```bash
```
### Ijaarsa
```bash
```
> Yaadachiisa: Heap 2GB kan duraa 254 faayila afaanii + bundle editor Monaco (~38MB renderer) hin guutuu.

### Qindaa'ina Projeektii
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

## 🙏 Galatoomaa

Hirmaannaan kamiyyuu WIA SOOM fooyyessuuf gargaara, deggertoota addunyaa irratti.

Yoo dogoggora barreessuu sirreessite, tarree tokko hiikite, plugin ijaarte, ykn amaloota guddaa dabalatte — **sii seenaa kana keessatti hirmaattuu dha.**

---

<p align="center"><em>❤️n ijaare SmileStory Inc. fi hirmaattota addunyaa irraa.</em></p>