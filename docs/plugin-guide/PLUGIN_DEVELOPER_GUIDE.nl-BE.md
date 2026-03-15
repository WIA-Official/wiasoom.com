<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Ontwikkelaarsgids</h1>
<p align="center"><strong>Bouw je eigen plugin in 5 minuten.</strong></p>
<p align="center">Creëer krachtige servertools, dashboards en automatiseringen — direct in WIA SOOM.</p>

---

## Inhoudsopgave

- [Deel 1: Snelle Start — Je Eerste Plugin in 5 Minuten](#deel-1-snelle-start--je-eerste-plugin-in-5-minuten)
- [Deel 2: Plugin Context API Referentie](#deel-2-plugin-context-api-referentie)
  - [ctx.terminal](#ctxterminal--voer-commando's-uit-op-afstandsservers)
  - [ctx.sftp](#ctxsftp--bestandsoverdracht)
  - [ctx.ui](#ctxui--gebruikersinterface)
  - [ctx.settings](#ctxsettings--persistente-opslag)
  - [ctx.ai](#ctxai--ai-integratie)
- [Deel 3: Aangepaste UI Bouwen met Webviews](#deel-3-aangepaste-ui-bouwen-met-webviews)
- [Deel 4: Je Plugin Publiceren](#deel-4-je-plugin-publiceren)
- [Deel 5: Beste Praktijken](#deel-5-beste-praktijken)
- [Deel 6: Voorbeelden uit de Praktijk](#deel-6-voorbeelden-uit-de-praktijk)
- [Bijlage: Categorieën & Iconen](#bijlage-categorieen--iconen)

---

## Deel 1: Snelle Start — Je Eerste Plugin in 5 Minuten

### Wat je gaat bouwen

Een "Hello World" plugin die een knop aan de zijbalk toevoegt. Wanneer erop geklikt wordt, toont het een melding.

### Stap 1: Maak de pluginmap
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Stap 2: Maak package.json
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
**Vereiste velden:** `name`, `version`, `description`, `author`, `main`

### Stap 3: Maak index.js
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
### Stap 4: Herstart WIA SOOM

Herstart de app (of schakel de plugin uit/aan in Instellingen → Plugins).

Je zou een **"Hello World"** knop in de zijbalk moeten zien. Klik erop — je ziet een succesmelding!

### Hoe het werkt
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

## Deel 2: Plugin Context API Referentie

Wanneer je `activate(context)` functie wordt aangeroepen, biedt `context` (of `ctx`) deze API's:
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

### `ctx.terminal` — Voer commando's uit op afstandservers

#### `terminal.send(sessionId, data)`

Stuur een commando (of enige data) naar een actieve terminalsessie.

| Parameter | Type | Beschrijving |
|-----------|------|--------------|
| `sessionId` | `string` | De terminalsessie waarnaar je wilt verzenden |
| `data` | `string` | Het commando of de data die je wilt verzenden |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abonneer je op alle output van een terminalsessie. Retourneert een **afmeldfunctie**.

| Parameter | Type | Beschrijving |
|-----------|------|--------------|
| `sessionId` | `string` | De terminalsessie om te volgen |
| `callback` | `(data: string) => void` | Wordt aangeroepen met elke chunk van output |
| **Retourneert** | `() => void` | Roep dit aan om te stoppen met luisteren |
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
**Belangrijk:** Bewaar altijd de afmeldfunctie en roep deze aan in `deactivate()` om geheugenlekken te voorkomen.

---

### `ctx.sftp` — Bestandsoverdracht

> **Status: Binnenkort Beschikbaar** — De SFTP API is gedefinieerd maar nog niet aangesloten op de SFTP-engine van de app. `list()` retourneert momenteel een lege array, en `upload()`/`download()` zijn no-ops. Dit zal volledig geïmplementeerd worden in een toekomstige release. Gebruik voorlopig `ctx.terminal.send()` met `scp` of `rsync` commando's als workaround.

#### `sftp.list(sessionId, path)`

Lijst bestanden in een externe map.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Upload een bestand van de lokale machine naar de externe server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Download een bestand van de externe server naar de lokale machine.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (totdat de SFTP API live is):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Gebruikersinterface

#### `ui.addSidebarButton(options)`

Voeg een knop toe aan de WIA SOOM zijbalk.

| Optie | Type | Vereist | Beschrijving |
|-------|------|---------|--------------|
| `id` | `string` | Nee | Unieke ID (standaard naar pluginnaam) |
| `icon` | `string` | Ja | Lucide icon naam (bijv., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ja | Knoptekst die in de zijbalk wordt weergegeven |
| `onClick` | `() => void` | Ja | Functie die wordt aangeroepen wanneer op de knop wordt geklikt |
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
**Iconenreferentie:** Blader door alle beschikbare iconen op [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibiliteitsopmerking:** Sommige oudere plugins gebruiken positionele argumenten zoals `addSidebarButton(id, icon, label, onClick)`. De officiële API gebruikt een **optiesobject** zoals hierboven gedocumenteerd. Gebruik altijd de objectstijl voor nieuwe plugins.

#### `ui.openWebview(options)`

Open een pop-upvenster met aangepaste HTML-inhoud. Dit is hoe je rijke UIs bouwt.

| Optie | Type | Beschrijving |
|-------|------|--------------|
| `title` | `string` | Venstertitel |
| `html` | `string` | Volledige HTML-inhoud om weer te geven |
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
> Zie [Deel 3](#part-3-building-custom-ui-with-webviews) voor geavanceerde webview-patronen.

#### `ui.showNotification(type, message)`

Toon een toast-notificatie.

| Parameter | Type | Beschrijving |
|-----------|------|--------------|
| `type` | `'success' \| 'error' \| 'info'` | Notificatiestijl |
| `message` | `string` | Te tonen tekst |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Voeg een persistent tekstitem toe aan de onderste statusbalk.

| Parameter | Type | Beschrijving |
|-----------|------|--------------|
| `id` | `string` | Unieke ID voor dit statusitem |
| `text` | `string` | Te tonen tekst |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Persistente opslag

Plugininstellingen worden permanent opgeslagen in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lees een opgeslagen waarde.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Geeft `undefined` terug als de sleutel niet bestaat.

#### `settings.set(key, value)`

Sla een waarde op. Ondersteunt strings, getallen, booleans, arrays en objecten.
§��§CHUNK_SEPARATOR§§§
**Voorbeeld: Onthoud gebruikersvoorkeuren**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — AI-integratie

> **Status: Binnenkort Beschikbaar** — De AI API is gedefinieerd maar nog niet verbonden met Soomy. Geeft momenteel `{ response: 'AI not yet connected' }` terug. Volledige AI-integratie is gepland voor een toekomstige release.

#### `ai.chat(messages, options?)`

Stuur berichten naar de AI-assistent (Soomy).
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

## Deel 3: Aangepaste UI bouwen met Webviews

De `openWebview()` API stelt je in staat om dashboard-UIs te bouwen met HTML, CSS en JavaScript — allemaal binnen een pop-upvenster.

> **Belangrijke beperking:** Webviews zijn **alleen ter weergave**. Ze kunnen geen terugroepacties doen naar plugin-API's (`ctx.settings`, `ctx.terminal`, enz.). Gebruik zijbalkknoppen voor alle gebruikersacties en gebruik `openWebview()` om de huidige status weer te geven. Als je interactieve functies nodig hebt, activeer deze dan vanuit zijbalkknoppen en heropen de webview om de weergave te vernieuwen.

### Patroon: Terminalopdracht → Parseer uitvoer → Toon in HTML

Dit is het meest voorkomende pluginpatroon. Je voert een opdracht uit, parseert het resultaat en toont het visueel.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Patroon: Interactief Dashboard met Auto-Refresh
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
### Patroon: Instellingen weergeven in een Webview

> **Opmerking:** Webviews zijn alleen ter weergave — ze kunnen geen terugroepacties doen naar plugin-API's. Gebruik `ctx.settings` in je zijbalkknophandlers om instellingen te wijzigen, en gebruik `openWebview()` om de huidige status te tonen.
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

## Deel 4: Je Plugin Publiceren

### Stap 1: Test lokaal

1. Kopieer je plugin naar `~/.wia-soom/plugins/{your-plugin}/`
2. Herstart WIA SOOM
3. Controleer of het werkt: zijbalkknop verschijnt, functies werken correct
4. Test randgevallen: wat gebeurt er als er geen terminal is verbonden?

### Stap 2: Voorbereiden op indiening

Je pluginmap moet bevatten:
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
**Vereiste `package.json` velden:**

| Veld | Beschrijving | Voorbeeld |
|------|--------------|-----------|
| `name` | Unieke kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantische versie | `"1.0.0"` |
| `description` | Een-regel beschrijving | `"Monitors nginx access logs in real-time"` |
| `author` | Jouw naam | `"John Doe"` |
| `main` | Toegangspunt | `"index.js"` |

**Optionele velden:**

| Veld | Beschrijving |
|------|--------------|
| `license` | Licentietype (MIT aanbevolen) |
| `keywords` | Array van zoektags |
| `soom.minVersion` | Minimale vereiste WIA SOOM versie |

### Stap 3: Indienen bij de Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Voeg** jouw plugin toe aan `plugins/{jouw-plugin-naam}/`
3. **Dien** een Pull Request in

### Stap 4: Beoordeling en goedkeuring

We beoordelen elke plugin op:

- **Beveiliging** — geen gevaarlijke API's (zie [Beveiligingsregels](#security-rules))
- **Kwaliteit** — werkt het? Is de code schoon?
- **Nuttigheid** — lost het een echt probleem op?

Na goedkeuring:
1. Jouw plugin wordt toegevoegd aan `registry.json`
2. Een ZIP-bundel wordt aangemaakt in `dist/`
3. Jouw plugin verschijnt in de **Plugin Store** voor alle WIA SOOM gebruikers!

---

## Deel 5: Beste Praktijken

### Beveiligingsregels

Deze regels zijn **verplicht**. Plugins die deze regels overtreden, worden afgewezen.

| Regel | Waarom |
|-------|--------|
| **NOOIT** gebruik `eval()` of `new Function()` | Risico op code-injectie |
| **NOOIT** gebruik `child_process`, `exec()`, `spawn()` | Gebruik alleen `ctx.terminal.send()` voor commando's |
| **NOOIT** externe URL's ophalen | Uitzondering: `wiasoom.com` API-eindpunten |
| **NOOIT** toegang tot `process.env` | Omgevingsvariabelen kunnen geheimen bevatten |
| **NOOIT** gebruik `require('fs')` direct | Gebruik `ctx.settings` voor opslag, `ctx.sftp` voor bestandsoverdracht |
| **NOOIT** gebruik externe npm-pakketten | Alleen pure JavaScript — geen node_modules |
| **MOET** `ctx.terminal.send()` gebruiken voor alle externe commando's | Dit gaat via het veilige SSH-kanaal |
| **MOET** opruimen in `deactivate()` | Verwijder listeners, wis intervallen |

### Foutafhandeling

Wikkel risicovolle operaties altijd in try/catch:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Opruimen in deactivate()

Als jouw plugin intervallen, listeners of abonnementen aanmaakt — ruim ze op:
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
### i18n Ondersteuning

WIA SOOM ondersteunt 254 talen. Om jouw pluginlabel vertaalbaar te maken, gebruik een eenvoudige aanpak:
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

## Deel 6: Voorbeelden uit de Praktijk

### Voorbeeld 1: Server Schijf Checker

Voert `df -h` uit op de externe server en toont gebruikte/beschikbare ruimte in de statusbalk.
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

### Voorbeeld 2: TODO Beheerder

Een plugin die een TODO-lijst beheert met instellingen voor persistente opslag en een webview voor weergave.

> **Ontwerppatroon:** Aangezien webviews geen directe oproepen naar plugin-API's kunnen doen, gebruikt deze plugin een "snapshot"-benadering — het leest TODO's uit de instellingen, rendert ze als alleen-lezen HTML, en biedt sidebar-gebaseerde acties voor het toevoegen van items. De webview is een **weergave** laag, geen interactief formulier.
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

### Voorbeeld 3: Foutbewaker

Monitort terminaluitvoer en stuurt een notificatie wanneer specifieke patronen worden gedetecteerd.
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

## Bijlage: Categorieën & Iconen

### Plugin Categorieën (29)

Gebruik deze in je `package.json` `keywords` of bij het indienen bij de registry:

| Categorie | Beschrijving |
|-----------|--------------|
| `server` | Algemene serverbeheer |
| `devtools` | Ontwikkelingstools |
| `calculator` | Rekenmachines en converters |
| `simulator` | Simulators |
| `game` | Terminalspellen |
| `business` | Zakelijke tools |
| `security` | Beveiliging en auditing |
| `web` | Webserverbeheer |
| `education` | Onderwijstools |
| `health` | Gezondheidsgerelateerde tools |
| `islamic` | Islamitische tools (gebedstijden, enz.) |
| `science` | Wetenschappelijke tools |
| `quantum` | Quantum computing tools |
| `ai` | AI-gestuurde tools |
| `biotech` | Biotechnologie tools |
| `space` | Ruimte- en astronomie tools |
| `network` | Netwerkt tools |
| `database` | Databasebeheer |
| `monitoring` | Servermonitoring |
| `devops` | DevOps en CI/CD |
| `utility` | Algemene hulpprogramma's |
| `design` | Ontwerptools |
| `ecommerce` | E-commerce tools |
| `automation` | Automatiseringstools |
| `kpop` | K-pop gerelateerde tools |
| `accessibility` | Toegankelijkheidstools |
| `analytics` | Analytics en rapportage |
| `wia` | WIA ecosysteemtools |
| `all` | Verschijnt in alle categorieën |

### Aanbevolen Iconen (Lucide)

| Icoon Naam | Gebruik voor |
|------------|--------------|
| `server` | Serverbeheer |
| `shield` | Beveiliging |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Terminaltools |
| `code` | Ontwikkeling |
| `hard-drive` | Schijf/opslag |
| `network` | Netwerken |
| `lock` | Auth/encryptie |
| `eye` | Kijken/monitoring |
| `check-square` | Taken/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuratie |
| `zap` | Automatisering |
| `globe` | Web/internationaal |

Blader door alle 1.500+ iconen: [lucide.dev/icons](https://lucide.dev/icons)

---

## Hulp Nodig?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Voorbeeld Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Bouw iets geweldig. Deel het met de wereld.</em></p>
<p align="center"><em>— Het WIA SOOM Team</em></p>
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
