<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Bilongim yu own plugin long 5 minit.</strong></p>
<p align="center">Mekim strongpela server tools, dashboards, na automations — long insait WIA SOOM.</p>

---

## Table of Contents

- [Part 1: Quick Start — Yu Fasta Plugin long 5 Minits](#part-1-quick-start--yu-fasta-plugin-long-5-minits)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Building Custom UI with Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Publishing Yu Plugin](#part-4-publishing-yu-plugin)
- [Part 5: Best Practices](#part-5-best-practices)
- [Part 6: Real-World Examples](#part-6-real-world-examples)
- [Appendix: Categories & Icons](#appendix-categories--icons)

---

## Part 1: Quick Start — Yu Fasta Plugin long 5 Minits

### Wanem yu bai mekim

Wanpela "Hello World" plugin we i addim wanpela button long sidebar. Wanem yu klikim, bai i soim wanpela notification.

### Step 1: Mekim plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Step 2: Mekim package.json
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
**Required fields:** `name`, `version`, `description`, `author`, `main`

### Step 3: Mekim index.js
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
### Step 4: Restartim WIA SOOM

Restartim app (o toggle plugin off/on long Settings → Plugins).

Yu mas lukim wanpela **"Hello World"** button long sidebar. Klikim — yu bai lukim wanpela success notification!

### Hau em wok
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

Wanem yu `activate(context)` function i kolim, `context` (o `ctx`) i givim ol API olsem:
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

### `ctx.terminal` — Run commands long remote servers

#### `terminal.send(sessionId, data)`

Sendim wanpela command (o wanpela data) long wanpela active terminal session.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal session we yu bai sendim long |
| `data` | `string` | Command o data we yu bai sendim |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscribe long olgeta output long wanpela terminal session. I returnim wanpela **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal session we yu bai lukim |
| `callback` | `(data: string) => void` | I kolim long olgeta chunk bilong output |
| **Returns** | `() => void` | Kolim dispela long stopim listen |
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
**Important:** Always saveim unsubscribe function na kolim em long `deactivate()` long preventim memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: I Kam Soon** — SFTP API i definim tasol i no yet wired long app's SFTP engine. `list()` i returnim wanpela empty array, na `upload()`/`download()` i no wok. Dispela bai kamap fully implement long wanpela future release. Long nau, yusim `ctx.terminal.send()` wantaim `scp` o `rsync` commands olsem wanpela workaround.

#### `sftp.list(sessionId, path)`

Listim ol faile long wanpela remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Uploadim wanpela faile long local machine i go long remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Downloadim wanpela faile long remote server i go long local machine.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (til SFTP API i live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Addim wanpela button long WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (defaults long plugin name) |
| `icon` | `string` | Yes | Lucide icon name (eg, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Button text we i soim long sidebar |
| `onClick` | `() => void` | Yes | Function we i kolim long taim button i klikim |
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
**Icon reference:** Lukim olgeta available icons long [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Samting ol auldala plugins yusim positional arguments olsem `addSidebarButton(id, icon, label, onClick)`. Olsem na, official API yusim wanpela **options object** olsem i makim antap. Always yusim object style long niu plugins.

#### `ui.openWebview(options)`

Openim wanpela popup window wantaim custom HTML content. Dispela em hau yu buildim rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content we yu bai renderim |
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
> Lukim [Part 3](#part-3-building-custom-ui-with-webviews) long ol webview patten bilong ol bikpela samting.

#### `ui.showNotification(type, message)`

Soim wanpela toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stail bilong notification |
| `message` | `string` | Tok bilong soim |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Addim wanpela persistent text item long daun bilong status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique ID bilong dispela status item |
| `text` | `string` | Tok bilong soim |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Persistent storage

Ol plugin settings i stap long permanent long `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Redim wanpela saved value.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
I givim `undefined` sapos key i no stap.

#### `settings.set(key, value)`

Saveim wanpela value. I sapotim strings, numbers, booleans, arrays, na objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Example: Remember user preferences**
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

> **Status: I kamap long bihain** — AI API i stap, tasol i no yet i konnekt long Soomy. Nau i givim `{ response: 'AI not yet connected' }`. Full AI integration i planim long wanpela futur release.

#### `ai.chat(messages, options?)`

Sendim ol message long AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Building Custom UI with Webviews

Dispela `openWebview()` API i givim yu ol samting bilong bildim dashboard UIs wantaim HTML, CSS, na JavaScript — olgeta insait long wanpela popup window.

> **Importen limitation:** Ol webviews i **display-only**. Ol i no inap kolim bek long ol plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Yusim ol sidebar buttons long olgeta user actions, na yusim `openWebview()` long soim current state. Sapos yu nidim ol interactive features, triggerim ol long ol sidebar buttons na reopenim webview long refreshim display.

### Pattern: Terminal Command → Parse Output → Show in HTML

Dispela em i most common plugin pattern. Yu ronim wanpela command, parseim result, na soim long visual.
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
### Pattern: Interactive Dashboard with Auto-Refresh
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
### Pattern: Displaying Settings in a Webview

> **Nots:** Ol webviews i display-only — ol i no inap kolim bek long ol plugin APIs. Yusim `ctx.settings` long ol sidebar button handlers bilong modifyim settings, na yusim `openWebview()` long soim current state.
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

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Kopim yu plugin i go long `~/.wia-soom/plugins/{your-plugin}/`
2. Restartim WIA SOOM
3. Verifai em i wok: sidebar button i kamap, ol features i wok gut
4. Testim ol edge cases: wanem i hapen sapos nogat terminal i konnekt?

### Step 2: Prepare for submission

Yu plugin folder i mas contain:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Olsem `package.json` filds:**

| Fild | Diskripsin | Eksampel |
|-------|-------------|---------|
| `name` | Unik kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantik version | `"1.0.0"` |
| `description` | Wanpela lain diskripsin | `"Monitors nginx access logs in real-time"` |
| `author` | Yopela nem | `"John Doe"` |
| `main` | Enti point | `"index.js"` |

**Optional filds:**

| Fild | Diskripsin |
|-------|-------------|
| `license` | Laisen tipe (MIT i rekomendim) |
| `keywords` | Array bilong searh tags |
| `soom.minVersion` | Minimum WIA SOOM version i require |

### Step 3: Subit long Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** yupla plugin long `plugins/{your-plugin-name}/`
3. **Submit** wanpela Pull Request

### Step 4: Review na approval

Mipela lukim olgeta plugin long:

- **Security** — nogat danis APIs (lukim [Security Rules](#security-rules))
- **Quality** — em i wok? Ol kode i klia?
- **Usefulness** — em i solvim wanpela tru problem?

Bihain long approval:
1. Yupla plugin i addim long `registry.json`
2. Wanpela ZIP bundle i kreitim long `dist/`
3. Yupla plugin i kamap long **Plugin Store** bilong olgeta WIA SOOM yusers!

---

## Part 5: Best Practices

### Security Rules

Ol dispela rul i **mandatory**. Ol plugin we i brekim ol dispela bai i no acceptim.

| Rul | Why |
|------|-----|
| **NEVER** yusim `eval()` o `new Function()` | Risk bilong kode injection |
| **NEVER** yusim `child_process`, `exec()`, `spawn()` | Yusim tasol `ctx.terminal.send()` bilong komands |
| **NEVER** fetchim external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** accessim `process.env` | Ol environment variables inap containim secrets |
| **NEVER** yusim `require('fs')` direkt | Yusim `ctx.settings` bilong storage, `ctx.sftp` bilong file transfer |
| **NEVER** yusim npm external packages | Pure JavaScript tasol — nogat node_modules |
| **MUST** yusim `ctx.terminal.send()` bilong olgeta remote komands | Dispela i go long secure SSH channel |
| **MUST** klinim long `deactivate()` | Removeim listeners, klinim intervals |

### Error Handling

Olsem na, alwe i wrapim ol risky operations long try/catch:
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
### Cleanup long deactivate()

Sapos yupla plugin i kreitim intervals, listeners, o subscriptions — klinim ol:
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
### i18n Support

WIA SOOM i suportim 254 languages. Bilong mekim yupla plugin label i ken translit, yusim wanpela simpl approach:
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

## Part 6: Real-World Examples

### Example 1: Server Disk Checker

I ranim `df -h` long remote server na i soim used/available space long status bar.
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

### Example 2: TODO Manager

Wanpela plugin we i manejim wanpela TODO list yusim settings bilong persistent storage na wanpela webview bilong display.

> **Design pattern:** Sapos webviews i no inap kolim plugin APIs direkt, dispela plugin i yusim wanpela "snapshot" approach — em i ridim TODOs long settings, i renderim ol olsem read-only HTML, na i providim sidebar-based actions bilong addim items. Dispela webview i wanpela **display** layer, no wanpela interactive form.
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

I monitim terminal output na i sendim wanpela notification sapos ol spesifik patterns i lukim.
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

### Plugin Kategori (29)

Yusim dispela long yu `package.json` `keywords` o taim yu submit long registry:

| Kategori | Diskripsen |
|----------|-------------|
| `server` | General server management |
| `devtools` | Development tools |
| `calculator` | Calculators na converters |
| `simulator` | Simulators |
| `game` | Terminal games |
| `business` | Business tools |
| `security` | Security na auditing |
| `web` | Web server management |
| `education` | Educational tools |
| `health` | Health-related tools |
| `islamic` | Islamic tools (prayer times, etc.) |
| `science` | Scientific tools |
| `quantum` | Quantum computing tools |
| `ai` | AI-powered tools |
| `biotech` | Biotechnology tools |
| `space` | Space na astronomy tools |
| `network` | Network tools |
| `database` | Database management |
| `monitoring` | Server monitoring |
| `devops` | DevOps na CI/CD |
| `utility` | General utilities |
| `design` | Design tools |
| `ecommerce` | E-commerce tools |
| `automation` | Automation tools |
| `kpop` | K-pop related tools |
| `accessibility` | Accessibility tools |
| `analytics` | Analytics na reporting |
| `wia` | WIA ecosystem tools |
| `all` | I stap long olgeta kategori |

### Recommended Ikon (Lucide)

| Ikon Namb | Yusim long |
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

Lukim olgeta 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Yu nidim help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Build something amazing. Share it with the world.</em></p>
<p align="center"><em>— The WIA SOOM Team</em></p>