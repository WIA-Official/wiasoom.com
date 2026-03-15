<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Vodnik za razvijalce vtičnikov</h1>
<p align="center"><strong>Ustvarite svoj vtičnik v 5 minutah.</strong></p>
<p align="center">Ustvarite močna orodja za strežnike, nadzorne plošče in avtomatizacije — kar znotraj WIA SOOM.</p>

---

## Kazalo

- [Del 1: Hiter začetek — Vaš prvi vtičnik v 5 minutah](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Del 2: Referenca API za kontekst vtičnika](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Del 3: Gradnja prilagojene UI z Webviews](#part-3-building-custom-ui-with-webviews)
- [Del 4: Objavljanje vašega vtičnika](#part-4-publishing-your-plugin)
- [Del 5: Najboljše prakse](#part-5-best-practices)
- [Del 6: Primeri iz resničnega sveta](#part-6-real-world-examples)
- [Dodatek: Kategorije in ikone](#appendix-categories--icons)

---

## Del 1: Hiter začetek — Vaš prvi vtičnik v 5 minutah

### Kaj boste ustvarili

Vtičnik "Hello World", ki doda gumb na stranski vrstici. Ko ga kliknete, se prikaže obvestilo.

### Korak 1: Ustvarite mapo za vtičnik
§§§CHUNK_SEPARATOR§§§
### Korak 2: Ustvarite package.json
§§§CHUNK_SEPARATOR§§§
**Obvezna polja:** `name`, `version`, `description`, `author`, `main`

### Korak 3: Ustvarite index.js
§§§CHUNK_SEPARATOR§§§
### Korak 4: Ponovno zaženite WIA SOOM

Ponovno zaženite aplikacijo (ali preklopite vtičnik izklop/uklop v Nastavitvah → Vtičniki).

Videli boste **"Hello World"** gumb na stranski vrstici. Kliknite nanj — videli boste obvestilo o uspehu!

### Kako deluje
§§§CHUNK_SEPARATOR§§§
---

## Del 2: Referenca API za kontekst vtičnika

Ko je vaša funkcija `activate(context)` poklicana, `context` (ali `ctx`) nudi te API-je:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Izvajanje ukazov na oddaljenih strežnikih

#### `terminal.send(sessionId, data)`

Pošlji ukaz (ali kakršne koli podatke) v aktivno terminalno sejo.

| Parameter | Tip | Opis |
|-----------|------|-------------|
| `sessionId` | `string` | Terminalna seja, kamor pošljemo |
| `data` | `string` | Ukaz ali podatki, ki jih pošljemo |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Naročite se na vse izhode iz terminalne seje. Vrne **funkcijo za odjavo**.

| Parameter | Tip | Opis |
|-----------|------|-------------|
| `sessionId` | `string` | Terminalna seja, ki jo spremljate |
| `callback` | `(data: string) => void` | Pokliče se z vsakim delom izhoda |
| **Vrne** | `() => void` | Pokličite to, da prenehate poslušati |
§§§CHUNK_SEPARATOR§§§
**Pomembno:** Vedno shranite funkcijo za odjavo in jo pokličite v `deactivate()`, da preprečite uhajanje pomnilnika.

---

### `ctx.sftp` — Prenos datotek

> **Status: Kmalu na voljo** — SFTP API je definiran, vendar še ni povezan z aplikacijo SFTP motorjem. `list()` trenutno vrne prazno tabelo, `upload()`/`download()` pa ne delujeta. To bo v celoti implementirano v prihodnji različici. Zaenkrat uporabite `ctx.terminal.send()` z ukazi `scp` ali `rsync` kot rešitev.

#### `sftp.list(sessionId, path)`

Seznam datotek v oddaljenem imeniku.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Naložite datoteko z lokalnega računalnika na oddaljeni strežnik.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Prenesite datoteko z oddaljenega strežnika na lokalni računalnik.
§§§CHUNK_SEPARATOR§§§
**Rešitev (dokler SFTP API ni aktiven):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Uporabniški vmesnik

#### `ui.addSidebarButton(options)`

Dodajte gumb na stransko vrstico WIA SOOM.

| Možnost | Tip | Obvezno | Opis |
|--------|------|----------|-------------|
| `id` | `string` | Ne | Edinstvena ID (privzeto ime vtičnika) |
| `icon` | `string` | Da | Ime ikone Lucide (npr. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Da | Besedilo gumba, prikazano na stranski vrstici |
| `onClick` | `() => void` | Da | Funkcija, ki se pokliče, ko je gumb kliknjen |
§§§CHUNK_SEPARATOR§§§
**Referenca ikon:** Oglejte si vse razpoložljive ikone na [lucide.dev/icons](https://lucide.dev/icons)

> **Opomba o združljivosti:** Nekateri starejši vtičniki uporabljajo pozicijske argumente, kot so `addSidebarButton(id, icon, label, onClick)`. Uradni API uporablja **objekt možnosti**, kot je dokumentirano zgoraj. Vedno uporabite slog objekta za nove vtičnike.

#### `ui.openWebview(options)`

Odprite pojav window z lastno HTML vsebino. Tako zgradite bogate uporabniške vmesnike.

| Možnost | Tip | Opis |
|--------|------|-------------|
| `title` | `string` | Naslov okna |
| `html` | `string` | Celotna HTML vsebina, ki jo je treba prikazati |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Oglejte si [Del 3](#part-3-building-custom-ui-with-webviews) za napredne vzorce webview.

#### `ui.showNotification(type, message)`

Prikaže obvestilo.

| Parameter | Tip | Opis |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Slog obvestila |
| `message` | `string` | Besedilo za prikaz |
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

Dodajte trajni besedilni element na spodnjo statusno vrstico.

| Parameter | Tip | Opis |
|-----------|------|-------------|
| `id` | `string` | Edinstvena ID za ta statusni element |
| `text` | `string` | Besedilo za prikaz |
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

### `ctx.settings` — Trajna shranjevanje

Nastavitve vtičnika so trajno shranjene v `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Preberite shranjeno vrednost.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Vrne `undefined`, če ključ ne obstaja.

#### `settings.set(key, value)`

Shrani vrednost. Podpira nize, številke, booleane, tabele in objekte.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Primer: Zapomni si uporabniške nastavitve**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Integracija AI

> **Status: Prihaja kmalu** — AI API je definiran, vendar še ni povezan s Soomy. Trenutno vrne `{ response: 'AI not yet connected' }`. Polna integracija AI je načrtovana za prihodnjo izdajo.

#### `ai.chat(messages, options?)`

Pošlji sporočila AI asistentu (Soomy).
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

## Del 3: Gradnja prilagojene UI z Webviews

API `openWebview()` vam omogoča gradnjo nadzornih plošč z HTML, CSS in JavaScript — vse v pojavnem oknu.

> **Pomembna omejitev:** Webviews so **samo za prikaz**. Ne morejo klicati nazaj v API-je vtičnikov (`ctx.settings`, `ctx.terminal`, itd.). Uporabite gumbe v stranski vrstici za vse uporabniške akcije in uporabite `openWebview()`, da prikažete trenutno stanje. Če potrebujete interaktivne funkcije, jih sprožite iz gumbov v stranski vrstici in ponovno odprite webview za osvežitev prikaza.

### Vzorec: Terminalska ukaz → Parsiraj izhod → Prikaži v HTML

To je najpogostejši vzorec vtičnika. Zaženete ukaz, analizirate rezultat in ga vizualno prikažete.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Vzorec: Interaktivna nadzorna plošča z samodejnim osveževanjem
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Vzorec: Prikazovanje nastavitev v webview

> **Opomba:** Webviews so samo za prikaz — ne morejo klicati nazaj v API-je vtičnikov. Uporabite `ctx.settings` v obdelovalcih gumbov v stranski vrstici za spreminjanje nastavitev in uporabite `openWebview()`, da prikažete trenutno stanje.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Del 4: Objavljanje vašega vtičnika

### Korak 1: Testirajte lokalno

1. Kopirajte svoj vtičnik v `~/.wia-soom/plugins/{your-plugin}/`
2. Ponovno zaženite WIA SOOM
3. Preverite, ali deluje: gumb v stranski vrstici se prikaže, funkcije delujejo pravilno
4. Preizkusite robne primere: kaj se zgodi, če ni povezanega terminala?

### Korak 2: Priprava na oddajo

Vaša mapa vtičnika mora vsebovati:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Zahtevana polja `package.json`:**

| Polje | Opis | Primer |
|-------|-------------|---------|
| `name` | Edinstven ID v kebab-case | `"my-awesome-plugin"` |
| `version` | Semantična različica | `"1.0.0"` |
| `description` | Enovrstičen opis | `"Spremlja nginx dostopne dnevnike v realnem času"` |
| `author` | Vaše ime | `"John Doe"` |
| `main` | Vhodna točka | `"index.js"` |

**Neobvezna polja:**

| Polje | Opis |
|-------|-------------|
| `license` | Vrsta licence (priporočena MIT) |
| `keywords` | Množica iskalnih oznak |
| `soom.minVersion` | Minimalna zahtevana različica WIA SOOM |

### Korak 3: Oddaja v Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Dodajte** svoj plugin v `plugins/{your-plugin-name}/`
3. **Oddajte** Pull Request

### Korak 4: Pregled in odobritev

Vsak plugin pregledamo glede na:

- **Varnost** — brez nevarnih API-jev (glejte [Varnostna pravila](#security-rules))
- **Kakovost** — ali deluje? Je koda čista?
- **Uporabnost** — ali rešuje pravi problem?

Po odobritvi:
1. Vaš plugin je dodan v `registry.json`
2. ZIP paket je ustvarjen v `dist/`
3. Vaš plugin se prikaže v **Plugin Store** za vse uporabnike WIA SOOM!

---

## Del 5: Najboljše prakse

### Varnostna pravila

Ta pravila so **obvezna**. Plugins, ki jih kršijo, bodo zavrnjeni.

| Pravilo | Zakaj |
|------|-----|
| **NIKOLI** ne uporabljajte `eval()` ali `new Function()` | Tveganje za injekcijo kode |
| **NIKOLI** ne uporabljajte `child_process`, `exec()`, `spawn()` | Uporabite samo `ctx.terminal.send()` za ukaze |
| **NIKOLI** ne pridobivajte zunanjih URL-jev | Izjema: API končne točke `wiasoom.com` |
| **NIKOLI** ne dostopajte do `process.env` | Spremenljivke okolja lahko vsebujejo skrivnosti |
| **NIKOLI** ne uporabljajte `require('fs')` neposredno | Uporabite `ctx.settings` za shranjevanje, `ctx.sftp` za prenos datotek |
| **NIKOLI** ne uporabljajte zunanjih npm paketov | Izključno čisti JavaScript — brez node_modules |
| **MORATE** uporabljati `ctx.terminal.send()` za vse oddaljene ukaze | To gre skozi varen SSH kanal |
| **MORATE** očistiti v `deactivate()` | Odstranite poslušalce, počistite intervale |

### Obvladovanje napak

Vedno ovijte tvegane operacije v try/catch:
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
### Čiščenje v deactivate()

Če vaš plugin ustvarja intervale, poslušalce ali naročnine — jih očistite:
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
### i18n Podpora

WIA SOOM podpira 254 jezikov. Da bo vaš pluginov označba prevodna, uporabite preprost pristop:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## Del 6: Primeri iz resničnega sveta

### Primer 1: Preverjevalnik diska strežnika

Izvede `df -h` na oddaljenem strežniku in prikaže uporabljeni/razpoložljivi prostor v statusni vrstici.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Primer 2: Upravljalnik TODO

Plugin, ki upravlja seznam TODO z uporabo nastavitev za trajno shranjevanje in webview za prikaz.

> **Oblikovni vzorec:** Ker webview ne morejo neposredno klicati plugin API-jev, ta plugin uporablja pristop "snapshot" — bere TODO-e iz nastavitev, jih prikaže kot samo za branje HTML in ponuja dejanja na podlagi stranske vrstice za dodajanje elementov. Webview je **prikazni** sloj, ne interaktivna oblika.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### Primer 3: Opazovalec napak

Spremlja izhod terminala in pošlje obvestilo, ko so zaznani specifični vzorci.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Dodatki: Kategorije in Ikone

### Kategorije Vtičnikov (29)

Uporabite te v vašem `package.json` `keywords` ali pri oddaji v registru:

| Kategorija | Opis |
|------------|------|
| `server` | Splošno upravljanje strežnikov |
| `devtools` | Orodja za razvoj |
| `calculator` | Kalkulatorji in pretvorniki |
| `simulator` | Simulatorji |
| `game` | Igra terminalskih iger |
| `business` | Orodja za poslovanje |
| `security` | Varnost in revizija |
| `web` | Upravljanje spletnih strežnikov |
| `education` | Izobraževalna orodja |
| `health` | Orodja, povezana z zdravjem |
| `islamic` | Islamska orodja (časi molitve itd.) |
| `science` | Znanstvena orodja |
| `quantum` | Orodja za kvantno računalništvo |
| `ai` | Orodja, podprta z umetno inteligenco |
| `biotech` | Orodja za biotehnologijo |
| `space` | Orodja za vesolje in astronomijo |
| `network` | Orodja za omrežje |
| `database` | Upravljanje baz podatkov |
| `monitoring` | Nadzor strežnikov |
| `devops` | DevOps in CI/CD |
| `utility` | Splošna orodja |
| `design` | Orodja za oblikovanje |
| `ecommerce` | Orodja za e-trgovino |
| `automation` | Orodja za avtomatizacijo |
| `kpop` | Orodja, povezana s K-popom |
| `accessibility` | Orodja za dostopnost |
| `analytics` | Analitika in poročanje |
| `wia` | Orodja ekosistema WIA |
| `all` | Pojavi se v vseh kategorijah |

### Priporočene Ikone (Lucide)

| Ime Ikone | Uporaba za |
|-----------|------------|
| `server` | Upravljanje strežnikov |
| `shield` | Varnost |
| `database` | Baza podatkov |
| `activity` | Nadzor |
| `terminal` | Terminalska orodja |
| `code` | Razvoj |
| `hard-drive` | Disk/shranjevanje |
| `network` | Omrežno povezovanje |
| `lock` | Avtentikacija/šifriranje |
| `eye` | Opazovanje/nadzor |
| `check-square` | Naloge/TODO |
| `layout-dashboard` | Nadzorne plošče |
| `settings` | Konfiguracija |
| `zap` | Avtomatizacija |
| `globe` | Spletno/mednarodno |

Oglejte si vseh 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Potrebujete Pomoč?

- **GitHub Težave:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Težave z Vtičniki:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Primeri Vtičnikov:** [Website](https://wiasoom.com)
- **Spletna Stran:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Ustvarite nekaj neverjetnega. Delite to s svetom.</em></p>
<p align="center"><em>— Ekipa WIA SOOM</em></p>
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
