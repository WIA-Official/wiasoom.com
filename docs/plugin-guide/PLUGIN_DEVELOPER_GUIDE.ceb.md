<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Himoa ang imong kaugalingong plugin sa sulod sa 5 ka minuto.</strong></p>
<p align="center">Magmahimo og kusgan nga mga himan sa server, dashboards, ug mga awtomasyon — direkta sa sulod sa WIA SOOM.</p>

---

## Table of Contents

- [Part 1: Quick Start — Your First Plugin in 5 Minutes](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Building Custom UI with Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Publishing Your Plugin](#part-4-publishing-your-plugin)
- [Part 5: Best Practices](#part-5-best-practices)
- [Part 6: Real-World Examples](#part-6-real-world-examples)
- [Appendix: Categories & Icons](#appendix-categories--icons)

---

## Part 1: Quick Start — Your First Plugin in 5 Minutes

### Unsay imong himoon

Usa ka "Hello World" nga plugin nga nagdugang og button sa sidebar. Kung i-klik, magpakita kini og notification.

### Lakang 1: Himoa ang plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Lakang 2: Himoa ang package.json
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
**Gikinahanglan nga mga field:** `name`, `version`, `description`, `author`, `main`

### Lakang 3: Himoa ang index.js
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
### Lakang 4: I-restart ang WIA SOOM

I-restart ang app (o i-toggle ang plugin off/on sa Settings → Plugins).

Dapat makita nimo ang usa ka **"Hello World"** nga button sa sidebar. I-klik kini — makakita ka og usa ka success notification!

### Unsay paagi niini
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

## Part 2: Plugin Context API Reference

Kung ang imong `activate(context)` nga function gitawag, ang `context` (o `ctx`) naghatag niining mga API:
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

### `ctx.terminal` — Magpatuman og mga sugo sa remote servers

#### `terminal.send(sessionId, data)`

Magpadala og sugo (o bisan unsang datos) sa usa ka aktibong terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Ang terminal session nga padalhan |
| `data` | `string` | Ang sugo o datos nga ipadala |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Mag-subscribe sa tanang output gikan sa usa ka terminal session. Mobalik kini og **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Ang terminal session nga tan-awon |
| `callback` | `(data: string) => void` | Gitawag uban sa matag chunk sa output |
| **Mobalik** | `() => void` | Tawaga kini aron mohunong sa pagpaminaw |
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
**Makahuluganon:** Kanunay nga i-save ang unsubscribe function ug tawaga kini sa `deactivate()` aron malikayan ang memory leaks.

---

### `ctx.sftp` — Pagbalhin sa file

> **Status: Coming Soon** — Ang SFTP API gidefine apan wala pa nakasumpay sa SFTP engine sa app. Ang `list()` sa pagkakaron nagbalik og walay sulod nga array, ug ang `upload()`/`download()` mga no-ops. Kini kay fully implemented sa umaabot nga release. Sa pagkakaron, gamita ang `ctx.terminal.send()` uban sa `scp` o `rsync` nga mga sugo isip workaround.

#### `sftp.list(sessionId, path)`

Ilista ang mga file sa usa ka remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

I-upload ang usa ka file gikan sa lokal nga makina ngadto sa remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

I-download ang usa ka file gikan sa remote server ngadto sa lokal nga makina.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (hangtod ang SFTP API mag-live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Magdugang og button sa WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Wala | Talagsaon nga ID (default sa ngalan sa plugin) |
| `icon` | `string` | Oo | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Oo | Teksto sa button nga ipakita sa sidebar |
| `onClick` | `() => void` | Oo | Function nga gitawag kung ang button i-klik |
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
**Icon reference:** Tan-awa ang tanang magamit nga mga icon sa [lucide.dev/icons](https://lucide.dev/icons)

> **Nota sa compatibility:** Ang pipila ka mga daan nga plugin naggamit og positional arguments sama sa `addSidebarButton(id, icon, label, onClick)`. Ang opisyal nga API naggamit og **options object** sama sa gidescribe sa ibabaw. Kanunay gamita ang object style alang sa bag-ong mga plugin.

#### `ui.openWebview(options)`

Ablihi ang usa ka popup window nga adunay custom HTML content. Mao kini ang paagi sa paghimo og mga rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Titulo sa bintana |
| `html` | `string` | Kumpletong HTML content nga i-render |
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
> Tan-awa ang [Part 3](#part-3-building-custom-ui-with-webviews) para sa mga advanced nga webview patterns.

#### `ui.showNotification(type, message)`

Ipakita ang toast notification.

| Parameter | Type | Deskripsyon |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Estilo sa notification |
| `message` | `string` | Teksto nga ipakita |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Magdugang ug persistent nga text item sa ubos nga status bar.

| Parameter | Type | Deskripsyon |
|-----------|------|-------------|
| `id` | `string` | Talagsaon nga ID para sa kini nga status item |
| `text` | `string` | Teksto nga ipakita |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Persistent storage

Ang mga setting sa plugin kay permanente nga gitipigan sa `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Basaha ang nasave nga value.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Mobalik ug `undefined` kung ang key wala mag-exist.

#### `settings.set(key, value)`

Isave ang usa ka value. Nagsuporta sa strings, numbers, booleans, arrays, ug objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Pananglitan: Hinumdumi ang mga preference sa user**
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

> **Status: Coming Soon** — Ang AI API kay na-define pero wala pa nakonektar sa Soomy. Sa pagkakaron, mobalik kini ug `{ response: 'AI not yet connected' }`. Ang tibuok nga AI integration giplano para sa umaabot nga pagpagawas.

#### `ai.chat(messages, options?)`

Magpadala ug mga mensahe sa AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Paghimo ug Custom UI gamit ang Webviews

Ang `openWebview()` API nagtugot kanimo sa paghimo ug dashboard UIs gamit ang HTML, CSS, ug JavaScript — tanan sulod sa usa ka popup nga bintana.

> **Mahal nga limitasyon:** Ang mga webview kay **display-only**. Dili sila makatawag balik sa plugin APIs (`ctx.settings`, `ctx.terminal`, ug uban pa). Gamita ang sidebar buttons para sa tanan nga aksyon sa user, ug gamita ang `openWebview()` aron ipakita ang kasamtangang estado. Kung kinahanglan nimo ang interactive nga mga feature, i-trigger kini gikan sa sidebar buttons ug i-reopen ang webview aron ma-refresh ang display.

### Pattern: Terminal Command → Parse Output → Ipakita sa HTML

Kini ang labing kasagarang pattern sa plugin. Nagpadagan ka ug command, gi-parse ang resulta, ug gipakita kini visually.
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
### Pattern: Interactive Dashboard nga adunay Auto-Refresh
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
### Pattern: Pagpakita sa Settings sa usa ka Webview

> **Nota:** Ang mga webview kay display-only — dili sila makatawag balik sa plugin APIs. Gamita ang `ctx.settings` sa imong sidebar button handlers aron usbon ang mga setting, ug gamita ang `openWebview()` aron ipakita ang kasamtangang estado.
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

## Part 4: Pag-publish sa Imong Plugin

### Lakang 1: Sulayi lokalmente

1. Kopyaha ang imong plugin sa `~/.wia-soom/plugins/{your-plugin}/`
2. I-restart ang WIA SOOM
3. Siguroha nga nagtrabaho kini: nagpakita ang sidebar button, nagtrabaho ang mga feature nga husto
4. Sulayi ang mga edge cases: unsay mahitabo kung walay terminal nga nakonektar?

### Lakang 2: Andama para sa submission

Ang imong plugin folder kinahanglan maglakip sa:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Kinahanglanon nga `package.json` nga mga field:**

| Field | Deskripsyon | Pananglitan |
|-------|-------------|---------|
| `name` | Talagsaon nga kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic nga bersyon | `"1.0.0"` |
| `description` | Usa ka-linya nga deskripsyon | `"Monitors nginx access logs in real-time"` |
| `author` | Imong ngalan | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Opsyonal nga mga field:**

| Field | Deskripsyon |
|-------|-------------|
| `license` | Klase sa lisensya (MIT ang girekomenda) |
| `keywords` | Array sa mga search tags |
| `soom.minVersion` | Minimum nga bersyon sa WIA SOOM nga gikinahanglan |

### Lakang 3: I-submit sa Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Idugang** ang imong plugin sa `plugins/{your-plugin-name}/`
3. **I-submit** ang usa ka Pull Request

### Lakang 4: Pagsusi ug pag-apruba

Ato nga susihon ang matag plugin alang sa:

- **Seguridad** — walay delikadong APIs (tan-awa ang [Security Rules](#security-rules))
- **Kalidad** — nagtrabaho ba kini? Limpyo ba ang code?
- **Kagamiton** — nagasolbad ba kini sa tinuod nga problema?

Human sa pag-apruba:
1. Ang imong plugin idugang sa `registry.json`
2. Usa ka ZIP bundle ang gihimo sa `dist/`
3. Ang imong plugin makita sa **Plugin Store** alang sa tanan nga WIA SOOM nga mga tiggamit!

---

## Bahin 5: Pinakamaayong mga Praktis

### Mga Lagda sa Seguridad

Kini nga mga lagda **kinahanglanon**. Ang mga plugin nga molapas niini kay pagadawaton.

| Lagda | Ngano |
|------|-----|
| **AYAW** gamita ang `eval()` o `new Function()` | Risgo sa code injection |
| **AYAW** gamita ang `child_process`, `exec()`, `spawn()` | Gamita lang ang `ctx.terminal.send()` para sa mga sugo |
| **AYAW** pagkuha og external nga mga URL | Eksepsyon: `wiasoom.com` API endpoints |
| **AYAW** pag-access sa `process.env` | Ang mga environment variables mahimong maglaman og mga sekreto |
| **AYAW** gamita ang `require('fs')` diretso | Gamita ang `ctx.settings` para sa storage, `ctx.sftp` para sa pagbalhin sa file |
| **AYAW** gamita ang npm external packages | Pure JavaScript lang — walay node_modules |
| **KINAHANGLAN** gamiton ang `ctx.terminal.send()` para sa tanan nga remote nga mga sugo | Kini moagi sa seguradong SSH channel |
| **KINAHANGLAN** maglimpyo sa `deactivate()` | Kuhaa ang mga listeners, limpyohi ang mga intervals |

### Pagdumala sa Sayop

Kanunay nga ibalot ang mga risgo nga operasyon sa try/catch:
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
### Paglimpyo sa deactivate()

Kung ang imong plugin nagmugna og mga intervals, listeners, o subscriptions — limpyohi kini:
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
### Suporta sa i18n

Ang WIA SOOM nagsuporta sa 254 ka mga sinultian. Aron ang imong plugin label mahimong ma-translate, gamita ang yano nga pamaagi:
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

## Bahin 6: Mga Tinuod nga Pananglitan

### Pananglitan 1: Server Disk Checker

Nagpadagan og `df -h` sa remote nga server ug nagpakita sa gigamit/available nga espasyo sa status bar.
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

### Pananglitan 2: TODO Manager

Usa ka plugin nga nagdumala sa usa ka TODO list gamit ang mga setting para sa persistent storage ug usa ka webview para sa display.

> **Disenyo nga pattern:** Tungod kay ang mga webviews dili makatawag diretso sa plugin APIs, kini nga plugin naggamit og "snapshot" nga pamaagi — kini nagbasa sa mga TODO gikan sa mga setting, nag-render niini isip read-only nga HTML, ug naghatag og mga aksyon nga nakabase sa sidebar para sa pagdugang sa mga item. Ang webview usa ka **display** layer, dili usa ka interactive nga porma.
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

### Pananglitan 3: Error Watcher

Nagsubay sa output sa terminal ug nagpadala og notification kung adunay mga espesipikong pattern nga madetekta.
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

## Appendix: Mga Kategorya & mga Icon

### Mga Kategorya sa Plugin (29)

Gamita kini sa imong `package.json` `keywords` o sa dihang nagsumite sa registry:

| Kategorya | Deskripsyon |
|-----------|-------------|
| `server` | Kinatibuk-ang pagdumala sa server |
| `devtools` | Mga himan sa pagpalambo |
| `calculator` | Mga kalkulador ug mga converter |
| `simulator` | Mga simulator |
| `game` | Mga dula sa terminal |
| `business` | Mga himan sa negosyo |
| `security` | Seguridad ug pag-audit |
| `web` | Pagdumala sa web server |
| `education` | Mga himan sa edukasyon |
| `health` | Mga himan nga may kalabutan sa kahimsog |
| `islamic` | Mga himan nga Islamiko (mga oras sa pag-ampo, ug uban pa) |
| `science` | Mga himan sa siyensya |
| `quantum` | Mga himan sa quantum computing |
| `ai` | Mga himan nga powered sa AI |
| `biotech` | Mga himan sa biotechnology |
| `space` | Mga himan sa espasyo ug astronomiya |
| `network` | Mga himan sa network |
| `database` | Pagdumala sa database |
| `monitoring` | Pagmonitor sa server |
| `devops` | DevOps ug CI/CD |
| `utility` | Kinahanglanon nga mga himan |
| `design` | Mga himan sa disenyo |
| `ecommerce` | Mga himan sa e-commerce |
| `automation` | Mga himan sa awtomasyon |
| `kpop` | Mga himan nga may kalabutan sa K-pop |
| `accessibility` | Mga himan sa accessibility |
| `analytics` | Analytics ug pagreport |
| `wia` | Mga himan sa WIA ecosystem |
| `all` | Nagpakita sa tanan nga mga kategorya |

### Mga Girekomendar nga Icon (Lucide)

| Ngalan sa Icon | Gamita alang sa |
|----------------|------------------|
| `server` | Pagdumala sa server |
| `shield` | Seguridad |
| `database` | Database |
| `activity` | Pagmonitor |
| `terminal` | Mga himan sa terminal |
| `code` | Pagpalambo |
| `hard-drive` | Disko/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Pagbantay/pagmonitor |
| `check-square` | Mga Buluhaton/TODO |
| `layout-dashboard` | Mga Dashboard |
| `settings` | Konfigurasyon |
| `zap` | Awtomasyon |
| `globe` | Web/internasyonal |

Tan-awa ang tanan nga 1,500+ nga mga icon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Nanginahanglan og Tabang?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Mga Pananglitan nga Plugin:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Maghimo og usa ka talagsaon nga butang. Ibahin kini sa kalibutan.</em></p>
<p align="center"><em>— Ang WIA SOOM Team</em></p>