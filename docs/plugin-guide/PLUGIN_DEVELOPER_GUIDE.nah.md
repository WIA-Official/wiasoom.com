<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Construye tu propio plugin en 5 minutos.</strong></p>
<p align="center">Crea herramientas poderosas para servidores, paneles de control y automatizaciones — directamente dentro de WIA SOOM.</p>

---

## Tabla de Contenidos

- [Parte 1: Inicio Rápido — Tu Primer Plugin en 5 Minutos](#parte-1-inicio-rápido--tu-primer-plugin-en-5-minutos)
- [Parte 2: Referencia de la API del Contexto del Plugin](#parte-2-referencia-de-la-api-del-contexto-del-plugin)
  - [ctx.terminal](#ctxterminal--ejecutar-comandos-en-servidores-remotos)
  - [ctx.sftp](#ctxsftp--transferencia-de-archivos)
  - [ctx.ui](#ctxui--interfaz-de-usuario)
  - [ctx.settings](#ctxsettings--almacenamiento-persistente)
  - [ctx.ai](#ctxai--integración-ai)
- [Parte 3: Construyendo UI Personalizada con Webviews](#parte-3-construyendo-ui-personalizada-con-webviews)
- [Parte 4: Publicando Tu Plugin](#parte-4-publicando-tu-plugin)
- [Parte 5: Mejores Prácticas](#parte-5-mejores-prácticas)
- [Parte 6: Ejemplos del Mundo Real](#parte-6-ejemplos-del-mundo-real)
- [Apéndice: Categorías e Íconos](#apéndice-categorías-e-íconos)

---

## Parte 1: Inicio Rápido — Tu Primer Plugin en 5 Minutos

### Lo que construirás

Un plugin "Hola Mundo" que añade un botón a la barra lateral. Al hacer clic, muestra una notificación.

### Paso 1: Crea la carpeta del plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Paso 2: Crea package.json
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
**Campos requeridos:** `name`, `version`, `description`, `author`, `main`

### Paso 3: Crea index.js
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
### Paso 4: Reinicia WIA SOOM

Reinicia la aplicación (o activa/desactiva el plugin en Configuración → Plugins).

Deberías ver un botón **"Hola Mundo"** en la barra lateral. ¡Haz clic en él y verás una notificación de éxito!

### Cómo funciona
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

## Parte 2: Referencia de la API del Contexto del Plugin

Cuando se llama a tu función `activate(context)`, `context` (o `ctx`) proporciona estas APIs:
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

### `ctx.terminal` — Ejecutar comandos en servidores remotos

#### `terminal.send(sessionId, data)`

Envía un comando (o cualquier dato) a una sesión de terminal activa.

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `sessionId` | `string` | La sesión de terminal a la que enviar |
| `data` | `string` | El comando o dato a enviar |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Suscríbete a toda la salida de una sesión de terminal. Devuelve una **función de cancelación**.

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `sessionId` | `string` | La sesión de terminal a observar |
| `callback` | `(data: string) => void` | Se llama con cada fragmento de salida |
| **Devuelve** | `() => void` | Llama a esto para dejar de escuchar |
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
**Importante:** Siempre guarda la función de cancelación y llámala en `deactivate()` para prevenir fugas de memoria.

---

### `ctx.sftp` — Transferencia de archivos

> **Estado: Próximamente** — La API de SFTP está definida pero aún no está conectada al motor SFTP de la aplicación. `list()` actualmente devuelve un array vacío, y `upload()`/`download()` no hacen nada. Esto se implementará completamente en una futura versión. Por ahora, usa `ctx.terminal.send()` con comandos `scp` o `rsync` como solución temporal.

#### `sftp.list(sessionId, path)`

Lista archivos en un directorio remoto.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Sube un archivo desde la máquina local al servidor remoto.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Descarga un archivo desde el servidor remoto a la máquina local.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solución temporal (hasta que la API de SFTP esté activa):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfaz de usuario

#### `ui.addSidebarButton(options)`

Añade un botón a la barra lateral de WIA SOOM.

| Opción | Tipo | Requerido | Descripción |
|--------|------|----------|-------------|
| `id` | `string` | No | ID único (por defecto el nombre del plugin) |
| `icon` | `string` | Sí | Nombre del ícono Lucide (por ejemplo, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Sí | Texto del botón mostrado en la barra lateral |
| `onClick` | `() => void` | Sí | Función llamada cuando se hace clic en el botón |
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
**Referencia de íconos:** Navega por todos los íconos disponibles en [lucide.dev/icons](https://lucide.dev/icons)

> **Nota de compatibilidad:** Algunos plugins más antiguos utilizan argumentos posicionales como `addSidebarButton(id, icon, label, onClick)`. La API oficial utiliza un **objeto de opciones** como se documenta arriba. Siempre usa el estilo de objeto para nuevos plugins.

#### `ui.openWebview(options)`

Abre una ventana emergente con contenido HTML personalizado. Así es como construyes UIs ricas.

| Opción | Tipo | Descripción |
|--------|------|-------------|
| `title` | `string` | Título de la ventana |
| `html` | `string` | Contenido HTML completo a renderizar |
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
> Xochitl [Part 3](#part-3-building-custom-ui-with-webviews) tlatlacazca webview patterns.

#### `ui.showNotification(type, message)`

Tlāzohcamati toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Text tlatlacazca |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Tlāzohcamati text item tlatlacazca tlamantli status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique ID tlatlacazca status item |
| `text` | `string` | Text tlatlacazca |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Tlāzohcamati storage

Plugin settings tlatlacazca tlatlacazca `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Tlāzohcamati saved value.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Tlāzohcamati `undefined` inin key no exist.

#### `settings.set(key, value)`

Tlāzohcamati value. Tlāzohcamati strings, numbers, booleans, arrays, and objects.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Tlāzohcamati: Remember user preferences**
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

> **Status: Coming Soon** — Inin AI API tlatlacazca tlatlacazca, achi no tlatlacazca Soomy. Tlāzohcamati `{ response: 'AI not yet connected' }`. Tlāzohcamati AI integration tlatlacazca tlatlacazca inin tlatlacazca.

#### `ai.chat(messages, options?)`

Tlāzohcamati messages tlatlacazca AI assistant (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Tlāzohcamati Custom UI tlatlacazca Webviews

Inin `openWebview()` API tlatlacazca tlatlacazca dashboard UIs tlatlacazca HTML, CSS, and JavaScript — inin tlatlacazca popup window.

> **Important limitation:** Webviews tlatlacazca **display-only**. Achi no tlatlacazca plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Tlāzohcamati sidebar buttons tlatlacazca tlatlacazca user actions, achi tlatlacazca `openWebview()` tlatlacazca tlatlacazca current state. Inin tlatlacazca interactive features, tlatlacazca inin sidebar buttons achi re-open tlatlacazca webview tlatlacazca refresh tlatlacazca display.

### Pattern: Terminal Command → Parse Output → Tlāzohcamati in HTML

Inin tlatlacazca most common plugin pattern. Tlāzohcamati command, parse tlatlacazca result, achi tlatlacazca tlatlacazca visually.
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
### Pattern: Interactive Dashboard tlatlacazca Auto-Refresh
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
### Pattern: Tlāzohcamati Settings in a Webview

> **Note:** Webviews tlatlacazca display-only — achi no tlatlacazca plugin APIs. Tlāzohcamati `ctx.settings` tlatlacazca inin sidebar button handlers tlatlacazca tlatlacazca settings, achi tlatlacazca `openWebview()` tlatlacazca tlatlacazca current state.
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

## Part 4: Tlāzohcamati Your Plugin

### Step 1: Test locally

1. Tlāzohcamati your plugin tlatlacazca `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Tlāzohcamati it works: sidebar button tlatlacazca, features tlatlacazca correctly
4. Tlāzohcamati edge cases: inin tlatlacazca no terminal tlatlacazca?

### Step 2: Tlāzohcamati for submission

Your plugin folder tlatlacazca tlatlacazca:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Tlahtolli `package.json` tlatlacaz:**

| Tlatlacaz | Tlahtolli | Tlāltikpak |
|-----------|-----------|------------|
| `name` | Tlāltikpak ID tlamikiliztli | `"my-awesome-plugin"` |
| `version` | Semantic tlatlacaz | `"1.0.0"` |
| `description` | Tlāltikpak tlatlacaz | `"Monitors nginx access logs in real-time"` |
| `author` | Tīcātlātlāz | `"John Doe"` |
| `main` | Tlāltikpak tlatlacaz | `"index.js"` |

**Tlāltikpak tlatlacaz:**

| Tlatlacaz | Tlahtolli |
|-----------|-----------|
| `license` | Tlāltikpak tlatlacaz (MIT tlamikiliztli) |
| `keywords` | Tlāltikpak tlatlacaz tlamikiliztli |
| `soom.minVersion` | WIA SOOM tlatlacaz tlamikiliztli tlatlacaz |

### Tlāltikpak 3: Tlāltikpak tlatlacaz tlamikiliztli

1. ****Package** your plugin as a ZIP file
2. **Add** tōnēn plugin tlatlacaz `plugins/{your-plugin-name}/`
3. **Submit** Pull Request

### Tlāltikpak 4: Tlāltikpak tlatlacaz tlamikiliztli

Tlāltikpak tlatlacaz tlatlacaz:

- **Tlāltikpak** — axcātlāz APIs (tlahtlāz [Tlāltikpak Tlāltikpak](#security-rules))
- **Tl��ltikpak** — xochitl? Tlāltikpak tlatlacaz?
- **Tlāltikpak** — xochitl tlatlacaz?

Tlāltikpak tlatlacaz:
1. Tōnēn plugin tlatlacaz tlatlacaz `registry.json`
2. ZIP tlatlacaz tlatlacaz `dist/`
3. Tōnēn plugin tlatlacaz tlatlacaz **Plugin Store** tlatlacaz WIA SOOM tlatlacaz!

---

## Tlāltikpak 5: Tlāltikpak Tlāltikpak

### Tlāltikpak Tlāltikpak

Tlāltikpak tlatlacaz **tlamikiliztli**. Tlāltikpak tlatlacaz tlatlacaz tlatlacaz.

| Tlāltikpak | Tlāltikpak |
|-------------|-------------|
| **AXCĀTLĀZ** `eval()` o `new Function()` | Tlāltikpak tlatlacaz |
| **AXCĀTLĀZ** `child_process`, `exec()`, `spawn()` | Tlāltikpak `ctx.terminal.send()` tlatlacaz |
| **AXCĀTLĀZ** tlatlacaz URLs | Tlāltikpak: `wiasoom.com` API tlatlacaz |
| **AXCĀTLĀZ** `process.env` | Tlāltikpak tlatlacaz tlatlacaz |
| **AXCĀTLĀZ** `require('fs')` tlatlacaz | Tlāltikpak `ctx.settings` tlatlacaz, `ctx.sftp` tlatlacaz |
| **AXCĀTLĀZ** npm tlatlacaz tlatlacaz | Tlāltikpak JavaScript tlatlacaz — axcātlāz node_modules |
| **TLAHTLACAZ** `ctx.terminal.send()` tlatlacaz tlatlacaz tlatlacaz | Tlāltikpak tlatlacaz tlatlacaz tlatlacaz |
| **TLAHTLACAZ** tlatlacaz `deactivate()` | Tlāltikpak tlatlacaz, tlatlacaz tlatlacaz |

### Tlāltikpak Tlāltikpak

Tlāltikpak tlatlacaz tlatlacaz tlatlacaz tlatlacaz:
§§§CHUNK_SEPARATOR§§§
### Tlāltikpak tlatlacaz tlatlacaz

Tlāltikpak tōnēn plugin tlatlacaz tlatlacaz, tlatlacaz, o tlatlacaz — tlatlacaz:
§§§CHUNK_SEPARATOR§§§
### i18n Tlāltikpak

WIA SOOM tlatlacaz 254 tlatlacaz. Tlāltikpak tōnēn plugin tlatlacaz tlatlacaz, tlatlacaz tlatlacaz:
§§§CHUNK_SEPARATOR§§§
---

## Tlāltikpak 6: Tlāltikpak Tlāltikpak

### Tlāltikpak 1: Tlāltikpak Disk Checker

Tlāltikpak `df -h` tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz.
§§§CHUNK_SEPARATOR§§§
---

### Tlāltikpak 2: TODO Tlāltikpak

Tōnēn plugin tlatlacaz tlatlacaz TODO tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz.

> **Tlāltikpak tlatlacaz:** Tlāltikpak webviews axcātlāz tlatlacaz tlatlacaz plugin APIs, tōnēn plugin tlatlacaz "snapshot" tlatlacaz — tlatlacaz TODOs tlatlacaz tlatlacaz, tlatlacaz tlatlacaz tlatlacaz HTML, tlatlacaz tlatlacaz tlatlacaz tlatlacaz. Tlāltikpak webview tlatlacaz **tlatlacaz** tlatlacaz, axcātlāz tlatlacaz tlatlacaz.
§§§CHUNK_SEPARATOR§§§
---

### Tlāltikpak 3: Tlāltikpak Tlāltikpak

Tlāltikpak terminal tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz tlatlacaz.
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

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | Tlāltikpak server tlamantli |
| `devtools` | Tlāltikpak tlamantli tēcuilo |
| `calculator` | Tlāltikpak tlamantli tlamikilistli |
| `simulator` | Tlāltikpak tlamantli tlamikilistli |
| `game` | Tlāltikpak tlamantli tlāltikpak |
| `business` | Tlāltikpak tlamantli tlamantli |
| `security` | Tlāltikpak tlamantli tlamantli y auditación |
| `web` | Tlāltikpak server tlamantli |
| `education` | Tlāltikpak tlamantli tlamikilistli |
| `health` | Tlāltikpak tlamantli tlamikilistli tlamantli |
| `islamic` | Tlāltikpak tlamantli islamika (tlāltikpak tlamantli, etc.) |
| `science` | Tlāltikpak tlamantli tlamikilistli |
| `quantum` | Tlāltikpak tlamantli quantum computing |
| `ai` | Tlāltikpak tlamantli AI-powered |
| `biotech` | Tlāltikpak tlamantli biotechnology |
| `space` | Tlāltikpak tlamantli tlamikilistli y astronomy |
| `network` | Tlāltikpak tlamantli tlamantli |
| `database` | Tlāltikpak tlamantli database |
| `monitoring` | Tlāltikpak tlamantli server |
| `devops` | Tlāltikpak tlamantli DevOps y CI/CD |
| `utility` | Tlāltikpak tlamantli tlamantli |
| `design` | Tlāltikpak tlamantli tlamikilistli |
| `ecommerce` | Tlāltikpak tlamantli e-commerce |
| `automation` | Tlāltikpak tlamantli tlamantli |
| `kpop` | Tlāltikpak tlamantli K-pop |
| `accessibility` | Tlāltikpak tlamantli accessibility |
| `analytics` | Tlāltikpak tlamantli analytics y reporting |
| `wia` | Tlāltikpak tlamantli WIA ecosystem |
| `all` | Tlāltikpak tlamantli in all categories |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Tlāltikpak server tlamantli |
| `shield` | Tlāltikpak seguridad |
| `database` | Tlāltikpak database |
| `activity` | Tlāltikpak monitoring |
| `terminal` | Tlāltikpak tlamantli |
| `code` | Tlāltikpak desarrollo |
| `hard-drive` | Tlāltikpak disk/storage |
| `network` | Tlāltikpak networking |
| `lock` | Tlāltikpak auth/encryption |
| `eye` | Tlāltikpak watching/monitoring |
| `check-square` | Tlāltikpak tasks/TODO |
| `layout-dashboard` | Tlāltikpak dashboards |
| `settings` | Tlāltikpak configuración |
| `zap` | Tlāltikpak automation |
| `globe` | Tlāltikpak web/international |

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
