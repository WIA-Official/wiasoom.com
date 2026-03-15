<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Fanda plugin na yo na miniti 5.</strong></p>
<p align="center">Landa bisika ya makasi ya serve, dashboards, na automations — na kati ya WIA SOOM.</p>

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

### What you'll build

Plugin ya "Hello World" oyo ebandaka buton na sidebar. Ntango eclicki, ekomaka notification.

### Step 1: Create the plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Step 2: Create package.json
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

### Step 3: Create index.js
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
### Step 4: Restart WIA SOOM

Restart the app (to toggle the plugin off/on in Settings → Plugins).

Ozozala na buton **"Hello World"** na sidebar. Clicki yango — ozozala na notification ya succès!

### How it works
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

Ntango `activate(context)` function ebandaka, `context` (to `ctx`) epa na bisika oyo:
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

### `ctx.terminal` — Run commands on remote servers

#### `terminal.send(sessionId, data)`

Send a command (to data nyonso) na session ya terminal oyo ezali active.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal oyo olingi ko sendela |
| `data` | `string` | Command to data oyo olingi ko sendela |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscribe na output nyonso ya session ya terminal. Ekomaka **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal oyo olingi kolanda |
| `callback` | `(data: string) => void` | Ebandaka na chunk nyonso ya output |
| **Returns** | `() => void` | Tanga yango mpo na kokanga kolanda |
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
**Important:** Tika na ko save unsubscribe function mpe tanga yango na `deactivate()` mpo na kokanga memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — SFTP API ezali kolanda kasi ezali te na moteur ya SFTP ya app. `list()` ezali kotalisa array ya mbala te, mpe `upload()`/`download()` ezali no-ops. Yango ekokisama na release oyo ebandaka. Na ntango oyo, tanga `ctx.terminal.send()` na `scp` to `rsync` commands lokola workaround.

#### `sftp.list(sessionId, path)`

Listi ba fichiers na directory ya remote.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Uploadi fichier na local machine to remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Downloadi fichier na remote server to local machine.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (till SFTP API ekokisami):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Addi buton na sidebar ya WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Te | ID ya solo (ekokaka na plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Texte ya buton oyo ekomaka na sidebar |
| `onClick` | `() => void` | Yes | Fonction oyo ebandaka ntango buton eclicki |
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
**Icon reference:** Landa ba icons nyonso oyo ezali na [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Bato mingi ya plugins ya kala balanda ba arguments ya position lokola `addSidebarButton(id, icon, label, onClick)`. API ya officiel ebandaka na **options object** lokola ekomaki likolo. Tika na ko landa style ya object mpo na plugins ya sika.

#### `ui.openWebview(options)`

Landa popup window na contenu HTML ya kitoko. Yango ezali ndenge ya kobongisa UIs ya makasi.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Title ya window |
| `html` | `string` | Contenu HTML ya solo oyo ekozala na render |
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
> Tanga [Part 3](#part-3-building-custom-ui-with-webviews) po na mikanda ya webview ya ntete.

#### `ui.showNotification(type, message)`

Kanga notification ya toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Moko ya notification |
| `message` | `string` | Liyangani ya kanga |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Kanga item ya text ya mabele na status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID ya ntete po na item ya status |
| `text` | `string` | Liyangani ya kanga |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Buka ya ntete

Mikanda ya plugin eza na bisika ya ntete na `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Landa value ya zinga.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Kanga `undefined` soki key eza te.

#### `settings.set(key, value)`

Kanga value. Elandaka strings, numbers, booleans, arrays, mpe objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Mokanda: Kanga preferences ya mosali**
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

> **Status: Eza na manso** — API ya AI eza na ntete kasi eza te na Soomy. Elandaka `{ response: 'AI not yet connected' }`. Integration ya AI eza na manso po na liboso.

#### `ai.chat(messages, options?)`

Tanga messages na AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Kanga UI ya ntete na Webviews

API ya `openWebview()` eleta yo kanga dashboard UIs na HTML, CSS, mpe JavaScript — nyonso na ndako ya popup.

> **Mokanda ya ntina:** Webviews eza **display-only**. Bazali te na kanga API ya plugin (`ctx.settings`, `ctx.terminal`, etc.). Tanga sidebar buttons po na nyonso ya mosali, mpe tanga `openWebview()` po na kanga etat ya sikoyo. Soki olingi biloko ya interaktif, tanga yango na sidebar buttons mpe tanga webview lisusu po na kokanga display.

### Pattern: Terminal Command → Parse Output → Show in HTML

Yango eza pattern ya plugin ya ntete. Olanda command, parse result, mpe kanga yango na ndenge ya mabele.
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
### Pattern: Kanga Settings na Webview

> **Mokanda:** Webviews eza display-only — bazali te na kanga API ya plugin. Tanga `ctx.settings` na ba handler ya sidebar button yo po na kosala mikanda, mpe tanga `openWebview()` po na kanga etat ya sikoyo.
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

## Part 4: Kanga Plugin na yo

### Etape 1: Teste na ndako

1. Kopi plugin na yo na `~/.wia-soom/plugins/{your-plugin}/`
2. Tanga WIA SOOM lisusu
3. Tanga soki eza na manso: sidebar button eza, biloko eza na manso
4. Teste ba edge cases: nini ekoya soki terminal eza te?

### Etape 2: Tanga po na submission

Folder ya plugin na yo esengeli kozala na:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Mavita ya `package.json` eza na:**

| Mavita | Nkombo | Moko na moko |
|--------|--------|--------------|
| `name` | ID ya kebab-case ya solo | `"my-awesome-plugin"` |
| `version` | Version ya semantiki | `"1.0.0"` |
| `description` | Nkombo moko | `"Monitors nginx access logs in real-time"` |
| `author` | Nkombo na yo | `"John Doe"` |
| `main` | Ebandeli | `"index.js"` |

**Mavita ya sika:**

| Mavita | Nkombo |
|--------|--------|
| `license` | Nkombo ya lisensi (MIT eza na malamu) |
| `keywords` | Lisanga ya makomi ya etalage |
| `soom.minVersion` | Version ya WIA SOOM ya sika oyo ezozali na ntina |

### Etape 3: Tinda na Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Kokota** plugin na yo na `plugins/{your-plugin-name}/`
3. **Tinda** Pull Request

### Etape 4: Kokanga mpe kokanisa

Tosala kokanga na plugin nyonso mpo na:

- **Sekurite** — te na API ya mabe (tala [Mavita ya Sekurite](#security-rules))
- **Quality** — eza na mosala? Nganga eza na malamu?
- **Utilité** — eza na solisano ya mabe?

Ntango okangaki:
1. Plugin na yo ekozala na `registry.json`
2. Bundle ya ZIP ekozala na `dist/`
3. Plugin na yo ekozala na **Plugin Store** mpo na ba utilisateur nyonso ya WIA SOOM!

---

## Part 5: Mibeko ya Malamu

### Mavita ya Sekurite

Mavita oyo eza **maboko**. Plugins oyo ebotaka yango ekozala na kokanga.

| Mavita | Mpo na nini |
|--------|-------------|
| **TE** salela `eval()` to `new Function()` | Riski ya code injection |
| **TE** salela `child_process`, `exec()`, `spawn()` | Salela kaka `ctx.terminal.send()` mpo na ba commandes |
| **TE** salela ba URL ya libanda | Excepción: `wiasoom.com` API endpoints |
| **TE** salela `process.env` | Ba variable ya environnement ekoki kozala na ba secrets |
| **TE** salela `require('fs')` na nzela ya mabe | Salela `ctx.settings` mpo na stockage, `ctx.sftp` mpo na transfert ya fichier |
| **TE** salela ba packages ya npm ya libanda | JavaScript ya solo kaka — te na node_modules |
| **SALA** salela `ctx.terminal.send()` mpo na ba commandes nyonso ya libanda | Oyo ekozwa na SSH channel ya sekurite |
| **SALA** salela `deactivate()` mpo na kokanga | Kanga ba listeners, salisa ba intervals |

### Kokanga Makambo ya Mabe

Soki olingi kokanga ba opérations ya riski, salela try/catch:
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
### Kokanga na deactivate()

Soki plugin na yo ebandaka ba intervals, ba listeners, to ba subscriptions — kokanga yango:
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

WIA SOOM eza na mboka 254. mpo na kokanga label ya plugin na yo, salela nzela ya malamu:
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

## Part 6: Mibeko ya Mabele

### Mibeko 1: Server Disk Checker

Ebandeli `df -h` na serveur ya libanda mpe ekomisa esika oyo esalemi/ezalaka na status bar.
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

### Mibeko 2: TODO Manager

Plugin oyo ebandeli TODO list na salaka na ba settings mpo na stockage ya mabele mpe webview mpo na kolakisa.

> **Design pattern:** Mpo na ba webviews oyo te ekoki kokota na API ya plugin, plugin oyo esalaka na nzela ya "snapshot" — ebandeli TODOs na ba settings, ekomisa yango lokola HTML ya kolanda, mpe ebandeli ba actions ya sidebar mpo na kokota ba items. Webview eza na **layer** ya kolakisa, te na fomu ya interaktif.
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

### Mibeko 3: Error Watcher

Emonisi output ya terminal mpe ebandeli notification ntango ba patterns ya mabe ekomi.
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
| `server` | Mangement ya server ya ntalu |
| `devtools` | Ntools ya développement |
| `calculator` | Ncalculators na converters |
| `simulator` | Nsimulators |
| `game` | Ngame ya terminal |
| `business` | Ntools ya business |
| `security` | Nsecurity na auditing |
| `web` | Mangement ya web server |
| `education` | Ntools ya éducation |
| `health` | Ntools ya santé |
| `islamic` | Ntools ya islam (ngolo ya nsuka, etc.) |
| `science` | Ntools ya science |
| `quantum` | Ntools ya quantum computing |
| `ai` | Ntools ya AI-powered |
| `biotech` | Ntools ya biotechnology |
| `space` | Ntools ya space na astronomy |
| `network` | Ntools ya réseau |
| `database` | Mangement ya database |
| `monitoring` | Monitoring ya server |
| `devops` | DevOps na CI/CD |
| `utility` | Nutilities ya ntalu |
| `design` | Ntools ya design |
| `ecommerce` | Ntools ya e-commerce |
| `automation` | Ntools ya automation |
| `kpop` | Ntools ya K-pop |
| `accessibility` | Ntools ya accessibility |
| `analytics` | Nanalytics na reporting |
| `wia` | Ntools ya WIA ecosystem |
| `all` | Eza na biloko nyonso |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Mangement ya server |
| `shield` | Security |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Ntools ya terminal |
| `code` | Développement |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Kotalela/monitoring |
| `check-square` | Biloko/TODO |
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