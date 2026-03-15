<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Að leggja fram til WIA SOOM</h1>
<p align="center"><strong>Við myndum elska að fá framlag þitt!</strong></p>
<p align="center">Hvort sem það er villu lagfæring, nýtt eiginleiki, viðbót eða þýðing — hvert framlag skiptir máli.</p>

---

## Efnisyfirlit

- [Siðareglur](#code-of-conduct)
- [Hvernig á að tilkynna villur](#-how-to-report-bugs)
- [Hvernig á að leggja til eiginleika](#-how-to-suggest-features)
- [Hvernig á að skila viðbót](#-how-to-submit-a-plugin)
- [Hvernig á að skila Pull Request](#-how-to-submit-a-pull-request)
- [Þýðingarfyrirkomulag (254 tungumál)](#-translation-contributions-254-languages)
- [Þróunaruppsetning](#-development-setup)

---

## Siðareglur

Við erum skuldbundin til að veita öllum vinalegt og innifalið umhverfi.

- **Vera virðingarfullur.** Meðhöndlaðu alla með reisn.
- **Vera uppbyggjandi.** Bjóða hjálplegar athugasemdir, ekki eyðileggjandi gagnrýni.
- **Vera innifalið.** Við styðjum 254 tungumál og fögnum framlagi frá öllum löndum jarðar.
- **Engin áreitni.** Engin þol fyrir mismunun af neinu tagi.

---

## 🐛 Hvernig á að tilkynna villur

1. Farðu á [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Smelltu á **"New Issue"**
3. Veldu **"Bug Report"** sniðmát
4. Innihalda:
   - WIA SOOM útgáfa (Stillingar → Um)
   - OS og útgáfa (Windows/macOS/Linux)
   - Skref til að endurtaka
   - Væntanleg vs. raunveruleg hegðun
   - Skjámyndir eða terminal úttak ef mögulegt

---

## 💡 Hvernig á að leggja til eiginleika

1. Farðu á [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Smelltu á **"New Issue"**
3. Veldu **"Feature Request"** sniðmát
4. Lýstu:
   - Hvaða vandamál þú ert að leysa
   - Hvernig þú ímyndar þér að það virki
   - Öllum valkostum sem þú hefur íhugað

---

## 🔌 Hvernig á að skila viðbót

WIA SOOM hefur öflugt viðbótakerfi — þú getur smíðað þína eigin viðbót á 5 mínútum.

### Fljótleg byrjun
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full leiðarvísir

Lestu **[Leiðbeiningar fyrir viðbótarsmíði](docs/PLUGIN_DEVELOPER_GUIDE.md)** fyrir:
- Fulla API tilvísun
- Virk dæmi
- Skref-fyrir-skref námskeið
- Bestu venjur og öryggisreglur

### Skilaðu viðbótinni þinni

1. Fork [Plugin Store](https://wiasoom.com)
2. Bættu viðbótinni þinni við `plugins/{your-plugin-name}/`
3. Skilaðu Pull Request
4. Eftir endurskoðun mun viðbótin þín birtast í Plugin Store fyrir alla notendur!

---

## 🔀 Hvernig á að skila Pull Request

### Fyrir aðalforritið (wia-soom)

1. Forkuðu geymsluna
2. Búðu til eiginleika grein: `git checkout -b feat/my-feature`
3. Gerðu breytingarnar þínar
4. Prófaðu staðbundið:
   ```bash
   ```
5. Skilaðu með skýru skilaboði:
   ```
   feat: bæta myrka stillingu við stillingar
   ```
6. Push og opnaðu PR gegn `main`

### Reglur fyrir skuldbindingar

| Forskeyti | Notað fyrir |
|-----------|-------------|
| `feat:`   | Nýr eiginleiki |
| `fix:`    | Villulagfæring |
| `docs:`   | Aðeins skjölun |
| `refactor:` | Kóðaskipulag (enginn hegðunarbreyting) |
| `i18n:`   | Þýðingaruppfærslur |
| `plugin:` | Breytingar tengdar viðbótum |

### PR Skoðunarskrá

- [ ] Kóðinn keyrir án villna
- [ ] Engin harðkóðuð strengir (notaðu i18n lykla)
- [ ] Engin `console.log` eftir í framleiðslukóða
- [ ] Núverandi prófanir eru enn í gildi

---

## 🌐 Þýðingarfyrirkomulag (254 tungumál)

WIA SOOM styður **254 tungumál** — frá Amharic til Zulu, þar á meðal Braille og RTL tungumál.

### Hvernig þýðing virkar

- Grunn tungumálaskrá: `src/renderer/src/i18n/en.json`
- Allar 254 tungumálaskrár eru í sama skráarsafni
- Þýðingin fer fram í gegnum `scripts/translate-patch.js` (GPT-4o-mini API)

### Hvernig á að leggja til þýðingar

#### Valkostur 1: Lagfærðu ákveðna þýðingu

1. Finndu tungumálaskrána: `src/renderer/src/i18n/{lang-code}.json`
2. Lagfærðu rangt þýðingu
3. Skilaðu PR með breytingunni

#### Valkostur 2: Bættu við vöntun lykla
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Valkostur 3: Skoðaðu vélþýðingar

Margar af okkar 254 tungumálum voru vélþýddar. Endurskoðanir frá móðurmálsræðum eru ómetanlegar!

1. Veldu tungumálaskrána þína
2. Skoðaðu þýðingarnar
3. Lagfærðu allar óþægilegar eða rangar þýðingar
4. Skilaðu PR

### Tungumálakóðar

Við notum staðlaða ISO 639-1 kóða (t.d. `ko`, `en`, `ja`, `ar`, `hi`) með svæðisbundnum afbrigðum þar sem þörf krefur (t.d. `zh-CN`, `pt-BR`).

---

## 🛠 Þróunaruppsetning

### Forsendur

- Node.js 18+
- npm 9+
- Git

### Uppsetning
```bash
```
### Bygging
```bash
```
> Athugið: Sjálfgefið 2GB heap er ekki nóg vegna 254 tungumálaskrár + Monaco ritstjóra pakka (~38MB renderer).

### Uppbygging verkefnis
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

## 🙏 Takk fyrir

Hver einasta framlag gerir WIA SOOM betri fyrir þróunaraðila um allan heim.

Hvort sem þú lagar stafsetningarvillur, þýðir texta, býr til viðbót eða bætir við stórum eiginleika — **þú ert hluti af þessari sögu.**

---

<p align="center"><em>Byggt með ❤️ af SmileStory Inc. og framlagara um allan heim.</em></p>