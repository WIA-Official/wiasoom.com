<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM སྤྱོད་བྱེད་བཟོ་མཁན་གྱི་ལམ་སྟོན།</h1>
<p align="center"><strong>སྐར་མ་ ༥ ནང་ཁྱེད་ཀྱི་སྤྱོད་བྱེད་དང་པོ་བཟོས།</strong></p>
<p align="center">WIA SOOM ནང་དུ་ནུས་པ་ཆེ་བའི་ཞབས་ཞུ་ཆས་ཀྱི་ཡོ་བྱད། གསལ་ཤེལ། དང་རང་འགུལ་གྱི་ཐབས་ལམ་བཟོས།</p>

---

## དཀར་ཆག

- [ཆ་ཤས་ ༡: མགྱོགས་འགོ — སྐར་མ་ ༥ ནང་ཁྱེད་ཀྱི་སྤྱོད་བྱེད་དང་པོ།](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ཆ་ཤས་ ༢: སྤྱོད་བྱེད་སྐབས་སྟོན་ API གཞུང་དོན།](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ཆ་ཤས་ ༣: Webviews བརྒྱུད་སྤྱོད་མཁན་མཐོང་ཡུལ་བཟོ་བ།](#part-3-building-custom-ui-with-webviews)
- [ཆ་ཤས་ ༤: ཁྱེད་ཀྱི་སྤྱོད་བྱེད་འགྲེམས་སྤེལ་བྱེད་པ།](#part-4-publishing-your-plugin)
- [ཆ་ཤས་ ༥: ལེགས་ཤོས་ཀྱི་སྤྱོད་ཕྱོགས།](#part-5-best-practices)
- [ཆ་ཤས་ ༦: དངོས་གནས་ཀྱི་དཔེ་མཚོན།](#part-6-real-world-examples)
- [ཟུར་སྣོན: རིགས་དབྱེ་དང་རྟགས་རིས།](#appendix-categories--icons)

---

## ཆ་ཤས་ ༡: མགྱོགས་འགོ — སྐར་མ་ ༥ ནང་ཁྱེད་ཀྱི་སྤྱོད་བྱེད་དང་པོ།

### ཁྱེད་ཀྱིས་ག་རེ་བཟོ་རྒྱུ

"Hello World" སྤྱོད་བྱེད་ཅིག་གིས་སྣེ་ཕྱོགས་སྟར་ཐོའི་ནང་མཐེབ་གནོན་ཞིག་སྣོན། དེ་མནན་དུས་བརྡ་ཕྲིན་ཞིག་མཐོང་ཐུབ།

### གོམ་པ་ ༡: སྤྱོད་བྱེད་ཀྱི་ཡིག་སྣོད་བཟོས།

```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```

### གོམ་པ་ ༢: package.json བཟོས།

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

**དགོས་ངེས་ཀྱི་ཁ་ཡིག:** `name`, `version`, `description`, `author`, `main`

### གོམ་པ་ ༣: index.js བཟོས།

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

### གོམ་པ་ ༤: WIA SOOM བསྐྱར་འགོ་བཙུགས།

ཉེར་སྤྱོད་བསྐྱར་འགོ་བཙུགས། (ཡང་ན་སྒྲིག་བཀོད → སྤྱོད་བྱེད་ནང་དུ་སྤྱོད་བྱེད་སྒོ་བརྒྱབ/ཕྱེས།)

ཁྱེད་ཀྱིས་སྣེ་ཕྱོགས་སྟར་ཐོའི་ནང་ **"Hello World"** མཐེབ་གནོན་ཞིག་མཐོང་དགོས། དེ་མནན་ན་ — ལེགས་གྲུབ་ཀྱི་བརྡ་ཕྲིན་ཞིག་མཐོང་ཐུབ!

### འདི་ཇི་ལྟར་ལས་བྱེད་པ།

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

## ཆ་ཤས་ ༢: སྤྱོད་བྱེད་སྐབས་སྟོན་ API གཞུང་དོན།

ཁྱེད་ཀྱི་ `activate(context)` བྱ་རིམ་འབོད་སྐབས། `context` (ཡང་ན་ `ctx`) གིས་འདི་དག་གི་ API མཁོ་སྤྲོད་བྱེད།:

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

### `ctx.terminal` — རྒྱང་རིང་ཞབས་ཞུ་ཆས་ཐོག་བཀའ་བཀོད་བཀོལ་སྤྱོད།

#### `terminal.send(sessionId, data)`

ལས་མཆན་སྒོ་ཕྱེ་བའི་ནང་བཀའ་བཀོད་ (ཡང་ན་གཞི་གྲངས་གང་ཡང) བསྐུར།

| གཞི་རྟེན་ཚད | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `sessionId` | `string` | བསྐུར་སའི་ལས་མཆན་སྒོ |
| `data` | `string` | བསྐུར་རྒྱུའི་བཀའ་བཀོད་ཡང་ན་གཞི་གྲངས |

```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```

#### `terminal.onOutput(sessionId, callback)`

ལས་མཆན་སྒོའི་ཕྱིར་འདོན་ཚང་མ་ལ་རྒྱུན་བཞིན་ཉན་ཐུབ། **ཉན་པ་མཚམས་འཇོག་གི་བྱ་རིམ** ཞིག་སྤྲོད།

| གཞི་རྟེན་ཚད | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `sessionId` | `string` | བལྟ་རྒྱུའི་ལས་མཆན་སྒོ |
| `callback` | `(data: string) => void` | ཕྱིར་འདོན་གྱི་ཆ་ཤས་རེ་རེར་འབོད |
| **སྤྲོད་ཐོན** | `() => void` | ཉན་པ་མཚམས་འཇོག་ལ་འདི་འབོད |

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

**གལ་ཆེན:** ཉན་པ་མཚམས་འཇོག་གི་བྱ་རིམ་རྟག་ཏུ་ཉར། དེ་ `deactivate()` ནང་འབོད་ནས་བརྗོད་ཤོར་འགོག་དགོས།

---

### `ctx.sftp` — ཡིག་ཆ་སྐྱེལ་འདྲེན།

> **གནས་བབ: མ་འོངས་པར་འབྱོར་རྒྱུ** — SFTP API འཆར་གཞི་ཡོད་ཀྱང་ད་ལྟ་ཉེར་སྤྱོད་ཀྱི་ SFTP འཕྲུལ་ཆས་དང་སྦྲེལ་མེད། `list()` ད་ལྟ་སྟོང་པའི་ཐོ་གཞུང་སྤྲོད། `upload()`/`download()` ཀྱང་ལས་བྱེད་མེད། མ་འོངས་པའི་པར་གཞིར་ཆ་ཚང་བསྒྲུབ་རྒྱུ། ད་ལྟ་`ctx.terminal.send()` བརྒྱུད་ `scp` ཡང་ན་ `rsync` བཀའ་བཀོད་སྤྱོད་ན་ཐབས་ལམ་ཡིན།

#### `sftp.list(sessionId, path)`

རྒྱང་རིང་ཡིག་སྣོད་ནང་གི་ཡིག་ཆ་ཐོ་གཞུང་བཤེར།

```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```

#### `sftp.upload(sessionId, localPath, remotePath)`

ས་གནས་འཕྲུལ་ཆས་ནས་རྒྱང་རིང་ཞབས་ཞུ་ཆས་ལ་ཡིག་ཆ་འཇོག

```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```

#### `sftp.download(sessionId, remotePath, localPath)`

རྒྱང་རིང་ཞབས་ཞུ་ཆས་ནས་ས་གནས་འཕྲུལ་ཆས་ལ་ཡིག་ཆ་ཕབ་ལེན།

```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```

**ཐབས་ལམ་ (SFTP API ལས་བྱེད་མ་བཟོ་བར):**

```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```

---

### `ctx.ui` — སྤྱོད་མཁན་མཐོང་ཡུལ།

#### `ui.addSidebarButton(options)`

WIA SOOM སྣེ་ཕྱོགས་སྟར་ཐོར་མཐེབ་གནོན་སྣོན།

| འདེམས་ག | རིགས | དགོས་ངེས | འགྲེལ་བཤད |
|--------|------|----------|-------------|
| `id` | `string` | མིན | ཐུན་མོང་མིན་པའི་ངོ་རྟགས (སྤྱོད་བྱེད་མིང་ལ་རང་བཞིན་འགྲོ) |
| `icon` | `string` | རེད | Lucide རྟགས་རིས་མིང (དཔེར་ན `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | རེད | སྣེ་ཕྱོགས་སྟར་ཐོར་མཐོང་བའི་མཐེབ་གནོན་ཡི་གེ |
| `onClick` | `() => void` | རེད | མཐེབ་གནོན་མནན་སྐབས་འབོད་པའི་བྱ་རིམ |

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

**རྟགས་རིས་གཞུང་དོན:** ཡོད་ཚད་ཀྱི་རྟགས་རིས་ [lucide.dev/icons](https://lucide.dev/icons) ལ་བལྟ།

> **མཐུན་སྤྱོད་མཆན:** སྤྱོད་བྱེད་རྙིང་པ་ཁ་ཤས་ཀྱིས་ `addSidebarButton(id, icon, label, onClick)` ལྟར་གོ་རིམ་གྱི་གཞི་རྟེན་ཚད་སྤྱོད། གཞུང་ཐོག་ API ཡིས་གོང་དུ་བཀོད་པ་ལྟར་ **འདེམས་གའི་བྱ་ཡུལ** སྤྱོད། སྤྱོད་བྱེད་གསར་པར་རྟག་ཏུ་བྱ་ཡུལ་གྱི་ཚུལ་སྤྱོད།

#### `ui.openWebview(options)`

རང་བཟོའི་ HTML ནང་དོན་ཅན་གྱི་སྒོ་ཕྱེ་སྒེའུ་ཁུང་ཞིག་ཕྱེས། འདི་ནི་ཁྱེད་ཀྱིས་ཕུན་སུམ་ཚོགས་པའི་མཐོང་ཡུལ་བཟོ་ཚུལ་ཡིན།

| འདེམས་ག | རིགས | འགྲེལ་བཤད |
|--------|------|-------------|
| `title` | `string` | སྒེའུ་ཁུང་གི་མགོ་བྱང |
| `html` | `string` | བཀོད་སྒྲིག་བྱ་རྒྱུའི་ HTML ནང་དོན་ཆ་ཚང |

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

> ཟབ་རྒྱས་ཀྱི་ webview ཚུལ་ལ་ [ཆ་ཤས་ ༣](#part-3-building-custom-ui-with-webviews) ལ་བལྟ།

#### `ui.showNotification(type, message)`

གློ་བུར་བརྡ་ཕྲིན་ཞིག་སྟོན།

| གཞི་རྟེན་ཚད | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | བརྡ་ཕྲིན་གྱི་ཚུལ |
| `message` | `string` | སྟོན་རྒྱུའི་ཡི་གེ |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

འོག་གི་གནས་ཚུལ་སྟར་ཐོར་རྟག་བརྟན་གྱི་ཡི་གེ་རྣམ་གྲངས་སྣོན།

| གཞི་རྟེན་ཚད | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `id` | `string` | གནས་ཚུལ་རྣམ་གྲངས་འདིའི་ཐུན་མོང་མིན་པའི་ངོ་རྟགས |
| `text` | `string` | བཀོད་སྒྲིག་བྱ་རྒྱུའི་ཡི་གེ |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — རྟག་བརྟན་གྱི་ཉར་ཚགས།

སྤྱོད་བྱེད་ཀྱི་སྒྲིག་བཀོད་ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ནང་རྟག་གཏན་ཉར།

#### `settings.get(key)`

ཉར་ཚགས་བྱས་པའི་གནས་ཐང་བཀླག

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

ལྡེ་མིག་མེད་ན་ `undefined` སྤྲོད།

#### `settings.set(key, value)`

གནས་ཐང་ཉར། ཡིག་ཕྲེང། གྲངས་ཀ། བདེན་རྫུན། ཐོ་གཞུང། བྱ་ཡུལ་བཅས་རྒྱབ་སྐྱོར་བྱེད།

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**དཔེ་མཚོན: སྤྱོད་མཁན་གྱི་གདམ་ག་ངེས་བརྗོད།**

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

### `ctx.ai` — AI མཉམ་སྦྲེལ།

> **གནས་བབ: མ་འོངས་པར་འབྱོར་རྒྱུ** — AI API འཆར་གཞི་ཡོད་ཀྱང་ད་ལྟ་ Soomy དང་སྦྲེལ་མེད། ད་ལྟ `{ response: 'AI not yet connected' }` སྤྲོད། མ་འོངས་པའི་པར་གཞིར་ AI མཉམ་སྦྲེལ་ཆ་ཚང་འཆར་གཞི་ཡོད།

#### `ai.chat(messages, options?)`

AI གྲོགས་རམ་པ (Soomy) ལ་འཕྲིན་ཐུང་བསྐུར།

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## ཆ་ཤས་ ༣: Webviews བརྒྱུད་སྤྱོད་མཁན་མཐོང་ཡུལ་བཟོ་བ།

`openWebview()` API ཡིས་ HTML, CSS, དང་ JavaScript བརྒྱུད་གསལ་ཤེལ་གྱི་མཐོང་ཡུལ་བཟོ་ཐུབ — ཚང་མ་སྒོ་ཕྱེ་སྒེའུ་ཁུང་ནང།

> **གལ་ཆེའི་ཚད་འཛིན:** Webviews ནི **བཀོད་སྒྲིག་ཙམ** ཡིན། སྤྱོད་བྱེད་ API (`ctx.settings`, `ctx.terminal`, སོགས) ལ་ཕྱིར་འབོད་མི་ཐུབ། སྤྱོད་མཁན་གྱི་བྱ་སྤྱོད་ཚང་མར་སྣེ་ཕྱོགས་ཀྱི་མཐེབ་གནོན་སྤྱོད། ད་ལྟའི་གནས་ཚུལ་བཀོད་པར་ `openWebview()` སྤྱོད། མཉམ་བཞུགས་ཀྱི་ཁྱད་ཆོས་དགོས་ན། སྣེ་ཕྱོགས་ཀྱི་མཐེབ་གནོན་ནས་འགུལ་སྐྱོད་བཟོས་ནས་ webview བསྐྱར་ཕྱེས་ནས་བསྐྱར་གསོ་བྱོས།

### ཚུལ: ལས་མཆན་སྒོའི་བཀའ་བཀོད → ཕྱིར་འདོན་དབྱེ་ཞིབ → HTML ནང་བཀོད།

འདི་ནི་སྤྱོད་བྱེད་ཀྱི་ཚུལ་ཆེས་ཐུན་མོང་བ་ཡིན། བཀའ་བཀོད་བཀོལ། འབྲས་བུ་དབྱེ་ཞིབ། མཐོང་ཡུལ་ཐོག་བཀོད།

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

### ཚུལ: རང་འགུལ་བསྐྱར་གསོ་ཅན་གྱི་མཉམ་བཞུགས་གསལ་ཤེལ།

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

### ཚུལ: Webview ནང་སྒྲིག་བཀོད་བཀོད་པ།

> **མཆན:** Webviews ནི་བཀོད་སྒྲིག་ཙམ་ཡིན — སྤྱོད་བྱེད་ API ལ་ཕྱིར་འབོད་མི་ཐུབ། སྒྲིག་བཀོད་བསྒྱུར་བ་ལ་སྣེ་ཕྱོགས་ཀྱི་མཐེབ་གནོན་ཐོག་ `ctx.settings` སྤྱོད། ད་ལྟའི་གནས་ཚུལ་བཀོད་པར་ `openWebview()` སྤྱོད།

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

## ཆ་ཤས་ ༤: ཁྱེད་ཀྱི་སྤྱོད་བྱེད་འགྲེམས་སྤེལ་བྱེད་པ།

### གོམ་པ་ ༡: ས་གནས་སུ་ཚོད་ལྟ།

1. ཁྱེད་ཀྱི་སྤྱོད་བྱེད `~/.wia-soom/plugins/{your-plugin}/` ལ་འཕྲིང།
2. WIA SOOM བསྐྱར་འགོ་བཙུགས།
3. ལས་བྱེད་ཀྱིན་ཡོད་པ་ཞིབ་བརྟག: སྣེ་ཕྱོགས་ཀྱི་མཐེབ་གནོན་མཐོང་ཡོད། ཁྱད་ཆོས་ཚང་མ་ཡག་པོ་ལས་བྱེད་ཀྱིན་ཡོད།
4. མཐའ་མཚམས་ཀྱི་གནས་ཚུལ་ཚོད་ལྟ: ལས་མཆན་སྒོ་མ་སྦྲེལ་ན་ག་རེ་བྱུང?

### གོམ་པ་ ༢: འབུལ་བའི་གྲ་སྒྲིག

ཁྱེད་ཀྱི་སྤྱོད་བྱེད་ཡིག་སྣོད་ནང་འདི་དག་དགོས:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**དགོས་ངེས་ཀྱི `package.json` ཁ་ཡིག:**

| ཁ་ཡིག | འགྲེལ་བཤད | དཔེ་མཚོན |
|-------|-------------|---------|
| `name` | ཐུན་མོང་མིན་པའི kebab-case ངོ་རྟགས | `"my-awesome-plugin"` |
| `version` | ཚོད་འཛིན་པར་གཞི | `"1.0.0"` |
| `description` | ཕྲེང་གཅིག་གི་འགྲེལ་བཤད | `"Monitors nginx access logs in real-time"` |
| `author` | ཁྱེད་ཀྱི་མིང | `"John Doe"` |
| `main` | འཛུལ་སྒོ | `"index.js"` |

**འདེམས་རུང་གི་ཁ་ཡིག:**

| ཁ་ཡིག | འགྲེལ་བཤད |
|-------|-------------|
| `license` | ཆོག་མཆན་རིགས (MIT འོས་སྦྱོར) |
| `keywords` | འཚོལ་བའི་ཐོ་འགོད་ཐོ་གཞུང |
| `soom.minVersion` | དགོས་ངེས་ཀྱི་ WIA SOOM པར་གཞི་ཆེས་དམའ |

### གོམ་པ་ ༣: སྤྱོད་བྱེད་ཐོ་འགོད་ལ་འབུལ།

1. [Plugin Store](https://wiasoom.com) **འགའ་ཕྱེ**
2. `plugins/{your-plugin-name}/` ལ་ཁྱེད་ཀྱི་སྤྱོད་བྱེད **སྣོན**
3. Pull Request **འབུལ**

### གོམ་པ་ ༤: ཞིབ་བརྟག་དང་ཆོག་མཆན།

ང་ཚོས་སྤྱོད་བྱེད་ཚང་མ་འདི་དག་ལ་ཞིབ་བརྟག་བྱེད:

- **བདེ་འཇགས** — ཉེན་ཁ་ཅན་གྱི་ API མེད (བལྟ [བདེ་འཇགས་སྒྲིག་ལམ](#security-rules))
- **ཟབ་ཚད** — ལས་བྱེད་ཀྱིན་ཡོད་དམ? ཨང་རྟགས་གཙང་མ་ཡོད་དམ?
- **ཕན་ཐོགས** — དངོས་གནས་ཀྱི་གནད་དོན་ཞིག་སེལ་ཐུབ་བམ?

ཆོག་མཆན་རྗེས:
1. ཁྱེད་ཀྱི་སྤྱོད་བྱེད `registry.json` ལ་སྣོན།
2. `dist/` ནང ZIP མཉམ་སྒྲིལ་བཟོས།
3. ཁྱེད་ཀྱི་སྤྱོད་བྱེད WIA SOOM སྤྱོད་མཁན་ཚང་མའི་ **Plugin Store** ནང་མཐོང་ཐུབ!

---

## ཆ་ཤས་ ༥: ལེགས་ཤོས་ཀྱི་སྤྱོད་ཕྱོགས།

### བདེ་འཇགས་སྒྲིག་ལམ།

འདི་དག་གི་སྒྲིག་ལམ **དགོས་ངེས** ཡིན། འགལ་བའི་སྤྱོད་བྱེད་དོར་འཕེན་བྱེད།

| སྒྲིག་ལམ | རྒྱུ་མཚན |
|------|-----|
| `eval()` ཡང་ན `new Function()` **ཐེངས་གཅིག་ཀྱང་མ་སྤྱོད** | ཨང་རྟགས་བཙུགས་པའི་ཉེན་ཁ |
| `child_process`, `exec()`, `spawn()` **ཐེངས་གཅིག་ཀྱང་མ་སྤྱོད** | བཀའ་བཀོད་ལ་ `ctx.terminal.send()` ཁོ་ན་སྤྱོད |
| ཕྱི་རོལ་ URL **ཐེངས་གཅིག་ཀྱང་མ་ལེན** | གཞན་ཆ: `wiasoom.com` API མཐའ་ཚིག |
| `process.env` **ཐེངས་གཅིག་ཀྱང་མ་འཛུལ** | ཁོར་ཡུག་འགྱུར་ཚད་ནང་གསང་བ་ཡོད་སྲིད |
| `require('fs')` ཐད་ཀར **ཐེངས་གཅིག་ཀྱང་མ་སྤྱོད** | ཉར་ཚགས་ལ `ctx.settings` སྤྱོད། ཡིག་ཆ་སྐྱེལ་འདྲེན་ལ `ctx.sftp` སྤྱོད |
| npm ཕྱི་རོལ་ཐུམ་སྒྲིལ **ཐེངས་གཅིག་ཀྱང་མ་སྤྱོད** | དག་པའི JavaScript ཁོ་ན — node_modules མེད |
| རྒྱང་རིང་བཀའ་བཀོད་ཚང་མར `ctx.terminal.send()` **དགོས་ངེས་སྤྱོད** | འདིས་བདེ་འཇགས་ SSH ལམ་ཀ་བརྒྱུད་འགྲོ |
| `deactivate()` ནང **དགོས་ངེས་གཙང་བཟོ** | ཉན་པ་བསུབ། དུས་བཀག་གཙང་བཟོ |

### ནོར་འཁྲུལ་འཛིན་སྐྱོང།

ཉེན་ཁ་ཅན་གྱི་བྱ་སྤྱོད་རྟག་ཏུ try/catch ནང་བཅུག:

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

### deactivate() ནང་གཙང་བཟོ།

ཁྱེད་ཀྱི་སྤྱོད་བྱེད་ཀྱིས་དུས་བཀག། ཉན་པ། ཡང་ན་ཉོ་མཆན་བཟོས་ན — གཙང་བཟོ་བྱོས:

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

### i18n རྒྱབ་སྐྱོར།

WIA SOOM གིས་སྐད་ཡིག་ ༢༥༤ རྒྱབ་སྐྱོར་བྱེད། ཁྱེད་ཀྱི་སྤྱོད་བྱེད་ཀྱི་ཐོ་འགོད་སྐད་བསྒྱུར་ཐུབ་པར་ཐབས་ལམ་སྟབས་བདེ་ཞིག་སྤྱོད:

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

## ཆ་ཤས་ ༦: དངོས་གནས་ཀྱི་དཔེ་མཚོན།

### དཔེ་མཚོན་ ༡: ཞབས་ཞུ་ཆས་སྡུད་སྡེར་ཞིབ་བརྟག་པ།

རྒྱང་རིང་ཞབས་ཞུ་ཆས་ཐོག `df -h` བཀོལ་ནས་བེད་སྤྱོད/ཡོད་ཚད་ཀྱི་གནས་ཚུལ་སྟར་ཐོར་བཀོད།

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

### དཔེ་མཚོན་ ༢: TODO འཛིན་སྐྱོང་པ།

སྒྲིག་བཀོད་བརྒྱུད་རྟག་བརྟན་ཉར་ཚགས་དང་ webview བརྒྱུད་བཀོད་སྒྲིག་བྱས་ནས TODO ཐོ་གཞུང་འཛིན་སྐྱོང་བྱེད་པའི་སྤྱོད་བྱེད།

> **བཟོ་ཚུལ:** webviews ཀྱིས་སྤྱོད་བྱེད་ API ཐད་ཀར་འབོད་མི་ཐུབ་པས། སྤྱོད་བྱེད་འདིས "ཡུལ་ལྗོངས" ཚུལ་སྤྱོད — སྒྲིག་བཀོད་ནས TODO བཀླག, བཀླག་ཙམ་གྱི HTML ལ་བཀོད། རྣམ་གྲངས་སྣོན་པར་སྣེ་ཕྱོགས་ཀྱི་བྱ་སྤྱོད་མཁོ་སྤྲོད། webview ནི **བཀོད་སྒྲིག** ཞིག་ཡིན། མཉམ་བཞུགས་ཀྱི་རྣམ་པ་མིན།

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

### དཔེ་མཚོན་ ༣: ནོར་འཁྲུལ་བལྟ་མཁན།

ལས་མཆན་སྒོའི་ཕྱིར་འདོན་ལ་བལྟ་ནས་ཁྱད་མཚན་གཏན་འཁེལ་བ་རྙེད་སྐབས་བརྡ་ཕྲིན་གཏོང།

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

## ཟུར་སྣོན: རིགས་དབྱེ་དང་རྟགས་རིས།

### སྤྱོད་བྱེད་རིགས་དབྱེ (༢༩)

ཁྱེད་ཀྱི package.json `keywords` ཡང་ན་ཐོ་འགོད་ལ་འབུལ་སྐབས་འདི་དག་སྤྱོད:

| རིགས་དབྱེ | འགྲེལ་བཤད |
|----------|-------------|
| `server` | སྤྱི་བའི་ཞབས་ཞུ་ཆས་འཛིན་སྐྱོང |
| `devtools` | སྒྲུབ་བཟོའི་ཡོ་བྱད |
| `calculator` | རྩིས་འཁོར་དང་བསྒྱུར་ཆས |
| `simulator` | བཟོ་བཀོད་ཆས |
| `game` | ལས་མཆན་སྒོའི་རྩེད་མོ |
| `business` | ཚོང་ལས་ཡོ་བྱད |
| `security` | བདེ་འཇགས་དང་ཞིབ་བརྟག |
| `web` | དྲ་ཞབས་ཞུ་ཆས་འཛིན་སྐྱོང |
| `education` | སློབ་གསོའི་ཡོ་བྱད |
| `health` | བདེ་ཐང་དང་འབྲེལ་བའི་ཡོ་བྱད |
| `islamic` | ཁ་ཆེའི་ཡོ་བྱད (གསོལ་འདེབས་དུས་ཚོད་སོགས) |
| `science` | ཚན་རིག་ཡོ་བྱད |
| `quantum` | གཟའ་རྡུལ་རྩིས་འཁོར་ཡོ་བྱད |
| `ai` | AI ཡིས་སྒྲུབ་པའི་ཡོ་བྱད |
| `biotech` | སྐྱེ་དངོས་ལག་རྩལ་ཡོ་བྱད |
| `space` | མཁའ་དབྱིངས་དང་སྐར་རྩིས་ཡོ་བྱད |
| `network` | དྲ་རྒྱའི་ཡོ་བྱད |
| `database` | གཞི་གྲངས་མཛོད་འཛིན་སྐྱོང |
| `monitoring` | ཞབས་ཞུ་ཆས་ལྟ་ཞིབ |
| `devops` | DevOps དང CI/CD |
| `utility` | སྤྱི་བའི་ཕན་ཆས |
| `design` | བཟོ་བཀོད་ཡོ་བྱད |
| `ecommerce` | ཚོང་ལས་གློག་རྡུལ་ཡོ་བྱད |
| `automation` | རང་འགུལ་ཡོ་བྱད |
| `kpop` | K-pop དང་འབྲེལ་བའི་ཡོ་བྱད |
| `accessibility` | འཛུལ་སྤྱོད་ཡོ་བྱད |
| `analytics` | དབྱེ་ཞིབ་དང་སྙན་ཞུ |
| `wia` | WIA ཁོར་ཡུག་ཡོ་བྱད |
| `all` | རིགས་དབྱེ་ཚང་མར་མཐོང |

### འོས་སྦྱོར་གྱི་རྟགས་རིས (Lucide)

| རྟགས་རིས་མིང | སྤྱོད་སའི |
|-----------|---------|
| `server` | ཞབས་ཞུ་ཆས་འཛིན་སྐྱོང |
| `shield` | བདེ་འཇགས |
| `database` | གཞི་གྲངས་མཛོད |
| `activity` | ལྟ་ཞིབ |
| `terminal` | ལས་མཆན་སྒོའི་ཡོ་བྱད |
| `code` | སྒྲུབ་བཟོ |
| `hard-drive` | སྡུད་སྡེར/ཉར་ཚགས |
| `network` | དྲ་རྒྱ |
| `lock` | ངོས་སྟོན/གསང་བཟོ |
| `eye` | བལྟ་བ/ལྟ་ཞིབ |
| `check-square` | ལས་འགན/TODO |
| `layout-dashboard` | གསལ་ཤེལ |
| `settings` | སྒྲིག་བཀོད |
| `zap` | རང་འགུལ |
| `globe` | དྲ་རྒྱ/རྒྱལ་སྤྱི |

རྟགས་རིས ༡,༥༠༠+ ཚང་མ་བལྟ: [lucide.dev/icons](https://lucide.dev/icons)

---

## གྲོགས་རམ་དགོས་སམ?

- **GitHub གནད་དོན:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **སྤྱོད་བྱེད་གནད་དོན:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **དཔེ་མཚོན་སྤྱོད་བྱེད:** [Website](https://wiasoom.com)
- **དྲ་ཚིགས:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ཡ་མཚན་ཅན་གྱི་དངོས་པོ་བཟོས། འཛམ་གླིང་ལ་མཉམ་སྤྱོད་བྱོས།</em></p>
<p align="center"><em>— WIA SOOM ཚོགས་ཆུང</em></p>
