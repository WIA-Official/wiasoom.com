<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Faka'osi ho'o plugin i he 5 miniti.</strong></p>
<p align="center">Faka'osi ngaahi me'angaue ma'ulalo, dashboards, mo ngaahi automations — i he WIA SOOM.</p>

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

E "Hello World" plugin e faka'aonga'i e button ki he sidebar. Ka toe 'alu, e faka'ava e notification.

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

Faka'osi e app (pe toggle e plugin off/on i he Settings → Plugins).

Te ke ikuna e **"Hello World"** button i he sidebar. Click e — te ke ikuna e notification faka'osi!

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

Ka e faka'osi e `activate(context)` function, e faka'ava e `context` (pe `ctx`) e ngaahi API ko'eni:
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

Faka'osi e command (pe ha data) ki he active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | E terminal session ke faka'osi ki ai |
| `data` | `string` | E command pe data ke faka'osi |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Faka'osi ki he output kotoa mai he terminal session. Faka'ava e **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | E terminal session ke mata'i |
| `callback` | `(data: string) => void` | Faka'osi e kotoa e chunk output |
| **Returns** | `() => void` | Faka'osi e ki he stop listening |
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
**Important:** Faka'osi e unsubscribe function kotoa mo faka'osi e i he `deactivate()` ke ta'e faka'osi e memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — E faka'osi e SFTP API ka 'ikai ke toe faka'osi ki he app's SFTP engine. `list()` ko'eni e faka'osi e empty array, mo e `upload()`/`download()` ko'eni e no-ops. Ko e me'a ko'eni e faka'osi lelei i he release ko e toe. Kae, faka'aonga'i e `ctx.terminal.send()` mo e `scp` pe `rsync` commands ke fai e workaround.

#### `sftp.list(sessionId, path)`

Faka'osi e ngaahi faila i he remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Faka'osi e faila mai he local machine ki he remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Faka'osi e faila mai he remote server ki he local machine.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (ki he SFTP API ko e live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Faka'osi e button ki he WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (defaults to plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Button text shown in sidebar |
| `onClick` | `() => void` | Yes | Function called when button is clicked |
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
**Icon reference:** Faka'ava e ngaahi icons kotoa i he [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Ko e ngaahi plugin tu'a e faka'aonga'i e positional arguments meimei `addSidebarButton(id, icon, label, onClick)`. E faka'osi e official API e **options object** ko e faka'osi i lalo. Faka'aonga'i e object style ki he ngaahi plugin fo'ou.

#### `ui.openWebview(options)`

Faka'osi e popup window mo e custom HTML content. Ko e me'angaue ko e faka'osi e ngaahi UIs ma'ulalo. 

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content ke faka'osi |
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
> Kuo faingata'a [Part 3](#part-3-building-custom-ui-with-webviews) mo e ngaahi pateni webview advanced.

#### `ui.showNotification(type, message)`

Faka'atu e toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Kakala notification |
| `message` | `string` | Tohi ke faka'atu |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Faka'atu e item tohi ta'e fakafoki ki he status bar lalo.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID tu'uta ki he item status ko 'eni |
| `text` | `string` | Tohi ke faka'atu |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Faka'atu ki he tu'unga

Ko e ngaahi settings plugin e faka'atu ki he taimi kotoa pe i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Fakafoki e tohi kehe.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Faka'atu `undefined` ki he taimi ko e key 'oku 'ikai ke ma'u.

#### `settings.set(key, value)`

Faka'atu e tohi. Faka'ava ki he ngaahi string, numbers, booleans, arrays, mo e objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Fakamatala: Kuo ma'u e ngaahi fiema'u 'o e user**
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

> **Tu'unga: Teu mai** — Ko e AI API 'oku faka'ata'anga, ka 'oku 'ikai ke toe fakafoki ki Soomy. Kuo faka'atu `{ response: 'AI not yet connected' }`. Ko e AI integration kotoa 'oku faka'ata'anga ki he release 'o e taimi e toe hiki.

#### `ai.chat(messages, options?)`

Faka'atu e ngaahi tohi ki he AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Faka'atu e UI Fakafonua mo e Webviews

Ko e `openWebview()` API 'oku fa'a tokoni kiate koe ke faka'atu e dashboard UIs mo e HTML, CSS, mo e JavaScript — kotoa i he popup window.

> **Faka'ava: Faka'ava lahi:** Ko e webviews 'oku **faka'atu pe**. 'Ikai ke nau faka'atu ki he plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Faka'atu e sidebar buttons ki he ngaahi action 'o e user kotoa, mo e faka'atu e `openWebview()` ki he tu'unga o e taimi. Kapau 'oku ke fiema'u e ngaahi feature interactive, faka'atu 'a ia mai he sidebar buttons mo e toe faka'atu e webview ke toe faka'atu e faka'atu.

### Pateni: Terminal Command → Parse Output → Faka'atu i he HTML

Ko e pateni plugin ko 'eni 'oku faingata'a lahi. Kuo ke fa'a hoko e command, parse e resulta, mo e faka'atu ia visually.

§��§CHUNK_SEPARATOR§§§

### Pateni: Interactive Dashboard mo e Auto-Refresh
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
### Pateni: Faka'atu e Settings i he Webview

> **Fakamatala:** Ko e webviews 'oku faka'atu pe — 'ikai ke nau faka'atu ki he plugin APIs. Faka'atu e `ctx.settings` i he sidebar button handlers ke fakafoki e settings, mo e faka'atu e `openWebview()` ki he tu'unga o e taimi.
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

## Part 4: Faka'atu e Plugin

### Tohi 1: Faka'atu i he lokali

1. Kuo faka'atu e plugin ki he `~/.wia-soom/plugins/{your-plugin}/`
2. Toe faka'atu e WIA SOOM
3. Faka'atu e 'ikai ke ma'u: 'oku 'i ai e sidebar button, 'oku lelei e ngaahi feature
4. Faka'atu e ngaahi edge cases: ko e hā e hoko ki he taimi 'oku 'ikai ke ma'u e terminal?

### Tohi 2: Faka'atu ki he faka'ava

Ko e folder plugin 'oku tatau ke ma'u:
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
**Ko e ngaahi `package.json` ngaahi tuʻunga:**

| Tuʻunga | Fakamatala | Fakakaukau |
|---------|------------|------------|
| `name` | ID kebab-case ko e taha | `"my-awesome-plugin"` |
| `version` | Version semantiki | `"1.0.0"` |
| `description` | Fakamatala kotoa ki he taha | `"Monitors nginx access logs in real-time"` |
| `author` | Ko hoʻo hingoa | `"John Doe"` |
| `main` | Taimi fakaʻilonga | `"index.js"` |

**Ngaahi tuʻunga ʻikai ko e ngaahi tuʻunga:**

| Tuʻunga | Fakamatala |
|---------|------------|
| `license` | Tohi faingataʻa (MIT ko e faingataʻa lelei) |
| `keywords` | Fakaʻilonga ʻo e ngaahi taga fakaʻilonga |
| `soom.minVersion` | Ko e WIA SOOM tuʻunga lahi e manaʻomia |

### Tohi 3: Tuku ki he Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** hoʻo plugin ki `plugins/{your-plugin-name}/`
3. **Submit** he Pull Request

### Tohi 4: Fakatokanga mo e fakaʻilonga

Te tau fakaʻilonga kotoa ki he plugin:

- **Fakamālie** — ʻikai ha ngaahi API fakamālohi (fakaʻilonga [Security Rules](#security-rules))
- **Quality** — ko e faingataʻa? Ko e code ko e mālohi?
- **Usefulness** — ko e faingataʻa e toe faingataʻa?

Koeʻuhi ko e fakaʻilonga:
1. Ko hoʻo plugin e tuku ki `registry.json`
2. Ko e ZIP bundle e fakahoko ki he `dist/`
3. Ko hoʻo plugin e fakaʻilonga ki he **Plugin Store** ki he ngaahi tagata ʻi he WIA SOOM!

---

## Vahe 5: Ngaahi Fakaako lelei

### Ngaahi Fakaako Fakamālie

Ko e ngaahi fakaako ko ʻeni **ko e manaʻomia**. Ko e ngaahi plugin e fakamālie ʻi he ngaahi fakaako ko ʻeni e tuku.

| Fakaako | Ko e hā |
|---------|---------|
| **ʻIKAI** fakaʻaonga `eval()` pe `new Function()` | Ko e risiki fakaʻi e code |
| **ʻIKAI** fakaʻaonga `child_process`, `exec()`, `spawn()` | Fakaʻaonga `ctx.terminal.send()` mo e ngaahi kau fakaʻilonga |
| **ʻIKAI** fakaʻaonga e ngaahi URL ʻi hena | Ko e faingataʻa: `wiasoom.com` API endpoints |
| **ʻIKAI** fakaʻaonga `process.env` | Ko e ngaahi tuʻunga fonua e mafai ke maʻu e ngaahi mea lele |
| **ʻIKAI** fakaʻaonga `require('fs')` ko e toʻonga | Fakaʻaonga `ctx.settings` ki he taʻu, `ctx.sftp` ki he fakaʻilonga faila |
| **ʻIKAI** fakaʻaonga e ngaahi pakage npm | JavaScript ko e pure — ʻikai ha node_modules |
| **MAU** fakaʻaonga `ctx.terminal.send()` ki he ngaahi kau fakaʻilonga ʻi hena | Ko e ʻeni e ʻalu ki he SSH channel fakamālohi |
| **MAU** fakaʻata ki he `deactivate()` | Fakaʻilonga e ngaahi listener, fakaʻata e ngaahi interval |

### Fakaʻilonga e Ngaahi Fakaʻilonga

Fakaʻaonga e ngaahi ngāue risiki kotoa ki he try/catch:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Fakaʻata ki he deactivate()

Koeʻuhi ko hoʻo plugin e fakaʻilonga e ngaahi interval, listeners, pe subscriptions — fakaʻata e ngaahi ʻeni:
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
### i18n Fakaʻilonga

Ko e WIA SOOM e fakaʻilonga e 254 lea. Ki he fakaʻilonga e label ʻo hoʻo plugin ke mafai ke fakaʻilonga, fakaʻaonga e founga maʻulalo:
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

## Vahe 6: Ngaahi Fakakaukau ʻo e Moʻui

### Fakakaukau 1: Server Disk Checker

Fakaʻaonga `df -h` ki he server fakamālohi mo e fakaʻilonga e ngaahi faingataʻa/maʻu ki he status bar.
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

### Fakakaukau 2: TODO Manager

Ko e plugin e puleʻi e TODO list fakaʻaonga e ngaahi settings ki he taʻu fakamālohi mo e webview ki he fakaʻilonga.

> **Fakaako fakaʻilonga:** Koeʻuhi ko e ngaahi webviews e ʻikai ke mafai ke fakaʻaonga e plugin APIs, ko e plugin ko ʻeni e fakaʻaonga e "snapshot" fakaʻilonga — e ʻi ai e TODOs mai he ngaahi settings, e fakaʻilonga e ngaahi mea ko ia ko e HTML ʻi he fakaʻilonga, mo e fakaʻaonga e ngaahi ngāue ki he sidebar ki he fakaʻata e ngaahi ʻitem. Ko e webview ko e **fakaʻilonga** layer, ʻikai ko e fomu fakaʻilonga.
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

### Fakakaukau 3: Error Watcher

Fakaʻilonga e ngaahi output terminal mo e tuku e fakaʻilonga kehekehe ko e ngaahi pateni ko ia e fakaʻilonga.
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

## Appendix: Kātāgā & Ikoni

### Kātāgā o e Plugin (29)

Utilize these in your `package.json` `keywords` or when submitting to the registry:

| Kātāgā | Fakamatala |
|--------|------------|
| `server` | Kākā o e pule'anga |
| `devtools` | Ngāue'anga faka'anga |
| `calculator` | Kākāla mo e fakamālohi |
| `simulator` | Kākāla fakakaukau |
| `game` | Ngāue'anga terminal |
| `business` | Ngāue'anga kāinga |
| `security` | Tū'ā mo e fakamālohi |
| `web` | Kākā o e pule'anga web |
| `education` | Ngāue'anga ako |
| `health` | Ngāue'anga mo e ma'umaumau |
| `islamic` | Ngāue'anga Islam (nōnō taimi, etc.) |
| `science` | Ngāue'anga faka'anga |
| `quantum` | Ngāue'anga quantum computing |
| `ai` | Ngāue'anga faka'āu |
| `biotech` | Ngāue'anga biotechnology |
| `space` | Ngāue'anga mo e vālea |
| `network` | Ngāue'anga network |
| `database` | Kākā o e pule'anga database |
| `monitoring` | Fakamālohi o e server |
| `devops` | DevOps mo e CI/CD |
| `utility` | Ngāue'anga faka'āu |
| `design` | Ngāue'anga faka'anga |
| `ecommerce` | Ngāue'anga e-commerce |
| `automation` | Ngāue'anga fakamālohi |
| `kpop` | Ngāue'anga K-pop |
| `accessibility` | Ngāue'anga accessibility |
| `analytics` | Ngāue'anga mo e fakamatala |
| `wia` | Ngāue'anga WIA |
| `all` | Fakakite ki he ngaahi kātagā kotoa |

### Ikoni Faka'ata (Lucide)

| Faka'ata Ikoni | Ngāue'anga |
|----------------|------------|
| `server` | Pule'anga server |
| `shield` | Tū'ā |
| `database` | Database |
| `activity` | Fakamālohi |
| `terminal` | Ngāue'anga terminal |
| `code` | Faka'anga |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Faka'ata/fakamālohi |
| `check-square` | Ngaahi ngāue/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Faka'ata |
| `zap` | Fakamālohi |
| `globe` | Web/international |

Fakakaukau ki he ngaahi ikoni 1,500+: [lucide.dev/icons](https://lucide.dev/icons)

---

## Faka'amu?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Faka'anga e ngaahi me'a ma'ama. Faka'amu ki he lalolagi.</em></p>
<p align="center"><em>— Ko e Kautaha WIA SOOM</em></p>
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
