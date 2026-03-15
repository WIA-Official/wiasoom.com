<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribuzzjonijiet għal WIA SOOM</h1>
<p align="center"><strong>Nixtiequlek il-kontribuzzjonijiet tiegħek!</strong></p>
<p align="center">Kemm jekk hi korrizzjoni ta' bug, karatteristika ġdida, plugin, jew traduzzjoni — kull kontribuzzjoni għandha importanza.</p>

---

## Indice

- [Kod ta' Kondotta](#code-of-conduct)
- [Kif Rapport Bug](#-how-to-report-bugs)
- [Kif Suggerixxi Karatteristiċi](#-how-to-suggest-features)
- [Kif Sottometti Plugin](#-how-to-submit-a-plugin)
- [Kif Sottometti Pull Request](#-how-to-submit-a-pull-request)
- [Kontribuzzjonijiet ta' Traduzzjoni (254 Lingwi)](#-translation-contributions-254-languages)
- [Setup ta' Żvilupp](#-development-setup)

---

## Kod ta' Kondotta

Aħna impenjati li nipprovdu esperjenza ta' merħba u inklużiva għal kulħadd.

- **Kun rispettuż.** Ittratta lil kulħadd b'dinjità.
- **Kun kostruttiv.** Offri feedback utli, mhux kritika destruttiva.
- **Kun inklużiv.** Aħna nappoġ��jaw 254 lingwa u nilqgħu kontribwenti minn kull pajjiż fid-dinja.
- **Ebda molestja.** Tolleranza żero għall-iskart ta' kwalunkwe tip.

---

## 🐛 Kif Rapport Bug

1. Mur fuq [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Ikklikkja **"New Issue"**
3. Agħżel il-mudell **"Bug Report"**
4. Inkludi:
   - Verżjoni ta' WIA SOOM (Settings → About)
   - OS u verżjoni (Windows/macOS/Linux)
   - Passi biex tirriproduċi
   - Imġieba mistenni vs. attwali
   - Screenshots jew output tal-terminal jekk possibbli

---

## 💡 Kif Suggerixxi Karatteristiċi

1. Mur fuq [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Ikklikkja **"New Issue"**
3. Agħżel il-mudell **"Feature Request"**
4. Deskrivi:
   - Liema problema qed issolvi
   - Kif taħseb li jaħdem
   - Xi alternattivi li kkunsidra

---

## 🔌 Kif Sottometti Plugin

WIA SOOM għandha sistema ta' plugins b'saħħitha — tista' tibni l-plugin tiegħek fi 5 minuti.

### Għażla Rapida
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Gwida Sħiħa

Aqra l-**[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** għal:
- Referenza kompluta tal-API
- Eżempji funzjonali
- Tutorials pass pass
- Aħjar prattiki u regoli ta' sigurtà

### Sottometti l-Plugin Tiegħek

1. Fork [Plugin Store](https://wiasoom.com)
2. Żid il-plugin tiegħek f' `plugins/{your-plugin-name}/`
3. Sottometti Pull Request
4. Wara r-reviżjoni, il-plugin tiegħek jidher fil-Plugin Store għal kulħadd!

---

## 🔀 Kif Sottometti Pull Request

### Għall-app prinċipali (wia-soom)

1. Fork ir-repo
2. Oħloq branch ta' karatteristika: `git checkout -b feat/my-feature`
3. Agħmel il-bidliet tiegħek
4. Ittestja lokalment:
   ```bash
   ```
5. Commit b'messaġġ ċar:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push u open PR kontra `main`

### Konvenzjoni ta' Messaġġ ta' Commit

| Prefix | Uża għal |
|--------|---------|
| `feat:` | Karatteristika ġdida |
| `fix:` | Korrizzjoni ta' bug |
| `docs:` | Dokumentazzjoni biss |
| `refactor:` | Ristrutturar tal-kodiċi (mingħajr tibdil fil-komportament) |
| `i18n:` | Aġġornamenti tat-traduzzjoni |
| `plugin:` | Bidliet relatati mal-plugin |

### Lista ta' Kontroll PR

- [ ] Il-kodiċi jaħdem mingħajr żbalji
- [ ] Ebda strings hardcoded (uża i18n keys)
- [ ] Ebda `console.log` f'kodiċi ta' produzzjoni
- [ ] It-testijiet eżistenti għadhom jgħaddu

---

## 🌐 Kontribuzzjonijiet ta' Traduzzjoni (254 Lingwi)

WIA SOOM tappoġġja **254 lingwi** — minn Amharic sa Zulu, inkluż Braille u lingwi RTL.

### Kif taħdem it-Traduzzjoni

- File tal-lingwa bażika: `src/renderer/src/i18n/en.json`
- Il-files tal-lingwa kollha 254 jinsabu fl-istess direttorju
- It-traduzzjoni ssir permezz ta' `scripts/translate-patch.js` (GPT-4o-mini API)

### Kif Tikkontribwixxi fit-Traduzzjonijiet

#### Għażla 1: Korreġu traduzzjoni speċifika

1. Sib il-file tal-lingwa: `src/renderer/src/i18n/{lang-code}.json`
2. Korreġi t-traduzzjoni żbaljata
3. Sottometti PR bil-bidla

#### Għażla 2: Żid keys nieqsa
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Għażla 3: Irrevedi t-traduzzjonijiet tal-magna

Bosta mill-254 lingwi tagħna ġew tradotti bil-magna. Ir-reviżjonijiet minn dawk li jitkellmu n-natura huma ta' valur kbir!

1. Agħżel il-file tal-lingwa tiegħek
2. Irrevedi t-traduzzjonijiet
3. Korreġi kwalunkwe traduzzjoni stramba jew żbaljata
4. Sottometti PR

### Kodiċi tal-Lingwi

Nużaw kodiċi ISO 639-1 standard (eż., `ko`, `en`, `ja`, `ar`, `hi`) bil-varjanti reġjonali fejn meħtieġ (eż., `zh-CN`, `pt-BR`).

---

## 🛠 Setup ta' Żvilupp

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
> Nota: Il-heap predefinit ta' 2GB mhux biżżejjed minħabba l-files tal-lingwa 254 + bundle tal-editor Monaco (~38MB renderer).

### Struttura tal-Proġett
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

## 🙏 Grazzi

Kull kontribuzzjoni tagħmel WIA SOOM aħjar għall-iżviluppaturi madwar id-dinja.

Kemm jekk tissewwa typo, tittraduċi string, tibni plugin, jew iżżid karatteristika kbira — **int partijiet minn din l-istorja.**

---

<p align="center"><em>Ħidma magħmula bil ❤️ minn SmileStory Inc. u kontribwenti madwar id-dinja.</em></p>