<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ръководство за разработчици на плъгини за WIA SOOM</h1>
<p align="center"><strong>Създайте свой собствен плъгин за 5 минути.</strong></p>
<p align="center">Създавайте мощни сървърни инструменти, табла и автоматизации — директно в WIA SOOM.</p>

---

## Съдържание

- [Част 1: Бързо начало — Вашият първи плъгин за 5 минути](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Част 2: Справочник на API за контекста на плъгина](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Част 3: Създаване на персонализиран UI с Webviews](#part-3-building-custom-ui-with-webviews)
- [Част 4: Публикуване на вашия плъгин](#part-4-publishing-your-plugin)
- [Част 5: Най-до��ри практики](#part-5-best-practices)
- [Част 6: Примери от реалния свят](#part-6-real-world-examples)
- [Приложение: Категории и икони](#appendix-categories--icons)

---

## Част 1: Бързо начало — Вашият първи плъгин за 5 минути

### Какво ще създадете

Плъгин "Hello World", който добавя бутон в страничната лента. Когато бъде натиснат, показва известие.

### Стъпка 1: Създайте папката на плъгина
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Стъпка 2: Създайте package.json
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
**Задължителни полета:** `name`, `version`, `description`, `author`, `main`

### Стъпка 3: Създайте index.js
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
### Стъпка 4: Рестартирайте WIA SOOM

Рестартирайте приложението (или превключете плъгина изключен/включен в Настройки → Плъгини).

Трябва да видите бутон **"Hello World"** в страничната лента. Натиснете го — ще видите известие за успех!

### Как работи
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

## Част 2: Справочник на API за контекста на плъгина

Когато вашата функция `activate(context)` бъде извикана, `context` (или `ctx`) предоставя тези API:
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

### `ctx.terminal` — Изпълнявайте команди на отдалечени сървъри

#### `terminal.send(sessionId, data)`

Изпратете команда (или всякакви данни) до активна терминална сесия.

| Параметър | Тип | Описание |
|-----------|------|-------------|
| `sessionId` | `string` | Терминалната сесия, до която да се изпрати |
| `data` | `string` | Командата или данните, които да се изпратят |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Абонирайте се за целия изход от терминалната сесия. Връща **функция за отписване**.

| Параметър | Тип | Описание |
|-----------|------|-------------|
| `sessionId` | `string` | Терминалната сесия, която да наблюдавате |
| `callback` | `(data: string) => void` | Извиква се с всеки фрагмент от изхода |
| **Връща** | `() => void` | Извикайте това, за да спрете слушането |
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
**Важно:** Винаги запазвайте функцията за отписване и я извиквайте в `deactivate()`, за да предотвратите изтичане на памет.

---

### `ctx.sftp` — Прехвърляне на файлове

> **Статус: Скоро** — SFTP API е дефинирано, но все още не е свързано с SFTP двигателя на приложението. `list()` в момента връща празен масив, а `upload()`/`download()` не извършват действия. Това ще бъде напълно реализирано в бъдеща версия. За момента използвайте `ctx.terminal.send()` с команди `scp` или `rsync` като обходен вариант.

#### `sftp.list(sessionId, path)`

Изброяване на файлове в отдалечена директория.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Качване на файл от локалната машина на отдалечения сървър.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Изтегляне на файл от отдалечения сървър на локалната машина.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Обходен вариант (докато SFTP API не е активен):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Потребителски интерфейс

#### `ui.addSidebarButton(options)`

Добавете бутон в страничната лента на WIA SOOM.

| Опция | Тип | Задължителна | Описание |
|--------|------|----------|-------------|
| `id` | `string` | Не | Уникален ID (по подразбиране е името на плъгина) |
| `icon` | `string` | Да | Име на иконата Lucide (напр. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Да | Текст на бутона, показан в страничната лента |
| `onClick` | `() => void` | Да | Функция, която се извиква, когато бутонът бъде натиснат |
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
**Справочник за икони:** Разгледайте всички налични икони на [lucide.dev/icons](https://lucide.dev/icons)

> **Забележка за съвместимост:** Някои по-стари плъгини използват позиционни аргументи като `addSidebarButton(id, icon, label, onClick)`. Официалният API използва **обект с опции**, както е документирано по-горе. Винаги използвайте стил на обект за нови плъгини.

#### `ui.openWebview(options)`

Отворете изскачащ прозорец с персонализирано HTML съдържание. Това е начинът, по който изграждате богати потребителски интерфейси.

| Опция | Тип | Описание |
|--------|------|-------------|
| `title` | `string` | Заглавие на прозореца |
| `html` | `string` | Пълно HTML съдържание за рендиране |
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
> Вижте [Част 3](#part-3-building-custom-ui-with-webviews) за напреднали шаблони на уеб изгледи.

#### `ui.showNotification(type, message)`

Показва известие.

| Параметър | Тип | Описание |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Стил на известието |
| `message` | `string` | Текст за показване |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Добавя постоянен текстов елемент в долната статус лента.

| Параметър | Тип | Описание |
|-----------|------|-------------|
| `id` | `string` | Уникален ID за този статусен елемент |
| `text` | `string` | Текст за показване |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Постоянно хранилище

Настройките на плъгина се съхраняват постоянно в `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Чете запазена стойност.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Връща `undefined`, ако ключът не съществува.

#### `settings.set(key, value)`

Запазва стойност. Поддържа низове, числа, булеви стойности, масиви и обекти.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Пример: Запомняне на предпочитанията на потребителя**
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

### `ctx.ai` — Интеграция с ИИ

> **Статус: Скоро** — ИИ API е дефинирано, но все още не е свързано с Soomy. В момента връща `{ response: 'AI not yet connected' }`. Планирана е пълна интеграция с ИИ за бъдещо издание.

#### `ai.chat(messages, options?)`

Изпраща съобщения до ИИ асистента (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Част 3: Създаване на персонализиран интерфейс с уеб изгледи

API-то `openWebview()` ви позволява да изграждате интерфейси на таблета с HTML, CSS и JavaScript — всичко в прозорец на изскачащ прозорец.

> **Важно ограничение:** Уеб изгледите са **само за показване**. Те не могат да извикват API-та на плъгини (`ctx.settings`, `ctx.terminal` и т.н.). И��ползвайте странични бутони за всички действия на потребителя и използвайте `openWebview()`, за да покажете текущото състояние. Ако имате нужда от интерактивни функции, задействайте ги от странични бутони и отворете отново уеб изгледа, за да обновите дисплея.

### Шаблон: Команда в терминал → Парсване на изход → Показване в HTML

Това е най-често срещаният шаблон за плъгини. Изпълнявате команда, парсвате резултата и го показвате визуално.
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
### Шаблон: Интерактивен табло с автоматично обновяване
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
### Шаблон: Показване на настройки в уеб изглед

> **Забележка:** Уеб изгледите са само за показване — те ��е могат да извикват API-та на плъгини. Използвайте `ctx.settings` в обработчиците на вашите странични бутони, за да модифицирате настройките, и използвайте `openWebview()`, за да покажете текущото състояние.
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

## Част 4: Публикуване на вашия плъгин

### Стъпка 1: Тест локално

1. Копирайте плъгина си в `~/.wia-soom/plugins/{your-plugin}/`
2. Рестартирайте WIA SOOM
3. Проверете дали работи: бутонът в страничната лента се появява, функциите работят правилно
4. Тествайте гранични случаи: какво се случва, ако няма свързан терминал?

### Стъпка 2: Подготовка за подаване

Вашата папка с плъгини трябва да съдържа:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Задължителни полета в `package.json`:**

| Поле | Описание | Пример |
|-------|-------------|---------|
| `name` | Уникален идентификатор в kebab-case | `"my-awesome-plugin"` |
| `version` | Семантична версия | `"1.0.0"` |
| `description` | Описание в едно изречение | `"Следи nginx access логовете в реално време"` |
| `author` | Вашето име | `"John Doe"` |
| `main` | Входна точка | `"index.js"` |

**Незадължителни полета:**

| Поле | Описание |
|-------|-------------|
| `license` | Тип на лиценза (препоръчва се MIT) |
| `keywords` | Масив от тагове за търсене |
| `soom.minVersion` | Минимална необходима версия на WIA SOOM |

### Стъпка 3: Подаване в Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Добавете** вашия плъги�� в `plugins/{your-plugin-name}/`
3. **Подайте** Pull Request

### Стъпка 4: Преглед и одобрение

Ние преглеждаме всеки плъгин за:

- **Сигурност** — без опасни API (вижте [Правила за сигурност](#security-rules))
- **Качество** — работи ли? Чист ли е кодът?
- **Полезност** — решава ли реален проблем?

След одобрение:
1. Вашият плъгин се добавя в `registry.json`
2. Създава се ZIP пакет в `dist/`
3. Вашият плъгин се появява в **Plugin Store** за всички потребители на WIA SOOM!

---

## Част 5: Най-добри практики

### Правила за сигурност

Тези правила са **задължителни**. Плъгини, които ги нарушават, ще бъдат отхвърлени.

| Правило | Защо |
|------|-----|
| **Никога** не използвайте `eval()` или `new Function()` | Риск от инжектиране на код |
| **Никога** не използвайте `child_process`, `exec()`, `spawn()` | Използвайте само `ctx.terminal.send()` за команди |
| **Никога** не извличайте външни URL адреси | Изключение: API крайни точки на `wiasoom.com` |
| **Никога** не достъпвайте `process.env` | Променливите на средата могат да съдържат тайни |
| **Никога** не използвайте `require('fs')` директно | Използвайте `ctx.settings` за съхранение, `ctx.sftp` за трансфер на файлове |
| **Никога** не използвайте външни npm пакети | Само чист JavaScript — без node_modules |
| **Трябва** да използвате `ctx.terminal.send()` за всички отдалечени команди | Това преминава през сигурния SSH канал |
| **Трябва** да почистите в `deactivate()` | Премахнете слушатели, изчистете интервали |

### Обработка на грешки

Винаги обвивайте рисковите операции в try/catch:
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
### Почистване в deactivate()

Ако вашият плъгин създава интервали, слушате��и или абонаменти — почистете ги:
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
### Поддръжка на i18n

WIA SOOM поддържа 254 езика. За да направите етикета на вашия плъгин преводим, използвайте прост подход:
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

## Част 6: Примери от реалния свят

### Пример 1: Проверка на диска на сървъра

Изпълнява `df -h` на отдалечения сървър и показва използваното/достъпно пространство в статус бара.
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

### Пример 2: Мениджър на TODO

Плъгин, който управлява списък с TODO, използвайки настройки за перманентно съхранение и уеб изглед за показване.

> **Дизайнерски модел:** Тъй като уеб изгледите не могат директно да извикват API на плъгини, този плъгин използва подхода "снимка" — той чете TODO от настройките, рендерира ги като само за четене HTML и предоставя действия, базирани на страничната лента, за добавяне на елементи. Уеб изгледът е **слой за показване**, а не интерактивна форма.
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

### Пример 3: Наблюдател на грешки

Следи изхода на терминала и изпраща известие, когато се открият специфични шаблони.
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

## Приложение: Категории и Икони

### Категории на Плъгини (29)

Използвайте тези в `package.json` `keywords` или при подаване в регистъра:

| Категория | Описание |
|-----------|----------|
| `server` | Общо управление на сървъри |
| `devtools` | Инструменти за разработка |
| `calculator` | Калкулатори и конвертори |
| `simulator` | Симулатори |
| `game` | Игри в терминал |
| `business` | Бизнес инструменти |
| `security` | Сигурност и одит |
| `web` | Управление на уеб сървъри |
| `education` | Образователни инструменти |
| `health` | Инструменти, свързани със здравето |
| `islamic` | Ислямски инструменти (времена за молитва и др.) |
| `science` | Научни инструменти |
| `quantum` | Инструменти за квантово изчисление |
| `ai` | Инструменти с изкуствен интелект |
| `biotech` | Инструменти за биотехнологии |
| `space` | Инструменти за космоса и астрономията |
| `network` | Мрежови инструменти |
| `database` | Управление на бази данни |
| `monitoring` | Наблюдение на сървъри |
| `devops` | DevOps и CI/CD |
| `utility` | Общи утилити |
| `design` | Инструменти за дизайн |
| `ecommerce` | Инструменти за електронна търговия |
| `automation` | Инструменти за автоматизация |
| `kpop` | Инструменти, свързани с K-pop |
| `accessibility` | Инструменти за достъпност |
| `analytics` | Анализ и отчитане |
| `wia` | Инструменти за екосистемата WIA |
| `all` | Появява се във всички категории |

### Препоръчителни Икони (Lucide)

| Име на Иконата | Използвайте за |
|----------------|----------------|
| `server` | Управление на сървъри |
| `shield` | Сигурнос�� |
| `database` | База данни |
| `activity` | Наблюдение |
| `terminal` | Инструменти за терминал |
| `code` | Разработка |
| `hard-drive` | Диск/съхранение |
| `network` | Мрежа |
| `lock` | Аутентификация/шифроване |
| `eye` | Наблюдение/мониторинг |
| `check-square` | Задачи/TODO |
| `layout-dashboard` | Табла за управление |
| `settings` | Конфигурация |
| `zap` | Автоматизация |
| `globe` | Уеб/международно |

Разгледайте всички 1,500+ икони: [lucide.dev/icons](https://lucide.dev/icons)

---

## Нуждаете се от помощ?

- **GitHub Проблеми:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Проблеми с Плъгини:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Примерни Плъгини:** [Website](https://wiasoom.com)
- **Уебсайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Създайте нещо удивително. Споделете го със света.</em></p>
<p align="center"><em>— Екипът на WIA SOOM</em></p>