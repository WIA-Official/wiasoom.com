<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guia de Desenvolopament de Plugins WIA SOOM</h1>
<p align="center"><strong>Construsís ton pròpri plugin en 5 minutas.</strong></p>
<p align="center">Crea d'aisinas poderosas per servidors, tablèus de bord, e automatizacions — directament dins WIA SOOM.</p>

---

## Taula de Contenuts

- [Part 1: Començar Rapidament — Ton Primièr Plugin en 5 Minutas](#part-1-començar-rapidament--ton-primièr-plugin-en-5-minutas)
- [Part 2: Referéncia de l'API de Contexte de Plugin](#part-2-referéncia-de-lapi-de-contexte-de-plugin)
  - [ctx.terminal](#ctxterminal--executar-comandas-sus-serveurs-remots)
  - [ctx.sftp](#ctxsftp--transferéncia-de-fichièrs)
  - [ctx.ui](#ctxui--interfàcia-utilizator)
  - [ctx.settings](#ctxsettings--estocatge-permanent)
  - [ctx.ai](#ctxai--integracion-ai)
- [Part 3: Construire una UI Personalizada amb Webviews](#part-3-construire-una-ui-personalizada-amb-webviews)
- [Part 4: Publicar Ton Plugin](#part-4-publicar-ton-plugin)
- [Part 5: Melhores Pràcticas](#part-5-melhores-pràcticas)
- [Part 6: Exemples del Mond Real](#part-6-exemples-del-mond-real)
- [Apèndix: Categories & Icons](#apèndix-categories--icons)

---

## Part 1: Començar Rapidament — Ton Primièr Plugin en 5 Minutas

### Cossí vas construir

Un plugin "Hello World" que ajunta un boton al panèl lateral. Quand es clicat, mòstra una notificació.

### Etapa 1: Crear lo dossier del plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Etapa 2: Crear package.json
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
**Camp requisits:** `name`, `version`, `description`, `author`, `main`

### Etapa 3: Crear index.js
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
### Etapa 4: Reïniciar WIA SOOM

Reïnicia l'aplicacion (o commuta lo plugin en desactivat/activat dins Paramètres → Plugins).

Deves veire un **"Hello World"** boton dins lo panèl lateral. Clica dessus — veiràs una notificació de succès!

### Cossí aquò fonciona
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

## Part 2: Referéncia de l'API de Contexte de Plugin

Quand ton `activate(context)` foncion es apelada, `context` (o `ctx`) fornís aquestes APIs:
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

### `ctx.terminal` — Executar comandos sus serveurs remots

#### `terminal.send(sessionId, data)`

Envia un comando (o qualsevol donada) a una session de terminal activa.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `sessionId` | `string` | La session de terminal a la qual enviar |
| `data` | `string` | Lo comando o donada a enviar |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

S'abòna a tot l'output d'una session de terminal. Retorna una **foncion de desabònament**.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `sessionId` | `string` | La session de terminal a observar |
| `callback` | `(data: string) => void` | Es apelada amb cada fragment d'output |
| **Retorna** | `() => void` | Apela aquò per cesser d'escotar |
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
**Important:** Sempre sauvega la foncion de desabònament e apela-la dins `deactivate()` per evitar de fuites de memòria.

---

### `ctx.sftp` — Transferéncia de fichièrs

> **Estat: Arribant Logo** — L'API SFTP es definida mas pas encara conectada a l'engin SFTP de l'aplicacion. `list()` retorna actualament un array buid, e `upload()`/`download()` son inoperants. Aquò serà completament implementat dins una futura version. Per ara, utiliza `ctx.terminal.send()` amb los comandos `scp` o `rsync` coma una solucion alternativa.

#### `sftp.list(sessionId, path)`

Lista los fichièrs dins un directòri remot.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Telecarrega un fichièr de la maquina local al servidor remot.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Telecarrega un fichièr del servidor remot a la maquina local.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solucion alternativa (fins que l'API SFTP siá activa):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfàcia utilizator

#### `ui.addSidebarButton(options)`

Ajunta un boton al panèl lateral de WIA SOOM.

| Opció | Tipus | Requisit | Descripció |
|--------|------|----------|-------------|
| `id` | `string` | Non | ID unic (per defaut es lo nom del plugin) |
| `icon` | `string` | Òc | Nom de l'icòna Lucide (per exemple, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Òc | Text del boton mostrada dins lo panèl lateral |
| `onClick` | `() => void` | Òc | Foncion apelada quand lo boton es clicat |
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
**Referéncia d'icòna:** Clica per veire totes las icònas disponibles a [lucide.dev/icons](https://lucide.dev/icons)

> **Nota de compatibilitat:** Qualques plugins mai vièlhs utilizan d'arguments posicionals coma `addSidebarButton(id, icon, label, onClick)`. L'API oficial utiliza un **objècte d'opcions** coma documentat aquí. Utiliza totjorn l'estil d'objècte per de nòus plugins.

#### `ui.openWebview(options)`

Ouvrir una fenèstra pop-up amb contengut HTML personalizat. Aquò es cossí construïr d'UIs ricas.

| Opció | Tipus | Descripció |
|--------|------|-------------|
| `title` | `string` | Títol de la fenèstra |
| `html` | `string` | Contengut HTML complet a renderizar |
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
> Veire [Part 3](#part-3-building-custom-ui-with-webviews) per de patterns avançats de webview.

#### `ui.showNotification(type, message)`

Montre una notificació toast.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Estil de notificació |
| `message` | `string` | Téxte a mostrar |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ajoute un element de tèxte persistent a la barra d'estat inferior.

| Paràmetre | Tipus | Descripció |
|-----------|------|-------------|
| `id` | `string` | ID unic per aquest element d'estat |
| `text` | `string` | Téxte a mostrar |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Emmagatzematge persistent

Los paramètres del plugin son emmagatzemats permanentament dins `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Llegir un valor desat.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Retorna `undefined` se la clau non existe pas.

#### `settings.set(key, value)`

Desa un valor. Suporta cadenas, nombres, booleans, taules, e objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemple: Retenir les preferéncias de l'usuari**
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

### `ctx.ai` — Integració AI

> **Estat: Arribant Logo** — L'API AI es definida mas encara non es connectada a Soomy. Actualament retorna `{ response: 'AI not yet connected' }`. Una integració completa de l'AI es prevista per una futura version.

#### `ai.chat(messages, options?)`

Envia missatges a l'assistent AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Construir una UI Personalizada amb Webviews

L'API `openWebview()` permet de construir UIs de tauler amb HTML, CSS, e JavaScript — tot dins una finestra emergent.

> **Limitacion important:** Los webviews son **somente de visualizacion**. Pòdon pas tornar a apelar als APIs del plugin (`ctx.settings`, `ctx.terminal`, etc.). Utiliza los botons de la barra lateral per totes las accions de l'usuari, e utiliza `openWebview()` per mostrar l'estat actual. Se necessitas caracteristicas interactives, desencadena-les dels botons de la barra lateral e reobri lo webview per refrescar la visualizacion.

### Patrón: Comanda de Terminal → Analitzar Sortida → Mostrar en HTML

Aquest es lo patró de plugin lo mai comun. Executes una comanda, analises lo resultat, e o mostras visualament.
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
### Patrón: Tauler Interactiu amb Auto-Refresca
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
### Patrón: Mostrar Configuracions dins un Webview

> **Nota:** Los webviews son soment de visualizacion — pòdon pas tornar a apelar als APIs del plugin. Utiliza `ctx.settings` dins los gestors de botons de la barra lateral per modificar las configuracions, e utiliza `openWebview()` per mostrar l'estat actual.
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

## Part 4: Publicar Ton Plugin

### Pas 1: Testar localament

1. Copia ton plugin a `~/.wia-soom/plugins/{your-plugin}/`
2. Reinicia WIA SOOM
3. Verifica que funciona: lo boton de la barra lateral apareis, las caracteristicas foncionan correctament
4. Testa los casos limites: que se passa se cap terminal es connectat?

### Pas 2: Preparar per la submissió

Ton dossier de plugin deu conténer:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Camp requisits de `package.json`:**

| Camp | Descripció | Exemple |
|------|------------|---------|
| `name` | ID unic en kebab-case | `"mon-plugin-genial"` |
| `version` | Version semantica | `"1.0.0"` |
| `description` | Descripció en una linha | `"Monitora los logs d'accès nginx en temps real"` |
| `author` | Ton nom | `"John Doe"` |
| `main` | Punt d'entrada | `"index.js"` |

**Camp opcionals:**

| Camp | Descripció |
|------|------------|
| `license` | Tipus de llicència (MIT recomanat) |
| `keywords` | Taula de mots-clau de cerca |
| `soom.minVersion` | Version mínima de WIA SOOM requerida |

### Pas 3: Enviar al registre de plugins

1. ****Package** your plugin as a ZIP file
2. **Ajusta** ton plugin a `plugins/{ton-nom-de-plugin}/`
3. **Envia** una Pull Request

### Pas 4: Revisió i aprovació

Revisem cada plugin per:

- **Seguretat** — cap API perillosa (veire [Regles de Seguretat](#security-rules))
- **Qualitat** — funciona? Es el codi net?
- **Utilitat** — resol un problema real?

Après l'aprovació:
1. Ton plugin es afegit a `registry.json`
2. Un paquet ZIP es creat dins `dist/`
3. Ton plugin apareix dins la **Plugin Store** per tots los usuaris de WIA SOOM!

---

## Part 5: Melhores Pràctiques

### Regles de Seguretat

Aquestes regles son **mandatòrias**. Los plugins que las violan seràn rebutjats.

| Regla | Perqué |
|-------|--------|
| **JA** usar `eval()` o `new Function()` | Risc d'injecció de codi |
| **JA** usar `child_process`, `exec()`, `spawn()` | Utiliza solament `ctx.terminal.send()` per ordres |
| **JA** buscar URLs externes | Excepció: punts d'API de `wiasoom.com` |
| **JA** accedir a `process.env` | Las variablas d'environament pòdon contenir secrets |
| **JA** usar `require('fs')` directament | Utiliza `ctx.settings` per l'emmagatzematge, `ctx.sftp` per la transferéncia de fichièrs |
| **JA** usar paquets externs npm | Somament JavaScript pur — pas de node_modules |
| **DEVE** usar `ctx.terminal.send()` per totes les ordres remotes | Aquò passa pel canal SSH segur |
| **DEVE** netejar dins `deactivate()` | Retira listeners, neteja intervals |

### Gestió d'Errors

En tot moment, envolopa las operacions arriscadas dins un try/catch:
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
### Neteja dins deactivate()

Se ton plugin crea intervals, listeners, o subscripcions — neteja'ls:
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

WIA SOOM suporta 254 lengas. Per far que l'etiqueta de ton plugin siga traduïble, utiliza un aprop simplificat:
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

## Part 6: Exemples del Món Real

### Exemple 1: Verificador de Disc del Servidor

Executa `df -h` sul servidor remot e mostra l'espai utilitzat/disponible dins la barra d'estat.
§§§CHUNK_SEPARATOR§§§
---

### Exemple 2: Gestor de TODO

Un plugin que gestiona una lista de TODO utilitzant configuracions per l'emmagatzematge persistent e un webview per l'afichatge.

> **Patrò de dissenh:** Com que los webviews pòdon pas cridar directament las APIs dels plugins, aquest plugin utiliza un aprop de "snapshot" — legís los TODOs de las configuracions, los renderiza en HTML de lectura sola, e proporciona accions basadas dins la barra lateral per afegir elements. Lo webview es una capa de **visualització**, pas un formulari interactiu.
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

### Exemple 3: Observador d'Errors

Monitora la sortida del terminal e envia una notificació quand de patrons especifics son detectats.
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

## Apèndix: Categories & Icons

### Categories de Plugins (29)

Utilizatz aqueles dins vòstra `package.json` `keywords` o quand s'inscrivètz al registre:

| Categoria | Descripcion |
|-----------|-------------|
| `server` | Gestion generala del servidor |
| `devtools` | Instruments de desvolopament |
| `calculator` | Calculators e convertidors |
| `simulator` | Simuladors |
| `game` | Jòcs de terminal |
| `business` | Instruments d'òbra |
| `security` | Seguretat e auditori |
| `web` | Gestion del servidor web |
| `education` | Instruments educatius |
| `health` | Instruments en relació amb la santat |
| `islamic` | Instruments islamics (temps de pregària, etc.) |
| `science` | Instruments scientifics |
| `quantum` | Instruments de computacion quanta |
| `ai` | Instruments amb IA |
| `biotech` | Instruments de biotecnologia |
| `space` | Instruments de l'espaci e d'astronomia |
| `network` | Instruments de retea |
| `database` | Gestion de basa de donadas |
| `monitoring` | Monitoratge del servidor |
| `devops` | DevOps e CI/CD |
| `utility` | Utilitats generala |
| `design` | Instruments de design |
| `ecommerce` | Instruments d'e-comerç |
| `automation` | Instruments d'automatisation |
| `kpop` | Instruments en relacion amb K-pop |
| `accessibility` | Instruments d'accessibilitat |
| `analytics` | Analisi e rapòrts |
| `wia` | Instruments de l'ecosistema WIA |
| `all` | Apareis dins totes las categorias |

### Icons Recommandats (Lucide)

| Nom de l'Icona | Utilizar per |
|----------------|--------------|
| `server` | Gestion del servidor |
| `shield` | Seguretat |
| `database` | Basa de donadas |
| `activity` | Monitoratge |
| `terminal` | Instruments de terminal |
| `code` | Desvolopament |
| `hard-drive` | Disc/stockatge |
| `network` | Retea |
| `lock` | Auth/encriptatge |
| `eye` | Observacion/monitoratge |
| `check-square` | Taches/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuracion |
| `zap` | Automatisation |
| `globe` | Web/internacional |

Exploratz totes los 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Besonh d'Ajudar?

- **Problemas GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemas de Plugins:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugins d'Exemple:** [Website](https://wiasoom.com)
- **Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construsissètz quicòm d'exceptional. Partagatz amb lo mond.</em></p>
<p align="center"><em>— Lo Team WIA SOOM</em></p>
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
