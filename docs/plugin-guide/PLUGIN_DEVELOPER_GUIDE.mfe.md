<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Gid Devlopman Plugin WIA SOOM</h1>
<p align="center"><strong>Kre enn plugin pou ou dan 5 minit.</strong></p>
<p align="center">Kre enn zouti server pwisan, dashboards, ek automations — direkt dan WIA SOOM.</p>

---

## Table of Contents

- [Part 1: Quick Start — Ou Premie Plugin dan 5 Minits](#part-1-quick-start--ou-premie-plugin-dan-5-minits)
- [Part 2: Referans API Context Plugin](#part-2-referans-api-context-plugin)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Bati UI Personalize avek Webviews](#part-3-bati-ui-personalize-avek-webviews)
- [Part 4: Publie Ou Plugin](#part-4-publie-ou-plugin)
- [Part 5: Meilleur Pratik](#part-5-meilleur-pratik)
- [Part 6: Lexan Reel](#part-6-lexan-reel)
- [Appendice: Kategori & Ikon](#appendice-kategori--ikon)

---

## Part 1: Quick Start — Ou Premie Plugin dan 5 Minits

### Ki ou pou bati

Enn plugin "Hello World" ki azout enn bouton dan sidebar. Kan ou klik, li montre enn notifikasyon.

### Etap 1: Kre folder plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Etap 2: Kre package.json
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
**Bann domenn obligatwar:** `name`, `version`, `description`, `author`, `main`

### Etap 3: Kre index.js
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

Restart aplikasion (ou bien toggle plugin off/on dan Settings → Plugins).

Ou bizin vwar enn **"Hello World"** bouton dan sidebar. Klik li — ou pou vwar enn notifikasyon de sikses!

### Kouma li marse
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

## Part 2: Referans API Context Plugin

Kan ou `activate(context)` fonksion apel, `context` (ou bien `ctx`) fourn sa bann API la:
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

Enn komann (ou bien okenn done) pou voye dan enn sesion terminal aktif.

| Parameter | Type | Deskripsion |
|-----------|------|-------------|
| `sessionId` | `string` | Sesion terminal pou voye |
| `data` | `string` | Komann ou done pou voye |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abon a tou output depi enn sesion terminal. Retourn enn **fonksion dezabonnement**.

| Parameter | Type | Deskripsion |
|-----------|------|-------------|
| `sessionId` | `string` | Sesion terminal pou swiv |
| `callback` | `(data: string) => void` | Apele avek sak chunk de output |
| **Retourn** | `() => void` | Apele sa pou aret ekout |
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
**Importan:** Touzour sove fonksion dezabonnement ek apel li dan `deactivate()` pou evite memwar leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — API SFTP la defini me ankor pa konekte avek moteur SFTP aplikasion. `list()` aktyelman retourn enn aray vid, ek `upload()`/`download()` pa fer anyin. Sa pou fini aplike dan enn lavant. Pou le moman, servi `ctx.terminal.send()` avek `scp` ou `rsync` komann kom enn workaround.

#### `sftp.list(sessionId, path)`

Liste bann fichiers dan enn directory remote.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Voye enn fichier depi lokal machin pou enn server remote.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Telechaz enn fichier depi enn server remote pou lokal machin.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (ziska SFTP API viv):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Azout enn bouton dan sidebar WIA SOOM.

| Option | Type | Obligatwar | Deskripsion |
|--------|------|------------|-------------|
| `id` | `string` | Non | ID unik (par defo li pran nom plugin) |
| `icon` | `string` | Wi | Nom ikon Lucide (par ex., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Wi | Tekst bouton ki montre dan sidebar |
| `onClick` | `() => void` | Wi | Fonksion ki apele kan bouton pe kliké |
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
**Referans ikon:** Browse tou bann ikon ki disponib dan [lucide.dev/icons](https://lucide.dev/icons)

> **Note de konpatibilite:** Kertin ansien plugins servi argument pozisyonel kouma `addSidebarButton(id, icon, label, onClick)`. API ofisyel servi enn **options object** kouma dokumente anler. Touzour servi stil object pou nouvo plugins.

#### `ui.openWebview(options)`

Ouvrir enn fenetre popup avek konteni HTML personnalizé. Sa kouma ou bati UI riche.

| Option | Type | Deskripsion |
|--------|------|-------------|
| `title` | `string` | Tit fenetre |
| `html` | `string` | Konteni HTML total pou rann |
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
> Vinn [Part 3](#part-3-building-custom-ui-with-webviews) pou modèl webview avanse.

#### `ui.showNotification(type, message)`

Montre enn notifikasyon toast.

| Paramètre | Tip | Deskripsyon |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stil notifikasyon |
| `message` | `string` | Tekst pou montre |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ajoute enn item text persistan dan bar status anba.

| Paramètre | Tip | Deskripsyon |
|-----------|------|-------------|
| `id` | `string` | ID inik pou sa item status la |
| `text` | `string` | Tekst pou afichaz |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Depo persistan

Settings plugin la stored permanan dan `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Li enn valè ki sove.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Retourn `undefined` si sa clé la pa exist.

#### `settings.set(key, value)`

Sove enn valè. Sipport string, nomb, boolean, array, ek objè.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Egzanp: Rappel preferans itilizater**
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

### `ctx.ai` — Entegrasyon AI

> **Statut: Vinn Bientôt** — API AI la defini me pa ankor konekte ar Soomy. Aktuellement retourn `{ response: 'AI not yet connected' }`. Entegrasyon total AI planifie pou enn lanné future.

#### `ai.chat(messages, options?)`

Voye mesaj ar AI asistan (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Bâtir enn UI Personnalizé ar Webviews

API `openWebview()` permet ou batir dashboard UIs ar HTML, CSS, ek JavaScript — tou dan enn fenetre popup.

> **Limitasyon inportan:** Webviews sont **display-only**. Zot pa kapav apel tounen dan plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Utiliz bouton sidebar pou tou aksyon itilizater, ek utiliz `openWebview()` pou montre l'état kouran. Si ou bizin fonctionnalités interactives, declenche zot depi bouton sidebar ek ré-ouvre webview pou rafraîchir l'affichage.

### Modèle: Commande Terminal → Analis Output → Montrer dan HTML

Sa se pli komen modèle plugin. Ou fer enn commande, analize rezilta, ek montre li vizyèlman.
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
### Modèle: Dashboard Interactif ar Auto-Rafraîchissement
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
### Modèle: Montrer Settings dan enn Webview

> **Remarque:** Webviews sont display-only — zot pa kapav apel tounen dan plugin APIs. Utiliz `ctx.settings` dan ou bouton sidebar handlers pou modifie settings, ek utiliz `openWebview()` pou montre l'état kouran.
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

## Part 4: Publier Ou Plugin

### Étape 1: Test lokalman

1. Kopi ou plugin dan `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verifye li marse: bouton sidebar aparé, fonctionnalités marse korekteman
4. Teste edge cases: ki arive si okenn terminal pa konekte?

### Étape 2: Prepare pou soumission

Ou dossier plugin bizin kontenir:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | ID in kebab-case ki inik | `"my-awesome-plugin"` |
| `version` | Version semantik | `"1.0.0"` |
| `description` | Deskripsyon an enn lini | `"Monitors nginx access logs in real-time"` |
| `author` | To nom | `"John Doe"` |
| `main` | Pwen d'entré | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | Kalite lisans (MIT rekomande) |
| `keywords` | Lis tag pou rechersh |
| `soom.minVersion` | Minim WIA SOOM version ki neseser |

### Step 3: Soumet a la Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** to plugin dan `plugins/{to-plugin-nom}/`
3. **Submit** enn Pull Request

### Step 4: Revizyon ek aprobatyon

Nou revize sak plugin pou:

- **Sekirite** — pa ena API danzere (regard [Security Rules](#security-rules))
- **Kalite** — eski li marse? Eski kod la prop?
- **Utilite** �� eski li rezoud enn vre problem?

Apre aprobatyon:
1. To plugin i azoute dan `registry.json`
2. Enn ZIP bundle i kree dan `dist/`
3. To plugin i apar dan **Plugin Store** pou tou WIA SOOM itilizater!

---

## Part 5: Meilleur Pratiques

### Security Rules

Sa bann regle la **mandatory**. Bann plugin ki violet sa pou ganny rejete.

| Rule | Why |
|------|-----|
| **NEVER** servi `eval()` ou `new Function()` | Risk kod injection |
| **NEVER** servi `child_process`, `exec()`, `spawn()` | Sere `ctx.terminal.send()` zis pou bann komann |
| **NEVER** fer fetch bann URL ekstern | Eksepsyon: `wiasoom.com` API endpoints |
| **NEVER** aksede `process.env` | Bann variable environnement kapav ena sekré |
| **NEVER** servi `require('fs')` dirèkteman | Sere `ctx.settings` pou stockaz, `ctx.sftp` pou transfè dosye |
| **NEVER** servi npm bann paké ekstern | Zis JavaScript pur — pa ena node_modules |
| **MUST** servi `ctx.terminal.send()` pou tou bann komann remote | Sa pase atraver kanal SSH sekirize |
| **MUST** fer netwayaz dan `deactivate()` | Retir bann listeners, klar intervals |

### Error Handling

Touletan, envelop bann operasyon riske dan try/catch:
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
### Cleanup in deactivate()

Si to plugin kree intervals, listeners, ou subscriptions — netwayaz zot:
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

WIA SOOM i suport 254 lang. Pou fer to plugin label traduisible, servi enn metod senp:
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

I fer `df -h` lor server remote ek montre espas ki servi/disponib dan status bar.
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

Enn plugin ki ger enn lis TODO en utilisant bann settings pou stockaz persistan ek enn webview pou afichaz.

> **Design pattern:** Parski webviews pa kapav apel dirèkteman bann API plugin, sa plugin la servi enn "snapshot" metod — li lir bann TODO dan settings, rann zot kom HTML ki li lis, ek donn aksyon baze lor sidebar pou azout bann item. Webview i enn **display** kouch, pa enn form interaktif.
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

I monitor output terminal ek envoie enn notifikasyon kan bann pattern spesifik i dete.
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

Utiliz sa dan ou `package.json` `keywords` ou kan ou soumet dan registry:

| Kategori | Deskripsyon |
|----------|-------------|
| `server` | Jesyon server jeneral |
| `devtools` | Zouti devlopman |
| `calculator` | Kalkilatè ek konversè |
| `simulator` | Simulateur |
| `game` | Ludi terminal |
| `business` | Zouti biznis |
| `security` | Sekirite ek audit |
| `web` | Jesyon server web |
| `education` | Zouti edikatif |
| `health` | Zouti ki lye ar lasante |
| `islamic` | Zouti islamik (tan priyer, etc.) |
| `science` | Zouti syantifik |
| `quantum` | Zouti pou computing quantum |
| `ai` | Zouti ki powered par AI |
| `biotech` | Zouti biotechnologie |
| `space` | Zouti espas ek astronomie |
| `network` | Zouti rezo |
| `database` | Jesyon baz donn |
| `monitoring` | Monitoring server |
| `devops` | DevOps ek CI/CD |
| `utility` | Utilitaires jeneral |
| `design` | Zouti design |
| `ecommerce` | Zouti e-commerce |
| `automation` | Zouti automatisation |
| `kpop` | Zouti ki lye ar K-pop |
| `accessibility` | Zouti aksesibilite |
| `analytics` | Analytik ek rapor |
| `wia` | Zouti ekosistèm WIA |
| `all` | Apare dan tou kategori |

### Ikon Rekomande (Lucide)

| Non Ikon | Utiliz pou |
|-----------|---------|
| `server` | Jesyon server |
| `shield` | Sekirite |
| `database` | Baz donn |
| `activity` | Monitoring |
| `terminal` | Zouti terminal |
| `code` | Devlopman |
| `hard-drive` | Disk/storage |
| `network` | Rezo |
| `lock` | Auth/ekriptasyon |
| `eye` | Gade/monitoring |
| `check-square` | Taks/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfigurasyon |
| `zap` | Automatisation |
| `globe` | Web/enternasyonal |

Browse tou 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Besoin D' Aide?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Kre enn kitsoz amazin. Partaz li ar lemond.</em></p>
<p align="center"><em>— L'équipe WIA SOOM</em></p>