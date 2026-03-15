<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM ውስጥ እንዴት እንደሚሰጥ</h1>
<p align="center"><strong>እባኮትን የምንቀበል እንደሆነ እንደምንቀበል!</strong></p>
<p align="center">እንደ ተለመደ በማይታወቅ ችግኝ እንደሆነ ወይም አዲስ ባህሪ ወይም ፕላግን ወይም ትርጉም — የሚሰጥ ሁሉም አስፈላጊ ነው።</p>

---

## የይዘት ዝርዝር

- [የአንደኛ ዙርያ](#code-of-conduct)
- [እንዴት ችግኝ እንደሚያስታውስ](#-how-to-report-bugs)
- [እንዴት ባህሪዎችን እንደሚመክር](#-how-to-suggest-features)
- [እንዴት ፕላግን እንደሚሰጥ](#-how-to-submit-a-plugin)
- [እንዴት ፖር ጥያቄ እንደሚሰጥ](#-how-to-submit-a-pull-request)
- [ትርጉም እንደሚሰጥ (254 ቋንቋዎች)](#-translation-contributions-254-languages)
- [የስራ አቀራረብ](#-development-setup)

---

## የአንደኛ ዙርያ

እኛ ለሁሉም የተስፋ ይደርስ የሚያደርግ እና የሚቀበል ተሞክሮ እንደምንሰጥ ተግባር ነን።

- **እንደ እንደኛ ይቀበሉ።** ሁሉንም በክብር ይቀበሉ።
- **እንደ ተግባር ይሁን።** የሚረዳ እንደ አስተያየት ይስጡ፣ አስተያየት አይደለም።
- **እንደ ተሞክሮ ይሁን።** እኛ 254 ቋንቋዎችን እና ከዓለም ሁሉ የሚመጡ እንደ ተሞክሮ እንቀበለዋለን።
- **የማይደርስ ይሁን።** ከማንኛውም ዓይነት የማይደርስ ይታወቃል።

---

## 🐛 እንዴት ችግኝ እንደሚያስታውስ

1. ወደ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) ይሂዱ
2. **"አዲስ ጉዳይ"** ይጫኑ
3. **"ችግኝ ዘገባ"** አቀማመጥ ይምረጡ
4. ይካተቱ:
   - WIA SOOM እትም (Settings → About)
   - ኦኤስ እና እትም (Windows/macOS/Linux)
   - ወደ መደብ የሚወስድ እንደሚያስታውስ
   - የተስፋ ወይም የተከታታይ እንደሚያስታውስ
   - የምስል ወይም የታሪክ ውጤት እንደሚቻል

---

## 💡 እንዴት ባህሪዎችን እንደሚመክር

1. ወደ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) ይሂዱ
2. **"አዲስ ጉዳይ"** ይጫኑ
3. **"ባህሪ ጥያቄ"** አቀማመጥ ይምረጡ
4. ይገልጹ:
   - የምንቀበል ችግኝ ምንድነው
   - እንዴት ይሰራ ይሆናል
   - የተመለከቱ ምንም እንደሆነ

---

## 🔌 እንዴት ፕላግን እንደሚሰጥ

WIA SOOM የበለጠ ፕላግን ስርዓት አለው — በ5 ደቂቀ ደቂቀ ወቅት የራስዎን ፕላግን ማምረት ይችላሉ።

### አስቀድሞ መጀመሪያ
§§§CHUNK_SEPARATOR§§§
### ሙሉ መመሪያ

**[ፕላግን አንባሳ መመሪያ](docs/PLUGIN_DEVELOPER_GUIDE.md)** ይይዙ ለ:
- ሙሉ የAPI እንደሚያስታውስ
- የሚሰሩ ምሳሌዎች
- ወደ አንደኛ ደረጃ መምሪያዎች
- የተመለከቱ ምርጥ እና የደህንነት ደንቦች

### ፕላግንዎን ይሰጡ

1. [Plugin Store](https://wiasoom.com) ይፎርክ ይደርሱ
2. ፕላግንዎን ወደ `plugins/{your-plugin-name}/` ይጨምሩ
3. ፖር ጥያቄ ይሰጡ
4. ከእንደ አስተያየት በ��ላ፣ ፕላግን��� ለሁሉም ተጠቃሚዎች በፕላግን ሱቅ ይታይ!

---

## 🔀 እንዴት ፖር ጥያቄ እንደሚሰጥ

### ለዋና መተግበሪያ (wia-soom)

1. የማዕከላዊ ዝርዝር ይፎርክ
2. የባህሪ ቅርንጫፍ ይፍጠሩ: `git checkout -b feat/my-feature`
3. ለዚህ ይለዋወጡ
4. በአንደኛ ይሞክሩ:
   ```bash
   ```
5. ከግምገማ ጋር ይገናኙ:
   ```
   feat: ወደ ቅንጫፍ ወይም ወደ ዝርዝር ይጨምሩ
   ```
6. ይገናኙ እና ወደ `main` ይክፈቱ

### የኮሚት መልእክት የተወሰነ ዘዴ

| ቅድሚያ | ለምን ይጠቀማል |
|--------|---------|
| `feat:` | አዲስ ባህሪ |
| `fix:` | ችግኝ አስተካክል |
| `docs:` | ለማስታወቂያ ብቻ |
| `refactor:` | የኮድ ዝርዝር (የባህሪ ለውጥ የለም) |
| `i18n:` | የትርጉም እንደሚያስታውስ |
| `plugin:` | የፕላግን ጋር የተያያዘ ለውጥ |

### የPR ዝርዝር

- [ ] ኮድ ከማንኛውም እንደሚሰራ ይሄዳል
- [ ] የተወሰነ አንደኛ የለም (i18n ቁልፍ ይጠቀሙ)
- [ ] በምርት ኮድ ውስጥ `console.log` የ���ም
- [ ] የታወቀ ፈተናዎች እንደሚሰሩ ይቀጥሉ

---

## 🌐 ትርጉም እንደሚሰጥ (254 ቋንቋዎች)

WIA SOOM የሚያገኙ **254 ቋንቋዎች** አለው — ከአማርኛ እስከ ዙሉ, በብራይል እና RTL ቋንቋዎች ጨምሮ።

### ትርጉም እንዴት ይሰራል

- የመሠረታዊ ቋንቋ ፋይል: `src/renderer/src/i18n/en.json`
- ሁሉም 254 ቋንቋ ፋይሎች በአንደኛ ዝርዝር ውስጥ አሉ
- ትርጉም በ `scripts/translate-patch.js` (GPT-4o-mini API) ይከናወናል

### ትርጉም እንዴት ይሰጡ

#### አማራጭ 1: የተወሰነ ትርጉም እንደሚሰጥ

1. የቋንቋ ፋይል ይፈልጉ: `src/renderer/src/i18n/{lang-code}.json`
2. የተሳሳተ ትርጉም ይከተቱ
3. ወደ ዚህ ወይም ወደ ዚህ ይሰጡ

#### አማራጭ 2: የገና የለውጥ ቁልፍ ይጨምሩ
§§§CHUNK_SEPARATOR§§§
#### አማራጭ 3: የማሽን ትርጉሞችን ይቅርብ

ከ254 ቋንቋዎች ብዙው በማሽን ትርጉም ተርጉሞች ናቸው። የተወላጅ ተንበርክ እጅግ ውድ ነው!

1. የቋንቋ ፋይል ይምረጡ
2. ትርጉሞችን ይገምግሙ
3. የተሳሳ�� ወይም የተወሰነ ትርጉም ይከተቱ
4. ወደ ዚህ ወይም ወደ ዚህ ይሰጡ

### የቋንቋ ኮዶች

እኛ የአማርኛ እንደ ወንጌል የISO 639-1 ኮዶችን (ለምሳሌ, `ko`, `en`, `ja`, `ar`, `hi`) እና የአካባቢ ተለዋዋጮች ይጠቀሙ (ለምሳሌ, `zh-CN`, `pt-BR`)።

---

## 🛠 የስራ አቀራረብ

### የቀደም ዝርዝር

- Node.js 18+
- npm 9+
- Git

### አቀራረብ
§§§CHUNK_SEPARATOR§§§
### ማስተካከል
§§§CHUNK_SEPARATOR§§§
> ማስታወሻ: የአስተካከል 2GB የሚሆን የማይበቃ ነው ምክንያቱም 254 ቋንቋ ፋይሎች + Monaco ኤዲተር በ~38MB ይሆናል።

### የፕሮጀክት አወዳድር
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 እናመሰግናለን

እያንዳንዱ መርምር ወይም እንደ ምርጥ ይሁን ወይም እንደ ተለዋዋጭ ይሁን ወይም እንደ አንድ ዋነኛ ባህሪ ይሁን — **አንተ በዚህ ታሪክ ውስጥ ነህ።**

---

<p align="center"><em>በ❤️ የተገነባ ከSmileStory Inc. እና ከዓለም ላይ የሚሰጡ አባላት።</em></p>
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
