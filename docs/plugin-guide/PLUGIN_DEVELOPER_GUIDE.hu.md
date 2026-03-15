<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Fejlesztői Útmutató</h1>
<p align="center"><strong>Építsd meg a saját pluginedet 5 perc alatt.</strong></p>
<p align="center">Hozz létre erőteljes szervereszközöket, irányítópultokat és automatizálásokat — közvetlenül a WIA SOOM-ban.</p>

---

## Tartalomjegyzék

- [1. rész: Gyors kezdés — Az első pluginod 5 perc alatt](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [2. rész: Plugin Kontextus API Referencia](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [3. rész: Egyedi UI építése Webview-k segítségével](#part-3-building-custom-ui-with-webviews)
- [4. rész: A pluginod közzététele](#part-4-publishing-your-plugin)
- [5. rész: Legjobb gyakorlatok](#part-5-best-practices)
- [6. rész: Valós példák](#part-6-real-world-examples)
- [Függelék: Kategóriák és ikonok](#appendix-categories--icons)

---

## 1. rész: Gyors kezdés — Az első pluginod 5 perc alatt

### Mit fogsz építeni

Egy "Hello World" plugint, amely egy gombot ad a sidebarhoz. Amikor rákattintasz, megjelenik egy értesítés.

### 1. lépés: Hozd létre a plugin mappát
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 2. lépés: Hozd létre a package.json-t
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
**Kötelező mezők:** `name`, `version`, `description`, `author`, `main`

### 3. lépés: Hozd létre az index.js-t
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
### 4. lépés: Indítsd újra a WIA SOOM-ot

Indítsd újra az alkalmazást (vagy kapcsold ki/ be a plugint a Beállítások → Pluginek menüpontban).

Látnod kell egy **"Hello World"** gombot a sidebarban. Kattints rá — sikeres értesítést fogsz látni!

### Hogyan működik
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

## 2. rész: Plugin Kontextus API Referencia

Amikor az `activate(context)` függvényed meghívásra kerül, a `context` (vagy `ctx`) ezeket az API-kat biztosítja:
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

### `ctx.terminal` — Parancsok futtatása távoli szervereken

#### `terminal.send(sessionId, data)`

Küldj egy parancsot (vagy bármilyen adatot) egy aktív terminál munkamenethez.

| Paraméter | Típus | Leírás |
|-----------|------|-------------|
| `sessionId` | `string` | A terminál munkamenet, ahová küldeni szeretnél |
| `data` | `string` | A küldendő parancs vagy adat |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Feliratkozás a terminál munkamenet összes kimenetére. Visszaad egy **lemondó függvényt**.

| Paraméter | Típus | Leírás |
|-----------|------|-------------|
| `sessionId` | `string` | A figyelni kívánt terminál munkamenet |
| `callback` | `(data: string) => void` | Minden kimeneti darabnál meghívásra kerül |
| **Visszatér** | `() => void` | Ezt hívd meg a hallgatás leállításához |
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
**Fontos:** Mindig mentsd el a lemondó függvényt, és hívd meg a `deactivate()`-ban, hogy elkerüld a memória szivárgást.

---

### `ctx.sftp` — Fájlátvitel

> **Állapot: Hamarosan érkezik** — A SFTP API definiálva van, de még nincs összekapcsolva az alkalmazás SFTP motorjával. A `list()` jelenleg egy üres tömböt ad vissza, és az `upload()`/`download()` nem végez semmilyen műveletet. Ez a jövőbeli kiadásban teljesen meg lesz valósítva. Addig is, használd a `ctx.terminal.send()`-t `scp` vagy `rsync` parancsokkal alternatívaként.

#### `sftp.list(sessionId, path)`

Fájlok listázása egy távoli könyvtárban.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Fájl feltöltése a helyi gépről a távoli szerverre.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Fájl letöltése a távoli szerverről a helyi gépre.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Alternatíva (amíg az SFTP API él):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Felhasználói felület

#### `ui.addSidebarButton(options)`

Gomb hozzáadása a WIA SOOM sidebarhoz.

| Opció | Típus | Kötelező | Leírás |
|--------|------|----------|-------------|
| `id` | `string` | Nem | Egyedi azonosító (alapértelmezés szerint a plugin neve) |
| `icon` | `string` | Igen | Lucide ikon neve (pl.: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Igen | A sidebarban megjelenő gomb szövege |
| `onClick` | `() => void` | Igen | A függvény, amely akkor hívódik meg, amikor a gombra kattintanak |
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
**Ikon referencia:** Böngéssz az összes elérhető ikon között a [lucide.dev/icons](https://lucide.dev/icons) oldalon.

> **Kompatibilitási megjegyzés:** Néhány régebbi plugin pozicionális argumentumokat használ, mint például `addSidebarButton(id, icon, label, onClick)`. A hivatalos API egy **opciós objektumot** használ, ahogy fent dokumentálva van. Mindig használd az objektum stílust új pluginekhez.

#### `ui.openWebview(options)`

Popup ablak megnyitása egyedi HTML tartalommal. Így építhetsz gazdag UI-kat.

| Opció | Típus | Leírás |
|--------|------|-------------|
| `title` | `string` | Az ablak címe |
| `html` | `string` | A megjelenítendő teljes HTML tartalom |
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
> Lásd a [3. részt](#part-3-building-custom-ui-with-webviews) az fejlett webview mintákért.

#### `ui.showNotification(type, message)`

Megjelenít egy értesítést.

| Paraméter | Típus | Leírás |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Értesítési stílus |
| `message` | `string` | Megjelenítendő szöveg |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Hozzáad egy tartós szöveges elemet az alsó állapotsorhoz.

| Paraméter | Típus | Leírás |
|-----------|------|-------------|
| `id` | `string` | Egyedi azonosító ehhez az állapotelemhez |
| `text` | `string` | Megjelenítendő szöveg |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Tartós tárolás

A plugin beállításai tartósan a `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` fájlban vannak tárolva.

#### `settings.get(key)`

Olvas egy mentett értéket.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Visszaadja az `undefined` értéket, ha a kulcs nem létezik.

#### `settings.set(key, value)`

Ment egy értéket. Támogatja a karakterláncokat, számokat, logikai értékeket, tömböket és objektumokat.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Példa: Felhasználói preferenciák megjegyzése**
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

### `ctx.ai` — AI integráció

> **Állapot: Hamarosan** — Az AI API definiálva van, de még nincs csatlakoztatva a Soomyhoz. Jelenleg `{ response: 'AI not yet connected' }` értéket ad vissza. Teljes AI integráció tervezett egy jövőbeli kiadásra.

#### `ai.chat(messages, options?)`

Üzenetek küldése az AI asszisztensnek (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## 3. rész: Egyedi UI építése Webview-kal

Az `openWebview()` API lehetővé teszi, hogy HTML, CSS és JavaScript segítségével irányítópult UI-kat építsen — mindezt egy felugró ablakban.

> **Fontos korlátozás:** A webview-k **csak megjelenítésre szolgálnak**. Nem tudják visszahívni a plugin API-kat (`ctx.settings`, `ctx.terminal`, stb.). Használjon oldalsáv gombokat minden felhasználói művelethez, és használja az `openWebview()`-t az aktuális állapot megjelenítésére. Ha interaktív funkciókra van szüksége, indítsa el őket az oldalsáv gombjairól, és nyissa meg újra a webview-t a megjelenítés frissítéséhez.

### Minta: Terminál Parancs → Kimenet Elemzése → Megjelenítés HTML-ben

Ez a leggyakoribb plugin minta. Futtat egy parancsot, elemzi az eredményt, és vizuálisan megjeleníti.
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
### Minta: Interaktív Irányítópult Automatikus Frissítéssel
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
### Minta: Beállítások Megjelenítése Webview-ban

> **Megjegyzés:** A webview-k csak megjelenítésre szolgálnak — nem tudják visszahívni a plugin API-kat. Használja a `ctx.settings`-t az oldalsáv gombkezelőiben a beállítások módosításához, és használja az `openWebview()`-t az aktuális állapot megjelenítésére.
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

## 4. rész: A Plugin Közzététele

### 1. lépés: Helyi tesztelés

1. Másolja a pluginját a `~/.wia-soom/plugins/{your-plugin}/` mappába
2. Indítsa újra a WIA SOOM-ot
3. Ellenőrizze, hogy működik: az oldalsáv gomb megjelenik, a funkciók helyesen működnek
4. Tesztelje a szélsőséges eseteket: mi történik, ha nincs csatlakoztatva terminál?

### 2. lépés: Felkészülés a benyújtásra

A plugin mappájának a következőket kell tartalmaznia:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Szükséges `package.json` mezők:**

| Mező | Leírás | Példa |
|------|--------|-------|
| `name` | Egyedi kebab-case azonosító | `"my-awesome-plugin"` |
| `version` | Szemantikus verzió | `"1.0.0"` |
| `description` | Egysoros leírás | `"Valós időben figyeli az nginx hozzáférési naplókat"` |
| `author` | A neved | `"John Doe"` |
| `main` | Belépési pont | `"index.js"` |

**Opcionális mezők:**

| Mező | Leírás |
|------|--------|
| `license` | Licenc típusa (ajánlott MIT) |
| `keywords` | Keresési címkék tömbje |
| `soom.minVersion` | Minimális WIA SOOM verzió, amely szükséges |

### 3. lépés: Küldd be a Plugin Regisztrációba

1. ****Package** your plugin as a ZIP file
2. **Add hozzá** a pluginodat a `plugins/{your-plugin-name}/` mappába
3. **Küldj be** egy Pull Request-et

### 4. lépés: Áttekintés és jóváhagyás

Minden plugint áttekintünk a következők miatt:

- **Biztonság** — nincsenek veszélyes API-k (lásd [Biztonsági Szabályok](#security-rules))
- **Minőség** — működik? Tiszta a kód?
- **Hasznosság** — megold-e egy valós problémát?

Jóváhagyás után:
1. A pluginod hozzáadódik a `registry.json`-hoz
2. Egy ZIP csomag készül a `dist/` mappában
3. A pluginod megjelenik a **Plugin Store**-ban minden WIA SOOM felhasználó számára!

---

## 5. rész: Legjobb Gyakorlatok

### Biztonsági Szabályok

Ezek a szabályok **kötelezőek**. Azok a pluginek, amelyek megszegik őket, elutasításra kerülnek.

| Szabály | Miért |
|---------|-------|
| **SOHA** ne használd az `eval()`-t vagy az `new Function()`-t | Kód injekciós kockázat |
| **SOHA** ne használd a `child_process`, `exec()`, `spawn()` | Csak a `ctx.terminal.send()`-et használd parancsokhoz |
| **SOHA** ne kérj le külső URL-eket | Kivétel: `wiasoom.com` API végpontok |
| **SOHA** ne férj hozzá a `process.env`-hez | A környezeti változók titkokat tartalmazhatnak |
| **SOHA** ne használd közvetlenül a `require('fs')`-t | Használj `ctx.settings`-et tárolásra, `ctx.sftp`-t fájlátvitelre |
| **SOHA** ne használj npm külső csomagokat | Csak tiszta JavaScript — nincs node_modules |
| **KÖTELEZŐ** használni a `ctx.terminal.send()`-et minden távoli parancshoz | Ez a biztonságos SSH csatornán keresztül megy |
| **KÖTELEZŐ** takarítani a `deactivate()`-ban | Távolítsd el a hallgatókat, tisztítsd meg az intervallumokat |

### Hibakezelés

Mindig csomagold be a kockázatos műveleteket try/catch:
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
### Takarítás a deactivate() során

Ha a pluginod intervallumokat, hallgatókat vagy előfizetéseket hoz létre — takarítsd el őket:
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
### i18n Támogatás

A WIA SOOM 254 nyelvet támogat. Ahhoz, hogy a pluginod címkéje lefordítható legyen, használj egy egyszerű megközelítést:
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

## 6. rész: Valós Példák

### Példa 1: Szerver Lemez Ellenőrző

Futtatja a `df -h` parancsot a távoli szerveren, és megjeleníti a használt/elérhető helyet a státuszsávban.
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

### Példa 2: TODO Kezelő

Egy plugin, amely egy TODO listát kezel a beállítások segítségével tartós tárolásra és egy webview-t a megjelenítéshez.

> **Tervezési minta:** Mivel a webview-k nem hívhatják meg közvetlenül a plugin API-kat, ez a plugin egy "pillanatkép" megközelítést használ — a TODO-kat a beállításokból olvassa, csak olvasható HTML-ként rendereli őket, és oldalsáv-alapú műveleteket biztosít a tételek hozzáadás��hoz. A webview egy **megjelenítési** réteg, nem interaktív űrlap.
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

### Példa 3: Hibafelügyelő

Figyeli a terminál kimenetét, és értesítést küld, amikor bizonyos minták észlelésre kerülnek.
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

## Függelék: Kategóriák és Ikonok

### Plugin Kategóriák (29)

Használja ezeket a `package.json` `keywords` mezőjében vagy a regisztráció során:

| Kategória | Leírás |
|-----------|--------|
| `server` | Általános szerverkezelés |
| `devtools` | Fejlesztői eszközök |
| `calculator` | Számológépek és átalakítók |
| `simulator` | Szimulátorok |
| `game` | Terminál játékok |
| `business` | Üzleti eszközök |
| `security` | Biztonság és auditálás |
| `web` | Webszerver kezelés |
| `education` | Oktatási eszközök |
| `health` | Egészséggel kapcsolatos eszközök |
| `islamic` | Iszlám eszközök (imaidők stb.) |
| `science` | Tudományos eszközök |
| `quantum` | Kvantumszámítástechnikai eszközök |
| `ai` | AI-alapú eszközök |
| `biotech` | Biotechnológiai eszközök |
| `space` | Űr- és csillagászati eszközök |
| `network` | Hálózati eszközök |
| `database` | Adatbázis kezelés |
| `monitoring` | Szerverfigyelés |
| `devops` | DevOps és CI/CD |
| `utility` | Általános segédprogramok |
| `design` | Tervezési eszközök |
| `ecommerce` | E-kereskedelmi eszközök |
| `automation` | Automatizálási eszközök |
| `kpop` | K-pop kapcsolódó eszközök |
| `accessibility` | Hozzáférhetőségi eszközök |
| `analytics` | Elemzés és jelentéskészítés |
| `wia` | WIA ökoszisztéma eszközök |
| `all` | Minden kategóriában megjelenik |

### Ajánlott Ikonok (Lucide)

| Ikon Név | Használat |
|----------|-----------|
| `server` | Szerverkezelés |
| `shield` | Biztonság |
| `database` | Adatbázis |
| `activity` | Figyelés |
| `terminal` | Terminál eszközök |
| `code` | Fejlesztés |
| `hard-drive` | Lemez/tárolás |
| `network` | Hálózat |
| `lock` | Hitelesítés/titkosítás |
| `eye` | Figyelés/monitorozás |
| `check-square` | Feladatok/TODO |
| `layout-dashboard` | Műszerfalak |
| `settings` | Konfiguráció |
| `zap` | Automatizálás |
| `globe` | Web/internacionális |

Böngésszen az összes 1,500+ ikon között: [lucide.dev/icons](https://lucide.dev/icons)

---

## Segítségre van szüksége?

- **GitHub Hibák:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Hibák:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Példa Pluginek:** [Website](https://wiasoom.com)
- **Weboldal:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Építs valami csodálatosat. Oszd meg a világgal.</em></p>
<p align="center"><em>— A WIA SOOM Csapata</em></p>