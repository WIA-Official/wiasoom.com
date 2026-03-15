<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Gabay sa Pagbuo ng Plugin</h1>
<p align="center"><strong>Gumawa ng sarili mong plugin sa loob ng 5 minuto.</strong></p>
<p align="center">Lumikha ng makapangyarihang mga tool sa server, dashboard, at automation — sa loob mismo ng WIA SOOM.</p>

---

## Talaan ng Nilalaman

- [Bahagi 1: Mabilis na Pagsisimula — Ang Iyong Unang Plugin sa 5 Minuto](#bahagi-1-mabilis-na-pagsisimula--ang-iyong-unang-plugin-sa-5-minuto)
- [Bahagi 2: Plugin Context API Sanggunian](#bahagi-2-plugin-context-api-sanggunian)
  - [ctx.terminal](#ctxterminal--magpatakbo-ng-mga-utos-sa-mga-remote-server)
  - [ctx.sftp](#ctxsftp--paglipat-ng-file)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Bahagi 3: Pagbuo ng Custom UI gamit ang Webviews](#bahagi-3-pagbuo-ng-custom-ui-gamit-ang-webviews)
- [Bahagi 4: Paglalathala ng Iyong Plugin](#bahagi-4-paglalathala-ng-iyong-plugin)
- [Bahagi 5: Mga Pinakamahusay na Kasanayan](#bahagi-5-mga-pinakamahusay-na-kasanayan)
- [Bahagi 6: Mga Halimbawa sa Tunay na Mundo](#bahagi-6-mga-halimbawa-sa-tunay-na-mundo)
- [Apendiks: Mga Kategorya at Icon](#apendiks-mga-kategorya-at-icon)

---

## Bahagi 1: Mabilis na Pagsisimula — Ang Iyong Unang Plugin sa 5 Minuto

### Ano ang iyong bubuuin

Isang "Hello World" na plugin na nagdaragdag ng button sa sidebar. Kapag na-click, nagpapakita ito ng notification.

### Hakbang 1: Lumikha ng plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Hakbang 2: Lumikha ng package.json
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
**Mga kinakailangang field:** `name`, `version`, `description`, `author`, `main`

### Hakbang 3: Lumikha ng index.js
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
### Hakbang 4: I-restart ang WIA SOOM

I-restart ang app (o i-toggle ang plugin off/on sa Settings → Plugins).

Dapat mong makita ang isang **"Hello World"** na button sa sidebar. I-click ito — makikita mo ang isang success notification!

### Paano ito gumagana
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

## Bahagi 2: Plugin Context API Sanggunian

Kapag tinawag ang iyong `activate(context)` na function, ang `context` (o `ctx`) ay nagbibigay ng mga API na ito:
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

### `ctx.terminal` — Magpatakbo ng mga utos sa mga remote server

#### `terminal.send(sessionId, data)`

Magpadala ng utos (o anumang data) sa isang aktibong terminal session.

| Parameter | Uri | Paglalarawan |
|-----------|------|-------------|
| `sessionId` | `string` | Ang terminal session na padadalhan |
| `data` | `string` | Ang utos o data na ipapadala |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Mag-subscribe sa lahat ng output mula sa isang terminal session. Nagbabalik ng isang **unsubscribe function**.

| Parameter | Uri | Paglalarawan |
|-----------|------|-------------|
| `sessionId` | `string` | Ang terminal session na susubaybayan |
| `callback` | `(data: string) => void` | Tinatawag sa bawat chunk ng output |
| **Nagbabalik** | `() => void` | Tawagan ito upang itigil ang pakikinig |
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
**Mahalaga:** Palaging i-save ang unsubscribe function at tawagan ito sa `deactivate()` upang maiwasan ang memory leaks.

---

### `ctx.sftp` — Paglipat ng file

> **Katayuan: Darating na** — Ang SFTP API ay nakadefine ngunit hindi pa nakakabit sa SFTP engine ng app. Ang `list()` ay kasalukuyang nagbabalik ng isang walang laman na array, at ang `upload()`/`download()` ay walang ginagawa. Ito ay ganap na ipapatupad sa isang hinaharap na release. Sa ngayon, gamitin ang `ctx.terminal.send()` na may mga utos na `scp` o `rsync` bilang workaround.

#### `sftp.list(sessionId, path)`

Ilista ang mga file sa isang remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Mag-upload ng file mula sa lokal na makina patungo sa remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Mag-download ng file mula sa remote server patungo sa lokal na makina.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (hanggang maging live ang SFTP API):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Magdagdag ng button sa sidebar ng WIA SOOM.

| Opsyon | Uri | Kinakailangan | Paglalarawan |
|--------|------|----------|-------------|
| `id` | `string` | Hindi | Natatanging ID (default ay pangalan ng plugin) |
| `icon` | `string` | Oo | Pangalan ng Lucide icon (hal., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Oo | Teksto ng button na ipinapakita sa sidebar |
| `onClick` | `() => void` | Oo | Function na tinatawag kapag na-click ang button |
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
**Sangguniang icon:** Tingnan ang lahat ng available na icon sa [lucide.dev/icons](https://lucide.dev/icons)

> **Tala ng pagkakatugma:** Ang ilang mas lumang plugins ay gumagamit ng positional arguments tulad ng `addSidebarButton(id, icon, label, onClick)`. Ang opisyal na API ay gumagamit ng isang **options object** gaya ng nakadokumento sa itaas. Palaging gamitin ang object style para sa mga bagong plugin.

#### `ui.openWebview(options)`

Buksan ang isang popup window na may custom HTML content. Ganito mo binubuo ang mayamang UIs.

| Opsyon | Uri | Paglalarawan |
|--------|------|-------------|
| `title` | `string` | Pamagat ng window |
| `html` | `string` | Buong HTML content na ipapakita |
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
> Tingnan ang [Bahagi 3](#part-3-building-custom-ui-with-webviews) para sa mga advanced na pattern ng webview.

#### `ui.showNotification(type, message)`

Ipakita ang isang toast notification.

| Parameter | Uri | Paglalarawan |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Estilo ng notification |
| `message` | `string` | Teksto na ipapakita |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Magdagdag ng isang persistent na text item sa ibabang status bar.

| Parameter | Uri | Paglalarawan |
|-----------|------|-------------|
| `id` | `string` | Natatanging ID para sa status item na ito |
| `text` | `string` | Teksto na ipapakita |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Persistent storage

Ang mga setting ng plugin ay permanenteng nakaimbak sa `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Basahin ang isang na-save na halaga.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Nagbabalik ng `undefined` kung ang key ay hindi umiiral.

#### `settings.set(key, value)`

I-save ang isang halaga. Sinusuportahan ang mga string, numero, boolean, arrays, at mga object.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Halimbawa: Tandaan ang mga kagustuhan ng gumagamit**
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

> **Katayuan: Darating na** — Ang AI API ay nakadefine ngunit hindi pa nakakonekta sa Soomy. Sa kasalukuyan ay nagbabalik ng `{ response: 'AI not yet connected' }`. Ang buong integrasyon ng AI ay nakatakdang ilabas sa hinaharap.

#### `ai.chat(messages, options?)`

Magpadala ng mga mensahe sa AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Bahagi 3: Paggawa ng Custom UI gamit ang Webviews

Ang `openWebview()` API ay nagpapahintulot sa iyo na bumuo ng mga dashboard UI gamit ang HTML, CSS, at JavaScript — lahat sa loob ng isang popup window.

> **Mahalagang limitasyon:** Ang mga webview ay **display-only**. Hindi sila makatawag pabalik sa mga plugin API (`ctx.settings`, `ctx.terminal`, atbp.). Gumamit ng mga sidebar button para sa lahat ng aksyon ng gumagamit, at gamitin ang `openWebview()` upang ipakita ang kasalukuyang estado. Kung kailangan mo ng mga interactive na tampok, i-trigger ang mga ito mula sa mga sidebar button at muling buksan ang webview upang i-refresh ang display.

### Pattern: Terminal Command → Parse Output → Ipakita sa HTML

Ito ang pinaka-karaniwang pattern ng plugin. Nagpapatakbo ka ng isang command, pinoproseso ang resulta, at ipinapakita ito nang biswal.
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
### Pattern: Interactive Dashboard na may Auto-Refresh
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
### Pattern: Ipinapakita ang Mga Setting sa isang Webview

> **Tandaan:** Ang mga webview ay display-only — hindi sila makatawag pabalik sa mga plugin API. Gumamit ng `ctx.settings` sa iyong mga sidebar button handlers upang baguhin ang mga setting, at gamitin ang `openWebview()` upang ipakita ang kasalukuyang estado.
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

## Bahagi 4: Paglalathala ng Iyong Plugin

### Hakbang 1: Subukan nang lokal

1. Kopyahin ang iyong plugin sa `~/.wia-soom/plugins/{your-plugin}/`
2. I-restart ang WIA SOOM
3. Tiyakin na ito ay gumagana: lumilitaw ang sidebar button, tama ang mga tampok
4. Subukan ang mga edge case: ano ang mangyayari kung walang terminal na nakakonekta?

### Hakbang 2: Maghanda para sa pagsusumite

Dapat maglaman ang iyong folder ng plugin ng:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Kinakailangang mga patlang sa `package.json`:**

| Patlang | Paglalarawan | Halimbawa |
|---------|--------------|-----------|
| `name` | Natatanging kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic na bersyon | `"1.0.0"` |
| `description` | Isang-linang paglalarawan | `"Monitors nginx access logs in real-time"` |
| `author` | Iyong pangalan | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Mga opsyonal na patlang:**

| Patlang | Paglalarawan |
|---------|--------------|
| `license` | Uri ng lisensya (MIT inirerekomenda) |
| `keywords` | Array ng mga search tag |
| `soom.minVersion` | Minimum na bersyon ng WIA SOOM na kinakailangan |

### Hakbang 3: Isumite sa Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Idagdag** ang iyong plugin sa `plugins/{your-plugin-name}/`
3. **Isumite** ang isang Pull Request

### Hakbang 4: Pagsusuri at pag-apruba

Sinasuri namin ang bawat plugin para sa:

- **Seguridad** — walang mapanganib na APIs (tingnan ang [Mga Panuntunan sa Seguridad](#security-rules))
- **Kalidad** — ito ba ay gumagana? Malinis ba ang code?
- **Kahalagahan** — ito ba ay nakakasolusyon sa isang tunay na problema?

Pagkatapos ng pag-apruba:
1. Ang iyong plugin ay idinadagdag sa `registry.json`
2. Isang ZIP bundle ang nilikha sa `dist/`
3. Ang iyong plugin ay lumalabas sa **Plugin Store** para sa lahat ng gumagamit ng WIA SOOM!

---

## Bahagi 5: Mga Pinakamahusay na Kasanayan

### Mga Panuntunan sa Seguridad

Ang mga panuntunang ito ay **mandatory**. Ang mga plugin na lumalabag dito ay tatanggihan.

| Panuntunan | Bakit |
|------------|-------|
| **HINDI** kailanman gumamit ng `eval()` o `new Function()` | Panganib ng code injection |
| **HINDI** kailanman gumamit ng `child_process`, `exec()`, `spawn()` | Gumamit lamang ng `ctx.terminal.send()` para sa mga utos |
| **HINDI** kailanman kumuha ng mga panlabas na URL | Eksepsyon: `wiasoom.com` API endpoints |
| **HINDI** kailanman ma-access ang `process.env` | Maaaring maglaman ng mga lihim ang mga environment variable |
| **HINDI** kailanman gumamit ng `require('fs')` nang direkta | Gumamit ng `ctx.settings` para sa imbakan, `ctx.sftp` para sa paglilipat ng file |
| **HINDI** kailanman gumamit ng mga panlabas na npm package | Purong JavaScript lamang — walang node_modules |
| **DAPAT** gumamit ng `ctx.terminal.send()` para sa lahat ng remote na utos | Dumadaan ito sa secure na SSH channel |
| **DAPAT** linisin sa `deactivate()` | Alisin ang mga listener, linisin ang mga interval |

### Pag-handle ng Error

Palaging balutin ang mga mapanganib na operasyon sa try/catch:
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
### Paglilinis sa deactivate()

Kung ang iyong plugin ay lumilikha ng mga interval, listener, o subscription — linisin ang mga ito:
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

Sinusuportahan ng WIA SOOM ang 254 na wika. Upang gawing maisasalin ang label ng iyong plugin, gumamit ng simpleng diskarte:
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

## Bahagi 6: Mga Tunay na Halimbawa

### Halimbawa 1: Server Disk Checker

Nagpapatakbo ng `df -h` sa remote server at nagpapakita ng ginamit/magagamit na espasyo sa status bar.
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

### Halimbawa 2: TODO Manager

Isang plugin na namamahala ng isang TODO list gamit ang mga setting para sa persistent storage at isang webview para sa pagpapakita.

> **Disenyo ng pattern:** Dahil ang mga webview ay hindi direktang makatawag sa mga API ng plugin, gumagamit ang plugin na ito ng "snapshot" na diskarte — binabasa nito ang mga TODO mula sa mga setting, inilalarawan ang mga ito bilang read-only HTML, at nagbibigay ng mga aksyon batay sa sidebar para sa pagdaragdag ng mga item. Ang webview ay isang **display** layer, hindi isang interactive na form.
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

### Halimbawa 3: Error Watcher

Nagmamanman ng output ng terminal at nagpapadala ng notification kapag may natukoy na tiyak na mga pattern.
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

## Appendix: Mga Kategorya at Icon

### Mga Kategorya ng Plugin (29)

Gamitin ang mga ito sa iyong `package.json` `keywords` o kapag nagsusumite sa registry:

| Kategorya | Paglalarawan |
|-----------|--------------|
| `server` | Pangkalahatang pamamahala ng server |
| `devtools` | Mga tool sa pag-unlad |
| `calculator` | Mga calculator at converter |
| `simulator` | Mga simulator |
| `game` | Mga laro sa terminal |
| `business` | Mga tool sa negosyo |
| `security` | Seguridad at auditing |
| `web` | Pamamahala ng web server |
| `education` | Mga tool sa edukasyon |
| `health` | Mga tool na may kaugnayan sa kalusugan |
| `islamic` | Mga tool na Islamic (mga oras ng dasal, atbp.) |
| `science` | Mga tool sa agham |
| `quantum` | Mga tool sa quantum computing |
| `ai` | Mga tool na pinapagana ng AI |
| `biotech` | Mga tool sa biotechnology |
| `space` | Mga tool sa espasyo at astronomiya |
| `network` | Mga tool sa network |
| `database` | Pamamahala ng database |
| `monitoring` | Pagsubaybay sa server |
| `devops` | DevOps at CI/CD |
| `utility` | Pangkalahatang utilities |
| `design` | Mga tool sa disenyo |
| `ecommerce` | Mga tool sa e-commerce |
| `automation` | Mga tool sa automation |
| `kpop` | Mga tool na may kaugnayan sa K-pop |
| `accessibility` | Mga tool sa accessibility |
| `analytics` | Analytics at pag-uulat |
| `wia` | Mga tool sa ekosistema ng WIA |
| `all` | Lumalabas sa lahat ng kategorya |

### Inirekumendang Icon (Lucide)

| Pangalan ng Icon | Gamit para sa |
|------------------|---------------|
| `server` | Pamamahala ng server |
| `shield` | Seguridad |
| `database` | Database |
| `activity` | Pagsubaybay |
| `terminal` | Mga tool sa terminal |
| `code` | Pag-unlad |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Pagsubaybay/pagtingin |
| `check-square` | Mga Gawain/TODO |
| `layout-dashboard` | Mga dashboard |
| `settings` | Konfigurasyon |
| `zap` | Automation |
| `globe` | Web/internasyonal |

Tingnan ang lahat ng 1,500+ icon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Kailangan ng Tulong?

- **Mga Isyu sa GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Mga Isyu sa Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Mga Halimbawa ng Plugin:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Gumawa ng isang kamangha-manghang bagay. Ibahagi ito sa mundo.</em></p>
<p align="center"><em>— Ang Koponan ng WIA SOOM</em></p>