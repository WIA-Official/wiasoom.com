<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">د WIA SOOM سره مرسته کول</h1>
<p align="center"><strong>موږ ستاسو مرستې ته اړتیا لرو!</strong></p>
<p align="center">که دا د تېروتنې اصلاح، نوې ځانګړتیا، پلگ ان، یا ژباړه وي — هره مرسته مهمه ده.</p>

---

## د محتوياتو جدول

- [د چلند کوډ](#code-of-conduct)
- [څنګه تېروتنې راپور کړئ](#-how-to-report-bugs)
- [څنګه ځانګړتیاوې وړاندیز کړئ](#-how-to-suggest-features)
- [څنګه پلگ ان وړاندې کړئ](#-how-to-submit-a-plugin)
- [څنګه د Pull Request وړاندې کړئ](#-how-to-submit-a-pull-request)
- [د ژباړې مرستې (254 ژبې)](#-translation-contributions-254-languages)
- [د پرمختګ ترتیب](#-development-setup)

---

## د چلند کوډ

موږ د هرچا لپاره د ښه راغلاست او شاملیدو تجربه چمتو کولو ته ژمن یو.

- **احترام وکړئ.** له هرچا سره د وقار سره چلند وکړئ.
- **بناء کړئ.** مفید فیډبیک وړاندې کړئ، نه تخریبي نیوکه.
- **شامل اوسئ.** موږ 254 ژبې ملاتړ کوو او د نړۍ له هر هیواد څخه مرستندویه کسان خوش آمدید وایو.
- **هیڅ ځورونه نشته.** د هر ډول تبعیض لپاره صفر زغم.

---

## 🐛 څنګه تېروتنې راپور کړئ

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) ته لاړ شئ
2. **"نوې ستونزه"** باندې کلیک وکړئ
3. **"تېروتنې راپور"** ټیمپلیټ انتخاب کړئ
4. شامل کړئ:
   - د WIA SOOM نسخه (تنظیمات → په اړه)
   - OS او نسخه (Windows/macOS/Linux)
   - د بیا تولید مرحلې
   - تمه شوې او حقیقي چلند
   - که ممکن وي، سکرین شاټونه یا ترمینل آوټپټ

---

## 💡 څنګه ځانګړتیاوې وړاندیز کړئ

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) ته لاړ شئ
2. **"نوې ستونزه"** باندې کلیک وکړئ
3. **"ځانګړتیا غوښتنه"** ټیمپلیټ انتخاب کړئ
4. تشریح کړئ:
   - تاسو کومه ستونزه حل کوئ
   - تاسو څنګه تصور کوئ چې دا کار وکړي
   - کوم بدیلونه چې تاسو غور کړی

---

## 🔌 څنګه پلگ ان وړاندې کړئ

WIA SOOM یو ځواکمن پلگ ان سیسټم لري — تاسو کولی شئ په 5 دقیقو کې خپل پلگ ان جوړ کړئ.

### چټک پیل
§§§CHUNK_SEPARATOR§§§
### بشپړ لارښود

د **[پلگ ان پراختیا کوونکي لارښود](docs/PLUGIN_DEVELOPER_GUIDE.md)** ولولئ:
- بشپړ API حواله
- کاري مثالونه
- ګام په ګام ښوونې
- غوره کړنې او امنیتي قواعد

### خپل پلگ ان وړاندې کړئ

1. [Plugin Store](https://wiasoom.com) ته فورک کړئ
2. خپل پلگ ان `plugins/{your-plugin-name}/` ته اضافه کړئ
3. د Pull Request وړاندې کړئ
4. د بیاکتنې وروسته، ستاسو پلگ ان د ټولو کاروونکو لپاره په پلگ ان سټور کې څرګندیږي!

---

## 🔀 څنګه د Pull Request وړاندې کړئ

### د اصلي اپلیکیشن لپاره (wia-soom)

1. د ریپوزیتورۍ فورک کړئ
2. د ځانګړتیا څانګه جوړه کړئ: `git checkout -b feat/my-feature`
3. خپل بدلونونه وکړئ
4. محلي ازموینه وکړئ:
   ```bash
   ```
5. د واضح پیغام سره کمیت وکړئ:
   ```
   feat: د تنظیماتو لپاره د تیاره حالت بدلول اضافه کړئ
   ```
6. فشار ورکړئ او د `main` پر وړاندې PR پرانیزئ

### د کمیت پیغام کنوانسیون

| پریفکس | د څه لپاره کارول کیږي |
|--------|---------------------|
| `feat:` | نوې ځانګړتیا |
| `fix:` | د تېروتنې اصلاح |
| `docs:` | یوازې مستندات |
| `refactor:` | د کوډ جوړښت (هیڅ چلند بدلون نشته) |
| `i18n:` | د ژباړې تازه معلومات |
| `plugin:` | د پلگ ان اړوند بدلونونه |

### د PR چک لیست

- [ ] کوډ پرته له تېروتنو چلند کوي
- [ ] هیڅ سخت کوډ شوي تارونه نشته (i18n کلیدونه وکاروئ)
- [ ] په تولیدي کوډ کې هیڅ `console.log` پاتې نشته
- [ ] موجوده ازموینې لا هم تېرېږي

---

## 🌐 د ژباړې مرستې (254 ��بې)

WIA SOOM **254 ژبې** ملاتړ کوي — له امهاریک څخه تر زولو پورې، د بریل او RTL ژبو په ګډون.

### څنګه ژباړه کار کوي

- د بنسټیزې ژبې فایل: `src/renderer/src/i18n/en.json`
- ټول 254 ژبې فایلونه په یوه او همدغه فولډر کې دي
- ژباړه د `scripts/translate-patch.js` له لارې ترسره کیږي (GPT-4o-mini API)

### څنګه د ژباړو مرستې وکړئ

#### انتخاب 1: یوه ځانګړې ژباړه اصلاح کړئ

1. د ژبې فایل ومومئ: `src/renderer/src/i18n/{lang-code}.json`
2. ناسمې ژباړې اصلاح کړئ
3. د بدلون سره PR وړاندې کړئ

#### انتخاب 2: نشتون لرونکي کلیدونه اضافه کړئ
§§§CHUNK_SEPARATOR§§§
#### انتخاب 3: د ماشین ژباړې بیاکتنه وکړئ

زموږ د 254 ژبو څخه ډیری د ماشین له لارې ژباړل شوي. د اصلي ژبې ویونکو بیاکتنې خورا ارزښتناکه دي!

1. خپل ژبې فایل انتخاب کړئ
2. ژباړې بیاکتنه کړئ
3. هر نا آرام یا ناسمې ژباړې اصلاح کړئ
4. PR وړاندې کړئ

### د ژبې کوډونه

موږ د معیاري ISO 639-1 کوډونه کاروو (لکه `ko`, `en`, `ja`, `ar`, `hi`) چې اړتیا په صورت کې سیمه ییز بدلونونه لري (لکه `zh-CN`, `pt-BR`).

---

## 🛠 د پرمختګ ترتیب

### مخکې له دې چې پیل وکړئ

- Node.js 18+
- npm 9+
- Git

### ترتیب
§§§CHUNK_SEPARATOR§§§
### جوړول
§§§CHUNK_SEPARATOR§§§
> یادونه: د 2GB ډیفالټ هیپ د 254 ژبې فایلونو + د Monaco ایډیټر بنډل (~38MB رینډر) لپاره کافي نه دی.

### د پروژې جوړښت
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 مننه

هره مرسته WIA SOOM د نړۍ په کچه د پراختیا کونکو لپاره غوره کوي.

که تاسو یو ټایپو اصلاح کړئ، یوه رشته ژباړه کړئ، یو پلگ ان جوړ کړئ، یا یوه لویه ځانګړتیا اضافه کړئ — **تاسو د دې کیسې یوه برخه یاست.**

---

<p align="center"><em>د ❤️ سره جوړ شوی د SmileStory Inc. او نړیوالو همکارانو لخوا.</em></p>
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
