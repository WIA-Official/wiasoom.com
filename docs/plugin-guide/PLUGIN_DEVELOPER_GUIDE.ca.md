<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guia del Desenvolupador de Plugins de WIA SOOM</h1>
<p align="center"><strong>Crea el teu propi plugin en 5 minuts.</strong></p>
<p align="center">Crea potents eines de servidor, taulers de control i automatitzacions — directament dins de WIA SOOM.</p>

---

## Taula de Continguts

- [Part 1: Inici Ràpid — El Teu Primer Plugin en 5 Minuts](#part-1-inici-ràpid--el-teu-primer-plugin-en-5-minuts)
- [Part 2: Referència de l'API del Contexte del Plugin](#part-2-referència-de-lapi-del-contexte-del-plugin)
  - [ctx.terminal](#ctxterminal--executar-comandes-en-serveis-remots)
  - [ctx.sftp](#ctxsftp--transferència-de-fitxers)
  - [ctx.ui](#ctxui--interfície-dusuari)
  - [ctx.settings](#ctxsettings--emmagatzematge-persistent)
  - [ctx.ai](#ctxai--integració-ai)
- [Part 3: Construint UI Personalitzada amb Webviews](#part-3-construint-ui-personalitzada-amb-webviews)
- [Part 4: Publicant el Teu Plugin](#part-4-publicant-el-teu-plugin)
- [Part 5: Millors Pràctiques](#part-5-millors-pràctiques)
- [Part 6: Exemples del Món Real](#part-6-exemples-del-món-real)
- [Annex: Categories i Icons](#annex-categories-i-icons)

---

## Part 1: Inici Ràpid — El Teu Primer Plugin en 5 Minuts

### El que construiràs

Un plugin "Hello World" que afegeix un botó a la barra lateral. Quan es fa clic, mostra una notificació.

### Pas 1: Crea la carpeta del plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Pas 2: Crea package.json
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
**Camp necessaris:** `name`, `version`, `description`, `author`, `main`

### Pas 3: Crea index.js
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
### Pas 4: Reinicia WIA SOOM

Reinicia l'aplicació (o activa/desactiva el plugin a Configuració → Plugins).

Hauries de veure un botó **"Hello World"** a la barra lateral. Fes-hi clic — veuràs una notificació d'èxit!

### Com funciona
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

## Part 2: Referència de l'API del Contexte del Plugin

Quan la teva funció `activate(context)` és cridada, `context` (o `ctx`) proporciona aquestes APIs:
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

### `ctx.terminal` — Executar comandes en serveis remots

#### `terminal.send(sessionId, data)`

Envia una comanda (o qualsevol dada) a una sessió de terminal activa.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `sessionId` | `string` | La sessió de terminal a la qual enviar |
| `data` | `string` | La comanda o dada a enviar |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscriu-te a tota la sortida d'una sessió de terminal. Retorna una **funció de cancel·lar subscripció**.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `sessionId` | `string` | La sessió de terminal a observar |
| `callback` | `(data: string) => void` | Cridat amb cada fragment de sortida |
| **Retorna** | `() => void` | Crida això per aturar l'escolta |
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
**Important:** Sempre desa la funció de cancel·lar subscripció i crida-la a `deactivate()` per evitar fuites de memòria.

---

### `ctx.sftp` — Transferència de fitxers

> **Estat: Properament** — L'API SFTP està definida però encara no connectada al motor SFTP de l'aplicació. `list()` actualment retorna un array buit, i `upload()`/`download()` no fan res. Això es implementarà completament en una futura versió. Per ara, utilitza `ctx.terminal.send()` amb comandes `scp` o `rsync` com a solució alternativa.

#### `sftp.list(sessionId, path)`

Llista fitxers en un directori remot.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Puja un fitxer de la màquina local al servidor remot.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Descarrega un fitxer del servidor remot a la màquina local.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solució alternativa (fins que l'API SFTP estigui activa):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfície d'usuari

#### `ui.addSidebarButton(options)`

Afegeix un botó a la barra lateral de WIA SOOM.

| Opció | Tipus | Requerit | Descripció |
|--------|------|----------|-------------|
| `id` | `string` | No | ID únic (per defecte és el nom del plugin) |
| `icon` | `string` | Sí | Nom de l'icona Lucide (per exemple, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Sí | Text del botó que es mostra a la barra lateral |
| `onClick` | `() => void` | Sí | Funció cridada quan es fa clic al botó |
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
**Referència d'icones:** Navega per totes les icones disponibles a [lucide.dev/icons](https://lucide.dev/icons)

> **Nota de compatibilitat:** Alguns plugins més antics utilitzen arguments posicionals com `addSidebarButton(id, icon, label, onClick)`. L'API oficial utilitza un **objecte d'opcions** com s'ha documentat anteriorment. Sempre utilitza l'estil d'objecte per a nous plugins.

#### `ui.openWebview(options)`

Obre una finestra emergent amb contingut HTML personalitzat. Això és com construeixes UIs riques.

| Opció | Tipus | Descripció |
|--------|------|-------------|
| `title` | `string` | Títol de la finestra |
| `html` | `string` | Contingut HTML complet a renderitzar |
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
> Vegeu [Part 3](#part-3-building-custom-ui-with-webviews) per patrons avançats de webview.

#### `ui.showNotification(type, message)`

Mostra una notificació emergent.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Estil de notificació |
| `message` | `string` | Text a mostrar |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Afegeix un element de text persistent a la barra d'estat inferior.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `id` | `string` | ID únic per a aquest element d'estat |
| `text` | `string` | Text a mostrar |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Emmagatzematge persistent

Les configuracions del plugin s'emmagatzemen de manera permanent a `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Llegeix un valor desat.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Retorna `undefined` si la clau no existeix.

#### `settings.set(key, value)`

Desa un valor. Admet cadenes, nombres, booleans, arrays i objectes.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemple: Recordar preferències d'usuari**
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

### `ctx.ai` — Integració d'IA

> **Estat: Properament** — L'API d'IA està definida però encara no està connectada a Soomy. Actualment retorna `{ response: 'AI not yet connected' }`. La integració completa de l'IA està prevista per a una futura versió.

#### `ai.chat(messages, options?)`

Envia missatges a l'assistent d'IA (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Construint UI Personalitzada amb Webviews

L'API `openWebview()` et permet construir UIs de tauler de control amb HTML, CSS i JavaScript — tot dins d'una finestra emergent.

> **Limitació important:** Les webviews són **només per a visualització**. No poden fer crides a les APIs del plugin (`ctx.settings`, `ctx.terminal`, etc.). Utilitza botons de barra lateral per a totes les accions d'usuari, i utilitza `openWebview()` per mostrar l'estat actual. Si necessites funcions interactives, activa-les des dels botons de la barra lateral i torna a obrir la webview per actualitzar la visualització.

### Patró: Comandament de Terminal → Analitzar Sortida → Mostrar en HTML

Aquest és el patró de plugin més comú. Executes un comandament, analitzes el resultat i el mostres visualment.
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
### Patró: Tauler Interactiu amb Auto-Actualització
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
### Patró: Mostrant Configuracions en una Webview

> **Nota:** Les webviews són només per a visualització — no poden fer crides a les APIs del plugin. Utilitza `ctx.settings` en els gestors de botons de la barra lateral per modificar les configuracions, i utilitza `openWebview()` per mostrar l'estat actual.
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

## Part 4: Publicant el Teu Plugin

### Pas 1: Prova localment

1. Copia el teu plugin a `~/.wia-soom/plugins/{your-plugin}/`
2. Reinicia WIA SOOM
3. Verifica que funcioni: apareix el botó de la barra lateral, les funcions funcionen correctament
4. Prova casos límit: què passa si no hi ha cap terminal connectada?

### Pas 2: Prepara't per a la submissió

La teva carpeta de plugin ha de contenir:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Camp necessaris de `package.json`:**

| Camp | Descripció | Exemple |
|------|------------|---------|
| `name` | ID únic en format kebab-case | `"my-awesome-plugin"` |
| `version` | Versió semàntica | `"1.0.0"` |
| `description` | Descripció en una línia | `"Monitors nginx access logs in real-time"` |
| `author` | El teu nom | `"John Doe"` |
| `main` | Punt d'entrada | `"index.js"` |

**Camp opcionals:**

| Camp | Descripció |
|------|------------|
| `license` | Tipus de llicència (recomanat MIT) |
| `keywords` | Array de tags de cerca |
| `soom.minVersion` | Versió mínima de WIA SOOM requerida |

### Pas 3: Enviar al registre de plugins

1. ****Package** your plugin as a ZIP file
2. **Afegeix** el teu plugin a `plugins/{el-teu-nom-de-plugin}/`
3. **Envia** una Pull Request

### Pas 4: Revisió i aprovació

Revisem cada plugin per:

- **Seguretat** — sense APIs perilloses (vegeu [Regles de seguretat](#security-rules))
- **Qualitat** — funciona? És el codi net?
- **Utilitat** — resol un problema real?

Després de l'aprovació:
1. El teu plugin s'afegeix a `registry.json`
2. Es crea un paquet ZIP a `dist/`
3. El teu plugin apareix a la **Plugin Store** per a tots els usuaris de WIA SOOM!

---

## Part 5: Millors pràctiques

### Regles de seguretat

Aquestes regles són **obligatòries**. Els plugins que les violen seran rebutjats.

| Regla | Per què |
|-------|---------|
| **MAI** utilitzeu `eval()` o `new Function()` | Risc d'injecció de codi |
| **MAI** utilitzeu `child_process`, `exec()`, `spawn()` | Només utilitzeu `ctx.terminal.send()` per a ordres |
| **MAI** obtingueu URLs externes | Excepció: punts d'API de `wiasoom.com` |
| **MAI** accediu a `process.env` | Les variables d'entorn poden contenir secrets |
| **MAI** utilitzeu `require('fs')` directament | Utilitzeu `ctx.settings` per a emmagatzematge, `ctx.sftp` per a transferència de fitxers |
| **MAI** utilitzeu paquets externs de npm | Només JavaScript pur — sense node_modules |
| **HA DE** utilitzar `ctx.terminal.send()` per a totes les ordres remotes | Això passa pel canal SSH segur |
| **HA DE** netejar a `deactivate()` | Eliminar listeners, netejar intervals |

### Maneig d'errors

Sempre emboliqueu operacions arriscades en try/catch:
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
### Neteja a deactivate()

Si el teu plugin crea intervals, listeners o subscripcions — neteja'ls:
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

WIA SOOM suporta 254 idiomes. Per fer que l'etiqueta del teu plugin sigui traduïble, utilitza un enfocament senzill:
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

## Part 6: Exemples del món real

### Exemple 1: Comprovador d'espai en disc del servidor

Executa `df -h` al servidor remot i mostra l'espai utilitzat/disponible a la barra d'estat.
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

### Exemple 2: Gestor de TODO

Un plugin que gestiona una llista de TODO utilitzant configuracions per a emmagatzematge persistent i un webview per a la visualització.

> **Patró de disseny:** Atès que els webviews no poden cridar directament les APIs dels plugins, aquest plugin utilitza un enfocament de "instantània" — llegeix els TODOs de les configuracions, els renderitza com a HTML només de lectura, i proporciona accions basades en la barra lateral per afegir elements. El webview és una capa de **visualització**, no un formulari interactiu.
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

### Exemple 3: Observador d'errors

Monitora la sortida del terminal i envia una notificació quan es detecten patrons específics.
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

## Apèndix: Categories i Icons

### Categories de Plugins (29)

Utilitzeu aquestes en el vostre `package.json` `keywords` o quan envieu a la registre:

| Categoria | Descripció |
|----------|-------------|
| `server` | Gestió general del servidor |
| `devtools` | Eines de desenvolupament |
| `calculator` | Calculadores i convertidors |
| `simulator` | Simuladors |
| `game` | Jocs de terminal |
| `business` | Eines per a negocis |
| `security` | Seguretat i auditoria |
| `web` | Gestió de servidors web |
| `education` | Eines educatives |
| `health` | Eines relacionades amb la salut |
| `islamic` | Eines islàmiques (hores de pregària, etc.) |
| `science` | Eines científiques |
| `quantum` | Eines de computació quàntica |
| `ai` | Eines impulsades per IA |
| `biotech` | Eines de biotecnologia |
| `space` | Eines d'espai i astronomia |
| `network` | Eines de xarxa |
| `database` | Gestió de bases de dades |
| `monitoring` | Monitorització del servidor |
| `devops` | DevOps i CI/CD |
| `utility` | Utilitats generals |
| `design` | Eines de disseny |
| `ecommerce` | Eines de comerç electrònic |
| `automation` | Eines d'automatització |
| `kpop` | Eines relacionades amb K-pop |
| `accessibility` | Eines d'accessibilitat |
| `analytics` | Anàlisis i informes |
| `wia` | Eines de l'ecosistema WIA |
| `all` | Apareix en totes les categories |

### Icons Recomanats (Lucide)

| Nom de l'Icona | Ús per a |
|-----------|---------|
| `server` | Gestió del servidor |
| `shield` | Seguretat |
| `database` | Base de dades |
| `activity` | Monitorització |
| `terminal` | Eines de terminal |
| `code` | Desenvolupament |
| `hard-drive` | Disc/emmagatzematge |
| `network` | Xarxes |
| `lock` | Autenticació/xifrat |
| `eye` | Vigilància/monitorització |
| `check-square` | Tasques/TODO |
| `layout-dashboard` | Taulers de control |
| `settings` | Configuració |
| `zap` | Automatització |
| `globe` | Web/internacional |

Explora més de 1.500 icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Necessites Ajuda?

- **Problemes de GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemes de Plugins:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugins d'Exemple:** [Website](https://wiasoom.com)
- **Lloc Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construeix alguna cosa increïble. Comparteix-ho amb el món.</em></p>
<p align="center"><em>— L'equip de WIA SOOM</em></p>