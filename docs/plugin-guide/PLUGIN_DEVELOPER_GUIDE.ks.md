<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM پلگن ڈویلپر گائیڈ</h1>
<p align="center"><strong>5 منٹ میں اپنا پلگن بنائیں۔</strong></p>
<p align="center">طاقتور سرور ٹولز، ڈیش بورڈز، اور خودکاریاں بنائیں — بالکل WIA SOOM کے اندر۔</p>

---

## مواد کی فہرست

- [حصہ 1: فوری آغاز — 5 منٹ میں آپ کا پہلا پلگن](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [حصہ 2: پلگن سیاق API حوالہ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [حصہ 3: ویب ویوز کے ساتھ حسب ضرورت UI بنانا](#part-3-building-custom-ui-with-webviews)
- [حصہ 4: اپنے پلگن کی اشاعت](#part-4-publishing-your-plugin)
- [حصہ 5: بہترین طریقے](#part-5-best-practices)
- [حصہ 6: حقیقی دنیا کی مثالیں](#part-6-real-world-examples)
- [ضمیمہ: زمرے اور آئیکن](#appendix-categories--icons)

---

## حصہ 1: فوری آغاز — 5 منٹ میں آپ کا پہلا پلگن

### آپ کیا بنائیں گے

ایک "ہیلو ورلڈ" پلگن جو سائیڈبار میں ایک بٹن شامل کرتا ہے۔ جب اس پر کلک کیا جائے تو یہ ایک اطلاع دکھاتا ہے۔

### مرحلہ 1: پلگن فولڈر بنائیں
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### مرحلہ 2: package.json بنائیں
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
**ضروری فیلڈز:** `name`, `version`, `description`, `author`, `main`

### مرحلہ 3: index.js بنائیں
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
### مرحلہ 4: WIA SOOM کو دوبارہ شروع کریں

ایپ کو دوبارہ شروع کریں (یا سیٹنگز → پلگنز میں پلگن کو بند/چالو کریں)۔

آپ کو سائیڈبار میں ایک **"ہیلو ورلڈ"** بٹن نظر آئے گا۔ اس پر کلک کریں — آپ کو ایک کامیابی کی اطلاع نظر آئے گی!

### یہ کیسے کام کرتا ہے
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

## حصہ 2: پلگن سیاق API حوالہ

جب آپ کا `activate(context)` فنکشن کال کیا جاتا ہے، تو `context` (یا `ctx`) یہ APIs فرا��م کرتا ہے:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — ریموٹ سرورز پر کمانڈز چلائیں

#### `terminal.send(sessionId, data)`

ایک کمانڈ (یا کوئی بھی ڈیٹا) ایک فعال ٹرمینل سیشن میں بھیجیں۔

| پیرامیٹر | قسم | وضاحت |
|-----------|------|-------------|
| `sessionId` | `string` | جس ٹرمینل سیشن میں بھیجنا ہے |
| `data` | `string` | جس کمانڈ یا ڈیٹا کو بھیجنا ہے |
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
#### `terminal.onOutput(sessionId, callback)`

ایک ٹرمینل سیشن سے تمام آؤٹ پٹ کے لیے سبسکرائب کریں۔ ایک **ان سبسکرائب فنکشن** واپس کرتا ہے۔

| پیرامیٹر | قسم | وضاحت |
|-----------|------|-------------|
| `sessionId` | `string` | جس ٹرمینل سیشن کو دیکھنا ہے |
| `callback` | `(data: string) => void` | ہر آؤٹ پٹ کے ٹکڑے کے ساتھ کال کیا جاتا ہے |
| **واپس کرتا ہے** | `() => void` | ��ننا بند کرنے ک�� لیے یہ کال کریں |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**اہم:** ہمیشہ ان سبسکرائب فنکشن کو محفوظ کریں اور `deactivate()` میں اسے کال کریں تاکہ میموری کے نقصان سے بچا جا سکے۔

---

### `ctx.sftp` — فائل کی منتقلی

> **حالت: جلد آرہا ہے** — SFTP API کی وضاحت کی گئی ہے لیکن ابھی ایپ کے SFTP انجن سے منسلک نہیں ہے۔ `list()` فی الحال ایک خالی صف واپس کرتا ہے، اور `upload()`/`download()` کوئی عمل نہیں کرتے۔ یہ مستقبل میں کسی ریلیز میں مکمل طور پر نافذ کیا جائے گا۔ اس وقت، `scp` یا `rsync` کمانڈز کے ساتھ `ctx.terminal.send()` کا استعمال کریں۔

#### `sftp.list(sessionId, path)`

ایک ریموٹ ڈائریکٹری میں فائلیں درج کریں۔
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
#### `sftp.upload(sessionId, localPath, remotePath)`

مقامی مشین سے ریموٹ سرور پر ایک فائل اپ لوڈ کریں۔
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

ریموٹ سرور سے مقامی مشین پر ایک فائل ڈاؤن لوڈ کریں۔
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**متبادل حل (جب تک SFTP API فعال نہ ہو):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — صارف کا انٹرفیس

#### `ui.addSidebarButton(options)`

WIA SOOM سائیڈبار میں ایک بٹن شامل کریں۔

| آپشن | قسم | ضروری | وضاحت |
|--------|------|----------|-------------|
| `id` | `string` | نہیں | منفرد ID (ڈیفالٹ پلگن کے نام پر) |
| `icon` | `string` | جی ہاں | Lucide آئیکن کا نام (جیسے، `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | جی ہاں | سائیڈبار میں دکھایا جانے والا بٹن کا متن |
| `onClick` | `() => void` | جی ہاں | جب بٹن پر کلک کیا جائے تو کال ہونے والا فنکشن |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**آئیکن حوالہ:** دستیاب تمام آئیکنز کو [lucide.dev/icons](https://lucide.dev/icons) پر براؤز کریں

> **ہم آہنگی کا نوٹ:** کچھ پرانے پلگنز پوزیشنل آرگومنٹس جیسے `addSidebarButton(id, icon, label, onClick)` استعمال کرتے ہیں۔ سرکاری API اوپر بیان کردہ **آپشنز آبجیکٹ** کا استعمال کرتی ہے۔ نئے پلگنز کے لیے ہمیشہ آبجیکٹ طرز استعمال کری��۔

#### `ui.openWebview(options)`

حسب ضرورت HTML مواد کے ساتھ ایک پاپ اپ ونڈو کھولیں۔ یہ آپ کو بھرپور UI بنانے کا طریقہ ہے۔

| آپشن | قسم | وضاحت |
|--------|------|-------------|
| `title` | `string` | ونڈو کا عنوان |
| `html` | `string` | مکمل HTML مواد جو رینڈر کرنا ہے |
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
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

Toast notification chhukh dikhawaan.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Dikhawaan text |
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
#### `ui.addStatusBarItem(id, text)`

Bottom status bar chhukh ek persistent text item add karun.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Ahn status item kyah unique ID |
| `text` | `string` | Dikhawaan text |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Persistent storage

Plugin settings chhukh permanently store karun `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Saved value padhun.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Agar key na chhukh, `undefined` return karun.

#### `settings.set(key, value)`

Value save karun. Strings, numbers, booleans, arrays, aawz objects support karun.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Example: User preferences yaad rakhun**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — AI integration

> **Status: Coming Soon** — AI API defined chhukh, par Soomy sath connect na chhukh. Halaan `{ response: 'AI not yet connected' }` return karun. Full AI integration future release kyah plan chhukh.

#### `ai.chat(messages, options?)`

AI assistant (Soomy) sath messages send karun.
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

## Part 3: Building Custom UI with Webviews

`openWebview()` API chhukh tuhi dashboard UIs HTML, CSS, aawz JavaScript sath banawaan — sab popup window andar.

> **Important limitation:** Webviews chhukh **display-only**. Yim plugin APIs (`ctx.settings`, `ctx.terminal`, etc.) sath call na karun. Sab user actions kyah sidebar buttons use karun, aawz current state dikhawaan kyah `openWebview()` use karun. Agar interactive features chhukh zaroorat, sidebar buttons sath trigger karun aawz webview dobara kholun display refresh karun.

### Pattern: Terminal Command → Parse Output → Show in HTML

Yim sab se aam plugin pattern chhukh. Tuhi command run karun, result parse karun, aawz visually dikhawaan.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Pattern: Interactive Dashboard with Auto-Refresh
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
### Pattern: Displaying Settings in a Webview

> **Note:** Webviews chhukh display-only — yim plugin APIs sath call na karun. `ctx.settings` tuhi sidebar button handlers andar use karun settings modify karun, aawz current state dikhawaan kyah `openWebview()` use karun.
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
---

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Tuhi plugin copy karun `~/.wia-soom/plugins/{your-plugin}/`
2. WIA SOOM restart karun
3. Verify karun chhukh kaam karun: sidebar button appear karun, features sahi kaam karun
4. Edge cases test karun: agar koi terminal connected na chhukh, kya hunda?

### Step 2: Prepare for submission

Tuhi plugin folder andar chhukh:
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
| `license` | لائسنس کی قسم (MIT تجویز کردہ) |
| `keywords` | تلاش کے ٹیگ کی صف |
| `soom.minVersion` | درکار کم از کم WIA SOOM ورژن |

### مرحلہ 3: پلگ ان رجسٹری میں جمع کروائیں

1. ****Package** your plugin as a ZIP file
2. **Add** اپنا پلگ ان `plugins/{your-plugin-name}/` میں
3. **Submit** ایک Pull Request

### مرحلہ 4: جائزہ اور منظوری

ہم ہر پلگ ان کا جائزہ لیتے ہیں:

- **سیکیورٹی** — کوئی خطرناک APIs نہیں (دیکھیں [Security Rules](#security-rules))
- **معیار** — کیا یہ کام کرتا ہے؟ کیا کوڈ صاف ہے؟
- **استعمالیت** — کیا یہ ایک حقیقی مسئلہ حل کرتا ہے؟

منظوری کے بعد:
1. آپ کا پلگ ان `registry.json` میں شامل کیا جاتا ہے
2. `dist/` میں ایک ZIP بنڈل بنایا جاتا ہے
3. آپ کا پلگ ان **Plugin Store** میں تمام WIA SOOM صارفین کے لیے ظاہر ہوتا ہے!

---

## حصہ 5: بہترین طریقے

### سیکیورٹی کے اصول

یہ اصول **لازمی** ہیں۔ جو پلگ ان ان کی خلاف ورزی کریں گے انہیں مسترد کر دیا جائے گا۔

| اصول | کیوں |
|------|-----|
| **کبھی بھی** `eval()` یا `new Function()` کا استعمال نہ کریں | کوڈ انجیکشن کا خطرہ |
| **کبھی بھی** `child_process`, `exec()`, `spawn()` کا استعمال نہ کریں | صرف `ctx.terminal.send()` کا استعمال کریں کمانڈز کے لیے |
| **کبھی بھی** بیرونی URLs کو fetch نہ کریں | استثنا: `wiasoom.com` API endpoints |
| **کبھی بھی** `process.env` تک رسائی نہ کریں | ماحولیاتی متغیرات میں راز ہو سکتے ہیں |
| **کبھی بھی** `require('fs')` کو براہ راست استعمال نہ کریں | اسٹوریج کے لیے `ctx.settings` کا استعمال کریں، فائل کی منتقلی کے لیے `ctx.sftp` |
| **کبھی بھی** npm بیرونی پیکجز کا استعمال نہ کریں | صرف خالص جاوا اسکرپٹ — کوئی node_modules نہیں |
| **ضروری** ہے کہ تمام دور دراز کمانڈز کے لیے `ctx.terminal.send()` کا استعمال کریں | یہ محفوظ SSH چینل کے ذریعے جاتا ہے |
| **ضروری** ہے کہ `deactivate()` میں صفائی کریں | سننے والوں کو ہٹا دیں، وقفے صاف کریں |

### غلطی کی ہینڈلنگ

ہمیشہ خطرناک کارروائیوں کو try/catch میں لپیٹیں:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### deactivate() میں صفائی

اگر آپ کا پلگ ان وقفے، سننے والے، یا سبسکرپشنز بناتا ہے — انہیں صاف کریں:
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
### i18n سپورٹ

WIA SOOM 254 زبانوں کی حمایت کرتا ہے۔ اپنے پلگ ان کے لیبل کو ترجمہ کے قابل بنانے کے لیے، ایک سادہ طریقہ استعمال کریں:
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
---

## حصہ 6: حقیقی دنیا کی مثالیں

### مثال 1: سرور ڈسک چیکر

دور دراز سرور پر `df -h` چلاتا ہے اور حیثیت بار میں استعمال شدہ/دستیاب جگہ دکھاتا ہے۔
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

### مثال 2: TODO منیجر

ایک پلگ ان جو ایک TODO فہرست کا انتظام کرتا ہے جو مستقل اسٹوریج کے لیے سیٹنگز اور ڈسپلے کے لیے ایک ویب ویو کا استعمال کرتا ہے۔

> **ڈیزائن پیٹرن:** چونکہ ویب ویوز براہ راست پلگ ان APIs کو کال نہیں کر سکتے، یہ پلگ ان "snapshot" نقطہ نظر کا استعمال کرتا ہے — یہ سیٹنگز سے TODOs کو پڑھتا ہے، انہیں پڑھنے کے قابل HTML کے طور پر رینڈر کرتا ہے، اور اشیاء شامل کرنے کے لیے سائیڈبار پر مبنی کارروائیاں فراہم کرتا ہے۔ ویب ویو ایک **ڈسپلے** پرت ہے، نہ کہ ایک تعاملاتی فارم۔
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

### مثال 3: غلطی کا نگران

ٹرمینل آؤٹ پٹ کی نگرانی کرتا ہے اور جب مخصوص پیٹرن کا پتہ چلتا ہے تو ایک اطلاع بھیجتا ہے۔
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

## ضمیمہ: زمرے اور آئیکن

### پلگ ان زمرے (29)

انھیں اپنے `package.json` `keywords` میں یا رجسٹری میں جمع کراتے وقت استعمال کریں:

| زمرہ | وضاحت |
|----------|-------------|
| `server` | عمومی سرور انتظام |
| `devtools` | ترقیاتی ٹولز |
| `calculator` | کیلکولیٹر اور کنورٹر |
| `simulator` | سمیولیٹر |
| `game` | ٹرمینل کھیل |
| `business` | کاروباری ٹولز |
| `security` | سیکیورٹی اور آڈٹنگ |
| `web` | ویب سرور انتظام |
| `education` | تعلیمی ٹولز |
| `health` | صحت سے متعلق ٹولز |
| `islamic` | اسلامی ٹولز (نماز کے اوقات، وغیرہ) |
| `science` | سائنسی ٹولز |
| `quantum` | کوانٹم کمپیوٹنگ ٹولز |
| `ai` | AI سے چلنے والے ٹولز |
| `biotech` | بایوٹیکنالوجی ٹولز |
| `space` | خلا اور فلکیات کے ٹولز |
| `network` | نیٹ ورک ٹولز |
| `database` | ڈیٹا بیس کا انتظام |
| `monitoring` | سرور کی نگرانی |
| `devops` | DevOps اور CI/CD |
| `utility` | عمومی افادیت |
| `design` | ڈیزائن ٹولز |
| `ecommerce` | ای کامرس ٹولز |
| `automation` | خودکار ٹولز |
| `kpop` | K-pop سے متعلق ٹولز |
| `accessibility` | رسائی کے ٹولز |
| `analytics` | تجزیات اور رپورٹنگ |
| `wia` | WIA ماحولیاتی ٹولز |
| `all` | تمام زمرے میں ظاہر ہوتا ہے |

### تجویز کردہ آئیکن (Lucide)

| آئیکن کا نام | استعمال کے لیے |
|-----------|---------|
| `server` | سرور کا انتظام |
| `shield` | سیکیورٹی |
| `database` | ڈیٹا بیس |
| `activity` | نگرانی |
| `terminal` | ٹرمینل ٹولز |
| `code` | ترقی |
| `hard-drive` | ڈسک/ذخیرہ |
| `network` | نیٹ ورکنگ |
| `lock` | توثیق/انکرپشن |
| `eye` | دیکھنا/نگرانی |
| `check-square` | کام/TODO |
| `layout-dashboard` | ڈیش بورڈز |
| `settings` | تشکیل |
| `zap` | خودکار |
| `globe` | ویب/بین الاقوامی |

تمام 1,500+ آئیکن براؤز کریں: [lucide.dev/icons](https://lucide.dev/icons)

---

## مدد کی ضرورت ہے؟

- **GitHub مسائل:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **پلگ ان مسائل:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **مثالی پلگ ان:** [Website](https://wiasoom.com)
- **ویب سائٹ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>کچھ حیرت انگیز بنائیں۔ اسے دنیا کے ساتھ شیئر کریں۔</em></p>
<p align="center"><em>— WIA SOOM ٹیم</em></p>
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
