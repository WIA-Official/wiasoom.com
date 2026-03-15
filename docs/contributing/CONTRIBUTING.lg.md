<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Okusaba mu WIA SOOM</h1>
<p align="center"><strong>Twagala obusaba bwo!</strong></p>
<p align="center">N'eky'ekikosa, ekirungi, plugin, oba okutegeka — buli busaba bulina obukulu.</p>

---

## Ekitabo ky'Obulamu

- [Amateeka g'Obulamu](#code-of-conduct)
- [Okwogera ku Bikosa](#-how-to-report-bugs)
- [Okwogera ku Bifaananyi](#-how-to-suggest-features)
- [Okwogera ku Kuteekamu Plugin](#-how-to-submit-a-plugin)
- [Okwogera ku Kuteekamu Pull Request](#-how-to-submit-a-pull-request)
- [Okusaba mu Kutegeka (254 Languages)](#-translation-contributions-254-languages)
- [Okutegekera Obulamu](#-development-setup)

---

## Amateeka g'Obulamu

Tugenda mu maaso okuteeka mu nkola obulamu obw'ekikadde n'obw'ekika ky'ekika.

- **Kiriza.** Kola buli muntu n'obulungi.
- **Kola obulungi.** Tandika okuddamu obukakafu, si okuddamu obubi.
- **Kola obutafaanana.** Tuwulira 254 z'ekika n'abaweereza okuva mu nsi zonna.
- **Tewali kutukiriza.** Tewali kuteekateeka kw'ekika kyonna.

---

## 🐛 Okwogera ku Bikosa

1. Genda ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kclicka **"New Issue"**
3. Kulaakulana **"Bug Report"** template
4. Kola:
   - WIA SOOM version (Settings → About)
   - OS ne version (Windows/macOS/Linux)
   - Enkola ezikola
   - Eky'ekirina n'ekikola
   - Ekitanda oba output y'ekikozesebwa singa kisoboka

---

## 💡 Okwogera ku Bifaananyi

1. Genda ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kclicka **"New Issue"**
3. Kulaakulana **"Feature Request"** template
4. Kola:
   - Ekizibu ky'ogenda okuzza
   - Okwogera ku bw'ogenda okukola
   - Ebyo by'ogenda okuzza

---

## 🔌 Okwogera ku Kuteekamu Plugin

WIA SOOM erina sisitimu ey'amaanyi ya plugin — osobola okuteeka plugin yo mu minutu 5.

### Okusaba Okw'amaanyi
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Okw'amaanyi Okukulu

Soma **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ku:
- Ekitabo ky'amaanyi
- Ebyokukola
- Ebyokukola mu ngeri ya step-by-step
- Ebyokukola eby'amaanyi n'amateeka g'ekikadde

### Teeka Plugin Yo

1. Fork [Plugin Store](https://wiasoom.com)
2. Teeka plugin yo mu `plugins/{your-plugin-name}/`
3. Teeka Pull Request
4. Olw'okuwandiika, plugin yo erina okuba mu Plugin Store ku bakkiriza bonna!

---

## 🔀 Okwogera ku Kuteekamu Pull Request

### Ku pulogula ya wansi (wia-soom)

1. Fork repository
2. Kola branch y'ekirungi: `git checkout -b feat/my-feature`
3. Kola obutafaanana bwo
4. Tandika mu kitundu:
   ```bash
   ```
5. Commit n'ekigambo eky'amaanyi:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ne kuteeka PR ku `main`

### Amateeka g'Ekigambo

| Prefix | Kikoze |
|--------|---------|
| `feat:` | Ekirungi eky'amaanyi |
| `fix:` | Okukosa |
| `docs:` | Okulabirako |
| `refactor:` | Okukola obutafaanana (tewandiikiddwa) |
| `i18n:` | Okukosa kw'ekiteeso |
| `plugin:` | Ebyo eby'amaanyi ku plugin |

### PR Checklist

- [ ] Code ekola nga tewali bubi
- [ ] Tewewandiikiddwa strings (kozesa i18n keys)
- [ ] Tewewandiikiddwa `console.log` mu code ey'ekikadde
- [ ] Ebyokukola eby'amaanyi bikiikiriziddwa

---

## 🌐 Okusaba mu Kutegeka (254 Languages)

WIA SOOM esuubira **254 languages** — okuva mu Amharic okutuuka ku Zulu, nga bwebuli Braille n'ekika ky'ekika.

### Okukola Kutegeka

- File y'ekika: `src/renderer/src/i18n/en.json`
- Ebyo 254 by'ekika biri mu kitundu kimu
- Okukola kutegeka kukolebwa mu `scripts/translate-patch.js` (GPT-4o-mini API)

### Okwogera ku Kutegeka Okusaba

#### Option 1: Kola kutegeka okw'ekika

1. Funa file y'ekika: `src/renderer/src/i18n/{lang-code}.json`
2. Kola kutegeka okw'ekika
3. Teeka PR n'ekikosa

#### Option 2: Teeka ebikosa ebikozesebwa
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Kola ku kutegeka kw'ekikozesebwa

Ebyo by'ekika 254 by'ekika byakolebwa mu kutegeka. Okukola kw'abantu ab'ekika kuli kimu!

1. Funa file y'ekika yo
2. Kola ku kutegeka
3. Kola ku kutegeka okw'ekika oba okw'ekikosa
4. Teeka PR

### Eby'ekika

Tukozesa standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) n'ebikozesebwa eby'ekika singa kisoboka (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Okutegekera Obulamu

### Eby'Okusaba

- Node.js 18+
- npm 9+
- Git

### Okutegekera
```bash
```
### Kola
```bash
```
> Note: Ekitundu 2GB heap tekinna kukwaniriza olw'ebikozesebwa 254 + Monaco editor bundle (~38MB renderer).

### Ekitundu ky'Obulamu
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

## 🙏 Webale

Obwakabaka bwonna bulaga WIA SOOM bulungi ku bakola ku nsi yonna.

N'ogenda okuzza ku typo, okutegeka olugero, okudda ku plugin, oba okuteekawo eky'ekikadde — **oli mu nkola eno.**

---

<p align="center"><em>Yakolebwa n'❤️ okuva ku SmileStory Inc. n'abakola ku nsi yonna.</em></p>