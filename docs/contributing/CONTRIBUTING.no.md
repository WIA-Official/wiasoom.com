<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Bidra til WIA SOOM</h1>
<p align="center"><strong>Vi setter pris på dine bidrag!</strong></p>
<p align="center">Enten det er en feilretting, ny funksjon, plugin eller oversettelse — hvert bidrag teller.</p>

---

## Innholdsfortegnelse

- [Kode for oppførsel](#kode-for-oppførsel)
- [Hvordan rapportere feil](#-hvordan-rapportere-feil)
- [Hvordan foreslå funksjoner](#-hvordan-foreslå-funksjoner)
- [Hvordan sende inn en plugin](#-hvordan-sende-inn-en-plugin)
- [Hvordan sende inn en Pull Request](#-hvordan-sende-inn-en-pull-request)
- [Oversettelsesbidrag (254 språk)](#-oversettelsesbidrag-254-språk)
- [Utviklingsoppsett](#-utviklingsoppsett)

---

## Kode for oppførsel

Vi er forpliktet til å gi en velkommen og inkluderende opplevelse for alle.

- **Vær respektfull.** Behandle alle med verdighet.
- **Vær konstruktiv.** Gi nyttig tilbakemelding, ikke destruktiv kritikk.
- **Vær inkluderende.** Vi støtter 254 språk og ønsker bidragsytere fra alle land på jorden velkommen.
- **Ingen trakassering.** Nulltoleranse for diskriminering av noe slag.

---

## 🐛 Hvordan rapportere feil

1. Gå til [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikk på **"New Issue"**
3. Velg **"Bug Report"** malen
4. Inkluder:
   - WIA SOOM versjon (Innstillinger → Om)
   - OS og versjon (Windows/macOS/Linux)
   - Trinn for å gjenskape
   - Forventet vs. faktisk oppførsel
   - Skjermbilder eller terminalutdata hvis mulig

---

## 💡 Hvordan foreslå funksjoner

1. Gå til [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikk på **"New Issue"**
3. Velg **"Feature Request"** malen
4. Beskriv:
   - Hvilket problem du løser
   - Hvordan du forestiller deg at det fungerer
   - Eventuelle alternativer du har vurdert

---

## 🔌 Hvordan sende inn en plugin

WIA SOOM har et kraftig pluginsystem — du kan bygge din egen plugin på 5 minutter.

### Rask start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full guide

Les **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** for:
- Fullstendig API-referanse
- Arbeidseksempler
- Trinn-for-trinn opplæringer
- Beste praksiser og sikkerhetsregler

### Send inn din plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Legg til din plugin i `plugins/{your-plugin-name}/`
3. Send inn en Pull Request
4. Etter gjennomgang, vil din plugin vises i Plugin Store for alle brukere!

---

## 🔀 Hvordan sende inn en Pull Request

### For hovedappen (wia-soom)

1. Fork repositoriet
2. Opprett en funksjonsgren: `git checkout -b feat/my-feature`
3. Gjør endringene dine
4. Test lokalt:
   ```bash
   ```
5. Commit med en klar melding:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push og åpne en PR mot `main`

### Commit-meldingskonvensjon

| Prefiks | Bruk for |
|---------|----------|
| `feat:` | Ny funksjon |
| `fix:`  | Feilretting |
| `docs:` | Kun dokumentasjon |
| `refactor:` | Omstrukturering av kode (ingen endring i oppførsel) |
| `i18n:` | Oversettelsesoppdateringer |
| `plugin:` | Plugin-relaterte endringer |

### PR-sjekkliste

- [ ] Koden kjører uten feil
- [ ] Ingen hardkodede strenger (bruk i18n-nøkler)
- [ ] Ingen `console.log` igjen i produksjonskode
- [ ] Eksisterende tester passer fortsatt

---

## 🌐 Oversettelsesbidrag (254 språk)

WIA SOOM støtter **254 språk** — fra amharisk til zulu, inkludert punktskrift og RTL-språk.

### Hvordan oversettelse fungerer

- Basisspråkfil: `src/renderer/src/i18n/en.json`
- Alle 254 språkfiler er i samme katalog
- Oversettelse gjøres via `scripts/translate-patch.js` (GPT-4o-mini API)

### Hvordan bidra med oversettelser

#### Alternativ 1: Fiks en spesifikk oversettelse

1. Finn språkfilen: `src/renderer/src/i18n/{lang-code}.json`
2. Fiks den feilaktige oversettelsen
3. Send inn en PR med endringen

#### Alternativ 2: Legg til manglende nøkler
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Alternativ 3: Gjennomgå maskinoversettelser

Mange av våre 254 språk ble maskinoversatt. Gjennomganger fra morsmålstalere er utrolig verdifulle!

1. Velg språkfilen din
2. Gjennomgå oversettelsene
3. Fiks eventuelle klønete eller feilaktige oversettelser
4. Send inn en PR

### Språkkoder

Vi bruker standard ISO 639-1 koder (f.eks., `ko`, `en`, `ja`, `ar`, `hi`) med regionale varianter der det er nødvendig (f.eks., `zh-CN`, `pt-BR`).

---

## 🛠 Utviklingsoppsett

### Forutsetninger

- Node.js 18+
- npm 9+
- Git

### Oppsett
```bash
```
### Bygg
```bash
```
> Merk: Den standard 2GB heap-en er ikke nok på grunn av de 254 språkfilene + Monaco editor-bundlet (~38MB renderer).

### Prosjektstruktur
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

Hver bidrag gjør WIA SOOM bedre for utviklere over hele verden.

Enten du retter en skrivefeil, oversetter en streng, bygger et plugin, eller legger til en stor funksjon — **du er en del av denne historien.**

---

<p align="center"><em>Bygget med ❤️ av SmileStory Inc. og bidragsytere over hele verden.</em></p>