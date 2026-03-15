<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Fakaako ki te WIA SOOM</h1>
<p align="center"><strong>Ka fiafia matou ki a koe nga fakaako!</strong></p>
<p align="center">Koe pe he faafitauli, he mea hou, he plugin, pe he faaliliu — e taua te fakaako katoa.</p>

---

## Tohi o nga Akomanga

- [Tuhinga o te Whakahaere](#code-of-conduct)
- [Pehea te Tohu i nga Faafitauli](#-how-to-report-bugs)
- [Pehea te Tohu i nga Mea Hou](#-how-to-suggest-features)
- [Pehea te Tohu i tetahi Plugin](#-how-to-submit-a-plugin)
- [Pehea te Tohu i tetahi Pull Request](#-how-to-submit-a-pull-request)
- [Fakaako Faaliliu (254 Languages)](#-translation-contributions-254-languages)
- [Setup Whanaketanga](#-development-setup)

---

## Tuhinga o te Whakahaere

Kei te whakakotahi matou ki te tuku i tetahi wheako manako me te whakauru mo te katoa.

- **Kia whakahonore.** Whakamahia te tangata katoa ki te mana.
- **Kia whai kiko.** Homai he whakahou awhina, kaua e tuku he whakapae kino.
- **Kia whakauru.** Ka tautoko matou i nga reo 254, ka powhiri i nga kaiwhakaako mai i nga whenua katoa o te Ao.
- **Kaua e whakahiato.** Kore utu mo te whakahawea o tetahi momo.

---

## 🐛 Pehea te Tohu i nga Faafitauli

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Whiriwhiri i te **"Bug Report"** tauira
4. Whakauruhia:
   - WIA SOOM putanga (Tautuhinga → Mo)
   - OS me te putanga (Windows/macOS/Linux)
   - Nga hikoinga hei whakahou
   - He titiro ki te tumanako vs. te whanaketanga
   - Nga whakaahua whakaahua, i te mea ka taea

---

## 💡 Pehea te Tohu i nga Mea Hou

1. Haere ki [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pāwhiri **"New Issue"**
3. Whiriwhiri i te **"Feature Request"** tauira
4. Whakaahuatia:
   - He aha te raru e whakatika ana koe
   - Me pehea e whakaaro ana koe ka mahi
   - Nga whakaritenga kei a koe i whakaaro

---

## 🔌 Pehea te Tohu i tetahi Plugin

He pūnaha plugin kaha te WIA SOOM — ka taea e koe te hanga i to ake plugin i roto i te 5 meneti.

### Tiimata Tere
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Aratohu Katoa

Panuihia te **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mo:
- Te whakaahua API katoa
- Nga tauira mahi
- Nga akoranga i nga hikoinga
- Nga tikanga pai me nga ture haumaru

### Tohu i to Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Tāpiri i to plugin ki `plugins/{your-plugin-name}/`
3. Tohu i tetahi Pull Request
4. I muri i te arotake, ka puta to plugin ki te Plugin Store mo nga kaiwhakamahi katoa!

---

## 🔀 Pehea te Tohu i tetahi Pull Request

### Mo te tono matua (wia-soom)

1. Fork te whare putunga
2. Hangaia he peka āhuatanga: `git checkout -b feat/my-feature`
3. Whakamahia nga huringa
4. Whakamātauria i te rohe:
   ```bash
   ```
5. Tūtohu me te karere mārama:
   ```
   feat: tāpiri i te huri i te mōhiohio ki ngā tautuhinga
   ```
6. Pāwhiri me te whakatuwhera i te PR ki te `main`

### Tikanga Tūtohu

| Tīpako | Whakamahia mo |
|--------|---------------|
| `feat:` | Mea hou |
| `fix:` | Whakatika faafitauli |
| `docs:` | Tuhinga anake |
| `refactor:` | Whakariterite waehere (kāore he huringa whanaketanga) |
| `i18n:` | Whakahou faaliliu |
| `plugin:` | Huringa e pā ana ki te plugin |

### Rārangi Tūtohu PR

- [ ] Ka rere te waehere me te kore he hapa
- [ ] Kaore he tuhinga e pa ana ki te waehere (whakamahia nga key i18n)
- [ ] Kaore he `console.log` i waiho i roto i te waehere whakaputa
- [ ] Ka pā ana tonu nga whakamātautau i mau

---

## 🌐 Fakaako Faaliliu (254 Languages)

Ka tautoko te WIA SOOM i **254 reo** — mai i Amharic ki Zulu, tae atu ki te Braille me nga reo RTL.

### Pehea te Mahi o te Faaliliu

- Kōnae reo matua: `src/renderer/src/i18n/en.json`
- Ko nga kōnae reo 254 katoa kei roto i te kōpaki kotahi
- Ka mahia te faaliliu mā `scripts/translate-patch.js` (GPT-4o-mini API)

### Pehea te Fakaako i nga Faaliliu

#### Kōwhiringa 1: Whakatika i tetahi faaliliu motuhake

1. Kimihia te kōnae reo: `src/renderer/src/i18n/{lang-code}.json`
2. Whakatikaina te faaliliu he
3. Tohu i tetahi PR me te huringa

#### Kōwhiringa 2: Tāpiri i nga ki e ngaro ana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Kōwhiringa 3: Arotake i nga faaliliu miihini

He maha o nga reo 254 i faaliliu i te miihini. Ko nga arotake a nga tangata taketake he mea nui!

1. Kōwhiria to kōnae reo
2. Arotakehia nga faaliliu
3. Whakatikaina nga faaliliu raru, he raru
4. Tohu i tetahi PR

### Kōwae Reo

Ka whakamahia e matou nga waehere ISO 639-1 paerewa (hei tauira, `ko`, `en`, `ja`, `ar`, `hi`) me nga whakarereketanga rohe i te wā e hiahiatia ana (hei tauira, `zh-CN`, `pt-BR`).

---

## 🛠 Setup Whanaketanga

### Nga whakaritenga

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Hanga
```bash
```
> Tuhinga: Ko te 2GB heap taunoa kaore e taea te whakatutuki i te 254 kōnae reo + Monaco editor bundle (~38MB renderer).

### Hanganga Pūkete
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

## 🙏 Fakaaue

Kua toe fakamalolo e WIA SOOM ki he kau fakamaketi kehekehe i he lalolagi.

Koe ki he faka'ali'ali e he ta'u, faka'ali'ali e he string, hanga e he plugin, pe faka'anga e he ngaahi me'a matua — **koe ko e vaega o e tala ko ia.**

---

<p align="center"><em>Hanga e he ❤️ e SmileStory Inc. mo e kau fakamalolo i he lalolagi.</em></p>