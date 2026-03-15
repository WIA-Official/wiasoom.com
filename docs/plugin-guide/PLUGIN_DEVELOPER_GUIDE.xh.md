<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Umkhombandlela Womphakathi Wokwakha</h1>
<p align="center"><strong>Yakha umphakathi wakho kwiiyure ezi-5.</strong></p>
<p align="center">Yenza amathuluzi anamandla, i-dashboard, kunye ne-automations — ngqo ngaphakathi kwi-WIA SOOM.</p>

---

## Uluhlu Lokuqukethwe

- [Ingxenye 1: Ukuqala Ngokukhawuleza — Umphakathi Wakho Wokuqala kwiiyure ezi-5](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Ingxenye 2: Umphakathi Wokucacisa i-API](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Ingxenye 3: Ukwakha i-UI eyenziwe ngokwezifiso ngeWebviews](#part-3-building-custom-ui-with-webviews)
- [Ingxenye 4: Ukupapasha Umphakathi Wakho](#part-4-publishing-your-plugin)
- [Ingxenye 5: Iindlela Ezingcono](#part-5-best-practices)
- [Ingxenye 6: Iimeko Zomhlaba Ongempela](#part-6-real-world-examples)
- [I-Appendix: Izigaba & Iikona](#appendix-categories--icons)

---

## Ingxenye 1: Ukuqala Ngokukhawuleza — Umphakathi Wakho Wokuqala kwiiyure ezi-5

### Yintoni ozakuyakha

Umphakathi "Hello World" oza kwongeza inkinobho kwi-sidebar. Xa ucofa, ibonisa isaziso.

### Isinyathelo 1: Yenza ifolda yomphakathi
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Isinyathelo 2: Yenza package.json
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
**Iifayile ezifunekayo:** `name`, `version`, `description`, `author`, `main`

### Isinyathelo 3: Yenza index.js
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
### Isinyathelo 4: Qalisa kwakhona i-WIA SOOM

Qalisa kwakhona usetyenziso (okanye ucofe umphakathi off/on kwi Settings → Plugins).

Uza kubona inkinobho **"Hello World"** kwi-sidebar. Ucofa kuyo — uza kubona isaziso sokuphumelela!

### Indlela esebenza ngayo
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

## Ingxenye 2: Umphakathi Wokucacisa i-API

Xa umsebenzi wakho `activate(context)` ubizwa, `context` (okanye `ctx`) ibonelela ngalezi zii-API:
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

### `ctx.terminal` — Sebenzisa imiyalelo kwiiseva ezikude

#### `terminal.send(sessionId, data)`

Thumela imiyalelo (okanye nayiphi na idatha) kumjikelo we-terminal osebenzayo.

| Umphakathi | Uhlobo | Inkcazo |
|-----------|------|-------------|
| `sessionId` | `string` | Umjikelo we-terminal onokuthumela kuye |
| `data` | `string` | Imiyalelo okanye idatha yokuthumela |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Bhalisela yonke imveliso evela kumjikelo we-terminal. Ibuyisela **umsebenzi wokungabhaliswanga**.

| Umphakathi | Uhlobo | Inkcazo |
|-----------|------|-------------|
| `sessionId` | `string` | Umjikelo we-terminal okufuneka uwujolise |
| `callback` | `(data: string) => void` | Ibuya nganye nganye ye-mveliso |
| **Ibuyisela** | `() => void` | Cofa oku ukuze uyeke ukulalela |
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
**Kubalsiswa:** Qinisekisa ukuba ugcina umsebenzi wokungabhaliswanga kwaye uwubize kwi `deactivate()` ukuze ugweme ukuvuza kwememori.

---

### `ctx.sftp` — Ukuthumela ifayile

> **Isimo: Kuza KwiXesha Elizayo** — I-SFTP API ichazwe kodwa ayikawuxhumi kwi-SFTP engine yesicelo. `list()` ngoku ibuyisela i-array engenanto, kwaye `upload()`/`download()` azisebenzi. Oku kuya kuphumeza ngokupheleleyo kwikhambo elizayo. Okwangoku, sebenzisa `ctx.terminal.send()` ngeemiyalelo `scp` okanye `rsync` njengendlela yokuphucula.

#### `sftp.list(sessionId, path)`

Bala iifayile kwi-directory ekude.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Thumela ifayile ukusuka kumatshini wendawo ukuya kwi-server ekude.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Landa ifayile ukusuka kwi-server ekude ukuya kumatshini wendawo.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Indlela yokuphucula (kude kube i-SFTP API isebenza):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Umphakathi womsebenzisi

#### `ui.addSidebarButton(options)`

Yongeza inkinobho kwi-sidebar ye-WIA SOOM.

| Ukhetho | Uhlobo | Kufuneka | Inkcazo |
|--------|------|----------|-------------|
| `id` | `string` | Hayi | ID eyodwa (iyakhetha igama lomphakathi) |
| `icon` | `string` | Ewe | Igama le-lucide icon (umzekelo, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ewe | Umbhalo wenkinobho oboniswa kwi-sidebar |
| `onClick` | `() => void` | Ewe | Umsebenzi obizwa xa inkinobho icofwa |
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
**Umphakathi we-icon:** Jonga zonke iikona ezikhoyo kwi [lucide.dev/icons](https://lucide.dev/icons)

> **Ingcebiso yokuhambelana:** Ezinye iiplug-ins ezindala zisebenzisa iimeko ezikhoyo ezifana `addSidebarButton(id, icon, label, onClick)`. I-API esemthethweni isebenzisa **into yeendlela** njengoko kuchaziwe ngasentla. Qinisekisa ukuba usebenzisa indlela ye-into kwiiplug-ins ezintsha.

#### `ui.openWebview(options)`

Vula iwindi le-popup eline-content ye-HTML eyenziwe ngokwezifiso. Le yindlela yokwakha i-UI ezinzulu. 

| Ukhetho | Uhlobo | Inkcazo |
|--------|------|-------------|
| `title` | `string` | Isihloko sewindi |
| `html` | `string` | Umxholo opheleleyo we-HTML okufuneka uboniswe |
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
> Jonga [Ingxenye 3](#part-3-building-custom-ui-with-webviews) ukuze ufumane iindlela eziphucukileyo zokusebenzisa i-webview.

#### `ui.showNotification(type, message)`

Bonisa isaziso se-toast.

| Umpharamitha | Uhlobo | Incazelo |
|--------------|--------|----------|
| `type` | `'success' \| 'error' \| 'info'` | Uhlobo lwesaziso |
| `message` | `string` | Umbhalo okufuneka uboniswe |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Yongeza into yombhalo eqhubekayo kwi-status bar engezantsi.

| Umpharamitha | Uhlobo | Incazelo |
|--------------|--------|----------|
| `id` | `string` | ID eyodwa yale nto ye-status |
| `text` | `string` | Umbhalo okufuneka uboniswe |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Ukugcina okuqhubekayo

Iimposthu ze-plugin zigcinwa ngonaphakade kwi-`~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Funda ixabiso eligciniweyo.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Ibuyisa `undefined` ukuba i-key ayikho.

#### `settings.set(key, value)`

Gcina ixabiso. Isebenzisa iikholi, amanani, iibhulorho, ii-array, kunye neempawu.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Umzekelo: Khumbula ukhetho lomsebenzisi**
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

### `ctx.ai` — Uxhulumaniso lwe-AI

> **Isimo: Kuza Kakhulu** — I-AI API ichazwe kodwa ayikaxhunyiwe kwi-Soomy. Okwangoku ibuyisa `{ response: 'AI not yet connected' }`. Uxhulumaniso olupheleleyo lwe-AI luhlelwe ukuba lwenziwe kwikhamandela elizayo.

#### `ai.chat(messages, options?)`

Thumela imiyalezo kumphathiswa we-AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Ingxenye 3: Ukwakha i-UI eyenziwe ngokwezifiso nge-Webviews

I-`openWebview()` API ikuvumela ukuba wakhe i-UI ye-dashboard ngama-HTML, CSS, kunye ne-JavaScript — konke ngaphakathi kwifestile ye-popup.

> **Umgca obalulekileyo:** I-webviews ziyi-**display-only**. Azinakubiza kwi-API ze-plugin (`ctx.settings`, `ctx.terminal`, njl.). Sebenzisa iisikhumbuzo ze-sidebar kuzo zonke iimeko zomsebenzisi, kwaye usebenzise `openWebview()` ukubonisa imeko yangoku. Ukuba ufuna iimpawu ezisebenzisanayo, ziqhube kwiisikhumbuzo ze-sidebar kwaye uvule kwakhona i-webview ukuze uhlaziye umboniso.

### Umfanekiso: Umcommand weTerminal → Phosa Umphumo → Bonisa kwi-HTML

Le yindlela evame kakhulu ye-plugin. Uqhuba umyalelo, uphosa umphumo, kwaye uwubonisa ngombono.
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
### Umfanekiso: I-Dashboard Ebonakalayo enezinto ezizenzekelayo
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
### Umfanekiso: Ukubonisa Iimposthu kwi-Webview

> **Qaphela:** I-webviews ziyi-display-only — azinakubiza kwi-API ze-plugin. Sebenzisa `ctx.settings` kwiisikhumbuzo zakho ze-sidebar ukuze uguqule iimposthu, kwaye usebenzise `openWebview()` ukubonisa imeko yangoku.
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

## Ingxenye 4: Ukupapasha i-Plugin Yakho

### Isinyathelo 1: Hlola kwiindawo zendawo

1. Kopisha i-plugin yakho kwi-`~/.wia-soom/plugins/{your-plugin}/`
2. Qalisa kwakhona i-WIA SOOM
3. Qinisekisa ukuba iyasebenza: isikhumbuzo se-sidebar siza, iimpawu zisebenza ngokuchanekileyo
4. Hlola iimeko ezinzima: kwenzekani ukuba akukho terminal ixhunyiwe?

### Isinyathelo 2: Lungiselela ukuthunyelwa

Ifolda ye-plugin yakho kufuneka ibe ne:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Imfuneko `package.json` iimpawu:**

| Impawu | Inkcazo | Umzekelo |
|--------|---------|----------|
| `name` | I-ID eyodwa ye-kebab-case | `"my-awesome-plugin"` |
| `version` | Inguqulo ye-semantic | `"1.0.0"` |
| `description` | Inkcazo ye-line enye | `"Monitors nginx access logs in real-time"` |
| `author` | Igama lakho | `"John Doe"` |
| `main` | Umjikelo wokungena | `"index.js"` |

**Iimpawu ezikhethwayo:**

| Impawu | Inkcazo |
|--------|---------|
| `license` | Uhlobo lwe-licence (MIT inrecommended) |
| `keywords` | Uluhlu lweetags zokukhangela |
| `soom.minVersion` | Inguqulo ye-WIA SOOM encinci efunekayo |

### Isinyathelo 3: Thumela kwiPlugin Registry

1. ****Package** your plugin as a ZIP file
2. **Faka** i-plugin yakho kwi `plugins/{your-plugin-name}/`
3. **Thumela** i-Pull Request

### Isinyathelo 4: Uhlolo kunye nolwaziso

Sijonga yonke i-plugin ukuze:

- **Ukhuseleko** — akukho API ezingozi (jonga [Imigaqo Yokhuseleko](#security-rules))
- **Umgangatho** — ingaba iyasebenza? Ingaba ikhowudi icacile?
- **Ukusetyenziswa** — ingaba ixazulula ingxaki yokwenyani?

Emva kokuvunywa:
1. I-plugin yakho ifakwe kwi `registry.json`
2. I-ZIP bundle yenziwe kwi `dist/`
3. I-plugin yakho ibonakala kwi **Plugin Store** kubo bonke abasebenzisi be-WIA SOOM!

---

## Icandelo 5: Iindlela Ezingcono

### Imigaqo Yokhuseleko

Le migaqo **iyafuneka**. Iiplugini eziphula le migaqo ziya kuxhaphazeka.

| Umgaqo | Kutheni |
|--------|--------|
| **UNGASIBHIDLA** usebenzisa `eval()` okanye `new Function()` | Umngcipheko wokufaka ikhowudi |
| **UNGASIBHIDLA** usebenzisa `child_process`, `exec()`, `spawn()` | Sebenzisa kuphela `ctx.terminal.send()` kwiimiyalelo |
| **UNGASIBHIDLA** ukufumana i-URLs zangaphandle | Umgaqo: `wiasoom.com` API endpoints |
| **UNGASIBHIDLA** ukufikelela `process.env` | Iimpawu zeMeko zingabamba imfihlo |
| **UNGASIBHIDLA** usebenzisa `require('fs')` ngqo | Sebenzisa `ctx.settings` yokugcina, `ctx.sftp` yokuhambisa ifayile |
| **UNGASIBHIDLA** usebenzisa iipakethe zangaphandle ze-npm | IJavaScript ye-Pure kuphela — akukho node_modules |
| **KUFUNEKA** usebenzisa `ctx.terminal.send()` kwiimiyalelo zonke ezikude | Oku kudlula kwi-channel ye-SSH ekhuselekileyo |
| **KUFUNEKA** uhlambulule kwi `deactivate()` | Susa abaphulaphuli, cima i-intervals |

### Ukuphathwa Kweziphumo

Ngokuqhelekileyo, gubungela imisebenzi enokubangela umngcipheko kwi try/catch:
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
### Uhlambululo kwi deactivate()

Ukuba i-plugin yakho idala i-intervals, abaphulaphuli, okanye i-subscriptions — uhlambulule:
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
### i18n Ukuxhaswa

WIA SOOM ixhasa iilwimi eziyi-254. Ukuze ube ne-plugin yakho ethe ngqo, sebenzisa indlela elula:
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

## Icandelo 6: Iimeko Zangempela

### Umzekelo 1: Umjolisi weDisk yeServer

Isebenza `df -h` kwi-server ekude kwaye ibonisa indawo esetyenzisiweyo/efumanekayo kwi-status bar.
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

### Umzekelo 2: Umphathiswa weTODO

I-plugin ephatha uluhlu lwe-TODO isebenzisa iimeko zokugcina ezihlala zihlala kunye ne-webview yokubonisa.

> **Umfanekiso wokuyila:** Njengoko i-webviews ingakwazi ukucela ngqo i-API ze-plugin, le plugin isebenzisa indlela ye-"snapshot" — ifunda i-TODOs kwiimeko, ibonisa njenge-HTML engafundwanga, kwaye ibonelela ngemicimbi esekwe kwi-sidebar yokongeza izinto. I-webview iyindawo ye-**bonisa**, hayi ifom ethandwayo.
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

### Umzekelo 3: Umjolisi weMphumo

Ujolisa umphumo we-terminal kwaye uthumela isaziso xa iipatheni ezithile zifumaneka.
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

## Appendix: Iigatya & Iimifanekiso

### Iigatya zePlugin (29)

Sebenzisa ezi kwi `package.json` `keywords` okanye xa uthumela kwi registry:

| Igatya | Inkcazo |
|--------|---------|
| `server` | Ulawulo lwe server jikelele |
| `devtools` | Izixhobo zophuhliso |
| `calculator` | Iikhalika kunye neenkqubo zokuguqula |
| `simulator` | Iisimulator |
| `game` | Imidlalo yeTerminal |
| `business` | Izixhobo zeshishini |
| `security` | Ubumfihlo kunye nokuhlola |
| `web` | Ulawulo lwe web server |
| `education` | Izixhobo zophando |
| `health` | Izixhobo ezinxulumene nempilo |
| `islamic` | Izixhobo ze-Islam (ixesha lemithandazo, njl.) |
| `science` | Izixhobo zescience |
| `quantum` | Izixhobo ze-quantum computing |
| `ai` | Izixhobo ezinamandla e-AI |
| `biotech` | Izixhobo ze-biotechnology |
| `space` | Izixhobo zeendawo kunye neastronomy |
| `network` | Izixhobo zenethiwekhi |
| `database` | Ulawulo lwe database |
| `monitoring` | Ukujolisa kwi server |
| `devops` | DevOps kunye ne-CI/CD |
| `utility` | Izixhobo jikelele |
| `design` | Izixhobo zokuyila |
| `ecommerce` | Izixhobo ze-e-commerce |
| `automation` | Izixhobo zokwenza ngokuzenzekelayo |
| `kpop` | Izixhobo ezinxulumene ne-K-pop |
| `accessibility` | Izixhobo zokufikelela |
| `analytics` | I-analytics kunye neengxelo |
| `wia` | Izixhobo ze-WIA ecosystem |
| `all` | Ibonakala kwiigatya zonke |

### Iimifanekiso Ezincomekayo (Lucide)

| Igama leMifanekiso | Sebenzisa kwi |
|-------------------|--------------|
| `server` | Ulawulo lwe server |
| `shield` | Ubumfihlo |
| `database` | Database |
| `activity` | Ukujolisa |
| `terminal` | Izixhobo zeTerminal |
| `code` | Uphuhliso |
| `hard-drive` | Idiski/ukugcina |
| `network` | Uxhulumaniso |
| `lock` | Auth/encryption |
| `eye` | Ukubukela/ukujolisa |
| `check-square` | Imisebenzi/TODO |
| `layout-dashboard` | Iidashboard |
| `settings` | Ulungiso |
| `zap` | Ukuzenzekelayo |
| `globe` | IWeb/international |

Jonga zonke i-1,500+ iimifanekiso: [lucide.dev/icons](https://lucide.dev/icons)

---

## Ufuna Uncedo?

- **I-GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Iingxaki zePlugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Iimveliso zePlugin:** [Website](https://wiasoom.com)
- **Webhusayithi:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Yila into emangalisayo. Yabelana ngayo nomhlaba.</em></p>
<p align="center"><em>— Iqela le-WIA SOOM</em></p>