<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Gid Devlopè Plugin WIA SOOM</h1>
<p align="center"><strong>Kreye pwòp plugin ou an 5 minit.</strong></p>
<p align="center">Kreye zouti sèvè pwisan, tablodbò, ak otomatik — dirèkteman nan WIA SOOM.</p>

---

## Tab Kontni

- [Pati 1: Kòmanse Rapid — Premye Plugin Ou an 5 Minit](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Pati 2: Referans API Kontèks Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Pati 3: Bati UI Pèsonalize ak Webviews](#part-3-building-custom-ui-with-webviews)
- [Pati 4: Piblikasyon Plugin Ou](#part-4-publishing-your-plugin)
- [Pati 5: Pi Bon Pratik](#part-5-best-practices)
- [Pati 6: Egzanp Reyèl](#part-6-real-world-examples)
- [Apendis: Kategori & Ikon](#appendix-categories--icons)

---

## Pati 1: Kòmanse Rapid — Premye Plugin Ou an 5 Minit

### Sa w ap bati

Yon plugin "Hello World" ki ajoute yon bouton nan ba latè. Lè ou klike sou li, li montre yon notifikasyon.

### Etap 1: Kreye katab plugin nan
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Etap 2: Kreye package.json
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
**Champs obligatwa:** `name`, `version`, `description`, `author`, `main`

### Etap 3: Kreye index.js
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
### Etap 4: Rekòmanse WIA SOOM

Rekòmanse aplikasyon an (oswa chanje plugin nan sou/off nan Anviwònman → Plugins).

Ou ta dwe wè yon **"Hello World"** bouton nan ba latè a. Klike sou li — ou pral wè yon notifikasyon siksè!

### Kijan sa mache
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

## Pati 2: Referans API Kontèks Plugin

Lè fonksyon ou `activate(context)` a rele, `context` (oswa `ctx`) bay API sa yo:
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

### `ctx.terminal` — Kouri kòmand sou sèvè aleka

#### `terminal.send(sessionId, data)`

Voye yon kòmand (oswa nenpòt done) nan yon sesyon tèminal aktif.

| Paramèt | Kalite | Deskripsyon |
|---------|--------|-------------|
| `sessionId` | `string` | Sesyon tèminal pou voye a |
| `data` | `string` | Kòmand oswa done pou voye |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abòne a tout sòti ki soti nan yon sesyon tèminal. Retounen yon **fonksyon dezabòne**.

| Paramèt | Kalite | Deskripsyon |
|---------|--------|-------------|
| `sessionId` | `string` | Sesyon tèminal pou gade |
| `callback` | `(data: string) => void` | Rele ak chak moso sòti |
| **Retounen** | `() => void` | Rele sa a pou sispann koute |
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
**Enpòtan:** Toujou sove fonksyon dezabòne a epi rele li nan `deactivate()` pou evite flit memwa.

---

### `ctx.sftp` — Transfè dosye

> **Estati: Ap vini byento** — API SFTP a defini men li poko konekte ak motè SFTP aplikasyon an. `list()` aktyèlman retounen yon tablo vid, ak `upload()`/`download()` se no-ops. Sa a pral konplètman aplike nan yon lage nan lavni. Pou kounye a, itilize `ctx.terminal.send()` ak kòmand `scp` oswa `rsync` kòm yon solisyon tanporè.

#### `sftp.list(sessionId, path)`

Lis dosye nan yon repèrtwa aleka.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Telechaje yon dosye soti nan machin lokal la ale nan sèvè aleka.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Telechaje yon dosye soti nan sèvè aleka ale nan machin lokal la.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solisyon tanporè (jiskaske API SFTP a aktif):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Entèfas itilizatè

#### `ui.addSidebarButton(options)`

Ajoute yon bouton nan ba latè WIA SOOM.

| Opsyon | Kalite | Obligatwa | Deskripsyon |
|--------|--------|-----------|-------------|
| `id` | `string` | Non | ID inik (default a non plugin nan) |
| `icon` | `string` | Wi | Non ikon Lucide (pa egzanp, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Wi | Tèks bouton ki montre nan ba latè |
| `onClick` | `() => void` | Wi | Fonksyon ki rele lè bouton an klike |
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
**Referans ikon:** Browse tout ikon ki disponib nan [lucide.dev/icons](https://lucide.dev/icons)

> **Nòt sou konpatibilite:** Kèk plugin pi ansyen itilize agiman pozisyon tankou `addSidebarButton(id, icon, label, onClick)`. API ofisyèl la itilize yon **objè opsyon** jan sa dokimante pi wo a. Toujou itilize stil objè a pou nouvo plugin.

#### `ui.openWebview(options)`

Ouvri yon fenèt popup ak kontni HTML pèsonalize. Se konsa ou bati UI rich yo.

| Opsyon | Kalite | Deskripsyon |
|--------|--------|-------------|
| `title` | `string` | Tit fenèt la |
| `html` | `string` | Kontni HTML konplè pou rann |
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
> Gade [Pati 3](#part-3-building-custom-ui-with-webviews) pou modèl avanse webview yo.

#### `ui.showNotification(type, message)`

Montre yon notifikasyon toast.

| Paramèt | Tip | Deskripsyon |
|---------|-----|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stil notifikasyon an |
| `message` | `string` | Tèks pou montre |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ajoute yon atik tèks ki pèsistan nan ba estati anba a.

| Paramèt | Tip | Deskripsyon |
|---------|-----|-------------|
| `id` | `string` | ID inik pou atik estati sa a |
| `text` | `string` | Tèks pou afiche |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Depo pèsistan

Anviwònman plugin yo estoke pèmanan nan `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Li yon valè ki sove.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Retounen `undefined` si kle a pa egziste.

#### `settings.set(key, value)`

Sove yon valè. Sipòte chenn, nimewo, booleans, tablo, ak objè.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Egzanp: Sonje preferans itilizatè yo**
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

### `ctx.ai` — Entegrasyon AI

> **Estati: Ap vini byento** — API AI a defini men li poko konekte ak Soomy. Kounye a li retounen `{ response: 'AI not yet connected' }`. Entegrasyon AI konplè planifye pou yon lage nan lavni.

#### `ai.chat(messages, options?)`

Voye mesaj bay asistan AI a (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Pati 3: Konstwi UI Pèsonalize ak Webviews

API `openWebview()` la pèmèt ou konstwi UI tablodbò ak HTML, CSS, ak JavaScript — tout andedan yon fenèt popup.

> **Limitasyon enpòtan:** Webviews yo se **sèlman pou afichaj**. Yo pa ka rele tounen nan API plugin yo (`ctx.settings`, `ctx.terminal`, elatriye). Itilize bouton sidebar pou tout aksyon itilizatè yo, epi itilize `openWebview()` pou montre eta aktyèl la. Si ou bezwen karakteristik entèaktif, deklanche yo soti nan bouton sidebar yo epi re-ouvri webview la pou rafrechi afichaj la.

### Modèl: Kòmand Terminal → Parse Rezilta → Montre nan HTML

Sa a se modèl plugin ki pi komen. Ou kouri yon kòmand, parse rezilta a, epi montre li vizyèlman.
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
### Modèl: Tablodbò Entèaktif ak Auto-Rafrechisman
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
### Modèl: Montre Anviwònman nan yon Webview

> **Remak:** Webviews yo se sèlman pou afichaj — yo pa ka rele tounen nan API plugin yo. Itilize `ctx.settings` nan manadjè bouton sidebar ou yo pou modifye anviwònman yo, epi itilize `openWebview()` pou montre eta aktyèl la.
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

## Pati 4: Piblikasyon Plugin Ou

### Etap 1: Tès lokalman

1. Kopye plugin ou a nan `~/.wia-soom/plugins/{your-plugin}/`
2. Rekòmanse WIA SOOM
3. Verifye li mache: bouton sidebar la parèt, karakteristik yo mache kòrèkteman
4. Tès ka limit: ki sa ki rive si pa gen okenn terminal ki konekte?

### Etap 2: Prepare pou soumèt

Dossier plugin ou a dwe gen:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Champs obligatwa `package.json`:**

| Champ | Deskripsyon | Egzanp |
|-------|-------------|---------|
| `name` | ID inik nan kebab-case | `"my-awesome-plugin"` |
| `version` | Vèsyon semantik | `"1.0.0"` |
| `description` | Deskripsyon an yon fraz | `"Monitors nginx access logs in real-time"` |
| `author` | Non ou | `"John Doe"` |
| `main` | Pwen antre | `"index.js"` |

**Champs opsyonèl:**

| Champ | Deskripsyon |
|-------|-------------|
| `license` | Kalite lisans (MIT rekòmande) |
| `keywords` | Tablo etikèt rechèch |
| `soom.minVersion` | Vèsyon WIA SOOM minimòm ki nesesè |

### Etap 3: Soumèt nan Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Ajoute** plugin ou nan `plugins/{your-plugin-name}/`
3. **Soumèt** yon Pull Request

### Etap 4: Revizyon ak apwobasyon

Nou revize chak plugin pou:

- **Sekirite** — pa gen APIs danjere (gade [Règleman Sekirite](#security-rules))
- **Kalite** — èske li mache? Èske kòd la pwòp?
- **Itilite** — èske li rezoud yon pwoblèm reyèl?

Apre apwobasyon:
1. Plugin ou a ajoute nan `registry.json`
2. Yon ZIP bundle kreye nan `dist/`
3. Plugin ou a parèt nan **Plugin Store** pou tout itilizatè WIA SOOM!

---

## Pati 5: Pi Bon Pratik

### Règleman Sekirite

Règleman sa yo se **obligatwa**. Plugins ki vyole yo ap rejte.

| Règle | Poukisa |
|------|-----|
| **JAMAIS** itilize `eval()` oswa `new Function()` | Risk enjeksyon kòd |
| **JAMAIS** itilize `child_process`, `exec()`, `spawn()` | Sèlman itilize `ctx.terminal.send()` pou kòmand yo |
| **JAMAIS** fè demann pou URL ekstèn | Eksepsyon: `wiasoom.com` API endpoints |
| **JAMAIS** aksede `process.env` | Varyab anviwònman ka gen sekrè |
| **JAMAIS** itilize `require('fs')` dirèkteman | Itilize `ctx.settings` pou depo, `ctx.sftp` pou transfè dosye |
| **JAMAIS** itilize pakè ekstèn npm | Sèlman JavaScript pwòp — pa gen node_modules |
| **DWE** itilize `ctx.terminal.send()` pou tout kòmand remote | Sa a pase atravè chanèl SSH sekirize |
| **DWE** netwaye nan `deactivate()` | Retire dinamik, netwaye entèval |

### Jere Erè

Toujou vlope operasyon riske yo nan try/catch:
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
### Netwayaj nan deactivate()

Si plugin ou a kreye entèval, dinamik, oswa abònman — netwaye yo:
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
### Sipò i18n

WIA SOOM sipòte 254 lang. Pou fè etikèt plugin ou a tradui, itilize yon apwòch senp:
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

## Pati 6: Egzanp nan Mond Reyèl

### Egzanp 1: Tcheke Disk Sèvè

Kouri `df -h` sou sèvè remote a epi montre espas itilize/disponib nan ba estati a.
§§§CHUNK_SEPARATOR§§§
---

### Egzanp 2: Manadjè TODO

Yon plugin ki jere yon lis TODO ki itilize anviwònman pou depo pèmanan ak yon webview pou afichaj.

> **Modèl konsepsyon:** Depi webviews pa ka rele APIs plugin dirèkteman, plugin sa a itilize yon apwòch "snapshot" — li li TODO yo nan anviwònman, rann yo kòm HTML ki sèlman pou li, epi bay aksyon ki baze sou sidebar pou ajoute atik. Webview a se yon **kouch afichaj**, pa yon fòm entèaktif.
§§§CHUNK_SEPARATOR§§§
---

### Egzanp 3: Gade Erè

Siveye pwodiksyon tèminal la epi voye yon notifikasyon lè modèl espesifik yo detekte.
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

## Apendis: Kategori & Ikon

### Kategori Plugin (29)

Sèvi ak sa yo nan `package.json` `keywords` ou oswa lè w ap soumèt nan rejis la:

| Kategori | Deskripsyon |
|----------|-------------|
| `server` | Jere sèvè jeneral |
| `devtools` | Zouti devlopman |
| `calculator` | Kalkilatè ak konvètisè |
| `simulator` | Similatè |
| `game` | Jwèt tèminal |
| `business` | Zouti biznis |
| `security` | Sekirite ak odit |
| `web` | Jere sèvè entènèt |
| `education` | Zouti edikatif |
| `health` | Zouti ki gen rapò ak sante |
| `islamic` | Zouti Islamik (tan lapriyè, elatriye) |
| `science` | Zouti syantifik |
| `quantum` | Zouti pou òdinatè kwantik |
| `ai` | Zouti ki gen pouvwa AI |
| `biotech` | Zouti biyoteknoloji |
| `space` | Zouti espas ak astwonomi |
| `network` | Zouti rezo |
| `database` | Jere baz done |
| `monitoring` | Suivi sèvè |
| `devops` | DevOps ak CI/CD |
| `utility` | Zouti jeneral |
| `design` | Zouti konsepsyon |
| `ecommerce` | Zouti e-commerce |
| `automation` | Zouti otomatik |
| `kpop` | Zouti ki gen rapò ak K-pop |
| `accessibility` | Zouti aksè |
| `analytics` | Analiz ak rapò |
| `wia` | Zouti ekosistèm WIA |
| `all` | Ap parèt nan tout kategori |

### Ikon Rekòmande (Lucide)

| Non Ikon | Sèvi pou |
|-----------|---------|
| `server` | Jere sèvè |
| `shield` | Sekirite |
| `database` | Baz done |
| `activity` | Suivi |
| `terminal` | Zouti tèminal |
| `code` | Devlopman |
| `hard-drive` | Disk/jesyon depo |
| `network` | Rezo |
| `lock` | Otantifikasyon/kripte |
| `eye` | Gade/suivi |
| `check-square` | Tâches/TODO |
| `layout-dashboard` | Tablo de bord |
| `settings` | Konfigirasyon |
| `zap` | Otomatik |
| `globe` | Entènèt/entènasyonal |

Browse tout 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Bezwen Èd?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Egzanp Plugins:** [Website](https://wiasoom.com)
- **Sit wèb:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Kreye yon bagay etonan. Pataje li ak mond lan.</em></p>
<p align="center"><em>— Ekip WIA SOOM</em></p>
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
