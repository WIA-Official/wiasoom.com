<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin གསར་བཟོ་པའི་ལམ་སྟོན</h1>
<p align="center"><strong>སྐར་མ་ ༥ ནང་ཁྱོད་རའི་ plugin གཅིག་བཟོ།</strong></p>
<p align="center">WIA SOOM གི་ནང་འཁོད་ལུ་ ནུས་ཅན་གི་ སར་བར་ཡོ་བྱད་ དེ་ལས་ དྲན་ཤོག་ དེ་ལས་ རང་འགུལ་ཚུ་བཟོ།</p>

---

## ནང་དོན་གི་ཐོ་ཡིག

- [དུམ་བུ་ ༡: མགྱོགས་འགོ་བཙུགས — སྐར་མ་ ༥ ནང་ཁྱོད་ཀྱི་ Plugin དང་པ](#དུམ་བུ་-༡-མགྱོགས་འགོ་བཙུགས--སྐར་མ་-༥-ནང་ཁྱོད་ཀྱི-plugin-དང་པ)
- [དུམ་བུ་ ༢: Plugin Context API གཞི་བསྟུན](#དུམ་བུ་-༢-plugin-context-api-གཞི་བསྟུན)
  - [ctx.terminal](#ctxterminal--རྒྱང་རིང་སར་བར་ཚུ་ནང་བཀའ་རྒྱ་འཁྱེར)
  - [ctx.sftp](#ctxsftp--ཡིག་སྣོད་བརྗེ་སྤོ)
  - [ctx.ui](#ctxui--ལག་ལེན་པའི་མཐོང་སྣང)
  - [ctx.settings](#ctxsettings--བརྟན་ཏན་གྱི་གསོག་འཇོག)
  - [ctx.ai](#ctxai--ai-མཉམ་སྦྲགས)
- [དུམ་བུ་ ༣: Webviews བཀོལ་སྤྱོད་འབད་ དམིགས་བསལ་གྱི UI བཟོ་ནི](#དུམ་བུ་-༣-webviews-བཀོལ་སྤྱོད་འབད་དམིགས་བསལ་གྱི-ui-བཟོ་ནི)
- [དུམ་བུ་ ༤: ཁྱོད་ཀྱི Plugin པར་སྐྲུན་འབད་ནི](#དུམ་བུ་-༤-ཁྱོད་ཀྱི-plugin-པར་སྐྲུན་འབད་ནི)
- [དུམ་བུ་ ༥: སྤྱོད་ལམ་མཆོག་ཤོས](#དུམ་བུ་-༥-སྤྱོད་ལམ་མཆོག་ཤོས)
- [དུམ་བུ་ ༦: འཇིག་རྟེན་ཕྱོགས་ཀྱི་དཔེ་ཚུ](#དུམ་བུ་-༦-འཇིག་རྟེན་ཕྱོགས་ཀྱི་དཔེ་ཚུ)
- [ཟུར་སྦྲགས: དབྱེ་རིམ་ དང་ ངོས་དཔར](#ཟུར་སྦྲགས-དབྱེ་རིམ་-དང་-ངོས་དཔར)

---

## དུམ་བུ་ ༡: མགྱོགས་འགོ་བཙུགས — སྐར་མ་ ༥ ནང་ཁྱོད་ཀྱི Plugin དང་པ

### ཁྱོད་ཀྱིས་ག་ཅི་བཟོ་ནི་ཨིན་ན

"Hello World" plugin གཅིག་བཟོ་ནི་ཨིན། དེ་གིས་ sidebar ནང་ལུ་ ཨེབ་རྟ་གཅིག་བཀོད་ནི་ཨིན། ཨེབ་རྟ་དེ་ཨེབ་པའི་སྐབས་ བརྡ་བསྐུལ་གཅིག་སྟོན་ནི་ཨིན།

### གོམ་པ་ ༡: Plugin ཡིག་སྣོད་ཁག་བཟོ

```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```

### གོམ་པ་ ༢: package.json བཟོ

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

**དགོས་མཁོ་ཡོད་མི་ས་སྒོ་ཚུ:** `name`, `version`, `description`, `author`, `main`

### གོམ་པ་ ༣: index.js བཟོ

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

### གོམ་པ་ ༤: WIA SOOM བསྐྱར་འགོ་བཙུགས

ཨེཔ་དེ་བསྐྱར་འགོ་བཙུགས (ཡང་ན་ སྒྲིག་སྟངས → Plugins ནང་ plugin འབད་/མ་འབད་སོར)།

ཁྱོད་ཀྱིས་ sidebar ནང་ **"Hello World"** ཨེབ་རྟ་མཐོང་དགོས། དེ་ཨེབ — མཐར་འཁྱོལ་གྱི་བརྡ་བསྐུལ་མཐོང་ནི་ཨིན!

### འདི་ག་དེ་སྦེ་ལཱ་འབད་ནི་ཨིན་ན

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

## དུམ་བུ་ ༢: Plugin Context API གཞི་བསྟུན

ཁྱོད་ཀྱི `activate(context)` ལས་འགན་འབོད་པའི་སྐབས་ `context` (ཡང་ན་ `ctx`) གིས་ API འདི་ཚུ་བྱིན་ནི་ཨིན:

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

### `ctx.terminal` — རྒྱང་རིང་སར་བར་ཚུ་ནང་བཀའ་རྒྱ་འཁྱེར

#### `terminal.send(sessionId, data)`

ཐུན་མཐུད་ཅན་གྱི་ terminal session གཅིག་ལུ་ བཀའ་རྒྱ (ཡང་ན་ གཞི་གྲངས་གང་རུང་) བཏང་ནི།

| ཚད་བཟུང | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `sessionId` | `string` | བཏང་ནི་ཡོད་མི་ terminal session |
| `data` | `string` | བཀའ་རྒྱ་ཡང་ན་གཞི་གྲངས |

```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```

#### `terminal.onOutput(sessionId, callback)`

Terminal session གཅིག་གི་ཕྱིར་འཐོན་ཆ་མཉམ་ལུ་ གསར་བརྗེ་གནས་ཚད་ཞུགས། **གསར་བརྗེ་གནས་ཚད་ཕྱིར་ལོག་ལས་འགན** གཅིག་སླར་ལོག་འབད།

| ཚད་བཟུང | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `sessionId` | `string` | བལྟ་ནི་ཡོད་མི་ terminal session |
| `callback` | `(data: string) => void` | ཕྱིར་འཐོན་གྱི་ཡིག་ཆ་རེ་རེ་བཞིན་འབོད་ནི |
| **སླར་ལོག** | `() => void` | ཉན་ནི་བཀག་བཞག་ནིའི་དོན་ལུ་འབོད |

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

**གལ་ཆེ:** ཉན་ནི་བཀག་བཞག་ལས་འགན་དེ་ཉར་ཚགས་འབད་ དེ་ལས་ `deactivate()` ནང་འབོད་ དེ་འབད་བ་ཅིན་ ཤུགས་རྗེས་ཀྱི་ཆུ་བོ་རྒྱུད་ལས་བཀག་ཚུགས།

---

### `ctx.sftp` — ཡིག་སྣོད་བརྗེ་སྤོ

> **གནས་སྟངས: མ་འོངས་པར་འོང** — SFTP API དེ་ངེས་འཛིན་འབད་ཡོདཔ་ཨིན་རུང་ ད་ལྟོ་ཨེཔ་གྱི SFTP ཡོ་བྱད་དང་མཐུད་ཅི་མེད། `list()` གིས་ ད་ལྟོ་ གཏེང་ཁ་ལི་སླར་ལོག་འབད་ནི་ཨིན། `upload()`/`download()` ཚུ་ ལཱ་འགན་མེད། མ་འོངས་པའི་པར་གཞི་ནང་ ཆ་ཚང་སྦེ་བཀོད་སྒྲིག་འབད་ནི་ཨིན། ད་ལྟོའི་དོན་ལུ་ `ctx.terminal.send()` དང་ `scp` ཡང་ན་ `rsync` བཀའ་རྒྱ་ཚུ་བཀོལ་སྤྱོད་འབད།

#### `sftp.list(sessionId, path)`

རྒྱང་རིང་སྣོད་ཁག་ནང་གི་ཡིག་སྣོད་ཚུ་ཐོ་བཀོད་འབད།

```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```

#### `sftp.upload(sessionId, localPath, remotePath)`

ས་གནས་འཕྲུལ་ཆས་ལས་ རྒྱང་རིང་སར་བར་ལུ་ ཡིག་སྣོད་ཕར་བསྐྱལ་འབད།

```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```

#### `sftp.download(sessionId, remotePath, localPath)`

རྒྱང་རིང་སར་བར་ལས་ ས་གནས་འཕྲུལ་ཆས་ལུ་ ཡིག་སྣོད་ཕབ་ལེན་འབད།

```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```

**འཐུས་སྒྲུབ (SFTP API ཐོག་མར་མ་འོངམ་ཚུན):**

```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```

---

### `ctx.ui` — ལག་ལེན་པའི་མཐོང་སྣང

#### `ui.addSidebarButton(options)`

WIA SOOM sidebar ནང་ ཨེབ་རྟ་གཅིག་བཀོད།

| གདམ་ཁ | རིགས | དགོས་མཁོ | འགྲེལ་བཤད |
|--------|------|----------|-------------|
| `id` | `string` | མེད | དམིགས་བསལ་ ID (plugin མིང་ལུ་རང་བཞིན་སྒྲིག) |
| `icon` | `string` | ཨིན | Lucide ངོས་དཔར་མིང (དཔེར་ན `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ཨིན | Sidebar ནང་བཀོད་མི་ ཨེབ་རྟའི་ཡིག་ཆ |
| `onClick` | `() => void` | ཨིན | ཨེབ་རྟ་ཨེབ་པའི་སྐབས་འབོད་མི་ལས་འགན |

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

**ངོས་དཔར་གཞི་བསྟུན:** [lucide.dev/icons](https://lucide.dev/icons) ནང་ ངོས་དཔར་ཆ་མཉམ་བལྟ།

> **མཐུན་ཕྱོགས་དྲན་བསྐུལ:** Plugin རྙིངམ་ལ་ལུ་ཅིག་གིས་ `addSidebarButton(id, icon, label, onClick)` བཟུམ་གྱི་ གོ་རིམ་ཅན་གྱི་ argument ལག་ལེན་འཐབ། ངོ་མའི API གིས་ ཡིག་ཆ་བཀོད་ཡོད་མི་བཟུམ **options object** ལག་ལེན་འཐབ། Plugin གསརཔ་ཚུ་ནང་ object ཐབས་ལམ་ཧེ་མ་ལག་ལེན་འཐབ།

#### `ui.openWebview(options)`

དམིགས་བསལ་གྱི HTML ནང་དོན་དང་བཅས་མི་ སྒོ་སྒྲིག་སླེབས་མ་གཅིག་ཕྱེ། འདི་གིས་ ཕུན་སུམ་ཚོགས་མི UI བཟོ་ཐབས་ཨིན།

| གདམ་ཁ | རིགས | འགྲེལ་བཤད |
|--------|------|-------------|
| `title` | `string` | སྒོ་སྒྲིག་གི་མཚན་བྱང |
| `html` | `string` | བཀོད་རྒྱ་འབད་ནི་ཡོད་མི་ HTML ནང་དོན་ཆ་ཚང |

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

> མཐོ་རིམ་ webview ཐབས་ལམ་ཚུ་གི་དོན་ལུ [དུམ་བུ་ ༣](#དུམ་བུ་-༣-webviews-བཀོལ་སྤྱོད་འབད་དམིགས་བསལ་གྱི-ui-བཟོ་ནི) བལྟ།

#### `ui.showNotification(type, message)`

Toast བརྡ་བསྐུལ་གཅིག་སྟོན།

| ཚད་བཟུང | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | བརྡ་བསྐུལ་གྱི་རྣམ་པ |
| `message` | `string` | སྟོན་ནིའི་ཡིག་ཆ |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

མཇུག་གི་ གནས་ཚད་ཕྲེང་ནང་ བརྟན་ཏན་གྱི་ ཡིག་ཆ་རྣམ་གྲངས་གཅིག་བཀོད།

| ཚད་བཟུང | རིགས | འགྲེལ་བཤད |
|-----------|------|-------------|
| `id` | `string` | གནས་ཚད་རྣམ་གྲངས་འདིའི་དམིགས་བསལ ID |
| `text` | `string` | བཀོད་སྒྲིག་འབད་ནིའི་ཡིག་ཆ |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — བརྟན་ཏན་གྱི་གསོག་འཇོག

Plugin སྒྲིག་སྟངས་ཚུ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ནང་ བརྟན་ཏན་སྦེ་ཉར་ཚགས་འབད།

#### `settings.get(key)`

ཉར་ཚགས་འབད་ཡོད་མི་ གནས་གོང་ལྷག།

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

ལྡེ་མིག་མེད་པ་ཅིན་ `undefined` སླར་ལོག་འབད།

#### `settings.set(key, value)`

གནས་གོང་གཅིག་ཉར་ཚགས་འབད། ཡིག་རྒྱུན་ ཨང་གྲངས་ བདེན་མི་བདེན་ ཐོ་ཡིག་ དེ་ལས་ དངོས་པོ་ཚུ་རྒྱབ་སྐྱོར་འབད།

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**དཔེ: ལག་ལེན་པའི་གདམ་ཁ་ཚུ་ངེས་བཟུང་འབད**

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

### `ctx.ai` — AI མཉམ་སྦྲགས

> **གནས་སྟངས: མ་འོངས་པར་འོང** — AI API དེ་ངེས་འཛིན་འབད་ཡོདཔ་ཨིན་རུང་ ད་ལྟོ Soomy དང་མཐུད་ཅི་མེད། ད་ལྟོ `{ response: 'AI not yet connected' }` སླར་ལོག་འབད། AI མཉམ་སྦྲགས་ཆ་ཚང་ མ་འོངས་པའི་པར་གཞི་ནང་འཆར་གཞི་བཟོ་ཡོད།

#### `ai.chat(messages, options?)`

AI རོགས་མཁན (Soomy) ལུ་ འཕྲིན་དོན་ཚུ་བཏང།

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## དུམ་བུ་ ༣: Webviews བཀོལ་སྤྱོད་འབད་ དམིགས་བསལ་གྱི UI བཟོ་ནི

`openWebview()` API གིས་ སྒོ་སྒྲིག་སླེབས་མའི་ནང་ HTML, CSS, དེ་ལས JavaScript བཀོལ་སྤྱོད་འབད་ དྲན་ཤོག UI བཟོ་ཚུགས།

> **གལ་ཆེའི་མཐའ་ཚད:** Webviews ཚུ་ **བཀོད་སྒྲིག་རྐྱང་པ** ཨིན། དེ་ཚུ་གིས་ plugin API (`ctx.settings`, `ctx.terminal`, སོགས) ལུ་ སླར་འབོད་འབད་མི་ཚུགས། ལག་ལེན་པའི་བྱ་སྤྱོད་ཆ་མཉམ་ sidebar ཨེབ་རྟ་ཚུ་ལག་ལེན་འཐབ་ དེ་ལས་ ད་ལྟོའི་གནས་ཚད་བཀོད་སྒྲིག་འབད་ནིའི་དོན་ལུ `openWebview()` ལག་ལེན་འཐབ། ཕན་ཐོགས་ཅན་གྱི་ཁྱད་ཆོས་དགོ་པ་ཅིན sidebar ཨེབ་རྟ་ཚུ་ལས་འགོ་བཙུགས་ དེ་ལས་ བཀོད་སྒྲིག་གསརཔ་འབད་ནིའི་དོན་ལུ webview ལོག་ཕྱེ།

### ཐབས་ལམ: Terminal བཀའ་རྒྱ → ཕྱིར་འཐོན་དབྱེ་ཞིབ → HTML ནང་སྟོན

འདི་ plugin གྱི་ཐབས་ལམ་ཧེ་མ་ཤོས་ཨིན། ཁྱོད་ཀྱིས་བཀའ་རྒྱ་གཅིག་འཁྱེར་ དེ་ལས་ གྲུབ་འབྲས་དབྱེ་ཞིབ་འབད་ དེ་ལས་ མཐོང་སྣང་ཅན་སྦེ་བཀོད་སྒྲིག་འབད།

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

### ཐབས་ལམ: རང་འགུལ་གསར་བསྐྱར་དང་བཅས་མི་ ཕན་ཐོགས་ཅན་གྱི་དྲན་ཤོག

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

### ཐབས་ལམ: Webview ནང་སྒྲིག་སྟངས་བཀོད་སྒྲིག

> **དྲན་བསྐུལ:** Webviews ཚུ་བཀོད་སྒྲིག་རྐྱང་པ་ཨིན — plugin API ལུ་སླར་འབོད་འབད་མི་ཚུགས། སྒྲིག་སྟངས་བསྒྱུར་བཅོས་འབད་ནིའི་དོན་ལུ sidebar ཨེབ་རྟའི་ handler ནང་ `ctx.settings` ལག་ལེན་འཐབ་ དེ་ལས་ ད་ལྟོའི་གནས་ཚད་སྟོན་ནིའི་དོན་ལུ `openWebview()` ལག་ལེན་འཐབ།

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

## དུམ་བུ་ ༤: ཁྱོད་ཀྱི Plugin པར་སྐྲུན་འབད་ནི

### གོམ་པ་ ༡: ས་གནས་ནང་ཚོད་ལྟ་འབད

1. ཁྱོད་ཀྱི plugin `~/.wia-soom/plugins/{your-plugin}/` ལུ་འདྲ་བཤུས་འབད
2. WIA SOOM བསྐྱར་འགོ་བཙུགས
3. ལཱ་འབད་ཚུགས་མི་ར་ཚོད་ལྟ་འབད: sidebar ཨེབ་རྟ་མཐོང་ནི་ཡོད། ཁྱད་ཆོས་ཚུ་ལེགས་ཤོམ་སྦེ་ལཱ་འབད་ནི་ཡོད
4. མཐའ་མཚམས་ཀྱི་གནས་ཚད་ཚོད་ལྟ་འབད: terminal མཐུད་ཅི་མེད་པ་ཅིན་ ག་ཅི་འབྱུང་ནི་ཨིན་ན

### གོམ་པ་ ༢: དཔེ་མཛོད་ལུ་འཕུལ་ནིའི་གྲ་སྒྲིག་འབད

ཁྱོད་ཀྱི plugin ཡིག་སྣོད་ཁག་ནང་ འདི་ཚུ་དགོས:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**དགོས་མཁོ་ཡོད་མི `package.json` ས་སྒོ་ཚུ:**

| ས་སྒོ | འགྲེལ་བཤད | དཔེ |
|-------|-------------|---------|
| `name` | དམིགས་བསལ kebab-case ID | `"my-awesome-plugin"` |
| `version` | གོ་བརྗོད་ཅན་གྱི་པར་གཞི | `"1.0.0"` |
| `description` | ཕྲེང་གཅིག་གི་འགྲེལ་བཤད | `"Monitors nginx access logs in real-time"` |
| `author` | ཁྱོད་ཀྱི་མིང | `"John Doe"` |
| `main` | འཛུལ་སྒོ་ས | `"index.js"` |

**གདམ་ཁའི་ས་སྒོ་ཚུ:**

| ས་སྒོ | འགྲེལ་བཤད |
|-------|-------------|
| `license` | ཆོག་མཆན་རིགས (MIT འོས་འབབ) |
| `keywords` | འཚོལ་ཞིབ་ tag ཚུའི་ཐོ་ཡིག |
| `soom.minVersion` | དགོས་མཁོ་ཡོད་མི WIA SOOM པར་གཞི་ཉུང་ཤོས |

### གོམ་པ་ ༣: Plugin དཔེ་མཛོད་ལུ་འཕུལ

1. [Plugin Store](https://wiasoom.com) **Fork འབད**
2. ཁྱོད་ཀྱི plugin `plugins/{your-plugin-name}/` ནང་ **བཀོད**
3. Pull Request **འཕུལ**

### གོམ་པ་ ༤: ཞིབ་དཔྱད་ དང་ ཆོག་མཆན

ང་ཅག་གིས་ plugin རེ་རེ་བཞིན་ འདི་ཚུའི་དོན་ལུ་ཞིབ་དཔྱད་འབད:

- **བདེ་འཇགས** — ཉེན་ཅན་གྱི API མེད་པ ([བདེ་འཇགས་སྒྲིག་གཞི](#བདེ་འཇགས་སྒྲིག་གཞི) བལྟ)
- **སྤུས་ཚད** — ལཱ་འབད་ཚུགས་སམ? ཨང་རྟགས་གཙང་མ་ཨིན་ན?
- **ཕན་ཐོགས** — ངོ་མའི་དཀའ་ངལ་གཅིག་སེལ་ཚུགས་མི་ཨིན་ན?

ཆོག་མཆན་ཐོབ་ཚར་བའི་ཤུལ་ལུ:
1. ཁྱོད་ཀྱི plugin `registry.json` ནང་བཀོད
2. `dist/` ནང་ ZIP མཉམ་སྒྲིག་བཟོ
3. ཁྱོད་ཀྱི plugin WIA SOOM ལག་ལེན་པ་ཆ་མཉམ་གྱི་དོན་ལུ **Plugin Store** ནང་མཐོང!

---

## དུམ་བུ་ ༥: སྤྱོད་ལམ་མཆོག་ཤོས

### བདེ་འཇགས་སྒྲིག་གཞི

སྒྲིག་གཞི་འདི་ཚུ **ངེས་པར་དུ་སྲུང་དགོས**། འདི་ཚུ་འགལ་བའི plugin ཚུ་ཁས་མི་ལེན།

| སྒྲིག་གཞི | ག་ཅི་འབད |
|------|-----|
| `eval()` ཡང་ན `new Function()` **ནམ་ཡང་ལག་ལེན་མ་འཐབ** | ཨང་རྟགས་བཙུགས་ཉེན་ཁ |
| `child_process`, `exec()`, `spawn()` **ནམ་ཡང་ལག་ལེན་མ་འཐབ** | བཀའ་རྒྱ་ཚུའི་དོན་ལུ `ctx.terminal.send()` རྐྱང་པ་ལག་ལེན་འཐབ |
| ཕྱིའི URL **ནམ་ཡང་ལེན་མ་འཐབ** | ལས་འཛོལ: `wiasoom.com` API མཐའ་ས |
| `process.env` **ནམ་ཡང་ལྷོད་འཇུག་མ་འབད** | མཐའ་འཁོར་འགྲུབ་རྟགས་ནང་ གསང་བ་ཚུ་ཡོད་སྲིད |
| `require('fs')` ཐད་ཀར **ནམ་ཡང་ལག་ལེན་མ་འཐབ** | གསོག་འཇོག་གི་དོན་ལུ `ctx.settings` ལག་ལེན་འཐབ། ཡིག་སྣོད་བརྗེ་སྤོའི་དོན་ལུ `ctx.sftp` ལག་ལེན་འཐབ |
| npm ཕྱིའི་ཐུམ་སྒྲིལ **ནམ་ཡང་ལག་ལེན་མ་འཐབ** | JavaScript དག་མ་རྐྱང་པ — node_modules མེད |
| རྒྱང་རིང་བཀའ་རྒྱ་ཆ་མཉམ་གྱི་དོན་ལུ `ctx.terminal.send()` **ངེས་པར་དུ་ལག་ལེན་འཐབ** | བདེ་འཇགས་ SSH ལམ་ཁ་རྒྱུད་ནི་ཨིན |
| `deactivate()` ནང་ **ངེས་པར་དུ་གཙང་མ་བཟོ** | ཉན་མཁན་ཚུ་བཏོན་ intervals གཙང་མ་བཟོ |

### འཛོལ་བ་ལག་ལེན

ཉེན་ཅན་གྱི་ལས་སྒོ་ཚུ try/catch ནང་བཅུག:

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

### deactivate() ནང་གཙང་མ་བཟོ

ཁྱོད་ཀྱི plugin གིས intervals, ཉན་མཁན་ ཡང་ན གསར་བརྗེ་གནས་ཚད་བཟོ་ཡོད་པ་ཅིན — གཙང་མ་བཟོ:

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

### i18n རྒྱབ་སྐྱོར

WIA SOOM གིས་ སྐད་ཡིག་ ༢༥༤ རྒྱབ་སྐྱོར་འབད། ཁྱོད་ཀྱི plugin ཁ་བྱང་ སྐད་བསྒྱུར་ཚུགས་མི་བཟོ་ནིའི་དོན་ལུ ཐབས་ལམ་འཕྲོ་མཐུད་འདི་ལག་ལེན་འཐབ:

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

## དུམ་བུ་ ༦: འཇིག་རྟེན་ཕྱོགས་ཀྱི་དཔེ་ཚུ

### དཔེ ༡: སར་བར་ཌིསཀ་ ཚོད་ལྟ་འབད་མཁན

རྒྱང་རིང་སར་བར་ནང་ `df -h` འཁྱེར་ དེ་ལས་ གནས་ཚད་ཕྲེང་ནང་ ལག་ལེན་འབད་ཡོད་མི/ཐོབ་ཚུགས་མི་ བར་སྟོང་སྟོན།

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

### དཔེ ༢: TODO འཛིན་སྐྱོང་པ

བརྟན་ཏན་གྱི་གསོག་འཇོག་གི་དོན་ལུ settings ལག་ལེན་འཐབ་ དེ་ལས་ བཀོད་སྒྲིག་གི་དོན་ལུ webview ལག་ལེན་འཐབ་མི TODO ཐོ་ཡིག་འཛིན་སྐྱོང་འབད་མི plugin།

> **བཟོ་རྣམ་ཐབས་ལམ:** Webviews གིས་ plugin API ཐད་ཀར་འབོད་མི་ཚུགས་པས། plugin འདི་གིས "མཐོང་རིས" ཐབས་ལམ་ལག་ལེན་འཐབ — settings ལས TODO ཚུ་ལྷག་ དེ་ལས་ ལྷག་རྐྱང་པའི HTML སྦེ་བཀོད་སྒྲིག་འབད་ དེ་ལས་ རྣམ་གྲངས་བཀོད་ནིའི་དོན་ལུ sidebar གཞི་བཟུང་གི་བྱ་སྤྱོད་བྱིན། Webview དེ **བཀོད་སྒྲིག** རིམ་པ་ཨིན། ཕན་ཐོགས་ཅན་གྱི་འབྲི་ཤོག་མེན།

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

### དཔེ ༣: འཛོལ་བ་བལྟ་མཁན

Terminal ཕྱིར་འཐོན་བལྟ་ དེ་ལས་ དམིགས་བསལ་གྱི་རྣམ་པ་ཚུ་ཐོབ་པའི་སྐབས་བརྡ་བསྐུལ་བཏང།

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

## ཟུར་སྦྲགས: དབྱེ་རིམ་ དང་ ངོས་དཔར

### Plugin དབྱེ་རིམ (༢༩)

ཁྱོད་ཀྱི `package.json` `keywords` ནང་ ཡང་ན དཔེ་མཛོད་ལུ་འཕུལ་བའི་སྐབས འདི་ཚུ་ལག་ལེན་འཐབ:

| དབྱེ་རིམ | འགྲེལ་བཤད |
|----------|-------------|
| `server` | སར་བར་འཛིན་སྐྱོང་ཡོངས་ཁྱབ |
| `devtools` | གསར་བཟོ་ཡོ་བྱད |
| `calculator` | རྩིས་འཁོར་ དང་ བསྒྱུར་མཁན |
| `simulator` | འདྲ་བཟོ་འཕྲུལ་ཆས |
| `game` | Terminal རྩེད་མོ |
| `business` | ཚོང་ལས་ཡོ་བྱད |
| `security` | བདེ་འཇགས་ དང་ ཞིབ་དཔྱད |
| `web` | དྲ་ཚིགས་སར་བར་འཛིན་སྐྱོང |
| `education` | སློབ་སྦྱོང་ཡོ་བྱད |
| `health` | ཉེན་སྲུང་འབྲེལ་བའི་ཡོ་བྱད |
| `islamic` | ཆོས་ལུགས་ཡོ་བྱད (གསོལ་འདེབས་དུས་ཚོད་ སོགས) |
| `science` | ཚན་རིག་ཡོ་བྱད |
| `quantum` | ཀོན་ཊམ་རྩིས་འཁོར་ཡོ་བྱད |
| `ai` | AI འཁྱེར་མི་ཡོ་བྱད |
| `biotech` | འཚོ་རིག་དང་འཕྲུལ་རིག་ཡོ་བྱད |
| `space` | ནམ་མཁའ་ དང་ སྐར་རྩིས་ཡོ་བྱད |
| `network` | དྲ་རྒྱ་ཡོ་བྱད |
| `database` | གཞི་གྲངས་མཛོད་འཛིན་སྐྱོང |
| `monitoring` | སར་བར་བལྟ་རྟོག |
| `devops` | DevOps དང་ CI/CD |
| `utility` | ཡོངས་ཁྱབ་ཕན་ཐོགས་ཡོ་བྱད |
| `design` | བཟོ་བཀོད་ཡོ་བྱད |
| `ecommerce` | དྲ་ཐོག་ཚོང་ལས་ཡོ་བྱད |
| `automation` | རང་འགུལ་ཡོ་བྱད |
| `kpop` | K-pop འབྲེལ་བའི་ཡོ་བྱད |
| `accessibility` | འཛུལ་སྤྱོད་ཡོ་བྱད |
| `analytics` | དབྱེ་ཞིབ་ དང་ སྙན་ཞུ |
| `wia` | WIA མ་ལག་ཡོ་བྱད |
| `all` | དབྱེ་རིམ་ཆ་མཉམ་ནང་མཐོང |

### འོས་འབབ་ཅན་གྱི་ངོས་དཔར (Lucide)

| ངོས་དཔར་མིང | ལག་ལེན་ |
|-----------|---------|
| `server` | སར་བར་འཛིན་སྐྱོང |
| `shield` | བདེ་འཇགས |
| `database` | གཞི་གྲངས་མཛོད |
| `activity` | བལྟ་རྟོག |
| `terminal` | Terminal ཡོ་བྱད |
| `code` | གསར་བཟོ |
| `hard-drive` | ཌིསཀ/གསོག་འཇོག |
| `network` | དྲ་རྒྱ |
| `lock` | དབང་ཆ/གསང་བཟོ |
| `eye` | བལྟ་ནི/བལྟ་རྟོག |
| `check-square` | ལཱ/TODO |
| `layout-dashboard` | དྲན་ཤོག |
| `settings` | བཀོད་སྒྲིག |
| `zap` | རང་འགུལ |
| `globe` | དྲ་ཚིགས/འཛམ་གླིང |

ངོས་དཔར་ ༡,༥༠༠+ ཆ་མཉམ་བལྟ: [lucide.dev/icons](https://lucide.dev/icons)

---

## རོགས་རམ་དགོ་སམ?

- **GitHub དཀའ་ངལ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin དཀའ་ངལ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **དཔེ་ Plugin ཚུ:** [Website](https://wiasoom.com)
- **དྲ་ཚིགས:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ངོ་མཚར་ཅན་གྱི་ཅ་ལ་བཟོ། འཛམ་གླིང་དང་མཉམ་སྤྱོད་འབད།</em></p>
<p align="center"><em>— WIA SOOM ཚོགས་པ</em></p>
