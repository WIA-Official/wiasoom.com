<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guide ya Bato ya Plugin ya WIA SOOM</h1>
<p align="center"><strong>Fanda plugin na yo na miniti 5.</strong></p>
<p align="center">Sika na ba outils ya serveur, ba dashboards, mpe ba automations — na WIA SOOM.</p>

---

## Taba ya Mibeko

- [Partie 1: Koma Malamu — Plugin na Yo ya Mbala ya Liboso na Miniti 5](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Partie 2: Reference ya Plugin Context API](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Partie 3: Koma UI ya Sika na ba Webviews](#part-3-building-custom-ui-with-webviews)
- [Partie 4: Kobimisa Plugin na Yo](#part-4-publishing-your-plugin)
- [Partie 5: Ba Mibeko Malamu](#part-5-best-practices)
- [Partie 6: Ba Exemple ya Mibeko ya Mabele](#part-6-real-world-examples)
- [Appendice: Ba Kategori & Ba Ikoni](#appendix-categories--icons)

---

## Partie 1: Koma Malamu — Plugin na Yo ya Mbala ya Liboso na Miniti 5

### Oyo okokoma

Plugin "Hello World" oyo ebandeli na bouton na sidebar. Soki oklikaki, ekozala na notification.

### Etape 1: Koma folder ya plugin
§§§CHUNK_SEPARATOR§§§
### Etape 2: Koma package.json
§§§CHUNK_SEPARATOR§§§
**Ba moke ya sika:** `name`, `version`, `description`, `author`, `main`

### Etape 3: Koma index.js
§§§CHUNK_SEPARATOR§§§
### Etape 4: Tanga WIA SOOM

Tanga application (to toggle plugin off/on na Settings → Plugins).

Okosala na **"Hello World"** bouton na sidebar. Klikela — okokanga notification ya succès!

### Oyo ezali kosala
§§§CHUNK_SEPARATOR§§§
---

## Partie 2: Reference ya Plugin Context API

Soki fonction na yo `activate(context)` ebandaki, `context` (to `ctx`) ebandeli ba APIs oyo:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Kosala ba commandes na ba serveur ya mabele

#### `terminal.send(sessionId, data)`

Send commande (to data nyonso) na session ya terminal oyo ezali active.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal oyo okosenda |
| `data` | `string` | Commande to data oyo okosenda |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Sunga na nyonso output oyo ebandi na session ya terminal. Ekozala na **fonction ya ko désinscrire**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | Session ya terminal oyo okotala |
| `callback` | `(data: string) => void` | Ekozala na chaque chunk ya output |
| **Ekozala** | `() => void` | Kanga yango soki okosala ko sunga |
§§§CHUNK_SEPARATOR§§§
**Mokuse:** Tika na ko sunga fonction ya désinscription mpe tanga yango na `deactivate()` mpo na kokitisa ba memory leaks.

---

### `ctx.sftp` — Transfer ya file

> **Etat: Ekozala Koma** — SFTP API ebandeli, kasi ezali te na moteur ya SFTP ya application. `list()` ebandeli na array ya bobele, mpe `upload()`/`download()` ezali ba no-ops. Ekozala na implementation ya final na liboso ya release. Na tango oyo, sunga `ctx.terminal.send()` na ba commandes `scp` to `rsync` mpo na ko sunga.

#### `sftp.list(sessionId, path)`

List ba files na directory ya mabele.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Upload file na machine ya local to serveur ya mabele.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Download file na serveur ya mabele to machine ya local.
§§§CHUNK_SEPARATOR§§§
**Ko sunga (tango SFTP API ezali na vie):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Add bouton na sidebar ya WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Te | ID ya unique (ekoma na plugin name) |
| `icon` | `string` | Yes | Nkombo ya ikoni ya Lucide (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Texte ya bouton oyo eza na sidebar |
| `onClick` | `() => void` | Yes | Fonction oyo ebandeli soki bouton eklikaki |
§§§CHUNK_SEPARATOR§§§
**Ikoni reference:** Tala ba ikoni nyonso oyo ezali na [lucide.dev/icons](https://lucide.dev/icons)

> **Mokuse ya compatibilité:** Ba plugins ya kala bazali na ba arguments ya position ndenge `addSidebarButton(id, icon, label, onClick)`. API ya officiel ezali na **options object** ndenge ebandeli na likambo ya likolo. Sunga toujours style ya object mpo na ba plugins ya sika.

#### `ui.openWebview(options)`

Tanga popup window na contenu ya HTML ya sika. Oyo ezali ndenge ya kokoma ba UIs ya kitoko.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Nkombo ya window |
| `html` | `string` | Contenu ya HTML oyo eza na liboso |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Tala [Part 3](#part-3-building-custom-ui-with-webviews) mpo na ba-pattern ya webview ya niveau avancé.

#### `ui.showNotification(type, message)`

Koma notification ya toast.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Moko ya notification |
| `message` | `string` | Mabele ya kolakisa |
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

Kokota mabele ya texte ya seko na status bar ya nse.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID ya solo mpo na mabele ya status oyo |
| `text` | `string` | Mabele ya kolakisa |
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

### `ctx.settings` — Espace ya stockage ya seko

Ba-paramètres ya plugin ezalaka na stockage ya seko na `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Luka valeur oyo esalemi.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Ekozwa `undefined` soki clé ezali te.

#### `settings.set(key, value)`

Kokota valeur. Ekozwaka ba-strings, ba-nombres, ba-booleans, ba-tableaux, mpe ba-objets.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Exemple: Koyeba ba-préférences ya utilisateur**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — Intégration ya AI

> **État: Eza na nzela** — API ya AI eza na définition mais esaleli te na Soomy. Ekozwa `{ response: 'AI not yet connected' }`. Intégration complète ya AI eza na projet mpo na libération ya nsima.

#### `ai.chat(messages, options?)`

Tinda ba-messages na assistant ya AI (Soomy).
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

## Part 3: Kosembola UI ya Soko na Webviews

API ya `openWebview()` epermite yo kosala ba-UIs ya dashboard na HTML, CSS, mpe JavaScript — nyonso na ndako ya popup.

> **Limitation ya ntina:** Ba-webviews ezali **kosala kaka**. Bazali te na possibilité ya kokota na ba-API ya plugin (`ctx.settings`, `ctx.terminal`, etc.). Tinda ba-boutons ya sidebar mpo na ba-actions ya utilisateur, mpe tinda `openWebview()` mpo na kolakisa état ya sikoyo. Soki olingi ba-fonctions interactives, tinda yango na ba-boutons ya sidebar mpe kokota lisusu na webview mpo na kokitisa kolakisa.

### Pattern: Commande ya Terminal → Parse Output → Kolakisa na HTML

Oyo ezali pattern ya plugin ya sika. Okozwa commande, okoparser résultat, mpe okolakisa yango na ndenge ya mabele.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Pattern: Dashboard Interactif na Auto-Rafraîchissement
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Pattern: Kolakisa Ba-Paramètres na Webview

> **Note:** Ba-webviews ezali display-only — bazali te na possibilité ya kokota na ba-API ya plugin. Tinda `ctx.settings` na ba-handlers ya ba-boutons ya sidebar mpo na kosala ba-paramètres, mpe tinda `openWebview()` mpo na kolakisa état ya sikoyo.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Part 4: Koyebisa Plugin Na Yo

### Étape 1: Teste na ndako

1. Kopi plugin na yo na `~/.wia-soom/plugins/{your-plugin}/`
2. Kanga WIA SOOM lisusu
3. Tika ete ezali kosala: bouton ya sidebar ezali kolakisa, ba-fonctions ezali kosala malamu
4. Teste ba-cas extrêmes: nini ekoyekola soki terminal ezali te?

### Étape 2: Prepare mpo na submission

Folder ya plugin na yo esengeli kozala na:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Mokolo ya `package.json` oyo esengeli:**

| Mokolo | Nkombo | Mibeko |
|--------|--------|---------|
| `name` | ID ya kebab-case ya solo | `"my-awesome-plugin"` |
| `version` | Version ya semantique | `"1.0.0"` |
| `description` | Nkombo moko ya nkombo | `"Monitors nginx access logs in real-time"` |
| `author` | Nkombo na yo | `"John Doe"` |
| `main` | Mokolo ya ntango | `"index.js"` |

**Mokolo ya sika:**

| Mokolo | Nkombo |
|--------|--------|
| `license` | Nkombo ya lisensi (MIT esengeli) |
| `keywords` | Mibeko ya makambo ya recherche |
| `soom.minVersion` | Version ya WIA SOOM oyo esengeli | 

### Etape 3: Tindika na Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** plugin na yo na `plugins/{nom-plugin-na-yo}/`
3. **Submit** Pull Request

### Etape 4: Kokanga mpe kotalela

Tindaka na biso plugin nyonso mpo na:

- **Sécurité** — te na API ya mabe (tala [Règles de Sécurité](#security-rules))
- **Qualité** — ezali kosala? Code ezali malamu?
- **Utilité** — ezali kokanga likambo ya solo?

Ntango ekokani:
1. Plugin na yo ekozala na `registry.json`
2. Bundle ya ZIP ekosala na `dist/`
3. Plugin na yo ekozala na **Plugin Store** mpo na ba utilisateur nyonso ya WIA SOOM!

---

## Parte 5: Makambo Malamu

### Règles de Sécurité

Makambo oyo ezali **esengeli**. Ba plugins oyo ebotaka yango ekosala mabe.

| Règle | Pourquoi |
|-------|----------|
| **TE** salela `eval()` to `new Function()` | Risque ya code injection |
| **TE** salela `child_process`, `exec()`, `spawn()` | Salela kaka `ctx.terminal.send()` mpo na ba commandes |
| **TE** tinda ba URL ya libanda | Exemption: ba API endpoints ya `wiasoom.com` |
| **TE** salela `process.env` | Ba variables ya environnement ekoki kozala na ba secrets |
| **TE** salela `require('fs')` na ntango moko | Salela `ctx.settings` mpo na stockage, `ctx.sftp` mpo na transfert ya fichier |
| **TE** salela ba packages ya npm ya libanda | JavaScript ya solo kaka — te na node_modules |
| **ESENGELI** salela `ctx.terminal.send()` mpo na ba commandes nyonso ya libanda | Oyo ekozwa na SSH channel ya sécurité |
| **ESENGELI** salela `deactivate()` mpo na kokanga | Kanga ba listeners, pusa ba intervals |

### Kokanga Makambo Mabe

Tika nyonso na ba opérations ya risque na try/catch:
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
### Kokanga na deactivate()

Soki plugin na yo esala ba intervals, ba listeners, to ba subscriptions — kokanga yango:
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
### i18n Support

WIA SOOM etindaka 254 langues. Mpo na kosala ete label ya plugin na yo ekozala na makambo ya kolobela, salela nzela ya malamu:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## Parte 6: Ba Exemple ya Mokili

### Exemple 1: Server Disk Checker

Esalaka `df -h` na serveur ya libanda mpe elakisaka esika oyo esalemi/ekokani na status bar.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Exemple 2: TODO Manager

Plugin oyo ebandaka liste ya TODO na ba settings mpo na stockage ya mabele mpe webview mpo na kolakisa.

> **Design pattern:** Lokola ba webviews te ekoki kokota na ba plugin APIs, plugin oyo esalaka "snapshot" — eza na koluka ba TODOs na ba settings, eza na kolakisa yango lokola HTML ya kokanga, mpe eza na ba actions oyo ezali na sidebar mpo na kokota ba items. Webview ezali **layer** ya kolakisa, te formulaire ya interactive.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### Exemple 3: Error Watcher

Emonisaka output ya terminal mpe ebotaka notification ntango ba patterns ya solo ezali komonana.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | Gestion générale du serveur |
| `devtools` | Outils de développement |
| `calculator` | Calculatrices et convertisseurs |
| `simulator` | Simulateurs |
| `game` | Jeux de terminal |
| `business` | Outils d'affaires |
| `security` | Sécurité et audit |
| `web` | Gestion du serveur web |
| `education` | Outils éducatifs |
| `health` | Outils liés à la santé |
| `islamic` | Outils islamiques (horaires de prière, etc.) |
| `science` | Outils scientifiques |
| `quantum` | Outils de calcul quantique |
| `ai` | Outils alimentés par l'IA |
| `biotech` | Outils de biotechnologie |
| `space` | Outils d'espace et d'astronomie |
| `network` | Outils de réseau |
| `database` | Gestion de base de données |
| `monitoring` | Surveillance du serveur |
| `devops` | DevOps et CI/CD |
| `utility` | Utilitaires généraux |
| `design` | Outils de conception |
| `ecommerce` | Outils de commerce électronique |
| `automation` | Outils d'automatisation |
| `kpop` | Outils liés au K-pop |
| `accessibility` | Outils d'accessibilité |
| `analytics` | Analyse et reporting |
| `wia` | Outils de l'écosystème WIA |
| `all` | Apparaît dans toutes les catégories |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Gestion du serveur |
| `shield` | Sécurité |
| `database` | Base de données |
| `activity` | Surveillance |
| `terminal` | Outils de terminal |
| `code` | Développement |
| `hard-drive` | Disque/stockage |
| `network` | Réseautage |
| `lock` | Auth/encryption |
| `eye` | Surveillance/monitoring |
| `check-square` | Tâches/TODO |
| `layout-dashboard` | Tableaux de bord |
| `settings` | Configuration |
| `zap` | Automatisation |
| `globe` | Web/international |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construisez quelque chose d'incroyable. Partagez-le avec le monde.</em></p>
<p align="center"><em>— L'équipe WIA SOOM</em></p>
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
