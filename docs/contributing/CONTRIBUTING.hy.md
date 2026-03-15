<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Նպաստել WIA SOOM-ին</h1>
<p align="center"><strong>Մենք սիրով սպասում ենք ձեր ներդրումներին!</strong></p>
<p align="center">Թող դա լինի սխալների ուղղում, նոր հատկություն, պլագին կամ թարգմանություն՝ յուրաքանչյուր ներդրում կարևոր է:</p>

---

## բովանդակության աղյուսակ

- [Վարքագիծ](#code-of-conduct)
- [Ինչպես հաղորդել սխալներ](#-how-to-report-bugs)
- [Ինչպես առաջարկել հատկություններ](#-how-to-suggest-features)
- [Ինչպես ներկայացնել պլագին](#-how-to-submit-a-plugin)
- [Ինչպես ներկայացնել Pull Request](#-how-to-submit-a-pull-request)
- [Թարգմանության ներդրումներ (254 լեզու)](#-translation-contributions-254-languages)
- [Ազգային կարգավորումներ](#-development-setup)

---

## Վարքագիծ

Մենք հանձնառու ենք ապահովել ողջունելի և ներառական փորձառություն բոլորի համար:

- **Հարգեք:** Բոլորին վերաբերվեք արժանապատվությամբ:
- **Կառուցողական եղեք:** Առաջարկեք օգտակար արձագանք, ոչ թե ոչնչացնող քննադատություն:
- **Ներառական եղեք:** Մենք աջակցում ենք 254 լեզուների և ողջունում ենք ներդրողներին աշխարհի յուրաքանչյուր երկրից:
- **Ոչ հալածանք:** Ոչ մի հանդուրժողականություն որևէ տեսակի խտրականության համար:

---

## 🐛 Ինչպես հաղորդել սխալներ

1. Գնացեք [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Սեղմեք **"Նոր հարց"**
3. Ընտրեք **"Սխալի հաղորդում"** ձևանմուշը
4. Ներառեք:
   - WIA SOOM տարբերակ (Կարգավորումներ → Մասին)
   - ՕՀ և տարբերակ (Windows/macOS/Linux)
   - Վերարտադրության քայլեր
   - Սպասվող և իրական վարք
   - Էկրանային պատկերներ կամ տերմինալի ելք, եթե հնարավոր է

---

## 💡 Ինչպես առաջարկել հատկություններ

1. Գնացեք [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Սեղմեք **"Նոր հարց"**
3. Ընտրեք **"Հատկության խնդրանք"** ձևանմուշը
4. Նկարագրեք:
   - Որ խնդիրն եք լուծում
   - Ինչպես եք պատկերացնում դրա աշխատանքը
   - Որոշ այլընտրանքներ, որոնք դուք դիտարկել եք

---

## 🔌 Ինչպես ներկայացնել պլագին

WIA SOOM-ը ունի հզոր պլագինների համակարգ՝ դուք կարող եք կառուցել ձեր սեփական պլագինը 5 րոպեում:

### Արագ սկիզբ
§§§CHUNK_SEPARATOR§§§
### Լրիվ ուղեցույց

Կարդացեք **[Պլագինների մշակողի ուղեցույցը](docs/PLUGIN_DEVELOPER_GUIDE.md)**՝
- Լրիվ API-ի հղում
- Աշխատող օրինակներ
- Քայլ առ քայլ դասընթացներ
- Լավագույն պրակտիկա և անվտանգության կանոններ

### Ներկայացրեք ձեր ��լագինը

1. Fork [Plugin Store](https://wiasoom.com)
2. Ավելացրեք ձեր պլագինը `plugins/{your-plugin-name}/`
3. Ներկայացրեք Pull Request
4. Արձագանքից հետո, ձեր պլագինը կհայտնվի Plugin Store-ում բոլոր օգտվողների համար:

---

## 🔀 Ինչպես ներկայացնել Pull Request

### Հիմնական հավելվածի համար (wia-soom)

1. Fork արեք ռեպոզիտորը
2. Ստեղծեք հատկության ճյուղ. `git checkout -b feat/my-feature`
3. Կատարեք ձեր փոփոխությունները
4. Փորձարկեք տեղական:
   ```bash
   ```
5. Կոմիտ արեք հստակ հաղորդագրությամբ:
   ```
   feat: ավելացնել մութ ռեժիմի переключатель կարգավորումների
   ```
6. Push արեք և բացեք PR `main`-ի դեմ

### Կոմիտի հաղորդագրության կանոնակարգ

| Նախաբառ | Օգտագործեք |
|-----------|-------------|
| `feat:`   | Նոր հատկություն |
| `fix:`    | Սխալի ուղղում |
| `docs:`   | Միայն փաստաթղթավորում |
| `refactor:` | Կոդի կառուցվածքի փոփոխություն (ոչ վարքային փոփոխություն) |
| `i18n:`   | Թարգմանության թարմացումներ |
| `plugin:` | Պլագիններին վերաբերող փոփոխություններ |

### PR-ի ստուգման ցուցակ

- [ ] Կոդը աշխատում է առանց սխալների
- [ ] Ոչ մի կոշտ կոդավորված տեքստ (օգտագործեք i18n բանալիներ)
- [ ] Ոչ մի `console.log` թողնված արտադրական կոդում
- [ ] Առկա թեստերը դեռ անցնում են

---

## 🌐 Թարգմանության ներդրումներ (254 լեզու)

WIA SOOM-ը աջակցում է **254 լեզուների** — Ամհարերենից մինչև Զուլու, ներառյալ Բրայլը և RTL լեզուները:

### Ինչպես է աշխատում թարգմանությունը

- Բազային լեզվի ֆայլը: `src/renderer/src/i18n/en.json`
- Բոլոր 254 լեզվի ֆայլերը գտնվում են նույն директորայում
- Թարգմանությունը կատարվում է `scripts/translate-patch.js` (GPT-4o-mini API)

### Ինչպես ներդրումներ անել թարգմանություններում

#### Տվյալ 1: Հատուկ թարգմանություն ուղղել

1. Գտեք լեզվի ֆայլը: `src/renderer/src/i18n/{lang-code}.json`
2. Ուղղեք սխալ թարգմանությունը
3. Ներկայացրեք PR փոփոխությամբ

#### Տվյալ 2: Ավելացնել բացակայող բանալիներ
§§§CHUNK_SEPARATOR§§§
#### Տվյալ 3: Արձագանքել մեքենայական թարգմանություններին

Մեր 254 լեզուներից շատերը մեքենայական թարգմանվել են: Բնական խոսողների վերանայումները անչափ արժեքավոր են:

1. Ընտրեք ձեր լեզվի ֆայլը
2. Արձագանքեք թարգմանություններին
3. Ուղղեք ցանկացած անհարմար կամ սխալ թարգմանություններ
4. Ներկայացրեք PR

### Լեզվի կոդեր

Մենք օգտագործում ենք ստանդարտ ISO 639-1 կոդեր (օրինակ՝ `ko`, `en`, `ja`, `ar`, `hi`) տարածաշրջանային տարբերակներով, որտեղ անհրաժեշտ է (օրինակ՝ `zh-CN`, `pt-BR`).

---

## 🛠 Ազգային կարգավորումներ

### Նախապայմաններ

- Node.js 18+
- npm 9+
- Git

### Կարգավորում
§§§CHUNK_SEPARATOR§§§
### Կառուցել
§§§CHUNK_SEPARATOR§§§
> Նշում: Դեֆոլտ 2GB կույտը բավարար չէ 254 լեզվի ֆայլերի + Monaco խմբաքանակի (~38MB ռենդեր) պատճառով:

### Նախագծի կառուցվածք
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Շնորհակալություն

Յուրաքանչյուր ներդրում բարելավում է WIA SOOM-ը ամբողջ աշխարհի մշակողների համար:

Թեև դուք ուղղում եք տառասխալ, թարգմանում եք տող, կառուցում եք պլագին կամ ավելացնում եք կարևոր ֆունկցիա — **դուք այս պատմության մի մասն եք:**

---

<p align="center"><em>Կառուցված ❤️ SmileStory Inc.-ի և աշխարհով մեկ ներդրողների կողմից:</em></p>
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
