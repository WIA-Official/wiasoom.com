<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Udhëzues për Zhvilluesit e Plugin-eve WIA SOOM</h1>
<p align="center"><strong>Krijoni plugin-in tuaj në 5 minuta.</strong></p>
<p align="center">Krijoni mjete të fuqishme serveri, tabela kontrolli dhe automatizime — direkt brenda WIA SOOM.</p>

---

## Tabela e Përmbajtjes

- [Pjesa 1: Fillimi i Shpejtë — Plugin-i Juaj i Parë në 5 Minuta](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Pjesa 2: Referenca e API-së së Kontekstit të Plugin-it](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Pjesa 3: Ndërtimi i UI të Personalizuar me Webviews](#part-3-building-custom-ui-with-webviews)
- [Pjesa 4: Publikimi i Plugin-it Tuaj](#part-4-publishing-your-plugin)
- [Pjesa 5: Praktikat më të Mira](#part-5-best-practices)
- [Pjesa 6: Shembuj nga Bota Reale](#part-6-real-world-examples)
- [Shtojca: Kategoritë & Ikonat](#appendix-categories--icons)

---

## Pjesa 1: Fillimi i Shpejtë — Plugin-i Juaj i Parë në 5 Minuta

### Çfarë do të krijoni

Një plugin "Hello World" që shton një buton në anën e majtë. Kur klikoni, shfaqet një njoftim.

### Hapi 1: Krijoni dosjen e plugin-it
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Hapi 2: Krijoni package.json
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
**Fushat e kërkuara:** `name`, `version`, `description`, `author`, `main`

### Hapi 3: Krijoni index.js
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
### Hapi 4: Rinisni WIA SOOM

Rinisni aplikacionin (ose aktivizoni/çaktivizoni plugin-in në Cilësimet → Plugin-e).

Duhet të shihni një buton **"Hello World"** në anën e majtë. Klikoni mbi të — do të shihni një njoftim suksesi!

### Si funksionon
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

## Pjesa 2: Referenca e API-së së Kontekstit të Plugin-it

Kur funksioni juaj `activate(context)` thirret, `context` (ose `ctx`) ofron këto API:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Ekzekutoni komanda në servera të largët

#### `terminal.send(sessionId, data)`

Dërgoni një komandë (ose çdo të dhënë) në një sesion aktiv terminali.

| Parametri | Tipi | Përshkrimi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesioni i terminalit për të cilin dërgoni |
| `data` | `string` | Komanda ose të dhënat për t'u dërguar |
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

Abonohuni në të gjithë daljet nga një sesion terminali. Kthehet një **funksion për të anuluar abonimin**.

| Parametri | Tipi | Përshkrimi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesioni i terminalit për të cilin po shikoni |
| `callback` | `(data: string) => void` | Thirret me çdo copë daljeje |
| **Kthen** | `() => void` | Thirreni këtë për të ndaluar dëgjimin |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**E rëndësishme:** Ruani gjithmonë funksionin për të anuluar abonimin dhe thirreni atë në `deactivate()` për të parandaluar rrjedhjet e memories.

---

### `ctx.sftp` — Transferimi i skedarëve

> **Statusi: Po Vjen** — API-ja SFTP është e definuar, por ende nuk është lidhur me motorin SFTP të aplikacionit. `list()` aktualisht kthen një array të zbrazët, dhe `upload()`/`download()` janë pa veprim. Kjo do të implementohet plotësisht në një version të ardhshëm. Për tani, përdorni `ctx.terminal.send()` me komandat `scp` ose `rsync` si një zgjidhje alternative.

#### `sftp.list(sessionId, path)`

Listoni skedarët në një direktor të largët.
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

Ngarko një skedar nga makina lokale në serverin e largët.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

Shkarko një skedar nga serveri i largët në makinën lokale.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**Zgjidhje alternative (deri sa API-ja SFTP të jetë aktive):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — Ndërfaqja e përdoruesit

#### `ui.addSidebarButton(options)`

Shtoni një buton në anën e majtë të WIA SOOM.

| Opsioni | Tipi | E Kërkuar | Përshkrimi |
|---------|------|-----------|-------------|
| `id` | `string` | Jo | ID unike (default është emri i plugin-it) |
| `icon` | `string` | Po | Emri i ikonës Lucide (p.sh., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Po | Teksti i butonit që shfaqet në anën e majtë |
| `onClick` | `() => void` | Po | Funksioni që thirret kur butoni klikohet |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Referenca e ikonave:** Shikoni të gjitha ikonat e disponueshme në [lucide.dev/icons](https://lucide.dev/icons)

> **Shënim për kompatibilitetin:** Disa plugin-e më të vjetra përdorin argumente pozicionale si `addSidebarButton(id, icon, label, onClick)`. API-ja zyrtare përdor një **objekt opsionesh** siç është dokumentuar më sipër. Përdorni gjithmonë stilin e objektit për plugin-et e reja.

#### `ui.openWebview(options)`

Hapni një dritare pop-up me përmbajtje HTML të personalizuar. Kjo është mënyra se si ndërtoni UI të pasura.

| Opsioni | Tipi | Përshkrimi |
|---------|------|-------------|
| `title` | `string` | Titulli i dritares |
| `html` | `string` | Përmbajtja e plotë HTML për t'u renderuar |
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
> Shihni [Pjesa 3](#part-3-building-custom-ui-with-webviews) për modelet e avancuara të webview.

#### `ui.showNotification(type, message)`

Shfaq një njoftim toast.

| Parametri | Lloji | Përshkrimi |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stili i njoftimit |
| `message` | `string` | Teksti për të shfaqur |
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

Shto një element tekstual të përhershëm në barin e statusit në fund.

| Parametri | Lloji | Përshkrimi |
|-----------|------|-------------|
| `id` | `string` | ID unike për këtë element statusi |
| `text` | `string` | Teksti për të shfaqur |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Ruajtja e qëndrueshme

Cilësimet e plugin-it ruhen përherë në `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lexoni një vlerë të ruajtur.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Kthen `undefined` nëse çelësi nuk ekziston.

#### `settings.set(key, value)`

Ruani një vlerë. Mbështet vargje, numra, boolean, tabela dhe objekte.
��§§CHUNK_SEPARATOR§§§
**Shembull: Kujtoni preferencat e përdoruesit**
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### `ctx.ai` — Integrimi i AI

> **Statusi: Po Vjen** — API i AI është i definuar, por ende nuk është i lidhur me Soomy. Aktualisht kthen `{ response: 'AI not yet connected' }`. Integrimi i plotë i AI është planifikuar për një lëshim të ardhshëm.

#### `ai.chat(messages, options?)`

Dërgoni mesazhe në asistentin AI (Soomy).
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Pjesa 3: Ndërtimi i UI të Personalizuar me Webviews

API `openWebview()` ju lejon të ndërtoni UI të panelit me HTML, CSS, dhe JavaScript — të gjitha brenda një dritareje pop-up.

> **Kufizim i rëndësishëm:** Webviews janë **vetëm për shfaqje**. Ato nuk mund të thërrasin API-të e plugin-it (`ctx.settings`, `ctx.terminal`, etj.). Përdorni butonat e anës për të gjitha veprimet e përdoruesve dhe përdorni `openWebview()` për të shfaqur gjendjen aktuale. Nëse keni nevojë për funksionalitete interaktive, aktivizoni ato nga butonat e anës dhe rihapni webview për të rifreskuar shfaqjen.

### Modeli: Komanda e Terminalit → Analizo Daljen → Shfaq në HTML

Ky është modeli më i zakonshëm i plugin-it. Ju ekzekutoni një komandë, analizoni rezultatin dhe e shfaqni vizualisht.
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
### Modeli: Dashboard Interaktiv me Auto-Rifreskim
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Modeli: Shfaqja e Cilësimeve në një Webview

> **Shënim:** Webviews janë vetëm për shfaqje — ato nuk mund të thërrasin API-të e plugin-it. Përdorni `ctx.settings` në menaxherët e butonave tuaj të anës për të modifikuar cilësimet, dhe përdorni `openWebview()` për të shfaqur gjendjen aktuale.
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

## Pjesa 4: Publikimi i Plugin-it Tuaj

### Hapi 1: Testoni lokal

1. Kopjoni plugin-in tuaj në `~/.wia-soom/plugins/{your-plugin}/`
2. Rinisni WIA SOOM
3. Verifikoni nëse funksionon: butoni i anës shfaqet, funksionalitetet punojnë siç duhet
4. Testoni rastet ekstreme: çfarë ndodh nëse nuk është i lidhur asnjë terminal?

### Hapi 2: Përgatituni për dorëzim

Folderi juaj i plugin-it duhet të përmbajë:
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
**Fushat e kërkuara `package.json`:**

| Fusha | Përshkrimi | Shembulli |
|-------|-------------|---------|
| `name` | ID unik në formatin kebab-case | `"my-awesome-plugin"` |
| `version` | Versioni semantik | `"1.0.0"` |
| `description` | Përshkrim në një rresht | `"Monitors nginx access logs in real-time"` |
| `author` | Emri juaj | `"John Doe"` |
| `main` | Pika e hyrjes | `"index.js"` |

**Fushat opsionale:**

| Fusha | Përshkrimi |
|-------|-------------|
| `license` | Lloji i licencës (MIT e rekomanduar) |
| `keywords` | Array e etiketave për kërkim |
| `soom.minVersion` | Versioni minimal i WIA SOOM i kërkuar |

### Hapi 3: Dërgo në Regjistrin e Plugin-eve

1. ****Package** your plugin as a ZIP file
2. **Shto** plugin-in tuaj në `plugins/{emri-i-plugin-it-tuaj}/`
3. **Dërgo** një Pull Request

### Hapi 4: Rishikimi dhe miratimi

Ne rishikojmë çdo plugin për:

- **Sigurinë** — asnjë API e rrezikshme (shihni [Rregullat e Sigurisë](#security-rules))
- **Cilësinë** — a funksionon? A është kodi i pastër?
- **Përdorshmërinë** — a zgjidh një problem real?

Pas miratimit:
1. Plugin-i juaj shtohet në `registry.json`
2. Një paketë ZIP krijohet në `dist/`
3. Plugin-i juaj shfaqet në **Plugin Store** për të gjithë përdoruesit e WIA SOOM!

---

## Pjesa 5: Praktikat më të Mira

### Rregullat e Sigurisë

Këto rregulla janë **të detyrueshme**. Plugin-et që i shkelin ato do të refuzohen.

| Rregulli | Pse |
|------|-----|
| **KURR** mos përdorni `eval()` ose `new Function()` | Rreziku i injektimit të kodit |
| **KURR** mos përdorni `child_process`, `exec()`, `spawn()` | Përdorni vetëm `ctx.terminal.send()` për komandat |
| **KURR** mos merrni URL të jashtme | Përjashtim: piketat API të `wiasoom.com` |
| **KURR** mos aksesoni `process.env` | Variablat e ambientit mund të përmbajnë sekrete |
| **KURR** mos përdorni `require('fs')` direkt | Përdorni `ctx.settings` për ruajtje, `ctx.sftp` për transferimin e skedarëve |
| **KURR** mos përdorni paketa të jashtme npm | Vetëm JavaScript i pastër — pa node_modules |
| **DUHET** të përdorni `ctx.terminal.send()` për të gjitha komandat e largëta | Kjo kalon përmes kanalit të sigurt SSH |
| **DUHET** të pastroni në `deactivate()` | Hiqni dëgjuesit, pastroni intervalet |

### Menaxhimi i Gabimeve

Gjithmonë mbështillni operacionet me rrezik në try/catch:
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
### Pastrimi në deactivate()

Nëse plugin-i juaj krijon intervale, dëgjues, ose abonime — pastroni ato:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Mbështetje i18n

WIA SOOM mbështet 254 gjuhë. Për ta bërë etiketën e plugin-it tuaj të përkthyeshme, përdorni një qasje të thjeshtë:
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

## Pjesa 6: Shembuj nga Bota Reale

### Shembulli 1: Kontrolluesi i Diskut të Serverit

Ekzekuton `df -h` në serverin e largët dhe tregon hapësirën e përdorur/të disponueshme në shiritin e statusit.
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

### Shembulli 2: Menaxheri i TODO-ve

Një plugin që menaxhon një listë TODO duke përdorur cilësimet për ruajtje të qëndrueshme dhe një webview për shfaqje.

> **Modeli i dizajnit:** Duke qenë se webview-t nuk mund të thërrasin drejtpërdrejt API-të e plugin-it, ky plugin përdor një qasje "snapshot" — lexon TODO-t nga cilësimet, i paraqet ato si HTML të lexueshëm, dhe ofron veprime të bazuara në anën për të shtuar elemente. Webview është një **shtresë** shfaqjeje, jo një formë interaktive.
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

### Shembulli 3: Vëzhguesi i Gabimeve

Monitoron daljen e terminalit dhe dërgon një njoftim kur zbulohen modele specifike.
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

## Shtojca: Kategoritë & Ikonat

### Kategoritë e Plugin-eve (29)

Përdorni këto në `package.json` `keywords` ose kur dërgoni në regjistër:

| Kategoria | Përshkrimi |
|-----------|------------|
| `server` | Menaxhimi i përgjithshëm i serverëve |
| `devtools` | Veglat e zhvillimit |
| `calculator` | Kalkulatorët dhe konvertuesit |
| `simulator` | Simuluesit |
| `game` | Lojërat në terminal |
| `business` | Veglat e biznesit |
| `security` | Siguria dhe auditimi |
| `web` | Menaxhimi i serverëve web |
| `education` | Veglat edukative |
| `health` | Veglat e shëndetit |
| `islamic` | Veglat islame (koha e lutjeve, etj.) |
| `science` | Veglat shkencore |
| `quantum` | Veglat e kompjuterëve kuantik |
| `ai` | Veglat e fuqizuara nga AI |
| `biotech` | Veglat e bioteknologjisë |
| `space` | Veglat për hapësirën dhe astronominë |
| `network` | Veglat e rrjetit |
| `database` | Menaxhimi i bazave të të dhënave |
| `monitoring` | Monitorimi i serverëve |
| `devops` | DevOps dhe CI/CD |
| `utility` | Veglat e përgjithshme |
| `design` | Veglat e dizajnit |
| `ecommerce` | Veglat e tregtisë elektronike |
| `automation` | Veglat e automatizimit |
| `kpop` | Veglat e lidhura me K-pop |
| `accessibility` | Veglat e aksesueshmërisë |
| `analytics` | Analizat dhe raportimi |
| `wia` | Veglat e ekosistemit WIA |
| `all` | Shfaqet në të gjitha kategoritë |

### Ikonat e rekomanduara (Lucide)

| Emri i Ikonës | Përdor për |
|----------------|------------|
| `server` | Menaxhimi i serverëve |
| `shield` | Siguria |
| `database` | Baza e të dhënave |
| `activity` | Monitorimi |
| `terminal` | Veglat e terminalit |
| `code` | Zhvillimi |
| `hard-drive` | Disku/ruajtja |
| `network` | Rrjetëzimi |
| `lock` | Autentikimi/enkriptimi |
| `eye` | Vëzhgimi/monitorimi |
| `check-square` | Detyrat/TODO |
| `layout-dashboard` | Paneli i kontrollit |
| `settings` | Konfigurimi |
| `zap` | Automatizimi |
| `globe` | Web/internacional |

Shikoni të gjitha 1,500+ ikona: [lucide.dev/icons](https://lucide.dev/icons)

---

## Keni Nevojë për Ndihmë?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Shembuj Plugin-esh:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Krijoni diçka të mahnitshme. Ndani atë me botën.</em></p>
<p align="center"><em>— Ekipi WIA SOOM</em></p>
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
