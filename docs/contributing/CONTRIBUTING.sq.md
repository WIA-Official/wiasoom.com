<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontributi në WIA SOOM</h1>
<p align="center"><strong>Na pëlqen kontributi juaj!</strong></p>
<p align="center">Nëse është një rregullim gabimi, një veçori e re, një plugin, ose një përkthim — çdo kontribut ka rëndësi.</p>

---

## Tabela e Përmbajtjes

- [Kodi i Sjelljes](#code-of-conduct)
- [Si të Raportoni Gabimet](#-how-to-report-bugs)
- [Si të Sugjeroni Veçori](#-how-to-suggest-features)
- [Si të Dërgoni një Plugin](#-how-to-submit-a-plugin)
- [Si të Dërgoni një Pull Request](#-how-to-submit-a-pull-request)
- [Kontributet e Përkthimit (254 Gjuhë)](#-translation-contributions-254-languages)
- [Konfigurimi i Zhvillimit](#-development-setup)

---

## Kodi i Sjelljes

Ne jemi të angazhuar për të ofruar një përvojë mikpritëse dhe përfshirëse për të gjithë.

- **Bëni respektues.** Trajtoni të gjithë me dinjitet.
- **Bëni konstruktiv.** Ofroni feedback ndihmues, jo kritikë shkatërruese.
- **Bëni përfshirës.** Ne mbështesim 254 gjuhë dhe mirëpresim kontribues nga çdo vend në Tokë.
- **Asnjë ngacmim.** Zero tolerancë për diskriminimin e çdo lloji.

---

## 🐛 Si të Raportoni Gabimet

1. Shkoni në [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikoni **"New Issue"**
3. Zgjidhni shabllonin **"Bug Report"**
4. Përfshini:
   - Versionin e WIA SOOM (Cilësimet → Rreth)
   - OS dhe versionin (Windows/macOS/Linux)
   - Hapat për të riprodhuar
   - Sjellja e pritur vs. e vërtetë
   - Pamje të ekranit ose dalje terminali nëse është e mundur

---

## 💡 Si të Sugjeroni Veçori

1. Shkoni në [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikoni **"New Issue"**
3. Zgjidhni shabllonin **"Feature Request"**
4. Përshkruani:
   - Cilin problem po zgjidhni
   - Si e imagjinoni se do të funksionojë
   - Çfarëdo alternativash që keni marrë parasysh

---

## 🔌 Si të Dërgoni një Plugin

WIA SOOM ka një sistem të fuqishëm plugin — mund të ndërtoni plugin tuaj në 5 minuta.

### Fillimi i Shpejtë
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Udhëzuesi i Plotë

Lexoni **[Udhëzuesin për Zhvilluesit e Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** për:
- Referencën e plotë të API
- Shembuj funksionalë
- Tutoriale hap pas hapi
- Praktikat më të mira dhe rregullat e sigurisë

### Dërgoni Plugin-in Tuaj

1. Fork [Plugin Store](https://wiasoom.com)
2. Shtoni plugin-in tuaj në `plugins/{emri-i-plugin-it-tuaj}/`
3. Dërgoni një Pull Request
4. Pas rishikimit, plugin-i juaj shfaqet në Dyqanin e Plugin për të gjithë përdoruesit!

---

## 🔀 Si të Dërgoni një Pull Request

### Për aplikacionin kryesor (wia-soom)

1. Fork repo-në
2. Krijoni një degë veçorie: `git checkout -b feat/my-feature`
3. Bëni ndryshimet tuaja
4. Testoni lokalisht:
   ```bash
   ```
5. Komitoni me një mesazh të qartë:
   ```
   feat: shtoni kalimin në modalitetin e errët në cilësime
   ```
6. Dërgoni dhe hapni një PR ndaj `main`

### Konvencioni i Mesazhit të Komitit

| Prefix | Përdoret për |
|--------|--------------|
| `feat:` | Veçori e re |
| `fix:` | Rregullim gabimi |
| `docs:` | Vetëm dokumentacion |
| `refactor:` | Ristrukturim kodi (pa ndryshim sjelljeje) |
| `i18n:` | Përditësime përkthimi |
| `plugin:` | Ndryshime të lidhura me plugin |

### Lista e Kontrollit të PR

- [ ] Kodi funksionon pa gabime
- [ ] Asnjë varg i koduar fort (përdorni çelësat i18n)
- [ ] Asnjë `console.log` e lënë në kodin e prodhimit
- [ ] Testet ekzistuese ende kalojnë

---

## 🌐 Kontributet e Përkthimit (254 Gjuhë)

WIA SOOM mbështet **254 gjuhë** — nga Amharic në Zulu, duke përfshirë Braille dhe gjuhë RTL.

### Si Funksionon Përkthimi

- Skedari i gjuhës bazë: `src/renderer/src/i18n/en.json`
- Të gjitha 254 skedarët e gjuhëve janë në të njëjtin direktor
- Përkthimi bëhet përmes `scripts/translate-patch.js` (GPT-4o-mini API)

### Si të Kontribuoni me Përkthime

#### Opsioni 1: Rregulloni një përkthim të caktuar

1. Gjeni skedarin e gjuhës: `src/renderer/src/i18n/{lang-code}.json`
2. Rregulloni përkthimin e gabuar
3. Dërgoni një PR me ndryshimin

#### Opsioni 2: Shtoni çelësa të munguar
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsioni 3: Rishikoni përkthimet e makinerive

Shumë nga 254 gjuhët tona janë përkthyer nga makinat. Rishikimet e folësve natyrorë janë jashtëzakonisht të vlefshme!

1. Zgjidhni skedarin tuaj të gjuhës
2. Rishikoni përkthimet
3. Rregulloni çdo përkthim të çuditshëm ose të gabuar
4. Dërgoni një PR

### Kodet e Gjuhëve

Ne përdorim kodet standarde ISO 639-1 (p.sh., `ko`, `en`, `ja`, `ar`, `hi`) me variante rajonale kur është e nevojshme (p.sh., `zh-CN`, `pt-BR`).

---

## 🛠 Konfigurimi i Zhvillimit

### Parakushtet

- Node.js 18+
- npm 9+
- Git

### Konfigurimi
```bash
```
### Ndërtimi
```bash
```
> Shënim: Heap-i i paracaktuar 2GB nuk është i mjaftueshëm për shkak të 254 skedarëve të gjuhëve + paketimi i editorit Monaco (~38MB renderer).

### Struktura e Projektit
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

## 🙏 Faleminderit

Çdo kontribut e bën WIA SOOM më të mirë për zhvilluesit në mbarë botën.

Pavarësisht nëse korrigjoni një gabim, përktheni një varg, ndërtoni një plugin, ose shtoni një veçori të madhe — **ju jeni pjesë e kësaj historie.**

---

<p align="center"><em>Ndërtuar me ❤️ nga SmileStory Inc. dhe kontribues të mbarë botës.</em></p>