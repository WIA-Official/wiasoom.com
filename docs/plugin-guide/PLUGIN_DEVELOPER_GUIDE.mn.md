<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Плагин Хөгжүүлэгчийн Гарын Авлага</h1>
<p align="center"><strong>5 минутын дотор өөрийн плагинаа бүтээ.</strong></p>
<p align="center">WIA SOOM дотор хүчирхэг серверийн хэрэгслүүд, самбар, автоматжуулалтуудыг бий болго.</p>

---

## Агуулгын Жагсаалт

- [1-р хэсэг: Хурдан эхлэл — Таны анхны плагин 5 минутын дотор](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [2-р хэсэг: Плагин Контекст API-ийн лавлах](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [3-р хэсэг: Веб харагдацтай өөрийн UI-г бүтээх](#part-3-building-custom-ui-with-webviews)
- [4-р хэсэг: Плагинаа нийтлэх](#part-4-publishing-your-plugin)
- [5-р хэсэг: Шилдэг практик](#part-5-best-practices)
- [6-р хэсэг: Жинхэнэ жишээнүүд](#part-6-real-world-examples)
- [Нэмэлт: Ангилал & Икон](#appendix-categories--icons)

---

## 1-р хэсэг: Хурдан эхлэл — Таны анхны плагин 5 минутын дотор

### Та юу бүтээх вэ

Сайдбар дээр товчлуур нэмдэг "Hello World" плагин. Дарахад, мэдэгдэл гарч ирнэ.

### Алхам 1: Плагин хавтсыг бий болго
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Алхам 2: package.json үүсгэх
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
**Шаардлагатай талбарууд:** `name`, `version`, `description`, `author`, `main`

### Алхам 3: index.js үүсгэх
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
### Алхам 4: WIA SOOM-ыг дахин эхлүүлэх

Апп-ыг дахин эхлүүл (эсвэл Тохиргоо → Плагинууд хэсэгт плагинаа унтрааж/асгах).

Сайдбар дээр **"Hello World"** товчлуур харагдах ёстой. Дараад үзээрэй — та ��мжилтын мэдэгдэл харах болно!

### Энэ хэрхэн ажилладаг
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

## 2-р хэсэг: Плагин Контекст API-ийн лавлах

Таны `activate(context)` функц дуудлагдсан үед, `context` (эсвэл `ctx`) эдгээр API-уудыг хангана:
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

### `ctx.terminal` — Алсын серверүүд дээр командуудыг гүйцэтгэх

#### `terminal.send(sessionId, data)`

Идэвхтэй терминалын сессид команд (эсвэл ямар ч өгөгдөл) илгээх.

| Параметр | Төрөл | Тайлбар |
|----------|-------|---------|
| `sessionId` | `string` | Илгээх терминалын сесс |
| `data` | `string` | Илгээх команд эсвэл өгөгдөл |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Терминалын сессийн бүх гаралтыг захиалах. **Захиалгыг цуцлах функц** буцаана.

| Параметр | Төрөл | Тайлбар |
|----------|-------|---------|
| `sessionId` | `string` | Хянах терминалын сесс |
| `callback` | `(data: string) => void` | Гаралтын бүр хэсэгтэй дуудна |
| **Буцаах** | `() => void` | Хүлээн авахыг зогсоохын тулд дуудна |
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
**Чухал:** Завсарлах функцыг үргэлж хадгалаад, `deactivate()` дотор дуудна уу, ингэснээр санах ой алдагдлаас зайлсхийх болно.

---

### `ctx.sftp` — Файлын дамжуулалт

> **Төлөв: Ойрын үед ирнэ** — SFTP API тодорхойлогдсон боловч апп-ийн SFTP хөдөлгүүрт холбогдоогүй байна. `list()` одоогоор хоосон массив буцааж, `upload()`/`download()` нь үйлдэлгүй байна. Энэ нь ирээдүйн хувилбарт бүрэн хэрэгжих болно. Одоогоор, `scp` эсвэл `rsync` командуудтай `ctx.terminal.send()`-ийг ашиглана уу.

#### `sftp.list(sessionId, path)`

Алсын директорт файлуудыг жагсаах.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Местийн компьютераас алсын сервер рүү файл илгээх.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Алсын серверээс местий�� компьютер рүү файл татаж авах.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Альтернатив (SFTP API идэвхжих хүртэл):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Хэрэглэгчийн интерфейс

#### `ui.addSidebarButton(options)`

WIA SOOM сайдбар дээр товчлуур нэмэх.

| Сонголт | Төрөл | Шаардлагатай | Тайлбар |
|---------|-------|---------------|---------|
| `id` | `string` | Үгүй | Онцгой ID (плагин нэрээр үндсэндээ) |
| `icon` | `string` | Тийм | Lucide икон нэр (жишээ нь, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Тийм | Сайдбар дээр харагдах товчлуурын текст |
| `onClick` | `() => void` | Тийм | Товчлуурыг дархад дуудлагдах функц |
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
**Икон лавлах:** Бүх боломжит иконуудыг [lucide.dev/icons](https://lucide.dev/icons) хуудаснаас үзнэ үү.

> **Зохицох тэмдэглэл:** Зарим хуучин плагинууд `addSidebarButton(id, icon, label, onClick)` шиг байрлалаар аргументуудыг ашигладаг. Албан ёсны API дээрх шиг **сонголтын объект** ашигладаг. Шинэ плагинуудын хувьд үргэлж объектын хэв маягийг ашиглаарай.

#### `ui.openWebview(options)`

Өөрийн HTML агуулгатай попап цонх нээх. Энэ нь та баялаг UI-уудыг хэрхэн бүтээх вэ.

| Сонголт | Төрөл | Тайлбар |
|---------|-------|---------|
| `title` | `string` | Цонхны гарчиг |
| `html` | `string` | Бүтээх бүрэн HTML агуулга |
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
> [3-р хэсэг](#part-3-building-custom-ui-with-webviews)-д дэвшилтэт веб үзэгдлийн загваруудыг үзнэ үү.

#### `ui.showNotification(type, message)`

Тост мэдэгдэл харуулах.

| Параметр | Төрөл | Тайлбар |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Мэдэгдлийн стиль |
| `message` | `string` | Харуулах текст |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Доод статус бар руу тогтмол текст элемент нэмэх.

| Параметр | Төрөл | Тайлбар |
|-----------|------|-------------|
| `id` | `string` | Энэ статус элементын өвөрмөц ID |
| `text` | `string` | Харуулах текст |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Тогтмол хадгалах

Плагиний тохиргоонууд `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`-д байнга хадгалагдана.

#### `settings.get(key)`

Хадгалагдсан утгыг унших.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Хлюч байхгүй бол `undefined` буцаана.

#### `settings.set(key, value)`

Утгыг хадгалах. Стринг, тоо, логик, массив, болон объектуудыг дэмжинэ.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Жишээ: Хэрэглэгчийн тохиргоог санах**
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

### `ctx.ai` — AI интеграци

> **Төлөв: Ирэхэд бэлэн** — AI API тодорхойлогдсон боловч Soomy-д холбогдоогүй. Одоогоор `{ response: 'AI not yet connected' }` буцаана. Бүтэн AI интеграци ирээдүйн хувилбарт төлөвлөгдсөн.

#### `ai.chat(messages, options?)`

AI туслах (Soomy)-д мессеж илгээх.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## 3-р хэсэг: Веб үзэгдлүүдээр өөрийн UI-ийг бүтээх

`openWebview()` API нь HTML, CSS, болон JavaScript-ийг ашиглан самбарын UI-ийг popup цонхонд бүтээх боломжийг олгодог.

> **Чухал хязгаарлалт:** Веб үзэгдлүүд нь **зөвхөн харуулах** зориулалттай. Тэд плагиний API-ууд руу бу��аж дуудаж чадахгүй (`ctx.settings`, `ctx.terminal`, гэх мэт). Хэрэглэгчийн бүх үйлдлүүдийн хувьд sidebar товчлууруудыг ашиглаж, одоогийн төлвийг харуулахын тулд `openWebview()`-ийг ашиглаарай. Хэрэв интерактив функцүүд хэрэгтэй бол, тэдгээрийг sidebar товчлуураас идэвхжүүлж, харуулалтыг шинэчлэхийн тулд веб үзэгдлийг дахин нээгээрэй.

### Загвар: Терминалын команд → Гаралтыг задлах → HTML-д харуулах

Энэ нь хамгийн түгээмэл плагиний загвар юм. Та команд ажиллуулж, үр дүнг задлан, визуалаар харуулдаг.
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
### Загвар: Автомат шинэчлэлттэй интерактив самбар
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
### Загвар: Веб үзэгдэлд тохиргоог харуулах

> **Анхаар:** Веб үзэгдлүүд нь зөвхөн харуулах зориулалттай — тэд плагиний API-ууд руу буцаж дуудаж чадахгүй. Тохиргоог өөрчлөхийн тулд sidebar товчлуурын хандалтуудад `ctx.settings`-ийг ашиглаж, одоогийн төлвийг харуулахын тулд `openWebview()`-ийг ашиглаарай.
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

## 4-р хэсэг: Плагинаа нийтлэх

### Алхам 1: Орон нутгийн тест

1. Плагинаа `~/.wia-soom/plugins/{your-plugin}/` руу хуулна уу.
2. WIA SOOM-ыг дахин эхлүүлнэ.
3. Энэ нь ажиллаж байгааг шалгана: sidebar товчлуур гарч ирэх, функцууд зөв ажиллах.
4. Эцсийн тохиолдлуудыг тестлэх: терминал холбогдоогүй бол юу болох вэ?

### Алхам 2: Илгээхэд бэлдэх

Таны плагиний хавтас дараах зүйлсийг агуулсан байх ёстой:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Шаардлагатай `package.json` талбарууд:**

| Талбар | Тодорхойлолт | Жишээ |
|-------|-------------|---------|
| `name` | Онцгой kebab-case ID | `"my-awesome-plugin"` |
| `version` | Семантик хувилбар | `"1.0.0"` |
| `description` | Нэг мөрийн тодорхойлолт | `"Monitors nginx access logs in real-time"` |
| `author` | Таны нэр | `"John Doe"` |
| `main` | Орох цэг | `"index.js"` |

**Сонголттой талбарууд:**

| Талбар | Тодорхойлолт |
|-------|-------------|
| `license` | Лицензийн төрөл (MIT санал болгож байна) |
| `keywords` | Хайлт шошгын массив |
| `soom.minVersion` | Шаардлагатай хамгийн бага WIA SOOM хувилбар |

### Алхам 3: Плагин Бүртгэлд Илгээх

1. ****Package** your plugin as a ZIP file
2. **Нэмэх** таны плагин `plugins/{your-plugin-name}/`
3. **Илгээх** Pull Request

### Алхам 4: Шалгалт ба баталгаа

Бид бүх плагинаа дараах зүйлсийн төлөө шалгадаг:

- **Аюулгүй байдал** — аюултай API-ууд байхгүй (үзнэ үү [Аюулгүй байдлын дүрэм](#security-rules))
- **Чанар** — энэ ажилладаг уу? Код цэвэр үү?
- **Ашиг тус** — энэ бодит асуудлыг шийдэж байна уу?

Баталгаажсаны дараа:
1. Таны плагин `registry.json`-д нэмэгдэнэ
2. `dist/`-д ZIP багц үүсгэнэ
3. Таны плагин **Plugin Store**-д бүх WIA SOOM хэрэглэгчдэд харагдана!

---

## 5-р хэсэг: Шилдэг практик

### Аюулгүй байдлын дүрэм

Эдгээр дүрмүүд **ал必**. Эдгээрийг зөрчсөн плагинууд татгалзах болно.

| Дүрэм | Яагаад |
|------|-----|
| **ХЭЗЭЭ Ч** `eval()` эсвэл `new Function()` ашиглахгүй | Кодын оруулгын эрсдэл |
| **ХЭЗЭЭ Ч** `child_process`, `exec()`, `spawn()` ашиглахгүй | Командын хувьд зөвхөн `ctx.terminal.send()` ашигла |
| **ХЭЗЭЭ Ч** гадаад URL-уудыг авахгүй | Онцгой тохиолдол: `wiasoom.com` API эцсийн цэгүүд |
| **ХЭЗЭЭ Ч** `process.env`-ийг хандахгүй | Орчны хувьсагчид нууц агуулж болно |
| **ХЭЗЭЭ Ч** `require('fs')`-ийг шууд ашиглахгүй | `ctx.settings`-ийг хадгалахын тулд, `ctx.sftp`-ийг файлын дамжуулалтад ашигла |
| **ХЭЗЭЭ Ч** npm гадаад багцуудыг ашиглахгүй | Зөвхөн цэвэр JavaScript — node_modules байхгүй |
| **ЗААВАЛ** бүх алсын командын хувьд `ctx.terminal.send()` ашиглах | Энэ нь аюулгүй SSH сувгаар явагдана |
| **ЗААВАЛ** `deactivate()`-т цэвэрлэгээ хийх | Слушчдыг устгах, интервалуудыг цэвэрлэх |

### Алдаа боловсруулах

Аюултай үйлдлүүдийг үргэлж try/catch-д оруулж байгаарай:
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
### deactivate() дотор цэвэрлэгээ хийх

Хэрэв таны плагин интервал, слушчид, эсвэл захиалгуудыг үүсгэдэг бо�� — тэдгээрийг цэвэрлэ:
§§§CHUNK_SEPARATOR§§§
### i18n Дэмжлэг

WIA SOOM 254 хэл дэмждэг. Таны плагины шошгыг орчуулж болохоор хийхийн тулд энгийн аргыг ашигла:
§§§CHUNK_SEPARATOR§§§
---

## 6-р хэсэг: Жишээ

### Жишээ 1: Серверийн Диск Шалгагч

Алсын сервер дээр `df -h` гүйцэтгэж, статусын мөрөнд ашигласан/бэлэн зайг харуулна.
§§§CHUNK_SEPARATOR§§§
---

### Жишээ 2: TODO Менежер

TODO жагсаалтыг менежердэгч плагин, тогтмол хадгалахын тулд тохиргоог ашиглаж, харуулахын тулд веб үзэгчийг ашигла.

> **Загварын загвар:** Веб үзэгчид плагин API-уудыг шууд дуудж чадахгүй тул энэ плагин "snapshot" хандлагаар ажилладаг — энэ нь тохиргооноос TODO-уудыг уншиж, тэдгээрийг уншигдах боломжтой HTML хэлбэрээр гаргаж, зүйлсийг нэмэхийн тулд талбарын үйлдлүүдийг хангадаг. Веб үзэгч нь **харуулах** давхарга бөгөөд интерактив хэлбэр биш юм.
§§§CHUNK_SEPARATOR§§§
---

### Жишээ 3: Алдаа Хянагч

Терминалын гаралтыг хянаж, тодорхой загварууд илрэх үед мэдэгдэл илгээдэг.
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
---

## Нэмэлт: Ангилал & Символууд

### Плагин Ангилал (29)

Эдгээрийг `package.json`-д `keywords`-д эсвэл регистрт илгээхдээ ашиглана уу:

| Ангилал | Тодорхойлолт |
|---------|---------------|
| `server` | Ерөнхий серверийн удирдлага |
| `devtools` | Хөгжилд зориулсан хэрэгслүүд |
| `calculator` | Тооцоолуур болон хөрвүүлэгчид |
| `simulator` | Симуляторууд |
| `game` | Терминалын тоглоомууд |
| `business` | Бизнесийн хэрэгслүүд |
| `security` | Аюулгүй байдал болон аудит |
| `web` | Веб серверийн удирдлага |
| `education` | Боловсролын хэрэгслүүд |
| `health` | Эрүүл мэндтэй холбоотой хэрэгслүүд |
| `islamic` | Исламын хэрэгслүүд (молебен цаг, гэх мэт) |
| `science` | Шинжлэх ухааны хэрэгслүүд |
| `quantum` | Квантын компьютерийн хэрэгслүүд |
| `ai` | AI-д суурилсан хэрэгслүүд |
| `biotech` | Биотехноло��ийн хэрэгслүүд |
| `space` | Сансар болон астрономийн хэрэгслүүд |
| `network` | Сүлжээний хэрэгслүүд |
| `database` | Мэдээллийн сангийн удирдлага |
| `monitoring` | Серверийн хяналт |
| `devops` | DevOps болон CI/CD |
| `utility` | Ерөнхий хэрэгслүүд |
| `design` | Дизайн хэрэгслүүд |
| `ecommerce` | E-commerce хэрэгслүүд |
| `automation` | Автоматжуулалтын хэрэгслүүд |
| `kpop` | K-pop-тэй холбоотой хэрэгслүүд |
| `accessibility` | Хандалтын хэрэгслүүд |
| `analytics` | Аналитик болон тайлагнал |
| `wia` | WIA экосистемийн хэрэгслүүд |
| `all` | Бүх ангилалд ордог |

### Зөвлөсөн Символууд (Lucide)

| Символын Нэр | Ашиглах |
|---------------|---------|
| `server` | Серверийн удирдлага |
| `shield` | Аюулгүй байдал |
| `database` | Мэдээллийн сан |
| `activity` | Хяналт |
| `terminal` | Терминалын хэрэгслүүд |
| `code` | Хөгжил |
| `hard-drive` | Диск/санах ой |
| `network` | Сүлжээ |
| `lock` | Нэвтрэх/шифрлэх |
| `eye` | Харах/хянах |
| `check-square` | Даалгавар/TODO |
| `layout-dashboard` | Дашбоард |
| `settings` | Тохиргоо |
| `zap` | Автоматжуулалт |
| `globe` | Веб/олон улсын |

Бүх 1,500+ символыг үзээрэй: [lucide.dev/icons](https://lucide.dev/icons)

---

## Тусламж хэрэгтэй юу?

- **GitHub Асуудлууд:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Плагин Асуудлууд:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Жишээ Плагинууд:** [Website](https://wiasoom.com)
- **Вебсайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Гайхамшигтай зүйл бүтээ. Дэлхийтэй хуваалц.</em></p>
<p align="center"><em>— WIA SOOM баг</em></p>
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
