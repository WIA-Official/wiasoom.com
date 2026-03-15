<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin ናይ ኣማዕበልቲ መምርሒ</h1>
<p align="center"><strong>ናይ ገዛእ ርእስኻ plugin ኣብ 5 ደቒቕ ስርሓዮ።</strong></p>
<p align="center">ሓያላት ናይ ሰርቨር መሳርሒታት፣ ዳሽቦርዳት፣ ከምኡ'ውን ኣውቶመሽናት — ቀጥታ ኣብ ውሽጢ WIA SOOM ፍጠር።</p>

---

## ትሕዝቶ

- [ክፋል 1: ቅልጡፍ ጅማሮ — ቀዳማይ Plugin ናትካ ኣብ 5 ደቒቕ](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ክፋል 2: Plugin Context API ማጣቀሻ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ክፋል 3: ብ Webviews ብሕታዊ UI ምህናጽ](#part-3-building-custom-ui-with-webviews)
- [ክፋል 4: Plugin ካ ምሕታም](#part-4-publishing-your-plugin)
- [ክፋል 5: ብሉጻት ኣሰራርሓታት](#part-5-best-practices)
- [ክፋል 6: ናይ ብሓቂ ዓለም ኣብነታት](#part-6-real-world-examples)
- [ተወሳኺ: ምድባት ከምኡ'ውን ኣይኮናት](#appendix-categories--icons)

---

## ክፋል 1: ቅልጡፍ ጅማሮ — ቀዳማይ Plugin ናትካ ኣብ 5 ደቒቕ

### እንታይ ከም እትሰርሕ

ኣብ ጎድኒ ባር ዝርከብ መልጎም ዝውስኽ "Hello World" plugin። ምስ ጠወቕካዮ ምልክታ የርኢ።

### ስጉምቲ 1: ናይ plugin ፎልደር ፍጠር

```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```

### ስጉምቲ 2: package.json ፍጠር

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

**ዘድልዩ ቦታታት:** `name`፣ `version`፣ `description`፣ `author`፣ `main`

### ስጉምቲ 3: index.js ፍጠር

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

### ስጉምቲ 4: WIA SOOM ዳግማይ ኣበግሶ

ኣፕሊኬሽን ዳግማይ ኣበግሶ (ወይ ኣብ ምቕማጦታት → Plugins ነቲ plugin ኣጥፍኣዮ/ኣብርሆ)።

ኣብ ጎድኒ ባር **"Hello World"** መልጎም ክትርኢ ኣለካ። ጠውቖ — ናይ ዓወት ምልክታ ክትርኢ ኢኻ!

### ከመይ ከም ዝሰርሕ

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

## ክፋል 2: Plugin Context API ማጣቀሻ

ናይ `activate(context)` ፋንክሽንካ ምስ ተጸውዐ፣ `context` (ወይ `ctx`) እዞም APIታት ይህብ:

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

### `ctx.terminal` — ኣብ ርሑቕ ሰርቨራት ትእዛዛት ምፍጻም

#### `terminal.send(sessionId, data)`

ናብ ንጡፍ ናይ ተርሚናል ጊዜ ትእዛዝ (ወይ ዝኾነ ዳታ) ስደድ።

| ፓራሚተር | ዓይነት | መግለጺ |
|-----------|------|-------------|
| `sessionId` | `string` | ዝለኣኸሉ ናይ ተርሚናል ጊዜ |
| `data` | `string` | ዝለኣኽ ትእዛዝ ወይ ዳታ |

```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```

#### `terminal.onOutput(sessionId, callback)`

ካብ ናይ ተርሚናል ጊዜ ንዝኾነ ውጽኢት ተመዝገብ። **ናይ ምስረዝ ምዝገባ ፋንክሽን** ይመልስ።

| ፓራሚተር | ዓይነት | መግለጺ |
|-----------|------|-------------|
| `sessionId` | `string` | ዝከታተሎ ናይ ተርሚናል ጊዜ |
| `callback` | `(data: string) => void` | ንነፍሲ ወከፍ ናይ ውጽኢት ክፋል ዝጽዋዕ |
| **ይመልስ** | `() => void` | ምስማዕ ንምቛም ነዚ ጸውዖ |

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

**ኣገዳሲ:** ናይ ምስረዝ ምዝገባ ፋንክሽን ኩሉ ግዜ ዓቕቦ ከምኡ'ውን ኣብ `deactivate()` ጸውዖ ናይ ዝኽሪ ምዝሕሓል ንምክልኻል።

---

### `ctx.sftp` — ናይ ፋይል ምስግጋር

> **ኩነታት: ቀሪቡ ይመጽእ** — SFTP API ተገሊጹ ኣሎ ግን ገና ምስ ናይ ኣፕ SFTP ሞተር ኣይተተሓሓዘን። `list()` ብዝሓ ባዶ ዝርዝር ይመልስ፣ ከምኡ'ውን `upload()`/`download()` ስራሕ ኣይገብሩን። ኣብ ዝመጽእ ዝወጽእ ምሉእ ብምሉእ ክትግበር እዩ። ንሕዚ ከም ኣማራጺ ፍታሕ `ctx.terminal.send()` ምስ `scp` ወይ `rsync` ትእዛዛት ተጠቐም።

#### `sftp.list(sessionId, path)`

ኣብ ርሑቕ ማህደር ዝርከቡ ፋይላት ዝርዝር።

```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```

#### `sftp.upload(sessionId, localPath, remotePath)`

ካብ ከባብያዊ ማሽን ናብ ርሑቕ ሰርቨር ፋይል ጸዓን።

```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```

#### `sftp.download(sessionId, remotePath, localPath)`

ካብ ርሑቕ ሰርቨር ናብ ከባብያዊ ማሽን ፋይል ኣውርድ።

```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```

**ኣማራጺ ፍታሕ (SFTP API ክሳብ ዝነጥፍ):**

```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```

---

### `ctx.ui` — ናይ ተጠቃሚ ኢንተርፌስ

#### `ui.addSidebarButton(options)`

ናብ WIA SOOM ጎድኒ ባር መልጎም ወስኽ።

| ኣማራጺ | ዓይነት | ዘድሊ | መግለጺ |
|--------|------|----------|-------------|
| `id` | `string` | ኣይፋል | ፍሉይ መለለዪ (ኣብ ዘይህሉ ናይ plugin ስም ይጥቀም) |
| `icon` | `string` | እወ | Lucide ናይ ኣይኮን ስም (ንኣብነት `'server'`፣ `'shield'`፣ `'database'`) |
| `label` | `string` | እወ | ኣብ ጎድኒ ባር ዝርአ ናይ መልጎም ጽሑፍ |
| `onClick` | `() => void` | እወ | መልጎም ምስ ተጠውቐ ዝጽዋዕ ፋንክሽን |

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

**ናይ ኣይኮን ማጣቀሻ:** ኩሎም ዘለዉ ኣይኮናት ኣብ [lucide.dev/icons](https://lucide.dev/icons) ረኣዮም

> **ናይ ተኸታተልነት ማስታወሻ:** ገሊኦም ኣረጊት plugins ከም `addSidebarButton(id, icon, label, onClick)` ዝኣመሰሉ ናይ ቦታ ክርክራት ይጥቀሙ። ወግዓዊ API ከምቲ ኣብ ላዕሊ ዝተገለጸ **ናይ ኣማራጺ ነገር** ይጥቀም። ንሓደሽቲ plugins ኩሉ ግዜ ናይ ነገር ስልቲ ተጠቐም።

#### `ui.openWebview(options)`

ብሕታዊ HTML ትሕዝቶ ዘለዎ ፖፕኣፕ መስኮት ክፈት። ብከምዚ ብሉጻት UIታት ትሰርሕ።

| ኣማራጺ | ዓይነት | መግለጺ |
|--------|------|-------------|
| `title` | `string` | ናይ መስኮት ኣርእስቲ |
| `html` | `string` | ዝቐርብ ምሉእ HTML ትሕዝቶ |

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

> ንዝሰፍሑ ናይ webview ቅዲታት [ክፋል 3](#part-3-building-custom-ui-with-webviews) ረአ።

#### `ui.showNotification(type, message)`

ናይ ቶስት ምልክታ ኣርኢ።

| ፓራሚተር | ዓይነት | መግለጺ |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ናይ ምልክታ ስልቲ |
| `message` | `string` | ዝረአ ጽሑፍ |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

ኣብ ታሕተዋይ ኩነታት ባር ቀዋሚ ናይ ጽሑፍ ኣቕሓ ወስኽ።

| ፓራሚተር | ዓይነት | መግለጺ |
|-----------|------|-------------|
| `id` | `string` | ንዚ ናይ ኩነታት ኣቕሓ ፍሉይ መለለዪ |
| `text` | `string` | ዝቐርብ ጽሑፍ |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — ቀዋሚ ማኽዘን

ናይ Plugin ምቕማጦታት ቀዋሚ ብዝኾነ መንገዲ ኣብ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ይዕቀቡ።

#### `settings.get(key)`

ዝተዓቀበ ዋጋ ኣንብብ።

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

እቲ ቁልፊ ዘይህሉ እንተ ኮይኑ `undefined` ይመልስ።

#### `settings.set(key, value)`

ዋጋ ዓቅቦ። ሕብረ ቃላት፣ ቁጽርታት፣ ቡሊያናት፣ ዝርዝራት፣ ከምኡ'ውን ነገራት ይድግፍ።

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**ኣብነት: ናይ ተጠቃሚ ምርጫታት ኣዘክር**

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

### `ctx.ai` — AI ምትእስሳር

> **ኩነታት: ቀሪቡ ይመጽእ** — AI API ተገሊጹ ኣሎ ግን ገና ምስ Soomy ኣይተተሓሓዘን። ብዝሓ `{ response: 'AI not yet connected' }` ይመልስ። ምሉእ ናይ AI ምትእስሳር ኣብ ዝመጽእ ዝወጽእ ይሕሰብ ኣሎ።

#### `ai.chat(messages, options?)`

ናብ AI ሓጋዚ (Soomy) መልእኽትታት ስደድ።

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## ክፋል 3: ብ Webviews ብሕታዊ UI ምህናጽ

`openWebview()` API ብ HTML፣ CSS፣ ከምኡ'ውን JavaScript ናይ ዳሽቦርድ UIታት ክትሰርሕ ይፈቕደልካ — ኩሉ ኣብ ውሽጢ ፖፕኣፕ መስኮት።

> **ኣገዳሲ ገደብ:** Webviews **ንምርኣይ ጥራይ** እዮም። ናብ plugin APIታት (`ctx.settings`፣ `ctx.terminal`፣ ወዘተ) ክጽውዑ ኣይክእሉን። ንኩሎም ናይ ተጠቃሚ ተግባራት ናይ ጎድኒ ባር መልጎማት ተጠቐም፣ ከምኡ'ውን ሕጂ ዘሎ ኩነታት ንምርኣይ `openWebview()` ተጠቐም። ኢንተራክቲቭ ባህርያት ዘድሊ እንተ ኮይኑ ካብ ናይ ጎድኒ ባር መልጎማት ኣበግሶም ከምኡ'ውን ምርኣይ ንምሕዳስ webview ዳግማይ ክፈት።

### ቅዲ: ናይ ተርሚናል ትእዛዝ → ውጽኢት ተንተን → ብ HTML ኣርኢ

እዚ እቲ ልሙድ ናይ plugin ቅዲ እዩ። ትእዛዝ ተካይድ፣ ውጽኢት ትተንትን፣ ከምኡ'ውን ብዓይኒ ዝርአ ብዝኾነ መንገዲ ተቐርቦ።

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

### ቅዲ: ብኣውቶ-ምሕዳስ ዝሰርሕ ኢንተራክቲቭ ዳሽቦርድ

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

### ቅዲ: ኣብ Webview ምቕማጦታት ምርኣይ

> **ማስታወሻ:** Webviews ንምርኣይ ጥራይ እዮም — ናብ plugin APIታት ክጽውዑ ኣይክእሉን። ምቕማጦታት ንምቕያር ኣብ ናይ ጎድኒ ባር መልጎም handlers ካ `ctx.settings` ተጠቐም፣ ከምኡ'ውን ሕጂ ዘሎ ኩነታት ንምርኣይ `openWebview()` ተጠቐም።

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

## ክፋል 4: Plugin ካ ምሕታም

### ስጉምቲ 1: ብከባብያዊ ፈትን

1. Plugin ካ ናብ `~/.wia-soom/plugins/{your-plugin}/` ቅዳሕ
2. WIA SOOM ዳግማይ ኣበግስ
3. ከም ዝሰርሕ ኣረጋግጽ: ናይ ጎድኒ ባር መልጎም ይርአ፣ ባህርያት ብግቡእ ይሰርሑ
4. ናይ ጠርዚ ኩነታት ፈትን: ዝኾነ ተርሚናል ዘይተተሓሓዘ እንተ ኮይኑ እንታይ ይኸውን?

### ስጉምቲ 2: ንምቕራብ ተዳሎ

ናይ plugin ፎልደርካ እዚኦም ክሓዝ ኣለዎ:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**ዘድልዩ ናይ `package.json` ቦታታት:**

| ቦታ | መግለጺ | ኣብነት |
|-------|-------------|---------|
| `name` | ፍሉይ kebab-case መለለዪ | `"my-awesome-plugin"` |
| `version` | ስማንቲክ ቨርዥን | `"1.0.0"` |
| `description` | ሓደ-መስመር መግለጺ | `"Monitors nginx access logs in real-time"` |
| `author` | ስምካ | `"John Doe"` |
| `main` | ናይ ኣተዊ ነጥቢ | `"index.js"` |

**ኣማራጺ ቦታታት:**

| ቦታ | መግለጺ |
|-------|-------------|
| `license` | ናይ ፍቓድ ዓይነት (MIT ይምከር) |
| `keywords` | ናይ ምድላይ ተግታት ዝርዝር |
| `soom.minVersion` | ዘድሊ ዝተሓተ ናይ WIA SOOM ቨርዥን |

### ስጉምቲ 3: ናብ Plugin ምዝገባ ኣቕርብ

1. [Plugin Store](https://wiasoom.com) **ፎርክ ግበር**
2. Plugin ካ ናብ `plugins/{your-plugin-name}/` **ወስኾ**
3. Pull Request **ኣቕርብ**

### ስጉምቲ 4: ግምገማ ከምኡ'ውን ፍቓድ

ንነፍሲ ወከፍ plugin ንእዚ ንግምግሞ:

- **ድሕንነት** — ሓደገኛ APIታት የብሉን (ረአ [ናይ ድሕንነት ሕግታት](#security-rules))
- **ጽሬት** — ይሰርሕ ድዩ? ኮድ ጽሩይ ድዩ?
- **ጠቕሚ** — ናይ ብሓቂ ጸገም ይፈትሕ ድዩ?

ድሕሪ ፍቓድ:
1. Plugin ካ ናብ `registry.json` ይውሰኽ
2. ZIP ጥቕሉል ኣብ `dist/` ይፍጠር
3. Plugin ካ ንኹሎም ናይ WIA SOOM ተጠቀምቲ ኣብ **Plugin Store** ይርአ!

---

## ክፋል 5: ብሉጻት ኣሰራርሓታት

### ናይ ድሕንነት ሕግታት

እዞም ሕግታት **ግዴታ** እዮም። ዝጥሕሱ plugins ክንጸጉ እዮም።

| ሕጊ | ስለምንታይ |
|------|-----|
| `eval()` ወይ `new Function()` **ፈጺምካ ኣይትጠቐም** | ናይ ኮድ ምእታው ስግኣት |
| `child_process`፣ `exec()`፣ `spawn()` **ፈጺምካ ኣይትጠቐም** | ንትእዛዛት `ctx.terminal.send()` ጥራይ ተጠቐም |
| ናይ ደገ URLታት **ፈጺምካ ኣይትረኽብ** | ፍሉይ ኩነት: `wiasoom.com` API ናይ መወዳእታ ነጥብታት |
| `process.env` **ፈጺምካ ኣይትጥቐም** | ናይ ከባቢ ተቐያይሮታት ምስጢራት ክሓዙ ይኽእሉ |
| ብቐጥታ `require('fs')` **ፈጺምካ ኣይትጠቐም** | ንማኽዘን `ctx.settings`፣ ንናይ ፋይል ምስግጋር `ctx.sftp` ተጠቐም |
| npm ናይ ደገ ጥቕሉላት **ፈጺምካ ኣይትጠቐም** | ንጹህ JavaScript ጥራይ — node_modules የለን |
| ንኹሎም ናይ ርሑቕ ትእዛዛት `ctx.terminal.send()` **ክትጥቀም ኣለካ** | እዚ ብውሑስ SSH ቻነል ይሓልፍ |
| ኣብ `deactivate()` **ከተጽሪ ኣለካ** | ኣዳመጽቲ ኣልግስ፣ ኢንተርቫላት ኣጽሪ |

### ናይ ጌጋ ኣተሓሕዛ

ኩሉ ግዜ ስግኣት ዘለዎም ስርሒታት ኣብ try/catch ግበሮም:

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

### ኣብ deactivate() ምጽራይ

Plugin ካ ኢንተርቫላት፣ ኣዳመጽቲ፣ ወይ ምዝገባታት ዝፈጥር እንተ ኮይኑ — ኣጽሪዮም:

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

### i18n ደገፍ

WIA SOOM 254 ቋንቋታት ይድግፍ። ናይ plugin ላበል ካ ክትርጎም ዝኽእል ንምግባር ቅልል ዝበለ ኣገባብ ተጠቐም:

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

## ክፋል 6: ናይ ብሓቂ ዓለም ኣብነታት

### ኣብነት 1: ናይ ሰርቨር ዲስክ መመርመሪ

ኣብ ርሑቕ ሰርቨር `df -h` የካይድ ከምኡ'ውን ዝተጠቐመ/ዘሎ ቦታ ኣብ ኩነታት ባር ይርኢ።

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

### ኣብነት 2: TODO ኣመሓዳሪ

ንቀዋሚ ማኽዘን ምቕማጦታት ከምኡ'ውን ንምርኣይ webview ዝጥቀም TODO ዝርዝር ዘመሓድር plugin።

> **ናይ ዲዛይን ቅዲ:** Webviews ብቐጥታ plugin APIታት ክጽውዑ ስለ ዘይክእሉ፣ እዚ plugin "ስናፕሾት" ኣገባብ ይጥቀም — TODOs ካብ ምቕማጦታት ይነብብ፣ ከም ንንባብ ጥራይ HTML የቕርቦም፣ ከምኡ'ውን ኣቕሓ ንምውሳኽ ናይ ጎድኒ ባር ተግባራት ይህብ። Webview **ናይ ምርኣይ** ደርቢ እዩ፣ ኢንተራክቲቭ ቅጥዒ ኣይኮነን።

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

### ኣብነት 3: ናይ ጌጋ ተኸታታሊ

ናይ ተርሚናል ውጽኢት ይከታተል ከምኡ'ውን ፍሉያት ቅዲታት ምስ ተረኽቡ ምልክታ ይልእኽ።

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

## ተወሳኺ: ምድባት ከምኡ'ውን ኣይኮናት

### ናይ Plugin ምድባት (29)

ኣብ `package.json` `keywords` ካ ወይ ናብ ምዝገባ ክትቅርብ ከለኻ እዚኦም ተጠቐም:

| ምድብ | መግለጺ |
|----------|-------------|
| `server` | ሓፈሻዊ ናይ ሰርቨር ምሕደራ |
| `devtools` | ናይ ልምዓት መሳርሒታት |
| `calculator` | ማእቀሊታት ከምኡ'ውን ለወጥቲ |
| `simulator` | ሲሙሌተራት |
| `game` | ናይ ተርሚናል ጸወታታት |
| `business` | ናይ ንግዲ መሳርሒታት |
| `security` | ድሕንነት ከምኡ'ውን ምርመራ |
| `web` | ናይ ወብ ሰርቨር ምሕደራ |
| `education` | ናይ ትምህርቲ መሳርሒታት |
| `health` | ምስ ጥዕና ዝተተሓሓዙ መሳርሒታት |
| `islamic` | ናይ እስልምና መሳርሒታት (ግዜ ሰላት፣ ወዘተ) |
| `science` | ናይ ሳይንስ መሳርሒታት |
| `quantum` | ናይ ኳንተም ኮምፕዩቲንግ መሳርሒታት |
| `ai` | ብ AI ዝሰርሑ መሳርሒታት |
| `biotech` | ናይ ባዮቴክኖሎጂ መሳርሒታት |
| `space` | ናይ ህዋን ኮኸብ ጥናትን መሳርሒታት |
| `network` | ናይ መርበብ መሳርሒታት |
| `database` | ናይ ዳታቤዝ ምሕደራ |
| `monitoring` | ናይ ሰርቨር ክትትል |
| `devops` | DevOps ከምኡ'ውን CI/CD |
| `utility` | ሓፈሻዊ ጠቐምቲ |
| `design` | ናይ ዲዛይን መሳርሒታት |
| `ecommerce` | ናይ ኢ-ኮመርስ መሳርሒታት |
| `automation` | ናይ ኣውቶመሽን መሳርሒታት |
| `kpop` | ምስ K-pop ዝተተሓሓዙ መሳርሒታት |
| `accessibility` | ናይ ተበጻሕነት መሳርሒታት |
| `analytics` | ትንተና ከምኡ'ውን ጸብጻብ |
| `wia` | ናይ WIA ኢኮሲስተም መሳርሒታት |
| `all` | ኣብ ኩሎም ምድባት ይርአ |

### ዝምከሩ ኣይኮናት (Lucide)

| ናይ ኣይኮን ስም | ንምንታይ ይጠቅም |
|-----------|---------|
| `server` | ናይ ሰርቨር ምሕደራ |
| `shield` | ድሕንነት |
| `database` | ዳታቤዝ |
| `activity` | ክትትል |
| `terminal` | ናይ ተርሚናል መሳርሒታት |
| `code` | ልምዓት |
| `hard-drive` | ዲስክ/ማኽዘን |
| `network` | ናይ መርበብ |
| `lock` | ምረጋገጽ/ምስጢራዊ ምልክት |
| `eye` | ምዕዛብ/ክትትል |
| `check-square` | ዕማማት/TODO |
| `layout-dashboard` | ዳሽቦርዳት |
| `settings` | ውቅር |
| `zap` | ኣውቶመሽን |
| `globe` | ወብ/ዓለምለኻዊ |

ኩሎም 1,500+ ኣይኮናት ኣብዚ ረኣዮም: [lucide.dev/icons](https://lucide.dev/icons)

---

## ሓገዝ ደሊኻ?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ናይ ኣብነት Plugins:** [Website](https://wiasoom.com)
- **ወብሳይት:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ዘደንቕ ነገር ስርሓዮ። ምስ ዓለም ኣካፍሎ።</em></p>
<p align="center"><em>— ናይ WIA SOOM ጋንታ</em></p>
