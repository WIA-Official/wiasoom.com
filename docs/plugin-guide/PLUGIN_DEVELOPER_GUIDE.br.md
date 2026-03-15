<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Kinnig Dihunadur</h1>
<p align="center"><strong>Sevelit ho plugin e 5 munut.</strong></p>
<p align="center">Krouit ur c'houlennoù server, dashboardoù, ha stummoù — e-barzh WIA SOOM.</p>

---

## Tamm 1: Distroù Kentañ — Ho Plugin Kentañ e 5 Munut

### Petra a c'houlennit

Ur plugin "Hello World" a ziskouez ur bouton e-barzh ar sidebar. Pa vez kliket, e kinnig ur notifikadur.

### Dremm 1: Krouit ar folder plugin
§§§CHUNK_SEPARATOR§§§
### Dremm 2: Krouit package.json
§§§CHUNK_SEPARATOR§§§
**Ar c'houlennioù ezhomm:** `name`, `version`, `description`, `author`, `main`

### Dremm 3: Krouit index.js
§§§CHUNK_SEPARATOR§§§
### Dremm 4: Restart WIA SOOM

Restartit ar aplikasion (pe troc'het ar plugin war/war e Merañ → Plugins).

Gwelet a rit ur **"Hello World"** bouton e-barzh ar sidebar. Klikit warno — gwelet a rit ur notifikadur a success!

### Penaos e labour
§§§CHUNK_SEPARATOR§§§
---

## Tamm 2: Plugin Context API Referans

Pa vez klevet ho `activate(context)` fonction, e kinnig `context` (pe `ctx`) ar c'houlennioù API-mañ:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Gwelet ar c'homandioù e servijoù pell

#### `terminal.send(sessionId, data)`

Sendit ur c'homand (pe data ebet) d'ur sesion terminal aktif.

| Parameter | Type | Deskrivadur |
|-----------|------|-------------|
| `sessionId` | `string` | Ar sesion terminal da zenv |
| `data` | `string` | Ar c'homand pe data da zenv |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Enrollit d'an holl output a ur sesion terminal. Adkas ur **fonction d'heuliañ**.

| Parameter | Type | Deskrivadur |
|-----------|------|-------------|
| `sessionId` | `string` | Ar sesion terminal da sellet |
| `callback` | `(data: string) => void` | Klevet gant pep chunk a output |
| **Adkas** | `() => void` | Klevit ar c'houlenn-mañ da stankaat ar c'hlevout |
§§§CHUNK_SEPARATOR§§§
**Important:** Dalc'hit da c'houlenn d'heuliañ ha klevit anezhi e `deactivate()` evit evit ar c'hleñvedoù mouezh.

---

### `ctx.sftp` — Distroù file

> **Status: O Tremen** — An API SFTP a zo dastumet met ne vez ket c'hwelet gant moteur SFTP ar aplikasion. `list()` a ro ur c'houlenn gwenn, ha `upload()`/`download()` a zo no-ops. Emañ ar pezh-mañ da vezañ implijet e ur goulenn da zont. Evit ar mare, implijit `ctx.terminal.send()` gant ar c'homandioù `scp` pe `rsync` evel ur c'houlenn.

#### `sftp.list(sessionId, path)`

Lec'hiañ ar fileoù e ur folder pell.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Uploadit ur file eus ar mesin leun da ur servijer pell.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Downloadit ur file eus ur servijer pell da ar mesin leun.
§§§CHUNK_SEPARATOR§§§
**C'houlenn (a-benn ma vo an API SFTP en oad):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Interface implijer

#### `ui.addSidebarButton(options)`

Ouzhpennañ ur bouton e-barzh ar sidebar WIA SOOM.

| Option | Type | Ezhomm | Deskrivadur |
|--------|------|--------|-------------|
| `id` | `string` | Nann | ID unik (a c'houlenn da vezañ anv ar plugin) |
| `icon` | `string` | Ya | Anv ikonoù Lucide (da skouer, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ya | Testenn ar bouton a vez diskouezet e-barzh ar sidebar |
| `onClick` | `() => void` | Ya | Funktion a vez klevet pa vez kliket ar bouton |
§§§CHUNK_SEPARATOR§§§
**Referans ikonoù:** Browseit an ikonoù a zo da gont at [lucide.dev/icons](https://lucide.dev/icons)

> **Notenn a-bouez:** Un nebeud plugins kozh a implij ar c'houlennioù a-berzh evel `addSidebarButton(id, icon, label, onClick)`. An API resmi a implij ur **objekt options** evel diskouezet uhel. Dalc'hit an doare objekt evit plugins nevez.

#### `ui.openWebview(options)`

Ouvrit ur fenestr pop-up gant konteni HTML personel. Setu penaos e krouit UIoù reizh. 

| Option | Type | Deskrivadur |
|--------|------|-------------|
| `title` | `string` | Titl ar fenestr |
| `html` | `string` | Konteni HTML a-bezh da ziskouez |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Gwel [Rann 3](#part-3-building-custom-ui-with-webviews) evit patromoù webview a zo ur c'hwezh.

#### `ui.showNotification(type, message)`

Diskouez ur c'hwezh tost.

| Parameter | Type | Deskrivadur |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stilec'hwezh |
| `message` | `string` | Testenn da diskouez |
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
#### `ui.addStatusBarItem(id, text)`

Ouzhpennañ ur stumm testenn peurliesañ d'ar bar status a-drenv.

| Parameter | Type | Deskrivadur |
|-----------|------|-------------|
| `id` | `string` | ID unik evit ar stumm status-mañ |
| `text` | `string` | Testenn da ziskouez |
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
---

### `ctx.settings` — Stokerezh peurliesañ

Ar c'hendiviz plugin a zo stoket da vat e `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lenn ur gwerzhiad savet.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Distro `undefined` ma n'eo ket ar c'hinnigoù.

#### `settings.set(key, value)`

Stoke ur gwerzhiad. Digeriñ a ra ar stringoù, niveroù, booleanoù, ar re a zo, ha k'objekt.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Dreist: Distro ar c'hendivizoù evit ar user**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Distro AI

> **Statut: O Tremen** — An API AI a zo difennet met ne vez ket c'hwezhet da Soomy. E-giz-se, e distro `{ response: 'AI not yet connected' }`. An distro AI a zo planed evit un embann da zont.

#### `ai.chat(messages, options?)`

Kas meskadoù d'ar sikour AI (Soomy).
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
---

## Rann 3: Krouiñ UI personel gant Webviews

An API `openWebview()` a ro deoc'h ar posibilite da sevel UI dashboard gant HTML, CSS, ha JavaScript — holl e-barzh ur fenestr pop-up.

> **Limitezh pouezus:** Webviews a zo **diskouez hepken**. Ne peuvent ket adkavout e plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Gwelet ar boutonioù sidebar evit an holl obererezhioù user, ha use `openWebview()` da diskouez ar stad a-vremañ. Ma fell deoc'h implij an elfennioù interaktif, adkavout anezho eus ar boutonioù sidebar ha adrouz ar webview da adrefresh ar diskouez.

### Patrom: Command Terminal → Parse Output → Diskouez e HTML

Setu ar patrom plugin ar muiañ a implij. Gwelet un urzh, parse ar result, ha diskouez anezho en ur mod vizuel.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Patrom: Dashboard Interaktif gant Auto-Refresh
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Patrom: Diskouez ar C'hendivizoù e Webview

> **Nota:** Webviews a zo diskouez hepken — ne peuvent ket adkavout e plugin APIs. Gwelet `ctx.settings` e ho kargadoù bouton sidebar evit kemmañ ar c'hendivizoù, ha use `openWebview()` da diskouez ar stad a-vremañ.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Rann 4: Embann ho Plugin

### P step 1: Testezoù e lec'h

1. Kopyit ho plugin da `~/.wia-soom/plugins/{your-plugin}/`
2. Adrestartit WIA SOOM
3. Gwiriait e labour: ar bouton sidebar a zo, ar c'hefridi a labour mat
4. Testit ar c'hazadoù: petra a c'hall bezañ ma n'eo ket un terminal c'hwezhet?

### P step 2: Prepar evit embann

Ho folder plugin a rank bezañ:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Fielioù `package.json` ezhomm:**

| Fiel | Deskrivadur | Digeriñ |
|-------|-------------|---------|
| `name` | ID unik en kebab-case | `"my-awesome-plugin"` |
| `version` | Rannyezhiad semantikel | `"1.0.0"` |
| `description` | Deskrivadur un lizh | `"Monitors nginx access logs in real-time"` |
| `author` | Ho anv | `"John Doe"` |
| `main` | Pentiñ prantad | `"index.js"` |

**Fielioù dibar:**

| Fiel | Deskrivadur |
|-------|-------------|
| `license` | Rummad a c'hontrol (MIT a vez kelaouet) |
| `keywords` | Rummad a zemglevioù | 
| `soom.minVersion` | Rannyezhiad WIA SOOM minim a zo ezhomm |

### P step 3: Kas da Rannerezh ar Plugin

1. ****Package** your plugin as a ZIP file
2. **Ouzhpennañ** ho plugin da `plugins/{ho-plugin-anv}/`
3. **Kas** ur Pull Request

### P step 4: Adkempenn ha kelaouenn

Gwelet a reomp pep plugin evit:

- **Securite** — netra a zo perilus (gwelet [Règleoù Securite](#security-rules))
- **Qualite** — ober a ra? Dalc'h a ra ar c'hod evezh?
- **Implij** — a ra ur gwir problem da dalvezout?

Apre ma vo kelaouet:
1. O plugin a vo ouzhpennañ da `registry.json`
2. Ur bundl ZIP a vo krouet e `dist/`
3. O plugin a vo diskouezet e **Plugin Store** evit an holl implijourien WIA SOOM!

---

## Rann 5: Prouezhioù Gwell

### Règleoù Securite

Ar reolennoù-mañ a zo **ezhomm**. Plugins a ra an diforc'h a vo rejeteet.

| Règle | Perak |
|------|-----|
| **NEVER** implij `eval()` pe `new Function()` | Peril kod a zo |
| **NEVER** implij `child_process`, `exec()`, `spawn()` | Implijit nemet `ctx.terminal.send()` evit ar c'homandioù |
| **NEVER** c'hwezh URLoù all | Dilemm: `wiasoom.com` API endpoints |
| **NEVER** mont da `process.env` | Gallout a ra ar varioù ambezen bezañ sekret |
| **NEVER** implij `require('fs')` dirak | Implijit `ctx.settings` evit ar stank, `ctx.sftp` evit ar c'houlzioù |
| **NEVER** implij pakadoù all eus npm | JavaScript pur hep node_modules |
| **MUST** implij `ctx.terminal.send()` evit an holl gomandioù a-fet | Dalc'h a ra e kanal SSH sur |
| **MUST** purg ar pezh e `deactivate()` | Dilec'hit an evezhioù, skignit an intervaluioù |

### Gwelet ar fazioù

Dalc'hit da goll ar c'hwezhioù perilus e try/catch:
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
### Purzh e deactivate()

Ma kreñvoc'h an intervaluioù, an evezhioù, pe ar c'houlzioù — purzhit anezho:
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
### Soutien i18n

WIA SOOM a souten 254 yezh. Evit ma vo translatable ho label plugin, implijit ur c'houlz simpl:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## Rann 6: Skouerioù E gwir Blebennoù

### Skouer 1: Kontroler Disk Server

Kroug `df -h` war ar server a-fet ha diskouez ar c'hresk implijet/da c'hortoz e bar ar statud.
§§§CHUNK_SEPARATOR§§§
---

### Skouer 2: Meneger TODO

Ur plugin a meneg ar listenn TODO o implij ar settings evit ar stank a-dreuz ha ur webview evit ar diskouez.

> **Patron dizain:** Emañ webviews o c'hwezh ne c'hallont ket komz dirak d'ar APIoù plugin, ar plugin-mañ a implij ur "snapshot" — e krog an TODOs eus ar settings, e krou ar re evel HTML da vezañ lenn, ha ro an actionoù a zo e-barzh ar c'houlz. Ar webview a zo ur **diskouez** kat, n'eo ket ur furm a-vremañ.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Skouer 3: Gwelet ar fazioù

Gwelet a ra ar c'hwezhioù terminal ha kas ur c'hwezh pa vez diskouezet ur c'hwezh bennañ.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

## Appendice: Rummadoù & Ikonoù

### Rummadoù Plugin (29)

Implijit ar rummadoù-mañ e ho `package.json` `keywords` pe pa submitit d'ar registry:

| Rummad | Deskrivadur |
|--------|-------------|
| `server` | Merañ ar servijerioù genel |
| `devtools` | Outils diorren |
| `calculator` | Kalkulatorioù ha konverterioù |
| `simulator` | Simulateurioù |
| `game` | Jeuioù terminal |
| `business` | Outils biznes |
| `security` | Sekurite ha kontrol |
| `web` | Merañ ar servijerioù web |
| `education` | Outils skol |
| `health` | Outils en relijenn ar yec'hed |
| `islamic` | Outils islamel (pennadoù preder, hag all) |
| `science` | Outils skiantel |
| `quantum` | Outils kempenn quantum |
| `ai` | Outils gant AI |
| `biotech` | Outils biotechnologie |
| `space` | Outils ar spaz ha stlenn |
| `network` | Outils networek |
| `database` | Merañ ar bankoù donnee |
| `monitoring` | Kontrol servijer |
| `devops` | DevOps ha CI/CD |
| `utility` | Outils genel |
| `design` | Outils dizain |
| `ecommerce` | Outils e-commerce |
| `automation` | Outils automatiz |
| `kpop` | Outils en relijenn K-pop |
| `accessibility` | Outils aes da c'hloar |
| `analytics` | Analiz ha raport |
| `wia` | Outils ekosystem WIA |
| `all` | Diskouezet e pep rummad |

### Ikonoù Rekommanded (Lucide)

| Anv Ikon | Implij evit |
|-----------|-------------|
| `server` | Merañ ar servijerioù |
| `shield` | Sekurite |
| `database` | Bank donnee |
| `activity` | Kontrol |
| `terminal` | Outils terminal |
| `code` | Diouer |
| `hard-drive` | Disk/stor |
| `network` | Networek |
| `lock` | Auth/encryptage |
| `eye` | Gwelet/kontrol |
| `check-square` | Kargadoù/TODO |
| `layout-dashboard` | Dashboardioù |
| `settings` | Konfiguradur |
| `zap` | Automatiz |
| `globe` | Web/internasional |

Browseit an 1,500+ ikonoù: [lucide.dev/icons](https://lucide.dev/icons)

---

## C'hortoz Skoazell?

- **Problemoù GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemoù Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Displegad:** [Website](https://wiasoom.com)
- **Webzine:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Sevel un dra disoñjet. Dilemel out d'ar bed.</em></p>
<p align="center"><em>— Tamm WIA SOOM</em></p>
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**Example: Remember user preferences**

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

### `ctx.ai` — AI integration

> **Status: Coming Soon** — The AI API is defined but not yet connected to Soomy. Currently returns `{ response: 'AI not yet connected' }`. Full AI integration is planned for a future release.

#### `ai.chat(messages, options?)`

Send messages to the AI assistant (Soomy).

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## Part 3: Building Custom UI with Webviews

The `openWebview()` API lets you build dashboard UIs with HTML, CSS, and JavaScript — all inside a popup window.

> **Important limitation:** Webviews are **display-only**. They cannot call back into plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Use sidebar buttons for all user actions, and use `openWebview()` to display current state. If you need interactive features, trigger them from sidebar buttons and re-open the webview to refresh the display.

### Pattern: Terminal Command → Parse Output → Show in HTML

This is the most common plugin pattern. You run a command, parse the result, and display it visually.

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

### Pattern: Interactive Dashboard with Auto-Refresh

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

### Pattern: Displaying Settings in a Webview

> **Note:** Webviews are display-only — they cannot call back into plugin APIs. Use `ctx.settings` in your sidebar button handlers to modify settings, and use `openWebview()` to show the current state.

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

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Copy your plugin to `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verify it works: sidebar button appears, features work correctly
4. Test edge cases: what happens if no terminal is connected?

### Step 2: Prepare for submission

Your plugin folder must contain:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | One-line description | `"Monitors nginx access logs in real-time"` |
| `author` | Your name | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | License type (MIT recommended) |
| `keywords` | Array of search tags |
| `soom.minVersion` | Minimum WIA SOOM version required |

### Step 3: Submit to the Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** your plugin to `plugins/{your-plugin-name}/`
3. **Submit** a Pull Request

### Step 4: Review and approval

We review every plugin for:

- **Security** — no dangerous APIs (see [Security Rules](#security-rules))
- **Quality** — does it work? Is the code clean?
- **Usefulness** — does it solve a real problem?

After approval:
1. Your plugin is added to `registry.json`
2. A ZIP bundle is created in `dist/`
3. Your plugin appears in the **Plugin Store** for all WIA SOOM users!

---

## Part 5: Best Practices

### Security Rules

These rules are **mandatory**. Plugins that violate them will be rejected.

| Rule | Why |
|------|-----|
| **NEVER** use `eval()` or `new Function()` | Code injection risk |
| **NEVER** use `child_process`, `exec()`, `spawn()` | Only use `ctx.terminal.send()` for commands |
| **NEVER** fetch external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** access `process.env` | Environment variables may contain secrets |
| **NEVER** use `require('fs')` directly | Use `ctx.settings` for storage, `ctx.sftp` for file transfer |
| **NEVER** use npm external packages | Pure JavaScript only — no node_modules |
| **MUST** use `ctx.terminal.send()` for all remote commands | This goes through the secure SSH channel |
| **MUST** clean up in `deactivate()` | Remove listeners, clear intervals |

### Error Handling

Always wrap risky operations in try/catch:

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

If your plugin creates intervals, listeners, or subscriptions — clean them up:

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

WIA SOOM supports 254 languages. To make your plugin label translatable, use a simple approach:

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

Runs `df -h` on the remote server and shows used/available space in the status bar.

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

A plugin that manages a TODO list using settings for persistent storage and a webview for display.

> **Design pattern:** Since webviews cannot directly call plugin APIs, this plugin uses a "snapshot" approach — it reads TODOs from settings, renders them as read-only HTML, and provides sidebar-based actions for adding items. The webview is a **display** layer, not an interactive form.

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

Monitors terminal output and sends a notification when specific patterns are detected.

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
