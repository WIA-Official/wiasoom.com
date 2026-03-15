<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Kaiwhakahaere Tūtohu</h1>
<p align="center"><strong>Whakawhanakehia tō ake tūtohu i roto i te 5 meneti.</strong></p>
<p align="center>Whakahaerehia ngā taputapu tūmau, ngā papamahi, me ngā whakautu — i roto i te WIA SOOM.</p>

---

## Rārangi Ihirangi

- [Wāhanga 1: Tiimata Tere — Tō Tūtohu Tuatahi i roto i te 5 Meneti](#wāhanga-1-tiimata-tere--tō-tūtohu-tuatahi-i-roto-i-te-5-meneti)
- [Wāhanga 2: Tūtohu Context API Tūtohu](#wāhanga-2-tūtohu-context-api-tūtohu)
  - [ctx.terminal](#ctxterminal--whakahaere-komanga-i-ngā-tūmau-ā-roto)
  - [ctx.sftp](#ctxsftp--whakawhiti-kōnae)
  - [ctx.ui](#ctxui--whakaaturanga-kaiwhakamahi)
  - [ctx.settings](#ctxsettings--pūmanawa-pūmau)
  - [ctx.ai](#ctxai--whakawhitinga-ai)
- [Wāhanga 3: Te Whakapakari i te UI Ritenga me ngā Webviews](#wāhanga-3-te-whakapakari-i-te-ui-ritenga-me-ngā-webviews)
- [Wāhanga 4: Te Pāhotanga o Tō Tūtohu](#wāhanga-4-te-pāhotanga-o-tō-tūtohu)
- [Wāhanga 5: Ngā Whakaaro Pai](#wāhanga-5-ngā-whakaaro-pai)
- [Wāhanga 6: Ngā Tauira Pātea](#wāhanga-6-ngā-tauira-pātea)
- [Appendix: Ngā Kōwae & Ngā Ikona](#appendix-ngā-kōwae--ngā-ikona)

---

## Wāhanga 1: Tiimata Tere — Tō Tūtohu Tuatahi i roto i te 5 Meneti

### He aha tāu e hanga

He tūtohu "Kia ora te Ao" e tāpiri ana i tētahi pātene ki te taha. Ka pā ki a ia, ka whakaatu i tētahi pānui.

### Hipanga 1: Hangaia te kōpaki tūtohu
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Hipanga 2: Hangaia te package.json
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
**Ngā pātea e hiahiatia ana:** `name`, `version`, `description`, `author`, `main`

### Hipanga 3: Hangaia te index.js
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
### Hipanga 4: Whakahouhia te WIA SOOM

Whakahouhia te taupānga (nāna i pānui te tūtohu i runga i ngā Tautuhinga → Ngā Tūtohu).

Me kite koe i tētahi pātene **"Kia ora te Ao"** i te taha. Pāhia — ka kite koe i tētahi pānui angitu!

### Me pēhea te mahi
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

## Wāhanga 2: Tūtohu Context API Tūtohu

I te wā e karangahia ana tō `activate(context)` mahi, ka tuku mai te `context` (nāna i `ctx`) i ēnei API:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Whakaharaitia ngā komanga i ngā tūmau ā-roto

#### `terminal.send(sessionId, data)`

Tukua he komanga (nāna i tetahi raraunga) ki tētahi wāhanga terminal active.

| Pātea | Momo | Whakaahuatanga |
|-------|------|----------------|
| `sessionId` | `string` | Te wāhanga terminal hei tuku ki |
| `data` | `string` | Te komanga, te raraunga hei tuku |
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
#### `terminal.onOutput(sessionId, callback)`

Rēhita ki ngā putanga katoa mai i tētahi wāhanga terminal. Ka hoki mai he **mahi kāore e rēhita**.

| Pātea | Momo | Whakaahuatanga |
|-------|------|----------------|
| `sessionId` | `string` | Te wāhanga terminal hei tirohia |
| `callback` | `(data: string) => void` | Ka karangahia i ngā kōpaki putanga |
| **Ka hoki mai** | `() => void` | Karangahia tēnei hei mutu te whakarongo |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**He mea nui:** Me tiaki tonu te mahi kāore e rēhita, ā, karangahia i roto i te `deactivate()` hei aukati i ngā raru mahara.

---

### `ctx.sftp` — Whakawhiti kōnae

> **Tūnga: Ka haere mai** — Kua whakatakotoria te SFTP API engari kāore i te hono ki te miihini SFTP o te taupānga. Ko te `list()` e hoki mai ana i tētahi rārangi pūāhua, ā, ko te `upload()`/`download()` he mahi kāore. Ka whakatutukihia tēnei i te putanga e whai ake nei. I tēnei wā, whakamahia te `ctx.terminal.send()` me ngā komanga `scp` rānei `rsync` hei whakakī.

#### `sftp.list(sessionId, path)`

Rārangi kōnae i tētahi kōpaki ā-roto.
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
#### `sftp.upload(sessionId, localPath, remotePath)`

Tuku kōnae mai i te rorohiko kāinga ki te tūmau ā-roto.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

Tīkitia he kōnae mai i te tūmau ā-roto ki te rorohiko kāinga.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**Whakakī (ākuanei ka noho te SFTP API i runga):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — Whakaaturanga kaiwhakamahi

#### `ui.addSidebarButton(options)`

Tāpiri i tētahi pātene ki te taha WIA SOOM.

| Kōwhiringa | Momo | E hiahiatia ana | Whakaahuatanga |
|------------|------|----------------|----------------|
| `id` | `string` | Kāore | ID motuhake (ka noho hei ingoa tūtohu) |
| `icon` | `string` | Āe | Ingoa ikona Lucide (hei tauira, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Āe | Te tuhinga pātene e kitea ana i te taha |
| `onClick` | `() => void` | Āe | Te mahi ka karangahia i te pātene ka pāhia |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Tūtohu ikona:** Tirohia ngā ikona e wātea ana i [lucide.dev/icons](https://lucide.dev/icons)

> **Tūtohu pātea:** Ko ētahi tūtohu tawhito e whakamahi ana i ngā kōwhiringa tuhinga pēnei i te `addSidebarButton(id, icon, label, onClick)`. Ko te API mana e whakamahi ana i tētahi **rōpū kōwhiringa** e pēnei ana i te tuhinga i runga. Me whakamahi tonu te āhua rōpū mō ngā tūtohu hou.

#### `ui.openWebview(options)`

Tīmatahia he matapihi pop-up me ngā ihirangi HTML ritenga. Ko tēnei te huarahi ki te hanga i ngā UI whaihua.

| Kōwhiringa | Momo | Whakaahuatanga |
|------------|------|----------------|
| `title` | `string` | Te taitara matapihi |
| `html` | `string` | Te ihirangi HTML katoa hei whakaatu |
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
> Tirohia te [Wāhanga 3](#part-3-building-custom-ui-with-webviews) mō ngā tauira paetukutuku matatau.

#### `ui.showNotification(type, message)`

Whakaatu he pānui toast.

| Tūtohu | Momo | Whakamārama |
|--------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Te āhua pānui |
| `message` | `string` | Te tuhinga hei whakaatu |
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

Tāpirihia he tuhinga pātea ki te pae teitei o raro.

| Tūtohu | Momo | Whakamārama |
|--------|------|-------------|
| `id` | `string` | ID motuhake mō tēnei tuhinga pātea |
| `text` | `string` | Te tuhinga hei whakaatu |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Te penapena mau

Ka penapenahia ngā tautuhinga pūtirotiro i roto i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Pānuihia he uara kua tiakina.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Ka hoki `undefined` mēnā kāore te key e noho.

#### `settings.set(key, value)`

Tiakina he uara. Ka tautoko i ngā tuhinga, tau, boolean, rārangi, me ngā mea.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Tauira: Maumahara ki ngā whakaritenga o te kaiwhakamahi**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — Te whakauru AI

> **Tūnga: Ka haere mai** — Kua whakatakotoria te AI API engari kāore anō i hono ki Soomy. I tēnei wā ka hoki `{ response: 'AI not yet connected' }`. E whakamaheretia ana te whakauru AI ki tētahi putanga ā muri ake.

#### `ai.chat(messages, options?)`

Tukuhia ngā karere ki te kaiāwhina AI (Soomy).
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

## Wāhanga 3: Te hanga UI Ritenga me ngā Paetukutuku

Ka taea e te API `openWebview()` te hanga i ngā UI papamahi me HTML, CSS, me JavaScript — i roto i tētahi matapihi pop-up.

> **Tūtohu nui:** Ko ngā paetukutuku he **whakaaturanga anake**. Kāore e taea te karanga ki ngā API pūtirotiro (`ctx.settings`, `ctx.terminal`, etc.). Whakamahia ngā pātene taha mō ngā mahi katoa a te kaiwhakamahi, ā, whakamahia te `openWebview()` hei whakaatu i te āhua o nāianei. Mēnā e hiahia ana koe ki ngā āhuatanga whakawhitiwhiti, whakaohohia ēnā mai i ngā pātene taha, ā, ka rehita anō te paetukutuku hei whakahou i te whakaaturanga.

### Tauira: Tono Terminal → Tātari i te Putanga → Whakaatu i roto i te HTML

Koinei te tauira pūtirotiro tino noa. Ka whakahaere koe i tētahi tono, ka tātari i te hua, ā, ka whakaatu i te āhua.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Tauira: Papamahi Interaktīva me te Auautanga Aunoa
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
### Tauira: Whakaatu i ngā Tautuhinga i roto i te Paetukutuku

> **Tēnā:** Ko ngā paetukutuku he whakaaturanga anake — kāore e taea te karanga ki ngā API pūtirotiro. Whakamahia te `ctx.settings` i roto i ngā kaiwhakahaere pātene taha hei whakarereke i ngā tautuhinga, ā, whakamahia te `openWebview()` hei whakaatu i te āhua o nāianei.
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

## Wāhanga 4: Te whakaputa i tō Pūtirotiro

### Hipanga 1: Whakamātauria i te rohe

1. Kōwhiria tō pūtirotiro ki `~/.wia-soom/plugins/{your-plugin}/`
2. Whakahouhia te WIA SOOM
3. Tirohia mēnā e mahi ana: ka puta te pātene taha, e mahi ana ngā āhuatanga tika
4. Whakamātauria ngā take pito: he aha ka tupu mēnā kāore he terminal e hono ana?

### Hipanga 2: Whakaritea mō te tuku

Me whai i tō kōpaki pūtirotiro:
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
**Ngā wāhanga e hiahiatia ana i te `package.json`:**

| Wāhanga | Whakamārama | Tauira |
|---------|--------------|--------|
| `name` | ID motuhake i te kebab-case | `"my-awesome-plugin"` |
| `version` | Putanga semantic | `"1.0.0"` |
| `description` | Whakamārama kotahi te rārangi | `"Monitors nginx access logs in real-time"` |
| `author` | Tō ingoa | `"John Doe"` |
| `main` | Tūāpapa | `"index.js"` |

**Ngā wāhanga kōwhiri:**

| Wāhanga | Whakamārama |
|---------|--------------|
| `license` | Te momo raihana (MIT te tūtohu) |
| `keywords` | Rārangi o ngā tohu rapu |
| `soom.minVersion` | Te putanga WIA SOOM iti rawa e hiahiatia ana |

### Hipanga 3: Tuku ki te Rēhita Plugin

1. ****Package** your plugin as a ZIP file
2. **Tāpirihia** tō plugin ki `plugins/{tō-ingoa-plugin}/`
3. **Tuku** he Pull Request

### Hipanga 4: Arotake me te whakaaetanga

Ka arotake mātou i ia plugin mo:

- **Haumaru** — kāore he API mōrearea (titirohia [Ngā Ture Haumaru](#security-rules))
- **Kōwhiringa** — kei te mahi? He ma te waehere?
- **Te whaihua** — kei te whakatika i tētahi raru pono?

I muri i te whakaaetanga:
1. Ka tāpirihia tō plugin ki `registry.json`
2. Ka hangaia he ZIP pūrua i `dist/`
3. Ka puta tō plugin i te **Plugin Store** mō ngā kaiwhakamahi WIA SOOM katoa!

---

## Wāhanga 5: Ngā Tikanga Pai

### Ngā Ture Haumaru

Ko ēnei ture he **whakaritenga**. Ka whakakorehia ngā plugins e pāhekeheke ana.

| Ture | He aha |
|------|--------|
| **KĀORE** e whakamahi i te `eval()` rānei i te `new Function()` | Te mōrearea o te whakauru waehere |
| **KĀORE** e whakamahi i te `child_process`, `exec()`, `spawn()` | Whakamahia anake te `ctx.terminal.send()` mō ngā whakahau |
| **KĀORE** e tango i ngā URL o waho | Te whakatūpato: ngā pūnaha API o `wiasoom.com` |
| **KĀORE** e uru ki te `process.env` | Ka taea e ngā taurangi taiao te pupuri i ngā mea ngaro |
| **KĀORE** e whakamahi i te `require('fs')` i te tika | Whakamahia te `ctx.settings` mō te penapena, te `ctx.sftp` mō te whakawhiti kōnae |
| **KĀORE** e whakamahi i ngā pakeke o waho npm | He JavaScript pure anake — kāore he node_modules |
| **ME** whakamahi i te `ctx.terminal.send()` mō ngā whakahau mamao katoa | Ka haere tēnei i roto i te huarahi SSH haumaru |
| **ME** whakakore i roto i te `deactivate()` | Tango i ngā kaiwhakarongo, māmā i ngā wāhanga |

### Te Whakahaere Hē

Kia mau tonu te whakakī i ngā mahi mōrearea i roto i te try/catch:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Te Whakakore i roto i te deactivate()

Mēnā ka hangaia e tō plugin ngā wāhanga, ngā kaiwhakarongo, rānei ngā rēhita — tīpakohia ēnā:
§§§CHUNK_SEPARATOR§§§
### Tautoko i18n

Ka tautoko te WIA SOOM i ngā reo 254. Kia taea te whakamaori i ngā tapanga o tō plugin, whakamahia he huarahi māmā:
§§§CHUNK_SEPARATOR§§§
---

## Wāhanga 6: Ngā Tauira i te Ao Tūturu

### Tauira 1: Kaitirotiro Puku Tūmau

Ka whakahaerehia te `df -h` i runga i te tūmau mamao ka whakaatu i te wātea/whakamahia i roto i te pae tūtohu.
§§§CHUNK_SEPARATOR§§§
---

### Tauira 2: Kaiwhakahaere TODO

He plugin e whakahaere ana i tētahi rārangi TODO e whakamahi ana i ngā tautuhinga mō te penapena mau me te tirohanga i roto i te paewhā.

> **Mōhiohio hoahoa:** I te mea kāore e taea e ngā paewhā te karanga i ngā API plugin, ka whakamahi tēnei plugin i te huarahi "snapshot" — ka pānui i ngā TODO mai i ngā tautuhinga, ka hangaia hei HTML pānui anake, ka tuku i ngā mahi i runga i te taha mō te tāpiri i ngā mea. Ko te paewhā he **pātea** ki te whakaatu, kāore he puka whakawhiti.
§§§CHUNK_SEPARATOR§§§
---

### Tauira 3: Kaitirotiro Hē

Ka tirotiro i te putanga terminal ka tuku he pānui i te wā e kitea ana ngā tauira motuhake.
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

## Appendix: Ngā Kategori & Ngā Ikona

### Ngā Kategori Plugin (29)

Whakamahia ēnei i roto i tō `package.json` `keywords` i te wā e tuku ana ki te rehita:

| Kategori | Whakamārama |
|----------|-------------|
| `server` | Whakahaere tūmau whānui |
| `devtools` | Ngā taputapu whakawhanaketanga |
| `calculator` | Ngā kaipōti me ngā whakawhiti |
| `simulator` | Ngā kaiwhakaari |
| `game` | Ngā kēmu terminal |
| `business` | Ngā taputapu pakihi |
| `security` | Te haumaru me te tirohanga |
| `web` | Whakahaere tūmau paetukutuku |
| `education` | Ngā taputapu mātauranga |
| `health` | Ngā taputapu e pā ana ki te hauora |
| `islamic` | Ngā taputapu īkarā (wa kāinga, etc.) |
| `science` | Ngā taputapu pūtaiao |
| `quantum` | Ngā taputapu rorohiko quantum |
| `ai` | Ngā taputapu e powered by AI |
| `biotech` | Ngā taputapu biotechnological |
| `space` | Ngā taputapu mō te rangi me te ātea |
| `network` | Ngā taputapu whatunga |
| `database` | Whakahaere pā data |
| `monitoring` | Tirohanga tūmau |
| `devops` | DevOps me CI/CD |
| `utility` | Ngā taputapu whānui |
| `design` | Ngā taputapu hoahoa |
| `ecommerce` | Ngā taputapu e-commerce |
| `automation` | Ngā taputapu aunoa |
| `kpop` | Ngā taputapu e pā ana ki te K-pop |
| `accessibility` | Ngā taputapu wātea |
| `analytics` | Ngā tātaritanga me te pūrongo |
| `wia` | Ngā taputapu pūnaha WIA |
| `all` | E puta ana i ngā kategori katoa |

### Ngā Ikona Tūtohu (Lucide)

| Ingoa Ikona | Whakamahia mō |
|-------------|---------------|
| `server` | Whakahaere tūmau |
| `shield` | Haumaru |
| `database` | Pā data |
| `activity` | Tirohanga |
| `terminal` | Ngā taputapu terminal |
| `code` | Whakawhanaketanga |
| `hard-drive` | Puku/whakaū |
| `network` | Whatunga |
| `lock` | Whakaū/whakakī |
| `eye` | Tirohanga/tirohia |
| `check-square` | Ngā mahi/TODO |
| `layout-dashboard` | Ngā papa rārangi |
| `settings` | Whakarite |
| `zap` | Aunoa |
| `globe` | Paetukutuku/ā-ao |

Tirohia ngā ikona 1,500+ katoa: [lucide.dev/icons](https://lucide.dev/icons)

---

## E hiahia ana i te awhina?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Ngā Plugin Tauira:** [Website](https://wiasoom.com)
- **Paetukutuku:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Whakawhanakehia he mea whakamīharo. Tōhia ki te ao.</em></p>
<p align="center"><em>— Te Rōpū WIA SOOM</em></p>
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
