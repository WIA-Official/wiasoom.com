<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">دليل مطور إضافة WIA SOOM</h1>
<p align="center"><strong>قم بإنشاء إضافتك الخاصة في 5 دقائق.</strong></p>
<p align="center">قم بإنشاء أدوات خادم قوية، ولوحات معلومات، وأتمتة — مباشرة داخل WIA SOOM.</p>

---

## جدول المحتويات

- [الجزء 1: البداية السريعة — أول إضافة لك في 5 دقائق](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [الجزء 2: مرجع واجهة برمجة تطبيقات سياق الإضافة](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [الجزء 3: بناء واجهة مستخدم مخصصة باستخدام Webviews](#part-3-building-custom-ui-with-webviews)
- [الجزء 4: نشر الإضافة الخاصة بك](#part-4-publishing-your-plugin)
- [الجزء 5: أفضل الممارسات](#part-5-best-practices)
- [الجزء 6: أمثلة من العالم الحقيقي](#part-6-real-world-examples)
- [الملحق: الفئات والرموز](#appendix-categories--icons)

---

## الجزء 1: البداية السريعة — أول إضافة لك في 5 دقائق

### ما الذي ستقوم بإنشائه

إضافة "مرحبا بالعالم" التي تضيف زرًا إلى الشريط الجانبي. عند النقر عليه، يظهر إشعار.

### الخطوة 1: إنشاء مجلد الإضافة
§§§CHUNK_SEPARATOR§§§
### الخطوة 2: إنشاء package.json
§§§CHUNK_SEPARATOR§§§
**الحقول المطلوبة:** `name`, `version`, `description`, `author`, `main`

### الخطوة 3: إنشاء index.js
§§§CHUNK_SEPARATOR§§§
### الخطوة 4: إعادة تشغيل WIA SOOM

أعد تشغيل التطبيق (أو قم بتبديل الإضافة في الإعدادات → الإضافات).

يجب أن ترى زر **"مرحبا بالعالم"** في الشريط الجانبي. انقر عليه — سترى إشعار نجاح!

### كيف يعمل
§§§CHUNK_SEPARATOR§§§
---

## الجزء 2: مرجع واجهة برمجة تطبيقات سياق الإضافة

عندما يتم استدعاء دالة `activate(context)` الخاصة بك، يوفر `context` (أو `ctx`) هذه الواجهات:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — تشغيل الأوامر على الخوادم البعيدة

#### `terminal.send(sessionId, data)`

أرسل أمرًا (أو أي بيانات) إلى جلسة طرفية نشطة.

| المعامل | النوع | الوصف |
|---------|------|-------|
| `sessionId` | `string` | جلسة الطرفية التي سترسل إليها |
| `data` | `string` | الأمر أو البيانات التي سترسلها |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

اشترك في جميع المخرجات من جلسة طرفية. يُرجع **دالة إلغاء الاشتراك**.

| المعامل | النوع | الوصف |
|---------|------|-------|
| `sessionId` | `string` | جلسة الطرفية التي ستراقبها |
| `callback` | `(data: string) => void` | يتم استدعاؤها مع كل جزء من المخرجات |
| **يرجع** | `() => void` | استدعِ هذا لإيقاف الا��تماع |
§§§CHUNK_SEPARATOR§§§
**مهم:** احفظ دائمًا دالة إلغاء الاشتراك واستدعها في `deactivate()` لمنع تسرب الذاكرة.

---

### `ctx.sftp` — نقل الملفات

> **الحالة: قادم قريبًا** — تم تعريف واجهة برمجة تطبيقات SFTP ولكن لم يتم توصيلها بعد بمحرك SFTP الخاص بالتطبيق. حاليًا، تعيد `list()` مصفوفة فارغة، و`upload()`/`download()` لا تفعل شيئًا. سيتم تنفيذ ذلك بالكامل في إصدار مستقبلي. في الوقت الحالي، استخدم `ctx.terminal.send()` مع أوامر `scp` أو `rsync` كحل بديل.

#### `sftp.list(sessionId, path)`

قائمة الملفات في دليل بعيد.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

قم بتحميل ملف من الجهاز المحلي إلى الخادم البعيد.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

قم بتنزيل ملف من الخادم البعيد إلى الجهاز المحلي.
§§§CHUNK_SEPARATOR§§§
**حل بديل (حتى تصبح واجهة برمجة تطبيقات SFTP نشطة):**
§§§CHUNK_SEPARATOR��§§
---

### `ctx.ui` — واجهة المستخدم

#### `ui.addSidebarButton(options)`

أضف زرًا إلى الشريط الجانبي لـ WIA SOOM.

| الخيار | النوع | مطلوب | الوصف |
|--------|------|--------|-------|
| `id` | `string` | لا | معرف فريد (يكون افتراضيًا اسم الإضافة) |
| `icon` | `string` | نعم | اسم أيقونة Lucide (مثل، `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | نعم | نص الزر المعروض في الشريط الجانبي |
| `onClick` | `() => void` | نعم | الدالة التي يتم استدعاؤها عند النقر على الزر |
§§§CHUNK_SEPARATOR§§§
**مرجع الأيقونات:** تصفح جميع الأيقونات المتاحة على [lucide.dev/icons](https://lucide.dev/icons)

> **ملاحظة توافق:** بعض الإضافات القديمة تستخدم معاملات موضعية مثل `addSidebarButton(id, icon, label, onClick)`. تستخدم واجهة برمجة التطبيقات الرسمية **كائن الخيارات** كما هو موثق أعلاه. استخدم دائمًا نمط الكائن للإضافات الجديدة.

#### `ui.openWebview(options)`

افتح نافذة منبثقة بمحتوى HTML ��خصص. هذه هي الطريقة التي تبني بها واجهات مستخدم غنية.

| الخيار | النوع | الوصف |
|--------|------|-------|
| `title` | `string` | عنوان النافذة |
| `html` | `string` | محتوى HTML الكامل الذي سيتم عرضه |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> انظر [الجزء 3](#part-3-building-custom-ui-with-webviews) لأنماط الويب المتقدمة.

#### `ui.showNotification(type, message)`

عرض إشعار منبثق.

| المعامل | النوع | الوصف |
|---------|------|-------|
| `type` | `'success' \| 'error' \| 'info'` | نمط الإشعار |
| `message` | `string` | النص الذي سيتم عرضه |
```json
{
  "name": "hello-world",
  "version": "1.0.0",
  "description": "My first WIA SOOM plugin — says hello!",
  "author": "Your Name",
  "main": "index.js",
  "license": "MIT",
  "keywords": ["hello", "example"],
  "soom": {
    "minVersion": "0.50.0"
  }
}
```
#### `ui.addStatusBarItem(id, text)`

إضافة عنصر نصي دائم إلى شريط الحالة السفلي.

| المعامل | النوع | الوصف |
|---------|------|-------|
| `id` | `string` | معرف فريد لهذا العنصر في شريط الحالة |
| `text` | `string` | النص الذي سيتم عرضه |
```javascript
'use strict';

/**
 * Hello World — WIA SOOM Plugin
 *
 * This is the simplest possible plugin.
 * It adds a sidebar button and shows a notification when clicked.
 */

exports.activate = function activate(context) {
  // Add a button to the sidebar
  context.ui.addSidebarButton({
    icon: 'hand-metal',      // Lucide icon name (see: lucide.dev/icons)
    label: 'Hello World',
    onClick: function() {
      context.ui.showNotification('success', 'Hello from WIA SOOM! Your first plugin works!');
    }
  });
};

// Optional: cleanup when plugin is disabled or app closes
exports.deactivate = function deactivate() {
  // Nothing to clean up in this example
};
```
---

### `ctx.settings` — التخزين الدائم

يتم تخزين إعدادات الإضافة بشكل دائم في `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

قراءة قيمة محفوظة.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
ترجع `undefined` إذا لم يكن المفتاح موجودًا.

#### `settings.set(key, value)`

حفظ قيمة. يدعم السلاسل النصية، الأرقام، القيم المنطقية، المصفوفات، والكائنات.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**مثال: تذكر تفضيلات المستخدم**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — تكامل الذكاء الاصطناعي

> **الحالة: قادم قريبًا** — تم تعريف واجهة برمجة التطبيقات للذكاء الاصطناعي ولكن لم يتم ربطها بعد بـ Soomy. حاليًا ترجع `{ response: 'AI not yet connected' }`. التخطيط لتكامل كامل للذكاء الاصطناعي في إصدار مستقبلي.

#### `ai.chat(messages, options?)`

إرسال الرسائل إلى مساعد الذكاء الاصطناعي (Soomy).
```javascript
// Watch terminal output for errors
var unsubscribe = context.terminal.onOutput(sessionId, function(data) {
  if (data.includes('ERROR') || data.includes('error')) {
    context.ui.showNotification('error', 'Error detected in terminal output!');
  }
});

// Later: stop watching
unsubscribe();
```
---

## الجزء 3: بناء واجهة مستخدم مخصصة باستخدام Webviews

تتيح لك واجهة برمجة التطبيقات `openWebview()` بناء واجهات لوحات المعلومات باستخدام HTML وCSS وJavaScript — كل ذلك داخل نافذة منبثقة.

> **قيود مهمة:** Webviews هي **للعرض فقط**. لا يمكنها الاتصال بواجهات برمجة التطبيقات للإضافات (`ctx.settings`، `ctx.terminal`، إلخ). استخدم أزرار الشريط الجانبي لجميع إجراءات المستخدم، واستخدم `openWebview()` لعرض الحالة الحالية. إذا كنت بحاجة إلى ميزات تفاعلية، قم بتحفيزها من أزرار الشريط الجانبي وأعد فتح Webview لتحديث العرض.

### نمط: أمر الطرفية → تحليل المخرجات → العرض في HTML

هذا هو النمط الأكثر شيوعًا للإضافات. تقوم بتشغيل أمر، وتحليل النتيجة، وعرضها بصريًا.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### نمط: لوحة معلومات تفاعلية مع تحديث تلقائي
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### نمط: عرض الإعدادات في Webview

> **ملاحظة:** Webviews هي للعرض فقط — لا يمكنها الاتصال بواجهات برمجة التطبيقات للإضافات. استخدم `ctx.settings` في معالجات أزرار الشريط الجانبي الخاصة بك لتعديل الإعدادات، واستخدم `openWebview()` لعرض الحالة الحالية.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## الجزء 4: نشر الإضافة الخاصة بك

### الخطوة 1: اختبار محليًا

1. انسخ الإضافة الخاصة بك إلى `~/.wia-soom/plugins/{your-plugin}/`
2. أعد تشغيل WIA SOOM
3. تحقق من أنها تعمل: يظهر زر الشريط الجانبي، وتعمل الميزات بشكل صحيح
4. اختبار الحالات الحدية: ماذا يحدث إذا لم يكن هناك طرفية متصلة؟

### الخطوة 2: التحضير للتقديم

يجب أن تحتوي مجلد الإضافة الخاصة بك على:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**الحقول المطلوبة في `package.json`:**

| الحقل | الوصف | المثال |
|-------|-------------|---------|
| `name` | معرف فريد بتنسيق kebab-case | `"my-awesome-plugin"` |
| `version` | إصدار دلالي | `"1.0.0"` |
| `description` | وصف من سطر واحد | `"Monitors nginx access logs in real-time"` |
| `author` | اسمك | `"John Doe"` |
| `main` | نقطة الدخول | `"index.js"` |

**الحقول الاختيارية:**

| الحقل | الوصف |
|-------|-------------|
| `license` | نوع الترخيص (يوصى بـ MIT) |
| `keywords` | مصفوفة من علامات البحث |
| `soom.minVersion` | الحد الأدنى من إصدار WIA SOOM المطلوب |

### الخطوة 3: تقديم إلى سجل الإضافات

1. **قم بعمل **Package** your plugin as a ZIP file
2. **أضف** الإضافة الخاصة بك إلى `plugins/{your-plugin-name}/`
3. **قدّم** طلب سحب

### الخطوة 4: المراجعة وا��موافقة

نقوم بمراجعة كل إضافة من أجل:

- **الأمان** — لا توجد واجهات برمجة تطبيقات خطيرة (انظر [قواعد الأمان](#security-rules))
- **الجودة** — هل تعمل؟ هل الكود نظيف؟
- **الفائدة** — هل تحل مشكلة حقيقية؟

بعد الموافقة:
1. تتم إضافة الإضافة الخاصة بك إلى `registry.json`
2. يتم إنشاء حزمة ZIP في `dist/`
3. تظهر الإضافة الخاصة بك في **متجر الإضافات** لجميع مستخدمي WIA SOOM!

---

## الجزء 5: أفضل الممارسات

### قواعد الأمان

هذه القواعد **إلزامية**. سيتم رفض الإضافات التي تنتهكها.

| القاعدة | السبب |
|------|-----|
| **لا تستخدم أبداً** `eval()` أو `new Function()` | خطر حقن الكود |
| **لا تستخدم أبداً** `child_process`، `exec()`، `spawn()` | استخدم فقط `ctx.terminal.send()` للأوامر |
| **لا تستخدم أبداً** جلب عناوين URL خارجية | استثناء: نقاط نهاية API لـ `wiasoom.com` |
| **لا تستخدم أبداً** الوصول إلى `process.env` | قد تحتوي ال��تغيرات البيئية على أسرار |
| **لا تستخدم أبداً** `require('fs')` مباشرة | استخدم `ctx.settings` للتخزين، و `ctx.sftp` لنقل الملفات |
| **لا تستخدم أبداً** حزم npm الخارجية | JavaScript النقي فقط — لا تستخدم node_modules |
| **يجب** استخدام `ctx.terminal.send()` لجميع الأوامر عن بُعد | يتم ذلك عبر قناة SSH الآمنة |
| **يجب** تنظيف في `deactivate()` | إزالة المستمعين، مسح الفواصل الزمنية |

### معالجة الأخطاء

قم دائماً بتغليف العمليات المهددة في try/catch:
```javascript
context.ui.addSidebarButton({
  id: 'my-dashboard',
  icon: 'layout-dashboard',
  label: 'My Dashboard',
  onClick: function() {
    // Open a webview, run a command, show a notification — anything!
    context.ui.openWebview({
      title: 'My Dashboard',
      html: '<h1>Hello!</h1><p>This is my dashboard.</p>'
    });
  }
});
```
### التنظيف في deactivate()

إذا كانت الإضافة الخاصة بك تنشئ فواصل زمنية، أو مستمعين، أو اشتراكات — قم بتنظيفها:
```javascript
context.ui.openWebview({
  title: 'Server Status',
  html: `
    <html>
    <body style="font-family: sans-serif; padding: 20px; background: #1a1a2e; color: #e2e8f0;">
      <h1>Server Status</h1>
      <div id="status">Loading...</div>
      <script>
        document.getElementById('status').textContent = 'All systems operational';
      </script>
    </body>
    </html>
  `
});
```
### دعم i18n

يدعم WIA SOOM 254 لغة. لجعل تسمية الإضافة الخاصة بك قابلة للترجمة، استخدم نهجاً بسيطاً:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## الجزء 6: أمثلة من العالم الحقيقي

### المثال 1: فاحص قرص الخادم

تشغيل `df -h` على الخادم البعيد وعرض المساحة المستخدمة/المتاحة في شريط الحالة.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### المثال 2: مدير TODO

إضافة تدير قائمة TODO باستخدام الإعدادات للتخزين الدائم وعرض الويب للعرض.

> **نمط التصميم:** بما أن واجهات الويب لا يمكنها استدعاء واجهات برمجة التطبيقات الخاصة بالإضافات مباشرة، تستخدم هذه الإضافة نهج "لقطة" — تقوم بقراءة TODOs من الإعدادات، وعرضها كـ HTML للقراءة فقط، وتوفر إجراءات قائمة جانبية لإضافة العناصر. واجهة الويب هي طبقة **عرض**، وليست نموذج تفاعلي.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### المثال 3: مراقب الأخطاء

يراقب مخرجات الطرفية ويرسل إشعاراً عند اكتشاف أنماط معينة.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## الملحق: الفئات والرموز

### فئات الإضافات (29)

استخدم هذه في `package.json` `keywords` أو عند التقديم إلى السجل:

| الفئة | الوصف |
|-------|-------|
| `server` | إدارة الخادم العامة |
| `devtools` | أدوات التطوير |
| `calculator` | الآلات الحاسبة والمحولات |
| `simulator` | المحاكيات |
| `game` | ألعاب الطرفية |
| `business` | أدوات الأعمال |
| `security` | الأمان والتدقيق |
| `web` | إدارة خادم الويب |
| `education` | أدوات تعليمية |
| `health` | أدوات متعلقة بالصحة |
| `islamic` | أدوات إسلامية (أوقات الصلاة، إلخ) |
| `science` | أدوات علمية |
| `quantum` | أدوات الحوسبة الكمومية |
| `ai` | أدوات مدعومة بالذكاء الاصطناعي |
| `biotech` | أدوات التكنولوجيا الحيوية |
| `space` | أدوات الفض��ء وعلم الفلك |
| `network` | أدوات الشبكة |
| `database` | إدارة قواعد البيانات |
| `monitoring` | مراقبة الخادم |
| `devops` | DevOps و CI/CD |
| `utility` | أدوات عامة |
| `design` | أدوات التصميم |
| `ecommerce` | أدوات التجارة الإلكترونية |
| `automation` | أدوات الأتمتة |
| `kpop` | أدوات متعلقة بـ K-pop |
| `accessibility` | أدوات الوصول |
| `analytics` | التحليلات والتقارير |
| `wia` | أدوات نظام WIA البيئي |
| `all` | تظهر في جميع الفئات |

### الرموز الموصى بها (Lucide)

| اسم الرمز | الاستخدام |
|-----------|----------|
| `server` | إدارة الخادم |
| `shield` | الأمان |
| `database` | قاعدة البيانات |
| `activity` | المراقبة |
| `terminal` | أدوات الطرفية |
| `code` | التطوير |
| `hard-drive` | القرص/التخزين |
| `network` | الشبكات |
| `lock` | المصادقة/التشفير |
| `eye` | المراقبة/المشاهدة |
| `check-square` | المهام/TODO |
| `layout-dashboard` | لوحات المعلومات |
| `settings` | التكوين |
| `zap` | ا��أتمتة |
| `globe` | الويب/الدولي |

تصفح جميع الرموز التي تزيد عن 1,500: [lucide.dev/icons](https://lucide.dev/icons)

---

## تحتاج إلى مساعدة؟

- **مشكلات GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **مشكلات الإضافات:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **أمثلة على الإضافات:** [Website](https://wiasoom.com)
- **الموقع الإلكتروني:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ابنِ شيئًا مذهلاً. شاركه مع العالم.</em></p>
<p align="center"><em>— فريق WIA SOOM</em></p>
```javascript
exports.activate = function(context) {
  // Load saved preference (or default to 60 seconds)
  var interval = context.settings.get('interval') || 60;

  context.ui.addSidebarButton({
    icon: 'settings',
    label: 'Configure',
    onClick: function() {
      // Toggle between 30s and 60s
      interval = (interval === 60) ? 30 : 60;
      context.settings.set('interval', interval);
      context.ui.showNotification('info', 'Refresh interval: ' + interval + 's');
    }
  });
};
```

---

### `ctx.ai` — AI integration

> **Status: Coming Soon** — The AI API is defined but not yet connected to Soomy. Currently returns `{ response: 'AI not yet connected' }`. Full AI integration is planned for a future release.

#### `ai.chat(messages, options?)`

Send messages to the AI assistant (Soomy).

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## Part 3: Building Custom UI with Webviews

The `openWebview()` API lets you build dashboard UIs with HTML, CSS, and JavaScript — all inside a popup window.

> **Important limitation:** Webviews are **display-only**. They cannot call back into plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Use sidebar buttons for all user actions, and use `openWebview()` to display current state. If you need interactive features, trigger them from sidebar buttons and re-open the webview to refresh the display.

### Pattern: Terminal Command → Parse Output → Show in HTML

This is the most common plugin pattern. You run a command, parse the result, and display it visually.

```javascript
exports.activate = function(context) {
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: 'Disk Usage',
    onClick: function() {
      // Collect terminal output
      var output = '';
      var unsub = context.terminal.onOutput('current', function(data) {
        output += data;
      });

      // Send the command
      context.terminal.send('current', 'df -h --output=target,pcent,size,used,avail 2>/dev/null\n');

      // Wait for output, then show it
      setTimeout(function() {
        unsub(); // Stop listening

        // Parse the output into HTML
        var rows = output.split('\n')
          .filter(function(line) { return line.includes('%'); })
          .map(function(line) {
            var parts = line.trim().split(/\s+/);
            return '<tr><td>' + parts.join('</td><td>') + '</td></tr>';
          })
          .join('');

        context.ui.openWebview({
          title: 'Disk Usage',
          html: '<html><body style="font-family:monospace;background:#1a1a2e;color:#e2e8f0;padding:20px;">' +
            '<h2>Disk Usage</h2>' +
            '<table style="width:100%;border-collapse:collapse;">' +
            '<tr style="border-bottom:1px solid #333;"><th>Mount</th><th>Used%</th><th>Size</th><th>Used</th><th>Avail</th></tr>' +
            rows +
            '</table></body></html>'
        });
      }, 1500); // Give the command time to complete
    }
  });
};
```

### Pattern: Interactive Dashboard with Auto-Refresh

```javascript
exports.activate = function(context) {
  context.ui.addSidebarButton({
    icon: 'activity',
    label: 'Live Monitor',
    onClick: function() {
      context.ui.openWebview({
        title: 'Live Server Monitor',
        html: [
          '<html><head><style>',
          '  body { font-family: -apple-system, sans-serif; background: #0f172a; color: #e2e8f0; padding: 24px; }',
          '  .card { background: #1e293b; border-radius: 12px; padding: 20px; margin: 12px 0; }',
          '  .card h3 { margin: 0 0 8px 0; color: #38bdf8; }',
          '  .metric { font-size: 2rem; font-weight: bold; }',
          '  .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; }',
          '  .bar { height: 8px; background: #334155; border-radius: 4px; overflow: hidden; }',
          '  .bar-fill { height: 100%; background: linear-gradient(90deg, #22c55e, #eab308, #ef4444); transition: width 0.5s; }',
          '</style></head><body>',
          '  <h1>Server Monitor</h1>',
          '  <div class="grid">',
          '    <div class="card"><h3>CPU</h3><div class="metric" id="cpu">--</div><div class="bar"><div class="bar-fill" id="cpu-bar" style="width:0%"></div></div></div>',
          '    <div class="card"><h3>Memory</h3><div class="metric" id="mem">--</div><div class="bar"><div class="bar-fill" id="mem-bar" style="width:0%"></div></div></div>',
          '    <div class="card"><h3>Disk</h3><div class="metric" id="disk">--</div><div class="bar"><div class="bar-fill" id="disk-bar" style="width:0%"></div></div></div>',
          '    <div class="card"><h3>Uptime</h3><div class="metric" id="uptime">--</div></div>',
          '  </div>',
          '  <p style="color:#64748b;margin-top:20px;">Refreshes every 5 seconds</p>',
          '</body></html>'
        ].join('\n')
      });
    }
  });
};
```

### Pattern: Displaying Settings in a Webview

> **Note:** Webviews are display-only — they cannot call back into plugin APIs. Use `ctx.settings` in your sidebar button handlers to modify settings, and use `openWebview()` to show the current state.

```javascript
function showCurrentSettings(context) {
  var url = context.settings.get('webhookUrl') || '(not set)';
  var interval = context.settings.get('interval') || 60;

  context.ui.openWebview({
    title: 'Current Settings',
    html: [
      '<html><body style="font-family:sans-serif;background:#0f172a;color:#e2e8f0;padding:24px;">',
      '<h2>Current Settings</h2>',
      '<div style="background:#1e293b;padding:16px;border-radius:8px;margin:12px 0;">',
      '  <p><strong>Webhook URL:</strong> ' + url + '</p>',
      '  <p><strong>Refresh Interval:</strong> ' + interval + 's</p>',
      '</div>',
      '<p style="color:#475569;font-size:0.8rem;">Use sidebar buttons to change settings.</p>',
      '</body></html>'
    ].join('\n')
  });
}

// Toggle interval via sidebar button
context.ui.addSidebarButton({
  icon: 'settings',
  label: 'Toggle Interval',
  onClick: function() {
    var current = context.settings.get('interval') || 60;
    var next = (current === 60) ? 30 : 60;
    context.settings.set('interval', next);
    context.ui.showNotification('info', 'Interval set to ' + next + 's');
  }
});
```

---

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Copy your plugin to `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verify it works: sidebar button appears, features work correctly
4. Test edge cases: what happens if no terminal is connected?

### Step 2: Prepare for submission

Your plugin folder must contain:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | One-line description | `"Monitors nginx access logs in real-time"` |
| `author` | Your name | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | License type (MIT recommended) |
| `keywords` | Array of search tags |
| `soom.minVersion` | Minimum WIA SOOM version required |

### Step 3: Submit to the Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** your plugin to `plugins/{your-plugin-name}/`
3. **Submit** a Pull Request

### Step 4: Review and approval

We review every plugin for:

- **Security** — no dangerous APIs (see [Security Rules](#security-rules))
- **Quality** — does it work? Is the code clean?
- **Usefulness** — does it solve a real problem?

After approval:
1. Your plugin is added to `registry.json`
2. A ZIP bundle is created in `dist/`
3. Your plugin appears in the **Plugin Store** for all WIA SOOM users!

---

## Part 5: Best Practices

### Security Rules

These rules are **mandatory**. Plugins that violate them will be rejected.

| Rule | Why |
|------|-----|
| **NEVER** use `eval()` or `new Function()` | Code injection risk |
| **NEVER** use `child_process`, `exec()`, `spawn()` | Only use `ctx.terminal.send()` for commands |
| **NEVER** fetch external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** access `process.env` | Environment variables may contain secrets |
| **NEVER** use `require('fs')` directly | Use `ctx.settings` for storage, `ctx.sftp` for file transfer |
| **NEVER** use npm external packages | Pure JavaScript only — no node_modules |
| **MUST** use `ctx.terminal.send()` for all remote commands | This goes through the secure SSH channel |
| **MUST** clean up in `deactivate()` | Remove listeners, clear intervals |

### Error Handling

Always wrap risky operations in try/catch:

```javascript
exports.activate = function(context) {
  try {
    // Your plugin logic
    context.ui.addSidebarButton({
      icon: 'server',
      label: 'My Tool',
      onClick: function() {
        try {
          // risky operation
        } catch (err) {
          context.ui.showNotification('error', 'Something went wrong: ' + err.message);
        }
      }
    });
  } catch (err) {
    console.error('[my-plugin] Failed to activate:', err);
  }
};
```

### Cleanup in deactivate()

If your plugin creates intervals, listeners, or subscriptions — clean them up:

```javascript
var intervals = [];
var unsubscribers = [];

exports.activate = function(context) {
  // Save references to things you need to clean up
  var unsub = context.terminal.onOutput('session1', function(data) { /* ... */ });
  unsubscribers.push(unsub);

  var timer = setInterval(function() { /* ... */ }, 5000);
  intervals.push(timer);
};

exports.deactivate = function() {
  // Clean up everything
  intervals.forEach(function(timer) { clearInterval(timer); });
  intervals = [];
  unsubscribers.forEach(function(unsub) { unsub(); });
  unsubscribers = [];
};
```

### i18n Support

WIA SOOM supports 254 languages. To make your plugin label translatable, use a simple approach:

```javascript
// Simple multi-language labels
// 간단한 다국어 라벨
var LABELS = {
  en: { name: 'Disk Checker', scanning: 'Scanning disks...' },
  ko: { name: '디스크 체커', scanning: '디스크 스캔 중...' },
  ja: { name: 'ディスクチェッカー', scanning: 'ディスクスキャン中...' },
  // Add more languages as needed
};

function t(key) {
  // Try to detect language from localStorage (set by WIA SOOM language modal)
  var lang = 'en'; // default
  try { lang = localStorage.getItem('soom_language') || 'en'; } catch(e) {}
  return (LABELS[lang] && LABELS[lang][key]) || LABELS.en[key] || key;
}

exports.activate = function(context) {
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: t('name'),
    onClick: function() {
      context.ui.showNotification('info', t('scanning'));
    }
  });
};
```

---

## Part 6: Real-World Examples

### Example 1: Server Disk Checker

Runs `df -h` on the remote server and shows used/available space in the status bar.

```javascript
'use strict';

/**
 * Server Disk Checker — WIA SOOM Plugin
 * 서버 디스크 용량 체커
 *
 * Shows disk usage in the status bar.
 * Alerts when any partition exceeds 90%.
 */

var checkInterval = null;
var unsubscribers = [];

exports.activate = function activate(context) {
  // Add sidebar button to trigger manual check
  // 사이드바 버튼: 수동 체크 트리거
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: 'Disk Check',
    onClick: function() {
      checkDisk(context);
    }
  });

  // Auto-check every 5 minutes
  // 5분마다 자동 체크
  var interval = context.settings.get('interval') || 300;
  checkInterval = setInterval(function() {
    checkDisk(context);
  }, interval * 1000);
};

function checkDisk(context) {
  var output = '';

  // Listen for terminal output
  // 터미널 출력 수신
  var unsub = context.terminal.onOutput('current', function(data) {
    output += data;
  });
  unsubscribers.push(unsub);

  // Send the command
  // 명령 전송
  context.terminal.send('current', "df -h / | tail -1 | awk '{print $5}'\n");

  // Parse after delay
  // 잠시 후 파싱
  setTimeout(function() {
    unsub();

    // Extract percentage (e.g., "73%")
    // 퍼센트 추출 (예: "73%")
    var match = output.match(/(\d+)%/);
    if (match) {
      var percent = parseInt(match[1]);
      context.ui.addStatusBarItem('disk-usage', 'Disk: ' + percent + '%');

      // Alert if over 90%
      // 90% 초과 시 경고
      if (percent > 90) {
        context.ui.showNotification('error', 'WARNING: Disk usage at ' + percent + '%! Free up space.');
      }
    }
  }, 2000);
}

exports.deactivate = function deactivate() {
  if (checkInterval) {
    clearInterval(checkInterval);
    checkInterval = null;
  }
  unsubscribers.forEach(function(unsub) { unsub(); });
  unsubscribers = [];
};
```

---

### Example 2: TODO Manager

A plugin that manages a TODO list using settings for persistent storage and a webview for display.

> **Design pattern:** Since webviews cannot directly call plugin APIs, this plugin uses a "snapshot" approach — it reads TODOs from settings, renders them as read-only HTML, and provides sidebar-based actions for adding items. The webview is a **display** layer, not an interactive form.

```javascript
'use strict';

/**
 * TODO Manager — WIA SOOM Plugin
 * 할일 관리자
 *
 * Pattern: settings-driven display (no webview↔plugin bridge needed)
 * 패턴: settings 기반 표시 (웹뷰↔플러그인 브릿지 불필요)
 */

exports.activate = function activate(context) {
  // Show current TODO count in status bar
  // 현재 TODO 수를 상태바에 표시
  updateStatusBar(context);

  // Button 1: View TODO list
  // 버튼 1: TODO 목록 보기
  context.ui.addSidebarButton({
    id: 'todo-view',
    icon: 'check-square',
    label: 'TODO List',
    onClick: function() {
      showTodoList(context);
    }
  });

  // Button 2: Quick-add a TODO via notification prompt
  // 버튼 2: 알림 프롬프트로 빠르게 TODO 추가
  context.ui.addSidebarButton({
    id: 'todo-add',
    icon: 'plus-square',
    label: 'Add TODO',
    onClick: function() {
      // Use terminal echo as a quick input method
      // 터미널 echo를 빠른 입력 방법으로 사용
      var todos = context.settings.get('todos') || [];
      var newItem = 'Task #' + (todos.length + 1) + ' — ' + new Date().toLocaleString();
      todos.push({ text: newItem, done: false, createdAt: new Date().toISOString() });
      context.settings.set('todos', todos);
      updateStatusBar(context);
      context.ui.showNotification('success', 'Added: ' + newItem);
    }
  });

  // Button 3: Clear completed TODOs
  // 버튼 3: 완료된 TODO 정리
  context.ui.addSidebarButton({
    id: 'todo-clear',
    icon: 'trash-2',
    label: 'Clear Done',
    onClick: function() {
      var todos = context.settings.get('todos') || [];
      var before = todos.length;
      todos = todos.filter(function(t) { return !t.done; });
      context.settings.set('todos', todos);
      updateStatusBar(context);
      context.ui.showNotification('info', 'Cleared ' + (before - todos.length) + ' completed tasks');
    }
  });
};

function updateStatusBar(context) {
  var todos = context.settings.get('todos') || [];
  var remaining = todos.filter(function(t) { return !t.done; }).length;
  context.ui.addStatusBarItem('todo-count', 'TODO: ' + remaining + '/' + todos.length);
}

function showTodoList(context) {
  var todos = context.settings.get('todos') || [];
  var remaining = todos.filter(function(t) { return !t.done; }).length;

  // Build read-only HTML display
  // 읽기 전용 HTML 표시 생성
  var rows = todos.map(function(todo, i) {
    var status = todo.done ? '✅' : '⬜';
    var style = todo.done ? 'text-decoration:line-through;color:#64748b;' : 'color:#e2e8f0;';
    var date = todo.createdAt ? new Date(todo.createdAt).toLocaleDateString() : '';
    return '<div style="display:flex;align-items:center;gap:12px;padding:12px 16px;background:#1e293b;border-radius:8px;margin:6px 0;">' +
      '<span style="font-size:1.2em;">' + status + '</span>' +
      '<span style="flex:1;' + style + '">' + escapeHtml(todo.text) + '</span>' +
      '<span style="color:#475569;font-size:0.75rem;">' + date + '</span>' +
      '</div>';
  }).join('');

  if (todos.length === 0) {
    rows = '<div style="text-align:center;padding:40px;color:#475569;">No tasks yet. Click "Add TODO" in the sidebar.</div>';
  }

  context.ui.openWebview({
    title: 'TODO List (' + remaining + ' remaining)',
    html: [
      '<html><body style="font-family:-apple-system,sans-serif;background:#0f172a;color:#e2e8f0;padding:24px;">',
      '<h1 style="margin-bottom:4px;">TODO List</h1>',
      '<p style="color:#64748b;margin-bottom:20px;">' + remaining + ' of ' + todos.length + ' remaining</p>',
      rows,
      '<p style="color:#334155;margin-top:24px;font-size:0.8rem;">Use sidebar buttons to add/clear tasks, then reopen this view.</p>',
      '</body></html>'
    ].join('\n')
  });
}

function escapeHtml(str) {
  return String(str).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

exports.deactivate = function deactivate() {};
```

---

### Example 3: Error Watcher

Monitors terminal output and sends a notification when specific patterns are detected.

```javascript
'use strict';

/**
 * Error Watcher — WIA SOOM Plugin
 * 에러 감시자
 *
 * Watches terminal output for error patterns.
 * Shows notification when errors are detected.
 * Configurable patterns via settings.
 */

var watchers = [];
var errorCount = 0;

// Default patterns to watch for
// 기본 감시 패턴
var DEFAULT_PATTERNS = [
  'FATAL',
  'CRITICAL',
  'OutOfMemory',
  'Segmentation fault',
  'kill -9',
  'No space left on device',
  'Connection refused',
  'Permission denied'
];

exports.activate = function activate(context) {
  // Load custom patterns or use defaults
  // 커스텀 패턴 로드 또는 기본값 사용
  var patterns = context.settings.get('patterns') || DEFAULT_PATTERNS;

  context.ui.addSidebarButton({
    icon: 'eye',
    label: 'Error Watcher',
    onClick: function() {
      context.ui.showNotification('info',
        'Watching for ' + patterns.length + ' error patterns. ' +
        errorCount + ' errors detected so far.'
      );
    }
  });

  // Update status bar
  // 상태바 업데이트
  context.ui.addStatusBarItem('error-watcher', 'Errors: ' + errorCount);

  // Watch current terminal
  // 현재 터미널 감시
  var unsub = context.terminal.onOutput('current', function(data) {
    for (var i = 0; i < patterns.length; i++) {
      if (data.includes(patterns[i])) {
        errorCount++;
        context.ui.addStatusBarItem('error-watcher', 'Errors: ' + errorCount);
        context.ui.showNotification('error',
          'Error detected: "' + patterns[i] + '" found in terminal output'
        );
        // Save error log
        // 에러 로그 저장
        var log = context.settings.get('errorLog') || [];
        log.push({
          pattern: patterns[i],
          time: new Date().toISOString(),
          snippet: data.substring(0, 200)
        });
        // Keep last 100 errors
        // 최근 100개만 유지
        if (log.length > 100) log = log.slice(-100);
        context.settings.set('errorLog', log);
        break; // One notification per output chunk
      }
    }
  });
  watchers.push(unsub);
};

exports.deactivate = function deactivate() {
  watchers.forEach(function(unsub) { unsub(); });
  watchers = [];
};
```

---

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | General server management |
| `devtools` | Development tools |
| `calculator` | Calculators and converters |
| `simulator` | Simulators |
| `game` | Terminal games |
| `business` | Business tools |
| `security` | Security and auditing |
| `web` | Web server management |
| `education` | Educational tools |
| `health` | Health-related tools |
| `islamic` | Islamic tools (prayer times, etc.) |
| `science` | Scientific tools |
| `quantum` | Quantum computing tools |
| `ai` | AI-powered tools |
| `biotech` | Biotechnology tools |
| `space` | Space and astronomy tools |
| `network` | Network tools |
| `database` | Database management |
| `monitoring` | Server monitoring |
| `devops` | DevOps and CI/CD |
| `utility` | General utilities |
| `design` | Design tools |
| `ecommerce` | E-commerce tools |
| `automation` | Automation tools |
| `kpop` | K-pop related tools |
| `accessibility` | Accessibility tools |
| `analytics` | Analytics and reporting |
| `wia` | WIA ecosystem tools |
| `all` | Appears in all categories |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Server management |
| `shield` | Security |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Terminal tools |
| `code` | Development |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Watching/monitoring |
| `check-square` | Tasks/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuration |
| `zap` | Automation |
| `globe` | Web/international |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Build something amazing. Share it with the world.</em></p>
<p align="center"><em>— The WIA SOOM Team</em></p>
