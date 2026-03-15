<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Bydra tot WIA SOOM</h1>
<p align="center"><strong>Ons waardeer jou bydraes!</strong></p>
<p align="center">Of dit nou 'n fout regstelling, nuwe funksie, plugin, of vertaling is — elke bydrae tel.</p>

---

## Inhoudsopgawe

- [Gedragkode](#code-of-conduct)
- [Hoe om Foute te Rapporteer](#-how-to-report-bugs)
- [Hoe om Funksies voor te stel](#-how-to-suggest-features)
- [Hoe om 'n Plugin in te dien](#-how-to-submit-a-plugin)
- [Hoe om 'n Pull Request in te dien](#-how-to-submit-a-pull-request)
- [Vertaling Bydrae (254 Tale)](#-translation-contributions-254-languages)
- [Ontwikkeling Setup](#-development-setup)

---

## Gedragkode

Ons is daartoe verbind om 'n verwelkomende en inklusiewe ervaring vir almal te bied.

- **Wees respekvol.** Behandel almal met waardigheid.
- **Wees konstruktief.** Bied nuttige terugvoer, nie vernietigende kritiek nie.
- **Wees inklusief.** Ons ondersteun 254 tale en verwelkom bydraers van elke land op aarde.
- **Geen teistering nie.** Nul toleransie vir enige vorm van diskriminasie.

---

## 🐛 Hoe om Foute te Rapporteer

1. Gaan na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik op **"Nuwe Probleem"**
3. Kies die **"Fout Rapport"** sjabloon
4. Sluit in:
   - WIA SOOM weergawe (Instellings → Oor)
   - OS en weergawe (Windows/macOS/Linux)
   - Stappe om te reproduceer
   - Verwachte vs. werklike gedrag
   - Skermskote of terminal-uitset indien moontlik

---

## 💡 Hoe om Funksies voor te stel

1. Gaan na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik op **"Nuwe Probleem"**
3. Kies die **"Funksie Versoek"** sjabloon
4. Beskryf:
   - Watter probleem jy oplos
   - Hoe jy dit voorstel om te werk
   - Enige alternatiewe wat jy oorweeg het

---

## 🔌 Hoe om 'n Plugin in te dien

WIA SOOM het 'n kragtige plugin-stelsel — jy kan jou eie plugin in 5 minute bou.

### Vinige Begin
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Volledige Gids

Lees die **[Plugin Ontwikkelaar Gids](docs/PLUGIN_DEVELOPER_GUIDE.md)** vir:
- Volledige API verwysing
- Werkende voorbeelde
- Stap-vir-stap tutorials
- Beste praktyke en sekuriteitsreëls

### Dien Jou Plugin In

1. Fork [Plugin Store](https://wiasoom.com)
2. Voeg jou plugin by `plugins/{your-plugin-name}/`
3. Dien 'n Pull Request in
4. Na hersiening, verskyn jou plugin in die Plugin Store vir alle gebruikers!

---

## 🔀 Hoe om 'n Pull Request in te dien

### Vir die hoof app (wia-soom)

1. Fork die repo
2. Skep 'n funksie tak: `git checkout -b feat/my-feature`
3. Maak jou veranderinge
4. Toets plaaslik:
   ```bash
   ```
5. Commit met 'n duidelike boodskap:
   ```
   feat: voeg donker modus skakelaar by instellings
   ```
6. Push en open 'n PR teen `main`

### Commit Boodskap Konvensie

| Vooraf | Gebruik vir |
|--------|---------|
| `feat:` | Nuwe funksie |
| `fix:` | Fout regstelling |
| `docs:` | Slegs dokumentasie |
| `refactor:` | Kode herstrukturering (geen gedragsverandering) |
| `i18n:` | Vertaling opdaterings |
| `plugin:` | Plugin-verwante veranderinge |

### PR Kontrolelys

- [ ] Kode loop sonder foute
- [ ] Geen hardgecodeerde strings nie (gebruik i18n sleutels)
- [ ] Geen `console.log` wat in produksiekode gelaat is nie
- [ ] Bestaande toetse slaag steeds

---

## 🌐 Vertaling Bydrae (254 Tale)

WIA SOOM ondersteun **254 tale** — van Amharies tot Zoeloe, insluitend Braille en RTL tale.

### Hoe Vertaling Werk

- Basistaal lêer: `src/renderer/src/i18n/en.json`
- Alle 254 taal lêers is in dieselfde gids
- Vertaling word gedoen via `scripts/translate-patch.js` (GPT-4o-mini API)

### Hoe om Vertalings by te dra

#### Opsie 1: Regstel 'n spesifieke vertaling

1. Vind die taal lêer: `src/renderer/src/i18n/{lang-code}.json`
2. Regstel die onakkurate vertaling
3. Dien 'n PR in met die verandering

#### Opsie 2: Voeg ontbrekende sleutels by
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsie 3: Hersien masjienvertalings

Baie van ons 254 tale is masjienvertaal. Moedertaalsprekers se hersienings is ongelooflik waardevol!

1. Kies jou taal lêer
2. Hersien die vertalings
3. Regstel enige ongemaklike of onakkurate vertalings
4. Dien 'n PR in

### Taal Kodes

Ons gebruik standaard ISO 639-1 kodes (bv., `ko`, `en`, `ja`, `ar`, `hi`) met streekvariantes waar nodig (bv., `zh-CN`, `pt-BR`).

---

## 🛠 Ontwikkeling Setup

### Voorvereistes

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Bou
```bash
```
> Nota: Die standaard 2GB hoop is nie genoeg nie weens die 254 taal lêers + Monaco redigeerder pakket (~38MB renderer).

### Projek Struktuur
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

## 🙏 Dankie

Elke bydrae maak WIA SOOM beter vir ontwikkelaars regoor die wêreld.

Of jy 'n tikfout regmaak, 'n string vertaal, 'n plugin bou, of 'n groot kenmerk toevoeg — **jy is deel van hierdie storie.**

---

<p align="center"><em>Gebou met ❤️ deur SmileStory Inc. en bydraers wêreldwyd.</em></p>