<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM కు సహకరించడం</h1>
<p align="center"><strong>మీ సహకారాలను మేము ఇష్టపడుతాము!</strong></p>
<p align="center">ఇది బగ్ ఫిక్స్, కొత్త ఫీచర్, ప్లగిన్ లేదా అనువాదం అయినా — ప్రతి సహకారం ముఖ్యమైనది.</p>

---

## విషయాల జాబితా

- [ఆచార సంకేతం](#code-of-conduct)
- [బగ్‌లను ఎలా నివేదించాలి](#-how-to-report-bugs)
- [ఫీచర్‌లను ఎలా సూచించాలి](#-how-to-suggest-features)
- [ప్లగిన్‌ను ఎలా సమర్పించాలి](#-how-to-submit-a-plugin)
- [పుల్ రిక్వెస్ట్‌ను ఎలా సమర్పించాలి](#-how-to-submit-a-pull-request)
- [అనువ��ద సహకారాలు (254 భాషలు)](#-translation-contributions-254-languages)
- [అభివృద్ధి సెటప్](#-development-setup)

---

## ఆచార సంకేతం

మేము అందరికీ స్వాగతం మరియు సమావిష్కరణ అనుభవాన్ని అందించడంలో కట్టుబడి ఉన్నాము.

- **గౌరవంగా ఉండండి.** అందరిని గౌరవంతో చూడండి.
- **సంకల్పాత్మకంగా ఉండండి.** సహాయకమైన అభిప్రాయాలను ఇవ్వండి, ధ్వంసకరమైన విమర్శలను కాదు.
- **సమావిష్కరణగా ఉండండి.** మేము 254 భాషలను మద్దతు ఇస్తాము మరియు ప్రపంచంలోని ప్రతి దేశం నుండి సహకారులను స్వాగతిస్తాము.
- **హరాస్మెంట్ లేదు.** ఏ విధమైన వివక్షకు జీరో సహనం.

---

## 🐛 బగ్‌లను ఎలా నివేదించాలి

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)కి వెళ్లండి
2. **"కొత్త సమస్య"**పై క్లిక్ చేయండి
3. **"బగ్ నివేదిక"** టెంప్లేట్‌ను ఎంచుకోండి
4. చేర్చండి:
   - WIA SOOM వెర్షన్ (సెట్టింగ్స్ → గురించి)
   - OS మరియు వెర్షన్ (Windows/macOS/Linux)
   - పునరుత్పత్తి చేయడానికి దశలు
   - ఆశించిన vs. వాస్తవ ప్రవర్తన
   - స్క్రీన్‌షాట్‌లు లేదా టెర్మినల్ అవుట్‌పుట్ ఉంటే

---

## 💡 ఫీచర్‌లను ఎలా సూచించాలి

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)కి వెళ్లండి
2. **"కొత్త సమస్య"**పై క్లిక్ చేయండి
3. **"ఫీచర్ అభ్యర్థన"** టెంప్లేట్‌ను ఎంచుకోండి
4. వివరిస్తారు:
   - మీరు పరిష్కరించాలనుకుంటున్న సమస్య
   - ఇది ఎలా పనిచేస్తుందని మీరు ఊహిస్తున్నారో
   - మీరు పరిగణించిన ఏ ఇతర ప్రత్యామ్నాయాలు

---

## 🔌 ప్లగిన్‌ను ఎలా సమర్పించాలి

WIA SOOMకు శక్తివంతమైన ప్లగిన్ వ్యవస్థ ఉంది — మీరు 5 నిమిషాల్లో మీ స్వంత ప్లగిన్‌ను నిర్మించవచ్చు.

### తక్షణ ప్రారంభం
§§§CHUNK_SEPARATOR§§§
### పూర్తి గైడ్

**[ప్లగిన్ డెవలపర్ గైడ్](docs/PLUGIN_DEVELOPER_GUIDE.md)** ను చదవండి:
- పూర్తి API సూచిక
- పని చేసే ఉదాహరణలు
- దశల వారీ ట్యుటోరియల్స్
- ఉత్తమ పద్ధతులు మరియు భద్రతా నియమాలు

### మీ ప్లగిన్‌ను సమర్పించండి

1. [Plugin Store](https://wiasoom.com)ను ఫోర్క్ చేయండి
2. మీ ప్లగిన్‌ను `plugins/{your-plugin-name}/`లో చేర్చండి
3. పుల్ రిక్వెస్ట్‌ను సమర్పించండి
4. సమీక్ష తర్వాత, మీ ప్లగిన్ అన్ని వినియోగదారుల కోసం ప్లగిన్ స్టోర్‌లో కనిపిస్తుంది!

---

## 🔀 పుల్ రిక్వెస్ట్‌ను ఎలా సమర్పించాలి

### ప్రధాన యాప్ కోసం (wia-soom)

1. రిపోజిటరీని ఫోర్క్ చేయండి
2. ఫీచర్ బ్రాంచ్‌ను సృష్టించండి: `git checkout -b feat/my-feature`
3. మీ మార్పులను చేయండి
4. స్థానికంగా పరీక్షించండి:
   ```bash
   ```
5. స్పష్టమైన సందేశంతో కమిట్ చేయండి:
   ```
   feat: సెట్టింగ్స్‌కు డార్క్ మోడ్ టోగుల్‌ను చేర్చండి
   ```
6. `main`కు వ్యతిరేకంగా పుష్ చేసి PRను తెరవండి

### కమిట్ సందేశం నియమావళి

| ప్ర���ఫిక్స్ | ఉపయోగించడానికి |
|-----------|----------------|
| `feat:`   | కొత్త ఫీచర్   |
| `fix:`    | బగ్ ఫిక్స్     |
| `docs:`   | కేవలం డాక్యుమెంటేషన్ |
| `refactor:` | కోడ్ పునరుద్ధరణ (ప్రవర్తన మార్పు లేదు) |
| `i18n:`   | అనువాద నవీకరణలు |
| `plugin:` | ప్లగిన్ సంబంధిత మార్పులు |

### PR చెక్‌లిస్ట్

- [ ] కోడ్ ఎర్రర్లను లేకుండా నడుస్తుంది
- [ ] కఠినమైన స్ట్రింగ్స్ లేవు (i18n కీలు ఉపయోగించండి)
- [ ] ఉత్పత్తి కోడ్‌లో `console.log` మిగిలి లేదు
- [ ] ఉన్న పరీక్షలు ఇంకా విజయవంతంగా ఉంటాయి

---

## 🌐 అనువాద సహకారాలు (254 భాషలు)

WIA SOOM **254 భాషలను** మద్దతు ఇస్తుంది — అంహారిక్ నుండి జులూ వరకు, బ్రెయిల్ మరియు RTL భాషల��ు కూడా కలిగి ఉంది.

### అనువాదం ఎలా పనిచేస్తుంది

- ప్రాథమిక భాష ఫైల్: `src/renderer/src/i18n/en.json`
- అన్ని 254 భాష ఫైళ్లు ఒకే డైరెక్టరీలో ఉన్నాయి
- అనువాదం `scripts/translate-patch.js` ద్వారా చేయబడుతుంది (GPT-4o-mini API)

### అనువాదాలను ఎలా సహకరించాలి

#### ఎంపిక 1: ప్రత్యేక అనువాదాన్ని సరి చేయండి

1. భాష ఫైల్‌ను కనుగొనండి: `src/renderer/src/i18n/{lang-code}.json`
2. తప్పు అనువాదాన్ని సరి చేయండి
3. మార్పుతో PRను సమర్పించండి

#### ఎంపిక 2: కోల్పోయిన కీలను చేర్చండి
§§§CHUNK_SEPARATOR§§§
#### ఎంపిక 3: యంత్ర అనువాదాలను సమీక్షించండి

మా 254 భాషలలో చాలా యంత్ర అనువాదం చేయబడింది. స్థానిక మాట్లాడేవారి సమీక���షలు చాలా విలువైనవి!

1. మీ భాష ఫైల్‌ను ఎంచుకోండి
2. అనువాదాలను సమీక్షించండి
3. ఏ అసౌకర్యంగా లేదా తప్పు అనువాదాలను సరి చేయండి
4. PRను సమర్పించండి

### భాష కోడ్లు

మేము ప్రామాణిక ISO 639-1 కోడ్లను (ఉదా: `ko`, `en`, `ja`, `ar`, `hi`) అవసరమైన ప్రాంతీయ వేరియంట్లతో ఉపయోగిస్తాము (ఉదా: `zh-CN`, `pt-BR`).

---

## 🛠 అభివృద్ధి సెటప్

### ముందస్తు అవసరాలు

- Node.js 18+
- npm 9+
- Git

### సెటప్
§§§CHUNK_SEPARATOR§§§
### నిర్మాణం
§§§CHUNK_SEPARATOR§§§
> గమనిక: 254 భాషా ఫైళ్ల + మోనాకో ఎడిటర్ బండిల్ (~38MB) కారణంగా డిఫాల్ట్ 2GB హీప్ సరిపోదు.

### ప్రాజెక్ట్ నిర్మాణం
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 ధన్యవాదాలు

ప్రతి సహాయం WIA SOOM ను ప్రపంచవ్యాప్తంగా డెవలపర్ల కోసం మెరుగుపరుస్తుంది.

మీరు ఒక టైపోను సరిచేస్తే, ఒక స్ట్రింగ్‌ను అనువదిస్తే, ఒక ప్లగిన్‌ను నిర్మిస్తే లేదా ఒక ముఖ్యమైన ఫీచర్‌ను జోడిస్తే — **మీరు ఈ కథలో భాగం.**

---

<p align="center"><em>❤️ తో నిర్మించబడింది SmileStory Inc. మరియు ప్రపంచవ్యాప్తంగా సహాయకర్తల ద్వారా.</em></p>
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
