<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM प्लगइन विकासक मार्गदर्शक</h1>
<p align="center"><strong>5 मिनिटांत तुमचा स्वतःचा प्लगइन तयार करा.</strong></p>
<p align="center">शक्तिशाली सर्व्हर टूल्स, डॅशबोर्ड आणि स्वयंचलन तयार करा — WIA SOOM च्या आतच.</p>

---

## सामग्रीची यादी

- [भाग 1: त्वरित प्रारंभ — तुमचा पहिला प्लगइन 5 मिनिटांत](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [भाग 2: प्लगइन संदर्भ API](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [भाग 3: वेबव्ह्यूजसह कस्टम UI तयार करणे](#part-3-building-custom-ui-with-webviews)
- [भाग 4: तुमचा प्लगइन प्रकाशित करणे](#part-4-publishing-your-plugin)
- [भाग 5: सर्वोत्तम प्रथा](#part-5-best-practices)
- [भाग 6: वास्तविक जगातील उदाहरणे](#part-6-real-world-examples)
- [परिशिष्ट: श्रेण्या आणि चिन्हे](#appendix-categories--icons)

---

## भाग 1: त्वरित प्रारंभ — तुमचा पहिला प्लगइन 5 मिनिटांत

### तुम्ही काय तयार कराल

एक "Hello World" प्लगइन जे साइडबारमध्ये एक बटण जोडते. क्लिक केल्यावर, ते एक सूचना दर्शवते.

### पाऊल 1: प्लगइन फोल्डर तयार करा
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### पाऊल 2: package.json तयार करा
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
**आवश्यक क्षेत्र:** `name`, `version`, `description`, `author`, `main`

### पाऊल 3: index.js तयार करा
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
### पाऊल 4: WIA SOOM पुन्हा सुरू करा

अॅप पुन्हा सुरू करा (किंवा सेटिंग्ज → प्लगइन्समध्ये प्लगइन बंद/सुरू करा).

तुम्हाला साइडबारमध्ये एक **"Hello World"** बटण दिसेल. त्यावर क्लिक करा — तुम्हाला एक यशस्वी सूचना दिसेल!

### हे कसे कार्य करते
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

जेव्हा तुमचा `activate(context)` फंक्शन कॉल केला जातो, तेव्हा `context` (किंवा `ctx`) हे API प्रदान करते:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — दूरस्थ सर्व्हरवर कमांड चालवा

#### `terminal.send(sessionId, data)`

सक्रिय टर्मिनल सत्रात एक कमांड (किंवा कोणतीही डेटा) पाठवा.

| पॅरामीटर | प्रकार | वर्णन |
|-----------|------|-------------|
| `sessionId` | `string` | पाठवण्यासाठी टर्मिनल सत्र |
| `data` | `string` | पाठवण्यासाठी कमांड किंवा डेटा |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

टर्मिनल सत्रातून सर्व आउटपुटसाठी सदस्यता घ्या. एक **अनसब्सक्राइब फंक्शन** परत करते.

| पॅरामीटर | प्रकार | वर्णन |
|-----------|------|-------------|
| `sessionId` | `string` | पाहण्यासाठी टर्मिनल सत्र |
| `callback` | `(data: string) => void` | प्रत्येक आउटपुटच्या तुकड्याबरोबर कॉल केला जातो |
| **परत करते** | `() => void` | ऐकणे थांबवण्यासाठी हे कॉल करा |
§§§CHUNK_SEPARATOR§§§
**महत्त्वाचे:** नेहमी अनसब्सक्राइब फंक्शन जतन करा आणि `deactivate()` मध्ये कॉल करा जेणेकरून मेमरी लीक होणार नाही.

---

### `ctx.sftp` — फाइल ट्रान्सफर

> **स्थिती: लवकरच येत आहे** — SFTP API परिभाषित केले आहे पण अद्याप अॅपच्या SFTP इंजिनशी जोडलेले नाही. `list()` सध्या एक रिक्त अॅरे परत करते, आणि `upload()`/`download()` हे काहीही करत नाहीत. हे भविष्यातील प्रकाशनात पूर्णपणे कार्यान्वित केले जाईल. सध्या, `scp` किंवा `rsync` कमांडसह `ctx.terminal.send()` वापरा.

#### `sftp.list(sessionId, path)`

दूरस्थ निर्देशिकेत फाइल्सची यादी करा.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

स्थानिक मशीनवरून दूरस्थ सर्व्हरवर एक फाइल अपलोड करा.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

दूरस्थ सर्व्हरवरून स्थानिक मशीनवर एक फाइल डाउनलोड करा.
§§§CHUNK_SEPARATOR§§§
**कामकाज (SFTP API सक्रिय होईपर्यंत):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — वापरकर्ता इंटरफेस

#### `ui.addSidebarButton(options)`

WIA SOOM साइडबारमध्ये एक बटण जोडा.

| पर्याय | प्रकार | आवश्यक | वर्णन |
|--------|------|----------|-------------|
| `id` | `string` | नाही | अद्वितीय ID (प्लगइन नावावरून डिफॉल्ट) |
| `icon` | `string` | होय | Lucide चिन्हाचे नाव (उदा., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | होय | साइडबारमध्ये दर्शविलेला बटण मजकूर |
| `onClick` | `() => void` | होय | बटणावर क्लिक केल्यावर कॉल केलेली फंक्शन |
§§§CHUNK_SEPARATOR§§§
**चिन्ह संदर्भ:** सर्व उपलब्ध चिन्हे पहा [lucide.dev/icons](https://lucide.dev/icons)

> **सुसंगतता नोट:** काही जुने प्लगइन positional arguments वापरतात जसे की `addSidebarButton(id, icon, label, onClick)`. अधिकृत API वर वरीलप्रमाणे **पर्याय वस्तू** वापरते. नवीन प्लगइनसाठी नेहमी वस्तू शैली वापरा.

#### `ui.openWebview(options)`

कस्टम HTML सामग्रीसह एक पॉपअप विंडो उघडा. हे तुम्हाला समृद्ध UI तयार करण्यास मदत करते.

| पर्याय | प्रकार | वर्णन |
|--------|------|-------------|
| `title` | `string` | विंडो शीर्षक |
| `html` | `string` | रेंडर करण्यासाठी पूर्ण HTML सामग्री |
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
> [भाग 3](#part-3-building-custom-ui-with-webviews) मध्ये प्रगत वेबव्ह्यू पॅटर्नसाठी पहा.

#### `ui.showNotification(type, message)`

एक टोस्ट सूचना दर्शवा.

| पॅरामीटर | प्रकार | वर्णन |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | सूचना शैली |
| `message` | `string` | दर्शवायचा मजकूर |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `ui.addStatusBarItem(id, text)`

तळच्या स्थिती बारमध्ये एक कायमचा मजकूर आयटम जोडा.

| पॅरामीटर | प्रकार | वर्णन |
|-----------|------|-------------|
| `id` | `string` | या स्थिती आयटमसाठी अद्वितीय आयडी |
| `text` | `string` | दर्शवायचा मजकूर |
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
---

### `ctx.settings` — कायमचा संग्रह

प्लगइन सेटिंग्ज कायमस्वरूपी `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` मध्ये संग्रहित केल्या जातात.

#### `settings.get(key)`

साठवलेला मूल्य वाचा.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
जर की अस्तित्वात नसेल तर `undefined` परत करते.

#### `settings.set(key, value)`

एक मूल्य साठवा. स्ट्रिंग, संख्या, बूलियन, अरे आणि वस्तूंचा समर्थन करते.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**उदाहरण: वापरकर्त्याच्या आवडी लक्षात ठेवा**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ai` — AI एकत्रीकरण

> **स्थिती: लवकरच येत आहे** — AI API परिभाषित आहे पण अद्याप Soomy शी कनेक्ट केलेले नाही. सध्या `{ response: 'AI not yet connected' }` परत करते. पूर्ण AI एकत्रीकरण भविष्यातील प्रकाशनासाठी नियोजित आहे.

#### `ai.chat(messages, options?)`

AI सहाय्यक (Soomy) कडे संदेश पाठवा.
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

## भाग 3: वेबव्ह्यूजसह कस्टम UI तयार करणे

`openWebview()` API तुम्हाला HTML, CSS, आणि JavaScript सह डॅशबोर्ड UIs तयार करण्याची परवानगी देते — सर्व एक पॉपअप विंडोमध्ये.

> **महत्वाची मर्यादा:** वेबव्ह्यूज **फक्त दर्शविण्यासाठी** आहेत. ते प्लगइन APIs (`ctx.settings`, `ctx.terminal`, इत्यादी) मध्ये कॉल करू शकत नाहीत. सर्व वापरकर्ता क्रियांसाठी साइडबार बटणांचा वापर करा, आणि वर्तमान स्थिती दर्शवण्यासाठी `openWebview()` वापरा. जर तुम्हाला इंटरएक्टिव्ह वैशिष्ट्ये आवश्यक असतील, तर त्यांना साइडबार बटणांद्वारे ट्रिगर करा आणि प्रदर्शन ताजेतवाने क��ण्यासाठी वेबव्ह्यू पुन्हा उघडा.

### पॅटर्न: टर्मिनल कमांड → आउटपुट पार्स करा → HTML मध्ये दर्शवा

हे सर्वात सामान्य प्लगइन पॅटर्न आहे. तुम्ही एक कमांड चालवता, परिणाम पार्स करता, आणि ते दृश्यात्मकपणे दर्शवता.
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
### पॅटर्न: ऑटो-रीफ्रेशसह इंटरएक्टिव्ह डॅशबोर्ड
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
### पॅटर्न: वेबव्ह्यूमध्ये सेटिंग्ज दर्शवणे

> **टीप:** वेबव्ह्यूज फक्त दर्शविण्यासाठी आहेत — ते प्लगइन APIs मध्ये कॉल करू शकत नाहीत. सेटिंग्जमध्ये बदल करण्यासाठी तुमच्या साइडबार बटण हँडलर्समध्ये `ctx.settings` वापरा, आणि वर्तमान स्थिती दर्शवण्यासाठी `openWebview()` ���ापरा.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## भाग 4: तुमचा प्लगइन प्रकाशित करणे

### पाऊल 1: स्थानिकपणे चाचणी करा

1. तुमचा प्लगइन `~/.wia-soom/plugins/{your-plugin}/` मध्ये कॉपी करा.
2. WIA SOOM पुन्हा सुरू करा.
3. ते कार्य करते का ते पडताळा: साइडबार बटण दिसते, वैशिष्ट्ये योग्यरित्या कार्य करतात.
4. कडवट प्रकरणांची चाचणी करा: जर कोणताही टर्मिनल कनेक्ट केलेला नसेल तर काय होते?

### पाऊल 2: सबमिशनसाठी तयारी करा

तुमच्या प्लगइन फोल्डरमध्ये खालील गोष्टी असाव्यात:
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
**आवश्यक `package.json` क्षेत्रे:**

| क्षेत्र | वर्णन | उदाहरण |
|-------|-------------|---------|
| `name` | अद्वितीय kebab-case ID | `"my-awesome-plugin"` |
| `version` | सेमांटिक आवृत्ती | `"1.0.0"` |
| `description` | एक-लाइन वर्णन | `"Monitors nginx access logs in real-time"` |
| `author` | तुमचे नाव | `"John Doe"` |
| `main` | प्रवेश बिंदू | `"index.js"` |

**ऐच्छिक क्षेत्रे:**

| क्षेत्र | वर्णन |
|-------|-------------|
| `license` | परवाना प्रकार (MIT शिफारस केलेले) |
| `keywords` | शोध टॅगची अरे |
| `soom.minVersion` | आवश्यक किमान WIA SOOM आवृत्ती |

### पाऊल 3: प्लगइन रजिस्ट्रीसाठी सबमिट करा

1. ****Package** your plugin as a ZIP file
2. **Add** तुमचा प्लगइन `plugins/{your-plugin-name}/` मध्ये
3. **Submit** एक Pull Request

### पाऊल 4: पुनरावलोकन आणि मान्यता

आम्ही प्रत्येक प्लगइनचे पुनरावलोकन करतो:

- **सुरक्षा** — धोकादायक APIs नाहीत (पहा [सुरक्षा नियम](#security-rules))
- **गुणवत्ता** — हे कार्य करते का? कोड स्वच्छ आहे का?
- **उपयोगिता** — हे खरे समस्या सोडवते का?

मान्यतेनंतर:
1. तुमचा प्लगइन `registry.json` मध्ये जोडला जातो
2. `dist/` मध्ये एक ZIP बंडल तयार केले जाते
3. तुमचा प्लगइन सर्व WIA SOOM वापरकर्त्यांसाठी **Plugin Store** मध्ये दिसतो!

---

## भाग 5: सर्वोत्तम पद्धती

### सुरक्षा नियम

हे नियम **अनिवार्य** आहेत. जे प्लगइन त्��ांचे उल्लंघन करतात ते नाकारले जातील.

| नियम | का |
|------|-----|
| **कधीही** `eval()` किंवा `new Function()` वापरू नका | कोड इंजेक्शनचा धोका |
| **कधीही** `child_process`, `exec()`, `spawn()` वापरू नका | आदेशांसाठी फक्त `ctx.terminal.send()` वापरा |
| **कधीही** बाह्य URLs मिळवू नका | अपवाद: `wiasoom.com` API समाप्ती |
| **कधीही** `process.env` मध्ये प्रवेश करू नका | पर्यावरणीय चलांमध्ये गुप्त माहिती असू शकते |
| **कधीही** `require('fs')` थेट वापरू नका | संग्रहासाठी `ctx.settings` वापरा, फाइल हस्तांतरणासाठी `ctx.sftp` वापरा |
| **कधीही** npm बाह्य पॅकेजेस वापरू नका | फक्त शुद्ध JavaScript — कोणतेही node_modules नाहीत |
| **आवश्यक** सर्व दूरस्थ आदेशांसाठ�� `ctx.terminal.send()` वापरा | हे सुरक्षित SSH चॅनेलद्वारे जाते |
| **आवश्यक** `deactivate()` मध्ये साफसफाई करा | श्रोते काढा, अंतराल साफ करा |

### त्रुटी हाताळणी

सर्व वेळ धोकादायक ऑपरेशन्स try/catch मध्ये लपवा:
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
### deactivate() मध्ये साफसफाई

जर तुमचा प्लगइन अंतराल, श्रोते, किंवा सदस्यता तयार करत असेल — त्यांना साफ करा:
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
### i18n समर्थन

WIA SOOM 254 भाषांचा समर्थन करतो. तुमचा प्लगइन लेबल अनुवाद करण्यायोग्य बनवण्यासाठी, एक साधा दृष्टिकोन वापरा:
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

## भाग 6: वास्तविक जगातील उदाहरणे

### उदाहरण 1: सर्व्हर डिस्क चेकर

दूरस्थ सर्व्हरवर `df -h` चालवतो आण�� स्थिती बारमध्ये वापरलेले/उपलब्ध जागा दर्शवतो.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

### उदाहरण 2: TODO व्यवस्थापक

एक प्लगइन जो सेटिंग्जचा वापर करून TODO यादी व्यवस्थापित करतो, कायमस्वरूपी संग्रहासाठी आणि प्रदर्शनासाठी वेबव्ह्यू वापरतो.

> **डिझाइन पॅटर्न:** कारण वेबव्ह्यू थेट प्लगइन APIs कॉल करू शकत नाहीत, हा प्लगइन "स्नॅपशॉट" दृष्टिकोन वापरतो — तो सेटिंग्जमधून TODO वाचतो, त्यांना वाचन-फक्त HTML म्हणून रेंडर करतो, आणि आयटम जोडण्यासाठी साइडबार-आधारित क्रिया प्रदान करतो. वेबव्ह्यू एक **प्रदर्शन** स्तर आहे, परस्पर संवादात्मक फॉर्म नाही.
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
---

### उदाहरण 3: त्रुटी वॉचर

टर्मिनल आउटपुटचे निरीक्षण करतो आणि विशिष्ट पॅटर्न शोधल्यावर एक सूचना पाठवतो.
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

## अनुप appendix: श्रेण्या आणि चिन्हे

### प्लगइन श्रेण्या (29)

हे आपल्या `package.json` `keywords` मध्ये किंवा नोंदणीसाठी सादर करताना वापरा:

| श्रेणी | वर्णन |
|----------|-------------|
| `server` | सामान्य सर्व्हर व्यवस्थापन |
| `devtools` | विकास साधने |
| `calculator` | कॅल्क्युलेटर आणि रूपांतरण साधने |
| `simulator` | सिम्युलेटर |
| `game` | टर्मिनल खेळ |
| `business` | व्यवसाय साधने |
| `security` | सुरक्षा आणि ऑडिटिंग |
| `web` | वेब सर्व्हर व्यवस्थापन |
| `education` | शैक्षणिक साधने |
| `health` | आरोग्याशी संबंधित साधने |
| `islamic` | इस्लामी साधने (प्रार्थना वेळ, इ.) |
| `science` | वैज्ञानिक साधने |
| `quantum` | क्वांटम संगणक साधने |
| `ai` | AI-शक्तीवर आधारित साधने |
| `biotech` | बायोटेक्नॉलॉजी साधने |
| `space` | अवकाश आणि खगोलशास्त्र साधने |
| `network` | नेटवर्क साधने |
| `database` | डेटाबेस व्यवस्थापन |
| `monitoring` | सर्व्हर देखरेख |
| `devops` | DevOps आणि CI/CD |
| `utility` | सामान्य उपयोगिताएँ |
| `design` | डिझाइन साधने |
| `ecommerce` | ई-कॉमर्स साधने |
| `automation` | ऑटोमेशन साधने |
| `kpop` | K-pop संबंधित साधने |
| `accessibility` | प्रवेशयोग्यता साधने |
| `analytics` | विश्लेषण आणि अहवाल |
| `wia` | WIA पारिस्थितिकी तंत्र साधने |
| `all` | सर्व श्रेणींमध्ये दिसते |

### शिफारस केलेली चिन्हे (Lucide)

| चिन्हाचे नाव | वापरासाठी |
|-----------|---------|
| `server` | सर्व्हर व्यवस्थ��पन |
| `shield` | सुरक्षा |
| `database` | डेटाबेस |
| `activity` | देखरेख |
| `terminal` | टर्मिनल साधने |
| `code` | विकास |
| `hard-drive` | डिस्क/साठवण |
| `network` | नेटवर्किंग |
| `lock` | प्रमाणीकरण/एन्क्रिप्शन |
| `eye` | पाहणे/देखरेख |
| `check-square` | कार्य/TODO |
| `layout-dashboard` | डॅशबोर्ड |
| `settings` | कॉन्फिगरेशन |
| `zap` | ऑटोमेशन |
| `globe` | वेब/आंतरराष्ट्रीय |

सर्व 1,500+ चिन्हे ब्राउझ करा: [lucide.dev/icons](https://lucide.dev/icons)

---

## मदतीची आवश्यकता आहे का?

- **GitHub समस्या:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **प्लगइन समस्या:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **उदाहरण प्लगइन्स:** [Website](https://wiasoom.com)
- **वेबसाइट:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>काही अद्भुत तयार करा. ते जगासोबत शेअर करा.</em></p>
<p align="center"><em>— WIA SOOM टीम</em></p>
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

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Copy your plugin to `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verify it works: sidebar button appears, features work correctly
4. Test edge cases: what happens if no terminal is connected?

### Step 2: Prepare for submission

Your plugin folder must contain:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | One-line description | `"Monitors nginx access logs in real-time"` |
| `author` | Your name | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | License type (MIT recommended) |
| `keywords` | Array of search tags |
| `soom.minVersion` | Minimum WIA SOOM version required |

### Step 3: Submit to the Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** your plugin to `plugins/{your-plugin-name}/`
3. **Submit** a Pull Request

### Step 4: Review and approval

We review every plugin for:

- **Security** — no dangerous APIs (see [Security Rules](#security-rules))
- **Quality** — does it work? Is the code clean?
- **Usefulness** — does it solve a real problem?

After approval:
1. Your plugin is added to `registry.json`
2. A ZIP bundle is created in `dist/`
3. Your plugin appears in the **Plugin Store** for all WIA SOOM users!

---

## Part 5: Best Practices

### Security Rules

These rules are **mandatory**. Plugins that violate them will be rejected.

| Rule | Why |
|------|-----|
| **NEVER** use `eval()` or `new Function()` | Code injection risk |
| **NEVER** use `child_process`, `exec()`, `spawn()` | Only use `ctx.terminal.send()` for commands |
| **NEVER** fetch external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** access `process.env` | Environment variables may contain secrets |
| **NEVER** use `require('fs')` directly | Use `ctx.settings` for storage, `ctx.sftp` for file transfer |
| **NEVER** use npm external packages | Pure JavaScript only — no node_modules |
| **MUST** use `ctx.terminal.send()` for all remote commands | This goes through the secure SSH channel |
| **MUST** clean up in `deactivate()` | Remove listeners, clear intervals |

### Error Handling

Always wrap risky operations in try/catch:

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

If your plugin creates intervals, listeners, or subscriptions — clean them up:

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
