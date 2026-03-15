<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Fai se koe te plugin i totonu o le 5 minute.</strong></p>
<p align="center">Fai ni meafaigaluega malosi mo le server, dashboards, ma automations — i totonu o le WIA SOOM.</p>

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

O se plugin "Hello World" e faʻaopoopo se button i le sidebar. A oʻo i le kiliki, e faʻaalia se faamatalaga.

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

E tatau ona e vaʻaia se **"Hello World"** button i le sidebar. Kiliki i ai — o le a e vaʻaia se faamatalaga manuia!

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

A o le `activate(context)` function e faaaogaina, e ofoina mai e le `context` (poʻo le `ctx`) nei APIs:
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

Faʻaulu se faiga (poʻo se faamatalaga) i se active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | O le terminal session e faʻaulu i ai |
| `data` | `string` | O le faiga poʻo le faamatalaga e faʻaulu |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Faʻaulu i le output uma mai se terminal session. E toe foʻi mai se **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | O le terminal session e mataʻituina |
| `callback` | `(data: string) => void` | Faʻaogaina i le vaega taʻitasi o le output |
| **Returns** | `() => void` | Faʻaoga lenei e taofi le faalogo |
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
**Important:** E tatau ona e teuina le unsubscribe function ma faaaoga i le `deactivate()` e taofi ai le leakage o le memory.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — O le SFTP API e faatulaga ae leʻi fesoʻotaʻi i le SFTP engine o le app. O le `list()` e toe foʻi mai se array e leʻo i ai, ma o le `upload()`/`download()` e leai ni galuega. O le a faamaeʻa i se faamatalaga i le lumanaʻi. I le taimi nei, faaaoga le `ctx.terminal.send()` ma le `scp` poʻo le `rsync` commands e fai ma auala.

#### `sftp.list(sessionId, path)`

Lisi i faailoa i se directory mamao.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Faʻaulu se faila mai le masini i le server mamao.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Faʻaulu se faila mai le server mamao i le masini.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (seʻia oʻo i le SFTP API e ola):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Faʻaopoopo se button i le sidebar o le WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | ID fa'avaa (default i le plugin name) |
| `icon` | `string` | Yes | Igoa o le icon Lucide (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | O le tusitusiga o le button e faʻaalia i le sidebar |
| `onClick` | `() => void` | Yes | Faiga e faaaoga a o kiliki le button |
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

> **Compatibility note:** O nisi plugins tuai e faaaoga le positional arguments e pei o `addSidebarButton(id, icon, label, onClick)`. O le API aloaia e faaaoga se **options object** e pei ona faailoa i luga. E tatau ona e faaaoga le object style mo plugins fou.

#### `ui.openWebview(options)`

Tatala se popup window ma le HTML content masani. O le auala lea e fai ai e te UI matagofie.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Igoa o le faamalama |
| `html` | `string` | O le HTML content atoa e faaaogaina |
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
> Vaai i [Part 3](#part-3-building-custom-ui-with-webviews) mo ngā tauira webview tāua.

#### `ui.showNotification(type, message)`

Tāuta he pānui toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Te āhua pānui |
| `message` | `string` | Te tuhinga hei whakaatu |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Tāpirihia he tuhinga pātea ki te raro o te pae tūtohi.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID motuhake mō tēnei tuhinga tūtohi |
| `text` | `string` | Te tuhinga hei whakaatu |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Te penapena mau

Ka penapenahia ngā whakaritenga o te plugin i roto i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Pānuihia he uara kua penapenahia.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Ka hoki `undefined` mēnā kāore te key e mau.

#### `settings.set(key, value)`

Penapenahia he uara. Ka tautoko i ngā tuhinga, tau, pōti, rārangi, me ngā taonga.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Tauira: Mahara ki ngā whakaritenga o te kaiwhakamahi**
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

### `ctx.ai` — Te whakauru AI

> **Tūnga: Ka haere mai i te wā e tūmanakohia ana** — Kua whakatakotoria te AI API engari kāore i hono ki Soomy. I tēnei wā ka hoki `{ response: 'AI not yet connected' }`. Kua whakamaheretia te whakauru AI ki te putanga kei te heke mai.

#### `ai.chat(messages, options?)`

Tukuhia ngā karere ki te āwhina AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Te hanga UI Ritenga me ngā Webviews

Ka taea e te API `openWebview()` te hanga i ngā UI papamahi me HTML, CSS, me JavaScript — katoa i roto i te matapihi pop-up.

> **Tūtohu nui:** Ko ngā webviews he **whakaatu anake**. Kāore e taea te karanga ki ngā API plugin (`ctx.settings`, `ctx.terminal`, etc.). Whakamahia ngā pātene taha mō ngā mahi katoa a te kaiwhakamahi, ā, whakamahia te `openWebview()` hei whakaatu i te āhua o nāianei. Mēnā e hiahia ana koe ki ngā āhuatanga interaktīve, whakaohohia mai i ngā pātene taha, ā, whakahouhia te webview hei whakahou i te whakaaturanga.

### Tauira: Command Terminal → Parse Output → Whakaatu i roto i te HTML

Ko tēnei te tauira plugin tino noa. Ka whakahaerehia e koe he whakahau, ka parse i te hua, ā, ka whakaatu i te āhua.
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
### Tauira: Dashboard Interaktīve me te Auto-Refresh
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
### Tauira: Whakaatu i ngā Whakaritenga i roto i te Webview

> **Tuhinga:** Ko ngā webviews he whakaatu anake — kāore e taea te karanga ki ngā API plugin. Whakamahia te `ctx.settings` i roto i ngā kaiwhakahaere pātene taha hei whakarereke i ngā whakaritenga, ā, whakamahia te `openWebview()` hei whakaatu i te āhua o nāianei.
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

## Part 4: Te whakaputa i tō Plugin

### Hipanga 1: Whakamātauria i te kāinga

1. Tāpiri tō plugin ki `~/.wia-soom/plugins/{your-plugin}/`
2. Tīmata anō i te WIA SOOM
3. Whakamātauria mēnā e mahi ana: ka puta te pātene taha, ka pai ngā āhuatanga
4. Whakamātauria ngā take pito: he aha ka tupu mēnā kāore he terminal e hono ana?

### Hipanga 2: Whakaritea mō te tuku

Me pupuni tō kōpaki plugin i ngā:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | ID unik i le kebab-case | `"my-awesome-plugin"` |
| `version` | Version semantika | `"1.0.0"` |
| `description` | Fa'ata'ita'iga i le laina tasi | `"Monitors nginx access logs in real-time"` |
| `author` | Lau igoa | `"John Doe"` |
| `main` | Fa'avae i le fa'avae | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | Ituaiga laisene (MIT e fautuaina) |
| `keywords` | Fa'ata'ita'iga o fa'ailoga su'esu'e |
| `soom.minVersion` | Fa'avae i le WIA SOOM e manaʻomia le fa'avae |

### Step 3: Fa'amaonia i le Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** lau plugin i `plugins/{your-plugin-name}/`
3. **Submit** se Pull Request

### Step 4: Iloilo ma le fa'amaonia

Matou te iloiloina uma plugin mo:

- **Saogalemu** — leai ni APIs e leaga (va'ai i le [Security Rules](#security-rules))
- **Tulaga** — e faigaluega? E mama le code?
- **Fa'aaoga** — e fo'ia se fa'afitauli moni?

A mae'a le fa'amaonia:
1. O lau plugin e fa'aopoopo i `registry.json`
2. O se ZIP bundle e faia i `dist/`
3. O lau plugin e foliga mai i le **Plugin Store** mo tagata uma o le WIA SOOM!

---

## Part 5: Fa'avae i le Fa'aaogaina

### Security Rules

O nei fa'avae e **mana'omia**. O plugin e fa'aleagaina i latou o le a fa'atekinolaina.

| Rule | Why |
|------|-----|
| **E le mafai** ona fa'aogaina `eval()` po'o `new Function()` | Risiki o le fa'ainisinia o le code |
| **E le mafai** ona fa'aogaina `child_process`, `exec()`, `spawn()` | Fa'aaoga na'o le `ctx.terminal.send()` mo fa'atonuga |
| **E le mafai** ona fa'amaonia URL i fafo | Fa'avae: `wiasoom.com` API endpoints |
| **E le mafai** ona fa'aaogaina `process.env` | E mafai ona iai fa'amaoniga i le si'osi'omaga |
| **E le mafai** ona fa'aogaina `require('fs')` i le fa'avae | Fa'aaoga `ctx.settings` mo le teuina, `ctx.sftp` mo le fa'amaonia fa'ailoa |
| **E le mafai** ona fa'aogaina npm external packages | JavaScript na'o le — leai ni node_modules |
| **E tatau** ona fa'aogaina `ctx.terminal.send()` mo fa'atonuga mamao uma | O le a fa'agaioia i le auala SSH saogalemu |
| **E tatau** ona fa'amaonia i le `deactivate()` | Avea le fa'ailoa, fa'asa'o le taimi |

### Error Handling

Fa'amolemole fa'aoga i taimi uma i le fa'amaonia o le fa'atinoga i le try/catch:
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
### Fa'amaonia i le deactivate()

Afai e fa'avae lau plugin i le taimi, fa'ailoa, po'o le fa'amaonia — fa'amaonia i latou:
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

E lagolagoina e le WIA SOOM le 254 gagana. E fa'ata'ita'iga lau plugin e mafai ona fa'alelei, fa'aaoga se auala faigofie:
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

## Part 6: Fa'ata'ita'iga i le Fa'avae

### Fa'ata'ita'iga 1: Server Disk Checker

Fa'aogaina `df -h` i le server mamao ma fa'aalia le avanoa e fa'aaogaina/i le status bar.
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

### Fa'ata'ita'iga 2: TODO Manager

O se plugin e fa'amaonia se lisi TODO e fa'aaoga fa'avae mo le teuina tumau ma se webview mo le fa'aalia.

> **Fa'avae i le fa'ata'ita'iga:** E le mafai e le webviews ona fa'amaonia i le plugin APIs, o le plugin e fa'aaoga se "snapshot" — e faitau le TODOs mai fa'avae, e fa'aalia i latou i le HTML e le mafai ona fa'amaonia, ma ofoina atu gaioiga e fa'avae i le sidebar mo le fa'aopoopoina o fa'ata'ita'iga. O le webview o se **fa'aalia** laupapa, e le o se fomu fa'atekinolaina.
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

### Fa'ata'ita'iga 3: Error Watcher

E mata'ituina le fa'output o le terminal ma fa'aalia se fa'ailoa pe a maua ni fa'ata'ita'iga patino.
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

## Appendix: Kātā & Ika

### Kātā Pūmanawa (29)

Fa'aaoga ēnei i tō `package.json` `keywords` vālea i te tuku ki te registry:

| Kātā | Whakamārama |
|------|-------------|
| `server` | Whakahaere pūmanawa whānui |
| `devtools` | Ngā taputapu whakawhanaketanga |
| `calculator` | Ngā kaipōti me ngā huri |
| `simulator` | Ngā whakatūpato |
| `game` | Ngā kēmu terminal |
| `business` | Ngā taputapu pakihi |
| `security` | Te haumaru me te arotake |
| `web` | Whakahaere pūmanawa paetukutuku |
| `education` | Ngā taputapu mātauranga |
| `health` | Ngā taputapu e pā ana ki te hauora |
| `islamic` | Ngā taputapu īkarā (ngā wā karakia, etc.) |
| `science` | Ngā taputapu pūtaiao |
| `quantum` | Ngā taputapu rorohiko quantum |
| `ai` | Ngā taputapu e whakahaerehia ana e te AI |
| `biotech` | Ngā taputapu biotechnological |
| `space` | Ngā taputapu mō te rangi me te ātea |
| `network` | Ngā taputapu whatunga |
| `database` | Whakahaere pūnaha raraunga |
| `monitoring` | Aroturuki pūmanawa |
| `devops` | DevOps me CI/CD |
| `utility` | Ngā taputapu whānui |
| `design` | Ngā taputapu hoahoa |
| `ecommerce` | Ngā taputapu e-tauhokohoko |
| `automation` | Ngā taputapu aunoa |
| `kpop` | Ngā taputapu e pā ana ki te K-pop |
| `accessibility` | Ngā taputapu wātea |
| `analytics` | Ngā tātaritanga me te pūrongo |
| `wia` | Ngā taputapu pūnaha WIA |
| `all` | E puta ana i ngā kātā katoa |

### Ngā Ika Tūtohu (Lucide)

| Ingoa Ika | Whakamahia mō |
|-----------|---------------|
| `server` | Whakahaere pūmanawa |
| `shield` | Haumaru |
| `database` | Pūnaha raraunga |
| `activity` | Aroturuki |
| `terminal` | Ngā taputapu terminal |
| `code` | Whakapakari |
| `hard-drive` | Puku/whakaū |
| `network` | Whatunga |
| `lock` | Tautuhinga/whakamarumaru |
| `eye` | Mātaki/aroturuki |
| `check-square` | Ngā mahi/TODO |
| `layout-dashboard` | Ngā papamahi |
| `settings` | Whakarite |
| `zap` | Aunoa |
| `globe` | Paetukutuku/ao |

Tirohia ngā ika 1,500+ katoa: [lucide.dev/icons](https://lucide.dev/icons)

---

## E hiahia ana i te awhina?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Ngā Pūmanawa Tauira:** [Website](https://wiasoom.com)
- **Paetukutuku:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Whakawhanakehia he mea whakamīharo. Tōhia ki te ao.</em></p>
<p align="center"><em>— Te Rōpū WIA SOOM</em></p>