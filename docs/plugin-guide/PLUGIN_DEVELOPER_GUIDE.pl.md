<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Przewodnik dla deweloperów wtyczek WIA SOOM</h1>
<p align="center"><strong>Stwórz swoją własną wtyczkę w 5 minut.</strong></p>
<p align="center">Twórz potężne narzędzia serwerowe, pulpity i automatyzacje — bezpośrednio w WIA SOOM.</p>

---

## Spis treści

- [Część 1: Szybki start — Twoja pierwsza wtyczka w 5 minut](#część-1-szybki-start--twoja-pierwsza-wtyczka-w-5-minut)
- [Część 2: Referencja API kontekstu wtyczki](#część-2-referencja-api-kontekstu-wtyczki)
  - [ctx.terminal](#ctxterminal--uruchamianie-komend-na-zdalnych-serwerach)
  - [ctx.sftp](#ctxsftp--transfer-plików)
  - [ctx.ui](#ctxui--interfejs-użytkownika)
  - [ctx.settings](#ctxsettings--trwałe-przechowywanie)
  - [ctx.ai](#ctxai--integracja-ai)
- [Część 3: Budowanie niestandardowego UI z Webviews](#część-3-budowanie-niestandardowego-ui-z-webviews)
- [Część 4: Publikowanie Twojej wtyczki](#część-4-publikowanie-twojej-wtyczki)
- [Część 5: Najlepsze praktyki](#część-5-najlepsze-praktyki)
- [Część 6: Przykłady z życia wzięte](#część-6-przykłady-z-życia-wzięte)
- [Dodatek: Kategorie i ikony](#dodatek-kategorie-i-ikony)

---

## Część 1: Szybki start — Twoja pierwsza wtyczka w 5 minut

### Co zbudujesz

Wtyczkę "Hello World", która dodaje przycisk do paska bocznego. Po kliknięciu wyświetla powiadomienie.

### Krok 1: Utwórz folder wtyczki
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Krok 2: Utwórz package.json
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
**Wymagane pola:** `name`, `version`, `description`, `author`, `main`

### Krok 3: Utwórz index.js
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
### Krok 4: Zrestartuj WIA SOOM

Zrestartuj aplikację (lub przełącz wtyczkę włącz/wyłącz w Ustawienia → Wtyczki).

Powinieneś zobaczyć przycisk **"Hello World"** w pasku bocznym. Kliknij go — zobaczysz powiadomienie o sukcesie!

### Jak to działa
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

## Część 2: Referencja API kontekstu wtyczki

Kiedy wywołana jest funkcja `activate(context)`, `context` (lub `ctx`) udostępnia te API:
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

### `ctx.terminal` — Uruchamianie komend na zdalnych serwerach

#### `terminal.send(sessionId, data)`

Wyślij komendę (lub jakiekolwiek dane) do aktywnej sesji terminala.

| Parametr | Typ | Opis |
|----------|-----|------|
| `sessionId` | `string` | Sesja terminala, do której wysyłasz |
| `data` | `string` | Komenda lub dane do wysłania |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Subskrybuj wszystkie wyjścia z sesji terminala. Zwraca funkcję **anulowania subskrypcji**.

| Parametr | Typ | Opis |
|----------|-----|------|
| `sessionId` | `string` | Sesja terminala do obserwacji |
| `callback` | `(data: string) => void` | Wywoływana z każdym fragmentem wyjścia |
| **Zwraca** | `() => void` | Wywołaj to, aby przestać nasłuchiwać |
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
**Ważne:** Zawsze zapisz funkcję anulowania subskrypcji i wywołaj ją w `deactivate()`, aby zapobiec wyciekom pamięci.

---

### `ctx.sftp` — Transfer plików

> **Status: Wkrótce** — API SFTP jest zdefiniowane, ale jeszcze nie podłączone do silnika SFTP aplikacji. `list()` obecnie zwraca pustą tablicę, a `upload()`/`download()` są no-opami. To zostanie w pełni zaimplementowane w przyszłej wersji. Na razie użyj `ctx.terminal.send()` z komendami `scp` lub `rsync` jako obejścia.

#### `sftp.list(sessionId, path)`

Wyświetl pliki w zdalnym katalogu.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Prześlij plik z lokalnego komputera na zdalny serwer.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Pobierz plik z zdalnego serwera na lokalny komputer.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Obejście (do czasu uruchomienia API SFTP):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interfejs użytkownika

#### `ui.addSidebarButton(options)`

Dodaj przycisk do paska bocznego WIA SOOM.

| Opcja | Typ | Wymagane | Opis |
|-------|-----|----------|------|
| `id` | `string` | Nie | Unikalny identyfikator (domyślnie nazwa wtyczki) |
| `icon` | `string` | Tak | Nazwa ikony Lucide (np. `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Tak | Tekst przycisku wyświetlany w pasku bocznym |
| `onClick` | `() => void` | Tak | Funkcja wywoływana po kliknięciu przycisku |
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
**Referencja ikon:** Przeglądaj wszystkie dostępne ikony na [lucide.dev/icons](https://lucide.dev/icons)

> **Uwaga dotycząca zgodności:** Niektóre starsze wtyczki używają argumentów pozycyjnych, takich jak `addSidebarButton(id, icon, label, onClick)`. Oficjalne API używa **obiektu opcji** zgodnie z dokumentacją powyżej. Zawsze używaj stylu obiektu dla nowych wtyczek.

#### `ui.openWebview(options)`

Otwórz okno popup z niestandardową zawartością HTML. W ten sposób budujesz bogate interfejsy użytkownika.

| Opcja | Typ | Opis |
|-------|-----|------|
| `title` | `string` | Tytuł okna |
| `html` | `string` | Cała zawartość HTML do renderowania |
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
> Zobacz [Część 3](#part-3-building-custom-ui-with-webviews) dla zaawansowanych wzorców webview.

#### `ui.showNotification(type, message)`

Wyświetl powiadomienie toast.

| Parametr | Typ | Opis |
|----------|-----|------|
| `type` | `'success' \| 'error' \| 'info'` | Styl powiadomienia |
| `message` | `string` | Tekst do wyświetlenia |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Dodaj trwały element tekstowy do dolnego paska statusu.

| Parametr | Typ | Opis |
|----------|-----|------|
| `id` | `string` | Unikalny identyfikator dla tego elementu statusu |
| `text` | `string` | Tekst do wyświetlenia |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Trwałe przechowywanie

Ustawienia wtyczki są przechowywane na stałe w `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Odczytaj zapisany wartość.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Zwraca `undefined`, jeśli klucz nie istnieje.

#### `settings.set(key, value)`

Zapisz wartość. Obsługuje ciągi, liczby, wartości logiczne, tablice i obiekty.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Przykład: Zapamiętaj preferencje użytkownika**
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

### `ctx.ai` — Integracja AI

> **Status: Wkrótce** — API AI jest zdefiniowane, ale jeszcze nie połączone z Soomy. Obecnie zwraca `{ response: 'AI not yet connected' }`. Pełna integracja AI jest planowana na przyszłą wersję.

#### `ai.chat(messages, options?)`

Wyślij wiadomości do asystenta AI (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Część 3: Budowanie niestandardowego UI z Webviews

API `openWebview()` pozwala na budowanie interfejsów dashboardów z HTML, CSS i JavaScript — wszystko w oknie popup.

> **Ważne ograniczenie:** Webviews są **tylko do wyświetlania**. Nie mogą wywoływać API wtyczek (`ctx.settings`, `ctx.terminal`, itd.). Użyj przycisków w pasku bocznym do wszystkich działań użytkownika, a `openWebview()` do wyświetlenia bieżącego stanu. Jeśli potrzebujesz interaktywnych funkcji, wyzwól je z przycisków w pasku bocznym i ponownie otwórz webview, aby odświeżyć wyświetlanie.

### Wzorzec: Komenda terminala → Parsowanie wyniku → Wyświetlanie w HTML

To najczęstszy wzorzec wtyczek. Uruchamiasz komendę, parsujesz wynik i wyświetlasz go wizualnie.
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
### Wzorzec: Interaktywny dashboard z automatycznym odświeżaniem
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
### Wzorzec: Wyświetlanie ustawień w webview

> **Uwaga:** Webviews są tylko do wyświetlania — nie mogą wywoływać API wtyczek. Użyj `ctx.settings` w swoich handlerach przycisków w pasku bocznym, aby modyfikować ustawienia, a `openWebview()` do pokazania bieżącego stanu.
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

## Część 4: Publikowanie Twojej wtyczki

### Krok 1: Testuj lokalnie

1. Skopiuj swoją wtyczkę do `~/.wia-soom/plugins/{your-plugin}/`
2. Uruchom ponownie WIA SOOM
3. Zweryfikuj, że działa: przycisk w pasku bocznym się pojawia, funkcje działają poprawnie
4. Przetestuj skrajne przypadki: co się stanie, jeśli żaden terminal nie jest podłączony?

### Krok 2: Przygotuj do przesłania

Folder Twojej wtyczki musi zawierać:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Wymagane pola `package.json`:**

| Pole | Opis | Przykład |
|-------|-------------|---------|
| `name` | Unikalny identyfikator w formacie kebab-case | `"my-awesome-plugin"` |
| `version` | Wersja semantyczna | `"1.0.0"` |
| `description` | Opis w jednej linii | `"Monitoruje logi dostępu nginx w czasie rzeczywistym"` |
| `author` | Twoje imię i nazwisko | `"John Doe"` |
| `main` | Punkt wejścia | `"index.js"` |

**Pola opcjonalne:**

| Pole | Opis |
|-------|-------------|
| `license` | Typ licencji (zalecane MIT) |
| `keywords` | Tablica tagów do wyszukiwania |
| `soom.minVersion` | Minimalna wymagana wersja WIA SOOM |

### Krok 3: Zgłoszenie do Rejestru Wtyczek

1. ****Package** your plugin as a ZIP file
2. **Dodaj** swoją wtyczkę do `plugins/{twoja-nazwa-wtyczki}/`
3. **Zgłoś** Pull Request

### Krok 4: Przegląd i zatwierdzenie

Przeglądamy każdą wtyczkę pod kątem:

- **Bezpieczeństwa** — brak niebezpiecznych API (zobacz [Zasady Bezpieczeństwa](#security-rules))
- **Jakości** — czy działa? Czy kod jest czysty?
- **Przydatności** — czy rozwiązuje rzeczywisty problem?

Po zatwierdzeniu:
1. Twoja wtyczka zostaje dodana do `registry.json`
2. Tworzony jest pakiet ZIP w `dist/`
3. Twoja wtyczka pojawia się w **Sklepie Wtyczek** dla wszystkich użytkowników WIA SOOM!

---

## Część 5: Najlepsze Praktyki

### Zasady Bezpieczeństwa

Te zasady są **obowiązkowe**. Wtyczki, które je naruszają, będą odrzucane.

| Zasada | Dlaczego |
|------|-----|
| **NIGDY** nie używaj `eval()` ani `new Function()` | Ryzyko wstrzykiwania kodu |
| **NIGDY** nie używaj `child_process`, `exec()`, `spawn()` | Używaj tylko `ctx.terminal.send()` do poleceń |
| **NIGDY** nie pobieraj zewnętrznych URL-i | Wyjątek: punkty końcowe API `wiasoom.com` |
| **NIGDY** nie uzyskuj dostępu do `process.env` | Zmienne środowiskowe mogą zawierać sekrety |
| **NIGDY** nie używaj `require('fs')` bezpośrednio | Używaj `ctx.settings` do przechowywania, `ctx.sftp` do transferu plików |
| **NIGDY** nie używaj zewnętrznych pakietów npm | Tylko czysty JavaScript — bez node_modules |
| **MUSISZ** używać `ctx.terminal.send()` do wszystkich zdalnych poleceń | Przechodzi to przez bezpieczny kanał SSH |
| **MUSISZ** posprzątać w `deactivate()` | Usuń nasłuchiwacze, wyczyść interwały |

### Obsługa błędów

Zawsze owijaj ryzykowne operacje w try/catch:
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
### Sprzątanie w deactivate()

Jeśli twoja wtyczka tworzy interwały, nasłuchiwacze lub subskrypcje — posprzątaj je:
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
### Wsparcie i18n

WIA SOOM obsługuje 254 języki. Aby uczynić etykietę twojej wtyczki przetłumaczalną, użyj prostego podejścia:
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

## Część 6: Przykłady z Życia

### Przykład 1: Sprawdzacz Dysku Serwera

Uruchamia `df -h` na zdalnym serwerze i pokazuje używaną/dostępną przestrzeń w pasku stanu.
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

### Przykład 2: Menedżer TODO

Wtyczka, która zarządza listą TODO, używając ustawień do trwałego przechowywania i webview do wyświetlania.

> **Wzorzec projektowy:** Ponieważ webview nie mogą bezpośrednio wywoływać API wtyczek, ta wtyczka używa podejścia "snapshot" — odczytuje TODO z ustawień, renderuje je jako HTML tylko do odczytu i zapewnia działania oparte na pasku bocznym do dodawania elementów. Webview jest warstwą **wyświetlania**, a nie interaktywnym formularzem.
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

### Przykład 3: Obserwator Błędów

Monitoruje wyjście terminala i wysyła powiadomienie, gdy wykryte zostaną określone wzorce.
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

## Dodatek: Kategorie i Ikony

### Kategorie Wtyczek (29)

Użyj ich w swoim `package.json` `keywords` lub przy zgłaszaniu do rejestru:

| Kategoria | Opis |
|-----------|------|
| `server` | Ogólne zarządzanie serwerem |
| `devtools` | Narzędzia deweloperskie |
| `calculator` | Kalkulatory i konwertery |
| `simulator` | Symulatory |
| `game` | Gry terminalowe |
| `business` | Narzędzia biznesowe |
| `security` | Bezpieczeństwo i audyt |
| `web` | Zarządzanie serwerem WWW |
| `education` | Narzędzia edukacyjne |
| `health` | Narzędzia związane ze zdrowiem |
| `islamic` | Narzędzia islamskie (czasy modlitw itp.) |
| `science` | Narzędzia naukowe |
| `quantum` | Narzędzia obliczeń kwantowych |
| `ai` | Narzędzia zasilane AI |
| `biotech` | Narzędzia biotechnologiczne |
| `space` | Narzędzia związane z przestrzenią i astronomią |
| `network` | Narzędzia sieciowe |
| `database` | Zarządzanie bazą danych |
| `monitoring` | Monitorowanie serwera |
| `devops` | DevOps i CI/CD |
| `utility` | Ogólne narzędzia użytkowe |
| `design` | Narzędzia projektowe |
| `ecommerce` | Narzędzia e-commerce |
| `automation` | Narzędzia automatyzacji |
| `kpop` | Narzędzia związane z K-popem |
| `accessibility` | Narzędzia dostępności |
| `analytics` | Analiza i raportowanie |
| `wia` | Narzędzia ekosystemu WIA |
| `all` | Pojawia się we wszystkich kategoriach |

### Rekomendowane Ikony (Lucide)

| Nazwa Ikony | Użyj do |
|-------------|---------|
| `server` | Zarządzanie serwerem |
| `shield` | Bezpieczeństwo |
| `database` | Baza danych |
| `activity` | Monitorowanie |
| `terminal` | Narzędzia terminalowe |
| `code` | Rozwój |
| `hard-drive` | Dysk/przechowywanie |
| `network` | Sieciowanie |
| `lock` | Uwierzytelnianie/szyfrowanie |
| `eye` | Obserwacja/monitorowanie |
| `check-square` | Zadania/TODO |
| `layout-dashboard` | Pulpity nawigacyjne |
| `settings` | Konfiguracja |
| `zap` | Automatyzacja |
| `globe` | Sieć/międzynarodowy |

Przeglądaj wszystkie 1,500+ ikon: [lucide.dev/icons](https://lucide.dev/icons)

---

## Potrzebujesz Pomocy?

- **Problemy na GitHubie:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Problemy z Wtyczkami:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Przykładowe Wtyczki:** [Website](https://wiasoom.com)
- **Strona Internetowa:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Stwórz coś niesamowitego. Podziel się tym ze światem.</em></p>
<p align="center"><em>— Zespół WIA SOOM</em></p>