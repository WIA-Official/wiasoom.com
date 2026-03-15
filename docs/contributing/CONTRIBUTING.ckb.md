<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">بەشداریکردن بۆ WIA SOOM</h1>
<p align="center"><strong>ئێمە پێویستە بە بەشداریکردنی تۆ!</strong></p>
<p align="center">ئەگەر ئەوە بێت کێشەیەک، تایبەتمەندی نوێ، پلەگین یان وەرگێڕانی — هەموو بەشداریکردنەکان گرنگن.</p>

---

## فهرستی ناوەکان

- [یاسای ڕووداو](#code-of-conduct)
- [چۆنیەتی ڕاپۆرتکردنی کێشەکان](#-how-to-report-bugs)
- [چۆنیەتی پێشنیازی تایبەتمەندیەکان](#-how-to-suggest-features)
- [چۆنیەتی ناردنی پلەگین](#-how-to-submit-a-plugin)
- [چۆنیەتی ناردنی داواکاری پەیوەندیدانی](#-how-to-submit-a-pull-request)
- [بەشداریکردنەکانی وەرگێڕانی (254 زمان)](#-translation-contributions-254-languages)
- [دامەزراندنی پەیوەندیدان](#-development-setup)

---

## یاسای ڕووداو

ئێمە پەیوەندیدانی خوشەویستی و پەیوەندیدانی گەورە بۆ هەموو کەسەکان پەیوەندیدانی دەکەین.

- **بە ڕوونی بەرز بەرەوپێشە.** هەموو کەسێک بە شێوەیەکی پەروەردەیی پەیوەندیدانی بکە.
- **بە شێوەیەکی پەیوەندیدانی بەرز.** فیدبەکی یارمەتیدەر پێشکەش بکە، نە فیدبەکی ناردنەوە.
- **بە شێوەیەکی پەیوەندیدانی گەورە.** ئێمە 254 زمان پشتیوانی دەکەین و بەشداریکەرەکان لە هەموو وڵاتەکان لە زەویەوە پەیوەندیدانی دەکەین.
- **هیچ کەسێک ناتوانێت کەسێکی تر بەرز بکات.** بەرزکردنەوەی بەرز بەرزەکان بەرزە.

---

## 🐛 چۆنیەتی ڕاپۆرتکردنی کێشەکان

1. بەرەو [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) بڕۆ
2. کلیک بکە لەسەر **"New Issue"**
3. شێوەی **"Bug Report"** هەڵبژێرە
4. پێویستە بگۆڕیت:
   - بەرزەی WIA SOOM (Settings → About)
   - سیستەمی کارکردن و بەرزە (Windows/macOS/Linux)
   - پێوەندیدانی بەرز
   - چۆنیەتی پێشنیازی بەرز و چۆنیەتی بەرز
   - وێنە یان ئەنجامی ترمینال ئەگەر پێویستە

---

## 💡 چۆنیەتی پێشنیازی تایبەتمەندیەکان

1. بەرەو [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) بڕۆ
2. کلیک بکە لەسەر **"New Issue"**
3. شێوەی **"Feature Request"** هەڵبژێرە
4. بڵێ:
   - کێشەیەکی تۆ چەندە
   - چۆن دەتەوێت ئەوە کار بکات
   - هەر شتێکی تر کە تۆ پێشنیاز کردووە

---

## 🔌 چۆنیەتی ناردنی پلەگین

WIA SOOM سیستەمی پلەگینی پەیوەندیدانی بەرزە — دەتوانیت پلەگینی خۆت لە 5 خولەکەدا دروست بکەیت.

### پەیوەندیدانی زود
§§§CHUNK_SEPARATOR§§§
### ڕێنمایی تەواو

بەخێرە بەرەو **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** بۆ:
- پەیوەندیدانی API تەواو
- نموونەی کارکردن
- فەرمی پەیوەندیدانی بەرز
- باشترین شێوەکان و یاساکانی پاراستن

### پلەگینی خۆت ناردن

1. Fork [Plugin Store](https://wiasoom.com)
2. پلەگینی خۆت بەرەو `plugins/{your-plugin-name}/` زیاد بکە
3. داواکاری پەیوەندیدانی ناردن
4. پاش پشکنینی، پلەگینی تۆ لە فەرمی پلەگینەکان بەرز دەبێت بۆ هەموو بەکارهێنەرەکان!

---

## 🔀 چۆنیەتی ناردنی داواکاری پەیوەندیدانی

### بۆ ئەو بەرنامەی سەرەکی (wia-soom)

1. Fork بەرزە
2. شاخەی تایبەتمەندی دروست بکە: `git checkout -b feat/my-feature`
3. گۆڕانکاریەکانت بکە
4. لە ناوەڕاستی بەرزە:
   ```bash
   ```
5. بە پەیامێکی ڕوون پەیوەندیدانی بکە:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push بکە و PR بەرەو `main` بکە

### یاسای پەیوەندیدانی

| پێشەوانە | بۆ چەندە |
|--------|---------|
| `feat:` | تایبەتمەندی نوێ |
| `fix:` | کێشەی ڕووداو |
| `docs:` | تۆمارکردن تەنها |
| `refactor:` | ڕووکاری کۆد (بێ گۆڕینی بەرز) |
| `i18n:` | نوێکردنەوەی وەرگێڕانی |
| `plugin:` | گۆڕینەوەی پەیوەندیدانی پلەگین |

### لیستی پەیوەندیدانی PR

- [ ] کۆد بێ هەڵەیە
- [ ] هیچ شتێکی هەڵبژێردراو نییە (بەکاربەرەکان i18n بکاربە)
- [ ] هیچ `console.log` لە کۆدی پەیوەندیدانی بەرز نییە
- [ ] تاقیکردنەوەی پێشتر هەموو کات��کان پەیوەندیدانی دەکات

---

## 🌐 بەشداریکردنەکانی وەرگێڕانی (254 زمان)

WIA SOOM **254 زمان** پشتیوانی دەکات — لە ئامەریکە بەرەو زولۆ، پشتیوانی لە بڕەی و RTL زمانەکان.

### چۆنیەتی کارکردنی وەرگێڕانی

- فایلی زمانە بنچینەیی: `src/renderer/src/i18n/en.json`
- هەموو 254 فایلی زمان لە هەمان پەیوەندیدانە
- وەرگێڕانی لە ڕووی `scripts/translate-patch.js` (GPT-4o-mini API) پەیوەندیدانی دەکرێت

### چۆنیەتی بەشداریکردنی وەرگێڕانی

#### هەڵبژاردنی 1: ڕووکاری تایبەتی وەرگێڕان

1. فایلی زمان بگەڕێ: `src/renderer/src/i18n/{lang-code}.json`
2. وەرگێڕانی هەڵەیەک ڕووکاری بکە
3. PR بەرەو گۆڕانکاری ناردن

#### هەڵبژاردنی 2: کلیلەکان نادروست بکە
§§§CHUNK_SEPARATOR§§§
#### هەڵبژاردنی 3: وەرگێڕانی مێشین بپشکنە

زۆرترین زمانەکانمان لە 254 زمانەکان بە وەرگێڕانی مێشین پەیوەندیدانی کراوە. پشکنینی نەتەوەیی زۆر گرنگ��!

1. فایلی زمانەکەت هەڵبژێرە
2. وەرگێڕانی پشکنە
3. هەر وەرگێڕانی هەڵە یان نادروست ڕووکاری بکە
4. PR بەرەو ناردن

### کۆدەکانی زمان

ئێمە کۆدەکانی ISO 639-1 یاسای پەیوەندیدانی بەرزە (بە شێوەی `ko`, `en`, `ja`, `ar`, `hi`) بە شێوەیەکی هەریمەوە کە پێویستە (بە شێوەی `zh-CN`, `pt-BR`).

---

## 🛠 دامەزراندنی پەیوەندیدان

### پێویستەکان

- Node.js 18+
- npm 9+
- Git

### دامەزراندن
§§§CHUNK_SEPARATOR§§§
### دروستکردن
§§§CHUNK_SEPARATOR§§§
> تێبینی: 2GB heap یەکی بنچینەیی کە بەرزەکان 254 زمانە + بڕەی Monaco (~38MB renderer) بەرزە.

### شێوەی پڕۆژە
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 سوپاس

هەر پێشکەشێک WIA SOOM بەرزتر دەکات بۆ پەرەپێدەرانی لە هەموو جیهانەوە.

ئەگەر تۆ هەڵەیەکی نووسینی پەیوەندیدا ڕاست بکەیت، شتێک بترجمەیت، پلگینیەک دروست بکەیت، یان تایبەتمەندیەکی گرنگ زیاد بکەیت — **تۆ بەشێک لە ئەم چیرۆکەی.**

---

<p align="center"><em>بە شێوەیەکی ❤️ دروستکراوە لە لایەن SmileStory Inc. و پێشکەشکەران لە هەموو جیهانەوە.</em></p>
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```

#### Option 3: Review machine translations

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup

```bash
```

### Build

```bash
```

> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

### Project Structure

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

## 🙏 Thank You

Every contribution makes WIA SOOM better for developers around the world.

Whether you fix a typo, translate a string, build a plugin, or add a major feature — **you are part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>
