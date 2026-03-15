<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>သင်၏ကိုယ်ပိုင် plugin ကို ၅ မိနစ်အတွင်း တည်ဆောက်ပါ။</strong></p>
<p align="center">WIA SOOM အတွင်းတွင် အင်အားကြီးသော server tools, dashboards, နှင့် automations များကို ဖန်တီးပါ။</p>

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

"Hello World" plugin တစ်ခုကို sidebar တွင် ခလုတ်တစ်ခု ထည့်သွင်းပါမည်။ နှိပ်သောအခါ သတိပေးချက်တစ်ခုကို ပြသပါမည်။

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

အက်ပလီကေးရှင်းကို ပြန်လည်စတင်ပါ (သို့မဟုတ် Settings → Plugins တွင် plugin ကို off/on ပြောင်းပါ)။

သင်သည် sidebar တွင် **"Hello World"** ခလုတ်ကို မြင်ရမည်။ ၎င်းကို နှိပ်ပါ - သင်သည် အောင်မြင်မှု သတိပေးချက်ကို မြင်ရမည်။

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

သင��၏ `activate(context)` function ကို ခေါ်ဆိုသောအခါ `context` (သို့မဟုတ် `ctx`) သည် ဤ API များကို ပံ့ပိုးပေးသည်။
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

Active terminal session သို့ command (သို့မဟုတ် အချက်အလက်) တစ်ခုကို ပို့ပါ။

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | ပို့ရန် terminal session |
| `data` | `string` | ပို့ရန် command သို့မဟုတ် အချက်အလက် |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Terminal session မှ output အားလုံးကို subscription လုပ်ပါ။ **unsubscribe function** တစ်ခုကို ပြန်လည်ပေးပါမည်။

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | ကြည���်ရှုရန် terminal session |
| `callback` | `(data: string) => void` | Output ၏ တစ်ခုချင်းစီ chunk ဖြင့် ခေါ်ဆိုပါသည် |
| **Returns** | `() => void` | နားထောင်မှုကို ရပ်တန့်ရန် ဤကို ခေါ်ပါ |
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
**Important:** Always save the unsubscribe function and call it in `deactivate()` to prevent memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — SFTP API သည် သတ်မှတ်ထားပြီး သို့သော် အက်ပလီကေး၏ SFTP engine နှင့် မချိတ်ဆက်သေးပါ။ `list()` သည် လက်ရှိတွင် အလွတ် array ကို ပြန်လည်ပေးသည်၊ နှင့် `upload()`/`download()` သည် no-ops ဖြစ်သည်။ ဤသည်ကို အနာဂတ်ထုတ်ဝေမှုတွင် အပြည့်အဝ အကောင်အထည်ဖော်မည်။ ယခုအခါတွင် `scp` သို့မဟုတ် `rsync` commands နှင့်အတူ `ctx.terminal.send()` ကို workaround အဖြစ် အသုံးပြုပါ။

#### `sftp.list(sessionId, path)`

Remote directory တွင် ဖိုင်များကို စာရင်းပြုစုပါ။
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Local machine မှ remote server သို့ ဖိုင်တစ်ခုကို upload လုပ်ပါ။
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Remote server မှ local machine သို့ ဖိုင်တစ်ခုကို download လုပ်ပါ။
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

WIA SOOM sidebar တွင် ခလုတ်တစ်ခု ထည့်ပါ။

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (plugin name အဖြစ် အခြေခံသည်) |
| `icon` | `string` | Yes | Lucide icon name (ဥပမာ၊ `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Sidebar တွင် ပြသမည့် ခလုတ်စာသား |
| `onClick` | `() => void` | Yes | ခလုတ်ကို နှိပ်သောအခါ ခေါ်ဆိုမည့် function |
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
**Icon reference:** Browse all available icons at [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** အချို့သော အဟောင်း plugin များသည် `addSidebarButton(id, icon, label, onClick)` ကဲ့သို့ positional arguments ကို အသုံးပြုသည်။ အတည်ပြု API သည် အထက်တွင် ဖော်ပြထားသည့် **options object** ကို အသုံးပြုသည်။ အသစ်သော plugin များအတွက် အမြဲ object style ကို အသုံးပြုပါ။

#### `ui.openWebview(options)`

Custom HTML content ဖြင့် popup window တစ်ခုကို ဖွင့်ပါ။ ဤသည်သည် သင့်အား ရှည်လျားသော UI များကို တည်ဆောက်ရန် ဖြစ်သည်။

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Render လုပ်ရန် အပြည့်အစုံ HTML content |
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
> [Part 3](#part-3-building-custom-ui-with-webviews) ကို အဆင့်မြင့် webview ပုံစံများအတွက် ကြည့်ပါ။

#### `ui.showNotification(type, message)`

Toast သတိပေးချက်ကို ပြသပါ။

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | သတိပေးချက် စတိုင် |
| `message` | `string` | ပြသရန် စာသား |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

အောက်ခြေ status bar တွင် အမြဲတမ်း စာသား အချက်အလက်ကို ထည့်ပါ။

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ဤ status item အတွက် ထူးခြားသော ID |
| `text` | `string` | ပြသရန် စာသား |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — အမြဲတမ်း သိမ်းဆည်းမှု

Plugin settings များကို `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` တွင် အမြဲတမ်း သိမ်းဆည်းထားသည်။

#### `settings.get(key)`

သိမ်းဆ���်းထားသော တန်ဖိုးကို ဖတ်ပါ။
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
key မရှိပါက `undefined` ကို ပြန်လည်ထုတ်ပေးပါသည်။

#### `settings.set(key, value)`

တန်ဖိုးကို သိမ်းဆည်းပါ။ စာသားများ၊ နံပါတ်များ၊ boolean များ၊ အစုအဖွဲ့များနှင့် အရာဝတ္ထုများကို ထောက်ပံ့သည်။
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**ဥပမာ: အသုံးပြုသူ ရွေးချယ်မှုများကို မှတ်သားပါ**
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

### `ctx.ai` — AI ပေါင်းစည်းခြင်း

> **အခြေအနေ: မကြာမီ ရောက်ရှိမည်** — AI API ကို သတ်မှတ်ထားပြီး Soomy နှင့် ချိတ်ဆက်ထားခြင်းမရှိသေးပါ။ လက်ရှိတွင် `{ response: 'AI not yet connected' }` ကို ပြန်လည်ထုတ်ပေးသည်။ AI ပေါင်းစည်းမှု အပြည့်အစုံကို အနာဂတ်ထုတ်ဝေမှုအတွက် စီစဉ်��ားသည်။

#### `ai.chat(messages, options?)`

AI အကူအညီ (Soomy) သို့ စာမေးခွန်းများကို ပို့ပါ။
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Webviews ဖြင့် အထူး UI တည်ဆောက်ခြင်း

`openWebview()` API သည် HTML, CSS, နှင့် JavaScript ဖြင့် dashboard UIs များကို popup ပြတင်းပေါက်အတွင်း တည်ဆောက်နိုင်သည်။

> **အရေးကြီးသော ကန့်သတ်ချက်:** Webviews သည် **ပြသရန်သာ** ဖြစ်သည်။ ၎င်းတို့သည် plugin APIs (`ctx.settings`, `ctx.terminal`, စသည်) သို့ ပြန်လည်ခေါ်ဆိုနိုင်ခြင်းမရှိပါ။ အသုံးပြုသူ လုပ်ဆောင်ချက်များအတွက် sidebar ခလုတ်များကို အသုံးပြုပါ၊ နှင့် လက်ရှိ အခြေအနေကို ��ြသရန် `openWebview()` ကို အသုံးပြုပါ။ အပြန်အလှန် လုပ်ဆောင်ချက်များလိုအပ်ပါက sidebar ခလုတ်များမှ ၎င်းတို့ကို ဖျက်သိမ်းပြီး webview ကို ပြန်ဖွင့်ပါ။

### ပုံစံ: Terminal Command → Output ကို ဖတ်ပါ → HTML တွင် ပြပါ

ဤသည်မှာ plugin ပုံစံအများဆုံးဖြစ်သည်။ သင်သည် အမိန့်တစ်ခုကို လည်ပတ်ပြီး၊ ရလဒ်ကို ဖတ်ပြီး၊ ဤကို ဗျူဟာအရ ပြသသည်။
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
### ပုံစံ: အလိုအလျောက် ပြန်လည်အသစ်ပြုလုပ်မှုနှင့် အပြန်အလှန် Dashboard
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
### ပုံစံ: Webview တွင် Settings များကို ပြသခြင်း

> **မှတ်ချက်:** Webviews သည် ပြသရန်သာ ဖြစ်သည် — ၎င်းတို့သည် plugin APIs သို့ ပြန်လည်ခေါ်ဆိုနိုင်ခြင်းမရှိပ��။ settings များကို ပြင်ဆင်ရန် sidebar ခလုတ် handler များတွင် `ctx.settings` ကို အသုံးပြုပါ၊ နှင့် လက်ရှိ အခြေအနေကို ပြသရန် `openWebview()` ကို အသုံးပြုပါ။
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

## Part 4: သင်၏ Plugin ကို ထုတ်ဝေခြင်း

### အဆင့် 1: ဒေသခံအဆင့်တွင် စမ်းသပ်ပါ

1. သင်၏ plugin ကို `~/.wia-soom/plugins/{your-plugin}/` သို့ ကူးပါ။
2. WIA SOOM ကို ပြန်လည်စတင်ပါ။
3. ၎င်းသည် အလုပ်လုပ်ပါသည်ဟု အတည်ပြုပါ: sidebar ခလုတ်သည် ပေါ်လာပြီး၊ လုပ်ဆောင်ချက်များသည် မှန်ကန်စွာ အလုပ်လုပ်သည်။
4. အထူးအခြေအနေများကို စမ်းသပ်ပါ: terminal မရှိပါက ဘာဖြစ်မလဲ။

### အဆင့် 2: တင်ပြရန် ပြင်ဆင်ပါ

သင်၏ plugin ဖိုလ်ဒါတွင် ပါဝင်ရမည့်အရာများ:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**လိုအပ်သော `package.json` အကွက်များ:**

| အကွက် | ဖော်ပြချက် | ဥပမာ |
|-------|-------------|---------|
| `name` | ထူးခြားသော kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | တစ်ကြောင်းဖော်ပြချက် | `"Monitors nginx access logs in real-time"` |
| `author` | သင့်အမည် | `"John Doe"` |
| `main` | အဓိပ္ပါယ်အချက် | `"index.js"` |

**ရွေးချယ်နိုင်သော အကွက်များ:**

| အကွက် | ဖော်ပြချက် |
|-------|-------------|
| `license` | လိုင်စင်အမျိုးအစား (MIT အကြံပြုသည်) |
| `keywords` | ရှာဖွေရန် အမှတ်တံဆိပ်များ၏ အစုအဝေး |
| `soom.minVersion` | လိုအပ်သော အနည်းဆုံး WIA SOOM ဗားရှင်��� |

### အဆင့် ၃: Plugin Registry သို့ တင်ပြပါ

1. ****Package** your plugin as a ZIP file
2. **Add** သင့် plugin ကို `plugins/{your-plugin-name}/` တွင်ထည့်ပါ
3. **Submit** Pull Request တစ်ခု

### အဆင့် ၄: ပြန်လည်သုံးသပ်ခြင်းနှင့် အတည်ပြုခြင်း

ကျွန်ုပ်တို့သည် plugin တစ်ခုချင်းစီကို အောက်ပါအချက်များအတွက် ပြန်လည်သုံးသပ်ပါသည် -

- **လုံခြုံမှု** — အန္တရာယ်ရှိသော APIs မရှိပါ (ကြည့်ပါ [Security Rules](#security-rules))
- **အရည်အသွေး** — ၎င်းသည် အလုပ်လုပ်ပါသလား? ကုဒ်သည် သန့်ရှင်းပါသလား?
- **အသုံးဝင်မှု** — ၎င်းသည် အမှန်တကယ် ပြဿနာတစ်ခုကို ဖြေရှင်းပါသလား?

အတည်ပြုခြင်းပြီးနောက် -
1. သင့် plugin ကို `registry.json` တွင် ထည့်သွင်းပါ
2. `dist/` တွင် ZIP bundle တစ်ခု ဖန်တီးပါ
3. သင့် plugin သည် **Plugin Store** တွင် WIA SOOM အသုံးပြုသူများအတွက် ပေါ်လာပါလိမ့်မည်!

---

## အပိုင်း ၅: အကောင်းဆုံး လုပ်ထုံးလုပ်နည်းများ

### လုံခြုံမှု စည်းမျဉ်းများ

ဤစည်းမျဉ်းများသည် **မဖြစ်မနေ** ဖြစ်သည်။ ဤစည်းမျဉ်းများကို ချိုးဖျက်သော plugin များကို ပယ်ဖျက်မည်။

| စည်းမျဉ်း | အကြောင်း |
|------|-----|
| **မည်သည့်အခါမှ** `eval()` သို့မဟုတ် `new Function()` ကို အသုံးမပြုပါနှင့် | ကုဒ်ထည့်သွင်းမှု အန္တရာယ် |
| **မည်သည့်အခါမှ** `child_process`, `exec()`, `spawn()` ကို အသုံးမပြုပါနှင့် | အမိန့်များအတွက် `ctx.terminal.send()` ကိုသာ အသုံးပြုပါ |
| **မည်သ��့်အခါမှ** အပြင် URL များကို ရယူမထားပါနှင့် | အထူးအခြေအနေ: `wiasoom.com` API endpoints |
| **မည်သည့်အခါမှ** `process.env` ကို ဝင်ရောက်မထားပါနှင့် | ပတ်ဝန်းကျင် အပြောင်းအလဲများတွင် လျှို့ဝှက်ချက်များ ပါရှိနိုင်သည် |
| **မည်သည့်အခါမှ** `require('fs')` ကို တိုက်ရိုက် အသုံးမပြုပါနှင့် | သိမ်းဆည်းရန် `ctx.settings` ကို အသုံးပြုပါ၊ ဖိုင်လွှဲပြောင်းရန် `ctx.sftp` ကို အသုံးပြုပါ |
| **မည်သည့်အခါမှ** npm အပြင် package များကို အသုံးမပြုပါနှင့် | သန့်ရှင်းသော JavaScript သာ — node_modules မရှိပါ |
| **မဖြစ်မနေ** `ctx.terminal.send()` ကို အားလုံးသော အဝင်အမိန့်များအတွက် အသုံးပြုပါ | ၎င်းသည် လုံခြုံသော SSH ချန်နယ��မှ လျှောက်ထားသည် |
| **မဖြစ်မနေ** `deactivate()` တွင် သန့်ရှင်းပါ | နားထောင်သူများကို ဖယ်ရှားပါ၊ အချိန်အတွင်းများကို ရှင်းလင်းပါ |

### အမှား ကိုင်တွယ်ခြင်း

အန္တရာယ်ရှိသော လုပ်ငန်းဆောင်မှုများကို အမြဲတမ်း try/catch ထဲတွင် ဖုံးအုပ်ပါ:
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
### deactivate() တွင် သန့်ရှင်းခြင်း

သင့် plugin သည် အချိန်အတွင်းများ၊ နားထောင်သူများ သို့မဟုတ် စာရင်းသွင်းမှုများကို ဖန်တီးပါက — ၎င်းတို့ကို သန့်ရှင်းပါ:
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
### i18n ထောက်ပံ့မှု

WIA SOOM သည် ဘာသာစကား 254 မျိုးကို ထောက်ပံ့သည်။ သင့် plugin ၏ အမှတ်တံဆိပ်ကို ဘာသာပြန်နိုင်ရန် ရိုးရှင်းသေ��� နည်းလမ်းကို အသုံးပြုပါ:
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

## အပိုင်း ၆: အမှန်တကယ် ကုန်ကြမ်းများ

### ဥပမာ ၁: ဆာဗာ ဒီစက် စစ်ဆေးခြင်း

အဝေးမှ ဆာဗာတွင် `df -h` ကို လည်ပတ်ပြီး status bar တွင် အသုံးပြု/ရရှိနိုင်သော အာရုံကို ပြသသည်။
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

### ဥပမာ ၂: TODO စီမံခန့်ခွဲမှု

TODO စာရင်းကို စီမံခန့်ခွဲရန် settings ကို အသုံးပြု၍ အမြဲတမ်း သိမ်းဆည်းမှုနှင့် ပြသရန် webview ကို အသုံးပြုသော plugin တစ်ခု။

> **ဒီဇိုင်း ပုံစံ:** Webviews များသည် plugin APIs ကို တိုက်ရိုက် ခေါ်ဆိုနိုင်မည်မဟုတ်သောကြောင့်၊ ဤ plugin သည် "snapshot" နည်းလမ်းကို အသုံးပြုသည် — ၎င်းသည် settings မှ TODO များကို ဖတ်ပြ��း၊ ၎င်းတို့ကို ဖတ်ရှုရန် HTML အဖြစ် ဖန်တီးပြီး၊ အရာဝတ္ထုများ ထည့်ရန် sidebar အခြေခံ လုပ်ဆောင်ချက်များကို ပံ့ပိုးသည်။ Webview သည် **ပြသမှု** အဆင့်ဖြစ်ပြီး၊ အပြန်အလှန် အမျိုးအစားမဟုတ်ပါ။
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

### ဥပမာ ၃: အမှား ကြည့်ရှုသူ

Terminal output ကို ကြည့်ရှု၍ သတ်မှတ်ထားသော ပုံစံများကို တွေ့ရှိပါက သတိပေးချက်တစ်ခု ပို့သည်။
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

## အပိုင်း: အမျိုးအစားများနှင့် အိုင်ကွန်းများ

### ပလပ်ဂင် အမျိုးအစားများ (29)

သင်၏ `package.json` `keywords` တွင် သို့မဟုတ် မှတ်ပုံတင်ရာတွင် အသုံးပြုပါ:

| အမျိုးအစား | ဖော်ပြချက် |
|----------|-------------|
| `server` | အထွေထွေ ဆာဗာ စီမံခန့်ခွဲမှု |
| `devtools` | ဖွံ့ဖြိုးတိုးတက်ရေး ကိရိယာများ |
| `calculator` | ကိန်းတွက်စက်များနှင့် ပြောင်းလဲမှုများ |
| `simulator` | စမ်းသပ်ကိရိယာများ |
| `game` | Terminal ဂိမ်းများ |
| `business` | စီးပွားရေး ကိရိယာများ |
| `security` | လုံခြုံရေးနှင့် စစ်ဆေးမှု |
| `web` | ဝက်ဘ် ဆာဗာ စီမံခန��်ခွဲမှု |
| `education` | ပညာရေး ကိရိယာများ |
| `health` | ကျန်းမာရေးနှင့် ဆက်နွယ်သော ကိရိယာများ |
| `islamic` | အစ္စလာမ် ကိရိယာများ (ဆုတောင်းချိန်များ၊ စသည်) |
| `science` | သိပ္ပံ ကိရိယာများ |
| `quantum` | Quantum ကွန်ပျူတာ ကိရိယာများ |
| `ai` | AI အခြေခံ ကိရိယာများ |
| `biotech` | ဘိုင်အိုတက်နိုလျူဂျီ ကိရိယာများ |
| `space` | အာကာသနှင့် ကြယ်လွင်ပြင် ကိရိယာများ |
| `network` | ကွန်ယက် ကိရိယာများ |
| `database` | ဒေတာဘေ့စ် စီမံခန့်ခွဲမှု |
| `monitoring` | ဆာဗာ စောင့်ကြည့်မှု |
| `devops` | DevOps နှင့် CI/CD |
| `utility` | အထွေထွေ အသုံးအဆောင်များ |
| `design` | ဒီဇိုင်း ကိရိယာများ |
| `ecommerce` | အွန်လိုင်း စီးပွားရေး ကိရိယာမ��ား |
| `automation` | အလိုအလျောက်လုပ်ငန်း ကိရိယာများ |
| `kpop` | K-pop နှင့် ဆက်နွယ်သော ကိရိယာများ |
| `accessibility` | လက်လှမ်းရောက်မှု ကိရိယာများ |
| `analytics` | အချက်အလက်များနှင့် အစီရင်ခံစာ |
| `wia` | WIA အစီအစဉ် ကိရိယာများ |
| `all` | အမျိုးအစားအားလုံးတွင် ရှိသည် |

### အကြံပြုထားသော အိုင်ကွန်းများ (Lucide)

| အိုင်ကွန်းအမည် | အသုံးပြုရန် |
|-----------|---------|
| `server` | ဆာဗာ စီမံခန့်ခွဲမှု |
| `shield` | လုံခြုံရေး |
| `database` | ဒေတာဘေ့စ် |
| `activity` | စောင့်ကြည့်မှု |
| `terminal` | Terminal ကိရိယာများ |
| `code` | ဖွံ့ဖြိုးတိုးတက်ရေး |
| `hard-drive` | ဒစ်ခ်/သိုလှောင်မှု |
| `network` | ကွန်ယက် |
| `lock` | အတည်ပြု/Encryption |
| `eye` | ကြည့်ရှု/စောင့်ကြည့်မှု |
| `check-square` | အလုပ်များ/TODO |
| `layout-dashboard` | Dashboard များ |
| `settings` | ကွန်ဖစ်ဂျူရေးရှင်း |
| `zap` | အလိုအလျောက်လုပ်ငန်း |
| `globe` | ဝက်ဘ်/အပြည်ပြည်ဆိုင်ရာ |

1,500+ အိုင်ကွန်းများအားလုံးကို ကြည့်ပါ: [lucide.dev/icons](https://lucide.dev/icons)

---

## အကူအညီလိုပါသလား?

- **GitHub ပြဿနာများ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ပလပ်ဂင် ပြဿနာများ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ဥပမာ ပလပ်ဂင်များ:** [Website](https://wiasoom.com)
- **ဝက်ဘ်ဆိုက်:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>အံ့သြစရာ တစ်ခုခု တည်ဆောက်ပါ။ ကမ္ဘာနှင���် မျှဝေပါ။</em></p>
<p align="center"><em>— WIA SOOM အဖွဲ့</em></p>