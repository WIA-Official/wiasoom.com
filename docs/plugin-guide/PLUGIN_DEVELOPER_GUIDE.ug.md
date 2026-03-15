<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>5 مىنۇت ئىچىدە ئۆز پىلاگىنىڭىزنى قۇرۇڭ.</strong></p>
<p align="center">WIA SOOM نىڭ ئىچىدە كۈچلۈك سەرۋر قوراللىرى، داشبوردلار ۋە ئاپتوماتلاشتۇرۇشلار قۇرۇڭ.</p>

---

## Table of Contents

- [Part 1: Quick Start — Your First Plugin in 5 Minutes](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Building Custom UI with Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Publishing Your Plugin](#part-4-publishing-your-plugin)
- [Part 5: Best Practices](#part-5-best-practices)
- [Part 6: Real-World Examples](#part-6-real-world-examples)
- [Appendix: Categories & Icons](#appendix-categories--icons)

---

## Part 1: Quick Start — Your First Plugin in 5 Minutes

### What you'll build

"Hello World" ناملىق پىلاگىن، سایدبارغا بىر كۇنۇپكا قوشىدۇ. كۇنۇپكىغا بېسىلغاندا، ئۇ بىر خەۋەرنى كۆرسىتىدۇ.

### Step 1: Create the plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Step 2: Create package.json
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
**Required fields:** `name`, `version`, `description`, `author`, `main`

### Step 3: Create index.js
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
### Step 4: Restart WIA SOOM

ئەپنى قايتا قوزغىتىڭ (ياكى تەڭشەكلەردە → پىلاگىنلاردا پىلاگىننى ئۆچۈرۈپ/يېڭىدىن قوزغىتىڭ).

سایدباردا **"Hello World"** كۇنۇپكىسىنى كۆرۈشىڭىز كېرەك. ئۇنى بېسىڭ — سىز بىر مۇۋەپپەقىيەت خەۋىرىنى كۆرۈسىز!

### How it works
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

## Part 2: Plugin Context API Reference

سىزنىڭ `activate(context)` فۇنكسىيىسى چاقىرىلغاندا، `context` (ياكى `ctx`) بۇ API لەرنى تەمىنلەيدۇ:
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

### `ctx.terminal` — Run commands on remote servers

#### `terminal.send(sessionId, data)`

ئاكتىپ تېرماينال سەسىيىسىگە بىر بويىچە (ياكى ھەر قانداق مەلۇمات) بىرىڭ.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | بويىچە بىرىدىغان تېرماينال سەسىيىسى |
| `data` | `string` | بويىچە بىرىدىغان بويىچە ياكى مەلۇمات |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

تېرماينال سەسىيىسىدىن بارلىق چىقىشقا ئەزا بولۇڭ. **ئەپسىز فۇنكسىيە** قايتۇرىدۇ.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | كۆزىتىدىغان تېرماينال سەسىيىسى |
| `callback` | `(data: string) => void` | ھەر بىر چىقىش بۆلەكتىكى چاقىرىلغان |
| **Returns** | `() => void` | بۇنى چاقىرىپ ئاڭلاشنى توختىتىڭ |
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
**Important:** دائىم ئەپسز فۇنكسىيىنى ساقلاڭ ۋە `deactivate()` دا چاقىرىپ، يادرو يوقىتىشتىن ساقلىنىڭ.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — SFTP API بەلگىلەنگەن، ئەمما ئەپنىڭ SFTP قورالغا توغرا كەلگەن يوق. `list()` ھازىر بوش تىزىملىك قايتۇرىدۇ، `upload()`/`download()` بولسا ئىشلىمەيدۇ. بۇ كەلگۈسى نەشرىدە تولۇق ئىجرا قىلىنىدۇ. ھازىرچە، `ctx.terminal.send()` نى `scp` ياكى `rsync` بويىچە بىرىڭ.

#### `sftp.list(sessionId, path)`

Remote directory دىن ھۆججەتلەرنى تىزىملاڭ.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

مېھمان كومپيۇتېرىدىن سەرۋرغا ھۆججەتنى يۈكلەڭ.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

سەرۋردىن مېھمان كومپيۇتېرىغا ھۆججەتنى چۈشۈرۈڭ.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (until SFTP API is live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

WIA SOOM سایدبارغا بىر كۇنۇپكا قوشۇڭ.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | يەككە ID (پىلاگىن نامىغا ئوخشايدۇ) |
| `icon` | `string` | Yes | Lucide نىڭ بەلگىسى (مەسىلەن، `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | سایدباردا كۆرسىتىلىدىغان كۇنۇپكا تېكىستى |
| `onClick` | `() => void` | Yes | كۇنۇپكا بېسىلغاندا چاقىرىلدىغان فۇنكسىيە |
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
**Icon reference:** [lucide.dev/icons](https://lucide.dev/icons) دىن بارلىق بار بەلگىلەرنى كۆرۈڭ.

> **Compatibility note:** بازى قەدىمكى پىلاگىنلار `addSidebarButton(id, icon, label, onClick)` قاتارلىق ئورۇن بويىچە پارامېتىرلارنى ئىشلىتىدۇ. رەسمىي API يۇقىرىدا بەلگىلەنگەن **options object** نى ئىشلىتىدۇ. يېڭى پىلاگىنلار ئۈچۈن دائىم ئوبئېكت شەكلىنى ئىشلىتىڭ.

#### `ui.openWebview(options)`

ئالاھىدە HTML مەزمۇنى بىلەن پاپ-ئويۇن ئېچىڭ. بۇ، سىزنىڭ باي UI قۇرۇش ئۇسۇلىڭىز.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | پەردە نامى |
| `html` | `string` | رەسىم قىلىنغان تول��ق HTML مەزمۇنى |
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
> [3-بۆلۈم](#part-3-building-custom-ui-with-webviews) نى زىيارەت قىلىپ، ئالدىنقى سەۋىيەدىكى webview شەكىللىرىنى كۆرۈڭ.

#### `ui.showNotification(type, message)`

بىر توست ئۇچۇرنى كۆرسىتىدۇ.

| پارامېتر | تىپى | چۈشەندۈرۈش |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ئۇچۇر شەكلى |
| `message` | `string` | كۆرسىتىدىغان تېكىست |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

ئاستىدىكى سىتون باندىغا داۋاملىق تېكىست بويىچە بىرىڭ.

| پارامېتر | تىپى | چۈشەندۈرۈش |
|-----------|------|-------------|
| `id` | `string` | بۇ سىتون باندىغا خاس يەككە ID |
| `text` | `string` | كۆرسىتىدىغان تېكىست |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — داۋاملىق ساقلاش

Plugin نىڭ تەڭشەكلەر دائىم `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` دا ساقلىنىدۇ.

#### `settings.get(key)`

ساقلانغان قىممەتنى ئوقۇيدۇ.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
ئەگەر كۇنۇپكا يوق بولسا، `undefined` نى قايتۇر��دۇ.

#### `settings.set(key, value)`

قىممەتنى ساقلايدۇ. سىزگە سىزىق، سان، بولىس، تىزىملىك ۋە نۇسخىلارنى قوللايدۇ.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**مەسىلەن: پايدىلىنىشچىلارنىڭ تاللاشلىرىنى ئەسكە ئېلىش**
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

### `ctx.ai` — AI ئىنتېگراسىيەسى

> **ھالەت: يېقىن كېلىشتە** — AI API بەلگىلەنگەن، ئەمما Soomy غا ئۇلانمىغان. ھازىر `{ response: 'AI not yet connected' }` نى قايتۇرىدۇ. تولۇق AI ئىنتېگراسىيەسى كەلگۈسى نەشرىدە پىلانلانغان.

#### `ai.chat(messages, options?)`

AI ياردەمچىسىگە (Soomy) ئۇچۇرلارنى ئەۋەتىدۇ.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## 3-بۆلۈم: Webviews بىلەن خاس UI قۇرۇش

`openWebview()` API سىزگە HTML، CSS، ۋە JavaScript بىلەن داشبورد UI قۇرۇشقا يول قويىدۇ — بارلىق ئىشلار پاپ-ئوينىدا.

> **مۇھىم چەكلىمە:** Webviews پەقەت **كورسىتىش ئۈچۈن**. ئۇلار plugin API لىرىگە ( `ctx.settings`, `ctx.terminal`, ۋە باشقا) قايتۇرۇش قىلالمايدۇ. پايدىلىنىشچىلارنىڭ بارل��ق ھەرىكەتلىرى ئۈچۈن سىتون كۇنۇپكىلارنى ئىشلىتىڭ، ۋە جارىقىن ھالەتنى كۆرسىتىش ئۈچۈن `openWebview()` نى ئىشلىتىڭ. ئەگەر سىزگە ئىنتېرئاكتىپ خاسلاشتۇرۇش كېرەك بولسا، ئۇلارنى سىتون كۇنۇپكىلاردىن قوزغىتىپ، كۆرسىتىشنى يېڭىلاش ئۈچۈن webview نى قايتا ئېچىڭ.

### شەكىل: Terminal بويىچە بۇيرۇق → نەتىجىنى پارسلاش → HTML دا كۆرسىتىش

بۇ ئەڭ كۆپ قوللىنىلىدىغان plugin شەكلى. سىز بۇيرۇقنى ئىجرا قىلىسىز، نەتىجىنى پارسلايسىز، ۋە ئۇنى كۆرۈنمە يۈزىدە كۆرسىتىسىز.
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
### شەكىل: ئاپتوماتىك يېڭىلىنىش بىلەن ئىنتېرئاكتىپ داشبورد
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
### شەكىل: Webview دا تەڭشەكلەرنى كۆرسىتىش

> **ئەسكەرتىش:** Webviews پەقەت كورسىتىش ئۈچۈن — ئۇلار plugin API ل��رىگە قايتۇرۇش قىلالمايدۇ. تەڭشەكلەرنى ئۆزگەرتىش ئۈچۈن سىتون كۇنۇپكىلاردىكى `ctx.settings` نى ئىشلىتىڭ، ۋە جارىقىن ھالەتنى كۆرسىتىش ئۈچۈن `openWebview()` نى ئىشلىتىڭ.
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

## 4-بۆلۈم: Plugin نىڭىزنى نەشر قىلىش

### 1-قەدەم: يەرلىك سىناش

1. Plugin نىڭىزنى `~/.wia-soom/plugins/{your-plugin}/` غا كۆچۈرۈڭ
2. WIA SOOM نى قايتا قوزغىتىڭ
3. ئىشلىشىنى تەكشۈرۈڭ: سىتون كۇنۇپكىسى كۆرۈنىدۇ، خاسلاشتۇرۇشلار توغرا ئىشلىشى كېرەك
4. چەكلەنگەن ھاللارنى سىناڭ: ئەگەر ھېچقانداق terminal ئۇلانمىغان بولسا، نېمە بولىدۇ؟

### 2-قەدەم: تاپشۇرۇشقا تەييارلىنىش

Plugin قاپچىقىڭىزدا تۆۋەندىكىلەر بولۇشى كېرەك:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Talap qilinadigan `package.json` maydonlari:**

| Maydon | Ta'rif | Misol |
|--------|--------|-------|
| `name` | Yagona kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantik versiya | `"1.0.0"` |
| `description` | Bir qatorli ta'rif | `"Monitors nginx access logs in real-time"` |
| `author` | Ismingiz | `"John Doe"` |
| `main` | Kirish nuqtasi | `"index.js"` |

**Ixtiyoriy maydonlar:**

| Maydon | Ta'rif |
|--------|--------|
| `license` | Litsenziya turi (MIT tavsiya etiladi) |
| `keywords` | Qidiruv teglarining massivi |
| `soom.minVersion` | Talab qilinadigan minimal WIA SOOM versiyasi |

### 3-qadam: Plugin Ro'yxatiga yuborish

1. ****Package** your plugin as a ZIP file
2. **Qo'shing** plugin'ingizni `plugins/{your-plugin-name}/`
3. **Yuboring** Pull Request

### 4-qadam: Ko'rib chiqish va tasdiqlash

Biz har bir pluginni quyidagilar uchun ko'rib chiqamiz:

- **Xavfsizlik** — xavfli API-lar yo'q (qarang [Xavfsizlik Qoidalari](#security-rules))
- **Sifat** — u ishlayaptimi? Kod toza mi?
- **Foydalilik** — u haqiqiy muammoni hal qiladimi?

Tasdiqlangandan so'ng:
1. Sizning plugin'ingiz `registry.json` ga qo'shiladi
2. `dist/` da ZIP to'plami yaratiladi
3. Sizning plugin'ingiz barcha WIA SOOM foydalanuvchilari uchun **Plugin Store** da ko'rinadi!

---

## 5-qism: Eng yaxshi amaliyotlar

### Xavfsizlik Qoidalari

Ushbu qoidalar **majburiy**. Ularni buzadigan plaginlar rad etiladi.

| Qoidalar | Nima uchun |
|----------|------------|
| **HECH QACHON** `eval()` yoki `new Function()` dan foydalanmang | Kodni kiritish xavfi |
| **HECH QACHON** `child_process`, `exec()`, `spawn()` dan foydalanmang | Faqat `ctx.terminal.send()` ni buyruqlar uchun ishlating |
| **HECH QACHON** tashqi URL-larni olishga harakat qilmang | Istisno: `wiasoom.com` API nuqtalari |
| **HECH QACHON** `process.env` ga kirishmang | Atrof-muhit o'zgaruvchilari sirlarni o'z ichiga olishi mumkin |
| **HECH QACHON** `require('fs')` ni to'g'ridan-to'g'ri ishlatmang | Saqlash uchun `ctx.settings`, fayl uzatish uchun `ctx.sftp` dan foydalaning |
| **HECH QACHON** npm tashqi paketlardan foydalanmang | Faqat toza JavaScript — hech qanday node_modules |
| **SHART** barcha masofaviy buyruqlar uchun `ctx.terminal.send()` dan foydalaning | Bu xavfsiz SSH kanali orqali o'tadi |
| **SHART** `deactivate()` da tozalang | Tinglovchilarni olib tashlang, intervalni tozalang |

### Xato Boshqaruvi

Har doim xavfli operatsiyalarni try/catch ichiga o'rab oling:
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
### deactivate() da tozalash

Agar plugin'ingiz interval, tinglovchilar yoki obunalarni yaratgan bo'lsa — ularni tozalang:
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
### i18n Qo'llab-quvvatlash

WIA SOOM 254 tilni qo'llab-quvvatlaydi. Pluginingiz yorlig'ini tarjima qilish mumkin bo'lishi uchun oddiy yondashuvni qo'llang:
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

## 6-qism: Haqiqiy Misollar

### Misol 1: Server Disk Tekshirgichi

Masofaviy serverda `df -h` ni bajaradi va holat panelida ishlatilgan/mavjud joyni ko'rsatadi.
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

### Misol 2: TODO Boshqaruvchisi

Doimiy saqlash uchun sozlamalarni va ko'rsatish uchun webview dan foydalanadigan TODO ro'yxatini boshqaruvchi plagin.

> **Dizayn naqsh:** Webview'lar to'g'ridan-to'g'ri plagin API'larini chaqira olmasligi sababli, ushbu plagin "snapshot" yondashuvini qo'llaydi — u TODO'larni sozlamalardan o'qiydi, ularni o'qish uchun HTML sifatida ko'rsatadi va elementlarni qo'shish uchun yon panelga asoslangan harakatlar taqdim etadi. Webview — bu **ko'rsatish** qatlami, interaktiv forma emas.
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

### Misol 3: Xato Kuzatuvchi

Terminal chiqishini kuzatadi va ma'lum naqshlar aniqlanganda bildirishnoma yuboradi.
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

## قوشۇمچە: تۈرلەر ۋە بەلگىلەر

### پىلگىن تۈرلىرى (29)

بۇلارنى `package.json` `keywords` نى ياكى تىزىملىككە يوللاشتا ئىشلىتىڭ:

| تۈر | چۈشەندۈرۈش |
|----------|-------------|
| `server` | ئومۇمىي سېرۋېر باشقۇرۇش |
| `devtools` | تەرەققىيات قوراللىرى |
| `calculator` | ھېسابلاش ۋە ئايلاندۇرۇش قوراللىرى |
| `simulator` | سىمۇلاتورلار |
| `game` | تېرمانال ئويۇنلىرى |
| `business` | سودا قوراللىرى |
| `security` | خەۋپسىزلىك ۋە تەكشۈرۈش |
| `web` | تور سېرۋېر باشقۇرۇش |
| `education` | تەعليم قوراللىرى |
| `health` | ساغلاملىققا دائىر قوراللار |
| `islamic` | ئىسلام قوراللىرى (ناماز ۋاقتى، ۋە باشقا) |
| `science` | پەن قوراللىرى |
| `quantum` | كۋانتۇم كومپيۇتېر قوراللىرى |
| `ai` | AI قۇۋۋىتىدىكى قوراللار |
| `biotech` | بيوتېخنىكا قوراللىرى |
| `space` | بوشلۇق ۋە ئاسمانشۇناسلىق قوراللىرى |
| `network` | تور قوراللىرى |
| `database` | مەلۇماتلارنى باشقۇرۇش |
| `monitoring` | سېرۋېرنى كۆزىتىش |
| `devops` | DevOps ۋە CI/CD |
| `utility` | ئومۇمىي پايدىلىق قوراللار |
| `design` | لايىھە قوراللىرى |
| `ecommerce` | ئېكۇنومىيە قوراللىرى |
| `automation` | ئاپتوماتلاشتۇرۇش قوراللىرى |
| `kpop` | K-pop بىلەن باغلانغان قوراللار |
| `accessibility` | كىرىش قوراللىرى |
| `analytics` | تەھلىل ۋە دوكلات |
| `wia` | WIA ئېكوسىستېما قوراللىرى |
| `all` | بارلىق تۈرلەردە كۆرۈنىدۇ |

### تەۋسىيە قىلىنغان بەلگىلەر (Lucide)

| بەلگە نامى | ئىشلىتىش ئۈچۈن |
|-----------|---------|
| `server` | سېرۋېر باشقۇرۇش |
| `shield` | خەۋپسىزلىك |
| `database` | مەلۇماتلار |
| `activity` | كۆزىتىش |
| `terminal` | تېرمانال قوراللىرى |
| `code` | تەرەققىيات |
| `hard-drive` | دىسكا/ساقلاش |
| `network` | تورلاش |
| `lock` | ئاۋتھ/شifreleme |
| `eye` | كۆرۈش/كۆزىتىش |
| `check-square` | ۋەزىپىلەر/TODO |
| `layout-dashboard` | داشبوردلار |
| `settings` | كونفىگۇرېيتسىيە |
| `zap` | ئاپتوماتلاشتۇرۇش |
| `globe` | تور/خەلقئارا |

بارلىق 1,500+ بەلگىلەرنى كۆرۈش: [lucide.dev/icons](https://lucide.dev/icons)

---

## ياردەم كېرەك؟

- **GitHub مەسىلىلىرى:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **پىلگىن مەسىلىلىرى:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **مەسىلەلەر پىلگىنلىرى:** [Website](https://wiasoom.com)
- **تور بەت:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>بىر نەرسە قۇرۇڭ. دۇنيا بىلەن بۆلۈشۈڭ.</em></p>
<p align="center"><em>— WIA SOOM كوماندىسى</em></p>