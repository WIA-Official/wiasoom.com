<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ukubamba iqhaza ku-WIA SOOM</h1>
<p align="center"><strong>Siyajabula ngeminikelo yakho!</strong></p>
<p align="center">Noma ngabe kuwukulungisa iphutha, isici esisha, ipulagi, noma ukuhumusha — yonke iminikelo ibalulekile.</p>

---

## Ithebula Lezinto

- [Ikhodi Yokuziphatha](#code-of-conduct)
- [Indlela Yokubika Iphutha](#-how-to-report-bugs)
- [Indlela Yokuphakamisa Izici](#-how-to-suggest-features)
- [Indlela Yokufaka Ipulagi](#-how-to-submit-a-plugin)
- [Indlela Yokufaka Isicelo Sokuhlanganisa](#-how-to-submit-a-pull-request)
- [Iminikelo Yokuhumusha (254 Izilimi)](#-translation-contributions-254-languages)
- [Ukwakhiwa Kwezinhlelo](#-development-setup)

---

## Ikhodi Yokuziphatha

Sizibophezele ekunikezeni isipiliyoni esamukelekayo nesihlanganisayo kubo bonke.

- **Hlonipha.** Phatha wonke umuntu ngedumela.
- **Yiba nomthelela.** Nikeza impendulo ewusizo, hhayi ukugxeka okukhubazayo.
- **Yiba nohlu.** Sihlinzeka ngama-254 ezilimi futhi samukela abahlanganyeli bazo zonke izizwe zomhlaba.
- **Akukho ukuhlasela.** Akukho ukubekezelelwa kokucwaswa kwanoma yiluphi uhlobo.

---

## 🐛 Indlela Yokubika Iphutha

1. Iya ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chofoza **"New Issue"**
3. Khetha ithempulethi **"Bug Report"**
4. Faka:
   - Uhlobo lwe-WIA SOOM (Izilungiselelo → Mayelana)
   - OS nohlobo (Windows/macOS/Linux)
   - Izinyathelo zokuphinda
   - Ukuziphatha okulindelekile vs. okwenziwayo
   - Izithombe-skrini noma umphumela we-terminal uma kungenzeka

---

## 💡 Indlela Yokuphakamisa Izici

1. Iya ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chofoza **"New Issue"**
3. Khetha ithempulethi **"Feature Request"**
4. Chaza:
   - Yini inkinga oyixazululayo
   - Ungayicabanga kanjani isebenza
   - Noma yiziphi ezinye izinketho ozicabangile

---

## 🔌 Indlela Yokufaka Ipulagi

I-WIA SOOM inohlelo lwepulagi oluqinile — ungakha ipulagi yakho emaminithini ama-5.

### Ukuqala Ngokushesha
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Umhlahlandlela Ophelele

Funda **[Umhlahlandlela Womkhiqizi WePulagi](docs/PLUGIN_DEVELOPER_GUIDE.md)** ukuze:
- Ube ne-referensi ye-API ephelele
- Ube nezibonelo ezisebenzayo
- Ube nezifundo ezinyathelisayo
- Ube nezindlela ezinhle nezimiso zokuphepha

### Faka Ipulagi Yakho

1. Fork [Plugin Store](https://wiasoom.com)
2. Faka ipulagi yakho ku-`plugins/{your-plugin-name}/`
3. Faka Isicelo Sokuhlanganisa
4. Ngemuva kokubuyekezwa, ipulagi yakho izovela ePulagi Store kubo bonke abasebenzisi!

---

## 🔀 Indlela Yokufaka Isicelo Sokuhlanganisa

### Ku-app eyinhloko (wia-soom)

1. Fork i-repository
2. Dala igatsha lesici: `git checkout -b feat/my-feature`
3. Yenza izinguquko zakho
4. Hlola endaweni:
   ```bash
   ```
5. Commit ngomyalezo ocacile:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push futhi uvule i-PR ngokumelene `main`

### Umthetho Womyalezo We-Commit

| I-Prefix | Sebenzisa ku |
|----------|--------------|
| `feat:`  | Isici esisha |
| `fix:`   | Ukulungisa iphutha |
| `docs:`  | Imibhalo kuphela |
| `refactor:` | Ukuhlelwa kwekhodi (akukho shintsho kokuziphatha) |
| `i18n:`  | Ukuvuselelwa kokuhumusha |
| `plugin:` | Izinguquko ezihlobene nepulagi |

### Uhlu Lokuhlola lwe-PR

- [ ] Ikhodi iyasebenza ngaphandle kwephutha
- [ ] Akukho zintambo ezibhalwe ngokuqinile (sebenzisa ama-i18n keys)
- [ ] Akukho `console.log` esalayo kukhodi yokukhiqiza
- [ ] Izivivinyo ezikhona zisasebenza

---

## 🌐 Iminikelo Yokuhumusha (254 Izilimi)

I-WIA SOOM isekela **254 izilimi** — kusukela ku-Amharic kuya ku-Zulu, kuhlanganisa neBraille nezilimi ze-RTL.

### Indlela Yokuhumusha Isebenza

- Ifayela lesizinda: `src/renderer/src/i18n/en.json`
- Bonke amafayela ezilimi angu-254 akwi-directory efanayo
- Ukuhumusha kwenziwa nge-`scripts/translate-patch.js` (GPT-4o-mini API)

### Indlela Yokubamba Iminikelo Yokuhumusha

#### Inketho 1: Lungisa ukuhumusha okuthile

1. Thola ifayela lezilimi: `src/renderer/src/i18n/{lang-code}.json`
2. Lungisa ukuhumusha okungalungile
3. Faka i-PR enoshintsho

#### Inketho 2: Faka okhiye abalahlekile
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Inketho 3: Bheka ukuhumusha kwemishini

Amanye ama-254 ezilimi zethu ahumushwe ngemishini. Ukuhlolwa kwabakhuluma bokuqala kubalulekile kakhulu!

1. Khetha ifayela lakho lezilimi
2. Bheka ukuhumusha
3. Lungisa noma yiziphi ukuhumusha okungahambi kahle noma okungalungile
4. Faka i-PR

### Amakhodi Ezilimi

Sisebenzisa amakhodi ajwayelekile e-ISO 639-1 (isb., `ko`, `en`, `ja`, `ar`, `hi`) ngezinguquko zomphakathi lapho kudingeka (isb., `zh-CN`, `pt-BR`).

---

## 🛠 Ukwakhiwa Kwezinhlelo

### Izidingo

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Yakha
```bash
```
> Qaphela: I-heap ejwayelekile engu-2GB ayanele ngenxa yamafayela ezilimi angu-254 + ibhande leMonaco (~38MB renderer).

### Uhlaka Lwephrojekthi
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

## 🙏 Ngiyabonga

Yonke iminikelo iyenza i-WIA SOOM ibe ngcono kubathuthukisi emhlabeni jikelele.

Noma ngabe ulungisa iphutha, uhumusha umusho, wakha i-plugin, noma wengeza isici esikhulu — **ungowesigameko lesi.**

---

<p align="center"><em>Yakhelwe nge ❤️ ngu-SmileStory Inc. kanye nabahlanganyeli emhlabeni jikelele.</em></p>