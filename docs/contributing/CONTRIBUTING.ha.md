<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Gudunmawa ga WIA SOOM</h1>
<p align="center"><strong>Muna son gudunmawarku!</strong></p>
<p align="center">Ko gyara kwari ne, sabon fasali, plugin, ko fassara — kowanne gudunmawa yana da mahimmanci.</p>

---

## Jaddawalin Abubuwan

- [Ka'idojin Halayya](#code-of-conduct)
- [Yadda Ake Rahoton Kwarai](#-how-to-report-bugs)
- [Yadda Ake Bayar da Shawarwari](#-how-to-suggest-features)
- [Yadda Ake Gabatar da Plugin](#-how-to-submit-a-plugin)
- [Yadda Ake Gabatar da Bukatar Juyawa](#-how-to-submit-a-pull-request)
- [Gudunmawar Fassara (254 Harsuna)](#-translation-contributions-254-languages)
- [Tsarin Ci Gaba](#-development-setup)

---

## Ka'idojin Halayya

Muna da niyyar samar da kyakkyawar kwarewa ga kowa.

- **Kasance mai girmamawa.** Yi wa kowa girmamawa.
- **Kasance mai gina jiki.** Bayar da ra'ayi mai amfani, ba tare da cin zarafi ba.
- **Kasance mai karɓa.** Muna goyon bayan harsuna 254 kuma muna maraba da masu bayar da gudunmawa daga kowanne ƙasa a Duniya.
- **Babu cin zarafi.** Babu juriya ga kowanne nau'in bambanci.

---

## 🐛 Yadda Ake Rahoton Kwarai

1. Je zuwa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Danna **"Sabon Matsala"**
3. Zaɓi samfurin **"Rahoton Kwarai"**
4. Haɗa:
   - Sigar WIA SOOM (Saituna → Game da)
   - OS da sigar (Windows/macOS/Linux)
   - Matakai don maimaitawa
   - Abin da aka zata da abin da ya faru
   - Hotunan allo ko fitarwa daga terminal idan zai yiwu

---

## 💡 Yadda Ake Bayar da Shawarwari

1. Je zuwa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Danna **"Sabon Matsala"**
3. Zaɓi samfurin **"Neman Fasali"**
4. Bayyana:
   - Wane matsala kake warwarewa
   - Yaya kake tunanin zai yi aiki
   - Kowanne madadin da ka yi la'akari da shi

---

## 🔌 Yadda Ake Gabatar da Plugin

WIA SOOM na da tsarin plugin mai ƙarfi — zaka iya gina plugin naka cikin minti 5.

### Fara Da Sauri
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Jagora Cikakke

Karanta **[Jagorar Masanin Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** don:
- Cikakken bayani akan API
- Misalai masu aiki
- Koyawa mataki-mataki
- Mafi kyawun hanyoyi da ka'idojin tsaro

### Gabatar da Plugin Dinka

1. Fork [Plugin Store](https://wiasoom.com)
2. Ƙara plugin dinka zuwa `plugins/{your-plugin-name}/`
3. Gabatar da Bukatar Juyawa
4. Bayan duba, plugin dinka zai bayyana a cikin Shagon Plugin ga duk masu amfani!

---

## 🔀 Yadda Ake Gabatar da Bukatar Juyawa

### Don babban aikace-aikacen (wia-soom)

1. Fork ma'ajin
2. Ƙirƙiri reshen fasali: `git checkout -b feat/my-feature`
3. Yi canje-canje
4. Gwada a cikin gida:
   ```bash
   ```
5. Yi commit tare da saƙo mai kyau:
   ```
   feat: ƙara maɓallin canza yanayin duhu zuwa saituna
   ```
6. Tura kuma bude PR a kan `main`

### Ka'idojin Saƙon Commit

| Prefix | Amfani da |
|--------|-----------|
| `feat:` | Sabon fasali |
| `fix:` | Gyaran kwari |
| `docs:` | Takaddun shaida kawai |
| `refactor:` | Tsarin lambar (ba tare da canjin hali ba) |
| `i18n:` | Sabuntawa na fassara |
| `plugin:` | Canje-canje masu alaƙa da plugin |

### Jerin Duba PR

- [ ] Lambar tana gudana ba tare da kuskure ba
- [ ] Babu rubutattun kalmomi (yi amfani da maɓallan i18n)
- [ ] Babu `console.log` da aka bari a cikin lambar samarwa
- [ ] Gwaje-gwaje masu akwai har yanzu suna wucewa

---

## 🌐 Gudunmawar Fassara (254 Harsuna)

WIA SOOM na goyon bayan **254 harsuna** — daga Amharic zuwa Zulu, ciki har da Braille da harsunan RTL.

### Yadda Fassara ke Aiki

- Fayil na harshe na asali: `src/renderer/src/i18n/en.json`
- Dukkan fayilolin harsuna 254 suna cikin wannan babban fayil
- Fassara ana yi ta hanyar `scripts/translate-patch.js` (GPT-4o-mini API)

### Yadda Ake Bayar da Fassara

#### Zaɓi 1: Gyara fassara takamaimai

1. Nemo fayil ɗin harshe: `src/renderer/src/i18n/{lang-code}.json`
2. Gyara fassarar da ba ta dace ba
3. Gabatar da PR tare da canjin

#### Zaɓi 2: Ƙara maɓallan da ba su wanzu ba
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Zaɓi 3: Duba fassarar na'ura

Yawancin harsunan mu 254 an fassara su ta na'ura. Duba masu magana na asali suna da matuƙar amfani!

1. Zaɓi fayil ɗin harshe naka
2. Duba fassarar
3. Gyara kowanne fassara da ba ta dace ba ko kuma mai wahala
4. Gabatar da PR

### Lambobin Harshe

Muna amfani da lambobin ISO 639-1 na al'ada (misali, `ko`, `en`, `ja`, `ar`, `hi`) tare da bambance-bambancen yanki inda ya dace (misali, `zh-CN`, `pt-BR`).

---

## 🛠 Tsarin Ci Gaba

### Abubuwan Da Ake Bukata

- Node.js 18+
- npm 9+
- Git

### Tsarin
```bash
```
### Gina
```bash
```
> Lura: Heap na 2GB na tsoho ba ya isa saboda fayilolin harsuna 254 + Monaco editor bundle (~38MB renderer).

### Tsarin Aikin
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

## 🙏 Na gode

Kowane gudummawa yana sa WIA SOOM ya fi kyau ga masu haɓaka a duk faɗin duniya.

Ko kuna gyara kuskure, fassara wani rubutu, gina wani plugin, ko ƙara babban fasali — **ku ne wani ɓangare na wannan labarin.**

---

<p align="center"><em>An gina tare da ❤️ daga SmileStory Inc. da masu bayar da gudummawa daga ko'ina cikin duniya.</em></p>