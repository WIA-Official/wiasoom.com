<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM માં યોગદાન આપવું</h1>
<p align="center"><strong>અમે તમારી યોગદાનની રાહ જોઈ રહ્યા છીએ!</strong></p>
<p align="center">ચુકી સુધારણા, નવી સુવિધા, પ્લગિન, અથવા અનુવાદ — દરેક યોગદાન મહત્વપૂર્ણ છે.</p>

---

## સામગ્રીની યાદી

- [આચરણ સંહિતા](#code-of-conduct)
- [બગ્સ કેવી રીતે રિપોર્ટ કરવો](#-how-to-report-bugs)
- [સુવિધાઓ કેવી રીતે સૂચવવી](#-how-to-suggest-features)
- [પ્લગિન કેવી રીતે સબમિટ કરવો](#-how-to-submit-a-plugin)
- [પુલ રિક્વેસ્ટ કેવી રીતે સબમિટ કરવો](#-how-to-submit-a-pull-request)
- [અનુવાદ યોગદાન (254 ભાષાઓ)](#-translation-contributions-254-languages)
- [વિકાસ સેટઅપ](#-development-setup)

---

## આચર�� સંહિતા

અમે દરેક માટે એક સ્વાગત અને સમાવેશકારી અનુભવ પ્રદાન કરવા માટે પ્રતિબદ્ધ છીએ.

- **આદર કરો.** દરેકને માન સાથે વર્તો.
- **ગણનાત્મક રહો.** મદદરૂપ પ્રતિસાદ આપો, વિનાશક ટીકા નહીં.
- **સમાવિષ્ટ રહો.** અમે 254 ભાષાઓને સમર્થન આપીએ છીએ અને પૃથ્વી પરના દરેક દેશના યોગદાતાઓનું સ્વાગત કરીએ છીએ.
- **હેરાનગી નહીં.** કોઈપણ પ્રકારની ભેદભાવ માટે શૂન્ય સહનશીલતા.

---

## 🐛 બગ્સ કેવી રીતે રિપોર્ટ કરવો

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) પર જાઓ
2. **"નવું ઇશ્યૂ"** પર ક્લિક કરો
3. **"બગ રિપોર્ટ"** ટેમ્પ્લેટ પસંદ કરો
4. સમાવેશ કરો:
   - WIA SOOM સંસ્કરણ (સેટિંગ્સ → ���િશે)
   - OS અને સંસ્કરણ (Windows/macOS/Linux)
   - પુનરાવૃત્તિ માટેના પગલાં
   - અપેક્ષિત અને વાસ્તવિક વર્તન
   - શક્ય હોય તો સ્ક્રીનશોટ અથવા ટર્મિનલ આઉટપુટ

---

## 💡 સુવિધાઓ કેવી રીતે સૂચવવી

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) પર જાઓ
2. **"નવું ઇશ્યૂ"** પર ક્લિક કરો
3. **"સુવિધા વિનંતી"** ટેમ્પ્લેટ પસંદ કરો
4. વર્ણન કરો:
   - તમે કયો સમસ્યા ઉકેલવા માંગો છો
   - તમે તે કેવી રીતે કાર્યરત હોવાની કલ્પના કરો છો
   - તમે વિચારેલા કોઈ વિકલ્પો

---

## 🔌 પ્લગિન કેવી રીતે સબમિટ કરવો

WIA SOOM પા���ે એક શક્તિશાળી પ્લગિન સિસ્ટમ છે — તમે 5 મિનિટમાં તમારું પોતાનું પ્લગિન બનાવી શકો છો.

### ઝડપી શરૂઆત
§§§CHUNK_SEPARATOR§§§
### સંપૂર્ણ માર્ગદર્શિકા

**[પ્લગિન ડેવલપર માર્ગદર્શિકા](docs/PLUGIN_DEVELOPER_GUIDE.md)** વાંચો:
- સંપૂર્ણ API સંદર્ભ
- કાર્યરત ઉદાહરણો
- પગલાં-દ્વારા-પગલાં ટ્યુટોરિયલ
- શ્રેષ્ઠ પ્રથાઓ અને સુરક્ષા નિયમો

### તમારું પ્લગિન સબમિટ કરો

1. [Plugin Store](https://wiasoom.com) ફોર્ક કરો
2. તમારા પ્લગિનને `plugins/{your-plugin-name}/` માં ઉમેરો
3. પુલ રિક્વેસ્ટ સબમિટ કરો
4. સમીક્ષા પછી, તમારું પ્લગિન તમામ વપરાશકર્તાઓ માટે પ્લગિન સ્ટોરમાં દેખાય છે!

---

## 🔀 પ���લ રિક્વેસ્ટ કેવી રીતે સબમિટ કરવો

### મુખ્ય એપ્લિકેશન (wia-soom) માટે

1. રિપોઝિટરી ફોર્ક કરો
2. એક ફીચર બ્રાંચ બનાવો: `git checkout -b feat/my-feature`
3. તમારા ફેરફારો કરો
4. સ્થાનિક રીતે પરીક્ષણ કરો:
   ```bash
   ```
5. સ્પષ્ટ સંદેશ સાથે કમિટ કરો:
   ```
   feat: સેટિંગ્સમાં ડાર્ક મોડ ટોગલ ઉમેરો
   ```
6. `main` સામે એક PR ખોલો

### કમિટ સંદેશ પરંપરા

| પ્રિફિક્સ | ઉપયોગ માટે |
|-----------|-------------|
| `feat:`   | નવી સુવિધા |
| `fix:`    | બગ સુધારણા |
| `docs:`   | ફક્ત દસ્તાવેજીકરણ |
| `refactor:` | કોડ પુનર્ગઠન (કોઈ વર્તન બદલાવ નથી) |
| `i18n:`   | અનુવાદ અપડેટ |
| `plugin:` | પ્લગિન સંબંધિત ફેરફારો |

### PR ચેકલિસ��ટ

- [ ] કોડ કોઈ ભૂલ વિના ચાલે છે
- [ ] કોઈ હાર્ડકોડેડ સ્ટ્રિંગ્સ નથી (i18n કીનો ઉપયોગ કરો)
- [ ] ઉત્પાદન કોડમાં કોઈ `console.log` નથી
- [ ] અસ્તિત્વમાં આવેલા પરીક્ષાઓ હજુ પણ પસાર થાય છે

---

## 🌐 અનુવાદ યોગદાન (254 ભાષાઓ)

WIA SOOM **254 ભાષાઓ**ને સમર્થન આપે છે — અમ્હારિકથી ઝુલુ સુધી, બ્રેલ અને RTL ભાષાઓ સહિત.

### અનુવાદ કેવી રીતે કાર્ય કરે છે

- આધારભૂત ભાષા ફાઇલ: `src/renderer/src/i18n/en.json`
- તમામ 254 ભાષા ફાઇલો એક જ ડિરેક્ટરીમાં છે
- અનુવાદ `scripts/translate-patch.js` (GPT-4o-mini API) દ્વારા કરવામાં આવે છે

### અનુવાદમાં યોગદાન કેવી રીતે આપવું

#### વિકલ્પ 1: ચોક્કસ અનુવાદ સુધારવો

1. ભાષા ફાઇલ ��ોધો: `src/renderer/src/i18n/{lang-code}.json`
2. ખોટા અનુવાદને સુધારો
3. ફેરફાર સાથે એક PR સબમિટ કરો

#### વિકલ્પ 2: ગુમ થયેલ કી ઉમેરો
§§§CHUNK_SEPARATOR§§§
#### વિકલ્પ 3: મશીન અનુવાદોની સમીક્ષા કરો

અમારી 254 ભાષાઓમાંથી ઘણી મશીન-અનુવાદિત છે. સ્થાનિક ભાષાશાસ્ત્રીઓની સમીક્ષા અત્યંત મૂલ્યવાન છે!

1. તમારી ભાષા ફાઇલ પસંદ કરો
2. અનુવાદોની સમીક્ષા કરો
3. કોઈપણ અણધાર્યા અથવા ખોટા અનુવાદોને સુધારો
4. એક PR સબમિટ કરો

### ભાષા કોડ

અમે માનક ISO 639-1 કોડ્સ (જેમ કે, `ko`, `en`, `ja`, `ar`, `hi`) નો ઉપયોગ કરીએ છીએ, જ્યાં જરૂરી હોય ત્યાં પ્રદેશીય વૈવિધ્ય સાથે (જેમ કે, `zh-CN`, `pt-BR`).

---

## 🛠 વિકાસ સેટઅપ

### પૂર્વશરતો

- Node.js 18+
- npm 9+
- Git

### સેટઅપ
§§§CHUNK_SEPARATOR§§§
### બિલ્ડ
§§§CHUNK_SEPARATOR§§§
> નોંધ: ડિફોલ્ટ 2GB હિપ 254 ભાષા ફાઇલો + મોનાકો સંપાદક બંડલ (~38MB રેન્ડરર)ને કારણે પૂરતું નથી.

### પ્રોજેક્ટ રચના
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 આભાર

દરેક યોગદાન WIA SOOM ને વિશ્વભરના વિકાસકર્તાઓ માટે વધુ સારું બનાવે છે.

તમે ટાઇપો ઠીક કરો, એક સ્ટ્રિંગ અનુવાદ કરો, એક પ્લગિન બનાવો, અથવા એક મુખ્ય ફીચર ઉમેરો — **તમે આ વાર્તાનો ભાગ છો.**

---

<p align="center"><em>❤️ સાથે બનાવ્યું SmileStory Inc. અને વિશ્વભરના યોગદાતાઓ દ્વારા.</em></p>
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
