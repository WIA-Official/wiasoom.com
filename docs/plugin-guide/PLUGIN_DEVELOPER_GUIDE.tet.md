<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Panduan Pengembang Plugin WIA SOOM</h1>
<p align="center"><strong>Bangun plugin Anda sendiri dalam 5 menit.</strong></p>
<p align="center">Buat alat server yang kuat, dasbor, dan otomatisasi — langsung di dalam WIA SOOM.</p>

---

## Daftar Isi

- [Bagian 1: Memulai dengan Cepat — Plugin Pertama Anda dalam 5 Menit](#bagian-1-memulai-dengan-cepat--plugin-pertama-anda-dalam-5-menit)
- [Bagian 2: Referensi API Konteks Plugin](#bagian-2-referensi-api-konteks-plugin)
  - [ctx.terminal](#ctxterminal--jalankan-perintah-di-server-jarak-jauh)
  - [ctx.sftp](#ctxsftp--transfer-file)
  - [ctx.ui](#ctxui--antarmuka-pengguna)
  - [ctx.settings](#ctxsettings--penyimpanan-berkelanjutan)
  - [ctx.ai](#ctxai--integrasi-ai)
- [Bagian 3: Membangun UI Kustom dengan Webviews](#bagian-3-membangun-ui-kustom-dengan-webviews)
- [Bagian 4: Menerbitkan Plugin Anda](#bagian-4-menerbitkan-plugin-anda)
- [Bagian 5: Praktik Terbaik](#bagian-5-praktik-terbaik)
- [Bagian 6: Contoh Dunia Nyata](#bagian-6-contoh-dunia-nyata)
- [Lampiran: Kategori & Ikon](#lampiran-kategori--ikon)

---

## Bagian 1: Memulai dengan Cepat — Plugin Pertama Anda dalam 5 Menit

### Apa yang akan Anda bangun

Sebuah plugin "Hello World" yang menambahkan tombol ke sidebar. Ketika diklik, itu akan menampilkan notifikasi.

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
### Langkah 4: Mulai ulang WIA SOOM

Mulai ulang aplikasi (atau aktifkan/nonaktifkan plugin di Pengaturan → Plugin).

Anda harus melihat tombol **"Hello World"** di sidebar. Klik itu — Anda akan melihat notifikasi sukses!

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
| `sessionId` | `string` | Sesi terminal untuk dikirim |
| `data` | `string` | Perintah atau data yang akan dikirim |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Berlangganan semua output dari sesi terminal. Mengembalikan **fungsi berhenti berlangganan**.

| Parameter | Tipe | Deskripsi |
|-----------|------|-------------|
| `sessionId` | `string` | Sesi terminal untuk dipantau |
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
**Penting:** Selalu simpan fungsi berhenti berlangganan dan panggil di `deactivate()` untuk mencegah kebocoran memori.

---

### `ctx.sftp` — Transfer file

> **Status: Segera Hadir** — API SFTP telah didefinisikan tetapi belum terhubung ke mesin SFTP aplikasi. `list()` saat ini mengembalikan array kosong, dan `upload()`/`download()` tidak berfungsi. Ini akan sepenuhnya diimplementasikan dalam rilis mendatang. Untuk saat ini, gunakan `ctx.terminal.send()` dengan perintah `scp` atau `rsync` sebagai solusi sementara.

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
|--------|------|----------|-------------|
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
|--------|------|-------------|
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
> See [Part 3](#part-3-building-custom-ui-with-webviews) ba padrões webview avançados.

#### `ui.showNotification(type, message)`

Hatoo notifikasaun toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stilu notifikasaun |
| `message` | `string` | Tekstua hatoo |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Adiciona item tekstu persistente ba barra status ba embaixo.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID unik ba item status ne'e |
| `text` | `string` | Tekstua hatoo |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Armazenamento persistente

Configurações do plugin sira armazenadas permanentemente iha `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lê valor ne'ebé salva.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Returna `undefined` se o key la iha.

#### `settings.set(key, value)`

Salva um valor. Suporta strings, números, booleanos, arrays, no objetos.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemplo: Lembra preferências do usuário**
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

### `ctx.ai` — Integração AI

> **Status: Vindo em breve** — A API AI está definida mas la ainda konekta ba Soomy. Agora retorna `{ response: 'AI not yet connected' }`. Integração total AI está planejada ba lançamento futuro.

#### `ai.chat(messages, options?)`

Envia mensagens ba assistente AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Parte 3: Construindo UI Personalizada com Webviews

A API `openWebview()` permite você construi UI dashboard ho HTML, CSS, no JavaScript — tudo dentro de uma janela popup.

> **Limitação importante:** Webviews são **apenas para exibição**. Elas não podem fazer chamadas de volta para APIs do plugin (`ctx.settings`, `ctx.terminal`, etc.). Usa botões na barra lateral ba todos ações do usuário, no usa `openWebview()` ba exibir estado atual. Se precisa recursos interativos, ativa sira husi botões na barra lateral no re-abre o webview ba atualiza exibição.

### Padrão: Comando de Terminal → Analisar Saída → Mostrar em HTML

Isto é o padrão de plugin mais comum. Você executa um comando, analisa o resultado, no exibe visualmente.
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
### Padrão: Dashboard Interativo ho Auto-Atualização
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
### Padrão: Exibindo Configurações em um Webview

> **Nota:** Webviews são apenas para exibição — elas não podem fazer chamadas de volta para APIs do plugin. Usa `ctx.settings` iha manipuladores de botões na barra lateral ba modifica configurações, no usa `openWebview()` ba mostra estado atual.
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

## Parte 4: Publicando Seu Plugin

### Passo 1: Teste localmente

1. Copia seu plugin ba `~/.wia-soom/plugins/{your-plugin}/`
2. Reinicia WIA SOOM
3. Verifica se funciona: botão na barra lateral aparece, recursos funcionam corretamente
4. Testa casos extremos: o que acontece se nenhum terminal está conectado?

### Passo 2: Prepara ba submissão

Pasta do seu plugin deve conter:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Requisitos `package.json` campos:**

| Campo | Descrição | Exemplo |
|-------|-------------|---------|
| `name` | ID único em kebab-case | `"my-awesome-plugin"` |
| `version` | Versão semântica | `"1.0.0"` |
| `description` | Descrição em uma linha | `"Monitors nginx access logs in real-time"` |
| `author` | Seu nome | `"John Doe"` |
| `main` | Ponto de entrada | `"index.js"` |

**Campos opcionais:**

| Campo | Descrição |
|-------|-------------|
| `license` | Tipo de licença (MIT recomendado) |
| `keywords` | Array de tags de busca |
| `soom.minVersion` | Versão mínima do WIA SOOM necessária |

### Passo 3: Enviar para o Registro de Plugins

1. ****Package** your plugin as a ZIP file
2. **Adicionar** seu plugin a `plugins/{seu-nome-do-plugin}/`
3. **Enviar** um Pull Request

### Passo 4: Revisão e aprovação

Nós revisamos cada plugin para:

- **Segurança** — nenhuma API perigosa (veja [Regras de Segurança](#security-rules))
- **Qualidade** — funciona? O código é limpo?
- **Utilidade** — resolve um problema real?

Após a aprovação:
1. Seu plugin é adicionado a `registry.json`
2. Um pacote ZIP é criado em `dist/`
3. Seu plugin aparece na **Plugin Store** para todos os usuários do WIA SOOM!

---

## Parte 5: Melhores Práticas

### Regras de Segurança

Essas regras são **obrigatórias**. Plugins que as violarem serão rejeitados.

| Regra | Por quê |
|------|-----|
| **NUNCA** use `eval()` ou `new Function()` | Risco de injeção de código |
| **NUNCA** use `child_process`, `exec()`, `spawn()` | Use apenas `ctx.terminal.send()` para comandos |
| **NUNCA** busque URLs externas | Exceção: endpoints da API `wiasoom.com` |
| **NUNCA** acesse `process.env` | Variáveis de ambiente podem conter segredos |
| **NUNCA** use `require('fs')` diretamente | Use `ctx.settings` para armazenamento, `ctx.sftp` para transferência de arquivos |
| **NUNCA** use pacotes externos npm | Apenas JavaScript puro — sem node_modules |
| **DEVE** usar `ctx.terminal.send()` para todos os comandos remotos | Isso passa pelo canal SSH seguro |
| **DEVE** limpar em `deactivate()` | Remova ouvintes, limpe intervalos |

### Tratamento de Erros

Sempre envolva operações arriscadas em try/catch:
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
### Limpeza em deactivate()

Se seu plugin criar intervalos, ouvintes ou assinaturas — limpe-os:
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
### Suporte a i18n

WIA SOOM suporta 254 idiomas. Para tornar o rótulo do seu plugin traduzível, use uma abordagem simples:
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

## Parte 6: Exemplos do Mundo Real

### Exemplo 1: Verificador de Disco do Servidor

Executa `df -h` no servidor remoto e mostra o espaço usado/disponível na barra de status.
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

### Exemplo 2: Gerenciador de TODO

Um plugin que gerencia uma lista de TODO usando configurações para armazenamento persistente e uma webview para exibição.

> **Padrão de design:** Como as webviews não podem chamar diretamente as APIs do plugin, este plugin usa uma abordagem de "snapshot" — ele lê os TODOs das configurações, renderiza-os como HTML somente leitura e fornece ações baseadas na barra lateral para adicionar itens. A webview é uma camada de **exibição**, não um formulário interativo.
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

### Exemplo 3: Monitor de Erros

Monitora a saída do terminal e envia uma notificação quando padrões específicos são detectados.
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

## Apêndice: Kategori & Ikon

### Kategori Plugin (29)

Uza ne'e iha `package.json` `keywords` ka durante submete ba registry:

| Kategori | Deskrisaun |
|----------|-------------|
| `server` | Manajementu server geral |
| `devtools` | Ferramentas desenvolvimentu |
| `calculator` | Kalkuladora no konversores |
| `simulator` | Simuladores |
| `game` | Jogos terminal |
| `business` | Ferramentas negócios |
| `security` | Seguransa no auditoria |
| `web` | Manajementu server web |
| `education` | Ferramentas edukativas |
| `health` | Ferramentas relasionadas ba saúde |
| `islamic` | Ferramentas islâmicas (oras reza, etc.) |
| `science` | Ferramentas científicas |
| `quantum` | Ferramentas komputasaun kuantum |
| `ai` | Ferramentas suportadas AI |
| `biotech` | Ferramentas biotecnologia |
| `space` | Ferramentas espásio no astronomia |
| `network` | Ferramentas rede |
| `database` | Manajementu database |
| `monitoring` | Monitorizasaun server |
| `devops` | DevOps no CI/CD |
| `utility` | Utilitários gerais |
| `design` | Ferramentas design |
| `ecommerce` | Ferramentas e-commerce |
| `automation` | Ferramentas automasaun |
| `kpop` | Ferramentas relasionadas ba K-pop |
| `accessibility` | Ferramentas acessibilidade |
| `analytics` | Analítika no relatóriu |
| `wia` | Ferramentas ekosistema WIA |
| `all` | Aparese iha todos kategori |

### Ikon Rekomendadu (Lucide)

| Naran Ikon | Uza ba |
|-----------|---------|
| `server` | Manajementu server |
| `shield` | Seguransa |
| `database` | Database |
| `activity` | Monitorizasaun |
| `terminal` | Ferramentas terminal |
| `code` | Desenvolvimentu |
| `hard-drive` | Disk/armazenamentu |
| `network` | Rede |
| `lock` | Autentikasaun/enkripasaun |
| `eye` | Observa/monitoriza |
| `check-square` | Tarefas/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfigurasaun |
| `zap` | Automasaun |
| `globe` | Web/internasional |

Buka todos 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Precisa Ajuda?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Exemplo Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construi algo incrível. Partilha ho mundo.</em></p>
<p align="center"><em>— Equipa WIA SOOM</em></p>