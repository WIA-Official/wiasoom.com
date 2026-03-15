<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Te Tautoko i te WIA SOOM</h1>
<p align="center"><strong>Ka pai tāu tautoko!</strong></p>
<p align="center">Ahakoa he raru, he āhuatanga hou, he plugin, he whakamaori — he mea nui te tautoko katoa.</p>

---

## Rārangi Pūrongo

- [Tikanga Whakaakoranga](#code-of-conduct)
- [Me pēhea te Ripoata i ngā Raru](#-how-to-report-bugs)
- [Me pēhea te Tūtohu i ngā Āhuatanga](#-how-to-suggest-features)
- [Me pēhea te Tuku i tētahi Plugin](#-how-to-submit-a-plugin)
- [Me pēhea te Tuku i tētahi Pull Request](#-how-to-submit-a-pull-request)
- [Ngā Tautoko Whakamāori (254 Reo)](#-translation-contributions-254-languages)
- [Te Tautuhi i te Whakaritenga](#-development-setup)

---

## Tikanga Whakaakoranga

Kei te ū mātou ki te whakarato i tētahi wheako pōwhiri me te whakauru mō te katoa.

- **Me whai whakaaro.** Whakamahia te mana o te tangata.
- **Me whai hua.** Whakarato i ngā urupare whai hua, kāore i ng�� whakakāhoretanga.
- **Me whakauru.** Ka tautokohia e mātou ngā reo 254, ā, ka pōwhiria ngā kaitautoko mai i ngā whenua katoa o te Ao.
- **Kāore e whakahawea.** Kāore e taea te whakatūpato mō te whakatūpato i ngā momo katoa.

---

## 🐛 Me pēhea te Ripoata i ngā Raru

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Tīpakohia te **"Bug Report"** tauira
4. Whakauruhia:
   - WIA SOOM putanga (Tautuhinga → Pātea)
   - Pūnaha whakahaere me te putanga (Windows/macOS/Linux)
   - Ngā hikoinga ki te whakahou
   - Ngā whanonga e tumanakohia ana vs. te whanonga i kitea
   - Ngā whakaahua, kāore e taea, i te terminal

---

## 💡 Me pēhea te Tūtohu i ngā Āhuatanga

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Tīpakohia te **"Feature Request"** tauira
4. Whakaahuatia:
   - He aha te raru e whakatika ana koe
   - Me pēhea e whakaaro ana koe ka mahi
   - He kōwhiringa kē atu i whakaaro ai koe

---

## 🔌 Me pēhea te Tuku i tētahi Plugin

He pūnaha plugin kaha te WIA SOOM — ka taea e koe te hanga i tāu ake plugin i roto i te 5 meneti.

### Tīmata Tere
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Aratohu Katoa

Pānuitia te **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mō:
- Te rīpene API katoa
- Ngā tauira mahi
- Ngā akoranga ā-ringa
- Ngā tikanga pai me ngā ture haumaru

### Tuku i tāu Plugin

1. Whakaritea [Plugin Store](https://wiasoom.com)
2. Tāpirihia tāu plugin ki `plugins/{your-plugin-name}/`
3. Tuku i tētahi Pull Request
4. I muri i te arotake, ka puta tāu plugin ki te Plugin Store mō ngā kaiwhakamahi katoa!

---

## 🔀 Me pēhea te Tuku i tētahi Pull Request

### Mō te tono matua (wia-soom)

1. Whakaritea te whare pukapuka
2. Hangaia he peka āhuatanga: `git checkout -b feat/my-feature`
3. Whakamahia āu panoni
4. Whakamātauria i te kāinga:
   ```bash
   ```
5. Tūtohu ki tētahi karere mārama:
   ```
   feat: tāpirihia te pātea mō te āhua pouri ki ngā tautuhinga
   ```
6. Pāwhiri me te whakatuwhera i tētahi PR ki te `main`

### Tikanga Pānui Tūtohu

| Tīpako | Whakamahia mō |
|--------|---------------|
| `feat:` | Āhuatanga hou |
| `fix:` | Whakatika raru |
| `docs:` | Pūrongo anake |
| `refactor:` | Whakariterite waehere (kāore he huringa whanonga) |
| `i18n:` | Ngā whakahou whakamaori |
| `plugin:` | Ngā panoni e pā ana ki te plugin |

### Rārangi Tīmata PR

- [ ] Ka rere te waehere kāore he hapa
- [ ] Kāore he tuhinga i te tuhinga (whakamahia ngā ki i18n)
- [ ] Kāore he `console.log` i mahue i te waehere whakaputa
- [ ] Ka pā mai ngā whakamātautau o nānā

---

## 🌐 Ngā Tautoko Whakamāori (254 Reo)

Ka tautokohia e te WIA SOOM **254 reo** — mai i te Amharic ki te Zulu, tae atu ki te Braille me ngā reo RTL.

### Me pēhea te Mahi o te Whakamāori

- Kōnae reo matua: `src/renderer/src/i18n/en.json`
- Kei roto katoa ngā kōnae reo 254 i te kāinga kotahi
- Ka mahia te whakamaori mā `scripts/translate-patch.js` (GPT-4o-mini API)

### Me pēhea te Tautoko i ngā Whakamāori

#### Kōwhiringa 1: Whakatika i tētahi whakamaori motuhake

1. Kimihia te kōnae reo: `src/renderer/src/i18n/{lang-code}.json`
2. Whakatikaina te whakamaori he hē
3. Tuku i tētahi PR me te panoni

#### Kōwhiringa 2: Tāpirihia ngā ki e ngaro ana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kōwhiringa 3: Arotake i ngā whakamaori mīhini

He maha o ō mātou 254 reo i whakamaorihia e te mīhini. He tino uara ngā arotake mai i ngā kaikawe reo!

1. Kōwhiria tāu kōnae reo
2. Arotakehia ngā whakamaori
3. Whakatikaina ngā whakamaori e pāngia ana, he hē
4. Tuku i tētahi PR

### Ngā Waehere Reo

Ka whakamahia e mātou ngā waehere paerewa ISO 639-1 (hei tauira, `ko`, `en`, `ja`, `ar`, `hi`) me ngā rerekētanga ā-rohe i te wā e tika ana (hei tauira, `zh-CN`, `pt-BR`).

---

## 🛠 Te Tautuhi i te Whakaritenga

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
> Tēnā, tirohia: Kāore te 2GB heap kāore e taea te whakamahi i ngā kōnae reo 254 + te pūnaha Monaco (~38MB renderer).

### Hanganga Pūranga
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

## 🙏 Meitaki Maata

Kua tāpiri teia i te WIA SOOM ki te mea pai mō ngā kaihanga i te ao katoa.

Kāore e taea te mea i te mea kāore e taea te whakatika i tētahi hapa, te whakamāori i tētahi rārangi, te hanga i tētahi plugin, i te tāpiri i tētahi āhuatanga nui — **koe te wāhanga o tēnei kōrero.**

---

<p align="center"><em>Te hanga i runga i te ❤️ e SmileStory Inc. me ngā kaiwhakawhanake i te ao.</em></p>