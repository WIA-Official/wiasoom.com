<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Te Tautoko i te WIA SOOM</h1>
<p align="center"><strong>Ka hiahia matou i au tautoko!</strong></p>
<p align="center">Ahakoa he whakatikatika hapa, he āhuatanga hou, he plugin, he whakamaori — he mea nui te tautoko katoa.</p>

---

## Rārangi o ngā Ihirangi

- [Tikanga Whakaari](#code-of-conduct)
- [Me pēhea te Ripoata Hapa](#-how-to-report-bugs)
- [Me pēhea te Tūtohu Āhuatanga](#-how-to-suggest-features)
- [Me pēhea te Tuku i te Plugin](#-how-to-submit-a-plugin)
- [Me pēhea te Tuku i te Pull Request](#-how-to-submit-a-pull-request)
- [Ngā Tautoko Whakamaori (254 Reo)](#-translation-contributions-254-languages)
- [Te Whakarite i te Whakawhanaketanga](#-development-setup)

---

## Tikanga Whakaari

Ka whakapau kaha mātou ki te whakarato i te wheako pōwhiri me te whakauru mō te katoa.

- **Kia whakaute.** Whakamahia te manaaki ki te katoa.
- **Kia whai hua.** Whakarato i ngā urupare whai hua, kāore e pāngia.
- **Kia whakauru.** Ka tautoko mātou i ngā reo 254, ā, ka pōwhiri i ngā kaitautoko mai i ngā whenua katoa o te Ao.
- **Kāore e pāngia.** Kāore e taea te whakaroa i te whakatūpato mō ngā momo whakahawea.

---

## 🐛 Me pēhea te Ripoata Hapa

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Whiriwhiri i te tauira **"Bug Report"**
4. Whakauruhia:
   - Putanga WIA SOOM (Tautuhinga → Mo)
   - Pūnaha whakahaere me te putanga (Windows/macOS/Linux)
   - Ngā hikoinga ki te whakahou
   - Te tumanako vs. te whanonga tūturu
   - Ngā whakaahua, ngā putanga terminal mēnā ka taea

---

## 💡 Me pēhea te Tūtohu Āhuatanga

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Whiriwhiri i te tauira **"Feature Request"**
4. Whakaahuatia:
   - He aha te raru e whakatika ana koe
   - Me pēhea e whakaaro ana koe ka mahi
   - He kōwhiringa kē e whakaaro ana koe

---

## 🔌 Me pēhea te Tuku i te Plugin

He pūnaha plugin kaha te WIA SOOM — ka taea e koe te hanga i tō ake plugin i roto i te 5 meneti.

### Tiimata Tere
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Aratohu Pūrākau

Pānuitia te **[Aratohu Kaihanga Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** mō:
- Te whakaahua API katoa
- Ngā tauira mahi
- Ngā akoranga ā-tuhinga
- Ngā tikanga pai me ngā ture haumaru

### Tuku i tō Plugin

1. Whakauru [Plugin Store](https://wiasoom.com)
2. Tāpirihia tō plugin ki `plugins/{your-plugin-name}/`
3. Tuku i te Pull Request
4. I muri i te arotake, ka puta tō plugin ki te Toa Plugin mō ngā kaiwhakamahi katoa!

---

## 🔀 Me pēhea te Tuku i te Pull Request

### Mō te taupānga matua (wia-soom)

1. Whakauru i te whare pukapuka
2. Hangaia he peka āhuatanga: `git checkout -b feat/my-feature`
3. Whakamahia āu huringa
4. Whakamātauria i te taha:
   ```bash
   ```
5. Tūtohu ki te karere mārama:
   ```
   feat: tāpiri i te pātea pango ki ngā tautuhinga
   ```
6. Pāwhiri ki te tuku me te whakatuwhera i te PR ki te `main`

### Tikanga Karere Tūtohu

| Tīpako | Whakamahia mō |
|--------|---------------|
| `feat:` | Āhuatanga hou |
| `fix:` | Whakatikatika hapa |
| `docs:` | Pukapuka anake |
| `refactor:` | Whakariterite waehere (kāore he huringa whanonga) |
| `i18n:` | Ngā whakahou whakamaori |
| `plugin:` | Ngā huringa e pā ana ki te plugin |

### Rārangi Tūtohu PR

- [ ] Ka rere te waehere kāore he hapa
- [ ] Kāore he tuhinga i te tuhinga (whakamahia ngā ki i18n)
- [ ] Kāore he `console.log` i mahue i te waehere whakaputa
- [ ] Ka pā ana ngā whakamātautau e wātea ana

---

## 🌐 Ngā Tautoko Whakamaori (254 Reo)

Ka tautoko te WIA SOOM i **254 reo** — mai i te Amharic ki te Zulu, tae atu ki te Braille me ngā reo RTL.

### Me pēhea te Whakahaere i ngā Whakamaori

- Kōnae reo matua: `src/renderer/src/i18n/en.json`
- Kei roto i te kōpaki kotahi ngā kōnae reo 254
- Ka mahia te whakamaori mā `scripts/translate-patch.js` (GPT-4o-mini API)

### Me pēhea te Tautoko i ngā Whakamaori

#### Kōwhiringa 1: Whakatika i tētahi whakamaori motuhake

1. Kimihia te kōnae reo: `src/renderer/src/i18n/{lang-code}.json`
2. Whakatikahia te whakamaori hē
3. Tuku i te PR me te huringa

#### Kōwhiringa 2: Tāpirihia ngā ki e ngaro ana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kōwhiringa 3: Arotake i ngā whakamaori miihini

He maha o ngā reo 254 i whakamaorihia e te miihini. He tino uara ngā arotake a ngā kaipōti!

1. Kōwhiria tō kōnae reo
2. Arotakehia ngā whakamaori
3. Whakatikahia ngā whakamaori raru, he hē rānei
4. Tuku i te PR

### Ngā Waehere Reo

Ka whakamahi mātou i ngā waehere ISO 639-1 paerewa (hei tauira, `ko`, `en`, `ja`, `ar`, `hi`) me ngā rerekētanga ā-rohe e hiahiatia ana (hei tauira, `zh-CN`, `pt-BR`).

---

## 🛠 Te Whakarite i te Whakawhanaketanga

### Ngā Whakaritenga

- Node.js 18+
- npm 9+
- Git

### Whakarite
```bash
```
### Hanga
```bash
```
> Tēnā: Kāore te 2GB heap taunoa e ranea i ngā kōnae reo 254 + te pūnaha Monaco (~38MB renderer).

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

## 🙏 Fa'afetai

O le fa'amaonia uma e fesoasoani i le fa'aleleia o WIA SOOM mo tagata atina'e i le lalolagi atoa.

E fa'avae pe e te fa'asa'o se fa'aletonu, fa'aluaina se fa'ata'ita'iga, fa'avae se plugin, po'o le fa'aopoopoina o se fa'avae taua — **o le vaega o lenei tala e te.**

---

<p align="center"><em>O le fa'avae i le ❤️ e SmileStory Inc. ma tagata fa'atau i le lalolagi atoa.</em></p>