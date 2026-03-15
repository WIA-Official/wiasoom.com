<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Bijdragen aan WIA SOOM</h1>
<p align="center"><strong>We waarderen je bijdragen!</strong></p>
<p align="center">Of het nu gaat om een bugfix, nieuwe functie, plugin of vertaling — elke bijdrage is belangrijk.</p>

---

## Inhoudsopgave

- [Gedragscode](#code-of-conduct)
- [Hoe Bugs Rapporteren](#-how-to-report-bugs)
- [Hoe Functies Voorstellen](#-how-to-suggest-features)
- [Hoe een Plugin Indienen](#-how-to-submit-a-plugin)
- [Hoe een Pull Request Indienen](#-how-to-submit-a-pull-request)
- [Vertaalbijdragen (254 Talen)](#-translation-contributions-254-languages)
- [Ontwikkelingssetup](#-development-setup)

---

## Gedragscode

We zijn toegewijd aan het bieden van een gastvrije en inclusieve ervaring voor iedereen.

- **Wees respectvol.** Behandel iedereen met waardigheid.
- **Wees constructief.** Bied nuttige feedback, geen destructieve kritiek.
- **Wees inclusief.** We ondersteunen 254 talen en verwelkomen bijdragers uit elk land op aarde.
- **Geen intimidatie.** Nul tolerantie voor discriminatie van welke aard dan ook.

---

## 🐛 Hoe Bugs Rapporteren

1. Ga naar [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik op **"Nieuwe Probleem"**
3. Kies de **"Bug Rapport"** sjabloon
4. Vermeld:
   - WIA SOOM versie (Instellingen → Over)
   - OS en versie (Windows/macOS/Linux)
   - Stappen om te reproduceren
   - Verwacht versus werkelijke gedrag
   - Screenshots of terminaluitvoer indien mogelijk

---

## 💡 Hoe Functies Voorstellen

1. Ga naar [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik op **"Nieuwe Probleem"**
3. Kies de **"Functie Verzoek"** sjabloon
4. Beschrijf:
   - Welk probleem je oplost
   - Hoe je het je voorstelt dat het werkt
   - Eventuele alternatieven die je hebt overwogen

---

## 🔌 Hoe een Plugin Indienen

WIA SOOM heeft een krachtig pluginsysteem — je kunt je eigen plugin in 5 minuten bouwen.

### Snelle Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Volledige Gids

Lees de **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** voor:
- Volledige API-referentie
- Werkende voorbeelden
- Stapsgewijze tutorials
- Beste praktijken en veiligheidsregels

### Dien Je Plugin In

1. Fork [Plugin Store](https://wiasoom.com)
2. Voeg je plugin toe aan `plugins/{your-plugin-name}/`
3. Dien een Pull Request in
4. Na beoordeling verschijnt je plugin in de Plugin Store voor alle gebruikers!

---

## 🔀 Hoe een Pull Request Indienen

### Voor de hoofdapp (wia-soom)

1. Fork de repository
2. Maak een feature branch: `git checkout -b feat/my-feature`
3. Maak je wijzigingen
4. Test lokaal:
   ```bash
   ```
5. Commit met een duidelijke boodschap:
   ```
   feat: voeg donkere modus toggle toe aan instellingen
   ```
6. Push en open een PR tegen `main`

### Commit Bericht Conventie

| Prefix | Gebruik voor |
|--------|--------------|
| `feat:` | Nieuwe functie |
| `fix:` | Bugfix |
| `docs:` | Alleen documentatie |
| `refactor:` | Code herschikking (geen gedragsverandering) |
| `i18n:` | Vertaalupdates |
| `plugin:` | Plugin-gerelateerde wijzigingen |

### PR Checklist

- [ ] Code draait zonder fouten
- [ ] Geen hardcoded strings (gebruik i18n sleutels)
- [ ] Geen `console.log` achtergelaten in productiecoded
- [ ] Bestaande tests slagen nog steeds

---

## 🌐 Vertaalbijdragen (254 Talen)

WIA SOOM ondersteunt **254 talen** — van Amhaars tot Zulu, inclusief Braille en RTL-talen.

### Hoe Vertaling Werkt

- Basistaalbestand: `src/renderer/src/i18n/en.json`
- Alle 254 taalbestanden bevinden zich in dezelfde map
- Vertaling gebeurt via `scripts/translate-patch.js` (GPT-4o-mini API)

### Hoe Bijdragen aan Vertalingen

#### Optie 1: Een specifieke vertaling corrigeren

1. Zoek het taalbestand: `src/renderer/src/i18n/{lang-code}.json`
2. Corrigeer de onjuiste vertaling
3. Dien een PR in met de wijziging

#### Optie 2: Ontbrekende sleutels toevoegen
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Optie 3: Machinevertalingen beoordelen

Veel van onze 254 talen zijn machinevertald. Beoordelingen door moedertaalsprekers zijn ongelooflijk waardevol!

1. Kies je taalbestand
2. Beoordeel de vertalingen
3. Corrigeer eventuele onhandige of onjuiste vertalingen
4. Dien een PR in

### Taalcodes

We gebruiken standaard ISO 639-1 codes (bijv., `ko`, `en`, `ja`, `ar`, `hi`) met regionale varianten waar nodig (bijv., `zh-CN`, `pt-BR`).

---

## 🛠 Ontwikkelingssetup

### Vereisten

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Bouw
```bash
```
> Opmerking: De standaard 2GB heap is niet genoeg vanwege de 254 taalbestanden + Monaco editor bundle (~38MB renderer).

### Projectstructuur
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

## 🙏 Bedankt

Elke bijdrage maakt WIA SOOM beter voor ontwikkelaars over de hele wereld.

Of je nu een typfout corrigeert, een string vertaalt, een plugin bouwt of een belangrijke functie toevoegt — **je maakt deel uit van dit verhaal.**

---

<p align="center"><em>Gebouwd met ❤️ door SmileStory Inc. en bijdragers wereldwijd.</em></p>