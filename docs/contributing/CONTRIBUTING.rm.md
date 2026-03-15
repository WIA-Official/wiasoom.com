<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuziun a WIA SOOM</h1>
<p align="center"><strong>Gustavain nossas contribuziuns!</strong></p>
<p align="center">Sia ch'i saja in fix da bug, in nova funcziun, in plugin u in traduziun — mintga contribuziun è impurtanta.</p>

---

## Table da Cuntgnì

- [Code da Conducta](#code-da-conducta)
- [Co rapportar bugs](#-co-rapportar-bugs)
- [Co suggerir funcziuns](#-co-suggerir-funcziuns)
- [Co presentar in plugin](#-co-presentar-in-plugin)
- [Co presentar in Pull Request](#-co-presentar-in-pull-request)
- [Contribuziuns da Traduziun (254 Linguas)](#-contribuziuns-da-traduziun-254-linguas)
- [Setup da Developament](#-setup-da-developament)

---

## Code da Conducta

Nus essan engagads a proporcionar in'experienza da bunvenuta e inclusiva per tuts.

- **Essar respectus.** Tratter tuts cun dignitad.
- **Essar constructiv.** Offrir feedback util, betg critica destructiva.
- **Essar inclusiv.** Nus sustegnain 254 linguas e acceptain contribuents da mintga pajais sin la Terra.
- **Nus vulain betg harassment.** Toleranza zero per discriminaziun da tuts geners.

---

## 🐛 Co rapportar bugs

1. Ir a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliccar **"New Issue"**
3. Sceglier il template **"Bug Report"**
4. Includer:
   - Versiun da WIA SOOM (Settings → About)
   - OS e versiun (Windows/macOS/Linux)
   - Pass per reproducir
   - Comportament spetgà vs. actual
   - Screenshots u output dal terminal sch'i è pussaivel

---

## 💡 Co suggerir funcziuns

1. Ir a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliccar **"New Issue"**
3. Sceglier il template **"Feature Request"**
4. Descriver:
   - Tge problem ch'ei vegn resolvì
   - Co ti imaginescha ch'ella funcziuna
   - Tuts alternatives ch'ei has considerà

---

## 🔌 Co presentar in plugin

WIA SOOM ha in sistem da plugins powerful — ti pudest construir tes propri plugin en 5 minutas.

### Start Svelt
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guida Completa

Leger il **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Referenza completa da l'API
- Exemplars da lavur
- Tutorials pass per pass
- Best practices e reglas da segirtad

### Presentar Tieu Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Aggiunger tes plugin a `plugins/{tes-plugin-nom}/`
3. Presentar in Pull Request
4. Suenter la revisiun, tes plugin apparirà en il Plugin Store per tuts utilisaders!

---

## 🔀 Co presentar in Pull Request

### Per l'app principal (wia-soom)

1. Fork il repository
2. Crear in branch da funcziun: `git checkout -b feat/my-feature`
3. Far tes midaments
4. Testar local:
   ```bash
   ```
5. Committar cun in messadi clar:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push e avrir in PR cunter `main`

### Convenziun da Messadi da Commit

| Prefix | Utilisar per |
|--------|--------------|
| `feat:` | Nova funcziun |
| `fix:` | Fix da bug |
| `docs:` | Documentaziun mo |
| `refactor:` | Ristructuraziun dal code (senza midada da comportament) |
| `i18n:` | Actualisaziuns da traduziun |
| `plugin:` | Midaments relatiuns a plugins |

### Checklist da PR

- [ ] Il code funcziuna senza errors
- [ ] Naginas strings hardcoded (utilisar i18n keys)
- [ ] Nagins `console.log` laschads en il code da producziun
- [ ] Tests existents anc passan

---

## 🌐 Contribuziuns da Traduziun (254 Linguas)

WIA SOOM sustegna **254 linguas** — da Amharic a Zulu, inclusiv Braille e linguas RTL.

### Co funcziuna la Traduziun

- File da lingua base: `src/renderer/src/i18n/en.json`
- Tut ils 254 files da lingua èn en la medema directiva
- La traduziun vegn fatga via `scripts/translate-patch.js` (GPT-4o-mini API)

### Co contribuir cun traduziuns

#### Opziun 1: Fixar ina traduziun specifica

1. Chattar il file da lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Fixar la traduziun incorrecta
3. Presentar in PR cun la midada

#### Opziun 2: Aggiunger keys mancants
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opziun 3: Revisar traduziuns da machina

Moltas da nossas 254 linguas èn vegnidas traducidas da machines. Revisiun da parlants nati è incredibilmain valuabla!

1. Scegli tes file da lingua
2. Revisar las traduziuns
3. Fixar tut las traduziuns awkward u incorrectas
4. Presentar in PR

### Codes da Linguas

Nus utilisain codes standard ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) cun variants regionalas sch'i è necessari (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Setup da Developament

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
> Nota: Il heap default da 2GB na basta betg pervia da las 254 linguas files + bundle dal editor Monaco (~38MB renderer).

### Structura dal Project
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

## 🙏 Grazia

Ogni contribuziun fa WIA SOOM meglier per ils sviluppaders da tut il mund.

Sia che ti corregiasch ina scrittira, traduiesch ina stringa, construiesch in plugin, u aggiuntas in'important funcziun — **ti fa part da questa istorgia.**

---

<p align="center"><em>Construit cun ❤️ da SmileStory Inc. e contribuents da tut il mund.</em></p>