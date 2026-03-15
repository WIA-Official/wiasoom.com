<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin ᠠᡵᠠᡥᠠ ᠨᡳᠶᠠᠯᠮᠠ ᠪᡳᡨᡥᡝ (Araha Niyalma Bithe)</h1>
<p align="center"><strong>ᠰᡠᠨᠵᠠ ᡝᡵᡳᠨ ᡩᠣᠯᠣ ᠰᡳᠨᡳ plugin ᠠᡵᠠ᠃ (Sunja erin dolo sini plugin ara.)</strong></p>
<p align="center">WIA SOOM ᡩᠣᠯᠣ ᡥᡡᠰᡠᠨ ᡶᡠᡩᠠᠰᡳ ᠠᡤᡡᡵᠠ᠂ ᡩᠠᠰᡥᡳᠪᠣᡵᡩ᠂ ᡝᠩᡤᡝᠮᡠ ᠠᡵᠠ᠃ (Husun fudasi agura, dashboard, enggemu ara.)</p>

---

## ᠪᡳᡨᡥᡝᡳ ᠰᡠᡥᡝ (Bithei Suhe — Hacin i Meyen)

- [ᡩᡠᡳᠴᡳ ᡶᠶᡝᠯᡝᠨ: ᡩᡠᡳᠴᡳ Plugin — ᠰᡠᠨᠵᠠ ᡝᡵᡳᠨ ᡩᠣᠯᠣ](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ᠵᠠᡳ ᡶᠶᡝᠯᡝᠨ: Plugin Context API ᠪᡳᡨᡥᡝ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ᡳᠯᠠᠴᡳ ᡶᠶᡝᠯᡝᠨ: Webview ᡩᠠᠪᠠᠯᡳ ᠨᡳᡵᡠᡤᠠᠨ UI ᠠᡵᠠᠮᡝ](#part-3-building-custom-ui-with-webviews)
- [ᡩᡠᡳᠴᡳ ᡶᠶᡝᠯᡝᠨ: Plugin ᡤᡝᠪᡠᠮᡝ](#part-4-publishing-your-plugin)
- [ᠰᡠᠨᠵᠠᠴᡳ ᡶᠶᡝᠯᡝᠨ: ᠰᠠᡳᠨ ᡩᠣᡵᠣ](#part-5-best-practices)
- [ᠨᡳᠩᡤᡠᠴᡳ ᡶᠶᡝᠯᡝᠨ: ᠶᠠᡵᡤᡳᠶᠠᠨ ᡩᡠᡵᡳᠪᡠᠯᡝᠨ](#part-6-real-world-examples)
- [ᡶᡠᠯᡠ: ᡥᠠᠴᡳᠨ ᠵᠠᡳ ᠨᡳᡵᡠᡤᠠᠨ ᠰᡠᡵᡝ](#appendix-categories--icons)

---

## ᡩᡠᡳᠴᡳ ᡶᠶᡝᠯᡝᠨ: ᡩᡠᡳᠴᡳ Plugin — ᠰᡠᠨᠵᠠ ᡝᡵᡳᠨ ᡩᠣᠯᠣ (Duici Fyelen: Duici Plugin — Sunja Erin Dolo)

### ᠰᡳ ᠠᡳ ᠠᡵᠠᠮᠪᡳ (Si ai arambi)

"Hello World" plugin — ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡩᡝ ᡝᠮᡠ ᡥᡝᡵᡤᡝᠨ ᠰᡳᠨᡩᠠᠮᠪᡳ᠃ ᡥᡝᡵᡤᡝᠨ ᡩᡝ ᡩᠠᡥᠠ ᡝᠮᡠ ᡠᠯᡥᡳᠴᡝᠨ ᡨᡠᠴᡳᠮᠪᡳ᠃ (Sidebar de emu hergen sindambi. Hergen de daha emu ulhicen tucimbi.)

### ᡩᡠᡳᠴᡳ ᡝᠶᡝᠨ: Plugin ᡶᠠᡳᠯᠠᠨ ᠠᡵᠠ (Duici Eyen: Plugin failan ara)

```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```

### ᠵᠠᡳ ᡝᠶᡝᠨ: package.json ᠠᡵᠠ (Jai Eyen: package.json ara)

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

**ᠠᡴᡩᡠᠨ ᠪᠠᡳᡨᠠ (Akdun baita — Required fields):** `name`, `version`, `description`, `author`, `main`

### ᡳᠯᠠᠴᡳ ᡝᠶᡝᠨ: index.js ᠠᡵᠠ (Ilaci Eyen: index.js ara)

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

### ᡩᡠᡳᠴᡳ ᡝᠶᡝᠨ: WIA SOOM ᡩᠠᠰᡳᠮᡝ ᡩᡝᡵᡳᠪᡠ (Duici Eyen: WIA SOOM dasime deribu)

WIA SOOM ᡩᠠᠰᡳᠮᡝ ᡩᡝᡵᡳᠪᡠ (ᡝᠴᡳ Settings → Plugins ᡩᡝ plugin ᡩᠠᡥᡡᠮᡝ ᡩᡝᡵᡳᠪᡠ)᠃ (Dasime deribu, eci Settings → Plugins de plugin dahume deribu.)

ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡩᡝ **"Hello World"** ᡥᡝᡵᡤᡝᠨ ᠰᠠᠪᡠᠮᠪᡳ᠃ ᡥᡝᡵᡤᡝᠨ ᡩᡝ ᡩᠠᡥᠠᡴᡳ — ᡝᠮᡠ ᠰᠠᡳᠨ ᡠᠯᡥᡳᠴᡝᠨ ᡨᡠᠴᡳᠮᠪᡳ! (Dergi gala de "Hello World" hergen sabumbi. Hergen de dahaci — emu sain ulhicen tucimbi!)

### ᡝᡵᡝ ᠠᡩᠠᠯᡳ ᠶᠠᠪᡠᠮᠪᡳ (Ere adali yabumbi — How it works)

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

## ᠵᠠᡳ ᡶᠶᡝᠯᡝᠨ: Plugin Context API ᠪᡳᡨᡥᡝ (Jai Fyelen: Plugin Context API Bithe)

ᠰᡳᠨᡳ `activate(context)` ᡶᡠᠩᡴᠰᡳ ᡥᡡᠯᠠᠪᡠᡵᡝ ᡶᠣᠨᡩᡝ, `context` (ᡝᠴᡳ `ctx`) ᡝᡵᡝ API ᠰᡝ ᠪᡠᠮᠪᡳ: (Sini activate(context) fungksi hulabure fonde, context (eci ctx) ere API se bumbi:)

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

### `ctx.terminal` — ᡤᠣᡵᠣᡴᡳ ᡶᡠᡩᠠᠰᡳ ᡩᡝ ᡶᠠᡶᡠᠨ ᠶᠠᠪᡠᠮᡝ (Goroki fudasi de fafun yabume)

#### `terminal.send(sessionId, data)`

ᡝᠮᡠ ᡶᠠᡶᡠᠨ (ᡝᠴᡳ ᡝᠮᡠ ᠪᠠᡳᡨᠠ) ᡩᡝᡵᡳᠪᡠᡥᡝ ᡨᡝᡵᠮᡳᠨᠠᠯ ᡩᡝ ᡠᠩᡤᡳ᠃ (Emu fafun (eci emu baita) deribuhe terminal de unggi.)

| ᠪᠠᡳᡨᠠ (Baita) | ᡥᠠᠴᡳᠨ (Hacin) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|-----------|------|-------------|
| `sessionId` | `string` | ᡨᡝᡵᠮᡳᠨᠠᠯ ᡳ ᡝᠮᡠᠨ (Terminal i emun) |
| `data` | `string` | ᡶᠠᡶᡠᠨ ᡝᠴᡳ ᠪᠠᡳᡨᠠ (Fafun eci baita) |

```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```

#### `terminal.onOutput(sessionId, callback)`

ᡨᡝᡵᠮᡳᠨᠠᠯ ᡳ ᡨᡠᠴᡳᡥᡝ ᠪᠠᡳᡨᠠ ᡩᠣᠨᠵᡳᠮᡝ᠃ ᡝᠮᡠ **ᠨᠠᡴᠠᠮᡝ ᡶᡠᠩᡴᠰᡳ** ᠪᡝᡩᡝᡵᡝᠮᠪᡳ᠃ (Terminal i tucihe baita donjime. Emu nakame fungksi bederembi.)

| ᠪᠠᡳᡨᠠ (Baita) | ᡥᠠᠴᡳᠨ (Hacin) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|-----------|------|-------------|
| `sessionId` | `string` | ᡨᡝᡵᠮᡳᠨᠠᠯ ᡳ ᡝᠮᡠᠨ (Terminal i emun) |
| `callback` | `(data: string) => void` | ᡨᡠᠴᡳᡥᡝ ᠪᠠᡳᡨᠠ ᡳ ᡶᡠᠩᡴᠰᡳ (Tucihe baita i fungksi) |
| **ᠪᡝᡩᡝᡵᡝᠮᠪᡳ** | `() => void` | ᡩᠣᠨᠵᡳᠮᡝ ᠨᠠᡴᠠᠮᡝ ᡩᡝ ᡥᡡᠯᠠ (Donjime nakame de hula) |

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

**ᡝᡵᡤᡝᠮᡠ: (Ergemu:)** ᠨᠠᡴᠠᠮᡝ ᡶᡠᠩᡴᠰᡳ ᡝᠵᡝᠨ ᡨᡝᠪᡠ, `deactivate()` ᡩᡝ ᡥᡡᠯᠠ — ᡝᡵᡝᡴᡳ ᠣᡴᡳ ᡨᡝᡳᠰᡠᠮᠪᡳ᠃ (Nakame fungksi ejen tebu, deactivate() de hula — ereci oki teisumbi.)

---

### `ctx.sftp` — ᡶᠠᡳᠯ ᡤᡡᡵᡳᠮᡝ (Fail gurime)

> **ᠠᡩᠠᠯᡳ: ᠵᡳᡥᡝ ᠵᡳᠮᠪᡳ (Adali: Jihe jimbi)** — SFTP API ᡝᠵᡝᡥᡝ ᠠᡴᡡ WIA SOOM ᡳ SFTP ᡝᠩᡤᡳᠨ ᡩᡝ ᡝᠮᡠ ᠣᡥᠣ᠃ `list()` ᠰᡠᠯᡶᠠ ᠪᡝᡩᡝᡵᡝᠮᠪᡳ, `upload()`/`download()` ᡤᡡᠸᠠ ᠠᡴᡡ᠃ ᠵᡳᡥᡝ ᡶᡠᠯᡤᡳᠶᠠᠨ ᡩᡝ ᡝᠨᡨᡝᡥᡝᠮᡝ ᡩᡝᡵᡳᠪᡠᠮᠪᡳ᠃ ᡨᡝ ᡶᠣᠨᡩᡝ, `ctx.terminal.send()` ᡩᡝ `scp` ᡝᠴᡳ `rsync` ᡶᠠᡶᡠᠨ ᠪᠠᡳᡨᠠᠯᠠ᠃ (SFTP API ejeehe aku WIA SOOM i SFTP engine de emu oho. list() sulfa bederembi, upload()/download() guwa aku. Jihe fulgiyan de enteheme deribumbi. Te fonde, ctx.terminal.send() de scp eci rsync fafun baitala.)

#### `sftp.list(sessionId, path)`

ᡤᠣᡵᠣᡴᡳ ᡶᡠᡩᠠᠰᡳ ᡩᡝ ᡶᠠᡳᠯ ᡨᡠᠴᡳᠪᡠ᠃ (Goroki fudasi de fail tucibu.)

```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```

#### `sftp.upload(sessionId, localPath, remotePath)`

ᡝᠵᡝᠨ ᡶᡠᡩᠠᠰᡳ ᠴᡳ ᡤᠣᡵᠣᡴᡳ ᡶᡠᡩᠠᠰᡳ ᡩᡝ ᡶᠠᡳᠯ ᡩᠣᠪᠣ᠃ (Ejen fudasi ci goroki fudasi de fail dobo.)

```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```

#### `sftp.download(sessionId, remotePath, localPath)`

ᡤᠣᡵᠣᡴᡳ ᡶᡠᡩᠠᠰᡳ ᠴᡳ ᡝᠵᡝᠨ ᡶᡠᡩᠠᠰᡳ ᡩᡝ ᡶᠠᡳᠯ ᡤᠠᠵᡳ᠃ (Goroki fudasi ci ejen fudasi de fail gaji.)

```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```

**ᡩᠠᠨᡤᠠᠮᡝ ᡩᠣᡵᠣ (Dangame doro — Workaround, SFTP API ᡩᡝᡵᡳᠪᡠᠮᡝ ᡩᡝ ᡳᠰᡳᠨᠠ):**

```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```

---

### `ctx.ui` — ᠨᡳᠶᠠᠯᠮᠠ ᡳ ᡩᡠᡵᡠᠨ (Niyalma i durun)

#### `ui.addSidebarButton(options)`

WIA SOOM ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡩᡝ ᡥᡝᡵᡤᡝᠨ ᠰᡳᠨᡩᠠ᠃ (WIA SOOM dergi gala de hergen sinda.)

| ᠪᠠᡳᡨᠠ (Baita) | ᡥᠠᠴᡳᠨ (Hacin) | ᠠᡴᡩᡠᠨ (Akdun) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|--------|------|----------|-------------|
| `id` | `string` | ᠸᠠᡴᠠ (Waka) | ᡝᠮᡠ ᡤᡝᠪᡠ (Emu gebu — plugin gebu adali) |
| `icon` | `string` | ᠠᡴᡩᡠᠨ (Akdun) | Lucide ᠨᡳᡵᡠᡤᠠᠨ ᡤᡝᠪᡠ (Lucide nirugan gebu, ᡠᡨᡥᠠᡳ `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ᠠᡴᡩᡠᠨ (Akdun) | ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡩᡝ ᡥᡝᡵᡤᡝᠨ (Dergi gala de hergen) |
| `onClick` | `() => void` | ᠠᡴᡩᡠᠨ (Akdun) | ᡩᠠᡥᠠ ᡶᡠᠩᡴᠰᡳ (Daha fungksi) |

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

**ᠨᡳᡵᡠᡤᠠᠨ ᠰᡠᡵᡝ ᡳ ᡤᡳᠰᡠᡵᡝᠨ (Nirugan sure i gisuren):** ᡤᡝᠮᡠ ᠨᡳᡵᡠᡤᠠᠨ ᠪᡝ [lucide.dev/icons](https://lucide.dev/icons) ᡩᡝ ᡨᡠᠸᠠ᠃ (Gemu nirugan be lucide.dev/icons de tuwa.)

> **ᡤᡳᠰᡠᡵᡝᠨ (Gisuren)::** ᡶᡝ ᠠᠨᠠ plugin ᡤᡳᡵᠠᠨ ᠠᡩᠠᠯᡳ `addSidebarButton(id, icon, label, onClick)` ᠪᠠᡳᡨᠠᠯᠠᠮᠪᡳ᠃ ᠵᠠᡳ ᡝᠨᡨᡝᡥᡝᠮᡝ API ᡝᡵᡝ ᠪᡳᡨᡥᡝ ᡩᡝ ᡤᡳᠰᡠᡵᡝᡥᡝ **ᡝᠮᡠ ᠣᠪᠵᡝᡴᡨ** ᠪᠠᡳᡨᠠᠯᠠᠮᠪᡳ᠃ ᡳᠴᡝ plugin ᡩᡝ ᡝᡵᡝ ᡩᠣᡵᠣ ᠪᠠᡳᡨᠠᠯᠠ᠃ (Fe ana plugin giran adali addSidebarButton(id, icon, label, onClick) baitalambi. Jai enteheme API ere bithe de gisurehe emu objekt baitalambi. Ice plugin de ere doro baitala.)

#### `ui.openWebview(options)`

ᡩᠠᠪᠠᠯᡳ ᠨᡳᡵᡠᡤᠠᠨ HTML ᡩᡝ ᡝᠮᡠ ᡶᠠ ᠪᡝ ᠨᡝᡳ᠃ ᡝᡵᡝ ᡩᡝ ᠰᡳ ᠪᠠᠶᠠᠨ UI ᠠᡵᠠᠮᠪᡳ᠃ (Dabali nirugan HTML de emu fa be nei. Ere de si bayan UI arambi.)

| ᠪᠠᡳᡨᠠ (Baita) | ᡥᠠᠴᡳᠨ (Hacin) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|--------|------|-------------|
| `title` | `string` | ᡶᠠ ᡳ ᡤᡝᠪᡠ (Fa i gebu) |
| `html` | `string` | HTML ᡩᠣᡵᡤᡳ (HTML dorgi) |

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

> [ᡳᠯᠠᠴᡳ ᡶᠶᡝᠯᡝᠨ](#part-3-building-custom-ui-with-webviews) ᡩᡝ ᡤᡝᡵᡝᠨ ᡶᡠᠯᡠ webview ᡩᠣᡵᠣ ᡨᡠᠸᠠ᠃ (Ilaci fyelen de geren fulu webview doro tuwa.)

#### `ui.showNotification(type, message)`

ᡝᠮᡠ ᠰᡝᠯᡤᡳᠶᡝᠨ ᡠᠯᡥᡳᠴᡝᠨ ᡨᡠᠴᡳᠪᡠ᠃ (Emu selgiyen ulhicen tucibu.)

| ᠪᠠᡳᡨᠠ (Baita) | ᡥᠠᠴᡳᠨ (Hacin) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ᡠᠯᡥᡳᠴᡝᠨ ᡳ ᡩᡠᡵᡠᠨ (Ulhicen i durun) |
| `message` | `string` | ᡨᡠᠴᡳᠪᡠᡵᡝ ᡤᡳᠰᡠᠨ (Tucibure gisun) |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

ᡶᡝᠵᡳᠯᡝ ᡩᡝ ᡝᠮᡠ ᡝᠨᡨᡝᡥᡝᠮᡝ ᡥᡝᡵᡤᡝᠨ ᠰᡳᠨᡩᠠ᠃ (Fejile de emu enteheme hergen sinda.)

| ᠪᠠᡳᡨᠠ (Baita) | ᡥᠠᠴᡳᠨ (Hacin) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|-----------|------|-------------|
| `id` | `string` | ᡝᠮᡠ ᡤᡝᠪᡠ (Emu gebu) |
| `text` | `string` | ᡨᡠᠴᡳᠪᡠᡵᡝ ᡤᡳᠰᡠᠨ (Tucibure gisun) |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — ᡝᠨᡨᡝᡥᡝᠮᡝ ᡨᡝᠪᡠᡵᡝ (Enteheme tebure)

Plugin ᡳ ᡨᡝᠪᡠᡵᡝ `~/.wia-soom/plugins/{sini-plugin}/.plugin-settings.json` ᡩᡝ ᡝᠨᡨᡝᡥᡝᠮᡝ ᡨᡝᠪᡠᠮᠪᡳ᠃ (Plugin i tebure ~/.wia-soom/plugins/{sini-plugin}/.plugin-settings.json de enteheme tebumbi.)

#### `settings.get(key)`

ᡨᡝᠪᡠᡥᡝ ᡩᠣᡵᡤᡳ ᠪᡝ ᡥᡡᠯᠠ᠃ (Tebuhe dorgi be hula.)

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

ᡝᡵᡝ ᡤᡝᠪᡠ ᠠᡴᡡ ᠣᠴᡳ `undefined` ᠪᡝᡩᡝᡵᡝᠮᠪᡳ᠃ (Ere gebu aku oci undefined bederembi.)

#### `settings.set(key, value)`

ᡩᠣᡵᡤᡳ ᡨᡝᠪᡠ᠃ ᡤᡳᠰᡠᠨ᠂ ᡨᠣᠨ᠂ ᠶᠠᡵᡤᡳᠶᠠᠨ᠂ ᠮᡝᠶᡝᠨ᠂ ᠣᠪᠵᡝᡴᡨ ᡤᡝᠮᡠ ᠣᠮᠪᡳ᠃ (Dorgi tebu. Gisun, ton, yargiyan, meyen, objekt gemu ombi.)

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**ᡩᡠᡵᡳᠪᡠᠯᡝᠨ: ᠨᡳᠶᠠᠯᠮᠠ ᡳ ᠴᡳᡥᠠᡳ ᡝᠵᡝ (Duribulen: Niyalma i cihai eje)**

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

### `ctx.ai` — AI ᡳᠰᡳᠪᡠᠮᡝ (AI isibume)

> **ᠠᡩᠠᠯᡳ: ᠵᡳᡥᡝ ᠵᡳᠮᠪᡳ (Adali: Jihe jimbi)** — AI API ᡝᠵᡝᡥᡝ ᠠᡴᡡ Soomy ᡩᡝ ᡳᠰᡳᠪᡠᡥᠠ᠃ ᡨᡝ ᡶᠣᠨᡩᡝ `{ response: 'AI not yet connected' }` ᠪᡝᡩᡝᡵᡝᠮᠪᡳ᠃ ᠵᡳᡥᡝ ᡶᡠᠯᡤᡳᠶᠠᠨ ᡩᡝ ᡝᠨᡨᡝᡥᡝᠮᡝ AI ᡳᠰᡳᠪᡠᠮᡝ ᡩᡝᡵᡳᠪᡠᠮᠪᡳ᠃ (AI API ejeehe aku Soomy de isibuha. Te fonde { response: 'AI not yet connected' } bederembi. Jihe fulgiyan de enteheme AI isibume deribumbi.)

#### `ai.chat(messages, options?)`

Soomy AI ᡩᡝ ᡤᡳᠰᡠᠨ ᡠᠩᡤᡳ᠃ (Soomy AI de gisun unggi.)

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## ᡳᠯᠠᠴᡳ ᡶᠶᡝᠯᡝᠨ: Webview ᡩᠠᠪᠠᠯᡳ ᠨᡳᡵᡠᡤᠠᠨ UI ᠠᡵᠠᠮᡝ (Ilaci Fyelen: Webview dabali nirugan UI arame)

`openWebview()` API ᡩᡝ HTML, CSS, JavaScript ᡩᠠᠪᠠᠯᡳ ᡩᠠᠰᡥᡳᠪᠣᡵᡩ UI ᠠᡵᠠᠮᠪᡳ — ᡤᡝᠮᡠ ᡝᠮᡠ ᡶᠠ ᡩᠣᡵᡤᡳ᠃ (openWebview() API de HTML, CSS, JavaScript dabali dashboard UI arambi — gemu emu fa dorgi.)

> **ᡝᡵᡤᡝᠮᡠ (Ergemu)::** Webview ᠪᡝ **ᡨᡠᠸᠠᠮᡝ ᡨᡝᡳᠯᡝ (tuwame teile)** ᠪᠠᡳᡨᠠᠯᠠᠮᠪᡳ᠃ Plugin API (`ctx.settings`, `ctx.terminal` ᡤᡠᠸᠠ) ᡩᡝ ᡩᠠᠰᡳᠮᡝ ᡥᡡᠯᠠᠮᡝ ᠮᡠᡨᡝᠮᠪᡳ᠃ ᡤᡝᠮᡠ ᠪᠠᡳᡨᠠ ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡥᡝᡵᡤᡝᠨ ᡩᡝ ᠶᠠᠪᡠ, `openWebview()` ᡩᡝ ᡨᡝ ᡶᠣᠨᡩᡝ ᡳ ᠠᡩᠠᠯᡳ ᡨᡠᠴᡳᠪᡠ᠃ (Webview be tuwame teile baitalambi. Plugin API (ctx.settings, ctx.terminal guwa) de dasime hulame mutembi. Gemu baita dergi gala hergen de yabu, openWebview() de te fonde i adali tucibu.)

### ᡩᠣᡵᠣ: ᡨᡝᡵᠮᡳᠨᠠᠯ ᡶᠠᡶᡠᠨ → ᡨᡠᠴᡳᡥᡝ ᠪᡝ ᡶᡠᡩᠠᠰᡳ → HTML ᡩᡝ ᡨᡠᠴᡳᠪᡠ (Doro: Terminal fafun → Tucihe be fudasi → HTML de tucibu)

ᡝᡵᡝ ᡝᠮᡠ plugin ᡳ ᡝᡵᡝ ᠴᡳ ᡤᡝᡵᡝᠨ ᠪᠠᡳᡨᠠᠯᠠᡵᠠ ᡩᠣᡵᠣ᠃ ᡶᠠᡶᡠᠨ ᠶᠠᠪᡠ᠂ ᡨᡠᠴᡳᡥᡝ ᠪᡝ ᡶᡠᡩᠠᠰᡳ᠂ ᠨᡳᡵᡠᡤᠠᠨ ᡩᡝ ᡨᡠᠴᡳᠪᡠ᠃ (Ere emu plugin i ere ci geren baitalara doro. Fafun yabu, tucihe be fudasi, nirugan de tucibu.)

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

### ᡩᠣᡵᠣ: ᡳᠨᡝᠩᡤᡳ ᡩᠠᠰᡥᡳᠪᠣᡵᡩ — ᠪᡝᠶᡝ ᠴᡳ ᡳᠴᡝᡥᡳᠶᠠᠮᡝ (Doro: Inenggi Dashboard — Beye ci icehiyame)

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

### ᡩᠣᡵᠣ: Webview ᡩᡝ ᡨᡝᠪᡠᡵᡝ ᡨᡠᠴᡳᠪᡠᠮᡝ (Doro: Webview de tebure tucibume)

> **ᡤᡳᠰᡠᡵᡝᠨ (Gisuren)::** Webview ᡨᡠᠸᠠᠮᡝ ᡨᡝᡳᠯᡝ — plugin API ᡩᡝ ᡩᠠᠰᡳᠮᡝ ᡥᡡᠯᠠᠮᡝ ᠮᡠᡨᡝᡵᠠᡴᡡ᠃ `ctx.settings` ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡥᡝᡵᡤᡝᠨ ᡩᡝ ᠪᠠᡳᡨᠠᠯᠠ, `openWebview()` ᡩᡝ ᡨᡝ ᡶᠣᠨᡩᡝ ᡳ ᠠᡩᠠᠯᡳ ᡨᡠᠴᡳᠪᡠ᠃ (Webview tuwame teile — plugin API de dasime hulame muteraku. ctx.settings dergi gala hergen de baitala, openWebview() de te fonde i adali tucibu.)

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

## ᡩᡠᡳᠴᡳ ᡶᠶᡝᠯᡝᠨ: Plugin ᡤᡝᠪᡠᠮᡝ (Duici Fyelen: Plugin gebume)

### ᡩᡠᡳᠴᡳ ᡝᠶᡝᠨ: ᡝᠵᡝᠨ ᡶᡠᡩᠠᠰᡳ ᡩᡝ ᡤᡳᠴᡳᡥᡳᠶᠠ (Duici Eyen: Ejen fudasi de gicihiya)

1. Plugin ᠪᡝ `~/.wia-soom/plugins/{sini-plugin}/` ᡩᡝ ᡩᠣᠰᡳᠮᠪᡠ᠃
2. WIA SOOM ᡩᠠᠰᡳᠮᡝ ᡩᡝᡵᡳᠪᡠ᠃
3. ᠶᠠᠪᡠᡵᡝ ᠪᡝ ᡤᡳᠴᡳᡥᡳᠶᠠ: ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡥᡝᡵᡤᡝᠨ ᡨᡠᠴᡳᠮᠪᡳᠣ? ᡶᡠᠩᡴᠰᡳ ᠰᠠᡳᠨ ᠶᠠᠪᡠᠮᠪᡳᠣ?
4. ᡤᡝᡵᡝᠨ ᠪᠠᡳᡨᠠ ᡤᡳᠴᡳᡥᡳᠶᠠ: ᡨᡝᡵᠮᡳᠨᠠᠯ ᡳᠰᡳᠪᡠᡥᠠ ᠠᡴᡡ ᠣᠴᡳ ᠠᡳ ᠣᠮᠪᡳ?

### ᠵᠠᡳ ᡝᠶᡝᠨ: ᡠᠩᡤᡳᡵᡝ ᡩᡝ ᠪᡝᠯᡥᡝ (Jai Eyen: Unggire de belhe)

ᠰᡳᠨᡳ plugin ᡶᠠᡳᠯᠠᠨ ᡩᠣᡵᡤᡳ ᡝᡵᡝ ᠪᡳ:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**ᠠᡴᡩᡠᠨ package.json ᡳ ᠪᠠᡳᡨᠠ (Akdun package.json i baita):**

| ᠪᠠᡳᡨᠠ (Baita) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) | ᡩᡠᡵᡳᠪᡠᠯᡝᠨ (Duribulen) |
|-------|-------------|---------|
| `name` | ᡝᠮᡠ kebab-case ᡤᡝᠪᡠ | `"my-awesome-plugin"` |
| `version` | ᡶᡠᠯᡤᡳᠶᠠᠨ ᡳ ᡨᠣᠨ | `"1.0.0"` |
| `description` | ᡝᠮᡠ ᠮᡝᠶᡝᠨ ᡤᡳᠰᡠᡵᡝᠨ | `"Monitors nginx access logs in real-time"` |
| `author` | ᠰᡳᠨᡳ ᡤᡝᠪᡠ | `"John Doe"` |
| `main` | ᡩᡝᡵᡳᠪᡠᡵᡝ ᡶᠠᡳᠯ | `"index.js"` |

**ᠴᡳᡥᠠᡳ ᠪᠠᡳᡨᠠ (Cihai baita — Optional fields):**

| ᠪᠠᡳᡨᠠ (Baita) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|-------|-------------|
| `license` | ᡩᠠᠰᠠᠨ ᡳ ᡥᠠᠴᡳᠨ (MIT ᡩᡠᡵᡳᠪᡠᠯᡝᠨ) |
| `keywords` | ᠪᠠᡳᡨᠠᠯᠠᡵᠠ ᡤᡳᠰᡠᠨ ᡳ ᠮᡝᠶᡝᠨ |
| `soom.minVersion` | WIA SOOM ᡳ ᡝᡵᡝ ᠴᡳ ᡶᡝᠵᡝᡵᡤᡳ ᡶᡠᠯᡤᡳᠶᠠᠨ |

### ᡳᠯᠠᠴᡳ ᡝᠶᡝᠨ: Plugin ᡩᠠᠩᠰᡝ ᡩᡝ ᡠᠩᡤᡳ (Ilaci Eyen: Plugin dangse de unggi)

1. [Plugin Store](https://wiasoom.com) ᠪᡝ **Fork** ᠠᡵᠠ
2. ᠰᡳᠨᡳ plugin ᠪᡝ `plugins/{sini-plugin-gebu}/` ᡩᡝ **ᠰᡳᠨᡩᠠ**
3. ᡝᠮᡠ Pull Request **ᡠᠩᡤᡳ**

### ᡩᡠᡳᠴᡳ ᡝᠶᡝᠨ: ᡤᡳᠴᡳᡥᡳᠶᠠᠮᡝ ᠵᠠᡳ ᡤᡳᠰᡠᠨ ᡩᠠᡥᠠᠮᡝ (Duici Eyen: Gicihiyame jai gisun dahame)

ᠮᡠᠰᡝ ᡤᡝᠮᡠ plugin ᠪᡝ ᡤᡳᠴᡳᡥᡳᠶᠠᠮᠪᡳ:

- **ᠠᡴᡩᡠᠨ (Akdun — Security)** — ᡝᡵᡤᡝᠯᡝᠮᡝ API ᠠᡴᡡ ᠪᡳᠣ ([ᠠᡴᡩᡠᠨ ᡴᠣᠣᠯᡳ](#security-rules) ᡨᡠᠸᠠ)
- **ᠰᠠᡳᠨ (Sain — Quality)** — ᠶᠠᠪᡠᠮᠪᡳᠣ? ᠪᡳᡨᡥᡝ ᠪᠣᠯᠵᠣᠮᠪᡳᠣ?
- **ᠪᠠᡳᡨᠠᠯᠠᡵᠠ (Baitalara — Usefulness)** — ᠶᠠᡵᡤᡳᠶᠠᠨ ᡶᡝᡩᡝᡵᡝᡴᡠ ᠪᡝ ᡩᠠᠰᠠᠮᠪᡳᠣ?

ᡤᡳᠰᡠᠨ ᡩᠠᡥᠠᡥᠠ ᠮᠠᠩᡤᡳ:
1. ᠰᡳᠨᡳ plugin ᠪᡝ `registry.json` ᡩᡝ ᠰᡳᠨᡩᠠᠮᠪᡳ
2. ᡝᠮᡠ ZIP ᠪᡠᠨᡩᡝᠯ `dist/` ᡩᡝ ᠠᡵᠠᠮᠪᡳ
3. ᠰᡳᠨᡳ plugin ᡤᡝᠮᡠ WIA SOOM ᠪᠠᡳᡨᠠᠯᠠᡵᠠ ᠨᡳᠶᠠᠯᠮᠠ ᡳ **Plugin Store** ᡩᡝ ᡨᡠᠴᡳᠮᠪᡳ!

---

## ᠰᡠᠨᠵᠠᠴᡳ ᡶᠶᡝᠯᡝᠨ: ᠰᠠᡳᠨ ᡩᠣᡵᠣ (Sunjaci Fyelen: Sain Doro)

### ᠠᡴᡩᡠᠨ ᡴᠣᠣᠯᡳ (Akdun Kooli — Security Rules)

ᡝᡵᡝ ᡴᠣᠣᠯᡳ **ᠠᡴᡩᡠᠨ** ᠪᡳ᠃ ᡝᡵᡝ ᠪᡝ ᡶᡠᠯᡳᠶᡝᡵᡝ plugin ᠨᠠᡴᠠᠪᡠᠮᠪᡳ᠃ (Ere kooli akdun bi. Ere be fuliyere plugin nakabumbi.)

| ᡴᠣᠣᠯᡳ (Kooli) | ᠠᡳᠴᡳ (Aici) |
|------|-----|
| `eval()` ᡝᠴᡳ `new Function()` ᠪᡝ **ᡝᠮᡠ ᡳᠨᡠ ᠪᠠᡳᡨᠠᠯᠠᡵᠠᡴᡡ** | ᠪᡳᡨᡥᡝ ᡩᠣᠰᡳᠮᠪᡠᡵᡝ ᡝᡵᡤᡝᠯᡝᠮᡝ |
| `child_process`, `exec()`, `spawn()` ᠪᡝ **ᡝᠮᡠ ᡳᠨᡠ ᠪᠠᡳᡨᠠᠯᠠᡵᠠᡴᡡ** | `ctx.terminal.send()` ᡨᡝᡳᠯᡝ ᠪᠠᡳᡨᠠᠯᠠ |
| ᡨᡠᠯᡝᡵᡤᡳ URL ᠪᡝ **ᡝᠮᡠ ᡳᠨᡠ ᡤᠠᠵᡳᡵᠠᡴᡡ** | ᡝᠮᡠ: `wiasoom.com` API ᡨᡝᡳᠯᡝ |
| `process.env` ᠪᡝ **ᡝᠮᡠ ᡳᠨᡠ ᠪᠠᡳᡨᠠᠯᠠᡵᠠᡴᡡ** | ᡝᠨᡨᡝᡥᡝᠮᡝ ᠠᡴᡩᡠᠨ ᡩᠣᡵᡤᡳ ᠪᡳ |
| `require('fs')` ᠪᡝ **ᡝᠮᡠ ᡳᠨᡠ ᠪᠠᡳᡨᠠᠯᠠᡵᠠᡴᡡ** | `ctx.settings` ᡩᡝ ᡨᡝᠪᡠ, `ctx.sftp` ᡩᡝ ᡶᠠᡳᠯ ᡤᡡᡵᡳ |
| npm ᡨᡠᠯᡝᡵᡤᡳ ᠪᡠᠩᡴᠣ ᠪᡝ **ᡝᠮᡠ ᡳᠨᡠ ᠪᠠᡳᡨᠠᠯᠠᡵᠠᡴᡡ** | ᠰᡠᠯᡶᠠ JavaScript ᡨᡝᡳᠯᡝ — node_modules ᠠᡴᡡ |
| ᡤᡝᠮᡠ ᡤᠣᡵᠣᡴᡳ ᡶᠠᡶᡠᠨ ᡩᡝ `ctx.terminal.send()` ᠪᠠᡳᡨᠠᠯᠠᠮᡝ **ᠠᡴᡩᡠᠨ** | ᡝᡵᡝ ᠠᡴᡩᡠᠨ SSH ᡩᠣᡵᠣ ᡩᠠᠪᠠᠯᡳ ᠶᠠᠪᡠᠮᠪᡳ |
| `deactivate()` ᡩᡝ ᡤᡝᠮᡠ ᠪᠣᠯᠵᠣ **ᠠᡴᡩᡠᠨ** | ᡩᠣᠨᠵᡳᡵᡝ ᠨᠠᡴᠠ, ᡝᡵᡳᠨ ᠪᡝ ᡝᠨᡨᡝ |

### ᡝᠨᡩᡠᡵᡳ ᡩᠠᠰᠠᠮᡝ (Enduri Dasame — Error Handling)

ᡝᡵᡤᡝᠯᡝᠮᡝ ᠪᠠᡳᡨᠠ ᠪᡝ try/catch ᡩᡝ ᠮᠠᡴᡨᠠ:

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

### deactivate() ᡩᡝ ᠪᠣᠯᠵᠣᠮᡝ (deactivate() de boljome — Cleanup)

ᠰᡳᠨᡳ plugin ᡝᡵᡳᠨ, ᡩᠣᠨᠵᡳᡵᡝ, ᡝᠴᡳ ᡩᠣᠨᠵᡳᠮᡝ ᠠᡵᠠᡥᠠ ᠣᠴᡳ — ᡤᡝᠮᡠ ᠪᠣᠯᠵᠣ:

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

### i18n ᡩᡝᠮᡠᠨ (i18n demun — i18n Support)

WIA SOOM 254 ᡤᡳᠰᡠᠨ ᡩᡝᠮᡠᠮᠪᡳ᠃ ᠰᡳᠨᡳ plugin ᡳ ᡤᡳᠰᡠᠨ ᡤᡡᡵᡳᠮᡝ ᡝᡵᡝ ᡩᠣᡵᠣ ᠪᠠᡳᡨᠠᠯᠠ:

```javascript
// Simple multi-language labels
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

## ᠨᡳᠩᡤᡠᠴᡳ ᡶᠶᡝᠯᡝᠨ: ᠶᠠᡵᡤᡳᠶᠠᠨ ᡩᡠᡵᡳᠪᡠᠯᡝᠨ (Ningguci Fyelen: Yargiyan Duribulen)

### ᡩᡠᡵᡳᠪᡠᠯᡝᠨ 1: ᡶᡠᡩᠠᠰᡳ ᡳ ᡩᡳᠰᡴ ᡤᡳᠴᡳᡥᡳᠶᠠᡵᠠ (Duribulen 1: Fudasi i disk gicihiyara)

ᡤᠣᡵᠣᡴᡳ ᡶᡠᡩᠠᠰᡳ ᡩᡝ `df -h` ᠶᠠᠪᡠᡶᡳ, ᡶᡝᠵᡳᠯᡝ ᡩᡝ ᠪᠠᡳᡨᠠᠯᠠᡥᠠ/ᠰᡠᠯᡶᠠ ᡨᡠᠴᡳᠪᡠᠮᠪᡳ᠃ (Goroki fudasi de df -h yabufi, fejile de baitalaha/sulfa tucibumbi.)

```javascript
'use strict';

/**
 * Server Disk Checker — WIA SOOM Plugin
 *
 * Shows disk usage in the status bar.
 * Alerts when any partition exceeds 90%.
 */

var checkInterval = null;
var unsubscribers = [];

exports.activate = function activate(context) {
  // Add sidebar button to trigger manual check
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: 'Disk Check',
    onClick: function() {
      checkDisk(context);
    }
  });

  // Auto-check every 5 minutes
  var interval = context.settings.get('interval') || 300;
  checkInterval = setInterval(function() {
    checkDisk(context);
  }, interval * 1000);
};

function checkDisk(context) {
  var output = '';

  // Listen for terminal output
  var unsub = context.terminal.onOutput('current', function(data) {
    output += data;
  });
  unsubscribers.push(unsub);

  // Send the command
  context.terminal.send('current', "df -h / | tail -1 | awk '{print $5}'\n");

  // Parse after delay
  setTimeout(function() {
    unsub();

    // Extract percentage (e.g., "73%")
    var match = output.match(/(\d+)%/);
    if (match) {
      var percent = parseInt(match[1]);
      context.ui.addStatusBarItem('disk-usage', 'Disk: ' + percent + '%');

      // Alert if over 90%
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

### ᡩᡠᡵᡳᠪᡠᠯᡝᠨ 2: TODO ᡩᠠᠰᠠᡵᠠ (Duribulen 2: TODO dasara)

ᡝᠮᡠ plugin — TODO ᠮᡝᠶᡝᠨ ᠪᡝ ᡨᡝᠪᡠᡵᡝ ᡩᡝ ᡝᠨᡨᡝᡥᡝᠮᡝ ᡝᠵᡝ, webview ᡩᡝ ᡨᡠᠴᡳᠪᡠ᠃ (Emu plugin — TODO meyen be tebure de enteheme eje, webview de tucibu.)

> **ᡩᠣᡵᠣ ᡳ ᡤᡳᠰᡠᡵᡝᠨ (Doro i gisuren)::** Webview ᡩᡝ plugin API ᡩᡝ ᡩᠠᠰᡳᠮᡝ ᡥᡡᠯᠠᠮᡝ ᠮᡠᡨᡝᡵᠠᡴᡡ ᠣᡶᡳ, ᡝᡵᡝ plugin ᡝᠮᡠ "ᡨᡠᠸᠠᠮᡝ ᡩᡠᡵᡠᠨ" ᡩᠣᡵᠣ ᠪᠠᡳᡨᠠᠯᠠᠮᠪᡳ — ᡨᡝᠪᡠᡵᡝ ᠴᡳ TODO ᡥᡡᠯᠠᡶᡳ, ᡥᡡᠯᠠᠮᡝ ᡨᡝᡳᠯᡝ HTML ᡩᡝ ᡨᡠᠴᡳᠪᡠ, ᡩᡝᡵᡤᡳ ᡤᠠᠯᠠ ᡥᡝᡵᡤᡝᠨ ᡩᡝ ᠪᠠᡳᡨᠠ ᠰᡳᠨᡩᠠ᠃ Webview ᡨᡠᠸᠠᡵᠠ **ᡩᡠᡵᡠᠨ** ᡨᡝᡳᠯᡝ᠂ ᡳᠰᡳᠪᡠᡵᡝ ᡥᡡᠸᠠᠩᡤᠠ ᠸᠠᡴᠠ᠃ (Webview de plugin API de dasime hulame muteraku ofi, ere plugin emu "tuwame durun" doro baitalambi — tebure ci TODO hulafi, hulame teile HTML de tucibu, dergi gala hergen de baita sinda. Webview tuwara durun teile, isibure huwangga waka.)

```javascript
'use strict';

/**
 * TODO Manager — WIA SOOM Plugin
 *
 * Pattern: settings-driven display (no webview↔plugin bridge needed)
 */

exports.activate = function activate(context) {
  // Show current TODO count in status bar
  updateStatusBar(context);

  // Button 1: View TODO list
  context.ui.addSidebarButton({
    id: 'todo-view',
    icon: 'check-square',
    label: 'TODO List',
    onClick: function() {
      showTodoList(context);
    }
  });

  // Button 2: Quick-add a TODO via notification prompt
  context.ui.addSidebarButton({
    id: 'todo-add',
    icon: 'plus-square',
    label: 'Add TODO',
    onClick: function() {
      // Use terminal echo as a quick input method
      var todos = context.settings.get('todos') || [];
      var newItem = 'Task #' + (todos.length + 1) + ' — ' + new Date().toLocaleString();
      todos.push({ text: newItem, done: false, createdAt: new Date().toISOString() });
      context.settings.set('todos', todos);
      updateStatusBar(context);
      context.ui.showNotification('success', 'Added: ' + newItem);
    }
  });

  // Button 3: Clear completed TODOs
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

### ᡩᡠᡵᡳᠪᡠᠯᡝᠨ 3: ᡝᠨᡩᡠᡵᡳ ᡨᡠᠸᠠᡵᠠ (Duribulen 3: Enduri tuwara — Error Watcher)

ᡨᡝᡵᠮᡳᠨᠠᠯ ᡳ ᡨᡠᠴᡳᡥᡝ ᠪᡝ ᡨᡠᠸᠠᠮᠪᡳ, ᡝᠨᡩᡠᡵᡳ ᡩᠣᡵᠣ ᠰᠠᠪᡠᡥᠠ ᠣᠴᡳ ᡠᠯᡥᡳᠴᡝᠨ ᡨᡠᠴᡳᠪᡠᠮᠪᡳ᠃ (Terminal i tucihe be tuwambi, enduri doro sabuha oci ulhicen tucibumbi.)

```javascript
'use strict';

/**
 * Error Watcher — WIA SOOM Plugin
 *
 * Watches terminal output for error patterns.
 * Shows notification when errors are detected.
 * Configurable patterns via settings.
 */

var watchers = [];
var errorCount = 0;

// Default patterns to watch for
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
  context.ui.addStatusBarItem('error-watcher', 'Errors: ' + errorCount);

  // Watch current terminal
  var unsub = context.terminal.onOutput('current', function(data) {
    for (var i = 0; i < patterns.length; i++) {
      if (data.includes(patterns[i])) {
        errorCount++;
        context.ui.addStatusBarItem('error-watcher', 'Errors: ' + errorCount);
        context.ui.showNotification('error',
          'Error detected: "' + patterns[i] + '" found in terminal output'
        );
        // Save error log
        var log = context.settings.get('errorLog') || [];
        log.push({
          pattern: patterns[i],
          time: new Date().toISOString(),
          snippet: data.substring(0, 200)
        });
        // Keep last 100 errors
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

## ᡶᡠᠯᡠ: ᡥᠠᠴᡳᠨ ᠵᠠᡳ ᠨᡳᡵᡠᡤᠠᠨ ᠰᡠᡵᡝ (Fulu: Hacin jai Nirugan Sure — Categories & Icons)

### Plugin ᡥᠠᠴᡳᠨ (29) (Plugin Hacin)

ᠰᡳᠨᡳ package.json ᡳ `keywords` ᡝᠴᡳ ᡩᠠᠩᠰᡝ ᡩᡝ ᡠᠩᡤᡳᡵᡝ ᡩᡝ ᡝᡵᡝ ᠪᠠᡳᡨᠠᠯᠠ:

| ᡥᠠᠴᡳᠨ (Hacin) | ᡤᡳᠰᡠᡵᡝᠨ (Gisuren) |
|----------|-------------|
| `server` | ᡶᡠᡩᠠᠰᡳ ᡩᠠᠰᠠᡵᠠ (Fudasi dasara) |
| `devtools` | ᠠᡵᠠᡵᠠ ᠠᡤᡡᡵᠠ (Arara agura) |
| `calculator` | ᠪᠣᡩᠣᡵᠠ ᠵᠠᡳ ᡤᡡᡵᡳᡵᡝ (Bodora jai guriire) |
| `simulator` | ᡩᡠᡵᡳᠪᡠᡵᡝ (Duribure) |
| `game` | ᡝᡶᡳᠮᠪᡳ (Efimbi) |
| `business` | ᡥᡡᡩᠠ ᡳ ᠠᡤᡡᡵᠠ (Huda i agura) |
| `security` | ᠠᡴᡩᡠᠨ ᠵᠠᡳ ᡤᡳᠴᡳᡥᡳᠶᠠᡵᠠ (Akdun jai gicihiyara) |
| `web` | ᡨᠣᡵ ᡶᡠᡩᠠᠰᡳ ᡩᠠᠰᠠᡵᠠ (Tor fudasi dasara) |
| `education` | ᡨᠠᠴᡳᡵᡝ ᠠᡤᡡᡵᠠ (Tacire agura) |
| `health` | ᠪᡝᠶᡝ ᡳ ᠠᡤᡡᡵᠠ (Beye i agura) |
| `islamic` | ᡳᠰᠯᠠᠮ ᡳ ᠠᡤᡡᡵᠠ (Islam i agura) |
| `science` | ᡝᡵᡩᡝᠮᡠ ᠠᡤᡡᡵᠠ (Erdemu agura) |
| `quantum` | ᡴᠸᠠᠨᡨᡠᠮ ᠠᡤᡡᡵᠠ (Kwantum agura) |
| `ai` | AI ᠠᡤᡡᡵᠠ |
| `biotech` | ᡝᡵᡤᡝᠨ ᡳ ᡝᡵᡩᡝᠮᡠ ᠠᡤᡡᡵᠠ (Ergen i erdemu agura) |
| `space` | ᠠᠪᡴᠠ ᠵᠠᡳ ᡠᠰᡳᡥᠠ ᠠᡤᡡᡵᠠ (Abka jai usiha agura) |
| `network` | ᠵᠠᠯᡤᠠᠨ ᠠᡤᡡᡵᠠ (Jalgan agura) |
| `database` | ᠪᠠᡳᡨᠠ ᡳ ᡴᡠᠸᠠᡵᠠᠨ ᡩᠠᠰᠠᡵᠠ (Baita i kuwaran dasara) |
| `monitoring` | ᡶᡠᡩᠠᠰᡳ ᡨᡠᠸᠠᡵᠠ (Fudasi tuwara) |
| `devops` | DevOps ᠵᠠᡳ CI/CD |
| `utility` | ᡝᠯᡤᡳᠶᡝᠨ ᠠᡤᡡᡵᠠ (Elgiyen agura) |
| `design` | ᠨᡳᡵᡠᡵᡝ ᠠᡤᡡᡵᠠ (Nirure agura) |
| `ecommerce` | ᡥᡡᡩᠠᠰᠠᡵᠠ ᠠᡤᡡᡵᠠ (Hudasara agura) |
| `automation` | ᠪᡝᠶᡝ ᡩᡝᡵᡳᠪᡠᡵᡝ ᠠᡤᡡᡵᠠ (Beye deribure agura) |
| `kpop` | K-pop ᡳ ᠠᡤᡡᡵᠠ |
| `accessibility` | ᡝᠯᡤᡳᠶᡝᠨ ᠪᠠᡳᡨᠠᠯᠠᡵᠠ ᠠᡤᡡᡵᠠ (Elgiyen baitalara agura) |
| `analytics` | ᡤᡳᠴᡳᡥᡳᠶᠠᡵᠠ ᠵᠠᡳ ᡝᠵᡝᠮᡝ (Gicihiyara jai ejeme) |
| `wia` | WIA ᡳ ᠠᡤᡡᡵᠠ |
| `all` | ᡤᡝᠮᡠ ᡥᠠᠴᡳᠨ ᡩᡝ ᡨᡠᠴᡳᠮᠪᡳ (Gemu hacin de tucimbi) |

### ᡩᡠᡵᡳᠪᡠᠯᡝᠨ ᠨᡳᡵᡠᡤᠠᠨ ᠰᡠᡵᡝ (Duribulen Nirugan Sure — Lucide)

| ᠨᡳᡵᡠᡤᠠᠨ ᡤᡝᠪᡠ (Nirugan Gebu) | ᠪᠠᡳᡨᠠᠯᠠᡵᠠ (Baitalara) |
|-----------|---------|
| `server` | ᡶᡠᡩᠠᠰᡳ ᡩᠠᠰᠠᡵᠠ (Fudasi dasara) |
| `shield` | ᠠᡴᡩᡠᠨ (Akdun) |
| `database` | ᠪᠠᡳᡨᠠ ᡳ ᡴᡠᠸᠠᡵᠠᠨ (Baita i kuwaran) |
| `activity` | ᡨᡠᠸᠠᡵᠠ (Tuwara) |
| `terminal` | ᡨᡝᡵᠮᡳᠨᠠᠯ ᠠᡤᡡᡵᠠ (Terminal agura) |
| `code` | ᠠᡵᠠᡵᠠ (Arara) |
| `hard-drive` | ᡩᡳᠰᡴ/ᡨᡝᠪᡠᡵᡝ (Disk/tebure) |
| `network` | ᠵᠠᠯᡤᠠᠨ (Jalgan) |
| `lock` | ᡥᡝᡵᡤᡝᠨ ᠠᡴᡩᡠᠨ (Hergen akdun) |
| `eye` | ᡨᡠᠸᠠᡵᠠ (Tuwara) |
| `check-square` | ᠪᠠᡳᡨᠠ/TODO |
| `layout-dashboard` | ᡩᠠᠰᡥᡳᠪᠣᡵᡩ (Dashboard) |
| `settings` | ᡩᠠᠰᠠᡵᠠ (Dasara) |
| `zap` | ᠪᡝᠶᡝ ᡩᡝᡵᡳᠪᡠᡵᡝ (Beye deribure) |
| `globe` | ᡨᠣᡵ/ᡤᡠᡵᡠᠨ (Tor/gurun) |

ᡤᡝᠮᡠ 1,500+ ᠨᡳᡵᡠᡤᠠᠨ ᠰᡠᡵᡝ: [lucide.dev/icons](https://lucide.dev/icons)

---

## ᠠᡳᠰᡳᠯᠠᠮᡝ ᠪᠠᡳᡥᠠᠣ? (Aisilame baihao? — Need Help?)

- **GitHub ᡩᡝ ᡶᡝᡩᡝᡵᡝᡴᡠ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin ᡩᡝ ᡶᡝᡩᡝᡵᡝᡴᡠ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ᡩᡠᡵᡳᠪᡠᠯᡝᠨ Plugin:** [Website](https://wiasoom.com)
- **ᡨᠣᡵ ᡶᡠᡩᠠᠰᡳ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ᡤᡝᡵᡝᠩᡤᡝ ᠪᠠᡳᡨᠠ ᠠᡵᠠ᠃ ᡤᡝᠮᡠ ᠨᡳᠶᠠᠯᠮᠠ ᡩᡝ ᠰᡝᠯᡤᡳᠶᡝ᠃ (Gerengge baita ara. Gemu niyalma de selgiye.)</em></p>
<p align="center"><em>— WIA SOOM ᡳ ᡴᡠᠸᠠᡵᠠᠨ (WIA SOOM i Kuwaran)</em></p>
