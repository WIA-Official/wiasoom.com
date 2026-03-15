<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Hướng dẫn phát triển Plugin WIA SOOM</h1>
<p align="center"><strong>Xây dựng plugin của riêng bạn trong 5 phút.</strong></p>
<p align="center">Tạo ra các công cụ máy chủ mạnh mẽ, bảng điều khiển và tự động hóa — ngay bên trong WIA SOOM.</p>

---

## Mục lục

- [Phần 1: Khởi động nhanh — Plugin đầu tiên của bạn trong 5 phút](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Phần 2: Tham khảo API ngữ cảnh Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Phần 3: Xây dựng UI tùy chỉnh với Webviews](#part-3-building-custom-ui-with-webviews)
- [Phần 4: Xuất bản Plugin của bạn](#part-4-publishing-your-plugin)
- [Phần 5: Thực hành tốt nhất](#part-5-best-practices)
- [Phần 6: Ví dụ thực tế](#part-6-real-world-examples)
- [Phụ lục: Danh mục & Biểu tượng](#appendix-categories--icons)

---

## Phần 1: Khởi động nhanh — Plugin đầu tiên của bạn trong 5 phút

### Những gì bạn sẽ xây dựng

Một plugin "Hello World" thêm một nút vào thanh bên. Khi nhấn vào, nó sẽ hiển thị một thông báo.

### Bước 1: Tạo thư mục plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Bước 2: Tạo package.json
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
**Các trường bắt buộc:** `name`, `version`, `description`, `author`, `main`

### Bước 3: Tạo index.js
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
### Bước 4: Khởi động lại WIA SOOM

Khởi động lại ứng dụng (hoặc bật/tắt plugin trong Cài đặt → Plugins).

Bạn sẽ thấy một nút **"Hello World"** trong thanh bên. Nhấn vào nó — bạn sẽ thấy một thông báo thành công!

### Cách hoạt động
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

## Phần 2: Tham khảo API ngữ cảnh Plugin

Khi hàm `activate(context)` của bạn được gọi, `context` (hoặc `ctx`) cung cấp các API sau:
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

### `ctx.terminal` — Chạy lệnh trên các máy chủ từ xa

#### `terminal.send(sessionId, data)`

Gửi một lệnh (hoặc bất kỳ dữ liệu nào) đến một phiên terminal đang hoạt động.

| Tham số | Kiểu | Mô tả |
|---------|------|-------|
| `sessionId` | `string` | Phiên terminal để gửi đến |
| `data` | `string` | Lệnh hoặc dữ liệu để gửi |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Đăng ký nhận tất cả đầu ra từ một phiên terminal. Trả về một **hàm hủy đăng ký**.

| Tham số | Kiểu | Mô tả |
|---------|------|-------|
| `sessionId` | `string` | Phiên terminal để theo dõi |
| `callback` | `(data: string) => void` | Được gọi với mỗi phần đầu ra |
| **Trả về** | `() => void` | Gọi điều này để dừng lắng nghe |
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
**Quan trọng:** Luôn lưu hàm hủy đăng ký và gọi nó trong `deactivate()` để ngăn ngừa rò rỉ bộ nhớ.

---

### `ctx.sftp` — Chuyển file

> **Trạng thái: Sắp có** — API SFTP đã được định nghĩa nhưng chưa được kết nối với engine SFTP của ứng dụng. `list()` hiện trả về một mảng rỗng, và `upload()`/`download()` không thực hiện gì. Điều này sẽ được thực hiện đầy đủ trong một phiên bản tương lai. Hiện tại, hãy sử dụng `ctx.terminal.send()` với các lệnh `scp` hoặc `rsync` như một giải pháp tạm thời.

#### `sftp.list(sessionId, path)`

Liệt kê các file trong một thư mục từ xa.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Tải lên một file từ máy tính cục bộ lên máy chủ từ xa.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Tải xuống một file từ máy chủ từ xa về máy tính cục bộ.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Giải pháp tạm thời (cho đến khi API SFTP hoạt động):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Giao diện người dùng

#### `ui.addSidebarButton(options)`

Thêm một nút vào thanh bên WIA SOOM.

| Tùy chọn | Kiểu | Bắt buộc | Mô tả |
|----------|------|----------|-------|
| `id` | `string` | Không | ID duy nhất (mặc định là tên plugin) |
| `icon` | `string` | Có | Tên biểu tượng Lucide (ví dụ: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Có | Văn bản nút hiển thị trong thanh bên |
| `onClick` | `() => void` | Có | Hàm được gọi khi nút được nhấn |
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
**Tham khảo biểu tượng:** Duyệt tất cả các biểu tượng có sẵn tại [lucide.dev/icons](https://lucide.dev/icons)

> **Lưu ý về tính tương thích:** Một số plugin cũ sử dụng các tham số vị trí như `addSidebarButton(id, icon, label, onClick)`. API chính thức sử dụng một **đối tượng tùy chọn** như đã được tài liệu hóa ở trên. Luôn sử dụng kiểu đối tượng cho các plugin mới.

#### `ui.openWebview(options)`

Mở một cửa sổ popup với nội dung HTML tùy chỉnh. Đây là cách bạn xây dựng các giao diện phong phú.

| Tùy chọn | Kiểu | Mô tả |
|----------|------|-------|
| `title` | `string` | Tiêu đề cửa sổ |
| `html` | `string` | Nội dung HTML đầy đủ để hiển thị |
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
> Xem [Phần 3](#part-3-building-custom-ui-with-webviews) để biết các mẫu webview nâng cao.

#### `ui.showNotification(type, message)`

Hiển thị thông báo toast.

| Tham số | Kiểu | Mô tả |
|---------|------|-------|
| `type` | `'success' \| 'error' \| 'info'` | Kiểu thông báo |
| `message` | `string` | Văn bản để hiển thị |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Thêm một mục văn bản cố định vào thanh trạng thái dưới cùng.

| Tham số | Kiểu | Mô tả |
|---------|------|-------|
| `id` | `string` | ID duy nhất cho mục trạng thái này |
| `text` | `string` | Văn bản để hiển thị |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Lưu trữ cố định

Cài đặt plugin được lưu trữ vĩnh viễn trong `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Đọc một giá trị đã lưu.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Trả về `undefined` nếu khóa không tồn tại.

#### `settings.set(key, value)`

Lưu một giá trị. Hỗ trợ chuỗi, số, boolean, mảng và đối tượng.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Ví dụ: Nhớ sở thích của người dùng**
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

### `ctx.ai` — Tích hợp AI

> **Trạng thái: Sắp ra mắt** — API AI đã được định nghĩa nhưng chưa được kết nối với Soomy. Hiện tại trả về `{ response: 'AI not yet connected' }`. Tích hợp AI đầy đủ dự kiến sẽ có trong phiên bản tương lai.

#### `ai.chat(messages, options?)`

Gửi tin nhắn đến trợ lý AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Phần 3: Xây dựng Giao diện Tùy chỉnh với Webviews

API `openWebview()` cho phép bạn xây dựng giao diện bảng điều khiển với HTML, CSS và JavaScript — tất cả trong một cửa sổ popup.

> **Giới hạn quan trọng:** Webviews chỉ **để hiển thị**. Chúng không thể gọi lại vào các API plugin (`ctx.settings`, `ctx.terminal`, v.v.). Sử dụng các nút thanh bên cho tất cả các hành động của người dùng, và sử dụng `openWebview()` để hiển thị trạng thái hiện tại. Nếu bạn cần các tính năng tương tác, hãy kích hoạt chúng từ các nút thanh bên và mở lại webview để làm mới hiển thị.

### Mẫu: Lệnh Terminal → Phân tích Đầu ra → Hiển thị trong HTML

Đây là mẫu plugin phổ biến nhất. Bạn chạy một lệnh, phân tích kết quả và hiển thị nó một cách trực quan.
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
### Mẫu: Bảng điều khiển Tương tác với Tự động Làm mới
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
### Mẫu: Hiển thị Cài đặt trong một Webview

> **Lưu ý:** Webviews chỉ để hiển thị — chúng không thể gọi lại vào các API plugin. Sử dụng `ctx.settings` trong các trình xử lý nút thanh bên của bạn để sửa đổi cài đặt, và sử dụng `openWebview()` để hiển thị trạng thái hiện tại.
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

## Phần 4: Xuất bản Plugin của Bạn

### Bước 1: Kiểm tra cục bộ

1. Sao chép plugin của bạn vào `~/.wia-soom/plugins/{your-plugin}/`
2. Khởi động lại WIA SOOM
3. Xác minh nó hoạt động: nút thanh bên xuất hiện, các tính năng hoạt động chính xác
4. Kiểm tra các trường hợp biên: điều gì xảy ra nếu không có terminal nào được kết nối?

### Bước 2: Chuẩn bị để nộp

Thư mục plugin của bạn phải chứa:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Các trường bắt buộc trong `package.json`:**

| Trường | Mô tả | Ví dụ |
|-------|-------------|---------|
| `name` | ID duy nhất theo định dạng kebab-case | `"my-awesome-plugin"` |
| `version` | Phiên bản ngữ nghĩa | `"1.0.0"` |
| `description` | Mô tả một dòng | `"Giám sát nhật ký truy cập nginx theo thời gian thực"` |
| `author` | Tên của bạn | `"John Doe"` |
| `main` | Điểm vào | `"index.js"` |

**Các trường tùy chọn:**

| Trường | Mô tả |
|-------|-------------|
| `license` | Loại giấy phép (khuyến nghị MIT) |
| `keywords` | Mảng các thẻ tìm kiếm |
| `soom.minVersion` | Phiên bản WIA SOOM tối thiểu yêu cầu |

### Bước 3: Gửi đến Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Thêm** plugin của bạn vào `plugins/{tên-plugin-của-bạn}/`
3. **Gửi** một Pull Request

### Bước 4: Xem xét và phê duyệt

Chúng tôi xem xét từng plugin về:

- **Bảo mật** — không có API nguy hiểm (xem [Quy tắc Bảo mật](#security-rules))
- **Chất lượng** — nó có hoạt động không? Mã có sạch không?
- **Tính hữu ích** — nó có giải quyết được vấn đề thực sự không?

Sau khi được phê duyệt:
1. Plugin của bạn sẽ được thêm vào `registry.json`
2. Một gói ZIP sẽ được tạo trong `dist/`
3. Plugin của bạn sẽ xuất hiện trong **Plugin Store** cho tất cả người dùng WIA SOOM!

---

## Phần 5: Thực hành tốt nhất

### Quy tắc Bảo mật

Các quy tắc này là **bắt buộc**. Các plugin vi phạm sẽ bị từ chối.

| Quy tắc | Tại sao |
|------|-----|
| **KHÔNG BAO GIỜ** sử dụng `eval()` hoặc `new Function()` | Nguy cơ tiêm mã |
| **KHÔNG BAO GIỜ** sử dụng `child_process`, `exec()`, `spawn()` | Chỉ sử dụng `ctx.terminal.send()` cho các lệnh |
| **KHÔNG BAO GIỜ** lấy URL bên ngoài | Ngoại lệ: các điểm cuối API của `wiasoom.com` |
| **KHÔNG BAO GIỜ** truy cập `process.env` | Biến môi trường có thể chứa bí mật |
| **KHÔNG BAO GIỜ** sử dụng `require('fs')` trực tiếp | Sử dụng `ctx.settings` để lưu trữ, `ctx.sftp` để chuyển file |
| **KHÔNG BAO GIỜ** sử dụng các gói npm bên ngoài | Chỉ JavaScript thuần — không có node_modules |
| **PHẢI** sử dụng `ctx.terminal.send()` cho tất cả các lệnh từ xa | Điều này đi qua kênh SSH an toàn |
| **PHẢI** dọn dẹp trong `deactivate()` | Xóa listeners, xóa intervals |

### Xử lý Lỗi

Luôn bọc các thao tác rủi ro trong try/catch:
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
### Dọn dẹp trong deactivate()

Nếu plugin của bạn tạo ra các intervals, listeners, hoặc subscriptions — hãy dọn dẹp chúng:
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
### Hỗ trợ i18n

WIA SOOM hỗ trợ 254 ngôn ngữ. Để làm cho nhãn plugin của bạn có thể dịch được, hãy sử dụng một cách tiếp cận đơn giản:
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

## Phần 6: Ví dụ Thực tế

### Ví dụ 1: Kiểm tra Đĩa Máy Chủ

Chạy `df -h` trên máy chủ từ xa và hiển thị không gian đã sử dụng/có sẵn trong thanh trạng thái.
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

### Ví dụ 2: Quản lý TODO

Một plugin quản lý danh sách TODO sử dụng cài đặt cho lưu trữ lâu dài và một webview để hiển thị.

> **Mẫu thiết kế:** Vì webviews không thể gọi trực tiếp các API của plugin, plugin này sử dụng cách tiếp cận "snapshot" — nó đọc các TODO từ cài đặt, hiển thị chúng dưới dạng HTML chỉ đọc, và cung cấp các hành động dựa trên thanh bên để thêm mục. Webview là một lớp **hiển thị**, không phải là một biểu mẫu tương tác.
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

### Ví dụ 3: Giám sát Lỗi

Giám sát đầu ra của terminal và gửi thông báo khi phát hiện các mẫu cụ thể.
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

## Phụ lục: Danh mục & Biểu tượng

### Danh mục Plugin (29)

Sử dụng những điều này trong `package.json` `keywords` hoặc khi gửi lên registry:

| Danh mục | Mô tả |
|----------|-------------|
| `server` | Quản lý máy chủ chung |
| `devtools` | Công cụ phát triển |
| `calculator` | Máy tính và bộ chuyển đổi |
| `simulator` | Trình giả lập |
| `game` | Trò chơi trên terminal |
| `business` | Công cụ kinh doanh |
| `security` | Bảo mật và kiểm toán |
| `web` | Quản lý máy chủ web |
| `education` | Công cụ giáo dục |
| `health` | Công cụ liên quan đến sức khỏe |
| `islamic` | Công cụ Hồi giáo (thời gian cầu nguyện, v.v.) |
| `science` | Công cụ khoa học |
| `quantum` | Công cụ điện toán lượng tử |
| `ai` | Công cụ sử dụng AI |
| `biotech` | Công cụ công nghệ sinh học |
| `space` | Công cụ không gian và thiên văn học |
| `network` | Công cụ mạng |
| `database` | Quản lý cơ sở dữ liệu |
| `monitoring` | Giám sát máy chủ |
| `devops` | DevOps và CI/CD |
| `utility` | Tiện ích chung |
| `design` | Công cụ thiết kế |
| `ecommerce` | Công cụ thương mại điện tử |
| `automation` | Công cụ tự động hóa |
| `kpop` | Công cụ liên quan đến K-pop |
| `accessibility` | Công cụ truy cập |
| `analytics` | Phân tích và báo cáo |
| `wia` | Công cụ hệ sinh thái WIA |
| `all` | Xuất hiện trong tất cả các danh mục |

### Biểu tượng Được Khuyến Nghị (Lucide)

| Tên Biểu Tượng | Sử dụng cho |
|-----------|---------|
| `server` | Quản lý máy chủ |
| `shield` | Bảo mật |
| `database` | Cơ sở dữ liệu |
| `activity` | Giám sát |
| `terminal` | Công cụ terminal |
| `code` | Phát triển |
| `hard-drive` | Đĩa/lưu trữ |
| `network` | Mạng |
| `lock` | Xác thực/mã hóa |
| `eye` | Theo dõi/giám sát |
| `check-square` | Nhiệm vụ/TODO |
| `layout-dashboard` | Bảng điều khiển |
| `settings` | Cấu hình |
| `zap` | Tự động hóa |
| `globe` | Web/quốc tế |

Duyệt tất cả 1,500+ biểu tượng: [lucide.dev/icons](https://lucide.dev/icons)

---

## Cần Giúp Đỡ?

- **Vấn đề trên GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Vấn đề Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Ví Dụ:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Xây dựng điều gì đó tuyệt vời. Chia sẻ nó với thế giới.</em></p>
<p align="center"><em>— Đội ngũ WIA SOOM</em></p>