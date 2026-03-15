<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuìndo a WIA SOOM</h1>
<p align="center"><strong>Ci piacerìa i tuoi contributi!</strong></p>
<p align="center">Sia che si tratti di una correzione di bug, una nuova funzionalità, un plugin o una traduzione — ogni contributo è importante.</p>

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

Siamo impegnati a fornire un'esperienza accogliente e inclusiva per tutti.

- **Essere rispettosi.** Tratta tutti con dignità.
- **Essere costruttivi.** Offri feedback utili, non critiche distruttive.
- **Essere inclusivi.** Supportiamo 254 lingue e accogliamo contributori da ogni paese della Terra.
- **Nessun molestia.** Zero tolleranza per discriminazioni di qualsiasi tipo.

---

## 🐛 Come Segnalare Bug

1. Vai a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca su **"Nuovo Problema"**
3. Scegli il modello **"Segnalazione Bug"**
4. Includi:
   - Versione di WIA SOOM (Impostazioni → Informazioni)
   - OS e versione (Windows/macOS/Linux)
   - Passi per riprodurre
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
   - Qualsiasi alternativa che hai considerato

---

## 🔌 Come Inviare un Plugin

WIA SOOM ha un potente sistema di plugin — puoi costruire il tuo plugin in 5 minuti.

### Avvio Veloce
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guida Completa

Leggi la **[Guida per Sviluppatori di Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Riferimento API completo
- Esempi funzionanti
- Tutorial passo-passo
- Migliori pratiche e regole di sicurezza

### Invia il Tuo Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Aggiungi il tuo plugin a `plugins/{your-plugin-name}/`
3. Invia una Pull Request
4. Dopo la revisione, il tuo plugin apparirà nel Plugin Store per tutti gli utenti!

---

## 🔀 Come Inviare una Pull Request

### Per l'app principale (wia-soom)

1. Forka il repository
2. Crea un branch per la funzionalità: `git checkout -b feat/my-feature`
3. Fai le tue modifiche
4. Testa localmente:
   ```bash
   ```
5. Fai un commit con un messaggio chiaro:
   ```
   feat: aggiungi interruttore della modalità scura alle impostazioni
   ```
6. Fai push e apri una PR contro `main`

### Convenzione per il Messaggio di Commit

| Prefisso | Uso per |
|----------|---------|
| `feat:`  | Nuova funzionalità |
| `fix:`   | Correzione di bug |
| `docs:`  | Solo documentazione |
| `refactor:` | Ristrutturazione del codice (senza cambiamenti di comportamento) |
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
- Tutti i 254 file di lingua sono nella stessa directory
- La traduzione avviene tramite `scripts/translate-patch.js` (API GPT-4o-mini)

### Come Contribuire alle Traduzioni

#### Opzione 1: Correggi una traduzione specifica

1. Trova il file di lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Correggi la traduzione errata
3. Invia una PR con la modifica

#### Opzione 2: Aggiungi chiavi mancanti
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
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

### Requisiti

- Node.js 18+
- npm 9+
- Git

### Impostazione
```bash
```
### Costruzione
```bash
```
> Nota: L'heap predefinito di 2GB non è sufficiente a causa dei 254 file di lingua + bundle dell'editor Monaco (~38MB renderer).

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

## 🙏 Gràtzias

Ogni cuntributzione fa WIA SOOM mègius pro sos sviluppadores in totu su mundu.

Custa chi correggi un errore, tradùci una stringa, custruisci un plugin, o aggiungi una funzionalità majore — **fais parte de cust'istòria.**

---

<p align="center"><em>Costruìu cun ❤️ da SmileStory Inc. e cuntribuidores in totu su mundu.</em></p>