<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Hagaha Horumarinta Plugin-ka WIA SOOM</h1>
<p align="center"><strong>Dhiso plugin-kaaga gaarka ah 5 daqiiqo gudaheed.</strong></p>
<p align="center">Abuur qalabyo server xoog leh, dashboards, iyo otomaatik — gudaha WIA SOOM.</p>

---

## Jadwalka Mawduuca

- [Qaybta 1: Bilow Degdeg ah — Plugin-kaaga Koowaad 5 Daqiiqo gudaheed](#qaybta-1-bilow-degdeg-ah--plugin-kaaga-koowaad-5-daqiiqo-gudaheed)
- [Qaybta 2: Tixraaca API Context Plugin](#qaybta-2-tixraaca-api-context-plugin)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Qaybta 3: Dhisida UI Custom ah oo leh Webviews](#qaybta-3-dhisida-ui-custom-ah-oo-leh-webviews)
- [Qaybta 4: Daabacaadda Plugin-kaaga](#qaybta-4-daabacaadda-plugin-kaaga)
- [Qaybta 5: Hababka Ugu Wanaagsan](#qaybta-5-hababka-ugu-wanaagsan)
- [Qaybta 6: Tusaalooyinka Dhabta ah](#qaybta-6-tusaalooyinka-dhabta-ah)
- [Lifaaq: Qaybaha & Astaamaha](#lifaaq-qaybaha--astaamaha)

---

## Qaybta 1: Bilow Degdeg ah — Plugin-kaaga Koowaad 5 Daqiiqo gudaheed

### Waxaad dhisi doontaa

Plugin "Hello World" ah oo ku daraya badhan dhinaca. Markaad gujiso, waxay muujinaysaa ogeysiis.

### Tallaabada 1: Abuur galka plugin-ka
§§§CHUNK_SEPARATOR§§§
### Tallaabada 2: Abuur package.json
§§§CHUNK_SEPARATOR§§§
**Goobaha loo baahan yahay:** `name`, `version`, `description`, `author`, `main`

### Tallaabada 3: Abuur index.js
§§§CHUNK_SEPARATOR§§§
### Tallaabada 4: Dib u bilow WIA SOOM

Dib u bilow app-ka (ama beddel plugin-ka off/on gudaha Settings → Plugins).

Waxaad arki doontaa badhan **"Hello World"** ah oo dhinaca ah. Guji — waxaad arki doontaa ogeysiis guul ah!

### Sida ay u shaqeyso
§§§CHUNK_SEPARATOR§§§
---

## Qaybta 2: Tixraaca API Context Plugin

Marka la waco shaqadaada `activate(context)`, `context` (ama `ctx`) waxay bixisaa APIs-yadan:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Ku ordo amarrada server-yada fog

#### `terminal.send(sessionId, data)`

Dir amar (ama wax kasta oo xog ah) si toos ah loogu dirayo kalfadhiga terminal-ka firfircoon.

| Qodob | Nooca | Sharaxaad |
|-------|-------|-----------|
| `sessionId` | `string` | Kalfadhiga terminal-ka ee la dirayo |
| `data` | `string` | Amar ama xog la dirayo |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Isku qor dhammaan wax soo saarka kalfadhiga terminal-ka. Waxay soo celineysaa **hawl ka noqoshada**.

| Qodob | Nooca | Sharaxaad |
|-------|-------|-----------|
| `sessionId` | `string` | Kalfadhiga terminal-ka ee la daawanayo |
| `callback` | `(data: string) => void` | Waxaa la wacaa qayb kasta oo wax soo saar ah |
| **Soo celi** | `() => void` | Wac tan si aad u joojiso dhageysiga |
§§§CHUNK_SEPARATOR§§§
**Muhiim:** Had iyo jeer kaydi hawsha ka noqoshada oo wac gudaha `deactivate()` si looga hortago xasuusta in ay buuxsanto.

---

### `ctx.sftp` — Wareejinta faylka

> **Xaaladda: Soo Socota** — API SFTP waa la qeexay laakiin weli laguma xidhin injineerka SFTP ee app-ka. `list()` hadda waxay soo celineysaa array madhan, iyo `upload()`/`download()` waa no-ops. Tani si buuxda ayaa loogu hirgelin doonaa sii deyn mustaqbalka. Hadda, isticmaal `ctx.terminal.send()` oo leh amarrada `scp` ama `rsync` sidii xal.

#### `sftp.list(sessionId, path)`

Liis garee faylasha ku jira galka fog.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Soo dir fayl ka yimid mashiinka deegaanka ilaa server-ka fog.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Soo dejiso fayl ka yimid server-ka fog ilaa mashiinka deegaanka.
§§§CHUNK_SEPARATOR§§§
**Xal (illaa API SFTP la hawlgeliyo):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Interface-ka isticmaale

#### `ui.addSidebarButton(options)`

Ku dar badhan dhinaca WIA SOOM.

| Ikhtiyaar | Nooca | Loo baahan yahay | Sharaxaad |
|-----------|-------|------------------|-----------|
| `id` | `string` | Maya | ID gaar ah (waxaa loo dejiyaa magaca plugin-ka) |
| `icon` | `string` | Haa | Magaca astaanta Lucide (tusaale, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Haa | Qoraalka badhanka ee lagu muujinayo dhinaca |
| `onClick` | `() => void` | Haa | Hawsha la wacayo marka badhanka la gujiyo |
§§§CHUNK_SEPARATOR§§§
**Tixraaca Astaanta:** Ka raadi dhammaan astaamaha la heli karo [lucide.dev/icons](https://lucide.dev/icons)

> **Ogeysiis ku saabsan waafaqsanaanta:** Qaar ka mid ah plugins-ka duugga ah waxay isticmaalaan doodaha booska sida `addSidebarButton(id, icon, label, onClick)`. API rasmi ah waxay isticmaashaa **shay ikhtiyaar** sida kor lagu sharraxay. Had iyo jeer isticmaal qaabka shayga ee plugins cusub.

#### `ui.openWebview(options)`

Fur daaqad pop-up ah oo leh nuxur HTML gaarka ah. Tani waa sida aad u dhiseyso UIs hodan ah.

| Ikhtiyaar | Nooca | Sharaxaad |
|-----------|-------|-----------|
| `title` | `string` | Cinwaanka daaqadda |
| `html` | `string` | Nuxurka HTML oo dhameystiran oo la sawirayo |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Eeg [Qaybta 3](#part-3-building-custom-ui-with-webviews) qaababka webview-ka ee horumarsan.

#### `ui.showNotification(type, message)`

Muuji ogeysiis toos ah.

| Qodob | Nooca | Sharaxaad |
|-------|-------|-----------|
| `type` | `'success' \| 'error' \| 'info'` | Qaabka ogeysiiska |
| `message` | `string` | Qoraalka la muujinayo |
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

Ku dar shay qoraal ah oo joogto ah baararka hoose.

| Qodob | Nooca | Sharaxaad |
|-------|-------|-----------|
| `id` | `string` | ID gaar ah oo loogu talagalay shaygan xaaladda |
| `text` | `string` | Qoraalka la muujinayo |
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

### `ctx.settings` — Kayd joogto ah

Dejinta plugin-ka waxaa lagu kaydiyaa si joogto ah `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Akhriso qiimo la keydiyay.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Waxay soo celineysaa `undefined` haddii furaha uusan jirin.

#### `settings.set(key, value)`

Keydi qiimo. Waxay taageertaa xarfo, tirooyin, boolean, liisaska, iyo walxaha.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Tusaale: Xusuusnow doorashooyinka isticmaale**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Isku-dhafka AI

> **Xaalad: Soo Socota** — API-ga AI waa la qeexay laakiin weli laguma xirin Soomy. Hadda waxay soo celineysaa `{ response: 'AI not yet connected' }`. Isku-dhafka AI oo dhammaystiran ayaa lagu qorsheeyay sii deyn mustaqbalka.

#### `ai.chat(messages, options?)`

Dir fariimaha ka socda kaaliyaha AI (Soomy).
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

## Qaybta 3: Dhisida UI Custom ah oo leh Webviews

API-ga `openWebview()` wuxuu kuu ogolaanayaa inaad dhisto UI-yada dashboard-ka adigoo isticmaalaya HTML, CSS, iyo JavaScript — dhammaan gudaha daaqad pop-up ah.

> **Xaddidaad muhiim ah:** Webviews waa **kaliya muujin**. Ma wici karaan APIs-ka plugin-ka (`ctx.settings`, `ctx.terminal`, iwm.). Isticmaal badhamada dhinaca dhammaan ficillada isticmaale, oo isticmaal `openWebview()` si aad u muujiso xaaladda hadda. Haddii aad u baahan tahay astaamo is-dhexgal ah, ku dhaqaaji badhamada dhinaca oo dib u fur webview si aad u cusboonaysiiso muujinta.

### Qaabka: Amarka Terminal → Falanqee Natiijada → Muuji HTML

Tani waa qaabka plugin-ka ugu caansan. Waxaad orodsiisaa amar, falanqeysaa natiijada, oo waxaad muujisaa muuqaal ahaan.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Qaabka: Dashboard Is-dhexgal ah oo leh Auto-Refresh
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Qaabka: Muuji Dejinta Webview

> **Fiiro gaar ah:** Webviews waa kaliya muujin — ma wici karaan APIs-ka plugin-ka. Isticmaal `ctx.settings` gudaha gacmaha badhamada dhinacaaga si aad u beddesho dejinta, oo isticmaal `openWebview()` si aad u muujiso xaaladda hadda.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Qaybta 4: Daabacaadda Plugin-kaaga

### Tallaabada 1: Tijaabi si maxalli ah

1. Nuqul plugin-kaaga ku dhaji `~/.wia-soom/plugins/{your-plugin}/`
2. Dib u bilaaw WIA SOOM
3. Hubi inay shaqeyneyso: badhanka dhinaca ayaa muuqda, astaamaha si sax ah ayey u shaqeeyaan
4. Tijaabi xaaladaha xuduudaha: maxaa dhaca haddii aan terminal la xirin?

### Tallaabada 2: Diyaarso gudbinta

Faylkaaga plugin-ka waa inuu ka kooban yahay:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Shuruudaha `package.json` fields:**

| Field | Sharaxaad | Tusaale |
|-------|-------------|---------|
| `name` | Aqoonsi gaar ah oo kebab-case ah | `"my-awesome-plugin"` |
| `version` | Nooca semantic | `"1.0.0"` |
| `description` | Sharaxaad hal sadar ah | `"Monitors nginx access logs in real-time"` |
| `author` | Magacaaga | `"John Doe"` |
| `main` | Meesha laga galo | `"index.js"` |

**Goobaha ikhtiyaariga ah:**

| Field | Sharaxaad |
|-------|-------------|
| `license` | Nooca rukhsadda (MIT ayaa lagu talinayaa) |
| `keywords` | Array ka mid ah tags raadinta |
| `soom.minVersion` | Nooca ugu yar ee WIA SOOM ee loo baahan yahay |

### Tallaabada 3: Gudbi diiwaanka Plugin

1. ****Package** your plugin as a ZIP file
2. **Ku dar** plugin-kaaga `plugins/{your-plugin-name}/`
3. **Gudbi** Pull Request

### Tallaabada 4: Dib u eegis iyo oggolaansho

Waxaan dib u eegnaa plugin kasta oo ku saabsan:

- **Amniga** — ma jiraan APIs khatar ah (eeg [Xeerarka Amniga](#security-rules))
- **Tayada** — ma shaqeeyaa? Ma nadiif tahay koodhka?
- **Faa'iidada** — ma xallisaa dhibaato dhab ah?

Kadib oggolaanshaha:
1. Plugin-kaaga waxaa lagu daraa `registry.json`
2. Xidhmo ZIP ah ayaa lagu sameeyaa `dist/`
3. Plugin-kaaga wuxuu ka muuqdaa **Plugin Store** dhammaan isticmaaleyaasha WIA SOOM!

---

## Qaybta 5: Hababka Ugu Fiican

### Xeerarka Amniga

Xeerarkan waa **qasab**. Plugins-ka ku xadgudba waxay la kulmi doonaan diidmo.

| Xeer | Sababta |
|------|-----|
| **NEVER** isticmaal `eval()` ama `new Function()` | Khatarta gelinta koodhka |
| **NEVER** isticmaal `child_process`, `exec()`, `spawn()` | Kaliya isticmaal `ctx.terminal.send()` amarrada |
| **NEVER** soo qaado URLs dibadda | Istisnaanta: `wiasoom.com` API endpoints |
| **NEVER** galo `process.env` | Isbedelada deegaanka waxay ka koobnaan karaan sir |
| **NEVER** si toos ah u isticmaal `require('fs')` | Isticmaal `ctx.settings` kaydinta, `ctx.sftp` wareejinta faylasha |
| **NEVER** isticmaal xirmooyinka dibadda ee npm | Kaliya JavaScript nadiif ah — ma jiraan node_modules |
| **MUST** isticmaal `ctx.terminal.send()` dhammaan amarrada fog | Tani waxay martiqaadaysaa kanaalka SSH ee amniga |
| **MUST** nadiifi `deactivate()` | Ka saar dhageystayaasha, nadiifi intervals |

### Maareynta Khaladaadka

Mar walba ku duub hawlgallada khatarta ah isku day/catch:
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
### Nadiifinta deactivate()

Haddii plugin-kaaga uu abuuro intervals, dhageystayaal, ama rukhsado — nadiifi:
§§§CHUNK_SEPARATOR§§§
### Taageerada i18n

WIA SOOM waxay taageertaa 254 luqadood. Si aad u samayso summada plugin-kaaga mid la tarjumi karo, isticmaal hab fudud:
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

## Qaybta 6: Tusaalooyinka Dhabta ah

### Tusaale 1: Kormeeraha Disk Server

Waxay orodsiisaa `df -h` server-ka fog waxayna muujisaa meel la isticmaalay/taagan ee barnaamijka xaaladda.
§§§CHUNK_SEPARATOR§§§
---

### Tusaale 2: Maareeyaha TODO

Plugin maareeya liiska TODO iyadoo la adeegsanayo dejimaha kaydinta joogtada ah iyo webview si loo muujiyo.

> **Qaabka naqshadeynta:** Maadaama webviews aysan si toos ah u wicin APIs plugin-ka, plugin-kan wuxuu isticmaalaa hab "snapshot" — wuxuu akhriyaa TODOs ka socda dejimaha, wuxuu u rogaa HTML akhris oo kaliya, wuxuuna bixiyaa ficillo ku saleysan sidebar si loo daro walxaha. Webview waa lakab **muujin** ah, ma ahan foom isdhexgal ah.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### Tusaale 3: Kormeeraha Khaladaadka

Waxay kormeertaa wax soo saarka terminal-ka waxayna dirtaa ogeysiis marka qaababka gaarka ah la ogaado.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

## Qaybta: Qaybaha & Astaamaha

### Qaybaha Plugin (29)

Isticmaal kuwaas gudaha `package.json` `keywords` ama markaad soo gudbineyso diiwaanka:

| Qayb | Sharaxaad |
|------|-----------|
| `server` | Maareynta server-ka guud |
| `devtools` | Qalabka horumarinta |
| `calculator` | Xisaabiyeyaasha iyo beddelayaasha |
| `simulator` | Simulaatooyinka |
| `game` | Ciyaaraha terminal-ka |
| `business` | Qalabka ganacsiga |
| `security` | Amniga iyo kormeerka |
| `web` | Maareynta server-ka webka |
| `education` | Qalabka waxbarashada |
| `health` | Qalabka la xiriira caafimaadka |
| `islamic` | Qalabka Islaamiga (waqtiyada salaadda, iwm.) |
| `science` | Qalabka sayniska |
| `quantum` | Qalabka xisaabinta quantum-ka |
| `ai` | Qalabka awoodda AI |
| `biotech` | Qalabka biotechnology |
| `space` | Qalabka hawada iyo xiddigaha |
| `network` | Qalabka shabakadda |
| `database` | Maareynta xogta |
| `monitoring` | Kormeerka server-ka |
| `devops` | DevOps iyo CI/CD |
| `utility` | Qalabka guud |
| `design` | Qalabka naqshadeynta |
| `ecommerce` | Qalabka e-commerce |
| `automation` | Qalabka otomaatiga |
| `kpop` | Qalabka la xiriira K-pop |
| `accessibility` | Qalabka helitaanka |
| `analytics` | Falanqaynta iyo warbixinta |
| `wia` | Qalabka deegaanka WIA |
| `all` | Ka muuqda dhammaan qaybaha |

### Astaamaha La Talo Bixinayo (Lucide)

| Magaca Astaanta | Isticmaalka |
|------------------|------------|
| `server` | Maareynta server-ka |
| `shield` | Amniga |
| `database` | Xogta |
| `activity` | Kormeerka |
| `terminal` | Qalabka terminal-ka |
| `code` | Horumarinta |
| `hard-drive` | Disk/ kaydinta |
| `network` | Shabakadda |
| `lock` | Oggolaansho/qarinta |
| `eye` | Daawashada/kormeerka |
| `check-square` | Hawlaha/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Qeexitaanka |
| `zap` | Otomaatiga |
| `globe` | Web/international |

Baadh dhammaan 1,500+ astaamaha: [lucide.dev/icons](https://lucide.dev/icons)

---

## Ma U Baahan Tahay Caawimaad?

- **GitHub Arrimaha:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Arrimaha Plugin:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Tusaalooyinka Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Dhiso wax cajiib ah. La wadaag aduunka.</em></p>
<p align="center"><em>— Kooxda WIA SOOM</em></p>
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
