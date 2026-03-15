<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ghid ta' Sviluppatur tal-Plugin WIA SOOM</h1>
<p align="center"><strong>Ibni l-plugin tiegħek stess f'5 minuti.</strong></p>
<p align="center>Oħloq għodod qawwija tas-server, dashboards, u awtomazzjonijiet — ġewwa WIA SOOM.</p>

---

## Indiċi

- [Parti 1: Bidu Malajr — Il-Plugin Tiegħek L-ewwel F'5 Minuti](#parti-1-bidu-malajr--il-plugin-tiegħek-l-ewwel-f5-minuti)
- [Parti 2: Referenza tal-API tal-Kuntest tal-Plugin](#parti-2-referenza-tal-api-tal-kuntest-tal-plugin)
  - [ctx.terminal](#ctxterminal--eżegwixxi-kmandi-fis-servizzi-remoti)
  - [ctx.sftp](#ctxsftp--trasferiment-ta-fajls)
  - [ctx.ui](#ctxui--interfaċċa-tal-utent)
  - [ctx.settings](#ctxsettings--ħażna-persistenti)
  - [ctx.ai](#ctxai--integrazzjoni-ai)
- [Parti 3: Ibbini UI Custom bl-Webviews](#parti-3-ibbin-ui-custom-bl-webviews)
- [Parti 4: Pubblikazzjoni tal-Plugin Tiegħek](#parti-4-publikazzjoni-tal-plugin-tiegħek)
- [Parti 5: Aħjar Prattiċi](#parti-5-aħjar-prattiċi)
- [Parti 6: Eżempji Fil-Ħajja Veru](#parti-6-eżempji-fil-ħajja-veru)
- [Appendix: Kategoriji & Ikoni](#appendix-kategoriji--ikoni)

---

## Parti 1: Bidu Malajr — Il-Plugin Tiegħek L-ewwel F'5 Minuti

### X'tiġġenera

Plugin "Hello World" li żżid buttuna mal-sidebar. Meta tiġi kklikkjata, turi notifika.

### Pass 1: Oħloq il-folder tal-plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Pass 2: Oħloq package.json
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
**Fields meħtieġa:** `name`, `version`, `description`, `author`, `main`

### Pass 3: Oħloq index.js
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
### Pass 4: Irreġistra WIA SOOM

Irreġistra l-app (jew ibiddel il-plugin off/on fil-Ħtiġijiet → Plugins).

Għandek tara buttuna **"Hello World"** fil-sidebar. Ikklikkaha — tara notifika ta' suċċess!

### Kif jaħdem
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

## Parti 2: Referenza tal-API tal-Kuntest tal-Plugin

Meta tiġi kcalljata l-funzjoni `activate(context)`, `context` (jew `ctx`) jipprovdi dawn l-APIs:
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

### `ctx.terminal` — Eżegwixxi kmandi fis-servizzi remoti

#### `terminal.send(sessionId, data)`

Ibgħat kmand (jew kwalunkwe data) għal sessjoni attiva tal-terminal.

| Parametru | Tip | Deskrizzjoni |
|-----------|------|-------------|
| `sessionId` | `string` | Is-sessjoni tal-terminal li għandek tibgħat għaliha |
| `data` | `string` | Il-kmand jew id-data li għandek tibgħat |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abbonati għal kull output minn sessjoni tal-terminal. Iġib **funzjoni ta' disabbilitar**.

| Parametru | Tip | Deskrizzjoni |
|-----------|------|-------------|
| `sessionId` | `string` | Is-sessjoni tal-terminal li għandek tara |
| `callback` | `(data: string) => void` | Iċċempel ma' kull chunk ta' output |
| **Iġib** | `() => void` | Iċċempel dan biex tieqaf tisma' |
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
**Importanti:** Dejjem ħażna l-funzjoni ta' disabbilitar u iċċempelha fil-`deactivate()` biex tevita t-telf ta' memorja.

---

### `ctx.sftp` — Trasferiment ta' fajls

> **Stat: Ġej Dalwaqt** — L-API SFTP huwa definit iżda għadu mhux konness mal-magna SFTP tal-app. `list()` bħalissa iġib array vojt, u `upload()`/`download()` huma no-ops. Dan se jiġi implimentat b'mod sħiħ f'ħarġa futura. Għalissa, uża `ctx.terminal.send()` bil-kmandi `scp` jew `rsync` bħala workaround.

#### `sftp.list(sessionId, path)`

Listja ta' fajls f'direttorju remot.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Ibgħat fajl minn tagħmir lokali għal server remot.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Niżżel fajl minn server remot għal tagħmir lokali.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (sakemm l-API SFTP tkun attiva):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfaċċa tal-utent

#### `ui.addSidebarButton(options)`

Żid buttuna mal-sidebar tal-WIA SOOM.

| Għażla | Tip | Meħtieġ | Deskrizzjoni |
|--------|------|----------|-------------|
| `id` | `string` | Le | ID uniku (jiddefinixxi għall-isem tal-plugin) |
| `icon` | `string` | Iva | Isem tal-ikona Lucide (eż. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Iva | Test tal-buttuna murija fil-sidebar |
| `onClick` | `() => void` | Iva | Funzjoni li tiġi kcalljata meta l-buttuna tiġi kklikkjata |
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
**Referenza tal-ikona:** Iċċekkja l-ikoni kollha disponibbli fuq [lucide.dev/icons](https://lucide.dev/icons)

> **Nota ta' kompatibbiltà:** Xi plugins antiki jużaw argumenti pożizzjonali bħal `addSidebarButton(id, icon, label, onClick)`. L-API uffiċjali juża **oġġett ta' għażliet** kif dokumentat hawn fuq. Dejjem uża l-istil tal-oġġett għal plugins ġodda.

#### `ui.openWebview(options)`

Iftaħ window pop-up b'kontenut HTML personalizzat. Dan huwa kif tibni UIs rikka.

| Għażla | Tip | Deskrizzjoni |
|--------|------|-------------|
| `title` | `string` | Titolu tal-window |
| `html` | `string` | Kontenut HTML sħiħ li għandu jiġi renderjat |
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
> Ara [Part 3](#part-3-building-custom-ui-with-webviews) għal patterns avvanzati ta' webview.

#### `ui.showNotification(type, message)`

Uri notifika toast.

| Parametru | Tip | Deskrizzjoni |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stil tan-notifika |
| `message` | `string` | Test li juri |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Żid item ta' test persistenti mal-bar ta' status t'isfel.

| Parametru | Tip | Deskrizzjoni |
|-----------|------|-------------|
| `id` | `string` | ID uniku għal dan l-item ta' status |
| `text` | `string` | Test li juri |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Ħażna persistenti

Is-settings tal-plugin huma maħżuna b'mod permanenti f'`~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Aqra valur maħżun.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Irritorna `undefined` jekk il-key ma teżistix.

#### `settings.set(key, value)`

Ħażen valur. Jappoġġa strings, numri, booleans, arrays, u oġġetti.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Eżempju: Ftakar il-preferenzi tal-utent**
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

### `ctx.ai` — Integrazjoni AI

> **Status: Ġej Iżda** — L-API tal-AI huwa definit iżda għadu mhux konness ma' Soomy. Attwalment jirritorna `{ response: 'AI not yet connected' }`. Integrazjoni sħiħa tal-AI hija pjanata għal rilaxx futuri.

#### `ai.chat(messages, options?)`

Ibgħat messaġġi lill-assistent AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Bini ta' UI Personalizzat bl-Webviews

L-API `openWebview()` jippermettilek toħloq UIs ta' dashboard bl-HTML, CSS, u JavaScript — kollha ġewwa f'fenestri pop-up.

> **Limitazzjoni importanti:** Webviews huma **display-only**. Ma jistgħux jsejħu lura fl-APIs tal-plugin (`ctx.settings`, `ctx.terminal`, eċċ.). Uża buttuni tal-sidebar għal kull azzjoni tal-utent, u uża `openWebview()` biex turi l-istat attwali. Jekk għandek bżonn karatteristiċi interattivi, iġġibhom minn buttuni tal-sidebar u re-open il-webview biex tħarreġ id-dispjaċer.

### Pattern: Komand Terminal → Parse Output → Uri fl-HTML

Dan huwa l-aktar pattern komuni tal-plugin. Tħaddem komand, tippasta r-riżultat, u turih viżwalment.
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
### Pattern: Dashboard Interattiv bl-Auto-Refresh
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
### Pattern: Uri Settings f'Webview

> **Nota:** Webviews huma display-only — ma jistgħux jsejħu lura fl-APIs tal-plugin. Uża `ctx.settings` fil-manipulaturi tal-buttuni tal-sidebar tiegħek biex tibdel is-settings, u uża `openWebview()` biex turi l-istat attwali.
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

## Part 4: Pubblikazzjoni tal-Plugin Tiegħek

### Pass 1: Ittestja lokali

1. Ikteb il-plugin tiegħek f'`~/.wia-soom/plugins/{your-plugin}/`
2. Irreġistra WIA SOOM
3. Ikkonferma li jaħdem: buttuna tal-sidebar tidher, karatteristiċi jaħdmu b'mod korrett
4. Ittesta każijiet ta' limitu: x'jiġri jekk m'hemmx terminal konness?

### Pass 2: Ipprepara għall-sottomissjoni

Il-folder tal-plugin tiegħek għandu jkun fih:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Meħtieġ `package.json` fields:**

| Field | Deskrizzjoni | Eżempju |
|-------|-------------|---------|
| `name` | ID uniku fil-kebab-case | `"my-awesome-plugin"` |
| `version` | Versjoni semantika | `"1.0.0"` |
| `description` | Deskrizzjoni f'linja waħda | `"Monitors nginx access logs in real-time"` |
| `author` | Ismek | `"John Doe"` |
| `main` | Punt ta' dħul | `"index.js"` |

**Fields fakultattivi:**

| Field | Deskrizzjoni |
|-------|-------------|
| `license` | Tip ta' liċenzja (MIT rakkomandat) |
| `keywords` | Array ta' tags ta' tfittxija |
| `soom.minVersion` | Minimum WIA SOOM version meħtieġ |

### Pass 3: Ibgħat għall-Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Żid** il-plugin tiegħek f`plugins/{your-plugin-name}/`
3. **Ibgħat** Pull Request

### Pass 4: Reviżjoni u approvazzjoni

Aħna nrevijaw kull plugin għal:

- **Sigurtà** — ebda APIs perikolużi (ara [Regoli tas-Sigurtà](#security-rules))
- **Kwalità** — taħdem? Huwa l-kodiċi nadif?
- **Utilità** — tissolvi problema reali?

Wara l-approvazzjoni:
1. Il-plugin tiegħek jiġi miżjud f`registry.json`
2. Bundel ZIP jiġi maħluq f`dist/`
3. Il-plugin tiegħek jidher fil-**Plugin Store** għal kull utent ta' WIA SOOM!

---

## Parti 5: Aħjar Prattiċi

### Regoli tas-Sigurtà

Dawn ir-regoli huma **obbligatorji**. Plugins li jiksruhom se jiġu rrifjutati.

| Regola | Għaliex |
|------|-----|
| **QATT** uża `eval()` jew `new Function()` | Riskju ta' injezzjoni tal-kodiċi |
| **QATT** uża `child_process`, `exec()`, `spawn()` | Uża biss `ctx.terminal.send()` għal komandi |
| **QATT** ma tniżżilx URLs esterni | Eċċezzjoni: `wiasoom.com` API endpoints |
| **QATT** aċċessa `process.env` | Il-varjabbli tal-ambjent jistgħu jikkontinwaw sigri |
| **QATT** uża `require('fs')` direttament | Uża `ctx.settings` għall-ħażna, `ctx.sftp` għall-trasferiment tal-fajls |
| **GĦANDHOM** jużaw `ctx.terminal.send()` għal kull komanda remota | Dan jgħaddi permezz tal-kanal sigur SSH |
| **GĦANDHOM** jitnaddfu f`deactivate()` | Neħħi l-listeners, ċara l-intervals |

### Ġestjoni tal-Istejjer

Dejjem agħmel wrap l-operazzjonijiet riskjużi f'try/catch:
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
### Tindif f`deactivate()`

Jekk il-plugin tiegħek joħloq intervals, listeners, jew abbonamenti — tnaddafhom:
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
### Appoġġ i18n

WIA SOOM tappoġġa 254 lingwa. Biex tagħmel il-label tal-plugin tiegħek traduzzibbli, uża approċċ sempliċi:
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

## Parti 6: Eżempji Fil-Ħajja Reali

### Eżempju 1: Kontrollur tad-Disk tas-Server

Jħaddem `df -h` fuq is-server remota u juri l-ispazju użat/disponevoli fil-bar tal-istatus.
§§§CHUNK_SEPARATOR§§§
---

### Eżempju 2: Maniġer TODO

Plugin li jmexxi lista TODO bl-użu ta' settings għall-ħażna persistenti u webview għall-wiri.

> **Mudell ta' disinn:** Peress li l-webviews ma jistgħux jsejħu direttament APIs tal-plugin, dan il-plugin juża approċċ "snapshot" — jaqra l-TODOs mill-settings, jurihom bħala HTML li jista' jinqara biss, u jipprovdi azzjonijiet ibbażati fuq il-sidebar għall-agħdida ta' oġġetti. Il-webview huwa **layer** ta' wiri, mhux forma interattiva.
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

### Eżempju 3: Osservatur tal-Istejjer

Jmonitorja l-output tat-terminal u jibgħat notifika meta patterns speċifiċi jiġu identifikati.
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

## Appendix: Kategoriji & Ikoni

### Kategoriji tal-Plugins (29)

Uża dawnhom fil-`package.json` `keywords` jew meta tibgħat għall-registry:

| Kategorija | Deskrizzjoni |
|------------|--------------|
| `server` | Ġestjoni ġenerali tas-server |
| `devtools` | Għodod għall-iżvilupp |
| `calculator` | Kalkulaturi u konvertituri |
| `simulator` | Simulaturi |
| `game` | Logħob fit-terminal |
| `business` | Għodod tan-negozju |
| `security` | Sigurtà u awditjar |
| `web` | Ġestjoni tas-server tal-web |
| `education` | Għodod edukattivi |
| `health` | Għodod relatati mas-saħħa |
| `islamic` | Għodod Islamika (ħinijiet tal-liturgija, eċċ.) |
| `science` | Għodod xjentifiċi |
| `quantum` | Għodod għall-komputazzjoni kwantika |
| `ai` | Għodod b'ħila ta' AI |
| `biotech` | Għodod għall-bioteknoloġija |
| `space` | Għodod għall-ispazju u l-astronomija |
| `network` | Għodod tan-netwerk |
| `database` | Ġestjoni tad-database |
| `monitoring` | Monitoraġġ tas-server |
| `devops` | DevOps u CI/CD |
| `utility` | Utilitajiet ġenerali |
| `design` | Għodod għall-ippjanar |
| `ecommerce` | Għodod għall-e-commerce |
| `automation` | Għodod għall-awtomazzjoni |
| `kpop` | Għodod relatati ma' K-pop |
| `accessibility` | Għodod għall-aċċessibilità |
| `analytics` | Analiżi u rapporti |
| `wia` | Għodod għall-ekosistema WIA |
| `all` | Juri f'kull kategorija |

### Ikoni Rrakkomandati (Lucide)

| Isem tal-Ikon | Uża għal |
|----------------|---------|
| `server` | Ġestjoni tas-server |
| `shield` | Sigurtà |
| `database` | Database |
| `activity` | Monitoraġġ |
| `terminal` | Għodod fit-terminal |
| `code` | Żvilupp |
| `hard-drive` | Disk/ħażna |
| `network` | Networking |
| `lock` | Awtorizzazzjoni/kriptaġġ |
| `eye` | Osservazzjoni/monitoraġġ |
| `check-square` | Attivitajiet/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfigurazzjoni |
| `zap` | Awtomazzjoni |
| `globe` | Web/internazzjonali |

Iċċekkja l-1,500+ ikoni kollha: [lucide.dev/icons](https://lucide.dev/icons)

---

## Għandek Bżonn Għajnuna?

- **Problemi fuq GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemi tal-Plugins:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Eżempji tal-Plugins:** [Website](https://wiasoom.com)
- **Websajt:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Ibni xi ħaġa straordinarja. Aqsamha mal-dinja.</em></p>
<p align="center"><em>— It-Tim WIA SOOM</em></p>
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
