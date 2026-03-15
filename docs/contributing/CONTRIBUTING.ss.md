<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kukhonza ku WIA SOOM</h1>
<p align="center"><strong>Silindzele iminikelo yakho!</strong></p>
<p align="center">Noma kungumphumela wokulungisa iphutha, isici esisha, i-plugin, noma ukuhumusha — yonke iminikelo ibalulekile.</p>

---

## Ithebula Lezinto

- [Umthetho Wokuziphatha](#code-of-conduct)
- [Indlela Yokubika Iziphuthu](#-how-to-report-bugs)
- [Indlela Yokuphakamisa Izici](#-how-to-suggest-features)
- [Indlela Yokufaka I-Plugin](#-how-to-submit-a-plugin)
- [Indlela Yokufaka Isicelo Sokuhlanganisa](#-how-to-submit-a-pull-request)
- [Iminikelo Yokuhumusha (254 Languages)](#-translation-contributions-254-languages)
- [Uhlelo Lokuthuthukisa](#-development-setup)

---

## Umthetho Wokuziphatha

Sizibophezele ekuhlinzekeni ngolwazi olwamukelekayo noluhlanganisayo kubo bonke.

- **Yiba nenhlonipho.** Phatha bonke abantu ngenhlonipho.
- **Yiba nesizotha.** Nikeza impendulo ewusizo, hhayi ukugxeka okuphambene.
- **Yiba nohlu.** Sisebenzisa izilimi eziyi-254 futhi samukela abanikeli bazo zonke izwe emhlabeni.
- **Akukho ukuhlukunyezwa.** Akukho tolerance yokucwaswa kwanoma yiluphi uhlobo.

---

## 🐛 Indlela Yokubika Iziphuthu

1. Iya ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chofoza **"New Issue"**
3. Khetha ithempulethi ye-**"Bug Report"**
4. Faka:
   - Ingxenye ye-WIA SOOM (Izilungiselelo → Mayelana)
   - OS kanye nenguqulo (Windows/macOS/Linux)
   - Izinyathelo zokuphinda
   - Ukuziphatha okulindelekile vs. okwenziwayo
   - Izithombe-skrini noma umphumela we-terminal uma kungenzeka

---

## 💡 Indlela Yokuphakamisa Izici

1. Iya ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chofoza **"New Issue"**
3. Khetha ithempulethi ye-**"Feature Request"**
4. Chaza:
   - Yini inkinga oyixazululayo
   - Ungayicabanga kanjani
   - Noma yiziphi ezinye izinketho ozicabangile

---

## 🔌 Indlela Yokufaka I-Plugin

I-WIA SOOM inohlelo lwe-plugin oluqinile — ungakha i-plugin yakho ezinsukwini ezi-5.

### Ukuqala Ngokushesha
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Umhlahlandlela Ophelele

Funda i-**[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ukuze uthole:
- I-referensi ye-API ephelele
- Izibonelo ezisebenzayo
- Izifundo ezinyathelweni
- Izindlela ezinhle nezimiso zokuphepha

### Faka I-Plugin Yakho

1. Fork [Plugin Store](https://wiasoom.com)
2. Faka i-plugin yakho ku-`plugins/{your-plugin-name}/`
3. Faka Isicelo Sokuhlanganisa
4. Ngemuva kokubuyekezwa, i-plugin yakho izovela ePlugin Store kubo bonke abasebenzisi!

---

## 🔀 Indlela Yokufaka Isicelo Sokuhlanganisa

### Ku-app eyinhloko (wia-soom)

1. Fork ithala
2. Dala igatsha lesici: `git checkout -b feat/my-feature`
3. Yenza izinguquko zakho
4. Hlola endaweni:
   ```bash
   ```
5. Faka isiqinisekiso esinemiyalezo ecacile:
   ```
   feat: engeza ukushintsha kwemodi emnyama kuzilungiselelo
   ```
6. Push futhi uvule i-PR ngokumelene ne-`main`

### Umthetho Wokufaka Isiqinisekiso

| I-Prefix | Sebenzisa ku |
|----------|--------------|
| `feat:`  | Isici esisha |
| `fix:`   | Ukulungiswa kwephutha |
| `docs:`  | Imibhalo kuphela |
| `refactor:` | Ukuhlelwa kwekhodi (akukho shintsho ekuziphatheni) |
| `i18n:`  | Ukuvuselelwa kokuhumusha |
| `plugin:` | Izinguquko ezihlobene ne-plugin |

### Uhlu Lokuhlola i-PR

- [ ] Ikhodi iyasebenza ngaphandle kweziphuthu
- [ ] Akukho zisho ezibhalwe ngokuqinile (sebenzisa ama-i18n keys)
- [ ] Akukho `console.log` esele kukhodi yokukhiqiza
- [ ] Izivivinyo ezikhona zisasebenza

---

## 🌐 Iminikelo Yokuhumusha (254 Languages)

I-WIA SOOM isekela **254 languages** — kusukela kwi-Amharic kuya kwi-Zulu, kuhlanganisa ne-Braille nezilimi ze-RTL.

### Indlela Yokuhumusha Isebenza

- Ifayela lesizinda: `src/renderer/src/i18n/en.json`
- Bonke amafayela ezilimi eziyi-254 akwi-directory efanayo
- Ukuhumusha kwenziwa nge-`scripts/translate-patch.js` (GPT-4o-mini API)

### Indlela Yokuhlinzeka Ngokuhumusha

#### Inketho 1: Lungisa ukuhumusha okuthile

1. Thola ifayela lezilimi: `src/renderer/src/i18n/{lang-code}.json`
2. Lungisa ukuhumusha okungalungile
3. Faka i-PR enoshintsho

#### Inketho 2: Engeza okhiye abalahlekile
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Inketho 3: Bheka ukuhumusha kwemishini

Iningi lezilimi zethu eziyi-254 zaku-humushiwe ngemishini. Ukuhlolwa kwabakhuluma abavela ezindaweni zendawo kubalulekile kakhulu!

1. Khetha ifayela lakho lezilimi
2. Bheka ukuhumusha
3. Lungisa noma yiziphi ukuhumusha okungalungile noma okungahambi kahle
4. Faka i-PR

### Amakhodi Ezilimi

Sisebenzisa amakhodi ajwayelekile e-ISO 639-1 (isb., `ko`, `en`, `ja`, `ar`, `hi`) ngezinhlobo zendawo lapho kudingeka (isb., `zh-CN`, `pt-BR`).

---

## 🛠 Uhlelo Lokuthuthukisa

### Izidingo

- Node.js 18+
- npm 9+
- Git

### Uhlelo
```bash
```
### Ukwakhiwa
```bash
```
> Qaphela: I-heap ejwayelekile ye-2GB ayanele ngenxa yamafayela ezilimi eziyi-254 + i-bundle ye-Monaco editor (~38MB renderer).

### Isakhiwo Sephrojekthi
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

Kukhona umnikelo wonke okwenza i-WIA SOOM ibe ngcono kubathuthukisi emhlabeni jikelele.

Noma ngabe ulungisa iphutha, uhumusha umusho, wakhe i-plugin, noma ungeza isici esikhulu — **ungumgibeli wale ndaba.**

---

<p align="center"><em>Yakhelwe ngothando ❤️ yi-SmileStory Inc. kanye nabathuthukisi emhlabeni jikelele.</em></p>