<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Leabhar-stiùiridh Leasachaidh Plugin WIA SOOM</h1>
<p align="center"><strong>Cruthaich do phlugin fhèin ann an 5 mionaidean.</strong></p>
<p align="center">Cruthaich innealan freagairte, dashboards, agus fèin-ghluasadan — dìreach taobh a-staigh WIA SOOM.</p>

---

## Clàr-innse

- [Part 1: Fast Start — Do Phlugin Àrsaidh ann an 5 Mionaidean](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Part 2: Plugin Context API Freagairte](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: A' Togail UI Custom le Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Foillseachadh do Phlugin](#part-4-publishing-your-plugin)
- [Part 5: Na Cleachdaidhean as Fheàrr](#part-5-best-practices)
- [Part 6: Eisimpleirean Fìor-shaoghail](#part-6-real-world-examples)
- [Appendix: Roinnean & Ìomhaighean](#appendix-categories--icons)

---

## Part 1: Fast Start — Do Phlugin Àrsaidh ann an 5 Mionaidean

### Dè thèid thu a thogail

Plugin "Hello World" a thogras putan anns a' phannal taobh. Nuair a thèid e a bhualadh, seallaidh e rabhadh.

### Ceum 1: Cruthaich am pasgan plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Ceum 2: Cruthaich package.json
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
**Raon riatanach:** `name`, `version`, `description`, `author`, `main`

### Ceum 3: Cruthaich index.js
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
### Ceum 4: Ath-thòisich WIA SOOM

Ath-thòisich an aplacaid (no atharrachadh a' phlugin dheth/air a' phàirt Settings → Plugins).

Bu chòir dhut putan **"Hello World"** fhaicinn anns a' phannal taobh. Cuir e — chì thu rabhadh soirbheachais!

### Mar a tha e ag obair
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

## Part 2: Plugin Context API Freagairte

Nuair a thèid do ghnìomh `activate(context)` a ghairm, bidh `context` (no `ctx`) a' toirt seachad na APIs sin:
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

### `ctx.terminal` — Ruith ordan air freagairtean iomallach

#### `terminal.send(sessionId, data)`

Cuir a-steach òrdugh (no dàta sam bith) gu seisean terminal gnìomhach.

| Paramadair | Seòrsa | Tuairisgeul |
|------------|--------|-------------|
| `sessionId` | `string` | An seisean terminal a thèid a chuir gu |
| `data` | `string` | An òrdugh no dàta a thèid a chuir |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Clàraich airson a h-uile toradh bho shreath terminal. Thèid **gnìomh fo-sgrìobhaidh** a thoirt air ais.

| Paramadair | Seòrsa | Tuairisgeul |
|------------|--------|-------------|
| `sessionId` | `string` | An seisean terminal a thèid a choimhead |
| `callback` | `(data: string) => void` | Gairm le gach pìos toradh |
| **Thèid a thoirt air ais** | `() => void` | Gairm seo gus stad a chur air a bhith a' cluinntinn |
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
**Cudromach:** Cuir an-còmhnaidh an gnìomh fo-sgrìobhaidh air ais agus gairm e ann an `deactivate()` gus casg a chuir air freagairtean cuimhne.

---

### `ctx.sftp` — Gluasad faidhle

> **Staid: A' tighinn a dh'ionnsaigh** — Tha an API SFTP air a shònrachadh ach chan eil e air a cheangal ris an einnsean SFTP de na h-aplacaidean. Tha `list()` a' toirt air ais freagairtean falamh an-dràsta, agus tha `upload()`/`download()` mar no-ops. Thèid seo a chur an gnìomh gu h-iomlan ann an leigeil a-mach san àm ri teachd. A-nis, cleachd `ctx.terminal.send()` le òrdughan `scp` no `rsync` mar dhòigh-obrach.

#### `sftp.list(sessionId, path)`

Liostaich faidhlichean ann an earrann iomallach.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Luchdaich suas faidhle bho inneal ionadail gu freagairtean iomallach.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Luchdaich sìos faidhle bho freagairtean iomallach gu inneal ionadail.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Dòigh-obrach (gus an tèid API SFTP a chuir an gnìomh):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Freagairtean cleachdaiche

#### `ui.addSidebarButton(options)`

Cuir putan ris a' phannal taobh WIA SOOM.

| Roghainn | Seòrsa | Riatanach | Tuairisgeul |
|----------|--------|-----------|-------------|
| `id` | `string` | Chan eil | ID sònraichte (a' dol gu ainm a' phlugin) |
| `icon` | `string` | Tha | Ainm ìomhaigh Lucide (mar eisimpleir, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Tha | Teacsa a' phutain a thèid a shealltainn anns a' phannal taobh |
| `onClick` | `() => void` | Tha | Gnìomh a thèid a ghairm nuair a thèid a' phutan a bhualadh |
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
**Freagairte ìomhaigh:** Thoir sùil air na h-ìomhaighean ri fhaighinn aig [lucide.dev/icons](https://lucide.dev/icons)

> **Nota freagairte:** Bidh cuid de phlugaichean nas sine a' cleachdadh argamaidean suidheachail mar `addSidebarButton(id, icon, label, onClick)`. Tha an API oifigeil a' cleachdadh **obrachadh roghainnean** mar a tha air a sgrìobhadh gu h-àrd. Cleachd an stoidhle obrachadh an-còmhnaidh airson na plugaichean ùra.

#### `ui.openWebview(options)`

Fosgail uinneag pop-up le susbaint HTML gnàthaichte. Seo mar a thogras tu UI beairteach.

| Roghainn | Seòrsa | Tuairisgeul |
|----------|--------|-------------|
| `title` | `string` | Tiotal na h-uinneige |
| `html` | `string` | Susbaint HTML iomlan a thèid a thaisbeanadh |
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
> Faic [Part 3](#part-3-building-custom-ui-with-webviews) airson pàtranan adhartach webview.

#### `ui.showNotification(type, message)`

Seall freagairtean toast.

| Paramadair | Seòrsa | Tuairisgeul |
|------------|--------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stoidhle freagairtean |
| `message` | `string` | Teacsa airson sealltainn |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Cuir freagairtean teacsa maireannach ris a' phannal stàite aig a' bhonn.

| Paramadair | Seòrsa | Tuairisgeul |
|------------|--------|-------------|
| `id` | `string` | ID unique airson an freagairtean stàite seo |
| `text` | `string` | Teacsa airson taisbeanadh |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Stòras maireannach

Tha freagairtean plugin air an stòradh gu maireannach ann an `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Leugh luach air a shàbhaladh.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Tillidh `undefined` ma tha an clé gun a bhith ann.

#### `settings.set(key, value)`

Sàbhail luach. Tha taic aig a’ mhodail seo do shreathan, àireamhan, booleans, freagairtean, agus nithean.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Example: Cuimhnich air roghainnean an neach-cleachdaidh**
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

### `ctx.ai` — Integreachadh AI

> **Status: A' tighinn a dh'aithghearr** — Tha an API AI air a mholadh ach fhathast gun a bhith ceangailte ri Soomy. A-nis tha e a' tillidh `{ response: 'AI not yet connected' }`. Tha integreachadh AI làn air a bhith air a phlanadh airson leigeil ma sgaoil san àm ri teachd.

#### `ai.chat(messages, options?)`

Cuir teachdaireachdan gu neach-taic AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: A' togail UI Custom le Webviews

Tha an API `openWebview()` a' leigeil leat UI dashboard a thogail le HTML, CSS, agus JavaScript — a h-uile càil taobh a-staigh uinneag popup.

> **Cuingealachd chudromach:** Tha webviews **a-mhàin airson taisbeanadh**. Chan urrainn dhaibh freagairtean a ghairm air ais gu API freagairtean (`ctx.settings`, `ctx.terminal`, msaa). Cleachd putanan taobh airson na h-gnìomhan uile, agus cleachd `openWebview()` airson an stàit a thaisbeanadh. Ma tha feartan eadar-obrachail agad, cuir iad air bhog bho putanan taobh agus fosgail a-rithist an webview gus an taisbeanadh ùrachadh.

### Pàtran: Àithne Terminal → Parse Output → Seall ann an HTML

Is e seo am pàtran plugin as cumanta. Bidh thu a’ ruith àithne, a’ parseadh an toradh, agus a’ taisbeanadh e gu lèir.
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
### Pàtran: Dashboard Eadar-obrachail le Auto-Refresh
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
### Pàtran: A' taisbeanadh Freagairtean ann an Webview

> **Nota:** Tha webviews a-mhàin airson taisbeanadh — chan urrainn dhaibh freagairtean a ghairm air ais gu API freagairtean. Cleachd `ctx.settings` ann an do làimhseadairean putanan taobh gus freagairtean atharrachadh, agus cleachd `openWebview()` gus an stàit a thaisbeanadh.
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

## Part 4: Foillseachadh do Plugin

### Ceum 1: Deuchainn gu h-ionadail

1. Cuir do phlugin gu `~/.wia-soom/plugins/{your-plugin}/`
2. Ath-thòisich WIA SOOM
3. Dèan dearbhadh gu bheil e ag obair: tha putan taobh a' nochdadh, tha feartan ag obair gu ceart
4. Deuchainn chùisean oir: dè thachras ma tha terminal gun cheangal?

### Ceum 2: Deiseal airson cur a-steach

Feumaidh do fholder plugin a bhith a' toirt a-steach:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Feumail `package.json` raointean:**

| Raon | Tuairisgeul | Eisimpleir |
|-------|-------------|---------|
| `name` | ID unique ann an kebab-case | `"my-awesome-plugin"` |
| `version` | Dreachd semantic | `"1.0.0"` |
| `description` | Tuairisgeul aon-loidhne | `"Monitors nginx access logs in real-time"` |
| `author` | Do ainm | `"John Doe"` |
| `main` | Puing inntrigidh | `"index.js"` |

**Raointean roghnach:**

| Raon | Tuairisgeul |
|-------|-------------|
| `license` | Seòrsa cead (MIT molta) |
| `keywords` | Array de thaghaidhean rannsachaidh |
| `soom.minVersion` | Dreachd as ìsle de WIA SOOM a tha feumail |

### Ceum 3: Cur a-steach don Chlàr Plugin

1. ****Package** your plugin as a ZIP file
2. **Cuir** do phlugin gu `plugins/{your-plugin-name}/`
3. **Cur a-steach** iarrtas Pull

### Ceum 4: Ath-sgrùdadh agus freagairtean

Bidh sinn a’ dèanamh ath-sgrùdadh air gach pluinne airson:

- **Sàbhailteachd** — chan eil APIan cunnartach (faic [Riaghailtean Sàbhailteachd](#security-rules))
- **Càileachd** — a bheil e ag obair? A bheil an còd glan?
- **Feumail** — a bheil e a’ fuasgladh duilgheadas fìor?

Às deidh freagairtean:
1. Thèid do phlugin a chur ris `registry.json`
2. Thèid pacadh ZIP a chruthachadh ann an `dist/`
3. Tha do phlugin a’ nochdadh anns an **Plugin Store** airson a h-uile neach-cleachdaidh WIA SOOM!

---

## Pàirt 5: Na Cleachdaidhean as Fheàrr

### Riaghailtean Sàbhailteachd

Tha na riaghailtean sin **feumail**. Thèid pluinnean a bhriseas iad a dhiùltadh.

| Riaghailt | Carson |
|------|-----|
| **A-NIS** na cleachd `eval()` no `new Function()` | Rìs freagairtean còd |
| **A-NIS** na cleachd `child_process`, `exec()`, `spawn()` | Cleachd `ctx.terminal.send()` a-mhàin airson ordhaichean |
| **A-NIS** na faigh URLs a-muigh | Excep: `wiasoom.com` API endpoints |
| **A-NIS** na faigh `process.env` | Faodaidh freagairtean àrainneachd dèanamh seilbh |
| **A-NIS** na cleachd `require('fs')` gu dìreach | Cleachd `ctx.settings` airson stòras, `ctx.sftp` airson gluasad faidhle |
| **A-NIS** na cleachd pacadh a-muigh npm | JavaScript fìor a-mhàin — chan eil node_modules |
| **FEUMAIDH** cleachdadh `ctx.terminal.send()` airson a h-uile ordhaichean remote | Tha seo a’ dol tro an t-slighe SSH tèarainte |
| **FEUMAIDH** glanadh a dhèanamh ann an `deactivate()` | Thoir air falbh luchd-èisteachd, soilleir eadar-ama |

### Làimhseachadh Mì-thuigse

Cuir a h-uile obrachadh cunnartach ann an try/catch:
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
### Glanadh ann an deactivate()

Ma tha do phlugin a’ cruthachadh eadar-ama, luchd-èisteachd, no fo-sgrìobhadh — glan iad:
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
### Taic i18n

Tha WIA SOOM a’ toirt taic do 254 cànanan. Gus dèanamh cinnteach gu bheil do phlugin freagairteach, cleachd dòigh shimplidh:
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

## Pàirt 6: Eisimpleirean Fìor-Àrainneachd

### Eisimpleir 1: Seicear Diog Servair

Bidh e a’ ruith `df -h` air an t-seirbheis air falbh agus a’ sealltainn an àite a chaidh a chleachdadh/fhaighinn anns an staid bar.
§§§CHUNK_SEPARATOR§§§
---

### Eisimpleir 2: Manaidsear TODO

Pluinnean a bhios a’ riaghladh liosta TODO a’ cleachdadh freagairtean airson stòras seasmhach agus webview airson taisbeanadh.

> **Pàtran dealbhaidh:** Leis gu bheil webviews unable gu dìreach a’ cur fios gu APIan pluinnean, tha an pluinnean seo a’ cleachdadh dòigh "snapshot" — tha e a’ leughadh TODOan bho freagairtean, a’ cur an gnìomh iad mar HTML le leughadh a-mhàin, agus a’ toirt seachad gnìomhachdan stèidhichte air taobh airson a bhith a’ cur nithean ris. Tha an webview na **taisbeanadh** leaghan, chan e foirm eadar-obrachail.
§§§CHUNK_SEPARATOR§§§
---

### Eisimpleir 3: Sùil air Mì-thuigse

Bidh e a’ sùileachadh toradh terminal agus a’ cur fiosrachadh nuair a thèid pàtrain sònraichte a lorg.
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

## A' Phàirt: Roinnean & Ìomhaighean

### Roinnean Plugin (29)

Cleachd seo ann an do `package.json` `keywords` no nuair a tha thu a' cur a-steach don chlàr-gnothaich:

| Roinn | Tuairisgeul |
|-------|-------------|
| `server` | Rianachd fhrithealaiche coitcheann |
| `devtools` | Innealan leasachaidh |
| `calculator` | Innealan cunntachaidh agus atharrachaidh |
| `simulator` | Simulators |
| `game` | Geamannan teirminal |
| `business` | Innealan gnìomhachais |
| `security` | Tèarainteachd agus sgrùdadh |
| `web` | Rianachd fhrithealaiche lìn |
| `education` | Innealan foghlaim |
| `health` | Innealan co-cheangailte ri slàinte |
| `islamic` | Innealan Islamach (ùinean duain, msaa) |
| `science` | Innealan saidheans |
| `quantum` | Innealan coimpiutair quantum |
| `ai` | Innealan le cumhachd AI |
| `biotech` | Innealan biotecnology |
| `space` | Innealan spàisean agus rionnagach |
| `network` | Innealan lìonra |
| `database` | Rianachd stòran-dàta |
| `monitoring` | Sgrùdadh fhrithealaiche |
| `devops` | DevOps agus CI/CD |
| `utility` | Innealan coitcheann |
| `design` | Innealan dealbhaidh |
| `ecommerce` | Innealan e-malairt |
| `automation` | Innealan fèin-obrachail |
| `kpop` | Innealan co-cheangailte ri K-pop |
| `accessibility` | Innealan ruigsinneachd |
| `analytics` | Analasadh agus aithris |
| `wia` | Innealan ecosistema WIA |
| `all` | A' nochdadh ann an gach roinn |

### Ìomhaighean Molta (Lucide)

| Ainm Ìomhaigh | Cleachd airson |
|---------------|---------------|
| `server` | Rianachd fhrithealaiche |
| `shield` | Tèarainteachd |
| `database` | Stòr-dàta |
| `activity` | Sgrùdadh |
| `terminal` | Innealan teirminal |
| `code` | Leasachadh |
| `hard-drive` | Disg/stòradh |
| `network` | Lìonrachadh |
| `lock` | Auth/cryptography |
| `eye` | A' coimhead/sgrùdadh |
| `check-square` | Gnìomhan/TODO |
| `layout-dashboard` | Dashboardan |
| `settings` | Freagairtean |
| `zap` | Fèin-obrachadh |
| `globe` | Lìn/eadar-nàiseanta |

Briog air na h-ìomhaighean 1,500+: [lucide.dev/icons](https://lucide.dev/icons)

---

## Feumaidh Tu Cuideachadh?

- **GitHub Duilgheadasan:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Duilgheadasan Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Eisimpleirean Plugins:** [Website](https://wiasoom.com)
- **Làrach-lìn:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Cruthaich rudeigin iongantach. Roinn e leis an t-saoghal.</em></p>
<p align="center"><em>— Sgioba WIA SOOM</em></p>
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
