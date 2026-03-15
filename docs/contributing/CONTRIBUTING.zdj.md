<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kuchangia WIA SOOM</h1>
<p align="center"><strong>Tunapenda michango yenu!</strong></p>
<p align="center">Iwe ni kurekebisha makosa, kipengele kipya, plugin, au tafsiri — kila mchango ni muhimu.</p>

---

## Orodha ya Yaliyomo

- [Kanuni za Tabia](#code-of-conduct)
- [Jinsi ya Kuripoti Makosa](#-how-to-report-bugs)
- [Jinsi ya Kupendekeza Kipengele](#-how-to-suggest-features)
- [Jinsi ya Kutuma Plugin](#-how-to-submit-a-plugin)
- [Jinsi ya Kutuma Ombi la Mabadiliko](#-how-to-submit-a-pull-request)
- [Michango ya Tafsiri (254 Lugha)](#-translation-contributions-254-languages)
- [Mipangilio ya Maendeleo](#-development-setup)

---

## Kanuni za Tabia

Tumejizatiti kutoa uzoefu wa kukaribisha na wa kujumuisha kwa kila mtu.

- **Heshimu.** Treat everyone with dignity.
- **Kuwa na ujenzi.** Toa mrejesho wa kusaidia, si ukosoaji wa kubomoa.
- **Kuwa wa kujumuisha.** Tunasaidia lugha 254 na tunakaribisha wachangiaji kutoka kila nchi duniani.
- **Hakuna unyanyasaji.** Sera ya sifuri ya uvunjaji wa sheria za aina yoyote.

---

## 🐛 Jinsi ya Kuripoti Makosa

1. Nenda kwenye [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Bonyeza **"New Issue"**
3. Chagua template ya **"Bug Report"**
4. Jumuisha:
   - Toleo la WIA SOOM (Mipangilio → Kuhusu)
   - OS na toleo (Windows/macOS/Linux)
   - Hatua za kurudia
   - Tabia inayotarajiwa dhidi ya ile halisi
   - Picha za skrini au matokeo ya terminal ikiwa inawezekana

---

## 💡 Jinsi ya Kupendekeza Kipengele

1. Nenda kwenye [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Bonyeza **"New Issue"**
3. Chagua template ya **"Feature Request"**
4. Eleza:
   - Ni tatizo gani unalotatua
   - Unafikiri itafanya kazi vipi
   - Mbadala wowote uliozingatia

---

## 🔌 Jinsi ya Kutuma Plugin

WIA SOOM ina mfumo mzuri wa plugin — unaweza kujenga plugin yako mwenyewe ndani ya dakika 5.

### Mwanzo wa Haraka
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Mwongozo Kamili

Soma **[Mwongozo wa Wendelezaji wa Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** kwa:
- Marejeleo kamili ya API
- Mifano inayofanya kazi
- Miongozo ya hatua kwa hatua
- Mbinu bora na sheria za usalama

### Tuma Plugin Yako

1. Fork [Plugin Store](https://wiasoom.com)
2. Ongeza plugin yako kwenye `plugins/{your-plugin-name}/`
3. Tuma Ombi la Mabadiliko
4. Baada ya mapitio, plugin yako itaonekana kwenye Duka la Plugin kwa watumiaji wote!

---

## 🔀 Jinsi ya Kutuma Ombi la Mabadiliko

### Kwa programu kuu (wia-soom)

1. Fork hifadhi
2. Unda tawi la kipengele: `git checkout -b feat/my-feature`
3. Fanya mabadiliko yako
4. Jaribu kwa ndani:
   ```bash
   ```
5. Fanya commit na ujumbe wazi:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push na fungua PR dhidi ya `main`

### Kanuni za Ujumbe wa Commit

| Kichwa | Tumia kwa |
|--------|---------|
| `feat:` | Kipengele kipya |
| `fix:` | Kurekebisha makosa |
| `docs:` | Hati pekee |
| `refactor:` | Upangaji wa msimbo (hakuna mabadiliko ya tabia) |
| `i18n:` | Sasisho za tafsiri |
| `plugin:` | Mabadiliko yanayohusiana na plugin |

### Orodha ya Kuthibitisha PR

- [ ] Msimbo unafanya kazi bila makosa
- [ ] Hakuna nyuzi zilizowekwa (tumia funguo za i18n)
- [ ] Hakuna `console.log` iliyobaki katika msimbo wa uzalishaji
- [ ] Majaribio yaliyopo bado yanapita

---

## 🌐 Michango ya Tafsiri (254 Lugha)

WIA SOOM inaunga mkono **lugha 254** — kutoka Amharic hadi Zulu, ikiwa ni pamoja na Braille na lugha za RTL.

### Jinsi Tafsiri Inavyofanya Kazi

- Faili ya lugha ya msingi: `src/renderer/src/i18n/en.json`
- Faili zote za lugha 254 ziko kwenye saraka moja
- Tafsiri inafanywa kupitia `scripts/translate-patch.js` (GPT-4o-mini API)

### Jinsi ya Kuchangia Tafsiri

#### Chaguo la 1: Rekebisha tafsiri maalum

1. Tafuta faili ya lugha: `src/renderer/src/i18n/{lang-code}.json`
2. Rekebisha tafsiri isiyo sahihi
3. Tuma PR na mabadiliko

#### Chaguo la 2: Ongeza funguo zinazokosekana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Chaguo la 3: Kagua tafsiri za mashine

Lugha zetu 254 nyingi zilitafsiriwa kwa mashine. Mapitio ya wazungumzaji wa asili ni ya thamani sana!

1. Chagua faili yako ya lugha
2. Kagua tafsiri
3. Rekebisha tafsiri yoyote isiyo sahihi au isiyo ya kawaida
4. Tuma PR

### Mifumo ya Lugha

Tunatumia mifumo ya kawaida ya ISO 639-1 (kwa mfano, `ko`, `en`, `ja`, `ar`, `hi`) na tofauti za kikanda inapohitajika (kwa mfano, `zh-CN`, `pt-BR`).

---

## 🛠 Mipangilio ya Maendeleo

### Masharti ya Awali

- Node.js 18+
- npm 9+
- Git

### Mipangilio
```bash
```
### Kujenga
```bash
```
> Kumbuka: Heap ya GB 2 ya msingi haitoshi kutokana na faili za lugha 254 + kifurushi cha mhariri wa Monaco (~38MB renderer).

### Muundo wa Mradi
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

## 🙏 Shukrani

Kila mchango unafanya WIA SOOM kuwa bora kwa waendelezaji duniani kote.

Iwe unarekebisha makosa ya tahajia, unatafsiri mfuatano, unajenga plugin, au unongeza kipengele kikubwa — **wewe ni sehemu ya hadithi hii.**

---

<p align="center"><em>Imepangwa kwa ❤️ na SmileStory Inc. na wachangiaji kutoka duniani kote.</em></p>