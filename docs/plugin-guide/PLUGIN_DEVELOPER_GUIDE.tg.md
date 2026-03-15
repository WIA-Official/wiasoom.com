<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Роҳнамои рушди плагини WIA SOOM</h1>
<p align="center"><strong>Плагини худро дар 5 дақиқа созед.</strong></p>
<p align="center">Воситаҳои сервери пурқувват, панелҳо ва автоматизатсияҳоро дар WIA SOOM созед.</p>

---

## Мазмуни Феҳрист

- [Қисми 1: Шурӯъ кардани зуд — Плагини аввалини шумо дар 5 дақиқа](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Қисми 2: Маъруфи API контексти плагин](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Қисми 3: Сохтани UI фармоишӣ бо Webviews](#part-3-building-custom-ui-with-webviews)
- [Қисми 4: Нашри плагини шумо](#part-4-publishing-your-plugin)
- [Қисми 5: Практикаҳои беҳтарин](#part-5-best-practices)
- [Қисми 6: Мисолҳои воқеӣ](#part-6-real-world-examples)
- [Иловагӣ: Категорияҳо ва иконҳо](#appendix-categories--icons)

---

## Қисми 1: Шурӯъ кардани зуд — Плагини аввалини шумо дар 5 дақиқа

### Чизе, ки шумо сохта метавонед

Плагини "Hello World" ки тугмаи дар паҳлӯро илова мекунад. Вақте ки клик мекунед, хабаре нишон медиҳад.

### Қадами 1: Фолдери плагинро созед
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Қадами 2: package.json созед
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
**Майдони зарурӣ:** `name`, `version`, `description`, `author`, `main`

### Қадами 3: index.js созед
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
### Қадами 4: WIA SOOM-ро дубора оғоз кунед

Барнома (ё плагинро дар Танзимот → Плагинҳо хомӯш/фаъол кунед) дубора оғоз кунед.

Шумо бояд тугмаи **"Hello World"** -ро дар паҳлӯ бинед. Клик кунед — шумо хабаре бо муваффақият мебинед!

### ��ӣ гуна кор мекунад
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

## Қисми 2: Маъруфи API контексти плагин

Вақте ки функсияи `activate(context)` шумо даъват мешавад, `context` (ё `ctx`) ин API-ҳоро таъмин мекунад:
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

### `ctx.terminal` — Вақте ки командҳоро дар серверҳои дурдаст иҷро мекунад

#### `terminal.send(sessionId, data)`

Команд (ё ҳар гуна маълумот) ба як сессияи терминали фаъол фиристед.

| Параметр | Намуд | Тавсиф |
|----------|-------|--------|
| `sessionId` | `string` | Сессияи терминал, ки ба он фиристода мешавад |
| `data` | `string` | Команд ё маълумоте, ки фиристода мешавад |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Ба тамоми хуруҷи сессияи терминал обуна шавед. Функсияи **бозгашти обуна** -ро бармегардонад.

| Параметр | Намуд | Тавсиф |
|----------|-------|--------|
| `sessionId` | `string` | Сессияи терминал, ки бояд назорат шавад |
| `callback` | `(data: string) => void` | Бо ҳар пори хуруҷ даъват мешавад |
| **Бармегардонад** | `() => void` | Инро барои қатъ кардани гӯш кардан даъват кунед |
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
**Маҳсулот:** Ҳамеша функсияи бозгаш��и обунаро захира кунед ва онро дар `deactivate()` даъват кунед, то аз хати хотира пешгирӣ кунед.

---

### `ctx.sftp` — интиқоли файл

> **Вазъият: Ба зудӣ меояд** — API SFTP муайян шудааст, аммо ҳанӯз ба муҳаррики SFTP барнома пайваст нашудааст. `list()` ҳоло массиви холи бармегардонад, ва `upload()`/`download()` амал намекунанд. Ин дар нашри оянда пурра амалӣ хоҳад шуд. Ҳоло, `ctx.terminal.send()`-ро бо командҳои `scp` ё `rsync` ҳамчун роҳи ҳалли муваққатӣ истифода баред.

#### `sftp.list(sessionId, path)`

Файлҳоро дар директорияи дурдаст рӯйхат кунед.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Файлро аз мошини маҳаллӣ ба сервери дурдаст бор кунед.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Файлро аз сервери дурдаст ба мошини маҳаллӣ бор кунед.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Роҳи ҳалли (то API SFTP фаъол шавад):**
��§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Интерфейси корбар

#### `ui.addSidebarButton(options)`

Тугмаро ба паҳлӯи WIA SOOM илова кунед.

| Опсия | Намуд | Зарурӣ | Тавсиф |
|-------|-------|--------|--------|
| `id` | `string` | Не | Идентитатори уникалӣ (ба номи плагин пешфарз) |
| `icon` | `string` | Ҳа | Номи иконаи Lucide (масалан, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ҳа | Тексти тугма, ки дар паҳлӯ нишон дода мешавад |
| `onClick` | `() => void` | Ҳа | Функсия, ки вақте тугма клик мешавад, даъват мешавад |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Маъруфи икон:** Ҳамаи иконҳои дастрасро дар [lucide.dev/icons](https://lucide.dev/icons) бубинед.

> **Эзоҳи мувофиқ:** Баъзе плагинҳои кӯҳна аргументҳои позиционалиро истифода мебаранд, ба монанди `addSidebarButton(id, icon, label, onClick)`. API расмӣ объекте бо **опсияҳоро** ба таври зикршударо истифода мебарад. Ҳамеша барои плагинҳои нав услуби объектиро истифода баред.

#### `ui.openWebview(options)`

Варзишгоҳи поп-ап бо мундариҷаи HTML фармоишӣ боз кунед. Ин тавре, ки шумо UI-ҳои бой месозед.

| Опсия | Намуд | Тавсиф |
|-------|-------|--------|
| `title` | `string` | Номи тирезе |
| `html` | `string` | Мундариҷаи пурраи HTML барои намоиш |
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
> Бинед [Қисми 3](#part-3-building-custom-ui-with-webviews) барои намунаҳои пешрафтаи веб-намойиш.

#### `ui.showNotification(type, message)`

Нишон додани огоҳии toast.

| Параметр | Намуд | Тавсиф |
|----------|-------|--------|
| `type` | `'success' \| 'error' \| 'info'` | Стилъи огоҳӣ |
| `message` | `string` | Текст барои нишон додан |
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
#### `ui.addStatusBarItem(id, text)`

Илова кардани элемент текстии доимӣ ба статус бари поён.

| Параметр | Намуд | Тавсиф |
|----------|-------|--------|
| `id` | `string` | Идентфикатори уникалии ин статус элемент |
| `text` | `string` | Текст барои намоиш |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Захираи доимӣ

Танзимотҳои плагин доимӣ дар `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` захира мешаванд.

#### `settings.get(key)`

Қимати захирашударо хонед.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Агар калид мавҷуд набошад, `undefined` бармегардонад.

#### `settings.set(key, value)`

Қиматро захира кунед. Роҳнамоии строкаҳо, рақамҳо, boolean, массивҳо ва объектҳоро дастгирӣ мекунад.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Мисол: Ёддошт кардани афзалиятҳои корбар**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — Интегратсияи AI

> **Вазъият: Ба зудӣ меояд** — API-и AI муайян шудааст, аммо ҳанӯз ба Soomy пайваст нашудааст. Ҳоло `{ response: 'AI not yet connected' }` бармегардонад. Интегратсияи пурраи AI барои нашри оянда нақша гирифта шудааст.

#### `ai.chat(messages, options?)`

Паёмҳоро ба ёрдамчии AI (Soomy) фиристед.
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

## Қисми 3: Сохтани UI-и фармоишӣ бо Веб-намойишҳо

API-и `openWebview()` ба шумо имкон медиҳад, ки интерфейсҳои панели назоратро бо HTML, CSS ва JavaScript созед — ҳама дар дохили як тирезаи поп-ап.

> **Маҳдудияти муҳим:** Веб-намойишҳо **фақат барои намоиш** мебошанд. Онҳо наметавонанд ба API-и плагинҳо ( `ctx.settings`, `ctx.terminal`, ва ғайра) баргарданд. Барои ҳамаи амалҳои корбар, тугмаҳои паҳлӯиро истифода баред ва `openWebview()`-ро барои намоиши ҳолати кунунӣ истифода баред. Агар ба хусусиятҳои интерактивӣ ниёз дошта бошед, онҳоро аз тугмаҳои паҳлӯӣ фаъол созед ва веб-намойишро дубора кушоед, то намоишро навсозӣ кунед.

### Нишон: Фармони терминал → Таҳлили натиҷа → Намоиш дар HTML

Ин маъмултарин намунаи плагин мебошад. Шумо як фармонро иҷро мекунед, натиҷаро таҳлил мекунед ва онро визуалӣ намоиш медиҳед.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Нишон: Панели интерактивӣ бо навсозии автоматӣ
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
### Нишон: Намоиши танзимот дар веб-намойиш

> **Эзоҳ:** Веб-намойишҳо фақат барои намоиш мебошанд — онҳо наметавонанд ба API-и плагинҳо баргарданд. `ctx.settings`-ро дар корбарии тугмаи паҳлӯӣ барои тағйир додани танзимот истифода баред ва `openWebview()`-ро барои намоиши ҳолати кунунӣ истифода баред.
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

## Қисми 4: Нашри Плагини Шумо

### Қадами 1: Локалӣ санҷиш кунед

1. Плагини худро ба `~/.wia-soom/plugins/{your-plugin}/` нусхабардорӣ кунед
2. WIA SOOM-ро дубора оғоз кунед
3. Тасдиқ кунед, ки кор мекунад: тугмаи паҳлӯӣ пайдо мешавад, хусусиятҳо дуруст кор мекунанд
4. Вариантҳои марзиро санҷед: агар терминал пайваст набошад, чӣ мешавад?

### Қадами 2: Барои пешниҳод омода шавед

Фолдери плагини шумо бояд дорои:
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
**Маҳсулоти зарурии `package.json`:**

| Маҳсулот | Тавсиф | Намуна |
|-------|-------------|---------|
| `name` | Идентфикатори уникалии kebab-case | `"my-awesome-plugin"` |
| `version` | Версияи семантикӣ | `"1.0.0"` |
| `description` | Тавсифи як хат | `"Monitors nginx access logs in real-time"` |
| `author` | Номи шумо | `"John Doe"` |
| `main` | Нуқтаи воридшавӣ | `"index.js"` |

**Маҳсулотҳои ихтиёрӣ:**

| Маҳсулот | Тавсиф |
|-------|-------------|
| `license` | Намуди иҷозатнома (MIT тавсия мешавад) |
| `keywords` | Массиви тегҳои ҷустуҷӯ |
| `soom.minVersion` | Версияи минималии WIA SOOM, ки лозим аст |

### Гам 3: Ба реестри Плагин пешниҳод кунед

1. ****Package** your plugin as a ZIP file
2. **Илова** кунед плагини худро ба `plugins/{your-plugin-name}/`
3. **Пешниҳод** кунед Pull Request

### Гам 4: Баррасӣ ва тасдиқ

Мо ҳар плагинро барои:

- **Амният** — ҳеҷ API-и хатарнок (бубинед [Қоидаҳои амният](#security-rules))
- **Сифат** — оё он кор мекунад? Оё код тозаст?
- **Фоида** — оё он мушкили воқеиро ҳал мекунад?

Пас аз тасдиқ:
1. Плагини шумо ба `registry.json` илова мешавад
2. Пакети ZIP дар `dist/` сохта мешавад
3. Плагини шумо дар **Plugin Store** барои ҳамаи корбарони WIA SOOM намоён мешавад!

---

## Қисми 5: Тақсимоти беҳтарин

### Қоидаҳои амният

Ин қоидаҳо **мавҷуданд**. Плагинҳое, ки онҳоро вайрон мекунанд, рад карда мешаванд.

| Қоида | Чаро |
|------|-----|
| **ҲЕЧГАҲ** `eval()` ё `new Function()`-ро истифода набаред | Хатари ворид кардани код |
| **ҲЕЧГАҲ** `child_process`, `exec()`, `spawn()`-ро истифода набаред | Танҳо `ctx.terminal.send()`-ро барои фармонҳо истифода баред |
| **ҲЕЧГАҲ** URL-ҳои хориҷиро гирифт накунед | Исключение: нуқтаҳои API `wiasoom.com` |
| **ҲЕЧГАҲ** `process.env`-ро дастрас накунед | Вариантҳои муҳити корӣ метавонанд асрор дошта бошанд |
| **ҲЕЧГАҲ** `require('fs')`-ро мустақиман истифода набаред | Барои захира `ctx.settings`-ро истифода баред, барои интиқоли файл `ctx.sftp`-ро истифода баред |
| **ҲЕЧГАҲ** пакетҳои хориҷии npm-ро истифода набаред | Танҳо JavaScript холис — ҳеҷ `node_modules` |
| **МЕҲТАР** барои ҳамаи фармонҳои дурдаст `ctx.terminal.send()`-ро истифода баред | Ин тавассути канали амн SSH мегузарад |
| **МЕҲТАР** дар `deactivate()` тозагӣ кунед | Гӯшкунандагонро нест кунед, интервалҳоро тоза кунед |

### Мудирияти хатогиҳо

Ҳамеша амалиёти хатарнокро дар try/catch печонед:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Тозагӣ дар deactivate()

Агар плагини шумо интервалҳо, гӯшкунандагон ё обунаҳо эҷод кунад — онҳоро тоза кунед:
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
### Дастгирии i18n

WIA SOOM 254 забонро дастгирӣ мекунад. Барои он ки нишони плагини шумо тарҷумашаванда бошад, усули оддиро истифода баред:
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

## Қисми 6: Мисолҳои воқеӣ

### Мисол 1: Санҷандаи диски сервер

`df -h`-ро дар сервери дурдаст иҷро мекунад ва фазои истифодашударо/фазои дастрасро дар панели статус нишон медиҳад.
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

### Мисол 2: Менежери TODO

Плагине, ки рӯйхати TODO-ро бо истифода аз танзимот барои захираи доимӣ ва вебвью барои намоиш идора мекунад.

> **Намунаи тарҳ:** Азбаски вебвьюҳо наметавонанд мустақиман API-ҳои плагинро занг зананд, ин плагин усули "снпшот" -ро истифода мебарад — он TODO-ҳоро аз танзимот мехонад, онҳоро ҳамчун HTML-и хонданӣ намоиш медиҳад ва амалҳои асоси паҳлӯиро барои илова кардани элементҳо пешниҳод мекунад. Вебвью як қабати **намойиш** аст, на шакли интерактивӣ.
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

### Мисол 3: Нозири хатогиҳо

Харитаи хуруҷи терминалро назорат мекунад ва вақте ки намунаҳои махсус муайян мешаванд, огоҳӣ мефиристад.
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

## Зам annex: Категорияҳо ва Иконҳо

### Категорияҳои Плагин (29)

Инҳоро дар `package.json` `keywords` ё ҳангоми фиристодан ба реестр истифода баред:

| Категория | Тавсиф |
|----------|-------------|
| `server` | Идоракунии умумии сервер |
| `devtools` | Асбобҳои рушд |
| `calculator` | Ҳисобкунакҳо ва конвертерҳо |
| `simulator` | Симуляторҳо |
| `game` | Бозии терминал |
| `business` | Асбобҳои тиҷорат |
| `security` | Амният ва аудити |
| `web` | Идоракунии сервери веб |
| `education` | Асбобҳои таълимӣ |
| `health` | Асбобҳои марбут ба саломатӣ |
| `islamic` | Асбобҳои исломӣ (вақтҳои намоз ва ғайра) |
| `science` | Асбобҳои илмӣ |
| `quantum` | Асбобҳои компютерии квантӣ |
| `ai` | Асбобҳои бо AI дастгиришаванда |
| `biotech` | Асбобҳои биотехнологӣ |
| `space` | Асбобҳои фосила ва астрономия |
| `network` | Асбобҳои шабака |
| `database` | Идоракунии базаи маълумот |
| `monitoring` | Назорати сервер |
| `devops` | DevOps ва CI/CD |
| `utility` | Асбобҳои умумӣ |
| `design` | Асбобҳои тарҳрезӣ |
| `ecommerce` | Асбобҳои электронӣ |
| `automation` | Асбобҳои автоматизатсия |
| `kpop` | Асбобҳои марбут ба K-pop |
| `accessibility` | Асбобҳои дастрасӣ |
| `analytics` | Анализ ва гузоришдиҳӣ |
| `wia` | Асбобҳои экосистемаи WIA |
| `all` | Дар ҳамаи категорияҳо пайдо мешавад |

### Иконҳои тавсияшаванда (Lucide)

| Номи Икон | Барои истифода |
|-----------|---------|
| `server` | Идоракунии сервер |
| `shield` | Амният |
| `database` | Базаи маълумот |
| `activity` | Назорат |
| `terminal` | Асбобҳои терминал |
| `code` | Рушд |
| `hard-drive` | Диск/хотира |
| `network` | Шабака |
| `lock` | Тасдиқ/шифр |
| `eye` | Нозирӣ/назорат |
| `check-square` | Вазифаҳо/TODO |
| `layout-dashboard` | Дашбордҳо |
| `settings` | Танзимот |
| `zap` | Автоматизатсия |
| `globe` | Веб/байналмилалӣ |

Ба ҳамаи 1,500+ иконҳо назар кунед: [lucide.dev/icons](https://lucide.dev/icons)

---

## Кӯмак лозим аст?

- **Масъалаҳои GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Масъалаҳои Плагин:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Плагинҳои намунавӣ:** [Website](https://wiasoom.com)
- **Вебсайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Чизе аҷиб созед. Онро бо ҷаҳон мубодила кунед.</em></p>
<p align="center"><em>— Гурӯҳи WIA SOOM</em></p>
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
