<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Guia di Desenvolvedor di Plugin di WIA SOOM</h1>
<p align="center"><strong>Konstruí se próprio plugin na 5 minutos.</strong></p>
<p align="center">Krea ferraméntas di servidor, dashboards, i automações poderosas — dretu dentru di WIA SOOM.</p>

---

## Tabela di Conteúdo

- [Parte 1: Início Rápido — Seu Primeiro Plugin na 5 Minutos](#parte-1-início-rápido--seu-primeiro-plugin-na-5-minutos)
- [Parte 2: Referência di API di Contexto di Plugin](#parte-2-referência-di-api-di-contexto-di-plugin)
  - [ctx.terminal](#ctxterminal--executa-comandos-em-servidores-remotos)
  - [ctx.sftp](#ctxsftp--transferência-di-arquivos)
  - [ctx.ui](#ctxui--interface-di-usuário)
  - [ctx.settings](#ctxsettings--armazenamento-persistente)
  - [ctx.ai](#ctxai--integração-di-ai)
- [Parte 3: Konstruí UI Personalizada ku Webviews](#parte-3-konstruí-ui-personalizada-ku-webviews)
- [Parte 4: Publiká Se Plugin](#parte-4-publiká-se-plugin)
- [Parte 5: Melhores Práticas](#parte-5-melhores-práticas)
- [Parte 6: Exemplos di Mundo Real](#parte-6-exemplos-di-mundo-real)
- [Apêndice: Categorias & Ícones](#apêndice-categorias--ícones)

---

## Parte 1: Início Rápido — Seu Primeiro Plugin na 5 Minutos

### Kusa ki bo ta konstruí

Um plugin "Hello World" ki adiciona um botão na barra lateral. Kuando clica, ele mostra uma notificação.

### Passo 1: Krea a pasta di plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### Passo 2: Krea package.json
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
**Campos obrigatórios:** `name`, `version`, `description`, `author`, `main`

### Passo 3: Krea index.js
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
### Passo 4: Reinicia WIA SOOM

Reinicia o app (ou alterna o plugin fora/ligado em Configurações → Plugins).

Bo deve vê um **"Hello World"** botão na barra lateral. Clica nele — bo vai vê uma notificação di sucesso!

### Kusa ki ta funciona
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

## Parte 2: Referência di API di Contexto di Plugin

Kuando a função `activate(context)` é chamada, `context` (ou `ctx`) ta providencia es APIs:
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

### `ctx.terminal` — Executa comandos em servidores remotos

#### `terminal.send(sessionId, data)`

Manda um comando (ou qualquer dado) para um sessão di terminal ativa.

| Parâmetro | Tipo | Descrição |
|-----------|------|-------------|
| `sessionId` | `string` | A sessão di terminal pa manda |
| `data` | `string` | O comando ou dado pa manda |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

Inscreve-se pa tudu saída di um sessão di terminal. Retorna uma **função di desinscrição**.

| Parâmetro | Tipo | Descrição |
|-----------|------|-------------|
| `sessionId` | `string` | A sessão di terminal pa observa |
| `callback` | `(data: string) => void` | Chamado ku cada pedaço di saída |
| **Retorna** | `() => void` | Chama es pa para di escuta |
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
**Importante:** Sempri guarda a função di desinscrição i chama ela na `deactivate()` pa preveni vazamentos di memória.

---

### `ctx.sftp` — Transferência di arquivos

> **Status: Vindo Brevemente** — A API di SFTP é definida mas ainda não está conectada ku motor di SFTP di app. `list()` atualmente retorna um array vazio, i `upload()`/`download()` são no-ops. Es vai ser totalmente implementado na um futura versão. Pa agora, usa `ctx.terminal.send()` ku comandos `scp` ou `rsync` como uma solução alternativa.

#### `sftp.list(sessionId, path)`

Lista arquivos em um diretório remoto.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

Faz upload di um arquivo di máquina local pa servidor remoto.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

Faz download di um arquivo di servidor remoto pa máquina local.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Solução alternativa (até a API di SFTP estar ativa):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — Interface di usuário

#### `ui.addSidebarButton(options)`

Adiciona um botão na barra lateral di WIA SOOM.

| Opção | Tipo | Obrigatório | Descrição |
|--------|------|----------|-------------|
| `id` | `string` | Não | ID único (padrão é o nome di plugin) |
| `icon` | `string` | Sim | Nome di ícone di Lucide (ex., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Sim | Texto di botão mostrado na barra lateral |
| `onClick` | `() => void` | Sim | Função chamada kuando botão é clicado |
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
**Referência di ícone:** Navega tudu ícones disponíveis em [lucide.dev/icons](https://lucide.dev/icons)

> **Nota di compatibilidade:** Algus plugins mais antigos usa argumentos posicionais como `addSidebarButton(id, icon, label, onClick)`. A API oficial usa um **objeto di opções** como documentado acima. Sempri usa o estilo di objeto pa novos plugins.

#### `ui.openWebview(options)`

Abre uma janela pop-up ku conteúdo HTML personalizado. Es é como bo konstruí UIs ricas.

| Opção | Tipo | Descrição |
|--------|------|-------------|
| `title` | `string` | Títulu di janela |
| `html` | `string` | Conteúdo HTML completo pa renderizar |
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
> Vê [Parte 3](#part-3-building-custom-ui-with-webviews) pa padrões avansadu di webview.

#### `ui.showNotification(type, message)`

Mostra un notifikason di toast.

| Parâmetro | Tipo | Descrição |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Estilo di notifikason |
| `message` | `string` | Texto pa mostra |
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
#### `ui.addStatusBarItem(id, text)`

Adiciona un item di texto persistente na barra di status di baixo.

| Parâmetro | Tipo | Descrição |
|-----------|------|-------------|
| `id` | `string` | ID úniku pa es item di status |
| `text` | `string` | Texto pa exibi |
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### `ctx.settings` — Armazenamento persistente

As configurações di plugin são armazenadas permanentemente em `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Lê un valor guardadu.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
Retorna `undefined` se a chave não existe.

#### `settings.set(key, value)`

Guarda un valor. Suporta strings, números, booleanos, arrays, e objetos.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
**Exemplo: Lembrar preferências di usuário**
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

### `ctx.ai` — Integração di IA

> **Status: Vindo Brevemente** — A API di IA está definida mas ainda não está conectada a Soomy. Atualmente retorna `{ response: 'AI not yet connected' }`. Integração completa di IA está planejada pa un lançamento futuro.

#### `ai.chat(messages, options?)`

Envia mensagens pa assistente di IA (Soomy).
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

## Parte 3: Construindo UI Personalizada com Webviews

A API `openWebview()` permite que você construa UIs di dashboard com HTML, CSS, e JavaScript — tudo dentro di un janela pop-up.

> **Limitação importante:** Webviews são **apenas para exibição**. Elas não podem chamar de volta as APIs di plugin (`ctx.settings`, `ctx.terminal`, etc.). Use botões na barra lateral pa todas as ações di usuário, e use `openWebview()` pa mostrar o estado atual. Se você precisar di recursos interativos, ative-os a partir di botões na barra lateral e reabra o webview pa atualizar a exibição.

### Padrão: Comando di Terminal → Analisar Saída → Mostrar em HTML

Este é o padrão di plugin mais comum. Você executa um comando, analisa o resultado, e exibe visualmente.
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
### Padrão: Dashboard Interativo com Auto-Atualização
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
### Padrão: Exibindo Configurações em um Webview

> **Nota:** Webviews são apenas para exibição — elas não podem chamar de volta as APIs di plugin. Use `ctx.settings` nos manipuladores di botões na barra lateral pa modificar configurações, e use `openWebview()` pa mostrar o estado atual.
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

## Parte 4: Publicando Seu Plugin

### Passo 1: Testar localmente

1. Copie seu plugin para `~/.wia-soom/plugins/{your-plugin}/`
2. Reinicie WIA SOOM
3. Verifique se funciona: botão na barra lateral aparece, recursos funcionam corretamente
4. Teste casos extremos: o que acontece se nenhum terminal estiver conectado?

### Passo 2: Preparar para submissão

Sua pasta di plugin deve conter:
```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```
**Requisitos `package.json` campos:**

| Campo | Descrição | Exemplo |
|-------|-------------|---------|
| `name` | ID único em kebab-case | `"my-awesome-plugin"` |
| `version` | Versão semântica | `"1.0.0"` |
| `description` | Descrição em uma linha | `"Monitors nginx access logs in real-time"` |
| `author` | Seu nome | `"John Doe"` |
| `main` | Ponto de entrada | `"index.js"` |

**Campos opcionais:**

| Campo | Descrição |
|-------|-------------|
| `license` | Tipo de licença (MIT recomendado) |
| `keywords` | Array de tags de busca |
| `soom.minVersion` | Versão mínima do WIA SOOM requerida |

### Passo 3: Submeter ao Registro de Plugins

1. ****Package** your plugin as a ZIP file
2. **Adicionar** seu plugin a `plugins/{seu-nome-do-plugin}/`
3. **Submeter** um Pull Request

### Passo 4: Revisão e aprovação

Nós revisamos cada plugin para:

- **Segurança** — sem APIs perigosas (veja [Regras de Segurança](#security-rules))
- **Qualidade** — funciona? O código é limpo?
- **Utilidade** — resolve um problema real?

Após aprovação:
1. Seu plugin é adicionado a `registry.json`
2. Um pacote ZIP é criado em `dist/`
3. Seu plugin aparece na **Plugin Store** para todos os usuários do WIA SOOM!

---

## Parte 5: Melhores Práticas

### Regras de Segurança

Essas regras são **obrigatórias**. Plugins que as violarem serão rejeitados.

| Regra | Por quê |
|------|-----|
| **NUNCA** use `eval()` ou `new Function()` | Risco de injeção de código |
| **NUNCA** use `child_process`, `exec()`, `spawn()` | Use apenas `ctx.terminal.send()` para comandos |
| **NUNCA** busque URLs externas | Exceção: endpoints da API `wiasoom.com` |
| **NUNCA** acesse `process.env` | Variáveis de ambiente podem conter segredos |
| **NUNCA** use `require('fs')` diretamente | Use `ctx.settings` para armazenamento, `ctx.sftp` para transferência de arquivos |
| **NUNCA** use pacotes externos npm | Apenas JavaScript puro — sem node_modules |
| **DEVE** usar `ctx.terminal.send()` para todos os comandos remotos | Isso passa pelo canal SSH seguro |
| **DEVE** limpar em `deactivate()` | Remover ouvintes, limpar intervalos |

### Tratamento de Erros

Sempre envolva operações arriscadas em try/catch:
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
### Limpeza em deactivate()

Se seu plugin cria intervalos, ouvintes ou assinaturas — limpe-os:
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
### Suporte a i18n

WIA SOOM suporta 254 idiomas. Para tornar o rótulo do seu plugin traduzível, use uma abordagem simples:
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

## Parte 6: Exemplos do Mundo Real

### Exemplo 1: Verificador de Disco do Servidor

Executa `df -h` no servidor remoto e mostra o espaço usado/disponível na barra de status.
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

### Exemplo 2: Gerenciador de TODO

Um plugin que gerencia uma lista de TODO usando configurações para armazenamento persistente e uma webview para exibição.

> **Padrão de design:** Como as webviews não podem chamar diretamente as APIs do plugin, este plugin usa uma abordagem de "snapshot" — ele lê os TODOs das configurações, renderiza-os como HTML somente leitura e fornece ações baseadas na barra lateral para adicionar itens. A webview é uma camada de **exibição**, não um formulário interativo.
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

### Exemplo 3: Monitor de Erros

Monitora a saída do terminal e envia uma notificação quando padrões específicos são detectados.
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

## Apêndice: Kategorias & Ícones

### Kategorias di Plugin (29)

Usa es na se `package.json` `keywords` ou kuando ta submeti na registry:

| Kategoria | Descrição |
|-----------|-----------|
| `server` | Gerensia di server general |
| `devtools` | Ferramentas di desenvolvimento |
| `calculator` | Calculadoras e conversores |
| `simulator` | Simuladores |
| `game` | Jogos di terminal |
| `business` | Ferramentas di negócios |
| `security` | Segurança e auditoria |
| `web` | Gerensia di servidor web |
| `education` | Ferramentas di educação |
| `health` | Ferramentas relasionadu ku saúde |
| `islamic` | Ferramentas islâmicas (horários di oração, etc.) |
| `science` | Ferramentas científicas |
| `quantum` | Ferramentas di computação quântica |
| `ai` | Ferramentas alimentadu pa AI |
| `biotech` | Ferramentas di biotecnologia |
| `space` | Ferramentas di espaço e astronomia |
| `network` | Ferramentas di rede |
| `database` | Gerensia di base di dados |
| `monitoring` | Monitorização di servidor |
| `devops` | DevOps e CI/CD |
| `utility` | Utilidades gerais |
| `design` | Ferramentas di design |
| `ecommerce` | Ferramentas di e-commerce |
| `automation` | Ferramentas di automação |
| `kpop` | Ferramentas relasionadu ku K-pop |
| `accessibility` | Ferramentas di acessibilidade |
| `analytics` | Análise e relatórios |
| `wia` | Ferramentas di ecossistema WIA |
| `all` | Aparece na tudu as kategorias |

### Ícones Recomendadu (Lucide)

| Nome di Ícone | Usa pa |
|---------------|--------|
| `server` | Gerensia di servidor |
| `shield` | Segurança |
| `database` | Base di dados |
| `activity` | Monitorização |
| `terminal` | Ferramentas di terminal |
| `code` | Desenvolvimento |
| `hard-drive` | Disco/armazenamento |
| `network` | Rede |
| `lock` | Autenticação/encriptação |
| `eye` | Observando/monitorizando |
| `check-square` | Tarefas/TODO |
| `layout-dashboard` | Painéis di controle |
| `settings` | Configuração |
| `zap` | Automação |
| `globe` | Web/internacional |

Nha tudu 1,500+ ícones: [lucide.dev/icons](https://lucide.dev/icons)

---

## Precisa di Ajuda?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Exemplos di Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Construi algo incrível. Partilha ku mundu.</em></p>
<p align="center"><em>— A Equipa di WIA SOOM</em></p>