<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">راهنمای توسعه‌دهندگان پلاگین WIA SOOM</h1>
<p align="center"><strong>پلاگین خود را در ۵ دقیقه بسازید.</strong></p>
<p align="center">ابزارهای قدرتمند سرور، داشبوردها و اتوماسیون‌ها را — درست در داخل WIA SOOM — ایجاد کنید.</p>

---

## فهرست مطالب

- [قسمت ۱: شروع سریع — اولین پلاگین شما در ۵ دقیقه](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [قسمت ۲: مرجع API زمینه پلاگین](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [قسمت ۳: ساخت UI سفارشی با وب‌ویوها](#part-3-building-custom-ui-with-webviews)
- [قسمت ۴: انتشار پلاگین شما](#part-4-publishing-your-plugin)
- [قسمت ۵: بهترین شیوه‌ها](#part-5-best-practices)
- [قسمت ۶: مثال‌ه��ی واقعی](#part-6-real-world-examples)
- [ضمیمه: دسته‌ها و آیکون‌ها](#appendix-categories--icons)

---

## قسمت ۱: شروع سریع — اولین پلاگین شما در ۵ دقیقه

### چه چیزی خواهید ساخت

یک پلاگین "سلام دنیا" که یک دکمه به نوار کناری اضافه می‌کند. وقتی روی آن کلیک می‌شود، یک اعلان نمایش داده می‌شود.

### مرحله ۱: ایجاد پوشه پلاگین
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### مرحله ۲: ایجاد package.json
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
**فیلدهای مورد نیاز:** `name`, `version`, `description`, `author`, `main`

### مرحله ۳: ایجاد index.js
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
### مرحله ۴: راه‌اندازی مجدد WIA SOOM

برنامه را راه‌اندازی مجدد کنید (یا پلاگین را در تنظیمات → پلاگین‌ها خاموش/روشن کنید).

باید یک دکمه **"سلام دنیا"** در نوار کناری ببینید. روی آن کلیک کنید — یک اعلان موفقیت را خواهید دید!

### نحوه کار
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
---

## قسمت ۲: مرجع API زمینه پلاگین

زمانی که تابع `activate(context)` شما فراخوانی می‌شود، `context` (یا `ctx`) این APIها را فراهم می‌کند:
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
---

### `ctx.terminal` — اجرای دستورات بر روی سرورهای از راه دور

#### `terminal.send(sessionId, data)`

یک دستور (یا هر داده‌ای) را به یک جلسه ترمینال فعال ارسال کنید.

| پارامتر | نوع | توضیحات |
|-----------|------|-------------|
| `sessionId` | `string` | جلسه ترمینال که باید به آن ارسال شود |
| `data` | `string` | دستور یا داده‌ای که باید ارسال شود |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

به تمام خروجی‌ها از یک جلسه ترمینال مشترک شوید. یک **تابع لغو اشتراک** برمی‌گرداند.

| پارامتر | نوع | توضیحات |
|-----------|------|-------------|
| `sessionId` | `string` | جلسه ترمینال که باید مشاهده شود |
| `callback` | `(data: string) => void` | با هر بخش از خروجی فراخوانی می‌شود |
| **برمی‌گرداند** | `() => void` | این را برای متوقف کردن گوش دادن فراخوانی کنید |
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
**مهم:** همیشه تابع لغو اشتراک را ذخیره کنید و آن را در `deactivate()` فراخوانی کنید تا از نشت حافظه جلوگیری کنید.

---

### `ctx.sftp` — انتقال فایل

> **وضعیت: به زودی** — API SFTP تعریف شده اما هنوز به موتور SFTP برنامه متصل نشده است. `list()` در حال حاضر یک آرایه خالی برمی‌گرداند و `upload()`/`download()` هیچ عملی انجام نمی‌دهند. این در یک نسخه آینده به طور کامل پیاده‌سازی خواهد شد. فعلاً، از `ctx.terminal.send()` با دستورات `scp` یا `rsync` به عنوان یک راه‌حل موقت استفاده کنید.

#### `sftp.list(sessionId, path)`

فایل‌ها را در یک دایرکتوری از راه دور فهرست کنید.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

یک فایل را از ماشین محلی به سرور از راه دور بارگذاری کنید.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

یک فایل را از سرور از راه دور به ماشین محلی دانلود کنید.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**راه‌حل موقت (تا زمانی که API SFTP فعال شود):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — رابط کاربری

#### `ui.addSidebarButton(options)`

یک دکمه به نوار کناری WIA SOOM اضافه کنید.

| گزینه | نوع | الزامی | توضیحات |
|--------|------|----------|-------------|
| `id` | `string` | خیر | شناسه منحصر به فرد (به طور پیش‌فرض به نام پلاگین) |
| `icon` | `string` | بله | نام آیکون Lucide (به عنوان مثال، `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | بله | متن دکمه‌ای که در نوار کناری نمایش داده می‌شود |
| `onClick` | `() => void` | بله | تابعی که هنگام کلیک بر روی دکمه فراخوانی می‌شود |
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
**مرجع آیکون:** تمام آیکون‌های موجود را در [lucide.dev/icons](https://lucide.dev/icons) مرور کنید.

> **یادداشت سازگاری:** برخی از پلاگین‌های قدیمی از آرگومان‌های موقعیتی مانند `addSidebarButton(id, icon, label, onClick)` استفاده می‌کنند. API رسمی از یک **شیء گزینه** به عنوان مستند شده در بالا استفاده می‌کند. همیشه از سبک شیء برای پلاگین‌های جدید استفاده کنید.

#### `ui.openWebview(options)`

یک پنجره پاپ‌آپ با محتوای HTML سفارشی باز کنید. اینگونه است که شما UIهای غنی می‌سازید.

| گزینه | نوع | توضیحات |
|--------|------|-------------|
| `title` | `string` | عنوان پنجره |
| `html` | `string` | محتوای کامل HTML برای رندر |
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
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

Toast notification nishan dain.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Neshan dain text |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Permanently text item status bar ke zamin zindah karnay.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | A unique ID is for this status item |
| `text` | `string` | Neshan dain text |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Permanent storage

Plugin settings `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` mein hamesha kay liye rakhi jati hain.

#### `settings.get(key)`

Ek saved value parhna.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Agar key mojood nahi hai to `undefined` return karega.

#### `settings.set(key, value)`

Ek value save karna. Strings, numbers, booleans, arrays, aur objects ko support karta hai.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Example: User preferences yaad rakhna**
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

> **Status: Aane wala hai** — AI API define kiya gaya hai lekin abhi Soomy se connected nahi hai. Filhal `{ response: 'AI not yet connected' }` return karta hai. Full AI integration ka plan future release ke liye hai.

#### `ai.chat(messages, options?)`

AI assistant (Soomy) ko messages bhejna.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Webviews ke sath Custom UI banana

`openWebview()` API aapko HTML, CSS, aur JavaScript ke sath dashboard UIs banane ki ijaazat deti hai — sab kuch ek popup window ke andar.

> **Ahm ahd:** Webviews sirf **display-only** hain. Ye plugin APIs (`ctx.settings`, `ctx.terminal`, etc.) ko call nahi kar sakte. Sab user actions ke liye sidebar buttons ka istemal karein, aur current state dikhane ke liye `openWebview()` ka istemal karein. Agar aapko interactive features ki zarurat hai, to unhe sidebar buttons se trigger karein aur webview ko dobara khol kar display refresh karein.

### Pattern: Terminal Command → Parse Output → HTML mein dikhana

Ye sab se aam plugin pattern hai. Aap ek command chalayenge, result ko parse karenge, aur ise visual tor par dikhayenge.
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
### Pattern: Webview mein Settings dikhana

> **Note:** Webviews sirf display-only hain — ye plugin APIs ko call nahi kar sakte. Settings ko modify karne ke liye apne sidebar button handlers mein `ctx.settings` ka istemal karein, aur current state dikhane ke liye `openWebview()` ka istemal karein.
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

## Part 4: Apne Plugin ko Publish karna

### Step 1: Local par test karein

1. Apne plugin ko `~/.wia-soom/plugins/{your-plugin}/` mein copy karein
2. WIA SOOM ko dobara shuru karein
3. Yeh verify karein ke yeh kaam karta hai: sidebar button nazar aata hai, features sahi kaam karte hain
4. Edge cases ko test karein: agar koi terminal connected nahi hai to kya hota hai?

### Step 2: Submission ke liye tayar karein

Aapke plugin folder mein ye hona chahiye:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**ضروری `package.json` فیلڈز:**

| فیلڈ | وضاحت | مثال |
|-------|-------------|---------|
| `name` | منفرد kebab-case ID | `"my-awesome-plugin"` |
| `version` | سمیٹک ورژن | `"1.0.0"` |
| `description` | ایک لائن کی وضاحت | `"Monitors nginx access logs in real-time"` |
| `author` | آپ کا نام | `"John Doe"` |
| `main` | انٹری پوائنٹ | `"index.js"` |

**اختیاری فیلڈز:**

| فیلڈ | وضاحت |
|-------|-------------|
| `license` | لائسنس کی قسم (MIT کی سفارش کی جاتی ہے) |
| `keywords` | تلاش کے ٹیگ کی ایک صف |
| `soom.minVersion` | درکار کم از کم WIA SOOM ورژن |

### مرحلہ 3: پلگ ان رجسٹری میں جمع کروائیں

1. ****Package** your plugin as a ZIP file
2. **Add** اپنے پلگ ان کو `plugins/{your-plugin-name}/` میں
3. **Submit** ایک Pull Request

### مرحلہ 4: جائزہ اور منظوری

ہم ہر پلگ ان کا ��ائزہ لیتے ہیں:

- **سیکیورٹی** — کوئی خطرناک APIs نہیں (دیکھیں [سیکیورٹی کے قواعد](#security-rules))
- **معیار** — کیا یہ کام کرتا ہے؟ کیا کوڈ صاف ہے؟
- **استعمال کی افادیت** — کیا یہ حقیقی مسئلے کو حل کرتا ہے؟

منظوری کے بعد:
1. آپ کا پلگ ان `registry.json` میں شامل کیا جاتا ہے
2. `dist/` میں ایک ZIP بنڈل بنایا جاتا ہے
3. آپ کا پلگ ان تمام WIA SOOM صارفین کے لیے **Plugin Store** میں ظاہر ہوتا ہے!

---

## حصہ 5: بہترین طریقے

### سیکیورٹی کے قواعد

یہ قواعد **لازمی** ہیں۔ جو پلگ ان ان کی خلاف ورزی کریں گے انہیں مسترد کر دیا جائے گا۔

| قاعدہ | کیوں |
|------|-----|
| **کبھی بھی** `eval()` یا `new Function()` کا استعمال نہ کریں | کوڈ انجیکشن کا خطرہ |
| **کبھی بھی** `child_process`, `exec()`, `spawn()` کا استعمال نہ کریں | صرف `ctx.terminal.send()` کو کمانڈز کے لیے استعمال کریں |
| **کبھی بھی** بیرونی URLs کو حاصل نہ کریں | استثنا: `wiasoom.com` API endpoints |
| **کبھی بھی** `process.env` تک رسائی نہ کریں | ماحولیاتی متغیرات میں راز ہو سکتے ہیں |
| **کبھی بھی** `require('fs')` کو براہ راست استعمال نہ کریں | اسٹوریج کے لیے `ctx.settings` کا استعمال کریں، فائل کی منتقلی کے لیے `ctx.sftp` کا استعمال کریں |
| **کبھی بھی** npm کے بیرونی پیکجز کا استعمال نہ کریں | صرف خالص جاوا اسکرپٹ — کوئی node_modules نہیں |
| **ضروری** ہے کہ تمام دور دراز کمانڈز کے لیے `ctx.terminal.send()` کا استعمال کریں | یہ محفوظ SSH چینل کے ذریعے جاتا ہے |
| **ضروری** ہے کہ `deactivate()` میں صفائی کریں | سننے والوں کو ہٹا دیں، وقفے صاف کریں |

### غلطی کی ہینڈلنگ

ہمیشہ خطرناک کارروائیوں کو try/catch میں لپیٹیں:
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
### deactivate() میں صفائی

اگر آپ کا پلگ ان وقفے، سننے والے، یا سبسکرپشنز بناتا ہے — انہیں صاف کریں:
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
### i18n سپورٹ

WIA SOOM 254 زبان��ں کی حمایت کرتا ہے۔ اپنے پلگ ان کے لیبل کو ترجمہ کے قابل بنانے کے لیے، ایک سادہ طریقہ استعمال کریں:
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

## حصہ 6: حقیقی دنیا کی مثالیں

### مثال 1: سرور ڈسک چیکر

دور دراز کے سرور پر `df -h` چلاتا ہے اور حیثیت بار میں استعمال شدہ/دستیاب جگہ دکھاتا ہے۔
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

### مثال 2: TODO منیجر

ایک پلگ ان جو سیٹنگز کا استعمال کرتے ہوئے TODO فہرست کا انتظام کرتا ہے تاکہ مستقل اسٹوریج اور ڈسپلے کے لیے ایک ویب ویو ہو۔

> **ڈیزائن پیٹرن:** چونکہ ویب ویوز براہ راست پلگ ان APIs کو کال نہیں کر سکتے، یہ پلگ ان "snapshot" نقطہ نظر کا استعمال کرتا ہے — یہ سیٹنگز سے TODOs کو پڑھتا ہے، انہیں صرف پڑھنے کے قابل HTML کے طور پر پیش کرتا ہے، اور اشیاء شامل کرنے کے لیے سائیڈبار پر مبنی کارروائیاں فراہم کرتا ہے۔ ویب ویو ایک **ڈسپلے** پرت ہے، نہ کہ ایک تعاملاتی فارم۔
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

### مثال 3: غلطی کا نگران

ٹرمنل کی آؤٹ پٹ کی نگرانی کرتا ہے اور مخصوص پیٹرن کی شناخت پر ایک اطلاع بھیجتا ہے۔
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

## ضمیمه: دسته‌ها و آیکون‌ها

### دسته‌های پلاگین (۲۹)

از این‌ها در `package.json` `keywords` یا هنگام ارسال به رجیستری استفاده کنید:

| دسته | توضیحات |
|------|---------|
| `server` | مدیریت کلی سرور |
| `devtools` | ابزارهای توسعه |
| `calculator` | ماشین‌حساب‌ها و تبدیل‌کننده‌ها |
| `simulator` | شبیه‌سازها |
| `game` | بازی‌های ترمینالی |
| `business` | ابزارهای کسب و کار |
| `security` | امنیت و حسابرسی |
| `web` | مدیریت سرور وب |
| `education` | ابزارهای آموزشی |
| `health` | ابزارهای مرتبط با سلامت |
| `islamic` | ابزارهای اسلامی (زمان‌های نماز و غیره) |
| `science` | ابزارهای علمی |
| `quantum` | ابزارهای محاسبات کوانتومی |
| `ai` | ابزارهای مبتنی بر هوش مصنوعی |
| `biotech` | ابزارهای ب��وتکنولوژی |
| `space` | ابزارهای فضایی و نجوم |
| `network` | ابزارهای شبکه |
| `database` | مدیریت پایگاه داده |
| `monitoring` | نظارت بر سرور |
| `devops` | DevOps و CI/CD |
| `utility` | ابزارهای عمومی |
| `design` | ابزارهای طراحی |
| `ecommerce` | ابزارهای تجارت الکترونیک |
| `automation` | ابزارهای اتوماسیون |
| `kpop` | ابزارهای مرتبط با K-pop |
| `accessibility` | ابزارهای دسترسی |
| `analytics` | تجزیه و تحلیل و گزارش‌دهی |
| `wia` | ابزارهای اکوسیستم WIA |
| `all` | در تمام دسته‌ها ظاهر می‌شود |

### آیکون‌های پیشنهادی (Lucide)

| نام آیکون | استفاده برای |
|-----------|---------------|
| `server` | مدیریت سرور |
| `shield` | امنیت |
| `database` | پایگاه داده |
| `activity` | نظارت |
| `terminal` | ابزارهای ترمینال |
| `code` | توسعه |
| `hard-drive` | دیسک/ذخیره‌سازی |
| `network` | شبکه‌سازی |
| `lock` | احراز هویت/رمزنگاری |
| `eye` | مشاهده/نظارت |
| `check-square` | وظایف/TODO |
| `layout-dashboard` | داشبوردها |
| `settings` | پیکربندی |
| `zap` | اتوماسیون |
| `globe` | وب/بین‌المللی |

تمام ۱,۵۰۰+ آیکون را مرور کنید: [lucide.dev/icons](https://lucide.dev/icons)

---

## نیاز به کمک دارید؟

- **مسائل GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **مسائل پلاگین:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **پلاگین‌های نمونه:** [Website](https://wiasoom.com)
- **وب‌سایت:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>چیزی شگفت‌انگیز بسازید. آن را با دنیا به اشتراک بگذارید.</em></p>
<p align="center"><em>— تیم WIA SOOM</em></p>