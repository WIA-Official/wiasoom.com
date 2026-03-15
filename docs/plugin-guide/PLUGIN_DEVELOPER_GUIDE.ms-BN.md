<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Panduan Pembangun Plugin WIA SOOM</h1>
<p align="center"><strong>Bina plugin anda sendiri dalam 5 minit.</strong></p>
<p align="center">Cipta alat pelayan yang berkuasa, papan pemuka, dan automasi — terus di dalam WIA SOOM.</p>

---

## Jadual Kandungan

- [Bahagian 1: Permulaan Pantas — Plugin Pertama Anda dalam 5 Minit](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Bahagian 2: Rujukan API Konteks Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Bahagian 3: Membangun UI Kustom dengan Webviews](#part-3-building-custom-ui-with-webviews)
- [Bahagian 4: Menerbitkan Plugin Anda](#part-4-publishing-your-plugin)
- [Bahagian 5: Amalan Terbaik](#part-5-best-practices)
- [Bahagian 6: Contoh Dunia Sebenar](#part-6-real-world-examples)
- [Lampiran: Kategori & Ikon](#appendix-categories--icons)

---

## Bahagian 1: Permulaan Pantas — Plugin Pertama Anda dalam 5 Minit

### Apa yang akan anda bina

Plugin "Hello World" yang menambah butang ke sidebar. Apabila diklik, ia menunjukkan notifikasi.

### Langkah 1: Cipta folder plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Langkah 2: Cipta package.json
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
**Medan yang diperlukan:** `name`, `version`, `description`, `author`, `main`

### Langkah 3: Cipta index.js
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
### Langkah 4: Mulakan semula WIA SOOM

Mulakan semula aplikasi (atau togol plugin off/on di Tetapan → Plugin).

Anda sepatutnya melihat butang **"Hello World"** di sidebar. Klik ia — anda akan melihat notifikasi kejayaan!

### Cara ia berfungsi
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

## Bahagian 2: Rujukan API Konteks Plugin

Apabila fungsi `activate(context)` anda dipanggil, `context` (atau `ctx`) menyediakan API ini:
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

### `ctx.terminal` — Jalankan arahan di pelayan jauh

#### `terminal.send(sessionId, data)`

Hantar arahan (atau sebarang data) ke sesi terminal yang aktif.

| Parameter | Jenis | Penerangan |
|-----------|-------|------------|
| `sessionId` | `string` | Sesi terminal untuk dihantar |
| `data` | `string` | Arahan atau data untuk dihantar |
§��§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Langgan semua output dari sesi terminal. Mengembalikan **fungsi berhenti melanggan**.

| Parameter | Jenis | Penerangan |
|-----------|-------|------------|
| `sessionId` | `string` | Sesi terminal untuk dipantau |
| `callback` | `(data: string) => void` | Dipanggil dengan setiap bahagian output |
| **Mengembalikan** | `() => void` | Panggil ini untuk berhenti mendengar |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**Penting:** Sentiasa simpan fungsi berhenti melanggan dan panggil ia dalam `deactivate()` untuk mengelakkan kebocoran memori.

---

### `ctx.sftp` — Pemindahan fail

> **Status: Akan Datang** — API SFTP telah ditakrifkan tetapi belum disambungkan ke enjin SFTP aplikasi. `list()` pada masa ini mengembalikan array kosong, dan `upload()`/`download()` adalah tidak berfungsi. Ini akan dilaksanakan sepenuhnya dalam keluaran akan datang. Untuk sekarang, gunakan `ctx.terminal.send()` dengan arahan `scp` atau `rsync` sebagai penyelesaian sementara.

#### `sftp.list(sessionId, path)`

Senaraikan fail dalam direktori jauh.
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
#### `sftp.upload(sessionId, localPath, remotePath)`

Muat naik fail dari mesin tempatan ke pelayan jauh.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

Muat turun fail dari pelayan jauh ke mesin tempatan.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**Penyelesaian sementara (sehingga API SFTP berfungsi):**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — Antara muka pengguna

#### `ui.addSidebarButton(options)`

Tambah butang ke sidebar WIA SOOM.

| Pilihan | Jenis | Diperlukan | Penerangan |
|---------|-------|------------|------------|
| `id` | `string` | Tidak | ID unik (secara default kepada nama plugin) |
| `icon` | `string` | Ya | Nama ikon Lucide (contoh: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ya | Teks butang yang ditunjukkan di sidebar |
| `onClick` | `() => void` | Ya | Fungsi yang dipanggil apabila butang diklik |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Rujukan ikon:** Layari semua ikon yang tersedia di [lucide.dev/icons](https://lucide.dev/icons)

> **Nota keserasian:** Beberapa plugin lama menggunakan argumen posisi seperti `addSidebarButton(id, icon, label, onClick)`. API rasmi menggunakan **objek pilihan** seperti yang didokumenkan di atas. Sentiasa gunakan gaya objek untuk plugin baru.

#### `ui.openWebview(options)`

Buka tetingkap popup dengan kandungan HTML kustom. Inilah cara anda membina UI yang kaya.

| Pilihan | Jenis | Penerangan |
|---------|-------|------------|
| `title` | `string` | Tajuk tetingkap |
| `html` | `string` | Kandungan HTML penuh untuk dipaparkan |
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
> Lihat [Bahagian 3](#part-3-building-custom-ui-with-webviews) untuk corak webview yang lebih maju.

#### `ui.showNotification(type, message)`

Tunjukkan notifikasi toast.

| Parameter | Jenis | Penerangan |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Gaya notifikasi |
| `message` | `string` | Teks untuk ditunjukkan |
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

Tambah item teks yang kekal ke bar status bawah.

| Parameter | Jenis | Penerangan |
|-----------|------|-------------|
| `id` | `string` | ID unik untuk item status ini |
| `text` | `string` | Teks untuk dipaparkan |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — Penyimpanan kekal

Tetapan plugin disimpan secara kekal di `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Baca nilai yang disimpan.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
Mengembalikan `undefined` jika kunci tidak wujud.

#### `settings.set(key, value)`

Simpan nilai. Menyokong string, nombor, boolean, array, dan objek.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**Contoh: Ingat pilihan pengguna**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — Integrasi AI

> **Status: Akan Datang** — API AI telah ditentukan tetapi belum disambungkan ke Soomy. Kini mengembalikan `{ response: 'AI not yet connected' }`. Integrasi AI penuh dirancang untuk keluaran akan datang.

#### `ai.chat(messages, options?)`

Hantar mesej kepada pembantu AI (Soomy).
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

## Bahagian 3: Membangun UI Khusus dengan Webviews

API `openWebview()` membolehkan anda membina UI papan pemuka dengan HTML, CSS, dan JavaScript — semuanya dalam tetingkap popup.

> **Had penting:** Webviews adalah **hanya paparan**. Mereka tidak boleh memanggil kembali ke dalam API plugin (`ctx.settings`, `ctx.terminal`, dll.). Gunakan butang sidebar untuk semua tindakan pengguna, dan gunakan `openWebview()` untuk memaparkan keadaan semasa. Jika anda memerlukan ciri interaktif, picu mereka dari butang sidebar dan buka semula webview untuk menyegarkan paparan.

### Corak: Perintah Terminal → Parse Output → Papar dalam HTML

Ini adalah corak plugin yang paling biasa. Anda menjalankan perintah, menguraikan hasil, dan memaparkannya secara visual.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### Corak: Papan Pemuka Interaktif dengan Auto-Refresh
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
### Corak: Memaparkan Tetapan dalam Webview

> **Nota:** Webviews adalah hanya paparan — mereka tidak boleh memanggil kembali ke dalam API plugin. Gunakan `ctx.settings` dalam pengendali butang sidebar anda untuk mengubah tetapan, dan gunakan `openWebview()` untuk menunjukkan keadaan semasa.
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

## Bahagian 4: Menerbitkan Plugin Anda

### Langkah 1: Uji secara tempatan

1. Salin plugin anda ke `~/.wia-soom/plugins/{your-plugin}/`
2. Mulakan semula WIA SOOM
3. Sahkan ia berfungsi: butang sidebar muncul, ciri berfungsi dengan betul
4. Uji kes tepi: apa yang berlaku jika tiada terminal yang disambungkan?

### Langkah 2: Sediakan untuk penyerahan

Folder plugin anda mesti mengandungi:
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
**Medan `package.json` yang Diperlukan:**

| Medan | Penerangan | Contoh |
|-------|-------------|---------|
| `name` | ID unik dalam kebab-case | `"my-awesome-plugin"` |
| `version` | Versi semantik | `"1.0.0"` |
| `description` | Penerangan satu baris | `"Monitors nginx access logs in real-time"` |
| `author` | Nama anda | `"John Doe"` |
| `main` | Titik masuk | `"index.js"` |

**Medan Pilihan:**

| Medan | Penerangan |
|-------|-------------|
| `license` | Jenis lesen (MIT disyorkan) |
| `keywords` | Array tag carian |
| `soom.minVersion` | Versi WIA SOOM minimum yang diperlukan |

### Langkah 3: Hantar ke Pendaftaran Plugin

1. ****Package** your plugin as a ZIP file
2. **Tambah** plugin anda ke `plugins/{your-plugin-name}/`
3. **Hantar** Permintaan Tarik

### Langkah 4: Semakan dan kelulusan

Kami menyemak setiap plugin untuk:

- **Keselamatan** — tiada API berbahaya (lihat [Peraturan Keselamatan](#security-rules))
- **Kualiti** — adakah ia berfungsi? Adakah kodnya bersih?
- **Kegunaan** — adakah ia menyelesaikan masalah sebenar?

Selepas kelulusan:
1. Plugin anda ditambah ke `registry.json`
2. Bundel ZIP dibuat di `dist/`
3. Plugin anda muncul di **Plugin Store** untuk semua pengguna WIA SOOM!

---

## Bahagian 5: Amalan Terbaik

### Peraturan Keselamatan

Peraturan ini adalah **wajib**. Plugin yang melanggar peraturan ini akan ditolak.

| Peraturan | Kenapa |
|------|-----|
| **JANGAN PERNAH** gunakan `eval()` atau `new Function()` | Risiko suntikan kod |
| **JANGAN PERNAH** gunakan `child_process`, `exec()`, `spawn()` | Hanya gunakan `ctx.terminal.send()` untuk arahan |
| **JANGAN PERNAH** ambil URL luar | Pengecualian: titik akhir API `wiasoom.com` |
| **JANGAN PERNAH** akses `process.env` | Pembolehubah persekitaran mungkin mengandungi rahsia |
| **JANGAN PERNAH** gunakan `require('fs')` secara langsung | Gunakan `ctx.settings` untuk penyimpanan, `ctx.sftp` untuk pemindahan fail |
| **JANGAN PERNAH** gunakan pakej luar npm | Hanya JavaScript tulen — tiada node_modules |
| **HARUS** gunakan `ctx.terminal.send()` untuk semua arahan jauh | Ini melalui saluran SSH yang selamat |
| **HARUS** bersihkan dalam `deactivate()` | Buang pendengar, kosongkan selang |

### Pengendalian Ralat

Sentiasa balut operasi berisiko dalam try/catch:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### Pembersihan dalam deactivate()

Jika plugin anda mencipta selang, pendengar, atau langganan — bersihkan mereka:
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
### Sokongan i18n

WIA SOOM menyokong 254 bahasa. Untuk menjadikan label plugin anda boleh diterjemah, gunakan pendekatan yang mudah:
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

## Bahagian 6: Contoh Dunia Nyata

### Contoh 1: Pemeriksa Disk Pelayan

Menjalankan `df -h` pada pelayan jauh dan menunjukkan ruang yang digunakan/tersedia di bar status.
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

### Contoh 2: Pengurus TODO

Sebuah plugin yang mengurus senarai TODO menggunakan tetapan untuk penyimpanan berterusan dan webview untuk paparan.

> **Corak reka bentuk:** Oleh kerana webview tidak dapat memanggil API plugin secara langsung, plugin ini menggunakan pendekatan "snapshot" — ia membaca TODO dari tetapan, merendernya sebagai HTML yang hanya boleh dibaca, dan menyediakan tindakan berasaskan sidebar untuk menambah item. Webview adalah lapisan **paparan**, bukan borang interaktif.
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

### Contoh 3: Pengawas Ralat

Memantau output terminal dan menghantar notifikasi apabila corak tertentu dikesan.
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

## Lampiran: Kategori & Ikon

### Kategori Plugin (29)

Gunakan ini dalam `package.json` `keywords` atau semasa menghantar ke registry:

| Kategori | Penerangan |
|----------|-------------|
| `server` | Pengurusan server umum |
| `devtools` | Alat pembangunan |
| `calculator` | Pengira dan penukar |
| `simulator` | Simulator |
| `game` | Permainan terminal |
| `business` | Alat perniagaan |
| `security` | Keselamatan dan audit |
| `web` | Pengurusan server web |
| `education` | Alat pendidikan |
| `health` | Alat berkaitan kesihatan |
| `islamic` | Alat Islam (waktu solat, dll.) |
| `science` | Alat saintifik |
| `quantum` | Alat pengkomputeran kuantum |
| `ai` | Alat berkuasa AI |
| `biotech` | Alat bioteknologi |
| `space` | Alat angkasa dan astronomi |
| `network` | Alat rangkaian |
| `database` | Pengurusan pangkalan data |
| `monitoring` | Pemantauan server |
| `devops` | DevOps dan CI/CD |
| `utility` | Utiliti umum |
| `design` | Alat reka bentuk |
| `ecommerce` | Alat e-dagang |
| `automation` | Alat automasi |
| `kpop` | Alat berkaitan K-pop |
| `accessibility` | Alat aksesibiliti |
| `analytics` | Analitik dan pelaporan |
| `wia` | Alat ekosistem WIA |
| `all` | Muncul dalam semua kategori |

### Ikon Disyorkan (Lucide)

| Nama Ikon | Digunakan untuk |
|-----------|----------------- |
| `server` | Pengurusan server |
| `shield` | Keselamatan |
| `database` | Pangkalan data |
| `activity` | Pemantauan |
| `terminal` | Alat terminal |
| `code` | Pembangunan |
| `hard-drive` | Disk/simpanan |
| `network` | Rangkaian |
| `lock` | Pengesahan/enkripsi |
| `eye` | Memantau/pemantauan |
| `check-square` | Tugas/TODO |
| `layout-dashboard` | Papan pemuka |
| `settings` | Konfigurasi |
| `zap` | Automasi |
| `globe` | Web/antarabangsa |

Lihat semua 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Perlukan Bantuan?

- **Isu GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Isu Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Contoh Plugin:** [Website](https://wiasoom.com)
- **Laman Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Bina sesuatu yang menakjubkan. Kongsikan dengan dunia.</em></p>
<p align="center"><em>— Pasukan WIA SOOM</em></p>
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
