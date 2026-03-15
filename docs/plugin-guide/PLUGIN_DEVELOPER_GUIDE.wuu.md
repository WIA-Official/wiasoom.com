<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM 插件开发者指南</h1>
<p align="center"><strong>5分钟内构建你自己的插件。</strong></p>
<p align="center">在 WIA SOOM 内部创建强大的服务器工具、仪表板和自动化。</p>

---

## 目录

- [第一部分：快速入门 — 你的第一个插件在5分钟内](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [第二部分：插件上下文 API 参考](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [第三部分：使用 Webviews 构建自定义 UI](#part-3-building-custom-ui-with-webviews)
- [第四部分：发布你的插件](#part-4-publishing-your-plugin)
- [第五部分：最佳实践](#part-5-best-practices)
- [第六部分：真实世界的例子](#part-6-real-world-examples)
- [附录：类别与图标](#appendix-categories--icons)

---

## 第一部分：快速入门 — 你的第一个插件在5分钟内

### 你将构建什么

一个“Hello World”插件，向侧边栏添加一个按钮。点击时，会显示一个通知。

### 步骤 1：创建插件文件夹
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 步骤 2：创建 package.json
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
**必填字段：** `name`, `version`, `description`, `author`, `main`

### 步骤 3：创建 index.js
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
### 步骤 4：重启 WIA SOOM

重启应用（或在设置 → 插件中切换插件的开/关）。

你应该在侧边栏看到一个 **"Hello World"** 按钮。点击它 — 你会看到一个成功的通知！

### 它是如何工作的
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

## 第二部分：插件上下文 API 参考

当你的 `activate(context)` 函数被调用时，`context`（或 `ctx`）提供这些 API：
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

### `ctx.terminal` — 在远程服务器上运行命令

#### `terminal.send(sessionId, data)`

向活动终端会话发送命令（或任何数据）。

| 参数 | 类型 | 描述 |
|-----------|------|-------------|
| `sessionId` | `string` | 要发送到的终端会话 |
| `data` | `string` | 要发送的命令或数据 |
§§§CHUNK_SEPARATOR��§§
#### `terminal.onOutput(sessionId, callback)`

订阅来自终端会话的所有输出。返回一个 **取消订阅函数**。

| 参数 | 类型 | 描述 |
|-----------|------|-------------|
| `sessionId` | `string` | 要监视的终端会话 |
| `callback` | `(data: string) => void` | 每次输出块时调用 |
| **返回** | `() => void` | 调用此函数以停止监听 |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
**重要：** 始终保存取消订阅函数，并在 `deactivate()` 中调用它以防止内存泄漏。

---

### `ctx.sftp` — 文件传输

> **状态：即将推出** — SFTP API 已定义，但尚未连接到应用的 SFTP 引擎。`list()` 目前返回一个空数组，`upload()`/`download()` 是无操作的。这将在未来的版本中完全实现。现在，使用 `ctx.terminal.send()` 和 `scp` 或 `rsync` 命令作为解决方法。

#### `sftp.list(sessionId, path)`

列出远程目录中的文件。
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

将文件从本地计算机上传到远程服务器。
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.download(sessionId, remotePath, localPath)`

将文件从远程服务器下载到本地计算机。
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
**解决方法（直到 SFTP API 上线）：**
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

### `ctx.ui` — 用户界面

#### `ui.addSidebarButton(options)`

向 WIA SOOM 侧边栏添加一个按钮。

| 选项 | 类型 | 必填 | 描述 |
|--------|------|----------|-------------|
| `id` | `string` | 否 | 唯一 ID（默认为插件名称） |
| `icon` | `string` | 是 | Lucide 图标名称（例如，`'server'`、`'shield'`、`'database'`） |
| `label` | `string` | 是 | 在侧边栏中显示的按钮文本 |
| `onClick` | `() => void` | 是 | 按钮被点击时调用的函数 |
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**图标参考：** 在 [lucide.dev/icons](https://lucide.dev/icons) 浏览所有可用图标

> **兼容性说明：** 一些旧插件使用位置参数，如 `addSidebarButton(id, icon, label, onClick)`。官方 API 使用上面文档中的 **选项对象**。新插件始终使用对象样式。

#### `ui.openWebview(options)`

打开一个带有自定义 HTML 内容的弹出窗口。这是构建丰富 UI 的方式。

| 选项 | 类型 | 描述 |
|--------|------|-------------|
| `title` | `string` | 窗口标题 |
| `html` | `string` | 要渲染的完整 HTML 内容 |
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
> 查看 [第 3 部分](#part-3-building-custom-ui-with-webviews) 以获取高级 webview 模式。

#### `ui.showNotification(type, message)`

显示一个 toast 通知。

| 参数 | 类型 | 描述 |
|------|------|------|
| `type` | `'success' \| 'error' \| 'info'` | 通知样式 |
| `message` | `string` | 要显示的文本 |
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

在底部状态栏添加一个持久的文本项。

| 参数 | 类型 | 描述 |
|------|------|------|
| `id` | `string` | 此状态项的唯一 ID |
| `text` | `string` | 要显示的文本 |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### `ctx.settings` — 持久存储

插件设置永久存储在 `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` 中。

#### `settings.get(key)`

读取保存的值。
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
如果键不存在，则返回 `undefined`。

#### `settings.set(key, value)`

保存一个值。支持字符串、数字、布尔值、数组和对象。
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
**示例：记住用户偏好**
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

### `ctx.ai` — AI 集成

> **状态：即将推出** — AI API 已定义，但尚未连接到 Soomy。目前返回 `{ response: 'AI not yet connected' }`。完整的 AI 集成计划在未来版本中实现。

#### `ai.chat(messages, options?)`

向 AI 助手（Soomy）发送消息。
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

## 第 3 部分：使用 Webviews 构建自定义 UI

`openWebview()` API 让你可以使用 HTML、CSS 和 JavaScript 构建仪表板 UI — 所有这些都在一个弹出窗口中。

> **重要限制：** Webviews 是 **仅用于显示**。它们无法回调插件 API（`ctx.settings`、`ctx.terminal` 等）。使用侧边栏按钮进行所有用户操作，并使用 `openWebview()` 显示当前状态。如果需要交互功能，请从侧边栏按钮触发它们，并重新打开 webview 以刷新显示。

### 模式：终端命令 → 解析输出 → 在 HTML 中显示

这是最常见的插件模式。你运行一个命令，解析结果，并以可视化方式显示。
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
### 模式：具有自动刷新的交互式仪表板
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
### 模式：在 Webview 中显示设置

> **注意：** Webviews 仅用于显示 — 它们无法回调插件 API。在你的侧边栏按钮处理程序中使用 `ctx.settings` 来修改设置，并使用 `openWebview()` 显示当前状态。
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

## 第 4 部分：发布你的插件

### 第 1 步：本地测试

1. 将你的插件复制到 `~/.wia-soom/plugins/{your-plugin}/`
2. 重启 WIA SOOM
3. 验证它是否正常工作：侧边栏按钮出现，功能正常
4. 测试边缘情况：如果没有连接终端会发生什么？

### 第 2 步：准备提交

你的插件文件夹必须包含：
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
**必要的 `package.json` 字段：**

| 字段 | 描述 | 示例 |
|-------|-------------|---------|
| `name` | 唯一的 kebab-case ID | `"my-awesome-plugin"` |
| `version` | 语义版本 | `"1.0.0"` |
| `description` | 一行描述 | `"实时监控 nginx 访问日志"` |
| `author` | 你的名字 | `"John Doe"` |
| `main` | 入口点 | `"index.js"` |

**可选字段：**

| 字段 | 描述 |
|-------|-------------|
| `license` | 许可证类型（推荐 MIT） |
| `keywords` | 搜索标签数组 |
| `soom.minVersion` | 所需的最低 WIA SOOM 版本 |

### 第 3 步：提交到插件注册表

1. ****Package** your plugin as a ZIP file
2. **添加** 你的插件到 `plugins/{your-plugin-name}/`
3. **提交** 一个 Pull Request

### 第 4 步：审核和批准

我们会审核每个插件，关注以下几点：

- **安全性** — 不得使用危险的 API（见 [安全规则](#security-rules)）
- **质量** — 它能正常工作吗？代码是否干净？
- **实用性** — 它解决了实际问题吗？

审核通过后：
1. 你的插件会被添加到 `registry.json`
2. 在 `dist/` 中创建一个 ZIP 包
3. 你的插件会出现在所有 WIA SOOM 用户的 **插件商店** 中！

---

## 第 5 部分：最佳实践

### 安全规则

这些规则是 **强制性的**。违反这些规则的插件将被拒绝。

| 规则 | 原因 |
|------|-----|
| **绝对不要** 使用 `eval()` 或 `new Function()` | 代码注入风险 |
| **绝对不要** 使用 `child_process`、`exec()`、`spawn()` | 仅使用 `ctx.terminal.send()` 发送命令 |
| **绝对不要** 获取外部 URL | 例外：`wiasoom.com` API 端点 |
| **绝对不要** 访问 `process.env` | 环境变量可能包含秘密 |
| **绝对不要** 直接使用 `require('fs')` | 使用 `ctx.settings` 存储，`ctx.sftp` 进行文件传输 |
| **绝对不要** 使用 npm 外部包 | 仅限纯 JavaScript — 不得使用 node_modules |
| **必须** 使用 `ctx.terminal.send()` 发送所有远程命令 | 这通过安全的 SSH 通道进行 |
| **必须** 在 `deactivate()` 中清理 | 移除监听器，清除间隔 |

### 错误处理

始终将风险操作包裹在 try/catch 中：
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
### 在 deactivate() 中清理

如果你的插件创建了间隔、监听器或订阅 — 请清理它们：
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
### i18n 支持

WIA SOOM 支持 254 种语言。为了使你的插件标签可翻译，使用简单的方法：
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

## 第 6 部分：现实世界的示例

### 示例 1：服务器磁��检查器

在远程服务器上运行 `df -h`，并在状态栏中显示已用/可用空间。
§§§CHUNK_SEPARATOR§§§
---

### 示例 2：TODO 管理器

一个使用设置进行持久存储和使用 webview 进行显示的 TODO 列表管理插件。

> **设计模式：** 由于 webviews 不能直接调用插件 API，因此该插件使用了“快照”方法 — 它从设置中读取 TODO，将其渲染为只读 HTML，并提供基于侧边栏的添加项目操作。webview 是一个 **显示** 层，而不是交互表单。
§§§CHUNK_SEPARATOR§§§
---

### 示例 3：错误监视器

监控终端输出，并在检测到特定模式时发送通知。
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

## 附录：类别与图标

### 插件类别 (29)

在你的 `package.json` `keywords` 中使用这些，或者在提交到注册表时使用：

| 类别 | 描述 |
|------|------|
| `server` | 一般服务器管理 |
| `devtools` | 开发工具 |
| `calculator` | 计算器和转换器 |
| `simulator` | 模拟器 |
| `game` | 终端游戏 |
| `business` | 商务工具 |
| `security` | 安全与审计 |
| `web` | 网络服务器管理 |
| `education` | 教育工具 |
| `health` | 健康相关工具 |
| `islamic` | 伊斯兰工具（祷告时间等） |
| `science` | 科学工具 |
| `quantum` | 量子计算工具 |
| `ai` | 人工智能工具 |
| `biotech` | 生物技术工具 |
| `space` | 空间与天文学工具 |
| `network` | 网络工具 |
| `database` | 数据库管理 |
| `monitoring` | 服务器监控 |
| `devops` | DevOps 和 CI/CD |
| `utility` | 一般实用工具 |
| `design` | 设计工具 |
| `ecommerce` | 电子商务工具 |
| `automation` | 自动化工具 |
| `kpop` | K-pop 相关工具 |
| `accessibility` | 可访问性工具 |
| `analytics` | 分析与报告 |
| `wia` | WIA 生态系统工具 |
| `all` | 出现在所有类别中 |

### 推荐图标 (Lucide)

| 图标名称 | 用途 |
|----------|------|
| `server` | 服务器管理 |
| `shield` | 安全 |
| `database` | 数据库 |
| `activity` | 监控 |
| `terminal` | 终端工具 |
| `code` | 开发 |
| `hard-drive` | 硬盘/存储 |
| `network` | 网络连接 |
| `lock` | 认证/加密 |
| `eye` | 观察/监控 |
| `check-square` | 任务/TODO |
| `layout-dashboard` | 仪表板 |
| `settings` | 配置 |
| `zap` | 自动化 |
| `globe` | 网络/国际 |

浏览所有 1,500+ 图标：[lucide.dev/icons](https://lucide.dev/icons)

---

## 需要帮助吗？

- **GitHub 问题:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **插件问题:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **示例插件:** [Website](https://wiasoom.com)
- **网站:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>构建一些惊人的东西。与世界分享。</em></p>
<p align="center"><em>— WIA SOOM 团队</em></p>
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
