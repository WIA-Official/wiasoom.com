<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM ପ୍ଲଗଇନ ବିକାଶକ ଗାଇଡ୍</h1>
<p align="center"><strong>5 ମିନିଟ୍‌ରେ ଆପଣଙ୍କର ନିଜସ୍ୱ ପ୍ଲଗଇନ୍ ତିଆରି କରନ୍ତୁ।</strong></p>
<p align="center">ଶକ୍ତିଶାଳୀ ସର୍ଭର ଟୁଲ୍‌ସ, ଡ୍ୟାସ୍‌ବୋର୍ଡ୍‌ସ, ଏବଂ ଅଟୋମେସନ୍‌ଗୁଡିକୁ WIA SOOM ମଧ୍ୟରେ ସିଧାସଳଖ ତିଆରି କରନ୍ତୁ।</p>

---

## ସାମଗ୍ରୀର ତାଲିକା

- [ଅଂଶ 1: ଶୀଘ୍ର ଆରମ୍ଭ — 5 ମିନିଟ୍‌ରେ ଆପଣଙ୍କର ପ୍ରଥମ ପ୍ଲଗଇନ୍](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ଅଂଶ 2: ପ୍ଲଗଇନ୍ କନ୍ଟେକ୍ସଟ୍ API ସନ୍ଦର୍ଭ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ଅଂଶ 3: ୱେବ୍‌ଭ୍ୟୁସ୍‌ ସହିତ କଷ୍ଟମ୍ UI ତିଆରି](#part-3-building-custom-ui-with-webviews)
- [ଅଂଶ 4: ଆପଣଙ୍କର ପ୍ଲଗଇନ୍ ପ୍ରକାଶିତ କରନ୍ତୁ](#part-4-publishing-your-plugin)
- [ଅଂଶ 5: ସର୍ବୋତ୍ତମ ପ୍ରଥା](#part-5-best-practices)
- [ଅଂଶ 6: ବାସ୍ତବ ଦୁନିଆର ଉଦାହରଣ](#part-6-real-world-examples)
- [ଅନୁସୂଚୀ: ବର୍ଗ ଏବଂ ଆଇକନ୍‌ଗୁଡିକ](#appendix-categories--icons)

---

## ଅଂଶ 1: ଶୀଘ୍ର ଆରମ୍ଭ — 5 ମିନିଟ୍‌ରେ ଆପଣଙ୍କର ପ୍ରଥମ ପ୍ଲଗଇନ୍

### ଆପଣ କଣ ତିଆରି କରିବେ

ଏକ "Hello World" ପ୍ଲଗଇନ୍ ଯାହା ସାଇଡ୍‌ବାର୍‌କୁ ଏକ ବଟନ୍ ଯୋଡ଼େ। ଏହାକୁ କ୍ଲିକ୍ କଲେ, ଏହା ଏକ ସୂଚନା ଦେଖାଏ।

### ପଦକ୍ଷେପ 1: ପ୍ଲଗଇନ୍ ଫୋଲ୍ଡର୍ ତିଆରି କର���୍ତୁ
§§§CHUNK_SEPARATOR§§§
### ପଦକ୍ଷେପ 2: package.json ତିଆରି କରନ୍ତୁ
§§§CHUNK_SEPARATOR§§§
**ଆବଶ୍ୟକ ଫିଲ୍ଡ୍‌ଗୁଡିକ:** `name`, `version`, `description`, `author`, `main`

### ପଦକ୍ଷେପ 3: index.js ତିଆରି କରନ୍ତୁ
§§§CHUNK_SEPARATOR§§§
### ପଦକ୍ଷେପ 4: WIA SOOM ପୁନରାମ୍ଭ କରନ୍ତୁ

ଆପ୍‌ଟିକୁ ପୁନରାମ୍ଭ କରନ୍ତୁ (କିମ୍ବା ସେଟିଂସ୍ → ପ୍ଲଗଇନ୍‌ସ୍‌ରେ ପ୍ଲଗଇନ୍‌ଟିକୁ ଅନ୍/ଅଫ୍ କରନ୍ତୁ)।

ଆପଣ ସାଇଡ୍‌ବାର୍‌ରେ **"Hello World"** ବଟନ୍ ଦେଖିବେ। ଏହାକୁ କ୍ଲିକ୍ କରନ୍ତୁ — ଆପଣ ଏକ ସଫଳତା ସୂଚନା ଦେଖିବେ!

### କିପରି କାମ କରେ
§§§CHUNK_SEPARATOR§§§
---

## ଅଂଶ 2: ପ୍ଲଗଇନ୍ କନ୍ଟେକ୍ସଟ୍ API ସନ୍ଦର୍ଭ

ଯେତେବେଳେ ଆପଣଙ୍କର `activate(context)` ଫଙ୍କସନ୍ କୁ ଡାକାଯାଏ, `context` (କିମ୍ବା `ctx`) ଏହି APIଗୁଡିକୁ ପ୍ରଦାନ କରେ:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — ରିମୋଟ୍ ସର୍ଭର୍‌ଗୁଡିକରେ କମାଣ୍ଡ୍ ଚଲାନ୍ତୁ

#### `terminal.send(sessionId, data)`

ଏକ କମାଣ୍ଡ୍ (କିମ୍ବା କୌଣସି ତଥ୍ୟ)କୁ ଏକ ସକ୍ରିୟ ଟର୍ମିନାଲ୍ ସେସନ୍‌କୁ ପଠାନ୍ତୁ।

| ପ୍ୟାରାମିଟର୍ | ପ୍ରକାର | ବର୍ଣ୍ଣନା |
|-----------|------|-------------|
| `sessionId` | `string` | ପଠାଇବାକୁ ଟର୍ମିନାଲ୍ ସେସନ୍ |
| `data` | `string` | ପଠାଇବାକୁ କମାଣ୍ଡ୍ କିମ୍ବା ତଥ୍ୟ |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

ଏକ ଟର୍ମିନାଲ୍ ସେସନ୍‌ରୁ ସମସ୍ତ ଆଉଟପୁଟ୍‌କୁ ସବ୍ସ୍କ୍ରାଇବ୍ କରନ୍ତୁ। ଏହା ଏକ **ଅନସ୍କ୍ରାଇବ୍ ଫଙ୍କସନ୍** ଫେରାଇ।

| ପ୍ୟାରାମିଟର୍ | ପ୍ରକାର | ବର୍ଣ୍ଣନା |
|-----------|------|-------------|
| `sessionId` | `string` | ଦେଖିବାକୁ ଟର୍ମିନାଲ୍ ସେସନ୍ |
| `callback` | `(data: string) => void` | ପ୍ରତ୍���େକ ଚଙ୍କ୍ ଆଉଟପୁଟ୍ ସହିତ ଡାକାଯାଏ |
| **ଫେରାଇ** | `() => void` | ଏହାକୁ ଡାକି ଶୁଣିବା ବନ୍ଦ କରନ୍ତୁ |
§§§CHUNK_SEPARATOR§§§
**ଗୁରୁତ୍ୱ:** ସଦା ଅନସ୍କ୍ରାଇବ୍ ଫଙ୍କସନ୍‌କୁ ସଂରକ୍ଷଣ କରନ୍ତୁ ଏବଂ ଏହାକୁ `deactivate()` ରେ ଡାକିବାକୁ କରନ୍ତୁ ଯାହା ମେମୋରୀ ଲିକ୍‌ଗୁଡିକୁ ରୋକିବ।

---

### `ctx.sftp` — ଫାଇଲ୍ ହସ୍ତାନ୍ତର

> **ସ୍ଥିତି: ଆସୁଛି** — SFTP API ନିର୍ଦ୍ଧାରିତ ହୋଇଛି କିନ୍ତୁ ଏହା ଆପ୍‌ର SFTP ଇଞ୍ଜିନ୍‌କୁ ଯୋଡ଼ାଯାଇନି। `list()` ବର୍ତ୍ତମାନ ଏକ ଖାଲି ଆରେ ଫେରାଇ, ଏବଂ `upload()`/`download()` କୌଣସି କାମ କରେନି। ଏହା ଏକ ଭବିଷ୍ୟତ ମୁକ୍ତିରେ ସଂପୂର୍ଣ୍ଣ ଭାବେ କାମ କରିବ। ଏହା ପର୍ଯ୍ୟନ୍ତ, `ctx.terminal.send()` କୁ `scp` କିମ୍ବା `rsync` କମାଣ୍ଡ୍‌ଗୁଡିକ ସହିତ ବ୍ୟବହାର କରନ୍ତୁ।

#### `sftp.list(sessionId, path)`

ଏକ ରିମୋଟ୍ ଡିରେକ୍ଟରୀରେ ଫାଇଲ୍‌ଗୁଡିକୁ ତାଲିକା କରନ୍ତୁ।
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

ସ୍ଥାନୀୟ ଯନ୍ତ୍ରରୁ ରିମୋଟ୍ ସର୍ଭରକୁ ଏକ ଫାଇଲ୍ ଅପଲୋଡ୍ କରନ୍ତୁ।
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

ରିମୋଟ୍ ସର୍ଭରରୁ ସ୍ଥାନୀୟ ଯନ୍ତ୍ରକୁ ଏକ ଫାଇଲ୍ ଡାଉନଲୋଡ୍ କରନ୍ତୁ।
§§§CHUNK_SEPARATOR§§§
**ବ୍ୟବହାର କରିବା ପାଇଁ (SFTP API ଜୀବନ୍ତ ହେବା ପର୍ଯ୍ୟନ୍ତ):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — ବ୍ୟବହାରକାରୀ ସାମ୍ନା

#### `ui.addSidebarButton(options)`

WIA SOOM ସାଇଡ୍‌ବାର୍‌କୁ ଏକ ବଟନ୍ ଯୋଡ଼ନ୍ତୁ।

| ବିକଳ୍ପ | ପ୍ରକାର | ଆବଶ୍ୟକ | ବର୍ଣ୍ଣନା |
|--------|------|----------|-------------|
| `id` | `string` | ନା | ବିଶିଷ୍ଟ ID (ପ୍ଲଗଇନ୍ ନାମକୁ ଡିଫଲ୍ଟ୍ କ���େ) |
| `icon` | `string` | ହଁ | Lucide ଆଇକନ୍ ନାମ (ଉଦାହରଣ: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ହଁ | ସାଇଡ୍‌ବାର୍‌ରେ ଦେଖାଯିବା ବଟନ୍ ଟେକ୍ସ୍ଟ |
| `onClick` | `() => void` | ହଁ | ବଟନ୍ କ୍ଲିକ୍ କଲେ ଡାକାଯିବା ଫଙ୍କସନ୍ |
§§§CHUNK_SEPARATOR§§§
**ଆଇକନ୍ ସନ୍ଦର୍ଭ:** [lucide.dev/icons](https://lucide.dev/icons) ରେ ଉପଲବ୍ଧ ଆଇକନ୍‌ଗୁଡିକୁ ଦେଖନ୍ତୁ।

> **ସମ୍ପ୍ରତିକତା ଟିପ୍ପଣୀ:** କିଛି ପୁରାଣା ପ୍ଲଗଇନ୍‌ଗୁଡିକ `addSidebarButton(id, icon, label, onClick)` ଭଳି ପଦବୀଗତ ଆର୍ଗୁମେଣ୍ଟ୍‌ଗୁଡିକୁ ବ୍ୟବହାର କରନ୍ତି। ଅଧିକାରିକ API ଉପରେ ଉଲ୍ଲେଖିତ **ବିକଳ୍ପ ଅବଜେକ୍ଟ୍** ବ୍ୟବହାର କରେ। ନୂତନ ପ୍ଲଗଇନ୍‌ଗୁଡିକ ପାଇଁ ସଦା ଅବଜେକ୍ଟ୍ ଶୈଳୀ ବ୍ୟବହାର କରନ୍ତୁ।

#### `ui.openWebview(options)`

କଷ୍ଟମ୍ HTML ସାମଗ୍ରୀ ସହିତ ଏକ ପପ୍‌ଅପ��� ଜଣେ ଖିଣ୍ଟା ଖୋଲନ୍ତୁ। ଏହା ହେଉଛି ଆପଣ କିପରି ଧନ୍ୟ ୱେବ୍‌ UI ତିଆରି କରନ୍ତୁ।

| ବିକଳ୍ପ | ପ୍ରକାର | ବର୍ଣ୍ଣନା |
|--------|------|-------------|
| `title` | `string` | ଜଣେ ଖିଣ୍ଟାର ଶୀର୍ଷକ |
| `html` | `string` | ରେଣ୍ଡର୍ କରିବାକୁ ସମ୍ପୂର୍ଣ୍ଣ HTML ସାମଗ୍ରୀ |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> [Part 3](#part-3-building-custom-ui-with-webviews) ପାଇଁ ଉନ୍ନତ ୱେବଭ୍ୟୁ ପ୍ୟାଟର୍ନଗୁଡିକୁ ଦେଖନ୍ତୁ।

#### `ui.showNotification(type, message)`

ଟୋଷ୍ଟ ସୂଚନା ଦେଖାନ୍ତୁ।

| ପ୍ୟାରାମିଟର | ପ୍ରକାର | ବିବରଣୀ |
|--------------|---------|----------|
| `type` | `'success' \| 'error' \| 'info'` | ସୂଚନା ଶୈଳୀ |
| `message` | `string` | ଦେଖାଇବାକୁ ଟେକ୍ସଟ୍ |
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

ତଳ ଷ୍ଟାଟସ୍ ବାରରେ ଏକ ଦୀର୍ଘକାଳୀନ ଟେକ୍ସଟ୍ ଆଇଟମ୍ ଯୋଡନ୍ତୁ।

| ପ୍ୟାରାମିଟର | ପ୍ରକାର | ବିବରଣୀ |
|--------------|---------|----------|
| `id` | `string` | ଏହି ଷ୍ଟାଟସ୍ ଆଇଟମ୍ ପାଇଁ ବିଶିଷ୍ଟ ID |
| `text` | `string` | ଦେଖାଇବାକୁ ଟେକ୍ସଟ୍ |
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

### `ctx.settings` — ଦୀର୍ଘକାଳୀନ ସଂଗ୍ରହ

ପ୍ଲଗିନ ସେଟିଂଗୁଡିକୁ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ରେ ସ୍ଥା���ୀ ଭାବେ ସଂଗ୍ରହ କରାଯାଇଛି।

#### `settings.get(key)`

ଏକ ସଂରକ୍ଷିତ ମୂଲ୍ୟ ପଢନ୍ତୁ।
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
ଯଦି କୀ ଅବସ୍ଥିତ ନୁହେଁ, ତେବେ `undefined` ଫେରାଇବ।

#### `settings.set(key, value)`

ଏକ ମୂଲ୍ୟ ସଂରକ୍ଷିତ କରନ୍ତୁ। ଏହା ସ୍ଟ୍ରିଙ୍ଗ୍, ସଂଖ୍ୟା, ବୁଲିୟନ୍, ଆରେ ଏବଂ ଅବଜେକ୍ଟ୍କୁ ସମର୍ଥନ କରେ।
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**ଉଦାହରଣ: ବ୍ୟବହାରକାରୀ ପ୍ରାଥମିକତା ମନେ ରଖନ୍ତୁ**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — AI ସଂଯୋଜନ

> **ସ୍ଥିତି: ଶୀଘ୍ର ଆସୁଛି** — AI API ନିର୍ଦ୍ଧାରିତ ହୋଇଛି କିନ୍ତୁ ଏପର୍ଯ୍ୟନ୍ତ Soomy ସହିତ ସଂଯୋଜିତ ହୋଇନାହିଁ। ବର୍ତ୍ତମାନ `{ response: 'AI not yet connected' }` ଫେରାଇବ। ପୂର୍ଣ୍ଣ AI ସଂଯୋଜନ ଭବିଷ୍ୟତର ମୁକ୍ତି ପାଇଁ ଯୋଜନା କରାଯାଇଛି।

#### `ai.chat(messages, options?)`

AI ସହାୟକ (Soomy) କୁ ସନ���ଦେଶ ପଠାନ୍ତୁ।
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

## Part 3: Webviews ସହିତ କଷ୍ଟମ୍ UI ତିଆରି କରିବା

`openWebview()` API ଆପଣଙ୍କ�� HTML, CSS, ଏବଂ JavaScript ସହିତ ଡ୍ୟାସ୍ବୋର୍ଡ UI ତିଆରି କରିବାକୁ ଅନୁମତି ଦେଇଥାଏ — ସମସ୍ତ କିଛି ଏକ ପପ୍-ଅପ୍ ଜାଲକ ମଧ୍ୟରେ।

> **ଗୁରୁତ୍ୱପୂର୍ଣ୍ଣ ସୀମିତତା:** Webviews ହେଉଛି **ଦେଖାଇବାକୁ ମାତ୍ର**। ସେଗୁଡିକୁ ପ୍ଲଗିନ API (`ctx.settings`, `ctx.terminal`, ଇତ୍ୟାଦି) କୁ କଲ୍ ବ୍ୟବହାର କରିପାରିବେ ନାହିଁ। ସମସ୍ତ ବ୍ୟବହାରକାରୀ କାର୍ଯ୍ୟ ପାଇଁ ସାଇଡ୍ବାର ବଟନ୍ ବ୍ୟବହାର କରନ୍ତୁ, ଏବଂ ବର୍ତ୍ତମାନ ଅବସ୍ଥା ଦେଖାଇବା ପାଇଁ `openWebview()` ବ୍ୟବହାର କରନ୍ତୁ। ଯଦି ଆପଣ���୍କୁ ଇଣ୍ଟର୍ଆକ୍ଟିଭ୍ ବିଶେଷତା ଆବଶ୍ୟକ, ସେଗୁଡିକୁ ସାଇଡ୍ବାର ବଟନ୍ ଠାରୁ ଚାଲୁ କରନ୍ତୁ ଏବଂ ପ୍ରଦର୍ଶନକୁ ରିଫ୍ରେଶ୍ କରିବା ପାଇଁ ନୂତନ ଭାବେ ୱେବଭ୍ୟୁ ଖୋଲନ୍ତୁ।

### ପ୍ୟାଟର୍ନ: ଟର୍ମିନାଲ୍ କମାଣ୍ଡ → ପାର୍ସ ଆଉଟପୁଟ୍ → HTML ରେ ଦେଖାନ୍ତୁ

ଏହା ସର୍ବାଧିକ ସାଧାରଣ ପ୍ଲଗିନ ପ୍ୟାଟର୍ନ। ଆପଣ ଏକ କମାଣ୍ଡ ଚାଲାନ୍ତି, ଫଳାଫଳକୁ ପାର୍ସ କରନ୍ତି, ଏବଂ ଏହାକୁ ଦୃଶ୍ୟମାନ ଭାବେ ଦେଖାଇବେ।
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### ପ୍ୟାଟର୍ନ: ଇଣ୍ଟର୍ଆକ୍ଟିଭ୍ ଡ୍ୟାସ୍ବୋର୍ଡ ସହିତ ଆଟୋ-ରିଫ୍ରେଶ୍
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### ପ୍ୟାଟର୍ନ: Webview ରେ ସେଟିଂଗୁଡିକୁ ଦେଖାଇବା

> **ଟିପ୍ପଣୀ:** Webviews ହେଉଛି ଦେଖାଇବାକୁ ମାତ୍ର — ସେଗୁଡିକୁ ପ୍ଲଗିନ API କୁ କଲ୍ ବ୍ୟବହାର କରିପାରିବେ ନ���ହିଁ। ସେଟିଂଗୁଡିକୁ ସଂଶୋଧନ କରିବା ପାଇଁ ଆପଣଙ୍କର ସାଇଡ୍ବାର ବଟନ୍ ହ୍ୟାଣ୍ଡଲର୍ ମଧ୍ୟରେ `ctx.settings` ବ୍ୟବହାର କରନ୍ତୁ, ଏବଂ ବର୍ତ୍ତମାନ ଅବସ୍ଥା ଦେଖାଇବା ପାଇଁ `openWebview()` ବ୍ୟବହାର କରନ୍ତୁ।
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Part 4: ଆପଣଙ୍କର ପ୍ଲଗିନ୍ ପ୍ରକାଶ କରିବା

### ପଦକ୍ଷେପ 1: ସ୍ଥାନୀୟ ଭାବରେ ପରୀକ୍ଷା କରନ୍ତୁ

1. ଆପଣଙ୍କର ପ୍ଲଗିନ୍ କୁ `~/.wia-soom/plugins/{your-plugin}/` କୁ କପି କରନ୍ତୁ
2. WIA SOOM ପୁନରାମ୍ଭ କରନ୍ତୁ
3. ଏହା କାମ କରୁଛି କି ନାହିଁ ଯାଞ୍ଚ କରନ୍ତୁ: ସାଇଡ୍ବାର ବଟନ୍ ଦେଖାଯାଉଛି, ବିଶେଷତାଗୁଡିକ ସଠିକ୍ ଭାବେ କାମ କରୁଛି
4. କିନ୍ତୁ କେଉଁଠି ଟର୍ମିନାଲ୍ ସଂଯୋଗ ନାହିଁ ହେଲେ କଣ ଘଟିବ?

### ପଦକ୍ଷେପ 2: ସମ୍ମିଳନ ପାଇଁ ପ୍ରସ୍ତୁତ କରନ୍ତୁ

ଆପଣଙ୍���ର ପ୍ଲଗିନ୍ ଫୋଲ୍ଡରରେ ଏହା ଥିବା ଆବଶ୍ୟକ:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**ଆବଶ୍ୟକ `package.json` କ୍ଷେତ୍ର:**

| କ୍ଷେତ୍ର | ବିବରଣୀ | ଉଦାହରଣ |
|-------|-------------|---------|
| `name` | ଏକକ ନିଜସ୍ୱ kebab-case ID | `"my-awesome-plugin"` |
| `version` | ସେମାଣ୍ଟିକ୍ ଭର୍ସନ | `"1.0.0"` |
| `description` | ଏକ-ଲାଇନ୍ ବିବରଣୀ | `"Monitors nginx access logs in real-time"` |
| `author` | ତୁମର ନାମ | `"John Doe"` |
| `main` | ଏଣ୍ଟ୍ରୀ ପଏଣ୍ଟ | `"index.js"` |

**ବିକଳ୍ପ କ୍ଷେତ୍ର:**

| କ୍ଷେତ୍ର | ବିବରଣୀ |
|-------|-------------|
| `license` | ଲାଇସେନ୍ସ ପ୍ରକାର (MIT ସୁପାରିଶ କରାଯାଇଛି) |
| `keywords` | ସନ୍ଧାନ ଟ୍ୟାଗ୍‌ଗୁଡିକର ଏକ ଆରେ |
| `soom.minVersion` | ଆବଶ୍ୟକ WIA SOOM ସର୍ବନିମ୍ନ ସଂସ୍କରଣ |

### ପଦକ୍ଷେପ 3: ପ୍ଲଗଇନ୍ ରେଜିଷ୍ଟ୍ରୀକୁ ସମର୍ପଣ କରନ୍ତୁ

1. ****Package** your plugin as a ZIP file
2. **Add** ତୁମର ପ୍ଲଗଇନ୍ `plugins/{your-plugin-name}/` କୁ
3. **Submit** ଏକ Pull Request

### ପଦକ୍ଷେପ 4: ପରୀକ୍ଷା ଏବଂ ମଞ୍ଜୁରୀ

ଆମେ ପ୍ଲଗଇନ୍‌ଗୁଡିକୁ ପରୀକ୍ଷା କରୁଛୁ:

- **ସୁରକ୍ଷା** — କୌଣସି ଖତରନାକ API ନାହିଁ (ଦେଖନ୍ତୁ [Security Rules](#security-rules))
- **ଗୁଣବତ୍ତା** — ଏହା କାମ କରେ କି? କୋଡ୍ କ୍ଲିନ୍ କି?
- **ଲାଭଦାୟକତା** — ଏହା କୌଣସି ବାସ୍ତବ ଅସୁବିଧା ସମାଧାନ କରେ କି?

ମଞ୍ଜୁରୀ ପରେ:
1. ତୁମର ପ୍ଲଗଇନ୍ `registry.json` କୁ ଯୋଡାଯିବ
2. `dist/` ରେ ଏକ ZIP bundle ସୃଷ୍ଟି କରାଯିବ
3. ତୁମର ପ୍ଲଗଇନ୍ ସମସ୍ତ WIA SOOM ବ୍ୟବହାରକାରୀଙ୍କ ପାଇଁ **Plugin Store** ରେ ଦେଖାଯିବ!

---

## ଅଂଶ 5: ସର୍ବୋତ୍ତମ ପ୍ରଥା

### ସୁରକ୍ଷା ନିୟମ

ଏହି ନିୟମଗୁଡିକ **ବାଧ୍ୟତାମୂଳକ**। ଯେଉଁ ପ୍ଲଗଇନ୍ ଏହାକୁ ଉଲ୍ଲଙ୍ଘନ କରେ ସେଗୁଡିକୁ ଅସ୍ୱୀକୃତ ���ରାଯିବ।

| ନିୟମ | କାହିଁକି |
|------|-----|
| **କଦାଚିତ** `eval()` କିମ୍ବା `new Function()` ବ୍ୟବହାର କରନ୍ତୁ | କୋଡ୍ ଇଞ୍ଜେକ୍ସନ୍ ଝୁଲି |
| **କଦାଚିତ** `child_process`, `exec()`, `spawn()` ବ୍ୟବହାର କରନ୍ତୁ | କମାଣ୍ଡ୍‌ଗୁଡିକ ପାଇଁ କେବଳ `ctx.terminal.send()` ବ୍ୟବହାର କରନ୍ତୁ |
| **କଦାଚିତ** ବାହ୍ୟ URL ଗୁଡିକୁ ଆଣନ୍ତୁ | ବିଶେଷଣ: `wiasoom.com` API ଏଣ୍ଡପୋଇଣ୍ଟ |
| **କଦାଚିତ** `process.env` କୁ ପ୍ରବେଶ କରନ୍ତୁ | ପରିବେଶ ଚର ଗୁଡିକରେ ଗୁପ୍ତତା ଥାଇପାରେ |
| **କଦାଚିତ** `require('fs')` ସିଧାସଳଖ ବ୍ୟବହାର କରନ୍ତୁ | ସଂଗ୍ରହ ପାଇଁ `ctx.settings` ବ୍ୟବହାର କରନ୍ତୁ, ଫାଇଲ୍ ହସ୍ତାନ୍ତର ପାଇଁ `ctx.sftp` ବ୍ୟବହାର କରନ୍ତୁ |
| **କଦାଚିତ** npm ବାହ୍ୟ ପ୍ୟାକେଜ୍ ବ୍ୟବହାର କରନ୍ତୁ | କେବଳ ପ୍ୟୁର୍ ଜାଭାସ୍କ୍ରିପ୍ଟ — କୌଣସି node_modules ନାହିଁ |
| **ବାଧ୍ୟ** ସମସ୍ତ ଦୂର କମାଣ୍ଡ୍‌ଗୁଡିକ ପାଇଁ `ctx.terminal.send()` ବ୍ୟବହାର କରନ୍ତୁ | ଏହା ସୁରକ୍ଷିତ SSH ଚ୍ୟାନେଲ୍ ମାଧ୍ୟମରେ ଯାଏ |
| **ବାଧ୍ୟ** `deactivate()` ରେ ସଫା କରନ୍ତୁ | ଲିସ୍ଟେନର୍‌ଗୁଡିକୁ ହଟାନ୍ତୁ, ଇଣ୍ଟରଭାଲ୍‌ଗୁଡିକୁ କ୍ଲିୟାର୍ କରନ୍ତୁ |

### ତ୍ରୁଟି ହାଣ୍ଡଲିଂ

ସଦା ଝୁଲି ଚାଲୁଥିବା କାର୍ଯ୍ୟଗୁଡିକୁ try/catch ମଧ୍ୟରେ ଘେରନ୍ତୁ:
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
### deactivate() ରେ ସଫା କରନ୍ତୁ

ଯଦି ତୁମର ପ୍ଲଗଇନ୍ ଇଣ୍ଟରଭାଲ୍, ଲିସ୍ଟେନର୍ କିମ୍ବା ସବ୍ସ୍କ୍ରିପ୍ସନ୍ ସୃଷ୍ଟି କରେ — ସେଗୁଡିକୁ ସଫା କରନ୍ତୁ:
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
### i18n ସମର୍ଥନ

WIA SOOM 254 ଭାଷାକୁ ସମର୍ଥନ କରେ। ତୁମର ପ୍ଲଗଇନ୍ ଲେବଲ୍‌କୁ ଅନୁବାଦ ଯୋଗ୍ୟ କରିବା ପାଇଁ, ସହଜ ପଦ୍ଧତି ବ��ୟବହାର କର:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## ଅଂଶ 6: ବାସ୍ତବ-ଜଗତର ଉଦାହରଣ

### ଉଦାହରଣ 1: ସର୍ଭର ଡିସ୍କ ଚେକର୍

ଦୂରସର୍ଭରରେ `df -h` ଚାଲାଏ ଏବଂ ସ୍ଥିତି ପଟରେ ବ୍ୟବହୃତ/ଲଭ୍ୟ ସ୍ଥାନ ଦେଖାଏ।
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### ଉଦାହରଣ 2: TODO ପରିଚାଳକ

ଏକ ପ୍ଲଗଇନ୍ ଯାହା ଏକ TODO ତାଲିକାକୁ ପରିଚାଳନା କରେ ସେଟିଂସ୍ ପାଇଁ ଦୀର୍ଘକାଳୀନ ସଂଗ୍ରହ ଏବଂ ପ୍ରଦର୍ଶନ ପାଇଁ ଏକ ଓବ୍‌ଜେକ୍ଟ ବ୍ୟବହାର କରେ।

> **ଡିଜାଇନ୍ ପ୍ୟାଟର୍ନ:** କାରଣ ଓବ୍‌ଜେକ୍ଟଗୁଡିକ ସିଧାସଳଖ ପ୍ଲଗଇନ୍ API କୁ କଲ୍ କରିପାରେ ନାହିଁ, ଏହି ପ୍ଲଗଇନ୍ "ସ୍ନାପସଟ୍" ପଦ୍ଧତି ବ୍ୟବହାର କରେ — ଏହା ସେଟିଂସ୍ରୁ TODO ଗୁଡିକୁ ପଢ଼େ, ସେଗୁଡିକୁ ପଢ଼ିବାକୁ ହେଲେ HTML ଭାବରେ ଦେଖାଏ, ଏବଂ ଆଇଟମ୍ ଯୋଡିବା ପ���ଇଁ ସାଇଡ୍‌ବାର୍-ଆଧାରିତ କାର୍ଯ୍ୟଗୁଡିକୁ ପ୍ରଦାନ କରେ। ଓବ୍‌ଜେକ୍ଟ ଏକ **ପ୍ରଦର୍ଶନ** ��ଳ, ଇଣ୍ଟର୍ଆକ୍ଟିଭ୍ ଫର୍ମ୍ ନୁହେଁ।
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### ଉଦାହରଣ 3: ତ୍ରୁଟି ନିରୀକ୍ଷକ

ଟର୍ମିନାଲ୍ ଆଉଟପୁଟ୍ କୁ ନିରୀକ୍ଷଣ କରେ ଏବଂ ନିର୍ଦ୍ଧିଷ୍ଟ ମାନ୍ଦ୍ରିକା ଚିହ୍ନିତ ହେଲେ ଏକ ସୂଚନା ପଠାଇଥାଏ।
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## ଅନୁଲେଖ: ବର୍ଗ ଓ ଚିହ୍ନ

### ପ୍ଲଗଇନ ବର୍ଗ (29)

ଏହାକୁ ଆପଣଙ୍କର `package.json` `keywords` ରେ ବ୍ୟବହାର କରନ୍ତୁ କିମ୍ବା ରେଜିଷ୍ଟ୍ରୀକୁ ଦାଖଲ କରିବା ସମୟରେ:

| ବର୍ଗ       | ବର୍ଣ୍ଣନା                     |
|-------------|-------------------------------|
| `server`    | ସାଧାରଣ ସର୍ଭର ପରିଚାଳନା    |
| `devtools`  | ବିକାଶ ଉପକରଣ               |
| `calculator`| ଗଣନାକାର ଓ ପରିବର୍ତ୍ତକ      |
| `simulator` | ସିମ୍ୟୁଲେଟର                  |
| `game`      | ଟର୍ମିନାଲ ଖେଳ               |
| `business`  | ବ୍ୟବସାୟ ଉପକରଣ            |
| `security`  | ସୁରକ୍ଷା ଓ ନିରୀକ୍ଷଣ        |
| `web`       | ୱେବ ସର୍ଭର ପରିଚାଳନା       |
| `education` | ଶିକ୍ଷା ସମ୍ବନ୍ଧୀୟ ଉପକରଣ  |
| `health`    | ସ୍ୱାସ୍ଥ୍ୟ ସମ୍ବନ୍ଧୀୟ ଉପକରଣ |
| `islamic`   | ଇସ୍ଲାମିକ ଉପକରଣ (ନମାଜ ସମୟ, ଇତ୍ୟାଦି) |
| `science`   | ବିଜ୍ଞାନ ଉପକରଣ             |
| `quantum`   | କ୍ୱାଣ୍ଟମ କମ୍ପ୍ୟୁଟିଂ ଉପକରଣ |
| `ai`        | AI-ଶକ୍ତିଶାଳୀ ଉପକରଣ        |
| `biotech`   | ବାୟୋଟେକ୍ନୋଲୋଜୀ ଉପକରଣ    |
| `space`     | ଅନ୍ତରିକ୍ଷ ଓ ତାରାଜ୍ୟ ଉପକରଣ |
| `network`   | ନେଟୱର୍କ ଉପକରଣ            |
| `database`  | ଡାଟାବେସ ପରିଚାଳନା          |
| `monitoring`| ସର୍ଭର ନିରୀକ୍ଷଣ            |
| `devops`    | DevOps ଓ CI/CD               |
| `utility`   | ସାଧାରଣ ସାହାଯ୍ୟକାରୀ        |
| `design`    | ଡିଜାଇନ ଉପକରଣ             |
| `ecommerce`  | ଇ-କମର୍ସ ଉପକରଣ            |
| `automation` | ସ୍ୱୟଂଚାଳନ ଉପକରଣ          |
| `kpop`      | K-pop ସମ୍ବନ୍ଧୀୟ ଉପକରଣ     |
| `accessibility` | ସୁବିଧାଜନକ ଉପକରଣ      |
| `analytics` | ବିଶ���ଲେଷଣ ଓ ରିପୋର୍ଟିଂ      |
| `wia`       | WIA ପରିବେଶ ଉପକରଣ          |
| `all`       | ସମସ୍ତ ବର୍ଗରେ ଦେଖାଯାଏ      |

### ସୁପାରିଶିତ ଚିହ୍ନ (Lucide)

| ଚିହ୍ନ ନାମ   | ବ୍ୟବହାର କରନ୍ତୁ           |
|---------------|-----------------------------|
| `server`      | ସର୍ଭର ପରିଚାଳନା          |
| `shield`      | ସୁରକ୍ଷା                    |
| `database`    | ଡାଟାବେସ                   |
| `activity`    | ନିରୀକ୍ଷଣ                  |
| `terminal`    | ଟର୍ମିନାଲ ଉପକରଣ          |
| `code`        | ବିକାଶ                      |
| `hard-drive`  | ଡିସ୍କ/ସ୍ଟୋରେଜ୍           |
| `network`     | ନେଟୱର୍କ                   |
| `lock`        | ପ୍ରମାଣିକରଣ/ଇନ୍କ୍ରିପ୍ସନ୍  |
| `eye`         | ଦେଖିବା/ନିରୀକ୍ଷଣ         |
| `check-square`| କାମ/TODO                   |
| `layout-dashboard` | ଡ୍ୟାଶବୋର୍ଡ          |
| `settings`    | କନଫିଗରେସନ୍               |
| `zap`         | ��୍ୱୟଂଚାଳନ                |
| `globe`       | ୱେବ/ଆନ୍ତର୍ଜାତୀୟ         |

ସମସ୍ତ 1,500+ ଚିହ୍ନ ଦେଖନ୍ତୁ: [lucide.dev/icons](https://lucide.dev/icons)

---

## ସାହାଯ୍ୟ ଆବଶ୍ୟକ?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>କିଛି ଅଦ୍ଭୁତ ତିଆରି କରନ୍ତୁ। ଏହାକୁ ସାମ୍ପ୍ରଦାୟ ସହିତ ଅଂଶୀଦାର କରନ୍ତୁ।</em></p>
<p align="center"><em>— WIA SOOM ଦଳ</em></p>
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
