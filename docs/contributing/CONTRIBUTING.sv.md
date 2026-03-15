<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Bidra till WIA SOOM</h1>
<p align="center"><strong>Vi skulle älska dina bidrag!</strong></p>
<p align="center">Oavsett om det handlar om en buggfix, ny funktion, plugin eller översättning — varje bidrag räknas.</p>

---

## Innehållsförteckning

- [Uppförandekod](#code-of-conduct)
- [Hur man rapporterar buggar](#-how-to-report-bugs)
- [Hur man föreslår funktioner](#-how-to-suggest-features)
- [Hur man skickar in en plugin](#-how-to-submit-a-plugin)
- [Hur man skickar in en pull-begäran](#-how-to-submit-a-pull-request)
- [Översättningsbidrag (254 språk)](#-translation-contributions-254-languages)
- [Utvecklingsinställning](#-development-setup)

---

## Uppförandekod

Vi är engagerade i att erbjuda en välkomnande och inkluderande upplevelse för alla.

- **Var respektfull.** Behandla alla med värdighet.
- **Var konstruktiv.** Ge hjälpsam feedback, inte destruktiv kritik.
- **Var inkluderande.** Vi stödjer 254 språk och välkomnar bidragsgivare från alla länder på jorden.
- **Ingen trakasseri.** Nolltolerans för diskriminering av något slag.

---

## 🐛 Hur man rapporterar buggar

1. Gå till [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klicka på **"New Issue"**
3. Välj **"Bug Report"**-mallen
4. Inkludera:
   - WIA SOOM-version (Inställningar → Om)
   - OS och version (Windows/macOS/Linux)
   - Steg för att reproducera
   - Förväntat vs. faktiskt beteende
   - Skärmdumpar eller terminalutdata om möjligt

---

## 💡 Hur man föreslår funktioner

1. Gå till [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klicka på **"New Issue"**
3. Välj **"Feature Request"**-mallen
4. Beskriv:
   - Vilket problem du löser
   - Hur du föreställer dig att det fungerar
   - Eventuella alternativ du har övervägt

---

## 🔌 Hur man skickar in en plugin

WIA SOOM har ett kraftfullt pluginsystem — du kan bygga din egen plugin på 5 minuter.

### Snabbstart
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Fullständig guide

Läs **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** för:
- Komplett API-referens
- Arbets exempel
- Steg-för-steg handledningar
- Bästa praxis och säkerhetsregler

### Skicka in din plugin

1. Forka [Plugin Store](https://wiasoom.com)
2. Lägg till din plugin i `plugins/{your-plugin-name}/`
3. Skicka in en Pull Request
4. Efter granskning kommer din plugin att visas i Plugin Store för alla användare!

---

## 🔀 Hur man skickar in en pull-begäran

### För huvudappen (wia-soom)

1. Forka repositoryt
2. Skapa en funktionsgren: `git checkout -b feat/my-feature`
3. Gör dina ändringar
4. Testa lokalt:
   ```bash
   ```
5. Commita med ett tydligt meddelande:
   ```
   feat: lägg till mörkt läge växlare i inställningar
   ```
6. Push och öppna en PR mot `main`

### Konvention för commit-meddelanden

| Prefix | Används för |
|--------|-------------|
| `feat:` | Ny funktion |
| `fix:` | Buggfix |
| `docs:` | Endast dokumentation |
| `refactor:` | Omstrukturering av kod (ingen beteendeförändring) |
| `i18n:` | Översättningsuppdateringar |
| `plugin:` | Plugin-relaterade ändringar |

### PR-checklista

- [ ] Koden körs utan fel
- [ ] Inga hårdkodade strängar (använd i18n-nycklar)
- [ ] Inga `console.log` kvar i produktionskod
- [ ] Befintliga tester passerar fortfarande

---

## 🌐 Översättningsbidrag (254 språk)

WIA SOOM stödjer **254 språk** — från amhariska till zulu, inklusive punktskrift och RTL-språk.

### Hur översättning fungerar

- Bas språkfil: `src/renderer/src/i18n/en.json`
- Alla 254 språkfiler finns i samma katalog
- Översättning görs via `scripts/translate-patch.js` (GPT-4o-mini API)

### Hur man bidrar med översättningar

#### Alternativ 1: Åtgärda en specifik översättning

1. Hitta språkfilen: `src/renderer/src/i18n/{lang-code}.json`
2. Åtgärda den felaktiga översättningen
3. Skicka in en PR med ändringen

#### Alternativ 2: Lägg till saknade nycklar
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Alternativ 3: Granska maskinöversättningar

Många av våra 254 språk har maskinöversatts. Granskningar av modersmålstalare är otroligt värdefulla!

1. Välj din språkfil
2. Granska översättningarna
3. Åtgärda eventuella klumpiga eller felaktiga översättningar
4. Skicka in en PR

### Språkkoder

Vi använder standard ISO 639-1-koder (t.ex. `ko`, `en`, `ja`, `ar`, `hi`) med regionala varianter där det behövs (t.ex. `zh-CN`, `pt-BR`).

---

## 🛠 Utvecklingsinställning

### Förutsättningar

- Node.js 18+
- npm 9+
- Git

### Inställning
```bash
```
### Bygg
```bash
```
> Obs: Den standard 2GB heap är inte tillräcklig på grund av de 254 språkfilerna + Monaco-editorbundlet (~38MB renderer).

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

## 🙏 Tack

Varje bidrag gör WIA SOOM bättre för utvecklare runt om i världen.

Oavsett om du rättar ett stavfel, översätter en sträng, bygger ett plugin eller lägger till en stor funktion — **du är en del av denna historia.**

---

<p align="center"><em>Byggt med ❤️ av SmileStory Inc. och bidragsgivare världen över.</em></p>