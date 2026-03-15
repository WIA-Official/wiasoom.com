<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Vula plugin ya wena hi mintlhontlho ya 5.</strong></p>
<p align="center">Hlela switlhavi swo olova, dashboards, na automations — hi le WIA SOOM.</p>

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

A "Hello World" plugin that adds a button to the sidebar. When clicked, it shows a notification.

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

Restart the app (or toggle the plugin off/on in Settings → Plugins).

You should see a **"Hello World"** button in the sidebar. Click it — you'll see a success notification!

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

When your `activate(context)` function is called, `context` (or `ctx`) provides these APIs:
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

Send a command (or any data) to an active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | The terminal session to send to |
| `data` | `string` | The command or data to send |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscribe to all output from a terminal session. Returns an **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | The terminal session to watch |
| `callback` | `(data: string) => void` | Called with each chunk of output |
| **Returns** | `() => void` | Call this to stop listening |
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

> **Status: Coming Soon** — The SFTP API is defined but not yet wired to the app's SFTP engine. `list()` currently returns an empty array, and `upload()`/`download()` are no-ops. This will be fully implemented in a future release. For now, use `ctx.terminal.send()` with `scp` or `rsync` commands as a workaround.

#### `sftp.list(sessionId, path)`

List files in a remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Upload a file from local machine to remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Download a file from remote server to local machine.
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

Add a button to the WIA SOOM sidebar.

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
**Icon reference:** Browse all available icons at [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Some older plugins use positional arguments like `addSidebarButton(id, icon, label, onClick)`. The official API uses an **options object** as documented above. Always use the object style for new plugins.

#### `ui.openWebview(options)`

Open a popup window with custom HTML content. This is how you build rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content to render |
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
> Hlanganisa [Part 3](#part-3-building-custom-ui-with-webviews) hi swifaniso swa webview leswi nga le henhla.

#### `ui.showNotification(type, message)`

Show a toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Mavonelo ya notification |
| `message` | `string` | Xifaniso lexi faneleke ku voniwa |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Hlanganisa xifaniso xa nkarhi hinkwawo endzhaku ka status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID leyi yinhle ya xifaniso lexi |
| `text` | `string` | Xifaniso lexi faneleke ku voniwa |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Ku hlayisa nkarhi hinkwawo

Mavonelo ya plugin ma hlayisiwa hi nkarhi wo leha eka `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Funda xiphemu lexi hlayisiweke.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
I `undefined` loko key a nga kona.

#### `settings.set(key, value)`

Hlayisa xiphemu. I tinyiko ta strings, numbers, booleans, arrays, na objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**M exemplo: Khumbula swikombiso swa murhangeri**
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

### `ctx.ai` — Ku hlanganisa AI

> **Mavonelo: Ku ta fika** — AI API yi hlamuseriwe kambe a yi hlanganisiwi na Soomy. Ku humelela `{ response: 'AI not yet connected' }`. Ku hlanganisiwa ka AI ku planeriwa hi ku tlanga ka nkarhi lowu taka.

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

## Part 3: Ku Hlanganisa UI ya Ntlhanu na Webviews

API `openWebview()` yi ku letela ku hlanganisa dashboard UIs hi HTML, CSS, na JavaScript — hinkwaswo ngaphakathi ka windo ya popup.

> **Mavonelo yo Hlula:** Webviews i **ku voniwa ntsena**. A nga na ku ringeta ku ya eka API ya plugin (`ctx.settings`, `ctx.terminal`, etc.). Hlanganisa mabutton ya sidebar ku endla swihoxo hinkwaswo, na ku tirhisa `openWebview()` ku vonisa state leyi nga kona. Loko u lava swihoxo leswi hlanganisiweke, vumba ku va na mabutton ya sidebar na ku tlhela u vula webview ku pfuxeta ku voniwa.

### Pattern: Terminal Command → Parse Output → Show in HTML

Lesi i swifaniso leswi endlaka plugin. U endla xcommand, u hlawula result, na u vonisa hi ndlela yo vonaka.
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
### Pattern: Ku Vonisa Settings eka Webview

> **Xikombiso:** Webviews i ku voniwa ntsena — a nga na ku ringeta ku ya eka API ya plugin. Hlanganisa `ctx.settings` eka swihlanganisi swa mabutton ya sidebar ku lunghisa settings, na ku tirhisa `openWebview()` ku vonisa state leyi nga kona.
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

## Part 4: Ku Hlanganisa Plugin Ya Wena

### Nhlayo 1: Hlanganisa hi ndzhaka

1. Kopi plugin ya wena ku ya eka `~/.wia-soom/plugins/{your-plugin}/`
2. Vusisa WIA SOOM
3. Tlhantlha leswaku i ntirho: button ya sidebar yi vonaka, swihoxo swi humelela kahle
4. Hlanganisa swiphemu leswi nga le henhla: yini leyi humelela loko a ku na terminal leyi hlanganisiweke?

### Nhlayo 2: Lunghelela ku rhangisa

Folda ya plugin ya wena yi fanele yi katsa:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Mafambiselo ya `package.json` ya nkoka:**

| Nhlangano | Tlhahlobo | Mfanekiso |
|-----------|-----------|-----------|
| `name` | ID ya kebab-case yo unikela | `"my-awesome-plugin"` |
| `version` | Version ya semantic | `"1.0.0"` |
| `description` | Tlhahlobo ya xihlawuhlawu | `"Monitors nginx access logs in real-time"` |
| `author` | Ndzawulo ya wena | `"John Doe"` |
| `main` | Ntlhanu ya ku nghena | `"index.js"` |

**Mafambiselo ya swakudya:**

| Nhlangano | Tlhahlobo |
|-----------|-----------|
| `license` | Nhlangano ya vuhlayiseki (MIT yi khomiwa) |
| `keywords` | Xihlahla xa mintlangu ya ku fola |
| `soom.minVersion` | Version ya WIA SOOM leyi pfumelaka |

### Ntlhanu 3: Tlhaya ku Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Tlhaya** plugin ya wena ku `plugins/{your-plugin-name}/`
3. **Tlhaya** Pull Request

### Ntlhanu 4: Ku hlola ni ku pfumala

Hi hlola plugin hinkwayo hi:

- **Vukorhokeri** — a ku na API ya vuyelo (tlhela u vona [Mafambiselo ya Vukorhokeri](#security-rules))
- **Mavonelo** — a ku na xihlawuhlawu? I yini xitlhangu?
- **Mfungho** — a ku na xiphiqo xa ntiyiso?

Endzhaku ka ku pfumala:
1. Plugin ya wena yi engetiwa ku `registry.json`
2. Bundl ya ZIP yi tumbuluxiwa ku `dist/`
3. Plugin ya wena yi vonaka ku **Plugin Store** ya hinkwavo va WIA SOOM!

---

## Ntlhanu 5: Mavonelo ya Ntlhanu

### Mafambiselo ya Vukorhokeri

Mafambiselo lawa i **mafanela**. Plugins leyi yi hlula ku ta va yi chaviwa.

| Ntlhanu | Hikwalaho |
|---------|----------|
| **A HI KUMI** ku tirhisa `eval()` kumbe `new Function()` | Xiphiqo xa ku hlanganisa xitlhangu |
| **A HI KUMI** ku tirhisa `child_process`, `exec()`, `spawn()` | Tshemba ku tirhisa `ctx.terminal.send()` hi ku ta ku ta ku ta ku ta |
| **A HI KUMI** ku hlanganisa URL ya le handle | Xikombiso: `wiasoom.com` API endpoints |
| **A HI KUMI** ku hlanganisa `process.env` | Mintirho ya le ndzeni yi nga va ni switsundzuxo |
| **A HI KUMI** ku tirhisa `require('fs')` hi ku hatlisa | Tshemba `ctx.settings` ku hlanganisa, `ctx.sftp` ku hlanganisa tifayili |
| **A HI KUMI** ku tirhisa npm external packages | JavaScript ya ntiyiso ntsena — a ku na node_modules |
| **I KUFANELA** ku tirhisa `ctx.terminal.send()` hi ku ta ku ta ku ta ku ta | Loku ku ya hi ku hlanganisa ka SSH ya vukorhokeri |
| **I KUFANELA** ku hlakulela ku `deactivate()` | Hlawula switsundzuxo, hlakulela mintlhangu |

### Ku Hlawula Swiphiqo

Hambi ku hlanganisa mintirho ya xiphiqo hi try/catch:
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
### Ku Hlawula ku deactivate()

Loko plugin ya wena yi tumbuluxa mintlhangu, switsundzuxo, kumbe swikombiso — hlakulela swona:
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

WIA SOOM yi tshemba mitlhontlho ya 254. Ku endla label ya plugin ya wena yi hlanganisiwa, tirhisa ndlela yo olova:
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

## Ntlhanu 6: Mifananiso ya Ntlhanu

### Mifananiso 1: Server Disk Checker

I hlanganisa `df -h` ku server ya handle ni ku kombisa leswi tirhisiwaka/ku kumiwa emavonelweni.
§§§CHUNK_SEPARATOR§§§
---

### Mifananiso 2: TODO Manager

Plugin leyi yi kumiwa ku hlanganisa list ya TODO hi ku tirhisa swikombiso ku hlanganisa switlhangu ni webview ku kombisa.

> **Mavonelo ya design:** Loko webviews a nga koti ku kutsula API ya plugin, plugin leyi yi tirhisa ndlela ya "snapshot" — yi hlawula TODOs ku swikombiso, yi hlanganisa swona tanihi HTML ya ku voniwa ntsena, ni ku nyika mintlhangu ya ku hlanganisa swikombiso. Webview i **ku voniwa** layer, a ku na fomu yo hlanganisa.
§§§CHUNK_SEPARATOR§§§
---

### Mifananiso 3: Error Watcher

I hlola ku humelerisa ka terminal ni ku rhumela xitlhangu loko swikombiso swo karhi swi hlanganisiwa.
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

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | Ku langutela ka server |
| `devtools` | Tihlanganisi ta ntshekelo |
| `calculator` | Tihlanganisi ta makhadi na ku converter |
| `simulator` | Tihlanganisi ta ku simula |
| `game` | Makhadi ya terminal |
| `business` | Tihlanganisi ta bhizinesi |
| `security` | Ku tsundzuxa na ku hlola |
| `web` | Ku langutela ka web server |
| `education` | Tihlanganisi ta dyondzo |
| `health` | Tihlanganisi ta vutomi |
| `islamic` | Tihlanganisi ta Islam (timhoro ta ku kholwa, etc.) |
| `science` | Tihlanganisi ta sayensi |
| `quantum` | Tihlanganisi ta ku khomisa ka quantum |
| `ai` | Tihlanganisi ta AI |
| `biotech` | Tihlanganisi ta biotechnology |
| `space` | Tihlanganisi ta ndzhaka na astronomy |
| `network` | Tihlanganisi ta network |
| `database` | Ku langutela ka database |
| `monitoring` | Ku langutela ka server |
| `devops` | DevOps na CI/CD |
| `utility` | Tihlanganisi ta ntlhantlha |
| `design` | Tihlanganisi ta ku aka |
| `ecommerce` | Tihlanganisi ta e-commerce |
| `automation` | Tihlanganisi ta ku tirhisa |
| `kpop` | Tihlanganisi ta K-pop |
| `accessibility` | Tihlanganisi ta ku fikelela |
| `analytics` | Tihlanganisi ta ku hlola na ku byela |
| `wia` | Tihlanganisi ta WIA ecosystem |
| `all` | Ku humelela eka swikombiso hinkwaswo |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Ku langutela ka server |
| `shield` | Ku tsundzuxa |
| `database` | Database |
| `activity` | Ku langutela |
| `terminal` | Tihlanganisi ta terminal |
| `code` | Ntshekelo |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Ku langutela/ku hlola |
| `check-square` | Mintsengo/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Ku lulamisa |
| `zap` | Ku tirhisa |
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
