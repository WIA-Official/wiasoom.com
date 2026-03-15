<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Na iVakacavacava ki na WIA SOOM</h1>
<p align="center"><strong>Keitou via marautaka na nomu i vakacavacava!</strong></p>
<p align="center">Na veika e rawa ni dua na vuni, i vakatagedegede vou, plugin, se na vosa — na i vakacavacava kece e bibi.</p>

---

## Taba ni Vakatagedegede

- [iTuvatuva ni Vakatagedegede](#code-of-conduct)
- [Na iTuvatuva me Taro na Vuni](#-how-to-report-bugs)
- [Na iTuvatuva me Vakaraitaka na iVakatagedegede](#-how-to-suggest-features)
- [Na iTuvatuva me Tuku e dua na Plugin](#-how-to-submit-a-plugin)
- [Na iTuvatuva me Tuku e dua na Pull Request](#-how-to-submit-a-pull-request)
- [Na iVakacavacava ni Vosa (254 Vosa)](#-translation-contributions-254-languages)
- [Na iTuvatuva ni Vakatuburi](#-development-setup)

---

## iTuvatuva ni Vakatagedegede

Keitou sa vakadonuya me solia e dua na ituvatuva vinaka ka veivakauqeti ki na veika kece.

- **Me marautaki.** Tiko vinaka na tamata kece.
- **Me veivakauqeti.** Solia na i vakatagedegede vinaka, kakua ni veivakacacani.
- **Me veivakauqeti.** Keitou vakauqeti 254 vosa ka marautaka na i vakacavacava mai na veimatanitu kece e vuravura.
- **Kakua ni veivakacacani.** Zero na veivakacacani ni veika e dua.

---

## 🐛 Na iTuvatuva me Taro na Vuni

1. Toso ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Chose the **"Bug Report"** template
4. Taucoko:
   - WIA SOOM version (Settings → About)
   - OS kei na version (Windows/macOS/Linux)
   - Na iTuvatuva me raica tale
   - Na iTuvatuva e nanuma vs. na iTuvatuva e yaco
   - Na iTuvatuva ni veivakauqeti se na terminal output ke rawa

---

## 💡 Na iTuvatuva me Vakaraitaka na iVakatagedegede

1. Toso ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Chose the **"Feature Request"** template
4. Vakaraitaka:
   - Na cava na leqa o sa vakadewataka
   - Na cava o nanuma me raica e cakacaka
   - Na veika e dua o sa nanuma

---

## 🔌 Na iTuvatuva me Tuku e dua na Plugin

WIA SOOM e tiko e dua na ivakarau ni plugin balavu — o rawa ni bulia na nomu plugin ena 5 na miniti.

### Na iTuvatuva Vakaivola
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Na iTuvatuva Tiko

Wilika na **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** me baleta:
- Na i vakatagedegede API taucoko
- Na i vakatagedegede e cakacaka
- Na iTuvatuva e veivakarautaki
- Na iTuvatuva vinaka kei na iTuvatuva ni veivakauqeti

### Tuku na Nomu Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Taucoko na nomu plugin ki `plugins/{your-plugin-name}/`
3. Tuku e dua na Pull Request
4. Ni sa raica, na nomu plugin e vakaraitaka ena Plugin Store me baleta na tamata kece!

---

## 🔀 Na iTuvatuva me Tuku e dua na Pull Request

### Me baleta na app levu (wia-soom)

1. Fork na repository
2. Bulia e dua na feature branch: `git checkout -b feat/my-feature`
3. Cakava na nomu veisau
4. Test locally:
   ```bash
   ```
5. Commit kei na i vakatagedegede vinaka:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ka vakaraitaka e dua na PR me baleta na `main`

### iTuvatuva ni Commit Message

| Prefix | Use for |
|--------|---------|
| `feat:` | iVakatagedegede vou |
| `fix:` | Vuni ni veika |
| `docs:` | iTuvatuva walega |
| `refactor:` | Na veisau ni code (kakua ni veisau na iTuvatuva) |
| `i18n:` | Na veisau ni vosa |
| `plugin:` | Na veisau e baleta na plugin |

### PR Checklist

- [ ] Na code e cakacaka vinaka
- [ ] Kakua ni tu na ivola e tu (vakayagataka na i18n keys)
- [ ] Kakua ni tu na `console.log` ena production code
- [ ] Na iTuvatuva e tu e sa tiko tiko

---

## 🌐 Na iVakacavacava ni Vosa (254 Vosa)

WIA SOOM e vakauqeti **254 vosa** — mai na Amharic ki na Zulu, e vakauqeti na Braille kei na vosa RTL.

### Na iTuvatuva ni Vosa

- Na vosa e bulia: `src/renderer/src/i18n/en.json`
- Na 254 na vosa e bulia e tiko ena dua na directory
- Na veisau e cakacaka ena `scripts/translate-patch.js` (GPT-4o-mini API)

### Na iTuvatuva me Vakaraitaka na iVakacavacava

#### iOption 1: Vakarautaka e dua na veisau e sega ni dodonu

1. Raica na vosa e bulia: `src/renderer/src/i18n/{lang-code}.json`
2. Vakarautaka na veisau e sega ni dodonu
3. Tuku e dua na PR kei na veisau

#### iOption 2: Taucoko na ivola e sega ni tu
§§§CHUNK_SEPARATOR§§§
#### iOption 3: Raica na veisau ni makete

E vuqa na noda 254 vosa e sa veisau mai na makete. Na veika e raica na tamata e bibi!

1. Kauta na vosa e bulia
2. Raica na veisau
3. Vakarautaka na veisau e sega ni dodonu se e veivakacacani
4. Tuku e dua na PR

### Na iVola ni Vosa

Keitou vakayagataka na iVola ISO 639-1 e dodonu (e.g., `ko`, `en`, `ja`, `ar`, `hi`) kei na veisau ni vanua e dodonu (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Na iTuvatuva ni Vakatuburi

### Na iTuvatuva e dodonu

- Node.js 18+
- npm 9+
- Git

### Na iTuvatuva
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
### Na iBulabula
```bash
```
> Na iTuvatuva: Na 2GB heap e sega ni toso vinaka ena vuku ni 254 na vosa e bulia + Monaco editor bundle (~38MB renderer).

### Na iTuvatuva ni Proyekti
```bash
```
---

## 🙏 Vinaka

Na veivuke kece e vakalevutaka na WIA SOOM me baleta na dauvola e vuravura taucoko.

Na cava na nomu veivakavinakataki e dua na vosa, veisautaka e dua na itukutuku, bulia e dua na plugin, se vakanamata e dua na iwalewale levu — **o iko e dua na part e na itukutuku oqo.**

---

<p align="center"><em>Bula tiko ena ❤️ mai na SmileStory Inc. kei na veivuke e vuravura taucoko.</em></p>
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
