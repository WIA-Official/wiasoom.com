<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kupereka kwa WIA SOOM</h1>
<p align="center"><strong>Tikufuna kuti mupereke!</strong></p>
<p align="center">Ngakhale kuti ndi kukonza zolakwika, mawonekedwe atsopano, plugin, kapena kutanthauzira — kupereka kulikonse kumakhala kofunika.</p>

---

## Tafalasi ya Zomwe Zili Mu Buku

- [Code of Conduct](#code-of-conduct)
- [How to Report Bugs](#-how-to-report-bugs)
- [How to Suggest Features](#-how-to-suggest-features)
- [How to Submit a Plugin](#-how-to-submit-a-plugin)
- [How to Submit a Pull Request](#-how-to-submit-a-pull-request)
- [Translation Contributions (254 Languages)](#-translation-contributions-254-languages)
- [Development Setup](#-development-setup)

---

## Code of Conduct

Tili ndi chikhumbo chogwiritsa ntchito chidziwitso chabwino komanso chovomerezeka kwa aliyense.

- **Khalani ndi ulemu.** Kuthandiza aliyense ndi ulemu.
- **Khalani okhazikika.** Perekani ndemanga zothandiza, osati zotsutsa.
- **Khalani ovomerezeka.** Tikulandira zilankhulo 254 ndipo tikulandira opereka kuchokera m'maiko onse a pa Dziko lapansi.
- **Palibe kupusitsa.** Palibe tolerance ya kusankhana kwachilendo chilichonse.

---

## 🐛 Momwe Mungathetse Zolakwika

1. Pitani ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Dinani **"New Issue"**
3. Sankhani **"Bug Report"** template
4. Onjezerani:
   - WIA SOOM version (Settings → About)
   - OS ndi version (Windows/macOS/Linux)
   - Njira zoti mupeze zolakwika
   - Zomwe mukuyembekezera vs. zomwe zili
   - Zithunzi kapena chidziwitso cha terminal ngati zingatheke

---

## 💡 Momwe Mungapereke Mawonekedwe

1. Pitani ku [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Dinani **"New Issue"**
3. Sankhani **"Feature Request"** template
4. Fotokozani:
   - Chifukwa chiyani mukukonza
   - Momwe mukuganizira kuti izigwira ntchito
   - Zinthu zina zomwe mwaganizira

---

## 🔌 Momwe Mungapereke Plugin

WIA SOOM ili ndi dongosolo la plugin lolimba — mutha kupanga plugin yanu mu mphindi 5.

### Quick Start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Full Guide

Werengani **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** kuti:
- Mauthenga a API apamwamba
- Zitsanzo zothandiza
- Malangizo a njira-zinthu
- Zoyenera kuchita ndi malamulo a chitetezo

### Perekani Plugin Yanu

1. Fork [Plugin Store](https://wiasoom.com)
2. Onjezerani plugin yanu ku `plugins/{your-plugin-name}/`
3. Perekani Pull Request
4. Pambuyo pa kuyang'aniridwa, plugin yanu ikuwoneka mu Plugin Store kwa ogwiritsa ntchito onse!

---

## 🔀 Momwe Mungapereke Pull Request

### Kwa pulogalamu yayikulu (wia-soom)

1. Fork repository
2. Pangani gulu la mawonekedwe: `git checkout -b feat/my-feature`
3. Sinthani zosintha zanu
4. Yesani pamalo:
   ```bash
   ```
5. Chitani ndi uthenga wosavuta:
   ```
   feat: onjezerani dark mode toggle ku settings
   ```
6. Push ndi kutsegula PR ku `main`

### Commit Message Convention

| Prefix | Use for |
|--------|---------|
| `feat:` | Mawonekedwe atsopano |
| `fix:` | Kukonza zolakwika |
| `docs:` | Zolemba zokha |
| `refactor:` | Kukhazikitsa kocode (palibe kusintha kwa makhalidwe) |
| `i18n:` | Zosintha za kutanthauzira |
| `plugin:` | Zosintha zokhudza plugin |

### PR Checklist

- [ ] Code ikugwira ntchito popanda zolakwika
- [ ] Palibe ma string omwe akugwiritsidwa ntchito (gwiritsani ntchito i18n keys)
- [ ] Palibe `console.log` yomwe yasala mu code yopanga
- [ ] Zoyesedwa zomwe zili zoyenera zikusunga

---

## 🌐 Zopereka za Kutanthauzira (254 Languages)

WIA SOOM imathandiza **254 languages** — kuchokera ku Amharic mpaka Zulu, kuphatikiza Braille ndi zilankhulo za RTL.

### Momwe Kutanthauzira Kumagwira

- Fayilo ya chilankhulo chachikulu: `src/renderer/src/i18n/en.json`
- Mafayilo a zilankhulo 254 ali mu directory imodzi
- Kutanthauzira kumachitika kudzera mu `scripts/translate-patch.js` (GPT-4o-mini API)

### Momwe Mungapereke Zotsatira Zokutanthauzira

#### Option 1: Kukhazikitsa kutanthauzira komwe kuli koyipa

1. Fufuzani fayilo ya chilankhulo: `src/renderer/src/i18n/{lang-code}.json`
2. Kukhazikitsa kutanthauzira komwe kuli koyipa
3. Perekani PR ndi kusintha

#### Option 2: Onjezerani makiyi osowa
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Onani kutanthauzira kwa makina

Zilankhulo zathu 254 zambiri zinasinthidwa ndi makina. Kuonetsedwa kwa anthu omwe akukhala m'dziko la chilankhulo kumakhala kofunika kwambiri!

1. Sankhani fayilo yanu ya chilankhulo
2. Onani kutanthauzira
3. Kukhazikitsa kutanthauzira kulikonse komwe kuli koyipa kapena kosakwanira
4. Perekani PR

### Makodi a Chilankhulo

Timagwiritsa ntchito makodi a ISO 639-1 (mwachitsanzo, `ko`, `en`, `ja`, `ar`, `hi`) ndi zosintha za mdera pamene zikufunika (mwachitsanzo, `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Zofunikira

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Chidziwitso: 2GB heap yachikhalidwe sichikwanira chifukwa cha mafayilo a zilankhulo 254 + Monaco editor bundle (~38MB renderer).

### Structure ya Project
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

## 🙏 Zikomo

Kuthandiza kulimbikitsa WIA SOOM kwa opanga pa dziko lonse.

Koma mutakhazikitsa mawu, kutanthauzira chingwe, kupanga pulogalamu, kapena kuwonjezera chinthu chachikulu — **ndinu gawo la nkhaniyi.**

---

<p align="center"><em>Yopangidwa ndi ❤️ ndi SmileStory Inc. ndi ogwira ntchito padziko lonse.</em></p>