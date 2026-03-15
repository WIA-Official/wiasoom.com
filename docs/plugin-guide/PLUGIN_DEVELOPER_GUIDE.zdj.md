<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Jenga plugin yako mwenyewe kwa dakika 5.</strong></p>
<p align="center">Unda zana za nguvu za seva, dashibodi, na automatisering — moja kwa moja ndani ya WIA SOOM.</p>

---

## Orodha ya Yaliyomo

- [Sehemu ya 1: Mwanzo wa Haraka — Plugin Yako ya Kwanza kwa Dakika 5](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Sehemu ya 2: Kumbukumbu ya API ya Muktadha wa Plugin](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Sehemu ya 3: Kujenga UI ya Kawaida na Webviews](#part-3-building-custom-ui-with-webviews)
- [Sehemu ya 4: Kuchapisha Plugin Yako](#part-4-publishing-your-plugin)
- [Sehemu ya 5: Mazoea Bora](#part-5-best-practices)
- [Sehemu ya 6: Mifano ya Uhalisia](#part-6-real-world-examples)
- [Kiambatisho: Makundi & Ikoni](#appendix-categories--icons)

---

## Sehemu ya 1: Mwanzo wa Haraka — Plugin Yako ya Kwanza kwa Dakika 5

### Unachojenga

Plugin ya "Hello World" inayoongeza kifungo kwenye sidebar. Ikibonyezwa, inaonyesha arifa.

### Hatua ya 1: Unda folda ya plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Hatua ya 2: Unda package.json
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
**Sehemu zinazohitajika:** `name`, `version`, `description`, `author`, `main`

### Hatua ya 3: Unda index.js
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
### Hatua ya 4: Anzisha upya WIA SOOM

Anzisha upya programu (au geuza plugin kuwa off/on katika Mipangilio → Plugins).

Unapaswa kuona kifungo cha **"Hello World"** kwenye sidebar. Bonyeza — utaona arifa ya mafanikio!

### Inavyofanya kazi
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

## Sehemu ya 2: Kumbukumbu ya API ya Muktadha wa Plugin

Wakati kazi yako `activate(context)` inaitwa, `context` (au `ctx`) inatoa hizi APIs:
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

### `ctx.terminal` — Kimbia amri kwenye seva za mbali

#### `terminal.send(sessionId, data)`

Tuma amri (au data yoyote) kwa kikao cha terminal kinachofanya kazi.

| Kigezo | Aina | Maelezo |
|--------|------|---------|
| `sessionId` | `string` | Kikao cha terminal ambacho unataka kutuma |
| `data` | `string` | Amri au data ya kutuma |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Jiandikishe kwa pato lote kutoka kwa kikao cha terminal. Inarudisha **kazi ya kujiondoa**.

| Kigezo | Aina | Maelezo |
|--------|------|---------|
| `sessionId` | `string` | Kikao cha terminal unachotaka kufuatilia |
| `callback` | `(data: string) => void` | Inaitwa na kila kipande cha pato |
| **Inarudisha** | `() => void` | Piga hii ili kusitisha kusikiliza |
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
**Muhimu:** Daima hifadhi kazi ya kujiondoa na uipige katika `deactivate()` ili kuzuia uvujaji wa kumbukumbu.

---

### `ctx.sftp` — Uhamisho wa faili

> **Hali: Inakuja Karibu** — API ya SFTP imefafanuliwa lakini bado haijakamilishwa kwa injini ya SFTP ya programu. `list()` kwa sasa inarudisha array tupu, na `upload()`/`download()` hazifanyi kazi. Hii itatekelezwa kikamilifu katika toleo la baadaye. Kwa sasa, tumia `ctx.terminal.send()` pamoja na amri za `scp` au `rsync` kama suluhisho.

#### `sftp.list(sessionId, path)`

Orodhesha faili katika saraka ya mbali.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Pakia faili kutoka kwa mashine ya ndani kwenda kwenye seva ya mbali.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Pata faili kutoka kwa seva ya mbali hadi kwenye mashine ya ndani.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Suluhisho (hadi API ya SFTP iwe hai):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Kiolesura cha mtumiaji

#### `ui.addSidebarButton(options)`

Ongeza kifungo kwenye sidebar ya WIA SOOM.

| Chaguo | Aina | Inahitajika | Maelezo |
|--------|------|------------|---------|
| `id` | `string` | Hapana | Kitambulisho cha kipekee (kimewekwa kuwa jina la plugin) |
| `icon` | `string` | Ndiyo | Jina la ikoni ya Lucide (mfano, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ndiyo | Maandishi ya kifungo yanayoonyeshwa kwenye sidebar |
| `onClick` | `() => void` | Ndiyo | Kazi inayoitwa wakati kifungo kinabonyezwa |
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
**Kumbukumbu ya ikoni:** Tembelea ikoni zote zinazopatikana kwenye [lucide.dev/icons](https://lucide.dev/icons)

> **Kumbuka ya ulinganifu:** Baadhi ya plugins za zamani hutumia hoja za nafasi kama `addSidebarButton(id, icon, label, onClick)`. API rasmi inatumia **kitu cha chaguo** kama ilivyoelezwa hapo juu. Daima tumia mtindo wa kitu kwa plugins mpya.

#### `ui.openWebview(options)`

Fungua dirisha la popup lenye yaliyomo ya HTML ya kawaida. Hivi ndivyo unavyounda UIs zenye utajiri.

| Chaguo | Aina | Maelezo |
|--------|------|---------|
| `title` | `string` | Kichwa cha dirisha |
| `html` | `string` | Yaliyomo kamili ya HTML ya kuonyesha |
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
> Wona [Sehemu ya 3](#part-3-building-custom-ui-with-webviews) kwa mifano ya hali ya juu ya webview.

#### `ui.showNotification(type, message)`

Onyesha arifa ya toast.

| Parameter | Type | Maelezo |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Mtindo wa arifa |
| `message` | `string` | Maandishi ya kuonyesha |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ongeza kipengee cha maandiko kisichobadilika kwenye bar ya chini ya hali.

| Parameter | Type | Maelezo |
|-----------|------|-------------|
| `id` | `string` | ID ya kipekee kwa kipengee hiki cha hali |
| `text` | `string` | Maandishi ya kuonyesha |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Hifadhi ya kudumu

Mipangilio ya Plugin inahifadhiwa daima katika `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Soma thamani iliyohifadhiwa.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Inarudisha `undefined` ikiwa funguo haipo.

#### `settings.set(key, value)`

Hifadhi thamani. Inasaidia nyuzi, nambari, booleans, orodha, na vitu.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Mfano: Kumbuka mapendeleo ya mtumiaji**
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

### `ctx.ai` — Uunganisho wa AI

> **Hali: Inakuja Karibu** — API ya AI imeainishwa lakini bado haijashikamana na Soomy. Hivi sasa inarudisha `{ response: 'AI not yet connected' }`. Uunganisho kamili wa AI unatarajiwa katika toleo la baadaye.

#### `ai.chat(messages, options?)`

Tuma ujumbe kwa msaidizi wa AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Sehemu ya 3: Kujenga UI ya Kijadi na Webviews

API ya `openWebview()` inakuwezesha kujenga UI za dashibodi kwa kutumia HTML, CSS, na JavaScript — yote ndani ya dirisha la popup.

> **Kikomo muhimu:** Webviews ni **kuonyesha tu**. Haziwezi kuita tena API za plugin (`ctx.settings`, `ctx.terminal`, nk.). Tumia vitufe vya sidebar kwa vitendo vyote vya mtumiaji, na tumia `openWebview()` kuonyesha hali ya sasa. Ikiwa unahitaji vipengele vya mwingiliano, viamsha kutoka kwa vitufe vya sidebar na fungua tena webview ili kuimarisha onyesho.

### Mfano: Amri ya Terminal → Parse Matokeo → Onyesha katika HTML

Huu ndio mfano wa kawaida wa plugin. Unatekeleza amri, unachambua matokeo, na kuonyesha kwa njia ya kuona.
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
### Mfano: Dashibodi ya Kuingiliana na Auto-Refresh
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
### Mfano: Kuonyesha Mipangilio katika Webview

> **Kumbuka:** Webviews ni za kuonyesha tu — haziwezi kuita tena API za plugin. Tumia `ctx.settings` katika wakala wa vitufe vya sidebar yako kubadilisha mipangilio, na tumia `openWebview()` kuonyesha hali ya sasa.
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

## Sehemu ya 4: Kuchapisha Plugin Yako

### Hatua ya 1: Jaribu kwa ndani

1. Nakili plugin yako kwenda `~/.wia-soom/plugins/{your-plugin}/`
2. Anzisha upya WIA SOOM
3. Hakiki inafanya kazi: kifungo cha sidebar kinatokea, vipengele vinafanya kazi ipasavyo
4. Jaribu hali za ukingo: nini kinatokea ikiwa hakuna terminal iliyounganishwa?

### Hatua ya 2: Andaa kwa ajili ya kuwasilisha

Folda yako ya plugin lazima iwe na:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Fields `package.json` zinazohitajika:**

| Uwanja | Maelezo | Mfano |
|--------|---------|-------|
| `name` | Kitambulisho cha kipekee cha kebab-case | `"my-awesome-plugin"` |
| `version` | Toleo la kisasa | `"1.0.0"` |
| `description` | Maelezo ya mstari mmoja | `"Monitors nginx access logs in real-time"` |
| `author` | Jina lako | `"John Doe"` |
| `main` | Kuingia | `"index.js"` |

**M fields ya hiari:**

| Uwanja | Maelezo |
|--------|---------|
| `license` | Aina ya leseni (MIT inapendekezwa) |
| `keywords` | Orodha ya maneno ya kutafuta |
| `soom.minVersion` | Toleo la chini la WIA SOOM linalohitajika |

### Hatua ya 3: Wasilisha kwa Usajili wa Plugin

1. ****Package** your plugin as a ZIP file
2. **Ongeza** plugin yako kwenye `plugins/{jina-la-plugin-yako}/`
3. **Wasilisha** Ombi la Pull

### Hatua ya 4: Mapitio na idhini

Tunapitia kila plugin kwa:

- **Usalama** — hakuna APIs hatari (angalia [Sheria za Usalama](#security-rules))
- **Ubora** — je, inafanya kazi? Je, msimbo ni safi?
- **Faida** — je, inatatua tatizo halisi?

Baada ya idhini:
1. Plugin yako inaongezwa kwenye `registry.json`
2. Kifurushi cha ZIP kinaundwa kwenye `dist/`
3. Plugin yako inaonekana kwenye **Plugin Store** kwa watumiaji wote wa WIA SOOM!

---

## Sehemu ya 5: Mbinu Bora

### Sheria za Usalama

Sheria hizi ni **lazima**. Plugins zinazovunja sheria hizi zitarudishwa.

| Sheria | Kwanini |
|--------|---------|
| **HATUWEZI** kutumia `eval()` au `new Function()` | Hatari ya kuingiza msimbo |
| **HATUWEZI** kutumia `child_process`, `exec()`, `spawn()` | Tumia tu `ctx.terminal.send()` kwa amri |
| **HATUWEZI** kupakua URLs za nje | Tofauti: `wiasoom.com` API endpoints |
| **HATUWEZI** kufikia `process.env` | Vigezo vya mazingira vinaweza kuwa na siri |
| **HATUWEZI** kutumia `require('fs')` moja kwa moja | Tumia `ctx.settings` kwa uhifadhi, `ctx.sftp` kwa uhamishaji wa faili |
| **HITAJI** kutumia `ctx.terminal.send()` kwa amri zote za mbali | Hii inapita kupitia njia salama ya SSH |
| **HITAJI** kusafisha katika `deactivate()` | Ondoa wasikilizaji, safisha vipindi |

### Kushughulikia Makosa

Daima funga operesheni zenye hatari katika try/catch:
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
### Kusafisha katika deactivate()

Ikiwa plugin yako inaunda vipindi, wasikilizaji, au usajili — safisha:
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
### Msaada wa i18n

WIA SOOM inasaidia lugha 254. Ili kufanya lebo ya plugin yako iweze kutafsiriwa, tumia njia rahisi:
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

## Sehemu ya 6: Mifano ya Uhalisia

### Mfano wa 1: Kichunguzi cha Disk ya Server

Inafanya kazi `df -h` kwenye server ya mbali na kuonyesha nafasi iliyotumika/iliyopatikana kwenye bar ya hali.
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

### Mfano wa 2: Meneja wa TODO

Plugin inayosimamia orodha ya TODO ikitumia mipangilio kwa uhifadhi wa kudumu na webview kwa kuonyesha.

> **Mchoro wa kubuni:** Kwa kuwa webviews haiwezi kuita moja kwa moja API za plugin, plugin hii inatumia njia ya "snapshot" — inasoma TODOs kutoka kwa mipangilio, inazionyesha kama HTML isiyoingilika, na inatoa hatua za upande kwa kuongeza vitu. Webview ni safu ya **kuonyesha**, sio fomu ya mwingiliano.
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

### Mfano wa 3: Mwangalizi wa Makosa

Inasimamia matokeo ya terminal na kutuma arifa wakati mifumo maalum inapatikana.
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
| `server` | Usimamizi wa seva kwa ujumla |
| `devtools` | Zana za maendeleo |
| `calculator` | Hesabu na converters |
| `simulator` | Simulators |
| `game` | Michezo ya terminal |
| `business` | Zana za biashara |
| `security` | Usalama na ukaguzi |
| `web` | Usimamizi wa seva za wavuti |
| `education` | Zana za elimu |
| `health` | Zana zinazohusiana na afya |
| `islamic` | Zana za Kiislamu (nyakati za sala, nk.) |
| `science` | Zana za kisayansi |
| `quantum` | Zana za kompyuta za quantum |
| `ai` | Zana zinazotumia AI |
| `biotech` | Zana za bioteknolojia |
| `space` | Zana za anga na astronomia |
| `network` | Zana za mtandao |
| `database` | Usimamizi wa hifadhidata |
| `monitoring` | Ufuatiliaji wa seva |
| `devops` | DevOps na CI/CD |
| `utility` | Zana za jumla |
| `design` | Zana za kubuni |
| `ecommerce` | Zana za biashara mtandaoni |
| `automation` | Zana za automatisering |
| `kpop` | Zana zinazohusiana na K-pop |
| `accessibility` | Zana za ufikiaji |
| `analytics` | Uchambuzi na ripoti |
| `wia` | Zana za ekosistimu ya WIA |
| `all` | Inaonekana katika makundi yote |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Usimamizi wa seva |
| `shield` | Usalama |
| `database` | Hifadhidata |
| `activity` | Ufuatiliaji |
| `terminal` | Zana za terminal |
| `code` | Maendeleo |
| `hard-drive` | Diski/hifadhi |
| `network` | Mtandao |
| `lock` | Auth/encryption |
| `eye` | Kuangalia/ufuataji |
| `check-square` | Majukumu/TODO |
| `layout-dashboard` | Dashibodi |
| `settings` | Mipangilio |
| `zap` | Automatisering |
| `globe` | Wavuti/kimataifa |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Jenga kitu cha ajabu. Shiriki na ulimwengu.</em></p>
<p align="center"><em>— Timu ya WIA SOOM</em></p>