<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Gid WIA SOOM Pou Devlopè Plugin</h1>
<p align="center"><strong>Kreye ou prop plugin an 5 minit.</strong></p>
<p align="center">Kreye zouti server pwisan, dashboards, ek automations — dirèk anndan WIA SOOM.</p>

---

## Tablo Konteni

- [Pati 1: Kòmansman Rapid — Ou Premye Plugin an 5 Minit](#pati-1-kòmansman-rapid--ou-premye-plugin-an-5-minit)
- [Pati 2: Referans API Kontèks Plugin](#pati-2-referans-api-kontèks-plugin)
  - [ctx.terminal](#ctxterminal--kouri-komannd-an-sèvè-remote)
  - [ctx.sftp](#ctxsftp--transfè-fichye)
  - [ctx.ui](#ctxui--entèfas-itilizatè)
  - [ctx.settings](#ctxsettings--stokaz-pèmanan)
  - [ctx.ai](#ctxai--entegre-ai)
- [Pati 3: Bati UI Pèsonalize avek Webviews](#pati-3-bati-ui-pèsonalize-avek-webviews)
- [Pati 4: Pibliye Ou Plugin](#pati-4-pibliye-ou-plugin)
- [Pati 5: Pi Bon Pratik](#pati-5-pi-bon-pratik)
- [Pati 6: Egzanp Nan Lavi Reyèl](#pati-6-egzanp-nan-lavi-reyèl)
- [Apendis: Kategori & Ikon](#apendis-kategori--ikon)

---

## Pati 1: Kòmansman Rapid — Ou Premye Plugin an 5 Minit

### Ki sa ou pral bati

En "Hello World" plugin ki ajoute en bouton dan sidebar. Kan ou klike lo, i montre en notifikasyon.

### Etap 1: Kreye le folder plugin
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
**Lazim domenn:** `name`, `version`, `description`, `author`, `main`

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
### Etap 4: Restart WIA SOOM

Restart l'aplikasyon (ou bien toggle le plugin off/on dan Settings → Plugins).

Ou devret vwar en **"Hello World"** bouton dan le sidebar. Klike lo — ou va vwar en notifikasyon siksè!

### Kouman i marche
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

Kan ou `activate(context)` fonksyon i apel, `context` (ou bien `ctx`) i donn sa bann APIs:
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

### `ctx.terminal` — Kouri komannd an sèvè remote

#### `terminal.send(sessionId, data)`

Voye en komannd (ou bien okenn done) dan en sesyon terminal aktif.

| Paramèt | Tip | Deskripsyon |
|---------|-----|-------------|
| `sessionId` | `string` | Sesyon terminal pou voye |
| `data` | `string` | Komand ou done pou voye |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abon lo tou sorti depi en sesyon terminal. I retourn en **fonksyon dezabone**.

| Paramèt | Tip | Deskripsyon |
|---------|-----|-------------|
| `sessionId` | `string` | Sesyon terminal pou swiv |
| `callback` | `(data: string) => void` | Apele avek sak chunk sorti |
| **Retourn** | `() => void` | Apele sa pou arete ekout |
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
**Enportan:** Toultan sove le fonksyon dezabone ek apel li dan `deactivate()` pou evite leak memwar.

---

### `ctx.sftp` — Transfè fichye

> **Leta: Vini Byento** — API SFTP i defini me pa ankor konekte avek motè SFTP l'aplikasyon. `list()` i retourn en aray vid, ek `upload()`/`download()` i no-ops. Sa pou ganny aplike konpletman dan en lavni lage. Pou le moman, servi `ctx.terminal.send()` avek `scp` ou `rsync` koman en workaround.

#### `sftp.list(sessionId, path)`

List fichye dan en rezo remote.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Voye en fichye depi lokal machin pou ale dan sèvè remote.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Telechaje en fichye depi sèvè remote pou ale dan lokal machin.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (zanmen API SFTP i aktif):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Entèfas itilizatè

#### `ui.addSidebarButton(options)`

Ajoute en bouton dan le sidebar WIA SOOM.

| Opsyon | Tip | Lazim | Deskripsyon |
|--------|-----|-------|-------------|
| `id` | `string` | Non | ID inik (i default a non plugin) |
| `icon` | `string` | Wi | Non ikon Lucide (par egzanp, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Wi | Tekst bouton montre dan sidebar |
| `onClick` | `() => void` | Wi | Fonksyon ki apel kan bouton i klike |
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
**Referans ikon:** Browse tou ikon ki disponib lo [lucide.dev/icons](https://lucide.dev/icons)

> **Nòt konpatibilite:** Kèk ansyen plugins i servi argument pozisyonel parey `addSidebarButton(id, icon, label, onClick)`. API ofisyel i servi en **objè opsyon** parey i dokimante pi o. Toultan servi stil objè pou nouvo plugins.

#### `ui.openWebview(options)`

Ouvrir en fenetre popup avek kontni HTML pèsonalize. Sa i fason ou bati UI rich.

| Opsyon | Tip | Deskripsyon |
|--------|-----|-------------|
| `title` | `string` | Tit fenetre |
| `html` | `string` | Kontni HTML konplet pou rann |
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
> Vwar [Part 3](#part-3-building-custom-ui-with-webviews) pou modèl webview avanse.

#### `ui.showNotification(type, message)`

Montre en notifikasyon toast.

| Parameter | Type | Deskripsyon |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stil notifikasyon |
| `message` | `string` | Tekst pou montre |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ajoute en atik tekst persistan dan ba status anba.

| Parameter | Type | Deskripsyon |
|-----------|------|-------------|
| `id` | `string` | ID inik pou sa atik status |
| `text` | `string` | Tekst pou afiche |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Depo persistan

Anviwonnman plugin i reste anrejistre permanan dan `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Li en valer anrejistre.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Retourn `undefined` si sa kle pa egziste.

#### `settings.set(key, value)`

Anrejistre en valer. I sipport string, nomb, boolean, aray, ek objè.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Egzanp: Rappel preferans itilizater**
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

> **Status: Coming Soon** — API AI i defini me pa ankor konekte avek Soomy. I retourn `{ response: 'AI not yet connected' }`. Entegrasyon konplè AI i planifye pou en lavni lage.

#### `ai.chat(messages, options?)`

Voye mesaj avek asistan AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Bati UI Pèsonalize avek Webviews

API `openWebview()` i permet ou bati UI dashboard avek HTML, CSS, ek JavaScript — tou dan en fenetre popup.

> **Limitation enportan:** Webviews i **display-only**. Zot pa kapab apel tounen dan API plugin (`ctx.settings`, `ctx.terminal`, elatriye). Utiliz bouton sidebar pou tou aksyon itilizater, ek servi `openWebview()` pou montre leta aktyel. Si ou bezwen karakteristik entèaktif, déclenche zot depi bouton sidebar ek re-ouver le webview pou rafrechi l'affichage.

### Modèl: Komann Terminal → Parse Output → Montre dan HTML

Sa i pli komen modèl plugin. Ou fer en komann, parse rezilta, ek montre li vizyèlman.
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
### Modèl: Dashboard Entèaktif avek Auto-Rafrechi
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
### Modèl: Montre Anviwonnman dan en Webview

> **Remak:** Webviews i display-only — zot pa kapab apel tounen dan API plugin. Utiliz `ctx.settings` dan ou bouton sidebar handlers pou modifye anviwonnman, ek servi `openWebview()` pou montre leta aktyel.
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

## Part 4: Pibliye Ou Plugin

### Etap 1: Test lokalman

1. Kopi ou plugin dan `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verifye i marche: bouton sidebar i aparé, karakteristik i travay korekteman
4. Test edge cases: ki arive si pa annan okenn terminal konekte?

### Etap 2: Prepare pou soumission

Ou dossier plugin i devret kontenir:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Reki `package.json` ki bezwen:**

| Field | Deskripsyon | Egzanp |
|-------|-------------|---------|
| `name` | ID inik an kebab-case | `"mon-awesome-plugin"` |
| `version` | Vèsyon semantik | `"1.0.0"` |
| `description` | Deskripsyon an enn line | `"Monitors nginx access logs in real-time"` |
| `author` | Ou non | `"John Doe"` |
| `main` | Pwen d'entré | `"index.js"` |

**Reki opsyonel:**

| Field | Deskripsyon |
|-------|-------------|
| `license` | Kalite lisans (MIT rekòmande) |
| `keywords` | Lis tag rechèch |
| `soom.minVersion` | Minim WIA SOOM vèsyon ki bezwen |

### Etap 3: Soumet dan Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Ajoute** ou plugin dan `plugins/{ou-plugin-non}/`
3. **Soumet** enn Pull Request

### Etap 4: Revizyon ek apwobasyon

Nou revize tou le plugin pou:

- **Sekirite** — pa ni API danzere (gade [Sekirite Règleman](#security-rules))
- **Kalite** — eski i travay? Eski kod la prop?
- **Itilite** — eski i rezoud enn vre problèm?

Apre apwobasyon:
1. Ou plugin i ajoute dan `registry.json`
2. Enn ZIP bundle i kreye dan `dist/`
3. Ou plugin i parèt dan **Plugin Store** pou tou le itilizater WIA SOOM!

---

## Pati 5: Mezi Pratik

### Sekirite Règleman

Sa bann règleman la i **mandatwar**. Bann plugin ki vyole zot pou ganny refize.

| Règleman | Poukwa |
|------|-----|
| **NEVÉ** servi `eval()` ou `new Function()` | Risk kod injection |
| **NEVÉ** servi `child_process`, `exec()`, `spawn()` | Sèvi zis `ctx.terminal.send()` pou bann lòd |
| **NEVÉ** al cherche bann URL ekstèn | Eksepsyon: `wiasoom.com` API endpoints |
| **NEVÉ** al aksede `process.env` | Bann varyab anviwonnman i kapab kontenir sekrè |
| **NEVÉ** servi `require('fs')` dirèkteman | Sèvi `ctx.settings` pou stockaz, `ctx.sftp` pou transfè dosye |
| **NEVÉ** servi npm bann pakèt ekstèn | Zis JavaScript pur — pa node_modules |
| **DOIT** servi `ctx.terminal.send()` pou tou bann lòd remote | Sa i pase atraver kanal SSH sekirize |
| **DOIT** netwaye dan `deactivate()` | Retire bann listeners, klir intervals |

### Erè Manzé

Toulezour anvlop bann operasyon riske dan try/catch:
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
### Netwayaz dan deactivate()

Si ou plugin i kreye intervals, listeners, ou subscriptions — netwaye zot:
§§§CHUNK_SEPARATOR§§§
### i18n Sipò

WIA SOOM i sipò 254 lang. Pou fer ou plugin label tradiktab, servi enn apwòch senp:
§§§CHUNK_SEPARATOR§§§
---

## Pati 6: Egzanp Dan Lavi Reel

### Egzanp 1: Server Disk Checker

I fer `df -h` lo server remote ek montre espas itilize/ki disponib dan status bar.
§§§CHUNK_SEPARATOR§§§
---

### Egzanp 2: TODO Manager

En plugin ki geré enn lis TODO an servi bann settings pou stockaz persistan ek enn webview pou afichaz.

> **Modèl design:** Depi bann webviews pa kapab apel dirèkteman bann API plugin, sa plugin i servi enn apwòch "snapshot" — i lir bann TODO depi bann settings, i rann zot kom HTML ki li lis, ek i donn aksyon baze lo sidebar pou ajoute bann item. La webview i enn **kouch** afichaz, pa enn fòm entèaktif.
§§§CHUNK_SEPARATOR§§§
---

### Egzanp 3: Error Watcher

I monitore sorti terminal ek i voye enn notifikasyon kan bann modèl spesifik i ganny detekte.
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

## Apendis: Kategori & Ikon

### Kategori Plugin (29)

Utiliz sa dan ou `package.json` `keywords` ouswa kan ou soumet dan registry:

| Kategori | Deskripsyon |
|----------|-------------|
| `server` | Jesyon server jeneral |
| `devtools` | Zouti devlopman |
| `calculator` | Kalkilatè ek konvètisè |
| `simulator` | Similatè |
| `game` | Ludi terminal |
| `business` | Zouti biznis |
| `security` | Sekirite ek audit |
| `web` | Jesyon server web |
| `education` | Zouti edikasyon |
| `health` | Zouti ki an relasyon avek lasante |
| `islamic` | Zouti islamik (tan priyèr, el.) |
| `science` | Zouti syantifik |
| `quantum` | Zouti pou konpitasyon quantum |
| `ai` | Zouti ki powered par AI |
| `biotech` | Zouti biotechnologie |
| `space` | Zouti espas ek astronomie |
| `network` | Zouti rezo |
| `database` | Jesyon database |
| `monitoring` | Monitoring server |
| `devops` | DevOps ek CI/CD |
| `utility` | Zouti jeneral |
| `design` | Zouti design |
| `ecommerce` | Zouti e-commerce |
| `automation` | Zouti otomatik |
| `kpop` | Zouti ki an relasyon avek K-pop |
| `accessibility` | Zouti aksesibilite |
| `analytics` | Analiz ek rapò |
| `wia` | Zouti ekosistèm WIA |
| `all` | Pare dan tou kategori |

### Ikon Rekomande (Lucide)

| Non Ikon | Utiliz pou |
|-----------|---------|
| `server` | Jesyon server |
| `shield` | Sekirite |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Zouti terminal |
| `code` | Devlopman |
| `hard-drive` | Disk/storaz |
| `network` | Rezo |
| `lock` | Otorizasyon/enkrypsyon |
| `eye` | Gade/monitoring |
| `check-square` | Taks/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfigurasyon |
| `zap` | Otomatik |
| `globe` | Web/enternasyonal |

Browse tou 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Bezwen Ed?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Exanple Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Konstwi enn keksoz etonan. Pataze li avek lemonn.</em></p>
<p align="center"><em>— Ekip WIA SOOM</em></p>
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
