<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Si wo plugin dea wɔ miniti 5 mu.</strong></p>
<p align="center">Bɔ abatoɔ a ɛyɛ den, dashboards, ne automations — pɛpɛɛpɛ wɔ WIA SOOM mu.</p>

---

## Table of Contents

- [Part 1: Quick Start — Wo Mmerɛ Nkyerɛkyerɛ Mu Plugin wɔ miniti 5 mu](#part-1-quick-start--wo-mmerɛ-nkyerɛkyerɛ-mu-plugin-wɔ-miniti-5-mu)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Bɔ Custom UI a ɛyɛ Fɛ wɔ Webviews Mu](#part-3-bɔ-custom-ui-a-ɛyɛ-fɛ-wɔ-webviews-mu)
- [Part 4: Pɛ Wo Plugin](#part-4-pɛ-wo-plugin)
- [Part 5: Nsɛm Pa](#part-5-nsɛm-pa)
- [Part 6: Amansan Nsɛm](#part-6-amansan-nsɛm)
- [Appendix: Abatoɔ ne Icons](#appendix-abatoɔ-ne-icons)

---

## Part 1: Quick Start — Wo Mmerɛ Nkyerɛkyerɛ Mu Plugin wɔ miniti 5 mu

### Dɛn na wopɛ bɛyɛ

"Hello World" plugin a ɛde abatoɔ bɛka sidebar ho. Sɛ wopɛ a, ɛbɛda nsɛm a ɛyɛ fɛ adi.

### Ɛkwan 1: Bɔ plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Ɛkwan 2: Bɔ package.json
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
**Nsɛm a ɛho hia:** `name`, `version`, `description`, `author`, `main`

### Ɛkwan 3: Bɔ index.js
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
### Ɛkwan 4: San hyɛ WIA SOOM mu

San hyɛ app no (anaa fa plugin no to so/si so wɔ Settings → Plugins).

Wobɛhunu **"Hello World"** abatoɔ wɔ sidebar no mu. Pɛ so — wobɛhunu nsɛm a ɛyɛ fɛ!

### Ɛyɛ dɛn na ɛyɛ adwuma
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

Sɛ wopɛ a, `activate(context)` function no bɛfrɛ, `context` (anaa `ctx`) bɛma saa APIs yi:
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

### `ctx.terminal` — Fa commands yɛ adwuma wɔ remote servers so

#### `terminal.send(sessionId, data)`

Fa command (anaa nsɛm biara) kɔ active terminal session mu.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal session a wopɛ sɛ wode kɔ |
| `data` | `string` | Command anaa nsɛm a wopɛ sɛ wode kɔ |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Bɔ mmɔden sɛ wopɛ nsɛm a ɛba fi terminal session mu. Bɛma **unsubscribe function**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal session a wopɛ sɛ wopɛ nsɛm fi mu |
| `callback` | `(data: string) => void` | Bɛfrɛ no wɔ nsɛm a ɛba mu biara |
| **Returns** | `() => void` | Frɛ eyi sɛ wopɛ sɛ wopɛ nsɛm fi mu |
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
**Nsɛm a ɛho hia:** Da so bɔ mmɔden sɛ wopɛ unsubscribe function no na frɛ no wɔ `deactivate()` mu sɛnea ɛbɛyɛ a ɛrenyɛ nsɛm a ɛda ho.

---

### `ctx.sftp` — File transfer

> **Status: Ɛbɛba Ntɛm** — SFTP API no yɛ a wɔakyerɛ, nanso ɛnyɛ a wɔde akɔ app no SFTP engine mu. `list()` deɛ, ɛbɛma empty array, na `upload()`/`download()` yɛ no-ops. Eyi bɛyɛ a wɔbɛyɛ no nyinaa wɔ ɔman foforɔ mu. Seesei, fa `ctx.terminal.send()` ne `scp` anaa `rsync` commands yɛ adwuma.

#### `sftp.list(sessionId, path)`

Kyerɛ fael a ɛwɔ remote directory mu.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Fa fael fi local machine kɔ remote server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Fa fael fi remote server kɔ local machine.
§§§CHUNK_SEPARATOR��§§
**Adwuma a ɛyɛ adwuma (kɔsi SFTP API no yɛ adwuma):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Fa abatoɔ bɛka WIA SOOM sidebar ho.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Da | Unique ID (yɛ default yɛ plugin din) |
| `icon` | `string` | Yɛ | Lucide icon din (sɛ e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yɛ | Abatoɔ nsɛm a ɛda sidebar mu |
| `onClick` | `() => void` | Yɛ | Function a wɔfrɛ no sɛ abatoɔ no yɛ adwuma |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Icon nsɛm:** Kɔ [lucide.dev/icons](https://lucide.dev/icons) na hwehwɛ icons a ɛwɔ hɔ.

> **Nsɛm a ɛho hia:** Nnɛ plugins bi yɛ positional arguments te sɛ `addSidebarButton(id, icon, label, onClick)`. Official API no de **options object** yɛ adwuma sɛnea wɔakyerɛ no wɔ soro. Da so yɛ object style de yɛ plugins foforɔ.

#### `ui.openWebview(options)`

Bue popup window a ɛwɔ custom HTML nsɛm mu. Eyi ne ɔkwan a wopɛ sɛ woyɛ rich UIs. 

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML nsɛm a wɔbɛda adi |
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
> Hwɛ [Part 3](#part-3-building-custom-ui-with-webviews) ma nsɛm a ɛfa webview ho a ɛyɛ akwan a ɛyɛ den.

#### `ui.showNotification(type, message)`

Kyerɛ toast nsɛm a ɛda ho adi.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Nsɛm a ɛda ho adi |
| `message` | `string` | Nsɛm a ɛbɛda ho adi |
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
#### `ui.addStatusBarItem(id, text)`

Ka nsɛm a ɛyɛ da ho adi kɔ nsɛm a ɛda so no mu.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Nkyerɛmu a ɛyɛ pɛ ma nsɛm yi |
| `text` | `string` | Nsɛm a ɛbɛda ho adi |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Nsɛm a ɛda ho adi

Plugin nsɛm no da ho adi wɔ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Kenkan nsɛm a wɔakɔda ho adi.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Sɛ key no nni hɔ a, ɛbɛsan `undefined`.

#### `settings.set(key, value)`

Da nsɛm a ɛda ho adi. Ɛyɛ a ɛyɛ mmerɛ ne nsɛm, nɔma, booleans, arrays, ne objects.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Nkyerɛkyerɛ: Ka ɔyarefoɔ pɛsɛmenkomenya ho nsɛm**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — AI ntam

> **Nsɛm a ɛda ho adi: Ɛbɛba ntɛm** — AI API no da ho adi nanso ɛnyɛ a wɔakɔda ho adi kɔ Soomy. Ɛyɛ a ɛbɛsan `{ response: 'AI not yet connected' }`. AI ntam a ɛyɛ pɛ no yɛ abɔdin a ɛda ho adi wɔ ɔman a ɛbɛba.

#### `ai.chat(messages, options?)`

Twerɛ nsɛm kɔ AI ɔyarefoɔ (Soomy).
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

## Part 3: Sɛnea Wobɛyɛ Custom UI a ɛda ho adi wɔ Webviews mu

`openWebview()` API no ma wo tumi yɛ dashboard UIs a ɛda ho adi wɔ HTML, CSS, ne JavaScript mu — nyinaa wɔ popup mfoni mu.

> **Nsɛm a ɛda ho adi:** Webviews yɛ **da ho adi pɛ**. Wɔn ntumi mfa nsɛm a ɛda ho adi kɔ plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Fa sidebar buttons yɛ nsɛm a ɔyarefoɔ yɛ, na fa `openWebview()` da nsɛm a ɛda ho adi no ho adi. Sɛ wopɛ nsɛm a ɛyɛ mmerɛ a, fa sidebar buttons yɛ wɔn na san bue webview no bio sɛnea ɛbɛyɛ a nsɛm a ɛda ho adi no bɛyɛ foforɔ.

### Nsɛm: Terminal Command → Parse Output → Da ho adi wɔ HTML

Eyi ne plugin nsɛm a ɛyɛ kɛse. Wopɛ sɛ woyɛ abatoɔ, kenkan nsɛm a ɛda ho adi, na da ho adi wɔ ɔkwan a ɛyɛ mmerɛ.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Nsɛm: Interactive Dashboard a ɛda ho adi kɔ so
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
### Nsɛm: Da Nsɛm a ɛda ho adi wɔ Webview mu

> **Nsɛm:** Webviews yɛ da ho adi pɛ — wɔn ntumi mfa nsɛm a ɛda ho adi kɔ plugin APIs. Fa `ctx.settings` yɛ wo sidebar button handlers mu sɛnea ɛbɛyɛ a wubetumi asesa nsɛm no, na fa `openWebview()` da nsɛm a ɛda ho adi no ho adi.
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
---

## Part 4: Sɛnea Wobɛyɛ Wo Plugin a ɛda ho adi

### Nsɛm 1: Sɛnea Wobɛyɛ Adwuma wɔ Fie

1. Kɔpɛ wo plugin kɔ `~/.wia-soom/plugins/{your-plugin}/`
2. San bue WIA SOOM
3. Sɛnea ɛda ho adi: sidebar button no bɛda hɔ, nsɛm no bɛyɛ nokware
4. Sɛnea ɛda ho adi: dɛn na ɛbɛyɛ sɛ terminal nni hɔ?

### Nsɛm 2: Sɛnea Wobɛyɛ Adwuma a ɛda ho adi

Wo plugin folder no bɛyɛ a ɛwɔ:
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
**Nsɛm a ɛho hia `package.json` nsɛm:**

| Nsɛm | Nsɛm a ɛkyerɛ | Nsɛm a ɛyɛ ɔkwan |
|-------|-------------|---------|
| `name` | Nkyerɛw a ɛyɛ kebab-case a ɛyɛ pɛ | `"my-awesome-plugin"` |
| `version` | Nsɛm a ɛda ho adi | `"1.0.0"` |
| `description` | Nsɛm koro a ɛda ho adi | `"Monitors nginx access logs in real-time"` |
| `author` | Wo din | `"John Doe"` |
| `main` | Ɛkwan a ɛda ho adi | `"index.js"` |

**Nsɛm a ɛyɛ ɔkwan:**

| Nsɛm | Nsɛm a ɛkyerɛ |
|-------|-------------|
| `license` | Nhyɛso a ɛyɛ (MIT na ɛyɛ a ɛda ho adi) |
| `keywords` | Nkyekyɛm a ɛyɛ nsɛm a wɔhwehwɛ |
| `soom.minVersion` | WIA SOOM nsɛm a ɛyɛ a ɛho hia |

### Nsɛm 3: Kɔma Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Ka** wo plugin kɔ `plugins/{wo-plugin-din}/`
3. **Kɔma** Pull Request

### Nsɛm 4: Nsɛm a wɔhwɛ ne ho

Yɛhwɛ plugin biara a ɛyɛ:

- **Sɛkuriti** — mfa APIs a ɛyɛ ɔhaw (hwɛ [Sɛkuriti Nsɛm](#security-rules))
- **Quality** — ɛyɛ adwuma anaa? Ɛyɛ kɔd a ɛyɛ fɛ?
- **Mfaso** — ɛyɛ ɔhaw a ɛyɛ nokware anaa?

Afei a wɔpɛ:
1. Wo plugin bɛka ho kɔ `registry.json`
2. ZIP bundle bɛyɛ wɔ `dist/`
3. Wo plugin bɛda **Plugin Store** mu ma WIA SOOM abatoɔfoɔ nyinaa!

---

## Abatoɔ 5: Nsɛm a ɛyɛ fɛ

### Sɛkuriti Nsɛm

Nsɛm yi yɛ **pɔtee**. Plugins a wɔbɛyɛ ɔhaw bɛyɛ abɔne.

| Nsɛm | Adɛn |
|------|-----|
| **MFA** mfa `eval()` anaa `new Function()` | Kɔd a ɛyɛ ɔhaw |
| **MFA** mfa `child_process`, `exec()`, `spawn()` | Fa `ctx.terminal.send()` nko ara yɛ adwuma |
| **MFA** mfa nsɛm a ɛyɛ abatoɔ a ɛyɛ ɔhaw | Nsɛm a ɛyɛ ɔhaw: `wiasoom.com` API endpoints |
| **MFA** mfa `process.env` | Nsɛm a ɛyɛ ɔhaw betumi abɔ nsɛm a ɛyɛ ɔhaw |
| **MFA** mfa `require('fs')` pɔtee | Fa `ctx.settings` yɛ adwuma, `ctx.sftp` fa yɛ fael transfer |
| **MFA** mfa npm nsɛm a ɛyɛ abatoɔ | JavaScript pɔtee nko — mfa node_modules |
| **SƐ** fa `ctx.terminal.send()` yɛ adwuma ma nsɛm a ɛyɛ abatoɔ nyinaa | Eyi bɛyɛ fa sɛkuriti SSH channel mu |
| **SƐ** yɛ adwuma wɔ `deactivate()` mu | Yi listeners, to intervals mu |

### Nsɛm a ɛyɛ ɔhaw

Da biara yɛ nsɛm a ɛyɛ ɔhaw wɔ try/catch mu:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Yi mu wɔ deactivate()

Sɛ wo plugin yɛ intervals, listeners, anaa subscriptions — yi mu:
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
### i18n Mmoa

WIA SOOM yɛ nsɛm 254. Sɛ wopɛ sɛ wo plugin nsɛm yɛ a wɔtumi yɛ nsɛm a ɛyɛ ɔhaw, fa ɔkwan a ɛyɛ mmerɛw:
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
---

## Abatoɔ 6: Nsɛm a ɛyɛ nokware

### Nsɛm a ɛyɛ 1: Server Disk Checker

Yɛ `df -h` wɔ server a ɛyɛ abatoɔ no so na ɛda ho adi sɛnea wɔde yɛ adwuma wɔ status bar mu.
§§§CHUNK_SEPARATOR§§§
---

### Nsɛm a ɛyɛ 2: TODO Manager

Plugin a ɛyɛ TODO list a ɔde nsɛm a ɛyɛ abatoɔ yɛ adwuma na ɔde webview yɛ adwuma.

> **Nsɛm a ɛyɛ fɛ:** Sɛ webviews ntumi mfa plugin APIs nkyerɛ, plugin yi de "snapshot" ɔkwan — ɔkenkan TODOs fi nsɛm a ɛyɛ abatoɔ mu, na ɔda no adi sɛ HTML a ɛyɛ a wɔntumi mfa nkyerɛ, na ɔde sidebar-based nsɛm yɛ adwuma ma ka ho. Webview yɛ **da ho adi** layer, ɛnyɛ ɔkwan a wɔde yɛ adwuma.
§§§CHUNK_SEPARATOR§§§
---

### Nsɛm a ɛyɛ 3: Error Watcher

Yɛhwɛ terminal nsɛm na yɛde nsɛm a ɛyɛ ɔhaw bɔ nkɔmɔ sɛ nsɛm a ɛyɛ ɔhaw biara a wɔda ho adi.
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

## Appendix: Categories & Icons

### Plugin Categories (29)

Fa eyi yɛ wo `package.json` `keywords` anaa sɛ wopɛ sɛ wode kɔ registry no mu:

| Category | Description |
|----------|-------------|
| `server` | Abatoɔ a ɛfa server ho |
| `devtools` | Nkyerɛkyerɛ a ɛfa nsɛm a wɔyɛ ho |
| `calculator` | Kɔkɔbɔ ne nsɛm a wɔyɛ ho |
| `simulator` | Simulators |
| `game` | Terminal agorɔ |
| `business` | Abatoɔ a ɛfa adwumayɛ ho |
| `security` | Abatoɔ a ɛfa ɔhaw ne nsɛm a wɔyɛ ho |
| `web` | Web server abatoɔ |
| `education` | Nkyerɛkyerɛ abatoɔ |
| `health` | Abatoɔ a ɛfa apɔwmuden ho |
| `islamic` | Abatoɔ a ɛfa Islam ho (mpaebɔ bere, ne nsɛm a ɛfa ho) |
| `science` | Abatoɔ a ɛfa nsɛm a ɛyɛ nokware ho |
| `quantum` | Abatoɔ a ɛfa quantum computing ho |
| `ai` | Abatoɔ a ɛyɛ AI dea |
| `biotech` | Abatoɔ a ɛfa biotechnology ho |
| `space` | Abatoɔ a ɛfa ɔsoro ne nsɛm a ɛfa ho |
| `network` | Abatoɔ a ɛfa network ho |
| `database` | Database abatoɔ |
| `monitoring` | Server nsɛm a wɔhwɛ |
| `devops` | DevOps ne CI/CD |
| `utility` | Abatoɔ a ɛyɛ nokware |
| `design` | Nkyerɛkyerɛ abatoɔ |
| `ecommerce` | E-commerce abatoɔ |
| `automation` | Abatoɔ a ɛfa ɔkwan a wɔyɛ nsɛm ho |
| `kpop` | K-pop ho abatoɔ |
| `accessibility` | Abatoɔ a ɛfa ɔkwan a wɔyɛ nsɛm ho |
| `analytics` | Nsɛm a wɔyɛ ho ne nsɛm a wɔkɔ so |
| `wia` | WIA ecosystem abatoɔ |
| `all` | Ɛda ho adi wɔ abatoɔ nyinaa mu |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Server abatoɔ |
| `shield` | Abatoɔ a ɛfa ɔhaw ho |
| `database` | Database |
| `activity` | Nsɛm a wɔhwɛ |
| `terminal` | Terminal nkyerɛkyerɛ |
| `code` | Nkyerɛkyerɛ |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Hwɛ/nhwehwɛmu |
| `check-square` | Nsɛm/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Nkyerɛkyerɛ |
| `zap` | Abatoɔ a ɛfa ɔkwan a wɔyɛ nsɛm ho |
| `globe` | Web/ɔman a ɛyɛ foforɔ |

Hwɛ nyinaa 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Yɛyɛ biribi a ɛyɛ fɛ. Kyɛ no ma wiase.</em></p>
<p align="center"><em>— WIA SOOM Team</em></p>
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
