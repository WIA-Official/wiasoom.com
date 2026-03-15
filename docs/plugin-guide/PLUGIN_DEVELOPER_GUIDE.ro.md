<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ghid pentru Dezvoltatori de Plugin-uri WIA SOOM</h1>
<p align="center"><strong>Construiește-ți propriul plugin în 5 minute.</strong></p>
<p align="center">Creează instrumente puternice pentru servere, tablouri de bord și automatizări — chiar în WIA SOOM.</p>

---

## Cuprins

- [Partea 1: Începere Rapidă — Primul Tău Plugin în 5 Minute](#partea-1-începere-rapidă--primul-tău-plugin-în-5-minute)
- [Partea 2: Referință API pentru Contextul Plugin-ului](#partea-2-referință-api-pentru-contextul-plugin-ului)
  - [ctx.terminal](#ctxterminal--executa-comenzi-pe-servere-de-la-distanță)
  - [ctx.sftp](#ctxsftp--transfer-de-fișiere)
  - [ctx.ui](#ctxui--interfață-utilizator)
  - [ctx.settings](#ctxsettings--stocare-persistentă)
  - [ctx.ai](#ctxai--integrare-ai)
- [Partea 3: Construirea unei Interfețe Personalizate cu Webviews](#partea-3-construirea-unei-interfețe-personalizate-cu-webviews)
- [Partea 4: Publicarea Plugin-ului Tău](#partea-4-publicarea-plugin-ului-tău)
- [Partea 5: Cele Mai Bune Practici](#partea-5-cele-mai-bune-practici)
- [Partea 6: Exemple din Lumea Reală](#partea-6-exemple-din-lumea-reală)
- [Anexă: Categorii & Iconițe](#anexă-categorii--iconițe)

---

## Partea 1: Începere Rapidă — Primul Tău Plugin în 5 Minute

### Ce vei construi

Un plugin "Hello World" care adaugă un buton în bara laterală. Când este apăsat, afișează o notificare.

### Pasul 1: Creează folderul plugin-ului
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Pasul 2: Creează package.json
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
**Câmpuri necesare:** `name`, `version`, `description`, `author`, `main`

### Pasul 3: Creează index.js
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
### Pasul 4: Reporneste WIA SOOM

Reporneste aplicația (sau comută plugin-ul pe oprit/activat în Setări → Plugin-uri).

Ar trebui să vezi un buton **"Hello World"** în bara laterală. Apasă-l — vei vedea o notificare de succes!

### Cum funcționează
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

## Partea 2: Referință API pentru Contextul Plugin-ului

Când funcția ta `activate(context)` este apelată, `context` (sau `ctx`) oferă aceste API-uri:
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

### `ctx.terminal` — Execută comenzi pe servere de la distanță

#### `terminal.send(sessionId, data)`

Trimite o comandă (sau orice date) către o sesiune de terminal activă.

| Parametru | Tip | Descriere |
|-----------|------|-------------|
| `sessionId` | `string` | Sesiunea de terminal către care se trimite |
| `data` | `string` | Comanda sau datele de trimis |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Abonează-te la toate ieșirile dintr-o sesiune de terminal. Returnează o **funcție de dezabonare**.

| Parametru | Tip | Descriere |
|-----------|------|-------------|
| `sessionId` | `string` | Sesiunea de terminal de urmărit |
| `callback` | `(data: string) => void` | Apelată cu fiecare bucată de ieșire |
| **Returnează** | `() => void` | Apelează aceasta pentru a opri ascultarea |
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
**Important:** Salvează întotdeauna funcția de dezabonare și apeleaz-o în `deactivate()` pentru a preveni scurgerile de memorie.

---

### `ctx.sftp` — Transfer de fișiere

> **Status: În Curând** — API-ul SFTP este definit, dar nu este încă conectat la motorul SFTP al aplicației. `list()` returnează în prezent un array gol, iar `upload()`/`download()` sunt ineficiente. Acesta va fi complet implementat într-o versiune viitoare. Până atunci, folosește `ctx.terminal.send()` cu comenzi `scp` sau `rsync` ca soluție alternativă.

#### `sftp.list(sessionId, path)`

Listează fișierele dintr-un director de la distanță.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Încarcă un fișier de pe mașina locală pe serverul de la distanță.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Descarcă un fișier de pe serverul de la distanță pe mașina locală.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Soluție alternativă (până când API-ul SFTP este activ):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfață utilizator

#### `ui.addSidebarButton(options)`

Adaugă un buton în bara laterală WIA SOOM.

| Opțiune | Tip | Necesare | Descriere |
|---------|-----|----------|-------------|
| `id` | `string` | Nu | ID unic (implicit numele plugin-ului) |
| `icon` | `string` | Da | Numele iconiței Lucide (de exemplu, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Da | Textul butonului afișat în bara laterală |
| `onClick` | `() => void` | Da | Funcția apelată când butonul este apăsat |
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
**Referință iconițe:** Răsfoiește toate iconițele disponibile la [lucide.dev/icons](https://lucide.dev/icons)

> **Notă de compatibilitate:** Unele plugin-uri mai vechi folosesc argumente poziționale precum `addSidebarButton(id, icon, label, onClick)`. API-ul oficial folosește un **obiect de opțiuni** așa cum este documentat mai sus. Folosește întotdeauna stilul obiectului pentru plugin-uri noi.

#### `ui.openWebview(options)`

Deschide o fereastră popup cu conținut HTML personalizat. Așa construiești interfețe bogate.

| Opțiune | Tip | Descriere |
|---------|-----|-------------|
| `title` | `string` | Titlul ferestrei |
| `html` | `string` | Conținut HTML complet de redat |
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
> Vezi [Partea 3](#part-3-building-custom-ui-with-webviews) pentru modele avansate de webview.

#### `ui.showNotification(type, message)`

Afișează o notificare toast.

| Parametru | Tip | Descriere |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stilul notificării |
| `message` | `string` | Textul de afișat |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Adaugă un element text persistent în bara de stare de jos.

| Parametru | Tip | Descriere |
|-----------|------|-------------|
| `id` | `string` | ID-ul unic pentru acest element de stare |
| `text` | `string` | Textul de afișat |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Stocare persistentă

Setările pluginului sunt stocate permanent în `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Citește o valoare salvată.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Returnează `undefined` dacă cheia nu există.

#### `settings.set(key, value)`

Salvează o valoare. Suportă șiruri de caractere, numere, booleeni, array-uri și obiecte.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemplu: Reține preferințele utilizatorului**
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

### `ctx.ai` — Integrarea AI

> **Status: În curând** — API-ul AI este definit, dar nu este încă conectat la Soomy. În prezent returnează `{ response: 'AI not yet connected' }`. Integrarea completă a AI este planificată pentru o viitoare versiune.

#### `ai.chat(messages, options?)`

Trimite mesaje asistentului AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Partea 3: Construirea UI personalizate cu Webviews

API-ul `openWebview()` îți permite să construiești UI-uri de tip dashboard cu HTML, CSS și JavaScript — toate într-o fereastră popup.

> **Limitare importantă:** Webview-urile sunt **doar pentru afișare**. Ele nu pot apela API-urile pluginului (`ctx.settings`, `ctx.terminal`, etc.). Folosește butoane în bara laterală pentru toate acțiunile utilizatorului și folosește `openWebview()` pentru a afișa starea curentă. Dacă ai nevoie de funcții interactive, declanșează-le din butoanele din bara laterală și redeschide webview-ul pentru a actualiza afișajul.

### Model: Comandă Terminal → Parsare Ieșire → Afișare în HTML

Acesta este cel mai comun model de plugin. Rulezi o comandă, parsezi rezultatul și îl afișezi vizual.
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
### Model: Dashboard Interactiv cu Auto-Refresh
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
### Model: Afișarea Setărilor într-un Webview

> **Notă:** Webview-urile sunt doar pentru afișare — ele nu pot apela API-urile pluginului. Folosește `ctx.settings` în handler-ele butoanelor din bara laterală pentru a modifica setările și folosește `openWebview()` pentru a arăta starea curentă.
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

## Partea 4: Publicarea Pluginului Tău

### Pasul 1: Testare locală

1. Copiază pluginul tău în `~/.wia-soom/plugins/{your-plugin}/`
2. Repornește WIA SOOM
3. Verifică dacă funcționează: butonul din bara laterală apare, funcțiile funcționează corect
4. Testează cazurile limită: ce se întâmplă dacă nu este conectat niciun terminal?

### Pasul 2: Pregătirea pentru trimitere

Folderul pluginului tău trebuie să conțină:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Câmpuri necesare `package.json`:**

| Câmp | Descriere | Exemplu |
|-------|-------------|---------|
| `name` | ID unic în format kebab-case | `"my-awesome-plugin"` |
| `version` | Versiune semantică | `"1.0.0"` |
| `description` | Descriere pe o linie | `"Monitorizează jurnalele de acces nginx în timp real"` |
| `author` | Numele tău | `"John Doe"` |
| `main` | Punct de intrare | `"index.js"` |

**Câmpuri opționale:**

| Câmp | Descriere |
|-------|-------------|
| `license` | Tipul licenței (MIT recomandat) |
| `keywords` | Array de tag-uri de căutare |
| `soom.minVersion` | Versiunea minimă WIA SOOM necesară |

### Pasul 3: Trimite la Registrul de Pluginuri

1. ****Package** your plugin as a ZIP file
2. **Adaugă** pluginul tău în `plugins/{numele-pluginului-tău}/`
3. **Trimite** o Pull Request

### Pasul 4: Revizuire și aprobat

Revizuim fiecare plugin pentru:

- **Securitate** — fără API-uri periculoase (vezi [Reguli de Securitate](#security-rules))
- **Calitate** — funcționează? Este codul curat?
- **Utilitate** — rezolvă o problemă reală?

După aprobat:
1. Pluginul tău este adăugat în `registry.json`
2. Un pachet ZIP este creat în `dist/`
3. Pluginul tău apare în **Plugin Store** pentru toți utilizatorii WIA SOOM!

---

## Partea 5: Cele Mai Bune Practici

### Reguli de Securitate

Aceste reguli sunt **obligatorii**. Pluginurile care le încalcă vor fi respinse.

| Regulă | De ce |
|------|-----|
| **NICIODATĂ** nu folosi `eval()` sau `new Function()` | Risc de injecție de cod |
| **NICIODATĂ** nu folosi `child_process`, `exec()`, `spawn()` | Folosește doar `ctx.terminal.send()` pentru comenzi |
| **NICIODATĂ** nu accesa URL-uri externe | Excepție: punctele finale API de la `wiasoom.com` |
| **NICIODATĂ** nu accesa `process.env` | Variabilele de mediu pot conține secrete |
| **NICIODATĂ** nu folosi `require('fs')` direct | Folosește `ctx.settings` pentru stocare, `ctx.sftp` pentru transfer de fișiere |
| **NICIODATĂ** nu folosi pachete externe npm | Numai JavaScript pur — fără node_modules |
| **TREBUIE** să folosești `ctx.terminal.send()` pentru toate comenzile remote | Acest lucru trece prin canalul SSH securizat |
| **TREBUIE** să faci curățenie în `deactivate()` | Elimină listenerii, curăță intervalele |

### Gestionarea Erorilor

Întotdeauna înfășoară operațiunile riscante în try/catch:
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
### Curățenie în deactivate()

Dacă pluginul tău creează intervale, listeneri sau subscripții — curăță-le:
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
### Suport i18n

WIA SOOM suportă 254 de limbi. Pentru a face eticheta pluginului tău traductibilă, folosește o abordare simplă:
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

## Partea 6: Exemple din Lumea Reală

### Exemplul 1: Verificator de Disc pe Server

Rulează `df -h` pe serverul remote și arată spațiul utilizat/disponibil în bara de stare.
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

### Exemplul 2: Manager TODO

Un plugin care gestionează o listă TODO folosind setări pentru stocare persistentă și un webview pentru afișare.

> **Model de design:** Deoarece webview-urile nu pot apela direct API-urile pluginului, acest plugin folosește o abordare de "snapshot" — citește TODO-urile din setări, le redă ca HTML doar în citire și oferă acțiuni bazate pe sidebar pentru adăugarea de elemente. Webview-ul este un **strat de afișare**, nu un formular interactiv.
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

### Exemplul 3: Monitor de Erori

Monitorizează ieșirea terminalului și trimite o notificare atunci când sunt detectate modele specifice.
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

## Anexă: Categorii & Icoane

### Categorii de Pluginuri (29)

Folosește acestea în `package.json` `keywords` sau când trimiți la registru:

| Categoria | Descriere |
|-----------|-----------|
| `server` | Management general al serverului |
| `devtools` | Instrumente de dezvoltare |
| `calculator` | Calculatoare și convertoare |
| `simulator` | Simulatoare |
| `game` | Jocuri terminale |
| `business` | Instrumente pentru afaceri |
| `security` | Securitate și audit |
| `web` | Managementul serverelor web |
| `education` | Instrumente educaționale |
| `health` | Instrumente legate de sănătate |
| `islamic` | Instrumente islamice (timpuri de rugăciune, etc.) |
| `science` | Instrumente științifice |
| `quantum` | Instrumente de calcul cuantic |
| `ai` | Instrumente alimentate de AI |
| `biotech` | Instrumente de biotehnologie |
| `space` | Instrumente pentru spațiu și astronomie |
| `network` | Instrumente de rețea |
| `database` | Managementul bazelor de date |
| `monitoring` | Monitorizarea serverelor |
| `devops` | DevOps și CI/CD |
| `utility` | Utilitare generale |
| `design` | Instrumente de design |
| `ecommerce` | Instrumente de comerț electronic |
| `automation` | Instrumente de automatizare |
| `kpop` | Instrumente legate de K-pop |
| `accessibility` | Instrumente de accesibilitate |
| `analytics` | Analiză și raportare |
| `wia` | Instrumente pentru ecosistemul WIA |
| `all` | Apare în toate categoriile |

### Icoane Recomandate (Lucide)

| Numele Icoanei | Folosit pentru |
|----------------|----------------|
| `server` | Managementul serverului |
| `shield` | Securitate |
| `database` | Bază de date |
| `activity` | Monitorizare |
| `terminal` | Instrumente terminale |
| `code` | Dezvoltare |
| `hard-drive` | Disc/stocare |
| `network` | Rețea |
| `lock` | Autentificare/criptare |
| `eye` | Observare/monitorizare |
| `check-square` | Sarcini/TODO |
| `layout-dashboard` | Panouri de control |
| `settings` | Configurare |
| `zap` | Automatizare |
| `globe` | Web/internațional |

Răsfoiește toate cele 1,500+ icoane: [lucide.dev/icons](https://lucide.dev/icons)

---

## Ai nevoie de ajutor?

- **Probleme GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Probleme cu Pluginuri:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Pluginuri Exemplu:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construiește ceva uimitor. Împărtășește-l cu lumea.</em></p>
<p align="center"><em>— Echipa WIA SOOM</em></p>