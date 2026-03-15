<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Kumu Hoʻololi Plugin</h1>
<p align="center"><strong>Hoʻokumu i kāu mau plugin ma 5 mau minuke.</strong></p>
<p align="center">E hana i nā mea hana koa, nā dashboard, a me nā hana maʻalahi — i loko o WIA SOOM.</p>

---

## Ka Puke O Nā Koina

- [Part 1: Hoʻomaka Pōkole — Kou Plugin Mua Ma 5 Mau Minuke](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Hoʻokumu UI Kūʻokoʻa Me nā Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Hoʻohana i kāu Plugin](#part-4-publishing-your-plugin)
- [Part 5: Nā Hana Pono](#part-5-best-practices)
- [Part 6: Nā Hiʻohiʻona Ma Ke Ao](#part-6-real-world-examples)
- [Appendix: Nā Kumu & Nā Ikona](#appendix-categories--icons)

---

## Part 1: Hoʻomaka Pōkole — Kou Plugin Mua Ma 5 Mau Minuke

### ʻO ka mea e hoʻokumu ai ʻoe

He plugin "Aloha Honua" e hoʻokomo i kahi pihi i ke koho. Ke koho ʻia, e hōʻike ia i kahi ʻike.

### Kumu 1: E kūkulu i ka folda plugin
§§§CHUNK_SEPARATOR§§§
### Kumu 2: E kūkulu i ka package.json
§§§CHUNK_SEPARATOR§§§
**Nā ʻĀkau Pono:** `name`, `version`, `description`, `author`, `main`

### Kumu 3: E kūkulu i ka index.js
§§§CHUNK_SEPARATOR§§§
### Kumu 4: E hoʻomaka hou i WIA SOOM

E hoʻomaka hou i ka polokalamu (a i ʻole e hoʻololi i ka plugin i ka Settings → Plugins).

E ʻike ʻoe i kahi pihi **"Aloha Honua"** i ke koho. E koho iā ia — e ʻike ʻoe i kahi ʻike kūʻokoʻa!

### Pehea e hana ai
§§§CHUNK_SEPARATOR§§§
---

## Part 2: Plugin Context API Reference

Ke hoʻouna ʻia kāu `activate(context)` hana, hāʻawi ʻo `context` (a i ʻole `ctx`) i kēia mau API:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — E hoʻokomo i nā kauoha ma nā ʻāina ʻē

#### `terminal.send(sessionId, data)`

E hoʻouna i kahi kauoha (a i ʻole kekahi ʻike) i kahi session terminal active.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Ke session terminal e hoʻouna i |
| `data` | `string` | Ke kauoha a i ʻole ʻike e hoʻouna |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

E komo i nā ʻike a pau mai kahi session terminal. E hoʻihoʻi i kahi **hana hoʻokuʻu**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Ke session terminal e nānā |
| `callback` | `(data: string) => void` | Hoʻouna ʻia me kēlā me kēia ʻāpana o nā ʻike |
| **E hoʻihoʻi** | `() => void` | E hoʻouna i kēia e hoʻopaʻa i ka hoʻolohe |
§§§CHUNK_SEPARATOR§§§
**Pūerto:** E mālama i ka hana hoʻokuʻu a e hoʻouna iā ia i loko o `deactivate()` e pale i nā leakage memory.

---

### `ctx.sftp` — Hoʻouna ʻike

> **Kūlana:** E komo mai i kēia manawa — Ua hoʻomaopopo ʻia ka SFTP API akā ʻaʻole i hoʻoili i ka engine SFTP o ka polokalamu. `list()` i kēia manawa e hoʻihoʻi i kahi array ʻōlelo ʻole, a ʻo nā `upload()`/`download()` he mau no-ops. E hoʻokomo ʻia kēia i loko o kahi hoʻokuʻu ʻana i ka wā e hiki mai ana. I kēia manawa, e hoʻohana i `ctx.terminal.send()` me nā kauoha `scp` a i ʻole `rsync` e like me ka hoʻoponopono.

#### `sftp.list(sessionId, path)`

E ʻike i nā faila ma kahi directory ʻē.

§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

E hoʻouna i kahi faila mai ke aupuni kūloko i ke aupuni ʻē.

§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

E hoʻokuʻu i kahi faila mai ke aupuni ʻē i ke aupuni kūloko.

§§§CHUNK_SEPARATOR§§§
**Hoʻoponopono (a hiki i ka SFTP API e noho ana):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — ʻIkona mea hoʻohana

#### `ui.addSidebarButton(options)`

E hoʻokomo i kahi pihi i ke koho WIA SOOM.

| ʻŌlelo | ʻIke | Pono | Description |
|--------|------|----------|-------------|
| `id` | `string` | ʻAʻole | ID kū hoʻokahi (ʻo ia hoʻi i nā inoa plugin) |
| `icon` | `string` | ʻĀkau | Inoa ikona Lucide (e like me, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ʻĀkau | ʻŌlelo pihi e hōʻike ʻia i ke koho |
| `onClick` | `() => void` | ʻĀkau | Hana e hoʻouna ʻia ke koho ʻia ka pihi |
§§§CHUNK_SEPARATOR§§§
**Kākoʻo ikona:** E nānā i nā ikona a pau i loaʻa ma [lucide.dev/icons](https://lucide.dev/icons)

> **Kūlana pili:** E hoʻohana ana nā plugin kahiko i nā ʻōlelo kūlana e like me `addSidebarButton(id, icon, label, onClick)`. E hoʻohana ana ka API kūloko i kahi **ʻōlelo koho** e like me ka hōʻike ʻia ma luna. E hoʻohana mau i ke ʻano ʻōlelo no nā plugin hou.

#### `ui.openWebview(options)`

E wehe i kahi puka pop-up me nā ʻike HTML kū hoʻokahi. ʻO kēia ke ala e kūkulu ai i nā UI waiwai. 

| ʻŌlelo | ʻIke | Description |
|--------|------|-------------|
| `title` | `string` | Ka inoa o ke aniani |
| `html` | `string` | Ka ʻike HTML piha e hoʻololi |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> E nānā i [Kā ʻĀkau 3](#part-3-building-custom-ui-with-webviews) no nā ʻano webview kiʻekiʻe.

#### `ui.showNotification(type, message)`

E hōʻike i kahi ʻike toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ʻIke o ka ʻike |
| `message` | `string` | Ka ʻike e hōʻike ʻia |
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

E hoʻohui i kahi mea ʻike mau i ke alanui kūlana ma lalo.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ʻIke kū hoʻokahi no kēia mea kūlana |
| `text` | `string` | Ka ʻike e hōʻike ʻia |
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

### `ctx.settings` — Ka mālama mau

Nā koho o ka plugin e mālama ʻia ana mau i `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

E heluhelu i kahi mea i mālama ʻia.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
E hoʻihoʻi i `undefined` inā ʻaʻole loaʻa ka ki.

#### `settings.set(key, value)`

E mālama i kahi mea. E kākoʻo i nā ʻōlelo, nā helu, nā boolean, nā arrays, a me nā objects.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Kumu: E hoʻomanaʻo i nā koho o ke koho**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Hoʻohui AI

> **Kūlana: E komo mai** — Ua hoʻokumu ʻia ka API AI akā, ʻaʻole i hoʻokomo ʻia i Soomy. I kēia manawa e hoʻihoʻi i `{ response: 'AI not yet connected' }`. Ua hoʻolālā ʻia ka hoʻohui ʻana i ka AI no ka hoʻokuʻu ʻana i ka wā e hiki mai ana.

#### `ai.chat(messages, options?)`

E hoʻouna i nā leka i ke kōkua AI (Soomy).
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

## Kā ʻĀkau 3: Ke Kumu ʻana i ka UI Kū hoʻokahi me nā Webviews

Hoʻokomo ka API `openWebview()` i ka hiki iā ʻoe ke kūkulu i nā UI dashboard me HTML, CSS, a me JavaScript — i loko o kahi puka pop-up.

> **Kākoʻo koʻikoʻi:** ʻO nā webviews he **hiki ke hōʻike wale nō**. ʻAʻole hiki iā lākou ke hoʻi i nā API plugin (`ctx.settings`, `ctx.terminal`, a me nā mea ʻē aʻe). E hoʻohana i nā pihi sidebar no nā hana a nā mea hoʻohana, a e hoʻohana i `openWebview()` e hōʻike i ka nohona o kēia manawa. Inā makemake ʻoe i nā hiʻohiʻona pili, e hoʻoikaika iā lākou mai nā pihi sidebar a hoʻihoʻi i ka webview e hoʻomaikaʻi i ka hōʻike.

### ʻAno: Kumu Command → Hoʻopili i ka Pākuʻi → Hōʻike i loko o HTML

O kēia ke ʻano plugin maʻamau. E hoʻoikaika ʻoe i kahi kauoha, e hoʻopili i ka hopena, a e hōʻike i ka ʻike.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### ʻAno: Dashboard Pili me ka Hoʻomaikaʻi ʻana i ka ʻike
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### ʻAno: Hōʻike i nā koho i loko o kahi Webview

> **Kipa:** ʻO nā webviews he hōʻike wale nō — ʻaʻole hiki iā lākou ke hoʻi i nā API plugin. E hoʻohana i `ctx.settings` i nā mea hana pihi sidebar e hoʻololi i nā koho, a e hoʻohana i `openWebview()` e hōʻike i ka nohona o kēia manawa.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Kā ʻĀkau 4: Ke hoʻokuʻu ʻana i kāu Plugin

### Kela ʻĀkau 1: E hoʻāʻo i loko

1. E kope i kāu plugin i `~/.wia-soom/plugins/{your-plugin}/`
2. E hoʻomaka hou i WIA SOOM
3. E hōʻoia i ka hana: e ʻike ana ka pihi sidebar, e hana ana nā hiʻohiʻona i ke ʻano kūpono
4. E hoʻāʻo i nā ʻano kūloko: he aha ka mea e hana inā ʻaʻohe terminal e pili ana?

### Kela ʻĀkau 2: E hoʻomākaukau no ka hoʻouna ʻana

Pono i kāu mau ʻāpana plugin e loaʻa:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Nā ʻĀkau e pono ai i ka `package.json`:**

| ʻĀkau | Hoʻololi | Laʻana |
|-------|-------------|---------|
| `name` | Kēia mau inoa kūʻokoʻa i loko o ka kebab-case | `"my-awesome-plugin"` |
| `version` | Ka helu ʻōlelo kūʻokoʻa | `"1.0.0"` |
| `description` | ʻŌlelo hoʻokumu hoʻokahi | `"Monitors nginx access logs in real-time"` |
| `author` | Kou inoa | `"John Doe"` |
| `main` | Ke koho ʻana | `"index.js"` |

**Nā ʻĀkau koho:**

| ʻĀkau | Hoʻololi |
|-------|-------------|
| `license` | ʻĀkau o ka palena (ʻo MIT ka mea i manaʻo ʻia) |
| `keywords` | ʻĀkau o nā hōʻailona hoʻāʻo |
| `soom.minVersion` | Ka WIA SOOM ʻĀkau minimum e pono ai |

### Kela Kumu 3: E hoʻouna i ka Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** i kāu plugin i loko o `plugins/{your-plugin-name}/`
3. **Submit** i ka Pull Request

### Kela Kumu 4: Nā nānā a me ka ʻae

Nānā mākou i nā plugin no:

- **Ke Kipa** — ʻaʻohe mau API ʻino (e nānā i nā [Kākoʻo Kipa](#security-rules))
- **Ke Kualono** — hana anei? Maikaʻi anei ka code?
- **Ke Kōkua** — e hoʻoponopono ana anei i kekahi pilikia maoli?

Ma hope o ka ʻae:
1. E hoʻokomo ʻia kāu plugin i loko o `registry.json`
2. E hoʻokumu ʻia he ZIP bundle i loko o `dist/`
3. E ʻike ʻia kāu plugin i loko o ka **Plugin Store** no nā mea hoʻohana WIA SOOM a pau!

---

## ʻĀkau 5: Nā Hana Pono

### Nā Kipa Kipa

O kēia mau kipa he **pono**. E hoʻokuʻu ʻia nā plugin e hoʻohaumia iā lākou.

| Kipa | No ke aha |
|------|-----|
| **E ʻAʻole** e hoʻohana i `eval()` a i ʻole `new Function()` | Ke koho ʻana i ka code injection |
| **E ʻAʻole** e hoʻohana i `child_process`, `exec()`, `spawn()` | E hoʻohana wale i `ctx.terminal.send()` no nā kauoha |
| **E ʻAʻole** e kiʻi i nā URL waho | Ke kūlana: nā ʻāina API o `wiasoom.com` |
| **E ʻAʻole** e komo i `process.env` | Hiki i nā ʻāina kūlana ke loaʻa nā mea hūnā |
| **E ʻAʻole** e hoʻohana i `require('fs')` ma ke ʻano kūʻokoʻa | E hoʻohana i `ctx.settings` no ka mālama ʻana, `ctx.sftp` no ka hoʻokuʻu ʻana i nā faila |
| **E PONO** e hoʻohana i `ctx.terminal.send()` no nā kauoha ʻāina | E hele kēia ma ke ala SSH palena |
| **E PONO** e hoʻomaikaʻi i loko o `deactivate()` | E hoʻopau i nā mea hoʻolohe, e hoʻomaikaʻi i nā manawa |

### Nā Hoʻoponopono Kōmike

E mālama mau i nā hana i loaʻa i loko o ka try/catch:
§§��CHUNK_SEPARATOR§§§
### Hoʻomaikaʻi i loko o deactivate()

Inā hoʻokumu kāu plugin i nā manawa, nā mea hoʻolohe, a i ʻole nā ​​palena — e hoʻomaikaʻi iā lākou:
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
### i18n Kōkua

Hoʻokomo ka WIA SOOM i nā ʻōlelo 254. E hoʻololi i kāu plugin label e hiki ke hoʻololi, e hoʻohana i ke ala maʻalahi:
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
---

## ʻĀkau 6: Nā Laʻana Maoli

### Laʻana 1: Nā ʻĀkau Disk o ke Kumu

Hana i `df -h` ma ke kumukūʻai a hōʻike i ka hoʻohana ʻia / ka loaʻa i loko o ka bar kūlana.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### Laʻana 2: Nā Manaʻo TODO

He plugin e mālama i ka papa inoa TODO e hoʻohana i nā koho no ka mālama mau a me ke koho ʻana no ka hōʻike.

> **Ke ʻano hoʻololi:** No ka mea, ʻaʻole hiki i nā webviews ke hoʻokaʻa i nā API plugin, e hoʻohana kēia plugin i ke ʻano "snapshot" — e helu ana i nā TODO mai nā koho, e hoʻololi iā lākou i ke ʻano HTML ʻole e helu ʻia, a hāʻawi i nā hana e pili ana i nā ʻāina no ka hoʻohui ʻana i nā mea. ʻO ke koho ʻana o ka webview he **pākuʻi** lā, ʻaʻole he ʻano pili.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Laʻana 3: Nā Nānā Kōmike

Nānā i ka ʻike ʻana o ke terminal a hoʻouna i ka ʻike i ka manawa e ʻike ʻia nā ʻano kūikawā.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

## ʻĀkau: Nā Kumu & Nā Ikona

### Nā Kumu Plugin (29)

E hoʻohana i kēia mau mea i loko o kāu `package.json` `keywords` a i ʻole ke hoʻouna ʻana i ka registry:

| Kumu | Hoʻohālikelike |
|----------|-------------|
| `server` | Hoʻokele ʻāina maʻamau |
| `devtools` | Nā mea hana hoʻomohala |
| `calculator` | Nā mea helu a me nā mea hoʻololi |
| `simulator` | Nā mea hoʻālike |
| `game` | Nā pāʻani terminal |
| `business` | Nā mea hana ʻoihana |
| `security` | Ke aupuni a me nā hoʻāʻo |
| `web` | Hoʻokele ʻāina pūnaewele |
| `education` | Nā mea hana hoʻonaʻauao |
| `health` | Nā mea hana pili i ke ola |
| `islamic` | Nā mea hana Islamika (nā manawa pule, etc.) |
| `science` | Nā mea hana ʻepekema |
| `quantum` | Nā mea hana kīnā kūlana |
| `ai` | Nā mea hana i hoʻokomo ʻia e AI |
| `biotech` | Nā mea hana biotechnological |
| `space` | Nā mea hana kūlana a me nā ʻike o ka lewa |
| `network` | Nā mea hana pūnaewele |
| `database` | Hoʻokele ʻikepili |
| `monitoring` | Ke aupuni ʻana o nā ʻāina |
| `devops` | DevOps a me CI/CD |
| `utility` | Nā mea hana maʻamau |
| `design` | Nā mea hana hoʻolālā |
| `ecommerce` | Nā mea hana e-commerce |
| `automation` | Nā mea hana hoʻomaikaʻi |
| `kpop` | Nā mea hana pili i ka K-pop |
| `accessibility` | Nā mea hana kūpono |
| `analytics` | Nā ʻike a me nā hōʻike |
| `wia` | Nā mea hana o ka ʻōnaehana WIA |
| `all` | E ʻike ʻia i nā kumu a pau |

### Nā Ikona Hoʻohālikelike (Lucide)

| Inoa Ikona | E hoʻohana no |
|-----------|---------|
| `server` | Hoʻokele ʻāina |
| `shield` | Ke aupuni |
| `database` | ʻIke Pākīpika |
| `activity` | Ke aupuni ʻana |
| `terminal` | Nā mea hana terminal |
| `code` | Hoʻomohala |
| `hard-drive` | Disk/keʻena |
| `network` | Hoʻokomo pūnaewele |
| `lock` | ʻAha/hoʻokomo |
| `eye` | Ke nānā ʻana/ke aupuni ʻana |
| `check-square` | Nā hana/TODO |
| `layout-dashboard` | Nā dashboard |
| `settings` | Hoʻonohonoho |
| `zap` | Hoʻomaikaʻi |
| `globe` | ʻĀkau/international |

E nānā i nā ikona 1,500+ a pau: [lucide.dev/icons](https://lucide.dev/icons)

---

## Pono i ke Kōkua?

- **Nā Pūerto GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Nā Pūerto Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Nā Plugin Kumu:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Hoʻokumu i kekahi mea kupaianaha. E kaʻana like me ke ao.</em></p>
<p align="center"><em>— Ke Kipa WIA SOOM</em></p>
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
