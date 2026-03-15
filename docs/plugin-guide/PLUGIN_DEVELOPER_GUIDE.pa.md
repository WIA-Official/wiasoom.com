<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM ਪਲੱਗਇਨ ਵਿਕਾਸਕ ਗਾਈਡ</h1>
<p align="center"><strong>5 ਮਿੰਟਾਂ ਵਿੱਚ ਆਪਣਾ ਪਲੱਗਇਨ ਬਣਾਓ।</strong></p>
<p align="center">ਸ਼ਕਤੀਸ਼ਾਲੀ ਸਰਵਰ ਟੂਲ, ਡੈਸ਼ਬੋਰਡ ਅਤੇ ਆਟੋਮੇਸ਼ਨ ਬਣਾਓ — ਸਿੱਧਾ WIA SOOM ਦੇ ਅੰਦਰ।</p>

---

## ਸਮੱਗਰੀ ਦੀ ਸੂਚੀ

- [ਭਾਗ 1: ਤੁਰੰਤ ਸ਼ੁਰੂਆਤ — 5 ਮਿੰਟਾਂ ਵਿੱਚ ਤੁਹਾਡਾ ਪਹਿਲਾ ਪਲੱਗਇਨ](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ਭਾਗ 2: ਪਲੱਗਇਨ ਸੰਦਰਭ API ਹਵਾਲਾ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ਭਾਗ 3: ਵੈਬਵਿਊਜ਼ ਨਾਲ ਕਸਟਮ UI ਬਣਾਉਣਾ](#part-3-building-custom-ui-with-webviews)
- [ਭਾਗ 4: ਆਪਣੇ ਪਲੱਗਇਨ ਨੂੰ ਪ੍ਰਕਾਸ਼ਿਤ ਕਰਨਾ](#part-4-publishing-your-plugin)
- [ਭਾਗ 5: ਬਿਹਤਰ ਅਭਿਆਸ](#part-5-best-practices)
- [ਭਾਗ 6: ਵਾਸਤਵਿਕ ਉਦਾਹਰਣ](#part-6-real-world-examples)
- [ਅਨੁਸੂਚੀ: ਸ਼੍ਰੇਣੀਆਂ ਅਤੇ ਆਈਕਨ](#appendix-categories--icons)

---

## ਭਾਗ 1: ਤੁਰੰਤ ਸ਼ੁਰੂਆਤ — 5 ਮਿੰਟਾਂ ਵਿੱਚ ਤੁਹਾਡਾ ਪਹਿਲਾ ਪਲੱਗਇਨ

### ਤੁਸੀਂ ਕੀ ਬਣਾਉਣ ਜਾ ਰਹੇ ਹੋ

ਇੱਕ "ਹੈਲੋ ਵਰਲਡ" ਪਲੱਗਇਨ ਜੋ ਸਾਈਡਬਾਰ ਵਿੱਚ ਇੱਕ ਬਟਨ ਜੋੜਦਾ ਹੈ। ਜਦੋਂ ਇਸ 'ਤੇ ਕਲਿਕ ਕੀਤਾ ਜਾਂਦਾ ਹੈ, ਇਹ ਇੱਕ ਨੋਟੀਫਿਕੇਸ਼ਨ ਦਿਖਾਉਂਦਾ ਹੈ।

### ਕਦਮ 1: ਪਲੱਗਇਨ ਫੋਲਡਰ ਬਣਾਓ
§§§CHUNK_SEPARATOR§§§
### ਕਦਮ 2: package.json ਬਣਾਓ
§§§CHUNK_SEPARATOR§§§
**ਜ਼ਰੂਰੀ ਖੇਤਰ:** `name`, `version`, `description`, `author`, `main`

### ਕਦਮ 3: index.js ਬਣਾਓ
§§§CHUNK_SEPARATOR§§§
### ਕਦਮ 4: WIA SOOM ਨੂੰ ਦੁਬਾਰਾ ਸ਼ੁਰੂ ਕਰੋ

ਐਪ ਨੂੰ ਦੁਬਾਰਾ ਸ਼ੁਰੂ ਕਰੋ (ਜਾਂ ਸੈਟਿੰਗਜ਼ → ਪਲੱਗਇਨ ਵਿੱਚ ਪਲੱਗਇਨ ਨੂੰ ਬੰਦ/ਚਾਲੂ ਕਰੋ)।

ਤੁਹਾਨੂੰ ਸਾਈਡਬਾਰ ਵਿੱਚ ਇੱਕ **"ਹੈਲੋ ਵਰਲਡ"** ਬਟਨ ਦੇਖਣਾ ਚਾਹੀਦਾ ਹੈ। ਇਸ 'ਤੇ ਕਲਿਕ ਕਰੋ — ਤੁਸੀਂ ਇੱਕ ਸਫਲਤਾ ਨੋਟੀਫਿਕੇਸ਼ਨ ਦੇਖੋਂਗੇ!

### ਇਹ ਕਿਵੇਂ ਕੰਮ ਕਰਦਾ ਹੈ
§§§CHUNK_SEPARATOR§§§
---

## ਭਾਗ 2: ਪਲੱਗਇਨ ਸੰਦਰਭ API ਹਵਾਲਾ

ਜਦੋਂ ਤੁਹਾਡਾ `activate(context)` ਫੰਕਸ਼ਨ ਕਾਲ ਕੀਤਾ ਜਾਂਦਾ ਹੈ, `context` (ਜਾਂ `ctx`) ਇਹ APIs ਪ੍ਰਦਾਨ ਕਰਦਾ ਹੈ:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — ਦੂਰ ਦੇ ਸਰਵਰਾਂ 'ਤੇ ਕਮਾਂਡ ਚਲਾਓ

#### `terminal.send(sessionId, data)`

ਇੱਕ ਕਮਾਂਡ (ਜਾਂ ਕੋਈ ਵੀ ਡੇਟਾ) ਇੱਕ ਸਰਗਰਮ ਟਰਮੀਨਲ ਸੈਸ਼ਨ ਵਿੱਚ ਭੇਜੋ।

| ਪੈਰਾਮੀਟਰ | ਕਿਸਮ | ਵੇਰਵਾ |
|-----------|------|-------------|
| `sessionId` | `string` | ਜਿਸ ਟਰਮੀਨਲ ਸੈਸ਼ਨ ਵਿੱਚ ਭੇਜਣਾ ਹੈ |
| `data` | `string` | ਭੇਜਣ ਲਈ ਕਮਾਂਡ ਜਾਂ ਡੇਟਾ |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

ਇੱਕ ਟਰਮੀਨਲ ਸੈਸ਼ਨ ਤੋਂ ਸਾਰੇ ਆਉਟਪੁੱਟ ਲਈ ਸਬਸਕ੍ਰਾਈਬ ਕਰੋ। ਇੱਕ **ਅਨਸਬਸਕ੍ਰਾਈਬ ਫੰਕਸ਼ਨ** ਵਾਪਸ ਕਰਦਾ ਹੈ।

| ਪੈਰਾਮੀਟਰ | ਕਿਸਮ | ਵੇਰਵਾ |
|-----------|------|-------------|
| `sessionId` | `string` | ਜਿਸ ਟਰਮੀਨਲ ਸੈਸ਼ਨ ਨੂੰ ਦੇਖਣਾ ਹੈ |
| `callback` | `(data: string) => void` | ਹਰ ਆਉਟਪੁੱਟ ਦੇ ਟੁਕੜੇ ਨਾਲ ਕਾਲ ਕੀਤਾ ਜਾਂਦਾ ਹੈ |
| **ਵਾਪਸ** | `() => void` | ਸੁਣਨਾ ਰੋਕਣ ਲਈ ਇਸਨੂੰ ਕਾਲ ਕਰੋ |
§§§CHUNK_SEPARATOR§§§
**ਮਹੱਤਵਪੂਰਨ:** ਹਮੇਸ਼ਾਂ ਅਨਸਬਸਕ੍ਰਾਈਬ ਫੰਕਸ਼ਨ ਨੂੰ ਸੁਰੱਖਿਅਤ ਕਰੋ ਅਤੇ ਯਾਦ ਰੱਖੋ ਕਿ ਮੈਮੋਰੀ ਲੀਕ ਤੋਂ ਬਚਣ ਲਈ `deactivate()` ਵਿੱਚ ਇਸਨੂੰ ਕਾਲ ਕਰੋ।

---

### `ctx.sftp` — ਫਾਈਲ ਟ੍ਰਾਂਸਫਰ

> **ਸਥਿਤੀ: ਜਲਦੀ ਆ ਰਿਹਾ ਹੈ** — SFTP API ਪਰਿਭਾਸ਼ਿਤ ਹੈ ਪਰ ਐਪ ਦੇ SFTP ਇੰਜਣ ਨਾਲ ਹੁਣ ਤੱਕ ਜੋੜਿਆ ਨਹੀਂ ਗਿਆ। `list()` ਇਸ ਵੇਲੇ ਖਾਲੀ ਐਰੇ ਵਾਪਸ ਕਰਦਾ ਹੈ, ਅਤੇ `upload()`/`download()` ਕੋਈ ਕਾਰਵਾਈ ਨਹੀਂ ਕਰਦੇ। ਇਹ ਭਵਿੱਖ ਦੇ ਰਿਲੀਜ਼ ਵਿੱਚ ਪੂਰੀ ਤਰ੍ਹਾਂ ਲਾਗੂ ਕੀਤਾ ਜਾਵੇਗਾ। ਇਸ ਸਮੇਂ, `scp` ਜਾਂ `rsync` ਕਮਾਂਡਾਂ ਨਾਲ `ctx.terminal.send()` ਦੀ ਵਰਤੋਂ ਕਰੋ।

#### `sftp.list(sessionId, path)`

ਦੂਰ ਦੇ ਡਾਇਰੈਕਟਰੀ ਵਿੱਚ ਫਾਈਲਾਂ ਦੀ ਸੂਚੀ ਬਣਾਓ।
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

ਲੋਕਲ ਮਸ਼ੀਨ ਤੋਂ ਦੂਰ ਦੇ ਸਰਵਰ 'ਤੇ ਇੱਕ ਫਾਈਲ ਅਪਲੋਡ ਕਰੋ।
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

ਦੂਰ ਦੇ ਸਰਵਰ ਤੋਂ ਲੋਕਲ ਮਸ਼ੀਨ 'ਤੇ ਇੱਕ ਫਾਈਲ ਡਾਊਨਲੋਡ ਕਰੋ।
§§§CHUNK_SEPARATOR§§§
**ਵਰਤੋਂ (ਜਦ ਤੱਕ SFTP API ਲਾਈਵ ਨਹੀਂ ਹੈ):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — ਯੂਜ਼ਰ ਇੰਟਰਫੇਸ

#### `ui.addSidebarButton(options)`

WIA SOOM ਸਾਈਡਬਾਰ ਵਿੱਚ ਇੱਕ ਬਟਨ ਜੋੜੋ।

| ਵਿਕਲਪ | ਕਿਸਮ | ਜ਼ਰੂਰੀ | ਵੇਰਵਾ |
|--------|------|----------|-------------|
| `id` | `string` | ਨਹੀਂ | ਵਿਲੱਖਣ ID (ਪਲੱਗਇਨ ਨਾਮ 'ਤੇ ਡਿਫਾਲਟ) |
| `icon` | `string` | ਹਾਂ | Lucide ਆਈਕਨ ਨਾਮ (ਉਦਾਹਰਨ ਲਈ, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ਹਾਂ | ਸਾਈਡਬਾਰ ਵਿੱਚ ਦਿਖਾਈ ਦੇਣ ਵਾਲਾ ਬਟਨ ਪਾਠ |
| `onClick` | `() => void` | ਹਾਂ | ਜਦੋਂ ਬਟਨ 'ਤੇ ਕਲਿਕ ਕੀਤਾ ਜਾਂਦਾ ਹੈ, ਤਾਂ ਕਾਲ ਕੀਤੀ ਜਾਣ ਵਾਲੀ ਫੰਕਸ਼ਨ |
§§§CHUNK_SEPARATOR§§§
**ਆਈਕਨ ਹਵਾਲਾ:** ਸਾਰੇ ਉਪਲਬਧ ਆਈਕਨ ਨੂੰ [lucide.dev/icons](https://lucide.dev/icons) 'ਤੇ ਵੇਖੋ।

> **ਸੰਗਤਤਾ ਨੋਟ:** ਕੁਝ ਪੁਰਾਣੇ ਪਲੱਗਇਨ ਸਥਾਨਕ ਆਰਗਯੂਮੈਂਟ ਵਰਤਦੇ ਹਨ ਜਿਵੇਂ `addSidebarButton(id, icon, label, onClick)`। ਅਧਿਕਾਰਿਕ API ਉਪਰ ਦਿੱਤੇ ਗਏ ਤੌਰ 'ਤੇ ਇੱਕ **ਵਿਕਲਪਾਂ ਦੀ ਵਸਤੂ** ਵਰਤਦੀ ਹੈ। ਨਵੇਂ ਪਲੱਗਇਨ ਲਈ ਹਮੇਸ਼ਾਂ ਵਸਤੂ ਸ਼ੈਲੀ ਦੀ ਵਰਤੋਂ ਕਰੋ।

#### `ui.openWebview(options)`

ਕਸਟਮ HTML ਸਮੱਗਰੀ ਨਾਲ ਇੱਕ ਪਾਪਅਪ ਵਿੰਡੋ ਖੋਲ੍ਹੋ। ਇਹ ਤੁਹਾਨੂੰ ਧਨਾਢ UI ਬਣਾਉਣ ਵਿੱਚ ਮਦਦ ਕਰਦਾ ਹੈ।

| ਵਿਕਲਪ | ਕਿਸਮ | ਵੇਰਵਾ |
|--------|------|-------------|
| `title` | `string` | ਵਿੰਡੋ ਦਾ ਸਿਰਲੇਖ |
| `html` | `string` | ਪੇਸ਼ ਕਰਨ ਲਈ ਪੂਰੀ HTML ਸਮੱਗਰੀ |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> [ਭਾਗ 3](#part-3-building-custom-ui-with-webviews) ਵਿੱਚ ਉੱਚ ਪੱਧਰੀ ਵੈਬਵਿਊ ਪੈਟਰਨਾਂ ਲਈ ਵੇਖੋ।

#### `ui.showNotification(type, message)`

ਇੱਕ ਟੋਸਟ ਨੋਟੀਫਿਕੇਸ਼ਨ ਦਿਖਾਓ।

| ਪੈਰਾਮੀਟਰ | ਕਿਸਮ | ਵੇਰਵਾ |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ਨੋਟੀਫਿਕੇਸ਼ਨ ਸ਼ੈਲੀ |
| `message` | `string` | ਦਿਖਾਉਣ ਲਈ ਪਾਠ |
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

ਹੇਠਾਂ ਦੇ ਸਥਿਤੀ ਪੱਟੇ ਵਿੱਚ ਇੱਕ ਸਥਾਈ ਪਾਠ ਆਈਟਮ ਸ਼ਾਮਲ ਕਰੋ।

| ਪੈਰਾਮੀਟਰ | ਕਿਸਮ | ਵੇਰਵਾ |
|-----------|------|-------------|
| `id` | `string` | ਇਸ ਸਥਿਤੀ ਆਈਟਮ ਲਈ ਵਿਲੱਖਣ ID |
| `text` | `string` | ਦਿਖਾਉਣ ਲਈ ਪਾਠ |
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

### `ctx.settings` — ਸਥਾਈ ਸਟੋਰੇਜ

ਪਲੱਗਇਨ ਸੈਟਿੰਗਜ਼ ਨੂੰ ਸਥਾਈ ਤੌਰ 'ਤੇ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ਵਿੱਚ ਸਟੋਰ ਕੀਤਾ ਜਾਂਦਾ ਹੈ।

#### `settings.get(key)`

ਇੱਕ ਸੁਰੱਖਿਅਤ ਮੁੱਲ ਪੜ੍ਹੋ।
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
ਜੇਕਰ ਕੁੰਜੀ ਮੌਜੂਦ ਨਹੀਂ ਹੈ ਤਾਂ `undefined` ਵਾਪਸ ਕਰਦਾ ਹੈ।

#### `settings.set(key, value)`

ਇੱਕ ਮੁੱਲ ਸਟੋਰ ਕਰੋ। ਸਟਰਿੰਗਜ਼, ਨੰਬਰ, ਬੂਲੀਅਨ, ਐਰੇਜ਼ ਅਤੇ ਆਬਜੈਕਟਾਂ ਦਾ ਸਮਰਥਨ ਕਰਦਾ ਹੈ।
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**ਉਦਾਹਰਨ: ਉਪਭੋਗਤਾ ਦੀਆਂ ਪਸੰਦਾਂ ਯਾਦ ਰੱਖਣਾ**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — ਏਆਈ ਇੰਟੀਗ੍ਰੇਸ਼ਨ

> **ਸਥਿਤੀ: ਜਲਦ ਆ ਰਿਹਾ ਹੈ** — ਏਆਈ ਏਪੀਐਈ ਪਰਿਭਾਸ਼ਿਤ ਹੈ ਪਰ ਹਜੇ ਤੱਕ Soomy ਨਾਲ ਜੁੜਿਆ ਨਹੀਂ ਗਿਆ। ਮੌਜੂਦਾ `{ response: 'AI not yet connected' }` ਵਾਪਸ ਕਰਦਾ ਹੈ। ਭਵਿੱਖ ਦੇ ਰਿਲੀਜ਼ ਲਈ ਪੂਰੀ ਏਆਈ ਇੰਟੀਗ੍ਰੇਸ਼ਨ ਦੀ ਯੋਜਨਾ ਹੈ।

#### `ai.chat(messages, options?)`

ਏਆਈ ਸਹਾਇਕ (Soomy) ਨੂੰ ਸੁਨੇਹੇ ਭੇਜੋ।
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

## ਭਾਗ 3: ਵੈਬਵਿਊਜ਼ ਨਾਲ ਕਸਟਮ ਯੂਆਈ ਬਣਾਉਣਾ

`openWebview()` ਏਪੀਐਈ ਤੁਹਾਨੂੰ HTML, CSS, ਅਤੇ JavaScript ਨਾਲ ਡੈਸ਼ਬੋਰਡ ਯੂਆਈ ਬਣਾਉਣ ਦੀ ਆਗਿਆ ਦਿੰਦਾ ਹੈ — ਸਾਰੇ ਇੱਕ ਪਾਪਅਪ ਵਿੰਡੋ ਦੇ ਅੰਦਰ।

> **ਮਹੱਤਵਪੂਰਨ ਸੀਮਾ:** ਵੈਬਵਿਊਜ਼ **ਦਿਖਾਈ ਦੇਣ ਵਾਲੇ** ਹਨ। ਉਹ ਪਲੱਗਇਨ ਏਪੀਐਈਜ਼ (`ctx.settings`, `ctx.terminal`, ਆਦਿ) ਵਿੱਚ ਵਾਪਸ ਕਾਲ ਨਹੀਂ ਕਰ ਸਕਦੇ। ਸਾਰੇ ਉਪਭੋਗਤਾ ਕਾਰਵਾਈਆਂ ਲਈ ਸਾਈਡਬਾਰ ਬਟਨ ਵਰਤੋ, ਅਤੇ ਮੌਜੂਦਾ ਸਥਿਤੀ ਦਿਖਾਉਣ ਲਈ `openWebview()` ਦੀ ਵਰਤੋਂ ਕਰੋ। ਜੇ ਤੁਹਾਨੂੰ ਇੰਟਰੈਕਟਿਵ ਫੀਚਰਾਂ ਦੀ ਲੋੜ ਹੈ, ਤਾਂ ਉਨ੍ਹਾਂ ਨੂੰ ਸਾਈਡਬਾਰ ਬਟਨਾਂ ਤੋਂ ਚਾਲੂ ਕਰੋ ਅਤੇ ਡਿਸਪਲੇ ਨੂੰ ਰੀਫ੍ਰੈ��਼ ਕਰਨ ਲਈ ਵ��ਬਵਿਊ ਨੂੰ ਦੁਬਾਰਾ ਖੋਲ੍ਹੋ।

### ਪੈਟਰਨ: ਟਰਮੀਨਲ ਕਮਾਂਡ → ਆਉਟਪੁੱਟ ਪਾਰਸ → HTML ਵਿੱਚ ਦਿਖਾਓ

ਇਹ ਸਭ ਤੋਂ ਆਮ ਪਲੱਗਇਨ ਪੈਟਰਨ ਹੈ। ਤੁਸੀਂ ਇੱਕ ਕਮਾਂਡ ਚਲਾਉਂਦੇ ਹੋ, ਨਤੀਜੇ ਨੂੰ ਪਾਰਸ ਕਰਦੇ ਹੋ, ਅਤੇ ਇਸ ਨੂੰ ਵਿਜ਼ੂਅਲ ਰੂਪ ਵਿੱਚ ਦਿਖਾਉਂਦੇ ਹੋ।
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### ਪੈਟਰਨ: ਆਟੋ-ਰੀਫ੍ਰੈਸ਼ ਨਾਲ ਇੰਟਰੈਕਟਿਵ ਡੈਸ਼ਬੋਰਡ
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### ਪੈਟਰਨ: ਵੈਬਵਿਊ ਵਿੱਚ ਸੈਟਿੰਗਜ਼ ਦਿਖਾਉਣਾ

> **ਨੋਟ:** ਵੈਬਵਿਊਜ਼ ਦਿਖਾਈ ਦੇਣ ਵਾਲੇ ਹਨ — ਉਹ ਪਲੱਗਇਨ ਏਪੀਐਈਜ਼ ਵਿੱਚ ਵਾਪਸ ਕਾਲ ਨਹੀਂ ਕਰ ਸਕਦੇ। ਸੈਟਿੰਗਜ਼ ਨੂੰ ਸੋਧਣ ਲਈ ਆਪਣੇ ਸਾਈਡਬਾਰ ਬਟਨ ਹੈਂਡਲਰਾਂ ਵਿੱਚ `ctx.settings` ਦੀ ਵਰਤੋਂ ਕਰੋ, ਅਤੇ ਮੌਜੂਦਾ ਸਥਿਤੀ ਦਿਖਾਉਣ ਲਈ `openWebview()` ਦੀ ਵਰਤੋਂ ਕਰੋ।
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## ਭਾਗ 4: ਆਪਣੇ ਪਲੱਗਇਨ ਨੂੰ ਪ੍ਰਕਾਸ਼ਿਤ ਕਰਨਾ

### ਕਦਮ 1: ਸਥਾਨਕ ਤੌਰ 'ਤੇ ਟੈਸਟ ਕਰੋ

1. ਆਪਣੇ ਪਲੱਗਇਨ ਨੂੰ `~/.wia-soom/plugins/{your-plugin}/` ਵਿੱਚ ਨਕਲ ਕਰੋ।
2. WIA SOOM ਨੂੰ ਦੁਬਾਰਾ ਸ਼ੁਰੂ ਕਰੋ।
3. ਇਹ ਕੰਮ ਕਰਦਾ ਹੈ ਇਹ ਯਕੀਨੀ ਬਣਾਓ: ਸਾਈਡਬਾਰ ਬਟਨ ਪ੍ਰਗਟ ਹੁੰਦਾ ਹੈ, ਫੀਚਰ ਸਹੀ ਤਰ੍ਹਾਂ ਕੰਮ ਕਰਦੇ ਹਨ।
4. ਐਜ ਕੇਸਾਂ ਦੀ ਜਾਂਚ ਕਰੋ: ਜੇ ਕੋਈ ਟਰਮੀਨਲ ਜੁੜਿਆ ਨਹੀਂ ਹੈ ਤਾਂ ਕੀ ਹੁੰਦਾ ਹੈ?

### ਕਦਮ 2: ਸਬਮਿਸ਼ਨ ਲਈ ਤਿਆਰ ਕਰੋ

ਤੁਹਾਡੇ ਪਲੱਗਇਨ ਫੋਲਡਰ ਵਿੱਚ ਇਹ ਹੋਣਾ ਚਾਹੀਦਾ ਹੈ:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**ਲੋੜੀਂਦੇ `package.json` ਖੇਤਰ:**

| ਖੇਤਰ | ਵੇਰਵਾ | ਉਦਾਹਰਨ |
|-------|-------------|---------|
| `name` | ਵਿਲੱਖਣ kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | ਇੱਕ-ਲਾਈਨ ਵੇਰਵਾ | `"Monitors nginx access logs in real-time"` |
| `author` | ਤੁਹਾਡਾ ਨਾਮ | `"John Doe"` |
| `main` | ਪ੍ਰਵੇਸ਼ ਬਿੰਦੂ | `"index.js"` |

**ਵਿਕਲਪਿਕ ਖੇਤਰ:**

| ਖੇਤਰ | ਵੇਰਵਾ |
|-------|-------------|
| `license` | ਲਾਈਸੈਂਸ ਕਿਸਮ (MIT ਦੀ ਸਿਫਾਰਸ਼ ਕੀਤੀ ਜਾਂਦੀ ਹੈ) |
| `keywords` | ਖੋਜ ਟੈਗਾਂ ਦੀ ਐਰੇ |
| `soom.minVersion` | ਘੱਟੋ-ਘੱਟ WIA SOOM ਵਰਜਨ ਦੀ ਲੋੜ |

### ਕਦਮ 3: ਪਲੱਗਇਨ ਰਜਿਸਟਰੀ ਵਿੱਚ ਭੇਜੋ

1. ****Package** your plugin as a ZIP file
2. **Add** ਆਪਣੇ ਪਲੱਗਇਨ ਨੂੰ `plugins/{your-plugin-name}/`
3. **Submit** ਇੱਕ Pull Request

### ਕਦਮ 4: ਸਮੀਖਿਆ ਅਤੇ ਮਨਜ਼ੂਰੀ

ਅਸੀਂ ਹਰ ਪਲੱਗਇਨ ਦੀ ਸਮੀਖਿਆ ਕਰਦੇ ਹਾਂ:

- **ਸੁਰੱਖਿਆ** — ਕੋਈ ਖਤਰਨਾਕ APIs ਨਹੀਂ (ਦੇਖੋ [ਸੁਰੱਖਿਆ ਨਿਯਮ](#security-rules))
- **ਗੁਣਵੱਤਾ** — ਕੀ ਇਹ ਕੰਮ ਕਰਦਾ ਹੈ? ਕੀ ਕੋਡ ਸਾਫ਼ ਹੈ?
- **ਉਪਯੋਗਤਾ** — ਕੀ ਇਹ ਕਿਸੇ ਅਸਲੀ ਸਮੱਸਿਆ ਨੂੰ ਹੱਲ ਕਰਦਾ ਹੈ?

ਮਨਜ਼ੂਰੀ ਤੋਂ ਬਾਅਦ:
1. ਤੁਹਾਡਾ ਪਲੱਗਇਨ `registry.json` ਵਿੱਚ ਸ਼ਾਮਲ ਕੀਤਾ ਜਾਂਦਾ ਹੈ
2. `dist/` ਵਿੱਚ ਇੱਕ ZIP ਬੰਡਲ ਬਣਾਇਆ ਜਾਂਦਾ ਹੈ
3. ਤੁਹਾਡਾ ਪਲੱਗਇਨ ਸਾਰੇ WIA SOOM ਉਪਭੋਗਤਾਵਾਂ ਲਈ **Plugin Store** ਵਿੱਚ ਦਿਖਾਈ ਦਿੰਦਾ ਹੈ!

---

## ਭਾਗ 5: ਬਿਹਤਰ ਅਭਿਆਸ

### ਸੁਰੱਖਿਆ ਨਿਯਮ

ਇਹ ਨਿਯਮ **ਲਾਜ਼ਮੀ** ਹਨ। ਜੋ ਪਲੱਗਇਨ ਇਨ੍ਹਾਂ ਦਾ ਉਲੰਘਣਾ ਕਰਦੇ ਹਨ ਉਹ ਰੱਦ ਕਰ ਦਿੱਤੇ ਜਾਣਗੇ।

| ਨਿਯਮ | ਕਿਉਂ |
|------|-----|
| **ਕਦੇ ਵੀ** `eval()` ਜਾਂ `new Function()` ਦਾ ��ਸਤੇਮਾਲ ਨਾ ਕਰੋ | ਕੋਡ ਇੰਜੈਕਸ਼ਨ ਦਾ ਖਤਰਾ |
| **ਕਦੇ ਵੀ** `child_process`, `exec()`, `spawn()` ਦਾ ਇਸਤੇਮਾਲ ਨਾ ਕਰੋ | ਹੁਕਮਾਂ ਲਈ ਸਿਰਫ `ctx.terminal.send()` ਦੀ ਵਰਤੋਂ ਕਰੋ |
| **ਕਦੇ ਵੀ** ਬਾਹਰੀ URLs ਨੂੰ ਫੈਚ ਨਾ ਕਰੋ | ਵਿਸ਼ੇਸ਼: `wiasoom.com` API endpoints |
| **ਕਦੇ ਵੀ** `process.env` ਨੂੰ ਐਕਸੈਸ ਨਾ ਕਰੋ | ਵਾਤਾਵਰਣ ਚਰਾਂ ਵਿੱਚ ਰਾਜ਼ ਹੋ ਸਕਦੇ ਹਨ |
| **ਕਦੇ ਵੀ** `require('fs')` ਨੂੰ ਸਿੱਧਾ ਨਾ ਵਰਤੋ | ਸਟੋਰੇਜ ਲਈ `ctx.settings` ਦੀ ਵਰਤੋਂ ਕਰੋ, ਫਾਈਲ ਟ੍ਰਾਂਸਫਰ ਲਈ `ctx.sftp` |
| **ਕਦੇ ਵੀ** npm ਬਾਹਰੀ ਪੈਕੇਜਾਂ ਦੀ ਵਰਤੋਂ ਨਾ ਕਰੋ | ਸਿਰਫ ਸ਼ੁੱਧ JavaScript — ਕੋਈ node_modules ਨਹੀਂ |
| **ਲਾਜ਼ਮੀ** ਹੈ ਕਿ ਸਾਰੇ ਦੂਰ ਦੇ ਹੁਕਮਾਂ ਲਈ `ctx.terminal.send()` ਦੀ ਵਰਤੋਂ ਕਰੋ | ਇਹ ਸੁਰੱਖਿਅਤ SSH ਚੈਨਲ ਰਾਹੀਂ ਜਾਂਦਾ ਹੈ |
| **ਲਾਜ਼ਮੀ** ਹੈ ਕਿ `deactivate()` ਵਿੱਚ ਸਾਫ਼ ਕਰੋ | ਸੁਣਨਹਾਰਾਂ ਨੂੰ ਹਟਾਓ, ਇੰਟਰਵਲ ਸਾਫ਼ ਕਰੋ |

### ਗਲਤੀ ਸੰਭਾਲਣਾ

ਹਮੇਸ਼ਾ ਖਤਰਨਾਕ ਕਾਰਵਾਈਆਂ ਨੂੰ try/catch ਵਿੱਚ ਲਪੇਟੋ:
§§§CHUNK_SEPARATOR§§§
### deactivate() ਵਿੱਚ ਸਾਫ਼ਾਈ

ਜੇ ਤੁਹਾਡਾ ਪਲੱਗਇਨ ਇੰਟਰਵਲ, ਸੁਣਨਹਾਰਾਂ ਜਾਂ ਸਬਸਕ੍ਰਿਪਸ਼ਨ ਬਣਾਉਂਦਾ ਹੈ — ਉਨ੍ਹਾਂ ਨੂੰ ਸਾਫ਼ ਕਰੋ:
§§§CHUNK_SEPARATOR§§§
### i18n ਸਮਰਥਨ

WIA SOOM 254 ਭਾਸ਼ਾਵਾਂ ਦਾ ਸਮਰਥਨ ਕਰਦਾ ਹੈ। ਆਪਣੇ ਪਲੱਗਇਨ ਲੇਬਲ ਨੂੰ ਅਨੁਵਾਦਯੋਗ ਬਣਾਉਣ ਲਈ, ਇੱਕ ਸਧਾਰਨ ਪਹੁੰਚ ਵਰਤੋ:
§§§CHUNK_SEPARATOR§§§
---

## ਭਾਗ 6: ਵਾਸਤਵਿਕ-ਜਗਤ ਉਦਾਹਰਨਾਂ

### ਉਦਾਹਰਨ 1: ਸਰਵਰ ਡਿਸਕ ਚੈੱਕਰ

ਦੂਰ ਦੇ ਸਰਵਰ 'ਤੇ `df -h` ਚਲਾਉਂਦਾ ਹੈ ਅਤੇ ਸਥਿਤੀ ਪੱਟੀ ਵਿੱਚ ਵਰਤੇ ਗਏ/ਉਪਲਬਧ ਸਥਾਨ ਦਿਖਾਉਂਦਾ ਹੈ.
§§§CHUNK_SEPARATOR§§§
---

### ਉਦਾਹਰਨ 2: TODO ਮੈਨੇਜਰ

ਇੱਕ ਪਲੱਗਇਨ ਜੋ ਸਥਾਈ ਸਟੋਰੇਜ ਲਈ ਸੈਟਿੰਗਾਂ ਅਤੇ ਦਿਖਾਈ ਲਈ ਇੱਕ ਵੈਬਵਿਊ ਦੀ ਵਰਤੋਂ ਕਰਕੇ TODO ਸੂਚੀ ਦਾ ਪ੍ਰਬੰਧ ਕਰਦਾ ਹੈ।

> **ਡਿਜ਼ਾਇਨ ਪੈਟਰਨ:** ਕਿਉਂਕਿ ਵੈਬਵਿਊ ਸਿੱਧੇ ਤੌਰ 'ਤੇ ਪਲੱਗਇਨ APIs ਨੂੰ ਕਾਲ ਨਹੀਂ ਕਰ ਸਕਦੇ, ਇਹ ਪਲੱਗਇਨ "ਸਨੈਪਸ਼ਾਟ" ਪਹੁੰਚ ਦੀ ਵਰਤੋਂ ਕਰਦਾ ਹੈ — ਇਹ ਸੈਟਿੰਗਾਂ ਤੋਂ TODOs ਨੂੰ ਪੜ੍ਹਦਾ ਹੈ, ਉਨ੍ਹਾਂ ਨੂੰ ਪੜ੍ਹਨ-ਯੋਗ HTML ਵਜੋਂ ਰੇਂਡਰ ਕਰਦਾ ਹੈ, ਅਤੇ ਆਈਟਮ ਸ਼ਾਮਲ ਕਰਨ ਲਈ ਸਾਈਡਬਾਰ-ਅਧਾਰਿਤ ਕਾਰਵਾਈਆਂ ਪ੍ਰਦਾਨ ਕਰਦਾ ਹੈ। ਵੈਬਵਿਊ ਇੱਕ **ਦਿਖਾਈ** ਪਰਤ ਹੈ, ਨਾ ਕਿ ਇੱਕ ਇੰਟਰੈਕਟਿਵ ਫਾਰਮ.
§§§CHUNK_SEPARATOR§§§
---

### ਉਦਾਹਰਨ 3: ਗਲਤੀ ਵਾਚਰ

ਟਰਮੀਨਲ ਆਉਟਪੁੱਟ ਦੀ ਨਿਗਰਾਨੀ ਕਰਦਾ ਹੈ ਅਤੇ ਜਦੋਂ ਵਿਸ਼ੇਸ਼ ਪੈਟਰਨ ਪਛਾਣੇ ਜਾਂਦੇ ਹਨ ਤਾਂ ਇੱਕ ਸੁਚਨਾ ਭੇਜਦਾ ਹੈ।
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
---

## ਐਪੈਂਡਿਕਸ: ਸ਼੍ਰੇਣੀਆਂ ਅਤੇ ਆਈਕਨ

### ਪਲੱਗਇਨ ਸ਼੍ਰੇਣੀਆਂ (29)

ਇਹਨਾਂ ਨੂੰ ਆਪਣੇ `package.json` `keywords` ਵਿੱਚ ਜਾਂ ਰਜਿਸਟਰੀ ਵਿੱਚ ਭੇਜਣ ਵੇਲੇ ਵਰਤੋਂ ਕਰੋ:

| ਸ਼੍ਰੇਣੀ | ਵੇਰਵਾ |
|----------|-------------|
| `server` | ਆਮ ਸਰਵਰ ਪ੍ਰਬੰਧਨ |
| `devtools` | ਵਿਕਾਸ ਦੇ ਟੂਲ |
| `calculator` | ਕੈਲਕੁਲੇਟਰ ਅਤੇ ਪਰਿਵਰਤਕ |
| `simulator` | ਸਿਮੂਲੇਟਰ |
| `game` | ਟਰਮੀਨਲ ਖੇਡਾਂ |
| `business` | ਵਪਾਰ ਦੇ ਟੂਲ |
| `security` | ਸੁਰੱਖਿਆ ਅਤੇ ਆਡੀਟਿੰਗ |
| `web` | ਵੈਬ ਸਰਵਰ ਪ੍ਰਬੰਧਨ |
| `education` | ਸ਼ਿਖਿਆ ਦੇ ਟੂਲ |
| `health` | ਸਿਹਤ-ਸੰਬੰਧੀ ਟੂਲ |
| `islamic` | ਇਸਲਾਮਿਕ ਟੂਲ (ਨਮਾਜ ਦੇ ਸਮੇਂ, ਆਦਿ) |
| `science` | ਵਿਗਿ��ਨਕ ਟੂਲ |
| `quantum` | ਕੁਆਂਟਮ ਕੰਪਿਊਟਿੰਗ ਟੂਲ |
| `ai` | ਏ.ਆਈ.-ਚਲਿਤ ਟੂਲ |
| `biotech` | ਬਾਇਓਟੈਕਨੋਲੋਜੀ ਟੂਲ |
| `space` | ਅੰਤਰਿਕਸ਼ ਅਤੇ ਖਗੋਲ ਵਿਗਿਆਨ ਦੇ ਟੂਲ |
| `network` | ਨੈੱਟਵਰਕ ਟੂਲ |
| `database` | ਡੇਟਾਬੇਸ ਪ੍ਰਬੰਧਨ |
| `monitoring` | ਸਰਵਰ ਨਿਗਰਾਨੀ |
| `devops` | ਡਿਵਓਪਸ ਅਤੇ CI/CD |
| `utility` | ਆਮ ਯੂਟਿਲਿਟੀ |
| `design` | ਡਿਜ਼ਾਈਨ ਦੇ ਟੂਲ |
| `ecommerce` | ਈ-ਕਾਮਰਸ ਦੇ ਟੂਲ |
| `automation` | ਆਟੋਮੇਸ਼ਨ ਦੇ ਟੂਲ |
| `kpop` | K-ਪਾਪ ਨਾਲ ਸਬੰਧਤ ਟੂਲ |
| `accessibility` | ਪਹੁੰਚ ਯੋਗਤਾ ਦੇ ਟੂਲ |
| `analytics` | ਵਿਸ਼ਲੇਸ਼ਣ ਅਤੇ ਰਿਪੋਰਟਿੰਗ |
| `wia` | WIA ਪਰਿਵਾਰ ਦੇ ਟੂਲ |
| `all` | ਸਾਰੀਆਂ ਸ਼੍ਰੇਣੀਆਂ ਵਿੱਚ ਦਿਖਾਈ ਦਿੰਦਾ ਹੈ |

### ਸਿਫਾਰਸ਼ੀ ਆਈਕਨ (Lucide)

| ਆਈਕਨ ਨਾਮ | ਵਰਤੋਂ ਲਈ |
|-----------|---------|
| `server` | ਸਰਵਰ ਪ੍ਰਬੰਧਨ |
| `shield` | ਸੁਰੱਖਿਆ |
| `database` | ਡੇਟਾਬੇਸ |
| `activity` | ਨਿਗਰਾਨੀ |
| `terminal` | ਟਰਮੀਨਲ ਦੇ ਟੂਲ |
| `code` | ਵਿਕਾਸ |
| `hard-drive` | ਡਿਸਕ/ਸਟੋਰੇਜ |
| `network` | ਨੈੱਟਵਰਕਿੰਗ |
| `lock` | ਪ੍ਰਮਾਣਿਕਤਾ/ਇਨਕ੍ਰਿਪਸ਼ਨ |
| `eye` | ਦੇਖਣਾ/ਨਿਗਰਾਨੀ |
| `check-square` | ਕੰਮ/TODO |
| `layout-dashboard` | ਡੈਸ਼ਬੋਰਡ |
| `settings` | ਸੰਰਚਨਾ |
| `zap` | ਆਟੋਮੇਸ਼ਨ |
| `globe` | ਵੈਬ/ਅੰਤਰਰਾਸ਼ਟਰੀ |

ਸਾਰੇ 1,500+ ਆਈਕਨ ਦੇਖੋ: [lucide.dev/icons](https://lucide.dev/icons)

---

## ਮਦਦ ਦੀ ਲੋੜ ਹੈ?

- **GitHub ਸਮੱਸਿਆਵਾਂ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ਪਲੱਗਇਨ ਸਮੱਸਿਆਵਾਂ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ਉਦਾਹਰਨ ਪਲੱਗਇਨ:** [Website](https://wiasoom.com)
- **ਵੈਬਸਾਈਟ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ਕੁਝ ਸ਼ਾਨਦਾਰ ਬਣਾਓ। ਇਸਨੂੰ ਦੁਨੀਆ ਨਾਲ ਸਾਂਝਾ ਕਰੋ।</em></p>
<p align="center"><em>— WIA SOOM ਟੀਮ</em></p>
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

> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

Show a toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Text to show |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

Add a persistent text item to the bottom status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique ID for this status item |
| `text` | `string` | Text to display |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — Persistent storage

Plugin settings are stored permanently in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Read a saved value.

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

Returns `undefined` if the key doesn't exist.

#### `settings.set(key, value)`

Save a value. Supports strings, numbers, booleans, arrays, and objects.

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**Example: Remember user preferences**

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
