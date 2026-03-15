<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Cuntribuendu à WIA SOOM</h1>
<p align="center"><strong>Ci piacerebbe e vostre cuntribuzioni!</strong></p>
<p align="center">Ch'ella sia una riparazione di bug, una nova funzione, un plugin, o una traduzzione — ogni cuntribuzione hè impurtante.</p>

---

## Tavula di Cuntenutu

- [Codice di Condotta](#code-of-conduct)
- [Cumu Rapurtà Bug](#-how-to-report-bugs)
- [Cumu Suggerisce Funzioni](#-how-to-suggest-features)
- [Cumu Presentà un Plugin](#-how-to-submit-a-plugin)
- [Cumu Presentà una Pull Request](#-how-to-submit-a-pull-request)
- [Cuntribuzioni di Traduzzione (254 Lingue)](#-translation-contributions-254-languages)
- [Configurazione di Sviluppu](#-development-setup)

---

## Codice di Condotta

Semu impegnati à furnisce una sperienza accugliente è inclusiva per tutti.

- **Siate rispettosi.** Trattate tutti cun dignità.
- **Siate costruttivi.** Offrite feedback utile, micca critica distruttiva.
- **Siate inclusivi.** Supportemu 254 lingue è accoglimu cuntribuenti da ogni paese di a Terra.
- **Nisun harcèlement.** Zero tolleranza per a discriminazione di ogni tipu.

---

## 🐛 Cumu Rapurtà Bug

1. Andate à [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliccate **"New Issue"**
3. Sceglite u mudellu **"Bug Report"**
4. Include:
   - Versione di WIA SOOM (Impostazioni → À propositu)
   - OS è versione (Windows/macOS/Linux)
   - Passi per ripruduce
   - Comportamentu aspettatu vs. attuale
   - Screenshots o output di terminal se pussibule

---

## 💡 Cumu Suggerisce Funzioni

1. Andate à [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliccate **"New Issue"**
3. Sceglite u mudellu **"Feature Request"**
4. Descrivite:
   - Chì prublema state risolvendu
   - Cumu vi imaginate ch'ella funzioni
   - Qualunque alternativa avete cunsideratu

---

## 🔌 Cumu Presentà un Plugin

WIA SOOM hà un sistema di plugin putente — pudete custruisce u vostru plugin in 5 minuti.

### Iniziu Rapidu
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guida Completa

Leghjite u **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Riferimentu API cumpletu
- Esempi funzionanti
- Tutorial passu-pasu
- Megliu pratiche è regule di sicurità

### Presentate u Vostru Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Agghiuncite u vostru plugin à `plugins/{your-plugin-name}/`
3. Presentate una Pull Request
4. Dopu a revisione, u vostru plugin apparisce in u Plugin Store per tutti l'utilizatori!

---

## 🔀 Cumu Presentà una Pull Request

### Per l'app principale (wia-soom)

1. Fork u repository
2. Create una branca di funzione: `git checkout -b feat/my-feature`
3. Fate e vostre modifiche
4. Testate localmente:
   ```bash
   ```
5. Commit cù un messagiu chjaru:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push è aprite un PR contr'à `main`

### Convenzione di Messagiu di Commit

| Prefix | Usatu per |
|--------|-----------|
| `feat:` | Nova funzione |
| `fix:` | Riparazione di bug |
| `docs:` | Documentazione solu |
| `refactor:` | Ristrutturazione di codice (senza cambiamentu di comportamento) |
| `i18n:` | Aggiornamenti di traduzzione |
| `plugin:` | Cambiamenti relativi à i plugin |

### Checklist di PR

- [ ] U codice funziona senza errori
- [ ] Nisuna stringa hardcoded (usate e chjave i18n)
- [ ] Nisun `console.log` lasciatu in u codice di produzzione
- [ ] I test esistenti passanu sempre

---

## 🌐 Cuntribuzioni di Traduzzione (254 Lingue)

WIA SOOM supporta **254 lingue** — da Amharic à Zulu, inclusi Braille è lingue RTL.

### Cumu Funziona a Traduzzione

- File di lingua di basa: `src/renderer/src/i18n/en.json`
- Tutti i 254 file di lingua sò in a stessa directory
- A traduzzione hè fatta via `scripts/translate-patch.js` (GPT-4o-mini API)

### Cumu Cuntribuisce à e Traduzzioni

#### Opzione 1: Riparà una traduzzione specifica

1. Truvate u file di lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Riparate a traduzzione scorretta
3. Presentate un PR cù a cambiamentu

#### Opzione 2: Agghiuncite chjave mancanti
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opzione 3: Rivista di traduzzioni di macchina

Parechje di e nostre 254 lingue sò state tradutte da a macchina. E riviste di parlanti nativi sò incredibilmente preziose!

1. Sceglite u vostru file di lingua
2. Riviste e traduzzioni
3. Riparate qualsiasi traduzzione strana o scorretta
4. Presentate un PR

### Codici di Lingua

Usamu codici standard ISO 639-1 (per esempiu, `ko`, `en`, `ja`, `ar`, `hi`) cù varianti regionali induve necessariu (per esempiu, `zh-CN`, `pt-BR`).

---

## 🛠 Configurazione di Sviluppu

### Prerequisiti

- Node.js 18+
- npm 9+
- Git

### Configurazione
```bash
```
### Costruisce
```bash
```
> Nota: U heap predefinitu di 2GB ùn hè micca abbastanza per via di i 254 file di lingua + pacchettu di l'editore Monaco (~38MB renderer).

### Struttura di u Prughjettu
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

## 🙏 Grazie

Ogni cuntribuzione rende WIA SOOM megliu per i sviluppatori in tuttu u mondu.

Chì tu ripari un errore di scrittura, traduci una stringa, custruisci un plugin, o aghjunghje una caratteristica impurtante — **sì parte di sta storia.**

---

<p align="center"><em>Custruitu cù ❤️ da SmileStory Inc. è cuntribuenti in tuttu u mondu.</em></p>