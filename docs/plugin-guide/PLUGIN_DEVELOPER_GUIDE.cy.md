<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Canllaw Datblygwr Plugin WIA SOOM</h1>
<p align="center"><strong>Creu eich plugin eich hun mewn 5 munud.</strong></p>
<p align="center>Creu offer gweinydd pwerus, dyfeisiau gwybodaeth, a awtomeiddiadau — yn union o fewn WIA SOOM.</p>

---

## Cynnwys

- [Rhan 1: Dechrau'n Gyflym — Eich Plugin Cyntaf mewn 5 Munud](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Rhan 2: Cyfeirnod API Cyd-destun Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Rhan 3: Adeiladu UI Custom gyda Webviews](#part-3-building-custom-ui-with-webviews)
- [Rhan 4: Cyhoeddi Eich Plugin](#part-4-publishing-your-plugin)
- [Rhan 5: Ymarferion Gorau](#part-5-best-practices)
- [Atodiad: Categori & Eiconau](#appendix-categories--icons)

---

## Rhan 1: Dechrau'n Gyflym — Eich Plugin Cyntaf mewn 5 Munud

### Beth fyddwch chi'n ei adeiladu

Plugin "Hello World" sy'n ychwanegu botwm i'r bar ochr. Pan fydd yn cael ei glicio, mae'n dangos hysbysiad.

### Cam 1: Creu'r ffolder plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Cam 2: Creu package.json
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
**Meysydd angenrheidiol:** `name`, `version`, `description`, `author`, `main`

### Cam 3: Creu index.js
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
### Cam 4: Ailgychwyn WIA SOOM

Ailgychwyn y cais (neu newid y plugin i ffwrdd/yn ôl yn y Gosodiadau → Plugins).

Dylech weld botwm **"Hello World"** yn y bar ochr. Cliciwch arno — byddwch yn gweld hysbysiad llwyddiant!

### Sut mae'n gweithio
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

## Rhan 2: Cyfeirnod API Cyd-destun Plugin

Pan fydd eich swyddogaeth `activate(context)` yn cael ei galw, mae `context` (neu `ctx`) yn darparu'r APIs hyn:
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

### `ctx.terminal` — Rhedeg gorchmynion ar weinyddion pell

#### `terminal.send(sessionId, data)`

Anfon gorchymyn (neu unrhyw ddata) i sesiwn derfynfa weithredol.

| Paramedr | Math | Disgrifiad |
|----------|------|------------|
| `sessionId` | `string` | Y sesiwn derfynfa i'w hanfon i |
| `data` | `string` | Y gorchymyn neu ddata i'w hanfon |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Cofrestrwch i bob allbwn o sesiwn derfynfa. Dychwelyd **swyddogaeth dad-gofrestru**.

| Paramedr | Math | Disgrifiad |
|----------|------|------------|
| `sessionId` | `string` | Y sesiwn derfynfa i'w gwylio |
| `callback` | `(data: string) => void` | Galw gyda phob darn o allbwn |
| **Dychwelyd** | `() => void` | Galwch hyn i stopio gwrando |
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
**Pwysig:** Cadwch bob amser y swyddogaeth dad-gofrestru a galwch hi yn `deactivate()` i atal colledion cof.

---

### `ctx.sftp` — Trosglwyddo ffeiliau

> **Statws: Yn Dod Yn Fuan** — Mae'r API SFTP wedi'i ddiffinio ond heb ei gysylltu â pheiriant SFTP y cais eto. Mae `list()` ar hyn o bryd yn dychwelyd array gwag, ac mae `upload()`/`download()` yn weithrediadau di-waith. Bydd hyn yn cael ei weithredu'n llwyr mewn rhyddhad yn y dyfodol. Ar hyn o bryd, defnyddiwch `ctx.terminal.send()` gyda gorchmynion `scp` neu `rsync` fel ffordd o osgoi.

#### `sftp.list(sessionId, path)`

Rhestru ffeiliau mewn cyfeiriad pell.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Uwchlwytho ffeil o'r peiriant lleol i'r gweinydd pell.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Lawrlwytho ffeil o'r gweinydd pell i'r peiriant lleol.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Osgoi (hyd nes bod API SFTP yn fyw):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Rhyngwyneb defnyddiwr

#### `ui.addSidebarButton(options)`

Ychwanegu botwm i bar ochr WIA SOOM.

| Opsiwn | Math | Angenrheidiol | Disgrifiad |
|--------|------|---------------|------------|
| `id` | `string` | Na | ID unigryw (yn dychwelyd i enw'r plugin) |
| `icon` | `string` | Ie | Enw eicon Lucide (e.e., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ie | Testun y botwm a ddangosir yn y bar ochr |
| `onClick` | `() => void` | Ie | Swyddogaeth a alwyd pan fydd y botwm yn cael ei glicio |
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
**Cyfeirnod eicon:** Porwch yr eiconau sydd ar gael yn [lucide.dev/icons](https://lucide.dev/icons)

> **Nodyn cydnawsedd:** Mae rhai plugins hŷn yn defnyddio dadleuon safle fel `addSidebarButton(id, icon, label, onClick)`. Mae'r API swyddogol yn defnyddio **objec opsiynau** fel y nodwyd uchod. Defnyddiwch bob amser y steil gwrthrych ar gyfer plugins newydd.

#### `ui.openWebview(options)`

Agor ffenestr pop-up gyda chynnwys HTML penodol. Dyma sut i adeiladu UIs cyfoethog.

| Opsiwn | Math | Disgrifiad |
|--------|------|------------|
| `title` | `string` | Teitl y ffenestr |
| `html` | `string` | Cynnwys HTML llawn i'w rendro |
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
> Gweler [Rhan 3](#part-3-building-custom-ui-with-webviews) am batrymau gwe-gweledol uwch.

#### `ui.showNotification(type, message)`

Dangos hysbysiad tost.

| Paramedr | Math | Disgrifiad |
|----------|------|------------|
| `type` | `'success' \| 'error' \| 'info'` | Arddull hysbysiad |
| `message` | `string` | Testun i'w ddangos |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ychwanegu eitem destun parhaus i'r bar statws gwaelod.

| Paramedr | Math | Disgrifiad |
|----------|------|------------|
| `id` | `string` | ID unigryw ar gyfer yr eitem statws hon |
| `text` | `string` | Testun i'w ddangos |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Storio parhaus

Mae gosodiadau'r plugin yn cael eu storio'n barhaol yn `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Darllen gwerth a gedwir.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Mae'n dychwelyd `undefined` os nad yw'r allwedd yn bodoli.

#### `settings.set(key, value)`

Cadw gwerth. Mae'n cefnogi stringiau, rhifau, booleans, rhestrau, a gwrthrychau.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Enghraifft: Cofiwch ddewislen y defnyddiwr**
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

### `ctx.ai` — Integreiddio AI

> **Statws: Yn Dod yn Fuan** — Mae'r API AI wedi'i ddiffinio ond heb ei gysylltu â Soomy eto. Ar hyn o bryd, mae'n dychwelyd `{ response: 'AI not yet connected' }`. Mae integreiddio llawn AI yn cael ei chynllunio ar gyfer rhyddhad yn y dyfodol.

#### `ai.chat(messages, options?)`

Anfon negeseuon at y cymorth AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Rhan 3: Adeiladu UI Custom gyda Gwe-gweledol

Mae'r API `openWebview()` yn eich galluogi i adeiladu UIau darllediadau gyda HTML, CSS, a JavaScript — i gyd o fewn ffenestr pop-up.

> **Cyfyngiad pwysig:** Mae gwe-gweledol yn **dangos dim ond**. Ni allant alw yn ôl i APIs plugin (`ctx.settings`, `ctx.terminal`, ac ati). Defnyddiwch botymau ochr ar gyfer pob gweithred defnyddiwr, a defnyddiwch `openWebview()` i ddangos y cyflwr cyfredol. Os oes angen nodweddion rhyngweithiol arnoch, cychwynwch nhw o botymau ochr a ail-agorwch y gwe-gweledol i adnewyddu'r ddangosfa.

### Patrymau: Gorchymyn Terminal → Dadansoddi Allbwn → Dangos yn HTML

Dyma'r patrwm plugin mwyaf cyffredin. Rydych yn rhedeg gorchymyn, yn dadansoddi'r canlyniad, a'i ddangos yn weledol.
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
### Patrymau: Darlledfa Rhyngweithiol gyda Ail-gynhyrchu
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
### Patrymau: Dangos Gosodiadau mewn Gwe-gweledol

> **Nodyn:** Mae gwe-gweledol yn dangos dim ond — ni allant alw yn ôl i APIs plugin. Defnyddiwch `ctx.settings` yn eich rheolwyr botwm ochr i newid gosodiadau, a defnyddiwch `openWebview()` i ddangos y cyflwr cyfredol.
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

## Rhan 4: Cyhoeddi Eich Plugin

### Cam 1: Prawf yn lleol

1. Copi eich plugin i `~/.wia-soom/plugins/{your-plugin}/`
2. Ailgychwyn WIA SOOM
3. Gwirio ei fod yn gweithio: ymddangos botwm ochr, mae nodweddion yn gweithio'n gywir
4. Prawf achosion ymylol: beth sy'n digwydd os nad yw terminal wedi'i gysylltu?

### Cam 2: Paratoi ar gyfer cyflwyno

Mae'n rhaid i'ch ffolder plugin gynnwys:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Meintiau `package.json` sydd eu hangen:**

| Maes | Disgrifiad | Enghraifft |
|------|------------|------------|
| `name` | ID unig yn kebab-case | `"my-awesome-plugin"` |
| `version` | Fersiwn semantig | `"1.0.0"` |
| `description` | Disgrifiad un llinell | `"Monitors nginx access logs in real-time"` |
| `author` | Eich enw | `"John Doe"` |
| `main` | Pwynt mynediad | `"index.js"` |

**Meintiau dewisol:**

| Maes | Disgrifiad |
|------|------------|
| `license` | Math o drwydded (MIT argymhellir) |
| `keywords` | Cyfres o dagiau chwilio |
| `soom.minVersion` | Fersiwn isaf WIA SOOM sydd ei hangen |

### Cam 3: Cyflwyno i'r Gofrestr Plugin

1. ****Package** your plugin as a ZIP file
2. **Ychwanegu** eich plugin i `plugins/{your-plugin-name}/`
3. **Cyflwyno** Cais Tynnu

### Cam 4: Adolygu a chymeradwyo

Rydym yn adolygu pob plugin am:

- **Diogelwch** — dim APIau peryglus (gweler [Reolau Diogelwch](#security-rules))
- **Ansawdd** — a yw'n gweithio? A yw'r cod yn glân?
- **Defnyddioldeb** — a yw'n datrys problem go iawn?

Ar ôl cymeradwyo:
1. Mae eich plugin yn cael ei ychwanegu i `registry.json`
2. Mae pecyn ZIP yn cael ei greu yn `dist/`
3. Mae eich plugin yn ymddangos yn y **Plugin Store** ar gyfer pob defnyddiwr WIA SOOM!

---

## Rhan 5: Ymarferion Gorau

### Reolau Diogelwch

Mae'r rheolau hyn yn **orfodol**. Bydd plugins sy'n eu torri yn cael eu gwrthod.

| Rheol | Pam |
|-------|-----|
| **PEIDIWCH** byth â defnyddio `eval()` nac `new Function()` | Risg mewnosod cod |
| **PEIDIWCH** byth â defnyddio `child_process`, `exec()`, `spawn()` | Defnyddiwch yn unig `ctx.terminal.send()` ar gyfer gorchmynion |
| **PEIDIWCH** byth â chael URLau allanol | Eithriad: pwyntiau API `wiasoom.com` |
| **PEIDIWCH** byth â mynediad i `process.env` | Gall newidynnau amgylchedd gynnwys cyfrinachau |
| **PEIDIWCH** byth â defnyddio `require('fs')` yn uniongyrchol | Defnyddiwch `ctx.settings` ar gyfer storfa, `ctx.sftp` ar gyfer trosglwyddo ffeiliau |
| **PEIDIWCH** byth â defnyddio pecynnau allanol npm | Dim ond JavaScript pur — dim node_modules |
| **MAE** angen defnyddio `ctx.terminal.send()` ar gyfer pob gorchymyn pell | Mae hyn yn mynd trwy'r sianel SSH ddiogel |
| **MAE** angen glanhau yn `deactivate()` | Tynnwch wrandawyr, clirwch gyfnodau |

### Rheoli Gwallau

Bob amser, lapio gweithrediadau risg yn try/catch:
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
### Glanhau yn deactivate()

Os yw eich plugin yn creu cyfnodau, wrandawyr, neu aelodaeth — glanhewch nhw:
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
### Cefnogaeth i i18n

Mae WIA SOOM yn cefnogi 254 iaith. I wneud label eich plugin yn drosiadwy, defnyddiwch ddull syml:
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

## Rhan 6: Enghreifftiau o'r Byd Go Iawn

### Enghraifft 1: Gwirfoddolwr Disg y Gweinydd

Mae'n rhedeg `df -h` ar y gweinydd pell a dangosir y gofod a ddefnyddir/ar gael yn y bar statws.
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

### Enghraifft 2: Rheolwr TODO

Plugin sy'n rheoli rhestr TODO gan ddefnyddio gosodiadau ar gyfer storfa barhaol a gwe-golwg ar gyfer arddangos.

> **Patrwm dylunio:** Gan nad yw gwe-golygfeydd yn gallu galw APIau plugin yn uniongyrchol, mae'r plugin hwn yn defnyddio dull "snapshot" — mae'n darllen TODOs o'r gosodiadau, yn eu harddangos fel HTML darllenadwy yn unig, ac yn darparu gweithredoedd seiliedig ar ochr ar gyfer ychwanegu eitemau. Mae'r gwe-golwg yn haen **arddangos**, nid ffurflen ryngweithiol.
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

### Enghraifft 3: Gwirfoddolwr Gwall

Mae'n monitro allbwn y terminal ac yn anfon hysbysiad pan fydd patrymau penodol yn cael eu canfod.
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

## Atodiad: Categoriadau & Eiconau

### Categoriadau Plugin (29)

Defnyddiwch y rhain yn eich `package.json` `keywords` neu wrth gyflwyno i'r gofrestr:

| Categori | Disgrifiad |
|----------|-------------|
| `server` | Rheoli gweinydd cyffredinol |
| `devtools` | Offer datblygu |
| `calculator` | Cyfrifiaduron a thrawswyr |
| `simulator` | Symulwyr |
| `game` | Gemau terminal |
| `business` | Offer busnes |
| `security` | Diogelwch a phrofiad |
| `web` | Rheoli gweinydd gwe |
| `education` | Offer addysgol |
| `health` | Offer sy'n gysylltiedig â iechyd |
| `islamic` | Offer Islamaidd (amserau gweddïo, ac ati) |
| `science` | Offer gwyddonol |
| `quantum` | Offer cyfrifiadura cwantwm |
| `ai` | Offer sy'n seiliedig ar AI |
| `biotech` | Offer biotechnoleg |
| `space` | Offer gofod a sêr |
| `network` | Offer rhwydwaith |
| `database` | Rheoli cronfeydd data |
| `monitoring` | Monitro gweinydd |
| `devops` | DevOps a CI/CD |
| `utility` | Defnyddiau cyffredinol |
| `design` | Offer dylunio |
| `ecommerce` | Offer e-fasnach |
| `automation` | Offer awtomatiaeth |
| `kpop` | Offer sy'n gysylltiedig â K-pop |
| `accessibility` | Offer hygyrchedd |
| `analytics` | Dadansoddi a adrodd |
| `wia` | Offer ecosystem WIA |
| `all` | Ymddangos yn yr holl gategorïau |

### Eiconau Argymelledig (Lucide)

| Enw Eicon | Defnyddiwch ar gyfer |
|-----------|---------|
| `server` | Rheoli gweinydd |
| `shield` | Diogelwch |
| `database` | Cronfa ddata |
| `activity` | Monitro |
| `terminal` | Offer terminal |
| `code` | Datblygu |
| `hard-drive` | Disg/storfa |
| `network` | Rhwydweithio |
| `lock` | Awdurdodi/encodi |
| `eye` | Gwyliadwriaeth/monitro |
| `check-square` | Tasgau/TODO |
| `layout-dashboard` | Dangosfeydd |
| `settings` | Ffurfweddiad |
| `zap` | Awtomatiaeth |
| `globe` | Gwe/rhyngwladol |

Browsewch yr holl eiconau 1,500+: [lucide.dev/icons](https://lucide.dev/icons)

---

## Angen Cymorth?

- **Materion GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Materion Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Enghreifftiau Plugin:** [Website](https://wiasoom.com)
- **Gwefan:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Creu rhywbeth anhygoel. Rhannwch ef gyda'r byd.</em></p>
<p align="center"><em>— Tîm WIA SOOM</em></p>