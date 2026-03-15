<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Go tsenya go WIA SOOM</h1>
<p align="center"><strong>Re rata ditlhopho tsa gago!</strong></p>
<p align="center">Le fa e le go lokisa phoso, se se amanang le ditlhopho, plugin, kgotsa phetolelo — ngwaga le ngwaga e amanang le go tsenya go botlhokwa.</p>

---

## Tlhopho ya Ditlhopho

- [Molao wa Boitshwaro](#code-of-conduct)
- [Mokgwa wa go Rapela Diphoso](#-how-to-report-bugs)
- [Mokgwa wa go Kgetha Ditlhopho](#-how-to-suggest-features)
- [Mokgwa wa go Romela Plugin](#-how-to-submit-a-plugin)
- [Mokgwa wa go Romela Pull Request](#-how-to-submit-a-pull-request)
- [Ditlhopho tsa Phetolelo (254 Languages)](#-translation-contributions-254-languages)
- [Tlhopho ya Tlhahiso](#-development-setup)

---

## Molao wa Boitshwaro

Re ikanyega go fa boexperience bo amanang le go amogela le go akaretsa mongwe le mongwe.

- **Nna le tlotlo.** Tlhopho mongwe le mongwe ka borai.
- **Nna le boikgopolelo.** Fa maikutlo a a thusang, eseng a a senyang.
- **Nna le go akaretsa.** Re tshegetsa dipuo tse 254 le go amogela ba tsenngang go tswa mo dinageng tsotlhe mo Lefatshe.
- **A go na le borai.** Go na le borai ka ga go kgethegile go amanang le mofuta ofe kapa ofe.

---

## 🐛 Mokgwa wa go Rapela Diphoso

1. Ya go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Khetha **"New Issue"**
3. Khetha **"Bug Report"** template
4. Akaretsa:
   - WIA SOOM version (Settings → About)
   - OS le version (Windows/macOS/Linux)
   - Melemo ya go boela morago
   - Go leboga go amanang le se se amanang le se se amanang
   - Ditshwantsho kgotsa diphetho tsa terminal fa go kgonega

---

## 💡 Mokgwa wa go Kgetha Ditlhopho

1. Ya go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Khetha **"New Issue"**
3. Khetha **"Feature Request"** template
4. Hlalosa:
   - Sephiri se o se rarabololeng
   - Mokgwa o o akanyang o amanang le go dira
   - Diphetho tse dingwe tse o di akanyeditse

---

## 🔌 Mokgwa wa go Romela Plugin

WIA SOOM e na le tsamaiso e matla ya plugin — o ka aga plugin ya gago ka metsotso e 5.

### Qalong ya Kgetsi
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tlhahiso e e Felletseng

Bala **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** go:
- Tsamaiso e e feletseng ya API
- Melemo e amanang
- Dithuto ka mekgwa
- Melemo e e amanang le ditlhopho le melawana ya tshegetso

### Romela Plugin ya gago

1. Fork [Plugin Store](https://wiasoom.com)
2. Tsenya plugin ya gago mo `plugins/{your-plugin-name}/`
3. Romela Pull Request
4. Morago ga go sekaseka, plugin ya gago e tla bonwa mo Plugin Store go baeteledipele botlhe!

---

## 🔀 Mokgwa wa go Romela Pull Request

### Go app e kgolo (wia-soom)

1. Fork repository
2. Bua branch ya ditlhopho: `git checkout -b feat/my-feature`
3. Dira diphetho tsa gago
4. Tlhopho mo lefelong:
   ```bash
   ```
5. Commit ka molaetsa o o kgethegileng:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push le go bula PR go `main`

### Molao wa Molaetsa wa Commit

| Prefix | Sebelisa ka |
|--------|---------|
| `feat:` | Ditlhopho tse disha |
| `fix:` | Go lokisa phoso |
| `docs:` | Dikhomphutha fela |
| `refactor:` | Go reorganisa khoutu (go se amanang le boitshwaro) |
| `i18n:` | Ditsela tse di amanang le phetolelo |
| `plugin:` | Diphetho tse di amanang le plugin |

### PR Checklist

- [ ] Khoutu e tsamaya ntle le diphoso
- [ ] A go na le mekgwa e amanang le ditlhopho (sebedisa i18n keys)
- [ ] A go na le `console.log` e e salang mo khoutong ya tlholego
- [ ] Ditshekatsheko tse di teng di ntse di tsweletse

---

## 🌐 Ditlhopho tsa Phetolelo (254 Languages)

WIA SOOM e tshegetsa **254 languages** — go simolola ka Amharic go ya go Zulu, go akaretsa le Braille le dipuo tse di amanang le RTL.

### Mokgwa wa Phetolelo

- Faele ya puo ya motheo: `src/renderer/src/i18n/en.json`
- Diphuthegong tsotlhe tse 254 di mo lefelong le le amanang
- Phetolelo e dirwa ka `scripts/translate-patch.js` (GPT-4o-mini API)

### Mokgwa wa go Tsenya Phetolelo

#### Khetho 1: Lokisa phetolelo e amanang

1. Fumana faele ya puo: `src/renderer/src/i18n/{lang-code}.json`
2. Lokisa phetolelo e e sa siamang
3. Romela PR ka phetolelo

#### Khetho 2: Tsenya mekgwa e e sa amanang
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Khetho 3: Sekaseka phetolelo ya mechine

Bontsi jwa dipuo tsa rona tse 254 di dirilwe ka mechine. Ditshekatsheko tse di amanang le borai di botlhokwa thata!

1. Khetha faele ya gago ya puo
2. Sekaseka diphetho
3. Lokisa diphetho tse di amanang le go se siame
4. Romela PR

### Melemo ya Dipuo

Re dirisa mekgwa ya ISO 639-1 (mohlala, `ko`, `en`, `ja`, `ar`, `hi`) ka diphetho tse di amanang le sebaka fa go tlhokega (mohlala, `zh-CN`, `pt-BR`).

---

## 🛠 Tlhopho ya Tlhahiso

### Ditsela tse di amanang

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Ageng
```bash
```
> Tlhokomeliso: The default 2GB heap e sitwa go lekana ka ntlha ya faele ya dipuo tse 254 + Monaco editor bundle (~38MB renderer).

### Sebopego sa Porojeke
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

Tlhopho e nngwe le e nngwe e dira WIA SOOM e betere bakeng sa baetapele lefatšeng ka bophara.

Na o lokisa phoso, o fetolela moqoqo, o haha plugin, kapa o eketsa sehlahisoa se seholo — **o karolo ea pale ena.**

---

<p align="center"><em>Hahiloeng ka ❤️ ke SmileStory Inc. le baetapele lefatšeng ka bophara.</em></p>