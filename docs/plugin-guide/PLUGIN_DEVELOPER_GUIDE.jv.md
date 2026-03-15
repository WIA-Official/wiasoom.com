<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Pandhuan Pengembang Plugin WIA SOOM</h1>
<p align="center"><strong>Bangun plugin sampeyan dhewe ing 5 menit.</strong></p>
<p align="center">Gawe alat server sing kuat, dasbor, lan automasi — langsung ing WIA SOOM.</p>

---

## Daftar Isi

- [Bagian 1: Miwiti Cepet — Plugin Pertama Sampeyan ing 5 Menit](#bagian-1-miwiti-cepet--plugin-pertama-sampeyan-ing-5-menit)
- [Bagian 2: Referensi API Konteks Plugin](#bagian-2-referensi-api-konteks-plugin)
  - [ctx.terminal](#ctxterminal--nglakokake-perintah-ing-server-remote)
  - [ctx.sftp](#ctxsftp--transfer-file)
  - [ctx.ui](#ctxui--antarmuka-pengguna)
  - [ctx.settings](#ctxsettings--penyimpanan-berkelanjutan)
  - [ctx.ai](#ctxai--integrasi-ai)
- [Bagian 3: Mbangun UI Kustom nganggo Webviews](#bagian-3-mbangun-ui-kustom-nganggo-webviews)
- [Bagian 4: Menerbitkan Plugin Sampeyan](#bagian-4-menerbitkan-plugin-sampeyan)
- [Bagian 5: Praktik Terbaik](#bagian-5-praktik-terbaik)
- [Bagian 6: Contoh Dunia Nyata](#bagian-6-contoh-dunia-nyata)
- [Lampiran: Kategori & Ikon](#lampiran-kategori--ikon)

---

## Bagian 1: Miwiti Cepet — Plugin Pertama Sampeyan ing 5 Menit

### Apa sing bakal sampeyan bangun

Plugin "Hello World" sing nambah tombol ing sidebar. Nalika diklik, bakal nuduhake notifikasi.

### Langkah 1: Gawe folder plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Langkah 2: Gawe package.json
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
**Bidang sing dibutuhake:** `name`, `version`, `description`, `author`, `main`

### Langkah 3: Gawe index.js
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
### Langkah 4: Restart WIA SOOM

Restart aplikasi (utawa ganti plugin mati/hidup ing Setelan → Plugin).

Sampeyan kudu ndeleng tombol **"Hello World"** ing sidebar. Klik — sampeyan bakal ndeleng notifikasi sukses!

### Kepiye cara kerjane
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

## Bagian 2: Referensi API Konteks Plugin

Nalika fungsi `activate(context)` sampeyan diarani, `context` (utawa `ctx`) nyedhiyakake API iki:
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

### `ctx.terminal` — Nggawe perintah ing server remote

#### `terminal.send(sessionId, data)`

Kirim perintah (utawa data apa wae) menyang sesi terminal aktif.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal kanggo dikirim |
| `data` | `string` | Perintah utawa data kanggo dikirim |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Langganan kabeh output saka sesi terminal. Ngasilake **fungsi batal langganan**.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal kanggo dipantau |
| `callback` | `(data: string) => void` | Diarani kanthi saben potongan output |
| **Ngasilake** | `() => void` | Telpon iki kanggo mandheg ngrungokake |
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
**Penting:** Selalu simpen fungsi batal langganan lan telpon ing `deactivate()` kanggo nyegah kebocoran memori.

---

### `ctx.sftp` — Transfer file

> **Status: Bakal Datang** — API SFTP wis ditemtokake nanging durung disambung menyang mesin SFTP aplikasi. `list()` saiki ngasilake array kosong, lan `upload()`/`download()` ora nindakake apa-apa. Iki bakal dilakoni kanthi lengkap ing rilis sabanjure. Saiki, gunakake `ctx.terminal.send()` nganggo perintah `scp` utawa `rsync` minangka solusi.

#### `sftp.list(sessionId, path)`

Dhaptar file ing direktori remote.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Unggah file saka mesin lokal menyang server remote.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Unduh file saka server remote menyang mesin lokal.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solusi (nganti API SFTP aktif):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Antarmuka pengguna

#### `ui.addSidebarButton(options)`

Tambah tombol ing sidebar WIA SOOM.

| Pilihan | Tipe | Diperlukan | Deskripsi |
|---------|------|------------|-------------|
| `id` | `string` | Ora | ID unik (standar dadi jeneng plugin) |
| `icon` | `string` | Ya | Jeneng ikon Lucide (contone, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ya | Teks tombol sing ditampilake ing sidebar |
| `onClick` | `() => void` | Ya | Fungsi sing diarani nalika tombol diklik |
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
**Referensi ikon:** Telusuri kabeh ikon sing kasedhiya ing [lucide.dev/icons](https://lucide.dev/icons)

> **Cathetan kompatibilitas:** Sawetara plugin lawas nggunakake argumen posisi kaya `addSidebarButton(id, icon, label, onClick)`. API resmi nggunakake **obyek pilihan** kaya sing didokumentasikake ing ndhuwur. Selalu gunakake gaya obyek kanggo plugin anyar.

#### `ui.openWebview(options)`

Mbukak jendela pop-up kanthi konten HTML kustom. Iki carane sampeyan mbangun UI sing sugih.

| Pilihan | Tipe | Deskripsi |
|---------|------|-------------|
| `title` | `string` | Judhul jendela |
| `html` | `string` | Konten HTML lengkap kanggo dirender |
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
> Deleng [Bagian 3](#part-3-building-custom-ui-with-webviews) kanggo pola webview sing luwih maju.

#### `ui.showNotification(type, message)`

Tampilake notifikasi toast.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Gaya notifikasi |
| `message` | `string` | Teks kanggo ditampilake |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Tambahake item teks sing tetep ing bar status ngisor.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `id` | `string` | ID unik kanggo item status iki |
| `text` | `string` | Teks kanggo ditampilake |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Penyimpanan permanen

Setelan plugin disimpen sacara permanen ing `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Maca nilai sing disimpen.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Bakal bali `undefined` yen kunci ora ana.

#### `settings.set(key, value)`

Simpen nilai. Ndukung string, angka, boolean, array, lan objek.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Conto: Elinga preferensi pangguna**
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

> **Status: Bakal Datang** — API AI wis ditemtokake nanging durung nyambung menyang Soomy. Saiki bali `{ response: 'AI not yet connected' }`. Integrasi AI lengkap direncanakake kanggo rilis ing mangsa ngarep.

#### `ai.chat(messages, options?)`

Kirim pesen menyang asisten AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Bagian 3: Mbangun UI Kustom nganggo Webviews

API `openWebview()` ngidini sampeyan mbangun UI dasbor nganggo HTML, CSS, lan JavaScript — kabeh ing jendela popup.

> **Batasan penting:** Webviews iku **mung tampilan**. Dheweke ora bisa nelpon maneh menyang API plugin (`ctx.settings`, `ctx.terminal`, lsp.). Gunakake tombol sidebar kanggo kabeh aksi pangguna, lan gunakake `openWebview()` kanggo nampilake status saiki. Yen sampeyan butuh fitur interaktif, picu saka tombol sidebar lan mbukak maneh webview kanggo nganyari tampilan.

### Pola: Perintah Terminal → Parse Output → Tampilake ing HTML

Iki minangka pola plugin sing paling umum. Sampeyan mbukak perintah, parse asil, lan nampilake kanthi visual.
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
### Pola: Dasbor Interaktif kanthi Auto-Refresh
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
### Pola: Nampilake Setelan ing Webview

> **Cathetan:** Webviews iku mung tampilan — dheweke ora bisa nelpon maneh menyang API plugin. Gunakake `ctx.settings` ing handler tombol sidebar sampeyan kanggo ngowahi setelan, lan gunakake `openWebview()` kanggo nampilake status saiki.
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

## Bagian 4: Nerbitake Plugin Sampeyan

### Langkah 1: Uji lokal

1. Salin plugin sampeyan menyang `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verifikasi manawa bisa digunakake: tombol sidebar muncul, fitur bisa digunakake kanthi bener
4. Uji kasus tepi: apa sing kedadeyan yen ora ana terminal sing nyambung?

### Langkah 2: Siap kanggo pengajuan

Folder plugin sampeyan kudu ngemot:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Diperlukan `package.json` fields:**

| Field | Deskripsi | Contoh |
|-------|-------------|---------|
| `name` | ID unik kebab-case | `"my-awesome-plugin"` |
| `version` | Versi semantik | `"1.0.0"` |
| `description` | Deskripsi satu baris | `"Monitors nginx access logs in real-time"` |
| `author` | Jenengmu | `"John Doe"` |
| `main` | Titik masuk | `"index.js"` |

**Fields opsional:**

| Field | Deskripsi |
|-------|-------------|
| `license` | Jenis lisensi (MIT dianjurkan) |
| `keywords` | Array tag pencarian |
| `soom.minVersion` | Versi WIA SOOM minimal yang dibutuhkan |

### Langkah 3: Kirim ke Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Tambah** pluginmu ing `plugins/{your-plugin-name}/`
3. **Kirim** Pull Request

### Langkah 4: Tinjauan lan persetujuan

Kita mriksa saben plugin kanggo:

- **Keamanan** — ora ana API berbahaya (deleng [Aturan Keamanan](#security-rules))
- **Kualitas** — apa iki bisa digunakake? Apa kode kasebut resik?
- **Kegunaan** — apa iki ngrampungake masalah nyata?

Sawise disetujoni:
1. Pluginmu ditambahake ing `registry.json`
2. Bundel ZIP digawe ing `dist/`
3. Pluginmu muncul ing **Plugin Store** kanggo kabeh pangguna WIA SOOM!

---

## Bagian 5: Praktik Terbaik

### Aturan Keamanan

Aturan iki **wajib**. Plugin sing nglanggar bakal ditolak.

| Aturan | Kenapa |
|------|-----|
| **ORA PERNAH** nggunakake `eval()` utawa `new Function()` | Risiko injeksi kode |
| **ORA PERNAH** nggunakake `child_process`, `exec()`, `spawn()` | Mung nggunakake `ctx.terminal.send()` kanggo perintah |
| **ORA PERNAH** njupuk URL eksternal | Pengecualian: titik akhir API `wiasoom.com` |
| **ORA PERNAH** ngakses `process.env` | Variabel lingkungan bisa ngemot rahasia |
| **ORA PERNAH** nggunakake `require('fs')` langsung | Gunakake `ctx.settings` kanggo panyimpenan, `ctx.sftp` kanggo transfer file |
| **ORA PERNAH** nggunakake paket eksternal npm | Mung JavaScript murni — ora ana node_modules |
| **HARUS** nggunakake `ctx.terminal.send()` kanggo kabeh perintah jarak jauh | Iki liwat saluran SSH sing aman |
| **HARUS** resik-resik ing `deactivate()` | Copot pendengar, resik interval |

### Penanganan Kesalahan

Selalu bungkus operasi berisiko ing try/catch:
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
### Resik-resik ing deactivate()

Yen pluginmu nggawe interval, pendengar, utawa langganan — resik-resik:
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

WIA SOOM ndhukung 254 basa. Kanggo nggawe label pluginmu bisa diterjemahake, gunakake pendekatan sing sederhana:
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

## Bagian 6: Contoh Dunia Nyata

### Contoh 1: Pengecek Disk Server

Njaluk `df -h` ing server jarak jauh lan nuduhake ruang sing digunakake/tersedia ing status bar.
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

### Contoh 2: Manajer TODO

Plugin sing ngatur daftar TODO nggunakake setelan kanggo panyimpenan permanen lan webview kanggo tampilan.

> **Pola desain:** Amarga webview ora bisa langsung nelpon API plugin, plugin iki nggunakake pendekatan "snapshot" — maca TODO saka setelan, nampilake minangka HTML baca-saja, lan nyedhiyakake tindakan adhedhasar sidebar kanggo nambah item. Webview minangka lapisan **tampilan**, dudu formulir interaktif.
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

### Contoh 3: Pengawas Kesalahan

Nglacak output terminal lan ngirim notifikasi nalika pola tartamtu terdeteksi.
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

Gunakake iki ing `package.json` `keywords` utawa nalika ngirim menyang registri:

| Kategori | Deskripsi |
|----------|-------------|
| `server` | Manajemen server umum |
| `devtools` | Alat pangembangan |
| `calculator` | Kalkulator lan konverter |
| `simulator` | Simulator |
| `game` | Game terminal |
| `business` | Alat bisnis |
| `security` | Keamanan lan audit |
| `web` | Manajemen server web |
| `education` | Alat pendidikan |
| `health` | Alat sing gegandhengan karo kesehatan |
| `islamic` | Alat Islam (waktu sholat, lsp.) |
| `science` | Alat ilmiah |
| `quantum` | Alat komputasi kuantum |
| `ai` | Alat sing didhukung AI |
| `biotech` | Alat bioteknologi |
| `space` | Alat ruang angkasa lan astronomi |
| `network` | Alat jaringan |
| `database` | Manajemen basis data |
| `monitoring` | Monitoring server |
| `devops` | DevOps lan CI/CD |
| `utility` | Utilitas umum |
| `design` | Alat desain |
| `ecommerce` | Alat e-commerce |
| `automation` | Alat otomatisasi |
| `kpop` | Alat sing gegandhengan karo K-pop |
| `accessibility` | Alat aksesibilitas |
| `analytics` | Analitik lan laporan |
| `wia` | Alat ekosistem WIA |
| `all` | Muncul ing kabeh kategori |

### Ikon Sing Disaranake (Lucide)

| Jeneng Ikon | Gunakake kanggo |
|-------------|----------------|
| `server` | Manajemen server |
| `shield` | Keamanan |
| `database` | Basis data |
| `activity` | Monitoring |
| `terminal` | Alat terminal |
| `code` | Pangembangan |
| `hard-drive` | Disk/penyimpanan |
| `network` | Jaringan |
| `lock` | Auth/enkripsi |
| `eye` | Nonton/monitoring |
| `check-square` | Tugas/TODO |
| `layout-dashboard` | Dasbor |
| `settings` | Konfigurasi |
| `zap` | Otomatisasi |
| `globe` | Web/internasional |

Jelajahi kabeh 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Butuh Bantuan?

- **Masalah GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Masalah Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Contoh:** [Website](https://wiasoom.com)
- **Situs Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Bangun sesuatu sing luar biasa. Bagikan karo donya.</em></p>
<p align="center"><em>— Tim WIA SOOM</em></p>