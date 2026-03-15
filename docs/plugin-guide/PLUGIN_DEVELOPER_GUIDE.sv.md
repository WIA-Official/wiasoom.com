<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Utvecklarhandbok</h1>
<p align="center"><strong>Bygg din egen plugin på 5 minuter.</strong></p>
<p align="center">Skapa kraftfulla serververktyg, instrumentpaneler och automatiseringar — direkt i WIA SOOM.</p>

---

## Innehållsförteckning

- [Del 1: Snabbstart — Din Första Plugin på 5 Minuter](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Del 2: Plugin Kontext API Referens](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Del 3: Bygga Anpassad UI med Webviews](#part-3-building-custom-ui-with-webviews)
- [Del 4: Publicera Din Plugin](#part-4-publishing-your-plugin)
- [Del 5: Bästa Praxis](#part-5-best-practices)
- [Del 6: Verkliga Exempel](#part-6-real-world-examples)
- [Bilaga: Kategorier & Ikoner](#appendix-categories--icons)

---

## Del 1: Snabbstart — Din Första Plugin på 5 Minuter

### Vad du kommer att bygga

En "Hello World" plugin som lägger till en knapp i sidofältet. När den klickas på, visas en notifikation.

### Steg 1: Skapa plugin-mappen
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Steg 2: Skapa package.json
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
**Obligatoriska fält:** `name`, `version`, `description`, `author`, `main`

### Steg 3: Skapa index.js
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
### Steg 4: Starta om WIA SOOM

Starta om appen (eller växla pluginen av/på i Inställningar → Plugins).

Du bör se en **"Hello World"** knapp i sidofältet. Klicka på den — du kommer att se en framgångsnotifikation!

### Hur det fungerar
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

## Del 2: Plugin Kontext API Referens

När din `activate(context)` funktion anropas, tillhandahåller `context` (eller `ctx`) dessa API:er:
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

### `ctx.terminal` — Kör kommandon på fjärrservrar

#### `terminal.send(sessionId, data)`

Skicka ett kommando (eller vilken data som helst) till en aktiv terminalsession.

| Parameter | Typ | Beskrivning |
|-----------|------|-------------|
| `sessionId` | `string` | Terminalsessionen att skicka till |
| `data` | `string` | Kommandot eller datan som ska skickas |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Prenumerera på all output från en terminalsession. Returnerar en **avprenumereringsfunktion**.

| Parameter | Typ | Beskrivning |
|-----------|------|-------------|
| `sessionId` | `string` | Terminalsessionen att övervaka |
| `callback` | `(data: string) => void` | Anropas med varje del av output |
| **Returnerar** | `() => void` | Anropa detta för att sluta lyssna |
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
**Viktigt:** Spara alltid avprenumereringsfunktionen och anropa den i `deactivate()` för att förhindra minnesläckor.

---

### `ctx.sftp` — Filöverföring

> **Status: Kommer Snart** — SFTP API:et är definierat men är ännu inte kopplat till appens SFTP-motor. `list()` returnerar för närvarande en tom array, och `upload()`/`download()` gör ingenting. Detta kommer att implementeras fullt ut i en framtida version. För tillfället, använd `ctx.terminal.send()` med `scp` eller `rsync` kommandon som en tillfällig lösning.

#### `sftp.list(sessionId, path)`

Lista filer i en fjärrkatalog.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Ladda upp en fil från lokal maskin till fjärrserver.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Ladda ner en fil fr��n fjärrserver till lokal maskin.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Tillfällig lösning (tills SFTP API är aktivt):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Användargränssnitt

#### `ui.addSidebarButton(options)`

Lägg till en knapp i WIA SOOM:s sidofält.

| Alternativ | Typ | Obligatorisk | Beskrivning |
|------------|------|--------------|-------------|
| `id` | `string` | Nej | Unik ID (standard till pluginens namn) |
| `icon` | `string` | Ja | Lucide ikon namn (t.ex., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ja | Knapptext som visas i sidofältet |
| `onClick` | `() => void` | Ja | Funktion som anropas när knappen klickas |
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
**Ikonreferens:** Bläddra bland alla tillgängliga ikoner på [lucide.dev/icons](https://lucide.dev/icons)

> **Kompatibilitetsnot:** Vissa äldre plugins använder positionsargument som `addSidebarButton(id, icon, label, onClick)`. Den officiella API:n använder ett **options-objekt** som dokumenterat ovan. Använd alltid objektstilen för nya plugins.

#### `ui.openWebview(options)`

Öppna ett popup-fönster med anpassat HTML-innehåll. Så här bygger du rika användargränssnitt.

| Alternativ | Typ | Beskrivning |
|------------|------|-------------|
| `title` | `string` | Fönstertitel |
| `html` | `string` | Fullt HTML-innehåll att rendera |
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
> Se [Del 3](#part-3-building-custom-ui-with-webviews) för avancerade webview-mönster.

#### `ui.showNotification(type, message)`

Visa en toast-notifikation.

| Parameter | Typ | Beskrivning |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notifikationsstil |
| `message` | `string` | Text att visa |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Lägg till ett beständigt textelement i den nedre statusfältet.

| Parameter | Typ | Beskrivning |
|-----------|------|-------------|
| `id` | `string` | Unik ID för detta statusobjekt |
| `text` | `string` | Text att visa |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Beständig lagring

Plugin-inställningar lagras permanent i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Läs ett sparat värde.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Returnerar `undefined` om nyckeln inte finns.

#### `settings.set(key, value)`

Spara ett värde. Stöder strängar, nummer, booleaner, arrayer och objekt.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exempel: Kom ihåg användarinställningar**
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

### `ctx.ai` — AI-integration

> **Status: Kommer Snart** — AI-API:et är definierat men är ännu inte kopplat till Soomy. Returnerar för närvarande `{ response: 'AI not yet connected' }`. Full AI-integration planeras för en framtida version.

#### `ai.chat(messages, options?)`

Skicka meddelanden till AI-assistenten (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Del 3: Bygga Anpassad UI med Webviews

API:et `openWebview()` låter dig bygga instrumentpaneler med HTML, CSS och JavaScript — allt inuti ett popup-fönster.

> **Viktig begränsning:** Webviews är **endast för visning**. De kan inte anropa plugin-API:er (`ctx.settings`, `ctx.terminal`, etc.). Använd sidofältknappar för alla användaråtgärder, och använd `openWebview()` för att visa nuvarande tillstånd. Om du behöver interaktiva funktioner, utlös dem från sidofältknappar och öppna webviewen igen för att uppdatera visningen.

### Mönster: Terminalkommando → Tolka utdata → Visa i HTML

Detta är det vanligaste plugin-mönstret. Du kör ett kommando, tolkar resultatet och visar det visuellt.
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
### Mönster: Interaktiv instrumentpanel med automatisk uppdatering
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
### Mönster: Visa inställningar i en Webview

> **Notera:** Webviews är endast för visning — de kan inte anropa plugin-API:er. Använd `ctx.settings` i dina sidofältknappshanterare för att modifiera inställningar, och använd `openWebview()` för att visa det aktuella tillståndet.
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

## Del 4: Publicera Ditt Plugin

### Steg 1: Testa lokalt

1. Kopiera ditt plugin till `~/.wia-soom/plugins/{your-plugin}/`
2. Starta om WIA SOOM
3. Verifiera att det fungerar: sidofältknappen visas, funktionerna fungerar korrekt
4. Testa gränsfall: vad händer om ingen terminal är ansluten?

### Steg 2: Förbered för inlämning

Din plugin-mapp måste innehålla:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Obligatoriska `package.json` fält:**

| Fält | Beskrivning | Exempel |
|------|-------------|---------|
| `name` | Unik kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantisk version | `"1.0.0"` |
| `description` | En rad beskrivning | `"Övervakar nginx access-loggar i realtid"` |
| `author` | Ditt namn | `"John Doe"` |
| `main` | Ingångspunkt | `"index.js"` |

**Valfria fält:**

| Fält | Beskrivning |
|------|-------------|
| `license` | Licenstyp (MIT rekommenderas) |
| `keywords` | Array av sökord |
| `soom.minVersion` | Minsta WIA SOOM-version som krävs |

### Steg 3: Skicka till Plugin-registret

1. ****Package** your plugin as a ZIP file
2. **Lägg till** din plugin i `plugins/{ditt-plugin-namn}/`
3. **Skicka** en Pull Request

### Steg 4: Granskning och godkännande

Vi granskar varje plugin för:

- **Säkerhet** — inga farliga API:er (se [Säkerhetsregler](#security-rules))
- **Kvalitet** — fungerar den? Är koden ren?
- **Användbarhet** — löser den ett verkligt problem?

Efter godkännande:
1. Din plugin läggs till i `registry.json`
2. Ett ZIP-paket skapas i `dist/`
3. Din plugin visas i **Plugin Store** för alla WIA SOOM-användare!

---

## Del 5: Bästa praxis

### Säkerhetsregler

Dessa regler är **obligatoriska**. Plugins som bryter mot dem kommer att avvisas.

| Regel | Varför |
|-------|-------|
| **ALDRIG** använd `eval()` eller `new Function()` | Risk för kodinjektion |
| **ALDRIG** använd `child_process`, `exec()`, `spawn()` | Använd endast `ctx.terminal.send()` för kommandon |
| **ALDRIG** hämta externa URL:er | Undantag: `wiasoom.com` API-endpoints |
| **ALDRIG** få tillgång till `process.env` | Miljövariabler kan innehålla hemligheter |
| **ALDRIG** använd `require('fs')` direkt | Använd `ctx.settings` för lagring, `ctx.sftp` för filöverföring |
| **ALDRIG** använd externa npm-paket | Endast ren JavaScript — inga node_modules |
| **MÅSTE** använda `ctx.terminal.send()` för alla fjärrkommandon | Detta går genom den säkra SSH-kanalen |
| **MÅSTE** städa upp i `deactivate()` | Ta bort lyssnare, rensa intervaller |

### Felhantering

Wrap alltid riskabla operationer i try/catch:
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
### Städa upp i deactivate()

Om din plugin skapar intervaller, lyssnare eller prenumerationer — städa upp dem:
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
### i18n Stöd

WIA SOOM stöder 254 språk. För att göra din plugin etikett översättningsbar, använd ett enkelt tillvägagångssätt:
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

## Del 6: Verkliga exempel

### Exempel 1: Server Disk Checker

Kör `df -h` på den fjärrservern och visar använt/tillgängligt utrymme i statusfältet.
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

### Exempel 2: TODO Manager

En plugin som hanterar en TODO-lista med hjälp av inställningar för beständig lagring och en webview för visning.

> **Designmönster:** Eftersom webviews inte kan anropa plugin-API:er direkt, använder denna plugin en "snapshot"-metod — den läser TODOs från inställningar, renderar dem som skrivskyddad HTML och tillhandahåller sidofältsbaserade åtgärder för att lägga till objekt. Webview är ett **visnings** lager, inte ett interaktivt formulär.
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

### Exempel 3: Error Watcher

Övervakar terminalutdata och skickar en notifikation när specifika mönster upptäckts.
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

## Bilaga: Kategorier & Ikoner

### Plugin Kategorier (29)

Använd dessa i din `package.json` `keywords` eller när du skickar in till registret:

| Kategori | Beskrivning |
|----------|-------------|
| `server` | Allmän serverhantering |
| `devtools` | Utvecklingsverktyg |
| `calculator` | Räknare och omvandlare |
| `simulator` | Simulatorer |
| `game` | Terminalspel |
| `business` | Affärsverktyg |
| `security` | Säkerhet och granskning |
| `web` | Webbserverhantering |
| `education` | Utbildningsverktyg |
| `health` | Hälsorelaterade verktyg |
| `islamic` | Islamiska verktyg (bönetider, etc.) |
| `science` | Vetenskapliga verktyg |
| `quantum` | Kvantdatorverktyg |
| `ai` | AI-drivna verktyg |
| `biotech` | Bioteknikverktyg |
| `space` | Rymd- och astronomiverktyg |
| `network` | Nätverksverktyg |
| `database` | Databashantering |
| `monitoring` | Serverövervakning |
| `devops` | DevOps och CI/CD |
| `utility` | Allmänna verktyg |
| `design` | Designverktyg |
| `ecommerce` | E-handelsverktyg |
| `automation` | Automationsverktyg |
| `kpop` | K-pop-relaterade verktyg |
| `accessibility` | Tillgänglighetsverktyg |
| `analytics` | Analys och rapportering |
| `wia` | WIA-ekosystemverktyg |
| `all` | Förekommer i alla kategorier |

### Rekommenderade Ikoner (Lucide)

| Ikon Namn | Använd för |
|-----------|---------|
| `server` | Serverhantering |
| `shield` | Säkerhet |
| `database` | Databas |
| `activity` | Övervakning |
| `terminal` | Terminalverktyg |
| `code` | Utveckling |
| `hard-drive` | Disk/lagring |
| `network` | Nätverk |
| `lock` | Autentisering/kryptering |
| `eye` | Tittande/övervakning |
| `check-square` | Uppgifter/TODO |
| `layout-dashboard` | Instrumentpaneler |
| `settings` | Konfiguration |
| `zap` | Automatisering |
| `globe` | Webb/internationell |

Bläddra bland alla 1,500+ ikoner: [lucide.dev/icons](https://lucide.dev/icons)

---

## Behöver du hjälp?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Exempel Plugins:** [Website](https://wiasoom.com)
- **Webbplats:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Bygg något fantastiskt. Dela det med världen.</em></p>
<p align="center"><em>— WIA SOOM Teamet</em></p>