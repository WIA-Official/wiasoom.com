<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Oqaatsit 5 minutimi plugin-ititit.</strong></p>
<p align="center">Suliassat pingaarutillit server-it, dashboard-it, aammalu automations — WIA SOOM-imi.</p>

---

## Taaguut

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

### Qanoq atuarsinnaavat

"Hello World" plugin, sidebar-imut button-itik atit. Klikkerit, notification-itik saqqummersarpoq.

### Step 1: Plugin folder-itik pilersuiffik
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Step 2: package.json pilersuiffik
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
**Pillarsuit pisariaqartinneqartut:** `name`, `version`, `description`, `author`, `main`

### Step 3: index.js pilersuiffik
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
### Step 4: WIA SOOM-it restart

App-it restart-it (imaluunniit plugin-it Settings → Plugins-imi off/on-it).

Sidebar-imi **"Hello World"** button-itik takusinnaavat. Klikkerit — angusaq notification-itik takusinnaavat!

### Qanoq atuarpoq
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

`activate(context)` function-itik atorlugu, `context` (imaluunniit `ctx`) API-itik uku tunniussinnaavai:
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

### `ctx.terminal` — Remote servers-imi command-itik ingerlati

#### `terminal.send(sessionId, data)`

Command (imaluunniit data) aktivi terminal session-itik nassiussinnaavat.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Nassiussinnaasup terminal session-it |
| `data` | `string` | Nassiussinnaasup command-it imaluunniit data-it |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Terminal session-itik output-itik tamaasa nassaassaavat. **unsubscribe function**-it tunniussinnaavat.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Nassaassinnaasup terminal session-it |
| `callback` | `(data: string) => void` | Output-itik tamaasa nassaassinnaavat |
| **Nassaarneq** | `() => void` | Nassiussinnaavat nassaassinnaajunnaarsinnaavat |
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
**Piginnaasat:** Unsubscribe function-itik sempre-save-it, `deactivate()`-imi atorlugu nassaassinnaajunnaarsinnaavat.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — SFTP API-ip aalajangersimasuupput, kisianni app-ip SFTP engine-itik atorlugu suliarineqanngilaq. `list()` immaqaluinnaq empty array-it tunniussinnaavaa, `upload()`/`download()`-it immaqaluinnaq no-ops. Taanna siullermik saqqummiunneqassaaq. Uanga, `ctx.terminal.send()` atorlugu `scp` imaluunniit `rsync` command-itik atorlugu sulissutigineqarsinnaavoq.

#### `sftp.list(sessionId, path)`

Remote directory-imi files-it nassaassaavat.
§§§CHUNK_SEPARATOR��§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Local machine-itik remote server-itik file-it nassiussinnaavat.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

Remote server-itik local machine-itik file-it nassaassinnaavat.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**Suliarineq (SFTP API-ip atuutilernerani):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

WIA SOOM sidebar-imut button-itik ilanngussinnaavat.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Na | Unik ID (plugin name-itik malillugu defaults) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Sidebar-imi button text-itik |
| `onClick` | `() => void` | Yes | Button-it klikkerit atorlugu function-itik |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Icon reference:** Available icons-itik tamaasa takusinnaavat [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Qaarsoq older plugins positional arguments-itik atorlugit `addSidebarButton(id, icon, label, onClick)`. Official API **options object**-imik atorlugu atuuppoq, uanga. Plugin-itik nutaat atorlugu object style-itik atorlugu atuinnassavat.

#### `ui.openWebview(options)`

Popup window-itik custom HTML content-itik atorlugu atorlugu. Taanna rich UIs-itik pilersuiffik. 

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content-itik atorlugu |
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
> Naatsorsuutig [Part 3](#part-3-building-custom-ui-with-webviews) pillugu webview-it pillugit annertunerusumik.

#### `ui.showNotification(type, message)`

Taasinissamut nalunaarut.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Nalunaarut stili |
| `message` | `string` | Taasissutissaq |
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

Tassunga ilanngullugu tekstip item-it siornatigut status barimut.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unika ID tassa status item-imut |
| `text` | `string` | Tusagassiaq |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Siunertaq

Plugin settings-it `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`-imi siornatigut katillugit inissinneqassapput.

#### `settings.get(key)`

Tassunga ilanngullugu toqqorsivimmiittup akia akuerisassanngorlugu.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Key-i atorlugu akuerisassanngitsoq `undefined`-imut akissuteqarpoq.

#### `settings.set(key, value)`

Akia toqqorsivimmut katillugit. String-it, nummerit, booleanit, arrays, aamma objects-it akuerisassanngorlugit.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Misissue: User preferences-it eqqaamajuk**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — AI ilanngussaq

> **Status: Nerisassanngitsoq** — AI API-p aalajangersarneqarsimasoq, kisianni Soomy-mut ilanngunneqanngilaq. Aammattaaq `{ response: 'AI not yet connected' }` akissuteqarpoq. AI ilanngussaq siornatigut saqqummiunneqassaaq.

#### `ai.chat(messages, options?)`

AI-assistentimut (Soomy) nalunaarut.
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

## Part 3: Webviews-imut UI-nik ineriartortitsineq

`openWebview()` API-p HTML, CSS, aamma JavaScript atorlugit dashboard UI-nik ineriartortitsinissamut periarfissaq siuarsarpoq — tamakku popup window-imi.

> **Aammattaaq pingaaruteqarpoq:** Webviews **tusagassiaq**-ut. Tamatigut plugin API-imut ( `ctx.settings`, `ctx.terminal`, aamma all.) akissuteqarani. User akioneranut sidebar button-it atorneqassapput, aamma `openWebview()` atorlugu kingullermik inissinneqassaaq. Interaktive features-it pisariaqartippat, sidebar button-it atorlugit aallartit, aamma webview kingulleq atorlugu saqqummiussaq.

### Pattern: Terminal Command → Parse Output → Show in HTML

Tassunga ilanngullugu plugin pattern-i amerlanerpaamik atorneqartoq. Command-i atorlugu, neriorsuutig, aamma takussutissaq saqqummiunneqassaaq.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Pattern: Interaktive Dashboard with Auto-Refresh
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
### Pattern: Settings-it webview-imi takussutissaq

> **Aammattaaq:** Webviews tusagassiaq-upput — plugin API-imut akissuteqarani. `ctx.settings` sidebar button-it atorlugit settings-it allanngortissinnaavat, aamma `openWebview()` atorlugu kingullermik takussutissaq.
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

## Part 4: Plugin-it saqqummersitsineq

### Step 1: Lokalimi misissuineq

1. Plugin-it `~/.wia-soom/plugins/{your-plugin}/`-imut nuunneq.
2. WIA SOOM nutartere.
3. Suliassap ilisarit: sidebar button-i takussutissaq, features-it akuerisassanngorlugit.
4. Edge cases-it misissuineq: terminal-i atorlugu qanoq pisoq?

### Step 2: Submission-imut piareersarneq

Plugin-it folder-it tassa atorlugu:
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
**Piginnaasat `package.json` immikkoortut:**

| Immikkoortut | Oqaatsit | Misilittagaq |
|-------------|----------|--------------|
| `name` | Unikaalimik kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | Oqaatsit ataatsimik | `"Monitors nginx access logs in real-time"` |
| `author` | Atit | `"John Doe"` |
| `main` | Aallartitaq | `"index.js"` |

**Piginnaasat:**

| Immikkoortut | Oqaatsit |
|-------------|----------|
| `license` | Licensit type (MIT siunnersuutigineqarpoq) |
| `keywords` | Qsearch tags array |
| `soom.minVersion` | WIA SOOM versionit minimumi pisariaqartoq |

### Step 3: Plugin Registry-mut nassiussineq

1. ****Package** your plugin as a ZIP file
2. **Add** plugin-itit `plugins/{your-plugin-name}/`
3. **Submit** Pull Request

### Step 4: Naggataarutaq aamma akuersissuteq

Uani plugin-it tamaasa nalilersorneqassapput:

- **Aammattaaq** — API-it ajortumik atorneqarsinnaanerat (naatsorsuutig [Aammattaaq Quleq](#security-rules))
- **Kvalitet** — sulivoq? Kodeq nakkutigeq? 
- **Atuisut** — isumannaatsumik aaqqiissutissaq?

Akuerineqarnermini:
1. Plugin-itit `registry.json`-imut ilanngunneqassapput
2. ZIP bundle `dist/`-mi pilersinneqassaaq
3. Plugin-itit **Plugin Store**-mi WIA SOOM atuisut tamarmik pillugit takuneqassapput!

---

## Part 5: Ilitsersuutit

### Aammattaaq Quleq

Ilaqutariit taakku **piginnaasat**-nik. Ilaqutariit ajortumik atorneqarsinnaanerat akuerineqassanngilaq.

| Ilaqutariit | Aammattaaq |
|-------------|------------|
| **MAANILU** `eval()` imaluunniit `new Function()` atorlugit | Kodeq nakkutigeq ajortumik |
| **MAANILU** `child_process`, `exec()`, `spawn()` atorlugit | `ctx.terminal.send()` atorlugu qaffasissumik |
| **MAANILU** URL-it externit atorlugit | Ajortumik: `wiasoom.com` API endpoints |
| **MAANILU** `process.env` atorlugu | Aallartitaq variables ajortumik atorlugit |
| **MAANILU** `require('fs')` siullermik atorlugu | `ctx.settings` atorlugu qaffasissumik, `ctx.sftp` atorlugu filit qaffasissumik |
| **MAANILU** npm externit packages atorlugit | Pure JavaScript nikanngitsoq — node_modules atorlugit |
| **MAANILU** `ctx.terminal.send()` atorlugu tamarmik qaffasissumik | Taanna secure SSH channel aqqutigalugu |
| **MAANILU** `deactivate()`-mi nakkutigeq | Nakkutigeq listeners, clear intervals |

### Aammattaaq Quleq

Sumiiffiit ajortumik atorlugit try/catch-ikkut:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Nakkutigeq `deactivate()`-mi

Plugin-itit intervals, listeners, imaluunniit subscriptions pilersitillugit — nakkutigeq:
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
### i18n Aammattaaq

WIA SOOM 254 oqaasillit atorlugu. Plugin-ititit atit nakkutigeq atorlugu, ilitsersuutit atorlugit:
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

## Part 6: Realaalikkut Misilittakkat

### Misilittagaq 1: Server Disk Checker

`df -h` remote server-imi sulissutigineqassaaq, status bar-imi atorneqartoq/atuisut takuneqassaaq.
§§§CHUNK_SEPARATOR§§§
---

### Misilittagaq 2: TODO Manager

Plugin-itit TODO list-it nakkutigeq atorlugit, persistent storage atorlugu settings, aammalu webview atorlugu takussutissi.

> **Design pattern:** Webviews plugin APIs-it siullermik atorlugit, plugin-itit "snapshot" atorlugu — TODOs settings-itit atorlugit, HTML-ikkut akuerineqarsinnaanera, aammalu sidebar-based actions atorlugit items-itit ilanngullugit. Webview **takussutissi** qaffasissumik, ajortumik form.
§§§CHUNK_SEPARATOR§§§
---

### Misilittagaq 3: Error Watcher

Terminal output nakkutigeq, aammalu ajortumik patterns takuneqarsimasut, notification-itit nassiunneqassapput.
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

## Appendix: Kategoriat & Ikonit

### Plugin Kategoriat (29)

Uani atorneqarsinnaapput `package.json` `keywords` imaluunniit registrimi ilanngussamik:

| Kategori | Oqaatsit |
|----------|-------------|
| `server` | General server management |
| `devtools` | Development tools |
| `calculator` | Kalkulatorit aamma allannguuteqartit |
| `simulator` | Simulators |
| `game` | Terminalimik qimussimik |
| `business` | Business tools |
| `security` | Security aamma auditing |
| `web` | Web server management |
| `education` | Ilinniartitaanermut atortussat |
| `health` | Suliassat peqqinnissamut tunngatillugu |
| `islamic` | Islamimut tunngasunik (prædika, aamma allat) |
| `science` | Siunnersuisut |
| `quantum` | Quantum computing tools |
| `ai` | AI-pitortumik atortussat |
| `biotech` | Bioteknologi atortussat |
| `space` | Space aamma astronomi atortussat |
| `network` | Network tools |
| `database` | Database management |
| `monitoring` | Server monitoring |
| `devops` | DevOps aamma CI/CD |
| `utility` | General utilities |
| `design` | Design tools |
| `ecommerce` | E-commerce tools |
| `automation` | Automation tools |
| `kpop` | K-pop-imut tunngasunik |
| `accessibility` | Accessibility tools |
| `analytics` | Analytics aamma nalunaarut |
| `wia` | WIA ecosystem tools |
| `all` | Kategoriat tamaasa ilanngullugit |

### Siunnersuutigineqartut Ikonit (Lucide)

| Ikon Nammineq | Atorukku |
|---------------|---------|
| `server` | Server management |
| `shield` | Security |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Terminal tools |
| `code` | Development |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Nakkutilliineq/monitoring |
| `check-square` | Sulisut/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuration |
| `zap` | Automation |
| `globe` | Web/international |

Sumiiffimmi 1,500+ ikonit: [lucide.dev/icons](https://lucide.dev/icons)

---

## Piffissaq pisariaqartit?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Misilittakkat Pluginit:** [Website](https://wiasoom.com)
- **Websait:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Immaqaluunniit pitsaanerpaamik pilersuiffik. Verfittuk tamarmiusut.</em></p>
<p align="center"><em>— WIA SOOM Team</em></p>
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
