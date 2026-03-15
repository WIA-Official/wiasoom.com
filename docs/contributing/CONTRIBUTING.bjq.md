<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kondribuye sa WIA SOOM</h1>
<p align="center"><strong>Gusto mi ang imong mga kontribusyon!</strong></p>
<p align="center">Bisan pa kini usa ka pag-ayo sa sayop, bag-ong bahin, plugin, o hubad — ang matag kontribusyon importante.</p>

---

## Talaan sa Sulod

- [Code of Conduct](#code-of-conduct)
- [Unsaon Pagreport sa mga Sayop](#-unsaon-pagreport-sa-mga-sayop)
- [Unsaon Pagsugyot sa mga Bahin](#-unsaon-pagsugyot-sa-mga-bahin)
- [Unsaon Pagsumite sa usa ka Plugin](#-unsaon-pagsumite-sa-usa-ka-plugin)
- [Unsaon Pagsumite sa usa ka Pull Request](#-unsaon-pagsumite-sa-usa-ka-pull-request)
- [Mga Kontribusyon sa Hubad (254 nga mga Wika)](#-mga-kontribusyon-sa-hubad-254-nga-mga-wika)
- [Pag-setup sa Pag-uswag](#-pag-setup-sa-pag-uswag)

---

## Code of Conduct

Kami nagkomit sa paghatag og usa ka welcoming ug inclusive nga kasinatian para sa tanan.

- **Magmatinahuron.** Tratara ang tanan nga may pagkamatarong.
- **Magkonstructibo.** Ihalad ang makatabang nga feedback, dili naguba nga kritisismo.
- **Mag-inclusibo.** Suportahan namo ang 254 nga mga wika ug welcome ang mga kontribyutor gikan sa tanang nasud sa Kalibutan.
- **Walay pangharas.** Zero tolerance sa bisan unsang matang sa diskriminasyon.

---

## 🐛 Unsaon Pagreport sa mga Sayop

1. Adto sa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. I-klik ang **"Bag-ong Isyu"**
3. Pilia ang **"Bug Report"** nga template
4. Isama ang:
   - Bersyon sa WIA SOOM (Mga Setting → Mahitungod)
   - OS ug bersyon (Windows/macOS/Linux)
   - Mga lakang aron ma-reproduce
   - Gilauman vs. aktwal nga pamatasan
   - Mga screenshot o terminal output kung mahimo

---

## 💡 Unsaon Pagsugyot sa mga Bahin

1. Adto sa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. I-klik ang **"Bag-ong Isyu"**
3. Pilia ang **"Feature Request"** nga template
4. Isaysay:
   - Unsang problema ang imong ginasulbad
   - Giunsa nimo kini pagtan-aw nga magtrabaho
   - Bisan unsang alternatibo nga imong giconsiderar

---

## 🔌 Unsaon Pagsumite sa usa ka Plugin

Ang WIA SOOM adunay usa ka kusgan nga sistema sa plugin — mahimo ka maghimo sa imong kaugalingong plugin sa sulod sa 5 minutos.

### Dali nga Pagsugod
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kumpletong Giya

Basaha ang **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** para sa:
- Kumpletong API reference
- Nagtrabaho nga mga ehemplo
- Step-by-step nga mga tutorial
- Mga labing maayo nga praktis ug mga lagda sa seguridad

### Isumite ang Imong Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Idugang ang imong plugin sa `plugins/{your-plugin-name}/`
3. Isumite ang usa ka Pull Request
4. Human sa pagrepaso, ang imong plugin makita sa Plugin Store para sa tanan nga mga tiggamit!

---

## 🔀 Unsaon Pagsumite sa usa ka Pull Request

### Para sa main app (wia-soom)

1. Fork ang repository
2. Maghimo og feature branch: `git checkout -b feat/my-feature`
3. Buhata ang imong mga kausaban
4. Sulayi lokalmente:
   ```bash
   ```
5. I-commit uban ang usa ka klaro nga mensahe:
   ```
   feat: add dark mode toggle to settings
   ```
6. I-push ug ablihi ang PR batok sa `main`

### Convention sa Mensahe sa Commit

| Prefix | Gamiton para |
|--------|--------------|
| `feat:` | Bag-ong bahin |
| `fix:` | Pag-ayo sa sayop |
| `docs:` | Dokumentasyon lamang |
| `refactor:` | Pag-restructure sa code (walay pagbag-o sa pamatasan) |
| `i18n:` | Mga pag-update sa hubad |
| `plugin:` | Mga kausaban nga may kalabutan sa plugin |

### PR Checklist

- [ ] Ang code nagdagan nga walay mga sayop
- [ ] Walay hardcoded nga mga string (gamiton ang i18n keys)
- [ ] Walay `console.log` nga nahabilin sa production code
- [ ] Ang mga existing nga tests nagpadayon nga molabay

---

## 🌐 Mga Kontribusyon sa Hubad (254 nga mga Wika)

Ang WIA SOOM nagsuporta sa **254 nga mga wika** — gikan sa Amharic hangtod Zulu, lakip ang Braille ug RTL nga mga wika.

### Giunsa ang Pagtrabaho sa Hubad

- Base nga file sa wika: `src/renderer/src/i18n/en.json`
- Ang tanan nga 254 nga mga file sa wika naa sa parehas nga direktoryo
- Ang hubad gihimo pinaagi sa `scripts/translate-patch.js` (GPT-4o-mini API)

### Unsaon Pagsumite sa mga Hubad

#### Kapilian 1: Ayuhon ang usa ka partikular nga hubad

1. Pangitaa ang file sa wika: `src/renderer/src/i18n/{lang-code}.json`
2. Ayuhon ang sayop nga hubad
3. Isumite ang usa ka PR uban ang kausaban

#### Kapilian 2: Idugang ang mga nawawalang yawe
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kapilian 3: Susiha ang mga machine translations

Daghan sa among 254 nga mga wika ang gi-machine-translate. Ang mga review sa mga native speaker labaw nga bililhon!

1. Pilia ang imong file sa wika
2. Susiha ang mga hubad
3. Ayuhon ang bisan unsang awkward o sayop nga mga hubad
4. Isumite ang usa ka PR

### Mga Kodigo sa Wika

Gamiton namo ang standard nga ISO 639-1 nga mga kodigo (e.g., `ko`, `en`, `ja`, `ar`, `hi`) uban ang mga rehiyonal nga variant kung gikinahanglan (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Pag-setup sa Pag-uswag

### Mga Kinahanglanon

- Node.js 18+
- npm 9+
- Git

### Pag-setup
```bash
```
### Pag-build
```bash
```
> Nota: Ang default nga 2GB nga heap dili igo tungod sa 254 nga mga file sa wika + Monaco editor bundle (~38MB renderer).

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

Bawat kontribusyon ay nagpapabuti sa WIA SOOM para sa mga developer sa buong mundo.

Kahit na nag-aayos ka ng typo, nagsasalin ng string, bumubuo ng plugin, o nagdaragdag ng malaking tampok — **ikaw ay bahagi ng kwentong ito.**

---

<p align="center"><em>Itinayo ng ❤️ ng SmileStory Inc. at mga kontribyutor sa buong mundo.</em></p>