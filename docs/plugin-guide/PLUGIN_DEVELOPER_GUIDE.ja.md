<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM プラグイン開発者ガイド</h1>
<p align="center"><strong>5 分で自分のプラグインを作成しましょう。</strong></p>
<p align="center">WIA SOOM 内で強力なサーバーツール、ダッシュボード、オートメーションを作成します。</p>

---

## 目次

- [第 1 部: クイックスタート — 5 分で最初のプラグインを作成](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [第 2 部: プラグインコンテキスト API リファレンス](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [第 3 部: Webview を使ったカスタム UI の構築](#part-3-building-custom-ui-with-webviews)
- [第 4 部: プラグインの公開](#part-4-publishing-your-plugin)
- [第 5 部: ベストプラクティス](#part-5-best-practices)
- [第 6 部: 実世界の例](#part-6-real-world-examples)
- [付録: カテゴリとアイコン](#appendix-categories--icons)

---

## 第 1 部: クイックスタート — 5 分で最初のプラグインを作成

### 作成するもの

サイドバーにボタンを追加する「Hello World」プラグイン。クリックすると通知が表示されます。

### ステップ 1: プラグインフォルダーを作成
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### ステップ 2: package.json を作成
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
**必須フィールド:** `name`, `version`, `description`, `author`, `main`

### ステップ 3: index.js を作成
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
### ステップ 4: WIA SOOM を再起動

アプリを再起動するか、設定 → プラグインでプラグインをオフ/オンに切り替えます。

サイドバーに **「Hello World」** ボタンが表示されるはずです。それをクリックすると、成功の通知が表示されます！

### 仕組み
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

## 第 2 部: プラグインコンテキスト API リファレンス

`activate(context)` 関数が呼び出されると、`context`（または `ctx`）はこれらの API を提供します：
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

### `ctx.terminal` — リモートサーバーでコマンドを実行

#### `terminal.send(sessionId, data)`

アクティブなターミナルセッションにコマンド（または任意のデータ）を送信します。

| パラメーター | 型 | 説明 |
|--------------|----|------|
| `sessionId`  | `string` | 送信先のターミナルセッション |
| `data`       | `string` | 送信するコマンドまたはデータ |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

ターミナルセッションからのすべての出力を購読します。**購読解除関数**を返します。

| パラメーター | 型 | 説明 |
|--------------|----|------|
| `sessionId`  | `string` | 監視するターミナルセッション |
| `callback`   | `(data: string) => void` | 各出力チャンクで呼び出されます |
| **返り値**   | `() => void` | これを呼び出してリスニングを停止します |
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
**重要:** 常に購読解除関数を保存し、`deactivate()` で呼び出してメモリリークを防いでください。

---

### `ctx.sftp` — ファイル転送

> **ステータス: 近日公開** — SFTP API は定義されていますが、アプリの SFTP エンジンにはまだ接続されていません���`list()` は現在空の���列を返し、`upload()`/`download()` は何もしません。これは今後のリリースで完全に実装されます。現時点では、`scp` または `rsync` コマンドを使って `ctx.terminal.send()` を代替手段として使用してください。

#### `sftp.list(sessionId, path)`

リモートディレクトリ内のファイルをリストします。
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

ローカルマシンからリモートサーバーにファイルをアップロードします。
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

リモートサーバーからローカルマシンにファイルをダウンロードします。
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**代替手段（SFTP API が稼働するまで）:**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — ユーザーインターフェース

#### `ui.addSidebarButton(options)`

WIA SOOM のサイドバーにボタンを追加します。

| オプション | 型 | 必須 | 説明 |
|------------|----|------|------|
| `id`       | `string` | いいえ | 一意の ID（デフォルトはプラグイン名） |
| `icon`     | `string` | はい | Lucide アイコン名（例: `'server'`, `'shield'`, `'database'`） |
| `label`    | `string` | はい | サイ��バーに表示されるボタンテキスト |
| `onClick`  | `() => void` | はい | ボタンがクリックされたときに呼び出される関数 |
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
**アイコンリファレンス:** 利用可能なアイコンは [lucide.dev/icons](https://lucide.dev/icons) で参照できます。

> **互換性の注意:** 一部の古いプラグインは、`addSidebarButton(id, icon, label, onClick)` のような位置引数を使用しています。公式 API は上記のように **オプションオブジェクト** を使用します。新しいプラグインには常にオブジェクトスタイルを使用してください。

#### `ui.openWebview(options)`

カスタム HTML コンテンツを持つポップアップウィンドウを開きます。これがリッチな UI を構築する方法です。

| オプション | 型 | 説明 |
|------------|----|------|
| `title`    | `string` | ウィンドウのタイトル |
| `html`     | `string` | レンダリングする完全な HTML コンテンツ |
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
> [パート3](#part-3-building-custom-ui-with-webviews) で高度なウェブビューのパターンを参照してください。

#### `ui.showNotification(type, message)`

トースト通知を表示します。

| パラメータ | 型 | 説明 |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | 通知スタイル |
| `message` | `string` | 表示するテキスト |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

下部ステータスバーに永続的なテキストアイテムを追加します。

| パラメータ | 型 | 説明 |
|-----------|------|-------------|
| `id` | `string` | このステータスアイテムのユニークID |
| `text` | `string` | 表示するテキスト |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — 永続ストレージ

プラグイン設定は `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` に永続的に保存されます。

#### `settings.get(key)`

保存された値を読み取ります。
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
キーが存在しない場合は `undefined` を返し���す。

#### `settings.set(key, value)`

値を保存します。文字列、数値、ブール値、配列、オブジェクトをサポートしています。
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**例: ユーザーの設定を記憶する**
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

### `ctx.ai` — AI統合

> **ステータス: 近日公開** — AI APIは定義されていますが、まだSoomyに接続されていません。現在は `{ response: 'AI not yet connected' }` を返します。完全なAI統合は将来のリリースで予定されています。

#### `ai.chat(messages, options?)`

AIアシスタント（Soomy）にメッセージを送信します。
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## パート3: ウェブビューを使ったカスタムUIの構築

`openWebview()` APIを使用すると、HTML、CSS、JavaScriptを使用してダッシュボードUIをポップアップウィンドウ内に構築できます。

> **重要な制限:** ウェブビューは**表示専用**です。プラグインAPI（`ctx.settings`、`ctx.terminal`など）を呼び出すことはできません。すべてのユーザーアクションにはサイドバーボタンを使用し、現在の状態を表示するには `openWebview()` を使用してください。インタラクティブな機能が必要な場合は、サイドバーボタンからトリガーし、表示を更新するためにウェブビューを再オープンしてください。

### パターン: ターミナルコマンド → 出力を解析 → HTMLで表示

これは最も一般的なプラグインパターンです。コマンドを実行し、結果を解析して視覚的に表示します。
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
### パターン: 自動更新付きインタラクティブダッシュボード
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
### パターン: ウェブビューでの設定の表示

> **注意:** ウェブビューは表示専用です — プラグインAPIを呼び出すことはできません。設定を変更するにはサイドバーボタンハンドラーで `ctx.settings` を使用し、現在の状態を表示するには `openWebview()` を使用してください。
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

## パート4: プラグインの公開

### ステップ1: ローカルでテスト

1. プラグインを `~/.wia-soom/plugins/{your-plugin}/` にコピーします。
2. WIA SOOMを再起動します。
3. 正常に動作することを確認します: サイドバーボタンが表示され、機能が正しく動作することを確認します。
4. エッジケースをテストします: ターミナルが接続���れていない場合はどうなりますか？

### ステップ2: 提出の準備

プラグインフォルダには次のものが含まれている必要があります:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**必要な `package.json` フィールド:**

| フィールド | 説明 | 例 |
|-------|-------------|---------|
| `name` | ユニークなケバブケースのID | `"my-awesome-plugin"` |
| `version` | セマンティックバージョン | `"1.0.0"` |
| `description` | 一行の説明 | `"Monitors nginx access logs in real-time"` |
| `author` | あなたの名前 | `"John Doe"` |
| `main` | エントリーポイント | `"index.js"` |

**オプションフィールド:**

| フィールド | 説明 |
|-------|-------------|
| `license` | ライセンスタイプ（MIT推奨） |
| `keywords` | 検索タグの配列 |
| `soom.minVersion` | 必要な最小WIA SOOMバージョン |

### ステップ3: プラグインレジストリに提出

1. **フォーク** [Plugin Store](https://wiasoom.com)
2. **追加** `plugins/{your-plugin-name}/` にあなたのプラグインを
3. **提出** プルリクエスト

### ステップ4: レビューと承認

私たちはすべてのプラグインを以下の点でレビューします:

- **セキュリティ** — 危険なAPIは使用しない（[セキュリティルール](#security-rules)を参照）
- **品質** — 動作し��すか？ コードはクリーンですか？
- **有用性** — 実際の問題を解決しますか？

承認後:
1. あなたのプラグインは `registry.json` に追加されます
2. `dist/` にZIPバンドルが作成されます
3. あなたのプラグインはすべてのWIA SOOMユーザーのために**プラグインストア**に表示されます！

---

## パート5: ベストプラクティス

### セキュリティルール

これらのルールは**必須**です。これに違反するプラグインは拒否されます。

| ルール | 理由 |
|------|-----|
| **決して** `eval()` または `new Function()` を使用しない | コードインジェクションのリスク |
| **決して** `child_process`, `exec()`, `spawn()` を使用しない | コマンドには `ctx.terminal.send()` のみを使用 |
| **決して** 外部URLを取得しない | 例外: `wiasoom.com` APIエンドポイント |
| **決して** `process.env` にアクセスしない | 環境変数には秘密が含まれる可能性があります |
| **決して** `require('fs')` を直接使用しない | ストレージには `ctx.settings` を、ファイル転送には `ctx.sftp` を使用 |
| **決して** npmの外部パッケージを使用しない | 純粋なJavaScriptのみ — node_modulesは使用しない |
| **すべてのリモートコマンドには** `ctx.terminal.send()` を使用する必要があります | これは安全なSSHチャネルを通過します |
| **`deactivate()` でクリーンアップする必要があります** | リスナーを削除し、インターバルをクリアする |

### エラーハンドリング

リスクのある操作は常にtry/catchでラップしてください：
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
### deactivate() でのクリーンアップ

プラグインがインターバル、リスナー、またはサブスクリプションを作成する場合 — それらをクリーンアップしてください：
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
### i18nサポート

WIA SOOMは254の言語をサポートしています。プラグインラベルを翻訳可能にするためには、シンプルなアプローチを使用してください：
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

## パート6: 実世界の例

### 例1: サーバーディスクチェッカー

リモートサーバーで `df -h` を実行し、ステータスバーに使用中/空きスペースを表示します。
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

### 例2: TODOマネージャー

設定を使用して永続的なストレージを管���し、表示にはウェブビューを使用するTODOリストを管理するプラグインです。

> **デザインパターン:** ウェブビューはプラグインAPIを直接呼び出すことができないため、このプラグインは「スナップショット」アプローチを使用します — 設定からTODOを読み取り、読み取り専用のHTMLとしてレンダリングし、アイテム追加のためのサイドバーアクションを提供します。ウェブビューは**表示**レイヤーであり、インタラクティブなフォームではありません。
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

### 例3: エラーウォッチャー

ターミナルの出力を監視し、特定のパターンが検出されたときに通知を送信します。
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

## 付録: カテゴリとアイコン

### プラグインカテゴリ (29)

これらを `package.json` の `keywords` に使用するか、レジストリに提出する際に使用してください:

| カテゴリ | 説明 |
|----------|-------------|
| `server` | 一般的なサーバー管理 |
| `devtools` | 開発ツール |
| `calculator` | 電卓とコンバーター |
| `simulator` | シミュレーター |
| `game` | ターミナルゲーム |
| `business` | ビジネスツール |
| `security` | セキュリティと監査 |
| `web` | ウェブサーバー管理 |
| `education` | 教育ツール |
| `health` | 健康関連ツール |
| `islamic` | イスラム関連ツール（祈りの時間など） |
| `science` | 科学ツール |
| `quantum` | 量子コンピューティングツール |
| `ai` | AI駆動のツール |
| `biotech` | バイオテクノロジーツール |
| `space` | 宇宙と天文学のツール |
| `network` | ネットワークツール |
| `database` | データベース管理 |
| `monitoring` | サーバー監視 |
| `devops` | DevOpsとCI/CD |
| `utility` | 一般的なユーティリティ |
| `design` | デザインツール |
| `ecommerce` | Eコマースツール |
| `automation` | 自動化ツール |
| `kpop` | K-pop関連ツール |
| `accessibility` | アクセシビリティツール |
| `analytics` | 分析と報告 |
| `wia` | WIAエコシステムツール |
| `all` | すべてのカテゴリに表示される |

### 推奨アイコン (Lucide)

| アイコン名 | 使用目的 |
|-----------|---------|
| `server` | サーバー管理 |
| `shield` | セキュリティ |
| `database` | データベース |
| `activity` | 監視 |
| `terminal` | ターミナルツール |
| `code` | 開発 |
| `hard-drive` | ディスク/ストレージ |
| `network` | ネットワーキング |
| `lock` | 認証/暗号化 |
| `eye` | 監視/モニタリング |
| `check-square` | タスク/TODO |
| `layout-dashboard` | ダッシュボード |
| `settings` | 設定 |
| `zap` | 自動化 |
| `globe` | ウェブ/国際 |

すべての1,500以上のアイコンをブラウズ: [lucide.dev/icons](https://lucide.dev/icons)

---

## 助けが必要ですか？

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **プラグインの問題:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **サンプルプラグイン:** [Website](https://wiasoom.com)
- **ウェブサイト:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>素晴らしいものを作りましょう。世界と共有しましょう。</em></p>
<p align="center"><em>— WIA SOOMチーム</em></p>