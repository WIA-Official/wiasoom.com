<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Cuntribbuennu a WIA SOOM</h1>
<p align="center"><strong>Ci piaci u vostru cuntributu!</strong></p>
<p align="center">Chi si tratta di na riparazioni di bug, na nova funzionalità, plugin, o traduzzioni — ogni cuntributu è impurtanti.</p>

---

## Tavola di Cuntenuti

- [Codice di Condotta](#code-of-conduct)
- [Comu Rapportari Bug](#-how-to-report-bugs)
- [Comu Suggeriri Funzionalità](#-how-to-suggest-features)
- [Comu Sottomettere un Plugin](#-how-to-submit-a-plugin)
- [Comu Sottomettere una Pull Request](#-how-to-submit-a-pull-request)
- [Cuntributi di Traduzzioni (254 Lingue)](#-translation-contributions-254-languages)
- [Setup di Sviluppu](#-development-setup)

---

## Codice di Condotta

Semu impegnati a pruvvidiri na spirienza accuglienti e inclusiva pi tutti.

- **Sii rispittusu.** Tratta tutti cu dignità.
- **Sii custruttivu.** Offri feedback utili, no critica distruttiva.
- **Sii inclusivu.** Supportamu 254 lingue e accugghiemu cuntributuri di ogni paisi di la Terra.
- **Nuddu molestia.** Zero tolleranza pi discriminazioni di ogni tipu.

---

## 🐛 Comu Rapportari Bug

1. Va a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"New Issue"**
3. Scegli u template **"Bug Report"**
4. Includi:
   - Versione di WIA SOOM (Impostazioni → Informazioni)
   - OS e versione (Windows/macOS/Linux)
   - Passi pi ripruduciri
   - Comportamentu aspettatu vs. attuali
   - Screenshots o output di terminali si pussibuli

---

## 💡 Comu Suggeriri Funzionalità

1. Va a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicca **"New Issue"**
3. Scegli u template **"Feature Request"**
4. Discrivi:
   - Chi prublema stai risolvendo
   - Comu ti immagini ca funziona
   - Qualunqui alternative ca hai cunsideratu

---

## 🔌 Comu Sottomettere un Plugin

WIA SOOM hà nu sistema di plugin putenti — poi custruiri u to plugin in 5 minuti.

### Inizio Veloce
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guida Completa

Leggi u **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** pi:
- Riferimentu API cumpletu
- Esempi funzionanti
- Tutorial passo-passo
- Megghiu pratiche e reguli di sicurizza

### Sottometti u To Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Aggiungi u to plugin a `plugins/{your-plugin-name}/`
3. Sottometti una Pull Request
4. Doppu a revisione, u to plugin apparirà ntô Plugin Store pi tutti l'utenti!

---

## 🔀 Comu Sottomettere una Pull Request

### Pi l'app principale (wia-soom)

1. Forka u repository
2. Crea na branch di funzionalità: `git checkout -b feat/my-feature`
3. Fai i to cambiamenti
4. Testa localmente:
   ```bash
   ```
5. Commit cu un messaggiu chiaru:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push e apri un PR contru `main`

### Convenzione di Messaggi di Commit

| Prefissu | Usatu pi |
|----------|----------|
| `feat:`  | Nova funzionalità |
| `fix:`   | Riparazioni di bug |
| `docs:`  | Documentazione sulu |
| `refactor:` | Ristrutturazioni di codice (senza cambiamenti di comportamento) |
| `i18n:`  | Aggiornamenti di traduzzioni |
| `plugin:` | Cambiamenti relativi ai plugin |

### Checklist di PR

- [ ] U codice funziona senza errori
- [ ] Nuddi stringhe hardcoded (usa chjavi i18n)
- [ ] Nuddu `console.log` lasciatu ntô codice di produzzioni
- [ ] I test esistenti continuanu a passari

---

## 🌐 Cuntributi di Traduzzioni (254 Lingue)

WIA SOOM supporta **254 lingue** — di Amharic a Zulu, inclusu Braille e lingue RTL.

### Comu Funziona a Traduzzioni

- File di lingua base: `src/renderer/src/i18n/en.json`
- Tutti i 254 file di lingua sunnu ntô stessu direttoriu
- A traduzzioni si fa attraversu `scripts/translate-patch.js` (GPT-4o-mini API)

### Comu Cuntribbuiri Traduzzioni

#### Opzioni 1: Ripara na traduzzioni specifica

1. Trova u file di lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Ripara a traduzzioni sbagliata
3. Sottometti un PR cu u cambiamentu

#### Opzioni 2: Aggiungi chjavi mancanti
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opzioni 3: Rivedi traduzzioni di macchina

Parecchi di i nostri 254 lingue foru tradutti di macchina. Rivedi di parlanti nativi sunnu incredibilmente preziosi!

1. Scegli u to file di lingua
2. Rivedi e traduzzioni
3. Ripara ogni traduzzioni strana o sbagliata
4. Sottometti un PR

### Codici di Lingua

Usamu codici standard ISO 639-1 (es., `ko`, `en`, `ja`, `ar`, `hi`) cu varianti regiunali quannu necessariu (es., `zh-CN`, `pt-BR`).

---

## 🛠 Setup di Sviluppu

### Prerequisiti

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Nota: U heap di 2GB di default non è abbastanza a causa di i 254 file di lingua + pacchettu di l'editor Monaco (~38MB renderer).

### Struttura di u Progettu
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

## 🙏 Grazii

Ogni cuntribbutu fa WIA SOOM megghiu pi li sviluppaturi di tuttu lu munnu.

Chiunque tu sia, ca ripari na scrittura, traduci na stringa, custruisci nu plugin, o aggiungi na funzione mpurtanti — **tu fai parti di sta storia.**

---

<p align="center"><em>Custruitu cu ❤️ di SmileStory Inc. e cuntribbuturi di tuttu lu munnu.</em></p>