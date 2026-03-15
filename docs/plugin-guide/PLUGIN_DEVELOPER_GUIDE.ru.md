<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Руководство разработчика плагинов WIA SOOM</h1>
<p align="center"><strong>Создайте свой собственный плагин за 5 минут.</strong></p>
<p align="center">Создавайте мощные серверные инструменты, панели управления и автоматизации — прямо внутри WIA SOOM.</p>

---

## Содержание

- [Часть 1: Быстрый старт — Ваш первый плагин за 5 минут](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Часть 2: Справочник по API контекста плагина](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Часть 3: Создание пользовательского интерфейса с помощью Webviews](#part-3-building-custom-ui-with-webviews)
- [Часть 4: Публикация вашего плагина](#part-4-publishing-your-plugin)
- [Часть 5: Лучшие практики](#part-5-best-practices)
- [Часть 6: Примеры из реальной жизни](#part-6-real-world-examples)
- [Приложение: Категории и значки](#appendix-categories--icons)

---

## Часть 1: Быстрый старт — Ваш первый плагин за 5 минут

### Что вы создадите

Плагин "Hello World", который добавляет кнопку на боковую панель. При нажатии она показывает уведомление.

### Шаг 1: Создайте папку плагина
§§§CHUNK_SEPARATOR§§§
### Шаг 2: Создайте package.json
§§§CHUNK_SEPARATOR§§§
**Обязательные поля:** `name`, `version`, `description`, `author`, `main`

### Шаг 3: Создайте index.js
§§§CHUNK_SEPARATOR§§§
### Шаг 4: Перезапустите WIA SOOM

Перезапустите приложение (или переключите плагин в вык��юченное/включенное состояние в Настройки → Плагины).

Вы должны увидеть кнопку **"Hello World"** на боковой панели. Нажмите на нее — вы увидите уведомление об успехе!

### Как это работает
§§§CHUNK_SEPARATOR§§§
---

## Часть 2: Справочник по API контекста плагина

Когда ваша функция `activate(context)` вызывается, `context` (или `ctx`) предоставляет эти API:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Выполнение команд на удаленных серверах

#### `terminal.send(sessionId, data)`

Отправить команду (или любые данные) в активную сессию терминала.

| Параметр | Тип | Описание |
|-----------|------|-------------|
| `sessionId` | `string` | Сессия терминала, в которую отправляется |
| `data` | `string` | Команда или данные для отправки |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Подписаться на все выводы из сессии терминала. Возвращает **функцию отмены подписки**.

| Параметр | Тип | Описание |
|-----------|------|-------------|
| `sessionId` | `string` | Сессия терминала, за которой нужно следить |
| `callback` | `(data: string) => void` | Вызывается с каждым фрагментом вывода |
| **Возвращает** | `() => void` | Вызовите это, чтобы прекратить прослушивание |
§§§CHUNK_SEPARATOR§§§
**Важно:** Всегда сохраняйте функцию отмены подписки и вызывайте ее в `deactivate()`, чтобы предотвратить утечки памяти.

---

### `ctx.sftp` — Передача файлов

> **Статус: Скоро** — API SFTP определен, но еще не подключен к SFTP-движку приложения. `list()` в настоящее время возвращает пустой массив, а `upload()`/`download()` не выполняют никаких действий. Это будет полностью реализовано в будущем релизе. Пока используйте `ctx.terminal.send()` с командами `scp` ил�� `rsync` в качестве обходного пути.

#### `sftp.list(sessionId, path)`

Список файлов в удаленной директории.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Загрузить файл с локального компьютера на удаленный сервер.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Скачать файл с удаленного сервера на локальный компьютер.
§§§CHUNK_SEPARATOR§§§
**Обходной путь (до активации API SFTP):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Пользовательский интерфейс

#### `ui.addSidebarButton(options)`

Добавить кнопку на боковую панель WIA SOOM.

| Опция | Тип | Обязательная | Описание |
|--------|------|----------|-------------|
| `id` | `string` | Нет | Уникальный ID (по умолчанию — имя плагина) |
| `icon` | `string` | Да | Название значка Lucide (например, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Да | Текст кнопки, отображаемый н�� боковой панели |
| `onClick` | `() => void` | Да | Функция, вызываемая при нажатии на кнопку |
§§§CHUNK_SEPARATOR§§§
**Справочник значков:** Просмотрите все доступные значки на [lucide.dev/icons](https://lucide.dev/icons)

> **Примечание о совместимости:** Некоторые старые плагины используют позиционные аргументы, такие как `addSidebarButton(id, icon, label, onClick)`. Официальный API использует **объект опций**, как описано выше. Всегда используйте стиль объекта для новых плагинов.

#### `ui.openWebview(options)`

Открыть всплывающее окно с пользовательским HTML-контентом. Так вы создаете богатые пользовательские интерфейсы.

| Опция | Тип | Описание |
|--------|------|-------------|
| `title` | `string` | Заголовок окна |
| `html` | `string` | Полный HTML-контент для рендеринга |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> См. [Часть 3](#part-3-building-custom-ui-with-webviews) для продвинутых паттернов веб-просмотров.

#### `ui.showNotification(type, message)`

Показать уведомление в виде тоста.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `type` | `'success' \| 'error' \| 'info'` | Стиль уведомления |
| `message` | `string` | Текст для отображения |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

Добавить постоянный текстовый элемент в нижнюю панель состояния.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `id` | `string` | Уникальный ID для этого элемента состояния |
| `text` | `string` | Текст для отображения |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — Постоянное хранилище

Настройки плагина хранятся постоянно в `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Считать сохраненное значение.
§§§CHUNK_SEPARATOR§§§
Возвращает `undefined`, если ключ не существует.

#### `settings.set(key, value)`

Сохранить значение. Поддерживает строки, числа, булевы значения, массивы и объекты.
§§§CHUNK_SEPARATOR§§§
**Пример: Запомнить предпочтения пользователя**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — Интеграция ИИ

> **Статус: Скоро** — API ИИ определен, но еще не подключен к Soomy. В настоящее время возвращает `{ response: 'AI not yet connected' }`. Полная интеграция ИИ запланирована на будущий релиз.

#### `ai.chat(messages, options?)`

Отправить сообщения ассистенту ИИ (Soomy).
§§§CHUNK_SEPARATOR§§§
---

## Часть 3: Создание пользовательского интерфейса с помощью веб-просмотров

API `openWebview()` позволяет создавать интерфейсы панелей управления с использованием HTML, CSS и JavaScript — все внутри всплывающего окна.

> **Ва��ное ограничение:** Веб-просмотры являются **только для отображения**. Они не могут вызывать API плагина (`ctx.settings`, `ctx.terminal` и т.д.). Используйте кнопки боковой панели для всех действий пользователя и используйте `openWebview()` для отображения текущего состояния. Если вам нужны интерактивные функции, запускайте их с помощью кнопок боковой панели и повторно открывайте веб-просмотр для обновления отображения.

### Паттерн: Команда терминала → Парсинг вывода → Отображение в HTML

Это самый распространенный паттерн плагина. Вы выполняете команду, парсите результат и визуально отображаете его.
§§§CHUNK_SEPARATOR§§§
### Паттерн: Интерактивная панель управления с автообновлением
§§§CHUNK_SEPARATOR§§§
### Паттерн: Отображение настроек в веб-просмотре

> **Примечание:** Веб-просмотры являются только для отображения — они не могут вызывать API плагина. Используйте `ctx.settings` в обработчиках кнопок боковой панели для изменения настроек и используйте `openWebview()` для отображения текущего состояния.
§§§CHUNK_SEPARATOR§§§
---

## Часть 4: Публикация вашего плагина

### Шаг 1: Тестирование локально

1. Скопируйте ваш плагин в `~/.wia-soom/plugins/{your-plugin}/`
2. Перезапустите WIA SOOM
3. Убедитесь, что все работает: кнопка боковой панели появляется, функции работают корректно
4. Протестируйте крайние случаи: что происходит, если терминал не подключен?

### Шаг 2: Подготовка к отправке

Ваша папка с плагином долж��а содержать:
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
**Обязательные поля `package.json`:**

| Поле | Описание | Пример |
|------|----------|--------|
| `name` | Уникальный идентификатор в kebab-case | `"my-awesome-plugin"` |
| `version` | Семантическая версия | `"1.0.0"` |
| `description` | Однострочное описание | `"Мониторит логи доступа nginx в реальном времени"` |
| `author` | Ваше имя | `"John Doe"` |
| `main` | Точка входа | `"index.js"` |

**Необязательные поля:**

| Поле | Описание |
|------|----------|
| `license` | Тип лицензии (рекомендуется MIT) |
| `keywords` | Массив тегов для поиска |
| `soom.minVersion` | Минимальная требуемая версия WIA SOOM |

### Шаг 3: Отправка в реестр плагинов

1. **Сделайте форк** [Plugin Store](https://wiasoom.com)
2. **Добавьте** ваш плагин в `plugins/{your-plugin-name}/`
3. **От��равьте** Pull Request

### Шаг 4: Проверка и одобрение

Мы проверяем каждый плагин на:

- **Безопасность** — отсутствие опасных API (см. [Правила безопасности](#security-rules))
- **Качество** — работает ли он? Чист ли код?
- **Полезность** — решает ли он реальную проблему?

После одобрения:
1. Ваш плагин добавляется в `registry.json`
2. Создается ZIP-архив в `dist/`
3. Ваш плагин появляется в **Магазине плагинов** для всех пользователей WIA SOOM!

---

## Часть 5: Лучшие практики

### Правила безопасности

Эти правила являются **обязательными**. Плагины, которые их нарушают, будут отклонены.

| Правило | Почему |
|---------|--------|
| **НИКОГДА** не используйте `eval()` или `new Function()` | Риск инъекции кода |
| **НИКОГДА** не используйте `child_process`, `exec()`, `spawn()` | Используйте только `ctx.terminal.send()` для команд |
| **НИКОГДА** не запрашивайте внешние URL | Исключение: API-эндпоинты `wiasoom.com` |
| **НИКОГДА** не получайте доступ к `process.env` | Переменные окружения могут содержать секреты |
| **НИКОГДА** не используйте `require('fs')` напрямую | Используйте `ctx.settings` для хранения, `ctx.sftp` для передачи файлов |
| **НИКОГДА** не используйте внешние пакеты npm | Только чистый JavaScript — без node_modules |
| **ДОЛЖНЫ** использовать `ctx.terminal.send()` для всех удаленных команд | Это проходит через защищенный SSH-канал |
| **ДОЛЖНЫ** очищать в `deactivate()` | Удаляйте слушатели, очищайте интервалы |

### Обработка ошибок

Всегда оборачивайте рискованные операции в try/catch:
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
### Очистка в deactivate()

Если ваш плагин создае�� интервалы, слушатели или подписки — очистите их:
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
### Поддержка i18n

WIA SOOM поддерживает 254 языка. Чтобы сделать метки вашего плагина переводимыми, используйте простой подход:
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

## Часть 6: Примеры из реальной жизни

### Пример 1: Проверка диска сервера

Запускает `df -h` на удаленном сервере и показывает использованное/доступное пространство в строке состояния.
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### Пример 2: Менеджер TODO

Плагин, который управляет списком TODO, используя настройки для постоянного хранения и веб-просмотр для отображения.

> **Шаблон проектирования:** Поскольку веб-просмотры не могут напрямую вызывать API плагинов, этот плагин использует подход "снимка" — он считывает TODO из настроек, отображает их как только для чтения HTML и предоставляет действия на основе боковой панели для добавления элементов. Веб-просмотр является **слоем отображения**, а не интерактивной формой.
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

### Пример 3: Наблюдатель за ошибками

Мониторит вывод терминала и отправляет уведомление, когда обнаруживаются определенные шаблоны.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
---

## Приложение: Категории и Иконки

### Категории Плагинов (29)

Используйте их в вашем `package.json` `keywords` или при отправке в реестр:

| Категория | Описание |
|-----------|----------|
| `server` | Общее управление сервером |
| `devtools` | Инструменты разработки |
| `calculator` | Калькуляторы и конвертеры |
| `simulator` | Симуляторы |
| `game` | Игры в терминале |
| `business` | Бизнес-инструменты |
| `security` | Безопасность и аудит |
| `web` | Управление веб-сервером |
| `education` | Образовательные инструменты |
| `health` | Инструменты, связанные со здоровьем |
| `islamic` | Исламские инструменты (время молитвы и т.д.) |
| `science` | Научные инструменты |
| `quantum` | Инструменты квантовых вычислений |
| `ai` | Инструменты на основе ИИ |
| `biotech` | Инструменты биотехнологии |
| `space` | Инструменты для космоса и астрономии |
| `network` | Сетевые инструменты |
| `database` | Управление базами данных |
| `monitoring` | Мониторинг серверов |
| `devops` | DevOps и CI/CD |
| `utility` | Общие утилиты |
| `design` | Инструменты дизайна |
| `ecommerce` | Инструменты электронной коммерции |
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
| `hard-drive` | Д��ск/хранение |
| `network` | Сетевые подключения |
| `lock` | Аутентификация/шифрование |
| `eye` | Наблюдение/мониторинг |
| `check-square` | Задачи/TODO |
| `layout-dashboard` | Панели управления |
| `settings` | Конфигурация |
| `zap` | Автоматизация |
| `globe` | Веб/международный |

Просмотрите все 1,500+ иконок: [lucide.dev/icons](https://lucide.dev/icons)

---

## Нужна Помощь?

- **Проблемы на GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Проблемы с Плагинами:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Примеры Плагинов:** [Website](https://wiasoom.com)
- **Вебсайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Создайте что-то удивительное. Под��литесь этим с миром.</em></p>
<p align="center"><em>— Команда WIA SOOM</em></p>
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```

#### `sftp.download(sessionId, remotePath, localPath)`

Download a file from remote server to local machine.

```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```

**Workaround (until SFTP API is live):**

```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```

---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Add a button to the WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (defaults to plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Button text shown in sidebar |
| `onClick` | `() => void` | Yes | Function called when button is clicked |

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

**Icon reference:** Browse all available icons at [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Some older plugins use positional arguments like `addSidebarButton(id, icon, label, onClick)`. The official API uses an **options object** as documented above. Always use the object style for new plugins.

#### `ui.openWebview(options)`

Open a popup window with custom HTML content. This is how you build rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content to render |

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

Show a toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Text to show |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

Add a persistent text item to the bottom status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique ID for this status item |
| `text` | `string` | Text to display |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — Persistent storage

Plugin settings are stored permanently in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Read a saved value.

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

Returns `undefined` if the key doesn't exist.

#### `settings.set(key, value)`

Save a value. Supports strings, numbers, booleans, arrays, and objects.

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**Example: Remember user preferences**

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

### `ctx.ai` — AI integration

> **Status: Coming Soon** — The AI API is defined but not yet connected to Soomy. Currently returns `{ response: 'AI not yet connected' }`. Full AI integration is planned for a future release.

#### `ai.chat(messages, options?)`

Send messages to the AI assistant (Soomy).

```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```

---

## Part 3: Building Custom UI with Webviews

The `openWebview()` API lets you build dashboard UIs with HTML, CSS, and JavaScript — all inside a popup window.

> **Important limitation:** Webviews are **display-only**. They cannot call back into plugin APIs (`ctx.settings`, `ctx.terminal`, etc.). Use sidebar buttons for all user actions, and use `openWebview()` to display current state. If you need interactive features, trigger them from sidebar buttons and re-open the webview to refresh the display.

### Pattern: Terminal Command → Parse Output → Show in HTML

This is the most common plugin pattern. You run a command, parse the result, and display it visually.

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

### Pattern: Interactive Dashboard with Auto-Refresh

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

### Pattern: Displaying Settings in a Webview

> **Note:** Webviews are display-only — they cannot call back into plugin APIs. Use `ctx.settings` in your sidebar button handlers to modify settings, and use `openWebview()` to show the current state.

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
