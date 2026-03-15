<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Panduan Pamekar Plugin WIA SOOM</h1>
<p align="center"><strong>Ngawangun plugin anjeun sorangan dina 5 menit.</strong></p>
<p align="center">Ngadamel alat server anu kuat, dasbor, sareng otomatisasi — langsung di jero WIA SOOM.</p>

---

## Daptar Eusi

- [Bagian 1: Mimitian Gancang — Plugin Pertama Anjeun dina 5 Menit](#bagian-1-mimitian-gancang--plugin-pertama-anjeun-dina-5-menit)
- [Bagian 2: Rujukan API Konteks Plugin](#bagian-2-rujukan-api-konteks-plugin)
  - [ctx.terminal](#ctxterminal--ngajalankeun-paréntah-di-server-jarak-jauh)
  - [ctx.sftp](#ctxsftp--transfer-file)
  - [ctx.ui](#ctxui--antarmuka-pamaké)
  - [ctx.settings](#ctxsettings--panyimpenan-permanén)
  - [ctx.ai](#ctxai--integrasi-ai)
- [Bagian 3: Ngawangun UI Kustom nganggo Webviews](#bagian-3-ngawangun-ui-kustom-nganggo-webviews)
- [Bagian 4: Menerbitkeun Plugin Anjeun](#bagian-4-menerbitkeun-plugin-anjeun)
- [Bagian 5: Prakték Terbaik](#bagian-5-prakték-terbaik)
- [Bagian 6: Conto Dunya Nyata](#bagian-6-conto-dunya-nyata)
- [Lampiran: Kategori & Ikon](#lampiran-kategori--ikon)

---

## Bagian 1: Mimitian Gancang — Plugin Pertama Anjeun dina 5 Menit

### Naon anu bakal anjeun bangun

Plugin "Hello World" anu nambahkeun tombol ka sidebar. Nalika diklik, éta bakal nunjukkeun béwara.

### Léngkah 1: Jieun folder plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Léngkah 2: Jieun package.json
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
**Lapangan anu diperyogikeun:** `name`, `version`, `description`, `author`, `main`

### Léngkah 3: Jieun index.js
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
### Léngkah 4: Balikan deui WIA SOOM

Balikan aplikasi (atawa ganti plugin ka off/on di Setélan → Plugins).

Anjeun kedah ningali tombol **"Hello World"** di sidebar. Klik éta — anjeun bakal ningali béwara kasuksesan!

### Kumaha cara gawéna
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

## Bagian 2: Rujukan API Konteks Plugin

Nalika fungsi `activate(context)` anjeun dipanggil, `context` (atawa `ctx`) nyayogikeun API ieu:
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

### `ctx.terminal` — Ngajalankeun paréntah di server jarak jauh

#### `terminal.send(sessionId, data)`

Kirim paréntah (atawa data naon waé) ka sesi terminal anu aktif.

| Parameter | Tipe | Pedaran |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal anu bakal dikirim |
| `data` | `string` | Paréntah atanapi data anu bakal dikirim |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Langganan ka sadaya keluaran ti sesi terminal. Ngabalikeun **fungsi unsubscribe**.

| Parameter | Tipe | Pedaran |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal anu bakal diawasan |
| `callback` | `(data: string) => void` | Dipanggil kalayan unggal potongan keluaran |
| **Ngabalikeun** | `() => void` | Panggil ieu pikeun eureun ngadangu |
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
**Penting:** Salawasna simpen fungsi unsubscribe sareng panggil éta di `deactivate()` pikeun nyegah bocoran mémori.

---

### `ctx.sftp` — Transfer file

> **Status: Bakal Datang** — API SFTP parantos ditetepkeun tapi acan disambungkeun ka mesin SFTP aplikasi. `list()` ayeuna ngabalikeun array kosong, sareng `upload()`/`download()` henteu ngalakukeun nanaon. Ieu bakal dilaksanakeun sacara lengkep dina rilis anu bakal datang. Pikeun ayeuna, anggo `ctx.terminal.send()` nganggo paréntah `scp` atanapi `rsync` salaku solusi.

#### `sftp.list(sessionId, path)`

Daptar file di diréktori jarak jauh.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Unggah file ti mesin lokal ka server jarak jauh.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Unduh file ti server jarak jauh ka mesin lokal.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solusi (dugi ka API SFTP aktif):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Antarmuka pamaké

#### `ui.addSidebarButton(options)`

Tambahkeun tombol ka sidebar WIA SOOM.

| Pilihan | Tipe | Diperyogikeun | Pedaran |
|--------|------|----------|-------------|
| `id` | `string` | Henteu | ID unik (standar kana ngaran plugin) |
| `icon` | `string` | Leres | Ngaran ikon Lucide (contona, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Leres | Teks tombol anu ditingalikeun di sidebar |
| `onClick` | `() => void` | Leres | Fungsi anu dipanggil nalika tombol diklik |
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
**Rujukan ikon:** Jelajah sadaya ikon anu sayogi di [lucide.dev/icons](https://lucide.dev/icons)

> **Catetan kasaluyuan:** Sababaraha plugin heubeul nganggo argumen posisi sapertos `addSidebarButton(id, icon, label, onClick)`. API resmi nganggo **objék pilihan** sapertos anu didokumentasikeun di luhur. Salawasna anggo gaya objék pikeun plugin anyar.

#### `ui.openWebview(options)`

Buka jandela popup kalayan kontén HTML kustom. Ieu cara anjeun ngawangun UI anu beunghar.

| Pilihan | Tipe | Pedaran |
|--------|------|-------------|
| `title` | `string` | Judul jandela |
| `html` | `string` | Kontén HTML lengkep pikeun dirender |
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
> Tingali [Bagian 3](#part-3-building-custom-ui-with-webviews) pikeun pola webview anu langkung canggih.

#### `ui.showNotification(type, message)`

Tembongkeun notifikasi toast.

| Parameter | Tipe | Pedaran |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Gaya notifikasi |
| `message` | `string` | Teks anu bakal ditembongkeun |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Tambihkeun item téks anu permanén ka status bar di handap.

| Parameter | Tipe | Pedaran |
|-----------|------|-------------|
| `id` | `string` | ID unik pikeun item status ieu |
| `text` | `string` | Teks anu bakal ditampilkeun |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Penyimpanan permanén

Setélan plugin disimpen sacara permanén di `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Maca nilai anu disimpen.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Bakal ngabalikeun `undefined` lamun konci teu aya.

#### `settings.set(key, value)`

Simpen nilai. Ngarojong string, angka, boolean, array, sareng objek.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Conto: Émut preferensi pangguna**
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

### `ctx.ai` — Integrasi AI

> **Status: Bakal Datang** — API AI parantos ditangtukeun tapi acan nyambung ka Soomy. Ayeuna ngabalikeun `{ response: 'AI not yet connected' }`. Integrasi AI lengkep direncanakeun pikeun rilis anu bakal datang.

#### `ai.chat(messages, options?)`

Kirim pesen ka asisten AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Bagian 3: Ngawangun UI Kustom sareng Webviews

API `openWebview()` ngamungkinkeun anjeun ngawangun UI dasbor nganggo HTML, CSS, sareng JavaScript — sadayana di jero jandela popup.

> **Batasan penting:** Webviews nyaéta **hanya tampilan**. Aranjeunna henteu tiasa nelepon deui ka API plugin (`ctx.settings`, `ctx.terminal`, jsb.). Anggo tombol sidebar pikeun sadaya tindakan pangguna, sareng anggo `openWebview()` pikeun nembongkeun kaayaan ayeuna. Lamun anjeun peryogi fitur interaktif, picu aranjeunna ti tombol sidebar sareng buka deui webview pikeun ngarefresh tampilan.

### Pola: Paréntah Terminal → Parse Kaluaran → Tembongkeun dina HTML

Ieu mangrupikeun pola plugin anu paling umum. Anjeun ngajalankeun paréntah, nganalisis hasilna, sareng nembongkeunana sacara visual.
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
### Pola: Dasbor Interaktif sareng Auto-Refresh
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
### Pola: Nembongkeun Setélan dina Webview

> **Catetan:** Webviews nyaéta hanya tampilan — aranjeunna henteu tiasa nelepon deui ka API plugin. Anggo `ctx.settings` dina handler tombol sidebar anjeun pikeun ngarobih setélan, sareng anggo `openWebview()` pikeun nembongkeun kaayaan ayeuna.
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

## Bagian 4: Menerbitkeun Plugin Anjeun

### Léngkah 1: Uji sacara lokal

1. Salin plugin anjeun ka `~/.wia-soom/plugins/{your-plugin}/`
2. Mimitian deui WIA SOOM
3. Pastikeun éta jalan: tombol sidebar muncul, fitur jalan kalayan leres
4. Uji kasus tepi: naon anu kajadian lamun teu aya terminal anu nyambung?

### Léngkah 2: Siapkeun pikeun pengajuan

Folder plugin anjeun kedah ngandung:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Diperyogikeun `package.json` fields:**

| Field | Pedaran | Conto |
|-------|-------------|---------|
| `name` | ID unik kebab-case | `"my-awesome-plugin"` |
| `version` | Versi semantik | `"1.0.0"` |
| `description` | Pedaran hiji baris | `"Monitors nginx access logs in real-time"` |
| `author` | Nami anjeun | `"John Doe"` |
| `main` | Titik asup | `"index.js"` |

**Fields opsional:**

| Field | Pedaran |
|-------|-------------|
| `license` | Jenis lisensi (MIT disarankeun) |
| `keywords` | Array tag pencarian |
| `soom.minVersion` | Versi WIA SOOM minimum anu diperyogikeun |

### Léngkah 3: Kirim ka Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Tambahkeun** plugin anjeun ka `plugins/{nami-plugin-anjeun}/`
3. **Kirim** Pull Request

### Léngkah 4: Tinjauan sareng persetujuan

Kami marios unggal plugin pikeun:

- **Kaamanan** — teu aya API anu bahaya (tingali [Aturan Kaamanan](#security-rules))
- **Kualitas** — naha éta jalan? Naha kode na bersih?
- **Manfaat** — naha éta nyumponan masalah nyata?

Saatos persetujuan:
1. Plugin anjeun ditambahkeun ka `registry.json`
2. Bundel ZIP dijieun di `dist/`
3. Plugin anjeun muncul di **Plugin Store** pikeun sadaya pangguna WIA SOOM!

---

## Bagian 5: Prakték Terbaik

### Aturan Kaamanan

Aturan ieu **wajib**. Plugin anu ngalanggar bakal ditolak.

| Aturan | Kunaon |
|------|-----|
| **ULAH** nganggo `eval()` atanapi `new Function()` | Risiko injeksi kode |
| **ULAH** nganggo `child_process`, `exec()`, `spawn()` | Ngan nganggo `ctx.terminal.send()` pikeun paréntah |
| **ULAH** ngunduh URL eksternal | Éksépsi: titik akhir API `wiasoom.com` |
| **ULAH** ngakses `process.env` | Variabel lingkungan tiasa ngandung rahasia |
| **ULAH** nganggo `require('fs')` sacara langsung | Anggo `ctx.settings` pikeun panyimpenan, `ctx.sftp` pikeun transfer file |
| **KUDU** nganggo `ctx.terminal.send()` pikeun sadaya paréntah jarak jauh | Ieu ngalangkungan saluran SSH anu aman |
| **KUDU** ngabersihan di `deactivate()` | Hapus pendengar, bersihkan interval |

### Penanganan Kasalahan

Salawasna bungkus operasi anu berisiko dina try/catch:
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
### Bersih di deactivate()

Upami plugin anjeun nyiptakeun interval, pendengar, atanapi langganan — bersihkeun aranjeunna:
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
### Dukungan i18n

WIA SOOM ngarojong 254 basa. Pikeun ngajantenkeun label plugin anjeun tiasa ditarjamahkeun, anggo pendekatan anu basajan:
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

## Bagian 6: Conto Dunya Nyata

### Conto 1: Pamariksa Disk Server

Ngajalankeun `df -h` dina server jarak jauh sareng nunjukkeun ruang anu dianggo/aya dina bilah status.
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

### Conto 2: Manajer TODO

Plugin anu ngatur daptar TODO nganggo setélan pikeun panyimpenan anu terus-terusan sareng webview pikeun tampilan.

> **Pola desain:** Kusabab webviews henteu tiasa langsung nelepon API plugin, plugin ieu nganggo pendekatan "snapshot" — éta maca TODO ti setélan, ngagambarkeunana salaku HTML baca-wungkul, sareng nyayogikeun tindakan dumasar sidebar pikeun nambahkeun barang. Webview mangrupikeun lapisan **tampilan**, sanés formulir interaktif.
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

### Conto 3: Pengawas Kasalahan

Ngawaskeun keluaran terminal sareng ngirim béwara nalika pola khusus dideteksi.
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

## Lampiran: Kategori & Ikon

### Kategori Plugin (29)

Gunakan ieu dina `package.json` `keywords` atanapi nalika ngirim ka registry:

| Kategori | Pedaran |
|----------|-------------|
| `server` | Manajemén server umum |
| `devtools` | Alat pamekaran |
| `calculator` | Kalkulator sareng konverter |
| `simulator` | Simulator |
| `game` | Kaulinan terminal |
| `business` | Alat bisnis |
| `security` | Kaamanan sareng auditing |
| `web` | Manajemén server web |
| `education` | Alat pendidikan |
| `health` | Alat anu patali sareng kaséhatan |
| `islamic` | Alat Islam (waktu sholat, jsb.) |
| `science` | Alat ilmiah |
| `quantum` | Alat komputasi kuantum |
| `ai` | Alat anu didorong ku AI |
| `biotech` | Alat bioteknologi |
| `space` | Alat ruang angkasa sareng astronomi |
| `network` | Alat jaringan |
| `database` | Manajemén database |
| `monitoring` | Monitoring server |
| `devops` | DevOps sareng CI/CD |
| `utility` | Utilitas umum |
| `design` | Alat desain |
| `ecommerce` | Alat e-commerce |
| `automation` | Alat otomatisasi |
| `kpop` | Alat anu patali sareng K-pop |
| `accessibility` | Alat aksésibilitas |
| `analytics` | Analitik sareng laporan |
| `wia` | Alat ekosistem WIA |
| `all` | Muncul di sadaya kategori |

### Ikon Anu Disarankeun (Lucide)

| Nami Ikon | Pake pikeun |
|-----------|---------|
| `server` | Manajemén server |
| `shield` | Kaamanan |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Alat terminal |
| `code` | Pamekaran |
| `hard-drive` | Disk/panyimpenan |
| `network` | Jaringan |
| `lock` | Auth/enkripsi |
| `eye` | Nonton/monitoring |
| `check-square` | Tugas/TODO |
| `layout-dashboard` | Dasbor |
| `settings` | Konfigurasi |
| `zap` | Otomatisasi |
| `globe` | Web/internasional |

Jelajahi sadaya 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Perlu Bantosan?

- **Masalah GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Masalah Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Conto:** [Website](https://wiasoom.com)
- **Situs Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Ngawangun hal anu luar biasa. Bagikeun ka dunya.</em></p>
<p align="center"><em>— Tim WIA SOOM</em></p>