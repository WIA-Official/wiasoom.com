<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Příručka pro vývojáře pluginů WIA SOOM</h1>
<p align="center"><strong>Vytvořte svůj vlastní plugin za 5 minut.</strong></p>
<p align="center">Vytvářejte výkonné serverové nástroje, panely a automatizace — přímo uvnitř WIA SOOM.</p>

---

## Obsah

- [Část 1: Rychlý start — Váš první plugin za 5 minut](#část-1-rychlý-start--váš-první-plugin-za-5-minut)
- [Část 2: Odkaz na API kontextu pluginu](#část-2-odkaz-na-api-kontekstu-pluginu)
  - [ctx.terminal](#ctxterminal--spouštění-příkazů-na-vzdálených-serverech)
  - [ctx.sftp](#ctxsftp--přenos-souborů)
  - [ctx.ui](#ctxui--uživatelské-rozhraní)
  - [ctx.settings](#ctxsettings--trvalé-úložiště)
  - [ctx.ai](#ctxai--integrace-ai)
- [Část 3: Vytváření vlastního UI pomocí Webview](#část-3-vytváření-vlastního-ui-pomocí-webview)
- [Část 4: Publikování vašeho pluginu](#část-4-publikování-vašeho-pluginu)
- [Část 5: Nejlepší praktiky](#část-5-nejlepší-praktiky)
- [Část 6: Příklady z reálného světa](#část-6-příklady-z-reálného-světa)
- [Dodatek: Kategorie a ikony](#dodatek-kategorie-a-ikony)

---

## Část 1: Rychlý start — Váš první plugin za 5 minut

### Co vytvoříte

Plugin "Hello World", který přidá tlačítko do postranního panelu. Po kliknutí se zobrazí oznámení.

### Krok 1: Vytvořte složku pluginu
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Krok 2: Vytvořte package.json
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
**Povinná pole:** `name`, `version`, `description`, `author`, `main`

### Krok 3: Vytvořte index.js
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
### Krok 4: Restartujte WIA SOOM

Restartujte aplikaci (nebo přepněte plugin vypnout/zapnout v Nastavení → Pluginy).

Měli byste vidět tlačítko **"Hello World"** v postranním panelu. Klikněte na něj — uvidíte úspěšné oznámení!

### Jak to funguje
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

## Část 2: Odkaz na API kontextu pluginu

Když je vaše funkce `activate(context)` zavolána, `context` (nebo `ctx`) poskytuje tyto API:
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

### `ctx.terminal` — Spouštění příkazů na vzdálených serverech

#### `terminal.send(sessionId, data)`

Odešlete příkaz (nebo jakákoliv data) do aktivní terminálové relace.

| Parametr | Typ | Popis |
|----------|-----|-------|
| `sessionId` | `string` | Terminálová relace, do které se má odeslat |
| `data` | `string` | Příkaz nebo data, která se mají odeslat |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Přihlaste se k veškerému výstupu z terminálové relace. Vrací **funkci pro odhlášení**.

| Parametr | Typ | Popis |
|----------|-----|-------|
| `sessionId` | `string` | Terminálová relace, kterou chcete sledovat |
| `callback` | `(data: string) => void` | Zavolá se s každým kusem výstupu |
| **Vrací** | `() => void` | Zavolejte to, abyste přestali poslouchat |
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
**Důležité:** Vždy uložte funkci pro odhlášení a zavolejte ji v `deactivate()`, abyste předešli únikům paměti.

---

### `ctx.sftp` — Přenos souborů

> **Stav: Brzy bude k dispozici** — API SFTP je definováno, ale zatím není propojeno s SFTP enginem aplikace. `list()` v současnosti vrací prázdné pole a `upload()`/`download()` jsou nefunkční. To bude plně implementováno v budoucím vydání. Zatím použijte `ctx.terminal.send()` s příkazy `scp` nebo `rsync` jako workaround.

#### `sftp.list(sessionId, path)`

Vylistujte soubory ve vzdáleném adresáři.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Nahrajte soubor z místního počítače na vzdálený server.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Stáhněte soubor ze vzdáleného serveru na místní počítač.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (dokud nebude API SFTP aktivní):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Uživatelské rozhraní

#### `ui.addSidebarButton(options)`

Přidejte tlačítko do postranního panelu WIA SOOM.

| Možnost | Typ | Povinné | Popis |
|---------|-----|---------|-------|
| `id` | `string` | Ne | Unikátní ID (výchozí je název pluginu) |
| `icon` | `string` | Ano | Název ikony Lucide (např. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ano | Text tlačítka zobrazený v postranním panelu |
| `onClick` | `() => void` | Ano | Funkce zavolaná při kliknutí na tlačítko |
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
**Odkaz na ikony:** Prozkoumejte všechny dostupné ikony na [lucide.dev/icons](https://lucide.dev/icons)

> **Poznámka o kompatibilitě:** Některé starší pluginy používají poziční argumenty jako `addSidebarButton(id, icon, label, onClick)`. Oficiální API používá **objekt možností**, jak je uvedeno výše. Vždy používejte styl objektu pro nové pluginy.

#### `ui.openWebview(options)`

Otevřete vyskakovací okno s vlastním HTML obsahem. Takto vytváříte bohatá uživatelská rozhraní.

| Možnost | Typ | Popis |
|---------|-----|-------|
| `title` | `string` | Název okna |
| `html` | `string` | Celý HTML obsah k vykreslení |
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
> Viz [Část 3](#part-3-building-custom-ui-with-webviews) pro pokročilé vzory webview.

#### `ui.showNotification(type, message)`

Zobrazí toast notifikaci.

| Parametr | Typ | Popis |
|----------|-----|-------|
| `type` | `'success' \| 'error' \| 'info'` | Styl notifikace |
| `message` | `string` | Text k zobrazení |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

Přidá trvalou textovou položku do spodního stavového řádku.

| Parametr | Typ | Popis |
|----------|-----|-------|
| `id` | `string` | Unikátní ID pro tuto stavovou položku |
| `text` | `string` | Text k zobrazení |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — Trvalé úložiště

Nastavení pluginu jsou trvale uložena v `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Přečte uloženou hodnotu.
§§§CHUNK_SEPARATOR§§§
Vrátí `undefined`, pokud klíč neexistuje.

#### `settings.set(key, value)`

Uloží hodnotu. Podporuje řetězce, čísla, booleany, pole a objekty.
§§§CHUNK_SEPARATOR§§§
**Příklad: Zapamatování uživatelských preferencí**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — Integrace AI

> **Stav: Brzy k dispozici** — AI API je definováno, ale dosud není připojeno k Soomy. Aktuálně vrací `{ response: 'AI not yet connected' }`. Plná integrace AI je plánována na budoucí verzi.

#### `ai.chat(messages, options?)`

Odeslat zprávy AI asistentovi (Soomy).
§§§CHUNK_SEPARATOR§§§
---

## Část 3: Vytváření vlastního UI s webview

API `openWebview()` vám umožňuje vytvářet dashboard UI pomocí HTML, CSS a JavaScriptu — vše uvnitř vyskakovacího okna.

> **Důležité omezení:** Webview jsou **pouze pro zobrazení**. Nemohou volat zpět do plugin API (`ctx.settings`, `ctx.terminal`, atd.). Používejte tlačítka na bočním panelu pro všechny uživatelské akce a použijte `openWebview()` k zobrazení aktuálního stavu. Pokud potřebujete interaktivní funkce, aktivujte je z tlačítek na bočním panelu a znovu otevřete webview pro obnovení zobrazení.

### Vzor: Terminálový příkaz → Zpracování výstupu → Zobrazení v HTML

Toto je nejběžnější vzor pluginu. Spustíte příkaz, zpracujete výsledek a vizuálně ho zobrazíte.
§§§CHUNK_SEPARATOR§§§
### Vzor: Interaktivní dashboard s automatickým obnovením
§§§CHUNK_SEPARATOR§§§
### Vzor: Zobrazení nastavení ve webview

> **Poznámka:** Webview jsou pouze pro zobrazení — nemohou volat zpět do plugin API. Používejte `ctx.settings` ve vašich obslužných funkcích tlačítek na bočním panelu k úpravě nastavení a použijte `openWebview()` k zobrazení aktuálního stavu.
§§§CHUNK_SEPARATOR§§§
---

## Část 4: Publikování vašeho pluginu

### Krok 1: Testování lokálně

1. Zkopírujte svůj plugin do `~/.wia-soom/plugins/{your-plugin}/`
2. Restartujte WIA SOOM
3. Ověřte, že to funguje: tlačítko na bočním panelu se objeví, funkce fungují správně
4. Otestujte okrajové případy: co se stane, pokud není připojen žádný terminál?

### Krok 2: Příprava na odeslání

Váš pluginový adresář musí obsahovat:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
**Povinná pole `package.json`:**

| Pole | Popis | Příklad |
|------|-------|---------|
| `name` | Unikátní ID ve formátu kebab-case | `"my-awesome-plugin"` |
| `version` | Sémantická verze | `"1.0.0"` |
| `description` | Popis v jedné větě | `"Sleduje nginx přístupové protokoly v reálném čase"` |
| `author` | Vaše jméno | `"John Doe"` |
| `main` | Vstupní bod | `"index.js"` |

**Volitelná pole:**

| Pole | Popis |
|------|-------|
| `license` | Typ licence (doporučeno MIT) |
| `keywords` | Pole vyhledávacích tagů |
| `soom.minVersion` | Minimální požadovaná verze WIA SOOM |

### Krok 3: Odeslání do Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Přidejte** svůj plugin do `plugins/{název-vášho-pluginu}/`
3. **Odešlete** Pull Request

### Krok 4: Kontrola a schválení

Každý plugin kontrolujeme na:

- **Bezpečnost** — žádné nebezpečné API (viz [Bezpečnostní pravidla](#security-rules))
- **Kvalitu** — funguje to? Je k��d čistý?
- **Užitečnost** — řeší to skutečný problém?

Po schválení:
1. Váš plugin je přidán do `registry.json`
2. ZIP balíček je vytvořen v `dist/`
3. Váš plugin se objeví v **Plugin Store** pro všechny uživatele WIA SOOM!

---

## Část 5: Nejlepší praktiky

### Bezpečnostní pravidla

Tato pravidla jsou **povinná**. Pluginy, které je poruší, budou odmítnuty.

| Pravidlo | Proč |
|----------|------|
| **NIKDY** nepoužívejte `eval()` nebo `new Function()` | Riziko injekce kódu |
| **NIKDY** nepoužívejte `child_process`, `exec()`, `spawn()` | Používejte pouze `ctx.terminal.send()` pro příkazy |
| **NIKDY** nevyžadujte externí URL | Výjimka: API koncové body `wiasoom.com` |
| **NIKDY** nepřistupujte k `process.env` | Proměnné prostředí mohou obsahovat tajemství |
| **NIKDY** nepoužívejte `require('fs')` přímo | Používejte `ctx.settings` pro úložiště, `ctx.sftp` pro přenos souborů |
| **NIKDY** nepoužívejte externí balíčky npm | Pouze čistý JavaScript — žádné node_modules |
| **MUSÍTE** používat `ctx.terminal.send()` pro všechny vzdálené příkazy | To prochází zabezpečeným SSH kanálem |
| **MUSÍTE** provést úklid v `deactivate()` | Odeberte posluchače, vymažte intervaly |

### Zpracování chyb

Vždy obalujte rizikové operace do try/catch:
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
### Úklid v deactivate()

Pokud váš plugin vytváří intervaly, posluchače nebo předplatné — vyčistěte je:
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
### Podpora i18n

WIA SOOM podporuje 254 jazyků. Aby byl váš pluginový štítek přeložitelný, použijte jednoduchý přístup:
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Část 6: Příklady ze skutečného světa

### Příklad 1: Kontrola disku serveru

Spouští `df -h` na vzdáleném serveru a zobrazuje použité/dostupné místo v stavovém řádku.
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

### Příklad 2: Správce TODO

Plugin, který spravuje seznam TODO pomocí nastavení pro trvalé úložiště a webview pro zobrazení.

> **Designový vzor:** Protože webview nemohou přímo volat pluginová API, tento plugin používá přístup "snapshot" — čte TODO z nastavení, zobrazuje je jako pouze pro čtení HTML a poskytuje akce na základě postranního panelu pro přidávání položek. Webview je **zobrazovací** vrstva, nikoli interaktivní formulář.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

### Příklad 3: Sledovač chyb

Sleduje výstup terminálu a posílá oznámení, když jsou detekovány specifické vzory.
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
---

## Dodatek: Kategorie & Ikony

### Kategorie pluginů (29)

Použijte je ve svém `package.json` `keywords` nebo při odesílání do registru:

| Kategorie | Popis |
|-----------|-------|
| `server` | Obecná správa serveru |
| `devtools` | Vývojové nástroje |
| `calculator` | Kalkulačky a převodníky |
| `simulator` | Simulátory |
| `game` | Terminálové hry |
| `business` | Nástroje pro podnikání |
| `security` | Bezpečnost a audit |
| `web` | Správa webového serveru |
| `education` | Vzdělávací nástroje |
| `health` | Nástroje související se zdravím |
| `islamic` | Islámské nástroje (časy modliteb atd.) |
| `science` | Vědecké nástroje |
| `quantum` | Nástroje pro kvantové počítačství |
| `ai` | Nástroje poháněné AI |
| `biotech` | Nástroje biotechnologie |
| `space` | Nástroje pro vesmír a astronomii |
| `network` | Nástroje pro síť |
| `database` | Správa databází |
| `monitoring` | Monitorování serveru |
| `devops` | DevOps a CI/CD |
| `utility` | Obecné utilitky |
| `design` | Nástroje pro design |
| `ecommerce` | Nástroje pro e-commerce |
| `automation` | Nástroje pro automatizaci |
| `kpop` | Nástroje související s K-popem |
| `accessibility` | Nástroje pro přístupnost |
| `analytics` | Analytika a reporting |
| `wia` | Nástroje ekosystému WIA |
| `all` | Objevuje se ve všech kategoriích |

### Doporučené ikony (Lucide)

| Název ikony | Použití pro |
|-------------|-------------|
| `server` | Správa serveru |
| `shield` | Bezpečnost |
| `database` | Databáze |
| `activity` | Monitorování |
| `terminal` | Terminálové nástroje |
| `code` | Vývoj |
| `hard-drive` | Disk/úložiště |
| `network` | Síťování |
| `lock` | Autentizace/šifrování |
| `eye` | Sledování/monitorování |
| `check-square` | Úkoly/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Konfigurace |
| `zap` | Automatizace |
| `globe` | Web/mezinárodní |

Prozkoumejte všech 1 500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Potřebujete pomoc?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Příklad pluginů:** [Website](https://wiasoom.com)
- **Web:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Vytvořte něco úžasného. Sdílejte to se světem.</em></p>
<p align="center"><em>— Tým WIA SOOM</em></p>
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
