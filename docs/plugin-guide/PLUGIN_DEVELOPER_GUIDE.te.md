<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM ప్లగిన్ డెవలపర్ గైడ్</h1>
<p align="center"><strong>మీ స్వంత ప్లగిన్‌ను 5 నిమిషాల్లో నిర్మించండి.</strong></p>
<p align="center">శక్తివంతమైన సర్వర్ టూల్స్, డాష్‌బోర్డ్స్ మరియు ఆటోమేషన్స్‌ను WIA SOOM లోనే సృష్టించండి.</p>

---

## కంటెంట్ పట్టిక

- [భాగం 1: క్విక్ స్టార్ట్ — మీ మొదటి ప్లగిన్ 5 నిమిషాల్లో](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [భాగం 2: ప్లగిన్ కాంటెక్స్ట్ API సూచిక](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [భాగం 3: వెబ్ వ్యూస్‌తో కస్టమ్ UI నిర్మించడం](#part-3-building-custom-ui-with-webviews)
- [భాగం 4: మీ ప్లగిన్‌ను ప్రచురించడం](#part-4-publishing-your-plugin)
- [భాగం 5: ఉత్తమ పద్ధతులు](#part-5-best-practices)
- [భాగం 6: వాస్తవ ప్రపంచ ఉదాహరణలు](#part-6-real-world-examples)
- [అనుబంధం: కేటగిరీలు & ఐకాన్లు](#appendix-categories--icons)

---

## భాగం 1: క్విక్ స్టార్ట్ — మీ మొదటి ప్లగిన్ 5 నిమిషాల్లో

### మీరు ఏమి నిర్మించబోతున్నారు

సైడ్‌బార్‌లో బటన్‌ను చేర్చే "హలో వరల్డ్" ప్లగిన్. క్లిక్ చేసినప్పుడు, ఇది ఒక నోటిఫికేషన్‌ను చూపిస్తుంది.

### దశ 1: ప్లగిన్ ఫోల్డర్‌ను సృష���టించండి
§§§CHUNK_SEPARATOR§§§
### దశ 2: package.json సృష్టించండి
§§§CHUNK_SEPARATOR§§§
**అవసరమైన ఫీల్డ్స్:** `name`, `version`, `description`, `author`, `main`

### దశ 3: index.js సృష్టించండి
§§§CHUNK_SEPARATOR§§§
### దశ 4: WIA SOOMను పునఃప్రారంభించండి

అప్‌ను పునఃప్రారంభించండి (లేదా సెట్టింగ్స్ → ప్లగిన్స్‌లో ప్లగిన్‌ను ఆఫ్/ఆన్ చేయండి).

మీరు సైడ్‌బార్‌లో **"హలో వరల్డ్"** బటన్‌ను చూడాలి. దానిపై క్లిక్ చేయండి — మీరు విజయ నోటిఫికేషన్‌ను చూడండి!

### ఇది ఎలా పనిచేస్తుంది
§§§CHUNK_SEPARATOR§§§
---

## భాగం 2: ప్లగిన్ కాంటెక్స్ట్ API సూచిక

మీ `activate(context)` ఫంక్షన్ కాల్ అయినప్పుడు, `context` (లేదా `ctx`) ఈ APIలను అందిస్తుంది:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — రిమోట్ సర్వర్లపై ఆదేశాలను నడపండి

#### `terminal.send(sessionId, data)`

సక్రియమైన టెర్మినల్ సెషన్‌కు ఆదేశం (లేదా ఏదైనా డేటా) పంపండి.

| పారామీటర్ | రకం | వివరణ |
|------------|------|--------|
| `sessionId` | `string` | పంపించాల్సిన టెర్మినల్ సెషన్ |
| `data` | `string` | పంపించాల్సిన ఆదేశం లేదా డేటా |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

టెర్మినల్ సెషన్ నుండి అన్ని అవుట్‌పుట్‌కు సబ్‌స్క్రైబ్ చేయండి. **అన్‌సబ్‌స్క్రైబ్ ఫంక్షన్**ను తిరిగి ఇస్తుంది.

| పారామీటర్ | రకం | వివరణ |
|------------|------|--------|
| `sessionId` | `string` | చూడాల్సిన టెర్మినల్ సెషన్ |
| `callback` | `(data: string) => void` | ప్రతి అవుట్‌పుట్ చం���తో కాల్ చేయబడుతుంది |
| **తిరిగి ఇస్తుంది** | `() => void` | వినడం ఆపడానికి దీన్ని కాల్ చేయండి |
§§§CHUNK_SEPARATOR§§§
**ముఖ్యమైనది:** ఎప్పుడూ అన్‌సబ్‌స్క్రైబ్ ఫంక్షన్‌ను సేవ్ చేయండి మరియు మెమరీ లీక్‌లను నివారించడానికి `deactivate()` లో దీన్ని కాల్ చేయండి.

---

### `ctx.sftp` — ఫైల్ బదిలీ

> **స్థితి: త్వరలో రాబోతోంది** — SFTP API నిర్వచించబడింది కానీ ఇంకా యాప్ యొక్క SFTP ఇంజిన్‌కు కేబుల్ చేయబడలేదు. `list()` ప్రస్తుతానికి ఖాళీ అరీను తిరిగి ఇస్తుంది, మరియు `upload()`/`download()` నో-ఆప్స్. ఇది భవిష్యత్తులో ఒక విడుదలలో పూర్తిగా అమలు చేయబడుతుంది. ఇప్పటి వరకు, `scp` లేదా `rsync` ఆదేశాలతో `ctx.terminal.send()` ఉపయోగించండి.

#### `sftp.list(sessionId, path)`

రిమోట్ డైరెక్టరీలో ఫైళ్లను జాబితా చేయండి.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

స్థానిక యంత్రం నుండి రిమోట్ సర్వర్‌కు ఫైల్‌ను అప్‌లోడ్ చేయండి.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

రిమోట్ సర్వర్ నుండి స్థానిక యంత్రానికి ఫైల్‌ను డౌన్‌లోడ్ చేయండి.
§§§CHUNK_SEPARATOR§§§
**పనితీరు (SFTP API ప్రత్యక్షంగా ఉండేవరకు):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — వినియోగదారు ఇంటర్ఫేస్

#### `ui.addSidebarButton(options)`

WIA SOOM సైడ్‌బార్‌కు బటన్‌ను చేర్చండి.

| ఎంపిక | రకం | అవసరం | వివరణ |
|-------|------|--------|--------|
| `id` | `string` | లేదు | ���్రత్యేక ID (ప్లగిన్ పేరుకు డిఫాల్ట్) |
| `icon` | `string` | అవశ్యం | Lucide ఐకాన్ పేరు (ఉదా: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | అవశ్యం | సైడ్‌బార్‌లో చూపించే బటన్ పాఠ్యం |
| `onClick` | `() => void` | అవశ్యం | బటన్‌పై క్లిక్ చేసినప్పుడు కాల్ చేయబడే ఫంక్షన్ |
§§§CHUNK_SEPARATOR§§§
**ఐకాన్ సూచిక:** అందుబాటులో ఉన్న అన్ని ఐకాన్లను [lucide.dev/icons](https://lucide.dev/icons) వద్ద బ్రౌజ్ చేయండి

> **సామర్థ్యం గమనిక:** కొన్ని పాత ప్లగిన్లు `addSidebarButton(id, icon, label, onClick)` వంటి స్థానిక ఆర్గ్యుమెంట్లను ఉపయోగిస్తాయి. అధికారిక API పై పేర్కొన్నట్లు **ఎంపికల ఆబ్జెక్ట్**ను ఉపయోగిస్తుంది. కొత్త ప్లగిన్ల కోసం ఎప్పుడూ ఆబ్జెక్ట్ శైలిని ఉపయోగించండి.

#### `ui.openWebview(options)`

కస్టమ్ HTML కంటెంట్‌తో పాప్-అప్ విండోను తెరవండి. ఇది మీరు ధనిక UIsని ఎలా నిర్మించాలో. 

| ఎంపిక | రకం | వివరణ |
|-------|------|--------|
| `title` | `string` | విండో శీర్షిక |
| `html` | `string` | రాండర్ చేయడానికి పూర్తి HTML కంటెంట్ |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

ఒక టోస్ట్ నోటిఫికేషన్ చూపించండి.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | నోటిఫికేషన్ శైలీ |
| `message` | `string` | చూపించాల్సిన పాఠ్యం |
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

కింద ఉన్న స్థితి పట్టికకు ఒక శాశ్వత పాఠ్య అంశాన్ని జోడించండి.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | ఈ స్థితి అంశానికి ప్రత్యేక ID |
| `text` | `string` | ప్రదర్శించాల్సిన పాఠ్యం |
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

### `ctx.settings` — శాశ్వత నిల్వ

ప్లగిన్ సెట్టింగ్స్ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` లో శాశ్వతంగా నిల్వ చేయబడతాయి.

#### `settings.get(key)`

సేవ్ చేయ���డిన విలువను చదవండి.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
కీ ఉనికిలో లేకపోతే `undefined` ను తిరిగి ఇస్తుంది.

#### `settings.set(key, value)`

ఒక విలువను సేవ్ చేయండి. స్ట్రింగ్స్, సంఖ్యలు, బూలియన్లు, అరిజ్ మరియు ఆబ్జెక్టులను మద్దతు ఇస్తుంది.
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**ఉదాహరణ: వినియోగదారుల అభిరుచులను గుర్తుంచుకోండి**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — AI సమీకరణ

> **స్థితి: త్వరలో రాబోతోంది** — AI API నిర్వచించబడింది కానీ ఇంకా Soomy కు కనెక్ట్ చేయబడలేదు. ప్రస్తుతానికి `{ response: 'AI not yet connected' }` ను తిరిగి ఇస్తుంది. పూర్తి AI సమీకరణ భవిష్యత్తులో విడుదలకు ప్రణాళిక చేయబడింది.

#### `ai.chat(messages, options?)`

AI సహాయకుడికి (Soomy) సందేశాలను పంపండి.
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

## Part 3: Webviews తో కస్టమ్ UI నిర్మించడం

`openWebview()` API మీకు HTML, CSS, మరియు JavaScript తో డాష్‌బోర్డ్ UIs ని నిర్మించడానికి అనుమతిస్తుంది — ఇవన్నీ ఒక పాప్-అప్ విండోలో.

> **ముఖ్యమైన పరిమితి:** Webviews **ప్రదర్శన మాత్రమే**. అవి ప్లగిన్ APIs (`ctx.settings`, `ctx.terminal`, మొదలైనవి) లోకి తిరిగి కాల్ చేయలేవు. అన్ని వినియోగదారుల చర్యల కోసం సైడ్‌బార్ బటన్లను ఉపయోగించండి, మరియు ప్రస్తుత స్థితిని చూపించడానికి `openWebview()` ను ఉపయోగించండి. మీరు పరస్పర లక్షణాలను అవసరం అయితే, వాటిని ��ైడ్‌బార్ బటన్ల నుండి ప్రారంభించండి మరియు ప్రదర్శనను నవీకరించడానికి వెబ్‌వ్యూ ను మళ్లీ తెరవండి.

### నమూనా: టెర్మినల్ ఆదేశం → అవుట్‌పుట్ పార్స్ చేయడం → HTML లో చూపించడం

ఇది అత్యంత సాధారణ ప్లగిన్ నమూనా. మీరు ఒక ఆదేశాన్ని నడుపుతారు, ఫలితాన్ని పార్స్ చేస్తారు, మరియు దాన్ని విజువల్‌గా ప్రదర్శిస్తారు.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### నమూనా: ఆటో-రిఫ్రెష్ తో పరస్పర డాష్‌బోర్డ్
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### నమూనా: వెబ్‌వ్యూ లో సెట్టింగ్స్ ప్రదర్శించడం

> **గమనిక:** Webviews ప్రదర్శన మాత్రమే — అవి ప్లగిన్ APIs లోకి తిరిగి కాల్ చేయలేవు. సెట్టింగ్స్ ను మార్చడానికి మ��� సైడ్‌బార్ బటన్ హ్యాండ్లర్‌లలో `ctx.settings` ను ఉపయోగించండి, మరియు ప్రస్తుత స్థితిని చూపించడానికి `openWebview()` ను ఉపయోగించండి.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Part 4: మీ ప్లగిన్ ప్రచురించడం

### దశ 1: స్థానికంగా పరీక్షించండి

1. మీ ప్లగిన్ ను `~/.wia-soom/plugins/{your-plugin}/` కు కాపీ చేయండి
2. WIA SOOM ను పునఃప్రారంభించండి
3. ఇది పనిచేస్తుందా అని నిర్ధారించండి: సైడ్‌బార్ బటన్ కనిపిస్తుంది, లక్షణాలు సరిగ్గా పనిచేస్తాయి
4. ఎడ్జ్ కేసులను పరీక్షించండి: టెర్మినల్ కనెక్ట్ చేయబడకపోతే ఏమి జరుగుతుంది?

### దశ 2: సమర్పణ కోసం సిద్ధం చేయండి

మీ ప్లగిన్ ఫోల్డర్ లో ఉండాలి:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**అవసరమైన `package.json` ఫీల్డ్స్:**

| ఫీల్డ్ | వివరణ | ఉదాహరణ |
|-------|-------------|---------|
| `name` | ప్రత్యేక కebab-case ID | `"my-awesome-plugin"` |
| `version` | సేమాంటిక్ వెర్షన్ | `"1.0.0"` |
| `description` | ఒక వాక్య వివరణ | `"Monitors nginx access logs in real-time"` |
| `author` | మీ పేరు | `"John Doe"` |
| `main` | ప్రవేశ బిందువు | `"index.js"` |

**ఐచ్ఛిక ఫీల్డ్స్:**

| ఫీల్డ్ | వివరణ |
|-------|-------------|
| `license` | లైసెన్స్ రకం (MIT సిఫారసు) |
| `keywords` | శోధన ట్యాగ్‌ల యొక్క అర్రే |
| `soom.minVersion` | అవసరమైన కనిష్ట WIA SOOM వెర్షన్ |

### దశ 3: ప్లగిన్ రిజిస్ట్రీలో సమర్పించండి

1. ****Package** your plugin as a ZIP file
2. **Add** మీ ప్లగిన్‌ను `plugins/{your-plugin-name}/` లో
3. **Submit** ఒక Pull Request

### దశ 4: సమీక్ష మరియు ఆమోదం

మేము ప్రతి ప్లగిన్‌ను ఈ విషయాల కోసం సమీక్షిస్తాము:

- **భద్రత** — ప్రమాదకరమైన APIs లేవు (చూడండి [భద్రత నియమాలు](#security-rules))
- **నాణ్యత** — ఇది పనిచేస్తుందా? కోడ్ శుభ్రంగా ఉందా?
- **ఉపయోగం** — ఇది నిజమైన సమస్యను పరిష్కరిస్తుందా?

ఆమోదం తర్వాత:
1. మీ ప్లగిన్ `registry.json` లో చేర్చబడుతుంది
2. `dist/` లో ఒక ZIP బండిల్ సృష్టించబడుతుంది
3. మీ ప్లగిన్ అన్ని WIA SOOM వినియోగదారుల కోసం **Plugin Store** లో కనిపిస్తుంది!

---

## భాగం 5: ఉత్తమ పద్ధతులు

### భద్రత నియమాలు

ఈ నియమాలు **అవసరమైనవి**. వీటిని ఉల్లంఘించే ప్లగిన్లను తిరస్కరించబడతాయి.

| ���ియమం | ఎందుకు |
|------|-----|
| **ఎప్పుడూ** `eval()` లేదా `new Function()` ఉపయోగించకండి | కోడ్ ఇంజెక్షన్ ప్రమాదం |
| **ఎప్పుడూ** `child_process`, `exec()`, `spawn()` ఉపయోగించకండి | కమాండ్ల కోసం కేవలం `ctx.terminal.send()` ఉపయోగించండి |
| **ఎప్పుడూ** బాహ్య URLs ను పొందవద్దు | మినహాయింపు: `wiasoom.com` API ఎండ్ పాయింట్లు |
| **ఎప్పుడూ** `process.env` ను యాక్సెస్ చేయవద్దు | పర్యావరణ చరాలు రహస్యాలను కలిగి ఉండవచ్చు |
| **ఎప్పుడూ** `require('fs')` ను నేరుగా ఉపయోగించకండి | నిల్వ కోసం `ctx.settings` ను, ఫైల్ బదిలీకి `ctx.sftp` ను ఉపయోగించండి |
| **ఎప్పుడూ** npm బాహ్య ప్యాకేజీలను ఉపయోగించకండి | కచ్చితమైన JavaScript మాత్రమే — node_modules లేదు |
| **కచ్చితంగా** అన్ని ��ూర కమాండ్ల కోసం `ctx.terminal.send()` ఉపయోగించాలి | ఇది భద్రతా SSH చానల్ ద్వారా వెళ్ళుతుంది |
| **కచ్చితంగా** `deactivate()` లో శుభ్రం చేయాలి | శ్రోతలను తొలగించండి, అంతరాలు క్లియర్ చేయండి |

### లోపాల నిర్వహణ

ఎప్పుడూ ప్రమాదకరమైన కార్యకలాపాలను try/catch లో చుట్టండి:
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
### deactivate() లో శుభ్రత

మీ ప్లగిన్ అంతరాలు, శ్రోతలు లేదా సభ్యత్వాలను సృష్టిస్తే — వాటిని శుభ్రం చేయండి:
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
### i18n మద్దతు

WIA SOOM 254 భాషలను మద్దతు ఇస్తుంది. మీ ప్లగిన్ లేబుల్ అనువదించదగినదిగా చేయడానికి, ఒక సరళమైన విధానాన్ని ఉపయోగించండి:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

## భాగం 6: వాస్తవ ప్రపంచ ఉదాహరణలు

### ఉదాహరణ 1: సర్వర్ డిస్క్ చెకర్

దూర సర్వర్‌లో `df -h` ను నడుపుతుంది మరియు స్థితి ప్యానెల్‌లో ఉపయోగించిన/లభ్యమైన స్థలాన్ని చూపిస్తుంది.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### ఉదాహరణ 2: TODO మేనేజర్

ఒక TODO జాబితాను నిర్వహించే ప్లగిన్, స్థిరమైన నిల్వ కోసం సెట్టింగ్స్ మరియు ప్రదర్శన కోసం వెబ్‌వ్యూ ఉపయోగిస్తుంది.

> **డిజైన్ నమూనా:** వెబ్‌వ్యూస్ నేరుగా ప్లగిన్ APIs ను కాల్ చేయలేకపోతే, ఈ ప్లగిన్ "స్నాప్‌షాట్" విధానాన్ని ఉపయోగిస్తుంది — ఇది సెట్టింగ్స్ నుండి TODOలను చదువుతుంది, వాటిని చదవడానికి మాత్రమే HTML గా రూపొందిస్తుంది మరియు అం��ాలను చేర్చడానికి పక్కన ఆధారిత చర్యలను అందిస్తుంది. వెబ్‌వ్యూ ఒక **ప్రదర్శన** పొర, పరస్పర రూపం కాదు.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

### ఉదాహరణ 3: లోపాల పర్యవేక్షకుడు

టర్మినల్ అవుట్‌పుట్‌ను పర్యవేక్షించి, ప్రత్యేక నమూనాలు గుర్తించినప్పుడు ఒక నోటిఫికేషన్ పంపిస్తుంది.
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## అనుబంధం: వర్గాలు & చిహ్నాలు

### ప్లగిన్ వర్గాలు (29)

ఇవి మీ `package.json` `keywords` లో లేదా రిజిస్ట్రీలో సమర్పించేటప్పుడు ఉపయోగించండి:

| వర్గం | వివరణ |
|----------|-------------|
| `server` | సాధారణ సర్వర్ నిర్వహణ |
| `devtools` | అభివృద్ధి సాధనాలు |
| `calculator` | లెక్కింపులు మరియు మార్పిడి సాధనాలు |
| `simulator` | అనుకరణలు |
| `game` | టర్మినల్ ఆటలు |
| `business` | వ్యాపార సాధనాలు |
| `security` | భద్రత మరియు ఆడిటింగ్ |
| `web` | వెబ్ సర్వర్ నిర్వహణ |
| `education` | విద్యా సాధనాలు |
| `health` | ఆరోగ్య సంబంధిత సాధనాలు |
| `islamic` | ఇస్లామిక్ సాధనాలు (ప్రార్థన సమయాలు, మొదలైనవి) |
| `science` | శాస్త్రీయ సాధ��ాలు |
| `quantum` | క్వాంటమ్ కంప్యూటింగ్ సాధనాలు |
| `ai` | AI ఆధారిత సాధనాలు |
| `biotech` | బయోటెక్నాలజీ సాధనాలు |
| `space` | అంతరిక్ష మరియు ఖగోళ శాస్త్ర సాధనాలు |
| `network` | నెట్‌వర్క్ సాధనాలు |
| `database` | డేటాబేస్ నిర్వహణ |
| `monitoring` | సర్వర్ మానిటరింగ్ |
| `devops` | DevOps మరియు CI/CD |
| `utility` | సాధారణ ఉపయుక్తతలు |
| `design` | డిజైన్ సాధనాలు |
| `ecommerce` | ఈ-కామర్స్ సాధనాలు |
| `automation` | ఆటోమేషన్ సాధనాలు |
| `kpop` | K-pop సంబంధిత సాధనాలు |
| `accessibility` | యాక్సెస్ibilit సాధనాలు |
| `analytics` | విశ్లేషణ మరియు నివేదికలు |
| `wia` | WIA పర్యావరణ సాధనాలు |
| `all` | అన్ని వర్గాలలో కనిపిస్తుంది |

### సిఫారసు చేసిన చిహ్నాలు (Lucide)

| చిహ్నం పేరు | ఉపయోగించండి |
|-----------|---------|
| `server` | సర్వర్ నిర్వహణ |
| `shield` | భద్రత |
| `database` | డేటాబేస్ |
| `activity` | మానిటరింగ్ |
| `terminal` | టర్మినల్ సాధనాలు |
| `code` | అభివృద్ధి |
| `hard-drive` | డిస్క్/స్టోరేజ్ |
| `network` | నెట్‌వర్కింగ్ |
| `lock` | ఆథ్/ఎన్‌క్రిప్షన్ |
| `eye` | గమనించడం/మానిటరింగ్ |
| `check-square` | పనులు/TODO |
| `layout-dashboard` | డాష్‌బోర్డులు |
| `settings` | కాన్ఫిగరేషన్ |
| `zap` | ఆటోమేషన్ |
| `globe` | వెబ్/అంతర్జాతీయ |

అన్ని 1,500+ చిహ్నాలను బ్రౌజ్ చేయండి: [lucide.dev/icons](https://lucide.dev/icons)

---

## సహాయం అవసరమా?

- **GitHub ఇష్యూస్:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ప్లగిన్ ఇష్యూస్:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ఉదాహరణ ప్లగిన్లు:** [Website](https://wiasoom.com)
- **వెబ్‌సైట్:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>అద్భుతమైనది నిర్మించండి. ప్రపంచంతో పంచుకోండి.</em></p>
<p align="center"><em>— WIA SOOM టీం</em></p>
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
