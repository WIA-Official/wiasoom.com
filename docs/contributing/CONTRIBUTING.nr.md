<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ukubamba iqhaza ku-WIA SOOM</h1>
<p align="center"><strong>Silangazelela ukubamba kwakho iqhaza!</strong></p>
<p align="center">Noma kungukulungiswa kwephutha, isici esisha, ipulagi, noma ukuhumusha — yonke iminikelo ibalulekile.</p>

---

## Ithebula Lezinto

- [Umthetho Wokuziphatha](#code-of-conduct)
- [Indlela Yokubika Iphutha](#-how-to-report-bugs)
- [Indlela Yokuphakamisa Izici](#-how-to-suggest-features)
- [Indlela Yokufaka Ipulagi](#-how-to-submit-a-plugin)
- [Indlela Yokufaka Isicelo Sokuhlanganisa](#-how-to-submit-a-pull-request)
- [Iminikelo Yokuhumusha (254 Izilimi)](#-translation-contributions-254-languages)
- [Uhlelo Lokuthuthukisa](#-development-setup)

---

## Umthetho Wokuziphatha

Sizibophezele ekuhlinzekeni ngolwazi olwamukelekayo noluhlanganisayo kubo bonke.

- **Phatha abanye ngenhlonipho.** Phatha wonke umuntu ngedignity.
- **Yiba nomthelela omuhle.** Nikeza impendulo ewusizo, hhayi ukugxeka okuphambene.
- **Yiba nohlu.** Sisebenzisa izilimi eziyi-254 futhi samukela ababambiqhaza abavela kuzo zonke izwe emhlabeni.
- **Akukho ukuhlasela.** Akukho ukubekezelelwa kokucwasa kwanoma yiluphi uhlobo.

---

## 🐛 Indlela Yokubika Iphutha

1. Vakashela ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chofoza **"New Issue"**
3. Khetha ithempulethi ye **"Bug Report"**
4. Faka:
   - I-WIA SOOM version (Settings → About)
   - OS kanye ne version (Windows/macOS/Linux)
   - Izinyathelo zokuphinda
   - Ukuziphatha okulindelekile vs. okwenziwayo
   - Izithombe noma umphumela we-terminal uma kungenzeka

---

## 💡 Indlela Yokuphakamisa Izici

1. Vakashela ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Chofoza **"New Issue"**
3. Khetha ithempulethi ye **"Feature Request"**
4. Chaza:
   - Yisiphi isizathu osixazululayo
   - Ucabanga kanjani ukuthi kuzosebenza
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

Funda i-**[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ukuze uthole:
- I-referensi ye-API ephelele
- Izibonelo ezisebenzayo
- Izifundo ezinyathelweni
- Izindlela ezinhle nezimiso zokuphepha

### Faka Ipulagi Yakho

1. Fork [Plugin Store](https://wiasoom.com)
2. Faka ipulagi yakho ku-`plugins/{your-plugin-name}/`
3. Faka Isicelo Sokuhlanganisa
4. Ngemuva kokubuyekezwa, ipulagi yakho izovela ePlugin Store kubo bonke abasebenzisi!

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
6. Push bese uvula i-PR ngokumelene ne-`main`

### Umthetho Wokubhalisa Imiyalezo

| I-Prefix | Sebenzisa ku |
|----------|--------------|
| `feat:`  | Isici esisha |
| `fix:`   | Ukulungiswa kwephutha |
| `docs:`  | Imibhalo kuphela |
| `refactor:` | Ukuhlelwa kwekhodi (akukho shintsho kokuziphatha) |
| `i18n:`  | Izibuyekezo zokuhumusha |
| `plugin:` | Izinguquko ezihlobene ne-plugin |

### Uhlu Lokuhlola i-PR

- [ ] Ikhodi iyasebenza ngaphandle kwamaphutha
- [ ] Akukho zintambo ezibhalwe ngokuqinile (sebenzisa ama-i18n keys)
- [ ] Akukho `console.log` esalayo kukhodi yokukhiqiza
- [ ] Izivivinyo ezikhona zisaphumelela

---

## 🌐 Iminikelo Yokuhumusha (254 Izilimi)

I-WIA SOOM isekela **254 izilimi** — kusukela ku-Amharic kuya ku-Zulu, kuhlanganisa neBraille nezilimi ze-RTL.

### Indlela Yokuhumusha Isebenza

- Ifayela lesizinda: `src/renderer/src/i18n/en.json`
- Zonke izilimi eziyi-254 zikhona endaweni efanayo
- Ukuhumusha kwenziwa nge-`scripts/translate-patch.js` (GPT-4o-mini API)

### Indlela Yokubamba Iqhaza Ekuthumeleni Ukuhumusha

#### Inketho 1: Lungisa ukuhumusha okuthile

1. Thola ifayela lesiZulu: `src/renderer/src/i18n/{lang-code}.json`
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

Eziningi zezilimi zethu eziyi-254 zaguqulwa ngemishini. Ukuhlola kwabakhuluma isiZulu kuyigugu kakhulu!

1. Khetha ifayela lesiZulu
2. Bheka ukuhumusha
3. Lungisa noma yiziphi ukuhumusha okungahambi kahle noma okungalungile
4. Faka i-PR

### Amakhodi Ezilimi

Sisebenzisa amakhodi ajwayelekile e-ISO 639-1 (isb., `ko`, `en`, `ja`, `ar`, `hi`) ngezinhlobo zomphakathi lapho kudingeka (isb., `zh-CN`, `pt-BR`).

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
> Qaphela: I-heap ejwayelekile ye-2GB ayanele ngenxa yamafayela ezilimi eziyi-254 + iMonaco editor bundle (~38MB renderer).

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

Konke okukhuluma ku-WIA SOOM kukwenza kube ngcono kubathuthukisi emhlabeni jikelele.

Noma ulungisa iphutha, uhumusha umusho, wakhe i-plugin, noma ungeza isici esikhulu — **ungumuntu oyingxenye yalesi sikhathi.**

---

<p align="center"><em>Yakhelwe ngothando ❤️ ngu-SmileStory Inc. kanye nabathuthukisi emhlabeni jikelele.</em></p>