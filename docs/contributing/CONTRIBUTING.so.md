<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ka Qaybgalka WIA SOOM</h1>
<p align="center"><strong>Waxaan jeclaan lahayn ka qaybgalkaaga!</strong></p>
<p align="center">Haddii ay tahay sixid bug, muuqaal cusub, plugin, ama turjumaad — ka qaybgal kasta waa muhiim.</p>

---

## Jadwalka Mawduucyada

- [Xeerka Dhaqanka](#code-of-conduct)
- [Sida Loo Warbixiyo Bugs](#-how-to-report-bugs)
- [Sida Loo Soo Jeediyo Muuqaalada](#-how-to-suggest-features)
- [Sida Loo Gudbiyo Plugin](#-how-to-submit-a-plugin)
- [Sida Loo Gudbiyo Pull Request](#-how-to-submit-a-pull-request)
- [Ka Qaybgalka Turjumaadaha (254 Luqadood)](#-translation-contributions-254-languages)
- [Dejinta Horumarinta](#-development-setup)

---

## Xeerka Dhaqanka

Waxaan ka go'an tahay in aan bixino khibrad soo dhawayn leh oo ka mid ah dhammaan dadka.

- **Ixtiraam.** U dhaqaaq qof walba si sharaf leh.
- **Noqo mid dhisaya.** Bixi jawaab celin waxtar leh, ma ahan dhaleeceyn burburinaysa.
- **Noqo mid ka mid ah.** Waxaan taageernaa 254 luqadood waxaanan soo dhaweyneynaa ka qaybgalayaasha ka socda dhammaan dalalka dhulka.
- **Ma jirto xadgudub.** Xaqiiqo la'aan ah xadgudub nooc kasta ah.

---

## 🐛 Sida Loo Warbixiyo Bugs

1. Tag [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Guji **"New Issue"**
3. Dooro template-ka **"Bug Report"**
4. Ku dar:
   - Nooca WIA SOOM (Settings → About)
   - OS iyo nooca (Windows/macOS/Linux)
   - Tallaabooyinka lagu soo celinayo
   - Dhaqanka la filayo vs. dhaqanka dhabta ah
   - Sawirada shaashadda ama wax soo saarka terminal haddii ay suurtagal tahay

---

## 💡 Sida Loo Soo Jeediyo Muuqaalada

1. Tag [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Guji **"New Issue"**
3. Dooro template-ka **"Feature Request"**
4. Sharax:
   - Dhibaatada aad xallinayso
   - Sida aad u malaynayso inay u shaqeyneyso
   - Wax kasta oo kale oo aad tixgelisay

---

## 🔌 Sida Loo Gudbiyo Plugin

WIA SOOM waxay leedahay nidaam plugin awood leh — waxaad dhisi kartaa plugin-kaaga 5 daqiiqo gudaheed.

### Bilow Degdeg ah
§§§CHUNK_SEPARATOR§§§
### Hagaha Buuxa

Akhriso **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** si aad u hesho:
- Tixraac API oo dhameystiran
- Tusaalooyin shaqeynaya
- Casharro talaabo-talaabo ah
- Hababka ugu wanaagsan iyo xeerarka amniga

### Gudbi Plugin-kaaga

1. Fork [Plugin Store](https://wiasoom.com)
2. Ku dar plugin-kaaga `plugins/{your-plugin-name}/`
3. Gudbi Pull Request
4. Kadib dib u eegis, plugin-kaaga wuxuu ka muuqan doonaa Plugin Store dhammaan isticmaale yaasha!

---

## 🔀 Sida Loo Gudbiyo Pull Request

### Barnaamijka weyn (wia-soom)

1. Fork kaydka
2. Abuur laan muuqaal: `git checkout -b feat/my-feature`
3. Samee isbeddeladaada
4. Tijaabi gudaha:
   ```bash
   ```
5. Ku xidh farriin cad:
   ```
   feat: ku dar beddelka habka madow ee dejimaha
   ```
6. Riix oo fur PR ka dhan ah `main`

### Xeerka Farriinta Commit

| Hore | Isticmaalka |
|--------|---------|
| `feat:` | Muuqaal cusub |
| `fix:` | Sixid bug |
| `docs:` | Kaliya dukumiinti |
| `refactor:` | Dib-u-habeyn koodh (isbedel aan jirin) |
| `i18n:` | Casriyeyn turjumaad |
| `plugin:` | Isbeddelada la xiriira plugin |

### Liiska Hubinta PR

- [ ] Koodhku wuu socdaa iyada oo aan qalad lahayn
- [ ] Ma jiraan xarfo adag (isticmaal furayaasha i18n)
- [ ] Ma jiraan `console.log` oo ku haray koodhka wax soo saarka
- [ ] Imtixaannada jira wali way soconayaan

---

## 🌐 Ka Qaybgalka Turjumaadaha (254 Luqadood)

WIA SOOM waxay taageertaa **254 luqadood** — laga bilaabo Amharic ilaa Zulu, oo ay ku jiraan Braille iyo luqadaha RTL.

### Sida Turjumaaddu U Shaqeyso

- Faylka luqadda aasaasiga ah: `src/renderer/src/i18n/en.json`
- Dhammaan 254 faylasha luqadaha waxay ku jiraan isla galka
- Turjumaadda waxaa lagu sameeyaa `scripts/translate-patch.js` (GPT-4o-mini API)

### Sida Loo Qaybqaato Turjumaadaha

#### Ikhtiyaar 1: Sax turjumaad gaar ah

1. Raadi faylka luqadda: `src/renderer/src/i18n/{lang-code}.json`
2. Sax turjumaadda khaldan
3. Gudbi PR leh isbeddelka

#### Ikhtiyaar 2: Ku dar furayaasha maqan
§§§CHUNK_SEPARATOR§§§
#### Ikhtiyaar 3: Dib u eeg turjumaadaha mashiinka

Luqadaha 254-ka ah ee aan leenahay badankood waxaa turjumay mashiin. Dib u eegista hadalka asaliga ah waa mid aad u qiimo badan!

1. Dooro faylkaaga luqadda
2. Dib u eeg turjumaadaha
3. Sax turjumaadaha aan habboonayn ama khaldan
4. Gudbi PR

### Koodhadhka Luqadda

Waxaan isticmaalnaa koodhadhka caadiga ah ee ISO 639-1 (tusaale, `ko`, `en`, `ja`, `ar`, `hi`) oo leh noocyo goboleed marka loo baahdo (tusaale, `zh-CN`, `pt-BR`).

---

## 🛠 Dejinta Horumarinta

### Shuruudaha

- Node.js 18+
- npm 9+
- Git

### Dejinta
§§§CHUNK_SEPARATOR§§§
### Dhisid
§§§CHUNK_SEPARATOR§§§
> Fiiro gaar ah: Heerkulka 2GB ee caadiga ah ma ku filna sababo la xiriira 254 faylasha luqadaha + xidhmada Monaco (~38MB renderer).

### Qaab-dhismeedka Mashruuca
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Mahadsanid

Kaalin kasta waxay ka dhigeysaa WIA SOOM mid ka wanaagsan horumariyeyaasha adduunka oo dhan.

Haddii aad saxdo qalad, tarjumto xaraf, dhisto plugin, ama ku darto muuqaal weyn — **adigu waxaad qayb ka tahay sheekadan.**

---

<p align="center"><em>Waxaa dhisay ❤️ SmileStory Inc. iyo ka qaybgalayaasha adduunka oo dhan.</em></p>
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```

#### Option 3: Review machine translations

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup

```bash
```

### Build

```bash
```

> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

### Project Structure

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
