<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Gutanga Umusanzu kuri WIA SOOM</h1>
<p align="center"><strong>Turashaka umusanzu wawe!</strong></p>
<p align="center">Niba ari ugukosora ikosa, igitekerezo gishya, plugin, cyangwa guhindura ururimi — umusanzu wese ufite agaciro.</p>

---

## Urutonde rw'Ibikubiye Muri Iki Gitabo

- [Amategeko y'Imyitwarire](#code-of-conduct)
- [Uko Wamenyesha Ibibazo](#-how-to-report-bugs)
- [Uko Watanga Ibitekerezo ku Mikoreshereze](#-how-to-suggest-features)
- [Uko Wohereza Plugin](#-how-to-submit-a-plugin)
- [Uko Wohereza Pull Request](#-how-to-submit-a-pull-request)
- [Gutanga Umusanzu mu Guhindura Ururimi (254 Languages)](#-translation-contributions-254-languages)
- [Gushyiraho Ibiranga](#-development-setup)

---

## Amategeko y'Imyitwarire

Twiyemeje gutanga uburambe bwiza kandi bwakira buri wese.

- **Ba wubaha.** Fata buri wese nk'uw'agaciro.
- **Ba inyangamugayo.** Tanga ibitekerezo bifasha, ntukore ku buryo bwangiza.
- **Ba uwakira.** Dushyigikiye indimi 254 kandi dukira abatanga umusanzu baturutse mu bihugu byose ku Isi.
- **Nta guhohotera.** Nta kwihanganira ivangura iryo ari ryo ryose.

---

## 🐛 Uko Wamenyesha Ibibazo

1. Jya kuri [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kanda **"New Issue"**
3. Hitamo **"Bug Report"** template
4. Shyiramo:
   - WIA SOOM version (Igenamigambi → Amakuru)
   - OS na version (Windows/macOS/Linux)
   - Intambwe zo kongera kubyara
   - Ibyo witeze vs. imikorere nyayo
   - Ifoto cyangwa output ya terminal niba bishoboka

---

## 💡 Uko Watanga Ibitekerezo ku Mikoreshereze

1. Jya kuri [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kanda **"New Issue"**
3. Hitamo **"Feature Request"** template
4. Sobanura:
   - Ikibazo urimo gukemura
   - Uko ubona bikora
   - Izindi nzira watekerejeho

---

## 🔌 Uko Wohereza Plugin

WIA SOOM ifite sisitemu ikomeye ya plugin — ushobora kubaka plugin yawe mu minota 5.

### Gutangira Byihuse
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Inyandiko Yuzuye

Soma **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** kugirango:
- Umenye API yose
- Urebe ingero zikora
- Umenye inyigisho zose
- Umenye imigenzo myiza n'amategeko y'umutekano

### Ohereza Plugin Yawe

1. Fork [Plugin Store](https://wiasoom.com)
2. Ongeramo plugin yawe muri `plugins/{your-plugin-name}/`
3. Ohereza Pull Request
4. Nyuma yo gusuzuma, plugin yawe izagaragara muri Plugin Store ku bakoresha bose!

---

## 🔀 Uko Wohereza Pull Request

### Ku rubuga nyamukuru (wia-soom)

1. Fork repository
2. Shyiraho branch y'igitekerezo: `git checkout -b feat/my-feature`
3. Kora impinduka zawe
4. Gerageza mu buryo bw'imbere:
   ```bash
   ```
5. Commit hamwe n'ubutumwa bugaragaza:
   ```
   feat: ongeramo toggle y'uburyo bw'umwijima mu igenamigambi
   ```
6. Push no gufungura PR kuri `main`

### Amategeko y'Ubutumwa bwa Commit

| Ikimenyetso | Gikoreshwa kuri |
|-------------|-----------------|
| `feat:`     | Igitekerezo gishya |
| `fix:`      | Gukosora ikosa |
| `docs:`     | Ibyerekeye inyandiko gusa |
| `refactor:` | Guhindura kode (nta mpinduka mu mikorere) |
| `i18n:`     | Guhindura ururimi |
| `plugin:`   | Impinduka zirebana na plugin |

### Urutonde rwa PR

- [ ] Kode ikora nta makosa
- [ ] Nta magambo yanditse mu buryo butaziguye (koresha i18n keys)
- [ ] Nta `console.log` yasigaye mu kode y'ibikorwa
- [ ] Ibizamini bisanzwe biracyakora

---

## 🌐 Gutanga Umusanzu mu Guhindura Ururimi (254 Languages)

WIA SOOM ishyigikiye **indimi 254** — kuva mu Kinyarwanda kugeza mu Zulu, harimo na Braille n'indimi zikoreshwa mu buryo bwa RTL.

### Uko Guhindura Bikorwa

- Ifishi y'ururimi shingiro: `src/renderer/src/i18n/en.json`
- Amadosiye y'indimi 254 yose ari mu bubiko bumwe
- Guhindura bikorwa hifashishijwe `scripts/translate-patch.js` (GPT-4o-mini API)

### Uko Watanga Umusanzu mu Guhindura

#### Uburyo bwa 1: Gukosora guhindura kudakwiye

1. Shaka ifishi y'ururimi: `src/renderer/src/i18n/{lang-code}.json`
2. Kora ku guhindura kudakwiye
3. Ohereza PR hamwe n'ihinduka

#### Uburyo bwa 2: Ongeramo imfunguzo zitarimo
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Uburyo bwa 3: Gusuzuma guhindura kwa mashini

Izindi ndimi 254 zacu zashizweho n'ibikoresho bya mashini. Gusuzuma abavuga ururimi kavukire ni ingenzi cyane!

1. Hitamo ifishi y'ururimi rwawe
2. Susuzuma guhindura
3. Kora ku guhindura kudakwiye cyangwa kutari mu mwanya
4. Ohereza PR

### Amakode y'Indimi

Dukoresha amakode ya ISO 639-1 asanzwe (nka `ko`, `en`, `ja`, `ar`, `hi`) hamwe n'ibisobanuro by'akarere aho bikenewe (nka `zh-CN`, `pt-BR`).

---

## 🛠 Gushyiraho Ibiranga

### Ibisabwa

- Node.js 18+
- npm 9+
- Git

### Gushyiraho
```bash
```
### Kubaka
```bash
```
> Nota: Ibisanzwe 2GB heap ntibihagije kubera amadosiye y'indimi 254 + Monaco editor bundle (~38MB renderer).

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

Ibyo ushyira mu bikorwa byose bituma WIA SOOM irushaho kuba nziza ku bashakashatsi ku isi hose.

Niba uhuye n'ikosa, uhindura umurongo, wubaka plugin, cyangwa wongeramo ikiranga gikomeye — **uri mu gice cy'iyi nkuru.**

---

<p align="center"><em>Yubatswe na ❤️ na SmileStory Inc. n'abatanga umusanzu ku isi hose.</em></p>