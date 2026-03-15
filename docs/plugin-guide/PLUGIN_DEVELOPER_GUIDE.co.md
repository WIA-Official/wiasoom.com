<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guida per Sviluppatori di Plugin WIA SOOM</h1>
<p align="center"><strong>Crea u to plugin in 5 minuti.</strong></p>
<p align="center">Crea strumenti server potenti, dashboard, è automazioni — direttamente in WIA SOOM.</p>

---

## Indice

- [Parte 1: Inizio Veloce — U To Primu Plugin in 5 Minuti](#parte-1-inizio-veloce--u-to-primu-plugin-in-5-minuti)
- [Parte 2: Riferimentu API di u Contextu di Plugin](#parte-2-riferimentu-api-di-u-contextu-di-plugin)
  - [ctx.terminal](#ctxterminal--esegui-cumandi-sui-server-remoti)
  - [ctx.sftp](#ctxsftp--trasferimentu-di-file)
  - [ctx.ui](#ctxui--interfaccia-utente)
  - [ctx.settings](#ctxsettings--memoria-persistente)
  - [ctx.ai](#ctxai--integrazione-ai)
- [Parte 3: Costruisce UI Persunalizata cù Webviews](#parte-3-costruisce-ui-persunalizata-cù-webviews)
- [Parte 4: Pubblicazione di u To Plugin](#parte-4-pubblicazione-di-u-to-plugin)
- [Parte 5: Megliu Pratiche](#parte-5-megli-pratiche)
- [Parte 6: Esempii di u Mondo Reale](#parte-6-esempii-di-u-mondo-reale)
- [Appendice: Categorie è Icone](#appendice-categorie--icone)

---

## Parte 1: Inizio Veloce — U To Primu Plugin in 5 Minuti

### Chì custruirete

Un plugin "Hello World" chì aghjusta un buttone à a barra laterale. Quandu hè cliccatu, mostra una notifica.

### Passu 1: Crea a cartella di plugin
§§§CHUNK_SEPARATOR§§§
### Passu 2: Crea package.json
§§§CHUNK_SEPARATOR§§§
**Campi necessarii:** `name`, `version`, `description`, `author`, `main`

### Passu 3: Crea index.js
§§§CHUNK_SEPARATOR§§§
### Passu 4: Riavvia WIA SOOM

Riavvia l'app (o attiva/disattiva u plugin in Impostazioni → Plugins).

Duvete vede un **"Hello World"** buttone in a barra laterale. Cliccateci — vedrete una notifica di successu!

### Cumu funziona
§§§CHUNK_SEPARATOR§§§
---

## Parte 2: Riferimentu API di u Contextu di Plugin

Quandu a vostra funzione `activate(context)` hè chjamata, `context` (o `ctx`) furnisce queste API:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Esegui cumandi nantu à server remoti

#### `terminal.send(sessionId, data)`

Inviate un cumandu (o qualsiasi dati) à una sessione terminal attiva.

| Parametru | Tipu | Descrizzione |
|-----------|------|-------------|
| `sessionId` | `string` | A sessione terminal à quale invià |
| `data` | `string` | U cumandu o i dati da invià |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Abbonatevi à tutte l'uscite da una sessione terminal. Ritorna una **funzione di disabbonamentu**.

| Parametru | Tipu | Descrizzione |
|-----------|------|-------------|
| `sessionId` | `string` | A sessione terminal da osservà |
| `callback` | `(data: string) => void` | Chjamata cù ogni pezzu di uscita |
| **Ritorna** | `() => void` | Chjamate questu per fermà di ascoltà |
§§§CHUNK_SEPARATOR§§§
**Impurtante:** Salva sempre a funzione di disabbonamentu è chjamala in `deactivate()` per prevene perdite di memoria.

---

### `ctx.sftp` — Trasferimentu di file

> **Statu: In Arrivu** — L'API SFTP hè definita ma ùn hè micca ancora cunnessa à u mutore SFTP di l'app. `list()` attualmente restituisce un array vacu, è `upload()`/`download()` sò no-ops. Questu serà pienamente implementatu in una futura versione. Per avà, utilizate `ctx.terminal.send()` cù i cumandi `scp` o `rsync` cum'è una soluzione alternativa.

#### `sftp.list(sessionId, path)`

Elenca i file in una directory remota.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Caricate un file da a macchina locale à u server remotu.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Scaricate un file da u server remotu à a macchina locale.
§§§CHUNK_SEPARATOR§§§
**Soluzione alternativa (fino à chì l'API SFTP hè attiva):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Interfaccia utente

#### `ui.addSidebarButton(options)`

Aghjunghjite un buttone à a barra laterale di WIA SOOM.

| Opzione | Tipu | Necessariu | Descrizzione |
|--------|------|----------|-------------|
| `id` | `string` | No | ID unicu (di default hè u nome di u plugin) |
| `icon` | `string` | Yes | Nome di l'icona Lucide (es., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Testu di u buttone mostratu in a barra laterale |
| `onClick` | `() => void` | Yes | Funzione chjamata quandu u buttone hè cliccatu |
§§§CHUNK_SEPARATOR§§§
**Riferimentu di l'icona:** Naviga tutte l'icone dispunibili à [lucide.dev/icons](https://lucide.dev/icons)

> **Nota di compatibilità:** Certi plugin più vechji utilizanu argomenti pusiziunali cum'è `addSidebarButton(id, icon, label, onClick)`. L'API ufficiale utilizza un **oggettu di opzioni** cum'è documentatu sopra. Utilizate sempre u stilu di l'oggettu per i novi plugin.

#### `ui.openWebview(options)`

Aprite una finestra pop-up cù cuntenutu HTML persunalizatu. Questu hè cumu custruite UIs ricche.

| Opzione | Tipu | Descrizzione |
|--------|------|-------------|
| `title` | `string` | Titulu di a finestra |
| `html` | `string` | Cuntenutu HTML cumpletu da renderizà |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Vedi [Parte 3](#part-3-building-custom-ui-with-webviews) per schemi avanzati di webview.

#### `ui.showNotification(type, message)`

Mostra una notifica toast.

| Parametru | Tipu | Descrizzione |
|-----------|------|--------------|
| `type` | `'success' \| 'error' \| 'info'` | Stile di notifica |
| `message` | `string` | Testu da mostrare |
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

Aghjunghje un elementu di testu persistente à a barra di statutu inferiore.

| Parametru | Tipu | Descrizzione |
|-----------|------|--------------|
| `id` | `string` | ID unicu per questu elementu di statutu |
| `text` | `string` | Testu da visualizà |
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

### `ctx.settings` — Memoria persistente

I parametri di u plugin sò almacenati permanentemente in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Leghje un valore salvatu.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Ritorna `undefined` se a chjave ùn esiste micca.

#### `settings.set(key, value)`

Salva un valore. Supporta stringhe, numeri, booleani, array, è oggetti.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Esempiu: Ricurdà e preferenze di l'utente**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Integrazione AI

> **Statu: In Arrivu** — L'API AI hè definita ma micca ancora cunnessa à Soomy. Attualmente ritorna `{ response: 'AI not yet connected' }`. L'integrazione completa di l'AI hè prevista per una futura versione.

#### `ai.chat(messages, options?)`

Invià messaghji à l'assistente AI (Soomy).
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

## Parte 3: Creazione di UI Personalizate cù Webviews

L'API `openWebview()` ti permette di custruisce UI di dashboard cù HTML, CSS, è JavaScript — tuttu in una finestra popup.

> **Limitazione impurtante:** I webviews sò **solu per visualizazione**. Ùn ponu micca chjamà l'API di plugin (`ctx.settings`, `ctx.terminal`, etc.). Usa i buttuni di sidebar per tutte l'azzioni di l'utente, è usa `openWebview()` per visualizà u statu attuale. Se hai bisognu di caratteristiche interattive, attivale da i buttuni di sidebar è riapri u webview per rinfrescà a visualizazione.

### Schema: Comandamentu di Terminal → Analizza l'Output → Mostra in HTML

Questu hè u schema di plugin più cumuni. Esegui un cumandamentu, analizza u risultatu, è visualizalu in modu vistu.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Schema: Dashboard Interattiva cù Auto-Rinfrescamentu
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Schema: Visualizazione di Parametri in un Webview

> **Nota:** I webviews sò solu per visualizazione — ùn ponu micca chjamà l'API di plugin. Usa `ctx.settings` in i gestori di buttuni di sidebar per mudificà i parametri, è usa `openWebview()` per mostrà u statu attuale.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Parte 4: Publicazione di u Tò Plugin

### Passu 1: Testa localmente

1. Copia u to plugin in `~/.wia-soom/plugins/{your-plugin}/`
2. Riavvia WIA SOOM
3. Verifica chì funziona: u buttone di sidebar appare, e caratteristiche funzionanu currettamente
4. Testa i casi estremi: chì succede se ùn ci hè micca terminal cunnessu?

### Passu 2: Prepara per a sottomissione

A cartella di u to plugin deve cuntene:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Campi richiesti di `package.json`:**

| Campo | Descrizione | Esempiu |
|-------|-------------|---------|
| `name` | ID unicu in kebab-case | `"my-awesome-plugin"` |
| `version` | Versione semantica | `"1.0.0"` |
| `description` | Descrizzione in una linea | `"Monitors nginx access logs in real-time"` |
| `author` | U to nome | `"John Doe"` |
| `main` | Puntu d'entrata | `"index.js"` |

**Campi opzionali:**

| Campo | Descrizione |
|-------|-------------|
| `license` | Tipu di licenza (MIT cunsigliatu) |
| `keywords` | Array di tag di ricerca |
| `soom.minVersion` | Versione minima di WIA SOOM necessaria |

### Passu 3: Invia à u Registru di Plugins

1. ****Package** your plugin as a ZIP file
2. **Aghjunghje** u vostru plugin à `plugins/{your-plugin-name}/`
3. **Invia** una Pull Request

### Passu 4: Revisione è approvazione

Revisemu ogni plugin per:

- **Sicurezza** — nisuna API periculosa (vede [Reguli di Sicurezza](#security-rules))
- **Qualità** — funziona? U codice hè pulitu?
- **Utilità** — risolve un prublema reale?

Dopu l'approvazione:
1. U vostru plugin hè aghjuntu à `registry.json`
2. Un pacchettu ZIP hè creatu in `dist/`
3. U vostru plugin appare in u **Plugin Store** per tutti l'utilizatori di WIA SOOM!

---

## Parte 5: Megliu Pratiche

### Reguli di Sicurezza

Queste regule sò **obbligatorie**. I plugins chì li violanu seranu rifiutati.

| Regola | Perchè |
|------|-----|
| **MAI** utilizà `eval()` o `new Function()` | Rischiu di iniezione di codice |
| **MAI** utilizà `child_process`, `exec()`, `spawn()` | Utilizà solu `ctx.terminal.send()` per i cumandamenti |
| **MAI** recuperà URL esterni | Eccezione: punti di API di `wiasoom.com` |
| **MAI** accede à `process.env` | I variabili d'ambiente ponu cuntene secreti |
| **MAI** utilizà `require('fs')` direttamente | Utilizà `ctx.settings` per a memoria, `ctx.sftp` per u trasferimentu di file |
| **MAI** utilizà pacchetti esterni npm | Solu JavaScript puru — senza node_modules |
| **DEVENU** utilizà `ctx.terminal.send()` per tutti i cumandamenti remoti | Questu passa per u canale SSH sicuru |
| **DEVENU** pulisce in `deactivate()` | Rimuovere ascoltatori, chjarisce intervalli |

### Gestione di Errori

Incuraghjite sempre à incapsulà operazioni rischiose in try/catch:
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
### Pulizia in deactivate()

Se u vostru plugin crea intervalli, ascoltatori, o abbonamenti — puliteli:
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
### Supportu i18n

WIA SOOM supporta 254 lingue. Per fà chì l'etichetta di u vostru plugin sia traducibile, utilizate un approcciu simplice:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## Parte 6: Esempii di u Mondo Reale

### Esempiu 1: Verificatore di Disco di Servitore

Esegue `df -h` nantu à u servitore remotu è mostra u spaziu utilizatu/dispunibule in a barra di statutu.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Esempiu 2: Gestore di TODO

Un plugin chì gestisce una lista TODO utilizendu impostazioni per a memoria persistente è una webview per a visualizazione.

> **Schema di design:** Datu chì e webviews ùn ponu micca chjamà direttamente l'API di plugin, stu plugin utilizza un approcciu "snapshot" — legge i TODO da e impostazioni, li rende cum'è HTML in sola lettura, è furnisce azzioni basate in sidebar per aghjunghje elementi. A webview hè un stratu di **visualizazione**, micca un modulo interattivu.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### Esempiu 3: Osservatore di Errori

Monitora l'uscita di u terminale è manda una notifica quandu schemi specifici sò rilevati.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Appendice: Categorie & Icone

### Categorie di Plugin (29)

Usate queste in u vostru `package.json` `keywords` o quandu submitte à u registru:

| Categuria | Descrizzione |
|-----------|--------------|
| `server` | Gestione generale di u server |
| `devtools` | Strumenti di sviluppu |
| `calculator` | Calcolatori è convertitori |
| `simulator` | Simulatori |
| `game` | Ghjochi di terminale |
| `business` | Strumenti per l'affari |
| `security` | Sicurezza è audit |
| `web` | Gestione di server web |
| `education` | Strumenti educativi |
| `health` | Strumenti relativi à a salute |
| `islamic` | Strumenti islamichi (tempi di preghiera, etc.) |
| `science` | Strumenti scientifici |
| `quantum` | Strumenti di computazione quantistica |
| `ai` | Strumenti alimentati da AI |
| `biotech` | Strumenti di biotecnologia |
| `space` | Strumenti di spaziu è astronomia |
| `network` | Strumenti di rete |
| `database` | Gestione di basi di dati |
| `monitoring` | Monitoraggio di server |
| `devops` | DevOps è CI/CD |
| `utility` | Utilità generali |
| `design` | Strumenti di design |
| `ecommerce` | Strumenti di e-commerce |
| `automation` | Strumenti di automazione |
| `kpop` | Strumenti relativi à K-pop |
| `accessibility` | Strumenti di accessibilità |
| `analytics` | Analisi è rapporti |
| `wia` | Strumenti di l'ecosistema WIA |
| `all` | Appare in tutte e categorie |

### Icone Raccomandate (Lucide)

| Nome di l'Icona | Usate per |
|------------------|-----------|
| `server` | Gestione di server |
| `shield` | Sicurezza |
| `database` | Basi di dati |
| `activity` | Monitoraggio |
| `terminal` | Strumenti di terminale |
| `code` | Sviluppu |
| `hard-drive` | Disco/stoccaggio |
| `network` | Rete |
| `lock` | Autenticazione/crittografia |
| `eye` | Vigilanza/monitoraggio |
| `check-square` | Compiti/TODO |
| `layout-dashboard` | Dashboard |
| `settings` | Configurazione |
| `zap` | Automazione |
| `globe` | Web/internaziunale |

Sfoglia tutte e 1,500+ icone: [lucide.dev/icons](https://lucide.dev/icons)

---

## Bisogna d'aiutu?

- **Problemi GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemi di Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Esempiu:** [Website](https://wiasoom.com)
- **Sito Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Custrui qualcosa d'eccezionale. Sparte lu cù u mondu.</em></p>
<p align="center"><em>— U Team di WIA SOOM</em></p>
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
