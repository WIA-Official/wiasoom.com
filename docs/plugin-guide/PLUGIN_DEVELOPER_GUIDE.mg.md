<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Torolàlana ho an'ny Mpamorona Plugin WIA SOOM</h1>
<p align="center"><strong>Manamboara plugin manokana ao anatin'ny 5 minitra.</strong></p>
<p align="center">Mametraha fitaovana matanjaka ho an'ny serveur, dashboards, ary automations — ao anatin'ny WIA SOOM.</p>

---

## Tabilao Fandaharana

- [Ampahany 1: Fanombohana Haingana — Ny Plugin Voalohany ao anatin'ny 5 Minitra](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Ampahany 2: Fanovàna API Context ho an'ny Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Ampahany 3: Fanamboarana UI Manokana miaraka amin'ny Webviews](#part-3-building-custom-ui-with-webviews)
- [Ampahany 4: Famoahana ny Plugin-nao](#part-4-publishing-your-plugin)
- [Ampahany 5: Fomba Fanao Tsara](#part-5-best-practices)
- [Ampahany 6: Ohatra amin'ny Tontolo Iainana](#part-6-real-world-examples)
- [Fanampiny: Sokajy & Sary](#appendix-categories--icons)

---

## Ampahany 1: Fanombohana Haingana — Ny Plugin Voalohany ao anatin'ny 5 Minitra

### Inona no hoforoninao

Plugin "Hello World" izay manampy bokotra ao amin'ny sidebar. Rehefa tsindriana, dia mampiseho fampandrenesana.

### Dingana 1: Mamorona ny lahatahiry plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Dingana 2: Mamorona package.json
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
**Tanjona ilaina:** `name`, `version`, `description`, `author`, `main`

### Dingana 3: Mamorona index.js
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
### Dingana 4: Avereno ny WIA SOOM

Avereno ny app (na ahitsio ny plugin ao amin'ny Settings → Plugins).

Tokony ho hitanao ny **"Hello World"** bokotra ao amin'ny sidebar. Tsindrio izany — hahita fampandrenesana fahombiazana ianao!

### Ahoana ny fiasan'izany
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

## Ampahany 2: Fanovàna API Context ho an'ny Plugin

Rehefa antsoina ny `activate(context)` function-nao, dia manome ireo API ireo ny `context` (na `ctx`):
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

### `ctx.terminal` — Mandehana baiko amin'ny serveur lavitra

#### `terminal.send(sessionId, data)`

Mandefa baiko (na data hafa) amin'ny session terminal mavitrika.

| Parameter | Karazana | Famaritana |
|-----------|----------|------------|
| `sessionId` | `string` | Ny session terminal alefa |
| `data` | `string` | Ny baiko na data alefa |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Misoratra anarana amin'ny vokatra rehetra avy amin'ny session terminal. Mamerina **fampiasa misoratra anarana**.

| Parameter | Karazana | Famaritana |
|-----------|----------|------------|
| `sessionId` | `string` | Ny session terminal hojerentsika |
| `callback` | `(data: string) => void` | Antsoina isaky ny chunk vokatra |
| **Mamerina** | `() => void` | Antsoy ity mba hifarana ny fihainoana |
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
**Zava-dehibe:** Aza adino ny mitahiry ny fampiasa misoratra anarana ary antsoy izany ao amin'ny `deactivate()` mba hisorohana ny fivoahan'ny fahatsiarovana.

---

### `ctx.sftp` — Fandefasana rakitra

> **Toe-javatra: Ho avy tsy ho ela** — Ny API SFTP dia voafaritra fa mbola tsy mifandray amin'ny motera SFTP an'ny app. Ny `list()` dia mamerina array foana amin'izao fotoana izao, ary ny `upload()`/`download()` dia tsy misy fiantraikany. Ho tanterahina tanteraka izany amin'ny famoahana ho avy. Amin'izao fotoana izao, ampiasao ny `ctx.terminal.send()` miaraka amin'ny baiko `scp` na `rsync` ho toy ny vahaolana.

#### `sftp.list(sessionId, path)`

Mamerina ny rakitra ao amin'ny lahatahiry lavitra.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Mandefa rakitra avy amin'ny milina eo an-toerana mankany amin'ny serveur lavitra.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Misintona rakitra avy amin'ny serveur lavitra mankany amin'ny milina eo an-toerana.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Vahaolana (mandra-pahavitan'ny API SFTP):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Fampiharana mpampiasa

#### `ui.addSidebarButton(options)`

Manampy bokotra ao amin'ny sidebar WIA SOOM.

| Safidy | Karazana | Ilaina | Famaritana |
|--------|----------|--------|------------|
| `id` | `string` | Tsia | ID tokana (default ho an'ny anaran'ny plugin) |
| `icon` | `string` | Eny | Anaran'ny sary Lucide (ohatra, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Eny | Ny lahatsoratra bokotra aseho ao amin'ny sidebar |
| `onClick` | `() => void` | Eny | Fiasa antsoina rehefa tsindriana ny bokotra |
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
**Fanovàna sary:** Jereo ny sary rehetra misy ao amin'ny [lucide.dev/icons](https://lucide.dev/icons)

> **Fanamarihana mifanaraka:** Ny plugin taloha sasany dia mampiasa arguments misy toerana toy ny `addSidebarButton(id, icon, label, onClick)`. Ny API ofisialy dia mampiasa **options object** araka ny voafaritra etsy ambony. Aza adino ny mampiasa ny fomba object ho an'ny plugin vaovao.

#### `ui.openWebview(options)`

Mamorona varavarankely popup miaraka amin'ny votoaty HTML manokana. Ity no fomba hananganana UI manankarena.

| Safidy | Karazana | Famaritana |
|--------|----------|------------|
| `title` | `string` | Lohatenin'ny varavarankely |
| `html` | `string` | Votoaty HTML feno ho aseho |
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
> Jereo ny [Part 3](#part-3-building-custom-ui-with-webviews) ho an'ny lamina webview mandroso.

#### `ui.showNotification(type, message)`

Asehoy ny fanamarihana toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Fomba fanamarihana |
| `message` | `string` | Lahatsoratra aseho |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ampio zavatra lahatsoratra maharitra amin'ny bara fanjakana ambany.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID manokana ho an'ity zavatra fanjakana ity |
| `text` | `string` | Lahatsoratra aseho |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Fitehirizana maharitra

Ny toe-javatra plugin dia tehirizina mandrakizay ao amin'ny `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Vakio ny sanda voatahiry.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Mamerina `undefined` raha tsy misy ny lakile.

#### `settings.set(key, value)`

Tehirizo ny sanda. Manohana strings, isa, booleans, arrays, ary objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Ohatra: Tsarovy ny safidin'ny mpampiasa**
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

### `ctx.ai` — Fampidirana AI

> **Toe-javatra: Ho avy tsy ho ela** — Ny API AI dia voafaritra fa tsy mbola mifandray amin'ny Soomy. Amin'izao fotoana izao dia mamerina `{ response: 'AI not yet connected' }`. Ny fampidirana AI tanteraka dia kasaina ho an'ny famoahana ho avy.

#### `ai.chat(messages, options?)`

Mandefasa hafatra amin'ny mpanampy AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Mamorona UI manokana amin'ny Webviews

Ny API `openWebview()` dia ahafahanao mamorona UI dashboard miaraka amin'ny HTML, CSS, ary JavaScript — ao anatin'ny varavarankely popup.

> **Fetra manan-danja:** Ny webviews dia **fampisehoana fotsiny**. Tsy afaka miantso indray amin'ny API plugin izy ireo (`ctx.settings`, `ctx.terminal`, sns.). Ampiasao ny bokotra sidebar ho an'ny hetsika rehetra ataon'ny mpampiasa, ary ampiasao ny `openWebview()` hanehoana ny toe-javatra ankehitriny. Raha mila endri-javatra mifandray ianao, ampiasao ny bokotra sidebar hanombohana azy ireo ary avereno sokafana ny webview mba hanavaozana ny fampisehoana.

### Lamina: Baiko Terminal → Parse Output → Asehoy amin'ny HTML

Ity no lamina plugin mahazatra indrindra. Manatanteraka baiko ianao, mandinika ny vokatra, ary aseho amin'ny fomba hita maso.
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
### Lamina: Dashboard Mifandray miaraka amin'ny Auto-Refresh
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
### Lamina: Aseho ny Toe-javatra ao amin'ny Webview

> **Fanamarihana:** Ny webviews dia fampisehoana fotsiny — tsy afaka miantso indray amin'ny API plugin izy ireo. Ampiasao ny `ctx.settings` ao amin'ny mpanampy bokotra sidebar anao mba hanovana ny toe-javatra, ary ampiasao ny `openWebview()` hanehoana ny toe-javatra ankehitriny.
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

## Part 4: Mamoaka ny Plugin-nao

### Dingana 1: Andramo eo an-toerana

1. Kopia ny plugin-nao ao amin'ny `~/.wia-soom/plugins/{your-plugin}/`
2. Avereno ny WIA SOOM
3. Hamarino fa miasa: miseho ny bokotra sidebar, miasa tsara ny endri-javatra
4. Andramo ny tranga farany: inona no mitranga raha tsy misy terminal mifandray?

### Dingana 2: Miomàna ho an'ny fanaterana

Ny lahatahiry plugin-nao dia tsy maintsy ahitana:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Ilaina ny saha `package.json`:**

| Saha | Famaritana | Ohatra |
|-------|-------------|---------|
| `name` | ID tokana amin'ny endrika kebab-case | `"my-awesome-plugin"` |
| `version` | Dikan-teny semantika | `"1.0.0"` |
| `description` | Famaritana iray andalana | `"Monitors nginx access logs in real-time"` |
| `author` | Anaranao | `"John Doe"` |
| `main` | Loharano fidirana | `"index.js"` |

**Saha safidy:**

| Saha | Famaritana |
|-------|-------------|
| `license` | Karazana fahazoan-dàlana (MIT no asaina) |
| `keywords` | Lisitry ny teny fanalahidy |
| `soom.minVersion` | Dikan'ny WIA SOOM farafahakeliny ilaina |

### Dingana 3: Alefaso any amin'ny Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Ampio** ny plugin anao ao amin'ny `plugins/{anarana-plugin-anao}/`
3. **Alefaso** ny Pull Request

### Dingana 4: Fanombanana sy fanekena

Hijerena ny plugin tsirairay izahay ho an'ny:

- **Fiarovana** — tsy misy API mampidi-doza (jereo ny [Fitsipika Fiarovana](#security-rules))
- **Kalitao** — miasa ve? Madio ve ny kaody?
- **Fampiasana** — mamaha olana tena misy ve?

Aorian'ny fanekena:
1. Ampiana ao amin'ny `registry.json` ny plugin anao
2. Mamorona bundle ZIP ao amin'ny `dist/`
3. Hiseho ao amin'ny **Plugin Store** ho an'ny mpampiasa WIA SOOM rehetra ny plugin anao!

---

## Fizarana 5: Fomba Fanao Tsara

### Fitsipika Fiarovana

Ireo fitsipika ireo dia **tsy maintsy**. Ny plugins mandika azy ireo dia ho voarara.

| Fitsipika | Nahoana |
|------|-----|
| **Aza** mampiasa `eval()` na `new Function()` | Loza amin'ny fanampiana kaody |
| **Aza** mampiasa `child_process`, `exec()`, `spawn()` | Ampiasao fotsiny ny `ctx.terminal.send()` ho an'ny baiko |
| **Aza** maka URLs ivelany | Fandavana: `wiasoom.com` API endpoints |
| **Aza** miditra amin'ny `process.env` | Mety ahitana tsiambaratelo ny variables amin'ny tontolo iainana |
| **Aza** mampiasa `require('fs')` mivantana | Ampiasao ny `ctx.settings` ho an'ny fitahirizana, `ctx.sftp` ho an'ny famindrana rakitra |
| **Aza** mampiasa fonosana ivelany npm | JavaScript madio fotsiny — tsy misy node_modules |
| **Tsy maintsy** mampiasa `ctx.terminal.send()` ho an'ny baiko lavitra rehetra | Mandeha amin'ny fantsona SSH azo antoka izany |
| **Tsy maintsy** manadio ao amin'ny `deactivate()` | Esory ny mpihaino, diovy ny intervals |

### Fikarakarana Hadisoana

Aza adino ny manodidina ny asa mety ho risika amin'ny try/catch:
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
### Fanadiovana ao amin'ny deactivate()

Raha mamorona intervals, mpihaino, na fidirana ny plugin anao — diovy ireo:
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
### Fanohanana i18n

Manohana fiteny 254 ny WIA SOOM. Mba hahafahanao mandika ny marika plugin anao, ampiasao fomba tsotra:
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

## Fizarana 6: Ohatra Amin'ny Tena Fiainana

### Ohatra 1: Mpandinika Disk Server

Mampandeha `df -h` amin'ny server lavitra ary mampiseho ny habaka ampiasaina/azo ampiasaina ao amin'ny baran'ny sata.
§§§CHUNK_SEPARATOR§§§
---

### Ohatra 2: Mpitantana TODO

Plugin iray izay mitantana lisitry ny TODO mampiasa toe-javatra ho an'ny fitahirizana maharitra sy webview ho an'ny fisehoana.

> **Lamina famolavolana:** Satria tsy afaka miantso mivantana ny API plugin ny webviews, ity plugin ity dia mampiasa fomba "snapshot" — mamaky TODO avy amin'ny toe-javatra, mandrafitra azy ireo ho HTML tsy azo ovaina, ary manome hetsika mifototra amin'ny sidebar ho an'ny fanampiana zavatra. Ny webview dia sosona **fisehoana**, fa tsy endrika ifandraisana.
§§§CHUNK_SEPARATOR§§§
---

### Ohatra 3: Mpijery Hadisoana

Mandinika ny vokatra terminal ary mandefa fanamarihana rehefa mahita lamina manokana.
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

Ampiasao ireto ao amin'ny `package.json` `keywords` na rehefa mandefa any amin'ny registry:

| Category | Description |
|----------|-------------|
| `server` | Fitantanana ny server ankapobeny |
| `devtools` | Fitaovana fampandrosoana |
| `calculator` | Calculators sy converters |
| `simulator` | Simulators |
| `game` | Lalao terminal |
| `business` | Fitaovana ho an'ny raharaham-barotra |
| `security` | Fiarovana sy fanadihadiana |
| `web` | Fitantanana ny web server |
| `education` | Fitaovana fanabeazana |
| `health` | Fitaovana mifandray amin'ny fahasalamana |
| `islamic` | Fitaovana islamika (fotoana vavaka, sns.) |
| `science` | Fitaovana siantifika |
| `quantum` | Fitaovana computing quantum |
| `ai` | Fitaovana powered by AI |
| `biotech` | Fitaovana bioteknolojia |
| `space` | Fitaovana momba ny habakabaka sy ny astronomie |
| `network` | Fitaovana tambajotra |
| `database` | Fitantanana ny database |
| `monitoring` | Fanaraha-maso ny server |
| `devops` | DevOps sy CI/CD |
| `utility` | Fitaovana ankapobeny |
| `design` | Fitaovana famolavolana |
| `ecommerce` | Fitaovana e-commerce |
| `automation` | Fitaovana fanautomatisme |
| `kpop` | Fitaovana mifandray amin'ny K-pop |
| `accessibility` | Fitaovana ho an'ny fidirana |
| `analytics` | Analytics sy tatitra |
| `wia` | Fitaovana ekosistema WIA |
| `all` | Miseho amin'ny sokajy rehetra |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Fitantanana ny server |
| `shield` | Fiarovana |
| `database` | Database |
| `activity` | Fanaraha-maso |
| `terminal` | Fitaovana terminal |
| `code` | Fampandrosoana |
| `hard-drive` | Kapila/fitehirizana |
| `network` | Tambajotra |
| `lock` | Fanamarinana/Encryption |
| `eye` | Fijery/fanaraha-maso |
| `check-square` | Asa/TODO |
| `layout-dashboard` | Dashboard |
| `settings` | Fanamboarana |
| `zap` | Fanautomatisme |
| `globe` | Web/iraisam-pirenena |

Jereo ny sary 1,500+ rehetra: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Manamboara zavatra mahagaga. Zarao amin'izao tontolo izao.</em></p>
<p align="center"><em>— Ny Ekipa WIA SOOM</em></p>
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
