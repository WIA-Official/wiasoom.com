<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Плагин Долбоорунун Жол көрсөтмөсү</h1>
<p align="center"><strong>5 мүнөттө өз плагиниңизди жасаңыз.</strong></p>
<p align="center">Күчтүү сервер куралдарын, панелдерди жана автоматташтырууларды түзүңүз — WIA SOOM ичинде.</p>

---

## Мазмундун Тизмеси

- [1-бөлүк: Жылдам баштоо — Сиздин биринчи плагиниңиз 5 мүнөттө](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [2-бөлүк: Плагин Контекст API Справкасы](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [3-бөлүк: Веб-көрсөтүүлөр менен Кастом UI куруу](#part-3-building-custom-ui-with-webviews)
- [4-бөлүк: Плагиниңизди жарыялоо](#part-4-publishing-your-plugin)
- [5-бөлүк: Эң жакшы тажрыйбалар](#part-5-best-practices)
- [6-бөлүк: Чыныгы дүйнө мисалдары](#part-6-real-world-examples)
- [Кошумча: Категориялар & Иконкалар](#appendix-categories--icons)

---

## 1-бөлүк: Жылдам баштоо — Сиздин биринчи плагиниңиз 5 мүнөттө

### Эмне курулат

Сайдбарга баскыч кошуучу "Hello World" плагини. Басылганда, ал билдирүү көрсөтөт.

### 1-кадам: Плагин папкасын түзүңүз
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 2-кадам: package.json түзүңүз
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
**Талап кылынган талаалар:** `name`, `version`, `description`, `author`, `main`

### 3-кадам: index.js түзүңүз
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
### 4-кадам: WIA SOOMду кайра жүктөңүз

Программаны кайра жүктөңүз (же Параметрлер → Плагиндерде плагинди өчүрүп/кайра күйгүзүңүз).

Сайдбарда **"Hello World"** баскычын көрүшүңүз керек. Басып коюңуз — сиз ийгиликтүү билдирүүнү көрөсүз!

### Ал кандай иштейт
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

## 2-бөлүк: Плагин Контекст API Справкасы

`activate(context)` функцияңыз чакырылганда, `context` (же `ctx`) бул API'лерди сунуштайт:
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

### `ctx.terminal` — Удаленный серверлерде командаларды иштетүү

#### `terminal.send(sessionId, data)`

Активдүү терминал сессиясына команда (же каалаган маалымат) жөнөтүңүз.

| Параметр | Тип | Тасвир |
|----------|-----|--------|
| `sessionId` | `string` | Жөнөтүлүүчү терминал сессиясы |
| `data` | `string` | Жөнөтүлүүчү команда же маалымат |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Терминал сессиясынан бардык чыгымдарды жазылууга алыңыз. **жазылуудан чыгуу функциясын** кайтарат.

| Параметр | Тип | Тасвир |
|----------|-----|--------|
| `sessionId` | `string` | Көзөмөлдөнүүчү терминал сессиясы |
| `callback` | `(data: string) => void` | Ар бир чыгым бөлүгү менен чакырылат |
| **Кайтарат** | `() => void` | Угууну токтотуу үчүн бул функцияны чакырыңыз |
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
**Маанилүү:** Жазылуудан чыгуу функциясын дайыма сактап, `deactivate()` ичинде чакырыңыз, бул жадатма агып кетүүдөн сактайт.

---

### `ctx.sftp` — Файлдарды өткөрүү

> **Статус: Жакында келет** — SFTP API аныкталган, бирок азырынча колдонмонун SFTP механизмине туташтырылган эмес. `list()` учурда бош массивди кайтарат, ал эми `upload()`/`download()` функциялары иштебейт. Бул келечектеги чыгарылышта толук ишке ашырылат. Азырынча, `scp` же `rsync` командалары менен `ctx.terminal.send()` колдонуп, альтернативалык жолду колдонуңуз.

#### `sftp.list(sessionId, path)`

Удаленный каталогдогу файлдарды тизмелеңиз.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Локалдык машинеден удаленный серверге файл жүктөө.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Удаленный серверден локалдык машинага файл жүктөө.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Альтернативалык жол (SFTP API ишке киргенге чейин):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Колдонуучу интерфейси

#### `ui.addSidebarButton(options)`

WIA SOOM сайдбарына баскыч кошуңуз.

| Опция | Тип | Талап кылынат | Тасвир |
|-------|-----|---------------|--------|
| `id` | `string` | Жок | Уникалдуу ID (плагиндин атын алат) |
| `icon` | `string` | Ооба | Lucide иконка аталышы (мисалы, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ооба | Сайдбарда көрсөтүлгөн баскыч текст |
| `onClick` | `() => void` | Ооба | Баскыч басылганда чакырылган функция |
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
**Иконка справкасы:** Бардык жеткиликтүү иконкаларды [lucide.dev/icons](https://lucide.dev/icons) сайтынан караңыз.

> **Совместимость эскертүүсү:** Кээ бир эски плагиндер позициялык аргументтерди колдонушат, мисалы, `addSidebarButton(id, icon, label, onClick)`. Расмий API жогоруда документтелген **опция объектин** колдонот. Жаңы плагиндер үчүн дайыма объект стилин колдонуңуз.

#### `ui.openWebview(options)`

Кастом HTML мазмуну менен попап терезесин ачыңыз. Бул бай интерфейстерди куруунун жолу.

| Опция | Тип | Тасвир |
|-------|-----|--------|
| `title` | `string` | Терезенин аталышы |
| `html` | `string` | Чыгарылуучу толук HTML мазмуну |
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
> [3-бөлүмдү](#part-3-building-custom-ui-with-webviews) advanced webview үлгүлөрү үчүн караңыз.

#### `ui.showNotification(type, message)`

Toast билдирүүсүн көрсөтүңүз.

| Параметр | Тип | Сипаттама |
|----------|-----|-----------|
| `type` | `'success' \| 'error' \| 'info'` | Билдирүү стили |
| `message` | `string` | Көрсөтүлүүчү текст |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Төмөнкү статус тилкесине туруктуу текст элементин кошуңуз.

| Параметр | Тип | Сипаттама |
|----------|-----|-----------|
| `id` | `string` | Бул статус элемент үчүн уникалдуу ID |
| `text` | `string` | Көрсөтүлүүчү текст |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Туруктуу сактоо

Плагиндин жөндөөлөрү туруктуу түрдө `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` файлында сакталат.

#### `settings.get(key)`

Сакталган маанини окуу.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Эгерде ачкыч жок болсо, `undefined` кайтарат.

#### `settings.set(key, value)`

Маанини сактоо. Жөнөкөй тексттерди, сандарды, логикалык маанилерди, массивдерди жана объектилерди колдойт.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Мисал: Колдонуучунун тандоолорун эстеп калуу**
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

### `ctx.ai` — AI интеграциясы

> **Статус: Жакында келет** — AI API аныкталган, бирок Soomy'ге туташкан эмес. Учурда `{ response: 'AI not yet connected' }` кайтарат. Толук AI интеграциясы келечектеги чыгарылышта пландалууда.

#### `ai.chat(messages, options?)`

AI жардамчысына (Soomy) билдирүүлөрдү жибериңиз.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## 3-бөлүм: Webviews менен Custom UI куруу

`openWebview()` API'сы HTML, CSS жана JavaScript менен панелдик UI'ларды курууга мүмкүндүк берет — баары поп-ап терезеде.

> **Маанилүү чектөө:** Webviews **тек гана көрсөтүү үчүн**. Алар плагин API'ларына (`ctx.settings`, `ctx.terminal` ж.б.) кайта чалуу жасай албайт. Колдонуучунун бардык аракеттери үчүн бүйүр кнопкаларын колдонуп, учурдагы абалды көрсөтүү үчүн `openWebview()` колдонуңуз. Эгер интерактивдүү функцияларга муктаж болсоңуз, аларды бүйүр кнопкаларынан жандандырып, көрсөтүүнү жаңыртуу үчүн webview'ди кайра ачыңыз.

### Үлгү: Терминал буйругу → Чыгымды талдоо → HTML'де көрсөтүү

Бул эң кеңири таралган плагин үлгүсү. Сиз буйрук иштетесиз, натыйжаны талдайсыз жана визуалдык түрдө көрсөтөсүз.
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
### Үлгү: Авто-жаңыртуу менен интерактивдүү панель
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
### Үлгү: Webview'де жөндөөлөрдү көрсөтүү

> **Эскертүү:** Webviews тек гана көрсөтүү үчүн — алар плагин API'ларына кайта чалуу жасай албайт. Жөндөөлөрдү өзгөртүү үчүн бүйүр кнопкаңыздын иштетүүчүлөрүндө `ctx.settings` колдонуңуз, жана учурдагы абалды көрсөтүү үчүн `openWebview()` колдонуңуз.
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

## 4-бөлүм: Плагиниңизди жарыялоо

### 1-кадам: Жерде тестирлөө

1. Плагиниңизди `~/.wia-soom/plugins/{your-plugin}/` папкасына көчүрүңүз.
2. WIA SOOM'ду кайра жүктөңүз.
3. Иштеп жатканын текшериңиз: бүйүр кнопкасы пайда болот, функциялар туура иштейт.
4. Чектик учурларды текшериңиз: терминал туташпаса эмне болот?

### 2-кадам: Жөнөтүүгө даярдануу

Плагиниңиздин папкасы төмөнкүлөрдү камтышы керек:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Талап кылынган `package.json` талаалары:**

| Талаа | Тасвир | Мисал |
|-------|-------------|---------|
| `name` | Уникалдуу kebab-case ID | `"my-awesome-plugin"` |
| `version` | Семантикалык версия | `"1.0.0"` |
| `description` | Бир жолку тасвир | `"Monitors nginx access logs in real-time"` |
| `author` | Сиздин атыңыз | `"John Doe"` |
| `main` | Кириш чекити | `"index.js"` |

**Мүмкүнчүлүктөр:**

| Талаа | Тасвир |
|-------|-------------|
| `license` | Лицензия түрү (MIT сунушталат) |
| `keywords` | Издөө тегдеринин массиви |
| `soom.minVersion` | Талап кылынган минималдуу WIA SOOM версиясы |

### 3-кадам: Плагин Регистрине жиберүү

1. ****Package** your plugin as a ZIP file
2. **Кошуңуз** плагиниңизди `plugins/{your-plugin-name}/`
3. **Жибериңиз** Pull Request

### 4-кадам: Кайра карап чыгуу жана бекитүү

Биз ар бир плагинди төмөнкү үчүн карап чыгабыз:

- **Коопсуздук** — кооптуу API'лер жок (көрүңүз [Коопсуздук Эрежелери](#security-rules))
- **Сапат** — ал иштейби? Код тазабы?
- **Пайдалуулук** — ал чыныгы маселени чечеби?

Бекитилгенден кийин:
1. Сиздин плагиниңиз `registry.json` файлына кошулат
2. `dist/` папкасында ZIP топтому түзүлөт
3. Сиздин плагиниңиз **Plugin Store**'до бардык WIA SOOM колдонуучулары үчүн көрүнөт!

---

## 5-бөлүк: Эң жакшы практикалар

### Коопсуздук Эрежелери

Бул эрежелер **мыйзамдуу**. Алардын бузулушу плагиндерди четтетет.

| Эреже | Негизи |
|------|-----|
| **ЭЧ КАЧАН** `eval()` же `new Function()` колдонбоо | Кодду киргизүү тобокелдиги |
| **ЭЧ КАЧАН** `child_process`, `exec()`, `spawn()` колдонбоо | Командалар үчүн гана `ctx.terminal.send()` колдонуу |
| **ЭЧ КАЧАН** тышкы URL'дерди алуу | Исключение: `wiasoom.com` API пункттары |
| **ЭЧ КАЧАН** `process.env` жетүү | Чөйрө өзгөрмөлөрү сырларды камтышы мүмкүн |
| **ЭЧ КАЧАН** `require('fs')` түздөн-түз колдонбоо | Сактоо үчүн `ctx.settings`, файл өткөрүү үчүн `ctx.sftp` колдонуу |
| **ЭЧ КАЧАН** npm тышкы пакеттерин колдонбоо | Таза JavaScript гана — node_modules жок |
| **МУНУН** бардык алыстан командалар үчүн `ctx.terminal.send()` колдонуу | Бул коопсуз SSH каналы аркылуу өтөт |
| **МУНУН** `deactivate()` ичинде тазалоо | Угуучуларды алып салуу, интервалдарды тазалоо |

### Ката менен иштөө

Ар дайым тобокелдүү операцияларды try/catch блокторуна орнотуу:
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
### Deactivate() ичинде тазалоо

Эгер плагиниңиз интервалдарды, угуучуларды же жазылууларды түзсө — аларды тазалаңыз:
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
### i18n Колдоо

WIA SOOM 254 тилди колдойт. Плагиниңиздин этикеткасын которууга мүмкүнчүлүк берүү үчүн, жөнөкөй ыкманы колдонуңуз:
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

## 6-бөлүк: Чыныгы дүйнөдөгү мисалдар

### Мисал 1: Сервер Диск Текшерүү

Алыскы серверде `df -h` командасын иштетип, статус тилкесинде колдонулган/бош орунду көрсөтөт.
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

### Мисал 2: TODO Менеджери

TODO тизмесин башкаруучу плагин, туруктуу сактоо үчүн параметрлерди жана көрсөтүү үчүн веб-көрүүчү колдонуп.

> **Дизайн үлгүсү:** Веб-көрүүчүлөр плагин API'лерин түздөн-түз чакыра албайт, бул плагин "сүрөт" ыкмасын колдонот — ал TODO'лорду параметрлерден окуйт, аларды окуу үчүн HTML катары көрсөтөт жана элементтерди кошуу үчүн бүйүр тараптагы аракеттерди сунуштайт. Веб-көрүүчү **көрсөтүү** катмары, интерактивдүү форма эмес.
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

### Мисал 3: Ката Көзөмөлдөөчү

Терминалдын чыгымын көзөмөлдөп, белгилүү үлгүлөр аныкталганда билдирүү жиберет.
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

## Кошумча: Категориялар & Иконкалар

### Плагин Категориялары (29)

Буларды `package.json` `keywords` ичинде же реестрге тапшырганда колдонуңуз:

| Категория | Сипаттама |
|-----------|-----------|
| `server` | Жалпы серверди башкаруу |
| `devtools` | Өнүктүрүү инструменттери |
| `calculator` | Эсептегичтер жана конвертерлер |
| `simulator` | Симуляторлор |
| `game` | Терминал оюндар |
| `business` | Бизнес инструменттери |
| `security` | Коопсуздук жана аудит |
| `web` | Веб серверди башкаруу |
| `education` | Билим берүү инструменттери |
| `health` | Саламаттыкка байланыштуу инструменттери |
| `islamic` | Ислам инструменттери (намаз убактысы ж.б.) |
| `science` | Илимий инструменттери |
| `quantum` | Кванттык эсептөө инструменттери |
| `ai` | ИИ күчөтүлгөн инструменттери |
| `biotech` | Биотехн��логия инструменттери |
| `space` | Космос жана астрономия инструменттери |
| `network` | Тармак инструменттери |
| `database` | Маалымат базасын башкаруу |
| `monitoring` | Серверди байкоо |
| `devops` | DevOps жана CI/CD |
| `utility` | Жалпы пайдалуу инструменттер |
| `design` | Дизайн инструменттери |
| `ecommerce` | Электрондук соода инструменттери |
| `automation` | Автоматташтыруу инструменттери |
| `kpop` | K-pop менен байланышкан инструменттери |
| `accessibility` | Жеткиликтүүлүк инструменттери |
| `analytics` | Аналитика жана отчет берүү |
| `wia` | WIA экосистемасы инструменттери |
| `all` | Бардык категорияларда көрүнөт |

### Тепкич Иконкалар (Lucide)

| Иконка Аты | Колдонуу үчүн |
|-------------|---------------|
| `server` | Серверди башкаруу |
| `shield` | Коопсуздук |
| `database` | Маалымат базасы |
| `activity` | Байкоо |
| `terminal` | Терминал инструменттери |
| `code` | Өнүктүрүү |
| `hard-drive` | Диск/сактоо |
| `network` | Тармак |
| `lock` | Аутентификация/шифрлөө |
| `eye` | Карап/байкоо |
| `check-square` | Тапшырмалар/TODO |
| `layout-dashboard` | Дашборддор |
| `settings` | Конфигурация |
| `zap` | Автоматташтыруу |
| `globe` | Веб/халыкаралык |

Бардык 1,500+ иконкаларды караңыз: [lucide.dev/icons](https://lucide.dev/icons)

---

## Жардам Керекпи?

- **GitHub Маселелери:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Плагин Маселелери:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Мисал Плагиндер:** [Website](https://wiasoom.com)
- **Вебсайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Керемет бир нерсе куруңуз. Булду дүйнө менен бөлүшүңүз.</em></p>
<p align="center"><em>— WIA SOOM Командасы</em></p>