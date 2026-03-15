<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Treoir Forbróirí Plugin WIA SOOM</h1>
<p align="center"><strong>Cruthaigh do phlugin féin i 5 nóiméad.</strong></p>
<p align="center">Cruthaigh uirlisí freastalaí cumhachtacha, painéil, agus uathoibrithe — díreach laistigh de WIA SOOM.</p>

---

## Tábla Ábhar

- [Cuid 1: Tús gasta — Do Phplugin Ar dtús i 5 Nóiméad](#cuid-1-tús-gasta--do-phplugin-ar-dtús-i-5-nóiméad)
- [Cuid 2: Tagairt API Conradh Plugin](#cuid-2-tagairt-api-conradh-plugin)
  - [ctx.terminal](#ctxterminal--rith-orduithe-ar-freastalaithe-iargúlta)
  - [ctx.sftp](#ctxsftp--tarchur-comhad)
  - [ctx.ui](#ctxui--comhoibriú-úsáideora)
  - [ctx.settings](#ctxsettings--stóráil-seasta)
  - [ctx.ai](#ctxai--integraíocht-ai)
- [Cuid 3: Tógáil UI Saincheaptha le Webviews](#cuid-3-tógáil-ui-saincheaptha-le-webviews)
- [Cuid 4: Foilsigh Do Phplugin](#cuid-4-foilsigh-do-phplugin)
- [Cuid 5: Cleachtais is Fearr](#cuid-5-cleachtais-is-fearr)
- [Leabhrán: Catagóirí & Deilbhíní](#leabhrán-catagóirí--deilbhíní)

---

## Cuid 1: Tús gasta — Do Phplugin Ar dtús i 5 Nóiméad

### Cad a thógfaidh tú

Plugin "Hello World" a chuirfidh cnaipe leis an taobhbar. Nuair a chliceáiltear é, taispeánann sé fógra.

### Céim 1: Cruthaigh an fillteán plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Céim 2: Cruthaigh package.json
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
**Réimsí riachtanacha:** `name`, `version`, `description`, `author`, `main`

### Céim 3: Cruthaigh index.js
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
### Céim 4: Athrestart WIA SOOM

Athrestart an aip (nó cas an plugin as/air i Socruithe → Plugins).

Ba chóir duit cnaipe **"Hello World"** a fheiceáil sa taobhbar. Cliceáil air — feicfidh tú fógra rathúil!

### Conas a oibríonn sé
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

## Cuid 2: Tagairt API Conradh Plugin

Nuair a gcalltar do fheidhm `activate(context)`, soláthraíonn `context` (nó `ctx`) na APIs seo:
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

### `ctx.terminal` — Rith orduithe ar fhreastalaithe iargúlta

#### `terminal.send(sessionId, data)`

Seol ordú (nó aon shonraí) chuig seisiún teirminéil gníomhach.

| Paraiméadar | Cineál | Cur Síos |
|-------------|--------|----------|
| `sessionId` | `string` | An seisiún teirminéil le seoladh |
| `data` | `string` | An t-ordú nó na sonraí le seoladh |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Cláraigh do gach aschur ó sheisiún teirminéil. Tuairisceann sé **fheidhm díliostála**.

| Paraiméadar | Cineál | Cur Síos |
|-------------|--------|----------|
| `sessionId` | `string` | An seisiún teirminéil le faire |
| `callback` | `(data: string) => void` | Gcalltar le gach paca aschuir |
| **Tuairisceann** | `() => void` | Gcall an seo chun éisteacht a stopadh |
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
**Tábhachtach:** Sábháil an fheidhm díliostála i gcónaí agus gcall é i `deactivate()` chun leáite cuimhne a chosc.

---

### `ctx.sftp` — Tarchur comhad

> **Stádas: Ag Teacht Go Luath** — Tá an API SFTP sainmhínithe ach níl sé ceangailte leis an inneall SFTP an aip go fóill. Taispeánann `list()` faoi láthair eochair folamh, agus tá `upload()`/`download()` neamh-oibríoch. Beidh sé curtha i bhfeidhm go hiomlán i leagan amach amach anseo. Ar an gcéad dul síos, bain úsáid as `ctx.terminal.send()` le horduithe `scp` nó `rsync` mar réiteach sealadach.

#### `sftp.list(sessionId, path)`

Liostaigh comhoibrithe i gcatagóir iargúlta.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Uaslódáil comhad ó mheaisín áitiúil chuig freastalaí iargúlta.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Íoslódáil comhad ó fhreastalaí iargúlta chuig meaisín áitiúil.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Réiteach sealadach (go dtí go mbeidh an API SFTP beo):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Comhoibriú úsáideora

#### `ui.addSidebarButton(options)`

Cuir cnaipe leis an taobhbar WIA SOOM.

| Rogha | Cineál | Riachtanach | Cur Síos |
|-------|--------|-------------|----------|
| `id` | `string` | Níl | ID uathúil (tá sé réamhshocraithe go dtí ainm an plugin) |
| `icon` | `string` | Tá | Ainm deilbhín Lucide (m.sh., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Tá | Téacs an chnaipe a thaispeántar sa taobhbar |
| `onClick` | `() => void` | Tá | Feidhm a gcalltar nuair a chliceáiltear an cnaipe |
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
**Tagairt deilbhín:** Brabhsáil na deilbhíní ar fáil ag [lucide.dev/icons](https://lucide.dev/icons)

> **Nóta comhoiriúnachta:** Úsáideann roinnt plugins níos sine argóintí suíomh cosúil le `addSidebarButton(id, icon, label, onClick)`. Úsáideann an API oifigiúil **obair roghanna** mar a dhoiciméadaíodh thuas. Úsáid an stíl obair i gcónaí do phlugins nua.

#### `ui.openWebview(options)`

Oscail fuinneog pop-up le hábhar HTML saincheaptha. Seo é conas a thógann tú UIs saibhir.

| Rogha | Cineál | Cur Síos |
|-------|--------|----------|
| `title` | `string` | Teideal na fuinneoige |
| `html` | `string` | Ábhar HTML iomlán le taispeáint |
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
> Féach [Cuid 3](#part-3-building-custom-ui-with-webviews) le haghaidh patrún webview ardleibhéil.

#### `ui.showNotification(type, message)`

Taispeáin fógra toast.

| Paraiméadar | Cineál | Cur síos |
|-------------|--------|----------|
| `type` | `'success' \| 'error' \| 'info'` | Stíl fógra |
| `message` | `string` | Téacs le taispeáint |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Cuir mír téacs buan leis an mbarr stádais íochtair.

| Paraiméadar | Cineál | Cur síos |
|-------------|--------|----------|
| `id` | `string` | ID uathúil don mhír stádais seo |
| `text` | `string` | Téacs le taispeáint |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Stóráil buan

Socruithe an plugin a stóráiltear go buan i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Léigh luach sábháilte.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Tugann sé `undefined` má tá an eochair ann.

#### `settings.set(key, value)`

Sábháil luach. Tacaíonn sé le sreangáin, uimhreacha, booleans, ailt, agus oblecta.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Sampla: Cuimhnigh ar roghanna úsáideora**
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

### `ctx.ai` — Comhoiriúnacht AI

> **Stádas: Ag Teacht Go Luath** — Tá an API AI sainmhínithe ach níl sé nasctha le Soomy go fóill. Taispeánann sé faoi láthair `{ response: 'AI not yet connected' }`. Tá comhoiriúnacht AI iomlán pleanáilte do scaoileadh sa todhchaí.

#### `ai.chat(messages, options?)`

Seol teachtaireachtaí chuig an gcuideachta AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Cuid 3: Tógáil UI Saincheaptha le Webviews

Ligeann an API `openWebview()` duit UIanna painéil a thógáil le HTML, CSS, agus JavaScript — go léir laistigh de fhuinneog pop-up.

> **Teorainn thábhachtach:** Is **taispeántas amháin** atá i webviews. Ní féidir leo glaoch ar APIs an plugin (`ctx.settings`, `ctx.terminal`, etc.). Úsáid na cnaipí taobh le haghaidh gach gníomh úsáideora, agus bain úsáid as `openWebview()` chun an staid reatha a thaispeáint. Má tá gnéithe idirghníomhacha uait, gníomhachtaigh iad ó na cnaipí taobh agus athoscail an webview chun an taispeáint a nuashonrú.

### Patrún: Ordú Téarma → Parse Toradh → Taispeáin i HTML

Is é seo an patrún plugin is coitianta. Rithfidh tú ordú, parse an toradh, agus taispeánfaidh tú é go radharcach.
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
### Patrún: Painéal Idirghníomhach le Nuashonrú Uathoibríoch
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
### Patrún: Taispeáint Socruithe i Webview

> **Nóta:** Is taispeántas amháin atá i webviews — ní féidir leo glaoch ar APIs an plugin. Úsáid `ctx.settings` i do láimhseálaithe cnaipí taobh chun socruithe a mhodhnú, agus bain úsáid as `openWebview()` chun an staid reatha a thaispeáint.
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

## Cuid 4: Foilsiú do Plugin

### Céim 1: Tástáil go háitiúil

1. Cóipeáil do plugin go `~/.wia-soom/plugins/{your-plugin}/`
2. Aththosnaigh WIA SOOM
3. Déan cinnte go n-oibríonn sé: tá cnaipí taobh le feiceáil, oibríonn na gnéithe go ceart
4. Tástáil cásanna imeall: cad a tharlaíonn má tá aon téarma nach bhfuil ceangailte?

### Céim 2: Ullmhúchán le haghaidh comhoiriúnachta

Caithfidh do fhóram plugin a bheith ina choinne:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Gnéithe riachtanacha `package.json`:**

| Gné | Cur síos | Sampla |
|-------|-------------|---------|
| `name` | Aitheantas uathúil i stíl kebab-case | `"my-awesome-plugin"` |
| `version` | Leagan seimeanta | `"1.0.0"` |
| `description` | Cur síos ar aon líne | `"Monitors nginx access logs in real-time"` |
| `author` | Do ainm | `"John Doe"` |
| `main` | Pointe iontrála | `"index.js"` |

**Gnéithe roghnach:**

| Gné | Cur síos |
|-------|-------------|
| `license` | Cineál ceadúnas (MIT molta) |
| `keywords` | Aibíocht de thagairtí cuardaigh |
| `soom.minVersion` | An leagan íosta WIA SOOM atá riachtanach |

### Céim 3: Cuir isteach chuig an gClár Plugin

1. ****Package** your plugin as a ZIP file
2. **Cuir** do phlugin isteach i `plugins/{your-plugin-name}/`
3. **Cuir isteach** iarratas Pull

### Céim 4: Athbhreithniú agus ceadú

Déanaimid athbhreithniú ar gach plugin maidir le:

- **Slándáil** — gan APIanna contúirteacha (féach [Rialacha Slándála](#security-rules))
- **Cáilíocht** — an oibríonn sé? An bhfuil an cód glan?
- **Úsáideacht** — an réitíonn sé fadhb fhíor?

Tar éis ceadú:
1. Cuirtear do phlugin isteach i `registry.json`
2. Cruthaítear pacáiste ZIP i `dist/`
3. Taispeántar do phlugin sa **Store Plugin** do gach úsáideoir WIA SOOM!

---

## Conradh 5: Cleachtais is Fearr

### Rialacha Slándála

Tá na rialacha seo **duillín**. Cuirfear as an áireamh na plugins a sháraíonn iad.

| Rial | Cén fáth |
|------|-----|
| **NÍL** úsáid `eval()` nó `new Function()` | Riosca ionchuir cód |
| **NÍL** úsáid `child_process`, `exec()`, `spawn()` | Úsáid `ctx.terminal.send()` amháin do na horduithe |
| **NÍL** faigh URLanna seachtracha | Eisceacht: `wiasoom.com` pointe API |
| **NÍL** rochtain a fháil ar `process.env` | D'fhéadfadh athróg timpeallachta rúin a bheith ann |
| **NÍL** úsáid `require('fs')` go díreach | Úsáid `ctx.settings` le haghaidh stórála, `ctx.sftp` le haghaidh aistriú comhad |
| **NÍL** úsáid pacáistí seachtracha npm | JavaScript glan amháin — gan node_modules |
| **CAITHfidh** úsáid `ctx.terminal.send()` do gach ordú iargúlta | Téann sé tríd an gcainéal SSH slán |
| **CAITHfidh** glanadh a dhéanamh i `deactivate()` | Bain na gcloigín��, glan na hidirghníomhachtaí |

### Láimhseáil Earráide

Bí i gcónaí ag cur oibríochtaí rioscaigh i wrap try/catch:
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
### Glanadh i deactivate()

Má chruthaíonn do phlugin idirghníomhachtaí, clónna, nó síntiúis — glan iad:
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
### Tacaíocht i18n

Tacaíonn WIA SOOM le 254 teanga. Chun do lipéad plugin a dhéanamh inaistrithe, bain úsáid as cur chuige simplí:
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

## Conradh 6: Samplaí Saol Réalaíoch

### Sampla 1: Seiceálaí Diosca Freastala

Ritheann `df -h` ar an freastala iargúlta agus taispeánann sé spás úsáidte/ar fáil sa bharra stádais.
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

### Sampla 2: Bainisteoir TODO

Pluigin a bhainistíonn liosta TODO ag baint úsáide as socruithe le haghaidh stórála buan agus webview le haghaidh taispeántais.

> **Patrún dearaidh:** Ós rud é nach féidir le webviews glaoch go díreach ar APIanna plugin, úsáideann an pluigin seo cur chuige "snapshot" — léann sé TODOanna ó shocruithe, déanann sé iad a chur i láthair mar HTML nach féidir a chur in iúl, agus soláthraíonn sé gníomhartha bunaithe ar thaobh le haghaidh earraí a chur leis. Is sraith **taispeána** é an webview, ní foirm idirghníomhach.
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

### Sampla 3: Faire Earráide

Monatóireacht ar aschur an téarmaínéil agus seolann sé foláireamh nuair a bhronntar patrúin shonracha.
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
| `server` | Bainistíocht ginearálta ar an freastalaí |
| `devtools` | Uirlisí forbartha |
| `calculator` | Conraitheoirí agus comhoibrithe |
| `simulator` | Simulóirí |
| `game` | Cluichí téarma |
| `business` | Uirlisí gnó |
| `security` | Slándáil agus iniúchadh |
| `web` | Bainistíocht freastalaí gréasáin |
| `education` | Uirlisí oideachais |
| `health` | Uirlisí a bhaineann le sláinte |
| `islamic` | Uirlisí Ioslamacha (amanna guí, srl.) |
| `science` | Uirlisí eolaíochta |
| `quantum` | Uirlisí ríomhaireachta cainníochtúla |
| `ai` | Uirlisí le cumhacht AI |
| `biotech` | Uirlisí bitheolaíochta |
| `space` | Uirlisí spáis agus réalteolaíochta |
| `network` | Uirlisí líonra |
| `database` | Bainistíocht bunachair sonraí |
| `monitoring` | Monatóireacht ar fhreastalaí |
| `devops` | DevOps agus CI/CD |
| `utility` | Uirlisí ginearálta |
| `design` | Uirlisí dearaidh |
| `ecommerce` | Uirlisí le haghaidh eobair |
| `automation` | Uirlisí uathoibrithe |
| `kpop` | Uirlisí a bhaineann le K-pop |
| `accessibility` | Uirlisí inrochtana |
| `analytics` | Anailís agus tuairisciú |
| `wia` | Uirlisí éiceachórais WIA |
| `all` | Feictear i ngach catagóir |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Bainistíocht freastalaí |
| `shield` | Slándáil |
| `database` | Bunachar sonraí |
| `activity` | Monatóireacht |
| `terminal` | Uirlisí téarma |
| `code` | Forbairt |
| `hard-drive` | Diosca/stóráil |
| `network` | Líonraíocht |
| `lock` | Údar/cryptiú |
| `eye` | Ag faire/monatóireacht |
| `check-square` | Tascanna/TODO |
| `layout-dashboard` | Painéil |
| `settings` | Confighuration |
| `zap` | Uathoibriú |
| `globe` | Gréasán/idirnáisiúnta |

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