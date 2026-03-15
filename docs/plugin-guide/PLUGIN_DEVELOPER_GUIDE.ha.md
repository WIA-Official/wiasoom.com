<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Jagorar Masu Haɓaka Plugin na WIA SOOM</h1>
<p align="center"><strong>Gina plugin naka cikin mintuna 5.</strong></p>
<p align="center">Ƙirƙiri kayan aikin uwar garken masu ƙarfi, dashboards, da automations — a cikin WIA SOOM.</p>

---

## Jadawalin Abun ciki

- [Sashi na 1: Fara Gaggawa — Plugin naka na Farko cikin Mintuna 5](#sashi-na-1-fara-gaggawa--plugin-naka-na-farko-cikin-mintuna-5)
- [Sashi na 2: Plugin Context API Reference](#sashi-na-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Sashi na 3: Gina UI na Musamman tare da Webviews](#sashi-na-3-gina-ui-na-musamman-tare-da-webviews)
- [Sashi na 4: Wallafa Plugin naka](#sashi-na-4-wallafa-plugin-naka)
- [Sashi na 5: Mafi Kyawun Ayyuka](#sashi-na-5-mafi-kyawun-ayyuka)
- [Sashi na 6: Misalan Duniya na Gaskiya](#sashi-na-6-misalan-duniya-na-gaskiya)
- [Karin Bayani: Categories & Icons](#karin-bayani-categories--icons)

---

## Sashi na 1: Fara Gaggawa — Plugin naka na Farko cikin Mintuna 5

### Abin da za ku gina

Plugin "Hello World" wanda ke ƙara maɓallin zuwa gefen shafi. Lokacin da aka danna, yana nuna sanarwa.

### Mataki na 1: Ƙirƙiri babban fayil na plugin
§§§CHUNK_SEPARATOR§§§
### Mataki na 2: Ƙirƙiri package.json
§§§CHUNK_SEPARATOR§§§
**Filayen da ake buƙata:** `name`, `version`, `description`, `author`, `main`

### Mataki na 3: Ƙirƙiri index.js
§§§CHUNK_SEPARATOR§§§
### Mataki na 4: Sake kunna WIA SOOM

Sake kunna aikace-aikacen (ko canza plugin daga kan/kan a cikin Saituna → Plugins).

Ya kamata ku ga **"Hello World"** maɓallin a gefen shafi. Danna shi — za ku ga sanarwar nasara!

### Yadda yake aiki
§§§CHUNK_SEPARATOR§§§
---

## Sashi na 2: Plugin Context API Reference

Lokacin da aka kira aikin ku na `activate(context)`, `context` (ko `ctx`) yana ba da waɗannan APIs:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Gudanar da umarni a kan uwar garken nesa

#### `terminal.send(sessionId, data)`

Aika umarni (ko kowanne bayanai) zuwa zaman terminal mai aiki.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Zaman terminal da za a aika zuwa |
| `data` | `string` | Umurnin ko bayanan da za a aika |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Yi rajista don duk fitarwa daga zaman terminal. Yana dawo da **aikin cire rajista**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Zaman terminal da za a kalli |
| `callback` | `(data: string) => void` | Ana kira tare da kowanne yanki na fitarwa |
| **Returns** | `() => void` | Kira wannan don dakatar da sauraro |
§§§CHUNK_SEPARATOR§§§
**Muhimmanci:** Koyaushe adana aikin cire rajista kuma kira shi a cikin `deactivate()` don hana zubar da ƙwaƙwalwa.

---

### `ctx.sftp` — Canja wurin fayil

> **Matsayi: Zai zo nan ba da jimawa ba** — An bayyana API na SFTP amma har yanzu ba a haɗa shi da injin SFTP na aikace-aikacen ba. `list()` a halin yanzu yana dawo da jerin komai, kuma `upload()`/`download()` ba su aiki. Wannan za a kammala shi a cikin wani sakin nan gaba. A halin yanzu, yi amfani da `ctx.terminal.send()` tare da umarnin `scp` ko `rsync` a matsayin hanyar magancewa.

#### `sftp.list(sessionId, path)`

Lissafa fayiloli a cikin babban fayil na nesa.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Loda fayil daga na'urar gida zuwa uwar garken nesa.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Zazzage fayil daga uwar garken nesa zuwa na'urar gida.
§§§CHUNK_SEPARATOR§§§
**Hanyar magancewa (har sai API na SFTP ya kasance a cikin aiki):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Fuskar mai amfani

#### `ui.addSidebarButton(options)`

Ƙara maɓallin zuwa gefen shafin WIA SOOM.

| Zaɓi | Nau'in | Ana buƙata | Bayani |
|--------|------|----------|-------------|
| `id` | `string` | A'a | ID na musamman (ya zama na sunan plugin) |
| `icon` | `string` | Iya | Sunan alamar Lucide (misali, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Iya | Rubutun maɓallin da aka nuna a gefen shafi |
| `onClick` | `() => void` | Iya | Aikin da aka kira lokacin da aka danna maɓallin |
§§§CHUNK_SEPARATOR§§§
**Tushen alama:** Duba duk alamomin da ake da su a [lucide.dev/icons](https://lucide.dev/icons)

> **Lura da dacewa:** Wasu tsofaffin plugins suna amfani da hujjoji masu matsayi kamar `addSidebarButton(id, icon, label, onClick)`. API na hukuma yana amfani da **abu na zaɓi** kamar yadda aka bayyana a sama. Koyaushe yi amfani da salon abu don sabbin plugins.

#### `ui.openWebview(options)`

Buɗe taga popup tare da abun cikin HTML na musamman. Haka ne yadda za ku gina UI masu arziki.

| Zaɓi | Nau'in | Bayani |
|--------|------|-------------|
| `title` | `string` | Sunan taga |
| `html` | `string` | Cikakken abun cikin HTML da za a fitar |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Duba [Sashi na 3](#part-3-building-custom-ui-with-webviews) don tsarin webview na ci gaba.

#### `ui.showNotification(type, message)`

Nuna sanarwa ta toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Salon sanarwa |
| `message` | `string` | Rubutu don nuna |
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

Kara wani abu mai rubutu mai dorewa a ƙasan sandar matsayin.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID na musamman don wannan abu na matsayin |
| `text` | `string` | Rubutu don nuna |
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

### `ctx.settings` — Ajiya mai dorewa

Saitunan plugin ana adana su har abada a `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Karanta wani ƙima da aka adana.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Yana dawo da `undefined` idan maɓallin bai wanzu ba.

#### `settings.set(key, value)`

Ajiye wani ƙima. Yana goyon bayan strings, lambobi, booleans, arrays, da abubuwa.
§��§CHUNK_SEPARATOR§§§
**Misali: Tuna zaɓin mai amfani**
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

### `ctx.ai` — Haɗin AI

> **Matsayi: Zai Zo Nan ba da jimawa ba** — An bayyana API na AI amma har yanzu ba a haɗa shi da Soomy ba. Yana dawo da `{ response: 'AI not yet connected' }` a halin yanzu. Cikakken haɗin AI ana shirin yi a cikin wani sakin nan gaba.

#### `ai.chat(messages, options?)`

Aika saƙonni zuwa mai taimako na AI (Soomy).
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

## Sashi na 3: Gina UI na Musamman tare da Webviews

API `openWebview()` yana ba ka damar gina UI na dashboard tare da HTML, CSS, da JavaScript — duk a cikin taga popup.

> **Muhimmin iyaka:** Webviews suna **nuni kawai**. Ba za su iya kiran APIs na plugin ba (`ctx.settings`, `ctx.terminal`, da sauransu). Yi amfani da maɓallan gefen don duk ayyukan mai amfani, kuma yi amfani da `openWebview()` don nuna halin yanzu. Idan kana buƙatar fasaloli masu hulɗa, kunna su daga maɓallan gefen kuma sake buɗe webview don sabunta nuni.

### Tsarin: Umarnin Terminal → Fassarawa Fitarwa → Nuna a HTML

Wannan shine mafi yawan tsarin plugin. Kana gudanar da umarni, ka fassara sakamakon, kuma ka nuna shi a fili.
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
### Tsarin: Dashboard Mai Hulɗa tare da Sabuntawa Ta atomatik
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Tsarin: Nuna Saituna a cikin Webview

> **Lura:** Webviews suna nuni kawai — ba za su iya kiran APIs na plugin ba. Yi amfani da `ctx.settings` a cikin masu sarrafa maɓallan gefen ka don gyara saituna, kuma yi amfani da `openWebview()` don nuna halin yanzu.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
---

## Sashi na 4: Wallafa Plugin ɗinka

### Mataki na 1: Gwada a gida

1. Kwafi plugin ɗinka zuwa `~/.wia-soom/plugins/{your-plugin}/`
2. Sake kunna WIA SOOM
3. Tabbatar yana aiki: maɓallin gefen yana bayyana, fasaloli suna aiki daidai
4. Gwada yanayin gefen: me zai faru idan babu terminal da aka haɗa?

### Mataki na 2: Shirya don gabatarwa

Folder na plugin ɗinka dole ne ya ƙunshi:
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Abubuwan da ake bukata na `package.json`:**

| Filin | Bayani | Misali |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | Takaitaccen bayani | `"Monitors nginx access logs in real-time"` |
| `author` | Sunanka | `"John Doe"` |
| `main` | Babban shafi | `"index.js"` |

**Filayen zaɓi:**

| Filin | Bayani |
|-------|-------------|
| `license` | Nau'in lasisi (MIT yana da shawara) |
| `keywords` | Jerin kalmomin bincike |
| `soom.minVersion` | Mafi ƙarancin sigar WIA SOOM da ake bukata |

### Mataki na 3: Aika zuwa Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Ƙara** plugin dinka a `plugins/{your-plugin-name}/`
3. **Aika** da Pull Request

### Mataki na 4: Bita da amincewa

Muna duba kowanne plugin don:

- **Tsaro** — babu APIs masu haɗari (duba [Ka'idojin Tsaro](#security-rules))
- **Inganci** — shin yana aiki? Shin lambar tana da tsabta?
- **Amfani** — shin yana warware matsala ta gaske?

Bayan amincewa:
1. An ƙara plugin dinka a `registry.json`
2. An ƙirƙiri ZIP bundle a `dist/`
3. Plugin dinka yana bayyana a cikin **Plugin Store** ga duk masu amfani da WIA SOOM!

---

## Sashe na 5: Mafi Kyawun Hanyoyi

### Ka'idojin Tsaro

Wannan ka'idoji suna **tilas**. Plugins da suka karya su za a ƙi su.

| Ka'ida | Me yasa |
|------|-----|
| **KADA** a yi amfani da `eval()` ko `new Function()` | Hadarin shigar da lamba |
| **KADA** a yi amfani da `child_process`, `exec()`, `spawn()` | Yi amfani da `ctx.terminal.send()` kawai don umarni |
| **KADA** a kawo URLs na waje | Keta: `wiasoom.com` API endpoints |
| **KADA** a sami `process.env` | Canje-canje na yanayi na iya ƙunshe da sirri |
| **KADA** a yi amfani da `require('fs')` kai tsaye | Yi amfani da `ctx.settings` don adanawa, `ctx.sftp` don canja wurin fayil |
| **DOLI** a yi amfani da `ctx.terminal.send()` don duk umarnin nesa | Wannan yana wucewa ta hanyar tashar SSH mai tsaro |
| **DOLI** a tsabtace a cikin `deactivate()` | Cire masu sauraro, share lokuta |

### Kula da Kurakurai

Koyaushe a rufe ayyukan da ke da haɗari a cikin try/catch:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
### Tsabtacewa a cikin deactivate()

Idan plugin dinka yana ƙirƙirar lokuta, masu sauraro, ko rajista — tsabtace su:
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
### i18n Tallafi

WIA SOOM yana tallafawa harsuna 254. Don sanya alamar plugin dinka ta zama mai fassara, yi amfani da hanya mai sauƙi:
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

## Sashe na 6: Misalan Duniya

### Misali na 1: Mai Duba Disk na Uwar Garke

Yana gudanar da `df -h` a kan uwar garken nesa kuma yana nuna sararin da aka yi amfani da shi/da ake da shi a cikin sandar matsayi.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### Misali na 2: Manajan TODO

Plugin da ke sarrafa jerin TODO ta amfani da saituna don adanawa mai ɗorewa da kuma webview don nuna.

> **Tsarin zane:** Tun da webviews ba za su iya kira APIs na plugin kai tsaye ba, wannan plugin yana amfani da hanyar "snapshot" — yana karanta TODOs daga saituna, yana fassara su azaman HTML mai karatu kawai, kuma yana ba da ayyukan bisa gefen don ƙara abubuwa. Webview yana kasancewa a matsayin **nuna** layer, ba wani tsari mai hulɗa ba.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Misali na 3: Mai Kula da Kurakurai

Yana lura da fitarwa na terminal kuma yana aika sanarwa lokacin da aka gano takamaiman tsarin.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

## Ƙarin Bayani: Categories & Icons

### Rukunin Plugins (29)

Yi amfani da waɗannan a cikin `package.json` `keywords` ko lokacin da kake gabatarwa ga rajistar:

| Rukuni | Bayani |
|--------|--------|
| `server` | Gudanar da sabar gaba ɗaya |
| `devtools` | Kayan aikin ci gaba |
| `calculator` | Kalkuleta da masu canza |
| `simulator` | Masu kwaikwayo |
| `game` | Wasannin terminal |
| `business` | Kayan aikin kasuwanci |
| `security` | Tsaro da bincike |
| `web` | Gudanar da sabar yanar gizo |
| `education` | Kayan aikin ilimi |
| `health` | Kayan aikin lafiya |
| `islamic` | Kayan aikin Musulunci (lokutan sallah, da sauransu) |
| `science` | Kayan aikin kimiyya |
| `quantum` | Kayan aikin kwamfuta na quantum |
| `ai` | Kayan aikin da ke amfani da AI |
| `biotech` | Kayan aikin biotechnology |
| `space` | Kayan aikin sararin samaniya da taurari |
| `network` | Kayan aikin hanyar sadarwa |
| `database` | Gudanar da bayanai |
| `monitoring` | Kulawa da sabar |
| `devops` | DevOps da CI/CD |
| `utility` | Kayan aikin gama gari |
| `design` | Kayan aikin zane |
| `ecommerce` | Kayan aikin e-kasuwanci |
| `automation` | Kayan aikin sarrafa kansa |
| `kpop` | Kayan aikin da suka shafi K-pop |
| `accessibility` | Kayan aikin samun dama |
| `analytics` | Nazari da rahoto |
| `wia` | Kayan aikin tsarin WIA |
| `all` | Yana bayyana a dukkan rukuni |

### Alamomin da aka Ba da Shawara (Lucide)

| Sunan Alama | Amfani da |
|-------------|-----------|
| `server` | Gudanar da sabar |
| `shield` | Tsaro |
| `database` | Bayanai |
| `activity` | Kulawa |
| `terminal` | Kayan aikin terminal |
| `code` | Ci gaba |
| `hard-drive` | Disk/ajiya |
| `network` | Hanyar sadarwa |
| `lock` | Tabbatarwa/ƙirƙira |
| `eye` | Kallon/kulawa |
| `check-square` | Ayyuka/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Tsarawa |
| `zap` | Sarrafa kansa |
| `globe` | Yanar gizo/ƙasa da ƙasa |

Duba duk alamomi 1,500+: [lucide.dev/icons](https://lucide.dev/icons)

---

## Kuna Bukatar Taimako?

- **Batutuwa na GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Batutuwa na Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Misalan Plugins:** [Website](https://wiasoom.com)
- **Yanar Gizo:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Gina wani abu mai ban mamaki. Raba shi tare da duniya.</em></p>
<p align="center"><em>— Ƙungiyar WIA SOOM</em></p>
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
