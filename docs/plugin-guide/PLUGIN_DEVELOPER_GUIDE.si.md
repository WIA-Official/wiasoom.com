<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM ප්ලගිනය සංවර්ධක මාර්ගෝපදේශය</h1>
<p align="center"><strong>ඔබගේම ප්ලගිනයක් 5 මිනිත්තුකින් සාදන්න.</strong></p>
<p align="center">ශක්තිමත් සේවාදායක මෙවලම්, ඩෑෂ්බෝඩ්, සහ ස්වයංක්‍රීය කිරීම් සාදන්න — WIA SOOM තුළම.</p>

---

## අන්තර්ගතය

- [කොටස 1: ඉක්මන් ආරම්භය — ඔබේ පළමු ප්ලගිනය 5 මිනිත්තුකින්](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [කොටස 2: ප්ලගිනයේ සන්දර්භ API යොමු](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [කොටස 3: වෙබ්දර්ශක සමඟ අභිරුචි UI සාදනවා](#part-3-building-custom-ui-with-webviews)
- [කොටස 4: ඔබේ ප්ලගිනය ප්‍රකාශනය කිරීම](#part-4-publishing-your-plugin)
- [කොටස 5: හොඳ අත්දැකීම්](#part-5-best-practices)
- [කොටස 6: වාසියෙන් ලැබෙන උදාහරණ](#part-6-real-world-examples)
- [අමුණුව: කාණ්ඩ සහ ආකාරය](#appendix-categories--icons)

---

## කොටස 1: ඉක්මන් ආරම්භය — ඔබේ පළමු ප්ලගිනය 5 මිනිත්තුකින්

### ඔබ සාදන දේ

සයිඩ්බාර්ට බොත්තමක් එකතු කරන "Hello World" ප්ලගිනයක්. ක්ලික් කළ විට, එය නිවේදනයක් පෙන්වයි.

### පියවර 1: ප්ලගිනයේ ෆෝල්ඩරය සාදන්න
§§§CHUNK_SEPARATOR§§§
### පියවර 2: package.json සාදන්න
§§§CHUNK_SEPARATOR§§§
**අවශ්‍ය ක්ෂේත්‍ර:** `name`, `version`, `description`, `author`, `main`

### පියවර 3: index.js සාදන්න
§§§CHUNK_SEPARATOR§§§
### පියවර 4: WIA SOOM නැවත ආරම���භ කරන්න

අයදුම්පත නැවත ආරම්භ කරන්න (හෝ සැකසුම් → ප්ලගිනයන් හි ප්ලගිනය අක්‍රීය/ක්‍රීය කරන්න).

ඔබට සයිඩ්බාර්හි **"Hello World"** බොත්තමක් දැකිය යුතුය. එය ක්ලික් කරන්න — ඔබට සාර්ථක නිවේදනයක් දැකීමට ලැබේ!

### එය කෙසේ ක්‍රියා කරයි
§§§CHUNK_SEPARATOR§§§
---

## කොටස 2: ප්ලගිනයේ සන්දර්භ API යොමු

ඔබගේ `activate(context)` ක්‍රියාවලිය කැඳවූ විට, `context` (හෝ `ctx`) මෙම API ලබා දෙයි:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — දුරස්ථ සේවාදායකයන්හි විධාන ක්‍රියාත්මක කරන්න

#### `terminal.send(sessionId, data)`

ක්‍රියාත්මක වන ටර්මිනල් සැසියකට විධානයක් (හෝ ඕනෑම දත්තයක්) යවන්න.

| පරාමිතිය | වර්ගය | විස්තර |
|-----------|------|-------------|
| `sessionId` | `string` | යවන්නා වූ ටර්මිනල් සැසිය |
| `data` | `string` | යවන්නා වූ විධානය හෝ දත්ත |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

ටර්මිනල් සැසියකින් සියලුම ප්‍රතිදාන සඳහා සබැඳි වන්න. **අනතුරු ඇඟවීමේ ක්‍රියාවලිය** ලබා දෙයි.

| පරාමිතිය | වර්ගය | විස්තර |
|-----------|------|-------------|
| `sessionId` | `string` | නිරීක්ෂණය කිරීමට ටර්මිනල් සැසිය |
| `callback` | `(data: string) => void` | ප්‍රතිදානේ සෑම කොටසකටම කැඳවයි |
| **ආපසු ලබා දෙයි** | `() => void` | නිරීක්ෂණය නවතා දැමීමට මෙය කැඳවන්න |
§§§CHUNK_SEPARATOR§§§
**මහත්:** සෑම විටම අනතුරු ඇඟවීමේ ක්‍රියාවලිය සුරකින්න සහ මතකය කුඩු වීම වැළැක්වීමට `deactivate()` හි එය කැඳවන්��.

---

### `ctx.sftp` — ගොනු මාරු කිරීම

> **තත්වය: ඉක්මනින් පැමිණෙයි** — SFTP API එක නිර්දේශිත කර ඇත නමුත් යෙදුමේ SFTP එන්ජිමට සම්බන්ධ කර නැත. `list()` වර්තමානයේ හිස් අරය ආපසු ලබා දෙයි, සහ `upload()`/`download()` ක්‍රියා නොකරයි. මෙය අනාගත නිකුතුවක සම්පූර්ණයෙන් ක්‍රියාත්මක කරනු ඇත. මේ වන විට, `scp` හෝ `rsync` විධාන සමඟ `ctx.terminal.send()` භාවිතා කරන්න.

#### `sftp.list(sessionId, path)`

දුරස්ථ ඩිරෙක්ටරියක ගොනු ලැයිස්තුගත කරන්න.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

දේශීය යන්ත්‍රයකින් දුරස්ථ සේවාදායකයට ගොනුවක් උඩුගත කරන්න.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

දුරස්ථ සේවාදායකයෙන් දේශීය යන්ත්‍රයට ගොනුවක් බාගත කරන්න.
§§§CHUNK_SEPARATOR§§§
**ආකාරය (SFTP API ක්‍රියාත��මක වන තුරු):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — පරිශීලක අතුරුමුහුණත

#### `ui.addSidebarButton(options)`

WIA SOOM සයිඩ්බාර්ට බොත්තමක් එකතු කරන්න.

| විකල්පය | වර්ගය | අවශ්‍යද | විස්තර |
|--------|------|----------|-------------|
| `id` | `string` | නැත | අනන්‍ය ID (ප්ලගිනයේ නමට පෙරනිමියෙන්) |
| `icon` | `string` | ඔව් | Lucide ආකාරයේ නම (උදා: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ඔව් | සයිඩ්බාර්හි පෙන්වන බොත්තමේ පෙළ |
| `onClick` | `() => void` | ඔව් | බොත්තම ක්ලික් කළ විට කැඳවනු ලබන ක්‍රියාවලිය |
§§§CHUNK_SEPARATOR§§§
**ආකාරය යොමු:** සියලුම ලබා ගත හැකි ආකාර බලන්න [lucide.dev/icons](https://lucide.dev/icons)

> **සමාන්‍යතාව සටහන:** කිහිපයක් පරණ ප්ලගිනයන් `addSidebarButton(id, icon, label, onClick)` වැනි ස්ථාන���ක ආකාර භාවිතා කරයි. නිල API එක ඉහත විස්තර කළ **විකල්ප වස්තුව** භාවිතා කරයි. නව ප්ලගිනයන් සඳහා සෑම විටම වස්තු ආකාරය භාවිතා කරන්න.

#### `ui.openWebview(options)`

අභිරුචි HTML අන්තර්ගතයක් ඇති පොප්-අප් ජනකයක් විවෘත කරන්න. මෙය ඔබට සම්පූර්ණ UI නිර්මාණය කිරීමට උපකාරී වේ.

| විකල්පය | වර්ගය | විස්තර |
|--------|------|-------------|
| `title` | `string` | ජනකයේ මාතෘකාව |
| `html` | `string` | විවෘත කිරීමට සම්පූර්ණ HTML අන්තර්ගතය |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

Toast නිවේදනයක් පෙන්වන්න.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | නිවේදන ශෛලිය |
| `message` | `string` | පෙන්විය යුතු පෙළ |
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

පහළ තත්ත්ව පේලියට ස්ථිර පෙළ අයිතමයක් එකතු කරන්න.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | මෙම තත්ත්ව අයිතමය සඳහා අනන්‍ය ID |
| `text` | `string` | පෙන්විය යුතු පෙළ |
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

### `ctx.settings` — ස්ථිර ගබඩා

Plugin සැකසුම් ස්ථිරව `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` හි ගබඩා වේ.

#### `settings.get(key)`

සුරක්ෂිත වටිනාකමක් කියවන්න.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
කීපය නොමැතිනම් `undefined` ආපසු ලබා දේ.

#### `settings.set(key, value)`

වටිනාකමක් සුරක්ෂිත කරන්න. පෙළ, සංඛ්‍යා, බූලියන්, අරය සහ වස්තු සහය දක්වයි.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**උදාහරණය: පරිශීලක කැමැත්තන් මතක තබා ගන්න**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — AI ඒකාබද්ධ කිරීම

> **තත්වය: ඉක්මනින් පැමිණෙයි** — AI API එක නිර්දේශිත වේ නමුත් Soomy සමඟ සම්බන්ධ කර නැත. වර්තමානයේ `{ response: 'AI not yet connected' }` ආපසු ලබා දේ. සම්පූර්ණ AI ඒකාබද්ධ කිරීමක් අනාගත නිකුතුවකට සැලසුම් කර ඇත.

#### `ai.chat(messages, options?)`

AI ආධාරකයාට (Soomy) පණිවිඩ යවන්න.
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

## Part 3: Webviews සමඟ අභිරුචි UI ගොඩනැගීම

`openWebview()` API එක ඔබට HTML, CSS, සහ JavaScript සමඟ ඩෑෂ්බෝඩ් UI ගොඩනැගීමට ඉඩ දෙයි — සියල්ලක්ම පෝප්-අප් ජනේලයකින්.

> **මහත් සීමාවක්:** Webviews **පෙන්වීම සඳහා පමණි**. එමඟින් plugin API ( `ctx.settings`, `ctx.terminal`, ආදිය) ආපසු කැඳවිය නොහැක. පරිශීලක ක්‍රියාවන් සඳහා සියල්ලටම පැතිබොත්තු භාවිතා කරන්න, සහ වර්තමාන තත්ත්වය පෙන්වීමට `openWebview()` භාවිතා කරන්න. ඔබට අන්තර්ක්‍රියාකාරී විශේෂාංග අවශ්‍ය නම්, පැතිබොත්තු වලින් එම විශේෂාංග ක්‍රියාත්මක කරන්න සහ පෙන්වීම යාවත්කාලීන කිරීමට webview නැවත විවෘත කරන්න.

### රටාව: Terminal Command → Parse Output → HTML හි පෙන්වන්න

මෙය ඉතා සාමාන්‍ය plugin රටාවකි. ඔබ විධානයක් ක්‍රියාත්මක කර, ප්‍රතිඵලය විශ්ලේෂණය කර, එය දෘශ්‍යමය ලෙස පෙන්වයි.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### රටාව: ස්වයං-නවීකරණය සහිත අන්තර්ක්‍රියාකාරී ඩෑෂ්බෝඩ්
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### රටාව: Webview හි සැකසුම් පෙන්වීම

> **සටහන:** Webviews පෙන්වීම සඳහා පමණි — එමඟින් plugin API ( `ctx.settings` ) ආපසු කැඳවිය නොහැක. සැකසුම් වෙනස් කිරීමට ඔබේ පැතිබොත්තු හසුරුවන ආකාරයේ `ctx.settings` භාවිතා කරන්න, සහ වර්තමාන තත්ත්වය පෙන්වීමට `openWebview()` භාවිතා කරන්න.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Part 4: ඔබේ Plugin එක ප්‍රකාශයට පත් කිරීම

### පියවර 1: ස්ථානීයව පරීක්ෂා කරන්න

1. ඔබේ plugin එක `~/.wia-soom/plugins/{your-plugin}/` වෙත පිටපත් කරන්න.
2. WIA SOOM නැවත ආරම්භ කරන්න.
3. එය ක්‍රියාත්මක වේද යන්න තහවුරු කරන්න: පැතිබොත්තුව පෙනේ, විශේෂාංග නිවැරදිව ක්‍රියා කරයි.
4. කෙළවර කේස් පරීක්ෂා කරන්න: කිසිදු ටර්මිනල් එකක් සම්බන්ධ නොවූ විට කුමක් සිදු වේද?

### පියවර 2: ඉදිරිපත් කිරීමට සූදානම් වන්න

ඔබේ plugin ෆෝල්ඩරය අඩංගු විය යුතුය:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**අවශ්‍ය `package.json` ක්ෂේත්‍ර:**

| ක්ෂේත්‍රය | විස්තර | උදාහරණ |
|-------|-------------|---------|
| `name` | අද්විතීය kebab-case හැඳුනුම | `"my-awesome-plugin"` |
| `version` | සමාන්‍ය සංස්කරණය | `"1.0.0"` |
| `description` | එක් පේළියේ විස්තරය | `"Monitors nginx access logs in real-time"` |
| `author` | ඔබගේ නම | `"John Doe"` |
| `main` | ප්‍රධාන පිවිසුම | `"index.js"` |

**අනිවාර්ය නොවන ක්ෂේත්‍ර:**

| ක්ෂේත්‍රය | විස්තර |
|-------|-------------|
| `license` | බලපත්‍ර වර්ගය (MIT යෝජනා කරයි) |
| `keywords` | සෙවීමේ ටැග් වල පරාසය |
| `soom.minVersion` | අවශ්‍යම WIA SOOM සංස්කරණය |

### පියවර 3: ප්ලගිනය Plugin Registry එකට යොමු කරන්න

1. ****Package** your plugin as a ZIP file
2. **Add** ඔබේ ප්ලගිනය `plugins/{your-plugin-name}/` වෙත
3. **Submit** Pull Request එකක්

### පියවර 4: සමාලෝචනය සහ අනුමැතිය

අපි සෑම ප්ලගිනයක්ම සමාලෝචනය කරමු:

- **ආරක්ෂාව** — භයානක APIs නැත (පරීක්ෂා කරන්න [Security Rules](#security-rules))
- **ගුණාත්මකභාවය** — එය ක්‍රියා කරද? කේතය පිරිසිදුද?
- **ආශ්‍රිතතාව** — එය යථාර්ථ ගැටළුවක් විසඳනවාද?

අනුමැතියෙන් පසු:
1. ඔබේ ප්ලගිනය `registry.json` වෙත එකතු කරයි
2. `dist/` හි ZIP පැකේජයක් සාදයි
3. ඔබේ ප්ලගිනය **Plugin Store** එකේ සියලු WIA SOOM පරිශීලකයන්ට පෙනේ!

---

## කොටස 5: හොඳ අත්හදා බැලීම්

### ආරක්ෂක නීති

මෙම නීති **අනිවාර්ය** වේ. එම නීති උල්ලංඝනය කරන ප්ලගිනයන් ප්‍රතික්ෂේප කෙරේ.

| නීතිය | ඇයි |
|------|-----|
| **කෙලින්ම** `eval()` හෝ `new Function()` භාවිතා නොකරන්න | කේත ඇතුළත් කිරීමේ ��වදානම |
| **කෙලින්ම** `child_process`, `exec()`, `spawn()` භාවිතා නොකරන්න | විධාන සඳහා පමණක් `ctx.terminal.send()` භාවිතා කරන්න |
| **කෙලින්ම** බාහිර URLs ලබා නොගන්න | විශේෂිත: `wiasoom.com` API අවසන් ලක්ෂ්‍ය |
| **කෙලින්ම** `process.env` ප්‍රවේශය නොකරන්න | පරිසර විචල්‍ය රහස් ඇතුළත් විය හැක |
| **කෙලින්ම** `require('fs')` සෘජුව භාවිතා නොකරන්න | ගබඩා සඳහා `ctx.settings` භාවිතා කරන්න, ගොනු මාරු සඳහා `ctx.sftp` භාවිතා කරන්න |
| **කෙලින්ම** npm බාහිර පැකේජ භාවිතා නොකරන්න | පිරිසිදු JavaScript පමණි — node_modules නැත |
| **අනිවාර්ය** සියලු දුරස්ථ විධාන සඳහා `ctx.terminal.send()` භාවිතා කරන්න | මෙය ආරක්ෂිත SSH නාලිකාව හරහා යයි |
| **අනිවාර්ය** `deactivate()` හි පිරිසිදු කරන්න | සවිකරුවන් ඉවත් කරන්න, අන්තර්ගතයන් පිරිසිදු කරන්න |

### දෝෂ කළමනාකරණය

සෑම අවදානම් ක්‍රියාවලියක්ම try/catch තුළ ඇතුළත් කරන්න:
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
### deactivate() හි පිරිසිදු කිරීම

ඔබේ ප්ලගිනය අන්තර්ගතයන්, සවිකරුවන්, හෝ සභාපතිත්වයන් සාදනවා නම් — එම අංග පිරිසිදු කරන්න:
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
### i18n සහය

WIA SOOM භාෂා 254 ක්ට සහය දක්වයි. ඔබේ ප්ලගිනයේ ලේබලය පරිවර්තනය කළ හැකි කිරීමට, සරල ආකාරයක් භාවිතා කරන්න:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## කොටස 6: යථාර්ථ ලෝක උදාහරණ

### උදාහරණ 1: සේවාදායක තැටි පරීක්ෂක

දුරස්ථ සේවාදායකයේ `df -h` ක්‍රියාත්මක කරයි සහ තත්ත්ව පුවරුවේ භාවිතා කරන/ලබා ගන්නා අවකාශය පෙන්වයි.
§§§CHUNK_SEPARATOR§§§
---

### උදාහරණ 2: TODO කළමනාකරු

සැකසුම් භාවිතා කරමින් සහ පවත්නා ගබඩා සඳහා සහ වෙබ්දර්ශනය සඳහා webview එකක් භාවිතා කරමින් TODO ලැයිස්තුව කළමනාකරණය කරන ප්ලගිනයක්.

> **සැලැස්ම:** වෙබ්දර්ශන සෘජුව ප්ලගිනයේ APIs කැඳවිය නොහැක, එම නිසා මෙම ප්ලගිනය "snapshot" ආකාරය භාවිතා කරයි — එය සැකසුම් වලින් TODOs කියවයි, ඒවා කියවිය නොහැකි HTML ලෙස රෙන්ඩර් කරයි, සහ අයිතම එකතු කිරීමට පාර්ශව පදනම් ක්‍රියාකාරකම් ලබා දෙයි. වෙබ්දර්ශනය **පෙන්වීමේ** තලයක් වන අතර, අන්තර්ක්‍රියාකාරී ආකාරයක් නොවේ.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### උදාහරණ 3: දෝෂ නිරීක්ෂක

ටර්මිනල් ප්‍රතිඵල නිරීක්ෂණය කරයි සහ විශේෂිත රටාවන් හඳුනා ගන්නා විට නිවේදනයක් යවයි.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

## අමුණුව: කාණ්ඩ සහ චිත්‍ර

### ප්ලගීන් කාණ්ඩ (29)

ඔබේ `package.json` `keywords` හෝ ලියාපදිංචි කිරීමට යොමු කරන විට මෙය භාවිතා කරන්න:

| කාණ්ඩ | විස්තර |
|----------|-------------|
| `server` | සාමාන්‍ය සේවාදායක කළමනාකරණය |
| `devtools` | සංවර්ධන මෙවලම් |
| `calculator` | ගණක සහ පරිවර්තක |
| `simulator` | සමානකාරක |
| `game` | ටර්මිනල් ක්‍රීඩා |
| `business` | ව්‍යාපාරික මෙවලම් |
| `security` | ආරක්ෂාව සහ පරීක්ෂණය |
| `web` | වෙබ් සේවාදායක කළමනාකරණය |
| `education` | අධ්‍යාපනික මෙවලම් |
| `health` | සෞඛ්‍ය සම්බන්ධ මෙවලම් |
| `islamic` | ඉස්ලාමීය මෙවලම් (ආරාධනා කාල, ආදිය) |
| `science` | විද්‍යාත්මක මෙවලම් |
| `quantum` | කාන්තා පරිගණක මෙවලම් |
| `ai` | AI බලගැන්වූ මෙවලම් |
| `biotech` | ජීව විද්‍යාත්මක මෙවලම් |
| `space` | අහස සහ ජ්‍යෝතිෂ්‍ය මෙවලම් |
| `network` | ජාල මෙවලම් |
| `database` | දත්ත ගබඩා කළමනාකරණය |
| `monitoring` | සේවාදායක නිරීක්ෂණය |
| `devops` | DevOps සහ CI/CD |
| `utility` | සාමාන්‍ය උපකරණ |
| `design` | නිර්මාණ මෙවලම් |
| `ecommerce` | ඊ-වාණිජ මෙවලම් |
| `automation` | ස්වයංක්‍රීය කිරීමේ මෙවලම් |
| `kpop` | K-pop සම්බන්ධ මෙවලම් |
| `accessibility` | ප්‍රවේශය සම්බන්ධ මෙවලම් |
| `analytics` | විශ්ලේෂණ සහ වාර්තා කිරීම |
| `wia` | WIA පරිසර මෙවලම් |
| `all` | සියලු කාණ්ඩ වල පෙනේ |

### නිර්දේශිත චිත්‍ර (Lucide)

| චිත්‍ර නාමය | භාවිතය සඳහා |
|-----------|---------|
| `server` | සේවාදායක කළමනාකරණය |
| `shield` | ආරක්ෂාව |
| `database` | දත්ත ගබඩා |
| `activity` | නිරීක්ෂණය |
| `terminal` | ටර්මිනල් මෙවලම් |
| `code` | සංවර්ධනය |
| `hard-drive` | ඩිස්ක්/ගබඩාව |
| `network` | ජාලකරණය |
| `lock` | අනුමැතිය/සංකේතනය |
| `eye` | නැරඹීම/නිරීක්ෂණය |
| `check-square` | කාර්ය/TODO |
| `layout-dashboard` | ඩැෂ්බෝඩ් |
| `settings` | වින්‍යාසය |
| `zap` | ස්වයංක්‍රීය කිරීම |
| `globe` | වෙබ්/අන්තර්ජාතික |

සියලු 1,500+ චිත්‍ර බ්‍රවුස් කරන්න: [lucide.dev/icons](https://lucide.dev/icons)

---

## උදව් අවශ්‍යද?

- **GitHub ගැටළු:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ප්ලගීන් ගැටළු:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **උදාහරණ ප්ලගීන්:** [Website](https://wiasoom.com)
- **වෙබ් අඩවිය:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>අපූරු දෙයක් සාදන්න. එය ලෝකය සමඟ බෙදා ගන්න.</em></p>
<p align="center"><em>— WIA SOOM කණ්ඩායම</em></p>
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
