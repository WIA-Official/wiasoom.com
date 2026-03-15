<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ieviešana WIA SOOM</h1>
<p align="center"><strong>Mēs priecājamies par jūsu ieguldījumiem!</strong></p>
<p align="center">Neatkarīgi no tā, vai tas ir kļūdas labojums, jauna funkcija, spraudnis vai tulkojums — katrs ieguldījums ir svarīgs.</p>

---

## Satura rādītājs

- [Uzvedības kodekss](#code-of-conduct)
- [Kā ziņot par kļūdām](#-how-to-report-bugs)
- [Kā ieteikt funkcijas](#-how-to-suggest-features)
- [Kā iesniegt spraudni](#-how-to-submit-a-plugin)
- [Kā iesniegt Pull Request](#-how-to-submit-a-pull-request)
- [Tulkošanas ieguldījumi (254 valodas)](#-translation-contributions-254-languages)
- [Izstrādes iestatījumi](#-development-setup)

---

## Uzvedības kodekss

Mēs apņemamies nodrošināt laipnu un iekļaujošu pieredzi visiem.

- **Esi cieņpilns.** Izturies pret visiem ar cieņu.
- **Esi konstruktīvs.** Piedāvā noderīgu atsauksmi, nevis destruktīvu kritiku.
- **Esi iekļaujošs.** Mēs atbalstām 254 valodas un laipni gaidām ieguldītājus no katras valsts uz Zemes.
- **Nav uzmākšanās.** Nulles tolerance pret jebkāda veida diskrimināciju.

---

## 🐛 Kā ziņot par kļūdām

1. Dodieties uz [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Noklikšķiniet uz **"Jauna problēma"**
3. Izvēlieties **"Kļūdas ziņojuma"** veidni
4. Iekļaujiet:
   - WIA SOOM versiju (Iestatījumi → Par)
   - Operētājsistēmu un versiju (Windows/macOS/Linux)
   - Soļus, lai reproducētu
   - Sagaidāmā vs. faktiskā uzvedība
   - Ekrānuzņēmumus vai termināla izvadi, ja iespējams

---

## 💡 Kā ieteikt funkcijas

1. Dodieties uz [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Noklikšķiniet uz **"Jauna problēma"**
3. Izvēlieties **"Funkcijas pieprasījuma"** veidni
4. Aprakstiet:
   - Kuru problēmu jūs risināt
   - Kā jūs iedomājaties, ka tas darbosies
   - Jebkuras alternatīvas, kuras esat apsvēris

---

## 🔌 Kā iesniegt spraudni

WIA SOOM ir jaudīga spraudņu sistēma — jūs varat izveidot savu spraudni 5 minūtēs.

### Ātrā uzsākšana
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Pilns ceļvedis

Izlasiet **[Spraudņu izstrādātāja ce��vedi](docs/PLUGIN_DEVELOPER_GUIDE.md)** par:
- Pilnīgu API atsauci
- Darbojošiem piemēriem
- Soli pa solim apmācībām
- Labākajām praksēm un drošības noteikumiem

### Iesniedziet savu spraudni

1. Fork [Plugin Store](https://wiasoom.com)
2. Pievienojiet savu spraudni `plugins/{your-plugin-name}/`
3. Iesniedziet Pull Request
4. Pēc pārskatīšanas jūsu spraudnis parādīsies Spraudņu veikalā visiem lietotājiem!

---

## 🔀 Kā iesniegt Pull Request

### Galvenajai lietotnei (wia-soom)

1. Fork repozitoriju
2. Izveidojiet funkciju zaru: `git checkout -b feat/my-feature`
3. Veiciet savas izmaiņas
4. Testējiet lokāli:
   ```bash
   ```
5. Apstipriniet ar skaidru ziņojumu:
   ```
   feat: pievienot tumšo režīmu iestatījumiem
   ```
6. Augšupielādējiet un atveriet PR pret `main`

### Apstiprinājuma ziņojuma konvencija

| Prefikss | Izmantošanai |
|----------|--------------|
| `feat:`  | Jauna funkcija |
| `fix:`   | Kļūdas labojums |
| `docs:`  | Tikai dokumentācija |
| `refactor:` | Koda pārstrukturēšana (nav uzvedības izmaiņu) |
| `i18n:`  | Tulkošanas atjauninājumi |
| `plugin:` | Spraudņa saistītās izmaiņas |

### PR kontrolsaraksts

- [ ] Kods darbojas bez kļūdām
- [ ] Nav cieti kodētu virkņu (izmantojiet i18n atslēgas)
- [ ] Nav `console.log`, kas atstāts ražošanas kodā
- [ ] Esošie testi joprojām izpildās

---

## 🌐 Tulkošanas ieguldījumi (254 valodas)

WIA SOOM atbalsta **254 valodas** — no amhara līdz zulu, tostarp Braila un RTL valodas.

### Kā darbojas tulkošana

- Bāzes valodas fails: `src/renderer/src/i18n/en.json`
- Visi 254 valodu faili atrodas tajā pašā direktorijā
- Tulkošana tiek veikta, izmantojot `scripts/translate-patch.js` (GPT-4o-mini API)

### Kā ieguldīt tulkojumos

#### Opcija 1: Labot konkrētu tulkojumu

1. Atrodiet valodas failu: `src/renderer/src/i18n/{lang-code}.json`
2. Labojiet nepareizo tulkojumu
3. Iesniedziet PR ar izmaiņām

#### Opcija 2: Pievienot trūkstošās atslēgas
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opcija 3: Pārskatīt mašīntulkojumus

Daudzas no mūsu 254 valodām tika mašīntulkotas. Vietējo runātāju atsauksmes ir ārkārtīgi vērtīgas!

1. Izvēlieties savu valodas failu
2. Pārskatiet tulkojumus
3. Labojiet jebkādus neveiklus vai nepareizus tulkojumus
4. Iesniedziet PR

### Valodu kodi

Mēs izmantojam standarta ISO 639-1 kodus (piemēram, `ko`, `en`, `ja`, `ar`, `hi`) ar reģionālām variācijām, ja nepieciešams (piemēram, `zh-CN`, `pt-BR`).

---

## 🛠 Izstrādes iestatījumi

### Prasības

- Node.js 18+
- npm 9+
- Git

### Iestatījumi
```bash
```
### Izveide
```bash
```
> Piezīme: Noklusējuma 2GB kaudze nav pietiekama, ņemot vērā 254 valodu failus + Monaco redaktora pakotni (~38MB renderer).

### Projekta struktūra
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

## 🙏 Paldies

Katrs ieguldījums padara WIA SOOM labāku izstrādātājiem visā pasaulē.

Neatkarīgi no tā, vai tu labojat drukas kļūdu, tulkojat virkni, veidojat spraudni vai pievienojat nozīmīgu funkciju — **tu esi šīs stāsta daļa.**

---

<p align="center"><em>Izveidots ar ❤️ no SmileStory Inc. un līdzautoriem visā pasaulē.</em></p>