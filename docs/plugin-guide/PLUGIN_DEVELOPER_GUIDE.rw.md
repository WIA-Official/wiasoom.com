<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Umuyoboro w'Abakora Plugins</h1>
<p align="center"><strong>Shyiramo plugin yawe mu minota 5.</strong></p>
<p align="center">Kora ibikoresho bikomeye bya seriveri, dashboards, na automations — imbere ya WIA SOOM.</p>

---

## Urutonde rw'Ibikubiye

- [Igice 1: Gutangira vuba — Plugin yawe ya Mbere mu Minota 5](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Igice 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Igice 3: Kubaka UI Yihariye na Webviews](#part-3-building-custom-ui-with-webviews)
- [Igice 4: Gutangaza Plugin Yawe](#part-4-publishing-your-plugin)
- [Igice 5: Imyitwarire Myiza](#part-5-best-practices)
- [Igice 6: Ingero z'Ukuri](#part-6-real-world-examples)
- [Inyongera: Amoko & Ibimenyetso](#appendix-categories--icons)

---

## Igice 1: Gutangira vuba — Plugin yawe ya Mbere mu Minota 5

### Icyo uzubaka

Plugin ya "Hello World" izongeraho buto ku ruhande. Iyo ikanditswe, igaragaza itangazo.

### Intambwe ya 1: Shyiramo dosiye ya plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Intambwe ya 2: Shyiramo package.json
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
**Ibikenewe:** `name`, `version`, `description`, `author`, `main`

### Intambwe ya 3: Shyiramo index.js
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
### Intambwe ya 4: Subiramo WIA SOOM

Subiramo porogaramu (cyangwa uhindure plugin mu buryo bwa ON/OFF muri Settings → Plugins).

Ugomba kubona buto ya **"Hello World"** ku ruhande. Kanda kuri yo — uzabona itangazo ry'intsinzi!

### Uko bikora
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

## Igice 2: Plugin Context API Reference

Iyo `activate(context)` yawe ikoze, `context` (cyangwa `ctx`) itanga izi APIs:
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

### `ctx.terminal` — Kora amabwiriza ku maseriveri y'ahantu hatandukanye

#### `terminal.send(sessionId, data)`

Ohereza itegeko (cyangwa ibindi bintu) ku iseshoni ya terminal ikora.

| Parameter | Ubwoko | Ibisobanuro |
|-----------|--------|-------------|
| `sessionId` | `string` | Isezerano rya terminal ugomba koherezaho |
| `data` | `string` | Itegeko cyangwa ibindi bintu byo kohereza |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Iyandikishe ku musaruro wose uva mu iseshoni ya terminal. Igarura **function yo kutiyandikisha**.

| Parameter | Ubwoko | Ibisobanuro |
|-----------|--------|-------------|
| `sessionId` | `string` | Isezerano rya terminal ugomba gukurikirana |
| `callback` | `(data: string) => void` | Ihamagarwa buri gihe habonetse igice cy'umusaruro |
| **Igarura** | `() => void` | Hamagara ibi kugira ngo uhagarike kumva |
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
**Icyitonderwa:** Buri gihe bika function yo kutiyandikisha kandi uyihamagarire muri `deactivate()` kugira ngo wirinde kumara umwanya.

---

### `ctx.sftp` — Kohereza amafayili

> **Imiterere: Igihe Kizaza** — SFTP API irateganijwe ariko ntabwo irakora ku mashini ya porogaramu. `list()` ubu igarura urutonde rw'ubusa, na `upload()`/`download()` ntacyo bikora. Ibi bizashyirwa mu bikorwa mu release itaha. Ubu, ukoreshe `ctx.terminal.send()` hamwe n'amabwiriza ya `scp` cyangwa `rsync` nk'igihunga.

#### `sftp.list(sessionId, path)`

Urutonde rw'amafayili mu bubiko bwo hanze.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Ohereza ifayili iva ku mashini y'aho mu bubiko bwo hanze.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Kura ifayili iva mu bubiko bwo hanze igana ku mashini y'aho.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Igihunga (kugira ngo SFTP API ikore):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Imiterere y'Umukoresha

#### `ui.addSidebarButton(options)`

Ongeraho buto ku ruhande rwa WIA SOOM.

| Icyemezo | Ubwoko | Ibisabwa | Ibisobanuro |
|----------|--------|----------|-------------|
| `id` | `string` | Oya | ID yihariye (ihinduka izina rya plugin) |
| `icon` | `string` | Yego | Izina ry'ikimenyetso cya Lucide (nka, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yego | Ijambo ry'ubuto rigaragara ku ruhande |
| `onClick` | `() => void` | Yego | Function ihamagarwa iyo buto ikanditswe |
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
**Icyerekezo cy'ibimenyetso:** Reba ibimenyetso byose bihari kuri [lucide.dev/icons](https://lucide.dev/icons)

> **Icyitonderwa ku bijyanye n'ubushobozi:** Bamwe mu plugins za kera bakoresha ibipimo by'ahantu nka `addSidebarButton(id, icon, label, onClick)`. API yemewe ikoresha **options object** nk'uko byanditse hejuru. Buri gihe ukoreshe uburyo bw'ikintu ku mishinga mishya.

#### `ui.openWebview(options)`

Fungura idirishya rishya rifite ibikubiye mu HTML byihariye. Ubu ni bwo buryo bwo kubaka UIs zifite ubukire.

| Icyemezo | Ubwoko | Ibisobanuro |
|----------|--------|-------------|
| `title` | `string` | Umutwe w'idirishya |
| `html` | `string` | Ibikubiye byose bya HTML bigomba kwerekanwa |
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
> Reba [Igice cya 3](#part-3-building-custom-ui-with-webviews) ku buryo buhanitse bwa webview.

#### `ui.showNotification(type, message)`

Erekana itangazo rya toast.

| Parameter | Type | Ibisobanuro |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Imiterere y'itangazo |
| `message` | `string` | Ijambo ryo kwerekana |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ongeramo ikintu cyanditse gihoraho ku murongo w'ibumoso.

| Parameter | Type | Ibisobanuro |
|-----------|------|-------------|
| `id` | `string` | ID yihariye kuri iki kintu cy'ibumoso |
| `text` | `string` | Ijambo ryo kwerekana |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Ububiko buhoraho

Ibyo guhindura plugin bibikwa burundu muri `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Soma agaciro kabitswe.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Igarura `undefined` niba urufunguzo rutaboneka.

#### `settings.set(key, value)`

Bika agaciro. Ibyemera imirongo, imibare, boolean, arrays, n'ibintu.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Urugero: Kwibuka ibyifuzo by'umukoresha**
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

### `ctx.ai` — Guhuza AI

> **Imiterere: Iza Vuba** — API ya AI irateganijwe ariko ntirahuza na Soomy. Ubu igarura `{ response: 'AI not yet connected' }`. Guhuza AI byuzuye birateganijwe mu release izaza.

#### `ai.chat(messages, options?)`

Ohereza ubutumwa ku murwanashyaka wa AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Igice cya 3: Kubaka UI yihariye ukoresheje Webviews

API ya `openWebview()` igufasha kubaka UI za dashboard ukoresheje HTML, CSS, na JavaScript — byose mu idirishya ry'ipopup.

> **Icyitonderwa:** Webviews ni **gukora gusa**. Ntabwo bashobora guhamagara APIs za plugin (`ctx.settings`, `ctx.terminal`, n'ibindi). Koresha buto z'ibumoso ku bikorwa byose by'abakoresha, kandi ukoreshe `openWebview()` kugirango ugaragaze uko ibintu bimeze. Niba ukeneye imikorere yihariye, shyira mu bikorwa ku buto z'ibumoso hanyuma usubiremo webview kugirango uvugurure igaragaza.

### Imiterere: Itangazo rya Terminal → Gusoma Ibisubizo → Kwerekana muri HTML

Iyi ni imiterere isanzwe ya plugin. Ukoresha itegeko, usoma igisubizo, hanyuma ukakwerekana mu buryo bugaragara.
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
### Imiterere: Dashboard yihariye ifite Auto-Refresh
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
### Imiterere: Kwerekana Ibyihutirwa muri Webview

> **Icyitonderwa:** Webviews ni gukorera gusa — ntibashobora guhamagara APIs za plugin. Koresha `ctx.settings` mu mirimo yawe y'ibumoso kugirango uhindure ibyihutirwa, kandi ukoreshe `openWebview()` kugirango ugaragaze uko ibintu bimeze.
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

## Igice cya 4: Kwamamaza Plugin yawe

### Intambwe ya 1: Gerageza mu buryo bw'imbere

1. Kopi plugin yawe muri `~/.wia-soom/plugins/{your-plugin}/`
2. Subukura WIA SOOM
3. Reba niba ikora: buto y'ibumoso igaragara, imikorere ikora neza
4. Gerageza ibibazo byihariye: ni iki kiba niba nta terminal ihujwe?

### Intambwe ya 2: Tegura ku itangwa

Agasanduku ka plugin yawe kagomba kuba karimo:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Ibisabwa `package.json` imirongo:**

| Imirongo | Ibisobanuro | Urugero |
|----------|-------------|---------|
| `name` | ID idasubirwaho mu buryo bwa kebab-case | `"my-awesome-plugin"` |
| `version` | Igenzura ry'ibisobanuro | `"1.0.0"` |
| `description` | Ibisobanuro mu murongo umwe | `"Monitors nginx access logs in real-time"` |
| `author` | Izina ryawe | `"John Doe"` |
| `main` | Inzira y'ibanze | `"index.js"` |

**Imirongo itari ngombwa:**

| Imirongo | Ibisobanuro |
|----------|-------------|
| `license` | Ubwoko bwa lisansi (MIT niyo ishyirwa imbere) |
| `keywords` | Urutonde rw'ibimenyetso byo gushakisha |
| `soom.minVersion` | Igihe ntarengwa cya WIA SOOM gikeneye |

### Igikorwa cya 3: Ohereza mu Ikarita ya Plugin

1. ****Package** your plugin as a ZIP file
2. **Ongeramo** plugin yawe muri `plugins/{izina-rya-plugin-yawe}/`
3. **Ohereza** Pull Request

### Igikorwa cya 4: Isuzuma no Kwemeza

Dusuzuma buri plugin ku:

- **Umutekano** — nta APIs zishobora kuba zifite ingaruka (reba [Amategeko y'Umutekano](#security-rules))
- **Ubwiza** — ese ikora? Ese kode irakeye?
- **Gufasha** — ese ikemura ikibazo nyakuri?

Nyuma yo kwemezwa:
1. Plugin yawe yongerwamo muri `registry.json`
2. Ikarita ya ZIP ikorwa muri `dist/`
3. Plugin yawe igaragara muri **Plugin Store** ku bakoresha bose ba WIA SOOM!

---

## Igice cya 5: Imyitwarire Myiza

### Amategeko y'Umutekano

Aya mategeko ni **ibisabwa**. Plugins zinyuranyije nayo zizakurwa.

| Itegeko | Impamvu |
|---------|---------|
| **NTUGAKORESHE** `eval()` cyangwa `new Function()` | Icyago cyo kwinjiza kode |
| **NTUGAKORESHE** `child_process`, `exec()`, `spawn()` | Koresha gusa `ctx.terminal.send()` ku mabwiriza |
| **NTUGAKORESHE** gufata URLs z'inyuma | Icyitonderwa: `wiasoom.com` API endpoints |
| **NTUGAKORESHE** kugera kuri `process.env` | Imiterere y'ibidukikije ishobora kuba irimo ibanga |
| **NTUGAKORESHE** `require('fs')` by'umwihariko | Koresha `ctx.settings` ku kubika, `ctx.sftp` ku kwimura dosiye |
| **UGOMBA** gukoresha `ctx.terminal.send()` ku mabwiriza yose yo kure | Ibi bigenda binyuze mu nzira y'umutekano ya SSH |
| **UGOMBA** gukora isuku muri `deactivate()` | Kuraho abumva, gusiba ibihe |

### Gukemura amakosa

Buri gihe shyira ibikorwa bifite ibyago mu try/catch:
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
### Isuku muri deactivate()

Niba plugin yawe ikora ibihe, abumva, cyangwa ubusabe — bikoze isuku:
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
### Ubufasha bwa i18n

WIA SOOM ishyigikira indimi 254. Kugira ngo ibirango bya plugin yawe bibe byashobora guhindurwa, kora mu buryo bworoshye:
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

## Igice cya 6: Ingero z'Ukuri

### Urugero rwa 1: Umukoresha Disk Checker

Ikora `df -h` ku murongo wa kure ikerekana umwanya wakoreshejwe/uboneka mu murongo w'ibimenyetso.
§§§CHUNK_SEPARATOR§§§
---

### Urugero rwa 2: Umuyobozi wa TODO

Plugin iyobora urutonde rwa TODO ikoresha imiterere yo kubika ibiramba no webview yo kwerekana.

> **Imiterere y'igishushanyo:** Kubera ko webviews zitabasha guhamagara APIs za plugin by'umwihariko, iyi plugin ikoresha uburyo bwa "snapshot" — isoma TODOs muri settings, ikabishyira mu buryo bwa HTML idashobora guhindurwa, kandi igatanga ibikorwa bishingiye ku ruhande byo kongeramo ibintu. Webview ni **urwego** rwo kwerekana, ntabwo ari ifishi y'ubufatanye.
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

### Urugero rwa 3: Umugenzuzi w'Amakosa

Igenzura umusaruro wa terminal kandi ikohereza itangazo igihe imiterere runaka ibonetse.
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

## Inyongera: Ibyiciro & Ibimenyetso

### Ibyiciro bya Plugin (29)

Koresha ibi mu `package.json` `keywords` cyangwa igihe ushyikiriza ku rubuga rwa registry:

| Icyiciro | Ibisobanuro |
|----------|-------------|
| `server` | Imicungire rusange y'ibikoresho |
| `devtools` | Ibikoresho by'iterambere |
| `calculator` | Imibare n'ibihinduranya |
| `simulator` | Ibikoresho byo gupima |
| `game` | Imikino ya terminal |
| `business` | Ibikoresho by'ubucuruzi |
| `security` | Umutekano n'igenzura |
| `web` | Imicungire y'ibikoresho bya web |
| `education` | Ibikoresho by'uburezi |
| `health` | Ibikoresho bifitanye isano n'ubuzima |
| `islamic` | Ibikoresho by'Islam (amasaha yo gusenga, n'ibindi) |
| `science` | Ibikoresho by'ubumenyi |
| `quantum` | Ibikoresho by'ikoranabuhanga rya quantum |
| `ai` | Ibikoresho byifashisha AI |
| `biotech` | Ibikoresho bya biotechnologie |
| `space` | Ibikoresho by'ikirere n'inyenyeri |
| `network` | Ibikoresho by'urubuga |
| `database` | Imicungire y'ibikoresho bya database |
| `monitoring` | Igenzura ry'ibikoresho |
| `devops` | DevOps na CI/CD |
| `utility` | Ibikoresho rusange |
| `design` | Ibikoresho byo gushushanya |
| `ecommerce` | Ibikoresho bya e-commerce |
| `automation` | Ibikoresho byo gukoresha mu buryo bwikora |
| `kpop` | Ibikoresho bifitanye isano na K-pop |
| `accessibility` | Ibikoresho by'ubushobozi bwo kugerwaho |
| `analytics` | Igenzura n'ibisubizo |
| `wia` | Ibikoresho bya ekositemu ya WIA |
| `all` | Igaragara mu byiciro byose |

### Ibimenyetso Byemewe (Lucide)

| Izina ry'Ikimenyetso | Koresha ku |
|---------------------|-----------|
| `server` | Imicungire y'ibikoresho |
| `shield` | Umutekano |
| `database` | Database |
| `activity` | Igenzura |
| `terminal` | Ibikoresho bya terminal |
| `code` | Iterambere |
| `hard-drive` | Disk/ububiko |
| `network` | Urubuga |
| `lock` | Kwiyandikisha/kwihisha |
| `eye` | Gukurikirana/igenzura |
| `check-square` | Imirimo/TODO |
| `layout-dashboard` | Imbonerahamwe |
| `settings` | Igenamigambi |
| `zap` | Gukoresha mu buryo bwikora |
| `globe` | Web/mpuzamahanga |

Reba ibimenyetso byose birenga 1,500: [lucide.dev/icons](https://lucide.dev/icons)

---

## Ukeneye Ubufasha?

- **Ibibazo bya GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Ibibazo bya Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugins z'Urugero:** [Website](https://wiasoom.com)
- **Urubuga:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Shyiraho ikintu cy'igitangaza. Kigeze ku isi.</em></p>
<p align="center"><em>— Ikipe ya WIA SOOM</em></p>
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
