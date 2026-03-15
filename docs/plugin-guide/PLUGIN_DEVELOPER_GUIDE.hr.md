<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Vodič za razvoj WIA SOOM dodataka</h1>
<p align="center"><strong>Izradite svoj vlastiti dodatak za 5 minuta.</strong></p>
<p align="center">Kreirajte moćne alate za poslužitelje, nadzorne ploče i automatizacije — izravno unutar WIA SOOM.</p>

---

## Sadržaj

- [Dio 1: Brzi početak — Vaš prvi dodatak za 5 minuta](#dio-1-brzi-početak--vaš-prvi-dodatak-za-5-minuta)
- [Dio 2: Referenca na Plugin Context API](#dio-2-referenca-na-plugin-context-api)
  - [ctx.terminal](#ctxterminal--izvršavanje-ponuda-na-udaljenim-poslužiteljima)
  - [ctx.sftp](#ctxsftp--prijenos-datoteka)
  - [ctx.ui](#ctxui--korisničko-sučelje)
  - [ctx.settings](#ctxsettings--trajna-pohrana)
  - [ctx.ai](#ctxai--ai-integracija)
- [Dio 3: Izgradnja prilagođenog UI-a s Webview-ima](#dio-3-izgradnja-prilagođenog-ui-a-s-webview-ima)
- [Dio 4: Objavljivanje vašeg dodatka](#dio-4-objavljivanje-vašeg-dodatka)
- [Dio 5: Najbolje prakse](#dio-5-najbolje-prakse)
- [Dio 6: Primjeri iz stvarnog svijeta](#dio-6-primjeri-iz-stvarnog-svijeta)
- [Dodatak: Kategorije i ikone](#dodatak-kategorije-i-ikone)

---

## Dio 1: Brzi početak — Vaš prvi dodatak za 5 minuta

### Što ćete izraditi

"Dodatak 'Hello World'" koji dodaje gumb na bočnu traku. Kada se klikne, prikazuje obavijest.

### Korak 1: Kreirajte mapu dodatka
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Korak 2: Kreirajte package.json
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
**Obavezna polja:** `name`, `version`, `description`, `author`, `main`

### Korak 3: Kreirajte index.js
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
### Korak 4: Ponovno pokrenite WIA SOOM

Ponovno pokrenite aplikaciju (ili prebacite dodatak isključeno/uključeno u Postavke → Dodaci).

Trebali biste vidjeti **"Hello World"** gumb u bočnoj traci. Kliknite ga — vidjet ćete obavijest o uspjehu!

### Kako to funkcionira
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

## Dio 2: Referenca na Plugin Context API

Kada se pozove vaša `activate(context)` funkcija, `context` (ili `ctx`) pruža ove API-je:
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

### `ctx.terminal` — Izvršavanje ponuda na udaljenim poslužiteljima

#### `terminal.send(sessionId, data)`

Pošaljite naredbu (ili bilo koje podatke) aktivnoj terminal sesiji.

| Parametar | Tip | Opis |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal sesija kojoj se šalje |
| `data` | `string` | Naredba ili podaci koji se šalju |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Pretplatite se na sav izlaz iz terminal sesije. Vraća **funkciju za odjavu**.

| Parametar | Tip | Opis |
|-----------|------|-------------|
| `sessionId` | `string` | Terminal sesija koju pratite |
| `callback` | `(data: string) => void` | Poziva se s svakim dijelom izlaza |
| **Vraća** | `() => void` | Pozovite ovo da prestanete slušati |
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
**Važno:** Uvijek spremite funkciju za odjavu i pozovite je u `deactivate()` kako biste spriječili curenje memorije.

---

### `ctx.sftp` — Prijenos datoteka

> **Status: Uskoro** — SFTP API je definiran, ali još nije povezan s aplikacijskim SFTP motorom. `list()` trenutno vraća praznu niz, a `upload()`/`download()` su bez učinka. Ovo će biti potpuno implementirano u budućem izdanju. Za sada, koristite `ctx.terminal.send()` s `scp` ili `rsync` naredbama kao zaobilazno rješenje.

#### `sftp.list(sessionId, path)`

Prikažite datoteke u udaljenom direktoriju.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Prenesite datoteku s lokalnog računala na udaljeni poslužitelj.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Preuzmite datoteku s udaljenog poslužitelja na lokalno računalo.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Zaobilazno rješenje (dok SFTP API ne bude aktivan):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Korisničko sučelje

#### `ui.addSidebarButton(options)`

Dodajte gumb na bočnu traku WIA SOOM.

| Opcija | Tip | Obavezno | Opis |
|--------|------|----------|-------------|
| `id` | `string` | Ne | Jedinstveni ID (zadano je ime dodatka) |
| `icon` | `string` | Da | Naziv Lucide ikone (npr., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Da | Tekst gumba prikazan u bočnoj traci |
| `onClick` | `() => void` | Da | Funkcija koja se poziva kada se gumb klikne |
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
**Referenca ikona:** Pregledajte sve dostupne ikone na [lucide.dev/icons](https://lucide.dev/icons)

> **Napomena o kompatibilnosti:** Neki stariji dodaci koriste pozicijske argumente poput `addSidebarButton(id, icon, label, onClick)`. Službeni API koristi **objekt opcija** kao što je dokumentirano iznad. Uvijek koristite stil objekta za nove dodatke.

#### `ui.openWebview(options)`

Otvorite prozor s iskačućim sadržajem prilagođenog HTML-a. Ovo je način na koji gradite bogate UI-e.

| Opcija | Tip | Opis |
|--------|------|-------------|
| `title` | `string` | Naslov prozora |
| `html` | `string` | Cjelokupni HTML sadržaj za prikaz |
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
> Pogledajte [Dio 3](#part-3-building-custom-ui-with-webviews) za napredne uzorke webview-a.

#### `ui.showNotification(type, message)`

Prikaži obavijest.

| Parametar | Tip | Opis |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Stil obavijesti |
| `message` | `string` | Tekst koji će se prikazati |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Dodaj trajni tekstualni element na donju statusnu traku.

| Parametar | Tip | Opis |
|-----------|------|-------------|
| `id` | `string` | Jedinstveni ID za ovu statusnu stavku |
| `text` | `string` | Tekst koji će se prikazati |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Trajno pohranjivanje

Postavke dodatka trajno se pohranjuju u `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Pročitaj spremljenu vrijednost.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Vraća `undefined` ako ključ ne postoji.

#### `settings.set(key, value)`

Spremi vrijednost. Podržava stringove, brojeve, booleane, nizove i objekte.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Primjer: Zapamti korisničke postavke**
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

### `ctx.ai` — AI integracija

> **Status: Uskoro** — AI API je definiran, ali još nije povezan sa Soomy. Trenutno vraća `{ response: 'AI not yet connected' }`. Potpuna AI integracija planirana je za buduće izdanje.

#### `ai.chat(messages, options?)`

Pošalji poruke AI asistentu (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Dio 3: Izrada prilagođenog UI-a s Webview-ima

API `openWebview()` omogućava vam izradu UI-a nadzorne ploče s HTML-om, CSS-om i JavaScript-om — sve unutar prozora za iskačuće prozore.

> **Važno ograničenje:** Webview-i su **samo za prikaz**. Ne mogu se pozivati natrag u API-je dodatka (`ctx.settings`, `ctx.terminal`, itd.). Koristite gumbe na bočnoj traci za sve korisničke akcije, a `openWebview()` za prikaz trenutnog stanja. Ako trebate interaktivne značajke, aktivirajte ih iz gumba na bočnoj traci i ponovno otvorite webview za osvježavanje prikaza.

### Uzorak: Terminal Komanda → Parsiraj Izlaz → Prikaži u HTML-u

Ovo je najčešći uzorak dodatka. Izvršite komandu, parsirajte rezultat i vizualno ga prikažite.
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
### Uzorak: Interaktivna Nadzorna Ploča s Automatskim Osvježavanjem
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
### Uzorak: Prikazivanje Postavki u Webview-u

> **Napomena:** Webview-i su samo za prikaz — ne mogu se pozivati natrag u API-je dodatka. Koristite `ctx.settings` u vašim handlerima gumba na bočnoj traci za izmjenu postavki, a `openWebview()` za prikaz trenutnog stanja.
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

## Dio 4: Objavljivanje vašeg dodatka

### Korak 1: Testirajte lokalno

1. Kopirajte svoj dodatak u `~/.wia-soom/plugins/{your-plugin}/`
2. Ponovno pokrenite WIA SOOM
3. Provjerite radi li: gumb na bočnoj traci se pojavljuje, značajke ispravno rade
4. Testirajte rubne slučajeve: što se događa ako nijedan terminal nije povezan?

### Korak 2: Pripremite se za predaju

Vaša mapa dodatka mora sadržavati:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Obavezna `package.json` polja:**

| Polje | Opis | Primjer |
|-------|-------------|---------|
| `name` | Jedinstveni kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantička verzija | `"1.0.0"` |
| `description` | Opis u jednoj rečenici | `"Prati nginx pristupne logove u stvarnom vremenu"` |
| `author` | Vaše ime | `"John Doe"` |
| `main` | Ulazna točka | `"index.js"` |

**Opcionalna polja:**

| Polje | Opis |
|-------|-------------|
| `license` | Tip licence (preporučuje se MIT) |
| `keywords` | Niz oznaka za pretraživanje |
| `soom.minVersion` | Minimalna verzija WIA SOOM koja je potrebna |

### Korak 3: Predajte u Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Dodajte** svoj plugin u `plugins/{your-plugin-name}/`
3. **Predajte** Pull Request

### Korak 4: Pregled i odobrenje

Svaki plugin pregledavamo zbog:

- **Sigurnosti** — nema opasnih API-ja (vidi [Pravila sigurnosti](#security-rules))
- **Kvalitete** — radi li? Je li kod čist?
- **Koristnosti** — rješava li pravi problem?

Nakon odobrenja:
1. Vaš plugin se dodaje u `registry.json`
2. ZIP paket se stvara u `dist/`
3. Vaš plugin se pojavljuje u **Plugin Store** za sve WIA SOOM korisnike!

---

## Dio 5: Najbolje prakse

### Pravila sigurnosti

Ova pravila su **obavezna**. Pluginovi koji ih krše bit će odbijeni.

| Pravilo | Zašto |
|------|-----|
| **NIKADA** ne koristite `eval()` ili `new Function()` | Rizik od injekcije koda |
| **NIKADA** ne koristite `child_process`, `exec()`, `spawn()` | Koristite samo `ctx.terminal.send()` za naredbe |
| **NIKADA** ne dohvaćajte vanjske URL-ove | Izuzetak: `wiasoom.com` API krajnje točke |
| **NIKADA** ne pristupajte `process.env` | Varijable okruženja mogu sadržavati tajne |
| **NIKADA** ne koristite `require('fs')` izravno | Koristite `ctx.settings` za pohranu, `ctx.sftp` za prijenos datoteka |
| **NIKADA** ne koristite vanjske npm pakete | Samo čisti JavaScript — bez node_modules |
| **MORATE** koristiti `ctx.terminal.send()` za sve daljinske naredbe | Ovo ide kroz siguran SSH kanal |
| **MORATE** očistiti u `deactivate()` | Uklonite slušatelje, očistite intervale |

### Rukovanje pogreškama

Uvijek obavijajte rizične operacije u try/catch:
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
### Očistite u deactivate()

Ako vaš plugin stvara intervale, slušatelje ili pretplate — očistite ih:
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
### i18n Podrška

WIA SOOM podržava 254 jezika. Da biste svoj plugin labelu učinili prevodivom, koristite jednostavan pristup:
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

## Dio 6: Primjeri iz stvarnog svijeta

### Primjer 1: Provjera diska poslužitelja

Pokreće `df -h` na udaljenom poslužitelju i prikazuje korišteni/dostupni prostor u statusnoj traci.
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

### Primjer 2: TODO Menadžer

Plugin koji upravlja TODO popisom koristeći postavke za trajnu pohranu i webview za prikaz.

> **Dizajnerski obrazac:** Budući da webview ne može izravno pozivati plugin API-je, ovaj plugin koristi pristup "snapshot" — čita TODO-ove iz postavki, prikazuje ih kao HTML samo za čitanje i pruža akcije temeljen na bočnoj traci za dodavanje stavki. Webview je **sloj** za prikaz, a ne interaktivni obrazac.
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

### Primjer 3: Promatrač pogrešaka

Prati izlaz terminala i šalje obavijest kada se otkriju određeni obrasci.
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

## Dodatak: Kategorije & Ikone

### Kategorije Plugina (29)

Koristite ove u vašem `package.json` `keywords` ili prilikom slanja u registar:

| Kategorija | Opis |
|------------|------|
| `server` | Opća uprava poslužiteljem |
| `devtools` | Alati za razvoj |
| `calculator` | Kalkulatori i konverteri |
| `simulator` | Simulatori |
| `game` | Igračke u terminalu |
| `business` | Poslovni alati |
| `security` | Sigurnost i revizija |
| `web` | Upravljanje web poslužiteljem |
| `education` | Obrazovni alati |
| `health` | Alati vezani uz zdravlje |
| `islamic` | Islamski alati (vrijeme molitve, itd.) |
| `science` | Znanstveni alati |
| `quantum` | Alati za kvantno računalstvo |
| `ai` | Alati potpomognuti AI-jem |
| `biotech` | Biotehnološki alati |
| `space` | Alati za svemir i astronomiju |
| `network` | Alati za mrežu |
| `database` | Upravljanje bazama podataka |
| `monitoring` | Praćenje poslužitelja |
| `devops` | DevOps i CI/CD |
| `utility` | Opće korisne usluge |
| `design` | Alati za dizajn |
| `ecommerce` | Alati za e-trgovinu |
| `automation` | Alati za automatizaciju |
| `kpop` | Alati vezani uz K-pop |
| `accessibility` | Alati za pristupačnost |
| `analytics` | Analitika i izvještavanje |
| `wia` | Alati WIA ekosustava |
| `all` | Pojavljuje se u svim kategorijama |

### Preporučene Ikone (Lucide)

| Ime Ikone | Koristite za |
|-----------|--------------|
| `server` | Upravljanje poslužiteljem |
| `shield` | Sigurnost |
| `database` | Baza podataka |
| `activity` | Praćenje |
| `terminal` | Terminalski alati |
| `code` | Razvoj |
| `hard-drive` | Disk/pohrana |
| `network` | Mrežno povezivanje |
| `lock` | Autentifikacija/šifriranje |
| `eye` | Gledanje/praćenje |
| `check-square` | Zadatci/TODO |
| `layout-dashboard` | Nadzorne ploče |
| `settings` | Konfiguracija |
| `zap` | Automatizacija |
| `globe` | Web/međunarodno |

Pretražite svih 1,500+ ikona: [lucide.dev/icons](https://lucide.dev/icons)

---

## Trebate Pomoć?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemi s Pluginima:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Primjeri Plugina:** [Website](https://wiasoom.com)
- **Web stranica:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Izradite nešto nevjerojatno. Podijelite to sa svijetom.</em></p>
<p align="center"><em>— Tim WIA SOOM</em></p>