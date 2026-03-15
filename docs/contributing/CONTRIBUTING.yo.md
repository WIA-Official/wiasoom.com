<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kópa sí WIA SOOM</h1>
<p align="center"><strong>À ń fẹ́ kí o kópa!</strong></p>
<p align="center">Bóyá ó jẹ́ àtúnṣe kokoro, ẹya tuntun, plugin, tàbí ìtúmọ̀ — gbogbo kópa jẹ́ pataki.</p>

---

## Àtòjọ Akọ́kọ

- [Ilana Iwa](#code-of-conduct)
- [Báwo ni láti Ròyìn Kokoro](#-how-to-report-bugs)
- [Báwo ni láti Ṣàpèjúwe Ẹya](#-how-to-suggest-features)
- [Báwo ni láti Fọwọ́sí Plugin](#-how-to-submit-a-plugin)
- [Báwo ni láti Fọwọ́sí ìbéèrè Pull](#-how-to-submit-a-pull-request)
- [Kópa Ìtúmọ̀ (254 Èdè)](#-translation-contributions-254-languages)
- [Ìtọ́sọ́nà Ìdàgbàsókè](#-development-setup)

---

## Ilana Iwa

A ti pinnu láti pese iriri tó ní ìbáṣepọ̀ àti ìkànsí fún gbogbo ènìyàn.

- **Jẹ́ kó ní ìbáṣepọ̀.** Tọju gbogbo ènìyàn pẹ̀lú ìyàsímímọ́.
- **Jẹ́ kó ní ìtẹ́lọ́run.** Pese ìfọ̀rọ̀wér���̀ tó wúlò, kì í ṣe àfihàn àìlera.
- **Jẹ́ kó ní ìkànsí.** A ń ṣe atilẹyin fún 254 èdè àti pé a ń gba àwọn tó kópa láti gbogbo orílẹ̀-èdè lórí ilẹ̀ ayé.
- **Kò sí ìkà.** A ní ìfarapa kankan fún ìyàtọ̀ kankan.

---

## 🐛 Báwo ni láti Ròyìn Kokoro

1. Lọ sí [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tẹ **"New Issue"**
3. Yan àtẹ̀jáde **"Bug Report"**
4. Kó:
   - Ẹ̀dá WIA SOOM (Settings → About)
   - OS àti ẹ̀dá (Windows/macOS/Linux)
   - Igbésẹ̀ láti tún ṣe
   - Iwa tó yẹ vs. iwa gangan
   - Àwòrán tàbí àtẹ̀jáde terminal bí ó bá ṣeé ṣe

---

## 💡 Báwo ni láti Ṣàpèjúwe Ẹya

1. Lọ sí [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tẹ **"New Issue"**
3. Yan àtẹ̀jáde **"Feature Request"**
4. Ṣàpèjúwe:
   - Kí ni iṣoro tó ń ṣe àtúnṣe
   - Báwo ni o ṣe rò pé yóò ṣiṣẹ́
   - Àwọn àṣàyàn míì tó o ti rò

---

## 🔌 Báwo ni láti Fọwọ́sí Plugin

WIA SOOM ní eto plugin tó lágbára — o lè kọ́ plugin tirẹ ní ìṣẹ́jú marun-un.

### Ìbẹ̀rẹ̀ Kánkán
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ìtọ́sọ́nà Pípé

Ka **[Ilana Olùdásílẹ̀ Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** fún:
- Ìtàn API pípé
- Àpẹẹrẹ tó n ṣiṣẹ́
- Ìtúmọ̀ ìgbésẹ̀-nipasẹ-ìgbésẹ̀
- Àwọn ìlànà tó dára jùlọ àti àwọn ofin ààbò

### Fọwọ́sí Plugin Rẹ

1. Fork [Plugin Store](https://wiasoom.com)
2. Fi plugin rẹ sí `plugins/{your-plugin-name}/`
3. Fọwọ́sí ìbéèrè Pull
4. Léyìn àyẹ̀wò, plugin rẹ yóò hàn nínú Plugin Store fún gbogbo àwọn olumulo!

---

## 🔀 Báwo ni láti Fọwọ́sí ìbéèrè Pull

### Fún ohun elo pàtàkì (wia-soom)

1. Fork ibi ipamọ́
2. Ṣẹda ẹka ẹya: `git checkout -b feat/my-feature`
3. Ṣe àwọn ayipada rẹ
4. Ṣàyẹ̀wò ní agbegbe:
   ```bash
   ```
5. Kó pẹ̀lú ìtàn tó mọ́:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push àti ṣí PR kan sí `main`

### Ilana Ìtàn Kó

| Prefix | Lo fún |
|--------|---------|
| `feat:` | Ẹya tuntun |
| `fix:` | Àtúnṣe kokoro |
| `docs:` | Ìwé ìtàn nìkan |
| `refactor:` | Atunṣe kóòdù (kò sí ayipada iwa) |
| `i18n:` | Àtúnṣe ìtúmọ̀ |
| `plugin:` | Àwọn ayipada tó ní í ṣe pẹ̀lú plugin |

### Àtòjọ PR

- [ ] Kóòdù ń ṣiṣẹ́ láìsí aṣiṣe
- [ ] Kò sí àwọn ọ��rọ̀ tó ti kọ́ (lo awọn bọtini i18n)
- [ ] Kò sí `console.log` tó kù nínú kóòdù iṣelọpọ
- [ ] Àwọn ìdánwò tó wà ṣáájú ṣi ń ṣiṣẹ́

---

## 🌐 Kópa Ìtúmọ̀ (254 Èdè)

WIA SOOM ń ṣe atilẹyin fún **254 èdè** — láti Amharic sí Zulu, pẹ̀lú Braille àti àwọn èdè RTL.

### Báwo ni Ìtúmọ̀ Ṣe N ṣiṣẹ́

- Fáìlì èdè ipilẹ: `src/renderer/src/i18n/en.json`
- Gbogbo 254 fáìlì èdè wà nínú àkóónú kan
- Ìtúmọ̀ ni a ṣe nípasẹ̀ `scripts/translate-patch.js` (GPT-4o-mini API)

### Báwo ni láti Kópa nínú Ìtúmọ̀

#### Àṣàyàn 1: Ṣàtúnṣe ìtúmọ̀ kan pato

1. Wa fáìlì èdè: `src/renderer/src/i18n/{lang-code}.json`
2. Ṣàtúnṣe ìtúmọ̀ tó jẹ́ aṣiṣe
3. Fọwọ́sí PR pẹ̀lú ayipada náà

#### Àṣàyàn 2: Fi bọtini tó kù sí
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Àṣàyàn 3: Ṣàyẹ̀wò ìtúmọ̀ ẹrọ

Ọ̀pọ̀ nínú 254 èdè wa ni a ti túmọ̀ sí ẹ̀rọ. Àwọn àyẹ̀wò olùkà èdè abinibi jẹ́ ohun tó wúlò gan-an!

1. Yan fáìlì èdè rẹ
2. Ṣàyẹ̀wò àwọn ìtúmọ̀
3. Ṣàtúnṣe àwọn ìtúmọ̀ tó jẹ́ àìlera tàbí aṣiṣe
4. Fọwọ́sí PR

### Àwọn Kóòdù Èdè

A ń lo àwọn kóòdù ISO 639-1 tó jẹ́ àmọ̀ràn (e.g., `ko`, `en`, `ja`, `ar`, `hi`) pẹ̀lú àwọn àtúnṣe agbègbè níbi tó yẹ (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Ìtọ́sọ́nà Ìdàgbàsókè

### Àwọn ìlànà tó yẹ

- Node.js 18+
- npm 9+
- Git

### Ìtọ́sọ́nà
```bash
```
### Kó
```bash
```
> Àkíyèsí: Ibi ipamọ́ 2GB aiyé kò tó nítorí àwọn fáìlì èdè 254 + àkóónú Monaco (~38MB renderer).

### Àtẹ̀jáde Ise
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

## 🙏 E seun

Gbogbo ilowosi kan mu WIA SOOM dara si fun awọn onimọ-ẹrọ ni gbogbo agbaye.

Boyá o ṣe atunṣe aṣiṣe kan, tumọ gbolohun kan, kọ plugin kan, tabi fi ẹya pataki kun — **o jẹ apakan ti itan yii.**

---

<p align="center"><em>Ti kọ pẹlu ❤️ nipasẹ SmileStory Inc. ati awọn olùkópa ni gbogbo agbaye.</em></p>