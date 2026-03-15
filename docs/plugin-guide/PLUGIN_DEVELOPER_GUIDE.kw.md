<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Gydhennys Plugin Developer Guide</h1>
<p align="center"><strong>Gwir dhywgh owth gynnag a plugin yn 5 munud.</strong></p>
<p align="center">Krevygh gwell tools server, dashboards, ha automations — yn gans WIA SOOM.</p>

---

## Tabel an Kothmyn

- [Rag 1: Kyns Start — Dhyworth Plugin Gans 5 Munud](#rag-1-kyns-start--dhyworth-plugin-gans-5-munud)
- [Rag 2: Plugin Context API Referans](#rag-2-plugin-context-api-referans)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Rag 3: Krevygh UI Kyns Webviews](#rag-3-krevygh-ui-kyns-webviews)
- [Rag 4: Publisya dhywgh Plugin](#rag-4-publisya-dhywgh-plugin)
- [Rag 5: An Best Practices](#rag-5-an-best-practices)
- [Rag 6: An Real-World Examples](#rag-6-an-real-world-examples)
- [Appendix: Kategori ha Icons](#appendix-kategori-ha-icons)

---

## Rag 1: Kyns Start — Dhyworth Plugin Gans 5 Munud

### Pyth a wrewgh

Un plugin "Hello World" a gans button dhe'n sidebar. Pyth a vynn, yth esowgh un notification.

### Dystep 1: Krevygh an plugin folder
§§§CHUNK_SEPARATOR§§§
### Dystep 2: Krevygh package.json
§§§CHUNK_SEPARATOR§§§
**Yma an gwellow:** `name`, `version`, `description`, `author`, `main`

### Dystep 3: Krevygh index.js
§§§CHUNK_SEPARATOR§§§
### Dystep 4: Restart WIA SOOM

Restart an app (po toggle an plugin off/on yn Settings → Plugins).

Yth esowgh un **"Hello World"** button yn an sidebar. Click dhod — yth esowgh un success notification!

### Pyth a worr
§§§CHUNK_SEPARATOR§§§
---

## Rag 2: Plugin Context API Referans

Pyth a vynn dhywgh an `activate(context)` function, `context` (po `ctx`) a ro dhywgh an APIs yma:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Runi commands yn servorow remota

#### `terminal.send(sessionId, data)`

Send un command (po an data) dhyworth un active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | An terminal session dhyworth a send |
| `data` | `string` | An command po data dhyworth a send |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Subscribe dhyworth an oll output dhyworth un terminal session. A ro un **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | An terminal session dhyworth a gath |
| `callback` | `(data: string) => void` | Kyns dhyworth an oll chunk a output |
| **Returns** | `() => void` | Galw dhod an a dhe dhis stop gath |
§§§CHUNK_SEPARATOR§§§
**Pimportant:** Gwell dhywgh an unsubscribe function ha galw dhod yn `deactivate()` dhyworth prevent memory leaks.

---

### `ctx.sftp` — Transfer an file

> **Status: Dros an Whel** — An SFTP API a wrewgh, mes ny ylls an app's SFTP engine. `list()` yn termyn a ro un array gwrys, ha `upload()`/`download()` a wrewgh no-ops. An a wrewgh yn gwell a whel yn termyn a dheu. Dros an prys, usi `ctx.terminal.send()` gans `scp` po `rsync` commands as un workaround.

#### `sftp.list(sessionId, path)`

Lyst an files yn un directory remota.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Upload un file dhyworth an local machine dhyworth an servor remota.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Download un file dhyworth an servor remota dhyworth an local machine.
§§§CHUNK_SEPARATOR§§§
**Workaround (dros an SFTP API a yll):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Add un button dhyworth an WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Na | Unique ID (defaults dhyworth an plugin name) |
| `icon` | `string` | Yma | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yma | An text a button a gath yn an sidebar |
| `onClick` | `() => void` | Yma | Un function a galw pyth a vynn an button a vynn |
§§§CHUNK_SEPARATOR§§§
**Icon referans:** Browse an oll icons a ylls yn [lucide.dev/icons](https://lucide.dev/icons)

> **Note an Compatibility:** Nyns an plugins hwi a usi positional arguments a wrewgh `addSidebarButton(id, icon, label, onClick)`. An official API a usi un **options object** a wrewgh yn an dokumented uhel. Gwell dhywgh an object style dhyworth an plugins nowydh.

#### `ui.openWebview(options)`

Open un popup window gans custom HTML content. An a yw pyth a wrewgh rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | An title an window |
| `html` | `string` | An full HTML content a wrewgh dhyworth a render |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Gwel [Rag 3](#part-3-building-custom-ui-with-webviews) rag patterns webview ow kelwel.

#### `ui.showNotification(type, message)`

Dhiworth un notifekans tost.

| Parameter | Type | Deskriftion |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stile notifekans |
| `message` | `string` | Tekst da dhisplaya |
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
#### `ui.addStatusBarItem(id, text)`

Yma un item tekst perhennys owth omdhal an bar status a-ber.

| Parameter | Type | Deskriftion |
|-----------|------|-------------|
| `id` | `string` | ID unik rag an item status yma |
| `text` | `string` | Tekst da dhisplaya |
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
---

### `ctx.settings` — Stori perhennys

An settans plugin a wre dhisplaya yn termyn owth omdhal yn `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Dhiworth un gwerth a gewsel.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Dhiworth `undefined` ma'n key ny wra existya.

#### `settings.set(key, value)`

Dhiworth un gwerth. Yma support rag strings, numbers, booleans, arrays, ha objects.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Donn: Kewsel preferansow an user**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Integreth AI

> **Status: Dros an Dher** — An API AI a wre dhewgh, mes ny wra connectya dhyworth Soomy. Yma'n respos a dhisplaya `{ response: 'AI not yet connected' }`. Integreth AI koth a wre rag un release a dheu.

#### `ai.chat(messages, options?)`

Dhiworth messajow dhyworth an assistans AI (Soomy).
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
---

## Rag 3: Kewsel UI Koth gans Webviews

An `openWebview()` API a wra dhisplaya UI dashboard gans HTML, CSS, ha JavaScript — oll yn un window popup.

> **Limitation pwys:** An webviews a wre **dhiworth-yn-ond**. Ny wra call back dhewgh an APIs plugin (`ctx.settings`, `ctx.terminal`, etc.). Usya buttons an sidebar rag an actionow user, ha usya `openWebview()` rag dhisplaya an stat current. Ma' dhywgh an features interaktif, trigger dhyn gans buttons an sidebar ha re-open an webview rag refreshya an dhisplay.

### Pattern: Command Terminal → Parse Output → Dhisplaya yn HTML

Yma yma an pattern plugin most common. Yma dhywgh un command, parse an result, ha dhisplaya y'n vynn.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Pattern: Dashboard Interaktif gans Auto-Refresh
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Pattern: Dhisplaya Settans yn Webview

> **Nod:** An webviews a wre dhiworth-yn-ond — ny wra call back dhewgh an APIs plugin. Usya `ctx.settings` yn owth handlers buttons an sidebar rag modifia settans, ha usya `openWebview()` rag dhisplaya an stat current.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Rag 4: Publisya an Plugin

### Dher 1: Testa yn le

1. Copya an plugin dhyworth `~/.wia-soom/plugins/{your-plugin}/`
2. Restartya WIA SOOM
3. Verifia na wra: an button an sidebar a wra appar, an features a wra gwell
4. Testa an edge cases: py' wra dhisplaya ma ny wra terminal connectya?

### Dher 2: Preparya rag submission

An folder plugin a wra contain:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Anfonedh `package.json` fields:**

| Field | Disgrys | Esample |
|-------|-------------|---------|
| `name` | Unik kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantyk wersyon | `"1.0.0"` |
| `description` | Disgrys yn un-lin | `"Monitors nginx access logs in real-time"` |
| `author` | Dhyworth | `"John Doe"` |
| `main` | Pwynt mynsi | `"index.js"` |

**Fieldh ow kelwel:**

| Field | Disgrys |
|-------|-------------|
| `license` | Teyl license (MIT a vynn) |
| `keywords` | Rann ar gath tags |
| `soom.minVersion` | WIA SOOM wersyon isel a vynn |

### Dher 3: Anfon dres an Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** dhyworth an plugin dhyworth `plugins/{your-plugin-name}/`
3. **Anfon** Pull Request

### Dher 4: Gwel an gath ha derivas

Ni a wel an gath ow kelwel:

- **Securita** — na API dangerus (gwel [Securita Rulys](#security-rules))
- **Qualita** — a vynn ow kelwel? Yma an kod clean?
- **Useli** — a vynn ow solva probelm gwir?

Wosa derivas:
1. An plugin a vynn ow kelwel dhyworth `registry.json`
2. A ZIP bundle a vynn gans `dist/`
3. An plugin a vynn ow kelwel yn an **Plugin Store** rag oll WIA SOOM users!

---

## Rann 5: Best Practices

### Securita Rulys

An rulys yma **mandatory**. An plugins a vynn ow kelwel an rulys a vynn be rejected.

| Rule | Why |
|------|-----|
| **NEVER** usya `eval()` po `new Function()` | Risk kod injekshyn |
| **NEVER** usya `child_process`, `exec()`, `spawn()` | Usya `ctx.terminal.send()` yn unig rag kommandys |
| **NEVER** fetcha URL ow kelwel | Egsample: `wiasoom.com` API endpoints |
| **NEVER** accessa `process.env` | An environment variables a vynn gans secrets |
| **NEVER** usya `require('fs')` yn dirag | Usya `ctx.settings` rag storjya, `ctx.sftp` rag transferrya fyle |
| **NEVER** usya npm external packages | Pure JavaScript yn unig — na node_modules |
| **MUST** usya `ctx.terminal.send()` rag oll kommandys remota | Henn a gela yn an secure SSH channel |
| **MUST** clean up yn `deactivate()` | Dhos an listeners, clear an intervals |

### Error Handling

A-wosa gela an risky operations yn try/catch:
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
### Cleanup yn deactivate()

Myns an plugin a gela intervals, listeners, po subscriptions — clean dhevel:
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
### i18n Support

WIA SOOM a vynn ow kelwel 254 yethow. Rag gans an plugin label ow kelwel translatable, usya an approach simpl:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## Rann 6: Esamples yn Dreal-World

### Esample 1: Server Disk Checker

Runs `df -h` war an server remota ha gela an space a usya/available yn an status bar.
§§§CHUNK_SEPARATOR§§§
---

### Esample 2: TODO Manager

An plugin a gela an TODO list usya an settings rag storjya persistenta ha an webview rag gela.

> **Design pattern:** Dref an webviews na vynn ow kelwel dirag an plugin APIs, an plugin yma usya an "snapshot" approach — he a redh an TODOs gans an settings, a gela hwi a HTML read-only, ha a provi actions based on sidebar rag addya items. An webview a vynn ow kelwel yn **display** layer, na form interactiv.

§§§CHUNK_SEPARATOR§§§
---

### Esample 3: Error Watcher

Monitors an terminal output ha anfon notification pan a vynn ow kelwel an patterns spesifik.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | Gweithredu generel an server |
| `devtools` | Kynsa geryow |
| `calculator` | Kynsorys ha konvertorys |
| `simulator` | Simulorys |
| `game` | Gemmow termynel |
| `business` | Kynsa geryow busnes |
| `security` | Diogelwch ha archwilieth |
| `web` | Gweithredu an server web |
| `education` | Kynsa geryow dyski |
| `health` | Kynsa geryow yeth |
| `islamic` | Kynsa geryow Islamek (amserow gweddi, etc.) |
| `science` | Kynsa geryow gwenyver |
| `quantum` | Kynsa geryow kwantum |
| `ai` | Kynsa geryow a vynnedh AI |
| `biotech` | Kynsa geryow biotechnologek |
| `space` | Kynsa geryow leun ha astromi |
| `network` | Kynsa geryow networek |
| `database` | Gweithredu an database |
| `monitoring` | Monitora an server |
| `devops` | DevOps ha CI/CD |
| `utility` | Kynsa geryow generel |
| `design` | Kynsa geryow dylans |
| `ecommerce` | Kynsa geryow e-fenn |
| `automation` | Kynsa geryow awtomatisedh |
| `kpop` | Kynsa geryow K-pop |
| `accessibility` | Kynsa geryow cledh |
| `analytics` | Analytik ha adroddi |
| `wia` | Kynsa geryow ekosystem WIA |
| `all` | Avel ow kelwel yn pob kategoria |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Gweithredu an server |
| `shield` | Diogelwch |
| `database` | Database |
| `activity` | Monitora |
| `terminal` | Kynsa geryow termynel |
| `code` | Kynsa geryow |
| `hard-drive` | Disk/archiv |
| `network` | Networek |
| `lock` | Auth/crypti |
| `eye` | Gwelwel/monitora |
| `check-square` | Tasow/TODO |
| `layout-dashboard` | Dashbords |
| `settings` | Konfigurasyon |
| `zap` | Awtomatisedh |
| `globe` | Web/internasional |

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
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

Returns `undefined` if the key doesn't exist.

#### `settings.set(key, value)`

Save a value. Supports strings, numbers, booleans, arrays, and objects.

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**Example: Remember user preferences**

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

> **Status: Coming Soon** — The AI API is defined but not yet connected to Soomy. Currently returns `{ response: 'AI not yet connected' }`. Full AI integration is planned for a future release.

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

## Part 3: Building Custom UI with Webviews

The `openWebview()` API lets you build dashboard UIs with HTML, CSS, and JavaScript — all inside a popup window.

> **Important limitation:** Webviews are **display-only**. They cannot call back into plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Use sidebar buttons for all user actions, and use `openWebview()` to display current state. If you need interactive features, trigger them from sidebar buttons and re-open the webview to refresh the display.

### Pattern: Terminal Command → Parse Output → Show in HTML

This is the most common plugin pattern. You run a command, parse the result, and display it visually.

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

### Pattern: Displaying Settings in a Webview

> **Note:** Webviews are display-only — they cannot call back into plugin APIs. Use `ctx.settings` in your sidebar button handlers to modify settings, and use `openWebview()` to show the current state.

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

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Copy your plugin to `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verify it works: sidebar button appears, features work correctly
4. Test edge cases: what happens if no terminal is connected?

### Step 2: Prepare for submission

Your plugin folder must contain:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | One-line description | `"Monitors nginx access logs in real-time"` |
| `author` | Your name | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | License type (MIT recommended) |
| `keywords` | Array of search tags |
| `soom.minVersion` | Minimum WIA SOOM version required |

### Step 3: Submit to the Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** your plugin to `plugins/{your-plugin-name}/`
3. **Submit** a Pull Request

### Step 4: Review and approval

We review every plugin for:

- **Security** — no dangerous APIs (see [Security Rules](#security-rules))
- **Quality** — does it work? Is the code clean?
- **Usefulness** — does it solve a real problem?

After approval:
1. Your plugin is added to `registry.json`
2. A ZIP bundle is created in `dist/`
3. Your plugin appears in the **Plugin Store** for all WIA SOOM users!

---

## Part 5: Best Practices

### Security Rules

These rules are **mandatory**. Plugins that violate them will be rejected.

| Rule | Why |
|------|-----|
| **NEVER** use `eval()` or `new Function()` | Code injection risk |
| **NEVER** use `child_process`, `exec()`, `spawn()` | Only use `ctx.terminal.send()` for commands |
| **NEVER** fetch external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** access `process.env` | Environment variables may contain secrets |
| **NEVER** use `require('fs')` directly | Use `ctx.settings` for storage, `ctx.sftp` for file transfer |
| **NEVER** use npm external packages | Pure JavaScript only — no node_modules |
| **MUST** use `ctx.terminal.send()` for all remote commands | This goes through the secure SSH channel |
| **MUST** clean up in `deactivate()` | Remove listeners, clear intervals |

### Error Handling

Always wrap risky operations in try/catch:

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

### Cleanup in deactivate()

If your plugin creates intervals, listeners, or subscriptions — clean them up:

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
