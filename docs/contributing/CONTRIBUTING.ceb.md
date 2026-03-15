<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Pag-apil sa WIA SOOM</h1>
<p align="center"><strong>Gusto namo ang inyong mga kontribusyon!</strong></p>
<p align="center">Bisan pa man kini usa ka pag-ayo sa sayup, bag-ong feature, plugin, o hubad — ang matag kontribusyon importante.</p>

---

## Talaan sa Sulod

- [Code of Conduct](#code-of-conduct)
- [Unsaon Pagreport sa mga Sayup](#-unsaon-pagreport-sa-mga-sayup)
- [Unsaon Pag-suggest sa mga Feature](#-unsaon-pag-suggest-sa-mga-feature)
- [Unsaon Pag-submit sa usa ka Plugin](#-unsaon-pag-submit-sa-usa-ka-plugin)
- [Unsaon Pag-submit sa usa ka Pull Request](#-unsaon-pag-submit-sa-usa-ka-pull-request)
- [Mga Kontribusyon sa Hubad (254 nga mga Wika)](#-mga-kontribusyon-sa-hubad-254-nga-mga-wika)
- [Pag-set up sa Pag-uswag](#-pag-set-up-sa-pag-uswag)

---

## Code of Conduct

Kami nagkomit sa paghatag og usa ka welcoming ug inclusive nga kasinatian para sa tanan.

- **Magpakita og respeto.** Tratuhon ang tanan nga may dignidad.
- **Magkonstructibo.** Ihalad ang makatabang nga feedback, dili ang naguba nga kritisismo.
- **Mag-inclusive.** Suportahan namo ang 254 nga mga wika ug welcome ang mga kontribyutor gikan sa matag nasud sa Yuta.
- **Walay harassment.** Zero tolerance alang sa diskriminasyon sa bisan unsang matang.

---

## 🐛 Unsaon Pagreport sa mga Sayup

1. Adto sa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. I-klik ang **"New Issue"**
3. Pilia ang **"Bug Report"** nga template
4. I-apil:
   - Bersyon sa WIA SOOM (Settings → About)
   - OS ug bersyon (Windows/macOS/Linux)
   - Mga lakang aron ma-reproduce
   - Gipaabot vs. aktwal nga pamatasan
   - Mga screenshot o terminal output kung mahimo

---

## 💡 Unsaon Pag-suggest sa mga Feature

1. Adto sa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. I-klik ang **"New Issue"**
3. Pilia ang **"Feature Request"** nga template
4. I-describe:
   - Unsa nga problema ang imong gisulbad
   - Giunsa nimo kini paghunahuna nga magtrabaho
   - Bisan unsang alternatibo nga imong gihunahuna

---

## 🔌 Unsaon Pag-submit sa usa ka Plugin

Ang WIA SOOM adunay usa ka kusgan nga sistema sa plugin — mahimo nimong buhaton ang imong kaugalingong plugin sa sulod sa 5 ka minuto.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Basaha ang **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** para sa:
- Kumpletong API reference
- Nagtrabaho nga mga ehemplo
- Step-by-step nga mga tutorial
- Mga best practices ug mga lagda sa seguridad

### I-submit ang Imong Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Idugang ang imong plugin sa `plugins/{your-plugin-name}/`
3. I-submit ang usa ka Pull Request
4. Human sa review, ang imong plugin makita sa Plugin Store para sa tanan nga mga tiggamit!

---

## 🔀 Unsaon Pag-submit sa usa ka Pull Request

### Para sa main app (wia-soom)

1. Fork ang repository
2. Maghimo og feature branch: `git checkout -b feat/my-feature`
3. Buhata ang imong mga kausaban
4. I-test locally:
   ```bash
   ```
5. I-commit uban ang klaro nga mensahe:
   ```
   feat: add dark mode toggle to settings
   ```
6. I-push ug ablihi ang usa ka PR batok sa `main`

### Commit Message Convention

| Prefix | Gamiton para sa |
|--------|------------------|
| `feat:` | Bag-ong feature |
| `fix:` | Pag-ayo sa sayup |
| `docs:` | Dokumentasyon ra |
| `refactor:` | Pag-restructure sa code (walay pagbag-o sa pamatasan) |
| `i18n:` | Mga pag-update sa hubad |
| `plugin:` | Mga kausaban nga may kalabutan sa plugin |

### PR Checklist

- [ ] Ang code nagdagan nga walay mga sayup
- [ ] Wala’y hardcoded nga mga string (gamiton ang i18n keys)
- [ ] Wala’y `console.log` nga nahabilin sa production code
- [ ] Ang mga existing nga tests nagpadayon nga molabay

---

## 🌐 Mga Kontribusyon sa Hubad (254 nga mga Wika)

Ang WIA SOOM nagsuporta sa **254 nga mga wika** — gikan sa Amharic hangtod Zulu, lakip ang Braille ug RTL nga mga wika.

### Unsaon Pagtrabaho ang Hubad

- Base language file: `src/renderer/src/i18n/en.json`
- Ang tanan nga 254 nga mga language files anaa sa parehas nga directory
- Ang hubad gihimo pinaagi sa `scripts/translate-patch.js` (GPT-4o-mini API)

### Unsaon Pag-contribute sa mga Hubad

#### Opsyon 1: Ayuha ang usa ka partikular nga hubad

1. Pangitaa ang language file: `src/renderer/src/i18n/{lang-code}.json`
2. Ayuha ang sayop nga hubad
3. I-submit ang usa ka PR uban ang kausaban

#### Opsyon 2: Idugang ang mga nawawalang keys
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsyon 3: Susiha ang mga machine translations

Daghan sa among 254 nga mga wika ang gi-machine-translate. Ang mga review gikan sa mga native speaker labaw nga bililhon!

1. Pilia ang imong language file
2. Susiha ang mga hubad
3. Ayuha ang bisan unsang awkward o sayop nga mga hubad
4. I-submit ang usa ka PR

### Mga Language Codes

Gamiton namo ang standard nga ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) uban ang mga regional variants kung gikinahanglan (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Pag-set up sa Pag-uswag

### Mga Kinahanglanon

- Node.js 18+
- npm 9+
- Git

### Pag-set up
```bash
```
### Build
```bash
```
> Nota: Ang default nga 2GB heap dili igo tungod sa 254 nga mga language files + Monaco editor bundle (~38MB renderer).

### Estruktura sa Proyekto
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

## 🙏 Salamat

Ang matag kontribusyon nagpaayo sa WIA SOOM para sa mga developer sa tibuok kalibutan.

Bisan pa man nga nag-ayo ka og sayop sa pagsulat, naghubad ka og string, nagtukod ka og plugin, o nagdugang ka og dako nga feature — **ikaw usa ka bahin sa kini nga istorya.**

---

<p align="center"><em>Gihimo uban ang ❤️ sa SmileStory Inc. ug mga kontribyutor sa tibuok kalibutan.</em></p>