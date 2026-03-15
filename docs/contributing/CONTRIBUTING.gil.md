<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Te Kautautau ki WIA SOOM</h1>
<p align="center"><strong>E te taetae, e te taetae, e te taetae!</strong></p>
<p align="center">Kare e mea, ko te whakatika hapa, ko te āhuatanga hou, ko te tāpiri, ko te whakamaori — ko te taetae katoa he mea nui.</p>

---

## Rārangi o ngā Ihirangi

- [Ture Whakauru](#code-of-conduct)
- [Me Pehea te Ripoata Hapa](#-how-to-report-bugs)
- [Me Pehea te Tūtohu Āhuatanga](#-how-to-suggest-features)
- [Me Pehea te Tuku i te Tāpirihanga](#-how-to-submit-a-plugin)
- [Me Pehea te Tuku i te Pūrongo Tūtohu](#-how-to-submit-a-pull-request)
- [Ngā Kōwhiringa Whakamaori (254 Reo)](#-translation-contributions-254-languages)
- [Te Whakarite i te Whakaaturanga](#-development-setup)

---

## Ture Whakauru

Kei te ū mātou ki te whakarato i te wheako pōwhiri me te whakauru mō te katoa.

- **Kia whakaute.** Whakautea te katoa.
- **Kia whaihua.** Tukua he urupare whaihua, kaua e waiho he whakakino.
- **Kia whakauru.** Ka tautoko mātou i ngā reo 254, ā, ka pōwhiria ngā kaitautoko mai i ngā whenua katoa o te Ao.
- **Kāore e whakatūpato.** Kāore e taea te whakatūpato mo te whakahiato o tetahi momo.

---

## 🐛 Me Pehea te Ripoata Hapa

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Tīpakohia te pūtea **"Bug Report"**
4. Whakauruhia:
   - Putanga WIA SOOM (Tautuhinga → Mo)
   - OS me te putanga (Windows/macOS/Linux)
   - Ngā hikoinga ki te whakahou
   - Te tumanako vs. te whanaketanga
   - Ngā whakaahua, kāore e taea te whakakī i te putanga

---

## 💡 Me Pehea te Tūtohu Āhuatanga

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Tīpakohia te pūtea **"Feature Request"**
4. Whakaahuatia:
   - He aha te raru e whakatika ana koe
   - Me pēhea e whakaaro ana koe ka mahi
   - He kōwhiringa kē e whakaaro ana koe

---

## 🔌 Me Pehea te Tuku i te Tāpirihanga

He pūnaha tāpirihanga kaha te WIA SOOM — ka taea e koe te hanga i tō tāpirihanga i roto i te 5 meneti.

### Tīmatanga Tere
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Aratohu Katoa

Pānuihia te **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mō:
- Te rārangi API katoa
- Ngā tauira mahi
- Ngā akoranga ā-ringa
- Ngā tikanga pai me ngā ture haumaru

### Tukua Tō Tāpirihanga

1. Fork [Plugin Store](https://wiasoom.com)
2. Tāpirihia tō tāpirihanga ki `plugins/{your-plugin-name}/`
3. Tukua he Pūrongo Tūtohu
4. I muri i te arotake, ka puta tō tāpirihanga ki te Toa Tāpirihanga mō ngā kaiwhakamahi katoa!

---

## 🔀 Me Pehea te Tuku i te Pūrongo Tūtohu

### Mō te tono matua (wia-soom)

1. Fork te whare pukapuka
2. Hangaia he peka āhuatanga: `git checkout -b feat/my-feature`
3. Whakaritehia ō panoni
4. Whakamātauria i te kāinga:
   ```bash
   ```
5. Pātea ki te karere mārama:
   ```
   feat: tāpirihia te pātea mō te āhua pouri ki ngā tautuhinga
   ```
6. Pātea me te whakatuwhera i te PR ki `main`

### Tikanga Karere Pūrongo

| Pēke | Whakamahia mō |
|--------|---------|
| `feat:` | Āhuatanga hou |
| `fix:` | Whakatika hapa |
| `docs:` | Pukapuka anake |
| `refactor:` | Whakariterite waehere (kāore he huringa whanaketanga) |
| `i18n:` | Ngā whakahou whakamaori |
| `plugin:` | Ngā panoni e pā ana ki te tāpirihanga |

### Rārangi Tūtohu PR

- [ ] Ka rere te waehere me te kore hapa
- [ ] Kāore he tuhinga i te whakatakoto (whakamahia ngā key i18n)
- [ ] Kāore he `console.log` e waihohia i te waehere whakaputa
- [ ] Ka pā mai ngā whakamātautau o nāna

---

## 🌐 Ngā Kōwhiringa Whakamaori (254 Reo)

Ka tautoko te WIA SOOM i ngā **254 reo** — mai i te Amharic ki te Zulu, tae atu ki te Braille me ngā reo RTL.

### Me Pehea te Mahi o te Whakamaori

- Kōnae reo taketake: `src/renderer/src/i18n/en.json`
- Ko ngā kōnae reo 254 kei roto i te kōpaki kotahi
- Ka mahia te whakamaori mā `scripts/translate-patch.js` (GPT-4o-mini API)

### Me Pehea te Tautoko i ngā Whakamaori

#### Kōwhiringa 1: Whakatika i tētahi whakamaori motuhake

1. Kimihia te kōnae reo: `src/renderer/src/i18n/{lang-code}.json`
2. Whakatikahia te whakamaori he
3. Tukua he PR me te panoni

#### Kōwhiringa 2: Tāpirihia ngā ki e ngaro ana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kōwhiringa 3: Arotakehia ngā whakamaori miihini

He maha o ngā reo 254 i whakamaorihia e te miihini. He tino uara ngā arotake a ngā kaikawe!

1. Tīpickia tō kōnae reo
2. Arotakehia ngā whakamaori
3. Whakatikahia ngā whakamaori raru, he raru
4. Tukua he PR

### Ngā Waehere Reo

Ka whakamahi mātou i ngā waehere ISO 639-1 paerewa (hei tauira, `ko`, `en`, `ja`, `ar`, `hi`) me ngā rerekētanga rohe i te wā e hiahiatia ana (hei tauira, `zh-CN`, `pt-BR`).

---

## 🛠 Te Whakarite i te Whakaaturanga

### Ngā Tūtohu

- Node.js 18+
- npm 9+
- Git

### Whakarite
```bash
```
### Hanga
```bash
```
> Tēnā, tirohia: Ko te 2GB heap kāore e taea te whakatutuki i te 254 kōnae reo + te pūnaha Monaco (~38MB renderer).

### Hanganga o te Pūkete
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

## 🙏 Te Mauri

Te taetae n te WIA SOOM e na riki n te taetae ni kabane n te ao.

Tei te taetae n te taetae n te riki, tei te taetae n te taetae n te riki, tei te taetae n te plugin, a tei te taetae n te taetae n te riki — **ko ni aei n te taetae n aei.**

---

<p align="center"><em>Te taetae n te ❤️ n te SmileStory Inc. a na riki n te ao.</em></p>