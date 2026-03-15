<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">المساهمة في WIA SOOM</h1>
<p align="center"><strong>نحن نحب مساهماتك!</strong></p>
<p align="center">سواء كانت إصلاح خطأ، ميزة جديدة، إضافة، أو ترجمة — كل مساهمة تهم.</p>

---

## جدول المحتويات

- [مدونة السلوك](#code-of-conduct)
- [كيفية الإبلاغ عن الأخطاء](#-how-to-report-bugs)
- [كيفية اقتراح ميزات](#-how-to-suggest-features)
- [كيفية تقديم إضافة](#-how-to-submit-a-plugin)
- [كيفية تقديم طلب سحب](#-how-to-submit-a-pull-request)
- [مساهمات الترجمة (254 لغة)](#-translation-contributions-254-languages)
- [إعداد التطوير](#-development-setup)

---

## مدونة السلوك

نحن ملتزمون بتوفير تجربة مرحبة وشاملة للجميع.

- **كن محترمًا.** عامل الجميع بكرامة.
- **كن بناءً.** قدم ملاحظات مفيدة، وليس انتقادات مدمرة.
- **كن شاملًا.** نحن ندعم 254 لغة ونرحب بالمساهمين من كل دولة على وجه الأرض.
- **لا للتحرش.** عدم التسامح مطلقًا مع التمييز من أي نوع.

---

## 🐛 كيفية الإبلاغ عن الأخطاء

1. انتقل إلى [مشاكل GitHub](https://github.com/WIA-Official/wiasoom.com/issues)
2. انقر على **"مشكلة جديدة"**
3. اختر قالب **"تقرير خطأ"**
4. أضف:
   - إصدار WIA SOOM (الإعدادات → حول)
   - نظام التشغيل والإصدار (Windows/macOS/Linux)
   - خطوات لإعادة الإنتاج
   - السلوك المتوقع مقابل السلوك الفعلي
   - لقطات شاشة أو مخرجات طرفية إذا أمكن

---

## 💡 كيفية اقتراح ميزات

1. انتقل إلى [مشاكل GitHub](https://github.com/WIA-Official/wiasoom.com/issues)
2. انقر على **"مشكلة جديدة"**
3. اختر قالب **"طلب ميزة"**
4. صف:
   - ما المشكلة التي تحلها
   - كيف تتخيل أن تعمل
   - أي بدائل قد考虑تها

---

## 🔌 كيفية تقديم إضافة

تتمتع WIA SOOM بنظام إضافات قوي — يمكنك بناء إضافتك الخاصة في 5 دقائق.

### البداية الس��يعة
§§§CHUNK_SEPARATOR§§§
### الدليل الكامل

اقرأ **[دليل مطور الإضافات](docs/PLUGIN_DEVELOPER_GUIDE.md)** لـ:
- مرجع API كامل
- أمثلة عملية
- دروس خطوة بخطوة
- أفضل الممارسات وقواعد الأمان

### تقديم إضافتك

1. قم بعمل فورك لـ [Plugin Store](https://wiasoom.com)
2. أضف إضافتك إلى `plugins/{your-plugin-name}/`
3. قدم طلب سحب
4. بعد المراجعة، ستظهر إضافتك في متجر الإضافات لجميع المستخدمين!

---

## 🔀 كيفية تقديم طلب سحب

### للتطبيق الرئيسي (wia-soom)

1. قم بعمل فورك للمستودع
2. أنشئ فرع ميزة: `git checkout -b feat/my-feature`
3. قم بإجراء التغييرات
4. اختبر محليًا:
   ```bash
   ```
5. قم بالتزام مع رسالة واضحة:
   ```
   feat: إضافة مفتاح الوضع الداكن إلى الإعدادات
   ```
6. ادفع وافتح طلب سحب ضد `main`

### اتفاقية رسالة الالتزام

| البادئة | الاستخدام |
|---------|-----------|
| `feat:` | ميزة جديدة |
| `fix:` | إصلاح خطأ |
| `docs:` | وثائق فقط |
| `refactor:` | إعادة هيكلة الكود (بدون تغيير في السلوك) |
| `i18n:` | تحديثات الترجمة |
| `plugin:` | تغييرات متعلقة بالإضافات |

### قائمة التحقق من طلب السحب

- [ ] يعمل الكود بدون أخطاء
- [ ] لا توجد سلاسل مشفرة (استخدم مفاتيح i18n)
- [ ] لا توجد `console.log` متبقية في كود الإنتاج
- [ ] لا تزال الاختبارات الحالية تمر

---

## 🌐 مساهمات الترجمة (254 لغة)

تدعم WIA SOOM **254 لغة** — من الأمهرية إلى الزولو، بما في ذلك لغة برايل واللغات من اليمين لليسار.

### كيفية عمل الترجمة

- ملف اللغة الأساسية: `src/renderer/src/i18n/en.json`
- جميع ملفات اللغات الـ 254 في نفس الدليل
- تتم الترجمة عبر `scripts/translate-patch.js` (GPT-4o-mini API)

### كيفية المساهمة في الترجمات

#### الخيار 1: إصلاح ترجمة محددة

1. ابحث عن ملف اللغة: `src/renderer/src/i18n/{lang-code}.json`
2. قم بإصلاح الترجمة غير الصحيحة
3. قدم طلب سحب مع التغيير

#### الخيار 2: إضافة مفاتيح مفقودة
§§§CHUNK_SEPARATOR§§§
#### الخيار 3: مراجعة الترجمات الآلية

تمت ترجمة العديد من لغاتنا الـ 254 بواسطة الآلات. مراجعات الناطقين الأصليين ذات قيمة كبيرة!

1. اختر ملف لغتك
2. راجع الترجمات
3. قم بإصلاح أي ترجمات غير ملائمة أو غير صحيحة
4. قدم طلب سحب

### رموز اللغات

نستخدم رموز ISO 639-1 القياسية (مثل `ko`, `en`, `ja`, `ar`, `hi`) مع المتغيرات الإقليمية عند الحاجة (مثل `zh-CN`, `pt-BR`).

---

## 🛠 إعداد التطوير

### المتطلبات الأساسية

- Node.js 18+
- npm 9+
- Git

### الإعداد
§§§CHUNK_SEPARATOR§§§
### البناء
§§§CHUNK_SEPARATOR§§§
> ملاحظة: الذاكرة الافتراضي�� 2GB ليست كافية بسبب ملفات اللغات الـ 254 + حزمة محرر موناكو (~38MB renderer).

### هيكل المشروع
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 شكرًا لك

كل مساهمة تجعل WIA SOOM أفضل للمطورين حول العالم.

سواءً كنت تصلح خطأ مطبعي، أو تترجم نصًا، أو تبني إضافة، أو تضيف ميزة رئيسية — **أنت جزء من هذه القصة.**

---

<p align="center"><em>تم البناء مع ❤️ بواسطة SmileStory Inc. ومساهمين من جميع أنحاء العالم.</em></p>
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
