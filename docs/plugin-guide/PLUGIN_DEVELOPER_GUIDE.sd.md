<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM پلگ ان ڊولپر گائيڊ</h1>
<p align="center"><strong>پنهنجو پنهنجو پلگ ان 5 منٽن ۾ ٺاهيو.</strong></p>
<p align="center">طاقتور سرور ٽولز، ڊيش بورڊ، ۽ خودڪار عمل ٺاهيو — سڌو WIA SOOM ۾.</p>

---

## مواد جي فهرست

- [حصو 1: جلدي شروعات — توهان جو پهريون پلگ ان 5 منٽن ۾](#حصو-1-جلدي-شروعات--توهان-جو-پهريون-پلگ-ان-5-منٽن-۾)
- [حصو 2: پلگ ان ڪنٽيڪسٽ API حوالو](#حصو-2-پلگ-ان-ڪنٽيڪسٽ-api-حوالو)
  - [ctx.terminal](#ctxterminal--remote-servers-تي-حکم-هلائڻ)
  - [ctx.sftp](#ctxsftp--فائل-منتقلي)
  - [ctx.ui](#ctxui--صارف-انٽرفيس)
  - [ctx.settings](#ctxsettings--پائيدار-ذخيره)
  - [ctx.ai](#ctxai--ai-انٽيگريشن)
- [حصو 3: ويب ويون سان حسب ضرورت UI ٺاهڻ](#حصو-3-ويب-ويون-سان-حسب-ضرورت-ui-ت ٺاهڻ)
- [حصو 4: توهان جو پلگ ان شايع ڪرڻ](#حصو-4-توهان-جو-پلگ-ان-شايع-ڪرڻ)
- [حصو 5: بهترين طريقيڪار](#حصو-5-بهترين-طريقيڪار)
- [ضميمه: ڪيٽيگريون ۽ آئيڪن](#ضميمه-ڪيٽيگريون--آئيڪن)

---

## حصو 1: جلدي شروعات — توهان جو پهريون پلگ ان 5 منٽن ۾

### توهان ڇا ٺاهيندؤ

هڪ "Hello World" پلگ ان جيڪو سائڊبار ۾ هڪ بٽڻ شامل ڪري ٿو. جڏهن ڪلڪ ڪيو ويندو، اهو هڪ نوٽيفڪيشن ڏيکاري ٿو.

### قدم 1: پلگ ان فولڊر ٺاهيو
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### قدم 2: package.json ٺاهيو
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
**ضروري فيلڊز:** `name`, `version`, `description`, `author`, `main`

### قدم 3: index.js ٺاهيو
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
### قدم 4: WIA SOOM کي ٻيهر شروع ڪريو

ايپ کي ٻيهر شروع ڪريو (يا سيٽنگز → پلگ ان ۾ پلگ ان کي بند/چالو ڪريو).

توهان کي سائڊبار ۾ هڪ **"Hello World"** بٽڻ نظر ايندو. ان تي ڪلڪ ڪريو — توهان هڪ ڪاميابي ��ي نوٽيفڪيشن ڏسندا!

### اهو ڪيئن ڪم ڪري ٿو
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

## حصو 2: پلگ ان ڪنٽيڪسٽ API حوالو

جڏهن توهان جو `activate(context)` فنڪشن سڏيو ويندو آهي، `context` (يا `ctx`) هي API مهيا ڪري ٿو:
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

### `ctx.terminal` — ريموٽ سرورز تي حڪم هلائڻ

#### `terminal.send(sessionId, data)`

هڪ حڪم (يا ڪنهن به ڊيٽا) کي هڪ فعال ٽرمينل سيشن ڏانهن موڪلڻ.

| پيراميٽر | قسم | وضاحت |
|-----------|------|-------------|
| `sessionId` | `string` | ٽرمينل سيشن جنهن ڏانهن موڪلڻو آهي |
| `data` | `string` | حڪم يا ڊيٽا جيڪا موڪلڻي آهي |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

هڪ ٽرمينل سيشن مان سڀني آئوٽ پٽ لاءِ سبسڪرائب ڪريو. هڪ **ان سبسڪرائب فنڪشن** موٽائي ٿو.

| پيراميٽر | قسم | وضاحت |
|-----------|------|-------------|
| `sessionId` | `string` | ٽرمينل سيشن جنهن کي ڏسڻ لاءِ |
| `callback` | `(data: string) => void` | هر آئوٽ پٽ جي چنڪ سان سڏيو ويند�� |
| **موٽائي ٿو** | `() => void` | ان کي بند ڪرڻ لاءِ سڏيو |
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
**مهم:** هميشه ان سبسڪرائب فنڪشن کي محفوظ ڪريو ۽ `deactivate()` ۾ ان کي سڏيو ته جيئن ميموري جي ليڪ کان بچي سگهجي.

---

### `ctx.sftp` — فائل منتقلي

> **حالت: جلد اچي رهيو آهي** — SFTP API بيان ڪيو ويو آهي پر اڃا تائين ايپ جي SFTP انجن سان ڳنڍيل ناهي. `list()` هن وقت هڪ خالي ايري موٽائي ٿو، ۽ `upload()`/`download()` ڪو عمل نه آهي. اهو مستقبل جي جاري ٿيڻ ۾ مڪمل طور تي لاڳو ڪيو ويندو. هن وقت، `scp` يا `rsync` حڪمن سان `ctx.terminal.send()` استعمال ڪريو.

#### `sftp.list(sessionId, path)`

هڪ ريموٽ ڊائريڪٽري ۾ فائلون لسٽ ڪريو.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

مقامي مشين مان ريموٽ سرور ڏانهن هڪ فائل اپ لوڊ ڪريو.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

ريمٽ سرور مان مقامي مشين ڏانهن هڪ فائل ڊائون لوڊ ڪريو.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**حل (جيستائين SFTP API فعال نه ٿئي):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — صارف انٽرفيس

#### `ui.addSidebarButton(options)`

WIA SOOM سائڊبار ۾ هڪ بٽڻ شامل ڪريو.

| اختيار | قسم | ضروري | وضاحت |
|--------|------|----------|-------------|
| `id` | `string` | نه | منفرد ID (پلگ ان جي نالي تي ڊيفالٽ) |
| `icon` | `string` | ها | Lucide آئيڪن جو نالو (مثال طور، `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ها | سائڊبار ۾ ڏيکاريل بٽڻ جو متن |
| `onClick` | `() => void` | ها | فنڪشن جيڪو بٽڻ تي ڪلڪ ڪرڻ تي سڏيو ويندو |
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
**آئيڪن جو حوالو:** موجوده آئيڪن کي [lucide.dev/icons](https://lucide.dev/icons) تي براؤز ڪريو

> **مطابقت نوٽ:** ڪجهه پراڻا پلگ ان پوزيشنل آرگومينٽس استعمال ڪن ٿا جهڙوڪ `addSidebarButton(id, icon, label, onClick)`. سرڪاري API هڪ **اختيارات جي شيء** کي استعمال ڪري ٿو جيئن مٿي بيان ڪيو ويو آهي. هميشه نون پلگ ان لاءِ شيء جي طرز استعمال ڪريو.

#### `ui.openWebview(options)`

حسب ضرورت HTML مواد سان هڪ پاپ اپ ونڊو کوليو. هي طريقيڪار توهان کي رچ UI ٺاهڻ ۾ مدد ڪري ٿو.

| اختيار | قسم | وضاحت |
|--------|------|-------------|
| `title` | `string` | ونڊو جو عنوان |
| `html` | `string` | مڪمل HTML مواد جيڪو رينڊر ڪرڻ لاءِ |
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
> ڏسو [حصو 3](#part-3-building-custom-ui-with-webviews) جديد ويب ويون نمونن لاءِ.

#### `ui.showNotification(type, message)`

هڪ ٽوسٽ نوٽيفڪيشن ڏيکاريو.

| پيرا ميٽر | قسم | وضاحت |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | نوٽيفڪيشن جو انداز |
| `message` | `string` | ڏيکارڻ لاءِ متن |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

هيٺين اسٽيٽس بار ۾ هڪ مستقل متن جو عنصر شامل ڪريو.

| پيرا ميٽر | قسم | وضاحت |
|-----------|------|-------------|
| `id` | `string` | هن اسٽيٽس عنصر لاءِ منفرد ID |
| `text` | `string` | ڏيکارڻ لاءِ متن |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — مستقل اسٽوريج

پلاگ ان جون سيٽنگون مستقل طور تي `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ۾ محفوظ ڪيون ويون آهن.

#### `settings.get(key)`

هڪ محفوظ ڪيل قدر پڙهو.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
جيڪڏهن چابي موجود نه هجي ته `undefined` موٽائي ٿو.

#### `settings.set(key, value)`

هڪ قدر محفوظ ڪريو. تارون، نمبر، بو��ين، ايري، ۽ شيون سپورٽ ڪري ٿو.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**مثال: صارف جي ترجيحن کي ياد رکڻ**
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

### `ctx.ai` — AI انضمام

> **حالت: جلد اچي رهي آهي** — AI API بيان ٿيل آهي پر اڃا تائين Soomy سان ڳنڍيل ناهي. موجوده طور تي `{ response: 'AI not yet connected' }` موٽائي ٿو. مڪمل AI انضمام مستقبل جي جاري ٿيڻ لاءِ منصوبه بندي ڪئي وئي آهي.

#### `ai.chat(messages, options?)`

AI اسسٽنٽ (Soomy) کي پيغام موڪليو.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## حصو 3: ويب ويون سان حسب ضرورت UI ٺاهڻ

`openWebview()` API توهان کي HTML، CSS، ۽ JavaScript سان ڊيش بورڊ UI ٺاهڻ جي اجازت ڏئي ٿي — سڀ ڪجهه هڪ پاپ اپ ونڊو ۾.

> **مهم ترين حد:** ويب ويون **صرف ڏيکارڻ لاءِ** آهن. اهي پلاگ ان APIs (`ctx.settings`, `ctx.terminal`, وغيره) ۾ واپس ڪال نه ڪري سگهن. سڀني صارف جي عملن لاءِ سائڊبار بٽڻ استعمال ڪريو، ۽ موجوده حالت ڏيکارڻ لاءِ `openWebview()` استعمال ڪريو. جيڪڏهن توهان کي انٽرايڪٽو خاصيتون گهربل آهن، انهن کي سائڊبار بٽڻ مان متحرڪ ڪريو ۽ ڏيک کي تازه ترين ڪرڻ لاءِ ويب ويون ٻيهر کوليو.

### نمونو: ٽرمينل حڪم → آؤٽ پٽ کي پارس ڪريو → HTML ۾ ڏيکاريو

هي سڀ کان عام پلاگ ان نمونو آهي. توهان هڪ حڪم هلائيندا آهيو، نتيجو پارس ڪندا آهيو، ۽ ان کي بصري طور تي ڏيکاريندا آهيو.
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
### نمونو: خودڪار تازه ترين سان انٽرايڪٽو ڊيش بورڊ
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
### نمونو: ويب ويون ۾ سيٽنگون ڏيکارڻ

> **نوٽ:** ويب ويون صرف ڏيکارڻ لاءِ آهن — اهي پلاگ ان APIs ۾ واپس ڪال نه ڪري سگهن. سيٽنگون تبديل ڪرڻ لاءِ پنهنجي سائڊبار بٽڻ هينڊلرز ۾ `ctx.settings` استعمال ڪريو، ۽ موجوده حالت ڏيکارڻ لاءِ `openWebview()` استعمال ڪريو.
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

## حصو 4: پنهنجي پلاگ ان کي شايع ڪرڻ

### قدم 1: مقامي طور تي جانچ ڪريو

1. پنهنجي پلاگ ان کي `~/.wia-soom/plugins/{your-plugin}/` ۾ ڪاپي ڪريو
2. WIA SOOM کي ٻيهر شروع ڪريو
3. تصديق ڪريو ته اهو ڪم ڪري ٿو: سائڊبار بٽڻ ظاهر ٿئي ٿو، خاصيتون صحيح طريقي سان ڪم ڪن ٿيون
4. ڪنٽراٽ ڪيسن کي جانچيو: جيڪڏهن ڪو ٽرمينل ڳنڍيل نه آهي ته ڇا ٿيندو؟

### قدم 2: جمع ڪرڻ لاءِ تيار ڪريو

توهان جي پلاگ ان جي فولڊر ۾ شامل هجڻ گهرجي:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**گهربل `package.json` فيلڊز:**

| فيلڊ | وضاحت | مثال |
|-------|-------------|---------|
| `name` | منفرد kebab-case ID | `"my-awesome-plugin"` |
| `version` | سيمانٽڪ ورجن | `"1.0.0"` |
| `description` | هڪ لائن جي وضاحت | `"Monitors nginx access logs in real-time"` |
| `author` | توهانجو نالو | `"John Doe"` |
| `main` | داخلا پوائنٽ | `"index.js"` |

**اختياري فيلڊز:**

| فيلڊ | وضاحت |
|-------|-------------|
| `license` | لائسنس جو قسم (MIT سفارش ڪيل) |
| `keywords` | ڳولا جي ٽيگ جو صف |
| `soom.minVersion` | گھٽ ۾ گھٽ WIA SOOM ورجن جي ضرورت |

### قدم 3: پلگ ان ريگسٽري ۾ جمع ڪرائڻ

1. ****Package** your plugin as a ZIP file
2. **Add** پنهنجو پلگ ان `plugins/{your-plugin-name}/` ۾
3. **Submit** هڪ Pull Request

### قدم 4: جائزو ۽ منظوري

اسان هر پلگ ان جو جائزو وٺون ٿا:

- **سڪيورٽي** — ڪو به خطرناڪ APIs نه (ڏسو [سڪيورٽي قاعدا](#security-rules))
- **معيار** — ڇا اهو ڪم ڪري ٿو؟ ڇا ڪوڊ صاف آهي؟
- **فائدو** — ڇا اهو هڪ حقيقي مسئلو حل ڪري ٿو؟

منظوري کان پوء:
1. توهانجو پلگ ان `registry.json` ۾ شامل ڪيو ويندو
2. `dist/` ۾ هڪ ZIP بنڊل ٺاهيو ويندو
3. توهانجو پلگ ان **Plugin Store** ۾ سڀ WIA SOOM استعمال ڪندڙن لاءِ ظاهر ٿيندو!

---

## حصو 5: بهترين طريقيڪار

### سڪيورٽي قاعدا

اهي قاعدا **لازمي** آهن. جيڪي پلگ ان انهن جي خلاف ورزي ڪندا، انهن کي رد ڪيو ويندو.

| قاعدو | ڇو |
|------|-----|
| **ڪڏهن به** `eval()` يا `new Function()` استعمال نه ڪريو | ڪوڊ داخل ڪرڻ جو خطرو |
| **ڪڏهن به** `child_process`, `exec()`, `spawn()` استعمال نه ڪريو | صرف حڪمن لاءِ `ctx.terminal.send()` استعمال ڪريو |
| **ڪڏهن به** خارجي URLs حاصل نه ڪريو | استثناء: `wiasoom.com` API اينڊ پوائنٽس |
| **ڪڏهن به** `process.env` تائين رسائي نه ڪريو | ماحولياتي متغيرات ۾ راز ٿي سگهن ٿا |
| **ڪڏهن به** سڌو `require('fs')` استعمال نه ڪريو | اسٽوريج لاءِ `ctx.settings` استعمال ڪريو، فائل جي منتقلي لاءِ `ctx.sftp` استعمال ڪريو |
| **ڪڏهن به** npm خارجي پيڪيجز استعمال نه ڪريو | صرف خالص جاوا اسڪرپٽ — ڪو node_modules ناهي |
| **ضروري** آهي ته سڀني ريموٽ حڪمن لاءِ `ctx.terminal.send()` استعمال ڪيو وڃي | اهو محفوظ SSH چينل ذريعي ويندو |
| **ضروري** آهي ته `deactivate()` ۾ صفائي ڪريو | ٻڌندڙن کي هٽايو، انٽروال صاف ڪريو |

### غلطي جي سنڀال

هميشه خطري وارن عملن کي try/catch ۾ لپيٽيو:
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
### deactivate() ۾ صفائي

جيڪڏهن توهانجو پلگ ان انٽروال، ٻڌندڙ، يا سبسڪرپشن ٺاهي ٿو — انهن کي صاف ڪريو:
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
### i18n سپورٽ

WIA SOOM 254 ٻوليون سپورٽ ڪري ٿو. توهانجي پلگ ان ليبل کي ترجمو ڪرڻ جي قابل بڻائڻ لاءِ، هڪ سادو طريقيڪار استعمال ڪريو:
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

## حصو 6: حقيقي دنيا جا مثال

### مثال 1: سرور ڊسڪ چيڪر

ريموٽ سرور تي `df -h` هلائي ٿو ۽ اسٽيٽس بار ۾ استعمال ٿيل/دستياب جڳهه ڏيکاري ٿو.
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

### مثال 2: TODO مينيجر

هڪ پلگ ان جيڪو هڪ TODO فهرست کي منظم ڪري ٿو جيڪو مستقل اسٽوريج لاءِ سيٽنگن ۽ ڏيک لاءِ هڪ ويب ويئو استعمال ڪري ٿو.

> **ڊيزائن نمونو:** ڇاڪاڻ ته ويب ويون سڌو پلگ ان APIs کي سڏڻ جي قابل نه آهن، هي پلگ ان "اسنپ شاٽ" طريقيڪار استعمال ڪري ٿو — اهو سيٽنگن مان TODO پڙهي ٿو، انهن کي پڙهڻ لاءِ HTML طور رينڊر ڪري ٿو، ۽ شيون شامل ڪرڻ لاءِ سائڊبار تي مبني عمل مهيا ڪري ٿو. ويب ويئو هڪ **ڊسپلي** ليئر آهي، نه هڪ تعاملاتي فارم.
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

### مثال 3: غلطي واچر

ٽرمينل جي آئوٽ پٽ کي مانيٽر ڪري ٿو ۽ جڏهن خاص نمونا معلوم ٿين ٿا ته هڪ نوٽيفڪيشن موڪلي ٿو.
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

## ضميمو: زمره ۽ آئڪن

### پلگ ان زمره (29)

انهن کي پنهنجي `package.json` `keywords` ۾ يا رجسٽري ۾ جمع ڪرائڻ وقت استعمال ڪريو:

| زمره | وضاحت |
|----------|-------------|
| `server` | عام سرور انتظام |
| `devtools` | ترقياتي اوزار |
| `calculator` | حساب ڪتاب ۽ تبديل ڪندڙ |
| `simulator` | سيميوليٽر |
| `game` | ٽرمينل جا رانديون |
| `business` | ڪاروباري اوزار |
| `security` | سيڪيورٽي ۽ آڊٽنگ |
| `web` | ويب سرور انتظام |
| `education` | تعليمي اوزار |
| `health` | صحت سان لاڳاپيل اوزار |
| `islamic` | اسلامي اوزار (نماز جا وقت، وغيره) |
| `science` | سائنسي اوزار |
| `quantum` | ڪوانٽم ڪمپيوٽنگ جا اوزار |
| `ai` | AI سان هلندڙ اوزار |
| `biotech` | حياتياتي ٽيڪنالاجي جا اوزار |
| `space` | خلا ۽ فلڪيات جا اوزار |
| `network` | نيٽورڪ جا اوزار |
| `database` | ڊيٽابيس انتظام |
| `monitoring` | سرور جي نگراني |
| `devops` | ڊيو اوپس ۽ CI/CD |
| `utility` | عام اوزار |
| `design` | ڊيزائن جا اوزار |
| `ecommerce` | اي-ڪامرس جا اوزار |
| `automation` | خودڪار اوزار |
| `kpop` | K-pop سان لاڳاپيل اوزار |
| `accessibility` | رسائي جا اوزار |
| `analytics` | تجزياتي ۽ رپورٽنگ |
| `wia` | WIA ماحولياتي اوزار |
| `all` | سڀني زمرن ۾ موجود آهي |

### تجويز ڪيل آئڪن (Lucide)

| آئڪن جو نالو | استعمال لاءِ |
|-----------|---------|
| `server` | سرور انتظام |
| `shield` | سيڪيورٽي |
| `database` | ڊيٽابيس |
| `activity` | نگراني |
| `terminal` | ٽرمينل جا اوزار |
| `code` | ترقيات |
| `hard-drive` | ڊسڪ/ذخيرو |
| `network` | نيٽورڪنگ |
| `lock` | تصديق/انڪرپشن |
| `eye` | ڏسڻ/نگراني |
| `check-square` | ڪم/TODO |
| `layout-dashboard` | ڊيش بورڊ |
| `settings` | ترتيب |
| `zap` | خودڪار |
| `globe` | ويب/بين الاقوامي |

سڀ 1,500+ آئڪن کي براؤز ڪريو: [lucide.dev/icons](https://lucide.dev/icons)

---

## مدد جي ضرورت آهي؟

- **GitHub مسئلا:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **پلگ ان مسئلا:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **مثال پلگ ان:** [Website](https://wiasoom.com)
- **ويب سائيٽ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ڪجهه شاندار ٺاهيو. ان کي دنيا سان شيئر ڪريو.</em></p>
<p align="center"><em>— WIA SOOM ٽيم</em></p>