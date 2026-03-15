<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ukunceda kwi WIA SOOM</h1>
<p align="center"><strong>Singathanda ukunceda kwakho!</strong></p>
<p align="center">Nokuba kukulungiswa kwesiphene, umphumo omtsha, ipulagi, okanye ukuhunyushwa — yonke iminikelo ibalulekile.</p>

---

## Uluhlu lweZihloko

- [Umgaqo-nkqubo Wokuziphatha](#code-of-conduct)
- [Indlela Yokubika Iziphene](#-how-to-report-bugs)
- [Indlela Yokuphakamisa Iimpawu](#-how-to-suggest-features)
- [Indlela Yokuthumela Ipulagi](#-how-to-submit-a-plugin)
- [Indlela Yokuthumela Isicelo sePulani](#-how-to-submit-a-pull-request)
- [Iminikelo Yokuhunyushwa (254 Iilwimi)](#-translation-contributions-254-languages)
- [Useto Lokuphuhlisa](#-development-setup)

---

## Umgaqo-nkqubo Wokuziphatha

Sizibophezele ekuboneleleni ngexperience emnandi nelungileyo kubo bonke.

- **Bahloniphe.** Phatha wonke umntu ngoxolo.
- **Bahloniphe.** Nikeza impendulo encedisayo, hayi ukugxeka okonakalisayo.
- **Bahloniphe.** Sisebenzisa iilwimi eziyi-254 kwaye samkela abaninzi abavela kwiilizwe zonke zeMhlaba.
- **Akukho ukuxhatshazwa.** Akukho tolerance yokwahlukana kwanyani.

---

## 🐛 Indlela Yokubika Iziphene

1. Iya kwi [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cofa **"New Issue"**
3. Khetha umphakathi **"Bug Report"**
4. Faka:
   - I-WIA SOOM version (Settings → About)
   - OS kunye neversion (Windows/macOS/Linux)
   - Iinyathelo zokuphinda
   - Ukulindela vs. ukuziphatha okwenene
   - Iiscreenshot okanye umphumo womboniso ukuba kunokwenzeka

---

## 💡 Indlela Yokuphakamisa Iimpawu

1. Iya kwi [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cofa **"New Issue"**
3. Khetha umphakathi **"Feature Request"**
4. Chaza:
   - Yintoni ingxaki oyixazululayo
   - Ucinga ukuba izakusebenza njani
   - Noma yiziphi ezinye iindlela ozijolise kuzo

---

## 🔌 Indlela Yokuthumela Ipulagi

I-WIA SOOM inenkqubo yeplagi enamandla — ungakha ipulagi yakho kwiiyure ezi-5.

### Qalisa Ngokukhawuleza
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Umhlahlandlela Opheleleyo

Funda i-**[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ukuze:
- Ube ne-API epheleleyo
- Imizekelo esebenzayo
- Iitutorials ezinyathelisayo
- Iindlela ezilungileyo kunye nemigaqo yokhuseleko

### Thumela Ipulagi Yakho

1. Fork [Plugin Store](https://wiasoom.com)
2. Faka ipulagi yakho kwi-`plugins/{your-plugin-name}/`
3. Thumela Isicelo sePulani
4. Emva kokuhlola, ipulagi yakho ibonakala kwiPlugin Store kubo bonke abasebenzisi!

---

## 🔀 Indlela Yokuthumela Isicelo sePulani

### Kwisicelo esikhulu (wia-soom)

1. Fork i-repository
2. Yenza igatya lempumelelo: `git checkout -b feat/my-feature`
3. Yenza utshintsho lwakho
4. Hlola kwiindawo:
   ```bash
   ```
5. Yenza umgca ophumelelayo:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push kwaye uvule i-PR ngokuchasene ne-`main`

### Umgaqo-nkqubo Womlayezo Wokubhalisa

| I-Prefix | Sebenzisa kwi |
|-----------|--------------|
| `feat:`   | Umsebenzi omtsha |
| `fix:`    | Ukulungiswa kwesiphene |
| `docs:`   | Idokhumenti kuphela |
| `refactor:` | Ukuphuculwa kwekhodi (akukho tshintsho kumsebenzi) |
| `i18n:`   | Iimpawu zokuhunyushwa |
| `plugin:` | Utshintsho oluhambelana neplagi |

### Uluhlu lwe-PR

- [ ] Ikhodi iyasebenza ngaphandle kweziphene
- [ ] Akukho migaqo ibhalwe ngqo (sebenzisa i-i18n keys)
- [ ] Akukho `console.log` esalayo kwiikhodi zokwenyani
- [ ] Iivavanyo ezikhoyo zihlala zisebenza

---

## 🌐 Iminikelo Yokuhunyushwa (254 Iilwimi)

I-WIA SOOM ixhasa **254 iilwimi** — ukusuka kwi-Amharic ukuya kwiZulu, kubandakanywa neBraille kunye neelwimi ze-RTL.

### Indlela Yokuhunyushwa Isebenza

- Ifayile yolwimi eyisiseko: `src/renderer/src/i18n/en.json`
- Zonke iifayile zeelwimi eziyi-254 zikwisikhumbuzo esifanayo
- Ukuhunyushwa kwenziwa nge-`scripts/translate-patch.js` (GPT-4o-mini API)

### Indlela Yokunceda Ukuhunyushwa

#### Ukhetho 1: Lungisa ukuhunyushwa okuthile

1. Fumana ifayile yolwimi: `src/renderer/src/i18n/{lang-code}.json`
2. Lungisa ukuhunyushwa okungachanekanga
3. Thumela i-PR enolu shintsho

#### Ukhetho 2: Faka iikhi zeziphene
§§§CHUNK_SEPARATOR§§§
#### Ukhetho 3: Jonga ukuhunyushwa kwemishini

Iilwimi zethu eziyi-254 zininzi ezihunyushwe ngemishini. Ukujonga kwabantu abaninzi kubalulekile kakhulu!

1. Khetha ifayile yolwimi yakho
2. Jonga ukuhunyushwa
3. Lungisa nayiphi na ukuhunyushwa okungafanelekanga okanye okungachanekanga
4. Thumela i-PR

### Iikhowudi zeLwimi

Sisebenzisa iikhowudi ezisemthethweni ze-ISO 639-1 (umzekelo, `ko`, `en`, `ja`, `ar`, `hi`) kunye neenguqulelo zendawo apho kufuneka (umzekelo, `zh-CN`, `pt-BR`).

---

## 🛠 Useto Lokuphuhlisa

### Izinto ezifunekayo

- Node.js 18+
- npm 9+
- Git

### Useto
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
### Yakha
```bash
```
> Qaphela: I-heap ye-2GB engama-default ayanele ngenxa yeefayile zeelwimi eziyi-254 + iMonaco editor bundle (~38MB renderer).

### Uhlaka lweProjekthi
```bash
```
---

## 🙏 Enkosi

Yonke iminikelo iyenza i-WIA SOOM ibe ngcono kubaphuhlisi kwihlabathi jikelele.

Nokuba ulungisa impazamo, uguqulela umgca, wakhe i-plugin, okanye wongeze umphumo omkhulu — **ungumgibeli kule ndaba.**

---

<p align="center"><em>Yakhelwe ngothando ❤️ nguSmileStory Inc. kunye nabaphuhlisi kwihlabathi jikelele.</em></p>
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
