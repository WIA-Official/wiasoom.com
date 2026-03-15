<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM প্লাগিন ডেভেলপার গাইড</h1>
<p align="center"><strong>৫ মিনিটত আপোনাৰ নিজৰ প্লাগিন বনাওক।</strong></p>
<p align="center">শক্তিশালী ছাৰ্ভাৰ টুল, ডেছবোর্ড, আৰু স্বয়ংক্ৰিয়তা সৃষ্টি কৰক — WIA SOOMৰ ভিতৰতেই।</p>

---

## বিষয়বস্তুৰ তালিকা

- [অংশ ১: তাড়াতাড়ি আৰম্ভ — আপোনাৰ প্ৰথম প্লাগিন ৫ মিনিটত](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [অংশ ২: প্লাগিন কনটেক্সট API ৰেফাৰেঞ্চ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [অংশ ৩: ৱেবভিউৰ সৈতে কাষ্টম UI নিৰ্মাণ](#part-3-building-custom-ui-with-webviews)
- [অংশ ৪: আপোনাৰ প্লাগিন প্ৰকাশ কৰা](#part-4-publishing-your-plugin)
- [অংশ ৫: সৰ্বশ্ৰেষ্ঠ প্ৰথা](#part-5-best-practices)
- [অংশ ৬: বাস্তৱ উদাহৰণ](#part-6-real-world-examples)
- [অ্যাপেনডিক্স: শ্ৰেণী আৰু আইকন](#appendix-categories--icons)

---

## অংশ ১: তাড়াতাড়ি আৰম্ভ — আপোনাৰ প্ৰথম প্লাগিন ৫ মিনিটত

### আপুনি কি বনাব

এখন "Hello World" প্লাগিন যি চাৰ্দ্বাৰত এখন বুটাম যোগ কৰে। ক্লিক কৰিলে, ই এখন নোটিফিকেশন দেখুৱায়।

### পদক্ষেপ ১: প্লাগিন ফোল্ডাৰ সৃষ্টি কৰক
§§§CHUNK_SEPARATOR§§§
### পদক্ষেপ ২: package.json ��ৃষ্টি কৰক
§§§CHUNK_SEPARATOR§§§
**আৱশ্যক ক্ষেত্ৰসমূহ:** `name`, `version`, `description`, `author`, `main`

### পদক্ষেপ ৩: index.js সৃষ্টি কৰক
§§§CHUNK_SEPARATOR§§§
### পদক্ষেপ ৪: WIA SOOM পুনৰাৰম্ভ কৰক

অ্যাপটো পুনৰাৰম্ভ কৰক (অথবা ছেটিংছ → প্লাগিনত প্লাগিনটো বন্ধ/খুলি)।

আপুনি চাৰ্দ্বাৰত এখন **"Hello World"** বুটাম দেখিবলৈ পাব। ক্লিক কৰক — আপুনি এখন সফলতা নোটিফিকেশন দেখিব!

### ই কেনেকৈ কাম কৰে
§§§CHUNK_SEPARATOR§§§
---

## অংশ ২: প্লাগিন কনটেক্সট API ৰেফাৰেঞ্চ

যেতিয়া আপোনাৰ `activate(context)` ফাংচনটো কল কৰা হয়, `context` (অথবা `ctx`) এই APIসমূহ প্ৰদান কৰে:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — দূৰৱৰ্তী ছাৰ্ভাৰত কমাণ্ড চলাও

#### `terminal.send(sessionId, data)`

এখন সক্ৰিয় টাৰ্মিনেল ছেছনলৈ এখন কমাণ্ড (অথবা যিকোনো ডাটা) পঠিয়াওক।

| প্যারামিটাৰ | প্ৰকাৰ | বিৱৰণ |
|-----------|------|-------------|
| `sessionId` | `string` | পঠিয়াবলগীয়া টাৰ্মিনেল ছেছন |
| `data` | `string` | পঠিয়াবলগীয়া কমাণ্ড বা ডাটা |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

এখন টাৰ্মিনেল ছেছনৰ পৰা সকলো আউটপুটৰ বাবে চাবলৈ চাব। এটা **অসুবস্ক্ৰাইব ফাংচন** উভতি দিয়ে।

| প্যারামিটাৰ | প্ৰকাৰ | বিৱৰণ |
|-----------|------|-------------|
| `sessionId` | `string` | চাবলৈ টাৰ্মিনেল ছেছন |
| `callback` | `(data: string) => void` | প্ৰতিটো আউটপুটৰ অংশৰ সৈতে কল কৰা হয় |
| **ফিৰাইছে** | `() => void` | শুনা বন্ধ কৰিবলৈ এইটো কল কৰক |
§§§CHUNK_SEPARATOR§§§
**গুরুত্বপূর্ণ:** সদায় অসুবস্ক্ৰাইব ফাংচনটো সংৰক্ষণ কৰক আৰু মেমৰি লিক ৰোধ কৰিবলৈ `deactivate()`ত ইয়াক কল কৰক।

---

### `ctx.sftp` — ফাইল স্থানান্তৰ

> **অৱস্থা: শীঘ্ৰেই আহিব** — SFTP API সংজ্ঞায়িত হৈছে কিন্তু এতিয়াও এপৰ SFTP ইঞ্জিনৰ সৈতে সংযুক্ত কৰা হোৱা নাই। `list()` বৰ্তমান এটা খালী এৰে উভতি দিয়ে, আৰু `upload()`/`download()` কোনো কাৰ্য্য নহয়। এইটো ভবিষ্যতৰ মুক্তিত সম্পূৰ্ণ ৰূপে কাৰ্য্যকৰী কৰা হ'ব। এতিয়ালৈ, `scp` বা `rsync` কমাণ্ডৰ সৈতে `ctx.terminal.send()` ব্যৱহাৰ কৰক।

#### `sftp.list(sessionId, path)`

এখন দূৰৱৰ্তী ডিৰেক্টৰিত ফাইলসমূহ তালিকা কৰক।
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

���্থানীয় মেশিনৰ পৰা দূৰৱৰ্তী ছাৰ্ভাৰত এখন ফাইল আপলোড কৰক।
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

দূৰৱৰ্তী ছাৰ্ভাৰৰ পৰা স্থানীয় মেশিনলৈ এখন ফাইল ডাউনলোড কৰক।
§§§CHUNK_SEPARATOR§§§
**কাৰ্য্যপদ্ধতি (SFTP API লাইভ হোৱাৰ আগলৈ):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — ব্যৱহাৰকাৰী ইণ্টাৰফেচ

#### `ui.addSidebarButton(options)`

WIA SOOM চাৰ্দ্বাৰত এখন বুটাম যোগ কৰক।

| বিকল্প | প্ৰকাৰ | আৱশ্যক | বিৱৰণ |
|--------|------|----------|-------------|
| `id` | `string` | নহয় | অনন্য ID (প্লাগিন নামৰ বাবে ডিফল্ট) |
| `icon` | `string` | হ্যাঁ | Lucide আইকনৰ নাম (যেনে, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | হ্যাঁ | চাৰ্দ্বাৰত দেখুওৱা বুটামৰ পাঠ্য |
| `onClick` | `() => void` | হ্যাঁ | বুটামটো ক্লিক কৰিলে কল কৰা ফাংচন |
§§§CHUNK_SEPARATOR§§§
**আইকন উল্লেখ:** উপলব্ধ সকলো আইকন চাবলৈ [lucide.dev/icons](https://lucide.dev/icons) ত যোৱা।

> **সামঞ্জস্য নোট:** কিছুমান পুৰণি প্লাগিনে `addSidebarButton(id, icon, label, onClick)`ৰ দৰে স্থানীয় আৰ্হি ব্যৱহাৰ কৰে। চৰকাৰী APIয়ে ওপৰত বৰ্ণিত **বিকল্প বস্তু** ব্যৱহাৰ কৰে। নতুন প্লাগিনৰ বাবে সদায় বস্তু শৈলী ব্যৱহাৰ কৰক।

#### `ui.openWebview(options)`

কাষ্টম HTML সামগ্ৰীৰ সৈতে এখন পপআপ উইণ্ডো খোলক। এইদৰে আপুনি সমৃদ্ধ UI নিৰ্মাণ কৰে।

| বিকল্প | প্ৰকাৰ | বিৱৰণ |
|--------|------|-------------|
| `title` | `string` | উইণ্ডোৰ শিৰোনাম |
| `html` | `string` | ৰেণ্ডাৰ কৰিবলৈ সম্পূৰ্ণ HTML সামগ্ৰী |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> চাওক [Part 3](#part-3-building-custom-ui-with-webviews) উন্নত ৱেবভিউ পেটাৰ্নৰ বাবে।

#### `ui.showNotification(type, message)`

এখন টোস্ট নোটিফিকেশন প্ৰদৰ্শন কৰক।

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | নোটিফিকেশ্যনৰ শৈলী |
| `message` | `string` | প্ৰদৰ্শন কৰিবলৈ পাঠ্য |
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

তলৰ স্থিতি বাৰত এখন স্থায়ী পাঠ্য আইটেম যোগ কৰক।

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | এই স্থিতি আইটেমৰ বাবে অনন্য ID |
| `text` | `string` | প্ৰদৰ্শন কৰিবলৈ পাঠ্য |
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

### `ctx.settings` — স্থায়ী সংৰক্��ণ

প্লাগিনৰ ছেটিংছ স্থায়ীভাৱে `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`ত সংৰক্ষণ কৰা হয়।

#### `settings.get(key)`

এখন সংৰক্ষিত মান পঢ়ক।
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
যদি কীটো নাই থাকে তেন্তে `undefined` উভতি দিয়ে।

#### `settings.set(key, value)`

এখন মান সংৰক্ষণ কৰক। ষ্ট্ৰিং, সংখ্যা, বুলিয়ান, এৰি আৰু অবজেক্ট সমৰ্থন কৰে।
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**উদাহৰণ: ব্যৱহাৰকাৰীৰ পছন্দসমূহ মনত ৰাখক**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — AI সংযোগ

> **অৱস্থা: শীঘ্ৰে আহিব** — AI API সংজ্ঞায়িত হৈছে কিন্তু এতিয়ালৈকে Soomyৰ সৈতে সংযুক্ত নহয়। বৰ্তমান `{ response: 'AI not yet connected' }` উভতি দিয়ে। পূৰ্ণ AI সংযোগৰ বাবে ভবিষ্যত মুক্তিৰ পৰিকল্পনা কৰা হৈছে।

#### `ai.chat(messages, options?)`

AI সহায়কৰ (Soomy) সৈতে বাৰ্তা প্ৰেৰণ কৰক।
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

## Part 3: ৱেবভিউৰ সৈতে কাষ্টম UI নিৰ্মাণ

`openWebview()` API আপোনাক HTML, CSS, আৰু JavaScriptৰ সৈতে ডেছব'ৰ্ড UI নিৰ্মাণ কৰিবলৈ অনুমতি দিয়ে — সকলো এখন পপআপ উইণ্ডোৰ ভিতৰত।

> **গুরুত্বপূর্ণ সীমাবদ্ধতা:** ৱেবভিউসমূহ **প্ৰদৰ্শন-মাত্ৰ**। সিহঁতে প্লাগিন APIসমূহলৈ (যেনে `ctx.settings`, `ctx.terminal`, আদি) কল কৰিব নোৱাৰে। সকলো ব্যৱহাৰকাৰী কাৰ্যৰ বাবে সাইডবাৰ বুটাম ব্যৱহাৰ কৰক, আৰু বৰ্তমান অৱস্থা প্ৰদৰ্শন কৰিবলৈ `openWebview()` ব্যৱহাৰ কৰক। যদি আপোনাৰ ইন্টাৰেক্টিভ বৈশিষ্ট্যৰ প্ৰয়োজন হয়, তেন্তে সিহঁতক সাইডবাৰ বুটামৰ পৰা ���ক্ৰিয় কৰক আৰু প্ৰদৰ্শন ৰিফ্ৰেশ কৰিবলৈ ৱেবভিউ পুনৰ খোলক।

### পেটাৰ্ন: টাৰ্মিনেল কমাণ্ড → আউটপুট পাৰ্স কৰক → HTMLত প্ৰদৰ্শন কৰক

এইটো আটাইতকৈ সাধাৰণ প্লাগিন পেটাৰ্ন। আপুনি এখন কমাণ্ড চলায়, ফলাফল পাৰ্স কৰে, আৰু দৃশ্যমানভাৱে প্ৰদৰ্শন কৰে।
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### পেটাৰ্ন: স্বয়ং-ৰিফ্ৰেশৰ সৈতে ইন্টাৰেক্টিভ ডেছব'ৰ্ড
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### পেটাৰ্ন: ৱেবভিউত ছেটিংছ প্ৰদৰ্শন কৰা

> **মন্তব্য:** ৱেবভিউসমূহ প্ৰদৰ্শন-মাত্ৰ — সিহঁতে প্লাগিন APIসমূহলৈ কল কৰিব নোৱাৰে। ছেটিংছ সলনি কৰিবলৈ আপোনাৰ সাইডবাৰ বুটাম হেণ্ডলাৰত `ctx.settings` ব্যৱহাৰ কৰক, আৰু বৰ্তমান অৱস্থা প্ৰ���ৰ্শন কৰিবলৈ `openWebview()` ব্যৱহাৰ কৰক।
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Part 4: আপোনাৰ প্লাগিন প্ৰকাশ কৰা

### পদক্ষেপ 1: স্থানীয়ভাৱে পৰীক্ষা কৰক

1. আপোনাৰ প্লাগিনটো `~/.wia-soom/plugins/{your-plugin}/`ত কপি কৰক।
2. WIA SOOM পুনৰ আৰম্ভ কৰক।
3. সেয়া কাম কৰে নে নাই পৰীক্ষা কৰক: সাইডবাৰ বুটাম প্ৰদৰ্শিত হয়, বৈশিষ্ট্যসমূহ সঠিকভাৱে কাম কৰে।
4. এজ কেচসমূহ পৰীক্ষা কৰক: যদি কোনো টাৰ্মিনেল সংযুক্ত নহয় তেন্তে কি হয়?

### পদক্ষেপ 2: জমা দিয়াৰ বাবে প্ৰস্তুত কৰক

আপোনাৰ প্লাগিন ফোল্ডাৰটোত অন্তর্ভুক্ত কৰিব লাগিব:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**আৱশ্যক `package.json` ক্ষেত্ৰসমূহ:**

| ক্ষেত্ৰ | বিৱৰণ | উদাহৰণ |
|-------|-------------|---------|
| `name` | অনন্য kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | এটা লাইন বিৱৰণ | `"Monitors nginx access logs in real-time"` |
| `author` | আপোনাৰ নাম | `"John Doe"` |
| `main` | প্ৰৱেশ বিন্দু | `"index.js"` |

**ঐচ্ছিক ক্ষেত্ৰসমূহ:**

| ক্ষেত্ৰ | বিৱৰণ |
|-------|-------------|
| `license` | লাইচেঞ্চৰ প্ৰকাৰ (MIT সুপারিশ কৰা হৈছে) |
| `keywords` | অনুসন্ধান ট্যাগৰ এৰি |
| `soom.minVersion` | আৱশ্যক সৰ্বনিম্ন WIA SOOM সংস্কৰণ |

### পদক্ষেপ ৩: প্লাগিন ৰেজিষ্ট্ৰিত জমা কৰক

1. ****Package** your plugin as a ZIP file
2. **Add** আপোনাৰ প্লাগিন `plugins/{your-plugin-name}/` ত
3. **Submit** এটা Pull Request

### পদক্ষেপ ৪: পৰ্যালোচনা আৰু অনুমোদন

আমাৰ প্ৰতিটো প্লাগিনৰ পৰ্যালোচনা কৰা হয়:

- **নিরাপত্তা** — বিপজ্জনক API নাই (দেখক [Security Rules](#security-rules))
- **গুণ** — ই কাম কৰে নে? কোডটো পৰিষ্কাৰ নে?
- **উপযোগিতা** — ই কি বাস্তৱ সমস্যা সমাধান কৰে?

অনুমোদনৰ পাছত:
1. আপোনাৰ প্লাগিন `registry.json` ত যোগ কৰা হয়
2. `dist/` ত এটা ZIP bundle সৃষ্টি কৰা হয়
3. আপোনাৰ প্লাগিন সকলো WIA SOOM ব্যৱহাৰকাৰীৰ বাবে **Plugin Store** ত প্ৰদৰ্শিত হয়!

---

## অংশ ৫: সৰ্বোত্তম অভ্যাসসমূহ

### নিরাপত্তা নিয়ম

এই নিয়মসমূহ **বাধ্যতামূলক**। যি প্লাগিনসমূহ এই নিয়মসমূহ ভংগ কৰে সিহঁত বাতিল কৰা হ'ব।

| নিয়ম | ক���য় |
|------|-----|
| **কদাচিৎ** `eval()` বা `new Function()` ব্যৱহাৰ কৰক | কোড ইনজেকশ্যন ৰিস্ক |
| **কদাচিৎ** `child_process`, `exec()`, `spawn()` ব্যৱহাৰ কৰক | কমাণ্ডৰ বাবে কেৱল `ctx.terminal.send()` ব্যৱহাৰ কৰক |
| **কদাচিৎ** বাহ্যিক URL সমূহ আহৰণ কৰক | ব্যতিক্ৰম: `wiasoom.com` API endpoints |
| **কদাচিৎ** `process.env` ত প্ৰৱেশ কৰক | পৰিৱেশ ভেৰিয়েবলসমূহত গোপন তথ্য থাকিব পাৰে |
| **কদাচিৎ** `require('fs')` সোজাকৈ ব্যৱহাৰ কৰক | সংৰক্ষণৰ বাবে `ctx.settings` ব্যৱহাৰ কৰক, ফাইল স্থানান্তৰৰ বাবে `ctx.sftp` |
| **কদাচিৎ** npm বাহ্যিক পেকেজসমূহ ব্যৱহাৰ কৰক | কেৱল শুদ্ধ JavaScript — কোনো node_modules নাই |
| **বাধ্যতামূলক** সকলো দূৰবর্তী কমাণ্ডৰ বাবে `ctx.terminal.send()` ব্যৱহাৰ কৰক | এইটো সুৰক্ষিত SSH চেনেলৰ মাজেৰে যায় |
| **বাধ্যতামূলক** `deactivate()` ত পৰিষ্কাৰ কৰক | Listener, interval মচি পেলাওক |

### ত্ৰুটি ব্যৱস্থাপনা

সৰ্বদা চেষ্টা/কেচত বিপজ্জনক কাৰ্যকলাপসমূহ মুড়ি ৰাখক:
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
### deactivate() ত পৰিষ্কাৰ কৰা

যদি আপোনাৰ প্লাগিন interval, listener, বা subscription সৃষ্টি কৰে — সিহঁতক পৰিষ্কাৰ কৰক:
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
### i18n সমৰ্থন

WIA SOOM ২৫৪ ভাষা সমৰ্থন কৰে। আপোনাৰ প্লাগিনৰ লেবেল অনুবাদযোগ্য কৰিবলৈ, এটা সহজ পদ্ধতি ব্যৱহাৰ কৰক:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## অংশ ৬: বাস্তৱ উদাহৰণসম��হ

### উদাহৰণ ১: ছাৰ্ভাৰ ডিস্ক চেকাৰ

দূৰৱৰ্তী ছাৰ্ভাৰত `df -h` চলায় আৰু স্থিতি বাৰত ব্যৱহৃত/উপলব্ধ স্থান দেখুৱায়।
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### উদাহৰণ ২: TODO ব্যৱস্থাপক

এটা প্লাগিন যি TODO তালিকা ব্যৱস্থাপনা কৰে স্থায়ী সংৰক্ষণৰ বাবে ছেটিংসমূহ আৰু প্ৰদৰ্শনৰ বাবে এটা ৱেবভিউ ব্যৱহাৰ কৰি।

> **ডিজাইন পেটাৰ্ন:** যিহেতু ৱেবভিউসমূহ সোজাকৈ প্লাগিন API সমূহক কল কৰিব নোৱাৰে, এই প্লাগিনে "snapshot" পদ্ধতি ব্যৱহাৰ কৰে — ই ছেটিংসমূহৰ পৰা TODO সমূহ পঢ়ে, সিহঁতক পঢ়া-শুধা HTML হিচাপে ৰেণ্ডাৰ কৰে, আৰু সামগ্ৰী যোগ কৰাৰ বাবে সাইডবাৰ-ভিত্তিক কাৰ্যসমূহ প্ৰদান কৰ���। ৱেবভিউ এটা **প্ৰদৰ্শন** পৰ্যায়, ইন্টাৰেক্টিভ ফৰ্ম নহয়।
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### উদাহৰণ ৩: ত্ৰুটি পৰ্যবেক্ষক

টার্মিনেল আউটপুট পৰ্যবেক্ষণ কৰে আৰু নিৰ্দিষ্ট পেটাৰ্নসমূহ চিনাক্ত হলে এটা নোটিফিকেশন পঠিয়ায়।
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | সাধাৰণ ছাৰ্ভাৰ ব্যৱস্থাপনা |
| `devtools` | উন্নয়ন সঁজুলি |
| `calculator` | গণনাকারী আৰু ৰূপান্তৰক |
| `simulator` | অনুকৰণকাৰী |
| `game` | টাৰ্মিনেল গেম |
| `business` | ব্যৱসায়িক সঁজুলি |
| `security` | সুৰক্ষা আৰু পৰীক্ষা |
| `web` | ৱেব ছাৰ্ভাৰ ব্যৱস্থাপনা |
| `education` | শিক্ষা সঁজুলি |
| `health` | স্বাস্থ্য-সম্পৰ্কীয় সঁজুলি |
| `islamic` | ইসলামিক সঁজুলি (নামাজৰ সময়, আদি) |
| `science` | বৈজ্ঞানিক সঁ��ুলি |
| `quantum` | কোয়ান্টাম কম্পিউটিং সঁজুলি |
| `ai` | AI-চালিত সঁজুলি |
| `biotech` | জীৱবিজ্ঞান সঁজুলি |
| `space` | মহাকাশ আৰু জ্যোতিষ্কবিদ্যা সঁজুলি |
| `network` | নেটৱৰ্ক সঁজুলি |
| `database` | ডেটাবেছ ব্যৱস্থাপনা |
| `monitoring` | ছাৰ্ভাৰ পৰ্যবেক্ষণ |
| `devops` | DevOps আৰু CI/CD |
| `utility` | সাধাৰণ ইউটিলিটিজ |
| `design` | ডিজাইন সঁজুলি |
| `ecommerce` | ই-কমাৰ্চ সঁজুলি |
| `automation` | স্বচালন সঁজুলি |
| `kpop` | K-pop সম্পৰ্কীয় সঁজুলি |
| `accessibility` | প্ৰৱেশযোগ্যতা সঁজুলি |
| `analytics` | বিশ্লেষণ আৰু প্ৰতিবেদন |
| `wia` | WIA পৰিৱেশ সঁজুলি |
| `all` | সকলো শ্ৰেণীত উপস্থিত |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | ছাৰ্ভাৰ ব্যৱস্থাপনা |
| `shield` | সুৰক্ষা |
| `database` | ডেটাবেছ |
| `activity` | পৰ্যবেক্ষণ |
| `terminal` | টাৰ্মিনেল সঁজুলি |
| `code` | উন্নয়ন |
| `hard-drive` | ডিস্ক/সংগ্ৰহ |
| `network` | নেটৱৰ্কিং |
| `lock` | প্ৰমাণীকৰণ/এনক্রিপশ্যন |
| `eye` | চোৱা/পৰ্যবেক্ষণ |
| `check-square` | কাৰ্য/TODO |
| `layout-dashboard` | ডেছবোর্ড |
| `settings` | কনফিগাৰেচন |
| `zap` | স্বচালন |
| `globe` | ৱেব/আন্তর্জাতিক |

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
