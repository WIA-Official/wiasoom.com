<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Οδηγός Ανάπτυξης Πρόσθετων WIA SOOM</h1>
<p align="center"><strong>Δημιουργήστε το δικό σας πρόσθετο σε 5 λεπτά.</strong></p>
<p align="center">Δημιουργήστε ισχυρά εργαλεία διακομιστή, πίνακες ελέγχου και αυτοματισμούς — απευθείας μέσα στο WIA SOOM.</p>

---

## Πίνακας Περιεχομένων

- [Μέρος 1: Γρήγορη Έναρξη — Το Πρώτο Σας Πρόσθετο σε 5 Λεπτά](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Μέρος 2: Αναφορά API Πλαισίου Πρόσθετου](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Μέρος 3: Δημιουργί�� Προσαρμοσμένου UI με Webviews](#part-3-building-custom-ui-with-webviews)
- [Μέρος 4: Δημοσίευση του Πρόσθετου Σας](#part-4-publishing-your-plugin)
- [Μέρος 5: Καλές Πρακτικές](#part-5-best-practices)
- [Μέρος 6: Παραδείγματα από τον Πραγματικό Κόσμο](#part-6-real-world-examples)
- [Παράρτημα: Κατηγορίες & Εικονίδια](#appendix-categories--icons)

---

## Μέρος 1: Γρήγορη Έναρξη — Το Πρώτο Σας Πρόσθετο σε 5 Λεπτά

### Τι θα δημιουργήσετε

Ένα πρόσθετο "Hello World" που προσθέτει ένα κουμπί στη γραμμή πλευρικών εργαλείων. Όταν κάνετε κλικ, εμφανίζει μια ειδοποίηση.

### Βήμα 1: Δημιουργία του φακέλου πρόσθετου
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Βήμα 2: Δημιουργία package.json
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
**Απαιτούμενα πεδία:** `name`, `version`, `description`, `author`, `main`

### Βήμα 3: Δημιουργία index.js
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
### Βήμα 4: Επανεκκίνηση του WIA SOOM

Επανεκκινήστε την εφαρμογή (ή αλλάξτε την κατάσταση του πρόσθετου στο Ρυθμίσεις → Πρόσθετα).

Θα πρέπει να δείτε ένα κουμπί **"Hello World"** στη γραμμή πλευρικών εργαλείων. Κάντε κλικ — θα δείτε μια ειδοποίηση επιτυχίας!

### Πώς λειτουργεί
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

## Μέρος 2: Αναφορά API Πλαισίου Πρόσθετου

Όταν καλείται η συνάρτηση `activate(context)`, το `context` (ή `ctx`) παρέχει αυτές τις APIs:
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

### `ctx.terminal` — Εκτέλεση εντολών σε απομακρυσμένους διακομιστές

#### `terminal.send(sessionId, data)`

Αποστολή μιας εντολής (ή οποιωνδήποτε δεδομένων) σε μια ενεργή συνεδρία τερματικού.

| Παράμετρος | Τύπος | Περιγραφή |
|------------|-------|-----------|
| `sessionId` | `string` | Η συνεδρία τερματικού στην οποία θα σταλεί |
| `data` | `string` | Η εντολή ή τα δεδομένα που θα σταλούν |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Εγγραφείτε σε όλη την ��ξοδο από μια συνεδρία τερματικού. Επιστρέφει μια **συνάρτηση ακύρωσης**.

| Παράμετρος | Τύπος | Περιγραφή |
|------------|-------|-----------|
| `sessionId` | `string` | Η συνεδρία τερματικού που παρακολουθείτε |
| `callback` | `(data: string) => void` | Καλείται με κάθε κομμάτι εξόδου |
| **Επιστρέφει** | `() => void` | Καλέστε το αυτό για να σταματήσετε την παρακολούθηση |
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
**Σημαντικό:** Αποθηκεύστε πάντα τη συνάρτηση ακύρωσης και καλέστε την στο `deactivate()` για να αποτρέψετε διαρροές μνήμης.

---

### `ctx.sftp` — Μεταφορά αρχείων

> **Κατάσταση: Έρχεται Σύντομα** — Το API SFTP έχει οριστεί αλλά δεν έχει συνδεθεί ακόμα με τη μηχανή SFTP της εφαρμογής. Το `list()` επιστρέφει αυτή τη στιγμή ένα κενό πίνακα, και τα `upload()`/`download()` είναι ανενεργά. Αυτό θα υλοποιηθεί πλήρως σε μελλοντική έκδοση. Προς το παρόν, χρησιμοποιήστε το `ctx.terminal.send()` με εντολές `scp` ή `rsync` ως workaround.

#### `sftp.list(sessionId, path)`

Λίστα αρχείων σε έναν απομακρυσμένο φάκελο.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Μεταφορά ενός αρχείου από τον τοπικό υπολογιστή σε απομακρυσμένο διακομιστή.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Λήψη ενός αρχείου από απομακρυσμένο διακομιστή στον τοπικό υπολογιστή.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Workaround (μέχρι το SFTP API να είναι ενεργό):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Διεπαφή χρήστη

#### `ui.addSidebarButton(options)`

Προσθέστε ένα κουμπί στη γραμμή πλευρικών εργαλείων του WIA SOOM.

| Επιλογή | Τύπος | Απαιτείται | Περιγραφή |
|---------|-------|------------|-----------|
| `id` | `string` | Όχι | Μοναδικό ID (προεπιλογή το όνομα του πρόσθετου) |
| `icon` | `string` | Ναι | Όνομα εικονιδίου Lucide (π.χ., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Ναι | Κείμενο κουμπιού που εμφανίζεται στη γραμμή πλευρικών εργαλείων |
| `onClick` | `() => void` | Ναι | Συνάρτηση που καλείται όταν κάνετε κλικ στο κουμπί |
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
**Αναφορά εικονιδίων:** Περιηγηθείτε σε όλα τα διαθέσιμα εικονίδια στο [lucide.dev/icons](https://lucide.dev/icons)

> **Σημείωση συμβατότητας:** Ορισμένα παλαιότερα πρόσθετα χρησιμοποιούν θεσής παραμέτρους όπως `addSidebarButton(id, icon, label, onClick)`. Η επίσημη API χρησιμοποιεί ένα **αντικείμενο επιλογών** όπως τεκμηριώνεται παραπάνω. Χρησιμοποιείτε πάντα το στυλ αντικειμένου για νέα πρόσθετα.

#### `ui.openWebview(options)`

Ανοίξτε ένα αναδυόμενο παράθυρο με προσαρμοσμένο περιεχόμενο HTML. Έτσι δημιουργείτε πλούσιες διεπαφές χρήστη.

| Επιλογή | Τύπος | Περιγραφή |
|---------|-------|-----------|
| `title` | `string` | Τίτλος παρ��θύρου |
| `html` | `string` | Πλήρες περιεχόμενο HTML προς απόδοση |
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
> Δείτε [Μέρος 3](#part-3-building-custom-ui-with-webviews) για προχωρημένα μοτίβα webview.

#### `ui.showNotification(type, message)`

Εμφάνιση ειδοποίησης toast.

| Παράμετρος | Τύπος | Περιγραφή |
|------------|-------|-----------|
| `type` | `'success' \| 'error' \| 'info'` | Στυλ ειδοποίησης |
| `message` | `string` | Κείμενο προς εμφάνιση |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Προσθέστε ένα μόνιμο στοιχείο κειμένου στη κάτω γραμμή κατάστασης.

| Παράμετρος | Τύπος | Περιγραφή |
|------------|-------|-----------|
| `id` | `string` | Μοναδικό ID για αυτό το στοιχείο κατάστασης |
| `text` | `string` | Κείμενο προς εμφάνιση |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Μόνιμη αποθήκευση

Οι ρυθμίσεις του plugin αποθηκεύονται μόνιμα στο `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Διαβάστε μια αποθηκευμένη τιμή.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Επιστρέφει `undefined` αν το κλειδί δεν υπάρχει.

#### `settings.set(key, value)`

Αποθηκεύστε μια τιμή. Υποστηρίζει συμβολοσειρές, αριθμούς, boolean, πίνακες και αντικείμενα.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Παράδειγμα: Θυμηθείτε τις προτιμήσεις του χρήστη**
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

### `ctx.ai` — Ενσωμάτωση AI

> **Κατάσταση: Έρχεται σύντομα** — Το API AI είναι καθορισμένο αλλά δεν έχει συνδεθεί ακόμα με το Soomy. Αυτή τη στιγμή επιστρέφει `{ response: 'AI not yet connected' }`. Η πλήρης ενσωμάτωση AI προγραμματίζεται για μελλοντική έκδοση.

#### `ai.chat(messages, options?)`

Στείλτε μηνύματα στον βοηθό AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Μέρος 3: Δημιουργία Προσαρμοσμένου UI με Webviews

Το API `openWebview()` σας επιτρέπει να δημιουργείτε UIs ταμπλό με HTML, CSS και JavaScript — όλα μέσα σε ένα παράθυρο popup.

> **Σημαντικός περιορισμός:** Τα webviews είναι **μόνο για εμφάνιση**. Δεν μ��ορούν να καλέσουν πίσω σε API plugins (`ctx.settings`, `ctx.terminal`, κ.λπ.). Χρησιμοποιήστε κουμπιά πλαϊνής μπάρας για όλες τις ενέργειες του χρήστη και χρησιμοποιήστε το `openWebview()` για να εμφανίσετε την τρέχουσα κατάσταση. Αν χρειάζεστε διαδραστικά χαρακτηριστικά, ενεργοποιήστε τα από τα κουμπιά της πλαϊνής μπάρας και ξανανοίξτε το webview για να ανανεώσετε την εμφάνιση.

### Μοτίβο: Εντολή Τερματικού → Ανάλυση Εξόδου → Εμφάνιση σε HTML

Αυτό είναι το πιο κοινό μοτίβο plugin. Εκτελείτε μια εντολή, αναλύετε το αποτέλεσμα και το εμφανίζετε οπτικά.
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
### Μοτίβο: Διαδραστικό Ταμπλό με Αυτόματη Ανανέωση
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
### Μοτίβο: Εμφάν��ση Ρυθμίσεων σε Webview

> **Σημείωση:** Τα webviews είναι μόνο για εμφάνιση — δεν μπορούν να καλέσουν πίσω σε API plugins. Χρησιμοποιήστε το `ctx.settings` στους χειριστές κουμπιών της πλαϊνής μπάρας σας για να τροποποιήσετε τις ρυθμίσεις και χρησιμοποιήστε το `openWebview()` για να δείξετε την τρέχουσα κατάσταση.
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

## Μέρος 4: Δημοσίευση του Plugin σας

### Βήμα 1: Δοκιμή τοπικά

1. Αντιγράψτε το plugin σας στο `~/.wia-soom/plugins/{your-plugin}/`
2. Επανεκκινήστε το WIA SOOM
3. Επαληθεύστε ότι λειτουργεί: το κουμπί της πλαϊνής μπάρας εμφανίζεται, οι δυνατότητες λειτουργούν σωστά
4. Δοκιμάστε περιπτώσεις άκρης: τι συμβαίνει αν δεν είναι συνδεδεμένος κανένας τερματικός;

### Βήμα 2: Προετοιμασία για υποβολή

Ο φάκελος του plugin σας πρέπει να περιέχει:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Απαιτούμενα πεδία `package.json`:**

| Πεδίο | Περιγραφή | Παράδειγμα |
|-------|-------------|---------|
| `name` | Μοναδικό ID σε kebab-case | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | Μια γραμμή περιγραφής | `"Παρακολουθεί τα logs πρόσβασης του nginx σε πραγματικό χρόνο"` |
| `author` | Το όνομά σας | `"John Doe"` |
| `main` | Σημείο εισόδου | `"index.js"` |

**Προαιρετικά πεδία:**

| Πεδίο | Περιγραφή |
|-------|-------------|
| `license` | Τύπος άδειας (συνιστάται MIT) |
| `keywords` | Πίνακας ετικετών αναζήτησης |
| `soom.minVersion` | Ελάχιστη απαιτούμενη έκδοση WIA SOOM |

### Βήμα 3: Υποβολή στο Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Προσθέστε** το plugin σας στο `plugins/{your-plugin-name}/`
3. **Υποβάλετε** ένα Pull Request

### Βήμα 4: Ανασκόπηση και έγκριση

Ανασκοπούμε κάθε plugin για:

- **Ασφάλεια** — καμία επικίνδυνη API (δείτε τους [Κανόνες Ασφαλείας](#security-rules))
- **Ποιότητα** — λειτουργεί; Είναι ο κώδικας καθαρός;
- **Χρησιμότητα** — λύνει ένα πραγματικό πρόβλημα;

Μετά την έγκριση:
1. Το plugin σας προστίθεται στο `registry.json`
2. Δημιουργείται ένα ZIP bundle στο `dist/`
3. Το plugin σας εμφανίζεται στο **Plugin Store** για όλους τους χρήστες WIA SOOM!

---

## Μέρος 5: Καλές Πρακτικές

### Κανόνες Ασφαλείας

Αυτοί οι κανόνες είναι **υποχρεωτικοί**. Plugins που τους παραβιάζουν θα απορρίπτονται.

| Κανόνας | Γιατί |
|------|-----|
| **ΠΟΤΕ** μην χρησιμοποιείτε `eval()` ή `new Function()` | Κίνδυνος εισαγωγής κώδικα |
| **ΠΟΤΕ** μην χρησιμοποιείτε `child_process`, `exec()`, `spawn()` | Χρησιμοποιείτε μόνο `ctx.terminal.send()` για εντολές |
| **ΠΟΤΕ** μην ανακτάτε εξωτερικές URLs | Εξαίρεση: API endpoints του `wiasoom.com` |
| **ΠΟΤΕ** μην έχετε πρόσβαση σε `process.env` | Οι μεταβλητές περιβάλλοντος μπορεί να περιέχουν μυστικά |
| **ΠΟΤΕ** μην χρησιμοποιείτε `require('fs')` απευθείας | Χρησιμοποιείτε `ctx.settings` για αποθήκευση, `ctx.sftp` για μεταφορά αρχείων |
| **ΠΟΤΕ** μην χρησιμοποιείτε εξωτερικά πακέτα npm | Μόνο καθαρός JavaScript — χωρίς node_modules |
| **ΠΡΕΠΕΙ** να χρησιμοποιείτε `ctx.terminal.send()` για όλες τις απομακρυσμένες εντολές | Αυτό περνάει μέσω του ασφαλούς καναλιού SSH |
| **ΠΡΕΠΕΙ** να καθαρίζετε στο `deactivate()` | Αφαιρέστε listeners, καθαρίστε intervals |

### Διαχείριση Σφαλμάτων

Πάντα να περιβάλλετε επικίνδυνες λειτουργίες σε try/catch:
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
### ��αθαρισμός στο deactivate()

Αν το plugin σας δημιουργεί intervals, listeners ή subscriptions — καθαρίστε τα:
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
### Υποστήριξη i18n

Το WIA SOOM υποστηρίζει 254 γλώσσες. Για να κάνετε την ετικέτα του plugin σας μεταφράσιμη, χρησιμοποιήστε μια απλή προσέγγιση:
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

## Μέρος 6: Πραγματικά Παραδείγματα

### Παράδειγμα 1: Έλεγχος Δίσκου Διακομιστή

Εκτελεί `df -h` στον απομακρυσμένο διακομιστή και δείχνει τον χρησιμοποιούμενο/διαθέσιμο χώρο στη γραμμή κατάστασης.
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

### Παράδειγμα 2: Διαχειριστής TODO

Ένα plugin που διαχειρίζεται μια λίστα TODO χρησιμοποιώντας ρυθμίσεις για μόνιμη αποθήκευση και ένα webview για εμφάνιση.

> **Σχέδιο:** Δεδομένου ότι τα webviews δεν μπορούν να καλέσουν άμεσα τις API του plugin, αυτό το plugin χρησιμοποιεί μια προσέγγιση "snapshot" — διαβάζει τα TODO από τις ρυθμίσεις, τα αποδίδει ως HTML μόνο για ανάγνωση και παρέχει ενέργειες βασισμένες σε sidebar για την προσθήκη στοιχείων. Το webview είναι ένα **στρώμα εμφάνισης**, όχι μια διαδραστική φόρμα.
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

### Παράδειγμα 3: Παρακολούθηση Σφαλμάτων

Παρακολουθεί την έξοδο του τερματικού και στέλνει μια ειδοποίηση όταν ανιχνεύονται συγκεκριμένα μοτίβα.
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

## Παράρτημα: Κατηγορίες & Εικονίδια

### Κατηγορίες Πρόσθετων (29)

Χρησιμοποιήστε αυτές στις `package.json` `keywords` ή κατά την υποβολή στο μητρώο:

| Κατηγορία | Περιγραφή |
|-----------|-----------|
| `server` | Γενική διαχείριση διακομιστή |
| `devtools` | Εργαλεία ανάπτυξης |
| `calculator` | Υπολογιστές και μετατροπείς |
| `simulator` | Προσομοιωτές |
| `game` | Παιχνίδια τερματικού |
| `business` | Εργαλεία επιχειρήσεων |
| `security` | Ασφάλεια και επιθεώρηση |
| `web` | Διαχείριση διακομιστή ιστού |
| `education` | Εκπαιδευτικά εργαλεία |
| `health` | Εργαλεία σχετιζόμενα με την υγεία |
| `islamic` | Ισλαμικά εργαλεία (ώρες προσευχής κ.λπ.) |
| `science` | Επισ��ημονικά εργαλεία |
| `quantum` | Εργαλεία κβαντικής υπολογιστικής |
| `ai` | Εργαλεία με τεχνητή νοημοσύνη |
| `biotech` | Εργαλεία βιοτεχνολογίας |
| `space` | Εργαλεία διαστήματος και αστρονομίας |
| `network` | Εργαλεία δικτύου |
| `database` | Διαχείριση βάσεων δεδομένων |
| `monitoring` | Παρακολούθηση διακομιστή |
| `devops` | DevOps και CI/CD |
| `utility` | Γενικά βοηθητικά εργαλεία |
| `design` | Εργαλεία σχεδίασης |
| `ecommerce` | Εργαλεία ηλεκτρονικού εμπορίου |
| `automation` | Εργαλεία αυτοματοποίησης |
| `kpop` | Εργαλεία σχετιζόμενα με K-pop |
| `accessibility` | Εργαλεία προσβασιμότητας |
| `analytics` | Αναλύσεις και αναφορές |
| `wia` | Εργαλεία οικοσυστήματος WIA |
| `all` | Εμφανίζεται σε όλες τις κατηγορίες |

### Συνιστώμενα Εικονίδια (Lucide)

| Όνομα Εικονιδίου | Χρήση για |
|------------------|-----------|
| `server` | Διαχείριση διακομιστή |
| `shield` | Ασφάλεια |
| `database` | Βάση δεδομένων |
| `activity` | Παρακολούθηση |
| `terminal` | Εργαλεία τερματικού |
| `code` | Ανάπτυξη |
| `hard-drive` | Δίσκος/αποθήκευση |
| `network` | Δικτύωση |
| `lock` | Αυθεντικοποίηση/κρυπτογράφηση |
| `eye` | Παρακολούθηση/επίβλεψη |
| `check-square` | Καθήκοντα/TODO |
| `layout-dashboard` | Πίνακες ελέγχου |
| `settings` | Ρυθμίσεις |
| `zap` | Αυτοματοποίηση |
| `globe` | Ιστός/διεθνές |

Περιηγηθείτε σε όλα τα 1,500+ εικονίδια: [lucide.dev/icons](https://lucide.dev/icons)

---

## Χρειάζεστε Βοήθεια;

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Παραδείγματα Πρόσθετων:** [Website](https://wiasoom.com)
- **Ιστοσελίδα:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Δημιουργήστε κάτι καταπληκτικό. Μοιραστείτε το με τον κόσμο.</em></p>
<p align="center"><em>— Η Ομάδα WIA SOOM</em></p>