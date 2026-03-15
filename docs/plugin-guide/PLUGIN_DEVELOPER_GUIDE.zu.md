<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Umhlahlandlela Wokuthuthukisa I-Plugin ye-WIA SOOM</h1>
<p align="center"><strong>Yakha i-plugin yakho emaminithini ama-5.</strong></p>
<p align="center">Dala amathuluzi anamandla eserver, ama-dashboard, kanye nezinhlelo zokusebenza — ngqo ngaphakathi kwe-WIA SOOM.</p>

---

## Ithebula Lezinto Eziqukethwe

- [Ingxenye 1: Ukuqala Ngokushesha — I-Plugin Yakho Yokuqala Emaminithini ama-5](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Ingxenye 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Ingxenye 3: Ukwakha i-UI Eyenziwe Ngokwezifiso Nge-Webviews](#part-3-building-custom-ui-with-webviews)
- [Ingxenye 4: Ukushicilela I-Plugin Yakho](#part-4-publishing-your-plugin)
- [Ingxenye 5: Izindlela Ezingcono](#part-5-best-practices)
- [Ingxenye 6: Izibonelo Zangempela](#part-6-real-world-examples)
- [Isithasiselo: Izigaba Nezithonjana](#appendix-categories--icons)

---

## Ingxenye 1: Ukuqala Ngokushesha — I-Plugin Yakho Yokuqala Emaminithini ama-5

### Lokho ozokwakha

I-plugin ethi "Hello World" engeza inkinobho ku-sidebar. Uma icindezelwe, ibonisa izaziso.

### Isinyathelo 1: Dala ifolda ye-plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Isinyathelo 2: Dala package.json
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
**Izinkambu ezidingekayo:** `name`, `version`, `description`, `author`, `main`

### Isinyathelo 3: Dala index.js
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
### Isinyathelo 4: Qala kabusha i-WIA SOOM

Qala kabusha uhlelo (noma ushintshe i-plugin ivaliwe/ivuliwe ku Settings → Plugins).

Kufanele ubone inkinobho ethi **"Hello World"** ku-sidebar. Cindezela — uzobona izaziso zokuphumelela!

### Indlela esebenza ngayo
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

## Ingxenye 2: Plugin Context API Reference

Lapho umsebenzi wakho `activate(context)` ubizwa, `context` (noma `ctx`) unikeza lezi API:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Sebenzisa imiyalo kumaseva akude

#### `terminal.send(sessionId, data)`

Thumela imiyalo (noma yimiphi idatha) kum sesi ye-terminal esebenzayo.

| Parameter | Uhlobo | Incazelo |
|-----------|------|-------------|
| `sessionId` | `string` | Iseshini ye-terminal okufanele ithunyelwe kuyo |
| `data` | `string` | Imiyalo noma idatha okufanele ithunyelwe |
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
#### `terminal.onOutput(sessionId, callback)`

Bhalisela yonke imiphumela esuka kuseshini ye-terminal. Buyisa **umsebenzi wokungabhalisile**.

| Parameter | Uhlobo | Incazelo |
|-----------|------|-------------|
| `sessionId` | `string` | Iseshini ye-terminal okufanele ibhekwane nayo |
| `callback` | `(data: string) => void` | Ibizwa ngenkathi ithola ingxenye ngayinye yemiphumela |
| **Buyisa** | `() => void` | Shayela lokhu ukuze uyeke ukulalela |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**Kubhalwe phansi:** Qiniseka ukuthi ugcina umsebenzi wokungabhalisile futhi uwushayele ku `deactivate()` ukuze uvimbele ukuvuza kwememori.

---

### `ctx.sftp` — Ukudluliswa kwefayela

> **Isimo: Sizofika Maduzane** — I-SFTP API ichazwe kodwa ayikaxhunywanga kumshini we-SFTP we-app. `list()` okwamanje ibuyisa uhlu olungenalutho, futhi `upload()`/`download()` azisebenzi. Lokhu kuzofakwa ngokuphelele emkhakheni ozayo. Okwamanje, sebenzisa `ctx.terminal.send()` nge `scp` noma `rsync` imiyalo njengendlela yokusebenza.

#### `sftp.list(sessionId, path)`

Bhala amafayela kudirectory ekude.
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
#### `sftp.upload(sessionId, localPath, remotePath)`

Thumela ifayela kusuka kumshini wendawo uye kumaseva akude.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

Landa ifayela kusuka kumaseva akude uye kumshini wendawo.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**Indlela yokusebenza (kuze kube i-SFTP API iphila):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — Uhlaka lomsebenzisi

#### `ui.addSidebarButton(options)`

Engeza inkinobho ku-sidebar ye-WIA SOOM.

| Inketho | Uhlobo | Edingekayo | Incazelo |
|--------|------|----------|-------------|
| `id` | `string` | Cha | I-ID eyingqayizivele (iyashintsha ibe igama le-plugin) |
| `icon` | `string` | Yebo | Igama lesithonjana se-Lucide (isb., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yebo | Umbhalo wenkinobho oboniswa ku-sidebar |
| `onClick` | `() => void` | Yebo | Umsebenzi obizwa uma inkinobho icindezelwe |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Isithonjana sokubhekisela:** Bheka zonke izithonjana ezitholakalayo ku [lucide.dev/icons](https://lucide.dev/icons)

> **Iphuzu lokuhambisana:** Amanye ama-plugin amadala asebenzisa izinketho ezibekwe njenge `addSidebarButton(id, icon, label, onClick)`. I-API esemthethweni isebenzisa **into yezinketho** njengoba kuchaziwe ngenhla. Qiniseka ukuthi usebenzisa isitayela se-into kuma-plugin amasha.

#### `ui.openWebview(options)`

Vula iwindi le-popup elinezinto ze-HTML ezenziwe ngokwezifiso. Lokhu kuyindlela yokwakha ama-UI anothile.

| Inketho | Uhlobo | Incazelo |
|--------|------|-------------|
| `title` | `string` | Isihloko sewindi |
| `html` | `string` | Okuqukethwe okuphelele kwe-HTML okufanele kudwetshwe |
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
> Bheka [Ingxenye 3](#part-3-building-custom-ui-with-webviews) ukuze uthole amaphethini we-webview athuthukile.

#### `ui.showNotification(type, message)`

Bonisa isaziso se-toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Isitayela sesaziso |
| `message` | `string` | Umbhalo okufanele uboniswe |
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
#### `ui.addStatusBarItem(id, text)`

Engeza into yombhalo eqhubekayo ebhange lesimo esiphansi.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID eyingqayizivele yale nto yesimo |
| `text` | `string` | Umbhalo okufanele uboniswe |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Ukugcina okuqhubekayo

Izilungiselelo ze-plugin zigcinwa njalo ku-`~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Funda inani eligciniwe.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Ibuyisela `undefined` uma i-key ingekho.

#### `settings.set(key, value)`

Gcina inani. Isekela izintambo, izinombolo, ama-booleans, ama-arrays, nezinto.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Isibonelo: Khumbula izintandokazi zomsebenzisi**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — Ukuhlanganiswa kwe-AI

> **Isimo: Kuza Maduzane** — I-AI API ichazwe kodwa ayikaxhunywanga ku-Soomy. Okwamanje ibuyisela `{ response: 'AI not yet connected' }`. Ukuhlanganiswa okuphelele kwe-AI kuhlelwe ukuze kuvele emkhakheni wesikhathi esizayo.

#### `ai.chat(messages, options?)`

Thumela imiyalezo kumphakathi we-AI (Soomy).
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

## Ingxenye 3: Ukwakha i-UI Eyenzelwe Ngokwezifiso Nge-Webviews

I-`openWebview()` API ikuvumela ukuthi wakhe ama-UI wedashboard nge-HTML, CSS, ne-JavaScript — konke ngaphakathi kwifasitela le-popup.

> **Ukukhawulelwa Okubalulekile:** Ama-webviews **awokubonisa kuphela**. Awekwazi ukubiza emuva kumaphoyisa e-plugin (`ctx.settings`, `ctx.terminal`, njll.). Sebenzisa izinkinobho ze-sidebar ukuze ubonise zonke izenzo zomsebenzisi, futhi usebenzise `openWebview()` ukuze ubonise isimo samanje. Uma udinga izici ezisebenzisanayo, ziqhube ezinkinobho ze-sidebar bese uvula kabusha i-webview ukuze uvuselele ukuboniswa.

### Iphattern: Um comando we-Terminal → Parse Output → Bonisa ku-HTML

Lena iyiphattern evamile ye-plugin. Uqhuba umyalo, uphinde uhlole umphumela, bese uwubonisa ngeso lengqondo.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Iphattern: I-Dashboard Esebenzisanayo enezivuselelo Zokuqhubeka
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
### Iphattern: Ukubonisa Izilungiselelo ku-Webview

> **Qaphela:** Ama-webviews awokubonisa kuphela — awekwazi ukubiza emuva kumaphoyisa e-plugin. Sebenzisa `ctx.settings` ezinkombeni zakho ze-sidebar ukuze uguqule izilungiselelo, futhi usebenzise `openWebview()` ukuze ubonise isimo samanje.
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

## Ingxenye 4: Ukushicilela I-Plugin Yakho

### Isinyathelo 1: Hlola endaweni

1. Kopisha i-plugin yakho ku-`~/.wia-soom/plugins/{your-plugin}/`
2. Qalisa kabusha i-WIA SOOM
3. Qinisekisa ukuthi iyasebenza: inkinobho ye-sidebar ibonakala, izici zisebenza kahle
4. Hlola izimo ezinzima: kwenzekani uma kungaxhunyiwe i-terminal?

### Isinyathelo 2: Lungiselela ukuthunyelwa

Ifolda ye-plugin yakho kumele iqukate:
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
**Izinkambu ezidingekayo ze-`package.json`:**

| Ikhambi | Incazelo | Isibonelo |
|---------|----------|-----------|
| `name` | I-ID eyingqayizivele ye-kebab-case | `"my-awesome-plugin"` |
| `version` | Inguqulo ye-semantic | `"1.0.0"` |
| `description` | Incazelo emugqeni owodwa | `"Monitors nginx access logs in real-time"` |
| `author` | Igama lakho | `"John Doe"` |
| `main` | Iphuzu lokungena | `"index.js"` |

**Izinkambu ezikhethwayo:**

| Ikhambi | Incazelo |
|---------|----------|
| `license` | Uhlobo lwe-licence (MIT kuyas rekomendwa) |
| `keywords` | Uhlu lwezimpawu zokusesha |
| `soom.minVersion` | Inguqulo encane ye-WIA SOOM edingekayo |

### Isinyathelo 3: Thumela ku-Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Faka** i-plugin yakho ku-`plugins/{your-plugin-name}/`
3. **Thumela** i-Pull Request

### Isinyathelo 4: Ukuhlola nokuvuma

Sihlola i-plugin ngayinye ukuze:

- **Ubumfihlo** — akukho ama-API ayingozi (buka [Imithetho Yokuphepha](#security-rules))
- **Ikhwalithi** — iyasebenza? Ingabe ikhodi ihlanzekile?
- **Ukusetshenziswa** — ingabe ixazulula inkinga yangempela?

Ngemuva kokuvuma:
1. I-plugin yakho ifakwa ku-`registry.json`
2. I-ZIP bundle idalwa ku-`dist/`
3. I-plugin yakho ibonakala ku-**Plugin Store** kubo bonke abasebenzisi be-WIA SOOM!

---

## Ingxenye 5: Izindlela Ezingcono

### Imithetho Yokuphepha

Le mithetho **iyadingeka**. Ama-plugin aphula lezi ziqondiso azophulwa.

| Umthetho | Kungani |
|----------|---------|
| **UNGAYISEBELI** `eval()` noma `new Function()` | Ingcuphe yokufaka ikhodi |
| **UNGAYISEBELI** `child_process`, `exec()`, `spawn()` | Sebenzisa kuphela `ctx.terminal.send()` kumyalo |
| **UNGAYISEBELI** ukuthola ama-URL angaphandle | Ukuhluka: ama-API endpoints e-`wiasoom.com` |
| **UNGAYISEBELI** ukufinyelela `process.env` | Izinguquko zemvelo zingaba nezimfihlo |
| **UNGAYISEBELI** ukusebenzisa `require('fs')` ngqo | Sebenzisa `ctx.settings` yokugcina, `ctx.sftp` yokudlulisa ifayela |
| **KUFANELE** usebenzise `ctx.terminal.send()` kumyalo wonke wokude | Lokhu kudlula emgqeni ophephile we-SSH |
| **KUFANELE** uhlanze ku-`deactivate()` | Susa abamukeli, hlanza izikhathi |

### Ukuphathwa Kwephutha

Hlale ufaka izenzo ezibucayi ku-try/catch:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Ukuhlanzwa ku-deactivate()

Uma i-plugin yakho idala izikhathi, abamukeli, noma ukubhaliswa — hlanza lezo:
§§§CHUNK_SEPARATOR§§§
### Ukusekela i18n

I-WIA SOOM isekela izilimi eziyi-254. Ukuze i-plugin yakho ibe ne-label ethokozisayo, sebenzisa indlela elula:
§§§CHUNK_SEPARATOR§§§
---

## Ingxenye 6: Izibonelo Zangempela

### Isibonelo 1: Umhloli Wesikhala Se-Server

Isebenza `df -h` ku-server ekude futhi ibonisa isikhala esisetshenzisiwe/itholakalayo ebhange lesimo.
§§§CHUNK_SEPARATOR§§§
---

### Isibonelo 2: Umphathi we-TODO

I-plugin ephatha uhlu lwe-TODO isebenzisa izilungiselelo zokugcina okuqhubekayo kanye ne-webview yokubonisa.

> **Umklamo wesibonelo:** Njengoba ama-webviews engakwazi ukubiza ngqo ama-API e-plugin, le plugin isebenzisa indlela ye-"snapshot" — ifunda ama-TODO kusukela kuzilungiselelo, iyawafaka njenge-HTML engashintshiwe, futhi inikeza izenzo ezisekelwe eceleni zokwengeza izinto. I-webview iyisendlalelo se-**bonisa**, hhayi ifomu elisebenzisanayo.
§§§CHUNK_SEPARATOR§§§
---

### Isibonelo 3: Umhloli Wephutha

Uhlola umphumela we-terminal futhi uthumela isaziso uma ama-pattern athile etholwa.
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
---

## Iphendla: Izigaba & Ama-Icon

### Izigaba zePlugin (29)

Sebenzisa lezi ku-`package.json` yakho `keywords` noma uma uthumela kwi-registry:

| Izigaba | Incazelo |
|---------|----------|
| `server` | Ukuphathwa kweserver okujwayelekile |
| `devtools` | Amathuluzi okuthuthukiswa |
| `calculator` | Amakhalekhukhwini neziguquli |
| `simulator` | Abasimulator |
| `game` | Imidlalo ye-terminal |
| `business` | Amathuluzi ebhizinisi |
| `security` | Ukuvikeleka nokuhlola |
| `web` | Ukuphathwa kweserver yewebhu |
| `education` | Amathuluzi ezemfundo |
| `health` | Amathuluzi ahlobene nezempilo |
| `islamic` | Amathuluzi e-Islam (izikhathi zokukhuleka, njll.) |
| `science` | Amathuluzi esayensi |
| `quantum` | Amathuluzi wokubala kwe-quantum |
| `ai` | Amathuluzi anogesi ye-AI |
| `biotech` | Amathuluzi e-biotechnology |
| `space` | Amathuluzi wesikhala nezinkanyezi |
| `network` | Amathuluzi wenethiwekhi |
| `database` | Ukuphathwa kwedatha |
| `monitoring` | Ukuhlola iserver |
| `devops` | DevOps ne-CI/CD |
| `utility` | Izinsiza eziyisisekelo |
| `design` | Amathuluzi wokwakha |
| `ecommerce` | Amathuluzi e-e-commerce |
| `automation` | Amathuluzi wokwenza ngokuzenzakalelayo |
| `kpop` | Amathuluzi ahlobene ne-K-pop |
| `accessibility` | Amathuluzi okufinyelela |
| `analytics` | Ukuhlaziywa nokubika |
| `wia` | Amathuluzi e-WIA ecosystem |
| `all` | Ivela kuzo zonke izigaba |

### Ama-Icon Aconyiwe (Lucide)

| Igama le-Icon | Sebenzisa ku |
|----------------|--------------|
| `server` | Ukuphathwa kweserver |
| `shield` | Ukuvikeleka |
| `database` | Idatha |
| `activity` | Ukuhlola |
| `terminal` | Amathuluzi e-terminal |
| `code` | Ukuthuthukiswa |
| `hard-drive` | Idiskhi/ukugcina |
| `network` | Ukuxhumana |
| `lock` | Auth/ukufihla |
| `eye` | Ukubuka/ukuhlola |
| `check-square` | Imisebenzi/TODO |
| `layout-dashboard` | Ama-dashboard |
| `settings` | Ukumisa |
| `zap` | Ukwenza ngokuzenzakalelayo |
| `globe` | Iwebhu/yangaphandle |

Bheka wonke ama-icon angaphezu kwe-1,500: [lucide.dev/icons](https://lucide.dev/icons)

---

## Udinga Usizo?

- **Izinkinga zeGitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Izinkinga zePlugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Izibonelo zePlugins:** [Website](https://wiasoom.com)
- **Iwebhusayithi:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Yakha okuthile okumangalisayo. Yabelana ngakho nomhlaba.</em></p>
<p align="center"><em>— Iqembu le-WIA SOOM</em></p>
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
