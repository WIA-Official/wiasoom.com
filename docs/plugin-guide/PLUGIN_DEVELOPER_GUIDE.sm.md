<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Faaiga lau plugin i le 5 minute.</strong></p>
<p align="center">Faia ni meafaigaluega malosi mo seva, dashboards, ma automations — i totonu o le WIA SOOM.</p>

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

O se plugin "Hello World" e faʻaopoopoina se butona i le sidebar. A e kiliki, e faʻaalia ai se faamatalaga.

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

Toe amata le app (poʻo le suia le plugin i le Settings → Plugins).

E tatau ona e vaʻaia se **"Hello World"** button i le sidebar. Kiliki i ai — e te vaʻaia se faamatalaga manuia!

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

A e valaʻauina le `activate(context)` function, o le `context` (poʻo le `ctx`) e ofoina mai nei APIs:
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

Faʻaulu se faatonuga (poʻo se faamatalaga) i se active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | O le terminal session e faʻaulu i ai |
| `data` | `string` | O le faatonuga poʻo le faamatalaga e faʻaulu |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Faʻaulu i le output uma mai se terminal session. Faʻafoʻi se **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | O le terminal session e mataʻituina |
| `callback` | `(data: string) => void` | Faʻaulu i le vaega uma o le output |
| **Returns** | `() => void` | Faʻaulu lenei e taofi le faalogo |
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
**Important:** Faʻaauau i le teuina o le unsubscribe function ma valaʻau i le `deactivate()` e taofi ai le leakage o le memory.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — O le SFTP API o lo'o fa'ata'ita'iga ae e le'i fa'atekinolosi i le SFTP engine o le app. O le `list()` o lo'o fa'afo'i se array tumu, ma o le `upload()`/`download()` e leai ni galuega. O le a fa'atekinolosi i se fa'asalalauga i le lumana'i. Mo le taimi nei, fa'aaoga le `ctx.terminal.send()` ma le `scp` po'o le `rsync` commands e avea ma se fofo.

#### `sftp.list(sessionId, path)`

Lisi fa'afileo i se fa'avaa mamao.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Fa'aulu se fa'afileo mai le masini i le fa'avaa mamao.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Download se fa'afileo mai le fa'avaa mamao i le masini.
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

Faʻaopoopo se butona i le WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Leai | ID fa'ava-o-mau (default i le igoa o le plugin) |
| `icon` | `string` | Ioe | Igoa o le Lucide icon (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ioe | O le tusitusiga o le butona e faʻaalia i le sidebar |
| `onClick` | `() => void` | Ioe | Faiga e valaʻauina a o e kiliki i le butona |
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

> **Compatibility note:** O nisi o plugins tuai e fa'aoga ai fa'ata'ita'iga i le tulaga e pei o le `addSidebarButton(id, icon, label, onClick)`. O le API aloa'ia e fa'aoga se **options object** e pei ona fa'ailoa i luga. Faʻaoga i taimi uma le faiga o le object mo plugins fou.

#### `ui.openWebview(options)`

Tatala se faamalama pop-up ma le HTML content masani. O lenei auala e fausia ai UIs matagofie.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Faailoga o le faamalama |
| `html` | `string` | O le HTML content atoa e fa'aalia |
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
> Fa'amolemole va'ai i le [Part 3](#part-3-building-custom-ui-with-webviews) mo fa'ata'ita'iga webview maualuga.

#### `ui.showNotification(type, message)`

Fa'aalia se fa'ailoa toast.

| Fa'ailoga | Ituaiga | Fa'amatalaga |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Faiga fa'ailoa |
| `message` | `string` | Tusitusiga e fa'aalia |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Fa'aopoopo se fa'ailoga tusitusiga e tumau i le pito i lalo o le status bar.

| Fa'ailoga | Ituaiga | Fa'amatalaga |
|-----------|------|-------------|
| `id` | `string` | ID fa'avae mo lenei fa'ailoga status |
| `text` | `string` | Tusitusiga e fa'aalia |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Fa'avae tumau

O fa'avae o le plugin e teu i le fa'avae i le `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Fa'amolemole faitau se tau e teuina.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Toe fo'i `undefined` pe afai e le i ai le ki.

#### `settings.set(key, value)`

Teu se tau. E lagolagoina strings, numera, booleans, arrays, ma objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Fa'ata'ita'iga: Manatua filifiliga o le tagata fa'aoga**
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

### `ctx.ai` — AI fa'atekinolosi

> **Fa'amaoniga: O le a sau i le taimi nei** — O le AI API e fa'ata'ita'iga ae e le'i feso'ota'i i Soomy. I le taimi nei e toe fo'i `{ response: 'AI not yet connected' }`. O le fa'atekinolosi AI atoa e fa'amoemoe i se fa'asalalauga i le lumana'i.

#### `ai.chat(messages, options?)`

Fa'asalalau fa'amatalaga i le fesoasoani AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Fa'avae UI Fa'atekinolosi ma Webviews

O le API `openWebview()` e mafai ai ona e fa'avae UI dashboard ma HTML, CSS, ma JavaScript — i totonu o se fa'amaufa'ailoga.

> **Fa'amaonia taua:** O webviews e **fa'aalia na'o le**. E le mafai ona latou toe fa'ailoa i le plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Fa'aaoga butona i le sidebar mo uma gaioiga a le tagata fa'aoga, ma fa'aaoga `openWebview()` e fa'aalia le tulaga o le taimi nei. Afai e te mana'omia fa'avae fa'atekinolosi, fa'ailoa i latou mai butona i le sidebar ma toe fa'avae le webview e fa'aleleia le fa'aalia.

### Fa'ata'ita'iga: Fa'ailoga Terminal → Fa'ata'ita'iga Fa'amaoniga → Fa'aalia i HTML

O le fa'ata'ita'iga plugin e masani ona fa'ata'ita'iga. E te fa'agaioia se fa'ailoga, fa'ata'ita'iga le taunu'uga, ma fa'aalia i se fa'aalia.
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
### Fa'ata'ita'iga: Dashboard Fa'atekinolosi ma Auto-Refresh
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
### Fa'ata'ita'iga: Fa'aalia Fa'avae i se Webview

> **Fa'amatalaga:** O webviews e fa'aalia na'o le — e le mafai ona latou toe fa'ailoa i le plugin APIs. Fa'aaoga `ctx.settings` i au butona i le sidebar e suia fa'avae, ma fa'aaoga `openWebview()` e fa'aalia le tulaga o le taimi nei.
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

## Part 4: Fa'asalalau Lau Plugin

### La'asaga 1: Su'esu'e i le lotoifale

1. Kopy i lau plugin i le `~/.wia-soom/plugins/{your-plugin}/`
2. Toe fa'aleleia le WIA SOOM
3. Fa'amaonia e galue: e fa'aalia le butona i le sidebar, e galue lelei fa'avae
4. Su'esu'e tulaga e le masani ai: o le a le mea e tupu pe afai e le'i feso'ota'i se terminal?

### La'asaga 2: Fa'atulaga mo le fa'asalalau

E tatau ona i ai i lau fa'avaa plugin:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**O le `package.json` e manaʻomia:**

| Fa'amatalaga | Fa'amatalaga | Fa'ata'ita'iga |
|--------------|-------------|----------------|
| `name` | ID e le fa'avae i le kebab-case | `"my-awesome-plugin"` |
| `version` | Fa'ata'ita'iga fa'avae i le fa'avae | `"1.0.0"` |
| `description` | Fa'amatalaga i se laina | `"Monitors nginx access logs in real-time"` |
| `author` | O au igoa | `"John Doe"` |
| `main` | Fa'avae i le fa'avae | `"index.js"` |

**O fa'amatalaga e mafai ona fa'aogaina:**

| Fa'amatalaga | Fa'amatalaga |
|--------------|-------------|
| `license` | Ituaiga laisene (MIT e fautuaina) |
| `keywords` | Fa'ata'ita'iga o taga su'esu'e |
| `soom.minVersion` | Fa'avaaiga WIA SOOM e manaʻomia |

### La'asaga 3: Fa'ailoa i le Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Fa'aopoopo** lau plugin i `plugins/{your-plugin-name}/`
3. **Fa'ailoa** se Pull Request

### La'asaga 4: Iloilo ma le fa'amaonia

Matou te iloiloina uma plugin mo:

- **Saogalemu** — e leai ni APIs e leaga (fa'aaoga [Security Rules](#security-rules))
- **Tulaga** — e faigaluega? E lelei le code?
- **Fa'aaoga** — e fo'ia se fa'afitauli moni?

A uma le fa'amaonia:
1. O lau plugin e fa'aopoopo i `registry.json`
2. O se ZIP bundle e fa'avae i `dist/`
3. O lau plugin e fa'aalia i le **Plugin Store** mo tagata fa'aoga WIA SOOM uma!

---

## Vaega 5: Fa'avae lelei

### Fa'avae Saogalemu

O nei fa'avae e **mana'omia**. O plugin e fa'aleagaina i latou o le a fa'atekinolosi.

| Fa'avae | Aisea |
|---------|-------|
| **E le mafai** ona fa'aogaina `eval()` po'o `new Function()` | Fa'ama'i le code injection |
| **E le mafai** ona fa'aogaina `child_process`, `exec()`, `spawn()` | Fa'amolemole fa'aoga `ctx.terminal.send()` mo fa'atonuga |
| **E le mafai** ona fa'amaonia URL i fafo | Fa'avae: `wiasoom.com` API endpoints |
| **E le mafai** ona fa'amaonia `process.env` | E mafai ona i ai fa'amaumauga i le si'osi'omaga |
| **E le mafai** ona fa'aogaina `require('fs')` i se auala ta'itasi | Fa'aoga `ctx.settings` mo le teuina, `ctx.sftp` mo le fa'amaumau fa'ailoa |
| **E le mafai** ona fa'aoga npm external packages | JavaScript na'o le — e leai ni node_modules |
| **E tatau** ona fa'aoga `ctx.terminal.send()` mo fa'atonuga mamao uma | O le a fa'agaioia i le auala saogalemu SSH |
| **E tatau** ona fa'asa'o i `deactivate()` | Avea le fa'ailoa, fa'amaonia fa'atinoga |

### Fa'amaonia le Mea sese

Fa'amolemole fa'amaonia uma gaioiga e mafai ona i ai le risiki i le try/catch:
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
### Fa'asa'o i le deactivate()

Afai e fa'avae i lau plugin fa'atinoga, fa'ailoa, po'o fa'amaumauga — fa'asa'o i latou:
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
### i18n Fa'avae

WIA SOOM e lagolagoina 254 gagana. E fa'avae i le fa'avae o lau plugin e mafai ona fa'ailoa, fa'aoga se auala faigofie:
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

## Vaega 6: Fa'ata'ita'iga i le Olaga Moni

### Fa'ata'ita'iga 1: Fa'ata'ita'iga Disk Server

Fa'agaoioi `df -h` i le server mamao ma fa'aalia le avanoa e fa'aaoga/avanoa i le status bar.
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

### Fa'ata'ita'iga 2: Fa'atonu TODO

O se plugin e fa'atonu se lisi TODO e fa'aoga fa'avae mo le teuina tumau ma se webview mo le fa'aalia.

> **Fa'avae i le mamanu:** E le mafai e le webviews ona fa'atonu i APIs o le plugin, o lenei plugin e fa'aoga se "snapshot" fa'avae — e faitau TODOs mai fa'avae, e fa'aalia i le HTML e le mafai ona fa'atonu, ma e ofoina atu gaioiga e fa'avae i le sidebar mo le fa'aopoopo i fa'ata'ita'iga. O le webview o se **fa'aalia** laupapa, e le o se fomu fa'atekinolosi.
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

### Fa'ata'ita'iga 3: Fa'ata'ita'iga Sese

E fa'amaonia le fa'amaumauga i le terminal ma e fa'ailoa se fa'ailoa pe a maua ni fa'ata'ita'iga patino.
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

Fa'aaoga nei i lau `package.json` `keywords` po'o le fa'aogaina i le fa'amaonia i le registry:

| Category | Description |
|----------|-------------|
| `server` | Fa'amalumalu i le server |
| `devtools` | Ogaoga fa'atekinolosi |
| `calculator` | Fa'ata'ita'iga ma fa'avaita'iga |
| `simulator` | Fa'ata'ita'iga |
| `game` | Ta'aloga i le terminal |
| `business` | Ogaoga fa'avaomalo |
| `security` | Saogalemu ma le su'esu'e |
| `web` | Fa'amalumalu i le web server |
| `education` | Ogaoga fa'atekinolosi |
| `health` | Ogaoga e fa'atatau i le soifuaga |
| `islamic` | Ogaoga Islam (taimi o le ta'ele, ma isi) |
| `science` | Ogaoga fa'asaienisi |
| `quantum` | Ogaoga fa'atekinolosi quantum |
| `ai` | Ogaoga e fa'atekinolosi AI |
| `biotech` | Ogaoga fa'atekinolosi biotechnolosi |
| `space` | Ogaoga i le vateatea ma le astromoni |
| `network` | Ogaoga i le netiwork |
| `database` | Fa'amalumalu i le database |
| `monitoring` | Su'esu'e i le server |
| `devops` | DevOps ma CI/CD |
| `utility` | Ogaoga fa'atekinolosi |
| `design` | Ogaoga fa'ata'ita'iga |
| `ecommerce` | Ogaoga e fa'atau |
| `automation` | Ogaoga fa'atekinolosi fa'atekinolosi |
| `kpop` | Ogaoga e fa'atatau i le K-pop |
| `accessibility` | Ogaoga e mafai ona fa'aaoga |
| `analytics` | Fa'ata'ita'iga ma le lipoti |
| `wia` | Ogaoga i le WIA ecosystem |
| `all` | E foliga mai i vaega uma |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Fa'amalumalu i le server |
| `shield` | Saogalemu |
| `database` | Database |
| `activity` | Su'esu'e |
| `terminal` | Ogaoga i le terminal |
| `code` | Fa'atekinolosi |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Fa'amaonia/encryption |
| `eye` | Va'aiga/su'esu'e |
| `check-square` | Ta'iala/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Fa'avae |
| `zap` | Fa'atekinolosi fa'atekinolosi |
| `globe` | Web/fa'ava-o-mālamalama |

Fa'avaa i le 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Fa'avae se mea ofoofogia. Fa'asoa atu i le lalolagi.</em></p>
<p align="center"><em>— O le Au WIA SOOM</em></p>