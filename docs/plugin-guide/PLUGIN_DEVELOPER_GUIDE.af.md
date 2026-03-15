<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Ontwikkelaar Gids</h1>
<p align="center"><strong>Bou jou eie plugin in 5 minute.</strong></p>
<p align="center">Skep kragtige bediener gereedskap, dashboards, en outomatiserings — reg binne WIA SOOM.</p>

---

## Inhoudsopgawe

- [Deel 1: Vinning Begin — Jou Eerste Plugin in 5 Minute](#deel-1-vinnig-begin--jou-eerste-plugin-in-5-minute)
- [Deel 2: Plugin Konteks API Verwysing](#deel-2-plugin-konteks-api-verwysing)
  - [ctx.terminal](#ctxterminal--voer-opdragte-uit-op-afgeleë-bedieners)
  - [ctx.sftp](#ctxsftp--lêer-oordrag)
  - [ctx.ui](#ctxui--gebruikerskoppelvlak)
  - [ctx.settings](#ctxsettings--volhoubare-berging)
  - [ctx.ai](#ctxai--kunsmatige-intelligensie-integrasie)
- [Deel 3: Bou Aangepaste UI met Webviews](#deel-3-bou-aangepaste-ui-met-webviews)
- [Deel 4: Publiseer Jou Plugin](#deel-4-publiseer-jou-plugin)
- [Deel 5: Beste Praktyke](#deel-5-beste-praktyke)
- [Deel 6: Regte-Wêreld Voorbeelde](#deel-6-regte-wêreld-voorbeelde)
- [Bylae: Kategoriene & Ikone](#bylae-kategoriene--ikone)

---

## Deel 1: Vinning Begin — Jou Eerste Plugin in 5 Minute

### Wat jy gaan bou

'n "Hello World" plugin wat 'n knoppie aan die sybalk voeg. Wanneer dit geklik word, wys dit 'n kennisgewing.

### Stap 1: Skep die plugin vouer
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Stap 2: Skep package.json
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
**Vereiste velde:** `name`, `version`, `description`, `author`, `main`

### Stap 3: Skep index.js
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
### Stap 4: Herbegin WIA SOOM

Herbegin die toepassing (of skakel die plugin af/aan in Instellings → Plugins).

Jy behoort 'n **"Hello World"** knoppie in die sybalk te sien. Klik daarop — jy sal 'n sukses kennisgewing sien!

### Hoe dit werk
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

## Deel 2: Plugin Konteks API Verwysing

Wanneer jou `activate(context)` funksie aangeroep word, bied `context` (of `ctx`) hierdie API's aan:
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

### `ctx.terminal` — Voer opdragte uit op afgeleë bedieners

#### `terminal.send(sessionId, data)`

Stuur 'n opdrag (of enige data) na 'n aktiewe terminal sessie.

| Parameter | Tipe | Beskrywing |
|-----------|------|-------------|
| `sessionId` | `string` | Die terminal sessie om na te stuur |
| `data` | `string` | Die opdrag of data om te stuur |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Teken in op alle uitvoer van 'n terminal sessie. Gee 'n **afskakel funksie** terug.

| Parameter | Tipe | Beskrywing |
|-----------|------|-------------|
| `sessionId` | `string` | Die terminal sessie om te kyk |
| `callback` | `(data: string) => void` | Word aangeroep met elke stuk uitvoer |
| **Gee terug** | `() => void` | Roep dit aan om te stop luister |
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
**Belangrik:** Bewaar altyd die afskakel funksie en roep dit in `deactivate()` aan om geheuelekas te voorkom.

---

### `ctx.sftp` — Lêer oordrag

> **Status: Binnekort Beskikbaar** — Die SFTP API is gedefinieer maar nog nie aan die toepassing se SFTP enjin gekoppel nie. `list()` gee tans 'n leë lys terug, en `upload()`/`download()` is geen operasies nie. Dit sal volledig geïmplementeer word in 'n toekomstige vrystelling. Vir nou, gebruik `ctx.terminal.send()` met `scp` of `rsync` opdragte as 'n werk rondom.

#### `sftp.list(sessionId, path)`

Lys lêers in 'n afgeleë gids.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Laai 'n lêer van die plaaslike masjien na die afgeleë bediener op.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Laai 'n lêer van die afgeleë bediener na die plaaslike masjien af.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Werk rondom (totdat SFTP API lewendig is):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Gebruikerskoppelvlak

#### `ui.addSidebarButton(options)`

Voeg 'n knoppie by die WIA SOOM sybalk.

| Opsie | Tipe | Vereis | Beskrywing |
|--------|------|----------|-------------|
| `id` | `string` | Nee | Unieke ID (standaard na plugin naam) |
| `icon` | `string` | Ja | Lucide ikoon naam (bv., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ja | Knoppieteks wat in die sybalk vertoon word |
| `onClick` | `() => void` | Ja | Funksie wat aangeroep word wanneer die knoppie geklik word |
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
**Ikon verwysing:** Blaai deur alle beskikbare ikone by [lucide.dev/icons](https://lucide.dev/icons)

> **Kompatibiliteit nota:** Sommige ouer plugins gebruik posisionele argumente soos `addSidebarButton(id, icon, label, onClick)`. Die amptelike API gebruik 'n **opsie objek** soos hierbo gedokumenteer. Gebruik altyd die objekstyl vir nuwe plugins.

#### `ui.openWebview(options)`

Maak 'n pop-up venster met aangepaste HTML inhoud. Dit is hoe jy ryk UIs bou.

| Opsie | Tipe | Beskrywing |
|--------|------|-------------|
| `title` | `string` | Venstertitel |
| `html` | `string` | Volledige HTML inhoud om te weergee |
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
> Sien [Deel 3](#part-3-building-custom-ui-with-webviews) vir gevorderde webview patrone.

#### `ui.showNotification(type, message)`

Wys 'n toast kennisgewing.

| Parameter | Tipe | Beskrywing |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Kennisgewing styl |
| `message` | `string` | Tekst om te wys |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Voeg 'n volgehoue teksitem by die onderste statusbalk.

| Parameter | Tipe | Beskrywing |
|-----------|------|-------------|
| `id` | `string` | Unieke ID vir hierdie statusitem |
| `text` | `string` | Tekst om te vertoon |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Volgehoue stoor

Plugin instellings word permanent gestoor in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lees 'n gestoor waarde.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Gee `undefined` terug as die sleutel nie bestaan nie.

#### `settings.set(key, value)`

Stoor 'n waarde. Ondersteun strings, getalle, booleans, arrays, en objekte.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Voorbeeld: Onthou gebruiker voorkeure**
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

### `ctx.ai` — KI integrasie

> **Status: Binnekort** — Die KI API is gedefinieer maar nog nie aan Soomy gekoppel nie. Huidiglik gee dit `{ response: 'AI not yet connected' }` terug. Volledige KI integrasie is beplan vir 'n toekomstige vrystelling.

#### `ai.chat(messages, options?)`

Stuur boodskappe na die KI assistent (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Deel 3: Bou Aangepaste UI met Webviews

Die `openWebview()` API laat jou toe om dashboard UI's te bou met HTML, CSS, en JavaScript — alles binne 'n pop-up venster.

> **Belangrike beperking:** Webviews is **slegs vertoon**. Hulle kan nie terugroep na plugin API's nie (`ctx.settings`, `ctx.terminal`, ens.). Gebruik sidebar knoppies vir alle gebruikers aksies, en gebruik `openWebview()` om die huidige toestand te vertoon. As jy interaktiewe kenmerke benodig, aktiveer hulle vanaf sidebar knoppies en heropen die webview om die vertoning te verfris.

### Patroon: Terminal Opdrag → Parse Uitset → Wys in HTML

Dit is die mees algemene plugin patroon. Jy voer 'n opdrag uit, parse die resultaat, en vertoon dit visueel.
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
### Patroon: Interaktiewe Dashboard met Outo-Verfrissing
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
### Patroon: Wys Instellings in 'n Webview

> **Let wel:** Webviews is slegs vertoon — hulle kan nie terugroep na plugin API's nie. Gebruik `ctx.settings` in jou sidebar knoppie handlers om instellings te wysig, en gebruik `openWebview()` om die huidige toestand te wys.
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

## Deel 4: Publiseer Jou Plugin

### Stap 1: Toets plaaslik

1. Kopieer jou plugin na `~/.wia-soom/plugins/{your-plugin}/`
2. Herbegin WIA SOOM
3. Verifieer dit werk: sidebar knoppie verskyn, funksies werk korrek
4. Toets rand gevalle: wat gebeur as daar geen terminal gekoppel is nie?

### Stap 2: Berei voor vir indiening

Jou plugin gids moet bevat:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Benodigde `package.json` velde:**

| Veld | Beskrywing | Voorbeeld |
|-------|-------------|---------|
| `name` | Unieke kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantiese weergawe | `"1.0.0"` |
| `description` | Eenlyn beskrywing | `"Monitors nginx access logs in real-time"` |
| `author` | Jou naam | `"John Doe"` |
| `main` | Toegangspunt | `"index.js"` |

**Opsionele velde:**

| Veld | Beskrywing |
|-------|-------------|
| `license` | Lisensie tipe (MIT aanbeveel) |
| `keywords` | Array van soek etikette |
| `soom.minVersion` | Minimum WIA SOOM weergawe vereis |

### Stap 3: Dien in by die Plugin Registrasie

1. ****Package** your plugin as a ZIP file
2. **Voeg** jou plugin by `plugins/{your-plugin-name}/`
3. **Dien** 'n Pull Request in

### Stap 4: Hersiening en goedkeuring

Ons hersien elke plugin vir:

- **Sekuriteit** — geen gevaarlike API's (sien [Sekuriteitsreëls](#security-rules))
- **Kwaliteit** — werk dit? Is die kode skoon?
- **Nuttigheid** — los dit 'n werklike probleem op?

Na goedkeuring:
1. Jou plugin word by `registry.json` gevoeg
2. 'n ZIP-pakket word in `dist/` geskep
3. Jou plugin verskyn in die **Plugin Store** vir alle WIA SOOM gebruikers!

---

## Deel 5: Beste Praktyke

### Sekuriteitsreëls

Hierdie reëls is **verpligtend**. Plugins wat dit oortree, sal verwerp word.

| Reël | Waarom |
|------|-----|
| **NOOIT** gebruik `eval()` of `new Function()` | Risiko van kode-inspuiting |
| **NOOIT** gebruik `child_process`, `exec()`, `spawn()` | Gebruik slegs `ctx.terminal.send()` vir opdragte |
| **NOOIT** haal eksterne URL's op | Uitsondering: `wiasoom.com` API eindpunte |
| **NOOIT** toegang tot `process.env` | Omgewingsveranderlikes kan geheime bevat |
| **NOOIT** gebruik `require('fs')` direk | Gebruik `ctx.settings` vir stoor, `ctx.sftp` vir lêer oordrag |
| **NOOIT** gebruik npm eksterne pakkette | Slegs suiwer JavaScript — geen node_modules |
| **MOET** `ctx.terminal.send()` gebruik vir alle afstandsopdragte | Dit gaan deur die veilige SSH-kanaal |
| **MOET** opruim in `deactivate()` | Verwyder luisteraars, maak intervalle skoon |

### Foutafhandeling

Wrap altyd riskante operasies in try/catch:
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
### Opruiming in deactivate()

As jou plugin intervalle, luisteraars of intekeninge skep — maak dit skoon:
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
### i18n Ondersteuning

WIA SOOM ondersteun 254 tale. Om jou plugin etiket vertaalbaar te maak, gebruik 'n eenvoudige benadering:
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

## Deel 6: Regte-Wêreld Voorbeelde

### Voorbeeld 1: Bediener Skyf Kontroleerder

Voer `df -h` uit op die afstandsbediener en wys gebruikte/beskikbare spasie in die statusbalk.
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

### Voorbeeld 2: TODO Bestuurder

'n Plugin wat 'n TODO lys bestuur met behulp van instellings vir volgehoue stoor en 'n webview vir vertoon.

> **Ontwerp patroon:** Aangesien webviews nie direk plugin API's kan aanroep nie, gebruik hierdie plugin 'n "snapshot" benadering — dit lees TODO's uit instellings, render dit as lees-alleen HTML, en bied sidebar-gebaseerde aksies vir die toevoeging van items. Die webview is 'n **weergave** laag, nie 'n interaktiewe vorm nie.
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

### Voorbeeld 3: Fout Toesighouer

Moniteer terminale uitset en stuur 'n kennisgewing wanneer spesifieke patrone opgespoor word.
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

## Bylae: Kategories & Ikone

### Plugin Kategories (29)

Gebruik hierdie in jou `package.json` `keywords` of wanneer jy indien aan die registrasie:

| Kategore | Beskrywing |
|----------|-------------|
| `server` | Algemene bediener bestuur |
| `devtools` | Ontwikkeling gereedskap |
| `calculator` | Sakrekenaars en omskakelaars |
| `simulator` | Simulators |
| `game` | Terminal speletjies |
| `business` | Besigheids gereedskap |
| `security` | Sekuriteit en ouditering |
| `web` | Web bediener bestuur |
| `education` | Onderwys gereedskap |
| `health` | Gesondheid-verwante gereedskap |
| `islamic` | Islamitiese gereedskap (gebedstye, ens.) |
| `science` | Wetenskaplike gereedskap |
| `quantum` | Kwantum rekenaar gereedskap |
| `ai` | KI-gedrewe gereedskap |
| `biotech` | Biotegnologie gereedskap |
| `space` | Ruimte en sterrekunde gereedskap |
| `network` | Netwerk gereedskap |
| `database` | Databasis bestuur |
| `monitoring` | Bediener monitering |
| `devops` | DevOps en CI/CD |
| `utility` | Algemene nutsmiddels |
| `design` | Ontwerp gereedskap |
| `ecommerce` | E-handel gereedskap |
| `automation` | Outomatisering gereedskap |
| `kpop` | K-pop verwante gereedskap |
| `accessibility` | Toeganklikheid gereedskap |
| `analytics` | Analise en verslagdoening |
| `wia` | WIA ekosisteem gereedskap |
| `all` | Verskyn in alle kategories |

### Aanbevole Ikone (Lucide)

| Ikon Naam | Gebruik vir |
|-----------|---------|
| `server` | Bediener bestuur |
| `shield` | Sekuriteit |
| `database` | Databasis |
| `activity` | Monitering |
| `terminal` | Terminal gereedskap |
| `code` | Ontwikkeling |
| `hard-drive` | Skyf/berging |
| `network` | Netwerk |
| `lock` | Auth/enkripsie |
| `eye` | Kyk/monitering |
| `check-square` | Take/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfigurasie |
| `zap` | Outomatisering |
| `globe` | Web/internasionaal |

Blaai deur al 1,500+ ikone: [lucide.dev/icons](https://lucide.dev/icons)

---

## Het Jy Hulp Nodig?

- **GitHub Kwessies:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Kwessies:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Voorbeeld Plugins:** [Website](https://wiasoom.com)
- **Webwerf:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Bou iets wonderliks. Deel dit met die wêreld.</em></p>
<p align="center"><em>— Die WIA SOOM Span</em></p>