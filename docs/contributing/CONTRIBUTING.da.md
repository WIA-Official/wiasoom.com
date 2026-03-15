<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Bidrag til WIA SOOM</h1>
<p align="center"><strong>Vi vil elske dine bidrag!</strong></p>
<p align="center">Uanset om det er en fejlrettelse, ny funktion, plugin eller oversættelse — hver bidrag tæller.</p>

---

## Indholdsfortegnelse

- [Adfærdskodeks](#code-of-conduct)
- [Sådan rapporteres fejl](#-how-to-report-bugs)
- [Sådan foreslås funktioner](#-how-to-suggest-features)
- [Sådan indsendes et plugin](#-how-to-submit-a-plugin)
- [Sådan indsendes en pull-anmodning](#-how-to-submit-a-pull-request)
- [Oversættelsesbidrag (254 sprog)](#-translation-contributions-254-languages)
- [Udviklingsopsætning](#-development-setup)

---

## Adfærdskodeks

Vi er forpligtet til at give en velkomponeret og inkluderende oplevelse for alle.

- **Vær respektfuld.** Behandl alle med værdighed.
- **Vær konstruktiv.** Giv nyttig feedback, ikke destruktiv kritik.
- **Vær inkluderende.** Vi understøtter 254 sprog og byder bidragydere fra hvert land på Jorden velkommen.
- **Ingen chikane.** Nul tolerance for diskrimination af enhver art.

---

## 🐛 Sådan rapporteres fejl

1. Gå til [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik på **"New Issue"**
3. Vælg **"Bug Report"** skabelonen
4. Inkluder:
   - WIA SOOM version (Indstillinger → Om)
   - OS og version (Windows/macOS/Linux)
   - Trin til at reproducere
   - Forventet vs. faktisk adfærd
   - Skærmbilleder eller terminaloutput hvis muligt

---

## 💡 Sådan foreslås funktioner

1. Gå til [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik på **"New Issue"**
3. Vælg **"Feature Request"** skabelonen
4. Beskriv:
   - Hvilket problem du løser
   - Hvordan du forestiller dig, at det fungerer
   - Eventuelle alternativer, du har overvejet

---

## 🔌 Sådan indsendes et plugin

WIA SOOM har et kraftfuldt pluginsystem — du kan bygge dit eget plugin på 5 minutter.

### Hurtig start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Fuld guide

Læs **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** for:
- Fuld API-referencer
- Arbejdseksempler
- Trin-for-trin vejledninger
- Bedste praksis og sikkerhedsregler

### Indsend dit plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Tilføj dit plugin til `plugins/{your-plugin-name}/`
3. Indsend en pull-anmodning
4. Efter gennemgang vises dit plugin i Plugin Store for alle brugere!

---

## 🔀 Sådan indsendes en pull-anmodning

### For hovedappen (wia-soom)

1. Fork repositoryet
2. Opret en funktionsgren: `git checkout -b feat/my-feature`
3. Foretag dine ændringer
4. Test lokalt:
   ```bash
   ```
5. Commit med en klar besked:
   ```
   feat: tilføj mørk tilstandsknap til indstillinger
   ```
6. Push og åbn en PR mod `main`

### Commit-beskedkonvention

| Præfiks | Brug til |
|---------|---------|
| `feat:` | Ny funktion |
| `fix:` | Fejlrettelse |
| `docs:` | Kun dokumentation |
| `refactor:` | Omstrukturering af kode (ingen adfærdsændring) |
| `i18n:` | Oversættelsesopdateringer |
| `plugin:` | Plugin-relaterede ændringer |

### PR-tjekliste

- [ ] Koden kører uden fejl
- [ ] Ingen hardkodede strenge (brug i18n-nøgler)
- [ ] Ingen `console.log` tilbage i produktionskode
- [ ] Eksisterende tests passer stadig

---

## 🌐 Oversættelsesbidrag (254 sprog)

WIA SOOM understøtter **254 sprog** — fra amharisk til zulu, inklusive punktskrift og RTL-sprog.

### Sådan fungerer oversættelse

- Basis sprogfil: `src/renderer/src/i18n/en.json`
- Alle 254 sprog filer er i samme mappe
- Oversættelse udføres via `scripts/translate-patch.js` (GPT-4o-mini API)

### Sådan bidrager du med oversættelser

#### Mulighed 1: Ret en specifik oversættelse

1. Find sprogfilen: `src/renderer/src/i18n/{lang-code}.json`
2. Ret den forkerte oversættelse
3. Indsend en PR med ændringen

#### Mulighed 2: Tilføj manglende nøgler
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Mulighed 3: Gennemgå maskinoversættelser

Mange af vores 254 sprog blev maskinoversat. Gennemgange fra modersmålstalere er utrolig værdifulde!

1. Vælg din sprogfil
2. Gennemgå oversættelserne
3. Ret eventuelle akavede eller forkerte oversættelser
4. Indsend en PR

### Sprogkoder

Vi bruger standard ISO 639-1 koder (f.eks. `ko`, `en`, `ja`, `ar`, `hi`) med regionale varianter hvor det er nødvendigt (f.eks. `zh-CN`, `pt-BR`).

---

## 🛠 Udviklingsopsætning

### Forudsætninger

- Node.js 18+
- npm 9+
- Git

### Opsætning
```bash
```
### Byg
```bash
```
> Bemærk: Den standard 2GB heap er ikke nok på grund af de 254 sprog filer + Monaco editor bundle (~38MB renderer).

### Projektstruktur
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

## 🙏 Tak

Hver bidrag gør WIA SOOM bedre for udviklere over hele verden.

Uanset om du retter en tastefejl, oversætter en streng, bygger et plugin eller tilføjer en stor funktion — **du er en del af denne historie.**

---

<p align="center"><em>Bygget med ❤️ af SmileStory Inc. og bidragydere verden over.</em></p>