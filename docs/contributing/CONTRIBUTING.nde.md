<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ukubamba iqhaza ku-WIA SOOM</h1>
<p align="center"><strong>Siyajabula ngeminikelo yakho!</strong></p>
<p align="center">Noma ngabe kuyisixazululo sebug, isici esisha, ipulagi, noma ukuhumusha — yonke iminikelo ibalulekile.</p>

---

## Ithebula Lezinto

- [Ikhodi Yokuziphatha](#code-of-conduct)
- [Indlela Yokubika Izigameko](#-how-to-report-bugs)
- [Indlela Yokuphakamisa Izici](#-how-to-suggest-features)
- [Indlela Yokufaka Ipulagi](#-how-to-submit-a-plugin)
- [Indlela Yokufaka Isicelo Sokuhlanganisa](#-how-to-submit-a-pull-request)
- [Iminikelo Yokuhumusha (254 Izilimi)](#-translation-contributions-254-languages)
- [Uhlelo Lokuthuthukisa](#-development-setup)

---

## Ikhodi Yokuziphatha

Sizibophezele ekuhlinzekeni ngolwazi olwamukelekayo noluhlanganisayo kubo bonke.

- **Hlonipha.** Phatha bonke ngedumela.
- **Yiba nesizotha.** Nikeza impendulo ewusizo, hhayi ukugxeka okonakalisa.
- **Yiba nohlu.** Sisebenzisa izilimi ezingu-254 futhi samukela abahlanganyeli bazo zonke izizwe emhlabeni.
- **Akukho ukuhlukumeza.** Akukho ukubekezelela ukuhlukumeza kwanoma yiluphi uhlobo.

---

## 🐛 Indlela Yokubika Izigameko

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
   - Yisiphi isizathu osixazululayo
   - Ungakubona kanjani ukusebenza
   - Noma yiziphi ezinye izinketho ozicabangile

---

## 🔌 Indlela Yokufaka Ipulagi

I-WIA SOOM inohlelo lwepulagi oluqinile — ungakha ipulagi yakho emaminithini ama-5.

### Qala Ngokushesha
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Umhlahlandlela Ophelele

Funda **[Umhlahlandlela Wokuthuthukisi Ipulagi](docs/PLUGIN_DEVELOPER_GUIDE.md)** ukuze uthole:
- Isikhumbuzo se-API esiphelele
- Izibonelo ezisebenzayo
- Izifundo ezinyathelisiwe
- Izindlela ezinhle nezimiso zokuphepha

### Faka Ipulagi Yakho

1. Fork [Plugin Store](https://wiasoom.com)
2. Faka ipulagi yakho ku-`plugins/{your-plugin-name}/`
3. Faka Isicelo Sokuhlanganisa
4. Ngemva kokubuyekezwa, ipulagi yakho izovela ePulagi Store kubo bonke abasebenzisi!

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
6. Push bese uvula i-PR ngokumelene `main`

### Umthetho Womyalezo Wokuhlanganisa

| I-Preffix | Sebenzisa ku- |
|-----------|---------------|
| `feat:`   | Isici esisha  |
| `fix:`    | Ukulungiswa kwebug |
| `docs:`   | Imibhalo kuphela |
| `refactor:` | Ukuhlelwa kwekhodi (akukho shintsho kokuziphatha) |
| `i18n:`   | Ukuvuselelwa kokuhumusha |
| `plugin:` | Izinguquko ezihlobene nepulagi |

### Uhlu Lokuhlola lwe-PR

- [ ] Ikhodi iyasebenza ngaphandle kweziphazamiso
- [ ] Akukho zisho ezibhalwe ngokuqinile (sebenzisa ama-i18n keys)
- [ ] Akukho `console.log` esalayo kukhodi yokukhiqiza
- [ ] Izivivinyo ezikhona zisasebenza

---

## 🌐 Iminikelo Yokuhumusha (254 Izilimi)

I-WIA SOOM isekela **izilimi ezingu-254** — kusukela ku-Amharic kuya ku-Zulu, kuhlanganisa neBraille nezilimi ze-RTL.

### Indlela Yokuhumusha Isebenza

- Ifayela lesizinda: `src/renderer/src/i18n/en.json`
- Wonke amafayela ezilimi ezingu-254 akwi-directory efanayo
- Ukuhumusha kwenziwa nge-`scripts/translate-patch.js` (GPT-4o-mini API)

### Indlela Yokubamba Iminikelo Yokuhumusha

#### Inketho 1: Lungisa ukuhumusha okuthile

1. Thola ifayela lesiZulu: `src/renderer/src/i18n/{lang-code}.json`
2. Lungisa ukuhumusha okungalungile
3. Faka i-PR enoshintsho

#### Inketho 2: Engeza okhiye abakhoyo
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Inketho 3: Bheka ukuhumusha kwemishini

Iningi lezilimi zethu ezingu-254 zaguqulwa ngemishini. Ukuhlola kwabakhuluma ulimi lwesiqhelo kubalulekile kakhulu!

1. Khetha ifayela lakho lesiZulu
2. Bheka ukuhumusha
3. Lungisa noma yiziphi ukuhumusha okungafanele noma okungalungile
4. Faka i-PR

### Amakhodi Ezilimi

Sisebenzisa amakhodi ajwayelekile e-ISO 639-1 (isb., `ko`, `en`, `ja`, `ar`, `hi`) ngezinhlobo zendawo lapho kudingeka (isb., `zh-CN`, `pt-BR`).

---

## 🛠 Uhlelo Lokuthuthukisa

### Izidingo

- Node.js 18+
- npm 9+
- Git

### Hlela
```bash
```
### Yakha
```bash
```
> Qaphela: I-heap ejwayelekile ye-2GB ayanele ngenxa yamafayela ezilimi ezingu-254 + iMonaco editor bundle (~38MB renderer).

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

Konke okuthunyelwe kwenza i-WIA SOOM ibe ngcono kubathuthukisi emhlabeni jikelele.

Noma uthola iphutha, uhunyushwa umusho, wakhe i-plugin, noma ungeza isici esikhulu — **ungumgibeli walesi sihloko.**

---

<p align="center"><em>Yakhelwe nge ❤️ ngu-SmileStory Inc. kanye nabathuthukisi emhlabeni jikelele.</em></p>