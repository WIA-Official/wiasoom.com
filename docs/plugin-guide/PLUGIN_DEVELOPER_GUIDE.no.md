<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Utviklerguide</h1>
<p align="center"><strong>Bygg din egen plugin på 5 minutter.</strong></p>
<p align="center">Lag kraftige serververktøy, dashbord og automasjoner — rett inne i WIA SOOM.</p>

---

## Innholdsfortegnelse

- [Del 1: Hurtigstart — Din første plugin på 5 minutter](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Del 2: Plugin Kontekst API Referanse](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Del 3: Bygge tilpasset UI med Webviews](#part-3-building-custom-ui-with-webviews)
- [Del 4: Publisere din plugin](#part-4-publishing-your-plugin)
- [Del 5: Beste praksis](#part-5-best-practices)
- [Del 6: Virkelige eksempler](#part-6-real-world-examples)
- [Appendiks: Kategorier & Ikoner](#appendix-categories--icons)

---

## Del 1: Hurtigstart — Din første plugin på 5 minutter

### Hva du vil bygge

En "Hello World" plugin som legger til en knapp i sidepanelet. Når den klikkes, viser den en varsling.

### Steg 1: Opprett plugin-mappen
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Steg 2: Opprett package.json
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
**Obligatoriske felt:** `name`, `version`, `description`, `author`, `main`

### Steg 3: Opprett index.js
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
### Steg 4: Start WIA SOOM på nytt

Start appen på nytt (eller slå pluginen av/på i Innstillinger → Plugins).

Du bør se en **"Hello World"** knapp i sidepanelet. Klikk på den — du vil se en suksessvarsling!

### Hvordan det fungerer
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

## Del 2: Plugin Kontekst API Referanse

Når din `activate(context)` funksjon blir kalt, gir `context` (eller `ctx`) disse API-ene:
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

### `ctx.terminal` — Kjør kommandoer på eksterne servere

#### `terminal.send(sessionId, data)`

Send en kommando (eller data) til en aktiv terminaløkt.

| Parameter | Type | Beskrivelse |
|-----------|------|-------------|
| `sessionId` | `string` | Terminaløkten å sende til |
| `data` | `string` | Kommandoen eller dataene som skal sendes |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abonner på all utdata fra en terminaløkt. Returnerer en **avmeldingsfunksjon**.

| Parameter | Type | Beskrivelse |
|-----------|------|-------------|
| `sessionId` | `string` | Terminaløkten å overvåke |
| `callback` | `(data: string) => void` | Kalles med hver del av utdata |
| **Returnerer** | `() => void` | Kall dette for å slutte å lytte |
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
**Viktig:** Husk alltid å lagre avmeldingsfunksjonen og kall den i `deactivate()` for å forhindre minnelekkasjer.

---

### `ctx.sftp` — Filoverføring

> **Status: Kommer snart** — SFTP API-en er definert, men er ennå ikke koblet til appens SFTP-motor. `list()` returnerer for øyeblikket et tomt array, og `upload()`/`download()` gjør ingenting. Dette vil bli fullt implementert i en fremtidig utgivelse. For nå, bruk `ctx.terminal.send()` med `scp` eller `rsync` kommandoer som en midlertidig løsning.

#### `sftp.list(sessionId, path)`

Liste filer i en ekstern katalog.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Last opp en fil fra lokal maskin til ekstern server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Last ned en fil fra ekstern server til lokal maskin.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Midler (til SFTP API er aktiv):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Brukergrensesnitt

#### `ui.addSidebarButton(options)`

Legg til en knapp i WIA SOOM sidepanelet.

| Alternativ | Type | Obligatorisk | Beskrivelse |
|------------|------|--------------|-------------|
| `id` | `string` | Nei | Unik ID (standard til plugin-navn) |
| `icon` | `string` | Ja | Lucide ikonnavn (f.eks. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ja | Knappetekst vist i sidepanelet |
| `onClick` | `() => void` | Ja | Funksjon som kalles når knappen klikkes |
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
**Ikonreferanse:** Bla gjennom alle tilgjengelige ikoner på [lucide.dev/icons](https://lucide.dev/icons)

> **Kompatibilitetsnotat:** Noen eldre plugins bruker posisjonelle argumenter som `addSidebarButton(id, icon, label, onClick)`. Den offisielle API-en bruker et **alternativsobjekt** som dokumentert ovenfor. Bruk alltid objektstilen for nye plugins.

#### `ui.openWebview(options)`

Åpne et popup-vindu med tilpasset HTML-innhold. Slik bygger du rike brukergrensesnitt.

| Alternativ | Type | Beskrivelse |
|------------|------|-------------|
| `title` | `string` | Vinduets tittel |
| `html` | `string` | Fullt HTML-innhold som skal gjengis |
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
> Se [Del 3](#part-3-building-custom-ui-with-webviews) for avanserte webview-mønstre.

#### `ui.showNotification(type, message)`

Vis en toast-varsling.

| Parameter | Type | Beskrivelse |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Varslingsstil |
| `message` | `string` | Tekst som skal vises |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Legg til et vedvarende tekstelement i den nederste statuslinjen.

| Parameter | Type | Beskrivelse |
|-----------|------|-------------|
| `id` | `string` | Unik ID for dette status-elementet |
| `text` | `string` | Tekst som skal vises |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Vedvarende lagring

Plugin-innstillinger lagres permanent i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Les en lagret verdi.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Returnerer `undefined` hvis nøkkelen ikke eksisterer.

#### `settings.set(key, value)`

Lagre en verdi. Støtter strenger, tall, boolske verdier, matriser og objekter.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Eksempel: Husk brukerpreferanser**
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

### `ctx.ai` — AI-integrasjon

> **Status: Kommer snart** — AI API-en er definert, men ikke koblet til Soomy ennå. Returnerer for øyeblikket `{ response: 'AI not yet connected' }`. Full AI-integrasjon er planlagt for en fremtidig utgivelse.

#### `ai.chat(messages, options?)`

Send meldinger til AI-assistenten (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Del 3: Bygge tilpasset UI med Webviews

API-en `openWebview()` lar deg bygge dashbord-UI-er med HTML, CSS og JavaScript — alt inne i et popup-vindu.

> **Viktig begrensning:** Webviews er **kun for visning**. De kan ikke kalle tilbake til plugin-API-er (`ctx.settings`, `ctx.terminal`, osv.). Bruk sidepanelknapper for alle brukerhandlinger, og bruk `openWebview()` for å vise nåværende tilstand. Hvis du trenger interaktive funksjoner, utløse dem fra sidepanelknapper og åpne webview-en på nytt for å oppdatere visningen.

### Mønster: Terminalkommando → Parse utdata → Vis i HTML

Dette er det vanligste plugin-mønsteret. Du kjører en kommando, parser resultatet, og viser det visuelt.
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
### Mønster: Interaktivt dashbord med automatisk oppdatering
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
### Mønster: Vise innstillinger i en webview

> **Merk:** Webviews er kun for visning — de kan ikke kalle tilbake til plugin-API-er. Bruk `ctx.settings` i håndtererne for sidepanelknapper for å endre innstillinger, og bruk `openWebview()` for å vise nåværende tilstand.
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

## Del 4: Publisere plugin-en din

### Trinn 1: Test lokalt

1. Kopier plugin-en din til `~/.wia-soom/plugins/{your-plugin}/`
2. Start WIA SOOM på nytt
3. Bekreft at det fungerer: sidepanelknappen vises, funksjoner fungerer som de skal
4. Test kanttilfeller: hva skjer hvis ingen terminal er koblet til?

### Trinn 2: Forbered for innsending

Plugin-mappen din må inneholde:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Nødvendige `package.json` felt:**

| Felt | Beskrivelse | Eksempel |
|-------|-------------|---------|
| `name` | Unik kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantisk versjon | `"1.0.0"` |
| `description` | En-linjers beskrivelse | `"Overvåker nginx tilgangslogger i sanntid"` |
| `author` | Ditt navn | `"John Doe"` |
| `main` | Inngangspunkt | `"index.js"` |

**Valgfri felt:**

| Felt | Beskrivelse |
|-------|-------------|
| `license` | Lisens type (MIT anbefales) |
| `keywords` | Array av søkeord |
| `soom.minVersion` | Minimum WIA SOOM versjon som kreves |

### Trinn 3: Send inn til Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Legg til** pluginen din i `plugins/{your-plugin-name}/`
3. **Send inn** en Pull Request

### Trinn 4: Gjennomgang og godkjenning

Vi gjennomgår hver plugin for:

- **Sikkerhet** — ingen farlige API-er (se [Sikkerhetsregler](#security-rules))
- **Kvalitet** — fungerer det? Er koden ren?
- **Nyttighet** — løser det et reelt problem?

Etter godkjenning:
1. Pluginen din legges til i `registry.json`
2. En ZIP-pakke opprettes i `dist/`
3. Pluginen din vises i **Plugin Store** for alle WIA SOOM-brukere!

---

## Del 5: Beste praksis

### Sikkerhetsregler

Disse reglene er **obligatoriske**. Plugins som bryter dem vil bli avvist.

| Regel | Hvorfor |
|------|-----|
| **ALDRI** bruk `eval()` eller `new Function()` | Risiko for kodeinjeksjon |
| **ALDRI** bruk `child_process`, `exec()`, `spawn()` | Bruk kun `ctx.terminal.send()` for kommandoer |
| **ALDRI** hent eksterne URL-er | Unntak: `wiasoom.com` API-endepunkter |
| **ALDRI** få tilgang til `process.env` | Miljøvariabler kan inneholde hemmeligheter |
| **ALDRI** bruk `require('fs')` direkte | Bruk `ctx.settings` for lagring, `ctx.sftp` for filoverføring |
| **ALDRI** bruk npm eksterne pakker | Kun ren JavaScript — ingen node_modules |
| **MÅ** bruke `ctx.terminal.send()` for alle eksterne kommandoer | Dette går gjennom den sikre SSH-kanalen |
| **MÅ** rydde opp i `deactivate()` | Fjern lyttere, tøm intervaller |

### Feilhåndtering

Pakk alltid risikable operasjoner inn i try/catch:
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
### Rydde opp i deactivate()

Hvis pluginen din oppretter intervaller, lyttere eller abonnementer — rydd dem opp:
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
### i18n Støtte

WIA SOOM støtter 254 språk. For å gjøre pluginetiketten din oversettbar, bruk en enkel tilnærming:
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

## Del 6: Virkelige eksempler

### Eksempel 1: Server Disk Sjekker

Kjører `df -h` på den eksterne serveren og viser brukt/tilgjengelig plass i statuslinjen.
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

### Eksempel 2: TODO Manager

En plugin som administrerer en TODO-liste ved å bruke innstillinger for vedvarende lagring og en webview for visning.

> **Designmønster:** Siden webviews ikke kan kalle plugin-API-er direkte, bruker denne pluginen en "snapshot"-tilnærming — den leser TODO-er fra innstillingene, gjengir dem som skrivebeskyttet HTML, og gir sidebar-baserte handlinger for å legge til elementer. Webviewen er et **visnings** lag, ikke et interaktivt skjema.
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

### Eksempel 3: Feilovervåker

Overvåker terminalutdata og sender en varsling når spesifikke mønstre oppdages.
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

## Vedlegg: Kategorier & Ikoner

### Plugin Kategorier (29)

Bruk disse i din `package.json` `keywords` eller når du sender inn til registeret:

| Kategori | Beskrivelse |
|----------|-------------|
| `server` | Generell serveradministrasjon |
| `devtools` | Utviklingsverktøy |
| `calculator` | Kalkulatorer og konverterere |
| `simulator` | Simulatorer |
| `game` | Terminalspill |
| `business` | Forretningsverktøy |
| `security` | Sikkerhet og revisjon |
| `web` | Webserveradministrasjon |
| `education` | Utdanningsverktøy |
| `health` | Helse-relaterte verktøy |
| `islamic` | Islam-relaterte verktøy (bønn tider, osv.) |
| `science` | Vitenskapelige verktøy |
| `quantum` | Kvanteberegningsverktøy |
| `ai` | AI-drevne verktøy |
| `biotech` | Bioteknologiske verktøy |
| `space` | Rom- og astronomiverktøy |
| `network` | Nettverksverktøy |
| `database` | Databaseadministrasjon |
| `monitoring` | Serverovervåking |
| `devops` | DevOps og CI/CD |
| `utility` | Generelle verktøy |
| `design` | Designverktøy |
| `ecommerce` | E-handelsverktøy |
| `automation` | Automatiseringsverktøy |
| `kpop` | K-pop relaterte verktøy |
| `accessibility` | Tilgjengelighetsverktøy |
| `analytics` | Analyse og rapportering |
| `wia` | WIA økosystemverktøy |
| `all` | Visas i alle kategorier |

### Anbefalte Ikoner (Lucide)

| Ikon Navn | Bruk for |
|-----------|---------|
| `server` | Serveradministrasjon |
| `shield` | Sikkerhet |
| `database` | Database |
| `activity` | Overvåking |
| `terminal` | Terminalverktøy |
| `code` | Utvikling |
| `hard-drive` | Disk/lager |
| `network` | Nettverksverktøy |
| `lock` | Autentisering/kryptering |
| `eye` | Overvåking/monitorering |
| `check-square` | Oppgaver/TODO |
| `layout-dashboard` | Dashbord |
| `settings` | Konfigurasjon |
| `zap` | Automatisering |
| `globe` | Web/internasjonal |

Bla gjennom alle 1,500+ ikoner: [lucide.dev/icons](https://lucide.dev/icons)

---

## Trenger du hjelp?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Eksempel Plugins:** [Website](https://wiasoom.com)
- **Nettside:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Bygg noe fantastisk. Del det med verden.</em></p>
<p align="center"><em>— WIA SOOM Teamet</em></p>