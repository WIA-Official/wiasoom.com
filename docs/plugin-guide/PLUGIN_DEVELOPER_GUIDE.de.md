<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Entwicklerhandbuch</h1>
<p align="center"><strong>Erstellen Sie Ihr eigenes Plugin in 5 Minuten.</strong></p>
<p align="center">Erstellen Sie leistungsstarke Server-Tools, Dashboards und Automatisierungen — direkt in WIA SOOM.</p>

---

## Inhaltsverzeichnis

- [Teil 1: Schnellstart — Ihr erstes Plugin in 5 Minuten](#teil-1-schnellstart--ihr-erstes-plugin-in-5-minuten)
- [Teil 2: Plugin Kontext API Referenz](#teil-2-plugin-kontext-api-referenz)
  - [ctx.terminal](#ctxterminal--befehle-auf-remote-servern-ausführen)
  - [ctx.sftp](#ctxsftp--dateiübertragung)
  - [ctx.ui](#ctxui--benutzeroberfläche)
  - [ctx.settings](#ctxsettings--persistenter-speicher)
  - [ctx.ai](#ctxai--ki-integration)
- [Teil 3: Benutzerdefinierte UI mit Webviews erstellen](#teil-3-benutzerdefinierte-ui-mit-webviews-erstellen)
- [Teil 4: Veröffentlichen Ihres Plugins](#teil-4-veröffentlichen-ihres-plugins)
- [Teil 5: Best Practices](#teil-5-best-practices)
- [Teil 6: Beispiele aus der Praxis](#teil-6-beispiele-aus-der-praxis)
- [Anhang: Kategorien & Icons](#anhang-kategorien--icons)

---

## Teil 1: Schnellstart — Ihr erstes Plugin in 5 Minuten

### Was Sie erstellen werden

Ein "Hello World" Plugin, das einen Button zur Seitenleiste hinzufügt. Wenn er angeklickt wird, zeigt er eine Benachrichtigung an.

### Schritt 1: Erstellen Sie den Plugin-Ordner
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Schritt 2: Erstellen Sie package.json
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
**Erforderliche Felder:** `name`, `version`, `description`, `author`, `main`

### Schritt 3: Erstellen Sie index.js
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
### Schritt 4: WIA SOOM neu starten

Starten Sie die App neu (oder schalten Sie das Plugin in den Einstellungen → Plugins aus/ein).

Sie sollten einen **"Hello World"** Button in der Seitenleiste sehen. Klicken Sie darauf — Sie werden eine Erfolgsmeldung sehen!

### Wie es funktioniert
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

## Teil 2: Plugin Kontext API Referenz

Wenn Ihre `activate(context)` Funktion aufgerufen wird, stellt `context` (oder `ctx`) diese APIs zur Verfügung:
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

### `ctx.terminal` — Befehle auf Remote-Servern ausführen

#### `terminal.send(sessionId, data)`

Sendet einen Befehl (oder beliebige Daten) an eine aktive Terminal-Sitzung.

| Parameter | Typ | Beschreibung |
|-----------|------|-------------|
| `sessionId` | `string` | Die Terminal-Sitzung, an die gesendet werden soll |
| `data` | `string` | Der Befehl oder die Daten, die gesendet werden sollen |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abonnieren Sie alle Ausgaben von einer Terminal-Sitzung. Gibt eine **Abmeldefunktion** zurück.

| Parameter | Typ | Beschreibung |
|-----------|------|-------------|
| `sessionId` | `string` | Die Terminal-Sitzung, die überwacht werden soll |
| `callback` | `(data: string) => void` | Wird mit jedem Ausgabestück aufgerufen |
| **Gibt zurück** | `() => void` | Rufen Sie dies auf, um das Zuhören zu stoppen |
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
**Wichtig:** Speichern Sie immer die Abmeldefunktion und rufen Sie sie in `deactivate()` auf, um Speicherlecks zu vermeiden.

---

### `ctx.sftp` — Dateiübertragung

> **Status: Bald verfügbar** — Die SFTP API ist definiert, aber noch nicht mit der SFTP-Engine der App verbunden. `list()` gibt derzeit ein leeres Array zurück, und `upload()`/`download()` sind No-ops. Dies wird in einer zukünftigen Version vollständig implementiert. Verwenden Sie vorerst `ctx.terminal.send()` mit `scp` oder `rsync` Befehlen als Workaround.

#### `sftp.list(sessionId, path)`

Listet Dateien in einem Remote-Verzeichnis auf.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Lädt eine Datei von der lokalen Maschine auf den Remote-Server hoch.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Lädt eine Datei vom Remote-Server auf die lokale Maschine herunter.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (bis die SFTP API aktiv ist):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Benutzeroberfläche

#### `ui.addSidebarButton(options)`

Fügt einen Button zur WIA SOOM Seitenleiste hinzu.

| Option | Typ | Erforderlich | Beschreibung |
|--------|------|--------------|-------------|
| `id` | `string` | Nein | Eindeutige ID (standardmäßig der Plugin-Name) |
| `icon` | `string` | Ja | Lucide Icon-Name (z.B. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ja | Text des Buttons, der in der Seitenleiste angezeigt wird |
| `onClick` | `() => void` | Ja | Funktion, die aufgerufen wird, wenn der Button angeklickt wird |
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
**Icon-Referenz:** Durchsuchen Sie alle verfügbaren Icons unter [lucide.dev/icons](https://lucide.dev/icons)

> **Kompatibilitätsnotiz:** Einige ältere Plugins verwenden positionsbasierte Argumente wie `addSidebarButton(id, icon, label, onClick)`. Die offizielle API verwendet ein **Optionsobjekt**, wie oben dokumentiert. Verwenden Sie immer den Objektstil für neue Plugins.

#### `ui.openWebview(options)`

Öffnet ein Popup-Fenster mit benutzerdefiniertem HTML-Inhalt. So erstellen Sie reichhaltige UIs.

| Option | Typ | Beschreibung |
|--------|------|-------------|
| `title` | `string` | Fenstertitel |
| `html` | `string` | Vollständiger HTML-Inhalt, der gerendert werden soll |
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
> Siehe [Teil 3](#part-3-building-custom-ui-with-webviews) für fortgeschrittene Webview-Muster.

#### `ui.showNotification(type, message)`

Zeigt eine Toast-Benachrichtigung an.

| Parameter | Typ | Beschreibung |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Benachrichtigungsstil |
| `message` | `string` | Anzuzeigender Text |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Fügt ein dauerhaftes Textelement zur unteren Statusleiste hinzu.

| Parameter | Typ | Beschreibung |
|-----------|------|-------------|
| `id` | `string` | Eindeutige ID für dieses Statuselement |
| `text` | `string` | Anzuzeigender Text |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Persistente Speicherung

Plugin-Einstellungen werden dauerhaft in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` gespeichert.

#### `settings.get(key)`

Liest einen gespeicherten Wert.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Gibt `undefined` zurück, wenn der Schlüssel nicht existiert.

#### `settings.set(key, value)`

Speichert einen Wert. Unterstützt Strings, Zahlen, Booleans, Arrays und Objekte.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Beispiel: Benutzerpräferenzen speichern**
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

### `ctx.ai` — KI-Integration

> **Status: Bald verfügbar** — Die KI-API ist definiert, aber noch nicht mit Soomy verbunden. Gibt derzeit `{ response: 'AI not yet connected' }` zurück. Eine vollständige KI-Integration ist für eine zukünftige Version geplant.

#### `ai.chat(messages, options?)`

Sendet Nachrichten an den KI-Assistenten (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Teil 3: Benutzerdefinierte UI mit Webviews erstellen

Die `openWebview()` API ermöglicht es Ihnen, Dashboard-UIs mit HTML, CSS und JavaScript zu erstellen — alles in einem Popup-Fenster.

> **Wichtige Einschränkung:** Webviews sind **nur zur Anzeige**. Sie können nicht auf Plugin-APIs (`ctx.settings`, `ctx.terminal` usw.) zurückgreifen. Verwenden Sie Sidebar-Buttons für alle Benutzeraktionen und verwenden Sie `openWebview()`, um den aktuellen Zustand anzuzeigen. Wenn Sie interaktive Funktionen benötigen, lösen Sie diese von Sidebar-Buttons aus und öffnen Sie das Webview erneut, um die Anzeige zu aktualisieren.

### Muster: Terminalbefehl → Ausgabe parsen → In HTML anzeigen

Dies ist das häufigste Plugin-Muster. Sie führen einen Befehl aus, parsen das Ergebnis und zeigen es visuell an.
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
### Muster: Interaktives Dashboard mit automatischer Aktualisierung
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
### Muster: Einstellungen in einem Webview anzeigen

> **Hinweis:** Webviews sind nur zur Anzeige — sie können nicht auf Plugin-APIs zurückgreifen. Verwenden Sie `ctx.settings` in Ihren Sidebar-Button-Handlern, um Einstellungen zu ändern, und verwenden Sie `openWebview()`, um den aktuellen Zustand anzuzeigen.
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

## Teil 4: Veröffentlichen Ihres Plugins

### Schritt 1: Lokal testen

1. Kopieren Sie Ihr Plugin nach `~/.wia-soom/plugins/{your-plugin}/`
2. WIA SOOM neu starten
3. Überprüfen, ob es funktioniert: Sidebar-Button erscheint, Funktionen arbeiten korrekt
4. Randfälle testen: Was passiert, wenn kein Terminal verbunden ist?

### Schritt 2: Vorbereitung zur Einreichung

Ihr Plugin-Ordner muss enthalten:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Erforderliche `package.json`-Felder:**

| Feld | Beschreibung | Beispiel |
|-------|-------------|---------|
| `name` | Eindeutige kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantische Version | `"1.0.0"` |
| `description` | Einzeilige Beschreibung | `"Überwacht nginx-Zugriffsprotokolle in Echtzeit"` |
| `author` | Ihr Name | `"John Doe"` |
| `main` | Einstiegspunkt | `"index.js"` |

**Optionale Felder:**

| Feld | Beschreibung |
|-------|-------------|
| `license` | Lizenztyp (MIT empfohlen) |
| `keywords` | Array von Suchbegriffen |
| `soom.minVersion` | Mindestversion von WIA SOOM erforderlich |

### Schritt 3: Einreichung im Plugin-Register

1. ****Package** your plugin as a ZIP file
2. **Fügen** Sie Ihr Plugin zu `plugins/{your-plugin-name}/` hinzu
3. **Reichen** Sie einen Pull Request ein

### Schritt 4: Überprüfung und Genehmigung

Wir überprüfen jedes Plugin auf:

- **Sicherheit** — keine gefährlichen APIs (siehe [Sicherheitsregeln](#security-rules))
- **Qualität** — funktioniert es? Ist der Code sauber?
- **Nützlichkeit** — löst es ein echtes Problem?

Nach der Genehmigung:
1. Ihr Plugin wird zu `registry.json` hinzugefügt
2. Ein ZIP-Bundle wird in `dist/` erstellt
3. Ihr Plugin erscheint im **Plugin Store** für alle WIA SOOM-Nutzer!

---

## Teil 5: Best Practices

### Sicherheitsregeln

Diese Regeln sind **verpflichtend**. Plugins, die dagegen verstoßen, werden abgelehnt.

| Regel | Warum |
|------|-----|
| **NIEMALS** `eval()` oder `new Function()` verwenden | Risiko der Code-Injektion |
| **NIEMALS** `child_process`, `exec()`, `spawn()` verwenden | Verwenden Sie nur `ctx.terminal.send()` für Befehle |
| **NIEMALS** externe URLs abrufen | Ausnahme: `wiasoom.com` API-Endpunkte |
| **NIEMALS** auf `process.env` zugreifen | Umgebungsvariablen können Geheimnisse enthalten |
| **NIEMALS** `require('fs')` direkt verwenden | Verwenden Sie `ctx.settings` für Speicherung, `ctx.sftp` für Dateiübertragungen |
| **NIEMALS** externe npm-Pakete verwenden | Nur reines JavaScript — keine node_modules |
| **MUSS** `ctx.terminal.send()` für alle Remote-Befehle verwenden | Dies erfolgt über den sicheren SSH-Kanal |
| **MUSS** in `deactivate()` aufräumen | Entfernen Sie Listener, löschen Sie Intervalle |

### Fehlerbehandlung

Wickeln Sie riskante Operationen immer in try/catch:
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
### Aufräumen in deactivate()

Wenn Ihr Plugin Intervalle, Listener oder Abonnements erstellt — räumen Sie diese auf:
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
### i18n Unterstützung

WIA SOOM unterstützt 254 Sprachen. Um Ihr Plugin-Label übersetzbar zu machen, verwenden Sie einen einfachen Ansatz:
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

## Teil 6: Beispiele aus der Praxis

### Beispiel 1: Server-Disk-Checker

Führt `df -h` auf dem Remote-Server aus und zeigt den verwendeten/verfügbaren Speicherplatz in der Statusleiste an.
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

### Beispiel 2: TODO-Manager

Ein Plugin, das eine TODO-Liste verwaltet, indem es Einstellungen für die persistente Speicherung und eine Webansicht für die Anzeige verwendet.

> **Entwurfsmuster:** Da Webansichten nicht direkt auf Plugin-APIs zugreifen können, verwendet dieses Plugin einen "Snapshot"-Ansatz — es liest TODOs aus den Einstellungen, rendert sie als schreibgeschütztes HTML und bietet sidebar-basierte Aktionen zum Hinzufügen von Elementen. Die Webansicht ist eine **Anzeige**-Schicht, kein interaktives Formular.
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

### Beispiel 3: Fehlerüberwacher

Überwacht die Terminalausgabe und sendet eine Benachrichtigung, wenn bestimmte Muster erkannt werden.
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

## Anhang: Kategorien & Icons

### Plugin-Kategorien (29)

Verwenden Sie diese in Ihrer `package.json` `keywords` oder beim Einreichen im Registry:

| Kategorie | Beschreibung |
|-----------|--------------|
| `server` | Allgemeine Serververwaltung |
| `devtools` | Entwicklungstools |
| `calculator` | Rechner und Konverter |
| `simulator` | Simulatoren |
| `game` | Terminalspiele |
| `business` | Geschäftstools |
| `security` | Sicherheit und Auditing |
| `web` | Webserververwaltung |
| `education` | Bildungstools |
| `health` | Gesundheitsbezogene Tools |
| `islamic` | Islamische Tools (Gebetszeiten usw.) |
| `science` | Wissenschaftliche Tools |
| `quantum` | Quantencomputing-Tools |
| `ai` | KI-gestützte Tools |
| `biotech` | Biotechnologie-Tools |
| `space` | Raumfahrt- und Astronomie-Tools |
| `network` | Netzwerk-Tools |
| `database` | Datenbankverwaltung |
| `monitoring` | Serverüberwachung |
| `devops` | DevOps und CI/CD |
| `utility` | Allgemeine Dienstprogramme |
| `design` | Designtools |
| `ecommerce` | E-Commerce-Tools |
| `automation` | Automatisierungstools |
| `kpop` | K-Pop bezogene Tools |
| `accessibility` | Barrierefreiheitstools |
| `analytics` | Analytik und Berichterstattung |
| `wia` | WIA-Ökosystem-Tools |
| `all` | Erscheint in allen Kategorien |

### Empfohlene Icons (Lucide)

| Icon-Name | Verwendung für |
|-----------|----------------|
| `server` | Serververwaltung |
| `shield` | Sicherheit |
| `database` | Datenbank |
| `activity` | Überwachung |
| `terminal` | Terminal-Tools |
| `code` | Entwicklung |
| `hard-drive` | Festplatte/Speicher |
| `network` | Netzwerkverbindungen |
| `lock` | Authentifizierung/Verschlüsselung |
| `eye` | Beobachtung/Überwachung |
| `check-square` | Aufgaben/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfiguration |
| `zap` | Automatisierung |
| `globe` | Web/international |

Durchsuchen Sie alle 1.500+ Icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Brauchen Sie Hilfe?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Beispiel-Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Baue etwas Unglaubliches. Teile es mit der Welt.</em></p>
<p align="center"><em>— Das WIA SOOM Team</em></p>