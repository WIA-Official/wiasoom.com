<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM प्लगइन विकासकर्ता मार्गदर्शिका</h1>
<p align="center"><strong>5 मिनेटमा आफ्नो प्लगइन बनाउनुहोस्।</strong></p>
<p align="center">शक्तिशाली सर्भर उपकरणहरू, ड्यासबोर्डहरू, र स्वचालनहरू सिर्जना गर्नुहोस् — WIA SOOM भित्रै।</p>

---

## सामग्रीको तालिका

- [भाग 1: चाँडो सुरु गर्नुहोस् — 5 मिनेटमा तपाईंको पहिलो प्लगइन](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [भाग 2: प्लगइन सन्दर्भ API](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [भाग 3: वेबभ्यूहरूमा अनुकूल UI निर्म��ण](#part-3-building-custom-ui-with-webviews)
- [भाग 4: तपाईंको प्लगइन प्रकाशित गर्नुहोस्](#part-4-publishing-your-plugin)
- [भाग 5: उत्तम अभ्यासहरू](#part-5-best-practices)
- [भाग 6: वास्तविक-विश्व उदाहरणहरू](#part-6-real-world-examples)
- [परिशिष्ट: श्रेणीहरू र चिह्नहरू](#appendix-categories--icons)

---

## भाग 1: चाँडो सुरु गर्नुहोस् — 5 मिनेटमा तपाईंको पहिलो प्लगइन

### के तपाईंले निर्माण गर्नुहुनेछ

एक "Hello World" प्लगइन जसले साइडबारमा एक बटन थप्छ। क्लिक गर्दा, यो एक सूचना देखाउँछ।

### चरण 1: प्लगइन फोल्डर सिर्जना गर्नुहोस्
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### चरण 2: package.json सिर्जना गर्नुहोस्
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
**आवश्यक क्षेत्रहरू:** `name`, `version`, `description`, `author`, `main`

### चरण 3: index.js सिर��जना गर्नुहोस्
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
### चरण 4: WIA SOOM पुनः सुरु गर्नुहोस्

एप्लिकेशन पुनः सुरु गर्नुहोस् (वा सेटिङहरू → प्लगइनहरूमा प्लगइनलाई बन्द/खोल्नुहोस्)।

तपाईंले साइडबारमा एक **"Hello World"** बटन देख्नु पर्नेछ। यसलाई क्लिक गर्नुहोस् — तपाईंले एक सफल सूचना देख्नु हुनेछ!

### यो कसरी काम गर्छ
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

## भाग 2: प्लगइन सन्दर्भ API

जब तपाईंको `activate(context)` कार्यलाई कल गरिन्छ, `context` (वा `ctx`) यी API हरू प्रदान गर्दछ:
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

### `ctx.terminal` — दूरस्थ सर्भरमा आदेशहरू चलाउनुहोस्

#### `terminal.send(sessionId, data)`

एक सक्रिय टर्मिनल सत्रमा आदेश (वा कुनै पनि डेटा) पठाउनुहोस्।

| प्यारामिटर | प्रकार | विवरण |
|------------|-------|--------|
| `sessionId` | `string` | पठाउनको लागि टर्मिनल सत्र |
| `data` | `string` | पठाउनको लागि आदेश वा डेटा |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

एक टर्मिनल सत्रबाट सबै आउटपुटमा सदस्यता लिनुहोस्। एक **अनसब्सक्राइब कार्य** फिर्ता गर्छ।

| प्यारामिटर | प्रकार | विवरण |
|------------|-------|--------|
| `sessionId` | `string` | हेर्नको लागि टर्मिनल सत्र |
| `callback` | `(data: string) => void` | प्रत्येक आउटपुटको टुक्रामा कल गरिन्छ |
| **फिर्ता** | `() => void` | सुन्न रोक्नको लागि यसलाई कल गर्नुहोस् |
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
**महत्वपूर��ण:** सधैं अनसब्सक्राइब कार्यलाई बचत गर्नुहोस् र `deactivate()` मा यसलाई कल गर्नुहोस् ताकि मेमोरी लिकहरू रोक्न सकियोस्।

---

### `ctx.sftp` — फाइल स्थानान्तरण

> **स्थिति: चाँडै आउनेछ** — SFTP API परिभाषित गरिएको छ तर अझै एप्लिकेशनको SFTP इन्जिनमा जडान गरिएको छैन। `list()` हाल एक खाली एरे फिर्ता गर्छ, र `upload()`/`download()` कुनै कार्य गर्दैन। यो भविष्यको रिलिजमा पूर्ण रूपमा कार्यान्वयन गरिनेछ। हालको लागि, `scp` वा `rsync` आदेशहरूसँग `ctx.terminal.send()` प्रयोग गर्नुहोस्।

#### `sftp.list(sessionId, path)`

दूरस्थ डाइरेक्टरीमा फाइलहरूको सूची बनाउनुहोस्।
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

स्थानीय मेसिनब���ट दूरस्थ सर्भरमा एक फाइल अपलोड गर्नुहोस्।
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

दूरस्थ सर्भरबाट स्थानीय मेसिनमा एक फाइल डाउनलोड गर्नुहोस्।
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**काम गर्ने तरिका (SFTP API सक्रिय नभएसम्म):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — प्रयोगकर्ता अन्तरफलक

#### `ui.addSidebarButton(options)`

WIA SOOM साइडबारमा एक बटन थप्नुहोस्।

| विकल्प | प्रकार | आवश्यक | विवरण |
|--------|-------|--------|--------|
| `id` | `string` | छैन | अद्वितीय ID (प्लगइन नाममा डिफल्ट) |
| `icon` | `string` | हो | Lucide चिह्न नाम (जस्तै, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | हो | साइडबारमा देखाइएको बटनको पाठ |
| `onClick` | `() => void` | हो | बटन क्लिक गर्दा कल गरिने कार्य |
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
**चिह्न सन्दर्भ:** उपलब्ध चिह्नहरूको सम्पूर्ण सूचीमा जानुहोस् [lucide.dev/icons](https://lucide.dev/icons)

> **संगतता नोट:** केही पुराना प्लगइनहरूले `addSidebarButton(id, icon, label, onClick)` जस्ता स्थिति तर्कहरू प्रयोग गर्दछन्। आधिकारिक API ले माथि वर्णन गरिएको **विकल्प वस्तु** प्रयोग गर्दछ। नयाँ प्लगइनहरूको लागि सधैं वस्तु शैली प्रयोग गर्नुहोस्।

#### `ui.openWebview(options)`

अनुकूल HTML सामग्रीसहितको पपअप विन्डो खोल्नुहोस्। यसरी तपाईं समृद्ध UI निर्माण गर्नुहुन्छ।

| विकल्प | प्रकार | विवरण |
|--------|-------|--------|
| `title` | `string` | विन्डोको शीर्षक |
| `html` | `string` | रेंडर गर्नको लागि पूर्ण HTML सामग्री |
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
> [भाग 3](#part-3-building-custom-ui-with-webviews) मा उन्नत वेबभ्यू ढाँचा हेर्नुहोस्।

#### `ui.showNotification(type, message)`

एक टोस्ट सूचनालाई देखाउनुहोस्।

| प्यारामिटर | प्रकार | विवरण |
|-------------|--------|--------|
| `type` | `'success' \| 'error' \| 'info'` | सूचना शैली |
| `message` | `string` | देखाउनको लागि पाठ |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

तलको स्थिति पट्टीमा एक स्थायी पाठ वस्तु थप्नुहोस्।

| प्यारामिटर | प्रकार | विवरण |
|-------------|--------|--------|
| `id` | `string` | यस स्थिति वस्तुको लागि अद्वितीय ID |
| `text` | `string` | प्रदर्शन गर्नको लागि पाठ |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — स्थायी ��ण्डारण

प्लगइन सेटिङहरू स्थायी रूपमा `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` मा भण्डारण गरिन्छ।

#### `settings.get(key)`

सुरक्षित गरिएको मान पढ्नुहोस्।
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
यदि कुंजी अवस्थित छैन भने `undefined` फर्काउँछ।

#### `settings.set(key, value)`

एक मान सुरक्षित गर्नुहोस्। स्ट्रिङ, संख्या, बूलियन, एरे, र वस्तुहरूलाई समर्थन गर्दछ।
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**उदाहरण: प्रयोगकर्ता प्राथमिकताहरू सम्झनुहोस्**
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

### `ctx.ai` — AI एकीकरण

> **स्थिति: चाँडै आउने** — AI API परिभाषित गरिएको छ तर Soomy सँग जडान गरिएको छैन। हाल `{ response: 'AI not yet connected' }` फर्काउँछ। पूर्ण AI एकीकरण भविष्यको रिलिजका लागि योजना गरिएको छ।

#### `ai.chat(messages, options?)`

AI सहायक (Soomy) लाई सन्देशहरू पठाउनुहोस्।
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## भाग 3: वेबभ्यूहरूसँग अनुकूल UI निर्माण

`openWebview()` API ले HTML, CSS, र JavaScript सँग ड्यासबोर्ड UIs बनाउन अनुमति दिन्छ — सबै पपअप विन्डो भित्र।

> **महत्वपूर्ण सीमितता:** वेबभ्यूहरू **प्रदर्शन मात्र** हुन्। तिनीहरूले प्लगइन APIs (`ctx.settings`, `ctx.terminal`, आदि) मा फर्कन सक्दैनन्। सबै प्रयोगकर्ता क्रियाकलापका लागि साइडबार बटनहरू प्रयोग गर्नुहोस्, र वर्तमान अवस्था देखाउन `openWebview()` प्रयोग गर्नुहोस्। यदि तपाईंलाई अन्तरक्रियात्मक सुविधाहरू आवश्यक छ भने, तिनीहरूलाई साइ���बार बटनहरूबाट ट्रिगर गर्नुहोस् र प्रदर्शन ताजा गर्न वेबभ्यू पुनः खोल्नुहोस्।

### ढाँचा: टर्मिनल कमाण्ड → आउटपुट पार्स गर्नुहोस् → HTML मा देखाउनुहोस्

यो सबैभन्दा सामान्य प्लगइन ढाँचा हो। तपाईंले एक कमाण्ड चलाउनुहुन्छ, परिणाम पार्स गर्नुहुन्छ, र दृश्य रूपमा देखाउनुहुन्छ।
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
### ढाँचा: स्वचालित ताजा हुने अन्तरक्रियात्मक ड्यासबोर्ड
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
### ढाँचा: वेबभ्यूमा सेटिङहरू प्रदर्शन गर्दै

> **नोट:** वेबभ्यूहरू प्रदर्शन मात्र हुन् — तिनीहरूले प्लगइन APIs मा फर्कन सक्दैनन्। सेटिङहरू परिवर्तन गर्नको लागि तपाईंको साइडबार बटन ह्यान्डलरहरूमा `ctx.settings` प्रयोग गर्नुहोस्, र वर्तमान अवस्था देखाउन `openWebview()` प्रयोग गर्नुहोस्।
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

## भाग 4: तपाईंको प्लगइन प्रकाशन गर्दै

### चरण 1: स्थानीय रूपमा परीक्षण गर्नुहोस्

1. तपाईंको प्लगइनलाई `~/.wia-soom/plugins/{your-plugin}/` मा प्रतिलिपि गर्नुहोस्।
2. WIA SOOM पुनः सुरु गर्नुहोस्।
3. यो काम गर्छ कि छैन भनेर सुनिश्चित गर्नुहोस्: साइडबार बटन देखा पर्छ, सुविधाहरू सही रूपमा काम गर्छन्।
4. किनारा केसहरू परीक्षण गर्नुहोस्: यदि कुनै टर्मिनल जडान गरिएको छैन भने के हुन्छ?

### चरण 2: पेश गर्नको लागि तयारी गर्नुहोस्

तपाईंको प्लगइन फोल्डर��ा समावेश गर्नुपर्छ:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**आवश्यक `package.json` क्षेत्रहरू:**

| क्षेत्र | विवरण | उदाहरण |
|-------|-------------|---------|
| `name` | अद्वितीय kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | एक पंक्तिको विवरण | `"Monitors nginx access logs in real-time"` |
| `author` | तपाईंको नाम | `"John Doe"` |
| `main` | प्रवेश बिन्दु | `"index.js"` |

**वैकल्पिक क्षेत्रहरू:**

| क्षेत्र | विवरण |
|-------|-------------|
| `license` | लाइसेन्स प्रकार (MIT सिफारिस गरिएको) |
| `keywords` | खोज ट्यागहरूको एरे |
| `soom.minVersion` | आवश्यक न्यूनतम WIA SOOM संस्करण |

### चरण 3: प्लगइन रजिष्ट्रिमा पेश गर्नुहोस्

1. ****Package** your plugin as a ZIP file
2. **Add** आफ्नो प्लगइनलाई `plugins/{your-plugin-name}/` मा
3. **Submit** एक Pull Request

### चरण 4: समीक्षा र स्वीकृति

हामी प्रत्येक प्लगइनको समीक्षा गर्छौं:

- **सुरक्षा** — कुनै खतरनाक APIs छैन (हेर्नुहोस् [सुरक्षा नियमहरू](#security-rules))
- **गुणस्तर** — के यसले काम गर्छ? कोड सफा छ?
- **उपयोगिता** — के यसले वास्तविक समस्याको समाधान गर्छ?

स्वीकृतिपछि:
1. तपाईंको प्लगइन `registry.json` मा थपिन्छ
2. `dist/` मा एक ZIP प्याकेज बनाइन्छ
3. तपाईंको प्लगइन सबै WIA SOOM प्रयोगकर्ताहरूको लागि **Plugin Store** मा देखा पर्दछ!

---

## भाग 5: उत्कृष्ट अभ्यासहरू

### सुरक्षा नियमहरू

यी नियमहरू **अनिवार्य** छन्। जसले तिनीहरूलाई उल्लङ्घन गर्छन्, ती अस्वीकृत गरिनेछन्।

| नियम | किन |
|------|-----|
| **कदापि** `eval()` वा `new Function()` प्रयोग नगर्नुहोस् | कोड इन्जेक्शनको जोखिम |
| **कदापि** `child_process`, `exec()`, `spawn()` प्रयोग नगर्नुहोस् | आदेशहरूको लागि केवल `ctx.terminal.send()` प्रयोग गर्नुहोस् |
| **कदापि** बाह्य URLs ल्याउनुहोस् | अपवाद: `wiasoom.com` API अन्तिम बिन्दु |
| **कदापि** `process.env` मा पहुँच नगर्नुहोस् | वातावरणीय चरहरूले रहस्य समावेश गर्न सक्छन् |
| **कदापि** `require('fs')` सिधै प्रयोग नगर्नुहोस् | भण्डारणका लागि `ctx.settings` प्रयोग गर्नुहोस्, फाइल स्थानान्तरणका लागि `ctx.sftp` प्रयोग गर���नुहोस् |
| **कदापि** npm बाह्य प्याकेजहरू प्रयोग नगर्नुहोस् | केवल शुद्ध JavaScript — कुनै node_modules छैन |
| **पक्कै** सबै दूरस्थ आदेशहरूको लागि `ctx.terminal.send()` प्रयोग गर्नुहोस् | यो सुरक्षित SSH च्यानलबाट जान्छ |
| **पक्कै** `deactivate()` मा सफा गर्नुहोस् | सुनेरहरू हटाउनुहोस्, अन्तरालहरू सफा गर्नुहोस् |

### त्रुटि व्यवस्थापन

सधैं जोखिमपूर्ण अपरेसनहरूलाई try/catch मा लपेट्नुहोस्:
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
### deactivate() मा सफाई

यदि तपाईंको प्लगइनले अन्तराल, सुनेरहरू, वा सदस्यताहरू सिर्जना गर्छ भने — तिनीहरूलाई सफा गर्नुहोस्:
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
### i18n समर्थन

WIA SOOM ले 254 भाषाहरूलाई समर्थन गर्दछ। तपाईंको प्लगइनको लेबल अनुवादयोग्य बनाउन, एक साधारण दृष्टिकोण प्रयोग गर्नुहोस्:
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

## भाग 6: वास्तविक संसारका उदाहरणहरू

### उदाहरण 1: सर्भर डिस्क चेक गर्ने

दूरस्थ सर्भरमा `df -h` चलाउँछ र स्थिति पट्टीमा प्रयोग गरिएको/उपलब्ध स्थान देखाउँछ।
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

### उदाहरण 2: TODO व्यवस्थापक

एक प्लगइन जसले स्थायी भण्डारणका लागि सेटिङहरू र प्रदर्शनका लागि वेबभ्यू प्रयोग गरेर TODO सूचीलाई व्यवस्थापन गर्छ।

> **डिजाइन ढाँचा:** किनभने वेबभ्यूले सिधै प्लगइन APIs लाई कल गर्न सक्दैन, यो प्लगइनले "स्न्यापशट" दृष्टिकोण प्रयोग गर���दछ — यसले सेटिङहरूबाट TODOs पढ्छ, तिनीहरूलाई पढ्नको लागि HTML मा रेंडर गर्छ, र वस्तुहरू थप्नका लागि साइडबार-आधारित क्रियाहरू प्रदान गर्छ। वेबभ्यू एक **प्रदर्शन** तह हो, अन्तरक्रियात्मक फर्म होइन।
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

### उदाहरण 3: त्रुटि निगरानी गर्ने

टर्मिनल आउटपुटलाई निगरानी गर्छ र विशेष ढाँचाहरू पहिचान गर्दा एक सूचनापत्र पठाउँछ।
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

## परिशिष्ट: श्रेणी र चिह्नहरू

### प्लगइन श्रेणीहरू (29)

यी तपाईंको `package.json` `keywords` मा वा रजिष्ट्रिमा पेश गर्दा प्रयोग गर्नुहोस्:

| श्रेणी | विवरण |
|----------|-------------|
| `server` | सामान्य सर्भर व्यवस्थापन |
| `devtools` | विकास उपकरणहरू |
| `calculator` | गणक र रूपान्तरण गर्ने उपकरणहरू |
| `simulator` | सिमुलेटरहरू |
| `game` | टर्मिनल खेलहरू |
| `business` | व्यवसायका उपकरणहरू |
| `security` | सुरक्षा र अडिटिङ |
| `web` | वेब सर्भर व्यवस्थापन |
| `education` | शैक्षिक उपकरणहरू |
| `health` | स्वास्थ्यसँग सम्बन्धित ��पकरणहरू |
| `islamic` | इस्लामिक उपकरणहरू (प्रार्थना समय, आदि) |
| `science` | वैज्ञानिक उपकरणहरू |
| `quantum` | क्वान्टम कम्प्युटिङ उपकरणहरू |
| `ai` | एआई-संचालित उपकरणहरू |
| `biotech` | जैव प्रौद्योगिकी उपकरणहरू |
| `space` | अन्तरिक्ष र खगोल विज्ञान उपकरणहरू |
| `network` | नेटवर्क उपकरणहरू |
| `database` | डेटाबेस व्यवस्थापन |
| `monitoring` | सर्भर निगरानी |
| `devops` | DevOps र CI/CD |
| `utility` | सामान्य उपयोगिताहरू |
| `design` | डिजाइन उपकरणहरू |
| `ecommerce` | ई-वाणिज्य उपकरणहरू |
| `automation` | स्वचालन उपकरणहरू |
| `kpop` | K-pop सम्बन्धी उपकरणहरू |
| `accessibility` | पहुँचयोग्यता उपकरणहरू |
| `analytics` | विश्लेषण र रिपोर्टिङ |
| `wia` | WIA पारिस्थितिकी प्रणालीका उपकरणहरू |
| `all` | सबै श्रेणीहरूमा देखा पर्ने |

### सिफारिस गरिएका चिह्नहरू (Lucide)

| चिह्नको नाम | प्रयोगको लागि |
|-----------|---------|
| `server` | सर्भर व्यवस्थापन |
| `shield` | सुरक्षा |
| `database` | डेटाबेस |
| `activity` | निगरानी |
| `terminal` | टर्मिनल उपकरणहरू |
| `code` | विकास |
| `hard-drive` | डिस्क/स्टोरेज |
| `network` | नेटवर्किङ |
| `lock` | प्रमाणीकरण/एन्क्रिप्शन |
| `eye` | हेर्ने/निगरानी गर्ने |
| `check-square` | कार्यहरू/TODO |
| `layout-dashboard` | ड्यासबोर्डहरू |
| `settings` | कन्फिगरेसन |
| `zap` | स्वचालन |
| `globe` | वेब/अन्तर्राष्ट्रिय |

सभी 1,500+ चिह्नहरू ब्राउज गर्नुहोस्: [lucide.dev/icons](https://lucide.dev/icons)

---

## मद्दत चाहिन्छ?

- **GitHub समस्याहरू:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **प्लगइन समस्याहरू:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **उदाहरण प्लगइनहरू:** [Website](https://wiasoom.com)
- **वेबसाइट:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>केही अद्भुत निर्माण गर्नुहोस्। यसलाई संसारसँग साझा गर्नुहोस्।</em></p>
<p align="center"><em>— WIA SOOM टोली</em></p>