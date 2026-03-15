<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guhabwa Umusanzu kuri WIA SOOM</h1>
<p align="center"><strong>Turakunda umusanzu wawe!</strong></p>
<p align="center">N'aho ari ugukosora ikosa, igikorwa gishasha, plugin, canke guhindura ururimi — umusanzu wese ufise akamaro.</p>

---

## Urutonde rw'Ibikubiyemo

- [Amategeko y'Imyitwarire](#code-of-conduct)
- [Uko Wobimenyesha Ibibazo](#-how-to-report-bugs)
- [Uko Wobishikiriza Ibikorwa](#-how-to-suggest-features)
- [Uko Wobandikira Plugin](#-how-to-submit-a-plugin)
- [Uko Wobandikira Pull Request](#-how-to-submit-a-pull-request)
- [Umusanzu w'Imihinduro (Indimi 254)](#-translation-contributions-254-languages)
- [Gushiraho Ibiranga](#-development-setup)

---

## Amategeko y'Imyitwarire

Twiyemeje gutanga uburambe bwiza kandi bwakira bose.

- **Ba mwiza.** Fata abantu bose mu bwitonzi.
- **Ba inyangamugayo.** Tanga ibitekerezo bifasha, ntugire umwiyahuzi.
- **Ba mwiza mu kwakira.** Turashigikiye indimi 254 kandi dukiriye abatanga umusanzu bava mu bihugu vyose ku isi.
- **Nta guhohoterwa.** Ntitwihanganira ivangura iryo ari ryo ryose.

---

## 🐛 Uko Wobimenyesha Ibibazo

1. Jya ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kanda **"New Issue"**
3. Hitamwo **"Bug Report"** template
4. Shyiramo:
   - WIA SOOM version (Settings → About)
   - OS n'igice (Windows/macOS/Linux)
   - Intambwe zo kongera
   - Ibigenda biba vyitezwe n'ibiri kuba
   - Amafoto y'ibikurikira canke ibivuye mu terminal niba bishoboka

---

## 💡 Uko Wobishikiriza Ibikorwa

1. Jya ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kanda **"New Issue"**
3. Hitamwo **"Feature Request"** template
4. Sobanura:
   - Ikibazo uriko urakemura
   - Uko ubona bizokora
   - Ibindi bisubizo wiyumvira

---

## 🔌 Uko Wobandikira Plugin

WIA SOOM ifise sisiteme ikomeye ya plugin — urashobora kubaka plugin yawe mu minota 5.

### Gutangura Vuba
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Inyandiko Yuzuye

Soma **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** ku:
- Ibaruwa y'API yuzuye
- Ibikorwa bikora
- Amasomo y'intambwe ku ntambwe
- Imigenzo myiza n'amategeko y'umutekano

### Andika Plugin Yawe

1. Fork [Plugin Store](https://wiasoom.com)
2. Ongeramo plugin yawe muri `plugins/{your-plugin-name}/`
3. Andika Pull Request
4. Nyuma yo gusuzuma, plugin yawe izoboneka muri Plugin Store ku bakoresha bose!

---

## 🔀 Uko Wobandikira Pull Request

### Ku gikorwa nyamukuru (wia-soom)

1. Fork repository
2. Shira ku murongo w'ibikorwa: `git checkout -b feat/my-feature`
3. Kora impinduka zawe
4. Gerageza mu buryo bw'akarorero:
   ```bash
   ```
5. Commit ufise ubutumwa bwerekeye:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push kandi ufungure PR ku `main`

### Amategeko y'Ubutumwa bwa Commit

| Icyapa | Gikoreshwa ku |
|--------|---------------|
| `feat:` | Igikorwa gishasha |
| `fix:` | Gukosora ikosa |
| `docs:` | Ibyanditswe gusa |
| `refactor:` | Guhindura code (nta mpinduka mu myitwarire) |
| `i18n:` | Guhindura imihinduro |
| `plugin:` | Impinduka zirebana na plugin |

### Urutonde rwa PR

- [ ] Code ikora nta makosa
- [ ] Nta magambo yanditse mu buryo butazwi (koresha i18n keys)
- [ ] Nta `console.log` yasigaye mu code y'ubucuruzi
- [ ] Ibizamini bisanzwe birakora

---

## 🌐 Umusanzu w'Imihinduro (Indimi 254)

WIA SOOM ishyigikiye **indimi 254** — kuva mu Kinyarwanda gushika mu Zulu, harimwo Braille n'indimi za RTL.

### Uko Guhindura Bikorwa

- Ifishi y'ururimi rw'ibanze: `src/renderer/src/i18n/en.json`
- Zose indimi 254 ziri mu nyandiko imwe
- Guhindura bikorwa hifashishijwe `scripts/translate-patch.js` (GPT-4o-mini API)

### Uko Wobandikira Imihinduro

#### Igikorwa 1: Gukosora imihinduro runaka

1. Shaka ifishi y'ururimi: `src/renderer/src/i18n/{lang-code}.json`
2. Kora ku gihinduka kitari cyo
3. Andika PR ifite impinduka

#### Igikorwa 2: Ongeramo imfunguzo zitaraboneka
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Igikorwa 3: Gusuzuma imihinduro y'ibikoresho

Amwe mu ndimi zacu 254 zarahinduwe n'ibikoresho. Gusuzuma abavuga ururimi ni ingenzi cane!

1. Hitamo ifishi y'ururimi rwawe
2. Susuzuma imihinduro
3. Kora ku mihinduro idakwiye canke itari yo
4. Andika PR

### Icyapa cy'Indimi

Dukoresha ibimenyetso bisanzwe ISO 639-1 (nko, `ko`, `en`, `ja`, `ar`, `hi`) hamwe n'ibisubizo by'akarere aho bikenewe (nko, `zh-CN`, `pt-BR`).

---

## 🛠 Gushiraho Ibiranga

### Ibikenewe

- Node.js 18+
- npm 9+
- Git

### Gushiraho
```bash
```
### Kubaka
```bash
```
> Nota: Igihe cy'ibanze cya 2GB ntikihagije kubera indimi 254 ziri mu nyandiko + bundle ya Monaco editor (~38MB renderer).

### Imiterere y'Umushinga
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

## 🙏 Urakoze

Ihiganwa ryose rituma WIA SOOM irushiriza kuba nziza ku bakora porogaramu kw'isi yose.

N'ukora ku kintu gito, ukavuga ikintu mu rundi rurimi, ukubaka plugin, canke ukongerako ikintu kinini — **uri mu gice c'iyi nkuru.**

---

<p align="center"><em>Yubatswe n'❤️ na SmileStory Inc. n'abatanga umusanzu ku isi yose.</em></p>