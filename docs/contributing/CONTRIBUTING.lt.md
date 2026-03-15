<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Prisidėjimas prie WIA SOOM</h1>
<p align="center"><strong>Mes laukiame jūsų indėlio!</strong></p>
<p align="center">Ar tai būtų klaidų taisymas, nauja funkcija, papildinys ar vertimas — kiekvienas indėlis yra svarbus.</p>

---

## Turinys

- [Elgesio kodeksas](#code-of-conduct)
- [Kaip pranešti apie klaidas](#-how-to-report-bugs)
- [Kaip pasiūlyti funkcijas](#-how-to-suggest-features)
- [Kaip pateikti papildinį](#-how-to-submit-a-plugin)
- [Kaip pateikti „Pull Request“](#-how-to-submit-a-pull-request)
- [Vertimo indėliai (254 kalbos)](#-translation-contributions-254-languages)
- [Plėtros nustatymas](#-development-setup)

---

## Elgesio kodeksas

Mes esame įsipareigoję teikti svetingą ir įtraukią patirtį visiems.

- **Būkite pagarbus.** Elkitės su visais oriai.
- **Būkite konstruktyvūs.** Pateikite naudingą atsiliepimą, o ne destruktyvią kritiką.
- **Būkite įtraukiantys.** Mes palaikome 254 kalbas ir laukiame indėlininkų iš kiekvienos pasaulio šalies.
- **Nėra priekabiavimo.** Nulinė tolerancija bet kokiai diskriminacijai.

---

## 🐛 Kaip pranešti apie klaidas

1. Eikite į [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Paspauskite **"New Issue"**
3. Pasirinkite **"Bug Report"** šabloną
4. Įtraukite:
   - WIA SOOM versiją (Nustatymai → Apie)
   - OS ir versiją (Windows/macOS/Linux)
   - Veiksmus, kaip reprodukuoti
   - Tikėtinas vs. faktinis elgesys
   - Ekrano nuotraukas arba terminalo išvestį, jei įmanoma

---

## 💡 Kaip pasiūlyti funkcijas

1. Eikite į [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Paspauskite **"New Issue"**
3. Pasirinkite **"Feature Request"** šabloną
4. Apibūdinkite:
   - Kokią problemą sprendžiate
   - Kaip įsivaizduojate, kad tai veiks
   - Bet kokias alternatyvas, kurias svarstėte

---

## 🔌 Kaip pateikti papildinį

WIA SOOM turi galingą papildinių sistemą — galite sukurti savo papildinį per 5 minutes.

### Greitas pradžia
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Pilnas vadovas

Perskaitykite **[Papildinio kūrėjo vadovą](docs/PLUGIN_DEVELOPER_GUIDE.md)**, kad gautumėte:
- Išsamią API nuorodą
- Veikiančius pavyzdžius
- Žingsnis po žingsnio pamokas
- Geriausias praktikas ir saugumo taisykles

### Pateikite savo papildinį

1. Fork [Plugin Store](https://wiasoom.com)
2. Pridėkite savo papildinį į `plugins/{your-plugin-name}/`
3. Pateikite „Pull Request“
4. Po peržiūros, jūsų papildinys pasirodys Papildinių parduotuvėje visiems vartotojams!

---

## 🔀 Kaip pateikti „Pull Request“

### Pagrindinei programai (wia-soom)

1. Fork repository
2. Sukurkite funkcijų šaką: `git checkout -b feat/my-feature`
3. Padarykite savo pakeitimus
4. Išbandykite lokaliai:
   ```bash
   ```
5. Įsipareigokite su aiškiu pranešimu:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ir atidarykite PR prieš `main`

### Įsipareigojimo pranešimo konvencija

| Prefiksas | Naudojamas |
|-----------|------------|
| `feat:`   | Nauja funkcija |
| `fix:`    | Klaidos taisymas |
| `docs:`   | Tik dokumentacija |
| `refactor:` | Kodo pertvarkymas (be elgesio pokyčių) |
| `i18n:`   | Vertimo atnaujinimai |
| `plugin:` | Su papildiniais susiję pakeitimai |

### PR kontrolinis sąrašas

- [ ] Kodas veikia be klaidų
- [ ] Nėra kietai užkoduotų tekstų (naudokite i18n raktus)
- [ ] Nėra `console.log` paliktų gamybos kode
- [ ] Esami testai vis dar praeina

---

## 🌐 Vertimo indėliai (254 kalbos)

WIA SOOM palaiko **254 kalbas** — nuo amharių iki zulų, įskaitant Brailio ir RTL kalbas.

### Kaip veikia vertimas

- Pagrindinis kalbos failas: `src/renderer/src/i18n/en.json`
- Visi 254 kalbos failai yra toje pačioje direktorijoje
- Vertimas atliekamas naudojant `scripts/translate-patch.js` (GPT-4o-mini API)

### Kaip prisidėti prie vertimų

#### Pasirinkimas 1: Ištaisyti konkrečią vertimą

1. Suraskite kalbos failą: `src/renderer/src/i18n/{lang-code}.json`
2. Ištaisyti neteisingą vertimą
3. Pateikite PR su pakeitimu

#### Pasirinkimas 2: Pridėti trūkstamus raktus
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Pasirinkimas 3: Peržiūrėti mašininį vertimą

Daugelis mūsų 254 kalbų buvo išverstos mašinomis. Gimtakalbių peržiūros yra neįkainojamos!

1. Pasirinkite savo kalbos failą
2. Peržiūrėkite vertimus
3. Ištaisyti bet kokius nepatogius ar neteisingus vertimus
4. Pateikite PR

### Kalbos kodai

Mes naudojame standartinius ISO 639-1 kodus (pvz., `ko`, `en`, `ja`, `ar`, `hi`) su regioniniais variantais, kai reikia (pvz., `zh-CN`, `pt-BR`).

---

## 🛠 Plėtros nustatymas

### Reikalavimai

- Node.js 18+
- npm 9+
- Git

### Nustatymas
```bash
```
### Kompiliacija
```bash
```
> Pastaba: Numatytoji 2GB krūva nėra pakankama dėl 254 kalbos failų + Monaco redaktoriaus paketo (~38MB renderer).

### Projekto struktūra
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

## 🙏 Ačiū

Kiekvienas indėlis daro WIA SOOM geresnį kūrėjams visame pasaulyje.

Ar taisote rašybos klaidą, verčiate eilutę, kuriate papildinį ar pridedate didelę funkciją — **jūs esate šios istorijos dalis.**

---

<p align="center"><em>Sukurtas su ❤️ SmileStory Inc. ir pasaulio prisidėjusiųjų.</em></p>