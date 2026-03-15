<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Te Tautoko ki te WIA SOOM</h1>
<p align="center"><strong>Ka tino koa mātou ki āu tautoko!</strong></p>
<p align="center">Ahakoa he whakatikanga hapa, he āhuatanga hou, he plugin, he whakamaori — he mea nui te tautoko katoa.</p>

---

## Rārangi Pukapuka

- [Tikanga Whakaute](#code-of-conduct)
- [Me Pehea te Ripoata Hapa](#-how-to-report-bugs)
- [Me Pehea te Tūtohu Āhuatanga](#-how-to-suggest-features)
- [Me Pehea te Tuku i te Plugin](#-how-to-submit-a-plugin)
- [Me Pehea te Tuku i te Pull Request](#-how-to-submit-a-pull-request)
- [Tautoko Whakamaori (254 Reo)](#-translation-contributions-254-languages)
- [Tautuhinga Whakamahi](#-development-setup)

---

## Tikanga Whakaute

Ka whakapau kaha mātou ki te tuku i te wheako pōwhiri me te whakauru mō te katoa.

- **Me whai whakaaro.** Whakamahia te mana o te tangata.
- **Me whai tikanga.** Tukua he whakahokinga kōrero whaihua, kāore he whakahe kino.
- **Me whakauru.** E tautoko ana mātou i ngā reo 254, ā, ka pōwhiria ngā kai tautoko mai i ngā whenua katoa o te Ao.
- **Kāore he whakatūpato.** Kāore he whakaaetanga mō te whakahawea o te momo.

---

## 🐛 Me Pehea te Ripoata Hapa

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Whiriwhiri i te tauira **"Bug Report"**
4. Tāpirihia:
   - Putanga WIA SOOM (Tautuhinga → Mō)
   - Pūnaha whakahaere me te putanga (Windows/macOS/Linux)
   - Ngā hikoinga hei whakahou
   - Te titiro i te tumanako vs. te mahi i te wā
   - Ngā whakaahua, ngā putanga terminal mēnā ka taea

---

## 💡 Me Pehea te Tūtohu Āhuatanga

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Whiriwhiri i te tauira **"Feature Request"**
4. Whakamārama:
   - He aha te raru e whakatika ana koe
   - Me pēhea e whakaaro ana koe ka mahi
   - He kōwhiringa kē e whakaaro ana koe

---

## 🔌 Me Pehea te Tuku i te Plugin

He pūnaha plugin kaha te WIA SOOM — ka taea e koe te hanga i tō ake plugin i roto i te 5 meneti.

### Tīmata Tere
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Aratohu Katoa

Pānuitia te **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mō:
- Te rēhita API katoa
- Ngā tauira mahi
- Ngā akoranga i runga i ngā hikoinga
- Ngā tikanga pai me ngā ture haumaru

### Tukua Tō Plugin

1. Whakaritea [Plugin Store](https://wiasoom.com)
2. Tāpirihia tō plugin ki `plugins/{your-plugin-name}/`
3. Tukua he Pull Request
4. I muri i te arotake, ka puta tō plugin ki te Toa Plugin mō ngā kaiwhakamahi katoa!

---

## 🔀 Me Pehea te Tuku i te Pull Request

### Mō te tono matua (wia-soom)

1. Whakaritea te whare pukapuka
2. Hangaia he peka āhuatanga: `git checkout -b feat/my-feature`
3. Whakamahia āu huringa
4. Whakamātauria i te kāinga:
   ```bash
   ```
5. Tūtohu ki he karere mārama:
   ```
   feat: tāpirihia te pātea pōuri ki ngā tautuhinga
   ```
6. Pāwhiri me te whakatuwhera i te PR ki `main`

### Tikanga Karere Tūtohu

| Pātea | Whakamahia mō |
|--------|---------|
| `feat:` | Āhuatanga hou |
| `fix:` | Whakatikanga hapa |
| `docs:` | Pukapuka anake |
| `refactor:` | Te whakariterite waehere (kāore he huringa i te whanonga) |
| `i18n:` | Ngā whakahou whakamaori |
| `plugin:` | Ngā huringa e pā ana ki te plugin |

### Rārangi Tīwhiri PR

- [ ] Ka rere te waehere kāore he hapa
- [ ] Kāore he tuhinga kua kī i te waehere (whakamahia ngā ki i18n)
- [ ] Kāore he `console.log` i waiho i roto i te waehere whakaputa
- [ ] Ka pā ana ngā whakamātautau e wātea ana

---

## 🌐 Tautoko Whakamaori (254 Reo)

E tautoko ana te WIA SOOM i ngā **254 reo** — mai i te Amharic ki te Zulu, tae atu ki te Braille me ngā reo RTL.

### Me Pehea te Mahi o te Whakamaori

- Kōnae reo matua: `src/renderer/src/i18n/en.json`
- Kei roto i te kōpaki kotahi ngā kōnae reo 254
- Ka mahia te whakamaori mā `scripts/translate-patch.js` (GPT-4o-mini API)

### Me Pehea te Tautoko i ngā Whakamaori

#### Kōwhiringa 1: Whakatika i tētahi whakamaori motuhake

1. Kimihia te kōnae reo: `src/renderer/src/i18n/{lang-code}.json`
2. Whakatikahia te whakamaori he
3. Tukua he PR me te huringa

#### Kōwhiringa 2: Tāpirihia ngā ki e ngaro ana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kōwhiringa 3: Arotake i ngā whakamaori miihini

He maha o ngā reo 254 i whakamaorihia e ngā miihini. He tino whaihua ngā arotake mai i ngā kōrero taketake!

1. Kōwhiria tō kōnae reo
2. Arotakehia ngā whakamaori
3. Whakatikahia ngā whakamaori raru, he rerekē rānei
4. Tukua he PR

### Ngā Waehere Reo

Ka whakamahi mātou i ngā waehere ISO 639-1 paerewa (hei tauira, `ko`, `en`, `ja`, `ar`, `hi`) me ngā rerekētanga ā-rohe i te wā e hiahiatia ana (hei tauira, `zh-CN`, `pt-BR`).

---

## 🛠 Tautuhinga Whakamahi

### Ngā Whakaritenga

- Node.js 18+
- npm 9+
- Git

### Whakaritea
```bash
```
### Hanga
```bash
```
> Tēnā, tirohia: Kāore te 2GB heap taunoa e ranea nāna i ngā kōnae reo 254 + te pūnaha editor Monaco (~38MB renderer).

### Hanganga Kaupapa
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

## 🙏 Ngā Mihi

Ko te whakaritenga katoa e pai ai te WIA SOOM mō ngā kaiwhakawhanake puta noa i te ao.

Ahakoa he whakatika i te hē, he whakamaori i te rārangi, he hanga i tētahi tāpiritanga, kāore he mea nui — **ko koe te wāhanga o tēnei kōrero.**

---

<p align="center"><em>Te hanga i runga i te ❤️ e SmileStory Inc. me ngā kaiwhakawhanake i te ao.</em></p>