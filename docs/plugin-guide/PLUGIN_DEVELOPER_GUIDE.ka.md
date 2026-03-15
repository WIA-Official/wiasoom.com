<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM პლაგინის განვითარების სახელმძღვანელო</h1>
<p align="center"><strong>შექმენით თქვენი საკუთარი პლაგინი 5 წუთში.</strong></p>
<p align="center">შექმენით ძლიერი სერვერის ინსტრუმენტები, დაფები და ავტომატიზაციები — პირდაპირ WIA SOOM-ის შიგნით.</p>

---

## შინაარსის ცხრილი

- [ ნაწილი 1: სწრაფი დაწყება — თქვენი პირველი პლაგინი 5 წუთში](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ ნაწილი 2: პლაგინის კონტექსტის API სიგნალები](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ ნაწილი 3: მორგებული UI-ის შექმნა Webviews-ის საშუალებით](#part-3-building-custom-ui-with-webviews)
- [ ნაწილი 4: თქვენი პლაგინის გამოქვეყნება](#part-4-publishing-your-plugin)
- [ ნაწილი 5: საუკეთესო პრაქტიკები](#part-5-best-practices)
- [ ნაწილი 6: რეალური მაგალითები](#part-6-real-world-examples)
- [ დანართი: კატეგორიები და სიმბოლოები](#appendix-categories--icons)

---

## ნაწილი 1: სწრაფი დაწყება — თქვენი პირველი პლაგინი 5 წუთში

### რას შექმნით

"Hello World" პლაგინი, რომელიც sidebar-ში ღილაკს ამატებს. როდესაც დააწკაპებთ, ის აჩვენებს შეტყობინებას.

### ნაბიჯი 1: შექმენით პლაგინის საქაღალე
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### ნაბიჯი 2: შექმენით package.json
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
**სავალდებულო ველები:** `name`, `version`, `description`, `author`, `main`

### ნაბიჯი 3: შექმენით index.js
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
### ნაბიჯი 4: გადატვირთეთ WIA SOOM

გადატვირთეთ აპლიკაცია (ან შეცვალეთ პლაგინის გამორთვა/ჩართვა პარამეტრებში → პლაგინები).

თქვენ უნდა ნახოთ **"Hello World"** ღილაკი sidebar-ში. დააწკაპეთ მასზე — თქვენ ნახავთ წარმატების შეტყობინებას!

### როგორ მუშაობს
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

## ნაწილი 2: პლაგინის კონტექსტის API სიგნალები

როდესაც თქვენი `activate(context)` ფუნქცია იწვევება, `context` (ან `ctx`) უზრუნველყოფს ამ API-ებს:
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

### `ctx.terminal` — ბრძანებების გაწვდვა შორეულ სერვერებზე

#### `terminal.send(sessionId, data)`

გააგზავნეთ ბრძანება (ან ნებისმიერი მონაცემი) აქტიურ ტერმინალ სესიაზე.

| პარამეტრი | ტიპი | აღწერა |
|-----------|------|-------------|
| `sessionId` | `string` | ტერმინალ სესია, რომელზეც უნდა გაწვდოს |
| `data` | `string` | ბრძანება ან მონაცემი, რომელიც უნდა გაწვდოს |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

გამოიწერეთ ყველა გამოსავალი ტერმინალ სესიიდან. აბრუნებს **გამოსწერის ფუნქციას**.

| პარამეტრი | ტიპი | აღწერა |
|-----------|------|-------------|
| `sessionId` | `string` | ტერმინალ სესია, რომელსაც უნდა დააკვირდეთ |
| `callback` | `(data: string) => void` | გამოიძახება თითოეულ გამოსავალზე |
| **აბრუნებს** | `() => void` | გამოიძახეთ ეს მოსმენას შეწყვეტისათვის |
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
**მნიშვნელოვანი:** ყოველთვის შეინახეთ გამოსწერის ფუნქცია და გამოიძახეთ `deactivate()`-ში მეხსიერების გაჟონვის თავიდან ასაცილებლად.

---

### `ctx.sftp` — ფაილების გადაცემა

> **სტატუსი: მალე იქნება** — SFTP API განსაზღვრულია, მაგრამ ჯერ არ არის დაკავშირებული აპლიკაციის SFTP ძრავასთან. `list()` ამჟამად აბრუნებს ცარიელ მასივს, ხოლო `upload()`/`download()` არაფერს აკეთებს. ეს სრულად განხორციელდება მომავალ ვერსიაში. ამჟამად, გამოიყენეთ `ctx.terminal.send()` `scp` ან `rsync` ბრძანებებთან ერთად, როგორც workaround.

#### `sftp.list(sessionId, path)`

შორეულ დირექტორიაში ფაილების ჩამონათვალი.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

ფაილის ატვირთვა ადგილობრივი კომპიუტერიდან შორეულ სერვერზე.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

ფაილის ჩამოტვირთვა შორეული სერვერიდან ადგილობრივ კომპიუტერზე.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (SFTP API-ს გაწვდამდე):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — მომხმარებლის ინტერფეისი

#### `ui.addSidebarButton(options)`

დამატეთ ღილაკი WIA SOOM-ის sidebar-ში.

| ვარიანტი | ტიპი | სავალდებულო | აღწერა |
|--------|------|----------|-------------|
| `id` | `string` | არა | უნიკალური ID (ნაგულისხმევად პლაგინის სახელი) |
| `icon` | `string` | დიახ | Lucide სიმბოლოს სახელი (მაგ., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | დიახ | ღილაკის ტექსტი, რომელიც ჩანს sidebar-ში |
| `onClick` | `() => void` | დიახ | ფუნქცია, რომელიც გამოიძახება, როდესაც ღილაკზე დააწკაპებთ |
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
**სიმბოლოს მითითება:** მოიძიეთ ყველა ხელმისაწვდომი სიმბოლო [lucide.dev/icons](https://lucide.dev/icons)

> **შესაბამისობის შენიშვნა:** ზოგი ძველი პლაგინები იყენებენ პოზიციურ არგუმენტებს, როგორიცაა `addSidebarButton(id, icon, label, onClick)`. ოფიციალური API იყენებს **ვარიანტების ობიექტს**, როგორც ზემოთ არის აღწერილი. ყოველთვის გამოიყენეთ ობიექტის სტილი ახალი პლაგინებისათვის.

#### `ui.openWebview(options)`

გახსენით პოპ-აპის ფანჯარა მორგებული HTML შინაარსით. ასე ქმნით მდიდარ UI-ებს.

| ვარიანტი | ტიპი | აღწერა |
|--------|------|-------------|
| `title` | `string` | ფანჯრის სათაური |
| `html` | `string` | სრული HTML შინაარსი, რომელიც უნდა გამოიცხადოს |
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
> იხილეთ [მესამე ნაწილი](#part-3-building-custom-ui-with-webviews) მოწინავე webview ნიმუშებისთვის.

#### `ui.showNotification(type, message)`

აჩვენეთ ტოსტის შეტყობინება.

| პარამეტრი | ტიპი | აღწერა |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | შეტყობინების სტილი |
| `message` | `string` | ტექსტი, რომელიც უნდა გამოჩნდეს |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

დაამატეთ მუდმივი ტექსტური ელემენტი ქვედა სტატუსის ზოლში.

| პარამეტრი | ტიპი | აღწერა |
|-----------|------|-------------|
| `id` | `string` | უნიკალური ID ამ სტატუსის ელემენტისთვის |
| `text` | `string` | ტექსტი, რომელიც უნდა გამოჩნდეს |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — მუდმივი შენახვა

პლაგინის პარამეტრები მუდმივად ინახება `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`-ში.

#### `settings.get(key)`

წაიკითხეთ შენახული მნიშვნელობა.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
უკვეთს `undefined`, თუ გასაღები არ არსებობს.

#### `settings.set(key, value)`

შეინახეთ მნიშვნელობა. მხარს უჭერს სტრინგებს, რიცხვებს, ბულეანებს, მასივებს და ობიექტებს.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**მაგალითი: მომხმარებლის პრეფერენციების გახსენება**
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

### `ctx.ai` — AI ინტეგრაცია

> **სტატუსი: უახლოეს მომავალში** — AI API განსაზღვრულია, მაგრამ ჯერ არ არის დაკავშირებული Soomy-სთან. ამჟამად აბრუნებს `{ response: 'AI not yet connected' }`. სრული AI ინტეგრაცია დაგეგმილია მომავალ ვერსიაში.

#### `ai.chat(messages, options?)`

გააგზავნეთ შეტყობინებები AI ასისტენტს (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## მესამე ნაწილი: მორგებული UI-ის მშენებლობა Webviews-ის საშუალებით

`openWebview()` API საშუალებას გაძლევთ შექმნათ დეშბორდის UI HTML, CSS და JavaScript-ის გამოყენებით — ყველაფერი პოპაპ ფანჯარაში.

> **მნიშვნელოვანი შეზღუდვა:** Webviews არის **მხოლოდ ჩვენების**. ისინი ვერ გამოიძახებენ პლაგინის API-ებში (`ctx.settings`, `ctx.terminal` და ა.შ.). გამოიყენეთ გვერდითი ღილაკები ყველა მომხმარებლის მოქმედებისთვის და გამოიყენეთ `openWebview()` მი���დინარე მდგომარეობის გამოსაჩენად. თუ საჭიროებთ ინტერაქტიულ ფუნქციებს, გააქტიურეთ ისინი გვერდითი ღილაკებიდან და ხელახლა გახსენით webview, რომ განაახლოთ ჩვენება.

### ნიმუში: ტერმინალის ბრძანება → შედეგის ანალიზი → HTML-ში ჩვენება

ეს არის ყველაზე გავრცელებული პლაგინის ნიმუში. თქვენ ასრულებთ ბრძანებას, ანალიზებთ შედეგს და ვიზუალურად აჩვენებთ მას.
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
### ნიმუში: ინტერაქტიული დეშბორდი ავტომატური განახლებით
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
### ნიმუში: პარამეტრების ჩვენება Webview-ში

> **შენიშვნა:** Webviews არის მხოლოდ ჩვენების — ისინი ვერ გამოიძახებენ პლაგინის API-ებში. გამოიყენეთ `ctx.settings` თქვენს გვერდითი ღილაკების დამხმარე ფუნქციებში პარამეტრების შესაცვლელად და გამოიყენეთ `openWebview()` მიმდინარე მდგომარეობის გამოსაჩენად.
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

## მეოთხე ნაწილი: თქვენი პლაგინის გამოქვეყნება

### ნაბიჯი 1: ადგილობრივად გამოცდა

1. დააკოპირეთ თქვენი პლაგინი `~/.wia-soom/plugins/{your-plugin}/`-ში
2. ხელახლა დაიწყეთ WIA SOOM
3. დაადასტურეთ, რომ მუშაობს: გვერდითი ღილაკი გამოჩნდება, ფუნქციები სწორად მუშაობს
4. შეამოწმეთ კიდური შემთხვევები: რა ხდება, თუ ტერმინალი არ არის დაკავშირებული?

### ნაბიჯი 2: მომზადება წარდგენისთვის

თქვენი პლაგინის საქაღალე უნდა შეიცავდეს:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**მطلობი `package.json` ველები:**

| ველი | აღწერა | მაგალითი |
|-------|-------------|---------|
| `name` | უნიკალური kebab-case ID | `"my-awesome-plugin"` |
| `version` | სემანტიკური ვერსია | `"1.0.0"` |
| `description` | ერთი ხაზის აღწერა | `"Monitors nginx access logs in real-time"` |
| `author` | თქვენი სახელი | `"John Doe"` |
| `main` | შესვლის წერტილი | `"index.js"` |

**სავალდებულო ველები:**

| ველი | აღწერა |
|-------|-------------|
| `license` | ლიცენზიის ტიპი (რეკომენდებულია MIT) |
| `keywords` | ძიების ტეგების მასივი |
| `soom.minVersion` | საჭირო მინიმალური WIA SOOM ვერსია |

### ნაბიჯი 3: გააგზავნეთ Plugin Registry-ში

1. ****Package** your plugin as a ZIP file
2. **მოამატეთ** თქვენი პლაგინი `plugins/{თქვენი-პლაგინის-სახელი}/`
3. **გააგზავნეთ** Pull Request

### ნაბიჯი 4: მიმოხილვა და დამტკიცება

ჩვენ ვიმოწმებთ თითოეულ პლაგინს შემდეგი კრიტერიუმებით:

- **უსაფრთხოება** — არ უნდა იყოს საშიში API-ები (იხილეთ [უსაფრთხოების წესები](#security-rules))
- **ხარისხი** — მუშაობს თუ არა? არის თუ არა კოდი სუფთა?
- **სარგებლიანობა** — ხსნის თუ არა რეალურ პრობლემას?

დამტკიცების შემდეგ:
1. თქვენი პლაგინი დაემატება `registry.json`
2. ZIP პაკეტი შეიქმნება `dist/`
3. თქვენი პლაგინი გამოჩნდება **Plugin Store**-ში ყველა WIA SOOM მომხმარებლისთვის!

---

## ნაწილი 5: საუკეთესო პრაქტიკები

### უსაფრთხოების წესები

ეს წესები არის **სავალდებულო**. პლაგინები, რომლებიც მათ არღვევენ, უარყოფილი იქნება.

| წესი | რატომ |
|------|-----|
| **არასდროს** გამოიყენოთ `eval()` ან `new Function()` | კოდის ინექციის რისკი |
| **არასდროს** გამოიყენოთ `child_process`, `exec()`, `spawn()` | მხოლოდ `ctx.terminal.send()` გამოიყენეთ ბრძანებებისთვის |
| **არასდროს** მოიძიოთ გარე URL-ები | გამონაკლისი: `wiasoom.com` API წერტილები |
| **არასდროს** მიაწვდოთ `process.env` | გარემოს ცვლადებში შეიძლება იყოს საიდუმლო ინფორმაცია |
| **არასდროს** გამოიყენოთ `require('fs')` პირდაპირ | გამოიყენეთ `ctx.settings` შენახვისთვის, `ctx.sftp` ფაილების გადაცემისთვის |
| **უნდა** გამოიყენოთ `ctx.terminal.send()` ყველა შორეული ბრძანებისთვის | ეს უსაფრთხო SSH არხით გადის |
| **უნდა** გაწმინდოთ `deactivate()`-ში | მოაშორეთ მოსმინელები, გაწმინდეთ ინტერვალები |

### შეცდომების მართვა

ყოველთვის მოათავსეთ რისკიანი ოპერაციები try/catch-ში:
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
### გაწმენდა deactivate()-ში

თუ თქვენი პლაგინი ქმნის ინტერვალებს, მოსმენებს ან გამოწერებს — გაწმინდეთ ისინი:
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
### i18n მხარდაჭერა

WIA SOOM მხარს უჭერს 254 ენას. რომ თქვენი პლაგინის ლეიბლი იყოს თარგმნადი, გამოიყენეთ მარტივი მიდგომა:
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

## ნაწილი 6: რეალური მაგალითები

### მაგალითი 1: სერვერის დისკის შემოწმება

შეასრულებს `df -h` შორეულ სერვერზე და აჩვენებს გამოყენებულ/ხელმისაწვდომ სივრცეს სტატუსის ზოლში.
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

### მაგალითი 2: TODO მენეჯერი

პლაგინი, რომელიც მართავს TODO სიას, იყენებს პარამეტრებს მუდმივი შენახვისთვის და ვებსათვალიერებელს ჩვენებისთვის.

> **დიზაინის ნიმუში:** რადგან ვებსათვალიერებლები ვერ შეუძლიათ პირდაპირ გამოიძახონ პლაგინის API-ები, ეს პლაგინი იყენებს "snapshot" მიდგომას — ის კითხულობს TODO-ებს პარამეტრებიდან, წარმოგიდგენთ მათ როგორც წაკითხულ HTML-ს და უზრუნველყოფს გვერდითი მოქმედებების დამატებას. ვებსათვალიერებელი არის **ჩვენების** ფენა, არა ინტერაქტიული ფორმა.
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

### მაგალითი 3: შეცდომების დამკვირვებელი

მოიცავს ტერმინალის გამოსავალს და აგზავნის შეტყობინებას, როდესაც კონკრეტული ნიმუშები იქმნება.
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

## დანართი: კატეგორიები და სიმბოლოები

### პლაგინის კატეგორიები (29)

გამოიყენეთ ეს თქვენი `package.json` `keywords`-ში ან რეგისტრაციის დროს:

| კატეგორია | აღწერა |
|-----------|--------|
| `server` | ზოგადი სერვერის მართვა |
| `devtools` | განვითარების ინსტრუმენტები |
| `calculator` | კალკულატორები და კონვერტორები |
| `simulator` | სიმულატორები |
| `game` | ტერმინალის თამაშები |
| `business` | ბიზნესის ინსტრუმენტები |
| `security` | უსაფრთხოება და აუდიტი |
| `web` | ვებსერვერის მართვა |
| `education` | საგანმანათლებლო ინსტრუმენტები |
| `health` | ჯანმრთელობასთან დაკავშირებული ინსტრუმენტები |
| `islamic` | ისლამური ინსტრუმენტები (ლოცვის დროები და ა.შ.) |
| `science` | სამეცნიერო ინსტრუმენტები |
| `quantum` | კვანტური კომპიუტერული ინსტრუმენტები |
| `ai` | ხელოვნური ინტელექტის ინსტრუმენტები |
| `biotech` | ბიოტექნოლოგიური ინსტრუმენტები |
| `space` | კოსმოსური და ასტრონომიული ინსტრუმენტები |
| `network` | ქსელის ინსტრუმენტები |
| `database` | მონაცემთა ბაზის მართვა |
| `monitoring` | სერვერის მონიტორინგი |
| `devops` | DevOps და CI/CD |
| `utility` | ზოგადი სასარგებლო ინსტრუმენტები |
| `design` | დიზაინის ინსტრუმენტები |
| `ecommerce` | ელექტრონული კომერციის ინსტრუმენტები |
| `automation` | ავტომატიზაციის ინსტრუმენტები |
| `kpop` | K-pop დაკავშირებული ინსტრუმენტებ�� |
| `accessibility` | ხელმისაწვდომობის ინსტრუმენტები |
| `analytics` | ანალიტიკა და ანგარიშგება |
| `wia` | WIA ეკოსისტემის ინსტრუმენტები |
| `all` | ყველა კატეგორიაში ჩანს |

### რეკომენდირებული სიმბოლოები (Lucide)

| სიმბოლოს სახელი | გამოყენება |
|----------------|-----------|
| `server` | სერვერის მართვა |
| `shield` | უსაფრთხოება |
| `database` | მონაცემთა ბაზა |
| `activity` | მონიტორინგი |
| `terminal` | ტერმინალის ინსტრუმენტები |
| `code` | განვითარება |
| `hard-drive` | დისკი/შენახვა |
| `network` | ���სელური |
| `lock` | ავტორიზაცია/შიფრირება |
| `eye` | თვალყურის დევნება/მონიტორინგი |
| `check-square` | დავალებები/TODO |
| `layout-dashboard` | დეშბორდები |
| `settings` | კონფიგურაცია |
| `zap` | ავტომატიზაცია |
| `globe` | ვებს/საერთაშორისო |

გადახედეთ ყველა 1,500+ სიმბოლოს: [lucide.dev/icons](https://lucide.dev/icons)

---

## დაგჭირდებათ დახმარება?

- **GitHub პრობლემები:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **პლაგინის პრობლემები:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **მაგალითი პლაგინები:** [Website](https://wiasoom.com)
- **ვებსაიტი:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>შექმენით რაღაც საოცარი. გაუზიარეთ ეს მსოფლიოს.</em></p>
<p align="center"><em>— WIA SOOM გუნდი</em></p>