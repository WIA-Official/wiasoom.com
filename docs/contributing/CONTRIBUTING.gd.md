<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">A' cur ri WIA SOOM</h1>
<p align="center"><strong>Tha sinn a' cur fàilte air do chuid a' cur ris!</strong></p>
<p align="center">Ge bith a bheil e na chùl-fhiosrachadh, feart ùr, plugan, no eadar-theangachadh — tha gach cur ris cudromach.</p>

---

## Clàr-innse

- [Còd Giùlain](#code-of-conduct)
- [Mar a Dh'Fhaodas Tu Cùl-fhiosrachadh a Thogail](#-how-to-report-bugs)
- [Mar a Dh'Fhaodas Tu Feartan a Mholadh](#-how-to-suggest-features)
- [Mar a Dh'Fhaodas Tu Plugan a Thaisbeanadh](#-how-to-submit-a-plugin)
- [Mar a Dh'Fhaodas Tu Iarrtas Tarraing a Thaisbeanadh](#-how-to-submit-a-pull-request)
- [Cur-ris Eadar-theangachaidh (254 Cànanan)](#-translation-contributions-254-languages)
- [Suidheachadh Leasachaidh](#-development-setup)

---

## Còd Giùlain

Tha sinn dealasach a thaobh a' toirt seachad eòlas fàilteach agus aonaichte do gach duine.

- **Bi spèisail.** Cuir ris gach duine le urram.
- **Bi togail.** Thoir freagairtean feumail, chan e breithneachadh destructive.
- **Bi aonaichte.** Tha sinn a' toirt taic do 254 cànanan agus a' cur fàilte air luchd-cur ris bho gach dùthaich air an Talamh.
- **Gun droch-rùn.** Neo-fhaighinn airson freagairtean sam bith de dhroch-rùn.

---

## 🐛 Mar a Dh'Fhaodas Tu Cùl-fhiosrachadh a Thogail

1. Thig gu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cuir a-steach **"Cùl-fhiosrachadh Ùr"**
3. Tagh an **"TEMPLATE Cùl-fhiosrachadh"**
4. Cuir a-steach:
   - Version WIA SOOM (Settings → About)
   - OS agus version (Windows/macOS/Linux)
   - Ceumannan airson ath-reproducing
   - Giùlan dùil vs. giùlan fìor
   - Screenshots no toradh terminal ma ghabhas

---

## 💡 Mar a Dh'Fhaodas Tu Feartan a Mholadh

1. Thig gu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cuir a-steach **"Cùl-fhiosrachadh Ùr"**
3. Tagh an **"TEMPLATE Iarrtas Feart"**
4. Tuairisgeul:
   - Dè an duilgheadas a tha thu a' fuasgladh
   - Ciamar a tha thu a' smaoineachadh gum bi e ag obair
   - Sam bith roghainnean a tha thu air a bhith a' beachdachadh

---

## 🔌 Mar a Dh'Fhaodas Tu Plugan a Thaisbeanadh

Tha siostam plugan làidir aig WIA SOOM �� faodaidh tu do phlugan fhèin a thogail ann an 5 mionaidean.

### Tòiseach
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Stiùireadh Làn

Leugh an **[Stiùireadh Leasachaidh Plugan](docs/PLUGIN_DEVELOPER_GUIDE.md)** airson:
- Iomradh API iomlan
- Eisimpleirean obrach
- Teagaisg ceum air cheum
- Na cleachdaidhean as fheàrr agus riaghailtean tèarainteachd

### Thoir a-steach do phlugan

1. Fork [Plugin Store](https://wiasoom.com)
2. Cuir do phlugan gu `plugins/{your-plugin-name}/`
3. Thoir a-steach Iarrtas Tarraing
4. Às deidh dha a bhith air a dhearbhadh, nochdaidh do phlugan anns a' Plugin Store airson a h-uile neach-cleachdaidh!

---

## 🔀 Mar a Dh'Fhaodas Tu Iarrtas Tarraing a Thaisbeanadh

### Airson an aplacaid mhòr (wia-soom)

1. Fork an stòr
2. Cruthaich brath feart: `git checkout -b feat/my-feature`
3. Dèan do dh'atharrachaidhean
4. Deuchainn gu h-ionadail:
   ```bash
   ```
5. Cuir a-steach le teachdaireachd soilleir:
   ```
   feat: cuir toggle modh dorcha ris na settings
   ```
6. Push agus fosgail PR an aghaidh `main`

### Freagairtean Teagaisg Commit

| Prefix | Cleachd airson |
|--------|---------|
| `feat:` | Feart ùr |
| `fix:` | Cùl-fhiosrachadh |
| `docs:` | D documentation a-mhàin |
| `refactor:` | Ath-structair còd (gun atharrachadh giùlain) |
| `i18n:` | Ùrachaidhean eadar-theangachaidh |
| `plugin:` | Atharrachaidhean co-cheangailte ri plugan |

### Liosta-check PR

- [ ] Bidh am còd ag obair gun mhearachdan
- [ ] Chan eil sreangan cruaidh (cleachd i18n keys)
- [ ] Chan eil `console.log` air fhàgail ann an còd cinneasachaidh
- [ ] Tha na deuchainnean a th' ann fhathast a' dol

---

## 🌐 Cur-ris Eadar-theangachaidh (254 Cànanan)

Tha WIA SOOM a' toirt taic do **254 cànanan** — bho Amharic gu Zulu, a' gabhail a-steach Braille agus cànanan RTL.

### Mar a tha Eadar-theangachadh ag Obair

- Faidhle cànain bun: `src/renderer/src/i18n/en.json`
- Tha na 254 faidhlichean cànain uile anns an aon earrann
- Tha eadar-theangachadh air a dhèanamh tro `scripts/translate-patch.js` (GPT-4o-mini API)

### Mar a Dh'Fhaodas Tu Cur-ris Eadar-theangachaidh

#### Roghainn 1: Cuir às do eadar-theangachadh sònraichte

1. Lorg an faidhle cànain: `src/renderer/src/i18n/{lang-code}.json`
2. Cuir às don eadar-theangachadh ceàrr
3. Thoir a-steach PR leis an atharrachadh

#### Roghainn 2: Cuir ris na keys a tha a dhìth
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Roghainn 3: Ath-sgrùdadh eadar-theangachaidhean innealan

Tha mòran de na 254 cànanan againn air an eadar-theangachadh le innealan. Tha ath-sgrùdaidhean bho neach-labhairt nàdarra gu math luachmhor!

1. Tagh do fhàidhle cànain
2. Ath-sgrùdadh na h-eadar-theangachaidhean
3. Cuir às do na h-eadar-theangachaidhean mì-chliùiteach no ceàrr
4. Thoir a-steach PR

### Còd Cànain

Cleachdaidh sinn còdan ISO 639-1 àbhaisteach (e.g., `ko`, `en`, `ja`, `ar`, `hi`) le variants roinneil far a bheil feum (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Suidheachadh Leasachaidh

### Riatanasan

- Node.js 18+
- npm 9+
- Git

### Suidheachadh
```bash
```
### Togail
```bash
```
> Nota: Chan eil an heap 2GB àbhaisteach gu leòr air sgàth na 254 faidhlichean cànain + pacadh Monaco (~38MB renderer).

### Structar Pròiseact
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

## 🙏 Tapadh Leat

Tha gach freagairte a’ dèanamh WIA SOOM nas fheàrr do luchd-leasachaidh air feadh an t-saoghail.

Ge bith a bheil thu a’ ceartachadh mearachd, a’ eadar-theangachadh sreang, a’ togail plugan, no a’ cur feart mòr ris — **tha thu nad phàirt den sgeulachd seo.**

---

<p align="center"><em>Air a thogail le ❤️ le SmileStory Inc. agus luchd-freagairte air feadh an t-saoghail.</em></p>