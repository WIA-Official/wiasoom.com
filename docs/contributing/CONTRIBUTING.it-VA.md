<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuire a WIA SOOM</h1>
<p align="center"><strong>Ci piacerebbe ricevere i tuoi contributi!</strong></p>
<p align="center">Che si tratti di una correzione di bug, una nuova funzionalità, un plugin o una traduzione — ogni contributo è importante.</p>

---

## Indice

- [Codice di Condotta](#code-of-conduct)
- [Come Segnalare Bug](#-how-to-report-bugs)
- [Come Suggerire Funzionalità](#-how-to-suggest-features)
- [Come Inviare un Plugin](#-how-to-submit-a-plugin)
- [Come Inviare una Pull Request](#-how-to-submit-a-pull-request)
- [Contributi di Traduzione (254 Lingue)](#-translation-contributions-254-languages)
- [Impostazione dello Sviluppo](#-development-setup)

---

## Codice di Condotta

Ci impegniamo a fornire un'esperienza accogliente e inclusiva per tutti.

- **Sii rispettoso.** Tratta tutti con dignità.
- **Sii costruttivo.** Offri feedback utili, non critiche distruttive.
- **Sii inclusivo.** Supportiamo 254 lingue e accogliamo contributori da ogni paese della Terra.
- **Nessun comportamento molesto.** Zero tolleranza per la discriminazione di qualsiasi tipo.

---

## 🐛 Come Segnalare Bug

1. Vai a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca su **"Nuovo Problema"**
3. Scegli il modello **"Segnalazione di Bug"**
4. Includi:
   - Versione di WIA SOOM (Impostazioni → Informazioni)
   - Sistema operativo e versione (Windows/macOS/Linux)
   - Passaggi per riprodurre il problema
   - Comportamento atteso vs. reale
   - Screenshot o output del terminale se possibile

---

## 💡 Come Suggerire Funzionalità

1. Vai a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca su **"Nuovo Problema"**
3. Scegli il modello **"Richiesta di Funzionalità"**
4. Descrivi:
   - Quale problema stai risolvendo
   - Come immagini che funzioni
   - Eventuali alternative che hai considerato

---

## 🔌 Come Inviare un Plugin

WIA SOOM ha un potente sistema di plugin — puoi costruire il tuo plugin in 5 minuti.

### Inizio Veloce
§§§CHUNK_SEPARATOR§§§
### Guida Completa

Leggi la **[Guida per Sviluppatori di Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Riferimento API completo
- Esempi funzionanti
- Tutorial passo-passo
- Migliori pratiche e regole di sicurezza

### Invia il Tuo Plugin

1. Forka [Plugin Store](https://wiasoom.com)
2. Aggiungi il tuo plugin a `plugins/{your-plugin-name}/`
3. Invia una Pull Request
4. Dopo la revisione, il tuo plugin apparirà nel Plugin Store per tutti gli utenti!

---

## 🔀 Come Inviare una Pull Request

### Per l'app principale (wia-soom)

1. Forka il repository
2. Crea un branch per la funzionalità: `git checkout -b feat/my-feature`
3. Apporta le tue modifiche
4. Testa localmente:
   ```bash
   ```
5. Fai un commit con un messaggio chiaro:
   ```
   feat: aggiungi interruttore della modalità scura nelle impostazioni
   ```
6. Pusha e apri una PR contro `main`

### Convenzione per il Messaggio di Commit

| Prefisso | Utilizzato per |
|----------|----------------|
| `feat:`  | Nuova funzionalità |
| `fix:`   | Correzione di bug |
| `docs:`  | Solo documentazione |
| `refactor:` | Ristrutturazione del codice (senza cambiamenti nel comportamento) |
| `i18n:`  | Aggiornamenti di traduzione |
| `plugin:` | Modifiche relative ai plugin |

### Checklist PR

- [ ] Il codice funziona senza errori
- [ ] Nessuna stringa hardcoded (usa chiavi i18n)
- [ ] Nessun `console.log` lasciato nel codice di produzione
- [ ] I test esistenti passano ancora

---

## 🌐 Contributi di Traduzione (254 Lingue)

WIA SOOM supporta **254 lingue** — dall'Amarico allo Zulu, comprese le lingue Braille e RTL.

### Come Funziona la Traduzione

- File di lingua base: `src/renderer/src/i18n/en.json`
- Tutti i 254 file di lingua si trovano nella stessa directory
- La traduzione avviene tramite `scripts/translate-patch.js` (API GPT-4o-mini)

### Come Contribuire con Traduzioni

#### Opzione 1: Correggi una traduzione specifica

1. Trova il file di lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Correggi la traduzione errata
3. Invia una PR con la modifica

#### Opzione 2: Aggiungi chiavi mancanti
§§§CHUNK_SEPARATOR§§§
#### Opzione 3: Rivedi le traduzioni automatiche

Molte delle nostre 254 lingue sono state tradotte automaticamente. Le revisioni da parte di madrelingua sono incredibilmente preziose!

1. Scegli il tuo file di lingua
2. Rivedi le traduzioni
3. Correggi eventuali traduzioni imprecise o scomode
4. Invia una PR

### Codici Lingua

Utilizziamo codici standard ISO 639-1 (ad es., `ko`, `en`, `ja`, `ar`, `hi`) con varianti regionali dove necessario (ad es., `zh-CN`, `pt-BR`).

---

## 🛠 Impostazione dello Sviluppo

### Requisiti Preliminari

- Node.js 18+
- npm 9+
- Git

### Impostazione
§§§CHUNK_SEPARATOR§§§
### Compilazione
§§§CHUNK_SEPARATOR§§§
> Nota: L'heap predefinito di 2GB non è sufficiente a causa dei 254 file di lingua + bundle dell'editor Monaco (~38MB renderer).

### Struttura del Progetto
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Grazie

Ogni contributo rende WIA SOOM migliore per gli sviluppatori di tutto il mondo.

Che tu stia correggendo un errore di battitura, traducendo una stringa, costruendo un plugin o aggiungendo una funzionalità importante — **fai parte di questa storia.**

---

<p align="center"><em>Costruito con ❤️ da SmileStory Inc. e collaboratori in tutto il mondo.</em></p>
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```

#### Option 3: Review machine translations

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup

```bash
```

### Build

```bash
```

> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

### Project Structure

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

## 🙏 Thank You

Every contribution makes WIA SOOM better for developers around the world.

Whether you fix a typo, translate a string, build a plugin, or add a major feature — **you are part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>
