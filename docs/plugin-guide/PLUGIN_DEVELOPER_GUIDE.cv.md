<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Плагин Разработчик Рководство</h1>
<p align="center"><strong>Создайте свой собственный плагин за 5 минут.</strong></p>
<p align="center">Создавайте мощные серверные инструменты, панели управления и автоматизации — прямо внутри WIA SOOM.</p>

---

## Содержание

- [Часть 1: Быстрый старт — Ваш первый плагин за 5 минут](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Часть 2: Справочник API контекста плагина](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Часть 3: Созда��ие пользовательского интерфейса с помощью Webviews](#part-3-building-custom-ui-with-webviews)
- [Часть 4: Публикация вашего плагина](#part-4-publishing-your-plugin)
- [Часть 5: Лучшие практики](#part-5-best-practices)
- [Часть 6: Примеры из реальной жизни](#part-6-real-world-examples)
- [Приложение: Категории и иконки](#appendix-categories--icons)

---

## Часть 1: Быстрый старт — Ваш первый плагин за 5 минут

### Что вы создадите

Плагин "Hello World", который добавляет кнопку на боковую панель. При нажатии она показывает уведомление.

### Шаг 1: Создайте папку плагина
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Шаг 2: Создайте package.json
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
**Обязательные поля:** `name`, `version`, `description`, `author`, `main`

### Шаг 3: Создайте index.js
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
### Шаг 4: Перезапустите WIA SOOM

Перезапустите приложение (или переключите плагин в выключенное/вклю��енное состояние в Настройки → Плагины).

Вы должны увидеть кнопку **"Hello World"** на боковой панели. Нажмите на нее — вы увидите уведомление об успехе!

### Как это работает
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

## Часть 2: Справочник API контекста плагина

Когда ваша функция `activate(context)` вызывается, `context` (или `ctx`) предоставляет эти API:
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

### `ctx.terminal` — Выполнение команд на удаленных серверах

#### `terminal.send(sessionId, data)`

Отправьте команду (или любые данные) в активную сессию терминала.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `sessionId` | `string` | Сессия терминала, в которую отправляется |
| `data` | `string` | Команда или данные для отправки |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Подпишитесь на все выводы из сессии терминала. Возвращает **функцию отписки**.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `sessionId` | `string` | Сессия терминала, которую нужно отслеживать |
| `callback` | `(data: string) => void` | Вызывается с каждым фрагментом вывода |
| **Возвращает** | `() => void` | Вызовите это, чтобы прекратить прослушивание |
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
**Важно:** Всегда сохраняйте функцию отписки и вызывайте ее в `deactivate()`, чтобы предотвратить утечки памяти.

---

### `ctx.sftp` — Передача файлов

> **Статус: Скоро** — API SFTP определен, но еще не подключен к SFTP-движку приложения. `list()` в настоящее время возвращает пустой массив, а `upload()`/`download()` не выполняются. Это будет полностью реализовано в будущем релизе. Пока используйте `ctx.terminal.send()` с командами `scp` или `rsync` в качестве обходного пути.

#### `sftp.list(sessionId, path)`

Список ��айлов в удаленной директории.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Загрузите файл с локального компьютера на удаленный сервер.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Скачайте файл с удаленного сервера на локальный компьютер.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Обходной путь (до активации API SFTP):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Пользовательский интерфейс

#### `ui.addSidebarButton(options)`

Добавьте кнопку на боковую панель WIA SOOM.

| Опция | Тип | Обязательная | Описание |
|-------|-----|--------------|----------|
| `id` | `string` | Нет | Уникальный ID (по умолчанию имя плагина) |
| `icon` | `string` | Да | Название иконки Lucide (например, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Да | Текст кнопки, отображаемый на боковой панели |
| `onClick` | `() => void` | Да | Функция, вызываемая при нажатии на кн��пку |
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
**Справочник иконок:** Просмотрите все доступные иконки на [lucide.dev/icons](https://lucide.dev/icons)

> **Примечание о совместимости:** Некоторые старые плагины используют позиционные аргументы, такие как `addSidebarButton(id, icon, label, onClick)`. Официальный API использует **объект опций**, как описано выше. Всегда используйте стиль объекта для новых плагинов.

#### `ui.openWebview(options)`

Откройте всплывающее окно с пользовательским HTML-контентом. Так вы создаете богатые пользовательские интерфейсы.

| Опция | Тип | Описание |
|-------|-----|----------|
| `title` | `string` | Заголовок окна |
| `html` | `string` | Полный HTML-контент для отображения |
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
> Кӗрӗ [Чӗн 3](#part-3-building-custom-ui-with-webviews) кӗнӗнчӗк веб-кӗнӗнчӗк паттернӗрӗ.

#### `ui.showNotification(type, message)`

Тост уведомление кӗрӗнчӗ.

| Параметр | Тип | Описание |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Стиль уведомления |
| `message` | `string` | Кӗрӗнчӗ текст |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Тӗнчӗк статус барӗнӗн кӗнӗнчӗ текст элементӗн кӗрӗнчӗ.

| Параметр | Тип | Описание |
|-----------|------|-------------|
| `id` | `string` | Уникальный ID для этого статус элемента |
| `text` | `string` | Кӗрӗнчӗ текст |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Периодик хранилище

Плагин хӗрӗнӗн кӗнӗнчӗ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` кӗрӗнчӗ.

#### `settings.get(key)`

Сакланӗн кӗнӗн��ӗн кӗрӗнчӗ.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Ключи юратӗн `undefined` кӗрӗнчӗ.

#### `settings.set(key, value)`

Кӗнӗнчӗн кӗрӗнчӗ. Строки, сан, булев, массив, и объект хӗрӗнӗн кӗрӗнчӗ.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Пример: Пользователь предпочтениелӗрӗн кӗрӗнчӗ**
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

### `ctx.ai` — ИИ интеграция

> **Статус: Кӗнӗнчӗн Кӗнӗнчӗ** — ИИ API определен, но Soomy-ӗн кӗрӗнчӗн. Хӗрӗнчӗ `{ response: 'AI not yet connected' }`. Полная ИИ интеграция кӗнӗнчӗн кӗнӗнчӗн.

#### `ai.chat(messages, options?)`

Сообщениян ИИ ассистентӗн (Soomy) кӗрӗнчӗ.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Чӗн 3: Веб-кӗнӗнчӗк белән Кастом UI

`openWebview()` API HTML, CSS, и JavaScript кӗнӗнчӗн кӗрӗнчӗк дашборд UI-не кӗрӗнчӗн — барӗнӗн поп-ап окнӗн.

> **Мӗнӗн чӗнӗ:** Веб-кӗнӗнчӗк **кӗрӗнчӗн**. Плагин API-не (`ctx.settings`, `ctx.terminal`, и т.д.) кӗрӗнчӗн юратӗн. Барӗнӗн пользователь хӗрӗнчӗн кӗрӗнчӗк кнопкӗн кӗрӗнчӗн, һәм `openWebview()` кӗрӗнчӗн хӗрӗнчӗн. Әгәр интерактив функциялӗрӗн кӗрӗнчӗн, кнопкӗн кӗрӗнчӗн һәм веб-кӗнӗнчӗк кӗрӗнчӗн яңартырӗн.

### Паттерн: Терминал Команда → Нәтижәне Парслау → HTML-да Кӗрӗнчӗ

Бу иң киң таралған плагин паттерны. Сез команда кӗрӗнчӗн, нәтиҗәне парслау, һәм визуаль кӗрӗнчӗн.
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
### Паттерн: Интерактив Дашборд белән Авто-Яңарту
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
### Паттерн: Веб-кӗнӗнчӗктә Хӗрӗнчӗн Кӗрӗнчӗ

> **Иҫкәрмә:** Веб-кӗнӗнчӗк кӗрӗнчӗн — плагин API-не кӗрӗнчӗн юратӗн. `ctx.settings` кӗрӗнчӗн кнопкӗн хӗрӗнчӗн кӗрӗнчӗн, һәм `openWebview()` кӗрӗнчӗн хӗрӗнчӗн кӗрӗнчӗн.
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

## Чӗн 4: Плагинӗн Кӗрӗнчӗн

### Адым 1: Локаль Тест

1. Плагинӗн кӗрӗнчӗн `~/.wia-soom/plugins/{your-plugin}/`
2. WIA SOOM-ны яңыртӗн
3. Эшл��ме тикшерӗн: кнопкӗн кӗрӗнчӗн, функциялӗр дөрөҫ эшли
4. Чик очраҡтарын тикшерӗн: терминал тоташтырылмаса ни була?

### Адым 2: Тапшыруға әзерләү

Сезнең плагин папкаһы кӗрӗнчӗн:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Таләп ителгән `package.json` кырлары:**

| Кыр | Тасвирлама | Мисал |
|-------|-------------|---------|
| `name` | Уникаль kebab-case ID | `"my-awesome-plugin"` |
| `version` | Семантик версия | `"1.0.0"` |
| `description` | Бер юллы тасвирлама | `"Monitors nginx access logs in real-time"` |
| `author` | Сезнең исем | `"John Doe"` |
| `main` | Керү ноктасы | `"index.js"` |

**Ихтимал кырлар:**

| Кыр | Тасвирлама |
|-------|-------------|
| `license` | Лицензия тибы (MIT тәкъдим ителә) |
| `keywords` | Эзләү теглары массивы |
| `soom.minVersion` | Кирәкле минималь WIA SOOM версиясе |

### 3-нче адым: Плагин реестрына җибәрү

1. ****Package** your plugin as a ZIP file
2. **Добавить** плагинны `plugins/{your-plugin-name}/`
3. **Submit** Pull Request

### 4-нче адым: К��зәтү һәм расланган

Без һәр плагинны түбәндәгеләр өчен күзәтәбез:

- **Куркынычсызлык** — куркыныч API-лар юк (кара [Куркынычсызлык кагыйдәләре](#security-rules))
- **Сыйфат** — ул эшлиме? Код чистамы?
- **Файдалылык** — ул чын проблема чишәме?

Расталганнан соң:
1. Сезнең плагин `registry.json` файлына өстәлә
2. `dist/` папкасында ZIP пакет ясала
3. Сезнең плагин **Plugin Store** да барлык WIA SOOM кулланучылары өчен күренә!

---

## 5-нче өлеш: Иң яхшы практикалар

### Куркынычсызлык кагыйдәләре

Бу кагыйдәләр **мәхкүм**. Аларны бозган плагиннар кире кагылачак.

| Кагыйдә | Нигә |
|------|-----|
| **КИЧЕКМӘГЕЗ** `eval()` яки `new Function()` кулланырга | Код инъекциясе куркынычы |
| **КИЧЕКМӘГЕЗ** `child_process`, `exec()`, `spawn()` кулланырга | Командалар өчен бары тик `ctx.terminal.send()` кулланыгы�� |
| **КИЧЕКМӘГЕЗ** тышкы URL-ларны алу | Искәрмә: `wiasoom.com` API нокталары |
| **КИЧЕКМӘГЕЗ** `process.env` га керергә | Мохит үзгәрүчәннәре серләрне үз эченә алырга мөмкин |
| **КИЧЕКМӘГЕЗ** `require('fs')` ны турыдан-туры кулланырга | Саклау өчен `ctx.settings` кулланыгыз, файл күчерү өчен `ctx.sftp` |
| **КИЧЕКМӘГЕЗ** npm тышкы пакетларын кулланырга | Тулысынча JavaScript — node_modules юк |
| **МӘҖБҮРИ** барлык чит командалар өчен `ctx.terminal.send()` кулланырга | Бу куркынычсыз SSH каналы аша үтә |
| **МӘҖБҮРИ** `deactivate()` да чистартырга | Тыңлаучыларны бетерегез, интервалларны чистартыгыз |

### Хата белән эш итү

Куркыныч операцияләрне һәрвакыт try/catch эченә урнаштырыгыз:
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
### deactivate() да чистарту

Әгәр сезнең плагин интерваллар, тыңлаучылар яки язылулар туды��са — аларны чистартыгыз:
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
### i18n Ярдәме

WIA SOOM 254 телне хуплый. Сезнең плагин ярлыкларын тәрҗемә итәргә мөмкин итү өчен, гади ысулны кулланыгыз:
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

## 6-нчы өлеш: Чын дөнья мисаллары

### Мисал 1: Сервер Диск Тикшерүчесе

Уздыра `df -h` ерак серверда һәм статус панелендә кулланылган/буш урынны күрсәтә.
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

### Мисал 2: TODO Менеджеры

TODO исемлеген идарә итүче плагин, тотрыклы саклау өчен көйләүләр һәм күрсәтү өчен веб-күренеш куллана.

> **Дизайн паттерны:** Веб-күренешләр плагин API-ларын турыдан-туры чакыра алмаганлыктан, бу плагин "снапшот" ысулын куллана — ул TODO-ларны көйләүләрдән укый, аларны укырга гына мөмкин булган HTML итеп ясый, һәм элементларны өстәү өчен сайдбар нигезендә гамәл��әр тәкъдим итә. Веб-күренеш — **күрсәтү** катламы, интерактив форма түгел.
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

### Мисал 3: Хата Күзәтүчесе

Терминал чыгышын күзәтә һәм билгеле паттерннар табылганда хәбәр җибәрә.
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

## Дополнение: Категории и Иконки

### Категории Плагинов (29)

Используйте это в вашем `package.json` `keywords` или при отправке в реестр:

| Категория | Описание |
|-----------|----------|
| `server` | Общее управление сервером |
| `devtools` | Инструменты разработки |
| `calculator` | Калькуляторы и конвертеры |
| `simulator` | Симуляторы |
| `game` | Игры в терминале |
| `business` | Инструменты для бизнеса |
| `security` | Безопасность и аудит |
| `web` | Управление веб-сервером |
| `education` | Образовательные инструменты |
| `health` | Инструменты, связанные со здоровьем |
| `islamic` | Исламские инструменты (время молитвы и т.д.) |
| `science` | Научные инструменты |
| `quantum` | Инструменты квантовых вычислений |
| `ai` | Инструменты на основе ИИ |
| `biotech` | Инструменты биотехнологий |
| `space` | Инструменты для космоса и астрономии |
| `network` | Сетевые инструменты |
| `database` | Управление базами данных |
| `monitoring` | Мониторинг серверов |
| `devops` | DevOps и CI/CD |
| `utility` | Общие утилиты |
| `design` | Инструменты дизайна |
| `ecommerce` | Инструменты для электронной коммерции |
| `automation` | Инструменты автоматизации |
| `kpop` | Инструменты, связанные с K-pop |
| `accessibility` | Инструменты доступности |
| `analytics` | Аналитика и отчетность |
| `wia` | Инструменты экосистемы WIA |
| `all` | Появляется во всех категориях |

### Рекомендуемые Иконки (Lucide)

| Название Иконки | Использовать для |
|-----------------|------------------|
| `server` | Управление сервером |
| `shield` | Безопасность |
| `database` | База данных |
| `activity` | Мониторинг |
| `terminal` | Инструменты терминала |
| `code` | Разработка |
| `hard-drive` | Диск/хранилище |
| `network` | Сетевое взаимодействие |
| `lock` | Аутентификация/шифрование |
| `eye` | Наблюдение/мониторинг |
| `check-square` | Задачи/TODO |
| `layout-dashboard` | Панели управления |
| `settings` | Конфигурация |
| `zap` | Автоматизация |
| `globe` | Веб/междун��родный |

Просмотрите все 1,500+ иконок: [lucide.dev/icons](https://lucide.dev/icons)

---

## Нужна помощь?

- **Проблемы на GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Проблемы с плагинами:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Примеры плагинов:** [Website](https://wiasoom.com)
- **Веб-сайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Создайте что-то удивительное. Поделитесь этим с миром.</em></p>
<p align="center"><em>— Команда WIA SOOM</em></p>