<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Go tshegetsa go WIA SOOM</h1>
<p align="center"><strong>Re a go rata go amogela ditlhopho tsa gago!</strong></p>
<p align="center">Le fa e le go lokisa diphoso, se se amanang le mekgwa e mecha, plugin, kgotsa phetolelo — ngwaga le ngwaga go tshegetsa go botlhokwa.</p>

---

## Tabela ya Ditlhopho

- [Molao wa Boitshwaro](#code-of-conduct)
- [Mokgwa wa go Rapela Diphoso](#-how-to-report-bugs)
- [Mokgwa wa go Kgetha Mekgwa e Mecha](#-how-to-suggest-features)
- [Mokgwa wa go Tsweletsa Plugin](#-how-to-submit-a-plugin)
- [Mokgwa wa go Tsweletsa Pull Request](#-how-to-submit-a-pull-request)
- [Ditlhopho tsa Phetolelo (254 Languages)](#-translation-contributions-254-languages)
- [Tlhopho ya Ntshetsopele](#-development-setup)

---

## Molao wa Boitshwaro

Re ikemiseditse go fa boexperience bo amanang le go amogela le go akaretsa go bohle.

- **Bua ka tlotlo.** Tlhomamisa boitshwaro mo go bohle.
- **Bua ka go thusa.** Fa diphetho tse di thusang, eseng go kgotsofatsa.
- **Bua ka go akaretsa.** Re tshegetsa dipuo tse 254 le go amogela ba tshegetsa go tswa mo dinageng tsotlhe mo Lefatshe.
- **Aowa go tsietsa.** Go na le borai go kgethegile ka ga go kgethegile ka mofuta ofe.

---

## 🐛 Mokgwa wa go Rapela Diphoso

1. Ya go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikanya **"New Issue"**
3. Kgetha template ya **"Bug Report"**
4. Akaretsa:
   - WIA SOOM version (Settings → About)
   - OS le version (Windows/macOS/Linux)
   - Mehato ya go boela
   - Boitshwaro bo amanang le se o se lebeletseng le se se amanang le se se diragalang
   - Dithoko kgotsa diphetho tsa terminal fa go kgonega

---

## 💡 Mokgwa wa go Kgetha Mekgwa e Mecha

1. Ya go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikanya **"New Issue"**
3. Kgetha template ya **"Feature Request"**
4. Hlalosa:
   - Bothata jo bo amanang le se o se rarabololeng
   - Mokgwa oo o o akanyang o tla dirisiwa
   - Diphetho dingwe tse o di akanyeditse

---

## 🔌 Mokgwa wa go Tsweletsa Plugin

WIA SOOM e na le tsamaiso e matla ya plugin — o ka aga plugin ya gago ka metsotso e 5.

### Qalong ya Borai
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tlhopho e Etelelwang

Bala **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** go:
- Tshedimosetso ya API e e feletseng
- Mehlala e amanang le tiriso
- Dithuto ka mehato
- Melemo e e amanang le ditlhopho le melawana ya tshegetso

### Tsweletsa Plugin ya gago

1. Fork [Plugin Store](https://wiasoom.com)
2. Tsweletsa plugin ya gago mo `plugins/{your-plugin-name}/`
3. Tsweletsa Pull Request
4. Morago ga go sekaseka, plugin ya gago e tla bonala mo Plugin Store go basebelisi ba botlhe!

---

## 🔀 Mokgwa wa go Tsweletsa Pull Request

### Go app e kgolo (wia-soom)

1. Fork repository
2. Bua feature branch: `git checkout -b feat/my-feature`
3. Dirisa diphetho tsa gago
4. Dira ditlhopho mo lefelong:
   ```bash
   ```
5. Commit ka molaetsa o o tlhaloganyang:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push le go bula PR kgatlhanong le `main`

### Melao ya Molaetsa wa Commit

| Prefix | Sebelisa go |
|--------|---------|
| `feat:` | Mekgwa e mecha |
| `fix:` | Go lokisa diphoso |
| `docs:` | Dikhomphutha fela |
| `refactor:` | Go fetola khoutu (go se fetoge boitshwaro) |
| `i18n:` | Ditlhopho tse di amanang le phetolelo |
| `plugin:` | Diphetho tse di amanang le plugin |

### PR Checklist

- [ ] Khoutu e a tsamaya ntle le diphoso
- [ ] Aowa mekgwa e amanang le diphetho (sebedisa i18n keys)
- [ ] Aowa `console.log` e siame mo khoutong ya tlhahiso
- [ ] Ditshekatsheko tse di teng di ntse di tswelela

---

## 🌐 Ditlhopho tsa Phetolelo (254 Languages)

WIA SOOM e tshegetsa **254 languages** — go tswa mo Amharic go ya mo Zulu, go akaretsa le Braille le dipuo tse di amanang le RTL.

### Mokgwa wa Phetolelo

- Faele ya puo ya motheo: `src/renderer/src/i18n/en.json`
- Diphuthegong tsotlhe tse di amanang le dipuo di mo lefelong le le kgethegileng
- Phetolelo e dirwa ka `scripts/translate-patch.js` (GPT-4o-mini API)

### Mokgwa wa go Tshegetsa Phetolelo

#### Kgetho ya 1: Lokisa phetolelo e e amanang

1. Fumana faele ya puo: `src/renderer/src/i18n/{lang-code}.json`
2. Lokisa phetolelo e e sa siamang
3. Tsweletsa PR ka phetolelo

#### Kgetho ya 2: Tsweletsa mekgwa e e sa amanang
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kgetho ya 3: Sekaseka diphetho tse di amanang le mechine

Go na le dipuo tse 254 tse di amanang le phetolelo ya mechine. Ditshegetso tse di amanang le borai di botlhokwa go gaisa!

1. Kgetha faele ya gago ya puo
2. Sekaseka diphetho
3. Lokisa diphetho tse di amanang le go sa siama
4. Tsweletsa PR

### Melemo ya Dipuo

Re dirisa mekgwa ya ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ka mekgwa ya kgaolo fa go tlhokega (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Tlhopho ya Ntshetsopele

### Ditshegetso

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Ageng
```bash
```
> Tlhokomeliso: The default 2GB heap e sitwa go lekana ka ntlha ya faele tse 254 tsa dipuo + Monaco editor bundle (~38MB renderer).

### Tshedimosetso ya Porojeke
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

## 🙏 Ke A Leboga

Tlhopho e nngwe le e nngwe e dira WIA SOOM e betere go bahlahisi mo lefatsheng ka bophara.

A o lokisa phoso, o fetolela molaetsa, o aga plugin, kgotsa o oketsa se se kgethegileng — **o a nna karolo ya pale eno.**

---

<p align="center"><em>Go agiwa ka ❤️ ke SmileStory Inc. le baokamedi mo lefatsheng ka bophara.</em></p>