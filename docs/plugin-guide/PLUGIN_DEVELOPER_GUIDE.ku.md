<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">بەرنامەی پەیوەندیدانی وەبەری WIA SOOM</h1>
<p align="center"><strong>پەیوەندیدانی خۆت لە 5 خولەکدا دروست بکە.</strong></p>
<p align="center">ئامادەکردنی توولە سێرڤەرە پەیوەندیدارەکان، داشبۆردەکان، و ئامادەکردنەکان — لە ناو WIA SOOM.</p>

---

## فهرست مەحتوا

- [بەش 1: دەستپێکی چەند کەس — پەیوەندیدانی یەکەمی تۆ لە 5 خولەکدا](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [بەش 2: پەیوەندیدانی پەیوەندیدانی API نیشانی](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [بەش 3: دروستکردنی UI تایبەتی بە شێوەی وێب](#part-3-building-custom-ui-with-webviews)
- [بەش 4: بڵاوکردنەوەی پەیوەندیدانی تۆ](#part-4-publishing-your-plugin)
- [بەش 5: رێنماییە باشەکان](#part-5-best-practices)
- [بەش 6: نموونەیەکی جیهانی](#part-6-real-world-examples)
- [پەیوەندیدان: پۆل و شێوەکان](#appendix-categories--icons)

---

## بەش 1: دەستپێکی چەند کەس — پەیوەندیدانی یەکەمی تۆ لە 5 خولەکدا

### ئەو شتانی کە دروست دەکەیت

پەیوەندیدانی "Hello World" کە شتێک بۆ سایدبار زیاد دەکات. کاتێک کرابە، ئاگاداریەکی نیشانی دەدات.

### پەیوەندیدانی 1: دروستکردنی فۆلدرە پەیوەندیدانی
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### پەیوەندیدانی 2: دروستکردنی package.json
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
**پەیوەندیدانی پێویست:** `name`, `version`, `description`, `author`, `main`

### پەیوەندیدانی 3: دروستکردنی index.js
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
### پەیوەندیدانی 4: دوبارە چالاککردنی WIA SOOM

بەرنامەکە دوبارە چالاک بکە (یان پەیوەندیدانی لەسەر/لادەنەوە لە ڕووکاری → پەیوەندیدان).

دەتوانیت **"Hello World"** شتێک لە سایدبار ببینی. کرابە — دەتوانیت ئاگاداریەکی سەرکەوتن ببینی!

### چۆن کار دەکات
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

## بەش 2: پەیوەندیدانی پەیوەندیدانی API نیشانی

کاتێک فەرمی `activate(context)` پەیوەندیدانی تۆ بکرێت، `context` (یان `ctx`) ئەم APIەی خوارەوە پێشکەش دەکات:
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

### `ctx.terminal` — فەرمانەکان لە سێرڤەرە دەرەکیەکان بەکاربەرە

#### `terminal.send(sessionId, data)`

فەرمانێک (یان هەر داتاێکی تر) بۆ کەیسە فەرمانە کە چالاکە بفرستە.

| پارامەتەر | جۆر | وەسف |
|-----------|------|-------------|
| `sessionId` | `string` | کەیسە فەرمانە بۆ ناردن |
| `data` | `string` | فەرمان یان داتا بۆ ناردن |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

بۆ هەموو دەرچوونی لە کەیسە فەرمانەکانەوە سەبسکرایب بکە. فەرمی **بەردەستکردن** بەرەوپێش دەکات.

| پارامەتەر | جۆر | وەسف |
|-----------|------|-------------|
| `sessionId` | `string` | کەیسە فەرمانە بۆ بینین |
| `callback` | `(data: string) => void` | لەگەڵ هەر پەرتووکێکی دەرچوونەوە پەیوەندیدانی دەکرێت |
| **بەرەوپێش دەکات** | `() => void` | ئەمە بکاربە بۆ بەرزکردنەوەی گوێگرتن |
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
**مهم:** هەموو کات بەردەستکردنی فەرمی بەردەستکردن بەرز بکە و لە `deactivate()` دا بکاربە بۆ پەیوەندیدانی بەرزکردنەوەی بەرزکردنەوە.

---

### `ctx.sftp` — ناردنی فایل

> **دۆخی: دەرەوە دێت** — APIی SFTP دیاری کراوە بەڵام هێشتا بۆ مێشکی SFTPی بەرنامە نیشانی نەکراوە. `list()` لە ئیشکراوی ئیشکراوی کە خالیە، و `upload()`/`download()` هیچ شتێکی نادەن. ئەمە لە بەرنامەی پاش ئەم ڕووکاریە بەرز دەکرێت. بۆ ئیشکردن، `ctx.terminal.send()` بە فەرمانەکانی `scp` یان `rsync` بەکاربەرە.

#### `sftp.list(sessionId, path)`

فایلەکان لە فۆلدرێکی دەرەکی نیشانی بکە.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

فایلێک لە ماشیندەی ناوەڕاست بۆ سێرڤەرە دەرەکی ناردن.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

فایلێک لە سێرڤەرە دەرەکی بۆ ماشیندەی ناوەڕاست داگرتن.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**چاره‌سەر (هەتا APIی SFTP چالاک بێت):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — بەرزکردنەوەی بەکارهێنەر

#### `ui.addSidebarButton(options)`

شێوەیەکی بەرزکردنەوە بۆ سایدبارە WIA SOOM زیاد بکە.

| هەڵبژاردن | جۆر | پێویست | وەسف |
|--------|------|----------|-------------|
| `id` | `string` | نا | IDی تایبەتی (بە شێوەی پەیوەندیدانی ناوی پەیوەندیدان) |
| `icon` | `string` | بەلێ | ناوی شێوەی Lucide (بە شێوەی `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | بەلێ | نوسینی شت لە سایدباردا |
| `onClick` | `() => void` | بەلێ | فەرمی کە کاتێک شت کرابە بکاربە |
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
**پەیوەندیدانی شێوە:** هەموو شێوەکان بەرزکردنەوە لە [lucide.dev/icons](https://lucide.dev/icons) بینە.

> **تێبینی پەیوەندیدان:** هەندێک پەیوەندیدانی کۆن بە شێوەی پەیوەندیدانی نیشانی `addSidebarButton(id, icon, label, onClick)` بەکار دەهێنێت. APIی فەرمی بە شێوەی **بەرزکردنەوەی هەڵبژاردن** وەسف کراوە. هەموو کات شێوەی بەرزکردنەوە بەرز بکە بۆ پەیوەندیدانی نوێ.

#### `ui.openWebview(options)`

پەنجەرەی پاپی بەرزکردنەوەی بە شێوەی HTML تایبەتی بەرز بکە. ئەمە چۆن بەرزکردنەوەی UIی زۆر بەرز دەکات.

| هەڵبژاردن | جۆر | وەسف |
|--------|------|-------------|
| `title` | `string` | ناوی پەنجەرە |
| `html` | `string` | تام HTML بەرزکردنەوەی بەرز بەرز بکە |
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
> بەرەوپێش [بەش 3](#part-3-building-custom-ui-with-webviews) بۆ شێوەیەکانی پەیوەندیدانی پەیوەندیدار.

#### `ui.showNotification(type, message)`

نیشانی توستی پەیامەکە نیشان بدە.

| پارامەتر | جۆر | تێبینی |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | شێوەی پەیامەکە |
| `message` | `string` | نوسین بۆ نیشاندانی |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

بەشێکی نوسراوی بەرز بۆ شەوەی نیشانی خوارەوە زیاد بکە.

| پارامەتر | جۆر | تێبینی |
|-----------|------|-------------|
| `id` | `string` | ID ی تایبەتی بۆ ئەم بەرزە نیشانیە |
| `text` | `string` | نوسین بۆ نیشاندانی |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — بەرزکردنەوەی پەیوەندیدار

بەشەکانی پلەگین بەرز دەکرێن لە `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

نرخی هەڵبژێردراوەکان بخوێنەوە.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
ئەگەر key نەبێت، `undefined` دەنرێت.

#### `settings.set(key, value)`

نرخی هەڵبژێردراوەکان پاشەکەوت بکە. پشتیوانی لە نوسینەکان، ژمارەکان، بۆلینەکان، ڕیزەکان، و شتەکان دەکات.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**نمونە: بەرزکردنەوەی پەیوەندیدانی بەکاربەر**
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

### `ctx.ai` — پەیوەندیدانی AI

> **دۆخی: هەریم بەرز** — API ی AI دیاری کراوە بەڵام بەرز نەکراوە بۆ Soomy. لە ئێستا `{ response: 'AI not yet connected' }` دەنرێت. پەیوەندیدانی تەواوی AI بۆ بەرزکردنەوەی پاشەکەوتی داهاتوو پلانی پێشنیازکراوە.

#### `ai.chat(messages, options?)`

پەیامەکان بۆ یارمەتیدەری AI (Soomy) بەرز بکە.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## بەش 3: دروستکردنی UI تایبەتی بە پەیوەندیدار

API ی `openWebview()` بەرز دەکات بۆ دروستکردنی UI ی داشبۆرد لەگەڵ HTML، CSS، و JavaScript — هەموو لە ناو پەنجەرەی پەیوەند��دار.

> **مهمەتی سەرەکی:** پەیوەندیدارەکان تەنها **نیشانی** نیشاندنە. ناتوانن بەرز بکەن بۆ API ی پلەگین (`ctx.settings`, `ctx.terminal`, و هتد). بەرز بکە لە بەرزە نیشانیەکان بۆ هەموو کردارەکانی بەکاربەر، و `openWebview()` بۆ نیشاندانی دۆخی ئیشکراو. ئەگەر پەیوەندیدارە تایبەتییەکان پێویستت، لە بەرزە نیشانیەکان بەرز بکە و پەیوەندیدارەکە دوبارە بەرز بکە بۆ نوێکردنەوەی نیشاندن.

### شێوە: فرمانی ترمینال → پەیوەندیدانی ئەنجام → نیشاندانی لە HTML

ئەمە شێوەی پلەگینی زۆر بەکارهاتووە. تۆ فرمانێک ڕووبەڕووت دەکەیت، ئەنجامەکان پەیوەندیدەکەیت، و نیشانی ڕوونی نیشاندن.
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
### شێوە: داشبۆردی پەیوەندیداری تایبەتی بە نوێکردنەوەی خۆکار
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
### شێوە: نیشاندانی دامەزراندن لە پەیوەندیدار

> **تێبینی:** پەیوەندیدارەکان تەنها نیشانی نیشاندن — ناتوانن بەرز بکەن بۆ API ی پلەگین. `ctx.settings` لە هەندڵەری بەرزە نیشانیەکانت بۆ گۆڕینی دامەزراندن بەکاربەرە، و `openWebview()` بۆ نیشاندانی دۆخی ئیشکراو.
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

## بەش 4: بڵاوکردنەوەی پلەگینەکەت

### پەیوەندیدانی 1: تاقیکردنەوە لە ناوەوە

1. پلەگینەکەت بۆ `~/.wia-soom/plugins/{your-plugin}/` کۆپی بکە
2. WIA SOOM دوبارە پەیوەندیدار بکە
3. دڵنیابە لە کارکردنەوە: بەرزە نیشانیەکە دەرکەوت، تایبەتمەندیەکان بە شێوەیەکی ڕاست کاردەکەن
4. تاقیکردنەوەی کەیسە کێشەکان: چەندە بەرزە ترمینالەکان پەیوەندیدار نەکراوە؟

### پەیوەندیدانی 2: ئامادەکردن بۆ ناردن

پۆلدرە پلەگینەکەت پێویستە بگات:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**پێویستە `package.json` زانیارییەکان:**

| زانیاری | تێبینی | نموونە |
|-------|-------------|---------|
| `name` | ID یەکەمی تایبەتی کەباب-کەیس | `"my-awesome-plugin"` |
| `version` | وەشانی سەمانتیک | `"1.0.0"` |
| `description` | تێبینیەکی یەک لە ڕووداوەکان | `"Monitors nginx access logs in real-time"` |
| `author` | ناوی تۆ | `"John Doe"` |
| `main` | بنەڕەتی پەیوەندیدار | `"index.js"` |

**زانیارییەکانەی هەڵبژێردراو:**

| زانیاری | تێبینی |
|-------|-------------|
| `license` | جۆری ماف (پێشنیازی MIT) |
| `keywords` | پەیوەندیدانی تێگیشتنی بەرز |
| `soom.minVersion` | کەمترین وەشانی WIA SOOM پێویستە |

### پەیوەندیدانی 3: ناردنی بۆ فەرمی پلگین

1. ****Package** your plugin as a ZIP file
2. **Add** پلگینەکەت بۆ `plugins/{ناوی پلگینەکەت}/`
3. **Submit** داواکاری پەیوەندیدان

### پەیوەندیدانی 4: تێبینی و پەسندکردن

ئێمە هەموو پلگینەکان تێبینی دەکەین بۆ:

- **ئامانجی پاراستن** — هیچ API یەکی خەطرناک (ببینە [قائیدەکانی پاراستن](#security-rules))
- **کیفیت** — ئەوە کار دەکات؟ کۆدەکە پاکە؟
- **بەکارهێنانی** — ئەوە کێشەیەکی راستەقینە چارەسەر دەکات؟

پاش پەسندکردن:
1. پلگینەکەت بۆ `registry.json` زیاد دەکرێت
2. پەکەی ZIP لە `dist/` دروست دەکرێت
3. پلگینەکەت لە **فەرمی پلگین** بۆ هەموو بەکارهێنەران WIA SOOM دەرکەوتن!

---

## بەش 5: ڕووداوەکانێکی باش

### قاعدەکانی پاراستن

ئەم قاعدەکان **پێویست** نین. پلگینەکان کە ئەم قاعدەکانە لە شێوەیەکی تێکەڵەوە دەکەن لە نێوەوە دەنرێن.

| قاعدە | بۆچی |
|------|-----|
| **هەرگیز** `eval()` یان `new Function()` بەکاربەرە | ریسکەکانی کۆد نیشانەکردن |
| **هەرگیز** `child_process`, `exec()`, `spawn()` بەکاربەرە | تەنها `ctx.terminal.send()` بۆ فەرمانەکان بەکاربەرە |
| **هەرگیز** URL یەکی دەرەوە بگرە | استثنای: `wiasoom.com` API endpoints |
| **هەرگیز** `process.env` بگەرەوە | متغیرە کەسایەتیەکان دەتوانن سەرپەرشتی بەرز بگرن |
| **هەرگیز** `require('fs')` بە شێوەی ڕاستەوخۆ بەکاربەرە | بکاربەرە `ctx.settings` بۆ پەیوەندیدانی, `ctx.sftp` بۆ گواستنەوەی فایل |
| **هەرگیز** پەکەجەکانی دەرەوەی npm بەکاربەرە | تەنها JavaScript خالص — هیچ `node_modules` نییە |
| **پێویستە** `ctx.terminal.send()` بۆ هەموو فەرمانە دەرەوەیەکان بەکاربەرە | ئەمە لە ڕووبەری SSH پاراستنەوە دەچێت |
| **پێویستە** لە `deactivate()` پاکسازی بکە | گوێزەرەکان لابردن, نیشانی کەوتن پاک بکە |

### چەندین کێشە

هەموو کەرتەکانی خەطرناک لە try/catch بگێڕە:
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
### پاکسازی لە deactivate()

ئەگەر پلگینەکەت کەرتەکان، گوێزەرەکان، یان پەیوەندیدانی دروست ��کات — پاک بکە:
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
### پشتیوانی i18n

WIA SOOM پشتیوانی 254 زمان دەکات. بۆ ئەوەی تێبینی پلگینەکەت بکرێت، شێوەیەکی سادە بەکاربەرە:
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

## بەش 6: نموونەیەکی ڕاستەقینە

### نموونە 1: پشکنینی دیسکی سرور

`df -h` لە سەر سرورە دەرەوە ڕووناک دەکات و بەرز/بەرزەکان لە نیشانی کەوتنەوە دەرکەوتن.
§§§CHUNK_SEPARATOR§§§
---

### نموونە 2: بەرێوەبردنی TODO

پلگینێکی بەرێوەبردنی لیستی TODO بە شێوەیەکی پەیوەندیدانی بەرز و webview بۆ نیشاندانی.

> **شێوەی دیزاین:** چونکە webviews ناتوانن بە شێوەیەکی ڕووبەری پلگین API یەکان بگەڕن، ئەم پلگینە شێوەی "snapshot" بەکاربەرە — ئەوە TODO یەکان لە پەیوەندیدانەکان وەردەگرێت، وەردەگریت بەرزەکان بەرزەکان بەرزەکان و پەیوەندیدانی سایدبار بۆ زیادکردنی بەرزەکان. Webview شێوەی **نیشاندانی** یە، نە شێوەی پەیوەندیدانی بەرزەکان.
§§§CHUNK_SEPARATOR§§§
---

### نموونە 3: پشکنینی هەڵە

پەیوەندیدانی نیشانی کەوتنەوە و ناردنی ئاگاداری کاتێک شێوەی تایبەتی دیاری بکرێت.
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

## پەیوەندیدان: پۆل و نیشانیەکان

### پۆلەکانی پلگین (29)

ئەمە بەکاربەرە لە `package.json` `keywords` یان کاتێک پێشنیاز دەکەیت بۆ رەجەستری:

| پۆل | تێبینی |
|-----|---------|
| `server` | بەڕێوەبردنی گشتی سێرڤەر |
| `devtools` | ئەم تۆڵەی پەرەسەندنی |
| `calculator` | هەژمارەکان و گۆڕینەکان |
| `simulator` | شێوەکارییەکان |
| `game` | یارییەکان لە ترمینال |
| `business` | تۆڵەکانی کاروبار |
| `security` | پاراستن و تاقیکردنەوە |
| `web` | بەڕێوەبردنی سێرڤەری وێب |
| `education` | تۆڵەکانی فێرکاری |
| `health` | تۆڵەکانی پەیوەندیدار بە تندرستی |
| `islamic` | تۆڵەکانی ئیسلامی (کاتی ن prayers، وەکوو) |
| `science` | تۆڵەکانی زانستی |
| `quantum` | تۆڵەکانی کۆمپیوتەری کوانتوم |
| `ai` | تۆڵەکانی پەی��ەندیدار بە AI |
| `biotech` | تۆڵەکانی بیو تیکنۆلۆژی |
| `space` | تۆڵەکانی فضا و ئەستێرەیی |
| `network` | تۆڵەکانی شەبکە |
| `database` | بەڕێوەبردنی داتابەیس |
| `monitoring` | تاقیکردنەوەی سێرڤەر |
| `devops` | DevOps و CI/CD |
| `utility` | تۆڵەکانی گشتی |
| `design` | تۆڵەکانی دیزاین |
| `ecommerce` | تۆڵەکانی کۆمەرەتی ئینتەرنێت |
| `automation` | تۆڵەکانی ئامادەکردن |
| `kpop` | تۆڵەکانی پەیوەندیدار بە K-pop |
| `accessibility` | تۆڵەکانی ئیشکراوی |
| `analytics` | تۆڵەکانی هەژمارکردن و راپۆرتی |
| `wia` | تۆڵەکانی ئیكوسیستەمی WIA |
| `all` | لە هەموو پۆلەکاندا دەرکەوتن |

### نیشانیە تایبەتییەکان (Lucide)

| ناوی نیشان | بۆ چی بەکارده‌هێنرێت |
|------------|---------------------|
| `server` | بەڕێوەبردنی سێرڤەر |
| `shield` | پاراستن |
| `database` | داتابەیس |
| `activity` | تاقیکردنەوە |
| `terminal` | تۆڵەکانی ترمینال |
| `code` | پەرەسەندنی |
| `hard-drive` | دیسک/خ��ینە |
| `network` | شەبکەیی |
| `lock` | پەیوەندیدار/پەیوەندیدانی |
| `eye` | بینینی/تاقیکردنەوە |
| `check-square` | کارەکان/TODO |
| `layout-dashboard` | داشبوردەکان |
| `settings` | پەیوەندیدانی |
| `zap` | ئامادەکردن |
| `globe` | وێب/بینەری نووسراو |

بەگەڕانەوە بۆ هەموو 1,500+ نیشانەکان: [lucide.dev/icons](https://lucide.dev/icons)

---

## پێویست بە یارمەتیدانی؟

- **کێشەکانی GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **کێشەکانی پلگین:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **پلگینە نمونەکان:** [Website](https://wiasoom.com)
- **ماڵپەڕ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>چیزێکی شگفتی بەرز بکە. پەخشی بکە بۆ جیهان.</em></p>
<p align="center"><em>— تیمی WIA SOOM</em></p>
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
