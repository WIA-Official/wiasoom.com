<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM প্লাগিন ডেভেলপার গাইড</h1>
<p align="center"><strong>৫ মিনিটে আপনার নিজস্ব প্লাগিন তৈরি করুন।</strong></p>
<p align="center">শক্তিশালী সার্ভার টুল, ড্যাশবোর্ড এবং অটোমেশন তৈরি করুন — WIA SOOM এর ভিতরেই।</p>

---

## বিষয়বস্তু

- [অংশ ১: দ্রুত শুরু — ৫ মিনিটে আপনার প্রথম প্লাগিন](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [অংশ ২: প্লাগিন কনটেক্সট API রেফারেন্স](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [অংশ ৩: ওয়েবভিউ ব্যবহার করে কাস্টম UI তৈরি করা](#part-3-building-custom-ui-with-webviews)
- [অংশ ৪: আপনার প্লাগিন প্রকাশ করা](#part-4-publishing-your-plugin)
- [অংশ ৫: সেরা অনুশীলন](#part-5-best-practices)
- [অংশ ৬: বাস্তব উদাহরণ](#part-6-real-world-examples)
- [পরিশিষ্ট: বিভাগ ও আইকন](#appendix-categories--icons)

---

## অংশ ১: দ্রুত শুরু — ৫ মিনিটে আপনার প্রথম প্লাগিন

### আপনি কি তৈরি করবেন

একটি "হ্যালো ওয়ার্ল্ড" প্লাগিন যা সাইডবারে একটি বোতাম যোগ করে। বোতামটিতে ক্লিক করলে একটি নোটিফিকেশন দেখায়।

### ধাপ ১: প্লাগিন ফোল্ডার তৈরি করুন
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### ধাপ ২: package.json তৈরি করুন
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
**প্রয়োজনীয় ক্ষেত্র:** `name`, `version`, `description`, `author`, `main`

### ধাপ ৩: index.js তৈরি করুন
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
### ধাপ ৪: WIA SOOM পুনরায় চালু করুন

অ্যাপটি পুনরায় চালু করুন (অথবা সেটিংস → প্লাগিনে প্লাগিনটি বন্ধ/চালু করুন)।

আপনার সাইডবারে একটি **"হ্যালো ওয়ার্ল্ড"** বোতাম দেখতে পাবেন। এটিতে ক্লিক করুন — আপনি একটি সফল নোটিফিকেশন দেখবেন!

### এটি কিভাবে কাজ করে
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

## অংশ ২: প্লাগিন কনটেক্সট API রেফারেন্স

যখন আপনার `activate(context)` ফাংশনটি কল করা হয়, `context` (অথবা `ctx`) এই API গুলি প্রদান করে:
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

### `ctx.terminal` — রিমোট সার্ভারে কমান্ড চালান

#### `terminal.send(sessionId, data)`

একটি সক্রিয় টার্মিনাল সেশনে একটি কমান্ড (অথবা যেকোনো ডেটা) পাঠান।

| প্যারামিটার | প্রকার | বর্ণনা |
|-------------|--------|---------|
| `sessionId` | `string` | পাঠানোর জন্য টার্মিনাল সেশন |
| `data` | `string` | পাঠানোর জন্য কমান্ড বা ডেটা |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

একটি টার্মিনাল সেশনের সমস্ত আউটপুটের জন্য সাবস্ক্রাইব করুন। একটি **আনসাবস্ক্রাইব ফাংশন** ফেরত দেয়।

| প্যারামিটার | প্রকার | বর্ণনা |
|-------------|--------|---------|
| `sessionId` | `string` | দেখার জন্য টার্মিনাল সেশন |
| `callback` | `(data: string) => void` | প্রতিটি আউটপুটের অংশের সাথে কল করা হয় |
| **ফেরত দেয়** | `() => void` | শোনা বন্ধ করতে এটি কল করুন |
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
**গুরুত্বপূর্ণ:** সর্বদা আনসাবস্ক্রাইব ফাংশনটি সংরক্ষণ করুন এবং মেমরি লিক প্রতিরোধের জন্য `deactivate()` এ এটি কল করুন।

---

### `ctx.sftp` — ফাইল স্থানান্তর

> **অবস্থা: শীঘ্রই আসছে** — SFTP API সংজ্ঞায়িত করা হয়েছে কিন্তু এখনও অ্যাপের SFTP ইঞ্জিনের সাথে সংযুক্ত নয়। `list()` বর্তমানে একটি খালি অ্যারে ফেরত দেয়, এবং `upload()`/`download()` কোনো কার্যকরী নয়। এটি ভবিষ্যতের একটি রিলিজে সম্পূর্ণরূপে বাস্তবায়িত হবে। আপাতত, `scp` বা `rsync` কমান্ডের সাথে `ctx.terminal.send()` ব্যবহার করুন।

#### `sftp.list(sessionId, path)`

একটি রিমোট ডিরেক্টরিতে ফাইলের তালিকা করুন।
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

লোকাল মেশিন থেকে রিমোট সার্ভারে একটি ফাইল আপলোড করুন।
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

রিমোট সার্ভার থেকে লোকাল মেশিনে একটি ফাইল ডাউনলোড করুন।
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**কাজের বিকল্প (যতক্ষণ না SFTP API লাইভ):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — ব্যবহারকারী ইন্টারফেস

#### `ui.addSidebarButton(options)`

WIA SOOM সাইডবারে একটি বোতাম যোগ করুন।

| বিকল্প | প্রকার | প্রয়োজনীয় | বর্ণনা |
|--------|--------|-------------|---------|
| `id` | `string` | না | অনন্য আইডি (ডিফল্ট প্লাগিন নাম) |
| `icon` | `string` | হ্যাঁ | Lucide আইকনের নাম (যেমন, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | হ্যাঁ | সাইডবারে প্রদর্শিত বোতামের টেক্সট |
| `onClick` | `() => void` | হ্যাঁ | বোতামে ক্লিক করলে কল করা হয় এমন ফাংশন |
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
**আইকন রেফারেন্স:** স��স্ত উপলব্ধ আইকন ব্রাউজ করুন [lucide.dev/icons](https://lucide.dev/icons)

> **সঙ্গতিপূর্ণ নোট:** কিছু পুরানো প্লাগিন পজিশনাল আর্গুমেন্ট ব্যবহার করে যেমন `addSidebarButton(id, icon, label, onClick)`। অফিসিয়াল API একটি **বিকল্প অবজেক্ট** ব্যবহার করে যেমন উপরে ডকুমেন্ট করা হয়েছে। নতুন প্লাগিনের জন্য সর্বদা অবজেক্ট স্টাইল ব্যবহার করুন।

#### `ui.openWebview(options)`

কাস্টম HTML কনটেন্ট সহ একটি পপআপ উইন্ডো খুলুন। এভাবেই আপনি সমৃদ্ধ UI তৈরি করেন।

| বিকল্প | প্রকার | বর্ণনা |
|--------|--------|---------|
| `title` | `string` | উইন্ডোর শিরোনাম |
| `html` | `string` | রেন্ডার করার জন্য পূর্ণ HTML কনটেন্ট |
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
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

একটি টোস্ট নোটিফিকেশন দেখান।

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | নোটিফিকেশন শৈলী |
| `message` | `string` | দেখানোর জন্য টেক্সট |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

নিচের স্ট্যাটাস বারে একটি স্থায়ী টেক্সট আইটেম যোগ করুন।

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | এই স্ট্যাটাস আইটেমের জন্য অনন্য আইডি |
| `text` | `string` | প্রদর্শনের জন্য টেক্সট |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — স্থায়ী স্টোরেজ

প্লাগিনের সেটিংস স্থায়ীভাবে `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` এ সংরক্ষিত হয়।

#### `settings.get(key)`

একটি সংরক্ষিত মান পড়ুন।
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
যদি কীটি বিদ্যমান না থাকে তবে `undefined` ফেরত দেয়।

#### `settings.set(key, value)`

একটি মান সংরক্ষণ করুন। স্ট্রিং, সংখ্যা, বুলিয়ান, অ্যারে এবং অবজেক্ট সমর্থন করে।
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**উদাহরণ: ব্যবহারকারীর পছন্দগুলি মনে রাখা**
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

### `ctx.ai` — AI ইন্টিগ্রেশন

> **স্ট্যাটাস: শীঘ্রই আসছে** — AI API সংজ্ঞায়িত হয়েছে কিন্তু এখনও Soomy এর সাথে সংযুক্ত নয়। বর্তমানে `{ response: 'AI not yet connected' }` ফেরত দেয়। পূর্ণ AI ইন্টিগ্রেশন ভবিষ্যতের একটি রিলিজের জন্য পরিকল্প��ত।

#### `ai.chat(messages, options?)`

AI সহায়কের (Soomy) কাছে বার্তা পাঠান।
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Webviews এর সাথে কাস্টম UI তৈরি করা

`openWebview()` API আপনাকে HTML, CSS, এবং JavaScript দিয়ে ড্যাশবোর্ড UI তৈরি করতে দেয় — সবকিছু একটি পপআপ উইন্ডোর ভিতরে।

> **গুরুতর সীমাবদ্ধতা:** Webviews হল **শুধুমাত্র প্রদর্শন**। এগুলি প্লাগিন API গুলিতে (যেমন `ctx.settings`, `ctx.terminal`, ইত্যাদি) কল করতে পারে না। সমস্ত ব্যবহারকারীর ক্রিয়ার জন্য সাইডবার বোতাম ব্যবহার করুন, এবং বর্তমান অবস্থা প্রদর্শনের জন্য `openWebview()` ব্যবহার করুন। যদি আপনি ইন্টারেক্টিভ বৈশিষ্ট্যগুলির প্রয়োজন হয়, তবে সেগুলি সাইডবার বোতাম থেকে ট্রিগার ক��ুন এবং প্রদর্শন রিফ্রেশ করতে ওয়েবভিউ পুনরায় খুলুন।

### প্যাটার্ন: টার্মিনাল কমান্ড → আউটপুট পার্স করা → HTML তে দেখানো

এটি সবচেয়ে সাধারণ প্লাগিন প্যাটার্ন। আপনি একটি কমান্ড চালান, ফলাফল পার্স করেন, এবং এটি ভিজ্যুয়ালি প্রদর্শন করেন।
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
### প্যাটার্ন: স্বয়ংক্রিয় রিফ্রেশ সহ ইন্টারেক্টিভ ড্যাশবোর্ড
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
### প্যাটার্ন: একটি Webview তে সেটিংস প্রদর্শন করা

> **নোট:** Webviews হল শুধুমাত্র প্রদর্শন — এগুলি প্লাগিন API গুলিতে কল করতে পারে না। সেটিংস পরিবর্তন করতে আপনার সাইডবার বোতাম হ্যান্ডলারগুলিতে `ctx.settings` ব্যবহার করুন, এবং বর্তমান অবস্থা দেখানোর জন্য `openWebview()` ব্যবহার করুন।
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

## Part 4: আপনার প্লাগিন প্রকাশ করা

### পদক্ষেপ 1: স্থানীয়ভাবে পরীক্ষা করুন

1. আপনার প্লাগিনটি `~/.wia-soom/plugins/{your-plugin}/` এ কপি করুন।
2. WIA SOOM পুনরায় চালু করুন।
3. এটি কাজ করছে কিনা তা নিশ্চিত করুন: সাইডবার বোতাম প্রদর্শিত হচ্ছে, বৈশিষ্ট্যগুলি সঠিকভাবে কাজ করছে।
4. প্রান্তের কেসগুলি পরীক্ষা করুন: যদি কোনও টার্মিনাল সংযুক্ত না থাকে তবে কি হয়?

### পদক্ষেপ 2: জমার জন্য প্রস্তুতি নিন

আপনার প্লাগিন ফোল্ডারে থাকতে হবে:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**প্রয়োজনীয় `package.json` ক্ষেত্রসমূহ:**

| ক্ষেত্র | বর্ণনা | উদাহরণ |
|-------|-------------|---------|
| `name` | অনন্য kebab-case ID | `"my-awesome-plugin"` |
| `version` | সেমান্টিক সংস্করণ | `"1.0.0"` |
| `description` | এক লাইনের বর্ণনা | `"Monitors nginx access logs in real-time"` |
| `author` | আপনার নাম | `"John Doe"` |
| `main` | এন্ট্রি পয়েন্ট | `"index.js"` |

**ঐচ্ছিক ক্ষেত্রসমূহ:**

| ক্ষেত্র | বর্ণনা |
|-------|-------------|
| `license` | লাইসেন্সের প্রকার (MIT সুপারিশকৃত) |
| `keywords` | অনুসন্ধান ট্যাগের অ্যারে |
| `soom.minVersion` | প্রয়োজনীয় সর্বনিম্ন WIA SOOM সংস্করণ |

### পদক্ষেপ ৩: প্লাগইন রেজিস্ট্রিতে জমা দিন

1. ****Package** your plugin as a ZIP file
2. **Add** আপনার প্লাগইন `plugins/{your-plugin-name}/` এ
3. **Submit** একটি Pull Request

### পদক্ষেপ ৪: পর্যালোচনা এবং অনুমোদন

আমরা প্রতিটি প্লাগইন পর্যালোচনা করি:

- **নিরাপত্তা** — বিপজ্জনক API নেই (দেখুন [নিরাপত্তা নিয়ম](#security-rules))
- **গুণমান** — এটি কি কাজ করে? কোড কি পরিষ্কার?
- **উপকারিতা** — এটি কি একটি বাস্তব সমস্যা সমাধান করে?

অনুমোদনের পরে:
1. আপনার প্লাগইন `registry.json` এ যোগ করা হয়
2. `dist/` এ একটি ZIP প্যাকেজ তৈরি হয়
3. আপনার প্লাগইন **Plugin Store** এ সমস্ত WIA SOOM ব্যবহারকারীদের জন্য প্রদর্শিত হয়!

---

## অংশ ৫: সেরা অনুশীলনসমূহ

### নিরাপত্তা নিয়ম

এই নিয়মগুলি **বাধ্যতামূলক**। যেসব প্লাগইন এগু���ি লঙ্ঘন করে সেগুলি প্রত্যাখ্যাত হবে।

| নিয়ম | কেন |
|------|-----|
| **কখনোই** `eval()` বা `new Function()` ব্যবহার করবেন না | কোড ইনজেকশনের ঝুঁকি |
| **কখনোই** `child_process`, `exec()`, `spawn()` ব্যবহার করবেন না | শুধুমাত্র `ctx.terminal.send()` কমান্ডের জন্য ব্যবহার করুন |
| **কখনোই** বাইরের URL সংগ্রহ করবেন না | ব্যতিক্রম: `wiasoom.com` API এন্ডপয়েন্ট |
| **কখনোই** `process.env` অ্যাক্সেস করবেন না | পরিবেশের ভেরিয়েবলগুলোতে গোপনীয়তা থাকতে পারে |
| **কখনোই** সরাসরি `require('fs')` ব্যবহার করবেন না | সংরক্ষণের জন্য `ctx.settings` ব্যবহার করুন, ফাইল স্থানান্তরের জন্য `ctx.sftp` ব্যবহার করুন |
| **কখনোই** npm বাইরের প্যাকেজ ব্যবহার করবেন ���া | শুধুমাত্র খাঁটি JavaScript — কোন node_modules নেই |
| **অবশ্যই** সমস্ত দূরবর্তী কমান্ডের জন্য `ctx.terminal.send()` ব্যবহার করুন | এটি নিরাপদ SSH চ্যানেলের মাধ্যমে যায় |
| **অবশ্যই** `deactivate()` এ পরিষ্কার করুন | শ্রোতা মুছে ফেলুন, অন্তর্বর্তী সময় পরিষ্কার করুন |

### ত্রুটি পরিচালনা

সবসময় ঝুঁকিপূর্ণ অপারেশনগুলোকে try/catch এর মধ্যে রাখুন:
§§§CHUNK_SEPARATOR§§§
### deactivate() এ পরিষ্কার করা

যদি আপনার প্লাগইন অন্তর্বর্তী সময়, শ্রোতা, বা সাবস্ক্রিপশন তৈরি করে — সেগুলো পরিষ্কার করুন:
§§§CHUNK_SEPARATOR§§§
### i18n সমর্থন

WIA SOOM ২৫৪টি ভাষা সমর্থন করে। আপনার প্লাগইন লেবেল অনুবাদযোগ্য করতে, একটি সহজ পদ্ধতি ব্যবহার করুন:
§§§CHUNK_SEPARATOR§§§
---

## অংশ ৬: বাস্তব-জীবনের উদাহরণসমূহ

### উদাহরণ ১: সার্ভার ডিস্ক চেকার

দূরবর্তী সার্ভারে `df -h` চালায় এবং স্ট্যাটাস বারে ব্যবহৃত/উপলব্ধ স্থান দেখায়।
§§§CHUNK_SEPARATOR§§§
---

### উদাহরণ ২: TODO ম্যানেজার

একটি প্লাগইন যা একটি TODO তালিকা পরিচালনা করে স্থায়ী সংরক্ষণের জন্য সেটিংস এবং প্রদর্শনের জন্য একটি ওয়েবভিউ ব্যবহার করে।

> **ডিজাইন প্যাটার্ন:** যেহেতু ওয়েবভিউ সরাসরি প্লাগইন API কল করতে পারে না, এই প্লাগইন একটি "স্ন্যাপশট" পদ্ধতি ব্যবহার করে — এটি সেটিংস থেকে TODO পড়ে, সেগুলোকে পড়ার জন্য HTML হিসেবে রেন্ডার করে, এবং আইটেম যোগ করার জন্য সাইডবার-ভিত্তিক কার্যক্রম প্রদান করে। ওয়েবভিউ একটি **প্রদর্শন** স্তর, এটি একটি ইন্টারেক্টিভ ফর্ম নয়।
§§§CHUNK_SEPARATOR§§§
---

### উদাহরণ ৩: ত্রুটি পর্যবেক্ষক

টার্মিনাল আউটপুট পর্যবেক্ষণ করে এবং নির্দিষ্ট প্যাটার্ন সনাক্ত হলে একটি বিজ্ঞপ্তি পাঠায়।
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

## পরিশিষ্ট: ক্যাটাগরি ও আইকন

### প্লাগইন ক্যাটাগরি (২৯)

আপনার `package.json` এ `keywords` হিসেবে বা রেজিস্ট্রিতে জমা দেওয়ার সময় এগুলি ব্যবহার করুন:

| ক্যাটাগরি | বর্ণনা |
|-----------|--------|
| `server` | সাধারণ সার্ভার ব্যবস্থাপনা |
| `devtools` | উন্নয়ন সরঞ্জাম |
| `calculator` | ক্যালকুলেটর এবং কনভার্টার |
| `simulator` | সিমুলেটর |
| `game` | টার্মিনাল গেমস |
| `business` | ব্যবসায়িক সরঞ্জাম |
| `security` | নিরাপত্তা এবং নিরীক্ষা |
| `web` | ওয়েব সার্ভার ব্যবস্থাপনা |
| `education` | শিক্ষা সংক্রান��ত সরঞ্জাম |
| `health` | স্বাস্থ্য সম্পর্কিত সরঞ্জাম |
| `islamic` | ইসলামিক সরঞ্জাম (নামাজের সময়, ইত্যাদি) |
| `science` | বৈজ্ঞানিক সরঞ্জাম |
| `quantum` | কোয়ান্টাম কম্পিউটিং সরঞ্জাম |
| `ai` | AI-চালিত সরঞ্জাম |
| `biotech` | বায়োটেকনোলজি সরঞ্জাম |
| `space` | মহাকাশ এবং জ্যোতির্বিজ্ঞান সরঞ্জাম |
| `network` | নেটওয়ার্ক সরঞ্জাম |
| `database` | ডেটাবেস ব্যবস্থাপনা |
| `monitoring` | সার্ভার মনিটরিং |
| `devops` | DevOps এবং CI/CD |
| `utility` | সাধারণ ইউটিলিটি |
| `design` | ডিজাইন সরঞ্জাম |
| `ecommerce` | ই-কমার্স সরঞ্জাম |
| `automation` | অটোমেশন সরঞ্জাম |
| `kpop` | K-pop সম্পর্কিত সরঞ্জাম |
| `accessibility` | অ্যাক্সেসিবিলিটি সরঞ্জাম |
| `analytics` | বিশ্লেষণ এবং রিপোর্টিং |
| `wia` | WIA ইকোসিস্টেম সরঞ্জাম |
| `all` | সব ক্যাটাগরিতে উপস্থিত |

### সুপারিশকৃত আইকন (Lucide)

| আইকনের নাম | ব্যবহারের জন্য |
|-------------|----------------|
| `server` | সার্ভার ব্যবস্থাপনা |
| `shield` | নিরাপত্তা |
| `database` | ডেটাবেস |
| `activity` | মনিটরিং |
| `terminal` | টার্মিনাল সরঞ্জাম |
| `code` | উন্নয়ন |
| `hard-drive` | ডিস্ক/স্টোরেজ |
| `network` | নেটওয়ার্কিং |
| `lock` | প্রমাণীকরণ/এনক্রিপশন |
| `eye` | দেখার/মনিটরিং |
| `check-square` | কাজ/TODO |
| `layout-dashboard` | ড্যাশবোর্ড |
| `settings` | কনফিগারেশন |
| `zap` | অটোমেশন |
| `globe` | ওয়েব/আন্তর্জাতিক |

সব ১,৫০০+ আইকন ব্রাউজ করুন: [lucide.dev/icons](https://lucide.dev/icons)

---

## সাহায্যের প্রয়োজন?

- **GitHub সমস্যা:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **প্লাগইন সমস্যা:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **উদাহরণ প্লাগইন:** [Website](https://wiasoom.com)
- **ওয়েবসাইট:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>কিছু অসাধারণ তৈরি করুন। এটি বিশ্বের সাথে শেয়ার করুন।</em></p>
<p align="center"><em>— WIA SOOM টিম</em></p>
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
