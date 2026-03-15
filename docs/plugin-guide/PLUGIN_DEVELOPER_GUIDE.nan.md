<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM 插件開發者指南</h1>
<p align="center"><strong>5分鐘內建立你自己的插件。</strong></p>
<p align="center">在 WIA SOOM 裡面創建強大的伺服器工具、儀表板和自動化功能。</p>

---

## 目錄

- [第一部分：快速開始 — 你的第一個插件在 5 分鐘內](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [第二部分：插件上下文 API 參考](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [第三部分：使用 Webviews 建立自訂 UI](#part-3-building-custom-ui-with-webviews)
- [第四部分：發佈你的插件](#part-4-publishing-your-plugin)
- [第五部分：最佳實踐](#part-5-best-practices)
- [第六部分：��際案例](#part-6-real-world-examples)
- [附錄：類別與圖示](#appendix-categories--icons)

---

## 第一部分：快速開始 — 你的第一個插件在 5 分鐘內

### 你將建立什麼

一個「Hello World」插件，會在側邊欄添加一個按鈕。當按下時，它會顯示一個通知。

### 步驟 1：創建插件資料夾
§§§CHUNK_SEPARATOR§§§
### 步驟 2：創建 package.json
§§§CHUNK_SEPARATOR§§§
**必填欄位：** `name`, `version`, `description`, `author`, `main`

### 步驟 3：創建 index.js
§§§CHUNK_SEPARATOR§§§
### 步驟 4：重啟 WIA SOOM

重啟應用程式（或在設定 → 插件中切換插件的開關）。

你應該會在側邊欄看到一個 **"Hello World"** 按鈕。點擊它 — 你會看到一個成功的通知！

### 它是如何運作的
§§§CHUNK_SEPARATOR§§§
---

## 第二部分：插件上下文 API 參考

當你的 `activate(context)` 函數被呼叫時，`context`（或 `ctx`）提供這些 API：
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — 在遠端伺服器上運行命令

#### `terminal.send(sessionId, data)`

將命令（或任何數據）發送到一個活躍的終端會話。

| 參數 | 類型 | 描述 |
|-----------|------|-------------|
| `sessionId` | `string` | 要發送到的終端會話 |
| `data` | `string` | 要發送的命令或數據 |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

訂閱來自終端會話的所有輸出。返回一個 **取消訂閱函數**。

| 參數 | 類型 | 描述 |
|-----------|------|-------------|
| `sessionId` | `string` | 要監視的終端會話 |
| `callback` | `(data: string) => void` | 每次輸出時被呼叫 |
| **返回** | `() => void` | 呼叫此函數以停止監聽 |
§§§CHUNK_SEPARATOR§§§
**重要：** 總是保存取消訂閱函數並在 `deactivate()` 中呼叫它，以防止內存洩漏。

---

### `ctx.sftp` — 文件傳輸

> **狀態：即將推出** — SFTP API 已經定義，但尚未連接到應用的 SFTP 引擎。`list()` 目前返回一個空數組，而 `upload()`/`download()` 是無操作。這將在未來的版本中完全實現。現在，使用 `ctx.terminal.send()` 搭配 `scp` 或 `rsync` 命令作為變通方法。

#### `sftp.list(sessionId, path)`

列出遠端目錄中的文件。
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

將文件從本地機器上傳到遠端伺服器。
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

將文件從遠端伺服器下載到本地機器。
§§§CHUNK_SEPARATOR§§§
**變通方法（直到 SFTP API 上線）：**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — 用戶界面

#### `ui.addSidebarButton(options)`

在 WIA SOOM 側邊欄添加一個按鈕。

| 選項 | 類型 | 必填 | 描述 |
|--------|------|----------|-------------|
| `id` | `string` | 否 | 唯一 ID（默認為插件名稱） |
| `icon` | `string` | 是 | Lucide 圖示名稱（例如，`'server'`、`'shield'`、`'database'`） |
| `label` | `string` | 是 | 在側邊欄顯示的按鈕文本 |
| `onClick` | `() => void` | 是 | 按鈕被點擊時呼叫的函數 |
§§§CHUNK_SEPARATOR§§§
**圖示參考：** 瀏覽所有可用圖示在 [lucide.dev/icons](https://lucide.dev/icons)

> **相容性注意：** 一些舊插件使用位置參數，如 `addSidebarButton(id, icon, label, onClick)`。官方 API 使用如上所述的 **選項對象**。對於新插件，請始終使用對象樣式。

#### `ui.openWebview(options)`

打開一個彈出窗口，顯示自訂 HTML 內容。這是建立豐富 UI 的方式。

| 選項 | 類型 | 描述 |
|--------|------|-------------|
| `title` | `string` | 窗口標題 |
| `html` | `string` | 要渲染的���整 HTML 內容 |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> 看 [第 3 部分](#part-3-building-custom-ui-with-webviews) 了解進階的 webview 模式。

#### `ui.showNotification(type, message)`

顯示一個 toast 通知。

| 參數 | 類型 | 描述 |
|------|------|------|
| `type` | `'success' \| 'error' \| 'info'` | 通知樣式 |
| `message` | `string` | 要顯示的文本 |
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
#### `ui.addStatusBarItem(id, text)`

在底部狀態欄添加一個持久的文本項目。

| 參數 | 類型 | 描述 |
|------|------|------|
| `id` | `string` | 此狀態項目的唯一 ID |
| `text` | `string` | 要顯示的文本 |
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
---

### `ctx.settings` — 持久存儲

插件設置永久存儲在 `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` 中。

#### `settings.get(key)`

讀取已保存的值。
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
如果鍵不存在，返回 `undefined`。

#### `settings.set(key, value)`

保存一個值。支持字符串、數字、布林值、數組和對象。
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**範例：記住用戶偏好**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — AI 整合

> **狀態：即將推出** — AI API 已定義，但尚未連接到 Soomy。目前返回 `{ response: 'AI not yet connected' }`。完整的 AI 整合計劃在未來的版本中實現。

#### `ai.chat(messages, options?)`

向 AI 助手（Soomy）發送消息。
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

## 第 3 部分：使用 Webviews 構建自定義 UI

`openWebview()` API 讓你可以用 HTML、CSS 和 JavaScript 構建儀表板 UI — 所有內容都在彈出窗口內。

> **重要限制：** Webviews 是 **僅顯示**。它們不能回調插件 API（`ctx.settings`、`ctx.terminal` 等）。對於所有用戶操作，請使用側邊按鈕，並使用 `openWebview()` 來顯示當前狀態。如果需要互動功能，請從側邊按鈕觸發它們，並重新打開 webview 以刷新顯示。

### 模式：終端命令 → 解析輸出 → 在 HTML 中顯示

這是最常見的插件模式。你運行一個命令，解析結果，並以視覺方式顯示它。
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### 模式：自動刷新互動儀表板
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### 模式：在 Webview 中顯示設置

> **注意：** Webviews 僅顯示 — 它們不能回調插件 API。請在側邊按鈕處理程序中使用 `ctx.settings` 來修改設置，並使用 `openWebview()` 來顯示當前狀��。
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## 第 4 部分：發布你的插件

### 步驟 1：本地測試

1. 將你的插件複製到 `~/.wia-soom/plugins/{your-plugin}/`
2. 重新啟動 WIA SOOM
3. 驗證其是否正常運作：側邊按鈕出現，功能正常
4. 測試邊緣情況：如果沒有終端連接會發生什麼？

### 步驟 2：準備提交

你的插件文件夾必須包含：
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**必需的 `package.json` 欄位：**

| 欄位 | 描述 | 範例 |
|-------|-------------|---------|
| `name` | 獨特的 kebab-case ID | `"my-awesome-plugin"` |
| `version` | 語意版本 | `"1.0.0"` |
| `description` | 一行描述 | `"即時監控 nginx 訪問日誌"` |
| `author` | 你的名字 | `"John Doe"` |
| `main` | 入口點 | `"index.js"` |

**選用欄位：**

| 欄位 | 描述 |
|-------|-------------|
| `license` | 授權類型（建議使用 MIT） |
| `keywords` | 搜尋標籤的陣列 |
| `soom.minVersion` | 所需的最低 WIA SOOM 版本 |

### 第 3 步：提交到插件註冊中心

1. ****Package** your plugin as a ZIP file
2. **新增** 你的插件到 `plugins/{your-plugin-name}/`
3. **提交** 一個 Pull Request

### 第 4 步：審核與批准

我們會審核每個插件以確保：

- **安全性** — 無危險的 API（參見 [安全規則](#security-rules)）
- **品質** — 它能正常運作嗎？代碼乾淨嗎？
- **實用性** — 它解決了真實的問題嗎？

批准後：
1. 你的插件會被新增到 `registry.json`
2. 在 `dist/` 中創建一個 ZIP 包
3. 你的插件會出現在所有 WIA SOOM 用戶的 **Plugin Store** 中！

---

## 第 5 部分：最佳實踐

### 安全規則

這些規則是 **強制性的**。違反這些規則的插件將被拒絕。

| 規則 | 原因 |
|------|-----|
| **絕對不要** 使用 `eval()` 或 `new Function()` | 代碼注入風險 |
| **絕對不要** 使用 `child_process`, `exec()`, `spawn()` | 只使用 `ctx.terminal.send()` 來執行命令 |
| **絕對不要** 獲取外部 URL | 例外：`wiasoom.com` API 端點 |
| **絕對不要** 訪問 `process.env` | 環境變數可能包含秘密 |
| **絕對不要** 直接使用 `require('fs')` | 使用 `ctx.settings` 進行存儲，使用 `ctx.sftp` 進行文件傳輸 |
| **絕對不要** 使用 npm 外部包 | 僅限純 JavaScript — 不要有 node_modules |
| **必須** 使用 `ctx.terminal.send()` 來執行所有遠程命令 | 這是通過安全的 SSH 通道進行的 |
| **必須** 在 `deactivate()` 中進行清理 | 移除監聽器，清除間隔 |

### 錯誤處理

始終將風險操作包裹在 try/catch 中：
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
### 在 deactivate() 中清理

如果你的插件創建了間隔、監聽器或訂閱 — 請進行清理：
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
### i18n 支援

WIA SOOM 支援 254 種語言。要使你的插件標籤可翻譯，請使用��單的方法：
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## 第 6 部分：實際案例

### 案例 1：伺服器磁碟檢查器

在遠程伺服器上運行 `df -h`，並在狀態欄中顯示已用/可用空間。
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### 案例 2：TODO 管理器

一個使用設置進行持久存儲和使用網頁視圖顯示的 TODO 列表管理插件。

> **設計模式：** 由於網頁視圖無法直接調用插件 API，這個插件使用了「快照」方法 — 它從設置中讀取 TODO，將其呈現為只讀 HTML，並提供基於側邊欄的操作來添加項目。網頁視圖是一個 **顯示** 層，而不是一個互動表單。
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### 案例 3：錯誤監視器

監控終端輸出，當檢測到特定模式時發送通知。
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## 附錄：類別與圖示

### 插件類別 (29)

在你的 `package.json` `keywords` 或提交到註冊中心時使用這些：

| 類別 | 描述 |
|------|------|
| `server` | 一般伺服器管理 |
| `devtools` | 開發工具 |
| `calculator` | 計算器與轉換器 |
| `simulator` | 模擬器 |
| `game` | 終端遊戲 |
| `business` | 商業工具 |
| `security` | 安全與審計 |
| `web` | 網頁伺服器管理 |
| `education` | 教育工具 |
| `health` | 健康相關工具 |
| `islamic` | 伊斯蘭工具（祈禱時間等） |
| `science` | 科學工具 |
| `quantum` | 量子計算工具 |
| `ai` | AI 驅動的工具 |
| `biotech` | 生物技術工具 |
| `space` | 太空與天文工具 |
| `network` | 網路工具 |
| `database` | 資料庫管理 |
| `monitoring` | 伺服器監控 |
| `devops` | DevOps 與 CI/CD |
| `utility` | 一般實用工具 |
| `design` | 設計工具 |
| `ecommerce` | 電子商務工具 |
| `automation` | 自動化工具 |
| `kpop` | K-pop 相關工具 |
| `accessibility` | 可及性工具 |
| `analytics` | 分析與報告 |
| `wia` | WIA 生態系統工具 |
| `all` | 出現在所有類別中 |

### 推薦圖示 (Lucide)

| 圖示名稱 | 用途 |
|----------|------|
| `server` | 伺服器管理 |
| `shield` | 安全 |
| `database` | 資料庫 |
| `activity` | 監控 |
| `terminal` | 終端工具 |
| `code` | 開發 |
| `hard-drive` | 磁碟/儲存 |
| `network` | 網路連接 |
| `lock` | 認證/加密 |
| `eye` | 監視/監控 |
| `check-square` | 任務/TODO |
| `layout-dashboard` | 儀表板 |
| `settings` | 配置 |
| `zap` | 自動化 |
| `globe` | 網頁/國際 |

瀏覽所有 1,500+ 圖示：[lucide.dev/icons](https://lucide.dev/icons)

---

## 需要幫助嗎？

- **GitHub 問題：** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **插件問題：** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **範例插件：** [Website](https://wiasoom.com)
- **網站：** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>建造一些驚人的東西。與世界分享。</em></p>
<p align="center"><em>— WIA SOOM 團隊</em></p>
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
