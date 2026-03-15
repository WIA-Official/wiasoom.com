<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM میں شراکت داری</h1>
<p align="center"><strong>ہم آپ کی شراکت داری کا خیرمقدم کرتے ہیں!</strong></p>
<p align="center">چاہے یہ ایک بگ فکس ہو، نئی خصوصیت، پلگ ان، یا ترجمہ — ہر شراکت اہم ہے۔</p>

---

## مواد کی فہرست

- [Conduct کا ضابطہ](#code-of-conduct)
- [بگ کی رپورٹ کیسے کریں](#-how-to-report-bugs)
- [خصوصیات کی تجویز کیسے کریں](#-how-to-suggest-features)
- [پلگ ان کیسے جمع کریں](#-how-to-submit-a-plugin)
- [پول درخواست کیسے جمع کریں](#-how-to-submit-a-pull-request)
- [ترجمہ کی شراکتیں (254 زبانیں)](#-translation-contributions-254-languages)
- [ترقی کی ترتیب](#-development-setup)

---

## Conduct کا ضابطہ

ہم ہر ایک کے لیے ایک خوش آئند اور شمولیتی تجربہ فراہم کرنے کے لیے پرعزم ہیں۔

- **احترام کریں۔** ہر ایک کے ساتھ عزت کے ساتھ پیش آئیں۔
- **تعمیراتی بنیں۔** مددگار فیڈبیک فراہم کریں، تخریبی تنقید نہیں۔
- **شامل کریں۔** ہم 254 زبانوں کی حمایت کرتے ہیں اور زمین کے ہر ملک سے شراکت داروں کا خیرمقدم کرتے ہیں۔
- **ہراساں نہ کریں۔** کسی بھی قسم کی تفریق کے لیے صفر برداشت۔

---

## 🐛 بگ کی رپورٹ کیسے کریں

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) پر جائیں
2. **"نئی مسئلہ"** پر کلک کریں
3. **"بگ رپورٹ"** ٹیمپلیٹ منتخب کریں
4. شامل کریں:
   - WIA SOOM ورژن (Settings → About)
   - OS اور ورژن (Windows/macOS/Linux)
   - دوبارہ پیدا کرنے کے مراحل
   - متوقع بمقابلہ حقیقی رویہ
   - اسکرین شاٹس یا ٹرمینل آؤٹ پٹ اگر ممکن ہو

---

## 💡 خصوصیات کی تجویز کیسے کریں

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) پر جائیں
2. **"نئی مسئلہ"** پر کلک کریں
3. **"خصوصیت کی درخواست"** ٹیمپلیٹ منتخب کریں
4. بیان کریں:
   - آپ کس مسئلے کو حل کر رہے ہیں
   - آپ اسے کیسے کام کرتے ہوئے تصور کرتے ہیں
   - کوئی متبادل جو آپ نے سوچا ہے

---

## 🔌 پلگ ان کیسے جمع کریں

WIA SOOM کا ایک طاقتور پلگ ان نظام ہے — آپ 5 منٹ میں اپنا پلگ ان بنا سکتے ہیں۔

### فوری آغاز
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### مکمل رہنما

**[پلگ ان ڈویلپر گائیڈ](docs/PLUGIN_DEVELOPER_GUIDE.md)** پڑھیں:
- مکمل API حوالہ
- کام کرنے کی مثالیں
- مرحلہ وار سبق
- بہترین طریقے اور حفاظتی قواعد

### اپنا پلگ ان جمع کریں

1. [Plugin Store](https://wiasoom.com) کو فورک کریں
2. اپنا پلگ ان `plugins/{your-plugin-name}/` میں شامل کریں
3. ایک پول درخواست جمع کریں
4. جائزے کے بعد، آپ کا پلگ ان تمام صارفین کے لیے پلگ ان اسٹور میں ظاہر ہوگا!

---

## 🔀 پول درخواست کیسے جمع کر��ں

### مرکزی ایپ کے لیے (wia-soom)

1. ریپوزٹری کو فورک کریں
2. ایک فیچر برانچ بنائیں: `git checkout -b feat/my-feature`
3. اپنی تبدیلیاں کریں
4. مقامی طور پر ٹیسٹ کریں:
   ```bash
   ```
5. واضح پیغام کے ساتھ کمٹ کریں:
   ```
   feat: سیٹنگز میں ڈارک موڈ ٹوگل شامل کریں
   ```
6. `main` کے خلاف ایک PR کھولیں

### کمٹ پیغام کا طریقہ کار

| Prefix | استعمال کے لیے |
|--------|----------------|
| `feat:` | نئی خصوصیت |
| `fix:` | بگ فکس |
| `docs:` | صرف دستاویزات |
| `refactor:` | کوڈ کی دوبارہ ساخت (کوئی رویہ کی تبدیلی نہیں) |
| `i18n:` | ترجمہ کی تازہ کاری |
| `plugin:` | پلگ ان سے متعلق تبدیلیاں |

### PR چیک لسٹ

- [ ] کوڈ بغیر کسی غلطی کے چلتا ہے
- [ ] کوئی ہارڈ کوڈڈ سٹرنگز نہیں (i18n کیز استعمال کریں)
- [ ] پروڈکشن کوڈ میں کوئی `console.log` نہیں چھوڑا گیا
- [ ] موجودہ ٹیسٹ اب بھی پاس ہیں

---

## 🌐 ترجمہ ��ی شراکتیں (254 زبانیں)

WIA SOOM **254 زبانوں** کی حمایت کرتا ہے — امہاری سے زولو تک، بشمول بریل اور RTL زبانیں۔

### ترجمہ کیسے کام کرتا ہے

- بنیادی زبان کی فائل: `src/renderer/src/i18n/en.json`
- تمام 254 زبان کی فائلیں ایک ہی ڈائریکٹری میں ہیں
- ترجمہ `scripts/translate-patch.js` (GPT-4o-mini API) کے ذریعے کیا جاتا ہے

### ترجمے میں شراکت کیسے کریں

#### آپشن 1: ایک خاص ترجمہ درست کریں

1. زبان کی فائل تلاش کریں: `src/renderer/src/i18n/{lang-code}.json`
2. غلط ترجمہ درست کریں
3. تبدیلی کے ساتھ ایک PR جمع کریں

#### آپشن 2: غائب کیز شامل کریں
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### آپشن 3: مشین کے ترجموں کا جائزہ لیں

ہماری 254 زبانوں میں سے بہت سی مشین کے ذریعے ترجمہ کی گئی تھیں۔ مقامی بولنے والوں کے جائزے انتہائی قیمتی ہیں!

1. اپنی زبان کی فائل منتخب کریں
2. ترجموں کا جائزہ لیں
3. کسی بھی عجیب یا غلط ترجمے کو درست کریں
4. ایک PR جمع کریں

### زبان کے کوڈز

ہم معیاری ISO 639-1 کوڈز (جیسے `ko`, `en`, `ja`, `ar`, `hi`) استعمال کرتے ہیں جہاں ضرورت ہو علاقائی مختلفات (جیسے `zh-CN`, `pt-BR`) کے ساتھ۔

---

## 🛠 ترقی کی ترتیب

### ضروریات

- Node.js 18+
- npm 9+
- Git

### ترتیب
```bash
```
### تعمیر
```bash
```
> نوٹ: 2GB کا ڈیفالٹ ہیپ 254 زبان کی فائلوں + Monaco ایڈیٹر بنڈل (~38MB رینڈرر) کی وجہ سے ناکافی ہے۔

### پروجیکٹ کا ڈھانچہ
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

## 🙏 Shukriya

Har ek hissa WIA SOOM ko duniya bhar ke developers ke liye behtar banata hai.

Chahe aap ek typo theek karein, ek string translate karein, ek plugin banayein, ya ek bada feature add karein — **aap is kahani ka hissa hain.**

---

<p align="center"><em>❤️ se banaya gaya hai SmileStory Inc. aur duniya bhar ke contributors ke zariye.</em></p>