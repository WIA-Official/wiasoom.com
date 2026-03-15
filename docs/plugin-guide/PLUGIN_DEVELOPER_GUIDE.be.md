<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Кіраўніцтва па распрацоўцы плагінаў WIA SOOM</h1>
<p align="center"><strong>Стварыце свой уласны плагін за 5 хвілін.</strong></p>
<p align="center">Стварайце магутныя серверныя інструменты, панэлі кіравання і аўтаматызацыі — непасрэдна ў WIA SOOM.</p>

---

## Змястоўны спіс

- [Частка 1: Хуткі старт — Ваш першы плагін за 5 хвілін](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Частка 2: Спасылка на API кантэксту плагіна](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Частка 3: Стварэнне карыстальніцкага інтэрфейсу з Webviews](#part-3-building-custom-ui-with-webviews)
- [Частка 4: Публікацыя вашага плагіна](#part-4-publishing-your-plugin)
- [Частка 5: Лепшыя практыкі](#part-5-best-practices)
- [Частка 6: Рэальныя прыклады](#part-6-real-world-examples)
- [Дадатак: Катэгорыі і значкі](#appendix-categories--icons)

---

## Частка 1: Хуткі старт — Ваш першы плагін за 5 хвілін

### Што вы створыце

Плагін "Hello World", які дадае кнопку ў бакавую панэль. Калі на яе націснуць, з'явіцца паведамленне.

### Крок 1: Стварыце тэчку плагіна
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Крок 2: Стварыце package.json
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
**Абавязковыя палі:** `name`, `version`, `description`, `author`, `main`

### Крок 3: Стварыце index.js
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
### Крок 4: Перазапусціце WIA SOOM

Перазапусціце прыкладанне (ці пераключыце плагін у выключаны/увімкнуты стан у Нал��дах → Плагіны).

Вы павінны ўбачыць кнопку **"Hello World"** у бакавай панэлі. Націсніце на яе — вы ўбачыце паведамленне аб паспяховым выкананні!

### Як гэта працуе
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

## Частка 2: Спасылка на API кантэксту плагіна

Калі ваша функцыя `activate(context)` выклікаецца, `context` (ці `ctx`) прадастаўляе гэтыя API:
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

### `ctx.terminal` — Выкананне каманд на аддаленых серверах

#### `terminal.send(sessionId, data)`

Адпраўце каманду (ці любыя дадзеныя) у актыўную сесію тэрмінала.

| Параметр | Тып | Апісанне |
|-----------|------|-------------|
| `sessionId` | `string` | Сесія тэрмінала, у якую адпраўляецца |
| `data` | `string` | Каманда або дадзеныя для адпраўкі |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Падпішыцеся на ўсе выхады з сесіі тэрмінала. Верне **функцыю адпісання**.

| Параметр | Тып | Апісанне |
|-----------|------|-------------|
| `sessionId` | `string` | Сесія тэрмінала, якую трэба адсочваць |
| `callback` | `(data: string) => void` | Выклікаецца з кожным фрагментам выхаду |
| **Вертае** | `() => void` | Выклічце гэта, каб спыніць праслухоўванне |
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
**Важна:** Заўсёды захоўвайце функцыю адпісання і выклікайце яе ў `deactivate()`, каб прадухіліць уцечкі памяці.

---

### `ctx.sftp` — Перадача файлаў

> **Статус: Скора** — API SFTP вызначаны, але яшчэ не падключаны да SFTP рухавіка прыкладання. `list()` у цяперашні час вяртае пусты масіў, а `upload()`/`download()` не працуюць. Гэта будзе цалкам рэалізавана ў будучым выпуску. Пакуль выкарыстоўвайце `ctx.terminal.send()` з камандамі `scp` або `rsync` у якасці абыходу.

#### `sftp.list(sessionId, path)`

Скласці спіс файлаў у аддаленай дырэктор��і.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Загрузіць файл з лакальнай машыны на аддалены сервер.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Спампаваць файл з аддаленага сервера на лакальную машыну.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Абыход (да таго часу, пакуль API SFTP не будзе актыўны):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Карыстальніцкі інтэрфейс

#### `ui.addSidebarButton(options)`

Дадайце кнопку ў бакавую панэль WIA SOOM.

| Опцыя | Тып | Абавязковая | Апісанне |
|--------|------|----------|-------------|
| `id` | `string` | Не | Унікальны ID (па змаўчанні — імя плагіна) |
| `icon` | `string` | Так | Назва значка Lucide (напрыклад, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Так | Тэкст кнопкі, які адлюстроўваецца ў бакавай панэлі |
| `onClick` | `() => void` | Так | Функцыя, якая выклікаецца, калі націскаюць на кнопку |
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
**Спасылка на значкі:** Паглядзіце ўсе даступныя значкі на [lucide.dev/icons](https://lucide.dev/icons)

> **Заўвага аб сумяшчальнасці:** Некаторыя старыя плагіны выкарыстоўваюць пазіцыйныя аргументы, такія як `addSidebarButton(id, icon, label, onClick)`. Афіцыйны API выкарыстоўвае **аб'ект опцый**, як дакументавана вышэй. Заўсёды выкарыстоўвайце стыль аб'екта для новых плагінаў.

#### `ui.openWebview(options)`

Адкрыйце акно з усплываючым акном з карыстальніцкім HTML-кантэнтам. Такім чынам, вы будуеце багатыя карыстальніцкія інтэрфейсы.

| Опцыя | Тып | Апісанне |
|--------|------|-------------|
| `title` | `string` | Загаловак акна |
| `html` | `string` | Поўны HTML-кантэнт для адлюстравання |
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
> Глядзіце [Частка 3](#part-3-building-custom-ui-with-webviews) для складаных шаблонаў webview.

#### `ui.showNotification(type, message)`

Паказаць паведамленне аб паведамленні.

| Параметр | Тып | Апісанне |
|----------|-----|----------|
| `type` | `'success' \| 'error' \| 'info'` | Стыль паведамлення |
| `message` | `string` | Тэкст для адлюстравання |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

Дадаць пастаянны тэкставы элемент у ніжнюю панэль статусу.

| Параметр | Тып | Апісанне |
|----------|-----|----------|
| `id` | `string` | Унікальны ID для гэтага элемента статусу |
| `text` | `string` | Тэкст для адлюстравання |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — Пастаяннае сховішча

Налады плагіна захоўваюцца назаўжды ў `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Чытаць захаванае значэнне.
§§§CHUNK_SEPARATOR§§§
В��ртае `undefined`, калі ключ не існуе.

#### `settings.set(key, value)`

Захаваць значэнне. Падтрымлівае радкі, лікі, булевыя значэнні, масівы і аб'екты.
§§§CHUNK_SEPARATOR§§§
**Прыклад: Запомніць перавагі карыстальніка**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — Інтэграцыя AI

> **Статус: Скора** — API AI вызначаны, але яшчэ не падключаны да Soomy. У цяперашні час вяртае `{ response: 'AI not yet connected' }`. Поўная інтэграцыя AI запланавана на будучы выпуск.

#### `ai.chat(messages, options?)`

Адправіць паведамленні AI памочніку (Soomy).
§§§CHUNK_SEPARATOR§§§
---

## Частка 3: Стварэнне карыстацкага інтэрфейсу з Webviews

API `openWebview()` дазваляе вам ствараць інтэрфейсы панэлі кіравання з HTML, CSS і JavaScript — усё ў акне ўсплывання.

> **Важнае абмежаванне:** Webviews з'яўляюцца **толькі для адлюстравання**. Яны не могуць вы��лікаць API плагіна (`ctx.settings`, `ctx.terminal` і г.д.). Выкарыстоўвайце кнопкі ў бакавой панэлі для ўсіх дзеянняў карыстальніка і выкарыстоўвайце `openWebview()`, каб адлюстраваць бягучы стан. Калі вам патрэбны інтэрактыўныя функцыі, актывуйце іх з кнопак у бакавой панэлі і паўторна адкрыйце webview, каб абнавіць адлюстраванне.

### Шаблон: Каманда тэрмінала → Парсіць вывад → Паказаць у HTML

Гэта найбольш распаўсюджаны шаблон плагіна. Вы запускаеце каманду, парсеце вынік і візуальна адлюстроўваеце яго.
§§§CHUNK_SEPARATOR§§§
### Шаблон: Інтэрактыўная панэль кіравання з аўтаматычным абнаўленнем
§§§CHUNK_SEPARATOR§§§
### Шаблон: Адлюстраванне налад у webview

> **Заўвага:** Webviews з'яўляюцца толькі для адлюстравання — яны не могуць выклікаць API плагіна. Выкарыстоўвайце `ctx.settings` у вашых апрацоўшчыках кнопак у бакавой панэлі, каб змяняць налады, і выкарыстоўвайце `openWebview()`, каб паказаць бягучы стан.
§§§CHUNK_SEPARATOR§§§
---

## Частка 4: Публікацыя вашага плагіна

### Крок 1: Тэстуйце лакальна

1. Скапіруйце ваш плагін у `~/.wia-soom/plugins/{your-plugin}/`
2. Перазапусціце WIA SOOM
3. Праверце, што ўсё працуе: кнопка ў бакавой панэлі з'явіцца, функцыі будуць працаваць правільна
4. Тэстуйце крайнія выпадкі: што адбываецца, калі тэрмінал не падключаны?

### Крок 2: Падрыхтуйцеся да адпраўкі

Ваш каталог плагіна павінен утрымліваць:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
**Патрабаваныя палі `package.json`:**

| Поле | Апісанне | Прыклад |
|-------|-------------|---------|
| `name` | Унікальны ID у kebab-case | `"my-awesome-plugin"` |
| `version` | Сэмантычная версія | `"1.0.0"` |
| `description` | Апісанне ў адным радку | `"Маніторыць доступ да лог-файлаў nginx у рэальным часе"` |
| `author` | Ваша імя | `"John Doe"` |
| `main` | Пункт уваходу | `"index.js"` |

**Дадатковыя палі:**

| Поле | Апісанне |
|-------|-------------|
| `license` | Тып ліцэнзіі (рекомендуецца MIT) |
| `keywords` | Масіў пошукавых тэгаў |
| `soom.minVersion` | Мінімальная версія WIA SOOM, якая патрабуецца |

### Крок 3: Падаць у Рэестр Плагінаў

1. **Скапіруйце** [Plugin Store](https://wiasoom.com)
2. **Дадайце** ваш плагін у `plugins/{your-plugin-name}/`
3. **Падайце** Pull Request

### Крок 4: Праверка і зацвярджэнне

Мы правяраем кожны плагін на:

- **Бяспеку** — няма небяспечных API (гл. [Правілы бяспекі](#security-rules))
- **Якасць** — ці працуе ён? Ці чысты код?
- **Карыснасць** — ці вырашае ён рэальную праблему?

Пасля зацвярджэння:
1. Ваш плагін дадаецца ў `registry.json`
2. Ствараецца ZIP-архіў у `dist/`
3. Ваш плагін з'явіцца ў **Plugin Store** для ўсіх карыстальнікаў WIA SOOM!

---

## Часць 5: Лепшыя практыкі

### Правілы бяспекі

Гэтыя правілы з'яўляюцца **абавязковымі**. Плагіны, якія іх парушаюць, будуць адхілены.

| Правіла | Чаму |
|------|-----|
| **НІКОЛІ** не выкарыстоўвайце `eval()` або `new Function()` | Рызыка ўколу кода |
| **НІКОЛІ** не выкарыстоўвайце `child_process`, `exec()`, `spawn()` | Выкарыстоўвайце толькі `ctx.terminal.send()` для каманд |
| **НІКОЛІ** не запытвайце знешнія URL | Выключэнне: API канцы `wiasoom.com` |
| **НІКОЛІ** не атрымлівайце доступ да `process.env` | Пераменныя асяроддзя могуць утрымліваць сакрэты |
| **НІКОЛІ** не выкарыстоўвайце `require('fs')` непасрэдна | Выкарыстоўвайце `ctx.settings` для захоўвання, `ctx.sftp` для перадачы файлаў |
| **НІКОЛІ** не выкарыстоўвайце знешнія пакеты npm | Толькі чысты JavaScript — без node_modules |
| **ПАВІННЫ** выкарыстоўваць `ctx.terminal.send()` для ўсіх аддаленых каманд | Гэта адбываецца праз бяспечны SSH-канал |
| **ПАВІННЫ** ачысціць у `deactivate()` | Выдаліць слухачоў, ачысціць інтэрвалы |

### Апрацоўка памылак

Заўсёды абгортвайце рызыкоўныя аперацыі ў try/catch:
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
### Ачыстка ў deactivate()

Калі ваш плагін стварае інтэрвалы, слухачоў або падп��скі — ачысціце іх:
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
### Падтрымка i18n

WIA SOOM падтрымлівае 254 мовы. Каб зрабіць мітку вашага плагіна перакладной, выкарыстоўвайце просты падыход:
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Часць 6: Рэальныя прыклады

### Прыклад 1: Праверка дыска сервера

Запускае `df -h` на аддаленым серверы і паказвае выкарыстаную/даступную прастору ў статусным радку.
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

### Прыклад 2: Менеджар TODO

Плагін, які кіруе спісам TODO, выкарыстоўваючы налады для пастаяннага захоўвання і веб-прагляд для адлюстравання.

> **Шаблон дызайну:** Паколькі веб-прагляды не могуць непасрэдна выклікаць API плагінаў, гэты плагін выкарыстоўвае падыход "снапшот" — ён счытвае TODO з налад, адлюстроўвае іх як HTML толькі для чытання і прапануе дзеянні на аснове ба��авой панэлі для дадавання элементаў. Веб-прагляд з'яўляецца **адлюстрацыйным** пластом, а не інтэрактыўнай формай.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

### Прыклад 3: Манітор памылак

Маніторыць вывад тэрмінала і адпраўляе паведамленне, калі выяўлены пэўныя шаблоны.
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

## Дадатак: Катэгорыі і Іконкі

### Катэгорыі Плагінаў (29)

Выкарыстоўвайце гэтыя ў вашым `package.json` `keywords` або пры падачы ў рэестр:

| Катэгорыя | Апісанне |
|-----------|----------|
| `server` | Агульнае кіраванне серверам |
| `devtools` | Інструменты распрацоўкі |
| `calculator` | Калькулятары і пераўтваральнікі |
| `simulator` | Сімулятары |
| `game` | Тэрмінальныя гульні |
| `business` | Бізнес-інструменты |
| `security` | Бяспека і аўдыт |
| `web` | Кіраванне вэб-серверам |
| `education` | Адукацыйныя інструменты |
| `health` | Інструменты, звязаныя са здароўем |
| `islamic` | Ісламскія інструменты (часы малітвы і г.д.) |
| `science` | Навуковыя інструменты |
| `quantum` | Інструменты квантавай камп'ютарнай тэхнікі |
| `ai` | Інструменты на базе ШІ |
| `biotech` | Інструменты біятэхналогій |
| `space` | Інструменты космасу і астранаміі |
| `network` | Сеткавыя інструменты |
| `database` | Кіраванне базамі даных |
| `monitoring` | Маніторынг сервера |
| `devops` | DevOps і CI/CD |
| `utility` | Агульныя ўтыліты |
| `design` | Інструменты дызайну |
| `ecommerce` | Інструменты электроннай камерцыі |
| `automation` | Інструменты аўтаматызацыі |
| `kpop` | Інструменты, звязаныя з K-pop |
| `accessibility` | Інструменты даступнасці |
| `analytics` | Аналітыка і справаздачы |
| `wia` | Інструменты экосістэмы WIA |
| `all` | З'яўляецца ва ўсіх катэгорыях |

### Рекомендаваные Іконкі (Lucide)

| Назва Іконкі | Выкарыстоўвайце для |
|---------------|-------------------|
| `server` | Кіраванне серверам |
| `shield` | Бяспека |
| `database` | База даных |
| `activity` | Маніторынг |
| `terminal` | Тэрмінальныя інструменты |
| `code` | Распрацоўка |
| `hard-drive` | Дыск/сховішча |
| `network` | Сеткі |
| `lock` | Аўтэнтыфікацыя/шыфраванне |
| `eye` | Нагляд/маніторынг |
| `check-square` | Задачы/TODO |
| `layout-dashboard` | Панэлі кіравання |
| `settings` | Канфігурацыя |
| `zap` | Аўтаматызацыя |
| `globe` | Вэб/міжнародны |

Праглядзіце ўсе 1,500+ ��кон: [lucide.dev/icons](https://lucide.dev/icons)

---

## Патрэбна дапамога?

- **Праблемы на GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Праблемы з плагінамі:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Прыклад плагінаў:** [Website](https://wiasoom.com)
- **Сайт:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Стварыце нешта цудоўнае. Падзяліцеся гэтым з светам.</em></p>
<p align="center"><em>— Каманда WIA SOOM</em></p>
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
