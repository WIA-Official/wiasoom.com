<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ku Hlanganisa na WIA SOOM</h1>
<p align="center"><strong>Hi rhandza ku vona switlhavelo swa wena!</strong></p>
<p align="center">Kwhether i ku pfuxeta swihoxo, swihlanganiso leswa, plugin, kumbe ku humesa — ku hlanganisa kwalaho ku ni nkoka.</p>

---

## Tabelo ya Tinhla

- [Code of Conduct](#code-of-conduct)
- [Ku Rhumba Swihoxo](#-how-to-report-bugs)
- [Ku Kombisa Swihlanganiso](#-how-to-suggest-features)
- [Ku Rhumba Plugin](#-how-to-submit-a-plugin)
- [Ku Rhumba Pull Request](#-how-to-submit-a-pull-request)
- [Ku Hlanganisa Tinhla (254 Languages)](#-translation-contributions-254-languages)
- [Ku Hlela Nhlangano](#-development-setup)

---

## Code of Conduct

Hi tirha ku nyika vutlhari na ku katsa ku va na munhu un'wana ni un'wana.

- **Kuma ku hlonipha.** Hlamusela munhu un'wana ni un'wana hi ku hlonipha.
- **Kuma ku vuyeriwa.** Naka ku nyika switlhavelo leswi pfuna, a swi nga va swihoxo.
- **Kuma ku katsa.** Hi tshemba 254 tindzimi na hi amukela switlhavelo ku suka eka tiko rin'wana ni rin'wana emisaveni.
- **A ku na ku khoma.** Ku na na ku khoma ka swihoxo swa rihanyu.

---

## 🐛 Ku Rhumba Swihoxo

1. Endla ku ya [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tswakela **"New Issue"**
3. Khetha **"Bug Report"** template
4. Hlanganisa:
   - WIA SOOM version (Settings → About)
   - OS na version (Windows/macOS/Linux)
   - Tinhla ta ku endla
   - Ku langutela ku fana na ku endla
   - Swifaniso kumbe ku humelela ka terminal loko ku ri kona

---

## 💡 Ku Kombisa Swihlanganiso

1. Endla ku ya [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tswakela **"New Issue"**
3. Khetha **"Feature Request"** template
4. Hlanganisa:
   - Xihoxo lexi u xi xiximaka
   - Loko u langutela ku tirha
   - Nhlayo yin'wana leyi u yi langutela

---

## 🔌 Ku Rhumba Plugin

WIA SOOM i na mfumo wa plugin lowu kuatlaka — u nga endla plugin ya wena hi 5 minti.

### Ku Tanga Kakhulu
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ghidhi Leyi Hlawulekeke

Funda **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ku:
- Hlanganisa API yo helela
- Tinhla leti tirhaka
- Tihloho ta ku famba-famba
- Swihlawulekiso leswikulu na mivuyelo ya vutlhari

### Rhumba Plugin Ya Wena

1. Fork [Plugin Store](https://wiasoom.com)
2. Hlanganisa plugin ya wena ku `plugins/{your-plugin-name}/`
3. Rhumba Pull Request
4. Endzhaku ka ku hlolwa, plugin ya wena yi ta humelela ePlugin Store ku vaaki hinkwavo!

---

## 🔀 Ku Rhumba Pull Request

### Ku wia-soom

1. Fork repository
2. Endla branch ya swihlanganiso: `git checkout -b feat/my-feature`
3. Endla swihlawulekiso swa wena
4. Tswakela hi ku langutela:
   ```bash
   ```
5. Commit hi xifaniso lexi hlamarhaka:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push na ku vula PR ku `main`

### Xifaniso xa Xikombiso xa Commit

| Prefix | Use for |
|--------|---------|
| `feat:` | Swihlanganiso leswa |
| `fix:` | Ku pfuxeta swihoxo |
| `docs:` | Tinhla ta switlhavelo ntsena |
| `refactor:` | Ku hlela kodu (a ku na ku humelela) |
| `i18n:` | Ku hlanganisa swihlanganiso |
| `plugin:` | Swihlawulekiso leswi amanaka na plugin |

### PR Checklist

- [ ] Kodu yi tirha ngaphandle ka swihoxo
- [ ] A ku na swifaniso leswi hlanganisiweke (tirhisa i18n keys)
- [ ] A ku na `console.log` leyi lefti ku kodu ya production
- [ ] Tinhla leti existing ti ya emahlweni

---

## 🌐 Ku Hlanganisa Tinhla (254 Languages)

WIA SOOM i na **254 languages** — ku suka eka Amharic ku ya eka Zulu, ku katsa na Braille na tindzimi ta RTL.

### Loko Ku Hlanganisa Ku Tirha

- Fayela ya xihlangano: `src/renderer/src/i18n/en.json`
- Tindzimi ta 254 ti na ku hlanganisiwa e ndzeni ya xihlangano
- Ku hlanganisa ku endliwa hi `scripts/translate-patch.js` (GPT-4o-mini API)

### Ku Hlanganisa Tinhla

#### Nhlayo 1: Pfuxeta xihlanganiso lexi nga riki kahle

1. Fumana xihlangano: `src/renderer/src/i18n/{lang-code}.json`
2. Pfuxeta xihlanganiso lexi nga riki kahle
3. Rhumba PR na ku hlanganisa

#### Nhlayo 2: Hlanganisa swikombiso leswi nga riki kona
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Nhlayo 3: Hlawula ku hlanganisa ka masin

Switlhavelo swa hina swa 254 swi hlanganisiwile hi masin. Ku hlawula ka vahlayi va rixaka i swa nkoka swinene!

1. Hlawula xihlangano xa wena
2. Hlawula swihlanganiso
3. Pfuxeta swihlanganiso leswi nga riki kahle kumbe leswi nga riki kahle
4. Rhumba PR

### Tikhodi ta Tindzimi

Hi tirhisa tikhodi ta ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) na swihlanganiso swa ndzhaka loko swi ri kona (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Ku Hlela Nhlangano

### Swikombiso Swa Mpfuno

- Node.js 18+
- npm 9+
- Git

### Hlela
```bash
```
### Hlela
```bash
```
> Xikombiso: Ku na na 2GB heap ku nga pfumeliwa hi ku va na 254 language files + Monaco editor bundle (~38MB renderer).

### Mpfuno wa Projeke
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

Ndzavula yin'wana yi endla WIA SOOM yi antswisa ku va swikombiso swa vaendzi emisaveni hinkwayo.

Kambe u lava ku fixa typo, ku humesa xifaniso, ku endla plugin, kumbe ku engetela xifaniso lexikulu — **u ri xiphemu xa nkombiso lowu.**

---

<p align="center"><em>Ku bumbiwa hi ❤️ hi SmileStory Inc. na vaendzi va misava hinkwayo.</em></p>