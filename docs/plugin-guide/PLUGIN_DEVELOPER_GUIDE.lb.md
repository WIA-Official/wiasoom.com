<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Entwéckler Guide</h1>
<p align="center"><strong>Maacht Ären eegene Plugin an 5 Minutten.</strong></p>
<p align="center">Schafft mächteg Server-Tools, Dashboards, an Automatisatiounen — direkt an WIA SOOM.</p>

---

## Inhalter

- [Deel 1: Schnellstart — Ären éischten Plugin an 5 Minutten](#deel-1-schnellstart--ären-éischten-plugin-an-5-minutten)
- [Deel 2: Plugin Kontext API Referenz](#deel-2-plugin-kontext-api-referenz)
  - [ctx.terminal](#ctxterminal--kommanden-op-remote-serveren-ausféieren)
  - [ctx.sftp](#ctxsftp--datei-Transfer)
  - [ctx.ui](#ctxui--benotzer-Interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integratioun)
- [Deel 3: Eegen UI mat Webviews bauen](#deel-3-eegen-ui-mat-webviews-bauen)
- [Deel 4: Ären Plugin verëffentlechen](#deel-4-ären-plugin-verëffentlechen)
- [Deel 5: Bescht Praktiken](#deel-5-bescht-praktiken)
- [Deel 6: Real-Welt Beispiller](#deel-6-real-welt-beispiller)
- [Appendix: Kategorien & Ikonen](#appendix-kategorien--ikonen)

---

## Deel 1: Schnellstart — Ären éischten Plugin an 5 Minutten

### Wat Dir wäert bauen

E "Hello World" Plugin dat e Knäppchen an der Sidebar derbäiset. Wann et geklickt gëtt, weist et eng Notifikatioun.

### Schrëtt 1: Erstellt de Plugin-Ordner
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Schrëtt 2: Erstellt package.json
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
**Felder déi gebraucht ginn:** `name`, `version`, `description`, `author`, `main`

### Schrëtt 3: Erstellt index.js
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
### Schrëtt 4: Start WIA SOOM nei

Start d'App nei (oder schalt de Plugin aus/a an de Settings → Plugins).

Dir sollt e **"Hello World"** Knäppchen an der Sidebar gesin. Klickt et — Dir wäert eng Erfolgsnotifikatioun gesin!

### Wéi et funktionéiert
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

## Deel 2: Plugin Kontext API Referenz

Wann Är `activate(context)` Funktioun opgeruff gëtt, stellt `context` (oder `ctx`) dës APIs zur Verfügung:
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

### `ctx.terminal` — Kommanden op remote Serveren ausféieren

#### `terminal.send(sessionId, data)`

Schéckt e Kommando (oder all Daten) un eng aktiv Terminal-Sitzung.

| Parameter | Typ | Beschreiwung |
|-----------|------|-------------|
| `sessionId` | `string` | D'Terminal-Sitzung déi geschéckt gëtt |
| `data` | `string` | D'Kommando oder Daten déi geschéckt ginn |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abonnéiert all Ausgab vun enger Terminal-Sitzung. Gëtt eng **unsubscriben Funktioun** zréck.

| Parameter | Typ | Beschreiwung |
|-----------|------|-------------|
| `sessionId` | `string` | D'Terminal-Sitzung déi iwwerwaacht gëtt |
| `callback` | `(data: string) => void` | Wird mat all Stéck Ausgab opgeruff |
| **Gëtt zréck** | `() => void` | Rifft dës un fir ze stoppen ze lauschteren |
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
**Wichteg:** Späichert ëmmer d'Unsubscribe-Funktioun an rufft se an `deactivate()` op fir Gedächtnisläckagen ze vermeiden.

---

### `ctx.sftp` — Datei Transfer

> **Status: Kommt geschwënn** — D'SFTP API ass definéiert, awer nach net mat der App's SFTP Engine verbonnen. `list()` gëtt aktuell e leere Array zréck, an `upload()`/`download()` sinn no-ops. Dëst wäert an enger zukünfteger Versioun voll implementéiert ginn. Fir elo, benotzt `ctx.terminal.send()` mat `scp` oder `rsync` Kommanden als Aarbechtsrumm.

#### `sftp.list(sessionId, path)`

Listet Dateien an engem remote Verzeechnes.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Lued eng Datei vun der lokaler Maschinn op den remote Server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Lued eng Datei vum remote Server op d'lokal Maschinn.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Aarbechtsrumm (bis d'SFTP API aktiv ass):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Benotzer Interface

#### `ui.addSidebarButton(options)`

Füügt e Knäppchen an der WIA SOOM Sidebar derbäi.

| Optioun | Typ | Noutwendeg | Beschreiwung |
|--------|------|----------|-------------|
| `id` | `string` | Nee | Eunique ID (standardméisseg den Plugin-Numm) |
| `icon` | `string` | Jo | Lucide Ikon Numm (z.B. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Jo | Knäppchen Text deen an der Sidebar gewisen gëtt |
| `onClick` | `() => void` | Jo | Funktioun déi opgeruff gëtt wann d'Knäppchen geklickt gëtt |
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
**Ikon Referenz:** Browse all verfügbar Ikonen op [lucide.dev/icons](https://lucide.dev/icons)

> **Kompatibilitéitsnotiz:** E puer méi al Plugins benotzen positional Argumenter wéi `addSidebarButton(id, icon, label, onClick)`. D'official API benotzt e **Optionsobjekt** wéi hei dokumentéiert. Benotzt ëmmer de Objektstil fir nei Plugins.

#### `ui.openWebview(options)`

Open e Popup-Fënster mat personaliséierte HTML-Inhalt. Dëst ass wéi Dir räich UIs baut.

| Optioun | Typ | Beschreiwung |
|--------|------|-------------|
| `title` | `string` | Fënster Titel |
| `html` | `string` | Voll HTML-Inhalt fir ze renderen |
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
> Kuckt [Teil 3](#part-3-building-custom-ui-with-webviews) fir fortgeschratt Webview-Muster.

#### `ui.showNotification(type, message)`

Weist eng Toast-Noriicht.

| Parameter | Typ | Beschreiwung |
|-----------|------|--------------|
| `type` | `'success' \| 'error' \| 'info'` | Noriicht-Stil |
| `message` | `string` | Text fir ze weisen |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Füügt e persistenten Textelement an der ënneschter Statusbar derbäi.

| Parameter | Typ | Beschreiwung |
|-----------|------|--------------|
| `id` | `string` | E unique ID fir dëst Statuselement |
| `text` | `string` | Text fir ze weisen |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Persistent Lagerung

Plugin-Einstellungen sinn permanent an `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` gespäichert.

#### `settings.get(key)`

Liest e gespäicherten Wäert.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Gëtt `undefined` zréck wann de Schlüssel net existéiert.

#### `settings.set(key, value)`

Späichert e Wäert. Ënnerstëtzt Strings, Zuelen, Booleans, Arrays, a Objeten.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Beispill: Erënnert un d'Benotzerpreferenzen**
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

### `ctx.ai` — AI Integratioun

> **Status: Kommt Geschwënn** — D'AI API ass definéiert, awer nach net mat Soomy verbonnen. Gëtt aktuell `{ response: 'AI not yet connected' }` zréck. Voll AI Integratioun ass fir eng zukünfteg Versioun geplangt.

#### `ai.chat(messages, options?)`

Schéckt Noriichten un den AI Assistent (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Deel 3: Eegen UI mat Webviews bauen

D'`openWebview()` API erlaabt Iech Dashboard UIs mat HTML, CSS, a JavaScript ze bauen — alles an engem Popup-Fënster.

> **Wichteg Einschränkung:** Webviews sinn **nëmmen fir d'Visibilitéit**. Si kënnen net zréck an d'Plugin APIs (`ctx.settings`, `ctx.terminal`, etc.) ruffen. Benotzt Sidebar-Buttons fir all Benotzeraktiounen, a benotzt `openWebview()` fir de aktuellen Zoustand ze weisen. Wann Dir interaktiv Funktiounen braucht, aktivéiert se vun Sidebar-Buttons a maacht d'Webview erëm op fir d'Visibilitéit ze aktualiséieren.

### Muster: Terminal Befehl → Parse Ausgab → Weisen an HTML

Dëst ass dat heefegst Plugin-Muster. Dir lauft e Befehl, parse de Resultat, a weist et visuell.
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
### Muster: Interaktiv Dashboard mat Auto-Refresh
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
### Muster: Weisen vun Einstellungen an engem Webview

> **Bemierkung:** Webviews sinn nëmmen fir d'Visibilitéit — si kënnen net zréck an d'Plugin APIs ruffen. Benotzt `ctx.settings` an Äre Sidebar-Button Handler fir d'Einstellungen ze änneren, a benotzt `openWebview()` fir de aktuellen Zoustand ze weisen.
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

## Deel 4: Äre Plugin verëffentlechen

### Schrëtt 1: Lokal testen

1. Koppéiert Äre Plugin an `~/.wia-soom/plugins/{your-plugin}/`
2. Start WIA SOOM nei
3. Vergewëssert Iech et funktionnéiert: Sidebar-Button erschéngt, Funktiounen funktionnéieren korrekt
4. Testt Randfäll: Wat geschitt wann keng Terminal verbannen ass?

### Schrëtt 2: Bereet Iech fir d'Ofginn

Äre Plugin-Ordner muss folgendes enthalen:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Required `package.json` fields:**

| Field | Beschreiwung | Beispill |
|-------|--------------|----------|
| `name` | Eenzegene Kebab-Case ID | `"my-awesome-plugin"` |
| `version` | Semantesch Versioun | `"1.0.0"` |
| `description` | Eenzegene Beschreiwung | `"Monitors nginx access logs in real-time"` |
| `author` | Ären Numm | `"John Doe"` |
| `main` | Entrée Punkt | `"index.js"` |

**Optional fields:**

| Field | Beschreiwung |
|-------|--------------|
| `license` | Lizenztyp (MIT empfohl) |
| `keywords` | Array vun Sich-Tags |
| `soom.minVersion` | Minimal WIA SOOM Versioun erfuerderlech |

### Schritt 3: An d'Plugin Registry ënnerstëtzen

1. ****Package** your plugin as a ZIP file
2. **Füügt** Ären Plugin zu `plugins/{your-plugin-name}/`
3. **Reecht** eng Pull Request

### Schritt 4: Iwwerpréifung an Genehmegung

Mir iwwerpréiwen all Plugin fir:

- **Sécherheet** — keng geféierlech APIs (seng [Sécherheetsreegelen](#security-rules))
- **Qualit��it** — Funktionnéiert et? Ass de Code propper?
- **Nëtzlechkeet** — Léisst et e richtegt Problem?

No Genehmegung:
1. Ären Plugin gëtt zu `registry.json` derbäigesat
2. Eng ZIP-Bundle gëtt an `dist/` erstallt
3. Ären Plugin erschéngt am **Plugin Store** fir all WIA SOOM Benotzer!

---

## Deel 5: Bescht Praktiken

### Sécherheetsreegelen

Dës Regelen sinn **verpflichtend**. Plugins déi se verletzen, ginn ofgelehnt.

| Regel | Firwat |
|-------|--------|
| **NEVER** benotzt `eval()` oder `new Function()` | Risiko vun Code-Injektioun |
| **NEVER** benotzt `child_process`, `exec()`, `spawn()` | Benotzt nëmmen `ctx.terminal.send()` fir Befehler |
| **NEVER** zitt extern URLs | Ausnam: `wiasoom.com` API Endpunkten |
| **NEVER** zougräifen op `process.env` | Ëmfeldvariabelen kënnen Geheimnisser enthalen |
| **NEVER** benotzt `require('fs')` direkt | Benotzt `ctx.settings` fir Lagerung, `ctx.sftp` fir Datei-Transfer |
| **NEVER** benotzt npm extern Pakete | Nëmme pur JavaScript — keng node_modules |
| **MUSS** benotzt `ctx.terminal.send()` fir all remote Befehler | Dëst geet duerch de sécheren SSH Kanal |
| **MUSS** opräumen an `deactivate()` | Entfernt Listener, läscht Intervallen |

### Feelerbehandlung

Wéi ëmmer risikoreich Operatiounen an try/catch ëmfaassen:
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
### Opräin an deactivate()

Wann Ären Plugin Intervallen, Listener oder Abonnements erstellt — räumt se op:
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
### i18n Ënnerstëtzung

WIA SOOM ënnerstëtzt 254 Sproochen. Fir Ären Plugin Label iwwersetzbar ze maachen, benotzt eng einfach Approche:
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

## Deel 6: Real-Welt Beispiller

### Beispill 1: Server Disk Checker

Féiert `df -h` op dem remote Server aus a weist benotzt/verfügbar Plaz an der Statusbar.
§§§CHUNK_SEPARATOR§§§
---

### Beispill 2: TODO Manager

E Plugin dat eng TODO-Lëscht verwaltet mat Einstellungen fir persistent Lagerung an engem Webview fir d'Anzeige.

> **Designmuster:** Well Webviews net direkt Plugin APIs uruffen kënnen, benotzt dësen Plugin eng "Snapshot" Approche — et liest TODOs aus de Einstellungen, render se als readonly HTML, an bitt sidebar-baséiert Aktiounen fir Elementer derbäizefügen. De Webview ass eng **Anzeige** Schicht, net eng interaktiv Form.
§§§CHUNK_SEPARATOR§§§
---

### Beispill 3: Feeler Watcher

Iwwerwaacht Terminal-Ausgabe an schéckt eng Notifikatioun wann spezifesch Musteren erkannt ginn.
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

## Appendix: Kategorien & Ikonen

### Plugin Kategorien (29)

Benotzt dës an Ärem `package.json` `keywords` oder wann Dir an der Registry ënnerbréngt:

| Kategorie | Beschreiwung |
|-----------|--------------|
| `server` | Allgemeng Serververwaltung |
| `devtools` | Entwécklungsinstrumenter |
| `calculator` | Rechner a Konverter |
| `simulator` | Simulatoren |
| `game` | Terminalspiller |
| `business` | Geschäftsinstrumenter |
| `security` | Sécherheet a Audit |
| `web` | Webserververwaltung |
| `education` | Educatiounsinstrumenter |
| `health` | Gesondheetsbezunnen Instrumenter |
| `islamic` | Islamesch Instrumenter (Gebetszäiten, etc.) |
| `science` | Wëssenschaftlech Instrumenter |
| `quantum` | Quantum Computing Instrumenter |
| `ai` | AI-gestéiert Instrumenter |
| `biotech` | Biotechnologie Instrumenter |
| `space` | Raum- a Astronomie Instrumenter |
| `network` | Netzwierkinstrumenter |
| `database` | Datebankverwaltung |
| `monitoring` | Servermonitoring |
| `devops` | DevOps a CI/CD |
| `utility` | Allgemeng Utilitéiten |
| `design` | Designinstrumenter |
| `ecommerce` | E-Commerce Instrumenter |
| `automation` | Automatisatiounsinstrumenter |
| `kpop` | K-pop bezunn Instrumenter |
| `accessibility` | Zougänglechkeetsinstrumenter |
| `analytics` | Analytik a Berichterstattung |
| `wia` | WIA Ökosystem Instrumenter |
| `all` | Erscheint an allen Kategorien |

### Empfohlene Ikonen (Lucide)

| Ikon Numm | Benotzt fir |
|------------|-------------|
| `server` | Serververwaltung |
| `shield` | Sécherheet |
| `database` | Datebank |
| `activity` | Monitoring |
| `terminal` | Terminalinstrumenter |
| `code` | Entwécklung |
| `hard-drive` | Disk/Speicher |
| `network` | Netzwierk |
| `lock` | Auth/Chiffreierung |
| `eye` | Beobachtung/Monitoring |
| `check-square` | Aufgaben/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfiguratioun |
| `zap` | Automatisatioun |
| `globe` | Web/international |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Braucht Hëllef?

- **GitHub Problemer:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Problemer:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Beispill Plugins:** [Website](https://wiasoom.com)
- **Websäit:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Maacht eppes erstaunlech. Deelt et mat der Welt.</em></p>
<p align="center"><em>— D'WIA SOOM Team</em></p>
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
