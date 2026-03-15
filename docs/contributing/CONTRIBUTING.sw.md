<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kuchangia katika WIA SOOM</h1>
<p align="center"><strong>Tunapenda michango yako!</strong></p>
<p align="center">Iwe ni kurekebisha hitilafu, kipengele kipya, plugin, au tafsiri — kila mchango ni muhimu.</p>

---

## Orodha ya Yaliyomo

- [Kanuni za Maadili](#code-of-conduct)
- [Jinsi ya Kuripoti Hitilafu](#-how-to-report-bugs)
- [Jinsi ya Kupendekeza Kipengele](#-how-to-suggest-features)
- [Jinsi ya Kuwasilisha Plugin](#-how-to-submit-a-plugin)
- [Jinsi ya Kuwasilisha Ombi la Mabadiliko](#-how-to-submit-a-pull-request)
- [Michango ya Tafsiri (Lugha 254)](#-translation-contributions-254-languages)
- [Mikakati ya Maendeleo](#-development-setup)

---

## Kanuni za Maadili

Tumejizatiti kutoa uzoefu wa kukaribisha na wa kujumuisha kwa kila mtu.

- **Heshimu.** Treat everyone with dignity.
- **Kuwa na ufanisi.** Toa maoni ya kusaidia, si ukosoaji wa kubomoa.
- **Kuwa jumuishi.** Tunasaidia lugha 254 na tunakaribisha wachangiaji kutoka kila nchi duniani.
- **Hakuna unyanyasaji.** Hakuna uvumilivu kwa ubaguzi wa aina yoyote.

---

## 🐛 Jinsi ya Kuripoti Hitilafu

1. Tembelea [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Bonyeza **"New Issue"**
3. Chagua kiolezo cha **"Bug Report"**
4. Jumuisha:
   - Toleo la WIA SOOM (Mipangilio → Kuhusu)
   - OS na toleo (Windows/macOS/Linux)
   - Hatua za kuzaa
   - Tabia inayotarajiwa dhidi ya ile halisi
   - Picha au matokeo ya terminal ikiwa inawezekana

---

## 💡 Jinsi ya Kupendekeza Kipengele

1. Tembelea [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Bonyeza **"New Issue"**
3. Chagua kiolezo cha **"Feature Request"**
4. Eleza:
   - Ni tatizo gani unalotatua
   - Unafikiri itafanya kazi vipi
   - Chaguzi zozote ulizozitafakari

---

## 🔌 Jinsi ya Kuwasilisha Plugin

WIA SOOM ina mfumo mzuri wa plugin — unaweza kujenga plugin yako mwenyewe ndani ya dakika 5.

### Mwanzo wa Haraka
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Mwongozo Kamili

Soma **[Mwongozo wa Wataalamu wa Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** kwa:
- Marejeo kamili ya API
- Mifano inayofanya kazi
- Mafunzo ya hatua kwa hatua
- Mbinu bora na sheria za usalama

### Wasilisha Plugin Yako

1. Fork [Plugin Store](https://wiasoom.com)
2. Ongeza plugin yako kwenye `plugins/{your-plugin-name}/`
3. Wasilisha Ombi la Mabadiliko
4. Baada ya uhakiki, plugin yako itaonekana kwenye Duka la Plugin kwa watumiaji wote!

---

## 🔀 Jinsi ya Kuwasilisha Ombi la Mabadiliko

### Kwa programu kuu (wia-soom)

1. Fork hazina
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
| `fix:` | Kurekebisha hitilafu |
| `docs:` | Hati pekee |
| `refactor:` | Upangaji wa msimbo (hakuna mabadiliko ya tabia) |
| `i18n:` | Sasisho za tafsiri |
| `plugin:` | Mabadiliko yanayohusiana na plugin |

### Orodha ya Kuthibitisha PR

- [ ] Msimbo unafanya kazi bila makosa
- [ ] Hakuna nyuzi zilizowekwa (tumia funguo za i18n)
- [ ] Hakuna `console.log` iliyobaki kwenye msimbo wa uzalishaji
- [ ] Majaribio yaliyopo bado yanapita

---

## 🌐 Michango ya Tafsiri (Lugha 254)

WIA SOOM inasaidia **lugha 254** — kuanzia Kiamhariki hadi Zulu, ikiwa ni pamoja na Braille na lugha za RTL.

### Jinsi Tafsiri Inavyofanya Kazi

- Faili ya msingi ya lugha: `src/renderer/src/i18n/en.json`
- Faili zote za lugha 254 ziko kwenye saraka moja
- Tafsiri inafanywa kupitia `scripts/translate-patch.js` (GPT-4o-mini API)

### Jinsi ya Kuchangia Tafsiri

#### Chaguo la 1: Rekebisha tafsiri maalum

1. Pata faili ya lugha: `src/renderer/src/i18n/{lang-code}.json`
2. Rekebisha tafsiri isiyo sahihi
3. Wasilisha PR na mabadiliko

#### Chaguo la 2: Ongeza funguo zinazokosekana
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Chaguo la 3: Kagua tafsiri za mashine

Lugha zetu 254 nyingi zilitafsiriwa kwa mashine. Mapitio ya wazungumzaji asilia ni ya thamani sana!

1. Chagua faili lako la lugha
2. Kagua tafsiri
3. Rekebisha tafsiri zozote zisizo sahihi au zisizo za kawaida
4. Wasilisha PR

### Mifumo ya Lugha

Tunatumia mifumo ya kawaida ya ISO 639-1 (mfano, `ko`, `en`, `ja`, `ar`, `hi`) na tofauti za kikanda inapohitajika (mfano, `zh-CN`, `pt-BR`).

---

## 🛠 Mikakati ya Maendeleo

### Vigezo vya Msingi

- Node.js 18+
- npm 9+
- Git

### Mipangilio
```bash
```
### Kujenga
```bash
```
> Kumbuka: Kumbukumbu ya GB 2 ya msingi haitoshi kutokana na faili 254 za lugha + kifurushi cha mhariri wa Monaco (~38MB renderer).

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

## 🙏 Asante

Kila mchango unafanya WIA SOOM kuwa bora zaidi kwa wabunifu kote ulimwenguni.

Iwe unarekebisha makosa ya tahajia, unatafsiri mfuatano, unajenga plugin, au unongeza kipengele kikubwa — **wewe ni sehemu ya hadithi hii.**

---

<p align="center"><em>Imejengwa kwa ❤️ na SmileStory Inc. na wachangiaji kutoka kote ulimwenguni.</em></p>