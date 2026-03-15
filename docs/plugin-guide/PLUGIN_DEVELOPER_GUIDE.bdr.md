<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Panduan Pengembang Plugin WIA SOOM</h1>
<p align="center"><strong>Buat plugin Anda sendiri dalam 5 menit.</strong></p>
<p align="center">Ciptakan alat server yang kuat, dasbor, dan otomatisasi — langsung di dalam WIA SOOM.</p>

---

## Daftar Isi

- [Bagian 1: Memulai Cepat — Plugin Pertama Anda dalam 5 Menit](#bagian-1-memulai-cepat--plugin-pertama-anda-dalam-5-menit)
- [Bagian 2: Referensi API Konteks Plugin](#bagian-2-referensi-api-konteks-plugin)
  - [ctx.terminal](#ctxterminal--jalankan-perintah-di-server-jarak-jauh)
  - [ctx.sftp](#ctxsftp--transfer-file)
  - [ctx.ui](#ctxui--antarmuka-pengguna)
  - [ctx.settings](#ctxsettings--penyimpanan-berkelanjutan)
  - [ctx.ai](#ctxai--integrasi-ai)
- [Bagian 3: Membangun UI Kustom dengan Webview](#bagian-3-membangun-ui-kustom-dengan-webview)
- [Bagian 4: Menerbitkan Plugin Anda](#bagian-4-menerbitkan-plugin-anda)
- [Bagian 5: Praktik Terbaik](#bagian-5-praktik-terbaik)
- [Bagian 6: Contoh Dunia Nyata](#bagian-6-contoh-dunia-nyata)
- [Lampiran: Kategori & Ikon](#lampiran-kategori--ikon)

---

## Bagian 1: Memulai Cepat — Plugin Pertama Anda dalam 5 Menit

### Apa yang akan Anda buat

Plugin "Hello World" yang menambahkan tombol ke sidebar. Ketika diklik, ia akan menampilkan notifikasi.

### Langkah 1: Buat folder plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Langkah 2: Buat package.json
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
**Bidang yang diperlukan:** `name`, `version`, `description`, `author`, `main`

### Langkah 3: Buat index.js
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

Restart aplikasi (atau toggle plugin off/on di Pengaturan → Plugin).

Anda akan melihat tombol **"Hello World"** di sidebar. Klik tombol tersebut — Anda akan melihat notifikasi sukses!

### Cara kerjanya
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

Ketika fungsi `activate(context)` Anda dipanggil, `context` (atau `ctx`) menyediakan API berikut:
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

### `ctx.terminal` — Jalankan perintah di server jarak jauh

#### `terminal.send(sessionId, data)`

Kirim perintah (atau data apa pun) ke sesi terminal yang aktif.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal yang akan dikirim |
| `data` | `string` | Perintah atau data yang akan dikirim |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Berlangganan semua output dari sesi terminal. Mengembalikan **fungsi unsubscribe**.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal yang akan dipantau |
| `callback` | `(data: string) => void` | Dipanggil dengan setiap potongan output |
| **Mengembalikan** | `() => void` | Panggil ini untuk berhenti mendengarkan |
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
**Penting:** Selalu simpan fungsi unsubscribe dan panggil di `deactivate()` untuk mencegah kebocoran memori.

---

### `ctx.sftp` — Transfer file

> **Status: Segera Hadir** — API SFTP telah didefinisikan tetapi belum terhubung ke mesin SFTP aplikasi. `list()` saat ini mengembalikan array kosong, dan `upload()`/`download()` tidak berfungsi. Ini akan sepenuhnya diimplementasikan di rilis mendatang. Untuk saat ini, gunakan `ctx.terminal.send()` dengan perintah `scp` atau `rsync` sebagai solusi sementara.

#### `sftp.list(sessionId, path)`

Daftar file di direktori jarak jauh.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Unggah file dari mesin lokal ke server jarak jauh.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Unduh file dari server jarak jauh ke mesin lokal.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solusi sementara (sampai API SFTP aktif):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Antarmuka pengguna

#### `ui.addSidebarButton(options)`

Tambahkan tombol ke sidebar WIA SOOM.

| Opsi | Tipe | Diperlukan | Deskripsi |
|------|------|------------|-------------|
| `id` | `string` | Tidak | ID unik (default ke nama plugin) |
| `icon` | `string` | Ya | Nama ikon Lucide (misalnya, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ya | Teks tombol yang ditampilkan di sidebar |
| `onClick` | `() => void` | Ya | Fungsi yang dipanggil saat tombol diklik |
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
**Referensi ikon:** Jelajahi semua ikon yang tersedia di [lucide.dev/icons](https://lucide.dev/icons)

> **Catatan kompatibilitas:** Beberapa plugin lama menggunakan argumen posisi seperti `addSidebarButton(id, icon, label, onClick)`. API resmi menggunakan **objek opsi** seperti yang didokumentasikan di atas. Selalu gunakan gaya objek untuk plugin baru.

#### `ui.openWebview(options)`

Buka jendela popup dengan konten HTML kustom. Inilah cara Anda membangun UI yang kaya.

| Opsi | Tipe | Deskripsi |
|------|------|-------------|
| `title` | `string` | Judul jendela |
| `html` | `string` | Konten HTML lengkap untuk dirender |
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
> Lihat [Bahagian 3](#part-3-building-custom-ui-with-webviews) untuk pola webview yang lebih maju.

#### `ui.showNotification(type, message)`

Tunjukkan notifikasi toast.

| Parameter | Jenis | Keterangan |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Gaya notifikasi |
| `message` | `string` | Teks yang akan ditunjukkan |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Tambah item teks yang kekal ke bar status bawah.

| Parameter | Jenis | Keterangan |
|-----------|------|-------------|
| `id` | `string` | ID unik untuk item status ini |
| `text` | `string` | Teks yang akan dipaparkan |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Penyimpanan kekal

Tetapan plugin disimpan secara kekal di `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Baca nilai yang disimpan.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Mengembalikan `undefined` jika kunci tidak wujud.

#### `settings.set(key, value)`

Simpan nilai. Menyokong string, nombor, boolean, array, dan objek.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Contoh: Ingat pilihan pengguna**
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

> **Status: Akan Datang** — API AI telah ditentukan tetapi belum disambungkan ke Soomy. Saat ini mengembalikan `{ response: 'AI not yet connected' }`. Integrasi AI penuh dirancang untuk rilis akan datang.

#### `ai.chat(messages, options?)`

Hantar mesej kepada pembantu AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Bahagian 3: Membangunkan UI Khusus dengan Webviews

API `openWebview()` membolehkan anda membina UI papan pemuka dengan HTML, CSS, dan JavaScript — semuanya di dalam tetingkap popup.

> **Had penting:** Webviews adalah **hanya paparan**. Mereka tidak boleh memanggil kembali ke API plugin (`ctx.settings`, `ctx.terminal`, dll.). Gunakan butang sidebar untuk semua tindakan pengguna, dan gunakan `openWebview()` untuk memaparkan keadaan semasa. Jika anda memerlukan ciri interaktif, picu mereka dari butang sidebar dan buka semula webview untuk menyegarkan paparan.

### Pola: Perintah Terminal → Parse Output → Tunjukkan dalam HTML

Ini adalah pola plugin yang paling biasa. Anda menjalankan perintah, memparse hasilnya, dan memaparkannya secara visual.
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
### Pola: Papan Pemuka Interaktif dengan Auto-Refresh
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
### Pola: Memaparkan Tetapan dalam Webview

> **Nota:** Webviews adalah hanya paparan — mereka tidak boleh memanggil kembali ke API plugin. Gunakan `ctx.settings` dalam pengendali butang sidebar anda untuk mengubah tetapan, dan gunakan `openWebview()` untuk menunjukkan keadaan semasa.
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

## Bahagian 4: Menerbitkan Plugin Anda

### Langkah 1: Uji secara lokal

1. Salin plugin anda ke `~/.wia-soom/plugins/{your-plugin}/`
2. Mulakan semula WIA SOOM
3. Sahkan ia berfungsi: butang sidebar muncul, ciri berfungsi dengan betul
4. Uji kes tepi: apa yang berlaku jika tiada terminal yang disambungkan?

### Langkah 2: Sediakan untuk penyerahan

Folder plugin anda mesti mengandungi:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Diperlukan `package.json` bidang:**

| Bidang | Keterangan | Contoh |
|-------|-------------|---------|
| `name` | ID unik dalam kebab-case | `"my-awesome-plugin"` |
| `version` | Versi semantik | `"1.0.0"` |
| `description` | Deskripsi satu baris | `"Memantau log akses nginx secara langsung"` |
| `author` | Nama Anda | `"John Doe"` |
| `main` | Titik masuk | `"index.js"` |

**Bidang opsional:**

| Bidang | Keterangan |
|-------|-------------|
| `license` | Jenis lisensi (MIT disarankan) |
| `keywords` | Array tag pencarian |
| `soom.minVersion` | Versi WIA SOOM minimum yang diperlukan |

### Langkah 3: Kirim ke Registri Plugin

1. ****Package** your plugin as a ZIP file
2. **Tambahkan** plugin Anda ke `plugins/{nama-plugin-anda}/`
3. **Kirim** Permintaan Tarik

### Langkah 4: Tinjauan dan persetujuan

Kami meninjau setiap plugin untuk:

- **Keamanan** — tidak ada API berbahaya (lihat [Aturan Keamanan](#security-rules))
- **Kualitas** — apakah itu berfungsi? Apakah kodenya bersih?
- **Kegunaan** — apakah itu menyelesaikan masalah nyata?

Setelah disetujui:
1. Plugin Anda ditambahkan ke `registry.json`
2. Bundel ZIP dibuat di `dist/`
3. Plugin Anda muncul di **Plugin Store** untuk semua pengguna WIA SOOM!

---

## Bagian 5: Praktik Terbaik

### Aturan Keamanan

Aturan ini adalah **wajib**. Plugin yang melanggar akan ditolak.

| Aturan | Mengapa |
|------|-----|
| **JANGAN PERNAH** gunakan `eval()` atau `new Function()` | Risiko injeksi kode |
| **JANGAN PERNAH** gunakan `child_process`, `exec()`, `spawn()` | Hanya gunakan `ctx.terminal.send()` untuk perintah |
| **JANGAN PERNAH** ambil URL eksternal | Pengecualian: endpoint API `wiasoom.com` |
| **JANGAN PERNAH** akses `process.env` | Variabel lingkungan mungkin mengandung rahasia |
| **JANGAN PERNAH** gunakan `require('fs')` secara langsung | Gunakan `ctx.settings` untuk penyimpanan, `ctx.sftp` untuk transfer file |
| **JANGAN PERNAH** gunakan paket eksternal npm | Hanya JavaScript murni — tidak ada node_modules |
| **HARUS** gunakan `ctx.terminal.send()` untuk semua perintah jarak jauh | Ini melalui saluran SSH yang aman |
| **HARUS** bersihkan di `deactivate()` | Hapus pendengar, bersihkan interval |

### Penanganan Kesalahan

Selalu bungkus operasi berisiko dalam try/catch:
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
### Pembersihan di deactivate()

Jika plugin Anda membuat interval, pendengar, atau langganan — bersihkan mereka:
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

WIA SOOM mendukung 254 bahasa. Untuk membuat label plugin Anda dapat diterjemahkan, gunakan pendekatan sederhana:
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

### Contoh 1: Pemeriksa Disk Server

Menjalankan `df -h` di server jarak jauh dan menunjukkan ruang yang digunakan/tersedia di bilah status.
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

Sebuah plugin yang mengelola daftar TODO menggunakan pengaturan untuk penyimpanan persisten dan webview untuk tampilan.

> **Pola desain:** Karena webview tidak dapat langsung memanggil API plugin, plugin ini menggunakan pendekatan "snapshot" — ia membaca TODO dari pengaturan, merendernya sebagai HTML hanya-baca, dan menyediakan tindakan berbasis sidebar untuk menambahkan item. Webview adalah lapisan **tampilan**, bukan formulir interaktif.
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

Memantau output terminal dan mengirim notifikasi ketika pola tertentu terdeteksi.
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

Gunakan ini dalam `package.json` `keywords` atau ketika mengirim ke registri:

| Kategori | Deskripsi |
|----------|-------------|
| `server` | Pengelolaan server umum |
| `devtools` | Alat pengembangan |
| `calculator` | Kalkulator dan konverter |
| `simulator` | Simulator |
| `game` | Permainan terminal |
| `business` | Alat bisnis |
| `security` | Keamanan dan audit |
| `web` | Pengelolaan server web |
| `education` | Alat pendidikan |
| `health` | Alat terkait kesehatan |
| `islamic` | Alat Islam (waktu sholat, dll.) |
| `science` | Alat ilmiah |
| `quantum` | Alat komputasi kuantum |
| `ai` | Alat bertenaga AI |
| `biotech` | Alat bioteknologi |
| `space` | Alat luar angkasa dan astronomi |
| `network` | Alat jaringan |
| `database` | Pengelolaan basis data |
| `monitoring` | Pemantauan server |
| `devops` | DevOps dan CI/CD |
| `utility` | Utilitas umum |
| `design` | Alat desain |
| `ecommerce` | Alat e-commerce |
| `automation` | Alat otomatisasi |
| `kpop` | Alat terkait K-pop |
| `accessibility` | Alat aksesibilitas |
| `analytics` | Analitik dan pelaporan |
| `wia` | Alat ekosistem WIA |
| `all` | Muncul di semua kategori |

### Ikon yang Direkomendasikan (Lucide)

| Nama Ikon | Digunakan untuk |
|-----------|---------|
| `server` | Pengelolaan server |
| `shield` | Keamanan |
| `database` | Basis data |
| `activity` | Pemantauan |
| `terminal` | Alat terminal |
| `code` | Pengembangan |
| `hard-drive` | Disk/penyimpanan |
| `network` | Jaringan |
| `lock` | Autentikasi/enkripsi |
| `eye` | Mengawasi/memantau |
| `check-square` | Tugas/TODO |
| `layout-dashboard` | Dasbor |
| `settings` | Konfigurasi |
| `zap` | Otomatisasi |
| `globe` | Web/internasional |

Jelajahi semua 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Perlu Bantuan?

- **Masalah GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Masalah Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Contoh:** [Website](https://wiasoom.com)
- **Situs Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Buat sesuatu yang menakjubkan. Bagikan dengan dunia.</em></p>
<p align="center"><em>— Tim WIA SOOM</em></p>