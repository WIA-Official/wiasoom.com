<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Посібник для розробників плагінів WIA SOOM</h1>
<p align="center"><strong>Створіть свій власний плагін за 5 хвилин.</strong></p>
<p align="center">Створюйте потужні серверні інструменти, інформаційні панелі та автоматизації — прямо в WIA SOOM.</p>

---

## Зміст

- [Частина 1: Швидкий старт — Ваш перший плагін за 5 хвилин](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Частина 2: Посилання на API контексту плагіна](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Частина 3: Створення користувацького інтерфейсу з веб-переглядачами](#part-3-building-custom-ui-with-webviews)
- [Частина 4: Публікація вашого плагіна](#part-4-publishing-your-plugin)
- [Частина 5: Найкращі практики](#part-5-best-practices)
- [Частина 6: Приклади з реального життя](#part-6-real-world-examples)
- [Додаток: Категорії та іконки](#appendix-categories--icons)

---

## Частина 1: Швидкий старт — Ваш перший плагін за 5 хвилин

### Що ви створите

Плагін "Hello World", який додає кнопку на бокову панель. Коли на неї натискають, з'являється сповіщення.

### Крок 1: Створіть папку плагіна
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Крок 2: Створіть package.json
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
**Обов'язкові поля:** `name`, `version`, `description`, `author`, `main`

### Крок 3: Створіть index.js
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
### Крок 4: Перезапустіть WIA SOOM

Перезапустіть додаток (або вимкніть/включіть плагін у Налаштування → Плагіни).

Ви повинні побачити кнопку **"Hello World"** на боковій панелі. Натисніть на неї — ви побачите сповіщення про успіх!

### Як це працює
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

## Частина 2: Посилання на API контексту плагіна

Коли ваша функція `activate(context)` викликається, `context` (або `ctx`) надає ці API:
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

### `ctx.terminal` — Виконання команд на віддалених серверах

#### `terminal.send(sessionId, data)`

Відправте команду (або будь-які дані) до активної сесії терміналу.

| Параметр | Тип | Опис |
|-----------|------|-------------|
| `sessionId` | `string` | Сесія терміналу, до якої потрібно надіслати |
| `data` | `string` | Команда або дані для відправлення |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Підпишіться на весь вихід з сесії терміналу. Повертає **функцію скасування підписки**.

| Параметр | Тип | Опис |
|-----------|------|-------------|
| `sessionId` | `string` | Сесія терміналу, за якою потрібно спостерігати |
| `callback` | `(data: string) => void` | Викликається з кожним фрагментом виходу |
| **Повертає** | `() => void` | Викликайте це, щоб зупинити прослуховування |
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
**Важливо:** Завжди зберігайте функцію скасування підписки та викликайте її в `deactivate()`, щоб уникнути витоків пам'яті.

---

### `ctx.sftp` — Передача файлів

> **Статус: Скоро** — API SFTP визначено, але ще не підключено до SFTP-двигуна програми. `list()` наразі повертає порожній масив, а `upload()`/`download()` не виконуються. Це буде повністю реалізовано в майбутньому випуску. Поки що використовуйте `ctx.terminal.send()` з командами `scp` або `rsync` як обхідний шлях.

#### `sftp.list(sessionId, path)`

Перегляньте файли в віддаленій директорії.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

��авантажте файл з локального комп'ютера на віддалений сервер.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Завантажте файл з віддаленого сервера на локальний комп'ютер.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Обхідний шлях (до активації API SFTP):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Користувацький інтерфейс

#### `ui.addSidebarButton(options)`

Додайте кнопку на бокову панель WIA SOOM.

| Опція | Тип | Обов'язково | Опис |
|--------|------|----------|-------------|
| `id` | `string` | Ні | Унікальний ID (за замовчуванням — ім'я плагіна) |
| `icon` | `string` | Так | Назва іконки Lucide (наприклад, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Так | Текст кнопки, що відображається на боковій панелі |
| `onClick` | `() => void` | Так | Функція, що викликається при натисканні кнопки |
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
**Посилання на іконки:** Перегляньте всі доступні іконки на [lucide.dev/icons](https://lucide.dev/icons)

> **Примітка щодо сумісності:** Деякі старі плагіни використовують позиційні аргументи, такі як `addSidebarButton(id, icon, label, onClick)`. Офіційний API використовує **об'єкт опцій**, як зазначено вище. Завжди використовуйте стиль об'єкта для нових плагінів.

#### `ui.openWebview(options)`

Відкрийте вікно спливаючого вікна з користувацьким HTML-контентом. Це спосіб створення багатих інтерфейсів.

| Опція | Тип | Опис |
|--------|------|-------------|
| `title` | `string` | Назва вікна |
| `html` | `string` | Повний HTML-контент для відображення |
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
> Дивіться [Частина 3](#part-3-building-custom-ui-with-webviews) для розширених шаблонів веб-переглядів.

#### `ui.showNotification(type, message)`

Показати сповіщення у вигляді тосту.

| Параметр | Тип | Опис |
|----------|-----|------|
| `type` | `'success' \| 'error' \| 'info'` | Стиль сповіщення |
| `message` | `string` | Текст для відображення |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Додати постійний текстовий елемент до нижньої панелі стану.

| Параметр | Тип | Опис |
|----------|-----|------|
| `id` | `string` | Унікальний ID для цього елемента стану |
| `text` | `string` | Текст для відображення |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Постійне зберігання

Налаштування плагіна зберігаються постійно у `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Прочитати збережене значення.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Повертає `undefined`, якщо ключ не існує.

#### `settings.set(key, value)`

Зберегти значення. Підтримує рядки, числа, булеві значення, масиви та об'єкти.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Приклад: Запам'ятати уподобання користувача**
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

### `ctx.ai` — Інтеграція ШІ

> **Статус: Скоро** — API ШІ визначено, але ще не підключено до Soomy. Наразі повертає `{ response: 'AI not yet connected' }`. Повна інтеграція ШІ запланована на майбутнє.

#### `ai.chat(messages, options?)`

Надіслати повідомлення асистенту ШІ (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Частина 3: Створення користувацького інтерфейсу з веб-переглядами

API `openWebview()` дозволяє створювати панелі приладів з HTML, CSS та JavaScript — все це в спливаючому вікні.

> **Важливе обмеження:** Веб-перегляди є **тільки для відображення**. Вони не можуть викликати API плагінів (`ctx.settings`, `ctx.terminal` тощо). Використовуйте кнопки бокової панелі для всіх дій користувача, а `openWebview()` для відображення поточного стану. Якщо вам потрібні інтерактивні функції, активуйте їх з кнопок бокової панелі та повторно відкрийте веб-перегляд для оновлення відображення.

### Шаблон: Команда терміналу → Обробка виходу → Відображення в HTML

Це найпоширеніший шаблон плагіна. Ви виконуєте команду, обробляєте результат і візуально його відображаєте.
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
### Шаблон: Інтерактивна панель приладів з автоматичним оновленням
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
### Шаблон: Відображення налаштувань у веб-перегляді

> **Примітка:** Веб-перегляди є лише для відображення — вон�� не можуть викликати API плагінів. Використовуйте `ctx.settings` у ваших обробниках кнопок бокової панелі для зміни налаштувань і використовуйте `openWebview()` для відображення поточного стану.
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

## Частина 4: Публікація вашого плагіна

### Крок 1: Тестування локально

1. Скопіюйте ваш плагін до `~/.wia-soom/plugins/{your-plugin}/`
2. Перезапустіть WIA SOOM
3. Перевірте, чи працює: кнопка бокової панелі з'являється, функції працюють правильно
4. Тестуйте крайні випадки: що станеться, якщо термінал не підключено?

### Крок 2: Підготовка до подання

Ваша папка плагіна повинна містити:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Обов'язкові поля `package.json`:**

| Поле | Опис | Приклад |
|------|------|---------|
| `name` | Унікальний ID в kebab-case | `"my-awesome-plugin"` |
| `version` | Семантична версія | `"1.0.0"` |
| `description` | Однорядковий опис | `"Моніторить доступ до логів nginx в реальному часі"` |
| `author` | Ваше ім'я | `"John Doe"` |
| `main` | Точка входу | `"index.js"` |

**Необов'язкові поля:**

| Поле | Опис |
|------|------|
| `license` | Тип ліцензії (рекомендується MIT) |
| `keywords` | Масив тегів для пошуку |
| `soom.minVersion` | Мінімальна версія WIA SOOM, яка потрібна |

### Крок 3: Подання до Реєстру Плагінів

1. **Зробіть **Package** your plugin as a ZIP file
2. **Додайте** ваш плагін до `plugins/{your-plugin-name}/`
3. **Подайте** Pull Request

### Крок 4: Розгляд та затвердження

Ми розглядаємо кожен плагін на предмет:

- **Безпеки** — жодних небезпечних API (див. [Правила безпеки](#security-rules))
- **Якості** — чи працює він? Чи чистий код?
- **Корисності** — чи вирішує він реальну проблему?

Після затвердження:
1. Ваш плагін буде додано до `registry.json`
2. Створено ZIP-архів у `dist/`
3. Ваш плагін з'явиться в **Магазині плагінів** для всіх користувачів WIA SOOM!

---

## Частина 5: Найкращі практики

### Правила безпеки

Ці правила є **обов'язковими**. Плагіни, які їх порушують, будуть відхилені.

| Правило | Чому |
|---------|------|
| **НІКОЛИ** не використовуйте `eval()` або `new Function()` | Ризик ін'єкції коду |
| **НІКОЛИ** не використовуйте `child_process`, `exec()`, `spawn()` | Використовуйте лише `ctx.terminal.send()` для команд |
| **НІКОЛИ** не запитуйте зовнішні URL | Виключення: API кінцеві точки `wiasoom.com` |
| **НІКОЛИ** не отримуйте доступ до `process.env` | Змінні середовища можуть містити секрети |
| **НІКОЛИ** не використовуйте `require('fs')` безпосередньо | Використовуйте `ctx.settings` для зберігання, `ctx.sftp` для передачі файлів |
| **НІКОЛИ** не використовуйте зовнішні пакети npm | Тільки чистий JavaScript — без node_modules |
| **ПОВИННІ** використовувати `ctx.terminal.send()` для всіх віддалених команд | Це проходить через безпечний SSH-канал |
| **ПОВИННІ** прибрати в `deactivate()` | Видалити слухачів, очистити інтервали |

### Обробка помилок

Завжди обгортайте ризикові операції в try/catch:
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
### Прибирання в deactivate()

Якщо ваш плагін створює інтервали, слухачів або ��ідписки — при��ирайте їх:
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
### Підтримка i18n

WIA SOOM підтримує 254 мови. Щоб зробити мітку вашого плагіна перекладною, використовуйте простий підхід:
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

## Частина 6: Приклади з реального життя

### Приклад 1: Перевірка диска сервера

Виконує `df -h` на віддаленому сервері та показує використане/доступне місце в рядку стану.
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

### Приклад 2: Менеджер TODO

Плагін, який управляє списком TODO, використовуючи налаштування для постійного зберігання та веб-перегляд для відображення.

> **Шаблон проектування:** Оскільки веб-перегляди не можуть безпосередньо викликати API плагінів, цей плагін використовує підхід "знімка" — він читає TODO з налаштувань, рендерить їх як HTML лише для читання та надає дії на основі бокової панелі для додавання елементів. Веб-перегляд є **шаром відображення**, а не інтерактивною формою.
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

### Приклад 3: Спостерігач за помилками

Моніторить вивід терміналу та надсилає сповіщення, коли виявляються певні шаблони.
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

## Додаток: Категорії та Іконки

### Категорії Плагінів (29)

Використовуйте ці категорії у вашому `package.json` в `keywords` або при подачі до реєстру:

| Категорія | Опис |
|-----------|------|
| `server` | Загальне управління сервером |
| `devtools` | Інструменти для розробки |
| `calculator` | Калькулятори та конвертери |
| `simulator` | Симулятори |
| `game` | Ігри в терміналі |
| `business` | Бізнес-інструменти |
| `security` | Безпека та аудит |
| `web` | Управління веб-сервером |
| `education` | Освітні інструменти |
| `health` | Інструменти, пов'язані зі здоров'ям |
| `islamic` | Ісламські інструменти (часи молитви тощо) |
| `science` | Наукові інструменти |
| `quantum` | Інструменти для квантових обчислень |
| `ai` | Інструменти на основі штучного інтелекту |
| `biotech` | Інструменти біотехнології |
| `space` | Інструменти для космосу та астрономії |
| `network` | Мережеві інструменти |
| `database` | Управління базами даних |
| `monitoring` | Моніторинг серверів |
| `devops` | DevOps та CI/CD |
| `utility` | Загальні утиліти |
| `design` | Інструменти дизайну |
| `ecommerce` | Інструменти електронної комерції |
| `automation` | Інструменти автоматизації |
| `kpop` | Інструменти, пов'язані з K-pop |
| `accessibility` | Інструменти доступності |
| `analytics` | Аналітика та звітність |
| `wia` | Інструменти екосистеми WIA |
| `all` | З'являється у всіх категоріях |

### Рекомендовані Іконки (Lucide)

| Назва Іконки | Використовується для |
|--------------|---------------------|
| `server` | Управління серве��ом |
| `shield` | Безпека |
| `database` | База даних |
| `activity` | Моніторинг |
| `terminal` | Інструменти терміналу |
| `code` | Розробка |
| `hard-drive` | Диск/зберігання |
| `network` | Мережа |
| `lock` | Аутентифікація/шифрування |
| `eye` | Спостереження/моніторинг |
| `check-square` | Завдання/TODO |
| `layout-dashboard` | Панелі управління |
| `settings` | Налаштування |
| `zap` | Автоматизація |
| `globe` | Веб/міжнародний |

Перегляньте всі 1,500+ іконок: [lucide.dev/icons](https://lucide.dev/icons)

---

## Потрібна Допомога?

- **Проблеми на GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Проблеми з Плагінами:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Приклад Плагінів:** [Website](https://wiasoom.com)
- **Вебсайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Створіть щось неймовір��е. Поділіться цим з світом.</em></p>
<p align="center"><em>— Команда WIA SOOM</em></p>