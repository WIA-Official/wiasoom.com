<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Vhuyani plugin ya nṋe ngauri 5 minutes.</strong></p>
<p align="center">Nangula zwivhuya zwa server, dashboards, na automations — u tshi khou itela WIA SOOM.</p>

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

Plugin ya "Hello World" i tshi ṱoḓa button kha sidebar. U tshi khou vhuya, i tshi khou sumbedza notification.

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

Vhuya u thoma app (kana u toggla plugin off/on kha Settings → Plugins).

U fanela u vhona **"Hello World"** button kha sidebar. Vhuya — u do vhona notification ya u bveledza!

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

Ndi ngani `activate(context)` function i tshi khou bva, `context` (kana `ctx`) i sumbedza API dzine dza vha na:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Run commands on remote servers

#### `terminal.send(sessionId, data)`

Senda command (kana data) kha session ya terminal i tshi khou shuma.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal ye u sendela |
| `data` | `string` | Command kana data ye u sendela |
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

Subscribe kha zwihuluhulu zwoṱhe zwo bva kha session ya terminal. I sumbedza **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal ye u khou ṱanganedza |
| `callback` | `(data: string) => void` | I khou bva na chunk yoṱhe ya output |
| **Returns** | `() => void` | Vhuyani u ita uri u vhuye u pfa |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**Important:** Vhuyani u shuma unsubscribe function na u i shuma kha `deactivate()` uri u thibela memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — The SFTP API i sumbedzwa fhedzi a i sumbedzi kha SFTP engine ya app. `list()` i sumbedza array ya u sa vha na tshedza, na `upload()`/`download()` ndi no-ops. I do vha i tshi shuma kha release ya misi i re na. U sa athu, shuma `ctx.terminal.send()` na `scp` kana `rsync` commands sa workaround.

#### `sftp.list(sessionId, path)`

Langa mafayili kha directory ya remote.
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

Uploada fayili u bva kha local machine u ya kha remote server.
§§§CHUNK_SEPARATOR§§��
#### `sftp.download(sessionId, remotePath, localPath)`

Downloada fayili u bva kha remote server u ya kha local machine.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
**Workaround (hasta SFTP API i tshi shuma):**
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Adda button kha sidebar ya WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | ID ya u sa vhe na tshanduko (defaults to plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Button text ye i sumbedzwa kha sidebar |
| `onClick` | `() => void` | Yes | Function ye i khou shuma u tshi khou vhuya button |
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Icon reference:** Tswala icons dzose dzine dza vha na [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Zvimwe zva plugins zwo vhuya zishuma positional arguments sa `addSidebarButton(id, icon, label, onClick)`. The official API i shuma **options object** sa i sumbedzwa ngeno. Vhuyani u shuma object style kha plugins dzoṱhe. 

#### `ui.openWebview(options)`

Vhuyani popup window na HTML content ya tsumbo. Ndi zwine u ita UI dzine dza vha na vhuimo. 

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content ye u sumbedza |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
> Tshiṱo [Part 3](#part-3-building-custom-ui-with-webviews) u itela maipfi a webview a aṱangwaho.

#### `ui.showNotification(type, message)`

Suma toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Mufhiso wa notification |
| `message` | `string` | Nyambo yo shumelwaho |
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

Nḓa u ṱanganya nyambo ya u shuma kha status bar ya nṱha.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID ya u bva kha item ya status |
| `text` | `string` | Nyambo yo ṱanganedzwa |
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

### `ctx.settings` — U shuma nga u ṱanganedza

Zwiṅwe zwa plugin zwi ṱanganedzwa nga u sa fheleli kha `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Landa vhuimo vhu ṱanganedzaho.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
I ṱanganya `undefined` arali key i sa vhe na.

#### `settings.set(key, value)`

Suma vhuimo. I ṱanganedza strings, numbers, booleans, arrays, na objects.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
**Mufhiso: Ramba u ṱanganedza mivhuso ya mushumi**
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### `ctx.ai` — U shuma na AI

> **Mufhiso: U ḓi ṱanganedza** — AI API i ṱanganedzwa fhedzi i sa kone u bva kha Soomy. I ṱanganya `{ response: 'AI not yet connected' }`. U shuma na AI i ṱanganedzwa u ḓi bva kha u ḓi ṱanganedza.

#### `ai.chat(messages, options?)`

Tuma mivhuso kha AI assistant (Soomy).
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Part 3: U ṱanganya UI ya U Suma na Webviews

API `openWebview()` i fa u ṱanganya dashboard UIs na HTML, CSS, na JavaScript — zwoṱhe kha pop-up window.

> **Mufhiso wa ndeme:** Webviews ndi **display-only**. A zwi nga kone u bvisa kha plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Shuma sidebar buttons u itela mivhuso ya mushumi, na shuma `openWebview()` u sumbedza vhuimo vhukuma. Arali u na zwifhinga zwa u shuma, vhulunga zwine u ṱoḓa u bva kha sidebar buttons na u ṱanganya webview u ṱanganedza vhuimo.

### Pattern: Terminal Command → Parse Output → Show in HTML

Iyi ndi pattern ya plugin ya u ṱangana. U ita command, u ṱanganedza mivhuso, na u sumbedza visually.
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
### Pattern: Interactive Dashboard na Auto-Refresh
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Pattern: U Sumbedza Settings kha Webview

> **Nḓivho:** Webviews ndi display-only — a zwi nga kone u bvisa kha plugin APIs. Shuma `ctx.settings` kha sidebar button handlers u sumbedza settings, na shuma `openWebview()` u sumbedza vhuimo vhukuma.
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

## Part 4: U Sumbedza Plugin Yau

### Nḓila 1: U ṱanganedza kha ndila

1. Kopisha plugin yau kha `~/.wia-soom/plugins/{your-plugin}/`
2. Dzhia WIA SOOM
3. Tshiṱo u ṱanganedza: sidebar button i vha, zwifhinga zwi shuma nga u ṱanganedza
4. Tshiṱo u ṱanganedza edge cases: ni ngani arali terminal i sa kone u bva?

### Nḓila 2: U ṱanganedza u itela u sumbedza

Fhodza ya plugin yau i fanela u vha na:
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
**Zwino `package.json` zwoṱhe:**

| Fhasi | Tshedza | Mufano |
|-------|-------------|---------|
| `name` | ID ya unique ya kebab-case | `"my-awesome-plugin"` |
| `version` | Version ya semantic | `"1.0.0"` |
| `description` | Tshedza ya muta | `"Monitors nginx access logs in real-time"` |
| `author` | Dzina lenu | `"John Doe"` |
| `main` | Nṱha ya u thoma | `"index.js"` |

**Zwino zwoṱhe:**

| Fhasi | Tshedza |
|-------|-------------|
| `license` | Mufano wa license (MIT u funwa) |
| `keywords` | Nḓila dza u ṱoḓa |
| `soom.minVersion` | Version ya WIA SOOM ya u thoma i funwa |

### Nḓila 3: Tshiṱanganya kha Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** plugin yaṋu kha `plugins/{your-plugin-name}/`
3. **Submit** a Pull Request

### Nḓila 4: U ṱanganedza na u amba

Ri ṱanganedza plugin dzothe nga:

- **Security** — a si na API dza u vhulunga (tshinya [Security Rules](#security-rules))
- **Quality** — a i shuma? Ndi mini code?
- **Usefulness** — a i sokouwa na u vhulunga?

Mushumo u amba:
1. Plugin yaṋu i ṱanganedzwa kha `registry.json`
2. Bundle ya ZIP i bveledzwa kha `dist/`
3. Plugin yaṋu i bveledzwa kha **Plugin Store** ya WIA SOOM vhashumeli vhoṱhe!

---

## Nḓila 5: Zwiitisi zwa U Shuma

### Security Rules

Zwiitisi zwino ndi **zwoṱhe**. Plugin dzine dza phusukanya dzino dzo dzula.

| Mvumo | Mbalo |
|------|-----|
| **A THOVHE** u shumisa `eval()` kana `new Function()` | Risk ya code injection |
| **A THOVHE** u shumisa `child_process`, `exec()`, `spawn()` | Shumisa `ctx.terminal.send()` fhedzi kha mirairo |
| **A THOVHE** u ṱoḓa URLs dza nṱha | Muvhuso: `wiasoom.com` API endpoints |
| **A THOVHE** u ṱoḓa `process.env` | Vhuri dza muṱa dza nga vha na zwifhiwa |
| **A THOVHE** u shumisa `require('fs')` nga ndila ya nḓila | Shumisa `ctx.settings` kha u fhedza, `ctx.sftp` kha u fhedza mafayili |
| **A FHELA** u shumisa npm external packages | JavaScript ya fhedzi — a si node_modules |
| **A FHELA** u shumisa `ctx.terminal.send()` kha mirairo yoṱhe ya nṱha | I ya nga nḓila ya SSH ya u vhulunga |
| **A FHELA** u shuma kha `deactivate()` | Tsha vhukuma, vhuyelela intervals |

### U Sika Mavhudzi

Ndi nga u tsireledza u ṱoḓa u shuma kha try/catch:
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
### U Tsha Vhuṱuku kha deactivate()

Arali plugin yaṋu i bveledza intervals, listeners, kana subscriptions — tshiṱanganya:
§§§CHUNK_SEPARATOR§§§
### i18n Support

WIA SOOM i tsireledza mitauro ya 254. U ita uri plugin yaṋu i vhe na u ṱoḓa, shumisa ndila yo simple:
§§§CHUNK_SEPARATOR§§§
---

## Nḓila 6: Mifano ya U Shuma

### Mufano 1: Server Disk Checker

I shuma `df -h` kha server ya nṱha na u bveledza vhukuma/na u shuma kha status bar.
§§§CHUNK_SEPARATOR§§§
---

### Mufano 2: TODO Manager

Plugin ine i shuma kha u langa TODO list u shumisa settings kha u fhedza na webview kha u bveledza.

> **Design pattern:** Ngauri webviews a nga si kone u ṱoḓa plugin APIs, plugin ino i shumisa "snapshot" approach — i ṱoḓa TODOs u bveledza kha settings, i i bveledza sa HTML ya u langa, na i fa vhuṱuku kha u ṱoḓa zwiṅwe. Webview i khou bveledza **display** layer, a si form ya u shuma.
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
---

### Mufano 3: Error Watcher

I langa terminal output na u fa notification arali mivhuso ine i vhuya i wanala.
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

## Appendix: Categories & Icons

### Plugin Categories (29)

Shandisa izvi mu `package.json` `keywords` kana uchiisa ku registry:

| Category | Description |
|----------|-------------|
| `server` | Kutungamira kwe server kwechokwadi |
| `devtools` | Zvishandiso zvekusimudzira |
| `calculator` | Makirasi nemakushanduri |
| `simulator` | Simulators |
| `game` | Mitambo ye terminal |
| `business` | Zvishandiso zvebhizinesi |
| `security` | Chengetedzo uye kuongorora |
| `web` | Kutungamira kwe web server |
| `education` | Zvishandiso zvedzidzo |
| `health` | Zvishandiso zvine chekuita nehutano |
| `islamic` | Zvishandiso zvechiIslam (nguva dzekunamata, nezvimwewo) |
| `science` | Zvishandiso zvesainzi |
| `quantum` | Zvishandiso zve quantum computing |
| `ai` | Zvishandiso zvinotungamirwa neAI |
| `biotech` | Zvishandiso zve biotechnology |
| `space` | Zvishandiso zvespase neastronomy |
| `network` | Zvishandiso zve network |
| `database` | Kutungamira kwe database |
| `monitoring` | Kuongorora server |
| `devops` | DevOps uye CI/CD |
| `utility` | Zvishandiso zvakajairika |
| `design` | Zvishandiso zvekugadzira |
| `ecommerce` | Zvishandiso zve e-commerce |
| `automation` | Zvishandiso zve automation |
| `kpop` | Zvishandiso zve K-pop |
| `accessibility` | Zvishandiso zvekuwana |
| `analytics` | Kuongorora uye kupfupikisa |
| `wia` | Zvishandiso zve WIA ecosystem |
| `all` | Inoratidzwa mumapoka ese |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Kutungamira kwe server |
| `shield` | Chengetedzo |
| `database` | Database |
| `activity` | Kuongorora |
| `terminal` | Zvishandiso zve terminal |
| `code` | Kusimudzira |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Kutarisa/kuongorora |
| `check-square` | Mabasa/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Kugadzirisa |
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
