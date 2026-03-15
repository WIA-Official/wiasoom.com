<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guida per Desenvolvedurs da Plugins WIA SOOM</h1>
<p align="center"><strong>Construì vossa propia plugin en 5 minutas.</strong></p>
<p align="center">Crea instruments da server puissants, dashboards e automaziuns — direct en WIA SOOM.</p>

---

## Index

- [Part 1: Start Svelt — Vossa Prima Plugin en 5 Minutas](#part-1-start-svelt--vossa-prima-plugin-en-5-minutas)
- [Part 2: Referenza API Context da Plugin](#part-2-referenza-api-context-da-plugin)
  - [ctx.terminal](#ctxterminal--exequar-commands-supra-servers-remots)
  - [ctx.sftp](#ctxsftp--transfer da files)
  - [ctx.ui](#ctxui--interface d'utilisader)
  - [ctx.settings](#ctxsettings--storage-persistent)
  - [ctx.ai](#ctxai--integraziun-ai)
- [Part 3: Construir UI Custom cun Webviews](#part-3-construir-ui-custom-cun-webviews)
- [Part 4: Publitgar Vossa Plugin](#part-4-publitgar-vossa-plugin)
- [Part 5: Best Practices](#part-5-best-practices)
- [Part 6: Exemplars dal Mund Real](#part-6-exemplars-dal-mund-real)
- [Appendix: Categorias & Icons](#appendix-categorias--icons)

---

## Part 1: Start Svelt — Vossa Prima Plugin en 5 Minutas

### Tge che vus construì

In plugin "Hello World" che aggiunta in buttun a la sidebar. Quandu è cliccà, el mussa ina notificaziun.

### Pass 1: Crea il folder da plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Pass 2: Crea package.json
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
**Fields necessaris:** `name`, `version`, `description`, `author`, `main`

### Pass 3: Crea index.js
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
### Pass 4: Restart WIA SOOM

Restart la app (u toggla la plugin off/on en Settings → Plugins).

Vus duvrì vesair in **"Hello World"** buttun en la sidebar. Cliccal — vus duvrì vesair ina notificaziun da success!

### Co ch'ella funcziuna
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

## Part 2: Referenza API Context da Plugin

Quandu vossa `activate(context)` funcziun è clamada, `context` (u `ctx`) furnescha quests APIs:
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

### `ctx.terminal` — Exequar commands supra servers remots

#### `terminal.send(sessionId, data)`

Send in command (u qualsi data) ad ina session da terminal activa.

| Parameter | Type | Descrizzione |
|-----------|------|--------------|
| `sessionId` | `string` | La session da terminal a la quala enviar |
| `data` | `string` | Il command u la data da enviar |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abuna a tut l'output d'ina session da terminal. Returna ina **funcziun da disabunar**.

| Parameter | Type | Descrizzione |
|-----------|------|--------------|
| `sessionId` | `string` | La session da terminal da guardar |
| `callback` | `(data: string) => void` | Clamada cun mintga chunk d'output |
| **Returns** | `() => void` | Clama quai per stoppar d'ascultar |
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
**Impartant:** Duvrè adina salvar la funcziun da disabunar e clamar ella en `deactivate()` per evitar leaks da memoria.

---

### `ctx.sftp` — Transfer da files

> **Status: Arrivà Svelt** — L'API SFTP è definì, ma anc betg colliada al motor SFTP da l'app. `list()` returna actualmain in array vuid, e `upload()`/`download()` èn no-ops. Quai vegn a vegnir implementà completamain en ina futura publicaziun. Per uss, duvrè `ctx.terminal.send()` cun commands `scp` u `rsync` sco workaround.

#### `sftp.list(sessionId, path)`

Lista files en ina directory remota.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Upload in file da la macchina locala al server remota.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Download in file dal server remota a la macchina locala.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (fin ch'l'API SFTP è live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interface d'utilisader

#### `ui.addSidebarButton(options)`

Aggiunta in buttun a la sidebar da WIA SOOM.

| Option | Type | Necessari | Descrizzione |
|--------|------|-----------|--------------|
| `id` | `string` | Na | ID unic (defaults a nom da plugin) |
| `icon` | `string` | Iè | Nom d'icon Lucide (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Iè | Text dal buttun mussà en la sidebar |
| `onClick` | `() => void` | Iè | Funcziun clamada quandu il buttun è cliccà |
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
**Referenza d'iconas:** Browsa tut las iconas disponiblas a [lucide.dev/icons](https://lucide.dev/icons)

> **Nota da compatibilitad:** Certs plugins vegls duvrà arguments posiziunals sco `addSidebarButton(id, icon, label, onClick)`. L'API ufficiala duvrà in **object d'opziuns** sco documentà sura. Duvrè adina il stil d'object per novas plugins.

#### `ui.openWebview(options)`

Auvra in fenestra popup cun contignì HTML custom. Quai è co ch'vulè construir UIs ric.

| Option | Type | Descrizzione |
|--------|------|--------------|
| `title` | `string` | Titul da la fenestra |
| `html` | `string` | Contignì HTML complet da renderizar |
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
> Vedi [Part 3](#part-3-building-custom-ui-with-webviews) per schemi avanza da webview.

#### `ui.showNotification(type, message)`

Mostra ina notificaziun toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stile da notificaziun |
| `message` | `string` | Text da mostrar |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Agiunta in element da text persistent a la bara da status dal basa.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID unic per quest element da status |
| `text` | `string` | Text da mostrar |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Memoria persistenta

Las settings dal plugin vegnan salvadas permanentamain en `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Legi in valur salvà.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Returns `undefined` sch'il key na exista betg.

#### `settings.set(key, value)`

Salva in valur. Supporta strings, numbers, booleans, arrays, e objects.
��§§CHUNK_SEPARATOR§§§
**Exempel: Regorda las preferenzas da l'utente**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — Integraziun AI

> **Status: Arrivà Ssoon** — L'API AI è definì, ma na è anc betg collada cun Soomy. Actualmain returns `{ response: 'AI not yet connected' }`. L'integraziun completa da l'AI è planificada per in'ulteriura publicaziun.

#### `ai.chat(messages, options?)`

Send messages al assistent AI (Soomy).
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

## Part 3: Construir UI Custom cun Webviews

L'API `openWebview()` permetta da construir UIs da dashboard cun HTML, CSS, e JavaScript — tut en in'fenestra popup.

> **Limitaziun impurtanta:** Las webviews èn **solament per mostrar**. Elas na pon betg sa clamar en las APIs dal plugin (`ctx.settings`, `ctx.terminal`, etc.). Duvra buttuns da sidebar per tut las acziuns da l'utente, e duvra `openWebview()` per mostrar l'estat actual. Sch'has bisogn d'features interactivas, triggers ellas da buttuns da sidebar e re-apri la webview per renewar la visualisaziun.

### Schema: Command da Terminal → Parse Output → Mostrar en HTML

Quest è il schema da plugin il pli cumün. Ti curras in command, parses il resultat, e mostrat el visivamain.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Schema: Dashboard Interactiv cun Auto-Renovaziun
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
### Schema: Mostrar Settings en in Webview

> **Nota:** Las webviews èn solament per mostrar — ellas na pon betg sa clamar en las APIs dal plugin. Duvra `ctx.settings` en tes handlers da buttuns da sidebar per modificar las settings, e duvra `openWebview()` per mostrar l'estat actual.
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

## Part 4: Publitgar Tiu Plugin

### Pass 1: Testar local

1. Copia tes plugin en `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verifitga ch'el funcziuna: buttun da sidebar apparis, features funcziunan correct
4. Testa cas extrem: co che succeda sch'inga terminal è collada?

### Pass 2: Preparar per la submissiun

Tia carpeta dal plugin sto cuntener:
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
**Fields requirids da `package.json`:**

| Campo | Descrizzione | Exempl |
|-------|--------------|--------|
| `name` | ID unich en kebab-case | `"my-awesome-plugin"` |
| `version` | Versiun semantica | `"1.0.0"` |
| `description` | Descrizzione en ina frase | `"Monitors nginx access logs in real-time"` |
| `author` | Tieu num | `"John Doe"` |
| `main` | Punt d'entrada | `"index.js"` |

**Fields opzionali:**

| Campo | Descrizzione |
|-------|--------------|
| `license` | Tip da licenza (MIT recumandà) |
| `keywords` | Array da tags da tschertga |
| `soom.minVersion` | Versiun minima da WIA SOOM necessaria |

### Pass 3: Invia a la registraziun da plugins

1. ****Package** your plugin as a ZIP file
2. **Agiunta** il to plugin a `plugins/{your-plugin-name}/`
3. **Invia** ina Pull Request

### Pass 4: Revisiun e approvaziun

Nus revisain mintga plugin per:

- **Securitad** — naginas APIs periclitantas (vedi [Reglas da Securitad](#security-rules))
- **Qualitad** — funcziuna? È il code cler?
- **Utilitad** — solv il problem real?

Suenter l'approvaziun:
1. Il to plugin vegn aggiuntà a `registry.json`
2. In pacchet ZIP vegn creat en `dist/`
3. Il to plugin appar in la **Plugin Store** per tuts utilizaders da WIA SOOM!

---

## Part 5: Best Practices

### Regulas da Securitad

Questas reglas èn **mandatorias**. Plugins che violan quellas vegnan refusadas.

| Regla | Perché |
|-------|--------|
| **MAI** duvrar `eval()` u `new Function()` | Rischa d'injectar code |
| **MAI** duvrar `child_process`, `exec()`, `spawn()` | Duvrar mo `ctx.terminal.send()` per commandas |
| **MAI** retrair URLs externas | Excepziun: endpoints API da `wiasoom.com` |
| **MAI** accedir a `process.env` | Variablas d'ambient pon cuntegnir segrets |
| **MAI** duvrar `require('fs')` direct | Duvrar `ctx.settings` per guardar, `ctx.sftp` per transferir files |
| **MAI** duvrar pacchets externs npm | Mo JavaScript pur — nagins node_modules |
| **DEVESS** duvrar `ctx.terminal.send()` per tuttas commandas remotas | Quist va tras il canal SSH secur |
| **DEVESS** far pulizia en `deactivate()` | Remover listeners, clear intervals |

### Gestiun d'Erors

Duvra adina in try/catch per operaziuns periculusas:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Pulizia en deactivate()

Sch'il to plugin crea intervals, listeners, u subscriptions — fa pulizia:
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
### Support da i18n

WIA SOOM sustegna 254 linguas. Per far che il label dal to plugin sia traducibel, duvrar in'approch sempla:
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

## Part 6: Exemplars dal Mund Real

### Exempl 1: Checker da Disk Server

Executa `df -h` sin il server remota e mussa il spazi utilisà/disponibel en la barra da status.
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

### Exempl 2: Manager da TODO

In plugin che gestiuna in'list da TODO duvrond settings per guardar persistent e in webview per la visualisaziun.

> **Pattern da design:** Dals webviews na pon betg directamain clamar APIs da plugins, quest plugin duvrà in'approch "snapshot" — el legia ils TODOs da settings, rendra els sco HTML read-only, e furnescha acziuns basadas sin la sidebar per aggiuntar elements. Il webview è in **layer** da visualisaziun, betg in formular interactiv.
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

### Exempl 3: Guardi d'Erors

Monitora l'output dal terminal e sends ina notificaziun cura che patterns specifics vegnan detectads.
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

## Appendice: Categorias & Iconas

### Categorias da Plugin (29)

Utilisez quists en voss `package.json` `keywords` u quanda submittezi al registru:

| Categoria | Descriziun |
|----------|-------------|
| `server` | Gestiun generala dal server |
| `devtools` | Utensils da svilupp |
| `calculator` | Calculaturs e converters |
| `simulator` | Simulator |
| `game` | Gieus da terminal |
| `business` | Utensils da negizi |
| `security` | Segirezza e audit |
| `web` | Gestiun dal server web |
| `education` | Utensils educativs |
| `health` | Utensils relatius a la sanadad |
| `islamic` | Utensils islamics (temp da priera, etc.) |
| `science` | Utensils scientifics |
| `quantum` | Utensils da computaziun quantistica |
| `ai` | Utensils cun AI |
| `biotech` | Utensils da biotecnologia |
| `space` | Utensils da spatium e astronomia |
| `network` | Utensils da ret |
| `database` | Gestiun da basa da datas |
| `monitoring` | Monitoraziun dal server |
| `devops` | DevOps e CI/CD |
| `utility` | Utilitats generala |
| `design` | Utensils da design |
| `ecommerce` | Utensils da e-commerce |
| `automation` | Utensils da automaziun |
| `kpop` | Utensils relatius a K-pop |
| `accessibility` | Utensils da accessibilitad |
| `analytics` | Analitica e rapport |
| `wia` | Utensils da l'ecosistema WIA |
| `all` | Appears in all categories |

### Iconas Recumandadas (Lucide)

| Nomm da l'icona | Utilisar per |
|-----------|---------|
| `server` | Gestiun dal server |
| `shield` | Segirezza |
| `database` | Basa da datas |
| `activity` | Monitoraziun |
| `terminal` | Utensils da terminal |
| `code` | Svilupp |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Guardar/monitorar |
| `check-square` | Tasks/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuraziun |
| `zap` | Automaziun |
| `globe` | Web/internaziun |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Basegnai D'Agid?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construì insatge amazing. Sparter cun il mund.</em></p>
<p align="center"><em>— Il Team WIA SOOM</em></p>
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
