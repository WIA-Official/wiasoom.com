<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guida per Sviluppatori di Plugin WIA SOOM</h1>
<p align="center"><strong>Crea il tuo plugin in 5 minuti.</strong></p>
<p align="center">Crea potenti strumenti server, dashboard e automazioni — direttamente all'interno di WIA SOOM.</p>

---

## Indice

- [Parte 1: Inizio Veloce — Il Tuo Primo Plugin in 5 Minuti](#parte-1-inizio-veloce--il-tuo-primo-plugin-in-5-minuti)
- [Parte 2: Riferimento API del Contesto del Plugin](#parte-2-riferimento-api-del-contesto-del-plugin)
  - [ctx.terminal](#ctxterminal--eseguire-comandi-su-server-remoti)
  - [ctx.sftp](#ctxsftp--trasferimento-file)
  - [ctx.ui](#ctxui--interfaccia-utente)
  - [ctx.settings](#ctxsettings--memoria-persistente)
  - [ctx.ai](#ctxai--integrazione-ai)
- [Parte 3: Costruire UI Personalizzate con Webviews](#parte-3-costruire-ui-personalizzate-con-webviews)
- [Parte 4: Pubblicare il Tuo Plugin](#parte-4-pubblicare-il-tuo-plugin)
- [Parte 5: Migliori Pratiche](#parte-5-migliori-pratiche)
- [Parte 6: Esempi del Mondo Reale](#parte-6-esempi-del-mondo-reale)
- [Appendice: Categorie & Icone](#appendice-categorie--icone)

---

## Parte 1: Inizio Veloce — Il Tuo Primo Plugin in 5 Minuti

### Cosa costruirai

Un plugin "Hello World" che aggiunge un pulsante alla barra laterale. Quando cliccato, mostra una notifica.

### Passo 1: Crea la cartella del plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Passo 2: Crea package.json
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

### Passo 3: Crea index.js
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
### Passo 4: Riavvia WIA SOOM

Riavvia l'app (o attiva/disattiva il plugin in Impostazioni → Plugin).

Dovresti vedere un pulsante **"Hello World"** nella barra laterale. Cliccalo — vedrai una notifica di successo!

### Come funziona
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

## Parte 2: Riferimento API del Contesto del Plugin

Quando la tua funzione `activate(context)` viene chiamata, `context` (o `ctx`) fornisce queste API:
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

Invia un comando (o qualsiasi dato) a una sessione terminale attiva.

| Parametro | Tipo | Descrizione |
|-----------|------|-------------|
| `sessionId` | `string` | La sessione terminale a cui inviare |
| `data` | `string` | Il comando o i dati da inviare |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Iscriviti a tutti gli output di una sessione terminale. Restituisce una **funzione di disiscrizione**.

| Parametro | Tipo | Descrizione |
|-----------|------|-------------|
| `sessionId` | `string` | La sessione terminale da monitorare |
| `callback` | `(data: string) => void` | Chiamato con ogni blocco di output |
| **Restituisce** | `() => void` | Chiama questo per smettere di ascoltare |
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
**Importante:** Salva sempre la funzione di disiscrizione e chiamala in `deactivate()` per prevenire perdite di memoria.

---

### `ctx.sftp` — Trasferimento file

> **Stato: In Arrivo** — L'API SFTP è definita ma non ancora collegata al motore SFTP dell'app. `list()` attualmente restituisce un array vuoto, e `upload()`/`download()` non fanno nulla. Questo sarà completamente implementato in una futura release. Per ora, usa `ctx.terminal.send()` con comandi `scp` o `rsync` come soluzione alternativa.

#### `sftp.list(sessionId, path)`

Elenca i file in una directory remota.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Carica un file dalla macchina locale al server remoto.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Scarica un file dal server remoto alla macchina locale.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Soluzione alternativa (fino a quando l'API SFTP sarà attiva):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfaccia utente

#### `ui.addSidebarButton(options)`

Aggiungi un pulsante alla barra laterale di WIA SOOM.

| Opzione | Tipo | Richiesta | Descrizione |
|--------|------|----------|-------------|
| `id` | `string` | No | ID unico (di default è il nome del plugin) |
| `icon` | `string` | Sì | Nome dell'icona Lucide (es. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Sì | Testo del pulsante mostrato nella barra laterale |
| `onClick` | `() => void` | Sì | Funzione chiamata quando il pulsante viene cliccato |
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
**Riferimento icone:** Sfoglia tutte le icone disponibili su [lucide.dev/icons](https://lucide.dev/icons)

> **Nota di compatibilità:** Alcuni plugin più vecchi utilizzano argomenti posizionali come `addSidebarButton(id, icon, label, onClick)`. L'API ufficiale utilizza un **oggetto opzioni** come documentato sopra. Usa sempre lo stile oggetto per i nuovi plugin.

#### `ui.openWebview(options)`

Apri una finestra popup con contenuto HTML personalizzato. Questo è il modo in cui costruisci UI ricche.

| Opzione | Tipo | Descrizione |
|--------|------|-------------|
| `title` | `string` | Titolo della finestra |
| `html` | `string` | Contenuto HTML completo da renderizzare |
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
> Vedi [Parte 3](#part-3-building-custom-ui-with-webviews) per schemi avanzati di webview.

#### `ui.showNotification(type, message)`

Mostra una notifica toast.

| Parametro | Tipo | Descrizione |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stile della notifica |
| `message` | `string` | Testo da mostrare |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Aggiungi un elemento di testo persistente alla barra di stato in basso.

| Parametro | Tipo | Descrizione |
|-----------|------|-------------|
| `id` | `string` | ID unico per questo elemento di stato |
| `text` | `string` | Testo da visualizzare |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Memoria persistente

Le impostazioni del plugin sono memorizzate permanentemente in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Leggi un valore salvato.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Restituisce `undefined` se la chiave non esiste.

#### `settings.set(key, value)`

Salva un valore. Supporta stringhe, numeri, booleani, array e oggetti.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Esempio: Ricorda le preferenze dell'utente**
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

> **Stato: In Arrivo** — L'API AI è definita ma non ancora collegata a Soomy. Attualmente restituisce `{ response: 'AI non ancora connessa' }`. L'integrazione completa dell'AI è pianificata per una futura release.

#### `ai.chat(messages, options?)`

Invia messaggi all'assistente AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Parte 3: Costruire UI Personalizzate con Webviews

L'API `openWebview()` ti consente di costruire interfacce dashboard con HTML, CSS e JavaScript — tutto all'interno di una finestra popup.

> **Limitazione importante:** Le webview sono **solo per visualizzazione**. Non possono richiamare le API del plugin (`ctx.settings`, `ctx.terminal`, ecc.). Usa i pulsanti della barra laterale per tutte le azioni degli utenti e usa `openWebview()` per visualizzare lo stato attuale. Se hai bisogno di funzionalità interattive, attivale dai pulsanti della barra laterale e riapri la webview per aggiornare la visualizzazione.

### Schema: Comando del Terminale → Analizza Output → Mostra in HTML

Questo è lo schema di plugin più comune. Esegui un comando, analizza il risultato e visualizzalo.
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
### Schema: Dashboard Interattiva con Aggiornamento Automatico
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
### Schema: Visualizzazione delle Impostazioni in una Webview

> **Nota:** Le webview sono solo per visualizzazione — non possono richiamare le API del plugin. Usa `ctx.settings` nei gestori dei pulsanti della barra laterale per modificare le impostazioni e usa `openWebview()` per mostrare lo stato attuale.
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

## Parte 4: Pubblicare il Tuo Plugin

### Passo 1: Testare localmente

1. Copia il tuo plugin in `~/.wia-soom/plugins/{your-plugin}/`
2. Riavvia WIA SOOM
3. Verifica che funzioni: il pulsante della barra laterale appare, le funzionalità funzionano correttamente
4. Testa i casi limite: cosa succede se nessun terminale è connesso?

### Passo 2: Prepararsi per la sottomissione

La tua cartella del plugin deve contenere:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Campi richiesti di `package.json`:**

| Campo | Descrizione | Esempio |
|-------|-------------|---------|
| `name` | ID unico in kebab-case | `"my-awesome-plugin"` |
| `version` | Versione semantica | `"1.0.0"` |
| `description` | Descrizione in una riga | `"Monitora i log di accesso nginx in tempo reale"` |
| `author` | Il tuo nome | `"John Doe"` |
| `main` | Punto di ingresso | `"index.js"` |

**Campi opzionali:**

| Campo | Descrizione |
|-------|-------------|
| `license` | Tipo di licenza (MIT raccomandata) |
| `keywords` | Array di tag di ricerca |
| `soom.minVersion` | Versione minima di WIA SOOM richiesta |

### Passo 3: Invia al Registro Plugin

1. ****Package** your plugin as a ZIP file
2. **Aggiungi** il tuo plugin a `plugins/{your-plugin-name}/`
3. **Invia** una Pull Request

### Passo 4: Revisione e approvazione

Esaminiamo ogni plugin per:

- **Sicurezza** — nessuna API pericolosa (vedi [Regole di Sicurezza](#security-rules))
- **Qualità** — funziona? Il codice è pulito?
- **Utilità** — risolve un problema reale?

Dopo l'approvazione:
1. Il tuo plugin viene aggiunto a `registry.json`
2. Viene creato un pacchetto ZIP in `dist/`
3. Il tuo plugin appare nel **Plugin Store** per tutti gli utenti di WIA SOOM!

---

## Parte 5: Migliori Pratiche

### Regole di Sicurezza

Queste regole sono **obbligatorie**. I plugin che le violano saranno rifiutati.

| Regola | Perché |
|--------|--------|
| **MAI** usare `eval()` o `new Function()` | Rischio di iniezione di codice |
| **MAI** usare `child_process`, `exec()`, `spawn()` | Usa solo `ctx.terminal.send()` per i comandi |
| **MAI** recuperare URL esterni | Eccezione: endpoint API di `wiasoom.com` |
| **MAI** accedere a `process.env` | Le variabili d'ambiente possono contenere segreti |
| **MAI** usare `require('fs')` direttamente | Usa `ctx.settings` per lo storage, `ctx.sftp` per il trasferimento file |
| **MAI** usare pacchetti esterni npm | Solo JavaScript puro — niente node_modules |
| **DEVE** usare `ctx.terminal.send()` per tutti i comandi remoti | Questo passa attraverso il canale SSH sicuro |
| **DEVE** pulire in `deactivate()` | Rimuovere listener, cancellare intervalli |

### Gestione degli Errori

Avvolgi sempre le operazioni rischiose in try/catch:
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

Se il tuo plugin crea intervalli, listener o iscrizioni — puliscili:
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
### Supporto i18n

WIA SOOM supporta 254 lingue. Per rendere l'etichetta del tuo plugin traducibile, utilizza un approccio semplice:
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

## Parte 6: Esempi del Mondo Reale

### Esempio 1: Controllore Disco Server

Esegue `df -h` sul server remoto e mostra lo spazio utilizzato/disponibile nella barra di stato.
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

### Esempio 2: Gestore TODO

Un plugin che gestisce una lista TODO utilizzando impostazioni per lo storage persistente e un webview per la visualizzazione.

> **Modello di design:** Poiché i webview non possono chiamare direttamente le API del plugin, questo plugin utilizza un approccio "snapshot" — legge i TODO dalle impostazioni, li rende come HTML di sola lettura e fornisce azioni basate sulla sidebar per aggiungere elementi. Il webview è uno strato di **visualizzazione**, non un modulo interattivo.
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

### Esempio 3: Monitor di Errori

Monitora l'output del terminale e invia una notifica quando vengono rilevati schemi specifici.
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

## Appendice: Categorie & Icone

### Categorie dei Plugin (29)

Usa queste nel tuo `package.json` `keywords` o quando invii al registro:

| Categoria | Descrizione |
|----------|-------------|
| `server` | Gestione generale del server |
| `devtools` | Strumenti di sviluppo |
| `calculator` | Calcolatori e convertitori |
| `simulator` | Simulatori |
| `game` | Giochi da terminale |
| `business` | Strumenti per il business |
| `security` | Sicurezza e auditing |
| `web` | Gestione del server web |
| `education` | Strumenti educativi |
| `health` | Strumenti relativi alla salute |
| `islamic` | Strumenti islamici (orari delle preghiere, ecc.) |
| `science` | Strumenti scientifici |
| `quantum` | Strumenti di calcolo quantistico |
| `ai` | Strumenti alimentati dall'IA |
| `biotech` | Strumenti di biotecnologia |
| `space` | Strumenti per lo spazio e l'astronomia |
| `network` | Strumenti di rete |
| `database` | Gestione del database |
| `monitoring` | Monitoraggio del server |
| `devops` | DevOps e CI/CD |
| `utility` | Utilità generali |
| `design` | Strumenti di design |
| `ecommerce` | Strumenti per l'e-commerce |
| `automation` | Strumenti di automazione |
| `kpop` | Strumenti relativi al K-pop |
| `accessibility` | Strumenti di accessibilità |
| `analytics` | Analisi e reporting |
| `wia` | Strumenti per l'ecosistema WIA |
| `all` | Appare in tutte le categorie |

### Icone Raccomandate (Lucide)

| Nome Icona | Uso per |
|-----------|---------|
| `server` | Gestione del server |
| `shield` | Sicurezza |
| `database` | Database |
| `activity` | Monitoraggio |
| `terminal` | Strumenti da terminale |
| `code` | Sviluppo |
| `hard-drive` | Disco/memoria |
| `network` | Networking |
| `lock` | Autenticazione/crittografia |
| `eye` | Osservazione/monitoraggio |
| `check-square` | Attività/TODO |
| `layout-dashboard` | Dashboard |
| `settings` | Configurazione |
| `zap` | Automazione |
| `globe` | Web/internazionale |

Sfoglia tutte le 1.500+ icone: [lucide.dev/icons](https://lucide.dev/icons)

---

## Hai bisogno di aiuto?

- **Problemi GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemi Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Esempio:** [Website](https://wiasoom.com)
- **Sito Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Costruisci qualcosa di straordinario. Condividilo con il mondo.</em></p>
<p align="center"><em>— Il Team di WIA SOOM</em></p>