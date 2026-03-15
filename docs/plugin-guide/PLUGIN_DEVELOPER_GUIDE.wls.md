<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Tohi Tūātea o ngā Kaihanga Plugin</h1>
<p align="center"><strong>Whakawhanakehia tō ake plugin i roto i te 5 meneti.</strong></p>
<p align="center">Tuhia ngā taputapu pūnaha kaha, ngā papamahi, me ngā aunoa — i roto i te WIA SOOM.</p>

---

## Rārangi Ihirangi

- [Wāhanga 1: Tīmata Tere — Tō Plugin Tuatahi i roto i te 5 Meneti](#wāhanga-1-tīmata-tere--tō-plugin-tuatahi-i-roto-i-te-5-meneti)
- [Wāhanga 2: Plugin Context API Tohutoro](#wāhanga-2-plugin-context-api-tohutoro)
  - [ctx.terminal](#ctxterminal--whakahaere-anga-i-ngā-server-tāhiko)
  - [ctx.sftp](#ctxsftp--whakawhiti-kōnae)
  - [ctx.ui](#ctxui--whakaaturanga-kaiwhakamahi)
  - [ctx.settings](#ctxsettings--pūmanawa-mōkī)
  - [ctx.ai](#ctxai--whakawhitinga-ai)
- [Wāhanga 3: Te Whakapakari i te UI Ritenga me ngā Webviews](#wāhanga-3-te-whakapakari-i-te-ui-ritenga-me-ngā-webviews)
- [Wāhanga 4: Te Whakaputa i tō Plugin](#wāhanga-4-te-whakaputa-i-tō-plugin)
- [Wāhanga 5: Ngā Tikanga Pai](#wāhanga-5-ngā-tikanga-pai)
- [Appendix: Ngā Kāwai me ngā Ikona](#appendix-ngā-kāwai--ngā-ikona)

---

## Wāhanga 1: Tīmata Tere — Tō Plugin Tuatahi i roto i te 5 Meneti

### He aha tāu e hanga

He plugin "Kia ora te ao" e tāpiri ana i tētahi pātene ki te taha. Ka pāwhiritia, ka whakaatu i tētahi pānui.

### Hipanga 1: Hangaia te kōpaki plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Hipanga 2: Hangaia te package.json
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
**Ngā wāhanga e hiahiatia ana:** `name`, `version`, `description`, `author`, `main`

### Hipanga 3: Hangaia te index.js
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
### Hipanga 4: Whakahouhia te WIA SOOM

Whakahouhia te taupānga (nāna i huri te plugin i runga/atu i ngā Tautuhinga → Plugins).

Me kite koe i tētahi pātene **"Kia ora te ao"** i te taha. Pāwhiritia — ka kite koe i tētahi pānui angitu!

### Me pēhea te mahi
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

## Wāhanga 2: Plugin Context API Tohutoro

I te wa e karangahia ana tō `activate(context)` mahi, ka tuku mai te `context` (nāna ko `ctx`) i ēnei API:
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

### `ctx.terminal` — Whakahaere-anga i ngā server tāhiko

#### `terminal.send(sessionId, data)`

Tukua he whakahau (nāna he raraunga) ki tētahi wāhanga terminal e mahi ana.

| Tāuru | Momo | Whakaahuatanga |
|-------|------|----------------|
| `sessionId` | `string` | Te wāhanga terminal hei tuku ki |
| `data` | `string` | Te whakahau, te raraunga hei tuku |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Tūhono ki ngā putanga katoa mai i tētahi wāhanga terminal. Ka hoki mai he **mahi tango**.

| Tāuru | Momo | Whakaahuatanga |
|-------|------|----------------|
| `sessionId` | `string` | Te wāhanga terminal hei mātaki |
| `callback` | `(data: string) => void` | Ka karangahia i ngā wāhanga katoa o te putanga |
| **Ka hoki mai** | `() => void` | Karangahia tēnei hei mutu te whakarongo |
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
**He mea nui:** Tiakina ngā mahi tango i ngā wā katoa, ā, karangahia i roto i te `deactivate()` hei aukati i ngā raru mahara.

---

### `ctx.sftp` — Whakawhiti kōnae

> **Tūnga: Ka haere mai** — Kua whakatakotoria te API SFTP engari kāore anō i te hono ki te pūnaha SFTP o te taupānga. Ko te `list()` e whakahoki ana i tētahi rārangi kore, ā, ko te `upload()`/`download()` he mahi kāore. Ka whakatutukihia tēnei i roto i tētahi putanga kei te heke mai. I tēnei wā, whakamahia te `ctx.terminal.send()` me ngā whakahau `scp` rānei `rsync` hei rongoā.

#### `sftp.list(sessionId, path)`

Rārangi ngā kōnae i roto i t��tahi kōpaki mamao.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Tuku kōnae mai i te rorohiko kāinga ki te server mamao.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Tārua kōnae mai i te server mamao ki te rorohiko kāinga.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Rongoā (a mua i te API SFTP):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Whakaaturanga kaiwhakamahi

#### `ui.addSidebarButton(options)`

Tāpiri i tētahi pātene ki te taha WIA SOOM.

| Kōwhiringa | Momo | E hiahiatia ana | Whakaahuatanga |
|------------|------|----------------|-----------------|
| `id` | `string` | Kāore | ID motuhake (ka waiho hei ingoa plugin) |
| `icon` | `string` | Āe | Ingoa ikona Lucide (hei tauira, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Āe | Te tuhinga pātene e kitea ana i te taha |
| `onClick` | `() => void` | Āe | Te mahi ka karangahia i te pātene e pāwhiritia ana |
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
**Tūtohu ikona:** Tirohia ngā ikona e wātea ana i [lucide.dev/icons](https://lucide.dev/icons)

> **Tūtohu pātea:** Ko ētahi plugin tawhito e whakamahi ana i ngā tāuru tuhinga pērā i te `addSidebarButton(id, icon, label, onClick)`. Ko te API whaimana e whakamahi ana i tētahi **whakaritenga kōwhiringa** e pēnei ana i te tuhinga i runga. Me whakamahi i te āhua kōwhiringa mō ngā plugin hou.

#### `ui.openWebview(options)`

Tīmatahia tētahi matapihi pop-up me ngā ihirangi HTML ritenga. Ko tēnei te huarahi ki te hanga i ngā UI whaihua. 

| Kōwhiringa | Momo | Whakaahuatanga |
|------------|------|-----------------|
| `title` | `string` | Te taitara matapihi |
| `html` | `string` | Te ihirangi HTML katoa hei whakaatu |
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
> Vaai i [Part 3](#part-3-building-custom-ui-with-webviews) mo e ngaahi pateni webview advanced.

#### `ui.showNotification(type, message)`

Fakaʻatu e toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Fakaʻilonga notification |
| `message` | `string` | Tohi ke fakaʻatu |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Fakaʻatu e item tohi mau ki he bottom status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID unique ki he item status ko ʻeni |
| `text` | `string` | Tohi ke fakaʻatu |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Fakaʻilonga mau

Ko e ngaahi settings plugin e fakaʻilonga mau i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Fakafou e value kuo fakaʻilonga.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Fakaʻatu `undefined` ki he key ko e ʻikai ke maʻu.

#### `settings.set(key, value)`

Fakaʻilonga e value. Fakaʻatu e ngaahi strings, numbers, booleans, arrays, mo e objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Fakakaukau: Fakaʻilonga e ngaahi preferences ʻo e user**
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

### `ctx.ai` — AI integration

> **Status: Faka��atu mai** — Ko e AI API e fakaʻilonga, ka ʻikai ke fakafesootai ki Soomy. Fakaʻatu `{ response: 'AI not yet connected' }`. Ko e AI integration katoa e fakaʻatu ki he release ʻo e taimi mai.

#### `ai.chat(messages, options?)`

Fakaʻatu e ngaahi messages ki he AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Fakaʻilonga e UI Custom mo e Webviews

Ko e `openWebview()` API e mafai ai ke ke fakaʻilonga e dashboard UIs mo e HTML, CSS, mo e JavaScript — kotoa i loko ʻo e popup window.

> **Fakaʻilonga mahuʻinga:** Ko e Webviews e **fakaʻatu pē**. ʻIkai ke nau hiki ki he plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Fakaʻaongaʻi e sidebar buttons ki he ngaahi action ʻo e user kotoa, mo e fakaʻaongaʻi e `openWebview()` ke fakaʻatu e state ʻo e taimi ko ʻeni. Kapau te ke manaʻomia e ngaahi feature interactive, fakaʻatu ia mai he sidebar buttons mo e toe fakaʻatu e webview ke refresh e fakaʻatu.

### Pateni: Terminal Command → Parse Output → Fakaʻatu ki he HTML

Ko e pateni plugin ko ia e maʻulalo. Te ke fakaʻatu e command, parse e resulta, mo e fakaʻatu ia visually.
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
### Pateni: Interactive Dashboard mo e Auto-Refresh
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
### Pateni: Fakaʻatu e Settings i he Webview

> **Fakaʻilonga:** Ko e Webviews e fakaʻatu pē — ʻikai ke nau hiki ki he plugin APIs. Fakaʻaongaʻi e `ctx.settings` i he sidebar button handlers ke fakafou e settings, mo e fakaʻaongaʻi e `openWebview()` ke fakaʻatu e state ʻo e taimi ko ʻeni.
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

## Part 4: Fakaʻatu e Plugin ʻo Koe

### Laʻunga 1: Fakaʻilonga locally

1. Kōpī e plugin ʻo koe ki `~/.wia-soom/plugins/{your-plugin}/`
2. Toe fakaʻosi e WIA SOOM
3. Fakaʻilonga ko ia e ngāue: e fakaʻatu e sidebar button, e ngāue lelei e ngaahi feature
4. Fakaʻilonga e ngaahi edge cases: ko e hā e hoko ki he ʻikai ke maʻu e terminal?

### Laʻunga 2: Fakaʻatā ki he submission

E tatau ke maʻu e folder plugin ʻo koe:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Faatufugaga `package.json` e manaʻomia:**

| Fa'ata'ita'iga | Fa'amatalaga | Fa'ata'ita'iga |
|----------------|--------------|----------------|
| `name` | ID e le fa'atauina i le kebab-case | `"my-awesome-plugin"` |
| `version` | Fa'ata'iga semantika | `"1.0.0"` |
| `description` | Fa'amatalaga i le laina tasi | `"Monitors nginx access logs in real-time"` |
| `author` | Lau igoa | `"John Doe"` |
| `main` | Fa'avaaiga | `"index.js"` |

**Fa'ata'ita'iga e le manaʻomia:**

| Fa'ata'ita'iga | Fa'amatalaga |
|----------------|--------------|
| `license` | Ituaiga laisene (MIT e fa'amoemoeina) |
| `keywords` | Fa'ata'ita'iga o fa'ailoga su'esu'e |
| `soom.minVersion` | Fa'avaaiga WIA SOOM e manaʻomia i le maualalo |

### La'asaga 3: Fa'amolemole i le Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Fa'aopoopo** lau plugin i `plugins/{your-plugin-name}/`
3. **Fa'amolemole** se Pull Request

### La'asaga 4: Iloilo ma le fa'amaonia

Matou te iloiloina uma plugins mo:

- **Saogalemu** — e le mafai ona i ai ni APIs e le saogalemu (va'ai i [Security Rules](#security-rules))
- **Tulaga** — e faigaluega? E lelei le code?
- **Fa'aaoga** — e fo'ia se fa'afitauli moni?

A uma le fa'amaonia:
1. O lau plugin e fa'aopoopo i `registry.json`
2. O se ZIP bundle e faia i `dist/`
3. O lau plugin e faʻaalia i le **Plugin Store** mo tagata uma o WIA SOOM!

---

## Vaega 5: Fa'avae i le Fa'avae

### Fa'avae Saogalemu

O nei fa'avae e **mana'omia**. O plugins e fa'aleagaina i latou o le a fa'ate'ia.

| Fa'avae | Aisea |
|---------|-------|
| **E le mafai** ona fa'aogaina `eval()` po'o `new Function()` | Fa'ama'i i le code injection |
| **E le mafai** ona fa'aogaina `child_process`, `exec()`, `spawn()` | Fa'amolemole na'o le `ctx.terminal.send()` mo fa'atonuga |
| **E le mafai** ona fa'amaonia URL i fafo | Fa'avae: `wiasoom.com` API endpoints |
| **E le mafai** ona fa'aogaina `process.env` | E mafai ona i ai fa'amaumauga i le si'osi'omaga |
| **E le mafai** ona fa'aogaina `require('fs')` i le fa'avae | Fa'amolemole fa'aaoga `ctx.settings` mo le teuina, `ctx.sftp` mo le fa'amaumau fa'ailoa |
| **E le mafai** ona fa'aogaina npm external packages | JavaScript na'o le — e le aofia ai node_modules |
| **E tatau** ona fa'aoga `ctx.terminal.send()` mo fa'atonuga uma i fafo | O le a fa'agaioia i le auala SSH saogalemu |
| **E tatau** ona fa'amama i le `deactivate()` | Aveese le au fa'ailoa, fa'amama le taimi |

### Fa'avae i le Fa'ama'i

Fa'amolemole fa'aoga le fa'avaaiga i le fa'ama'i i le try/catch:
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
### Fa'amama i le deactivate()

Afai e fa'avae lau plugin i le fa'avae i le taimi, au fa'ailoa, po'o fa'amaumauga — fa'amama i latou:
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
### i18n Fa'avae

WIA SOOM e lagolagoina le 254 gagana. E fa'avae i le fa'avae i lau plugin e mafai ona fa'ailoa, fa'aaoga se fa'avae faigofie:
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

## Vaega 6: Fa'ata'ita'iga i le Fa'avae

### Fa'ata'ita'iga 1: Fa'ata'ita'iga Disk Server

Fa'agaioia `df -h` i le server i fafo ma fa'aalia le avanoa e fa'aaogaina/ua fa'aaogaina i le status bar.
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

### Fa'ata'ita'iga 2: Fa'atonu TODO

O se plugin e fa'atonu se lisi TODO e fa'aaoga fa'avae mo le teuina tumau ma se webview mo le fa'aalia.

> **Fa'avae i le fa'avae:** Talu ai e le mafai e le webviews ona vala'au i le plugin APIs, o le plugin e fa'aaoga se "snapshot" fa'avae — e faitau TODOs mai fa'avae, e fa'avae i latou i le HTML e le mafai ona fa'aaogaina, ma e ofoina atu gaioiga i le itu mo le fa'aopoopoina o fa'ata'ita'iga. O le webview o se **fa'aalia** fa'avae, e le o se fa'ata'ita'iga fa'atekinolosi.
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

### Fa'ata'ita'iga 3: Fa'ata'ita'iga Fa'ama'i

E fa'ailoa le fa'ama'i i le terminal ma e fa'ailoa se fa'ailoa pe a maua ni fa'ata'ita'iga patino.
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

## Appendix: Kategori & Ikon

### Kategori Plugin (29)

Fa'aaoga 'a e ngaahi `package.json` `keywords` pe ka e fa'ahinga ki he registry:

| Kategori | Faka'uhinga |
|----------|-------------|
| `server` | Faka'uhinga 'o e ma'ama'a server |
| `devtools` | Ngaahi me'akai faka'ako |
| `calculator` | Ngaahi kalkulator mo e ngaahi fakamālohi |
| `simulator` | Ngaahi simulators |
| `game` | Ngaahi ta'anga terminal |
| `business` | Ngaahi me'akai faka'anga |
| `security` | Ngaahi me'akai faka'ofefine mo e fakamālohi |
| `web` | Faka'uhinga 'o e ma'ama'a web |
| `education` | Ngaahi me'akai faka'ako |
| `health` | Ngaahi me'akai faka'ola |
| `islamic` | Ngaahi me'akai faka'islama (naki taimi, etc.) |
| `science` | Ngaahi me'akai faka'enseni |
| `quantum` | Ngaahi me'akai faka'quantum |
| `ai` | Ngaahi me'akai faka'AI |
| `biotech` | Ngaahi me'akai faka'biotechnology |
| `space` | Ngaahi me'akai faka'anga mo e ngaahi fetu |
| `network` | Ngaahi me'akai faka'neti |
| `database` | Faka'uhinga 'o e database |
| `monitoring` | Faka'uhinga 'o e ma'ama'a |
| `devops` | DevOps mo e CI/CD |
| `utility` | Ngaahi me'akai faka'anga |
| `design` | Ngaahi me'akai faka'anga |
| `ecommerce` | Ngaahi me'akai faka'ekonomika |
| `automation` | Ngaahi me'akai faka'automation |
| `kpop` | Ngaahi me'akai faka'K-pop |
| `accessibility` | Ngaahi me'akai faka'ofefine |
| `analytics` | Ngaahi me'akai faka'analitika mo e fakamālohi |
| `wia` | Ngaahi me'akai faka'WIA |
| `all` | E 'asi ki he ngaahi kategori kotoa |

### Ngaahi Ikon Faka'ofa (Lucide)

| Faka'ahinga Ikon | Fa'aaoga ki he |
|------------------|---------------|
| `server` | Faka'uhinga 'o e ma'ama'a |
| `shield` | Faka'ofefine |
| `database` | Database |
| `activity` | Faka'uhinga 'o e ma'ama'a |
| `terminal` | Ngaahi me'akai terminal |
| `code` | Faka'ako |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Faka'anga/faka'uhinga 'o e ma'ama'a |
| `check-square` | Ngaahi ta'angata/TODO |
| `layout-dashboard` | Ngaahi dashboard |
| `settings` | Faka'uhinga |
| `zap` | Faka'automation |
| `globe` | Web/international |

Faka'ava ki he 1,500+ ngaahi ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Ongoongo 'i he Faka'ahinga?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Ngaahi Plugin Faka'ahinga:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Faka'ava e ngaahi me'akai ma'ama'a. Faka'ava ki he lalolagi.</em></p>
<p align="center"><em>— Ko e Kautaha WIA SOOM</em></p>