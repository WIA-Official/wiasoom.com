<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM പ്ലഗിൻ ഡവലപ്പർ ഗൈഡ്</h1>
<p align="center"><strong>5 മിനിറ്റിൽ നിങ്ങളുടെ സ്വന്തം പ്ലഗിൻ നിർമ്മിക്കുക.</strong></p>
<p align="center">ശക്തമായ സർവർ ടൂളുകൾ, ഡാഷ്ബോർഡുകൾ, ഓട്ടോമേഷനുകൾ എന്നിവ WIA SOOM-ൽ തന്നെ സൃഷ്ടിക്കുക.</p>

---

## ഉള്ളടക്കത്തിന്റെ പട്ടിക

- [ഭാഗം 1: ക്വിക് സ്റ്റാർട്ട് — നിങ്ങളുടെ ആദ്യ പ്ലഗിൻ 5 മിനിറ്റിൽ](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ഭാഗം 2: പ്ലഗിൻ കോൺടെക്സ്റ്റ് API റഫറൻസ്](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ഭാഗം 3: വെബ്‌വ്യൂസ് ഉപയോഗിച്ച് കസ്റ്റം UI നിർമ്മിക്കൽ](#part-3-building-custom-ui-with-webviews)
- [ഭാഗം 4: നിങ്ങളുടെ പ്ലഗിൻ പ്രസിദ്ധീകരിക്കൽ](#part-4-publishing-your-plugin)
- [ഭാഗം 5: മികച്ച പ്രായോഗികങ്ങൾ](#part-5-best-practices)
- [ഭാഗം 6: യാഥാർത്ഥ്യ ഉദാഹരണങ്ങൾ](#part-6-real-world-examples)
- [അനുബന്ധം: വിഭാഗങ്ങൾ & ഐക്കണുകൾ](#appendix-categories--icons)

---

## ഭാഗം 1: ക്വിക് സ്റ്റാർട്ട് — നിങ്ങളുടെ ആദ്യ പ്ലഗിൻ 5 മിനിറ്റിൽ

### നിങ്ങൾ നിർമ്മിക്കുന്നതെന്ത്

ഒരു "ഹലോ വേൾഡ്" പ്ലഗിൻ, ഇത് സൈഡ്‌ബാർയിൽ ഒരു ബട്ടൺ ചേർക്കുന്നു. ക്ലിക്ക് ചെയ്താൽ, ഇ���് ഒരു നോട്ടിഫിക്കേഷൻ കാണിക്കുന്നു.

### ഘട്ടം 1: പ്ലഗിൻ ഫോൾഡർ സൃഷ്ടിക്കുക
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### ഘട്ടം 2: package.json സൃഷ്ടിക്കുക
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
**ആവശ്യമായ ഫീൽഡുകൾ:** `name`, `version`, `description`, `author`, `main`

### ഘട്ടം 3: index.js സൃഷ്ടിക്കുക
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
### ഘട്ടം 4: WIA SOOM പുനരാരംഭിക്കുക

ആപ്പ് പുനരാരംഭിക്കുക (അല്ലെങ്കിൽ സെറ്റിംഗ്സ് → പ്ലഗിനുകളിൽ പ്ലഗിൻ ഓഫ്/ഓൺ ചെയ്യുക).

സൈഡ്‌ബാർയിൽ **"ഹലോ വേൾഡ്"** ബട്ടൺ കാണണം. അത് ക്ലിക്ക് ചെയ്യുക — നിങ്ങൾക്ക് ഒരു വിജയ നോട്ടിഫിക്കേഷൻ കാണാം!

### ഇത് എങ്ങനെ പ്രവർത്തിക്കുന്നു
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

## ഭാഗം 2: പ്ലഗിൻ കോൺടെക്സ്റ്റ് API റഫറൻസ്

നി��്ങളുടെ `activate(context)` ഫംഗ്ഷൻ വിളിക്കുമ്പോൾ, `context` (അല്ലെങ്കിൽ `ctx`) ഈ API-കൾ നൽകുന്നു:
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

### `ctx.terminal` — ദൂര സർവറുകളിൽ കമാൻഡുകൾ പ്രവർത്തിക്കുക

#### `terminal.send(sessionId, data)`

ഒരു സജീവ ടെർമിനൽ സെഷനിലേക്ക് ഒരു കമാൻഡ് (അല്ലെങ്കിൽ ഏതെങ്കിലും ഡാറ്റ) അയക്കുക.

| പാരാമീറ്റർ | തരം | വിവരണം |
|-----------|------|-------------|
| `sessionId` | `string` | അയക്കേണ്ട ടെർമിനൽ സെഷൻ |
| `data` | `string` | അയക്കേണ്ട കമാൻഡ് അല്ലെങ്കിൽ ഡാറ്റ |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

ഒരു ടെർമിനൽ സെഷനിൽ നിന്നുള്ള എല്ലാ ഔട്ട്പുട്ടിനും സബ്സ്ക്രൈബ് ചെയ്യുക. ഒരു **അൺസബ്സ്ക്രൈബ് ഫംഗ്ഷൻ** തിരികെ നൽകുന്നു.

| പാരാമീറ്റർ | തരം | വിവരണം |
|-----------|------|-------------|
| `sessionId` | `string` | ശ്രദ്ധിക്കേണ്ട ടെർമിനൽ സെഷൻ |
| `callback` | `(data: string) => void` | ഔട്ട്പുട്ടിന്റെ ഓരോ ചങ്കിലും വിളിക്കപ്പെടുന്നു |
| **തിരികെ നൽകുന്നു** | `() => void` | കേൾക്കുന്നത് അവസാനിപ്പിക്കാൻ ഇത് വിളിക്കുക |
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
**പ്രധാനമാണ്:** എപ്പോഴും അൺസബ്സ്ക്രൈബ് ഫംഗ്ഷൻ സംരക്ഷിക്കുക, മെമ്മറി ചീഞ്ഞുകൾ ഒഴിവാക്കാൻ `deactivate()`-ൽ അത് വിളിക്കുക.

---

### `ctx.sftp` — ഫയൽ കൈമാറ്റം

> **സ്ഥിതി: ഉടൻ വരുന്നു** — SFTP API നിർവചിച്ചിരിക്കുന്നു, എന്നാൽ ആപ്പിന്റെ SFTP എഞ്ചിനിലേക്ക് ഇപ്പോഴും കണക്ട് ചെയ്തിട്ടില്ല. `list()` നിലവിൽ ഒരു ശൂന്യമായ അറയി ത��രികെ നൽകുന്നു, കൂടാതെ `upload()`/`download()` പ്രവർത്തനമില്ല. ഇത് ഭാവിയിൽ ഒരു റിലീസിൽ പൂർണ്ണമായും നടപ്പിലാക്കും. ഇപ്പോൾ, `scp` അല്ലെങ്കിൽ `rsync` കമാൻഡുകൾ ഉപയോഗിച്ച് `ctx.terminal.send()` ഉപയോഗിക്കുക.

#### `sftp.list(sessionId, path)`

ഒരു ദൂര ഡയറക്ടറിയിലെ ഫയലുകൾ പട്ടികപ്പെടുത്തുക.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

ലോകൽ മെഷീനിൽ നിന്ന് ദൂര സർവറിലേക്ക് ഒരു ഫയൽ അപ്‌ലോഡ് ചെയ്യുക.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

ദൂര സർവറിൽ നിന്ന് ലോകൽ മെഷീനിലേക്ക് ഒരു ഫയൽ ഡൗൺലോഡ് ചെയ്യുക.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**പരിഹാരം (SFTP API പ്രവർത്തനക്ഷമമായതുവരെ):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — ഉപയോക്തൃ ഇന്റർഫേസ്

#### `ui.addSidebarButton(options)`

WIA SOOM സൈഡ്‌ബാറിൽ ഒരു ബട്ടൺ ചേർക്കുക.

| ഓപ്ഷൻ | തരം | ആവശ്യമാണ് | വിവരണം |
|--------|------|----------|-------------|
| `id` | `string` | ഇല്ല | പ്രത്യേക ID (പ്ലഗിൻ നാമത്തിന് ഡിഫോൾട്ടായി) |
| `icon` | `string` | അത്യാവശ്യമാണ് | Lucide ഐക്കൺ നാമം (ഉദാഹരണത്തിന്, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | അത്യാവശ്യമാണ് | സൈഡ്‌ബാറിൽ കാണുന്ന ബട്ടൺ ടെക്സ്റ്റ് |
| `onClick` | `() => void` | അത്യാവശ്യമാണ് | ബട്ടൺ ക്ലിക്ക് ചെയ്യുമ്പോൾ വിളിക്കപ്പെടുന്ന ഫംഗ്ഷൻ |
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
**ഐക്കൺ റഫറൻസ്:** ലഭ്യമായ എല്ലാ ഐക്കോണുകൾക്കായി [lucide.dev/icons](https://lucide.dev/icons) സന്ദർശിക്കുക

> **സമാനതാ കുറിപ്പ്:** ��ില പഴയ പ്ലഗിനുകൾ `addSidebarButton(id, icon, label, onClick)` പോലുള്ള സ്ഥാനീയ ആർഗ്യുമെന്റുകൾ ഉപയോഗിക്കുന്നു. ഔദ്യോഗിക API മുകളിൽ രേഖപ്പെടുത്തിയതുപോലെ ഒരു **ഓപ്ഷൻസ് ഒബ്ജക്ട്** ഉപയോഗിക്കുന്നു. പുതിയ പ്ലഗിനുകൾക്കായി എപ്പോഴും ഒബ്ജക്ട് ശൈലി ഉപയോഗിക്കുക.

#### `ui.openWebview(options)`

കസ്റ്റം HTML ഉള്ളടക്കത്തോടെ ഒരു പോപ്-അപ്പ് വിൻഡോ തുറക്കുക. ഇത് സമൃദ്ധമായ UIs നിർമ്മിക്കുന്നതിന് ആണ്.

| ഓപ്ഷൻ | തരം | വിവരണം |
|--------|------|-------------|
| `title` | `string` | വിൻഡോയുടെ ശീർഷകം |
| `html` | `string` | പ്രദർശിപ്പിക്കാൻ മുഴുവൻ HTML ഉള്ളടക്കം |
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
> [ഭാഗം 3](#part-3-building-custom-ui-with-webviews) എന്നതിൽ പുരോഗമന വെബ്‌വ്യൂ പാറ്റേണുകൾ കാണുക.

#### `ui.showNotification(type, message)`

ഒരു ടോസ്റ്റ് നോട്ടിഫിക്കേഷൻ കാണിക്കുക.

| പാരാമീറ്റർ | തരം | വിവരണം |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | നോട്ടിഫിക്കേഷൻ ശൈലി |
| `message` | `string` | കാണിക്കേണ്ട വാചകം |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

താഴത്തെ സ്റ്റാറ്റസ് ബാറിൽ സ്ഥിരമായ വാചക ഇനം ചേർക്കുക.

| പാരാമീറ്റർ | തരം | വിവരണം |
|-----------|------|-------------|
| `id` | `string` | ഈ സ്റ്റാറ്റസ് ഇനത്തിനുള്ള പ്രത്യേക ID |
| `text` | `string` | കാണിക്കേണ്ട വാചകം |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — സ്ഥിരമായ സംഭരണം

പ്ലഗിൻ ക്രമീകരണങ്ങൾ സ്ഥിരമായി `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ൽ സംഭരിക്കുന്നു.

#### `settings.get(key)`

ഒരു സംരക്ഷിത മൂല്യം വായിക്കുക.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
കീ നിലവിലില്ലെങ്കിൽ `undefined` തിരികെ നൽകുന്നു.

#### `settings.set(key, value)`

ഒരു മൂല്യം സംരക്ഷിക്കുക. സ്ട്രിങ്ങുകൾ, സംഖ്യകൾ, ബൂല്യൻ, അറകൾ, ഒബ്ജക്റ്റുകൾ എന്നിവയെ പിന്തുണയ്ക്കുന്നു.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**ഉദാഹരണം: ഉപയോക്തൃ ഇഷ്ടങ്ങൾ ഓർമ്മിക്കുക**
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

### `ctx.ai` — എഐ സംയോജനം

> **സ്ഥിതി: ഉടൻ വരുന്നു** — എഐ എപി‌ഐ നിർവചിച്ചിരിക്കുന്നു, എന്നാൽ ഇപ്പോൾ Soomy-യുമായി ബന്ധിപ്പി��്ചിട്ടില്ല. നിലവിൽ `{ response: 'AI not yet connected' }` തിരികെ നൽകുന്നു. പൂർണ്ണ എഐ സംയോജനം ഭാവിയിലെ റിലീസിനായി പദ്ധതിയിടുന്നു.

#### `ai.chat(messages, options?)`

എഐ അസിസ്റ്റന്റിലേക്ക് (Soomy) സന്ദേശങ്ങൾ അയക്കുക.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## ഭാഗം 3: വെബ്‌വ്യൂ ഉപയോഗിച്ച് കസ്റ്റം UI നിർമ്മിക്കുക

`openWebview()` എപി‌ഐ HTML, CSS, JavaScript എന്നിവ ഉപയോഗിച്ച് ഡാഷ്‌ബോർഡ് UIകൾ നിർമ്മിക്കാൻ അനുവദിക്കുന്നു — എല്ലാം ഒരു പോപ്‌അപ്പ് വിൻഡോയുടെ ഉള്ളിൽ.

> **പ്രധാന പരിധി:** വെബ്‌വ്യൂകൾ **കാണിക്കുന്നതിന് മാത്രം** ആണ്. അവ പ്ലഗിൻ എപി‌ഐകളിലേക്ക് ( `ctx.settings`, `ctx.terminal`, മുതലായവ) തിരിച്ചുകൂടാൻ കഴിയില്ല. എല്ലാ ഉപയോക്തൃ പ്രവർത്തനങ്ങൾക്കായി സൈഡ്‌ബാർ ബട്ടണുകൾ ഉപയോഗിക്കുക, നിലവിലെ സ്ഥിതി കാണിക്കാൻ `openWebview()` ഉപയോഗിക്കുക. ഇന്ററാക്ടീവ് ഫീച്ചറുകൾ ആവശ്യമുണ്ടെങ്കിൽ, അവ സൈഡ്‌ബാർ ബട്ടണുകളിൽ നിന്ന് പ്രേരിപ്പിക്കുക, പ്രദർശനം പുതുക്കാൻ വെബ്‌വ്യൂ വീണ്ടും തുറക്കുക.

### പാറ്റേർൺ: ടെർമിനൽ കമാൻഡ് → ഔട്ട്പുട്ട് പാഴ്സുചെയ്യുക → HTML-ൽ കാണിക്കുക

ഇത് ഏറ്റവും സാധാരണമായ പ്ലഗിൻ പാറ്റേൺ ആണ്. നിങ്ങൾ ഒരു കമാൻഡ് പ്രവർത്തിപ്പിക്കുന്നു, ഫലത്തെ പാഴ്സുചെയ്യുന്നു, അത് ദൃശ്യമായി കാണിക്കുന്നു.
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
### പാറ്റേർൺ: സ്വയം പുതുക്കുന്ന ഇന്ററാക്ടീവ് ഡാഷ്‌ബോർഡ്
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
### പാറ്റേർൺ: വെബ്‌വ്യൂയിൽ ക്രമീകരണങ്ങൾ കാണിക്കുക

> **കുറിപ്പ്:** വെബ്‌വ്യൂകൾ കാണിക്കുന്നതിനാണ് — അവ പ്ലഗിൻ എപി‌ഐകളിലേക്ക് തിരിച്ചുകൂടാൻ കഴിയില്ല. ക്രമീകരണങ്ങൾ മാറ്റാൻ നിങ്ങളുടെ സൈഡ്‌ബാർ ബട്ടൺ ഹാൻഡ്ലറുകളിൽ `ctx.settings` ഉപയോഗിക്കുക, നിലവിലെ സ്ഥിതി കാണിക്കാൻ `openWebview()` ഉപയോഗിക്കുക.
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

## ഭാഗം 4: നിങ്ങളുടെ പ്ലഗിൻ പ്രസിദ്ധീകരിക്കൽ

### ഘട്ടം 1: പ്രാദേശികമായി പരീക്ഷിക്കുക

1. നിങ്ങളുടെ പ്ലഗിൻ `~/.wia-soom/plugins/{your-plugin}/` ലേക്ക് പകർപ്പിക്കുക
2. WIA SOOM പുനരാരംഭിക്കുക
3. ഇത് പ്രവർത്തിക്കുന്നുണ്ടെന്ന് ഉറപ്പാക്കുക: സൈഡ്��ബാർ ബട്ടൺ പ്രത്യക്ഷപ്പെടുന്നു, ഫീച്ചറുകൾ ശരിയായി പ്രവർത്തിക്കുന്നു
4. എഡ്ജ് കേസുകൾ പരീക്ഷിക്കുക: ടെർമിനൽ ബന്ധിപ്പിച്ചില്ലെങ്കിൽ എന്ത് സംഭവിക്കും?

### ഘട്ടം 2: സമർപ്പണത്തിന് തയ്യാറാക്കുക

നിങ്ങളുടെ പ്ലഗിൻ ഫോൾഡറിൽ ഉൾപ്പെടേണ്ടത്:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**ആവശ്യമായ `package.json` ഫീൽഡുകൾ:**

| ഫീൽഡ് | വിവരണം | ഉദാഹരണം |
|-------|-------------|---------|
| `name` | ഏകീകൃത kebab-case ID | `"my-awesome-plugin"` |
| `version` | സെമാന്റിക് പതിപ്പ് | `"1.0.0"` |
| `description` | ഒരു വരി വിവരണം | `"Monitors nginx access logs in real-time"` |
| `author` | നിങ്ങളുടെ പേര് | `"John Doe"` |
| `main` | എൻട്രി പോയിന്റ് | `"index.js"` |

**ഐച്ഛിക ഫീൽഡുകൾ:**

| ഫീൽഡ് | വിവരണം |
|-------|-------------|
| `license` | ലൈസൻസ് തരം (MIT ശുപാർശ ചെയ്യുന്നു) |
| `keywords` | തിരച്ചിൽ ടാഗുകളുടെ ആറെ |
| `soom.minVersion` | ആവശ്യമായ കുറഞ്ഞ WIA SOOM പതിപ്പ് |

### ഘട്ടം 3: പ്ലഗിൻ രജിസ്ട്രിയിൽ സമർപ്പിക്കുക

1. ****Package** your plugin as a ZIP file
2. **Add** നിങ്ങളുടെ പ്ലഗിൻ `plugins/{your-plugin-name}/` ലേക���ക്
3. **Submit** ഒരു Pull Request

### ഘട്ടം 4: അവലോകനം ಮತ್ತು അംഗീകാരം

ഞങ്ങൾ ഓരോ പ്ലഗിനും പരിശോധിക്കുന്നു:

- **സുരക്ഷ** — അപകടകരമായ APIs ഇല്ല (കാണുക [സുരക്ഷാ നിയമങ്ങൾ](#security-rules))
- **ഗുണമേന്മ** — ഇത് പ്രവർത്തിക്കുന്നുണ്ടോ? കോഡ് ശുദ്ധമാണോ?
- **ഉപയോഗിത്വം** — ഇത് യാഥാർത്ഥ്യ പ്രശ്നം പരിഹരിക്കുന്നുണ്ടോ?

അംഗീകാരം ലഭിച്ചതിന് ശേഷം:
1. നിങ്ങളുടെ പ്ലഗിൻ `registry.json` ലേക്ക് ചേർക്കപ്പെടുന്നു
2. `dist/` ലേക്ക് ഒരു ZIP ബണ്ടിൽ സൃഷ്ടിക്കുന്നു
3. നിങ്ങളുടെ പ്ലഗിൻ എല്ലാ WIA SOOM ഉപയോക്താക്കൾക്കായി **Plugin Store** ൽ പ്രത്യക്ഷപ്പെടുന്നു!

---

## ഭാഗം 5: മികച്ച പ്രായോഗികതകൾ

### സുരക്ഷാ നിയമങ്ങൾ

ഈ നിയമങ്ങൾ **കൂടുതൽ നിർബന്ധമാണ്**. ഇവയെ ലംഘിക്കുന്ന പ്ലഗി��ുകൾ നിരസിക്കപ്പെടും.

| നിയമം | എന്തുകൊണ്ട് |
|------|-----|
| **എപ്പോഴും** `eval()` അല്ലെങ്കിൽ `new Function()` ഉപയോഗിക്കരുത് | കോഡ് ഇഞ്ചക്ഷൻ അപകടം |
| **എപ്പോഴും** `child_process`, `exec()`, `spawn()` ഉപയോഗിക്കരുത് | കമാൻഡുകൾക്കായി മാത്രം `ctx.terminal.send()` ഉപയോഗിക്കുക |
| **എപ്പോഴും** പുറം URLs എടുക്കരുത് | വ്യത്യാസം: `wiasoom.com` API എൻഡ്പോയിന്റുകൾ |
| **എപ്പോഴും** `process.env` ആക്‌സസ് ചെയ്യരുത് | പരിസ്ഥിതി വ്യത്യാസങ്ങൾ രഹസ്യങ്ങൾ അടങ്ങിയിരിക്കാം |
| **എപ്പോഴും** `require('fs')` നേരിട്ട് ഉപയോഗിക്കരുത് | സംഭരണത്തിനായി `ctx.settings` ഉപയോഗിക്കുക, ഫയൽ കൈമാറ്റത്തിനായി `ctx.sftp` ഉപയോഗിക്കുക |
| **എപ്പോഴും** npm പുറം പാക്കേജ��കൾ ഉപയോഗിക്കരുത് | ശുദ്ധമായ ജാവാസ്ക്രിപ്റ്റ് മാത്രം — node_modules ഇല്ല |
| **ഉപയോഗിക്കണം** എല്ലാ ദൂര കമാൻഡുകൾക്കായി `ctx.terminal.send()` | ഇത് സുരക്ഷിതമായ SSH ചാനലിലൂടെ പോകുന്നു |
| **ഉപയോഗിക്കണം** `deactivate()` ൽ ക്ലീൻ അപ്പ് ചെയ്യുക | ലിസണർമാർ നീക്കം ചെയ്യുക, ഇടവേളകൾ ക്ലിയർ ചെയ്യുക |

### പിശക് കൈകാര്യം ചെയ്യൽ

എപ്പോഴും അപകടകരമായ പ്രവർത്തനങ്ങൾ try/catch ൽ മൂടുക:
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
### deactivate() ൽ ക്ലീൻ അപ്പ്

നിങ്ങളുടെ പ്ലഗിൻ ഇടവേളകൾ, ലിസണർമാർ, അല്ലെങ്കിൽ സബ്സ്ക്രിപ്ഷനുകൾ സൃഷ്ടിച്ചാൽ — അവ ക്ലീൻ ചെയ്യുക:
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
### i18n പിന്തുണ

WIA SOOM 254 ഭാഷകൾ പിന്തുണയ്ക്കുന്നു. നിങ്ങളുടെ പ്ലഗിൻ ലേബൽ വിവർത്തനയോഗ്യമായതാക്കാൻ, ഒരു ലളിതമായ സമീപനം ഉപയോഗിക്കുക:
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

## ഭാഗം 6: യാഥാർത്ഥ്യ ഉദാഹരണങ്ങൾ

### ഉദാഹരണം 1: സർവർ ഡിസ്ക് ചെക്കർ

ദൂര സർവറിൽ `df -h` പ്രവർത്തിപ്പിച്ച് സ്റ്റാറ്റസ് ബാറിൽ ഉപയോഗിച്ച/ലഭ്യമായ സ്ഥലം കാണിക്കുന്നു.
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

### ഉദാഹരണം 2: TODO മാനേജർ

സ്ഥിരമായ സംഭരണത്തിനായി ക്രമീകരണങ്ങൾ ഉപയോഗിച്ച് TODO പട്ടികയെ നിയന്ത്രിക്കുന്ന ഒരു പ്ലഗിൻ, പ്രദർശനത്തിനായി ഒരു വെബ്‌വ്യൂ ഉപയോഗിക്കുന്നു.

> **ഡിസൈൻ മാതൃക:** വെബ്‌വ്യൂകൾ നേരിട്ട് പ്ലഗിൻ APIs വിളിക്കാനാവില്ല, ഈ പ്ലഗിൻ "സ്നാപ്ഷോട്ട്" സമീപനം ഉപയോഗിക്കുന്നു — ഇത് ക്രമീകരണങ്ങളിൽ നിന്ന് TODOകൾ വായിക്കുന്നു, അവയെ വായനയ്ക്ക് മാത്രം HTML ആയി-render ചെയ്യുന്നു, കൂടാതെ ഇനങ്ങൾ ചേർക്കുന്നതിനായി സൈഡ്‌ബാർ അടിസ്ഥാനമാക്കിയുള്ള പ്രവർത്തനങ്ങൾ നൽകുന്നു. വെബ്‌വ്യൂ ഒരു **പ്രദർശന** പാളിയാണ്, പരസ്പര പ്രവർത്തന ഫോമല്ല.
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

### ഉദാഹരണം 3: പിശക് വാചകങ്ങൾ

ടെർമിനൽ ഔട്ട്‌പുട്ട് നിരീക്ഷിച്ച് പ്രത്യേക പാറ്റേണുകൾ കണ്ടെത്തുമ്പോൾ ഒരു അറിയിപ്പ് അയക്കുന്നു.
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

## അനുബന്ധം: വിഭാഗങ്ങൾ & ഐക്കണുകൾ

### പ്ലഗിൻ വിഭാഗങ്ങൾ (29)

നിങ്ങളുടെ `package.json` `keywords`-ൽ അല്ലെങ്കിൽ രജിസ്ട്രിയിൽ സമർപ്പിക്കുമ്പോൾ ഇവ ഉപയോഗിക്കുക:

| വിഭാഗം | വിവരണം |
|--------|---------|
| `server` | പൊതുവായ സെർവർ മാനേജ്മെന്റ് |
| `devtools` | വികസന ഉപകരണങ്ങൾ |
| `calculator` | കാൽക്കുലേറ്ററുകൾ & പരിവർത്തകർ |
| `simulator` | സിമുലേറ്ററുകൾ |
| `game` | ടെർമിനൽ ഗെയിമുകൾ |
| `business` | ബിസിനസ് ഉപകരണങ്ങൾ |
| `security` | സുരക്ഷ & ഓഡിറ്റിംഗ് |
| `web` | വെബ് സെർവർ മാനേജ്മെന്റ് |
| `education` | വിദ്യാഭ്യാസ ഉപകരണങ്ങൾ |
| `health` | ആരോഗ്യ സംബന്ധമായ ഉപകരണങ്ങൾ |
| `islamic` | ഇസ്ലാമിക് ഉപകരണങ്ങൾ (പ്രാർത്ഥന സമയം, മുതലായവ) |
| `science` | ശാസ്ത്രീയ ഉപകരണങ്ങൾ |
| `quantum` | ക്വാണ്ടം കമ്പ്യൂട്ടിംഗ് ഉപകരണങ്ങൾ |
| `ai` | എ.ഐ. ശക്തിയുള്ള ഉപകരണങ്ങൾ |
| `biotech` | ബയോ ടെക്‌നോളജി ഉപകരണങ്ങൾ |
| `space` | ബഹിരാകാശ & ജ്യോതിശാസ്ത്ര ഉപകരണങ്ങൾ |
| `network` | നെറ്റ്‌വർക്കിംഗ് ഉപകരണങ്ങൾ |
| `database` | ഡാറ്റാബേസ് മാനേജ്മെന്റ് |
| `monitoring` | സെർവർ നിരീക്ഷണം |
| `devops` | ഡെവ്‌ഓപ്പ്സ് & CI/CD |
| `utility` | പൊതുവായ ഉപകരണങ്ങൾ |
| `design` | ഡിസൈൻ ഉപകരണങ്ങൾ |
| `ecommerce` | ഇ-കൊമേഴ്‌സ് ഉപകരണങ്ങൾ |
| `automation` | ഓട്ടോമേഷൻ ഉപകരണങ്ങൾ |
| `kpop` | K-pop സംബന്ധമായ ഉപകരണങ്ങൾ |
| `accessibility` | ആക്സസിബിലിറ്റി ഉപകരണങ്ങൾ |
| `analytics` | വിശകലന & റിപ്പോർട���ടിംഗ് |
| `wia` | WIA ഇക്കോസിസ്റ്റം ഉപകരണങ്ങൾ |
| `all` | എല്ലാ വിഭാഗങ്ങളിലും പ്രത്യക്ഷപ്പെടുന്നു |

### ശുപാർശ ചെയ്ത ഐക്കണുകൾ (Lucide)

| ഐക്കൺ നാമം | ഉപയോഗം |
|--------------|---------|
| `server` | സെർവർ മാനേജ്മെന്റ് |
| `shield` | സുരക്ഷ |
| `database` | ഡാറ്റാബേസ് |
| `activity` | നിരീക്ഷണം |
| `terminal` | ടെർമിനൽ ഉപകരണങ്ങൾ |
| `code` | വികസനം |
| `hard-drive` | ഡിസ്‌ക്/സ്റ്റൊറേജ് |
| `network` | നെറ്റ്വർകിംഗ് |
| `lock` | പ്രാമാണികത/എൻക്രിപ്ഷൻ |
| `eye` | കാണൽ/നിരീക്ഷണം |
| `check-square` | പ്രവർത്തനങ്ങൾ/TODO |
| `layout-dashboard` | ഡാഷ്ബോർഡുകൾ |
| `settings` | കോൺഫിഗറേഷൻ |
| `zap` | ഓട്ടോമേഷൻ |
| `globe` | വെബ്/അന്താരാഷ്ട്ര |

എല്ലാ 1,500+ ഐക്കണുകളും ബ്രൗസ് ചെ���്യുക: [lucide.dev/icons](https://lucide.dev/icons)

---

## സഹായം വേണോ?

- **GitHub പ്രശ്നങ്ങൾ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **പ്ലഗിൻ പ്രശ്നങ്ങൾ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ഉദാഹരണ പ്ലഗിൻസ്:** [Website](https://wiasoom.com)
- **വെബ്സൈറ്റ്:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>അദ്ഭുതകരമായ ഒന്നൊരുക്കുക. ലോകത്തോടു പങ്കുവയ്ക്കുക.</em></p>
<p align="center"><em>— WIA SOOM ടീം</em></p>