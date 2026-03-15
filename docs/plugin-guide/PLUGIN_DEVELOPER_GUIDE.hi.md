<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM प्लगइन डेवलपर गाइड</h1>
<p align="center"><strong>5 मिनट में अपना खुद का प्लगइन बनाएं।</strong></p>
<p align="center">शक्तिशाली सर्वर उपकरण, डैशबोर्ड और ऑटोमेशन बनाएं — WIA SOOM के अंदर ही।</p>

---

## सामग्री की तालिका

- [भाग 1: त्वरित शुरुआत — 5 मिनट में आपका पहला प्लगइन](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [भाग 2: प्लगइन संदर्भ API](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [भाग 3: वेबव्यू के साथ कस्टम UI बनाना](#part-3-building-custom-ui-with-webviews)
- [भाग 4: अपने प्ल��इन को प्रकाशित करना](#part-4-publishing-your-plugin)
- [भाग 5: सर्वोत्तम प्रथाएँ](#part-5-best-practices)
- [भाग 6: वास्तविक दुनिया के उदाहरण](#part-6-real-world-examples)
- [परिशिष्ट: श्रेणियाँ और आइकन](#appendix-categories--icons)

---

## भाग 1: त्वरित शुरुआत — 5 मिनट में आपका पहला प्लगइन

### आप क्या बनाएंगे

एक "Hello World" प्लगइन जो साइडबार में एक बटन जोड़ता है। जब उस पर क्लिक किया जाता है, तो यह एक अधिसूचना दिखाता है।

### चरण 1: प्लगइन फ़ोल्डर बनाएं
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### चरण 2: package.json बनाएं
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
**आवश्यक फ़ील्ड:** `name`, `version`, `description`, `author`, `main`

### चरण 3: index.js बनाएं
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
### चरण 4: WIA SOOM को पुनः प्रारंभ करें

ऐप को पुनः प्रारंभ करें (या सेटिंग्स → प्लगइन्स में प्लगइन को बंद/चालू करें)।

आपको साइडबार में एक **"Hello World"** बटन दिखाई देगा। उस पर क्लिक करें — आप एक सफल अधिसूचना देखेंगे!

### यह कैसे काम करता है
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

## भाग 2: प्लगइन संदर्भ API

जब आपकी `activate(context)` फ़ंक्शन को कॉल किया जाता है, तो `context` (या `ctx`) ये APIs प्रदान करता है:
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

### `ctx.terminal` — दूरस्थ सर्वरों पर कमांड चलाएं

#### `terminal.send(sessionId, data)`

एक सक्रिय टर्मिनल सत्र को कमांड (या कोई भी डेटा) भेजें।

| पैरामीटर | प्रकार | विव���ण |
|-----------|------|-------------|
| `sessionId` | `string` | जिस टर्मिनल सत्र को भेजना है |
| `data` | `string` | भेजने के लिए कमांड या डेटा |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

एक टर्मिनल सत्र से सभी आउटपुट पर सब्सक्राइब करें। एक **अनसब्सक्राइब फ़ंक्शन** लौटाता है।

| पैरामीटर | प्रकार | विवरण |
|-----------|------|-------------|
| `sessionId` | `string` | जिस टर्मिनल सत्र को देखना है |
| `callback` | `(data: string) => void` | प्रत्येक आउटपुट के टुकड़े के साथ कॉल किया जाता है |
| **लौटाता है** | `() => void` | सुनना बंद करने के लिए इसे कॉल करें |
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
**महत्वपूर्ण:** हमेशा अनसब्सक्राइब फ़ंक्शन को सहेजें और इसे `deactivate()` में कॉल करें ताकि मेमोरी लीक से बचा जा सके।

---

### `ctx.sftp` — फ़ाइल स्थानांतरण

> **स्थिति: जल्द आ रहा है** — SFTP API परिभाषित है लेकिन अभी तक ऐप के SFTP इंजन से जोड़ा नहीं गया है। `list()` वर्तमान में एक खाली एरे लौटाता है, और `upload()`/`download()` कोई कार्य नहीं करते हैं। इसे भविष्य के रिलीज में पूरी तरह से लागू किया जाएगा। अभी के लिए, `scp` या `rsync` कमांड के साथ `ctx.terminal.send()` का उपयोग करें।

#### `sftp.list(sessionId, path)`

एक दूरस्थ निर्देशिका में फ़ाइलों की सूची बनाएं।
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

स्थानीय मशीन से दूरस्थ सर्वर पर एक फ़ाइल अपलोड करें।
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

दूरस्थ सर्��र से स्थानीय मशीन पर एक फ़ाइल डाउनलोड करें।
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**कार्यaround (जब तक SFTP API लाइव नहीं हो जाता):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — उपयोगकर्ता इंटरफ़ेस

#### `ui.addSidebarButton(options)`

WIA SOOM साइडबार में एक बटन जोड़ें।

| विकल्प | प्रकार | आवश्यक | विवरण |
|--------|------|----------|-------------|
| `id` | `string` | नहीं | अद्वितीय ID (डिफ़ॉल्ट रूप से प्लगइन नाम) |
| `icon` | `string` | हाँ | Lucide आइकन नाम (जैसे, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | हाँ | साइडबार में दिखाया गया बटन पाठ |
| `onClick` | `() => void` | हाँ | जब बटन पर क्लिक किया जाता है तो कॉल की जाने वाली फ़ंक्शन |
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
**आइकन संदर्भ:** सभी उपलब्ध आइकनों को [lucide.dev/icons](https://lucide.dev/icons) प��� ब्राउज़ करें

> **संगतता नोट:** कुछ पुराने प्लगइन स्थिति संबंधी तर्कों का उपयोग करते हैं जैसे `addSidebarButton(id, icon, label, onClick)`। आधिकारिक API एक **विकल्प ऑब्जेक्ट** का उपयोग करता है जैसा कि ऊपर दस्तावेज़ित किया गया है। नए प्लगइन्स के लिए हमेशा ऑब्जेक्ट शैली का उपयोग करें।

#### `ui.openWebview(options)`

कस्टम HTML सामग्री के साथ एक पॉपअप विंडो खोलें। यही तरीका है जिससे आप समृद्ध UI बनाते हैं।

| विकल्प | प्रकार | विवरण |
|--------|------|-------------|
| `title` | `string` | विंडो का शीर्षक |
| `html` | `string` | रेंडर करने के लिए पूर्ण HTML सामग्री |
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
> [भाग 3](#part-3-building-custom-ui-with-webviews) में उन्नत वेबव्यू पैटर्न देखें।

#### `ui.showNotification(type, message)`

एक टोस्ट सूचना दिखाएं।

| पैरामीटर | प्रकार | विवरण |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | सूचना शैली |
| `message` | `string` | दिखाने के लिए पाठ |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

नीचे के स्थिति पट्टी में एक स्थायी पाठ आइटम जोड़ें।

| पैरामीटर | प्रकार | विवरण |
|-----------|------|-------------|
| `id` | `string` | इस स्थिति आइटम के लिए अद्वितीय आईडी |
| `text` | `string` | प्रदर्शित करने के लिए पाठ |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — स्थायी भंडारण

प्लगइन सेटिंग्स को स्थायी रूप से `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` में संग्रहीत ��िया जाता है।

#### `settings.get(key)`

एक सहेजा हुआ मान पढ़ें।
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
यदि कुंजी मौजूद नहीं है तो `undefined` लौटाता है।

#### `settings.set(key, value)`

एक मान सहेजें। स्ट्रिंग, संख्या, बूलियन, एरे और ऑब्जेक्ट का समर्थन करता है।
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**उदाहरण: उपयोगकर्ता प्राथमिकताएँ याद रखें**
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

### `ctx.ai` — एआई एकीकरण

> **स्थिति: जल्द आ रहा है** — एआई एपीआई परिभाषित है लेकिन अभी तक Soomy से जुड़ा नहीं है। वर्तमान में `{ response: 'AI not yet connected' }` लौटाता है। पूर्ण एआई एकीकरण भविष्य के रिलीज़ के लिए योजनाबद्ध है।

#### `ai.chat(messages, options?)`

एआई सहायक (Soomy) को संदेश भेजें।
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## भाग 3: वेबव्यू के साथ कस्टम यूआई बनाना

`openWebview()` एपीआई आपको HTML, CSS, और JavaScript के साथ डैशबोर्ड यूआई बनाने की अनुमति देता है — सभी एक पॉपअप विंडो के अंदर।

> **महत्वपूर्ण सीमा:** वेबव्यू केवल **प्रदर्शन-केवल** हैं। वे प्लगइन एपीआई (`ctx.settings`, `ctx.terminal`, आदि) में वापस कॉल नहीं कर सकते। सभी उपयोगकर्ता क्रियाओं के लिए साइडबार बटन का उपयोग करें, और वर्तमान स्थिति दिखाने के लिए `openWebview()` का उपयोग करें। यदि आपको इंटरैक्टिव सुविधाएँ चाहिए, तो उन्हें साइडबार बटन से ट्रिगर करें और प्रदर्श�� को ताज़ा करने के लिए वेबव्यू को फिर से खोलें।

### पैटर्न: टर्मिनल कमांड → आउटपुट पार्स करें → HTML में दिखाएँ

यह सबसे सामान्य प्लगइन पैटर्न है। आप एक कमांड चलाते हैं, परिणाम को पार्स करते हैं, और इसे दृश्य रूप में प्रदर्शित करते हैं।
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
### पैटर्न: ऑटो-रीफ्रेश के साथ इंटरैक्टिव डैशबोर्ड
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
### पैटर्न: वेबव्यू में सेटिंग्स प्रदर्शित करना

> **नोट:** वेबव्यू केवल प्रदर्शन-केवल हैं — वे प्लगइन एपीआई में वापस कॉल नहीं कर सकते। सेटिंग्स को संशोधित करने के लिए अपने साइडबार बटन हैंडलरों में `ctx.settings` का उपयोग करें, और वर्तमान स्थिति दिखाने के लिए `openWebview()` का उपयोग करें।
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

## भाग 4: अपने प्लगइन को प्रकाशित करना

### चरण 1: स्थानीय रूप से परीक्षण करें

1. अपने प्लगइन को `~/.wia-soom/plugins/{your-plugin}/` में कॉपी करें
2. WIA SOOM को पुनः प्रारंभ करें
3. सत्यापित करें कि यह काम करता है: साइडबार बटन प्रकट होता है, सुविधाएँ सही ढंग से काम करती हैं
4. किनारे के मामलों का परीक्षण करें: यदि कोई टर्मिनल कनेक्ट नहीं है तो क्या होता है?

### चरण 2: सबमिशन के लिए तैयारी करें

आपके प्लगइन फ़ोल्डर में निम्नलिखित होना चाहिए:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**आवश्यक `package.json` फ़ील्ड:**

| फ़ील्ड | विवरण | उदाहरण |
|-------|-------------|---------|
| `name` | अद्वितीय kebab-case ID | `"my-awesome-plugin"` |
| `version` | सेमांटिक संस्करण | `"1.0.0"` |
| `description` | एक पंक्ति का विवरण | `"Monitors nginx access logs in real-time"` |
| `author` | आपका नाम | `"John Doe"` |
| `main` | प्रवेश बिंदु | `"index.js"` |

**वैकल्पिक फ़ील्ड:**

| फ़ील्ड | विवरण |
|-------|-------------|
| `license` | लाइसेंस प्रकार (MIT अनुशंसित) |
| `keywords` | खोज टैग की सूची |
| `soom.minVersion` | आवश्यक न्यूनतम WIA SOOM संस्करण |

### चरण 3: प्लगइन रजिस्ट्री में सबमिट करें

1. ****Package** your plugin as a ZIP file
2. **Add** अपने प्लगइन को `plugins/{your-plugin-name}/` में
3. **Submit** एक Pull Request

### चरण 4: समीक्षा और स्वीकृति

हम हर प्लगइन की समीक्षा करते हैं:

- **सुरक्षा** — कोई खतरनाक APIs नहीं (देखें [सुरक्षा नियम](#security-rules))
- **गुणवत्ता** — क्या यह काम करता है? क्या कोड साफ है?
- **उपयोगिता** — क्या यह एक वास्तविक समस्या का समाधान करता है?

स्वीकृति के बाद:
1. आपका प्लगइन `registry.json` में जोड़ा जाता है
2. `dist/` में एक ZIP बंडल बनाया जाता है
3. आपका प्लगइन सभी WIA SOOM उपयोगकर्ताओं के लिए **Plugin Store** में दिखाई देता है!

---

## भाग 5: सर्वोत्तम प्रथाएँ

### सुरक्षा नियम

ये नियम **अनिवार्य** हैं। जो प्लगइन इनका उल्लंघन करते हैं, उन्हें अस्वीकृत किया जाएगा।

| नियम | क्यों |
|------|-----|
| **कभी भी** `eval()` या `new Function()` का उपयोग न करें | कोड इंजेक्शन का जोखिम |
| **कभी भी** `child_process`, `exec()`, `spawn()` का उपयोग न करें | केवल `ctx.terminal.send()` का उपयोग करें आदेशों के लिए |
| **कभी भी** बाहरी URLs को न लाएँ | अपवाद: `wiasoom.com` API एंडपॉइंट्स |
| **कभी भी** `process.env` तक न पहुँचें | पर्यावरण चर में रहस्य हो सकते हैं |
| **कभी भी** सीधे `require('fs')` का उपयोग न करें | भंडारण के लिए `ctx.settings` का उपयोग करें, फ़ाइल स्थानांतरण के लिए `ctx.sftp` का उपयोग करें |
| **कभी भी** npm बाहरी पैकेज का उपयोग न करें | केवल शुद्ध JavaScript — कोई node_modules नहीं |
| **ज़रूरी** है कि सभी दूरस्थ आदेशों के लिए `ctx.terminal.send()` का उपयोग करें | यह सुरक्षित SSH चैनल के माध्यम से जाता है |
| **ज़रूरी** है कि `deactivate()` में साफ-सफाई करें | श्रोता हटाएँ, अंतराल साफ करें |

### त्रुटि प्रबंधन

हमेशा जोखिम भरे संचालन को try/catch में लपेटें:
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
### deactivate() में साफ-सफाई

यदि आपका प्लगइन अंतराल, श्रोता, या सदस्यता बनाता है — उन्हें साफ करें:
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

WIA SOOM 254 भाषाओं का समर्थन करता है। अपने प्लगइन लेबल को अनुवादित करने योग्य बनाने के लिए, एक सरल दृष्टिकोण का उपयोग करें:
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

## भाग 6: वास्तविक दुनिया के उदाहरण

### उदाहरण 1: सर्वर डिस्क चेक करने वाला

दूरस्थ सर्वर पर `df -h` चलाता है और स्थिति पट्टी में उपयोग की गई/उपलब्ध जगह दिखाता है।
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

### उदाहरण 2: TODO प्रबंधक

एक प्लगइन जो स्थायी भंडारण के लिए सेटिंग्स का उपयोग करता है और प्रदर्शन के लिए एक वेबव्यू का उपयोग करता है।

> **डिज़ाइन पैटर्न:** चूंकि वेबव्यू सीधे प्लगइन APIs को कॉल नहीं कर सकते, यह प्लगइन "स्नैपशॉट" दृष्टिकोण का उपयोग करता है — यह सेटिंग्स से TODO पढ़ता है, उन्हें केवल पढ़ने योग्य HTML के रूप में प्रस्तुत करता है, और आइटम जोड़ने के लिए साइडबार-आधारित क्रियाएँ प्रदान करता है। वेबव्यू एक **प्रदर्शन** परत है, न कि एक इंटरैक्टिव फॉर्म।
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

### उदाहरण 3: त्रुटि वॉचर

टर्मिनल आउटपुट की निगरानी करता है और जब विशिष्ट पैटर्न का पता लगाया जाता है तो एक सूचना भेजता है।
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

## अनुपंड: श्रेणियाँ और आइकन

### प्लगइन श्रेणियाँ (29)

इनका उपयोग अपने `package.json` `keywords` में या रजिस्ट्र्री में सबमिट करते समय करें:

| श्रेणी | विवरण |
|----------|-------------|
| `server` | सामान्य सर्वर प्रबंधन |
| `devtools` | विकास उपकरण |
| `calculator` | कैलकुलेटर और कन्वर्टर्स |
| `simulator` | सिमुलेटर्स |
| `game` | टर्मिनल गेम्स |
| `business` | व्यवसाय उपकरण |
| `security` | सुरक्षा और ऑडिटिंग |
| `web` | वेब सर्वर प्रबंधन |
| `education` | शैक्षिक उपकरण |
| `health` | स्वास्थ्य से संबंधित उपकरण |
| `islamic` | इस्लामी उपकरण (प्रार्थना समय, आदि) |
| `science` | वैज्ञानिक उपकरण |
| `quantum` | क्वांटम कंप्यूटिंग उपकरण |
| `ai` | एआई-संचालित उपकरण |
| `biotech` | जैव प्रौद्योगिकी उपकरण |
| `space` | अंतरिक्ष और खगोल विज्ञान उपकरण |
| `network` | नेटवर्क उपकरण |
| `database` | डेटाबेस प्रबंधन |
| `monitoring` | सर्वर निगरानी |
| `devops` | देवऑप्स और CI/CD |
| `utility` | सामान्य उपयोगिताएँ |
| `design` | डिज़ाइन उपकरण |
| `ecommerce` | ई-कॉमर्स उपकरण |
| `automation` | स्वचालन उपकरण |
| `kpop` | के-पॉप से संबंधित उपकरण |
| `accessibility` | पहुंच उपकरण |
| `analytics` | विश्लेषण और रिपोर्टिंग |
| `wia` | WIA पारिस्थितिकी तंत्र उपकरण |
| `all` | सभी श्रेणियों में दिखाई देता है |

### अनुशंसित आइकन (Lucide)

| आइकन नाम | उपयोग के लिए |
|-----------|---------|
| `server` | सर्वर प्रबंधन |
| `shield` | सुरक्षा |
| `database` | डेटाबेस |
| `activity` | निगरानी |
| `terminal` | टर्मिनल उपकरण |
| `code` | विकास |
| `hard-drive` | डिस्क/स्टोरेज |
| `network` | नेटवर्किंग |
| `lock` | प्रमाणीकरण/एन्क्रिप्शन |
| `eye` | देखना/निगरानी |
| `check-square` | कार्य/TODO |
| `layout-dashboard` | डैशबोर्ड |
| `settings` | कॉन्फ़िगरेशन |
| `zap` | स्वचालन |
| `globe` | वेब/अंतरराष्ट्रीय |

सभी 1,500+ आइकन ब्राउज़ करें: [lucide.dev/icons](https://lucide.dev/icons)

---

## मदद चाहिए?

- **GitHub मुद्दे:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **प्लगइन मुद्दे:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **उदाहरण प्लगइन्स:** [Website](https://wiasoom.com)
- **वेबसाइट:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>कुछ अद्भुत बनाएं। इसे दुनिया के साथ साझा करें।</em></p>
<p align="center"><em>— WIA SOOM टीम</em></p>