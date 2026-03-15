<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Fai aoga e tau i te 5 miniti.</strong></p>
<p align="center">Fai e mafai e tau i te mau taoga, dashboard, mo e automations — i loto i te WIA SOOM.</p>

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

E "Hello World" plugin e tāpui e tau i te pa tuai. I te ta, e fakahā e tau i te notification.

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

Fakamuli i te app (po o fakamuli i te plugin i te Settings → Plugins).

E tatau e tau e **"Hello World"** button i te pa tuai. Klik e — e tau e fakahā e te success notification!

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

I te taimi e fakahā e tau `activate(context)` function, e fakahā e tau `context` (po o `ctx`) e mau API ko e:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Run commands on remote servers

#### `terminal.send(sessionId, data)`

Fakafesili e tau i te command (po o e mau data) ki te active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Te terminal session e fakafesili ki |
| `data` | `string` | Te command po o e data e fakafesili |
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

Fakafesili ki e mau output mai te terminal session. E toe mai e **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Te terminal session e matau |
| `callback` | `(data: string) => void` | E fakahā e tau i te mau chunk o e output |
| **Returns** | `() => void` | Fakamuli e tau e stop e matau |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**Important:** E mafai e tau e save e te unsubscribe function mo e fakahā i te `deactivate()` ke puipui e te memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — E fakahā e te SFTP API, akā e se e fakamau i te app's SFTP engine. `list()` e toe mai e te empty array, mo e `upload()`/`download()` e se e fai. E toe fakahā e i te fakamau i te taimi e toe. I te taimi nei, e mafai e tau e `ctx.terminal.send()` mo e `scp` po o e `rsync` commands e fai e te workaround.

#### `sftp.list(sessionId, path)`

Fakafesili e te mau faila i te remote directory.
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

Fakafesili e te faila mai te local machine ki te remote server.
��§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Fakafesili e te faila mai te remote server ki te local machine.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
**Workaround (kei te SFTP API e live):**
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Fakafesili e te button ki te WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (e default ki te plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Te button text e fakahā i te sidebar |
| `onClick` | `() => void` | Yes | Te function e fakahā i te taimi e klik e te button |
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Icon reference:** Fakafofonga e te mau icon e mafai i [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** E tolu e mau plugins e fakafesili e te positional arguments e like mo e `addSidebarButton(id, icon, label, onClick)`. E fakahā e te official API e te **options object** e like mo e fakahā i lalo. E mafai e tau e fai e te object style mo e mau plugins fou.

#### `ui.openWebview(options)`

Fakafofonga e te popup window mo e custom HTML content. Ko e auala e fai e te mau UI e tau. 

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Te window title |
| `html` | `string` | Te full HTML content e fakahā |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
> Eke [Part 3](#part-3-building-custom-ui-with-webviews) mo e ngaahi pateni webview faka'ata.

#### `ui.showNotification(type, message)`

Faka'atu e toasta notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Faka'ata e notification |
| `message` | `string` | Tohi ke faka'atu |
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
#### `ui.addStatusBarItem(id, text)`

Faka'ata e item tohi taute mo e taute i lalo o e status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID faka'ata mo e item status ko'eni |
| `text` | `string` | Tohi ke faka'atu |
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
---

### `ctx.settings` — Taofi e ngaahi faile

E ngaahi settings o e plugin e taofi mo e faile i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Fakafoki e tauhi faka'atu.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
E toe `undefined` kehe kehe e key ko e `key` e 'ikai ke i ai.

#### `settings.set(key, value)`

Taofi e tauhi faka'atu. E faka'atu e ngaahi tohi, numera, boolean, ngaahi lisi, mo e ngaahi object.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
**Fakamatala: Faka'atu e ngaahi filifiliga o e tagata**
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### `ctx.ai` — AI integration

> **Status: E hiki mai** — E faka'ata e AI API ka 'ikai ke toe faka'ata ki Soomy. I he taimi ko ia e toe faka'atu `{ response: 'AI not yet connected' }`. E faka'ata e AI integration ki he ngaahi release e hiki mai.

#### `ai.chat(messages, options?)`

Faka'atu e ngaahi tohi ki he AI assistant (Soomy).
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Part 3: Faka'atu e UI Faka'ata mo e Webviews

E `openWebview()` API e mafai ke ke faka'atu e UI dashboard mo e HTML, CSS, mo e JavaScript — kotoa i loko o e window popup.

> **Faka'ata: Faka'ata e ngaahi palani:** E **faka'atu-pe** e webviews. 'Ikai ke mafai ke toe faka'atu ki e plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Faka'atu e sidebar buttons ki he ngaahi me'a kotoa e fai e tagata, mo e faka'atu e `openWebview()` ke faka'atu e state ko ia. Ke ke manaʻomia e ngaahi feature interactive, faka'atu e ngaahi palani mai he sidebar buttons mo e toe faka'atu e webview ke toe faka'atu e display.

### Pateni: Terminal Command → Parse Output → Faka'atu i he HTML

Ko e pateni plugin ko ia e toe faka'atu. E ke faka'atu e command, faka'atu e resulta, mo e faka'atu e visual.
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
### Pateni: Interactive Dashboard mo e Auto-Refresh
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Pateni: Faka'atu e Settings i he Webview

> **Faka'ata:** E faka'atu e webviews — 'ikai ke mafai ke toe faka'atu ki e plugin APIs. Faka'atu e `ctx.settings` i he ngaahi sidebar button handlers ke faka'atu e settings, mo e faka'atu e `openWebview()` ke faka'atu e state ko ia.
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
---

## Part 4: Faka'atu e Plugin

### Step 1: Faka'ata i he loto

1. Kape e plugin ki `~/.wia-soom/plugins/{your-plugin}/`
2. Toe faka'ata e WIA SOOM
3. Faka'atu e lelei: e toe faka'atu e sidebar button, e lelei e ngaahi feature
4. Faka'ata e ngaahi edge cases: ko e hā e tupu ke 'ikai e terminal e faka'ata?

### Step 2: Faka'atu ki he faka'ata

E tatau ke i ai e plugin folder:
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
**Kia fakaako `package.json` ngaahi konga:**

| Konga | Fakaako | Fakaako |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | One-line description | `"Monitors nginx access logs in real-time"` |
| `author` | Oe hingoa | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Konga faka'eke:**

| Konga | Fakaako |
|-------|-------------|
| `license` | Konga laiseni (MIT ko e faka'eke) |
| `keywords` | Fakaako o e ngaahi tagi faka'anga |
| `soom.minVersion` | Ko e WIA SOOM version ko e minimum e manaʻomia |

### Tohi 3: Faka'atu ki he Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Tuku** ho'o plugin ki `plugins/{ho'o-plugin-hingoa}/`
3. **Faka'atu** he Pull Request

### Tohi 4: Fakatokanga mo e faka'atu

Tataki e plugin kotoa ki:

- **Faka'atu** — 'ikai ke fa'a faka'ali'i e ngaahi API (tsee [Fakaako Faka'atu](#security-rules))
- **Kualiti** — ko e fa'ahinga? Ko e code ko e ma'u lelei?
- **Faka'anga** — ko e fa'ahinga e fa'ahinga ma'ae mo'ui?

A e faka'atu:
1. Ko ho'o plugin e tuku ki `registry.json`
2. Ko e ZIP bundle e fai ki `dist/`
3. Ko ho'o plugin e faka'atu ki he **Plugin Store** ki he ngaahi tagata WIA SOOM kotoa!

---

## Faka'anga 5: Ngaahi Fakaako lelei

### Fakaako Faka'atu

Ko e ngaahi fakaako ko e **faka'atu**. Ko e ngaahi plugin e fa'ahinga ki ai e faka'atu e faka'atu.

| Fakaako | Ko e hā |
|------|-----|
| **'IKAI** ke fa'a faka'ali'i `eval()` pe `new Function()` | Ko e risiki e code injection |
| **'IKAI** ke fa'a faka'ali'i `child_process`, `exec()`, `spawn()` | Faka'ali'i `ctx.terminal.send()` mo e ngaahi fa'ahi |
| **'IKAI** ke fa'a faka'ali'i e ngaahi URL 'i hifo | Ko e faka'ali'i: `wiasoom.com` API endpoints |
| **'IKAI** ke fa'a faka'ali'i `process.env` | Ko e ngaahi variable e fa'ahinga e ma'u e secrets |
| **'IKAI** ke fa'a faka'ali'i `require('fs')` ko e to'onga | Faka'ali'i `ctx.settings` ki he storage, `ctx.sftp` ki he file transfer |
| **'IKAI** ke fa'a faka'ali'i e npm external packages | Ko e Pure JavaScript pe — 'ikai ke fa'a faka'ali'i e node_modules |
| **'UAA** ke fa'a faka'ali'i `ctx.terminal.send()` ki he ngaahi fa'ahi kotoa | Ko e fa'ahi ko e 'i he SSH channel faka'atu |
| **'UAA** ke faka'ali'i e cleanup ki he `deactivate()` | Tuku e listeners, faka'ali'i e intervals |

### Faka'ali'i e Faka'atu

Faka'ali'i e ngaahi fa'ahi risiki ki he try/catch:
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
### Cleanup ki he deactivate()

Koe'uhi ko ho'o plugin e fai e ngaahi intervals, listeners, pe subscriptions — faka'ali'i e ngaahi fa'ahi:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### i18n Fakaako

Ko e WIA SOOM e faka'ali'i e 254 ngaahi lea. Ki he faka'ali'i e plugin label ki he faka'ali'i, faka'ali'i e ngaahi fa'ahi maʻolunga:
§§§CHUNK_SEPARATOR§§§
---

## Faka'anga 6: Ngaahi Fakaako 'i he Mo'ui

### Fakaako 1: Server Disk Checker

Fai `df -h` ki he server 'i hifo mo e faka'ali'i e ngaahi fa'ahi e ma'u/ma'u 'i he status bar.
§§§CHUNK_SEPARATOR§§§
---

### Fakaako 2: TODO Manager

Ko e plugin e ta'ofi e TODO list e faka'ali'i e ngaahi settings ki he storage ma'ulalo mo e webview ki he faka'ali'i.

> **Fakaako design:** Koe'uhi ko e ngaahi webviews 'ikai ke faka'ali'i e plugin APIs, ko e plugin ko e "snapshot" — ko e faka'ali'i e TODOs mai he ngaahi settings, faka'ali'i e ngaahi HTML ko e read-only, mo e faka'ali'i e ngaahi fa'ahi ki he sidebar ki he faka'ali'i e ngaahi item. Ko e webview ko e **faka'ali'i** layer, 'ikai ko e interactive form.
§§§CHUNK_SEPARATOR§§§
---

### Fakaako 3: Error Watcher

Tataki e output terminal mo e faka'ali'i e notification ko e taimi e faka'ali'i e ngaahi fa'ahi ko e hā.
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
---

## Appendix: Kātogā & Ikoni

### Plugin Kātogā (29)

Fakamau i ēnei i tō `package.json` `keywords` pe i te taimi e tuku ai ki te registry:

| Kātogā | Fakamatalaga |
|--------|--------------|
| `server` | Tūmau fakakātoa |
| `devtools` | Meafaigaluega atinaʻe |
| `calculator` | Tūmau maʻi ma fa'avaʻa |
| `simulator` | Meafaigaluega simula |
| `game` | Taʻaloga terminal |
| `business` | Meafaigaluega pisinisi |
| `security` | Tūmau ma le su'esu'e |
| `web` | Tūmau fakakātoa i le initaneti |
| `education` | Meafaigaluega aʻoga |
| `health` | Meafaigaluega e fa'atatau i le soifuaga |
| `islamic` | Meafaigaluega Islam (taimi o le tatalo, etc.) |
| `science` | Meafaigaluega saienitifi |
| `quantum` | Meafaigaluega fa'atekinolosi quantum |
| `ai` | Meafaigaluega e fa'avae i le AI |
| `biotech` | Meafaigaluega biotekinolosi |
| `space` | Meafaigaluega i le vateatea ma le fa'asaienisi o le vateatea |
| `network` | Meafaigaluega fa'atekinolosi |
| `database` | Tūmau i le fa'amaumauga |
| `monitoring` | Su'esu'e i le server |
| `devops` | DevOps ma CI/CD |
| `utility` | Meafaigaluega masani |
| `design` | Meafaigaluega mamanu |
| `ecommerce` | Meafaigaluega e-commerce |
| `automation` | Meafaigaluega fa'atekinolosi |
| `kpop` | Meafaigaluega e fa'atatau i le K-pop |
| `accessibility` | Meafaigaluega e mafai ona fa'aaoga |
| `analytics` | Fa'ata'ita'iga ma le lipoti |
| `wia` | Meafaigaluega i le WIA ecosystem |
| `all` | E foliga mai i le katoa kātogā |

### Ikoni Fa'ata'ita'iga (Lucide)

| Igoa o le Ikoni | Fa'aaoga mo |
|-----------------|-------------|
| `server` | Tūmau fakakātoa |
| `shield` | Saogalemu |
| `database` | Fa'amaumauga |
| `activity` | Su'esu'e |
| `terminal` | Meafaigaluega terminal |
| `code` | Atinaʻe |
| `hard-drive` | Disk/storage |
| `network` | Fa'atekinolosi |
| `lock` | Fa'amaonia/encryption |
| `eye` | Fa'amau/monitoring |
| `check-square` | Taʻiala/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Fa'atonuga |
| `zap` | Fa'atekinolosi |
| `globe` | Initaneti/fa'ava-o-le-lalolagi |

Faitau uma ikoni e 1,500+: [lucide.dev/icons](https://lucide.dev/icons)

---

## E manaʻomia se fesoasoani?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Fa'ata'ita'iga Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Fai se mea ofoofogia. Fa'asoa atu i le lalolagi.</em></p>
<p align="center"><em>— O le Au WIA SOOM</em></p>
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

WIA SOOM supports 254 languages. To make your plugin label translatable, use a simple approach:

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

## Part 6: Real-World Examples

### Example 1: Server Disk Checker

Runs `df -h` on the remote server and shows used/available space in the status bar.

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
