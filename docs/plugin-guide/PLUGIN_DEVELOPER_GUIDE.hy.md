<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Plugin Developer Guide</h1>
<p align="center"><strong>Ստեղծեք ձեր սեփական պլագինը 5 րոպեում:</strong></p>
<p align="center">Ստեղծեք ուժեղ սերվերային գործիքներ, վահանակներ և ավտոմատացում — հենց WIA SOOM-ի ներսում:</p>

---

## Բովանդակության աղյուսակ

- [Մաս 1: Արագ սկսում — Ձեր առաջին պլագինը 5 րոպեում](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [Մաս 2: Plugin Context API-ի հղում](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [Մաս 3: Հարմար UI կառուցելը Webviews-ով](#part-3-building-custom-ui-with-webviews)
- [Մաս 4: Ձեր պլագինի հրապարակումը](#part-4-publishing-your-plugin)
- [Մաս 5: Լավագույն պրակտիկաներ](#part-5-best-practices)
- [Մաս 6: Իրական կյանքի օրինակներ](#part-6-real-world-examples)
- [Ավելացումներ: Կատեգորիաներ և պատկերակներ](#appendix-categories--icons)

---

## Մաս 1: Արագ սկսում — Ձեր առաջին պլագինը 5 րոպեում

### Ինչ եք կառուցելու

«Hello World» պլագին, որը կողային վահանակին ավելացնում է կոճակ: Երբ սեղմում եք, այն ցույց է տալիս ծանուցում:

### Քայլ 1: Ստեղծեք պլագինի թղթապանակ
§§§CHUNK_SEPARATOR§§§
### Քայլ 2: Ստեղծեք package.json
§§§CHUNK_SEPARATOR§§§
**Պահանջվող դաշտեր:** `name`, `version`, `description`, `author`, `main`

### Քայլ 3: Ստեղծեք index.js
§§§CHUNK_SEPARATOR§§§
### Քայլ 4: Վերագործարկեք WIA SOOM

Վերագործարկեք ծրագիրը (կամ անջատեք/կ միացրեք պլագինը Կարգավորումներ → Պլագիններ):

Դուք պետք է տեսնեք **«Hello World»** կոճակը կ��ղային վահանակում: Սեղմեք այն — դուք կտեսնեք հաջողության ծանուցում:

### Ինչպես է աշխատում
§§§CHUNK_SEPARATOR§§§
---

## Մաս 2: Plugin Context API-ի հղում

Երբ ձեր `activate(context)` ֆունկցիան կոչվում է, `context` (կամ `ctx`) տրամադրում է այս API-ները:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Կոմանդներ գործարկել հեռավոր սերվերներում

#### `terminal.send(sessionId, data)`

Ուղարկեք կոմանդ (կամ ցանկացած տվյալ) ակտիվ տերմինալ նստաշրջանին:

| Պարամետր | Տիպ | Նկարագրություն |
|-----------|------|-------------|
| `sessionId` | `string` | Տերմինալ նստաշրջանը, որի համար ուղարկվում է |
| `data` | `string` | Ուղարկվող կոմանդը կամ տվյալները |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Բաժանորդագրվեք տերմինալ նստաշրջանից բոլոր ելքներին: Վերադարձնում է **բաժանորդագրությունը դադարեցնելու ֆունկցիա**:

| Պարամետր | Տիպ | ��կարագրություն |
|-----------|------|-------------|
| `sessionId` | `string` | Տերմինալ նստաշրջանը, որը պետք է դիտել |
| `callback` | `(data: string) => void` | Կոչվում է յուրաքանչյուր ելքի կտորով |
| **Վերադարձնում է** | `() => void` | Կոչեք սա լսելը դադարեցնելու համար |
§§§CHUNK_SEPARATOR§§§
**Կարևոր:** Always save the unsubscribe function and call it in `deactivate()` to prevent memory leaks.

---

### `ctx.sftp` — Ֆայլերի փոխանցում

> **Կարգավիճակ: Շուտով** — SFTP API-ն սահմանված է, բայց դեռ չի միացվել application's SFTP շարժիչին: `list()`-ը ներկայումս վերադարձնում է դատարկ զանգված, իսկ `upload()`/`download()`-ը ոչ մի գործողություն չեն կատարում: Սա ամբողջությամբ իրականացվելու է ապագա թողարկման մեջ: Այս պահին օգտագործեք `ctx.terminal.send()` `scp` կամ `rsync` կոմանդներով որպես workaround:

#### `sftp.list(sessionId, path)`

Ցուցակավորեք ֆայլերը հեռավոր կատալոգում:
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Վերբեռնեք ֆայլը տեղական մեքենայից հեռավոր սերվեր:
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Ներբեռնեք ֆայլը հեռավոր սերվերից տեղական մեքենա:
§§§CHUNK_SEPARATOR§§§
**Workaround (մինչ SFTP API-ն կենդանի է):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Օգտագործողի ինտերֆեյս

#### `ui.addSidebarButton(options)`

Ավելացրեք կոճակ WIA SOOM կողային վահանակին:

| Ընտրանք | Տիպ | Պահանջվում է | Նկարագրություն |
|----------|------|-------------|-------------|
| `id` | `string` | Ոչ | Հատուկ ID (համարվում է պլագինի անունը) |
| `icon` | `string` | Այո | Lucide պատկերակի անուն (օրինակ, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Այո | Կոճակի տեքստը, որը ցույց է տրվում կողային վահանակում |
| `onClick` | `() => void` | Այո | Ֆունկցիա, որը կոչվում է, երբ կոճակը սեղմվում է |
§§§CHUNK_SEPARATOR§§§
**Պատկերակի հղում:** Բrowse all available icons at [lucide.dev/icons](https://lucide.dev/icons)

> **Համատեղելիության նշում:** Որոշ հին պլագիններ օգտագործում են դիրքային արգումենտներ, ինչպիսիք են `addSidebarButton(id, icon, label, onClick)`: Ապահովված API-ն օգտագործում է **ընտրանքների օբյեկտ** վերևում նկարագրված ձևով: Always use the object style for new plugins.

#### `ui.openWebview(options)`

Բացեք պոպ-ապ պատուհան հարմար HTML բովանդակությամբ: Սա այն է, թե ինչպես եք կառուցում հարուստ UI-ներ:

| Ընտրանք | Տիպ | Նկարագրություն |
|----------|------|-------------|
| `title` | `string` | Պատուհանի վերնագիր |
| `html` | `string` | Լրիվ HTML բովանդակություն, որը պետք է ցուցադրվի |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> Տեսեք [Համար 3](#part-3-building-custom-ui-with-webviews) առաջադեմ webview նախշերի համար։

#### `ui.showNotification(type, message)`

Ցույց տալ տոստ ծանուցում։

| Պարամետր | Տիպ | նկարագրություն |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Ծանուցման ոճ |
| `message` | `string` | Ցուցադրելի տեքստ |
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

Ավելացնել մշտական տեքստային տարր ներքևի կարգավիճակի տողին։

| Պարամետր | Տիպ | նկարագրություն |
|-----------|------|-------------|
| `id` | `string` | Այս կարգավիճակի տարրի եզակի ID |
| `text` | `string` | Ցուցադրելի տեքստ |
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

### `ctx.settings` — Մշտական պահեստավորում

Plugin-ի կարգավորումները մշտապես պահվում են `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`։

#### `settings.get(key)`

Կարդալ պահպանված արժեքը։
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Վերադարձնում է `undefined`, եթե բանալին գոյություն չունի։

#### `settings.set(key, value)`

Պահպանել արժեքը։ Աջակցում է տողերին, թվերին, բուլյաններին, զանգվածներին և օբյեկտներին։
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
**Օրինակ: Հիշել օգտվողի նախասիրությունները**
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### `ctx.ai` — AI ինտեգրում

> **Կարգավիճակ: Շուտով** — AI API-ն սահմանված է, բայց դեռ չի միացված Soomy-ին։ Այս պահին վերադարձնում է `{ response: 'AI not yet connected' }`։ Ամբողջական AI ինտեգրում նախատեսվում է ապագա թողարկման համար։

#### `ai.chat(messages, options?)`

Ուղարկել հաղորդագրություններ AI օգնականին (Soomy)։
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

## Համար 3: Անհատական UI կառուցում Webviews-ով

`openWebview()` API-ն թույլ է տալիս կառուցել դաշբորդի UI-ներ HTML-ով, CSS-ով և JavaScript-ով՝ բոլորն էլ պոպ-ապ պատուհանում։

> **Կարևոր սահմանափակում:** Webviews-ը **ցուցադրելու միայն**։ Նրանք չեն կարող զանգահարել plugin API-ներ (`ctx.settings`, `ctx.terminal`, և այլն)։ Օգտագործեք կողային կոճակներ բոլոր օգտվողի գործողությունների համար, և օգտագործեք `openWebview()` ներկայիս վիճակը ցուցադրելու համար։ Եթե ձեզ անհրաժեշտ են ինտերակտիվ հատկություններ, ակտիվացրեք դրանք կողային կոճակներից և կրկին բացեք webview-ը ցուցադրությունը թարմացնելու համար։

### Նախշ: Տերմինալի հրաման → Վերլուծել արդյունքը → Ցուցադրել HTML-ում

Սա ամենատարածված plugin նախշն է։ Դուք կատարում եք հրաման, վերլուծում արդյունքը և ցուցադրում այն տեսողականորեն։
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Նախշ: Ինտերակտիվ Դաշբորդ ավտո-թարմացմամբ
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
### Նախշ: Կարգավորումների ցուցադրում Webview-ում

> **Նշում:** Webviews-ը ցուցադրելու միայն են՝ նրանք չեն կարող զանգահա��ել plugin API-ներ։ ��գտագործեք `ctx.settings` ձեր կողային կոճակների մշակողներում կարգավորումները փոփոխելու համար, և օգտագործեք `openWebview()` ներկայիս վիճակը ցուցադրելու համար։
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
---

## Համար 4: Ձեր Plugin-ի հրապարակումը

### Քայլ 1: Թեստավորել տեղական

1. Պատճենեք ձեր plugin-ը `~/.wia-soom/plugins/{your-plugin}/`
2. Վերագործարկեք WIA SOOM
3. Համոզվեք, որ աշխատում է. կողային կոճակը երևում է, հատկությունները աշխատում են ճիշտ
4. Թեստավորեք եզրային դեպքերը. ինչ է տեղի ունենում, եթե ոչ մի տերմինալ չի միացված։

### Քայլ 2: Պատրաստվել ներկայացման համար

Ձեր plugin-ի թղթապանակը պետք է պարունակի:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
**Պահանջվող `package.json` դաշտեր:**

| Դաշտ | նկարագրություն | Օրինակ |
|-------|-------------|---------|
| `name` | Հատուկ kebab-case ID | `"my-awesome-plugin"` |
| `version` | Սեմանտիկ տարբերակ | `"1.0.0"` |
| `description` | Մի տողանոց նկարագրություն | `"Monitors nginx access logs in real-time"` |
| `author` | Ձեր անունը | `"John Doe"` |
| `main` | Մուտքային կետ | `"index.js"` |

**Ընտրովի դաշտեր:**

| Դաշտ | նկարագրություն |
|-------|-------------|
| `license` | Լիցենզիայի տեսակ (հrecommended MIT) |
| `keywords` | Որոնման պիտակների զանգված |
| `soom.minVersion` | Պահանջվող նվազագույն WIA SOOM տարբերակ |

### Քայլ 3: Ներկայացնել Plugin Registry-ին

1. ****Package** your plugin as a ZIP file
2. **Ավելացնել** ձեր plugin-ը `plugins/{your-plugin-name}/`
3. **Ներկայացնել** Pull Request

### Քայլ 4: Մասնագիտական վերանայում և հաստատում

Մենք վերանայում ենք յուրաքանչյուր plugin՝

- **Անվտանգություն** — վտանգավոր API-ներ չկան (տեսեք [Անվտանգության կանոնները](#security-rules))
- **Որակ** — արդյոք աշխատում է? Կոդը մաքուր է՞
- **Օգտակարություն** — արդյոք լուծում է իրական խնդիրներ?

Հաստատումից հետո:
1. Ձեր plugin-ը ավելացվում է `registry.json`
2. ZIP փաթեթը ստեղծվում է `dist/`
3. Ձեր plugin-ը երևում է **Plugin Store**-ում բոլոր WIA SOOM օգտվողների համար!

---

## Բաժին 5: Լավագույն պրակտիկաներ

### Անվտանգության կանոններ

Այս կանոնները **պարտադիր** են: Կողմնակի plugin-ները, որոնք խախտում են դրանք, կժխտվեն:

| Կանոն | Ինչու |
|------|-----|
| **Ե NEVER** օգտագործեք `eval()` կամ `new Function()` | Կոդի ներարկման ռիսկ |
| **Ե NEVER** օգտագործեք `child_process`, `exec()`, `spawn()` | Օգտագործեք միայն `ctx.terminal.send()` հրամանների համար |
| **Ե NEVER** ներբեռնեք արտաքին URL-ներ | Բացառություն: `wiasoom.com` API վերջնակետեր |
| **Ե NEVER** մուտք գործեք `process.env` | Շրջակա փոփոխականները կարող են պարունակել գաղտնիքներ |
| **Ե NEVER** օգտագործեք `require('fs')` ուղիղ | Օգտագործեք `ctx.settings` պահեստավորման համար, `ctx.sftp` ֆայլերի փոխանցման համար |
| **Ե MUST** օգտագործեք `ctx.terminal.send()` բոլոր հեռավոր հրամանների համար | Սա անցնում է անվտանգ SSH ալիքով |
| **Ե MUST** մաքրեք `deactivate()`-ում | Հեռացրեք լսողներին, մաքրեք միջակայքերը |

### Թերությունների կառավարում

Միշտ վտանգավոր գործողությունները փաթեթավորեք try/catch:
§§§CHUNK_SEPARATOR§§§
### Մաքրում deactivate()-ում

Եթե ձեր plugin-ը ստեղծում է միջակայքներ, լսողներ կամ բաժանորդագրություններ — մաքրեք դրանք:
§§§CHUNK_SEPARATOR§§§
### i18n աջակցություն

WIA SOOM-ը աջակ��ում է 254 լեզուների: Ձեր plugin-ի պիտակը թարգմանելի դարձնելու համար օգտագործեք պարզ մոտեցում:
§§§CHUNK_SEPARATOR§§§
---

## Բաժին 6: Իրական աշխարհում օրինակներ

### Օրինակ 1: Սերվերի սկավառակի ստուգիչ

Անցկացնում է `df -h` հեռավոր սերվերում և ցույց է տալիս օգտագործված/հասանելի տարածքը կարգավիճակի տողում:
§§§CHUNK_SEPARATOR§§§
---

### Օրինակ 2: TODO կառավարիչ

Plugin, որը կառավարում է TODO ցուցակը՝ օգտագործելով կարգավորումներ մշտական պահեստավորման համար և webview ցուցադրության համար:

> **Դիզայնի ձևաչափ:** Քանի որ webviews-ը չեն կարող ուղղակիորեն կանչել plugin API-ներ, այս plugin-ը օգտագործում է "snapshot" մոտեցում — այն կարդում է TODO-ները կարգավորումներից, ներկայացնում է դրանք որպես միայն ընթերցվող HTML և տրամադրում է կողային գործողություններ տարրեր ավելացնելու համար: Webview-ը **ցուցադրական** շերտ է, ոչ թե ինտերակտիվ ձև:

§§§CHUNK_SEPARATOR§§§
---

### Օրինակ 3: Թերությունների դիտարկիչ

Հսկում է տերմինալի ելքը և ուղարկում է ծանուցում, երբ որոշակի նախշեր են հայտնաբերվում:
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
---

## Ավելացումներ: Կատեգորիաներ & Իկոններ

### Plugin Կատեգորիաներ (29)

Օգտագործեք դրանք ձեր `package.json` `keywords`-ում կամ գրանցման ժամանակ՝

| Կատեգորիա | Նկարագրություն |
|------------|----------------|
| `server` | Ընդհանուր սերվերի կառավարում |
| `devtools` | Զարգացման գործիքներ |
| `calculator` | Հաշվիչներ և փոխարկիչներ |
| `simulator` | Սիմուլյատորներ |
| `game` | Տերմինալ խաղեր |
| `business` | Բիզնեսի գործիքներ |
| `security` | Անվտանգություն և աուդիտ |
| `web` | Վեբ սերվերի կառավարում |
| `education` | Կրթական գործիքներ |
| `health` | Առողջության հետ կապված գործիքներ |
| `islamic` | Իսլամական գործիքներ (աղոթքի ժամանակներ և այլն) |
| `science` | Գիտական գործիքներ |
| `quantum` | Քվանտային հաշվարկի գործիքներ |
| `ai` | Արհեստական բանականության վրա հիմնված գործիքներ |
| `biotech` | Բիոտեխնոլոգիայի գործիքներ |
| `space` | Տիեզերքի և աստղագիտության գործիքներ |
| `network` | Րանցանցային գործիքներ |
| `database` | Տվյալների բազայի կառավարում |
| `monitoring` | Սերվերի մոնիտորինգ |
| `devops` | DevOps և CI/CD |
| `utility` | Ընդհանուր օգտակարություններ |
| `design` | Դիզայնի գործիքներ |
| `ecommerce` | Էլեկտրոնային առևտրի գործիքներ |
| `automation` | Ավտոմատացման գործիքներ |
| `kpop` | K-pop հետ կապված գործիքներ |
| `accessibility` | Հասանելիության գործիքներ |
| `analytics` | Վերլուծություն և հաշվետվություն |
| `wia` | WIA էկոհամակարգի գործիքներ |
| `all` | Հանդիպում է բոլոր կատեգորիաներում |

### Առաջարկվող Իկոններ (Lucide)

| Իկոնի Անվանում | Օգտագործել համար |
|----------------|------------------|
| `server` | Սերվերի կառավար��ւմ |
| `shield` | Անվտանգություն |
| `database` | Տվյալների բազա |
| `activity` | Մոնիտորինգ |
| `terminal` | Տերմինալ գործիքներ |
| `code` | Զարգացում |
| `hard-drive` | Դիսկ/պահեստ |
| `network` | Րանցանցում |
| `lock` | Ավտորիզացիա/ encryption |
| `eye` | Դիտարկում/մոնիտորինգ |
| `check-square` | Պարտականություններ/TODO |
| `layout-dashboard` | Դաշտեր |
| `settings` | Կարգավորում |
| `zap` | Ավտոմատացում |
| `globe` | Վեբ/միջազգային |

Նայեք բոլոր 1,500+ իկոնները՝ [lucide.dev/icons](https://lucide.dev/icons)

---

## Պետք է օգնություն?

- **GitHub Խնդիրներ:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Խնդիրներ:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Օրինակ Plugin-ներ:** [Website](https://wiasoom.com)
- **Վեբ կայք:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Կառուցեք ինչ-որ բան հիանալի: Կիսվեք աշխարհին:</em></p>
<p align="center"><em>— WIA SOOM թիմը</em></p>
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
