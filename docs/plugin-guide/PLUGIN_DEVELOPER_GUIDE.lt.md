<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin kūrimo vadovas</h1>
<p align="center"><strong>Sukurkite savo plugin'ą per 5 minutes.</strong></p>
<p align="center">Kurkite galingus serverių įrankius, informacines sistemas ir automatizacijas — tiesiai WIA SOOM viduje.</p>

---

## Turinys

- [1 dalis: Greitas startas — Jūsų pirmasis plugin'as per 5 minutes](#1-dalis-greitas-startas--jūsų-pirmasis-pluginas-per-5-minutes)
- [2 dalis: Plugin konteksto API nuoroda](#2-dalis-plugin-konteksto-api-nuoroda)
  - [ctx.terminal](#ctxterminal--vykdyti-komandas-nuotoliniuose-serveriuose)
  - [ctx.sftp](#ctxsftp--failų-perdavimas)
  - [ctx.ui](#ctxui--vartotojo-sąsaja)
  - [ctx.settings](#ctxsettings--ilgalaikė-atmintis)
  - [ctx.ai](#ctxai--ai-integracija)
- [3 dalis: Pasiruošimas individualiai UI su Webviews](#3-dalis-pasiruošimas-individualiai-ui-su-webviews)
- [4 dalis: Publikuokite savo plugin'ą](#4-dalis-publikuokite-savo-pluginą)
- [5 dalis: Geriausios praktikos](#5-dalis-geriausios-praktikos)
- [6 dalis: Realių pavyzdžių](#6-dalis-realių-pavyzdžių)
- [Priedas: Kategorijos ir ikonėlės](#priedas-kategorijos-ir-ikonėlės)

---

## 1 dalis: Greitas startas — Jūsų pirmasis plugin'as per 5 minutes

### Ką sukursite

"Hello World" plugin'as, kuris prideda mygtuką prie šoninės juostos. Paspaudus jį, pasirodo pranešimas.

### 1 žingsnis: Sukurkite plugin'o aplanką
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 2 žingsnis: Sukurkite package.json
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
**Privalomi laukai:** `name`, `version`, `description`, `author`, `main`

### 3 žingsnis: Sukurkite index.js
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
### 4 žingsnis: Perkraukite WIA SOOM

Perkraukite programą (arba perjunkite plugin'ą išjungti/įjungti Nustatymuose → Plugin'ai).

Turėtumėte matyti **"Hello World"** mygtuką šoninėje juostoje. Paspauskite jį — pamatysite sėkmės pranešimą!

### Kaip tai veikia
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

## 2 dalis: Plugin konteksto API nuoroda

Kai jūsų `activate(context)` funkcija yra iškviečiama, `context` (arba `ctx`) suteikia šias API:
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

### `ctx.terminal` — Vykdyti komandas nuotoliniuose serveriuose

#### `terminal.send(sessionId, data)`

Išsiųskite komandą (arba bet kokius duomenis) į aktyvią terminalo sesij��.

| Parametras | Tipas | Aprašymas |
|------------|-------|-----------|
| `sessionId` | `string` | Terminalo sesija, į kurią siunčiama |
| `data` | `string` | Komanda arba duomenys, kuriuos reikia siųsti |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Prenumeruokite visą išvestį iš terminalo sesijos. Grąžina **atsijungimo funkciją**.

| Parametras | Tipas | Aprašymas |
|------------|-------|-----------|
| `sessionId` | `string` | Terminalo sesija, kurią stebėsite |
| `callback` | `(data: string) => void` | Iškviečiama su kiekvienu išvesties fragmentu |
| **Grąžina** | `() => void` | Iškvieskite tai, kad nustotumėte klausytis |
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
**Svarbu:** Visada išsaugokite atsijungimo funkciją ir iškvieskite ją `deactivate()`, kad išvengtumėte atminties nuotėkio.

---

### `ctx.sftp` — Failų perdavimas

> **Būsena: Greitai** — SFTP API yra apibrėžta, tačiau dar nėra prijungta prie programos SFTP variklio. `list()` šiuo metu grąžina tuščią masyvą, o `upload()`/`download()` yra be veiksmų. Tai bus visiškai įgyvendinta ateityje. Kol kas naudokite `ctx.terminal.send()` su `scp` arba `rsync` komandomis kaip apėjimą.

#### `sftp.list(sessionId, path)`

Išvardinkite failus nuotoliniame kataloge.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Įkelkite failą iš vietinio kompiuterio į nuotolinį serverį.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Atsisiųskite failą iš nuotolinio serverio į vietinį kompiuterį.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Apėjimas (kol SFTP API bus gyvas):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Vartotojo sąsaja

#### `ui.addSidebarButton(options)`

Pridėkite mygtuką prie WIA SOOM šoninės juostos.

| Pasirinktis | Tipas | Privaloma | Aprašymas |
|-------------|-------|-----------|-----------|
| `id` | `string` | Ne | Unikalus ID (numatytas plugin'o pavadinimas) |
| `icon` | `string` | Taip | Lucide ikonėlės pavadinimas (pvz., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Taip | Mygtuko tekstas, rodomas šoninėje juostoje |
| `onClick` | `() => void` | Taip | Funkcija, kuri iškviečiama, kai mygtukas paspaudžiamas |
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
**Ikonėms nuoroda:** Peržiūrėkite visas galimas ikonėles adresu [lucide.dev/icons](https://lucide.dev/icons)

> **Suderinamumo pastaba:** Kai kurie senesni plugin'ai naudoja pozicinius argumentus, tokius kaip `addSidebarButton(id, icon, label, onClick)`. Oficialus API naudoja **parinkčių objektą**, kaip aprašyta aukščiau. Visada naudokite objekto stilių naujiems plugin'ams.

#### `ui.openWebview(options)`

Atidarykite iššokantį langą su individualiu HTML turiniu. Taip kuriate turtingas vartotojo sąsajas.

| Pasirinktis | Tipas | Aprašymas |
|-------------|-------|-----------|
| `title` | `string` | Langų pavadinimas |
| `html` | `string` | Pilnas HTML turinys, kuris bus atvaizduotas |
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
> Žr. [3 dalis](#part-3-building-custom-ui-with-webviews) dėl pažangių webview modelių.

#### `ui.showNotification(type, message)`

Rodo toast pranešimą.

| Parametras | Tipas | Aprašymas |
|------------|-------|-----------|
| `type` | `'success' \| 'error' \| 'info'` | Pranešimo stilius |
| `message` | `string` | Tekstas, kuris bus rodomas |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Prideda nuolatinį teksto elementą į apatinę būsenos juostą.

| Parametras | Tipas | Aprašymas |
|------------|-------|-----------|
| `id` | `string` | Unikalus ID šiam būsenos elementui |
| `text` | `string` | Tekstas, kuris bus rodomas |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Nuolatinė saugykla

Plugin nustatymai saugomi nuolat `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Perskaityti išsaugotą reikšmę.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Grąžina `undefined`, jei raktas neegzistuoja.

#### `settings.set(key, value)`

Išsaugoti reikšmę. Palaiko tekstus, skaičius, boolean, masyvus ir objektus.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Pavyzdys: Prisiminti vartotojo nustatymus**
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

### `ctx.ai` — AI integracija

> **Būsena: Artimiausiu metu** — AI API yra apibrėžtas, bet dar nėra prijungtas prie Soomy. Šiuo metu grąžina `{ response: 'AI not yet connected' }`. Pilna AI integracija planuojama ateityje.

#### `ai.chat(messages, options?)`

Siųsti žinutes AI asistentui (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## 3 dalis: Pasiruošimas specializuotai UI su Webviews

`openWebview()` API leidžia kurti valdymo skydelių UI su HTML, CSS ir JavaScript — viską viduje iššokančio lango.

> **Svarbus apribojimas:** Webviews yra **tik rodymo**. Jie negali grąžinti į plugin API (`ctx.settings`, `ctx.terminal` ir kt.). Naudokite šoninius mygtukus visiems vartotojo veiksmams, ir naudokite `openWebview()` rodyti dabartinę būseną. Jei reikia interaktyvių funkcijų, suaktyvinkite jas iš šoninių mygtukų ir vėl atidarykite webview, kad atnaujintumėte rodymą.

### Modelis: Terminalo komanda → Išvesties analizė → Rodyti HTML

Tai yra dažniausiai pasitaikantis plugin modelis. Jūs vykdote komandą, analizuojate rezultatą ir vizualiai jį rodyti.
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
### Modelis: Interaktyvus valdymo skydelis su automatinio atnaujinimo funkcija
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
### Modelis: Nustatymų rodymas webview

> **Pastaba:** Webviews yra tik rodymo — jie negali grąžinti į plugin API. Naudokite `ctx.settings` savo šoninių mygtukų tvarkytuvuose, kad pakeistumėte nustatymus, ir naudokite `openWebview()` rodyti dabartinę būseną.
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

## 4 dalis: Jūsų plugin paskelbimas

### 1 žingsnis: Išbandykite lokaliai

1. Nukopijuokite savo plugin į `~/.wia-soom/plugins/{your-plugin}/`
2. Perkraukite WIA SOOM
3. Patikrinkite, ar veikia: šoninis mygtukas pasirodo, funkcijos veikia teisingai
4. Išbandykite kraštutinius atvejus: kas nutinka, jei terminalas nėra prijungtas?

### 2 žingsnis: Pasiruoškite pateikimui

Jūsų plugin aplankas turi turėti:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Reikalingi `package.json` laukai:**

| Laukas | Aprašymas | Pavyzdys |
|--------|-----------|----------|
| `name` | Unikalus kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantinė versija | `"1.0.0"` |
| `description` | Vienos eilutės aprašymas | `"Stebi nginx prieigos žurnalus realiuoju laiku"` |
| `author` | Jūsų vardas | `"John Doe"` |
| `main` | Įėjimo taškas | `"index.js"` |

**Pasirinktiniai laukai:**

| Laukas | Aprašymas |
|--------|-----------|
| `license` | Licencijos tipas (rekomenduojama MIT) |
| `keywords` | Paieškos žymų masyvas |
| `soom.minVersion` | Minimalus reikalaujamas WIA SOOM versijos numeris |

### 3 žingsnis: Pateikite į Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Pridėkite** savo pluginą į `plugins/{your-plugin-name}/`
3. **Pateikite** Pull Request

### 4 žingsnis: Peržiūra ir patvirtinimas

Mes peržiūrime kiekvieną pluginą dėl:

- **Saugumo** — jokių pavojingų API (žr. [Saugumo taisykles](#security-rules))
- **Kokybės** — ar jis veikia? Ar kodas švarus?
- **Naudingumo** — ar jis sprendžia realią problemą?

Po patvirtinimo:
1. Jūsų pluginas pridedamas į `registry.json`
2. ZIP paketas sukuriamas `dist/`
3. Jūsų pluginas pasirodo **Plugin Store** visiems WIA SOOM vartotojams!

---

## 5 dalis: Geriausios praktikos

### Saugumo taisyklės

Šios taisyklės yra **privalomos**. Pluginai, kurie jas pažeidžia, bus atmesti.

| Taisyklė | Kodėl |
|-----------|-------|
| **NIEKADA** nenaudokite `eval()` ar `new Function()` | Kodo injekcijos rizika |
| **NIEKADA** nenaudokite `child_process`, `exec()`, `spawn()` | Naudokite tik `ctx.terminal.send()` komandoms |
| **NIEKADA** negaunate iš išorinių URL | Išimtis: `wiasoom.com` API galiniai taškai |
| **NIEKADA** nepasiekite `process.env` | Aplinkos kintamieji gali turėti paslapčių |
| **NIEKADA** nenaudokite `require('fs')` tiesiogiai | Naudokite `ctx.settings` saugojimui, `ctx.sftp` failų perdavimui |
| **PRIVALOMA** naudoti `ctx.terminal.send()` visoms nuotolinėms komandoms | Tai vyksta per saugų SSH kanalą |
| **PRIVALOMA** išvalyti `deactivate()` | Pašalinkite klausytuvus, išvalykite intervalus |

### Klaidos tvarkymas

Visada apvyniokite rizikingas operacijas į try/catch:
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
### Valymas deactivate()

Jei jūsų pluginas sukuria intervalus, klausytuvus ar prenumeratas — juos išvalykite:
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
### i18n Parama

WIA SOOM palaiko 254 kalbas. Norėdami, kad jūsų pluginas būtų verčiamas, naudokite paprastą metodą:
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

## 6 dalis: Realių pavyzdžių

### Pavyzdys 1: Serverio disko tikrintuvas

Vykdo `df -h` nuotoliniame serveryje ir rodo naudojamą/laisvą vietą būsenos juostoje.
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

### Pavyzdys 2: TODO valdytojas

Pluginas, kuris valdo TODO sąrašą naudodamas nustatymus nuolatiniam saugojimui ir webview rodymui.

> **Dizaino modelis:** Kadangi webviews negali tiesiogiai kviesti pluginų API, šis pluginas naudoja "snapshot" metodą — jis skaito TODO iš nustatymų, atvaizduoja juos kaip tik skaitymo HTML, ir teikia veiksmus šoninėje juostoje, skirtus elementų pridėjimui. Webview yra **rodymo** sluoksnis, o ne interaktyvi forma.
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

### Pavyzdys 3: Klaidos stebėtojas

Stebi terminalo išvestį ir siunčia pranešimą, kai aptinkami konkretūs modeliai.
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

## Priedas: Kategorijos ir Piktogramos

### Priedų Kategorijos (29)

Naudokite šias kategorijas savo `package.json` `keywords` arba pateikdami registrui:

| Kategorija | Aprašymas |
|------------|-----------|
| `server` | Bendras serverio valdymas |
| `devtools` | Kūrimo įrankiai |
| `calculator` | Kalkuliatoriai ir konverteriai |
| `simulator` | Simuliatoriai |
| `game` | Terminalo žaidimai |
| `business` | Verslo įrankiai |
| `security` | Saugumo ir audito įrankiai |
| `web` | Interneto serverio valdymas |
| `education` | Švietimo įrankiai |
| `health` | Sveikatos susiję įrankiai |
| `islamic` | Islamo įrankiai (maldos laikai ir kt.) |
| `science` | Moksliniai įrankiai |
| `quantum` | Kvantinės kompiuterijos įrankiai |
| `ai` | Dirbtinio intelekto įrankiai |
| `biotech` | Biotechnologijų įrankiai |
| `space` | Kosmoso ir astronomijos įrankiai |
| `network` | Tinklo įrankiai |
| `database` | Duomenų bazės valdymas |
| `monitoring` | Serverio stebėjimas |
| `devops` | DevOps ir CI/CD |
| `utility` | Bendri įrankiai |
| `design` | Dizaino įrankiai |
| `ecommerce` | E-prekybos įrankiai |
| `automation` | Automatizavimo įrankiai |
| `kpop` | K-pop susiję įrankiai |
| `accessibility` | Prieinamumo įrankiai |
| `analytics` | Analizės ir ataskaitos |
| `wia` | WIA ekosistemos įrankiai |
| `all` | Pasirodo visose kategorijose |

### Rekomenduojamos Piktogramos (Lucide)

| Piktogramos Pavadinimas | Naudojama |
|-------------------------|-----------|
| `server` | Serverio valdymas |
| `shield` | Saugumas |
| `database` | Duomenų bazė |
| `activity` | Stebėjimas |
| `terminal` | Terminalo įrankiai |
| `code` | Kūrimas |
| `hard-drive` | Diskas/sandėliavimas |
| `network` | Tinklavimas |
| `lock` | Autentifikavimas/šifravimas |
| `eye` | Stebėjimas/monitoringas |
| `check-square` | Užduotys/TODO |
| `layout-dashboard` | Valdymo skydeliai |
| `settings` | Konfigūracija |
| `zap` | Automatizavimas |
| `globe` | Internetas/tarptautinis |

Peržiūrėkite visas 1,500+ piktogramų: [lucide.dev/icons](https://lucide.dev/icons)

---

## Reikia Pagalbos?

- **GitHub Problemos:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Priedų Problemos:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Pavyzdiniai Priedai:** [Website](https://wiasoom.com)
- **Tinklalapis:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Sukurkite kažką nuostabaus. Pasidalykite su pasauliu.</em></p>
<p align="center"><em>— WIA SOOM komanda</em></p>