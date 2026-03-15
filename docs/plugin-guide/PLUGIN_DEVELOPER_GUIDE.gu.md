<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM પ્લગિન ડેવલપર માર્ગદર્શિકા</h1>
<p align="center"><strong>5 મિનિટમાં તમારું પોતાનું પ્લગિન બનાવો.</strong></p>
<p align="center">શક્તિશાળી સર્વર ટૂલ્સ, ડેશબોર્ડ અને ઓટોમેશન બનાવો — સીધા WIA SOOMમાં.</p>

---

## સામગ્રીની યાદી

- [ભાગ 1: ઝડપી શરૂઆત — 5 મિનિટમાં તમારું પ્રથમ પ્લગિન](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ભાગ 2: પ્લગિન કોન્ટેક્સ્ટ API સંદર્ભ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ભાગ 3: વેબવ્યુઝ સાથે કસ્ટમ UI બનાવવું](#part-3-building-custom-ui-with-webviews)
- [ભાગ 4: તમારું પ્લગિન પ્રકાશિત કરવું](#part-4-publishing-your-plugin)
- [ભાગ 5: શ્રેષ્ઠ પ્રથાઓ](#part-5-best-practices)
- [ભાગ 6: વાસ્તવિક ઉદાહરણો](#part-6-real-world-examples)
- [પરિશિષ્ટ: કેટેગરીઝ અને આઇકોન્સ](#appendix-categories--icons)

---

## ભાગ 1: ઝડપી શરૂઆત — 5 મિનિટમાં તમારું પ્રથમ પ્લગિન

### તમે શું બનાવશો

એક "હેલો વર્લ્ડ" પ્લગિન જે સાઇડબારમાં એક બટન ઉમેરે છે. જ્યારે ક્લિક કરવામાં આવે છે, ત્યારે તે એક નોટિફિકેશન દર્શાવે છે.

### પગલું 1: પ્લગિન ફોલ્ડર બનાવો
§§§CHUNK_SEPARATOR§§§
### પગલું 2: package.json બનાવો
§§§CHUNK_SEPARATOR§§§
**આવશ્યક ક્ષેત્રો:** `name`, `version`, `description`, `author`, `main`

### પગલું 3: index.js બનાવો
§§§CHUNK_SEPARATOR§§§
### પગલું 4: WIA SOOM ફરી શરૂ કરો

એપ્લિકેશન ફરી શરૂ કરો (અથવા સેટિંગ્સ → પ્લગિનમાં પ્લગિનને બંધ/ચાલુ કરો).

તમે સાઇડબારમાં **"હેલો વર્લ્ડ"** બટન જોઈ શકો છો. તેને ક્લિક કરો — તમને સફળતા નોટિફિકેશન દેખાશે!

### તે કેવી રીતે કાર્ય કરે છે
§§§CHUNK_SEPARATOR§§§
---

## ભાગ 2: પ્લગિન કોન્ટેક્સ્ટ API સંદર્ભ

જ્યારે તમારી `activate(context)` ફંક્શનને બોલાવવામાં આવે છે, ત્યારે `context` (અથવા `ctx`) આ API પ્રદાન કરે છે:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — રીમોટ સર્વર્સ પર આદેશો ચલાવો

#### `terminal.send(sessionId, data)`

એક સક્રિય ટર્મિનલ સત્રમાં આદેશ (અથવા કોઈપણ ડેટા) મોકલો.

| પેરામીટર | પ્રકાર | વર્ણન |
|-----------|------|-------------|
| `sessionId` | `string` | મોકલવા માટેનો ટર્મિનલ સત્ર |
| `data` | `string` | મોકલવા માટેનો આદેશ અથવા ડેટા |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

એક ટર્મિનલ સત્રમાંથી તમામ આઉટપુટ માટે સબસ્ક્રાઇબ કરો. એક **અનસબસ્ક્રાઇબ ફંક્શન** પરત આપે છે.

| પેરામીટર | પ્રકાર | વર્ણન |
|-----------|------|-------------|
| `sessionId` | `string` | જોવાનું ટર્મિનલ સત્ર |
| `callback` | `(data: string) => void` | દરેક આઉટપુટના ટુકડાને સાથે બોલાવવામાં આવે છે |
| **પરત આપે છે** | `() => void` | સાંભળવાનું બંધ કરવા માટે આને બોલાવો |
§§§CHUNK_SEPARATOR§§§
**મહત્વપૂર્ણ:** હંમેશા અનસબસ્ક્રાઇબ ફંક્શન સાચવો અને મેમરી લીકને રોકવા માટે `deactivate()`માં તેને બોલાવ��.

---

### `ctx.sftp` — ફાઇલ ટ્રાન્સફર

> **સ્થિતિ: જલ્દી જ આવવા માટે** — SFTP API વ્યાખ્યાયિત છે પરંતુ હજુ સુધી એપ્લિકેશનના SFTP એન્જિન સાથે જોડાયેલ નથી. `list()` હાલમાં ખાલી એરે પરત આપે છે, અને `upload()`/`download()` કોઈ કાર્ય નથી. આ ભવિષ્યના પ્રકાશનમાં સંપૂર્ણ રીતે અમલમાં આવશે. હાલમાં, `scp` અથવા `rsync` આદેશો સાથે કામ કરવા માટે `ctx.terminal.send()` નો ઉપયોગ કરો.

#### `sftp.list(sessionId, path)`

રીમોટ ડિરેક્ટરીમાં ફાઇલોની યાદી બનાવો.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

સ્થાનિક મશીનથી રીમોટ સર્વર પર ફાઇલ અપલોડ કરો.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

રીમોટ સર્વરથી સ્થાનિક મશીન પર ફાઇલ ડાઉ���લોડ કરો.
§§§CHUNK_SEPARATOR§§§
**કામ કરવાની રીત (SFTP API જીવંત થાય ત્યાં સુધી):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — વપરાશકર્તા ઇન્ટરફેસ

#### `ui.addSidebarButton(options)`

WIA SOOM સાઇડબારમાં એક બટન ઉમેરો.

| વિકલ્પ | પ્રકાર | આવશ્યક | વર્ણન |
|--------|------|----------|-------------|
| `id` | `string` | નહીં | અનન્ય ID (ડિફોલ્ટ પ્લગિન નામ) |
| `icon` | `string` | હા | Lucide આઇકન નામ (ઉદાહરણ તરીકે, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | હા | સાઇડબારમાં દર્શાવાતી બટન ટેક્સ્ટ |
| `onClick` | `() => void` | હા | જ્યારે બટન ક્લિક થાય ત્યારે બોલાવવામાં આવતી ફંક્શન |
§§§CHUNK_SEPARATOR§§§
**આઇકન સંદર્ભ:** ઉપલબ્ધ તમામ આઇકોન્સને જુઓ [lucide.dev/icons](https://lucide.dev/icons)

> **સંગતતા નોંધ:** કેટલાક જૂના પ્લગિન પોઝિશનલ આર્ગ્યુમેન્ટ્સનો ઉપયોગ કરે છે જેમ કે `addSidebarButton(id, icon, label, onClick)`. સત્તાવાર API ઉપર દર્શાવેલા **વિકલ્પો ઓબ્જેક્ટ**નો ઉપયોગ કરે છે. નવા પ્લગિન માટે હંમેશા ઓબ્જેક્ટ શૈલીનો ઉપયોગ કરો.

#### `ui.openWebview(options)`

કસ્ટમ HTML સામગ્રી સાથે એક પોપઅપ વિન્ડો ખોલો. આ રીતે તમે સમૃદ્ધ UIs બનાવો છો.

| વિકલ્પ | પ્રકાર | વર્ણન |
|--------|------|-------------|
| `title` | `string` | વિન્ડોનું શીર્ષક |
| `html` | `string` | રેન્ડર કરવા માટે સંપૂર્ણ HTML સામગ્રી |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> [ભાગ 3](#part-3-building-custom-ui-with-webviews) માં અદ્યતન વેબવ્યૂ પેટર્ન માટે જુઓ.

#### `ui.showNotification(type, message)`

ટોસ્ટ સૂચના બતાવો.

| પેરામીટર | પ્રકાર | વર્ણન |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | સૂચના શૈલી |
| `message` | `string` | બતાવવા માટેનો ટેક્સ્ટ |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

નીચલા સ્થિતિ બારમાં એક સ્થાયી ટેક્સ્ટ આઇટમ ઉમેરો.

| પેરામીટર | પ્રકાર | વર્ણન |
|-----------|------|-------------|
| `id` | `string` | આ સ્થિતિ આઇટમ માટે અનન્ય ID |
| `text` | `string` | દર્શાવવા માટેનો ટેક્સ્ટ |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — સ્થાયી ���ંગ્રહ

પ્લગિન સેટિંગ્સને `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` માં કાયમ માટે સંગ્રહિત કરવામાં આવે છે.

#### `settings.get(key)`

સંગ્રહિત મૂલ્ય વાંચો.
§§§CHUNK_SEPARATOR§§§
જો કી અસ્તિત્વમાં નથી, તો `undefined` પાછું આપે.

#### `settings.set(key, value)`

એક મૂલ્ય સાચવો. સ્ટ્રિંગ્સ, સંખ્યાઓ, બુલિયન, એરે અને ઑબ્જેક્ટ્સને સપોર્ટ કરે છે.
§§§CHUNK_SEPARATOR§§§
**ઉદાહરણ: વપરાશકર્તા પસંદગીઓ યાદ રાખો**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — AI એકીકરણ

> **સ્થિતિ: ટૂંક સમયમાં આવનાર** — AI API વ્યાખ્યાયિત છે પરંતુ હજુ સુધી Soomy સાથે જોડાયેલ નથી. હાલમાં `{ response: 'AI not yet connected' }` પાછું આપે. સંપૂર્ણ AI એકીકરણ ભવિષ્યના પ્રકાશનમાં યોજના ���નાવવામાં આવી છે.

#### `ai.chat(messages, options?)`

AI સહાયક (Soomy) ને સંદેશા મોકલો.
§§§CHUNK_SEPARATOR§§§
---

## ભાગ 3: વેબવ્યૂઝ સાથે કસ્ટમ UI બનાવવું

`openWebview()` API તમને HTML, CSS, અને JavaScript સાથે ડેશબોર્ડ UIs બનાવવાની મંજૂરી આપે છે — બધા પોપઅપ વિન્ડોમાં.

> **મહત્વપૂર્ણ મર્યાદા:** વેબવ્યૂઝ **પ્રદર્શન-માત્ર** છે. તેઓ પ્લગિન APIs (`ctx.settings`, `ctx.terminal`, વગેરે) પર પાછા કોલ કરી શકતા નથી. તમામ વપરાશકર્તા ક્રિયાઓ માટે સાઇડબાર બટનોનો ઉપયોગ કરો, અને વર્તમાન સ્થિતિને દર્શાવવા માટે `openWebview()` નો ઉપયોગ કરો. જો તમને ઇન્ટરેક્ટિવ ફીચર્સની જરૂર હોય, તો તેમને સાઇડબાર બટનોમાંથી પ્રેરિત કરો અને પ્રદર્શનને રિફ્રેશ કરવા માટે વેબવ્યૂ ફરીથી ખોલો.

### પેટર્ન: ટર્મિનલ કમાન્ડ → આઉટપુટ પાર્સ કરો → HTML માં બતાવો

આ સૌથી સામાન્ય પ્લગિન પેટર્ન છે. તમે એક કમાન્ડ ચલાવો છો, પરિણામને પાર્સ કરો છો, અને તેને દૃશ્યમાન રીતે દર્શાવો છો.
§§§CHUNK_SEPARATOR§§§
### પેટર્ન: ઓટો-રીફ્રેશ સાથે ઇન્ટરેક્ટિવ ડેશબોર્ડ
§§§CHUNK_SEPARATOR§§§
### પેટર્ન: વેબવ્યૂમાં સેટિંગ્સ દર્શાવવી

> **નોંધ:** વેબવ્યૂઝ પ્રદર્શન-માત્ર છે — તેઓ પ્લગિન APIs પર પાછા કોલ કરી શકતા નથી. સેટિંગ્સને સુધારવા માટે તમારા સાઇડબાર બટન હેન્ડલર્સમાં `ctx.settings` નો ઉપયોગ કરો, અને વર્તમાન સ્થિતિ બતા���વા માટે `openWebview()` નો ઉપયોગ કરો.
§§§CHUNK_SEPARATOR§§§
---

## ભાગ 4: તમારા પ્લગિનને પ્રકાશિત કરવો

### પગલું 1: સ્થાનિક રીતે પરીક્ષણ કરો

1. તમારા પ્લગિનને `~/.wia-soom/plugins/{your-plugin}/` માં નકલ કરો
2. WIA SOOM પુનઃપ્રારંભ કરો
3. તે કાર્ય કરે છે તે ખાતરી કરો: સાઇડબાર બટન દેખાય છે, ફીચર્સ યોગ્ય રીતે કાર્ય કરે છે
4. કિનારી કેસોનું પરીક્ષણ કરો: જો કોઈ ટર્મિનલ જોડાયેલ નથી તો શું થાય?

### પગલું 2: સબમિશન માટે તૈયાર કરો

તમારા પ્લગિન ફોલ્ડરમાં હોવું જોઈએ:
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
**આવશ્યક `package.json` ક્ષેત્રો:**

| ક્ષેત્ર | વર્ણન | ઉદાહરણ |
|-------|-------------|---------|
| `name` | અનન્ય કેબાબ-કેસ ID | `"my-awesome-plugin"` |
| `version` | સેમેન્ટિક સંસ્કરણ | `"1.0.0"` |
| `description` | એક-લાઇન વર્ણન | `"Monitors nginx access logs in real-time"` |
| `author` | તમારું નામ | `"John Doe"` |
| `main` | પ્રવેશ બિંદુ | `"index.js"` |

**વૈકલ્પિક ક્ષેત્રો:**

| ક્ષેત્ર | વર્ણન |
|-------|-------------|
| `license` | લાઇસન્સ પ્રકાર (MIT ભલામણ કરેલ) |
| `keywords` | શોધ ટૅગ્સની શ્રેણી |
| `soom.minVersion` | જરૂરી ઓછામાં ઓછું WIA SOOM સંસ્કરણ |

### પગલું 3: પ્લગિન રજિસ્ટ્રીમાં સબમિટ કરો

1. **ફોર્ક** [Plugin Store](https://wiasoom.com)
2. **જોડો** તમારું પ્લગિન `plugins/{your-plugin-name}/` માં
3. **સબમિ��** એક પુલ રિક્વેસ્ટ

### પગલું 4: સમીક્ષા અને મંજૂરી

અમે દરેક પ્લગિનની સમીક્ષા કરીએ છીએ:

- **સુરક્ષા** — કોઈ જોખમી APIs નથી (જુઓ [Security Rules](#security-rules))
- **ગુણવત્તા** — શું તે કાર્ય કરે છે? શું કોડ સ્વચ્છ છે?
- **ઉપયોગિતા** — શું તે ખરેખર સમસ્યાનો ઉકેલ આપે છે?

મંજૂરી પછી:
1. તમારું પ્લગિન `registry.json` માં ઉમેરવામાં આવે છે
2. `dist/` માં એક ZIP બંડલ બનાવવામાં આવે છે
3. તમારું પ્લગિન તમામ WIA SOOM વપરાશકર્તાઓ માટે **Plugin Store** માં દેખાય છે!

---

## ભાગ 5: શ્રેષ્ઠ પ્રથાઓ

### સુરક્ષા નિયમો

આ નિયમો **જરૂરી** છે. જે પ્લગિન આનું ઉલ્લંઘન કરે છે તે નકારી દેવાશે.

| નિયમ | શા માટે |
|------|-----|
| **ક્યારેય** `eval()` અથવા `new Function()` નો ઉપયોગ ન કરો | કોડ ઇન્જેક્શનનો જોખમ |
| **ક્યારેય** `child_process`, `exec()`, `spawn()` નો ઉપયોગ ન કરો | આદેશો માટે ફક્ત `ctx.terminal.send()` નો ઉપયોગ કરો |
| **ક્યારેય** બાહ્ય URLs મેળવતા ન જાઓ | અપવાદ: `wiasoom.com` API અંતિમ બિંદુઓ |
| **ક્યારેય** `process.env` નો ઍક્સેસ ન કરો | પર્યાવરણના ચલોમાં ગુપ્ત માહિતી હોઈ શકે છે |
| **ક્યારેય** સીધા `require('fs')` નો ઉપયોગ ન કરો | સંગ્રહ માટે `ctx.settings` નો ઉપયોગ કરો, ફાઇલ ટ્રાન્સફર માટે `ctx.sftp` |
| **ક્યારેય** npm બાહ્ય પેકેજોનો ઉપયોગ ન કરો | શુદ્ધ જાવાસ્ક્રિપ્ટ જ — કોઈ node_modules નથી |
| **જરૂરી** છે કે બધા રિમોટ આદેશો માટે `ctx.terminal.send()` નો ઉપયોગ કરો | આ સુરક્ષિત SSH ચેનલ મારફતે જાય છે |
| **જરૂરી** છે કે `deactivate()` માં સફાઈ કરો | શ્રોતાઓને દૂર કરો, અંતરાલ સાફ કરો |

### ભૂલ સંભાળવું

હંમેશા જોખમી ઓપરેશન્સને try/catch માં લપેટો:
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
### deactivate() માં સફાઈ

જો તમારું પ્લગિન અંતરાલ, શ્રોતાઓ, અથવા સબ્સ્ક્રિપ્શન બનાવે છે — તેમને સાફ કરો:
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
### i18n સપોર્ટ

WIA SOOM 254 ભાષાઓને સપોર્ટ કરે છે. તમારા પ્લગિન લેબલને અનુવાદિત કરવા માટે, સરળ પદ્ધતિનો ઉપયોગ કરો:
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

## ભાગ 6: વાસ્તવિક-જગ્યા ઉદાહરણો

### ઉદાહરણ 1: સર્���ર ડિસ્ક ચેકર

દૂરના સર્વર પર `df -h` ચલાવે છે અને સ્થિતિ પટ્ટીમાં ઉપયોગમાં લેવાયેલ/ઉપલબ્ધ જગ્યા દર્શાવે છે.
§§§CHUNK_SEPARATOR§§§
---

### ઉદાહરણ 2: TODO મેનેજર

એક પ્લગિન જે ટકાઉ સંગ્રહ માટે સેટિંગ્સનો ઉપયોગ કરે છે અને પ્રદર્શનમાં વેબવ્યૂ માટે TODO યાદીનું સંચાલન કરે છે.

> **ડિઝાઇન પેટર્ન:** કારણ કે વેબવ્યૂ સીધા પ્લગિન APIsને કૉલ કરી શકતા નથી, આ પ્લગિન "સ્નેપશોટ" પદ્ધતિનો ઉપયોગ કરે છે — તે સેટિંગ્સમાંથી TODO વાંચે છે, તેમને વાંચવા માટે HTML તરીકે રેન્ડર કરે છે, અને આઇટમ્સ ઉમેરવા માટે સાઇડબાર આધારિત ક્રિયાઓ પ્રદાન કરે છે. વેબવ્યૂ એક **પ્રદર્શન** સ્��ર છે, ઇન્ટરેક્ટિવ ફોર્મ નથી.
§§§CHUNK_SEPARATOR§§§
---

### ઉદાહરણ 3: ભૂલ વોચર

ટર્મિનલ આઉટપુટને મોનીટ કરે છે અને ચોક્કસ પેટર્ન શોધાતા સમયે એક સૂચના મોકલે છે.
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

## પરિશિષ્ટ: શ્રેણીઓ અને ચિહ્નો

### પ્લગિન શ્રેણીઓ (29)

આને તમારા `package.json` `keywords` માં અથવા રજીસ્ટ્રીમાં સબમિટ કરતી વખતે ઉપયોગ કરો:

| શ્રેણી | વર્ણન |
|----------|-------------|
| `server` | સામાન્ય સર્વર મેનેજમેન્ટ |
| `devtools` | વિકાસ સાધનો |
| `calculator` | ગણતરી અને રૂપાંતરક |
| `simulator` | સિમ્યુલેટર્સ |
| `game` | ટર્મિનલ રમતો |
| `business` | બિઝનેસ સાધનો |
| `security` | સુરક્ષા અને ઓડિટિંગ |
| `web` | વેબ સર્વર મેનેજમેન્ટ |
| `education` | શૈક્ષણિક સાધનો |
| `health` | આરોગ્ય સંબંધિત સાધનો |
| `islamic` | ઇસ્લામિક સાધનો (પ્રાર્થના સમય, વગેરે) |
| `science` | વૈજ્ઞાનિક સાધનો |
| `quantum` | ક્વાન્ટમ કમ્પ્યુટિંગ સાધનો |
| `ai` | AI-શક્તિ ધરાવતી સાધનો |
| `biotech` | બાયોટેકનોલોજી સાધનો |
| `space` | અવકાશ અને ખગોળશાસ્ત્ર સાધનો |
| `network` | નેટવર્ક સાધનો |
| `database` | ડેટાબેસ મેનેજમેન્ટ |
| `monitoring` | સર્વર મોનિટરિંગ |
| `devops` | DevOps અને CI/CD |
| `utility` | સામાન્ય ઉપયોગીતા |
| `design` | ડિઝાઇન સાધનો |
| `ecommerce` | ઇ-કોમર્સ સાધનો |
| `automation` | ઓટોમેશન સાધનો |
| `kpop` | K-pop સંબંધિત સાધનો |
| `accessibility` | ઍક્સેસિબિલિટી સાધનો |
| `analytics` | વિશ્લેષણ અને અહેવાલ |
| `wia` | WIA ઇકોસિસ્ટમ સાધનો |
| `all` | તમામ શ્રેણીઓમાં દેખાય છે |

### ભલામણ કરેલ ચિહ્નો (Lucide)

| ચિહ્ન નામ | ઉપયોગ માટે |
|-----------|---------|
| `server` | સર્વર મેનેજમેન્ટ |
| `shield` | સુરક્ષા |
| `database` | ડેટાબેસ |
| `activity` | મોનિટરિંગ |
| `terminal` | ટર્મિનલ સાધનો |
| `code` | વિકાસ |
| `hard-drive` | ડિસ્ક/સ્ટોરેજ |
| `network` | નેટવર્કિંગ |
| `lock` | ઓથ/એન્ક્રિપ્શન |
| `eye` | જોવું/મોનિટરિંગ |
| `check-square` | કાર્ય/TODO |
| `layout-dashboard` | ડેશબોર્ડ |
| `settings` | રૂપરેખાંકન |
| `zap` | ઓટોમેશન |
| `globe` | વેબ/આંતરરાષ્ટ્રીય |

બધા 1,500+ ચિહ્નો બ્રાઉઝ કરો: [lucide.dev/icons](https://lucide.dev/icons)

---

## મદદની જરૂર છે?

- **GitHub સમસ્યાઓ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **પ્લગિન સમસ્યાઓ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ઉદાહરણ પ્લગિન:** [Website](https://wiasoom.com)
- **વેબસાઇટ:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>કાંઈ અદ્ભુત બનાવ���. તેને દુનિયા સાથે શેર કરો.</em></p>
<p align="center"><em>— WIA SOOM ટીમ</em></p>
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

**Important:** Always save the unsubscribe function and call it in `deactivate()` to prevent memory leaks.

---

### `ctx.sftp` — File transfer

> **Status: Coming Soon** — The SFTP API is defined but not yet wired to the app's SFTP engine. `list()` currently returns an empty array, and `upload()`/`download()` are no-ops. This will be fully implemented in a future release. For now, use `ctx.terminal.send()` with `scp` or `rsync` commands as a workaround.

#### `sftp.list(sessionId, path)`

List files in a remote directory.

```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```

#### `sftp.upload(sessionId, localPath, remotePath)`

Upload a file from local machine to remote server.

```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```

#### `sftp.download(sessionId, remotePath, localPath)`

Download a file from remote server to local machine.

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

Add a button to the WIA SOOM sidebar.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `id` | `string` | No | Unique ID (defaults to plugin name) |
| `icon` | `string` | Yes | Lucide icon name (e.g., `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Yes | Button text shown in sidebar |
| `onClick` | `() => void` | Yes | Function called when button is clicked |

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

> **Compatibility note:** Some older plugins use positional arguments like `addSidebarButton(id, icon, label, onClick)`. The official API uses an **options object** as documented above. Always use the object style for new plugins.

#### `ui.openWebview(options)`

Open a popup window with custom HTML content. This is how you build rich UIs.

| Option | Type | Description |
|--------|------|-------------|
| `title` | `string` | Window title |
| `html` | `string` | Full HTML content to render |

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

> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

Show a toast notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Notification style |
| `message` | `string` | Text to show |

```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```

#### `ui.addStatusBarItem(id, text)`

Add a persistent text item to the bottom status bar.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique ID for this status item |
| `text` | `string` | Text to display |

```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```

---

### `ctx.settings` — Persistent storage

Plugin settings are stored permanently in `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`.

#### `settings.get(key)`

Read a saved value.

```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```

Returns `undefined` if the key doesn't exist.

#### `settings.set(key, value)`

Save a value. Supports strings, numbers, booleans, arrays, and objects.

```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```

**Example: Remember user preferences**

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
