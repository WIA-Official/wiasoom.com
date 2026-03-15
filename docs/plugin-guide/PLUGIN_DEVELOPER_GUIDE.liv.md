<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Izveidojiet savu spraudni 5 minūtēs.</strong></p>
<p align="center">Izveidojiet jaudīgas servera rīkus, informācijas paneļus un automatizācijas — tieši WIA SOOM iekšienē.</p>

---

## Satura rādītājs

- [1. daļa: Ātrā uzsākšana — Jūsu pirmais spraudnis 5 minūtēs](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [2. daļa: Spraudņa konteksta API atsauce](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [3. daļa: Pielāgota UI izveide ar Webviews](#part-3-building-custom-ui-with-webviews)
- [4. daļa: Jūsu spraudņa publicēšana](#part-4-publishing-your-plugin)
- [5. daļa: Labākās prakses](#part-5-best-practices)
- [6. daļa: Reālas pasaules piemēri](#part-6-real-world-examples)
- [Pielikums: Kategorijas un ikonas](#appendix-categories--icons)

---

## 1. daļa: Ātrā uzsākšana ��� Jūsu pirmais spraudnis 5 minūtēs

### Ko jūs izveidosiet

"Hello World" spraudnis, kas pievieno pogu sānu joslai. Kad to noklikšķinās, tas parādīs paziņojumu.

### 1. solis: Izveidojiet spraudņa mapi
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 2. solis: Izveidojiet package.json
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
**Nepieciešamie lauki:** `name`, `version`, `description`, `author`, `main`

### 3. solis: Izveidojiet index.js
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
### 4. solis: Restartējiet WIA SOOM

Restartējiet lietotni (vai pārslēdziet spraudni izslēgt/ieslēgt Iestatījumos → Spraudņi).

Jums vajadzētu redzēt **"Hello World"** pogu sānu joslā. Noklikšķiniet uz tās — jūs redzēsiet veiksmīgu paziņojumu!

### Kā tas darbojas
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

## 2. daļa: Spraudņa konteksta API atsauce

Kad jūsu `activate(context)` funkcija tiek izsaukta, `context` (vai `ctx`) nodrošina šos API:
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

### `ctx.terminal` — Izpildīt komandas attālinātajos serveros

#### `terminal.send(sessionId, data)`

Nosūtiet komandu (vai jebkādus datus) uz aktīvu termināla sesiju.

| Parametrs | Tips | Apraksts |
|-----------|------|-------------|
| `sessionId` | `string` | Termināla sesija, uz kuru nosūtīt |
| `data` | `string` | Komanda vai dati, ko nosūtīt |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Pieteikties visam izvadei no termināla sesijas. Atgriež **atcelt funkciju**.

| Parametrs | Tips | Apraksts |
|-----------|------|-------------|
| `sessionId` | `string` | Termināla sesija, ko uzraudzīt |
| `callback` | `(data: string) => void` | Tiek izsaukts ar katru izvades daļu |
| **Atgriež** | `() => void` | Izsauciet to, lai pārtrauktu klausīšanos |
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
**Svarīgi:** Vienmēr saglabājiet atcelt funkciju un izsauciet to `deactivate()`, lai novērstu atmiņas noplūdes.

---

### `ctx.sftp` — Failu pārsūtīšana

> **Statuss: Drīzumā** — SFTP API ir definēts, bet vēl nav pieslēgts lietotnes SFTP dzinējam. `list()` pašlaik atgriež tukšu masīvu, un `upload()`/`download()` ir bezdarbības. Tas tiks pilnībā ieviests nākamajā laidienā. Pašlaik izmantojiet `ctx.terminal.send()` ar `scp` vai `rsync` komandām kā apiet.

#### `sftp.list(sessionId, path)`

Uzskaitiet failus attālinātajā direktorijā.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Augšupielādējiet failu no vietējās mašīnas uz attālināto serveri.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Lejupielādējiet failu no attālinātā servera uz vietējo mašīnu.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Apiet (līdz SFTP API ir aktīvs):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Lietotāja interfeiss

#### `ui.addSidebarButton(options)`

Pievienojiet pogu WIA SOOM sānu joslai.

| Opcija | Tips | Nepieciešams | Apraksts |
|--------|------|----------|-------------|
| `id` | `string` | Nē | Unikāls ID (pēc noklusējuma spraudņa nosaukums) |
| `icon` | `string` | Jā | Lucide ikonas nosaukums (piemēram, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Jā | Pogas teksts, kas parādās sānu joslā |
| `onClick` | `() => void` | Jā | Funkcija, kas tiek izsaukta, kad poga tiek noklikšķināta |
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
**Ikonu atsauce:** Pārlūkojiet visas pieejamās ikonas vietnē [lucide.dev/icons](https://lucide.dev/icons)

> **Saderības piezīme:** Daži vecāki spraudņi izmanto pozicionālas argumentus, piemēram, `addSidebarButton(id, icon, label, onClick)`. Oficiālā API izmanto **opciju objektu**, kā dokumentēts iepriekš. Vienmēr izmantojiet objektu stilu jauniem spraudņiem.

#### `ui.openWebview(options)`

Atveriet uznirstošo logu ar pielāgotu HTML saturu. Tā ir veids, kā izveidot bagātīgas UI.

| Opcija | Tips | Apraksts |
|--------|------|-------------|
| `title` | `string` | Loga nosaukums |
| `html` | `string` | Pilns HTML saturs, ko attēlot |
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
> Vaata [Osa 3](#part-3-building-custom-ui-with-webviews) edasijõudnud webview mustrite kohta.

#### `ui.showNotification(type, message)`

Näita teavitust.

| Parameeter | Tüüp | Kirjeldus |
|------------|------|-----------|
| `type` | `'success' \| 'error' \| 'info'` | Teavituse stiil |
| `message` | `string` | Näidatav tekst |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Lisa püsiv tekstielement alumisse olekureale.

| Parameeter | Tüüp | Kirjeldus |
|------------|------|-----------|
| `id` | `string` | Unikaalne ID selle olekuelemendi jaoks |
| `text` | `string` | Näidatav tekst |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Püsiv salvestus

Plugin'i seaded salvestatakse püsivalt faili `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Loe salvestatud väärtust.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Tagastab `undefined`, kui võti ei eksisteeri.

#### `settings.set(key, value)`

Salvesta väärtus. Toetab stringe, numbreid, booleane, massiive ja objekte.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Näide: Kasutaja eelistuste meelespidamine**
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

### `ctx.ai` — AI integreerimine

> **Olek: Peagi tulekul** — AI API on määratletud, kuid pole veel Soomy'ga ühendatud. Praegu tagastab `{ response: 'AI not yet connected' }`. Täielik AI integreerimine on plaanitud tulevase väljaande jaoks.

#### `ai.chat(messages, options?)`

Saada sõnumeid AI assistendile (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Osa 3: Kohandatud UI loomine Webview'dega

`openWebview()` API võimaldab sul luua juhtpaneeli UI-d HTML, CSS ja JavaScriptiga — kõik pop-up aknas.

> **Oluline piirang:** Webview'd on **ainult kuvamiseks**. Need ei saa tagasi kutsuda plugin API-sid (`ctx.settings`, `ctx.terminal` jne). Kasuta külgriba nuppe kõigi kasutaja toimingute jaoks ja kasuta `openWebview()` praeguse oleku kuvamiseks. Kui vajad interaktiivseid funktsioone, käivita need külgriba nuppudest ja ava webview uuesti, et kuvamine värskendada.

### Muster: Terminali käsk → Tulemuse analüüs → Kuvamine HTML-is

See on kõige levinum plugin'i muster. Sa käivitad käsu, analüüsid tulemuse ja kuvad selle visuaalselt.
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
### Muster: Interaktiivne juhtpaneel automaatse värskendusega
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
### Muster: Seadete kuvamine Webview's

> **Märkus:** Webview'd on ainult kuvamiseks — need ei saa tagasi kutsuda plugin API-sid. Kasuta `ctx.settings` oma külgriba nuppude käitlejates seadete muutmiseks ja kasuta `openWebview()` praeguse oleku kuvamiseks.
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

## Osa 4: Sinu plugin'i avaldamine

### Samm 1: Testi kohapeal

1. Kopeeri oma plugin `~/.wia-soom/plugins/{your-plugin}/`
2. Taaskäivita WIA SOOM
3. Kontrolli, et see töötab: külgriba nupp ilmub, funktsioonid töötavad õigesti
4. Testi äärmuslikke juhtumeid: mis juhtub, kui terminal ei ole ühendatud?

### Samm 2: Valmistu esitamiseks

Sinu plugin'i kaust peab sisaldama:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Nepieciešamie `package.json` lauki:**

| Lauks | Apraksts | Piemērs |
|-------|-------------|---------|
| `name` | Unikāls kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantiskā versija | `"1.0.0"` |
| `description` | Vienas rindas apraksts | `"Monitors nginx access logs in real-time"` |
| `author` | Jūsu vārds | `"John Doe"` |
| `main` | Ieejas punkts | `"index.js"` |

**Nepieciešamie lauki:**

| Lauks | Apraksts |
|-------|-------------|
| `license` | Licences veids (ieteicams MIT) |
| `keywords` | Meklēšanas tagu masīvs |
| `soom.minVersion` | Minimālā WIA SOOM versija, kas nepieciešama |

### 3. solis: Iesniegt Plugin Reģistrā

1. ****Package** your plugin as a ZIP file
2. **Pievienojiet** savu pluginu `plugins/{your-plugin-name}/`
3. **Iesniedziet** Pull Request

### 4. solis: Pārskats un apstiprinājums

Mēs pārskatām katru pluginu attiecībā uz:

- **Drošību** — nav bīstamu API (skat. [Drošības noteikumi](#security-rules))
- **Kvalitāti** — vai tas strādā? Vai kods ir tīrs?
- **Noderīgumu** — vai tas risina reālu problēmu?

Pēc apstiprinājuma:
1. Jūsu plugins tiek pievienots `registry.json`
2. ZIP pakotne tiek izveidota `dist/`
3. Jūsu plugins parādās **Plugin Store** visiem WIA SOOM lietotājiem!

---

## 5. daļa: Labākās prakses

### Drošības noteikumi

Šie noteikumi ir **obligāti**. Pluginus, kas tos pārkāpj, noraidīs.

| Noteikums | Kāpēc |
|------|-----|
| **NEDRĪKST** izmantot `eval()` vai `new Function()` | Koda injekcijas risks |
| **NEDRĪKST** izmantot `child_process`, `exec()`, `spawn()` | Izmantojiet tikai `ctx.terminal.send()` komandām |
| **NEDRĪKST** iegūt ārējās URL | Izņēmums: `wiasoom.com` API galapunkti |
| **NEDRĪKST** piekļūt `process.env` | Vides mainīgie var saturēt noslēpumus |
| **NEDRĪKST** tieši izmantot `require('fs')` | Izmantojiet `ctx.settings` glabāšanai, `ctx.sftp` failu pārsūtīšanai |
| **JĀ** izmantot `ctx.terminal.send()` visām attālajām komandām | Tas notiek caur drošo SSH kanālu |
| **JĀ** sakārtot `deactivate()` | Noņemt klausītājus, notīrīt intervālus |

### Kļūdu apstrāde

Vienmēr iesaiņojiet riskantās operācijas try/catch:
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
### Sakārtošana `deactivate()`

Ja jūsu plugins izveido intervālus, klausītājus vai abonēšanas — sakārtojiet tos:
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
### i18n Atbalsts

WIA SOOM atbalsta 254 valodas. Lai padarītu jūsu pluginu etiķeti tulkojamu, izmantojiet vienkāršu pieeju:
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

## 6. daļa: Reālas pasaules piemēri

### Piemērs 1: Servera diska pārbaudītājs

Izpilda `df -h` attālajā serverī un rāda izmantoto/pieejamo vietu statusa joslā.
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

### Piemērs 2: TODO pārvaldnieks

Plugin, kas pārvalda TODO sarakstu, izmantojot iestatījumus pastāvīgai glabāšanai un webview attēlošanai.

> **Dizaina paraugs:** Tā kā webviews nevar tieši izsaukt pluginu API, šis plugins izmanto "snapshot" pieeju — tas lasa TODO no iestatījumiem, attēlo tos kā tikai lasāmu HTML un nodrošina sānu joslā balstītas darbības priekšmetu pievienošanai. Webview ir **attēlošanas** slānis, nevis interaktīva forma.
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

### Piemērs 3: Kļūdu uzraudzītājs

Uzrauga termināla izvadi un nosūta paziņojumu, kad tiek noteikti konkrēti paraugi.
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

## Pielikums: Kategorijas un Ikonas

### Spraudņu Kategorijas (29)

Izmantojiet šīs savā `package.json` `keywords` vai iesniedzot reģistrā:

| Kategorija | Apraksts |
|------------|----------|
| `server` | Vispārēja servera pārvaldība |
| `devtools` | Izstrādes rīki |
| `calculator` | Kalkulatori un konvertētāji |
| `simulator` | Simulatori |
| `game` | Termināla spēles |
| `business` | Biznesa rīki |
| `security` | Drošība un auditi |
| `web` | Tīmekļa servera pārvaldība |
| `education` | Izglītības rīki |
| `health` | Rīki, kas saistīti ar veselību |
| `islamic` | Islāma rīki (lūgšanu laiki u.c.) |
| `science` | Zinātniskie rīki |
| `quantum` | Kvantu skaitļošanas rīki |
| `ai` | Mākslīgā intelekta rīki |
| `biotech` | Biotehnoloģiju rīki |
| `space` | Kosmosa un astronomijas rīki |
| `network` | Tīkla rīki |
| `database` | Datu bāzu pārvaldība |
| `monitoring` | Servera uzraudzība |
| `devops` | DevOps un CI/CD |
| `utility` | Vispārēji utilīti |
| `design` | Dizaina rīki |
| `ecommerce` | E-komercijas rīki |
| `automation` | Automatizācijas rīki |
| `kpop` | K-pop saistīti rīki |
| `accessibility` | Pieejamības rīki |
| `analytics` | Analītika un ziņošana |
| `wia` | WIA ekosistēmas rīki |
| `all` | Parādās visās kategorijās |

### Ieteiktās Ikonas (Lucide)

| Ikonas Nosaukums | Izmantošanai |
|------------------|--------------|
| `server` | Servera pārvaldība |
| `shield` | Drošība |
| `database` | Datu bāze |
| `activity` | Uzraudzība |
| `terminal` | Termināla rīki |
| `code` | Izstrāde |
| `hard-drive` | Disku/krātuves |
| `network` | Tīklveidošana |
| `lock` | Autentifikācija/šifrēšana |
| `eye` | Uzraudzīšana/monitorings |
| `check-square` | Uzdevumi/TODO |
| `layout-dashboard` | Vadības paneļi |
| `settings` | Konfigurācija |
| `zap` | Automatizācija |
| `globe` | Tīmeklis/starptautisks |

Pārlūkojiet visas 1,500+ ikonas: [lucide.dev/icons](https://lucide.dev/icons)

---

## Nepieciešama Palīdzība?

- **GitHub Problēmas:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Spraudņu Problēmas:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Piemēru Spraudņi:** [Website](https://wiasoom.com)
- **Mājaslapa:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Izveidojiet kaut ko pārsteidzošu. Dalieties ar to ar pasauli.</em></p>
<p align="center"><em>— WIA SOOM komanda</em></p>