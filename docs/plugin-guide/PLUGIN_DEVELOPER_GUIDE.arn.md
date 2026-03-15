<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Ranguey plugin tañi 5 minutu.</strong></p>
<p align="center">Küme tooles tañi server, dashboards, ka automations — ñi WIA SOOM mew.</p>

---

## Table of Contents

- [Part 1: Quick Start — Your First Plugin in 5 Minutes](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Building Custom UI with Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Publishing Your Plugin](#part-4-publishing-your-plugin)
- [Part 5: Best Practices](#part-5-best-practices)
- [Part 6: Real-World Examples](#part-6-real-world-examples)
- [Appendix: Categories & Icons](#appendix-categories--icons)

---

## Part 1: Quick Start — Your First Plugin in 5 Minutes

### What you'll build

Ñi plugin "Hello World" ka addiñ button ñi sidebar. Ñi clickiñ, ñi notification puwün.

### Step 1: Create the plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Step 2: Create package.json
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
**Required fields:** `name`, `version`, `description`, `author`, `main`

### Step 3: Create index.js
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
### Step 4: Restart WIA SOOM

Restartiñ ñi app (o toggleiñ ñi plugin off/on ñi Settings → Plugins).

Iñgün ñi **"Hello World"** button ñi sidebar. Clickiñ — iñgün ñi success notification!

### How it works
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

## Part 2: Plugin Context API Reference

Iñgün ñi `activate(context)` function puwün, `context` (o `ctx`) ñi APIs chachay:
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

### `ctx.terminal` — Run commands on remote servers

#### `terminal.send(sessionId, data)`

Sendiñ ñi command (o ñi data) ñi terminal session puwün.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Ñi terminal session puwün sendiñ |
| `data` | `string` | Ñi command o data puwün sendiñ |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subscribeiñ ��i output puwün ñi terminal session. Returnsiñ ñi **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Ñi terminal session puwün watchiñ |
| `callback` | `(data: string) => void` | Calliñiñ ka ñi chunk puwün output |
| **Returns** | `() => void` | Calliñiñ ñi stopiñ listeniñ |
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
**Important:** Siempre saveiñ ñi unsubscribe function ka calliñiñ ñi `deactivate()` ñi preventiñ memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — Ñi SFTP API chachay, pero ñi app's SFTP engine mew ñi wiring nuke. `list()` iñgün returnsiñ ñi empty array, ka `upload()`/`download()` ñi no-ops. Ñi full implementation puwün ñi future release. Ñi puwün, useiñ `ctx.terminal.send()` ka `scp` o `rsync` commands ka workaround.

#### `sftp.list(sessionId, path)`

Listiñ ñi files ñi remote directory.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Uploadiñ ñi file ñi local machine puwün ñi remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Downloadiñ ñi file ñi remote server puwün ñi local machine.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (until SFTP API is live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Addiñ ñi button ñi WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (defaults to plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Button text puwün ñi sidebar |
| `onClick` | `() => void` | Yes | Function puwün calliñiñ ñi button puwün clicked |
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
**Icon reference:** Browseiñ ñi all available icons ñi [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Ñi some older plugins useiñ positional arguments ñi `addSidebarButton(id, icon, label, onClick)`. Ñi official API usesiñ ñi **options object** ka chachay ñi above. Siempre useiñ ñi object style puwün new plugins.

#### `ui.openWebview(options)`

Openiñ ñi popup window ka custom HTML content. Ñi puwün buildiñ ñi rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content puwün renderiñ |
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
> Rangi [Part 3](#part-3-building-custom-ui-with-webviews) rumeiñma webview patterns.

#### `ui.showNotification(type, message)`

Püleñma toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Text to show |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Püleñma text item persistente peñi status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID unik peñi status item |
| `text` | `string` | Text to display |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Almacenamiento persistente

Plugin settings rumeiñma püleñma permanentemente en `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Rumeiñma un valor guardado.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Rumeiñma `undefined` ma key ñiñma.

#### `settings.set(key, value)`

Püleñma un valor. Soporta strings, números, booleans, arrays, y objetos.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Ejemplo: Recordar preferencias de usuario**
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

### `ctx.ai` — Integración de IA

> **Estado: Viene Pronto** — La API de IA está definida pero no conectada a Soomy. Actualmente rumeiñma `{ response: 'AI not yet connected' }`. Integración completa de IA está planificada para una futura versión.

#### `ai.chat(messages, options?)`

Envía mensajes al asistente de IA (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Construyendo UI Personalizado con Webviews

La API `openWebview()` rumeiñma püleñma construir UIs de dashboard con HTML, CSS, y JavaScript — todo dentro de una ventana emergente.

> **Limitación importante:** Webviews son **solo de visualización**. No pueden llamar de vuelta a las APIs de plugin (`ctx.settings`, `ctx.terminal`, etc.). Usa botones de barra lateral para todas las acciones de usuario, y usa `openWebview()` para mostrar el estado actual. Si necesitas características interactivas, actívalas desde los botones de la barra lateral y vuelve a abrir el webview para refrescar la visualización.

### Patrón: Comando de Terminal → Analizar Salida → Mostrar en HTML

Este es el patrón de plugin más común. Ejecutas un comando, analizas el resultado y lo muestras visualmente.
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
### Patrón: Dashboard Interactivo con Auto-Refresco
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
### Patrón: Mostrando Configuraciones en un Webview

> **Nota:** Webviews son solo de visualización — no pueden llamar de vuelta a las APIs de plugin. Usa `ctx.settings` en tus manejadores de botones de barra lateral para modificar configuraciones, y usa `openWebview()` para mostrar el estado actual.
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

## Part 4: Publicando Tu Plugin

### Paso 1: Probar localmente

1. Copia tu plugin a `~/.wia-soom/plugins/{your-plugin}/`
2. Reinicia WIA SOOM
3. Verifica que funcione: el botón de la barra lateral aparece, las características funcionan correctamente
4. Prueba casos extremos: ¿qué sucede si no hay terminal conectado?

### Paso 2: Preparar para la presentación

Tu carpeta de plugin debe contener:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | ID unik kebab-case | `"my-awesome-plugin"` |
| `version` | Versión semántica | `"1.0.0"` |
| `description` | Descripción en una línea | `"Monitors nginx access logs in real-time"` |
| `author` | Tu nombre | `"John Doe"` |
| `main` | Punto de entrada | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | Tipo de licencia (MIT recomendado) |
| `keywords` | Array de etiquetas de búsqueda |
| `soom.minVersion` | Versión mínima de WIA SOOM requerida |

### Step 3: Submit to the Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** tu plugin a `plugins/{your-plugin-name}/`
3. **Submit** un Pull Request

### Step 4: Review and approval

Revisamos cada plugin por:

- **Seguridad** — no APIs peligrosas (ver [Security Rules](#security-rules))
- **Calidad** — ¿funciona? ¿Está limpio el código?
- **Utilidad** — ¿resuelve un problema real?

Después de la aprobación:
1. Tu plugin se añade a `registry.json`
2. Se crea un paquete ZIP en `dist/`
3. Tu plugin aparece en la **Plugin Store** para todos los usuarios de WIA SOOM!

---

## Part 5: Best Practices

### Security Rules

Estas reglas son **obligatorias**. Los plugins que las violen serán rechazados.

| Rule | Why |
|------|-----|
| **NUNCA** use `eval()` o `new Function()` | Riesgo de inyección de código |
| **NUNCA** use `child_process`, `exec()`, `spawn()` | Solo use `ctx.terminal.send()` para comandos |
| **NUNCA** obtenga URLs externas | Excepción: puntos finales de API de `wiasoom.com` |
| **NUNCA** acceda a `process.env` | Las variables de entorno pueden contener secretos |
| **NUNCA** use `require('fs')` directamente | Use `ctx.settings` para almacenamiento, `ctx.sftp` para transferencia de archivos |
| **NUNCA** use paquetes externos de npm | Solo JavaScript puro — sin node_modules |
| **DEBE** usar `ctx.terminal.send()` para todos los comandos remotos | Esto pasa a través del canal SSH seguro |
| **DEBE** limpiar en `deactivate()` | Eliminar oyentes, limpiar intervalos |

### Error Handling

Siempre envuelva operaciones arriesgadas en try/catch:
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

Si tu plugin crea intervalos, oyentes o suscripciones — límpialos:
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

WIA SOOM soporta 254 idiomas. Para hacer que la etiqueta de tu plugin sea traducible, usa un enfoque simple:
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

Ejecuta `df -h` en el servidor remoto y muestra el espacio usado/disponible en la barra de estado.
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

Un plugin que gestiona una lista de TODO usando configuraciones para almacenamiento persistente y una vista web para mostrar.

> **Patrón de diseño:** Dado que las vistas web no pueden llamar directamente a las APIs de los plugins, este plugin utiliza un enfoque de "instantánea" — lee los TODOs de las configuraciones, los renderiza como HTML de solo lectura y proporciona acciones basadas en la barra lateral para agregar elementos. La vista web es una **capa de visualización**, no un formulario interactivo.
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

Monitorea la salida del terminal y envía una notificación cuando se detectan patrones específicos.
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
| `server` | Püñeñma server |
| `devtools` | Tüfañma dev |
| `calculator` | Kalkulador y converter |
| `simulator` | Simulador |
| `game` | Juegos en terminal |
| `business` | Tüfañma biznes |
| `security` | Tüfañma seguridad y auditoría |
| `web` | Püñeñma web server |
| `education` | Tüfañma educación |
| `health` | Tüfañma salud |
| `islamic` | Tüfañma islámica (tiempos de oración, etc.) |
| `science` | Tüfañma científica |
| `quantum` | Tüfañma computación cuántica |
| `ai` | Tüfañma potenciada por IA |
| `biotech` | Tüfañma biotecnología |
| `space` | Tüfañma espacio y astronomía |
| `network` | Tüfañma red |
| `database` | Püñeñma base de datos |
| `monitoring` | Monitoreo de servidor |
| `devops` | DevOps y CI/CD |
| `utility` | Utilidades generales |
| `design` | Tüfañma diseño |
| `ecommerce` | Tüfañma comercio electrónico |
| `automation` | Tüfañma automatización |
| `kpop` | Tüfañma relacionado con K-pop |
| `accessibility` | Tüfañma accesibilidad |
| `analytics` | Tüfañma analítica y reportes |
| `wia` | Tüfañma ecosistema WIA |
| `all` | Aparece en todas las categorías |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Püñeñma server |
| `shield` | Seguridad |
| `database` | Base de datos |
| `activity` | Monitoreo |
| `terminal` | Tüfañma terminal |
| `code` | Desarrollo |
| `hard-drive` | Disco/almacenamiento |
| `network` | Red |
| `lock` | Autenticación/encriptación |
| `eye` | Observación/monitoreo |
| `check-square` | Tareas/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuración |
| `zap` | Automatización |
| `globe` | Web/internacional |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construye algo asombroso. Compártelo con el mundo.</em></p>
<p align="center"><em>— El equipo de WIA SOOM</em></p>