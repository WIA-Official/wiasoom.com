<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribie dan WIA SOOM</h1>
<p align="center"><strong>Nou kontan ou kontribisyon!</strong></p>
<p align="center">Ki swa enn bug fix, nouvo karakteristik, plugin, ou tradiksyon — tou kontribisyon inportan.</p>

---

## Tablo Konteni

- [Kòd de Konduit](#code-of-conduct)
- [Koman Raporte Bug](#-how-to-report-bugs)
- [Koman Sijere Karakteristik](#-how-to-suggest-features)
- [Koman Soumet enn Plugin](#-how-to-submit-a-plugin)
- [Koman Soumet enn Pull Request](#-how-to-submit-a-pull-request)
- [Kontribisyon Tradiksyon (254 Lang)](#-translation-contributions-254-languages)
- [Devlopman Setup](#-development-setup)

---

## Kòd de Konduit

Nou angaze pou ofer enn lexperyans akeyan ek inklizif pou tou dimoun.

- **Respekte.** Trat tou dimoun avek dignite.
- **Konstriktif.** Ofer feedback itil, pa kritik destriktif.
- **Inklizif.** Nou sipor 254 lang ek akey tou kontribiter depi tou pei lor later.
- **Pa fer harasman.** Zero tolerans pou diskriminasyon dan okenn fason.

---

## 🐛 Koman Raporte Bug

1. Al dan [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klike **"New Issue"**
3. Chwazi template **"Bug Report"**
4. Inklir:
   - WIA SOOM vèsyon (Settings → About)
   - OS ek vèsyon (Windows/macOS/Linux)
   - Etap pou reproduir
   - Atann vs. reyalite konportman
   - Screenshots ou terminal output si posib

---

## 💡 Koman Sijere Karakteristik

1. Al dan [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klike **"New Issue"**
3. Chwazi template **"Feature Request"**
4. Dekrir:
   - Ki problem ou pe rezoud
   - Koman ou imazinn li pe marse
   - Nimport ki altènativ ou'nn konsider

---

## 🔌 Koman Soumet enn Plugin

WIA SOOM ena enn sistem plugin pwisan — ou kapav konstrir ou prop plugin dan 5 minit.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Gid Konplè

Li **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** pou:
- Referans API konplè
- Leksanpli ki pe travay
- Tutorial etap par etap
- Meilleur pratik ek règle sekirite

### Soumet Ou Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Ajoute ou plugin dan `plugins/{your-plugin-name}/`
3. Soumet enn Pull Request
4. Apre revizyon, ou plugin pou aparé dan Plugin Store pou tou itilizater!

---

## 🔀 Koman Soumet enn Pull Request

### Pou aplikasyon prensipal (wia-soom)

1. Fork repo
2. Kre enn feature branch: `git checkout -b feat/my-feature`
3. Fer ou chanjman
4. Test lokalman:
   ```bash
   ```
5. Komit avek enn mesaj klèr:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pouse ek ouvri enn PR kont `main`

### Konvansyon Mesaz Komit

| Prefix | Utiliz pou |
|--------|------------|
| `feat:` | Nouvo karakteristik |
| `fix:` | Bug fix |
| `docs:` | Dokimantasyon zis |
| `refactor:` | Restructurasyon kòd (pa ena chanjman konportman) |
| `i18n:` | Mizaz tradiksyon |
| `plugin:` | Chanjman ki relie avek plugin |

### PR Checklist

- [ ] Kòd marse san erer
- [ ] Pa ena string hardcoded (utiliz i18n keys)
- [ ] Pa ena `console.log` ki reste dan kòd prodiksyon
- [ ] Test ki egziste ankor pas

---

## 🌐 Kontribisyon Tradiksyon (254 Lang)

WIA SOOM sipor **254 lang** — depi Amharic ziska Zulu, inklir Braille ek lang RTL.

### Koman Tradiksyon Marse

- Dosye lang baz: `src/renderer/src/i18n/en.json`
- Tou 254 dosye lang dan mem direktoir
- Tradiksyon fer atraver `scripts/translate-patch.js` (GPT-4o-mini API)

### Koman Kontribie Tradiksyon

#### Opsyon 1: Fix enn tradiksyon spesifik

1. Trouv dosye lang: `src/renderer/src/i18n/{lang-code}.json`
2. Fix tradiksyon ki pa korek
3. Soumet enn PR avek chanjman

#### Opsyon 2: Ajoute kle ki mank
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsyon 3: Revize tradiksyon machin

Plizir parmi nou 254 lang ti tradwir par machin. Revizyon par natif se enn gran valer!

1. Pran ou dosye lang
2. Revize bann tradiksyon
3. Fix okenn tradiksyon ki pa korek ou ki pa bon
4. Soumet enn PR

### Kòd Lang

Nou utiliz kòd standard ISO 639-1 (par ex, `ko`, `en`, `ja`, `ar`, `hi`) avek variant regional kot neseser (par ex, `zh-CN`, `pt-BR`).

---

## 🛠 Devlopman Setup

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
> Note: Defolt 2GB heap pa ase akoz 254 dosye lang + bundle Monaco editor (~38MB renderer).

### Estrikti Proze
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

## 🙏 Mersi

Kouma ou kontribie, sa fer WIA SOOM pli bon pou devlopers partou dan lemond.

Ki ou korek enn tip, tradir enn string, bati enn plugin, ou azout enn gran karakteristik — **ou ena parti dan sa istwar-la.**

---

<p align="center"><em>Fini avek ❤️ par SmileStory Inc. ek kontribiter partou dan lemond.</em></p>