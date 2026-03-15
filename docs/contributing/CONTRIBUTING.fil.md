<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Pagsusumite sa WIA SOOM</h1>
<p align="center"><strong>Ikalulugod namin ang inyong mga kontribusyon!</strong></p>
<p align="center">Kahit ito ay isang pag-aayos ng bug, bagong tampok, plugin, o pagsasalin — bawat kontribusyon ay mahalaga.</p>

---

## Talaan ng Nilalaman

- [Code of Conduct](#code-of-conduct)
- [Paano Mag-ulat ng mga Bug](#-paano-mag-ulat-ng-mga-bug)
- [Paano Magmungkahi ng mga Tampok](#-paano-magmungkahi-ng-mga-tampok)
- [Paano Mag-submit ng Plugin](#-paano-mag-submit-ng-plugin)
- [Paano Mag-submit ng Pull Request](#-paano-mag-submit-ng-pull-request)
- [Mga Kontribusyon sa Pagsasalin (254 Wika)](#-mga-kontribusyon-sa-pagsasalin-254-wika)
- [Pag-set Up ng Pag-unlad](#-pag-set-up-ng-pag-unlad)

---

## Code of Conduct

Kami ay nakatuon sa pagbibigay ng isang nakaka-welcome at inclusive na karanasan para sa lahat.

- **Maging magalang.** Igalang ang lahat ng tao.
- **Maging nakabubuong.** Magbigay ng nakatutulong na feedback, hindi nakasisirang kritisismo.
- **Maging inclusive.** Sinusuportahan namin ang 254 wika at tinatanggap ang mga kontribyutor mula sa bawat bansa sa mundo.
- **Walang pang-aabuso.** Zero tolerance para sa diskriminasyon ng anumang uri.

---

## 🐛 Paano Mag-ulat ng mga Bug

1. Pumunta sa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. I-click ang **"New Issue"**
3. Pumili ng **"Bug Report"** na template
4. Isama:
   - Bersyon ng WIA SOOM (Settings → About)
   - OS at bersyon (Windows/macOS/Linux)
   - Mga hakbang upang muling likhain
   - Inaasahang vs. aktwal na pag-uugali
   - Mga screenshot o terminal output kung maaari

---

## 💡 Paano Magmungkahi ng mga Tampok

1. Pumunta sa [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. I-click ang **"New Issue"**
3. Pumili ng **"Feature Request"** na template
4. Ilarawan:
   - Anong problema ang iyong nilulutas
   - Paano mo ito naiisip na gumagana
   - Anumang alternatibong iyong isinasaalang-alang

---

## 🔌 Paano Mag-submit ng Plugin

Ang WIA SOOM ay may makapangyarihang sistema ng plugin — maaari kang bumuo ng iyong sariling plugin sa loob ng 5 minuto.

### Mabilis na Simula
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Buong Gabay

Basahin ang **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** para sa:
- Kumpletong API reference
- Mga gumaganang halimbawa
- Mga step-by-step na tutorial
- Mga pinakamahusay na kasanayan at mga alituntunin sa seguridad

### Isumite ang Iyong Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Idagdag ang iyong plugin sa `plugins/{your-plugin-name}/`
3. Mag-submit ng Pull Request
4. Pagkatapos ng pagsusuri, ang iyong plugin ay lilitaw sa Plugin Store para sa lahat ng gumagamit!

---

## 🔀 Paano Mag-submit ng Pull Request

### Para sa pangunahing app (wia-soom)

1. Fork ang repository
2. Lumikha ng feature branch: `git checkout -b feat/my-feature`
3. Gawin ang iyong mga pagbabago
4. Subukan nang lokal:
   ```bash
   ```
5. Mag-commit na may malinaw na mensahe:
   ```
   feat: add dark mode toggle to settings
   ```
6. I-push at buksan ang PR laban sa `main`

### Konbensyon ng Mensahe ng Commit

| Prefix | Gamit para sa |
|--------|---------------|
| `feat:` | Bagong tampok |
| `fix:` | Pag-aayos ng bug |
| `docs:` | Dokumentasyon lamang |
| `refactor:` | Pag-restructure ng code (walang pagbabago sa pag-uugali) |
| `i18n:` | Mga update sa pagsasalin |
| `plugin:` | Mga pagbabago na may kaugnayan sa plugin |

### PR Checklist

- [ ] Tumakbo ang code nang walang mga error
- [ ] Walang hardcoded na mga string (gamitin ang mga i18n key)
- [ ] Walang `console.log` na naiwan sa production code
- [ ] Patuloy na pumasa ang mga umiiral na pagsubok

---

## 🌐 Mga Kontribusyon sa Pagsasalin (254 Wika)

Sinusuportahan ng WIA SOOM ang **254 wika** — mula Amharic hanggang Zulu, kabilang ang Braille at mga wika na RTL.

### Paano Gumagana ang Pagsasalin

- Base language file: `src/renderer/src/i18n/en.json`
- Lahat ng 254 na language file ay nasa parehong direktoryo
- Ang pagsasalin ay ginagawa sa pamamagitan ng `scripts/translate-patch.js` (GPT-4o-mini API)

### Paano Mag-contribute ng mga Pagsasalin

#### Opsyon 1: Ayusin ang isang tiyak na pagsasalin

1. Hanapin ang language file: `src/renderer/src/i18n/{lang-code}.json`
2. Ayusin ang maling pagsasalin
3. Mag-submit ng PR na may pagbabago

#### Opsyon 2: Magdagdag ng mga nawawalang key
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsyon 3: Suriin ang mga machine translation

Marami sa aming 254 na wika ay na-translate ng machine. Ang mga pagsusuri mula sa mga katutubong nagsasalita ay napakahalaga!

1. Piliin ang iyong language file
2. Suriin ang mga pagsasalin
3. Ayusin ang anumang hindi maganda o maling pagsasalin
4. Mag-submit ng PR

### Mga Kodigo ng Wika

Gumagamit kami ng mga standard na ISO 639-1 na kodigo (hal., `ko`, `en`, `ja`, `ar`, `hi`) na may mga rehiyonal na variant kung kinakailangan (hal., `zh-CN`, `pt-BR`).

---

## 🛠 Pag-set Up ng Pag-unlad

### Mga Kinakailangan

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Tandaan: Ang default na 2GB heap ay hindi sapat dahil sa 254 language files + Monaco editor bundle (~38MB renderer).

### Estruktura ng Proyekto
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

Kahit na nag-aayos ka ng typographical error, nagsasalin ng isang string, bumubuo ng plugin, o nagdaragdag ng isang malaking tampok — **ikaw ay bahagi ng kwentong ito.**

---

<p align="center"><em>Itinayo ng ❤️ ng SmileStory Inc. at mga kontribyutor sa buong mundo.</em></p>