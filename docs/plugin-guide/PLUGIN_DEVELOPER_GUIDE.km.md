<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">មគ្គូបដ្ឋានអ្នកអភិវឌ្ឍន៍ Plugin WIA SOOM</h1>
<p align="center"><strong>បង្កើត plugin របស់អ្នកក្នុងរយៈពេល 5 នាទី។</strong></p>
<p align="center">បង្កើតឧបករណ៍ម៉ាស៊ីនមេដ៏មានអំណាច, ការបង្ហាញទិន្នន័យ, និងការប្រព្រឹត្តអូតូម៉ាទិច — នៅក្នុង WIA SOOM។</p>

---

## តារាងមាតិកា

- [ផ្នែក 1: ចាប់ផ្តើមយ៉ាងឆាប់រហ័ស — Plugin ដំបូងរបស់អ្នកក្នុងរយៈពេល 5 នាទី](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ផ្នែក 2: ការយោង API Context Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ផ្នែក 3: ការបង្កើត UI ផ្ទាល់ខ្លួនជាមួយ Webviews](#part-3-building-custom-ui-with-webviews)
- [ផ្នែក 4: បោះពុម្ព Plugin របស់អ្នក](#part-4-publishing-your-plugin)
- [ផ្នែក 5: អនុប្បទានល្អ](#part-5-best-practices)
- [ផ្នែក 6: ឧទាហរណ៍ពិត](#part-6-real-world-examples)
- [បន្ថែម: ប្រភេទ និងរូបសញ្ញា](#appendix-categories--icons)

---

## ផ្នែក 1: ចាប់ផ្តើមយ៉ាងឆាប់រហ័ស — Plugin ដំបូងរបស់អ្នកក្នុងរយៈពេល 5 នាទី

### អ្វីដែលអ្នកនឹងបង្កើត

Plugin "សួស្តីពិភពលោក" ដែលបន្ថែមប៊ូតុងទៅកាន់ផ្នែកខាងឆ្វេង។ នៅពេលដែលចុចវា, វាបង្ហាញការជូនដំណឹងមួយ។

### ជំហាន 1: បង្កើតថត plugin
§§§CHUNK_SEPARATOR§§§
### ជំហាន 2: បង្កើត package.json
§§§CHUNK_SEPARATOR§§§
**វាលដែលត្រូវការ:** `name`, `version`, `description`, `author`, `main`

### ជំហាន 3: បង្កើត index.js
§§§CHUNK_SEPARATOR§§§
### ជំហាន 4: ចាប់ផ្តើម WIA SOOM

ចាប់ផ្តើមកម្មវិធីឡើងវិញ (ឬប្ដូរប៊ូតុង plugin នៅក្នុង ការកំណត់ → Plugins)។

អ្នកគួរតែឃើញប៊ូតុង **"សួស្តីពិភពលោក"** នៅក្នុងផ្នែកខាងឆ្វេង។ ចុចវា — អ្នកនឹងឃើញការជូនដំណឹងជោគជ័យ!

### វាដំណើរការ​យ៉ាងដូចម្តេច
§§§CHUNK_SEPARATOR§§§
---

## ផ្នែក 2: ការយោង API Context Plugin

នៅពេលដែលមុខងារ `activate(context)` របស់អ្នកត្រូវបានហៅ, `context` (ឬ `ctx`) ផ្តល់ឱ្យនូវ API ទាំងនេះ:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — រត់ពាក្យបញ្ជានៅលើម៉ាស���ីនមេឆ្���ាយ

#### `terminal.send(sessionId, data)`

ផ្ញើពាក្យបញ្ជា (ឬទិន្នន័យណាមួយ) ទៅកាន់សម័យ terminal ដែលសកម្ម។

| ប៉ារ៉ាម៉ែត្រ | ប្រភេទ | ការពិពណ៌នា |
|-----------|------|-------------|
| `sessionId` | `string` | សម័យ terminal ដែលត្រូវផ្ញើទៅ |
| `data` | `string` | ពាក្យបញ្ជា ឬទិន្នន័យដែលត្រូវផ្ញើ |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

ចុះឈ្មោះដើម្បីទទួលបានអ្វីដែលចេញពីសម័យ terminal ទាំងអស់។ ត្រឡប់មកវិញជាមុខងារ **មិនចុះឈ្មោះ**។

| ប៉ារ៉ាម៉ែត្រ | ប្រភេទ | ការពិពណ៌នា |
|-----------|------|-------------|
| `sessionId` | `string` | សម័យ terminal ដែលត្រូវមើល |
| `callback` | `(data: string) => void` | ត្រូវបានហៅជាមួយនឹងគ្រាប់ទិន្នន័យនីមួយៗ |
| **ត្រឡប់មកវិញ** | `() => void` | ហ��វានេះដើម្បីបញ្ឈប់ការស្តាប់ |
§§§CHUNK_SEPARATOR§§§
**សំខាន់:** តែងតែរក្សាទុកមុខងារមិនចុះឈ្មោះ ហើយហៅវានៅក្នុង `deactivate()` ដើម្បីចៀសវាងការលេចធ្លាយនៃអង្គចងចាំ។

---

### `ctx.sftp` — ការផ្ទេរឯកសារ

> **ស្ថានភាព: កំពុងមកដល់** — API SFTP ត្រូវបានកំណត់ ប៉ុន្តែមិនទាន់ត្រូវបានភ្ជាប់ទៅម៉ាស៊ីន SFTP របស់កម្មវិធីទេ។ `list()` បច្ចុប្បន្នត្រឡប់មកវិញជាអារេទទេ ហើយ `upload()`/`download()` គឺជាការប្រព្រឹត្តមិនមាន។ នេះនឹងត្រូវបានអនុវត្តពេញលេញនៅក្នុងការចេញផ្សាយអនាគត។ សម្រាប់ឥឡូវនេះ, ប្រើ `ctx.terminal.send()` ជាមួយពាក្យបញ្ជា `scp` ឬ `rsync` ជាជម្រើស។

#### `sftp.list(sessionId, path)`

បញ្ជីឯកសា���នៅក្នុងថតឆ្ងាយ។
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

ផ្ទេរឯកសារពីម៉ាស៊ីនក្នុងស្រុកទៅម៉ាស៊ីនមេឆ្ងាយ។ 
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

ទាញយកឯកសារពីម៉ាស៊ីនមេឆ្ងាយទៅម៉ាស៊ីនក្នុងស្រុក។ 
§§§CHUNK_SEPARATOR§§§
**ជម្រើស (រហូតដល់ API SFTP មានស្រាប់):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — អ្នកប្រើប្រាស់ផ្ទាំង

#### `ui.addSidebarButton(options)`

បន្ថែមប៊ូតុងទៅផ្នែកខាងឆ្វេង WIA SOOM។

| ជម្រើស | ប្រភេទ | ត្រូវការ | ការពិពណ៌នា |
|--------|------|----------|-------------|
| `id` | `string` | មិន | ID យូរអង្វែង (លំនាំដើមជាមួយឈ្មោះ plugin) |
| `icon` | `string` | បាទ | ឈ្មោះរូបសញ្ញា Lucide (ឧ. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | បាទ | អត��ថបទប៊ូតុងដែលបង្ហាញនៅក្នុងផ្នែកខាងឆ្វេង |
| `onClick` | `() => void` | បាទ | មុខងារដែលត្រូវបានហៅនៅពេលដែលប៊ូតុងត្រូវបានចុច |
§§§CHUNK_SEPARATOR§§§
**ការយោងរូបសញ្ញា:** ស្វែងរករូបសញ្ញាទាំងអស់ដែលមាននៅ [lucide.dev/icons](https://lucide.dev/icons)

> **កំណត់ចំណាំអាចប្រើបាន:** Plugin ជាច្រើនដែលចាស់ជាងនេះប្រើអាគុយម៉ង់តាមទីតាំងដូចជា `addSidebarButton(id, icon, label, onClick)`។ API ផ្លូវការប្រើវត្ថុ **ជម្រើស** ដូចដែលបានឯកសារខាងលើ។ តែងតែប្រើស្ទីលវត្ថុសម្រាប់ plugin ថ្មីៗ។

#### `ui.openWebview(options)`

បើកប្រអប់បង្ហាញជាមួយមាតិកា HTML ផ្ទាល់ខ្លួន។ នេះគឺជាវិធីដែលអ្នកបង្កើត UI ដ៏សម្បូរបែប។

| ជម្រើស | ប្រភេទ | កា���ពិពណ៌នា |
|--------|------|-------------|
| `title` | `string` | ចំណងជើងប្រអប់ |
| `html` | `string` | មាតិកា HTML ពេញលេញដើម្បីបង្ហាញ |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> មើល [ផ្នែក 3](#part-3-building-custom-ui-with-webviews) សម្រាប់គំរូ webview កម្រិតខ្ពស់។

#### `ui.showNotification(type, message)`

បង្ហាញការជូនដំណឹង toast។

| ប៉ារ៉ាម៉ែត្រ | ប្រភេទ | ការពិពណ៌នា |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ស្តាយនៃការជូនដំណឹង |
| `message` | `string` | អត្ថបទដែលត្រូវបង្ហាញ |
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

បន្ថែមធាតុអត្ថបទដែលមានស្ថិរភាពទៅកាន់ស្ថានភាពបាត។

| ប៉ារ៉ាម៉ែត្រ | ប្រភេទ | ការពិពណ៌នា |
|-----------|------|-------------|
| `id` | `string` | ID យ៉ាងឯកត្តសាសន៍សម្រាប់ធាតុស្ថានភាពនេះ |
| `text` | `string` | អត្ថបទដែលត្រូវបង្ហាញ |
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

### `ctx.settings` — ការផ្ទុកដែលមានស្ថិរភាព

ការកំណត់ Plugin ត��រូវបានផ្ទុកយ៉ាងថេរនៅក្នុង `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`។

#### `settings.get(key)`

អានតម្លៃដែលបានរក្សាទុក។
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
ត្រឡប់ `undefined` ប្រសិនបើកូនសោមិនមាន។

#### `settings.set(key, value)`

រក្សាទុកតម្លៃ។ គាំទ្រដល់អត្ថបទ, លេខ, boolean, អារ៉េ, និងវត្ថុ។
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**ឧទាហរណ៍: ចងចាំការជ្រើសរើសរបស់អ្នកប្រើ**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — ការបញ្ចូល AI

> **ស្ថានភាព: កំពុងមកដល់** — API AI ត្រូវបានកំណត់ ប៉ុន្តែមិនទាន់ភ្ជាប់ទៅ Soomy ទេ។ បច្ចុប្បន្នត្រឡប់ `{ response: 'AI not yet connected' }`។ ការបញ្ចូល AI ពេញលេញគឺមានគម្រោងសម្រាប់ការចេញផ្សាយនៅពេលអនាគត។

#### `ai.chat(messages, options?)`

ផ្ញើសារទៅកាន់ជំនួយក្រុម AI (Soomy)។
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

## ផ្នែក 3: ការកសាង UI ផ្ទាល់ខ្លួនជាមួយ Webviews

API `openWebview()` អនុញ្ញាតឱ្យអ្នកកសាង UI ផ្ទាំងគ្រប់គ្រងជាមួយ HTML, CSS, និង JavaScript — ទាំងអស់នៅក្នុងបង្អួច popup។

> **កំណត់សំខាន់:** Webviews គឺជាការបង្ហាញតែប៉ុណ្ណោះ។ ពួកវាមិនអាចហៅត្រឡប់ទៅ API plugin ទេ (`ctx.settings`, `ctx.terminal`, ល។)។ ប្រើប៊ូតុងផ្នែកខាងសម្រាប់សកម្មភាពអ្នកប្រើទាំងអស់ ហើយប្រើ `openWebview()` ដើម្បីបង្ហាញស្ថានភាពបច្ចុប្បន្ន។ ប្រសិនបើអ្នកត្រូវការសមត្ថភាពអន្តរកម្ម សូមបញ្ចូលពួកវាពីប៊ូតុងផ្នែកខាង ហើយបើក webview ម្តងទៀតដើម្បីធ្វើឱ្យការបង្ហាញមើលឃើញឡើងវិញ។

### គំរូ: ការបញ្ជា Terminal → បកប្រែចេញ → បង្ហាញនៅក្នុង HTML

នេះគឺជាគំរូ plugin ដែលពេញនិយមបំផុត។ អ្នកដំណើរការបញ្ជា បកប្រែលទ្ធផល ហើយបង្ហាញវាដោយវិជ្ជមាន។
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### គំរូ: ផ្ទាំងគ្រប់គ្រងអន្តរកម្មជាមួយការអាប់ដេតស្វ័យប្រវត្តិ
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### គំរូ: បង្ហាញការកំណត់នៅក្នុង Webview

> **កំណត់:** Webviews គឺជាការបង្ហាញតែប៉ុណ្ណោះ — ពួកវាមិនអាចហៅត្រឡប់ទៅ API plugin ទេ។ ប្រើ `ctx.settings` នៅក្នុងអ្នកដោះស្រាយប៊ូតុងផ្នែកខាងរបស់អ្នកដើម្បីកែប្រែការកំណត់ ហើយប្រើ `openWebview()` ដើម្បីបង្ហាញស្ថានភាពបច្ចុប្បន្ន។
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## ផ្នែក 4: បោះពុម្ព Plugin របស់អ្នក

### ជំហាន 1: សាកល្បងនៅក្នុងមូលដ្��ាន

1. ចម្លង plugin របស់អ្នកទៅ `~/.wia-soom/plugins/{your-plugin}/`
2. ចាប់ផ្តើម WIA SOOM ម្តងទៀត
3. ធ្វើការត្រួតពិនិត្យវាដំណើរការបាន៖ ប៊ូតុងផ្នែកខាងបង្ហាញឡើង, សមត្ថភាពដំណើរការត្រឹមត្រូវ
4. សាកល្បងករណីជ្រៅ៖ តើកើតអ្វីឡើយប្រសិនបើមិនមាន terminal ត្រូវបានភ្ជាប់ទេ?

### ជំហាន 2: រៀបចំសម្រាប់ការដាក់ស្នើ

ថត plugin របស់អ្នកត្រូវតែមាន៖
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**តម្រូវការការបញ្ចូល `package.json`:**

| វាល | ការពិពណ៌នា | ឧទាហរណ៍ |
|-------|-------------|---------|
| `name` | អត្តសញ្ញាណដែលមានតែមួយក្នុងរូបរាង kebab-case | `"my-awesome-plugin"` |
| `version` | កំណែសមាសភាព | `"1.0.0"` |
| `description` | ការពិពណ៌នាដោយខ្លីមួយខ្សែ | `"Monitors nginx access logs in real-time"` |
| `author` | ឈ្មោះរបស់អ្នក | `"John Doe"` |
| `main` | ចំណុចចូល | `"index.js"` |

**វាលដែលមិនចាំបាច់:**

| វាល | ការពិពណ៌នា |
|-------|-------------|
| `license` | ប្រភេទអាជ្ញាប័ណ្ណ (MIT គឺត្រូវបានណែនាំ) |
| `keywords` | បញ្ជីនៃស្លាកស្វែងរក |
| `soom.minVersion` | កំណែ WIA SOOM ដែលត្រូវការក��នុងកម្រិតអប្បបរមា |

### ជំហានទី ៣: បញ្ចូនទៅកាន់ Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **បន្ថែម** plugin របស់អ្នកទៅ `plugins/{your-plugin-name}/`
3. **បញ្ចូន** Pull Request

### ជំហានទី ៤: ការពិនិត្យ និងការអនុម័ត

យើងពិនិត្យមើល plugin រាល់មួយសម្រាប់:

- **សុវត្ថិភាព** — មិនមាន API គ្រោះថ្នាក់ (មើល [ច្បាប់សុវត្ថិភាព](#security-rules))
- **គុណភាព** — វាដំណើរការបានទេ? កូដស្អាតទេ?
- **ប្រយោជន៍** — វាដោះសោបញ្ហាពិតមួយទេ?

បន្ទាប់ពីការអនុម័ត:
1. Plugin របស់អ្នកត្រូវបានបន្ថែមទៅ `registry.json`
2. កញ្ចប់ ZIP ត្រូវបានបង្កើតនៅក្នុង `dist/`
3. Plugin របស់អ្នកបង្ហាញនៅក្នុង **Plugin Store** សម្រាប់អ្នកប្រើ WIA SOOM ទាំងអស់!

---

## ផ្នែកទី ៥: អនុស្សាវរីយ៍ល្អ

### ច្បាប់សុវត្ថិភាព

ច្បាប់ទាំងនេះគឺ **ចាំបាច់**។ Plugin ដែលលើកលែងពួកវានឹងត្រូវបានបដិសេធ។

| ច្បាប់ | មូលហេតុ |
|------|-----|
| **កុំ** ប្រើ `eval()` ឬ `new Function()` | គ្រោះថ្នាក់នៃការបញ្ចូលកូដ |
| **កុំ** ប្រើ `child_process`, `exec()`, `spawn()` | ប្រើតែ `ctx.terminal.send()` សម្រាប់ការបញ្ជា |
| **កុំ** ទាញយក URL ខាងក្រៅ | ករណីលើកលែង: ចំណុច API `wiasoom.com` |
| **កុំ** ចូលដំណើរការ `process.env` | អថេរ​បរិយាកាសអាចមានអាថ៌កំបាំង |
| **កុំ** ប្រើ `require('fs')` ដោយផ្ទាល់ | ប្រើ `ctx.settings` សម្រាប់ការផ្ទុក, `ctx.sftp` សម្រាប់ការផ្ទេរឯកសារ |
| **កុំ** ប្រើកញ្ចប់ខាងក្រៅ npm | JavaScript ដោយសុទ្��តែ — មិនមាន node_modules |
| **ត្រូវ** ប្រើ `ctx.terminal.send()` សម្រាប់ការបញ្ជាទាំងអស់ | នេះត្រូវតាមរយៈឆានែល SSH សុវត្ថិភាព |
| **ត្រូវ** សម្អាតនៅក្នុង `deactivate()` | លុបអ្នកស្តាប់, សម្អាតអ៊ិនធើវ៉ល |

### ការដោះស្រាយកំហុស

តែងតែបង្រ្កាបប្រតិបត្តិការដែលមានគ្រោះថ្នាក់ក្នុង try/catch:
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
### ការសម្អាតនៅក្នុង deactivate()

ប្រសិនបើ plugin របស់អ្នកបង្កើតអ៊ិនធើវ៉ល, អ្នកស្តាប់, ឬការជាវ — សម្អាតពួកវា:
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
### ការគាំទ្រ i18n

WIA SOOM គាំទ្រភាសា 254 ភាសា។ ដើម្បីធ្វើឱ្យស្លាក plugin របស់អ្នកអាចបកប្រែបាន, ប្រើវិធីសាស្ត្រងាយស្រួល:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## ផ្នែកទី ៦: ���ទាហរណ៍ក្នុងពិភពលោក

### ឧទាហរណ៍ទី ១: កម្មវិធីពិនិត្យឌីសកុំព្យូទ័រ

ដំណើរការ `df -h` នៅលើម៉ាស៊ីនមេឆ្ងាយ និងបង្ហាញកន្លែងដែលបានប្រើ/អាចប្រើនៅក្នុងបន្ទាត់ស្ថានភាព។
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### ឧទាហរណ៍ទី ២: អ្នកគ្រប់គ្រង TODO

Plugin ដែលគ្រប់គ្រងបញ្ជី TODO ប្រើការកំណត់សម្រាប់ការផ្ទុកយូរអង្វែង និង webview សម្រាប់បង្ហាញ។

> **គំរូរចនាផលិតកម្ម:** ពីព្រោះ webviews មិនអាចហៅ API plugin ដោយផ្ទាល់បានទេ, plugin នេះប្រើវិធីសាស្ត្រដែលមាន "snapshot" — វាអាន TODO ពីការកំណត់, បង្ហាញពួកវាជា HTML ដែលអាចអានបានតែ, និងផ្តល់សកម្មភាពផ្អែកលើផ្ទាំងខាងសម្រាប់បន្ថែមធាតុ។ Webview គឺជាស្រទាប់ **បង្ហាញ**, មិនមែនជារូបភាពអន្តរកម្មទេ។
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### ឧទាហរណ៍ទី ៣: អ្នកតាមដានកំហុស

តាមដានចេញពី terminal និងផ្ញើសារជូនដំណឹងពេលដែលមានលំនាំជាក់លាក់ត្រូវបានរកឃើញ។
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## បន្ថែម: ប្រភេទ និងរូបតំណាង

### ប្រភេទ Plugin (29)

ប្រើវានៅក្នុង `package.json` `keywords` ឬនៅពេលដាក់ស្នើទៅកាន់កំណត់ត្រា៖

| ប្រភេទ | ការពិពណ៌នា |
|----------|-------------|
| `server` | ការគ្រប់គ្រងម៉ាស៊ីនមេទូទៅ |
| `devtools` | ឧបករណ៍អភិវឌ្ឍន៍ |
| `calculator` | គណនី និងការបម្លែង |
| `simulator` | ការសម្តែង |
| `game` | ល្បែងក្នុងម៉ាស៊ីនបម្រើ |
| `business` | ឧបករណ៍អាជីវកម្ម |
| `security` | សុវត្ថិភាព និងការត្រួតពិនិត្យ |
| `web` | ការគ្រប់គ្រងម៉ាស៊ីនមេវេប |
| `education` | ឧបករណ៍អប់រំ |
| `health` | ឧបករណ៍ដែលទាក់ទងនឹងសុខភាព |
| `islamic` | ឧបករណ៍អ៊ីស្លាម (ម៉ោងបួស, ល។) |
| `science` | ឧបករណ៍វិទ្យាសាស្ត្រ |
| `quantum` | ឧបករណ៍កុំព្យូទ័រ Quantum |
| `ai` | ឧបករណ៍ដែលមានកម្លាំង AI |
| `biotech` | ឧបករណ៍ជីវវិទ្យា |
| `space` | ឧបករណ៍អាកាស និងអាស្រ័យដី |
| `network` | ឧបករណ៍បណ្តាញ |
| `database` | ការគ្រប់គ្រងមូលដ្ឋានទិន្នន័យ |
| `monitoring` | ការត្រួតពិនិត្យម៉ាស៊ីនមេ |
| `devops` | DevOps និង CI/CD |
| `utility` | ឧបករណ៍ទូទៅ |
| `design` | ឧបករណ៍រចនា |
| `ecommerce` | ឧបករណ៍អេឡិចត្រូនិក |
| `automation` | ឧបករណ៍ស្វ័យប្រវត្តិការ |
| `kpop` | ឧបករណ៍ដែលទាក់ទងនឹង K-pop |
| `accessibility` | ឧបករណ៍ចូលប្រើ |
| `analytics` | ការវិភាគ និងការរាយការណ៍ |
| `wia` | ឧបករណ៍អេកូស៊ីស្តែម WIA |
| `all` | បង្ហាញនៅក្នុងប្រភេទទាំងអស់ |

### រូបតំណាងដែលបានណែនាំ (Lucide)

| ឈ្មោះរ���បតំណាង | ប្រើសម្រាប់ |
|-----------|---------|
| `server` | ការគ្រប់គ្រងម៉ាស៊ីនមេ |
| `shield` | សុវត្ថិភាព |
| `database` | មូលដ្ឋានទិន្នន័យ |
| `activity` | ការត្រួតពិនិត្យ |
| `terminal` | ឧបករណ៍ម៉ាស៊ីនបម្រើ |
| `code` | ការអភិវឌ្ឍ |
| `hard-drive` | ដ្រាយ/ស្តុក |
| `network` | បណ្តាញ |
| `lock` | អត្តសញ្ញាណ/ការអ៊ិនគ្រីប |
| `eye` | ការមើល/ការត្រួតពិនិត្យ |
| `check-square` | ភារកិច្ច/TODO |
| `layout-dashboard` | ផ្ទាំងគ្រប់គ្រង |
| `settings` | ការកំណត់ |
| `zap` | ស្វ័យប្រវត្តិការ |
| `globe` | វេប/អន្តរជាតិ |

មើលរូបតំណាងទាំងអស់ 1,500+៖ [lucide.dev/icons](https://lucide.dev/icons)

---

## ត្រូវការជំនួយទេ?

- **បញ្ហា GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **បញ្ហា Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin ឧទាហរណ៍:** [Website](https://wiasoom.com)
- **គេហទំព័រ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>បង្កើតអ្វីដែលអស្ចារ្យ។ ចែករំលែកវាជាមួយពិភពលោក។</em></p>
<p align="center"><em>— ក្រុម WIA SOOM</em></p>
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
