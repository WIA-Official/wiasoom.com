<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Faites votre propre plugin en 5 minutes.</strong></p>
<p align="center">Créez des outils puissants pour le serveur, des tableaux de bord et des automatisations — directement dans WIA SOOM.</p>

---

## Table of Contents

- [Part 1: Quick Start — Your First Plugin in 5 Minutes](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Part 2: Plugin Context API Reference](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Part 3: Building Custom UI with Webviews](#part-3-building-custom-ui-with-webviews)
- [Part 4: Publishing Your Plugin](#part-4-publishing-your-plugin)
- [Part 5: Best Practices](#part-5-best-practices)
- [Part 6: Real-World Examples](#part-6-real-world-examples)
- [Appendix: Categories & Icons](#appendix-categories--icons)

---

## Part 1: Quick Start — Your First Plugin in 5 Minutes

### What you'll build

Un plugin "Hello World" qui ajoute un bouton à la barre latérale. Lorsqu'il est cliqué, il affiche une notification.

### Step 1: Create the plugin folder
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Step 2: Create package.json
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
**Required fields:** `name`, `version`, `description`, `author`, `main`

### Step 3: Create index.js
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
### Step 4: Restart WIA SOOM

Redémarrez l'application (ou basculez le plugin hors/à nouveau dans Paramètres → Plugins).

Vous devriez voir un bouton **"Hello World"** dans la barre latérale. Cliquez dessus — vous verrez une notification de succès !

### How it works
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

## Part 2: Plugin Context API Reference

Lorsque votre fonction `activate(context)` est appelée, `context` (ou `ctx`) fournit ces APIs :
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

### `ctx.terminal` — Run commands on remote servers

#### `terminal.send(sessionId, data)`

Envoyer une commande (ou toute donnée) à une session terminal active.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | La session terminal à laquelle envoyer |
| `data` | `string` | La commande ou les données à envoyer |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

S'abonner à toute sortie d'une session terminal. Retourne une **fonction de désabonnement**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionId` | `string` | La session terminal à surveiller |
| `callback` | `(data: string) => void` | Appelé avec chaque morceau de sortie |
| **Returns** | `() => void` | Appelez ceci pour arrêter l'écoute |
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
**Important:** Toujours sauvegarder la fonction de désabonnement et l'appeler dans `deactivate()` pour éviter les fuites de mémoire.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — L'API SFTP est définie mais pas encore connectée au moteur SFTP de l'application. `list()` retourne actuellement un tableau vide, et `upload()`/`download()` ne font rien. Cela sera entièrement mis en œuvre dans une future version. Pour l'instant, utilisez `ctx.terminal.send()` avec des commandes `scp` ou `rsync` comme solution de contournement.

#### `sftp.list(sessionId, path)`

Lister les fichiers dans un répertoire distant.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Télécharger un fichier de la machine locale vers le serveur distant.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Télécharger un fichier du serveur distant vers la machine locale.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (until SFTP API is live):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — User interface

#### `ui.addSidebarButton(options)`

Ajouter un bouton à la barre latérale de WIA SOOM.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | Non | ID unique (par défaut au nom du plugin) |
| `icon` | `string` | Oui | Nom de l'icône Lucide (par exemple, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Oui | Texte du bouton affiché dans la barre latérale |
| `onClick` | `() => void` | Oui | Fonction appelée lorsque le bouton est cliqué |
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
**Icon reference:** Browse all available icons at [lucide.dev/icons](https://lucide.dev/icons)

> **Compatibility note:** Certains anciens plugins utilisent des arguments positionnels comme `addSidebarButton(id, icon, label, onClick)`. L'API officielle utilise un **objet d'options** comme documenté ci-dessus. Utilisez toujours le style objet pour les nouveaux plugins.

#### `ui.openWebview(options)`

Ouvrir une fenêtre popup avec du contenu HTML personnalisé. C'est ainsi que vous construisez des interfaces utilisateur riches.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Titre de la fenêtre |
| `html` | `string` | Contenu HTML complet à rendre |
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
> Vez [Part 3](#part-3-building-custom-ui-with-webviews) pour des modèles avancés de webview.

#### `ui.showNotification(type, message)`

Affiche une notification toast.

| Paramètre | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Style de notification |
| `message` | `string` | Texte à afficher |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Ajoute un élément de texte persistant à la barre de statut en bas.

| Paramètre | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ID unique pour cet élément de statut |
| `text` | `string` | Texte à afficher |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Stockage persistant

Les paramètres du plugin sont stockés de manière permanente dans `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lire une valeur sauvegardée.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Renvoie `undefined` si la clé n'existe pas.

#### `settings.set(key, value)`

Sauvegarder une valeur. Prend en charge les chaînes, les nombres, les booléens, les tableaux et les objets.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemple : Se souvenir des préférences utilisateur**
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

### `ctx.ai` — Intégration AI

> **Statut : À venir** — L'API AI est définie mais pas encore connectée à Soomy. Renvoie actuellement `{ response: 'AI not yet connected' }`. Une intégration complète de l'IA est prévue pour une future version.

#### `ai.chat(messages, options?)`

Envoyer des messages à l'assistant AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Part 3: Construction d'une interface utilisateur personnalisée avec des webviews

L'API `openWebview()` vous permet de construire des interfaces de tableau de bord avec HTML, CSS et JavaScript — le tout dans une fenêtre popup.

> **Limitation importante :** Les webviews sont **uniquement d'affichage**. Elles ne peuvent pas rappeler les API de plugin (`ctx.settings`, `ctx.terminal`, etc.). Utilisez des boutons de barre latérale pour toutes les actions utilisateur, et utilisez `openWebview()` pour afficher l'état actuel. Si vous avez besoin de fonctionnalités interactives, déclenchez-les à partir des boutons de la barre latérale et réouvrez la webview pour actualiser l'affichage.

### Modèle : Commande de terminal → Analyser la sortie → Afficher en HTML

C'est le modèle de plugin le plus courant. Vous exécutez une commande, analysez le résultat et l'affichez visuellement.
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
### Modèle : Tableau de bord interactif avec actualisation automatique
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
### Modèle : Affichage des paramètres dans une webview

> **Remarque :** Les webviews sont uniquement d'affichage — elles ne peuvent pas rappeler les API de plugin. Utilisez `ctx.settings` dans vos gestionnaires de boutons de barre latérale pour modifier les paramètres, et utilisez `openWebview()` pour montrer l'état actuel.
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

## Part 4: Publication de votre plugin

### Étape 1 : Tester localement

1. Copiez votre plugin dans `~/.wia-soom/plugins/{your-plugin}/`
2. Redémarrez WIA SOOM
3. Vérifiez que cela fonctionne : le bouton de la barre latérale apparaît, les fonctionnalités fonctionnent correctement
4. Testez les cas limites : que se passe-t-il si aucun terminal n'est connecté ?

### Étape 2 : Préparer la soumission

Votre dossier de plugin doit contenir :
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Requiert `package.json` champs :**

| Champ | Description | Exemple |
|-------|-------------|---------|
| `name` | ID unique en kebab-case | `"my-awesome-plugin"` |
| `version` | Version sémantique | `"1.0.0"` |
| `description` | Description en une ligne | `"Surveille les journaux d'accès nginx en temps réel"` |
| `author` | Votre nom | `"John Doe"` |
| `main` | Point d'entrée | `"index.js"` |

**Champs optionnels :**

| Champ | Description |
|-------|-------------|
| `license` | Type de licence (MIT recommandé) |
| `keywords` | Tableau de tags de recherche |
| `soom.minVersion` | Version minimale de WIA SOOM requise |

### Étape 3 : Soumettre au registre des plugins

1. ****Package** your plugin as a ZIP file
2. **Ajoutez** votre plugin à `plugins/{votre-nom-de-plugin}/`
3. **Soumettez** une Pull Request

### Étape 4 : Révision et approbation

Nous examinons chaque plugin pour :

- **Sécurité** — pas d'APIs dangereuses (voir [Règles de sécurité](#security-rules))
- **Qualité** — est-ce que ça fonctionne ? Le code est-il propre ?
- **Utilité** — est-ce que ça résout un vrai problème ?

Après approbation :
1. Votre plugin est ajouté à `registry.json`
2. Un paquet ZIP est créé dans `dist/`
3. Votre plugin apparaît dans le **Plugin Store** pour tous les utilisateurs de WIA SOOM !

---

## Partie 5 : Meilleures pratiques

### Règles de sécurité

Ces règles sont **obligatoires**. Les plugins qui les violent seront rejetés.

| Règle | Pourquoi |
|------|-----|
| **JAMAIS** utiliser `eval()` ou `new Function()` | Risque d'injection de code |
| **JAMAIS** utiliser `child_process`, `exec()`, `spawn()` | Utilisez seulement `ctx.terminal.send()` pour les commandes |
| **JAMAIS** récupérer des URLs externes | Exception : points de terminaison API de `wiasoom.com` |
| **JAMAIS** accéder à `process.env` | Les variables d'environnement peuvent contenir des secrets |
| **JAMAIS** utiliser `require('fs')` directement | Utilisez `ctx.settings` pour le stockage, `ctx.sftp` pour le transfert de fichiers |
| **JAMAIS** utiliser des paquets externes npm | JavaScript pur seulement — pas de node_modules |
| **DOIT** utiliser `ctx.terminal.send()` pour toutes les commandes distantes | Cela passe par le canal SSH sécurisé |
| **DOIT** nettoyer dans `deactivate()` | Supprimer les écouteurs, effacer les intervalles |

### Gestion des erreurs

Enveloppez toujours les opérations risquées dans try/catch :
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
### Nettoyage dans deactivate()

Si votre plugin crée des intervalles, des écouteurs ou des abonnements — nettoyez-les :
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
### Support i18n

WIA SOOM prend en charge 254 langues. Pour rendre l'étiquette de votre plugin traduisible, utilisez une approche simple :
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

## Partie 6 : Exemples du monde réel

### Exemple 1 : Vérificateur de disque serveur

Exécute `df -h` sur le serveur distant et montre l'espace utilisé/disponible dans la barre d'état.
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

### Exemple 2 : Gestionnaire de TODO

Un plugin qui gère une liste de TODO en utilisant des paramètres pour le stockage persistant et un webview pour l'affichage.

> **Modèle de conception :** Puisque les webviews ne peuvent pas appeler directement les APIs des plugins, ce plugin utilise une approche "instantanée" — il lit les TODO à partir des paramètres, les rend en HTML en lecture seule, et fournit des actions basées sur la barre latérale pour ajouter des éléments. Le webview est une **couche d'affichage**, pas un formulaire interactif.
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

### Exemple 3 : Observateur d'erreurs

Surveille la sortie du terminal et envoie une notification lorsque des motifs spécifiques sont détectés.
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
| `server` | Gestion générale du serveur |
| `devtools` | Outils de développement |
| `calculator` | Calculatrices et convertisseurs |
| `simulator` | Simulateurs |
| `game` | Jeux de terminal |
| `business` | Outils pour les affaires |
| `security` | Sécurité et audit |
| `web` | Gestion de serveur web |
| `education` | Outils éducatifs |
| `health` | Outils liés à la santé |
| `islamic` | Outils islamiques (heures de prière, etc.) |
| `science` | Outils scientifiques |
| `quantum` | Outils de calcul quantique |
| `ai` | Outils alimentés par l'IA |
| `biotech` | Outils de biotechnologie |
| `space` | Outils de l'espace et de l'astronomie |
| `network` | Outils de réseau |
| `database` | Gestion de base de données |
| `monitoring` | Surveillance de serveur |
| `devops` | DevOps et CI/CD |
| `utility` | Utilitaires généraux |
| `design` | Outils de conception |
| `ecommerce` | Outils de commerce électronique |
| `automation` | Outils d'automatisation |
| `kpop` | Outils liés au K-pop |
| `accessibility` | Outils d'accessibilité |
| `analytics` | Analytique et reporting |
| `wia` | Outils de l'écosystème WIA |
| `all` | Apparaît dans toutes les catégories |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Gestion de serveur |
| `shield` | Sécurité |
| `database` | Base de données |
| `activity` | Surveillance |
| `terminal` | Outils de terminal |
| `code` | Développement |
| `hard-drive` | Disque/stockage |
| `network` | Réseautage |
| `lock` | Auth/cryptage |
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

<p align="center"><em>Build something amazing. Share it with the world.</em></p>
<p align="center"><em>— The WIA SOOM Team</em></p>