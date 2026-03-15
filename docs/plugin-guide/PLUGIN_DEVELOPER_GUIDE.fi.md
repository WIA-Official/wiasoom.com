<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Kehittäjän Opas</h1>
<p align="center"><strong>Rakenna oma pluginisi 5 minuutissa.</strong></p>
<p align="center">Luo tehokkaita palvelintyökaluja, hallintapaneeleja ja automaatioita — suoraan WIA SOOM:ssa.</p>

---

## Sisällysluettelo

- [Osa 1: Nopeasti alkuun — Ensimmäinen pluginisi 5 minuutissa](#osa-1-nopeasti-alkuun--ensimmäinen-pluginisi-5-minuutissa)
- [Osa 2: Pluginin konteksti API Viite](#osa-2-pluginin-konteksti-api-viite)
  - [ctx.terminal](#ctxterminal--suorita-komentoja-etäpalvelimilla)
  - [ctx.sftp](#ctxsftp--tiedostonsiirto)
  - [ctx.ui](#ctxui--käyttöliittymä)
  - [ctx.settings](#ctxsettings--kestävä-tallennus)
  - [ctx.ai](#ctxai--tekoälyintegraatio)
- [Osa 3: Mukautetun käyttöliittymän rakentaminen Webview:lla](#osa-3-mukautetun-käyttöliittymän-rakentaminen-webviewlla)
- [Osa 4: Pluginisi julkaiseminen](#osa-4-pluginisi-julkaiseminen)
- [Osa 5: Parhaat käytännöt](#osa-5-parhaat-käytännöt)
- [Osa 6: Todelliset esimerkit](#osa-6-todelliset-esimerkit)
- [Liite: Kategoriat & Ikonit](#liite-kategoriat--ikonit)

---

## Osa 1: Nopeasti alkuun — Ensimmäinen pluginisi 5 minuutissa

### Mitä rakennat

"Hello World" plugin, joka lisää painikkeen sivupalkkiin. Kun sitä klikataan, se näyttää ilmoituksen.

### Vaihe 1: Luo plugin-kansio
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Vaihe 2: Luo package.json
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
**Vaaditut kentät:** `name`, `version`, `description`, `author`, `main`

### Vaihe 3: Luo index.js
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
### Vaihe 4: Käynnistä WIA SOOM uudelleen

Käynnistä sovellus uudelleen (tai kytke plugin pois päältä/päälle Asetukset → Lisäosat).

Näet **"Hello World"** painikkeen sivupalkissa. Klikkaa sitä — näet onnistumisilmoituksen!

### Kuinka se toimii
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

## Osa 2: Pluginin konteksti API Viite

Kun `activate(context)`-funktiosi kutsutaan, `context` (tai `ctx`) tarjoaa nämä API:t:
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

### `ctx.terminal` — Suorita komentoja etäpalvelimilla

#### `terminal.send(sessionId, data)`

Lähetä komento (tai mitä tahansa dataa) aktiiviseen terminaalisessioon.

| Parametri | Tyyppi | Kuvaus |
|-----------|--------|--------|
| `sessionId` | `string` | Terminaalisessio, johon lähetetään |
| `data` | `string` | Lähetettävä komento tai data |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Tilaa kaikki tulosteet terminaalisessiosta. Palauttaa **peruutusfunktion**.

| Parametri | Tyyppi | Kuvaus |
|-----------|--------|--------|
| `sessionId` | `string` | Terminaalisessio, jota seurataan |
| `callback` | `(data: string) => void` | Kutsutaan jokaisen tuloste-erän kanssa |
| **Palauttaa** | `() => void` | Kutsu tätä lopettaaksesi kuuntelun |
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
**Tärkeää:** Tallenna aina peruutusfunktio ja kutsu sitä `deactivate()`-funktiossa estääksesi muistivuodot.

---

### `ctx.sftp` — Tiedostonsiirto

> **Tila: Tulossa pian** — SFTP API on määritelty, mutta ei vielä kytketty sovelluksen SFTP-moottoriin. `list()` palauttaa tällä hetkellä tyhjää taulukkoa, ja `upload()`/`download()` eivät tee mitään. Tämä toteutetaan täysin tulevassa julkaisussa. Tällä hetkellä käytä `ctx.terminal.send()` `scp`- tai `rsync`-komentojen kanssa kiertotienä.

#### `sftp.list(sessionId, path)`

Listaa tiedostot etähakemistossa.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Lähetä tiedosto paikalliselta koneelta etäpalvelimelle.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Lataa tiedosto etäpalvelimelta paikalliselle koneelle.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Kiertotie (kunnes SFTP API on käytössä):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Käyttöliittymä

#### `ui.addSidebarButton(options)`

Lisää painike WIA SOOM:n sivupalkkiin.

| Vaihtoehto | Tyyppi | Pakollinen | Kuvaus |
|------------|--------|------------|--------|
| `id` | `string` | Ei | Yksilöllinen ID (oletuksena pluginin nimi) |
| `icon` | `string` | Kyllä | Lucide-ikonin nimi (esim. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Kyllä | Painikkeen teksti, joka näkyy sivupalkissa |
| `onClick` | `() => void` | Kyllä | Funktio, joka kutsutaan, kun painiketta klikataan |
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
**Ikoniviite:** Selaa kaikkia saatavilla olevia ikoneita osoitteessa [lucide.dev/icons](https://lucide.dev/icons)

> **Yhteensopivuus huomautus:** Jotkut vanhemmat pluginet käyttävät paikallisia argumentteja, kuten `addSidebarButton(id, icon, label, onClick)`. Virallinen API käyttää **options-objektia** kuten yllä on dokumentoitu. Käytä aina objektityyliä uusille plugineille.

#### `ui.openWebview(options)`

Avaa ponnahdusikkuna, jossa on mukautettua HTML-sisältöä. Näin rakennat rikkaita käyttöliittymiä.

| Vaihtoehto | Tyyppi | Kuvaus |
|------------|--------|--------|
| `title` | `string` | Ikkunan otsikko |
| `html` | `string` | Täysi HTML-sisältö, joka renderöidään |
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
> Katso [Osa 3](#part-3-building-custom-ui-with-webviews) edistyneistä webview-malleista.

#### `ui.showNotification(type, message)`

Näytä toast-ilmoitus.

| Parametri | Tyyppi | Kuvaus |
|-----------|--------|--------|
| `type` | `'success' \| 'error' \| 'info'` | Ilmoituksen tyyli |
| `message` | `string` | Näytettävä teksti |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Lisää pysyvä tekstielementti alareunan tilariville.

| Parametri | Tyyppi | Kuvaus |
|-----------|--------|--------|
| `id` | `string` | Tämä tilaelementti on ainutlaatuinen ID |
| `text` | `string` | Näytettävä teksti |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Pysyvä tallennus

Lisäosan asetukset tallennetaan pysyvästi `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` -tiedostoon.

#### `settings.get(key)`

Lue tallennettu arvo.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Palauttaa `undefined`, jos avainta ei ole.

#### `settings.set(key, value)`

Tallenna arvo. Tukee merkkijonoja, numeroita, totuusarvoja, taulukoita ja objekteja.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Esimerkki: Muista käyttäjän asetukset**
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

### `ctx.ai` — AI-integraatio

> **Tila: Tulossa Pian** — AI API on määritelty, mutta ei vielä yhdistetty Soomyyn. Palauttaa tällä hetkellä `{ response: 'AI not yet connected' }`. Täysi AI-integraatio on suunniteltu tulevaan julkaisuun.

#### `ai.chat(messages, options?)`

Lähetä viestejä AI-avustajalle (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Osa 3: Mukautetun käyttöliittymän rakentaminen Webviewilla

`openWebview()` API:n avulla voit rakentaa hallintapaneelin käyttöliittymiä HTML:n, CSS:n ja JavaScriptin avulla — kaikki popup-ikkunassa.

> **Tärkeä rajoitus:** Webviewt ovat **vain näyttöä varten**. Ne eivät voi kutsua takaisin lisäosan API:hin (`ctx.settings`, `ctx.terminal`, jne.). Käytä sivupalkin painikkeita kaikkiin käyttäjätoimiin ja käytä `openWebview()` nykytilan näyttämiseen. Jos tarvitset interaktiivisia ominaisuuksia, käynnistä ne sivupalkin painikkeista ja avaa webview uudelleen näyttöä päivittääksesi.

### Malli: Terminalkomento → Tuloksen jäsentäminen → Näytä HTML:ssä

Tämä on yleisin lisäosamalli. Suoritat komennon, jäsentät tuloksen ja näytät sen visuaalisesti.
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
### Malli: Interaktiivinen hallintapaneeli automaattisella päivityksellä
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
### Malli: Asetusten näyttäminen Webviewissa

> **Huom:** Webviewt ovat vain näyttöä varten — ne eivät voi kutsua takaisin lisäosan API:hin. Käytä `ctx.settings` sivupalkin painikkeiden käsittelijöissä asetusten muokkaamiseen ja käytä `openWebview()` nykytilan näyttämiseen.
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

## Osa 4: Lisäosan julkaiseminen

### Vaihe 1: Testaa paikallisesti

1. Kopioi lisäosasi `~/.wia-soom/plugins/{your-plugin}/`
2. Käynnistä WIA SOOM uudelleen
3. Varmista, että se toimii: sivupalkin painike ilmestyy, ominaisuudet toimivat oikein
4. Testaa äärimmäiset tapaukset: mitä tapahtuu, jos terminaalia ei ole kytketty?

### Vaihe 2: Valmistele lähettämistä varten

Lisäosakansiosi on sisällettävä:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Vaaditut `package.json` kentät:**

| Kenttä | Kuvaus | Esimerkki |
|--------|--------|-----------|
| `name` | Yksilöllinen kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semanttinen versio | `"1.0.0"` |
| `description` | Yhden lauseen kuvaus | `"Valvoo nginxin käyttölokit reaaliajassa"` |
| `author` | Nimesi | `"John Doe"` |
| `main` | Sisäänkäyntipiste | `"index.js"` |

**Valinnaiset kentät:**

| Kenttä | Kuvaus |
|--------|--------|
| `license` | Lisenssityyppi (suositellaan MIT) |
| `keywords` | Hakutunnisteiden taulukko |
| `soom.minVersion` | Vähimmäisvaatimus WIA SOOM versiolle |

### Vaihe 3: Lähetä Plugin-rekisteriin

1. ****Package** your plugin as a ZIP file
2. **Lisää** pluginisi `plugins/{your-plugin-name}/`
3. **Lähetä** Pull Request

### Vaihe 4: Tarkistus ja hyväksyntä

Tarkistamme jokaisen pluginin seuraavien perusteiden mukaan:

- **Turvallisuus** — ei vaarallisia API:ita (katso [Turvallisuussäännöt](#security-rules))
- **Laatu** — toimiiko se? Onko koodi siisti?
- **Hyödyllisyys** — ratkaiseeko se oikean ongelman?

Hyväksynnän jälkeen:
1. Pluginisi lisätään `registry.json`
2. ZIP-paketti luodaan `dist/`
3. Pluginisi näkyy **Plugin Store** -kaupassa kaikille WIA SOOM -käyttäjille!

---

## Osa 5: Parhaat käytännöt

### Turvallisuussäännöt

Nämä säännöt ovat **pakollisia**. Sääntöjä rikkovia plugineja ei hyväksytä.

| Sääntö | Miksi |
|--------|-------|
| **ÄLÄ KOSKAAN** käytä `eval()` tai `new Function()` | Koodin injektointiriski |
| **ÄLÄ KOSKAAN** käytä `child_process`, `exec()`, `spawn()` | Käytä vain `ctx.terminal.send()` komentoihin |
| **ÄLÄ KOSKAAN** hae ulkoisia URL-osoitteita | Poikkeus: `wiasoom.com` API-päätteet |
| **ÄLÄ KOSKAAN** käytä `process.env` | Ympäristömuuttujat voivat sisältää salaisuuksia |
| **ÄLÄ KOSKAAN** käytä `require('fs')` suoraan | Käytä `ctx.settings` tallennukseen, `ctx.sftp` tiedostonsiirtoon |
| **ON PAKKO** käyttää `ctx.terminal.send()` kaikille etäkomentoille | Tämä kulkee turvallisen SSH-kanavan kautta |
| **ON PAKKO** siivota `deactivate()`-funktiossa | Poista kuuntelijat, tyhjennä aikaväli |

### Virheiden käsittely

Kääri aina riskialttiit toiminnot try/catch:
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
### Siivous deactivate() -funktiossa

Jos pluginisi luo aikavälejä, kuuntelijoita tai tilauksia — siivoa ne:
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
### i18n-tuki

WIA SOOM tukee 254 kieltä. Jotta pluginisi etiketti olisi käännettävissä, käytä yksinkertaista lähestymistapaa:
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

## Osa 6: Todelliset esimerkit

### Esimerkki 1: Palvelimen levytarkistaja

Suorittaa `df -h` etäpalvelimella ja näyttää käytetyn/saavutettavan tilan tilarivillä.
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

### Esimerkki 2: TODO-hallinta

Plugin, joka hallitsee TODO-listaa käyttäen asetuksia pysyvään tallennukseen ja webview'ta näyttöön.

> **Suunnittelumalli:** Koska webview't eivät voi suoraan kutsua plugin API:ita, tämä plugin käyttää "snapshot"-lähestymistapaa — se lukee TODO:t asetuksista, renderöi ne vain luku -HTML:ksi ja tarjoaa sivupalkkipohjaisia toimintoja kohteiden lisäämiseksi. Webview on **näyttö**kerros, ei interaktiivinen lomake.
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

### Esimerkki 3: Virhevalvoja

Valvoo terminaalin ulostuloa ja lähettää ilmoituksen, kun tiettyjä kuvioita havaitaan.
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

## Liite: Kategoriat & Ikonit

### Liitännäisten Kategoriat (29)

Käytä näitä `package.json` `keywords`-kentässä tai rekisteriin lähettäessäsi:

| Kategoria | Kuvaus |
|-----------|--------|
| `server` | Yleinen palvelimen hallinta |
| `devtools` | Kehitystyökalut |
| `calculator` | Laskimet ja muuntimet |
| `simulator` | Simulaattorit |
| `game` | Päätepelit |
| `business` | Liiketoimintatyökalut |
| `security` | Turvallisuus ja auditointi |
| `web` | Verkkopalvelimen hallinta |
| `education` | Koulutustyökalut |
| `health` | Terveyteen liittyvät työkalut |
| `islamic` | Islamilaiset työkalut (rukousajat jne.) |
| `science` | Tieteelliset työkalut |
| `quantum` | Kvanttiteknologian työkalut |
| `ai` | AI-pohjaiset työkalut |
| `biotech` | Bioteknologian työkalut |
| `space` | Avaruus- ja tähtitieteelliset työkalut |
| `network` | Verkkotyökalut |
| `database` | Tietokannan hallinta |
| `monitoring` | Palvelimen valvonta |
| `devops` | DevOps ja CI/CD |
| `utility` | Yleiset apuohjelmat |
| `design` | Suunnittelutyökalut |
| `ecommerce` | Verkkokaupan työkalut |
| `automation` | Automaatio työkalut |
| `kpop` | K-pop liittyvät työkalut |
| `accessibility` | Esteettömyystyökalut |
| `analytics` | Analytiikka ja raportointi |
| `wia` | WIA-ekosysteemin työkalut |
| `all` | Näkyy kaikissa kategorioissa |

### Suositellut Ikonit (Lucide)

| Ikonin Nimi | Käyttö |
|-------------|--------|
| `server` | Palvelimen hallinta |
| `shield` | Turvallisuus |
| `database` | Tietokanta |
| `activity` | Valvonta |
| `terminal` | Pääte työkalut |
| `code` | Kehitys |
| `hard-drive` | Levy/tallennus |
| `network` | Verkko |
| `lock` | Autentikointi/salaus |
| `eye` | Tarkkailu/valvonta |
| `check-square` | Tehtävät/TODO |
| `layout-dashboard` | Hallintapaneelit |
| `settings` | Konfiguraatio |
| `zap` | Automaatio |
| `globe` | Verkkosivut/kansainvälinen |

Selaa kaikkia 1,500+ ikonia: [lucide.dev/icons](https://lucide.dev/icons)

---

## Tarvitsetko apua?

- **GitHub-ongelmat:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Liitännäisongelmat:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Esimerkkiliitännäiset:** [Website](https://wiasoom.com)
- **Verkkosivusto:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Rakenna jotain upeaa. Jaa se maailman kanssa.</em></p>
<p align="center"><em>— WIA SOOM Tiimi</em></p>