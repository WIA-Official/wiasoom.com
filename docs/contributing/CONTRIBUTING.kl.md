<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-imut peqataasinnaanermut</h1>
<p align="center"><strong>Peqataasinnaanermut qilanaarput!</strong></p>
<p align="center">Uanga bug-ikkut allannguuteqartitsisoq, nutaamik piujuartitsisoq, plugin, imaluunniit oqaatsit allannguuteqartitsisoq — tamarmik pingaaruteqarput.</p>

---

## Oqaatsoq

- [Suliassat malillugit](#code-of-conduct)
- [Bug-ikkut nalunaarusiorneq](#-how-to-report-bugs)
- [Piujuartitsinerit siunnersuutaasinnaasut](#-how-to-suggest-features)
- [Plugin-submit](#-how-to-submit-a-plugin)
- [Pull Request-submit](#-how-to-submit-a-pull-request)
- [Oqaatsit allannguuteqartitsinerit (254 Oqaatsit)](#-translation-contributions-254-languages)
- [Atuinerup pilersinneqarnissaa](#-development-setup)

---

## Suliassat malillugit

Uagut tamarmik peqataasinnaanermut qanimut piginnaasatsinnik qanimut piginnaasatsinnik qularnaarsinnaavugut.

- **Uagut qanimut piginnaasatsinnik.** Tamarmik qanimut piginnaasatsinnik.
- **Uagut qanimut piginnaasatsinnik.** Qanorluunniit iluaqutaanngitsumik oqaaseqaatinik tunniussinissaq.
- **Uagut qanimut piginnaasatsinnik.** 254 oqaasii qularnaarsinnaavugut, tamarmillu nunarsuarmi peqataasinnaasunik qanimut piginnaasatsinnik.
- **Qanorluunniit qinikkat.** Qanorluunniit qinikkat, qinikkat assigiinngitsut pillugit.

---

## 🐛 Bug-ikkut nalunaarusiorneq

1. Aallartillugu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Saqqummersitsisoq **"Bug Report"** malillugu
4. ilanngullugu:
   - WIA SOOM-version (Settings → About)
   - OS aamma version (Windows/macOS/Linux)
   - Aallartinneq
   - Qanorluunniit piujuartitsisoq vs. piujuartitsisoq
   - Skærmbilleder imaluunniit terminal output, periarfissaqartillugu

---

## 💡 Piujuartitsinerit siunnersuutaasinnaasut

1. Aallartillugu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Saqqummersitsisoq **"Feature Request"** malillugu
4. Qeqqi:
   - Qanorluunniit piujuartitsisoq
   - Qanorluunniit piujuartitsisoq
   - Qanorluunniit allanik eqqarsaatersuutigineqarsimasoq

---

## 🔌 Plugin-submit

WIA SOOM-ip plugin system-ia qaffasissuuvoq — 5 minutimi plugin-itut pilersinnaavutit.

### Siullermik Aallartinneq
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tamarmik Qeqqi

**[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** atuarlugu:
- API-p malittarisassai
- Suliassat malillugit
- Qeqqi-qeqqi
- Piginnaasat pitsaanerpaamik aamma inooqatigiinnermut malittarisassai

### Plugin-itit Submit

1. Fork [Plugin Store](https://wiasoom.com)
2. Plugin-itit `plugins/{your-plugin-name}/`-imut ilannguttagit
3. Pull Request-submit
4. Naluarut, plugin-itit Plugin Store-imi tamarmut atuarsinnaasinnaavutit!

---

## 🔀 Pull Request-submit

### Main app-imut (wia-soom)

1. Repository-p fork
2. Piujuartitsinermi branch: `git checkout -b feat/my-feature`
3. allannguuteqartit
4. Lokalimi misissuk:
   ```bash
   ```
5. Qularnaarsillugu:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push aamma `main`-imut PR-ini qaaq

### Qularnaarsillugu Oqaatsit

| Prefix | Atuisut |
|--------|---------|
| `feat:` | Nutaamik piujuartitsisoq |
| `fix:` | Bug-ikkut allannguuteqartitsineq |
| `docs:` | Dokumentation-itut |
| `refactor:` | Kode-p allannguuteqartitsineq (piujuartitsitsineq allannguuteqartinneqanngilaq) |
| `i18n:` | Oqaatsit allannguuteqartitsineq |
| `plugin:` | Plugin-imut tunngatillugu allannguuteqartitsineq |

### PR Checklist

- [ ] Kode-p qinikkat malillugit
- [ ] Qanorluunniit string-it (i18n key-itut)
- [ ] Qanorluunniit `console.log` qinikkat production kode-p iluani
- [ ] Siusinnerusukkut misissorneqartut malillugit

---

## 🌐 Oqaatsit allannguuteqartitsinerit (254 Oqaatsit)

WIA SOOM **254 oqaasii** qularnaarsinnaavugut — Amharic-imit Zulu-imut, Braille aamma RTL oqaasii ilanngullugit.

### Oqaatsit Qanorluunniit

- Base oqaasii faili: `src/renderer/src/i18n/en.json`
- 254 oqaasii faili tamarmik ataatsimut inissinneqassapput
- Oqaatsit allannguuteqartinneqassaaq `scripts/translate-patch.js` (GPT-4o-mini API)

### Oqaatsit allannguuteqartitsinerit

#### Aningaasaqarneq 1: Oqaatsit allannguuteqartitsinerit

1. Oqaatsit faili: `src/renderer/src/i18n/{lang-code}.json`
2. Qanorluunniit allannguuteqartitsinerit
3. PR-submit

#### Aningaasaqarneq 2: Qanorluunniit key-itut ilannguttagit
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Aningaasaqarneq 3: Maskin-ikkut allannguuteqartitsinerit

Tamatigut 254 oqaasii maskin-ikkut allannguuteqartitsinerat. Native speaker-itut qinikkat pingaaruteqarput!

1. Oqaatsit faili toqqaruk
2. Oqaatsit misissuk
3. Qanorluunniit allannguuteqartitsinerit
4. PR-submit

### Oqaatsit Kode-it

Uagut ISO 639-1 kode-it (e.g., `ko`, `en`, `ja`, `ar`, `hi`) malillugit atorneqassapput, immikkoortitsisartut (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Atuinerup pilersinneqarnissaa

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
> Qularnaarsillugu: Default 2GB heap 254 oqaasii faili + Monaco editor bundle (~38MB renderer) malillugit qinikkat.

### Projekti-p Qeqqi
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

## 🙏 Qujanaq

Tamassa peqataatitsineq WIA SOOM-ip ineriartortitsivinnut nunarsuarmi tamarmi pitsaanerulersitsivoq.

Uanga typokit allanngortitsissinnaavutit, stringit oqaatsinik allanngortitsissinnaavutit, pluginik pilersuussinnaavutit, imaluunniit pingaarutilimmik featuresi ilanngullugit — **suliat tamassa oqaluttuarisaanermut ilanngussinnaavutit.**

---

<p align="center"><em>❤️-mik SmileStory Inc.-ip aamma nunarsuarmi tamarmi peqataatitsisut pilersitseqattaarpaat.</em></p>