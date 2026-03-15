<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Ishlab Chiquvchilar Qo'llanmasi</h1>
<p align="center"><strong>O'z plaginini 5 daqiqada yarating.</strong></p>
<p align="center">Kuchli server vositalarini, boshqaruv panellari va avtomatlashtirishni yarating — to'g'ridan-to'g'ri WIA SOOM ichida.</p>

---

## Mazmun jadvali

- [1-qism: Tez boshlash — Birinchi plagin 5 daqiqada](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [2-qism: Plagin Kontekst API Ma'lumotnomasi](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [3-qism: Webviews bilan Maxsus UI Yaratish](#part-3-building-custom-ui-with-webviews)
- [4-qism: Plaginingizni Noshir Qilish](#part-4-publishing-your-plugin)
- [5-qism: Eng Yaxshi Amaliyotlar](#part-5-best-practices)
- [6-qism: Haqiqiy Dunyodagi Misollar](#part-6-real-world-examples)
- [Qo'shimcha: Kategoriyalar va Ikonlar](#appendix-categories--icons)

---

## 1-qism: Tez boshlash — Birinchi plagin 5 daqiqada

### Nima yaratishingiz kerak

"Hello World" plaginini, bu yon panelga tugma qo'shadi. Bosilganda, u xabarnoma ko'rsatadi.

### 1-qadam: Plagin papkasini yarating
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 2-qadam: package.json yarating
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
**Talab qilinadigan maydonlar:** `name`, `version`, `description`, `author`, `main`

### 3-qadam: index.js yarating
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
### 4-qadam: WIA SOOMni qayta ishga tushiring

Ilovani qayta ishga tushiring (yoki Sozlamalar → Plaginlar bo'limida plagin tugmasini o'chirib/yoqing).

Yon panelda **"Hello World"** tugmasini ko'rishingiz kerak. Uni bosing — muvaffaqiyatli xabarnoma ko'rasiz!

### Qanday ishlaydi
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

## 2-qism: Plagin Kontekst API Ma'lumotnomasi

`activate(context)` funksiyangiz chaqirilganda, `context` (yoki `ctx`) quyidagi APIlarni taqdim etadi:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Uzoq serverlarda buyruqlarni bajarish

#### `terminal.send(sessionId, data)`

Faol terminal sessiyasiga buyruq (yoki har qanday ma'lumot) yuborish.

| Parametr | Tur | Tavsif |
|----------|-----|--------|
| `sessionId` | `string` | Yuboriladigan terminal sessiyasi |
| `data` | `string` | Yuboriladigan buyruq yoki ma'lumot |
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
#### `terminal.onOutput(sessionId, callback)`

Terminal sessiyasidan barcha chiqishlarni obuna bo'ling. **Obunani bekor qilish funksiyasini** qaytaradi.

| Parametr | Tur | Tavsif |
|----------|-----|--------|
| `sessionId` | `string` | Kuzatiladigan terminal sessiyasi |
| `callback` | `(data: string) => void` | Har bir chiqish qismi bilan chaqiriladi |
| **Qaytaradi** | `() => void` | Buni chaqirib, tinglashni to'xtating |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**Muhim:** Har doim obunani bekor qilish funksiyasini saqlang va `deactivate()`da uni chaqiring, bu xotira oqimlarini oldini olish uchun.

---

### `ctx.sftp` — Fayl uzatish

> **Holat: Tez orada** — SFTP API belgilangan, lekin hali ilovaning SFTP dvigateliga ulanmadi. `list()` hozirda bo'sh massivni qaytaradi, va `upload()`/`download()` hech narsa qilmaydi. Bu kelajakda to'liq amalga oshiriladi. Hozirda, `ctx.terminal.send()` bilan `scp` yoki `rsync` buyruqlaridan foydalaning.

#### `sftp.list(sessionId, path)`

Uzoq katalogdagi fayllarni ro'yxatini tuzing.
§§§CHUNK_SEPARATOR§��§
#### `sftp.upload(sessionId, localPath, remotePath)`

Mahalliy kompyuterdan uzoq serverga fayl yuklang.
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
#### `sftp.download(sessionId, remotePath, localPath)`

Uzoq serverdan mahalliy kompyuterga fayl yuklab oling.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
**Muqobil (SFTP API ishga tushguncha):**
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
---

### `ctx.ui` — Foydalanuvchi interfeysi

#### `ui.addSidebarButton(options)`

WIA SOOM yon paneliga tugma qo'shing.

| Tanlov | Tur | Talab qilinadi | Tavsif |
|--------|-----|----------------|--------|
| `id` | `string` | Yo'q | Unikal ID (plagin nomiga o'rnatiladi) |
| `icon` | `string` | Ha | Lucide ikonasi nomi (masalan, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ha | Yon panelda ko'rsatiladigan tugma matni |
| `onClick` | `() => void` | Ha | Tugma bosilganda chaqiriladigan funksiya |
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Ikona ma'lumotnomasi:** Mavjud barcha ikonalarni [lucide.dev/icons](https://lucide.dev/icons) saytida ko'ring.

> **Moslik eslatmasi:** Ba'zi eski plaginlar `addSidebarButton(id, icon, label, onClick)` kabi pozitsion argumentlardan foydalanadi. Rasmiy API yuqorida hujjatlangan **options obyekti**dan foydalanadi. Yangi plaginlar uchun har doim obyekt uslubidan foydalaning.

#### `ui.openWebview(options)`

Maxsus HTML kontenti bilan pop-up oynasini oching. Bu boy UI larni yaratish usuli.

| Tanlov | Tur | Tavsif |
|--------|-----|--------|
| `title` | `string` | Oyna sarlavhasi |
| `html` | `string` | Ko'rsatish uchun to'liq HTML kontenti |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
> [3-qism](#part-3-building-custom-ui-with-webviews) da ilg'or webview naqshlari haqida ma'lumot oling.

#### `ui.showNotification(type, message)`

Toast xabarnomasini ko'rsatadi.

| Parametr | Tur | Tavsif |
|----------|-----|--------|
| `type` | `'success' \| 'error' \| 'info'` | Xabarnoma uslubi |
| `message` | `string` | Ko'rsatish uchun matn |
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
#### `ui.addStatusBarItem(id, text)`

Pastki holat paneliga doimiy matn elementini qo'shadi.

| Parametr | Tur | Tavsif |
|----------|-----|--------|
| `id` | `string` | Ushbu holat elementi uchun noyob ID |
| `text` | `string` | Ko'rsatish uchun matn |
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
---

### `ctx.settings` — Doimiy saqlash

Plugin sozlamalari doimiy ravishda `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` da saqlanadi.

#### `settings.get(key)`

Saqlangan qiymatni o'qiydi.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
Agar kalit mavjud bo'lmasa, `undefined` qaytaradi.

#### `settings.set(key, value)`

Qiymatni saqlaydi. Stringlar, raqamlar, booleanlar, massivlar va obyektlarni qo'llab-quvvatlaydi.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
**Misol: Foydalanuvchi afzalliklarini eslab qolish**
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### `ctx.ai` — AI integratsiyasi

> **Holat: Tez orada** — AI API belgilangan, lekin hali Soomy ga ulanmadi. Hozirda `{ response: 'AI not yet connected' }` qaytaradi. To'liq AI integratsiyasi kelajakda rejalashtirilgan.

#### `ai.chat(messages, options?)`

AI yordamchisiga (Soomy) xabarlar yuboradi.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## 3-qism: Webview yordamida maxsus UI yaratish

`openWebview()` API sizga HTML, CSS va JavaScript yordamida boshqaruv panellari UI larini yaratishga imkon beradi — barchasi pop-up oynasi ichida.

> **Muhim cheklov:** Webview lar **faqat ko'rsatish uchun**. Ular plugin API lariga (`ctx.settings`, `ctx.terminal` va boshqalar) qaytib chaqira olmaydi. Foydalanuvchi harakatlari uchun yon panel tugmalaridan foydalaning va joriy holatni ko'rsatish uchun `openWebview()` dan foydalaning. Agar interaktiv funksiyalar kerak bo'lsa, ularni yon panel tugmalaridan ishga tushiring va ko'rsatishni yangilash uchun webview ni qayta oching.

### Naqsh: Terminal Buyrug'i → Natijani Tahlil Qilish → HTML da Ko'rsatish

Bu eng keng tarqalgan plugin naqshidir. Siz buyruqni bajarishingiz, natijani tahlil qilishingiz va uni vizual ko'rsatishingiz kerak.
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
### Naqsh: Avtomatik Yangilanish bilan Interaktiv Boshqaruv Paneli
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Naqsh: Webview da Sozlamalarni Ko'rsatish

> **Eslatma:** Webview lar faqat ko'rsatish uchun — ular plugin API lariga qaytib chaqira olmaydi. Sozlamalarni o'zgartirish uchun yon panel tugmalari ishlovchilarida `ctx.settings` dan foydalaning va joriy holatni ko'rsatish uchun `openWebview()` dan foydalaning.
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

## 4-qism: Pluginingizni nashr etish

### 1-qadam: Mahalliy sinov

1. Pluginingizni `~/.wia-soom/plugins/{your-plugin}/` ga nusxalash
2. WIA SOOM ni qayta ishga tushirish
3. Ishlayotganini tekshirish: yon panel tugmasi paydo bo'ladi, funksiyalar to'g'ri ishlaydi
4. Chegaraviy holatlarni sinab ko'ring: terminal ulanishi bo'lmasa nima bo'ladi?

### 2-qadam: Taqdimotga tayyorgarlik

Plugin papkangiz quyidagilarni o'z ichiga olishi kerak:
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
**Talab qilinadigan `package.json` maydonlari:**

| Maydon | Tavsif | Misol |
|--------|--------|-------|
| `name` | Yagona kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantik versiya | `"1.0.0"` |
| `description` | Bir qatorli tavsif | `"Monitors nginx access logs in real-time"` |
| `author` | Sizning ismingiz | `"John Doe"` |
| `main` | Kirish nuqtasi | `"index.js"` |

**Ixtiyoriy maydonlar:**

| Maydon | Tavsif |
|--------|--------|
| `license` | Litsenziya turi (MIT tavsiya etiladi) |
| `keywords` | Qidiruv teglarining massivi |
| `soom.minVersion` | Talab qilinadigan minimal WIA SOOM versiyasi |

### 3-qadam: Plugin Ro'yxatiga yuborish

1. ****Package** your plugin as a ZIP file
2. **Qo'shing** o'z pluginingizni `plugins/{your-plugin-name}/`
3. **Yuboring** Pull Request

### 4-qadam: Ko'rib chiqish va tasdiqlash

Biz har bir plaginni quyidagilar uchun ko'rib chiqamiz:

- **Xavfsizlik** — xavfli API-lar yo'q (qarang [Xavfsizlik Qoidalari](#security-rules))
- **Sifat** — u ishlayaptimi? Kod toza mi?
- **Foydalilik** — u haqiqiy muammoni hal qiladimi?

Tasdiqlangandan so'ng:
1. Sizning pluginingiz `registry.json` ga qo'shiladi
2. `dist/` da ZIP to'plami yaratiladi
3. Sizning pluginingiz **Plugin Store** da barcha WIA SOOM foydalanuvchilari uchun ko'rinadi!

---

## 5-qism: Eng yaxshi amaliyotlar

### Xavfsizlik Qoidalari

Ushbu qoidalar **majburiydir**. Ularni buzadigan plaginlar rad etiladi.

| Qoidalar | Nima uchun |
|----------|------------|
| **HECH QACHON** `eval()` yoki `new Function()` dan foydalanmang | Kodni kiritish xavfi |
| **HECH QACHON** `child_process`, `exec()`, `spawn()` dan foydalanmang | Faqat `ctx.terminal.send()` dan buyruqlar uchun foydalaning |
| **HECH QACHON** tashqi URL-larni chaqirmang | Istisno: `wiasoom.com` API nuqtalari |
| **HECH QACHON** `process.env` ga kirishmang | Atrof-muhit o'zgaruvchilari sirlarni o'z ichiga olishi mumkin |
| **HECH QACHON** `require('fs')` ni to'g'ridan-to'g'ri ishlatmang | Saqlash uchun `ctx.settings` dan, fayl uzatish uchun `ctx.sftp` dan foydalaning |
| **HECH QACHON** npm tashqi paketlardan foydalanmang | Faqat toza JavaScript — node_modules yo'q |
| **SHART** barcha masofaviy buyruqlar uchun `ctx.terminal.send()` dan foydalaning | Bu xavfsiz SSH kanali orqali o'tadi |
| **SHART** `deactivate()` da tozalang | Tinglovchilarni olib tashlang, intervalni tozalang |

### Xato Boshqaruvi

Har doim xavfli operatsiyalarni try/catch ichiga o'rab oling:
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
### deactivate() da tozalash

Agar sizning pluginingiz interval, tinglovchilar yoki obunalarni yaratadigan bo'lsa — ularni tozalang:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### i18n Qo'llab-quvvatlash

WIA SOOM 254 tilni qo'llab-quvvatlaydi. Pluginingiz etiketkasini tarjima qilish uchun oddiy yondashuvdan foydalaning:
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

## 6-qism: Haqiqiy misollar

### Misol 1: Server Disk Tekshirgichi

Masofaviy serverda `df -h` ni ishga tushiradi va holat panelida ishlatilgan/mavjud joyni ko'rsatadi.
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

### Misol 2: TODO Boshqaruvchisi

Doimiy saqlash uchun sozlamalardan foydalanadigan va ko'rsatish uchun webview ishlatadigan TODO ro'yxatini boshqaruvchi plagin.

> **Dizayn naqsh:** Webview-lar to'g'ridan-to'g'ri plagin API-larini chaqira olmasligi sababli, ushbu plagin "snapshot" yondashuvini qo'llaydi — u TODO-larni sozlamalardan o'qiydi, ularni o'qish uchun HTML sifatida chizadi va elementlarni qo'shish uchun yon panel asosidagi harakatlarni taqdim etadi. Webview — bu **ko'rsatish** qatlami, interaktiv shakl emas.
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

### Misol 3: Xato Kuzatuvchi

Terminal chiqishini kuzatadi va ma'lum naqshlar aniqlanganda bildirishnoma yuboradi.
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

## Qo'shimcha: Kategoriyalar va Ikonkalar

### Plugin Kategoriyalari (29)

Ushbu kategoriyalarni `package.json` `keywords` da yoki registrga yuborishda foydalaning:

| Kategoriya | Tavsif |
|------------|--------|
| `server` | Umumiy server boshqaruvi |
| `devtools` | Rivojlantirish vositalari |
| `calculator` | Hisoblagichlar va konvertorlar |
| `simulator` | Simulyatorlar |
| `game` | Terminal o'yinlari |
| `business` | Biznes vositalari |
| `security` | Xavfsizlik va audit |
| `web` | Veb server boshqaruvi |
| `education` | Ta'lim vositalari |
| `health` | Sog'liq bilan bog'liq vositalar |
| `islamic` | Islomiy vositalar (duo vaqtlari va boshqalar) |
| `science` | Ilmiy vositalar |
| `quantum` | Kvant hisoblash vositalari |
| `ai` | AI asosidagi vositalar |
| `biotech` | Biotexnologiya vositalari |
| `space` | Kosmos va astronomiya vositalari |
| `network` | Tarmoq vositalari |
| `database` | Ma'lumotlar bazasini boshqarish |
| `monitoring` | Server monitoringi |
| `devops` | DevOps va CI/CD |
| `utility` | Umumiy yordamchi vositalar |
| `design` | Dizayn vositalari |
| `ecommerce` | E-commerce vositalari |
| `automation` | Avtomatlashtirish vositalari |
| `kpop` | K-pop bilan bog'liq vositalar |
| `accessibility` | Mavjudlik vositalari |
| `analytics` | Tahlil va hisobot |
| `wia` | WIA ekotizim vositalari |
| `all` | Barcha kategoriyalarda mavjud |

### Tavsiya etilgan Ikonkalar (Lucide)

| Ikonka Nomi | Foydalanish uchun |
|--------------|-------------------|
| `server` | Server boshqaruvi |
| `shield` | Xavfsizlik |
| `database` | Ma'lumotlar bazasi |
| `activity` | Monitoring |
| `terminal` | Terminal vositalari |
| `code` | Rivojlantirish |
| `hard-drive` | Disk/xotira |
| `network` | Tarmoqlash |
| `lock` | Avtorizatsiya/shifrlash |
| `eye` | Kuzatish/monitoring |
| `check-square` | Vazifalar/TODO |
| `layout-dashboard` | Dashbordlar |
| `settings` | Konfiguratsiya |
| `zap` | Avtomatlashtirish |
| `globe` | Veb/xalqaro |

Barcha 1,500+ ikonkalarga qarang: [lucide.dev/icons](https://lucide.dev/icons)

---

## Yordam kerakmi?

- **GitHub Muammolari:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Muammolari:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Misol Pluginlar:** [Website](https://wiasoom.com)
- **Vebsayt:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Ajablanarli bir narsa yarating. Buni dunyo bilan baham ko'ring.</em></p>
<p align="center"><em>— WIA SOOM Jamoasi</em></p>
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
