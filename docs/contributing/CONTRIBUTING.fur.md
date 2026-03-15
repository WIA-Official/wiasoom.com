<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuî a WIA SOOM</h1>
<p align="center"><strong>O vin di plâs che contribuîs!</strong></p>
<p align="center">Che si trate di un bug fix, une nove funzionalitât, un plugin, o une traduzione — ogni contribuizion e je importante.</p>

---

## Tavola di Contenûts

- [Codice di Condûta](#code-of-conduct)
- [Come Reportâ i Bugs](#-how-to-report-bugs)
- [Come Suggerî Funzionalitâts](#-how-to-suggest-features)
- [Come Presentâ un Plugin](#-how-to-submit-a-plugin)
- [Come Presentâ une Pull Request](#-how-to-submit-a-pull-request)
- [Contribuizioni di Traduzione (254 Lingue)](#-translation-contributions-254-languages)
- [Impostazioni di Sviluppo](#-development-setup)

---

## Codice di Condûta

O vin impegnâts a furnî une esperience acoliente e inclusiva par ducj.

- **Esi rispietôs.** Trati ducj cun dignitât.
- **Esi costruttîfs.** Ofri feedback util, no critiche distrutîfs.
- **Esi inclusîfs.** O supportin 254 lingue e o vin benvignûs i contribuîtôrs da ogni paîs de chê Terre.
- **Nissun molestâment.** Toleranze zero par discriminazion di ogni tipe.

---

## 🐛 Come Reportâ i Bugs

1. Vâ a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"New Issue"**
3. Scegli il template **"Bug Report"**
4. Includi:
   - Version di WIA SOOM (Impostazions → Informaçions)
   - SO e version (Windows/macOS/Linux)
   - Passs per riprodûr
   - Comportament spetât vs. real
   - Screenshots o output dal terminal se possibile

---

## 💡 Come Suggerî Funzionalitâts

1. Vâ a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"New Issue"**
3. Scegli il template **"Feature Request"**
4. Descrivi:
   - Quale problema tu stai risolvendo
   - Come tu imagjinis che funzioni
   - Qualsiasi alternativa che tu ai considerât

---

## 🔌 Come Presentâ un Plugin

WIA SOOM al à un sistem di plugin puissânt — tu puedis costruî il to plugin in 5 minuti.

### Avvi Rapid
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guida Completa

Legi il **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** par:
- Riferiment API complet
- Esempli funzionants
- Tutorial passo-passo
- Best practices e regole di sicurezze

### Presenta il To Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Zonti il to plugin in `plugins/{your-plugin-name}/`
3. Presenta une Pull Request
4. Dopo la revisione, il to plugin al compare tal Plugin Store par ducj i utilizadôrs!

---

## 🔀 Come Presentâ une Pull Request

### Par l'app principale (wia-soom)

1. Fork il repository
2. Crea une branche di funzionalitât: `git checkout -b feat/my-feature`
3. Fâ le to modifiche
4. Testa localment:
   ```bash
   ```
5. Commit cun un messaggi clar:
   ```
   feat: zontâ il toggle di dark mode ai impostazions
   ```
6. Push e vâ a une PR contro `main`

### Convenzion di Messaggi di Commit

| Prefix | Usâ par |
|--------|---------|
| `feat:` | Nuove funzionalitâts |
| `fix:` | Bug fix |
| `docs:` | Documentazion dome |
| `refactor:` | Ristrutturazion di code (senza cambiament di comportament) |
| `i18n:` | Aggiornaments di traduzione |
| `plugin:` | Modifiche in relazion ai plugin |

### Checklist di PR

- [ ] Il code al corse senza erors
- [ ] Nissun string hardcoded (usâ i cheis i18n)
- [ ] Nissun `console.log` lassât in code di produzione
- [ ] I test esistents a continuin a passâ

---

## 🌐 Contribuizioni di Traduzione (254 Lingue)

WIA SOOM al supporta **254 lingue** — da Amharic a Zulu, inclusi Braille e lingue RTL.

### Come Funziona la Traduzione

- File di lingue base: `src/renderer/src/i18n/en.json`
- Ducj i 254 file di lingue a son tal stes diretòri
- La traduzione e je fatte tramite `scripts/translate-patch.js` (API GPT-4o-mini)

### Come Contribuî a Traduzioni

#### Opzion 1: Corregi une traduzione specifiche

1. Trovi il file di lingue: `src/renderer/src/i18n/{lang-code}.json`
2. Corregi la traduzione incorrette
3. Presenta une PR cun la modifiche

#### Opzion 2: Zonti cheis mancants
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opzion 3: Revisâ traduzioni automatiche

Molte di lis 254 lingue a son state tradotte a machine. Le revisi di parlanti nativi a son incredibilmente preziose!

1. Scegli il to file di lingue
2. Revisâ le traduzioni
3. Corregi qualsiasi traduzione strana o incorrette
4. Presenta une PR

### Codici di Lingue

Usin codici standard ISO 639-1 (es., `ko`, `en`, `ja`, `ar`, `hi`) cun varianti regionali dove necessarie (es., `zh-CN`, `pt-BR`).

---

## 🛠 Impostazioni di Sviluppo

### Prerequisiti

- Node.js 18+
- npm 9+
- Git

### Impostazioni
```bash
```
### Build
```bash
```
> Nota: Il heap di default di 2GB no je a sufficience par via dai 254 file di lingue + pacchet di Monaco (~38MB renderer).

### Struttura dal Proget
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

## 🙏 Gracie

Ogni contributo fa di WIA SOOM un strument di lavor per i sviluppatori di dut il mond.

Che tu corregis un errore tipografic, tradus un string, costruis un plugin, o zontis une funsion major — **tu fâs parte di cheste storie.**

---

<p align="center"><em>Costruît cun ❤️ da SmileStory Inc. e contributôrs di dut il mond.</em></p>