<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guida pe Sviluppaturi di Plugin WIA SOOM</h1>
<p align="center"><strong>Crea u tò plugin in 5 minuti.</strong></p>
<p align="center">Crea strumenti potenti pe server, dashboard e automazioni — drittu dintra WIA SOOM.</p>

---

## Tavola di Contenuti

- [Parti 1: Inizio Veloce — U Tò Primu Plugin in 5 Minuti](#parti-1-inizio-veloce--u-tò-primu-plugin-in-5-minuti)
- [Parti 2: Riferimentu API di Context di Plugin](#parti-2-riferimentu-api-di-context-di-plugin)
  - [ctx.terminal](#ctxterminal--esegui-comandi-su-server-remoti)
  - [ctx.sftp](#ctxsftp--trasferimentu-file)
  - [ctx.ui](#ctxui--interfaccia-utente)
  - [ctx.settings](#ctxsettings--memoria-persistente)
  - [ctx.ai](#ctxai--integrazione-ai)
- [Parti 3: Costruire UI Personalizzata cu Webviews](#parti-3-costruire-ui-personalizzata-cu-webviews)
- [Parti 4: Pubblicare U Tò Plugin](#parti-4-pubblicare-u-tò-plugin)
- [Parti 5: Megghiu Pratiche](#parti-5-meghiu-pratiche)
- [Appendice: Categorie & Icone](#appendice-categorie--icone)

---

## Parti 1: Inizio Veloce — U Tò Primu Plugin in 5 Minuti

### Chi costruirai

Un plugin "Hello World" ca aggiungi un buttuni a la sidebar. Quannu cliccatu, mostra na notifica.

### Passu 1: Crea la cartella di plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Passu 2: Crea package.json
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
**Campi richiesti:** `name`, `version`, `description`, `author`, `main`

### Passu 3: Crea index.js
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
### Passu 4: Riavvia WIA SOOM

Riavvia l'app (o attiva/disattiva u plugin in Impostazioni → Plugins).

Dovresti vidiri un **"Hello World"** buttuni nella sidebar. Cliccalu — vidrai na notifica di successu!

### Comu funziona
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

## Parti 2: Riferimentu API di Context di Plugin

Quannu a tò funzione `activate(context)` è chiamata, `context` (o `ctx`) furnisci sti API:
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

### `ctx.terminal` — Esegui comandi su server remoti

#### `terminal.send(sessionId, data)`

Manda un comando (o qualsiasi dati) a na sessione di terminale attiva.

| Parametro | Tipo | Descrizzioni |
|-----------|------|--------------|
| `sessionId` | `string` | A sessione di terminale a cui mannari |
| `data` | `string` | U comando o dati da mannari |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Iscriviti a tutti l'output da na sessione di terminale. Ritorna na **funzione di disiscrizione**.

| Parametro | Tipo | Descrizzioni |
|-----------|------|--------------|
| `sessionId` | `string` | A sessione di terminale da osservare |
| `callback` | `(data: string) => void` | Chiamata cu ogni pezzu di output |
| **Ritorna** | `() => void` | Chiama chistu pe fermari d'ascoltare |
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
**Impurtanti:** Sempri salva a funzione di disiscrizione e chiamala in `deactivate()` pe preveniri perdite di memoria.

---

### `ctx.sftp` — Trasferimentu file

> **Statu: Arrivando Presto** — L'API SFTP è definita ma ancora non è cunnessa all'ingegneria SFTP di l'app. `list()` attualmente ritorna un array vacante, e `upload()`/`download()` sò no-ops. Chistu sarà implementatu completamente in una futura versione. Pe ora, usa `ctx.terminal.send()` cu comandi `scp` o `rsync` come soluzione temporanea.

#### `sftp.list(sessionId, path)`

Elenca i file in una directory remota.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Carica un file da a macchina locale a u server remotu.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Scarica un file da u server remotu a a macchina locale.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Soluzione temporanea (fino a quannu l'API SFTP è attiva):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfaccia utente

#### `ui.addSidebarButton(options)`

Aggiungi un buttuni a la sidebar di WIA SOOM.

| Opzione | Tipo | Richiesta | Descrizzioni |
|---------|------|-----------|--------------|
| `id` | `string` | No | ID unicu (di default u nome di plugin) |
| `icon` | `string` | Si | Nome icona Lucide (es., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Si | Testu di buttuni mostratu in sidebar |
| `onClick` | `() => void` | Si | Funzione chiamata quannu u buttuni è cliccatu |
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
**Riferimentu icone:** Sfoglia tutte l'icone disponibili a [lucide.dev/icons](https://lucide.dev/icons)

> **Nota di compatibilità:** Alcuni plugin più vechji usanu argomenti posizionali come `addSidebarButton(id, icon, label, onClick)`. L'API ufficiale usa un **oggettu di opzioni** cum'è documentatu sopra. Sempri usa u stile di oggettu pe novi plugin.

#### `ui.openWebview(options)`

Apre una finestra popup cu cuntenutu HTML personalizatu. Chistu è comu costruisci UI ricche.

| Opzione | Tipo | Descrizzioni |
|---------|------|--------------|
| `title` | `string` | Titulu di finestra |
| `html` | `string` | Cuntenutu HTML cumpletu da renderizzari |
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
> Vidi [Part 3](#part-3-building-custom-ui-with-webviews) pi mudelli avanzati di webview.

#### `ui.showNotification(type, message)`

Mostra na notifica toast.

| Parametru | Tipu | Descrizzioni |
|-----------|------|--------------|
| `type` | `'success' \| 'error' \| 'info'` | Stile di notifica |
| `message` | `string` | Testu di mostrare |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Aggiungi un elementu di testu persistenti a la barra di stato in fondu.

| Parametru | Tipu | Descrizzioni |
|-----------|------|--------------|
| `id` | `string` | ID unicu pi chistu elementu di stato |
| `text` | `string` | Testu di visualizzari |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Storage persistenti

I settings di lu plugin sunnu sarvati permanentementi in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Leggi un valore sarvatu.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Ritorna `undefined` si la chiavi nun esisti.

#### `settings.set(key, value)`

Sarva un valore. Supporta stringhe, numiri, booleani, array, e oggetti.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Esempiu: Ricorda i preferenzi di l'utenti**
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

### `ctx.ai` — Integrazione AI

> **Statu: Arrivannu Prestu** — L'API AI è definita ma ancora nun è cunnessa a Soomy. Attualmente ritorna `{ response: 'AI not yet connected' }`. L'integrazione completa di AI è pianificata pi na futura rilascio.

#### `ai.chat(messages, options?)`

Manda messaggi all'assistente AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Costruiri UI Custom cu Webviews

L'API `openWebview()` ti permette di custruiri UI di dashboard cu HTML, CSS, e JavaScript — tuttu dintra a na finestra popup.

> **Limitazioni impurtanti:** I webviews sunnu **sulu di visualizzazioni**. Nun ponnu chiamari API di plugin (`ctx.settings`, `ctx.terminal`, etc.). Usa i buttuni di sidebar pi tutti l'azzioni di l'utenti, e usa `openWebview()` pi mustrari lu statu attuali. Si hai bisognu di funzioni interattivi, attivali di buttuni di sidebar e ri-apri lu webview pi rinfrescari la visualizzazioni.

### Mudellu: Comandu di Terminal → Parsifica Output → Mostra in HTML

Chistu è lu mudellu di plugin cchiù cumuni. Tu lanci un comandu, parsifichi lu risultatu, e lu mostri visualmenti.
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
### Mudellu: Dashboard Interattivu cu Auto-Rinfrescata
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
### Mudellu: Mostrari i Settings in un Webview

> **Nota:** I webviews sunnu sulu di visualizzazioni — nun ponnu chiamari API di plugin. Usa `ctx.settings` nei handler di buttuni di sidebar pi mudificari i settings, e usa `openWebview()` pi mustrari lu statu attuali.
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

## Part 4: Pubblicari lu Tò Plugin

### Passu 1: Testa localmenti

1. Copia lu tò plugin in `~/.wia-soom/plugins/{your-plugin}/`
2. Riavvia WIA SOOM
3. Verifica ca funziona: lu buttuni di sidebar appare, i funzioni funzionanu currettamenti
4. Testa i casi estremi: chi succedi si nun c'è nisciunu terminal cunnessu?

### Passu 2: Preparati pi la sottomissioni

La cartella di lu tò plugin ha a cunteniri:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Campi richiesti di `package.json`:**

| Campo | Descrizzioni | Esempiu |
|-------|--------------|---------|
| `name` | ID unicu in kebab-case | `"my-awesome-plugin"` |
| `version` | Versioni semantica | `"1.0.0"` |
| `description` | Descrizzioni in una linea | `"Monitors nginx access logs in real-time"` |
| `author` | U to nomu | `"John Doe"` |
| `main` | Puntu d'entrata | `"index.js"` |

**Campi opzionali:**

| Campo | Descrizzioni |
|-------|--------------|
| `license` | Tipu di licenza (MIT cunsigliatu) |
| `keywords` | Array di tag di ricerca |
| `soom.minVersion` | Versioni minimu di WIA SOOM necessaria |

### Passu 3: Sottometti a u Registru di Plugin

1. ****Package** your plugin as a ZIP file
2. **Aggiungi** u to plugin a `plugins/{your-plugin-name}/`
3. **Sottometti** una Pull Request

### Passu 4: Revisione e approvazione

Revisamu ogni plugin per:

- **Sicurezza** — nessun API periculusu (vidi [Reguli di Sicurezza](#security-rules))
- **Qualità** — funziona? U codice è pulitu?
- **Utilità** — risolve un problema reale?

Dopu l'approvazione:
1. U to plugin è aghjuntu à `registry.json`
2. Un pacchettu ZIP hè creatu in `dist/`
3. U to plugin appare in u **Plugin Store** per tutti l'utenti di WIA SOOM!

---

## Parte 5: Megliu Pratiche

### Reguli di Sicurezza

Questi reguli sò **obbligatori**. I plugin chì li violanu seranu rifiutati.

| Regola | Perchè |
|--------|--------|
| **MAI** utilizà `eval()` o `new Function()` | Rischiu di iniezione di codice |
| **MAI** utilizà `child_process`, `exec()`, `spawn()` | Utilizà solu `ctx.terminal.send()` per i cumandamenti |
| **MAI** recuperà URL esterni | Eccezzioni: punti API di `wiasoom.com` |
| **MAI** accede à `process.env` | I variabili d'ambiente ponu cuntene secreti |
| **MAI** utilizà `require('fs')` direttamente | Utilizà `ctx.settings` per u almacenamentu, `ctx.sftp` per u trasferimentu di file |
| **MAI** utilizà pacchetti esterni npm | Sulu JavaScript puru — senza node_modules |
| **DEV'ESSERE** utilizatu `ctx.terminal.send()` per tutti i cumandamenti remoti | Questu passa attraversu u canale SSH sicuru |
| **DEV'ESSERE** pulitu in `deactivate()` | Rimuovere ascoltatori, pulisce intervalli |

### Gestione di Errori

Sempre avvolge operazioni rischiose in try/catch:
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
### Pulizia in deactivate()

Se u to plugin crea intervalli, ascoltatori, o abbonamenti — puliscili:
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
### Supportu i18n

WIA SOOM supporta 254 lingue. Per fà u to etichetta di plugin traducibile, utilizza un approcciu simplici:
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

## Parte 6: Esempi di Mondo Reale

### Esempiu 1: Verificatore di Disco Server

Esegui `df -h` nantu à u server remotu è mostra spaziu utilizatu/dispunibule in a barra di statutu.
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

### Esempiu 2: Gestore di TODO

Un plugin chì gestisce una lista TODO utilizendu impostazioni per l'archiviazione persistente è un webview per a visualizazione.

> **Schema di design:** Datu chì i webviews ùn ponu micca chjamà direttamente l'API di plugin, stu plugin utilizza un approcciu "snapshot" — legge i TODO da e impostazioni, li rende cum'è HTML in sola lettura, è fornisce azzioni basate in sidebar per aghjunghje elementi. U webview hè un **livellu** di visualizazione, micca un modulo interattivu.
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

### Esempiu 3: Monitor di Errori

Monitora l'output di u terminale è manda una notifica quandu schemi specifici sò rilevati.
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

## Appendice: Categorie & Iconi

### Categorie di Plugin (29)

Usa sti termini nei tuoi `package.json` `keywords` o quannu sottometti a lu registru:

| Categoria | Descrizzioni |
|----------|-------------|
| `server` | Gestione generale di server |
| `devtools` | Strumenti di sviluppu |
| `calculator` | Calcolatori e convertitori |
| `simulator` | Simulatori |
| `game` | Giochi di terminale |
| `business` | Strumenti per l'affari |
| `security` | Sicurezza e auditing |
| `web` | Gestione di server web |
| `education` | Strumenti educativi |
| `health` | Strumenti relativi alla salute |
| `islamic` | Strumenti islamici (orari di preghiera, ecc.) |
| `science` | Strumenti scientifici |
| `quantum` | Strumenti di computazione quantistica |
| `ai` | Strumenti alimentati da AI |
| `biotech` | Strumenti di biotecnologia |
| `space` | Strumenti di spazio e astronomia |
| `network` | Strumenti di rete |
| `database` | Gestione di database |
| `monitoring` | Monitoraggio di server |
| `devops` | DevOps e CI/CD |
| `utility` | Utilità generali |
| `design` | Strumenti di design |
| `ecommerce` | Strumenti di e-commerce |
| `automation` | Strumenti di automazione |
| `kpop` | Strumenti relativi al K-pop |
| `accessibility` | Strumenti di accessibilità |
| `analytics` | Analisi e reportistica |
| `wia` | Strumenti dell'ecosistema WIA |
| `all` | Appare in tutte le categorie |

### Iconi Raccomandati (Lucide)

| Nome Icona | Uso per |
|-----------|---------|
| `server` | Gestione di server |
| `shield` | Sicurezza |
| `database` | Database |
| `activity` | Monitoraggio |
| `terminal` | Strumenti di terminale |
| `code` | Sviluppo |
| `hard-drive` | Disco/memoria |
| `network` | Networking |
| `lock` | Autenticazione/crittografia |
| `eye` | Osservazione/monitoraggio |
| `check-square` | Compiti/TODO |
| `layout-dashboard` | Dashboard |
| `settings` | Configurazione |
| `zap` | Automazione |
| `globe` | Web/internazionale |

Sfoglia tutti i 1.500+ iconi: [lucide.dev/icons](https://lucide.dev/icons)

---

## Hai Bisogno di Aiuto?

- **Problemi GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemi Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin di Esempio:** [Website](https://wiasoom.com)
- **Sito Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Crea qualcosa di straordinario. Condividilo cu lu munnu.</em></p>
<p align="center"><em>— La Squadra di WIA SOOM</em></p>