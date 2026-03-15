<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Cyfrannu i WIA SOOM</h1>
<p align="center"><strong>Byddem yn caru eich cyfraniadau!</strong></p>
<p align="center">P'un a yw'n atgyweiriad bug, nodwedd newydd, plugin, neu gyfieithiad — mae pob cyfraniad yn bwysig.</p>

---

## Cynnwys

- [Cod Ymddygiad](#code-of-conduct)
- [Sut i Adrodd am Fygiau](#-how-to-report-bugs)
- [Sut i Awgrymu Nodweddion](#-how-to-suggest-features)
- [Sut i Cyflwyno Plugin](#-how-to-submit-a-plugin)
- [Sut i Gyflwyno Cais Tynnu](#-how-to-submit-a-pull-request)
- [Cyfraniadau Cyfieithiadau (254 Ieithoedd)](#-translation-contributions-254-languages)
- [Gosodiad Datblygu](#-development-setup)

---

## Cod Ymddygiad

Rydym wedi ymrwymo i ddarparu profiad croesawgar ac inclusif i bawb.

- **Byddwch yn barchus.** Trin pawb gyda pharch.
- **Byddwch yn adeiladol.** Cynnig adborth defnyddiol, nid beirniadaeth ddinistriol.
- **Byddwch yn gynhwysol.** Rydym yn cefnogi 254 iaith ac yn croesawu cyfranwyr o bob gwlad ar y Ddaear.
- **Dim aflonyddu.** Dim goddefgarwch am wahaniaethu o unrhyw fath.

---

## 🐛 Sut i Adrodd am Fygiau

1. Ewch i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliciwch **"New Issue"**
3. Dewiswch y templed **"Bug Report"**
4. Cynnwys:
   - Fersiwn WIA SOOM (Gosodiadau → Am)
   - OS a fersiwn (Windows/macOS/Linux)
   - Camau i ail-greu
   - Ymddygiad disgwyliedig yn erbyn ymddygiad go iawn
   - Delweddau sgrin neu allbwn terminal os yn bosibl

---

## 💡 Sut i Awgrymu Nodweddion

1. Ewch i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliciwch **"New Issue"**
3. Dewiswch y templed **"Feature Request"**
4. Disgrifiwch:
   - Pa broblem rydych chi'n ei datrys
   - Sut rydych chi'n dychmygu ei weithio
   - Unrhyw ddewisiadau rydych chi wedi ystyried

---

## 🔌 Sut i Cyflwyno Plugin

Mae gan WIA SOOM system plugin pwerus — gallwch adeiladu eich plugin eich hun mewn 5 munud.

### Dechrau Cyflym
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Canllaw Llawn

Darllenwch y **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** am:
- Cyfeirnod API cyflawn
- Enghreifftiau gweithredol
- Tiwtorialau cam wrth gam
- Ymarfer da a rheolau diogelwch

### Cyflwyno Eich Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Ychwanegwch eich plugin i `plugins/{your-plugin-name}/`
3. Cyflwynwch Gais Tynnu
4. Ar ôl adolygiad, bydd eich plugin yn ymddangos yn y Plugin Store i bawb!

---

## 🔀 Sut i Gyflwyno Cais Tynnu

### Ar gyfer yr ap prif (wia-soom)

1. Fork y repository
2. Creu brach nodwedd: `git checkout -b feat/my-feature`
3. Gwnewch eich newidiadau
4. Profwch yn lleol:
   ```bash
   ```
5. Cyfuno gyda neges glir:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pwyswch a agor PR yn erbyn `main`

### Cyfuniad Neges Cyfuno

| Rhagddodiad | Defnyddir ar gyfer |
|-------------|-------------------|
| `feat:`     | Nodwedd newydd     |
| `fix:`      | Atgyweiriad bug    |
| `docs:`     | Dogfennaeth yn unig |
| `refactor:` | Ailstrwythuro cod (dim newid ymddygiad) |
| `i18n:`     | Diweddariadau cyfieithiadau |
| `plugin:`   | Newidiadau sy'n gysylltiedig â plugin |

### Rhestr wirio PR

- [ ] Mae'r cod yn rhedeg heb fethiant
- [ ] Dim llinynnau caled (defnyddiwch allweddi i18n)
- [ ] Dim `console.log` ar ôl yn y cod cynhyrchu
- [ ] Mae profion presennol yn dal i basio

---

## 🌐 Cyfraniadau Cyfieithiadau (254 Ieithoedd)

Mae WIA SOOM yn cefnogi **254 iaith** — o Amhariac i Zulu, gan gynnwys Braille a ieithoedd RTL.

### Sut Mae Cyfieithiadau'n Gweithio

- Ffeil iaith sylfaen: `src/renderer/src/i18n/en.json`
- Mae'r holl 254 o ffeiliau iaith yn yr un cyfeiriadur
- Mae cyfieithiadau yn cael eu gwneud trwy `scripts/translate-patch.js` (GPT-4o-mini API)

### Sut i Gyfrannu Cyfieithiadau

#### Opsiwn 1: Gwella cyfieithiad penodol

1. Dod o hyd i'r ffeil iaith: `src/renderer/src/i18n/{lang-code}.json`
2. Gwnewch y cyfieithiad anghywir
3. Cyflwynwch PR gyda'r newid

#### Opsiwn 2: Ychwanegu allweddi coll
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsiwn 3: Adolygu cyfieithiadau peiriant

Mae llawer o'n 254 iaith wedi'u cyfieithu gan beiriant. Mae adolygiadau gan siaradwyr brodorol yn hynod werthfawr!

1. Dewiswch eich ffeil iaith
2. Adolygwch y cyfieithiadau
3. Gwnewch unrhyw gyfieithiadau rhyfedd neu anghywir yn iawn
4. Cyflwynwch PR

### Codau Ieithoedd

Defnyddiwn godau ISO 639-1 safonol (e.e., `ko`, `en`, `ja`, `ar`, `hi`) gyda phriodweddau rhanbarthol pan fo angen (e.e., `zh-CN`, `pt-BR`).

---

## 🛠 Gosodiad Datblygu

### Gofynion Rhagofalon

- Node.js 18+
- npm 9+
- Git

### Gosodiad
```bash
```
### Adeiladu
```bash
```
> Nodyn: Nid yw'r heap 2GB yn ddigon oherwydd y 254 o ffeiliau iaith + pecyn golygydd Monaco (~38MB renderer).

### Strwythur y Prosiect
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

## 🙏 Diolch

Mae pob cyfraniad yn gwneud WIA SOOM yn well i ddatblygwyr ledled y byd.

P'un a ydych chi'n cywiro typo, yn cyfieithu llinyn, yn adeiladu plugin, neu'n ychwanegu nodwedd fawr — **rydych chi'n rhan o'r stori hon.**

---

<p align="center"><em>Wedi'i adeiladu gyda ❤️ gan SmileStory Inc. a chyfranwyr ledled y byd.</em></p>