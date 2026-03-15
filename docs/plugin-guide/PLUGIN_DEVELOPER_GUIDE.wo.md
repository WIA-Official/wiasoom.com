<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Jëfandikoo sa plugin ci 5 minit.</strong></p>
<p align="center">Jëfandikoo ay outil server, dashboards, ak automations — ci WIA SOOM.</p>

---

## Table of Contents

- [Part 1: Quick Start — Sa Plugin Bii Nuyoo ci 5 Minits](#part-1-quick-start--sa-plugin-bii-nuyoo-ci-5-minits)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Jëfandikoo UI Custom ak Webviews](#part-3-jëfandikoo-ui-custom-ak-webviews)
- [Part 4: Jàppale Sa Plugin](#part-4-jàppale-sa-plugin)
- [Part 5: Ay Njàngaan Yu Nuyoo](#part-5-ay-njàngaan-yu-nuyoo)
- [Part 6: Njàngaan Yu Jëfandikoo](#part-6-njàngaan-yu-jëfandikoo)
- [Appendix: Categories & Icons](#appendix-categories--icons)

---

## Part 1: Quick Start — Sa Plugin Bii Nuyoo ci 5 Minits

### Lii nga jëfandikoo

Plugin "Hello World" bu jëfandikoo na button ci sidebar. Bi nga toog, muñu jëfandikoo notification.

### Etap 1: Jëfandikoo plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Etap 2: Jëfandikoo package.json
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
**Ay bopp yu am solo:** `name`, `version`, `description`, `author`, `main`

### Etap 3: Jëfandikoo index.js
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
### Etap 4: Restart WIA SOOM

Restart the app (wala togglé plugin bi ci Settings → Plugins).

Danga war a gis **"Hello World"** button ci sidebar. Toog ko — danga gis a success notification!

### Naka loolu am
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

Bi `activate(context)` function bi am, `context` (wala `ctx`) jëfandikoo loolu:
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

### `ctx.terminal` — Jëfandikoo commands ci remote servers

#### `terminal.send(sessionId, data)`

Send a command (wala data bu am) ci terminal session bu am.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal session bu jëfandikoo ko |
| `data` | `string` | Command wala data bu jëfandikoo |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscribe ci output yuy am ci terminal session. Muñu jëfandikoo **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal session bu jëfandikoo ko |
| `callback` | `(data: string) => void` | Muñu jëfandikoo ak ay chunk yuy am ci output |
| **Returns** | `() => void` | Jëfandikoo loolu ngir stop listening |
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
**Important:** Jàppale loolu unsubscribe function bi ak jëfandikoo ko ci `deactivate()` ngir defar memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Nuyoo ci Njàngaan** — SFTP API bi am na, waaye du jëfandikoo ci app bi SFTP engine bi. `list()` buñu jëfandikoo array bu amul, ak `upload()`/`download()` amul. Loolu dina am ci jàmmu jàppale. Ngir loolu, jëfandikoo `ctx.terminal.send()` ak `scp` wala `rsync` commands.

#### `sftp.list(sessionId, path)`

Jëfandikoo files ci remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Jëfandikoo file bu am ci local machine ngir remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Jëfandikoo file bu am ci remote server ngir local machine.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (jusqu'à ce que SFTP API am):**
§§��CHUNK_SEPARATOR§§§
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Jëfandikoo button ci sidebar WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Déedéet | Unique ID (defaults to plugin name) |
| `icon` | `string` | Waaw | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Waaw | Button text bu am ci sidebar |
| `onClick` | `() => void` | Waaw | Function bu jëfandikoo bi button bi toog |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Icon reference:** Jëfandikoo ay icons yuy am ci [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Ay plugins yu am ci yenn ci jëfandikoo ay arguments positional ni `addSidebarButton(id, icon, label, onClick)`. Official API bi jëfandikoo **options object** ni muñu jëfandikoo ci loolu. Jëfandikoo object style bi ngir plugins yu bees.

#### `ui.openWebview(options)`

Jëfandikoo popup window ak custom HTML content. Loolu na luy jëfandikoo rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content ngir render |
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
> Naka [Part 3](#part-3-building-custom-ui-with-webviews) ci patterns webview yu jëm ci diggante.

#### `ui.showNotification(type, message)`

Sooñal na notification toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Njàngat style |
| `message` | `string` | Text bu soñal |
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

Sooñal na item text bu am jëm ci status bar bi.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID bu am solo ci item status bi |
| `text` | `string` | Text bu jëfandikoo |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Storage bu am jëm

Settings yu plugin yi am nañu ci `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Dafay jëfandikoo value bu am.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Dafa jëfandikoo `undefined` su key bi amul.

#### `settings.set(key, value)`

Sooñal na value. Am na support ci strings, numbers, booleans, arrays, ak objects.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Example: Naka user preferences**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — AI integration

> **Status: Naka Nuyoo** — AI API bi am na, waaye du jëm ci Soomy. Dafa jëfandikoo `{ response: 'AI not yet connected' }`. Full AI integration bi am na ci jàmmu jëfandikoo.

#### `ai.chat(messages, options?)`

Sooñal na messages ci AI assistant bi (Soomy).
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

## Part 3: Jëfandikoo UI bu am jëm ak Webviews

API `openWebview()` dafa jëfandikoo la jëfandikoo dashboard UIs ak HTML, CSS, ak JavaScript — bu yàgg ci window bu popup.

> **Limitation bu am jëm:** Webviews am nañu **display-only**. Duñu jëfandikoo ci plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Jëfandikoo buttons ci sidebar bi ngir action yu user, ak jëfandikoo `openWebview()` ngir soñal ci état bu am.

### Pattern: Command Terminal → Parse Output → Soñal ci HTML

Loolu mooy pattern plugin bu am jëm. Danga jëfandikoo command, parse result bi, ak soñal na ci visual.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Pattern: Dashboard Interactive ak Auto-Refresh
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
### Pattern: Soñal ci Settings ci Webview

> **Note:** Webviews am nañu display-only — duñu jëfandikoo ci plugin APIs. Jëfandikoo `ctx.settings` ci buttons ci sidebar bi ngir modify settings, ak jëfandikoo `openWebview()` ngir soñal ci état bu am.
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
---

## Part 4: Jëfandikoo Plugin Yuy

### Étape 1: Test ci lokal

1. Copy plugin bi ci `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Vérifier nekk: button ci sidebar bi am, features yi jëm ci sañ-sañ
4. Test edge cases: Naka la am su terminal bi amul?

### Étape 2: Prepare ngir submission

Fichier folder bi plugin bi am nañu:
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
**Néew `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Kebab-case ID bu njëkk | `"my-awesome-plugin"` |
| `version` | Version semantik | `"1.0.0"` |
| `description` | Njàngum bu njëkk | `"Monitors nginx access logs in real-time"` |
| `author` | Sa tur | `"John Doe"` |
| `main` | Bopp bi | `"index.js"` |

**Fields bu amul solo:**

| Field | Description |
|-------|-------------|
| `license` | Noonu xibaar (MIT am na solo) |
| `keywords` | Array bu tags yu jëfandikoo | 
| `soom.minVersion` | WIA SOOM version bu am solo |

### Etap 3: Tànn ci Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** sa plugin ci `plugins/{sa-plugin-naam}/`
3. **Submit** a Pull Request

### Etap 4: Njàngum ak jàmm

Njàngum ci plugin bu nekk:

- **Sécurité** — amul API yu dañs (gisee [Security Rules](#security-rules))
- **Qualité** — doo am? Code bi dafa neex?
- **Utilité** — doo jàppale ci jàmm bu nekk?

Benn j��mm:
1. Sa plugin am na ci `registry.json`
2. Benn ZIP bundle am na ci `dist/`
3. Sa plugin am na ci **Plugin Store** ngir WIA SOOM users bu nekk!

---

## Part 5: Njàngum yu am solo

### Security Rules

Njàngum yi **sont**. Plugins yu jàppale ci ñoom dinañu jàmm.

| Rule | Lu tax |
|------|-----|
| **NEVER** jëfandikoo `eval()` walla `new Function()` | Riska ci code injection |
| **NEVER** jëfandikoo `child_process`, `exec()`, `spawn()` | Jëfandikoo `ctx.terminal.send()` ngir commands |
| **NEVER** jëfandikoo external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** jëfandikoo `process.env` | Variables environnement yi am nañu secrets |
| **NEVER** jëfandikoo `require('fs')` ci diggante | Jëfandikoo `ctx.settings` ngir storage, `ctx.sftp` ngir file transfer |
| **NEVER** jëfandikoo npm external packages | JavaScript bu nekk — amul node_modules |
| **MUST** jëfandikoo `ctx.terminal.send()` ngir commands bu remote | Loolu dafa jëfandikoo ci SSH channel bu secure |
| **MUST** jëfandikoo ci `deactivate()` | Fekk listeners, clear intervals |

### Error Handling

Dinañu jëfandikoo ci operations yu riski ci try/catch:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Cleanup ci deactivate()

Soo sa plugin jëfandikoo intervals, listeners, walla subscriptions — fekk leen:
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
### i18n Support

WIA SOOM am na 254 languages. Ngir sa plugin label am nañu jàppale, jëfandikoo njaay bu am solo:
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

## Part 6: Njàngum yu am solo ci bopp

### Njàngum 1: Server Disk Checker

Jëfandikoo `df -h` ci server bu remote ak jox seen bopp/available space ci status bar.
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

### Njàngum 2: TODO Manager

Plugin bu jëfandikoo ci TODO list jëfandikoo settings ngir storage bu am solo ak webview ngir display.

> **Design pattern:** Soo webviews du jëfandikoo ci plugin APIs, plugin bi jëfandikoo "snapshot" approach — dafa jëfandikoo TODOs ci settings, jox leen ci HTML bu read-only, ak jox actions ci sidebar ngir add items. Webview bi dafa am na ci **display** layer, du form interactive.
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

### Njàngum 3: Error Watcher

Njàngum ci output terminal bi ak jox notification soo patterns yu nekk am nañu.
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

## Appendix: Categories & Icons

### Plugin Categories (29)

Jëfandikoo ci `package.json` `keywords` walla bu jëfandikoo ci registry:

| Category | Description |
|----------|-------------|
| `server` | Jëfandikoo bu server bu am solo |
| `devtools` | Jëfandikoo bu jëfandikoo |
| `calculator` | Calculators ak converters |
| `simulator` | Simulators |
| `game` | Gëstu yi ci terminal |
| `business` | Jëfandikoo bu jëfandikoo |
| `security` | Jëfandikoo ak auditing |
| `web` | Jëfandikoo bu web server |
| `education` | Jëfandikoo bu xam-xam |
| `health` | Jëfandikoo bu santé |
| `islamic` | Jëfandikoo bu Islam (waxtu ngëm, etc.) |
| `science` | Jëfandikoo bu xam-xam |
| `quantum` | Jëfandikoo bu quantum computing |
| `ai` | Jëfandikoo bu AI-powered |
| `biotech` | Jëfandikoo bu biotechnology |
| `space` | Jëfandikoo bu space ak astronomy |
| `network` | Jëfandikoo bu réseau |
| `database` | Jëfandikoo bu database |
| `monitoring` | Jëfandikoo bu server monitoring |
| `devops` | DevOps ak CI/CD |
| `utility` | Jëfandikoo bu jëfandikoo |
| `design` | Jëfandikoo bu design |
| `ecommerce` | Jëfandikoo bu e-commerce |
| `automation` | Jëfandikoo bu automation |
| `kpop` | Jëfandikoo bu K-pop |
| `accessibility` | Jëfandikoo bu accessibility |
| `analytics` | Jëfandikoo ak reporting |
| `wia` | Jëfandikoo bu WIA ecosystem |
| `all` | Am ci kategori yi ñu tollu |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Jëfandikoo bu server |
| `shield` | Sécurité |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Jëfandikoo bu terminal |
| `code` | Jëfandikoo |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Nuyoo/monitoring |
| `check-square` | Tasks/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuration |
| `zap` | Automation |
| `globe` | Web/international |

Jëfandikoo ci 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Build something amazing. Share it with the world.</em></p>
<p align="center"><em>— The WIA SOOM Team</em></p>
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
