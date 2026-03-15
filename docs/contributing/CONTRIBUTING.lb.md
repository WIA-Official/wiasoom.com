<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Bäitrëg zu WIA SOOM</h1>
<p align="center"><strong>Mir géifen eis Äre Bäiträg gär!</strong></p>
<p align="center">Ob et eng Bugfix, eng nei Feature, e Plugin oder eng Iwwersetzung ass — all Bäitrag zielt.</p>

---

## Inhalter

- [Code of Conduct](#code-of-conduct)
- [Wéi fir Bugs ze mellen](#-wéi-fir-bugs-ze-mellen)
- [Wéi fir Features ze proposéieren](#-wéi-fir-features-ze-proposéieren)
- [Wéi fir e Plugin anzerechnen](#-wéi-fir-e-plugin-anzerechnen)
- [Wéi fir eng Pull Request ze soumissiounen](#-wéi-fir-eng-pull-request-ze-soumissiounen)
- [Iwwersetzungsbäiträg (254 Sproochen)](#-iwwersetzungsbäiträg-254-sproochen)
- [Entwécklungssetup](#-entwécklungssetup)

---

## Code of Conduct

Mir sinn engagéiert fir eng wëllkomm a inklusiv Erfahrung fir jiddereen ze bidden.

- **Sei respektvoll.** Behandelt jiddereen mat Würde.
- **Sei konstruktiv.** Bitt hëllefräich Feedback, net destruktiv Kritik.
- **Sei inklusiv.** Mir ënnerstëtzen 254 Sproochen a wëllkommen Bäiträg aus all Land op der Welt.
- **Keng Belästigung.** Null Toleranz fir Diskriminatioun vun all Art.

---

## 🐛 Wéi fir Bugs ze mellen

1. Gitt op [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klickt op **"Neie Problem"**
3. Wielt de **"Bug Report"** Template
4. Füügt folgendes derbäi:
   - WIA SOOM Versioun (Astellungen → Iwwer)
   - OS a Versioun (Windows/macOS/Linux)
   - Schrëtt fir ze reproduzéieren
   - Erwaart vs. tatsächlech Verhalen
   - Screenshots oder Terminalausgabe wann et méiglech ass

---

## 💡 Wéi fir Features ze proposéieren

1. Gitt op [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klickt op **"Neie Problem"**
3. Wielt de **"Feature Request"** Template
4. Beschreift:
   - Wéi eng Problemer Dir léisst
   - Wéi Dir Iech virstellt datt et funktionéiert
   - All Alternativen déi Dir berücksichtegt hutt

---

## 🔌 Wéi fir e Plugin anzerechnen

WIA SOOM huet e mächtege Plugin-System — Dir kënnt Äre eigene Plugin an 5 Minutten bauen.

### Schnellstart
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Vollstänneg Guide

Liest de **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** fir:
- Komplett API Referenz
- Fonctionnéierend Beispiller
- Schrëtt-fir-Schrëtt Tutorials
- Bescht Praktiken an Sécherheetsregelen

### Soumissioun vun Ärem Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Füügt Ären Plugin zu `plugins/{your-plugin-name}/` derbäi
3. Soumissioun vun enger Pull Request
4. No der Iwwerpréiwung, erschéngt Ären Plugin am Plugin Store fir all Benotzer!

---

## 🔀 Wéi fir eng Pull Request ze soumissiounen

### Fir d'Haupt-App (wia-soom)

1. Fork de Repository
2. Erstellt eng Feature Branch: `git checkout -b feat/my-feature`
3. Maacht Är Ännerungen
4. Test lokal:
   ```bash
   ```
5. Commit mat enger kloerer Message:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push an opent eng PR géint `main`

### Commit Message Konventioun

| Prefix | Benotzt fir |
|--------|-------------|
| `feat:` | Nei Feature |
| `fix:` | Bugfix |
| `docs:` | NUR Dokumentatioun |
| `refactor:` | Code Restrukturéierung (keine Verhalensännerung) |
| `i18n:` | Iwwersetzungsupdates |
| `plugin:` | Plugin-bezogene Ännerungen |

### PR Checklist

- [ ] Code leeft ouni Feeler
- [ ] Keng hardcodéiert Strings (benotzt i18n Schlüssel)
- [ ] Keng `console.log` am Produktiounscode
- [ ] Bestehende Tester sinn nach ëmmer korrekt

---

## 🌐 Iwwersetzungsbäiträg (254 Sproochen)

WIA SOOM ënnerstëtzt **254 Sproochen** — vun Amharesch bis Zulu, dorënner Braille a RTL Sproochen.

### Wéi d'Iwwersetzung funktionéiert

- Basis Sprooch Datei: `src/renderer/src/i18n/en.json`
- All 254 Sproochendateien sinn an der selwechter Directory
- D'Iwwersetzung geschitt iwwer `scripts/translate-patch.js` (GPT-4o-mini API)

### Wéi fir Iwwersetzungen ze bäiträchnen

#### Optioun 1: Eng spezifesch Iwwersetzung reparéieren

1. Fannt d'Sprooch Datei: `src/renderer/src/i18n/{lang-code}.json`
2. Reparéiert d'feelerhaft Iwwersetzung
3. Soumissioun vun enger PR mat der Ännerung

#### Optioun 2: Feeler Schlüssel derbäi
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Optioun 3: Maschinn-Iwwersetzungen iwwerpréiwen

Vill vun eisen 254 Sproochen sinn maschinn-ëwwersat. Iwwersetzungen vun nativen Sproochsproofer sinn immens wäertvoll!

1. Wielt Är Sprooch Datei
2. Iwwerpréift d'Iwwersetzungen
3. Reparéiert all ongewéinlech oder falsch Iwwersetzungen
4. Soumissioun vun enger PR

### Sproochencoden

Mir benotzen standard ISO 639-1 Coden (z.B., `ko`, `en`, `ja`, `ar`, `hi`) mat regionalen Varianten wann néideg (z.B., `zh-CN`, `pt-BR`).

---

## 🛠 Entwécklungssetup

### Viraussetzungen

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Notiz: De standard 2GB Heap ass net genuch wéinst den 254 Sproochendateien + Monaco Editor Bundle (~38MB Renderer).

### Projetstruktur
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

## 🙏 Merci

Jede Beiträg mécht WIA SOOM besser fir Entwéckler ronderëm d'Welt.

Ob Dir e Tippfeeler reparéiert, eng Zeil iwwersetzt, e Plugin baut oder eng grouss Feature derbäifügt — **Dir sidd Deel vun dëser Geschicht.**

---

<p align="center"><em>Gebaut mat ❤️ vun SmileStory Inc. an de Beiträger weltwäit.</em></p>