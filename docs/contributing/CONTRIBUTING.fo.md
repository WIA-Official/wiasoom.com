<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Atkvøtt til WIA SOOM</h1>
<p align="center"><strong>Vit elska tykkara atkvøtt!</strong></p>
<p align="center">Hvørji tað er ein bug-fix, nýggj funksjón, plugin, ella týðing — hvør atkvøtt telur.</p>

---

## Innihaldslýsing

- [Reglur um atferð](#code-of-conduct)
- [Hvussu tú kann boða frá bugum](#-how-to-report-bugs)
- [Hvussu tú kann leggja til rættis funksjónir](#-how-to-suggest-features)
- [Hvussu tú kann senda ein plugin](#-how-to-submit-a-plugin)
- [Hvussu tú kann senda ein Pull Request](#-how-to-submit-a-pull-request)
- [Týðingar atkvøtt (254 mál)](#-translation-contributions-254-languages)
- [Menning Setup](#-development-setup)

---

## Reglur um atferð

Vit eru forpliktar til at veita eina vælkomna og innlima uppliving fyri øll.

- **Vera virðilig.** Beina øll við virðing.
- **Vera konstruktiv.** Bjóða hjálpsom ummæli, ikki destruktiv kritikk.
- **Vera innlima.** Vit stuðla 254 málum og bjóða atkvøttum frá hvørjum landi á jørðini vælkomin.
- **Einki harðskap.** Eina nulltoleransu fyri mismuni av hvørjum slag.

---

## 🐛 Hvussu tú kann boða frá bugum

1. Far til [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikka á **"New Issue"**
3. Vel **"Bug Report"** skablonina
4. Inkludera:
   - WIA SOOM útgáva (Innstillingar → Um)
   - OS og útgáva (Windows/macOS/Linux)
   - Skref til at endurtaka
   - Væntað vs. veruligt atferði
   - Skermmyndir ella terminal úttøka um møguligt

---

## 💡 Hvussu tú kann leggja til rættis funksjónir

1. Far til [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikka á **"New Issue"**
3. Vel **"Feature Request"** skablonina
4. Lýsing:
   - Hvat trupulleika tú ert at loysa
   - Hvussu tú imaginerar at tað arbeiðir
   - Einar aðrar loysnir tú hevur hugt at

---

## 🔌 Hvussu tú kann senda ein plugin

WIA SOOM hevur ein sterkan plugin-system — tú kanst byggja tín egna plugin á 5 minuttum.

### Skjótt byrja
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Fullur leiðbeining

Les **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** fyri:
- Fulla API vísa
- Virkandi dømi
- Skref-fyriskipaðar leiðbeiningar
- Bestu venjur og trygdareglur

### Senda tín plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Legg tín plugin til `plugins/{your-plugin-name}/`
3. Senda ein Pull Request
4. Eftir granskning, tín plugin kemur fram í Plugin Store fyri allar brúkarar!

---

## 🔀 Hvussu tú kann senda ein Pull Request

### Fyrir høvuðsappina (wia-soom)

1. Forka repository
2. Skapa ein funksjón grein: `git checkout -b feat/my-feature`
3. Ger tínar broytingar
4. Testa lokalt:
   ```bash
   ```
5. Commit við einum klárum boð:
   ```
   feat: leggi til myrka hátt til innstillingar
   ```
6. Push og opna ein PR móti `main`

### Commit Boð Regla

| Prefix | Brúka til |
|--------|---------|
| `feat:` | Nýggj funksjón |
| `fix:` | Bug-fix |
| `docs:` | Bert dokumentatión |
| `refactor:` | Koda umskipan (eingin atferðar broyting) |
| `i18n:` | Týðingar uppdateringar |
| `plugin:` | Plugin-tengdar broytingar |

### PR Checklista

- [ ] Koda arbeiðir uttan feil
- [ ] Einki harðkoda strikur (brúka i18n lyklar)
- [ ] Einki `console.log` eftirlatin í framleiðslukoda
- [ ] Finnandi testir framvegis ganga

---

## 🌐 Týðingar atkvøtt (254 mál)

WIA SOOM stuðlar **254 málum** — frá amharisk til zulu, viðtikið Braille og RTL mál.

### Hvussu Týðing arbeiðir

- Grunn mál filur: `src/renderer/src/i18n/en.json`
- Allar 254 mál filur eru í sama mappu
- Týðing verður gjørd við `scripts/translate-patch.js` (GPT-4o-mini API)

### Hvussu tú kanst leggja til rættis týðingar

#### Val 1: Rættað ein serligan týðing

1. Finn mál filin: `src/renderer/src/i18n/{lang-code}.json`
2. Rættaða óneyðuga týðingina
3. Senda ein PR við broytingini

#### Val 2: Leggja til manglandi lyklar
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Val 3: Granska maskin týðingar

Margar av okkara 254 málum vóru maskin-týdd. Granskingar frá innføddum talaðari eru ómetaliga virðismiklar!

1. Vel tín mál fil
2. Granska týðingarnar
3. Rættaða allar óhóskandi ella óneyðugar týðingar
4. Senda ein PR

### Mál Kóðar

Vit brúka standard ISO 639-1 kóðar (t.d., `ko`, `en`, `ja`, `ar`, `hi`) við regionálum variantum har tað er neyðugt (t.d., `zh-CN`, `pt-BR`).

---

## 🛠 Menning Setup

### Krøv

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Bygg
```bash
```
> Merkið: Default 2GB heap er ikki nóg mikið vegna 254 mál filur + Monaco editor bundle (~38MB renderer).

### Projekt Struktura
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

## 🙏 Takk

Hvørjga tilfeingur ger WIA SOOM betri fyri utviklarar kring heimin.

Hvønn tú rætta ein typa, týðir ein strik, byggir ein plugin, ella leggur eina stóran funksjón aftrat — **tú ert partur av hesi søgu.**

---

<p align="center"><em>Bygdur við ❤️ av SmileStory Inc. og tilfeingum kring heimin.</em></p>