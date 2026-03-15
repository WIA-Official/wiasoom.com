<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuì a WIA SOOM</h1>
<p align="center"><strong>Avremm a piacer i tò contributi!</strong></p>
<p align="center">Sia ch' l'è un bug fix, una nova funzionalità, un plugin, o una traduzione — ogni contributo l'è impurtant.</p>

---

## Indice

- [Codice de Condotta](#code-of-conduct)
- [Cme Riferì i Bug](#-how-to-report-bugs)
- [Cme Suggerì Funzionalità](#-how-to-suggest-features)
- [Cme Sottomettere un Plugin](#-how-to-submit-a-plugin)
- [Cme Sottomettere una Pull Request](#-how-to-submit-a-pull-request)
- [Contributi de Traduzione (254 Lingue)](#-translation-contributions-254-languages)
- [Impostazione de Sviluppo](#-development-setup)

---

## Codice de Condotta

Sémm impegnè a fornì un'esperienza accogliente e inclusiva per tutti.

- **Sè rispettuós.** Trèta tüt i con dignità.
- **Sè costruttiv.** Offrì feedback util, minga critica distruttiva.
- **Sè inclusiv.** Sostenèm 254 lingue e accoglièm contributori da ogni paes del mond.
- **Nissun molestia.** Zero tolleranza per discriminazione de ogni tipo.

---

## 🐛 Cme Riferì i Bug

1. Va a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"New Issue"**
3. Scegli il template **"Bug Report"**
4. Includi:
   - Version de WIA SOOM (Impostazioni → Info)
   - OS e versione (Windows/macOS/Linux)
   - Passi per riprodurre
   - Comportamento atteso vs. reale
   - Screenshot o output de terminal se possibul

---

## 💡 Cme Suggerì Funzionalità

1. Va a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"New Issue"**
3. Scegli il template **"Feature Request"**
4. Descrivi:
   - Qual problema stai risolvendo
   - Cme t'immagini che funzioni
   - Eventuali alternative che hai considerato

---

## 🔌 Cme Sottomettere un Plugin

WIA SOOM l'ha un sistema de plugin potente — t'può costruì il tò plugin in 5 minuti.

### Avvio Veloce
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guida Completa

Leggi il **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Riferimento API completo
- Esempi funzionanti
- Tutorial passo-passo
- Migliori pratiche e regole de sicurezza

### Sottometti il Tò Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Aggiungi il tò plugin a `plugins/{your-plugin-name}/`
3. Sottometti una Pull Request
4. Dopo la revisione, il tò plugin apparirà nel Plugin Store per tutti gli utenti!

---

## 🔀 Cme Sottomettere una Pull Request

### Per l'app principale (wia-soom)

1. Forka il repository
2. Crea un branch de funzionalità: `git checkout -b feat/my-feature`
3. Fai i tò cambiamenti
4. Testa localmente:
   ```bash
   ```
5. Commetti con un messaggio chiaro:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pusha e apri un PR contro `main`

### Convenzione del Messaggio de Commit

| Prefisso | Uso per |
|----------|---------|
| `feat:`  | Nuova funzionalità |
| `fix:`   | Bug fix |
| `docs:`  | Solo documentazione |
| `refactor:` | Ristrutturazione del codice (senza cambiamento di comportamento) |
| `i18n:`  | Aggiornamenti de traduzione |
| `plugin:` | Cambiamenti relativi ai plugin |

### Checklist PR

- [ ] Il codice gira senza errori
- [ ] Nissuna stringa hardcoded (usa le chiavi i18n)
- [ ] Nissun `console.log` lasciato nel codice di produzione
- [ ] I test esistenti continuano a passare

---

## 🌐 Contributi de Traduzione (254 Lingue)

WIA SOOM supporta **254 lingue** — da Amharic a Zulu, inclusi Braille e lingue RTL.

### Cme Funziona la Traduzione

- File lingua base: `src/renderer/src/i18n/en.json`
- Tutti i 254 file lingua sono nella stessa directory
- La traduzione la se fa tramite `scripts/translate-patch.js` (API GPT-4o-mini)

### Cme Contribuire alle Traduzioni

#### Opzione 1: Correggi una traduzione specifica

1. Trova il file lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Correggi la traduzione errata
3. Sottometti un PR con il cambiamento

#### Opzione 2: Aggiungi chiavi mancanti
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opzione 3: Rivedi le traduzioni automatiche

Molte delle nostre 254 lingue sono state tradotte automaticamente. Le revisioni di un madrelingua sono incredibilmente preziose!

1. Scegli il tuo file lingua
2. Rivedi le traduzioni
3. Correggi eventuali traduzioni imbarazzanti o errate
4. Sottometti un PR

### Codici Lingua

Usèm i codici standard ISO 639-1 (es., `ko`, `en`, `ja`, `ar`, `hi`) con varianti regionali dove necessario (es., `zh-CN`, `pt-BR`).

---

## 🛠 Impostazione de Sviluppo

### Prerequisiti

- Node.js 18+
- npm 9+
- Git

### Impostazione
```bash
```
### Build
```bash
```
> Nota: L'heap di default de 2GB non l'è abbastanza a causa dei 254 file lingua + bundle de Monaco editor (~38MB renderer).

### Struttura del Progetto
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

## 🙏 Mèrcia

Ogni contribuziòun la fa WIA SOOM mej par i svilupadùr da tót al mond.

Sia che t'còrreg un errore, t'traduzi un string, t'costruìs un plugin, o t'aggiòng un feature impurtànt — **t'se parte d'questa storia.**

---

<p align="center"><em>Costruìda cun ❤️ da SmileStory Inc. e contribuènt da tót al mond.</em></p>