<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Féyé na plugin na yo na 5 minutes.</strong></p>
<p align="center">Féyé na nzoni ya server, dashboards, na automations — na WIA SOOM.</p>

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

Na plugin "Hello World" oyo ebandaka buton na sidebar. Soko ekliké, ekomi na notification.

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

Restart na app (to toggle na plugin off/on na Settings → Plugins).

Oko mona buton **"Hello World"** na sidebar. Klike yango — okomonisa notification ya succès!

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

Soko `activate(context)` function ebandaka, `context` (to `ctx`) ebandaka na API oyo:
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

Send na command (to data nyonso) na session ya terminal oyo ezali active.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal oyo olingi kokoma na yango |
| `data` | `string` | Command to data oyo olingi kokoma |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscribe na output nyonso oyo ebandaka na session ya terminal. Ekomi na **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal oyo olingi kolanda |
| `callback` | `(data: string) => void` | Ekomaka na chunk nyonso ya output |
| **Returns** | `() => void` | Koma yango soki olingi kokanga kolanda |
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
**Important:** Soki esengeli, zala na unsubscribe function mpe komi yango na `deactivate()` mpo na kolongola memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — SFTP API ezali komonana kasi ezali te na engine ya SFTP ya app. `list()` ezali komona array ya sika, mpe `upload()`/`download()` ezali no-ops. Yango ekokisama na release ya nsima. Na tango oyo, zala na `ctx.terminal.send()` na `scp` to `rsync` commands lokola workaround.

#### `sftp.list(sessionId, path)`

List na ba fichiers na directory ya remote.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Upload na fichier oyo ezali na machine ya local mpo na remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Download na fichier oyo ezali na remote server mpo na machine ya local.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (tango SFTP API ezali te):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Add na buton na sidebar ya WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | ID ya sika (ekomi na plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Texte ya buton oyo ekomi na sidebar |
| `onClick` | `() => void` | Yes | Fonction oyo ebandaka soki buton ekliké |
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
**Icon reference:** Zala na ba icons nyonso oyo ezali na [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Bato bazali na plugins ya kala oyo bazali na ba arguments ya position lokola `addSidebarButton(id, icon, label, onClick)`. API ya officiel ezali na **options object** lokola ekomi na likolo. Zala na style ya object mpo na plugins sika.

#### `ui.openWebview(options)`

Open na popup window na contenu ya HTML oyo ezali sika. Yango ezali ndenge ya kosala ba UIs ya kitoko.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Title ya window |
| `html` | `string` | Contenu ya HTML oyo ekomi na yango |
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
> Tîna [Part 3](#part-3-building-custom-ui-with-webviews) na patterns webview ya bîngâ.

#### `ui.showNotification(type, message)`

Kêlê notification ya toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Style ya notification |
| `message` | `string` | Tîkâ ya kêlê |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Kêlê item ya tîkâ ya bîngâ na status bar ya bîngâ.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID unique ya item ya status |
| `text` | `string` | Tîkâ ya kêlê |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Kêlê storage ya bîngâ

Settings ya plugin bîngâ na `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lîka tîkâ ya bîngâ.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Bîngâ `undefined` sî tîkâ na key o yê.

#### `settings.set(key, value)`

Kêlê tîkâ. Kêlê strings, numbers, booleans, arrays, na objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemple: Mémoriser préférences ya utilisateur**
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

### `ctx.ai` — Intégration ya AI

> **Statut: À venir** — API ya AI e bîngâ mais na yê connectée na Soomy. Bîngâ `{ response: 'AI not yet connected' }`. Intégration complète ya AI e planifiée na libération ya bîngâ.

#### `ai.chat(messages, options?)`

Kêlê messages na assistant ya AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Kêlê UI ya bîngâ na Webviews

API ya `openWebview()` e tîna yê bîngâ dashboard UIs na HTML, CSS, na JavaScript — bîngâ na window ya popup.

> **Limitation importante:** Webviews e **display-only**. Na yê kîlâ na plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Kêlê sidebar buttons na bîngâ ya actions ya utilisateur, na yê `openWebview()` na tîna yê bîngâ ya état actuel. Sî tîna yê features interactives, déclenche-les na sidebar buttons na ré-ouvre le webview na rafraîchir l'affichage.

### Pattern: Commande Terminal → Analyser Sortie → Afficher en HTML

E bîngâ pattern ya plugin ya bîngâ. Tîna yê commande, analyser le résultat, na afficher visuellement.
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
### Pattern: Dashboard Interactif avec Auto-Rafraîchissement
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
### Pattern: Affichage des Paramètres dans un Webview

> **Remarque:** Webviews e display-only — na yê kîlâ na plugin APIs. Kêlê `ctx.settings` na handlers ya sidebar buttons na modifier les paramètres, na yê `openWebview()` na tîna yê état actuel.
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

## Part 4: Publier votre Plugin

### Étape 1: Tester localement

1. Kopya plugin ya bîngâ na `~/.wia-soom/plugins/{your-plugin}/`
2. Redémarrer WIA SOOM
3. Vérifier sî ça fonctionne: bouton ya sidebar e tîna, fonctionnalités e fonctionne correctement
4. Tester les cas limites: que se passe-t-il si aucun terminal n'est connecté ?

### Étape 2: Préparer pour la soumission

Dossier ya plugin ya bîngâ doit contenir:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Ndinganga `package.json` na:**

| Ndinganga | Ndinganga na | Ndinganga na |
|-----------|---------------|---------------|
| `name` | ID ya kebab-case ya moke | `"my-awesome-plugin"` |
| `version` | Ndinganga ya semantiki | `"1.0.0"` |
| `description` | Ndinganga ya moke | `"Monitors nginx access logs in real-time"` |
| `author` | Nkombo na yo | `"John Doe"` |
| `main` | Ndinganga ya mboka | `"index.js"` |

**Ndinganga ya moke:**

| Ndinganga | Ndinganga na |
|-----------|---------------|
| `license` | Ndinganga ya lisensi (MIT eza na mposa) |
| `keywords` | Nguya ya makambo ya etali |
| `soom.minVersion` | Ndinganga ya WIA SOOM ya moke oyo eza na mposa |

### Etape 3: Tindika na Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** plugin na yo na `plugins/{nom-plugin-yo}/`
3. **Tindika** Pull Request

### Etape 4: Koyeba mpe kokanisa

Toko yeba plugin nyonso mpo na:

- **Securite** — te na API ya mabe (tala [Security Rules](#security-rules))
- **Qualite** — eza na mosala? Code eza na malamu?
- **Utilite** — eza na solusyon ya mabe?

Na kokanisa:
1. Plugin na yo ekozwa na `registry.json`
2. ZIP bundle ekozwa na `dist/`
3. Plugin na yo ekozwa na **Plugin Store** mpo na ba utilisateur nyonso ya WIA SOOM!

---

## Part 5: Malamu na Mosala

### Security Rules

Bato oyo ezali **mposo**. Plugins oyo ekozala na mabe ekozala na kokangama.

| Mabe | Mpo na nini |
|------|-------------|
| **TE** senga `eval()` to `new Function()` | Risque ya code injection |
| **TE** senga `child_process`, `exec()`, `spawn()` | Senga `ctx.terminal.send()` mpo na ba commandes |
| **TE** senga ba URL ya ndenge na ndenge | Exemption: `wiasoom.com` API endpoints |
| **TE** senga `process.env` | Ba variables ya environnement eza na makambo ya mabe |
| **TE** senga `require('fs')` na ndenge ya moke | Senga `ctx.settings` mpo na stockage, `ctx.sftp` mpo na transfert ya fichier |
| **TE** senga ba npm external packages | JavaScript ya solo — te na node_modules |
| **MPO** senga `ctx.terminal.send()` mpo na ba commandes nyonso ya mbali | Oyo ekozwa na SSH ya sekre |
| **MPO** senga kokanga na `deactivate()` | Kanga ba listeners, pusa ba intervals |

### Koyeba Mabe

Tika nyonso na ba operations ya mabe na try/catch:
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
### Koyeba na deactivate()

Soki plugin na yo ekozwa na ba intervals, ba listeners, to ba subscriptions — tika yango:
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

WIA SOOM ekozwa na 254 langues. mpo na kosala label ya plugin na yo eza na mposa, senga ndenge ya moke:
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

## Part 6: Ba Exemple ya Mosala

### Exemple 1: Server Disk Checker

Ekozwa `df -h` na serveur ya mbali mpe ekozala na esika ya kosalela/kolanda na status bar.
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

### Exemple 2: TODO Manager

Plugin oyo ekozwa na lisanga ya TODO na ndenge ya ba settings mpo na stockage ya moke mpe webview mpo na kolakisa.

> **Design pattern:** Lokola ba webviews te ekozwa na API ya plugin, plugin oyo ekozwa na "snapshot" — ekozwa ba TODO na ba settings, ekozala na yango lokola HTML ya te eza na mposa, mpe ekozala na ba actions ya sidebar mpo na kokanga ba items. Webview eza na **display** layer, te na formulaire ya interactive.
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

### Exemple 3: Error Watcher

Ekozala na koyeba output ya terminal mpe ekozwa notification soki ba patterns ya mabe ekozala.
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
| `server` | Gestion générale du serveur |
| `devtools` | Outils de développement |
| `calculator` | Calculatrices et convertisseurs |
| `simulator` | Simulateurs |
| `game` | Jeux de terminal |
| `business` | Outils d'affaires |
| `security` | Sécurité et audit |
| `web` | Gestion de serveur web |
| `education` | Outils éducatifs |
| `health` | Outils liés à la santé |
| `islamic` | Outils islamiques (horaires de prière, etc.) |
| `science` | Outils scientifiques |
| `quantum` | Outils de calcul quantique |
| `ai` | Outils alimentés par l'IA |
| `biotech` | Outils de biotechnologie |
| `space` | Outils liés à l'espace et à l'astronomie |
| `network` | Outils de réseau |
| `database` | Gestion de base de données |
| `monitoring` | Surveillance de serveur |
| `devops` | DevOps et CI/CD |
| `utility` | Utilitaires généraux |
| `design` | Outils de design |
| `ecommerce` | Outils de commerce électronique |
| `automation` | Outils d'automatisation |
| `kpop` | Outils liés au K-pop |
| `accessibility` | Outils d'accessibilité |
| `analytics` | Analytique et reporting |
| `wia` | Outils de l'écosystème WIA |
| `all` | Apparaît dans toutes les catégories |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Gestion de serveur |
| `shield` | Sécurité |
| `database` | Base de données |
| `activity` | Surveillance |
| `terminal` | Outils de terminal |
| `code` | Développement |
| `hard-drive` | Disque/stockage |
| `network` | Réseautage |
| `lock` | Auth/cryptage |
| `eye` | Surveillance/monitoring |
| `check-square` | Tâches/TODO |
| `layout-dashboard` | Tableaux de bord |
| `settings` | Configuration |
| `zap` | Automatisation |
| `globe` | Web/international |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construisez quelque chose d'incroyable. Partagez-le avec le monde.</em></p>
<p align="center"><em>— L'équipe WIA SOOM</em></p>