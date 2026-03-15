<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM பிளகின் டெவலப்பர் வழிகாட்டி</h1>
<p align="center"><strong>5 நிமிடங்களில் உங்கள் சொந்த பிளகினை உருவாக்குங்கள்.</strong></p>
<p align="center">சக்திவாய்ந்த சர்வர் கருவிகள், டாஷ்போர்டுகள் மற்றும் தானியங்கி செயல்பாடுகளை உருவாக்குங்கள் — WIA SOOM இல் நேரடியாக.</p>

---

## உள்ளடக்க அட்டவணை

- [பகுதி 1: விரைவு தொடக்கம் — உங்கள் முதல் பிளகின் 5 நிமிடங்களில்](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [பகுதி 2: பிளகின் சூழல் API கு���ிப்பு](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [பகுதி 3: வலைக்காட்சிகளுடன் தனிப்பயன் UI உருவாக்குதல்](#part-3-building-custom-ui-with-webviews)
- [பகுதி 4: உங்கள் பிளகினை வெளியிடுதல்](#part-4-publishing-your-plugin)
- [பகுதி 5: சிறந்த நடைமுறைகள்](#part-5-best-practices)
- [பகுதி 6: உண்மையான உலக எடுத்துக்காட்டுகள்](#part-6-real-world-examples)
- [குறிப்பு: வகைகள் & ஐகான்கள்](#appendix-categories--icons)

---

## பகுதி 1: விரைவு தொடக்கம் — உங்கள் முதல் பிளகின் 5 நிமிடங்களில்

### நீங்கள் உருவாக்கப்போகும் விஷயம்

ஒரு "Hello World" பிளகின், இது பக்கம் பட்டியலில் ஒரு பொத்தானை சேர்க்கிறது. கிளிக் செய்தால், இது ஒரு அறிவிப்பை காட்டுகிறது.

### படி 1: பிளகின் கோப்புறை உருவாக்கவும்
§§§CHUNK_SEPARATOR§§§
### படி 2: package.json உருவாக்கவும்
§§§CHUNK_SEPARATOR§§§
**தேவையான புலங்கள்:** `name`, `version`, `description`, `author`, `main`

### படி 3: index.js உருவாக்கவும்
§§§CHUNK_SEPARATOR§§§
### படி 4: WIA SOOM ஐ மறுதொடக்கம் செய்யவும்

அப்பிளிக்கேஷனை மறுதொடக்கம் செய்யவும் (அல்லது அமைப்புகள் → பிளகின்களில் பிளகினை அண்மையில்/ஆன் செய்யவும்).

நீங்கள் பக்கம் பட்டியலில் ஒரு **"Hello World"** பொத்தானை காண வேண்டும். அதை கிளிக் செய்யவும் — நீங்கள் ஒரு வெற்றி அறிவிப்பு காண்பீர்கள்!

### இது எப்படி வேலை செய்கிறது
§§§CHUNK_SEPARATOR§§§
---

## பகுதி 2: பிளகின் சூழல் API குறிப்பு

உங்கள் `activate(context)` செயல்பாடு அழைக்கப்படும் போது, `context` (அல்லது `ctx`) இந்த API களை வழங்குகிறது:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — தொலைநிலையிலான சர்வர்களில் கட்டளைகளை இயக்கவும்

#### `terminal.send(sessionId, data)`

ஒரு செயல்பாட்டில் (அல்லது எந்த தரவிலும்) ஒரு கட்டளையை அனுப்பவும்.

| அளவுரு | வகை | விளக்கம் |
|---------|------|----------|
| `sessionId` | `string` | அனுப்ப வேண்டிய தொலைநிலையிலான செயல்பாடு |
| `data` | `string` | அனுப்ப வேண்டிய கட்டளை அல்லது தரவு |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

ஒரு தொலைநில���யிலான செயல்பாட்டிலிருந்து அனைத்து வெளியீடுகளுக்கும் சந்தா செய்யவும். ஒரு **சந்தா நீக்குதல் செயல்பாடு** திருப்புகிறது.

| அளவுரு | வகை | விளக்கம் |
|---------|------|----------|
| `sessionId` | `string` | கவனிக்க வேண்டிய தொலைநிலையிலான செயல்பாடு |
| `callback` | `(data: string) => void` | ஒவ்வொரு வெளியீட்டு துண்டுடன் அழைக்கப்படுகிறது |
| **திருப்புகிறது** | `() => void` | கேட்கவும் இதை நிறுத்தவும் |
§§§CHUNK_SEPARATOR§§§
**முக்கியம்:** எப்போதும் சந்தா நீக்குதல் செயல்பாட்டை சேமிக்கவும் மற்றும் நினைவக கசிவுகளைத் தடுக்கும் வகையில் `deactivate()` இல் அதை அழைக்கவும்.

---

### `ctx.sftp` — கோப்பு பரிமாற்றம்

> **நிலை: விரைவில் வருகிறேன்** — SFTP API வரையறுக்கப்பட்டுள்ளது ஆனால் இன்னும் செயலியில் SFTP இயந்திரத்துடன் இணைக்கப்படவில்லை. `list()` தற்போதைய காலத்தில் ஒரு காலியான வரிசையை திருப்புகிறது, மற்றும் `upload()`/`download()` செயல்பாடுகள் செயலிழந்துள்ளன. இது எதிர்கால வெளியீட்டில் முழுமையாக செயல்படுத்தப்படும். இப்போது, `scp` அல்லது `rsync` கட்டளைகளுடன் `ctx.terminal.send()` ஐ மாற்றமாக பயன்படுத்தவும்.

#### `sftp.list(sessionId, path)`

ஒரு தொலைநிலையிலான அடைவில் கோப்புகளை பட்டியலிடவும்.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

இருப்பிடத்தில் உள்ள கணினியிலிருந்து தொலைநிலையிலான சர்வருக்கு ஒரு கோப்பை பதிவேற்றவும்.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

தொலைநிலையிலான சர்வரிலிருந்து உள்ள கணினிக்கு ஒரு கோப்பை பதிவிறக்கவும்.
§§§CHUNK_SEPARATOR§§§
**மாற்று (SFTP API செயல்பாட்டில் வரும்வரை):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — பயனர் இடைமுகம்

#### `ui.addSidebarButton(options)`

WIA SOOM பக்கம் பட்டியலில் ஒரு பொத்தானை சேர்க்கவும்.

| விருப்பம் | வகை | தேவையானது | விளக்கம் |
|----------|------|------------|-------------|
| `id` | `string` | இல்லை | தனித்துவமான ID (பிளகின் பெயருக்கு இயல்பாக அமைக்கப்படுகிறது) |
| `icon` | `string` | ஆம் | Lucide ஐகான் பெயர் (உதாரணமாக, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ஆம் | பக்கம் பட்டியலில் காணப்படும் பொத்தான் உரை |
| `onClick` | `() => void` | ஆம் | பொத்தானை கிளிக் செய்தால் அழைக்கப்படும் செயல்பாடு |
§§§CHUNK_SEPARATOR§§§
**ஐகான் குறிப்பு:** [lucide.dev/icons](https://lucide.dev/icons) இல் அனைத்து கிடைக்கக்கூடிய ஐகான்களைப் பார்வையிடவும்

> **இணக்குறிப்பு:** சில பழைய பிளகின்கள் `addSidebarButton(id, icon, label, onClick)` போன்ற இடம் அடிப்படையிலான அளவுருக்களைப் பயன்படுத்துகின்றன. அதிகாரப்பூர்வ API மேலே விவரிக்கப்பட்டுள்ள **விருப்பங்கள் பொருள்** ஐப் பயன்படுத்துகிறது. புதிய பிளகின்களுக்கு எப்போதும் பொருள் பாணியைப் பயன்படுத்தவும்.

#### `ui.openWebview(options)`

தனிப்பயன் HTML உள்ளடக���கத்துடன் ஒரு பாப் அப் ஜன்னலை திறக்கவும். இது நீங்கள் வளிமண்டலங்களை உருவாக்கும் முறை.

| விருப்பம் | வகை | விளக்கம் |
|----------|------|-------------|
| `title` | `string` | ஜன்னலின் தலைப்பு |
| `html` | `string` | உருவாக்க வேண்டிய முழு HTML உள்ளடக்கம் |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

ஒரு டோஸ்ட் அறிவிப்பை காண்பிக்கவும்.

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | அறிவிப்பு பாணி |
| `message` | `string` | காண்பிக்க வேண்டிய உரை |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

கீழ் நிலை பட்டியில் ஒரு நிலையான உரை உருப்படியை சேர்க்கவும்.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | இந்த நிலை உருப்படியின் தனிப்பட்ட ID |
| `text` | `string` | காண்பிக்க வேண்டிய உரை |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — நிலையான சேமிப்பு

பிளக்கின் அமைப்புகள் நிரந்தரமாக `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` இல் சேமிக���கப்படுகின்றன.

#### `settings.get(key)`

சேமிக்கப்பட்ட மதிப்பை படிக்கவும்.
§§§CHUNK_SEPARATOR§§§
மதிப்பு கிடைக்காதால் `undefined` ஐ திருப்புகிறது.

#### `settings.set(key, value)`

ஒரு மதிப்பை சேமிக்கவும். உரைகள், எண்கள், பூலியன்கள், அணி மற்றும் பொருட்களை ஆதரிக்கிறது.
§§§CHUNK_SEPARATOR§§§
**உதாரணம்: பயனர் விருப்பங்களை நினைவில் வைக்கவும்**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — AI ஒருங்கிணைப்பு

> **நிலை: விரைவில் வருகை** — AI API வரையறுக்கப்பட்டுள்ளது ஆனால் Soomy க்கு இன்னும் இணைக்கப்படவில்லை. தற்போது `{ response: 'AI not yet connected' }` ஐ திருப்புகிறது. முழுமையான AI ஒருங்கிணைப்பு எதிர்கால வெளியீட்டிற்கு திட்டமிடப்பட்டுள்ளது.

#### `ai.chat(messages, options?)`

AI உதவியாளர் (Soomy) க்கு செய்திகளை அனுப்பவும்.
§§§CHUNK_SEPARATOR§§§
---

## Part 3: Webviews உடன் தனிப்பயனாக்கப்பட்ட UI உருவாக்குதல்

`openWebview()` API மூலம் HTML, CSS மற்றும் JavaScript உடன் டாஷ்போர்ட் UI களை உருவாக்கலாம் — அனைத்தும் ஒரு பாப் அப் ஜன்னலில் உள்ளன.

> **முக்கிய கட்டுப்பாடு:** Webviews **காண்பிக்க மட்டுமே**. அவை பிளக்கின் API களை ( `ctx.settings`, `ctx.terminal`, மற்றும் பிற) அழைக்க முடியாது. அனைத்து பயனர் செயல்பாடுகளுக்கும் பக்க பட்டன் களைப் பயன்படுத்தவும், தற்போதைய நிலையை காண்பிக்க `openWebview()` ஐப் பயன்படுத்தவும். நீங்கள் இடையீட்டு அம்சங்களை தேவைப்பட்டால், அவற்றைப் பக்க பட்டன் களில் இருந்து தூண்டவும் மற்றும் காட்சியை புதுப்பிக்க மீண்டும் webview ஐ திறக்கவும்.

### மாதிரி: டெர்மினல் கட்டளை → வெளியீட்டை பகுப்பாய்வு செய்யவும் → HTML இல் காண்பிக்கவும்

இது மிகவும் பொதுவான பிளக்கின் மாதிரி. நீங்கள் ஒரு கட்டளையை இயக்குகிறீர்கள், முடிவைப் பகுப்பாய்வு செய்கிறீர்கள், மற்றும் அதை காட்சியாகக் காண்பிக்கிறீர்கள்.
§§§CHUNK_SEPARATOR§§§
### மாதிரி: தானாக புதுப்பிக்கும் இடைமுகம்
§§§CHUNK_SEPARATOR§§§
### மாதிரி: Webview இல் அமைப்புகளை காண்பித்தல்

> **குறிப்பு:** Webviews காண்பிக்க மட்டுமே — அவை பிளக்கின் API களை அழைக்க முடியாது. அமைப்புகளை மாற்ற `ctx.settings` ஐ உங்கள் பக்க பட்டன் கையாளர்களில் பயன்படுத்தவும், மற்றும் தற்போதைய நிலையை காண்பிக்க `openWebview()` ஐப் பயன்படுத்தவும்.
§§§CHUNK_SEPARATOR§§§
---

## Part 4: உங்கள் பிளக்கினை வெளியிடுதல்

### படி 1: உள்ளூர் சோதனை

1. உங்கள் பிளக்கினை `~/.wia-soom/plugins/{your-plugin}/` இல் நகலெடுக்கவும்
2. WIA SOOM ஐ மறுதொடக்கம் செய்யவும்
3. இது வேலை செய்கிறதா என்பதை உறுதி செய்யவும்: பக்க பட்டன் தோன்றுகிறது, அம்சங்கள் சரியாக வ���லை செய்கின்றன
4. எல்லை நிலைகளை சோதிக்கவும்: எந்த டெர்மினல் இணைக்கப்படவில்லை என்றால் என்ன ஆகும்?

### படி 2: சமர்ப்பிக்க தயாராக இருக்கவும்

உங்கள் பிளக்கின் கோப்புறை உள்ளடக்க வேண்டும்:
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
**தேவையான `package.json` புலங்கள்:**

| புலம் | விளக்கம் | எடுத்துக்காட்டு |
|-------|-------------|---------|
| `name` | தனித்துவமான kebab-case அடையாளம் | `"my-awesome-plugin"` |
| `version` | செம்மையான பதிப்பு | `"1.0.0"` |
| `description` | ஒரு வரி விளக்கம் | `"Monitors nginx access logs in real-time"` |
| `author` | உங்கள் பெயர் | `"John Doe"` |
| `main` | நுழைவுப் புள்ளி | `"index.js"` |

**விருப்ப புலங்கள்:**

| புலம் | விளக்கம் |
|-------|-------------|
| `license` | உரிமம் வகை (MIT பரிந்துரைக்கப்படுகிறது) |
| `keywords` | தேடல் குறிச்சொற்களின் அடுக்கு |
| `soom.minVersion` | தேவையான குறைந்தபட்ச WIA SOOM பதிப்பு |

### படி 3: பிளக்கின் பதிவேட்டுக்கு சமர்ப்பிக்கவும்

1. ****Package** your plugin as a ZIP file
2. **Add** உங்கள் பிளக்கினை `plugins/{your-plugin-name}/` இல்
3. **Submit** ஒரு Pull Request

### படி 4: மதிப்பீடு மற்றும் ஒப்புதல்

நாங்கள் ஒவ்வொரு பிளக்கினையும் மதிப்பீடு செய்கிறோம்:

- **பாதுகாப்பு** — ஆபத்தான APIகள் இல்லை (பார்க்கவும் [Security Rules](#security-rules))
- **தரம்** — இது வேலை செய்கிறதா? குறியீடு சுத்தமாக இருக்கிறதா?
- **பயன்பாடு** — இது ஒரு உண்மையான பிரச்சினையை தீர்க்கிறதா?

ஒப்புதலுக்குப் பிறகு:
1. உங்கள் பிளக்கின் `registry.json` இல் சேர்க்கப்படுகிறது
2. `dist/` இல் ஒரு ZIP தொகுப்பு உருவாக்கப்படுகிறது
3. உங்கள் பிளக்கின் அனைத்து WIA SOOM பயனர்களுக்காக **Plugin Store** இல் தோன்றுகிறது!

---

## பகுதி 5: சிறந்த நடைமுறைகள்

### பாதுகாப்பு விதிகள்

இந்த விதிகள் **கட்டாயம்**. அவற்றை மீறும் பிளக்குகள் நிராகரிக்கப்படும்.

| விதி | ஏன் |
|------|-----|
| **எப்போது** `eval()` அல்லது `new Function()` ஐப் பயன்படுத்த வேண்டாம் | குறியீடு ஊடுருவல் ஆபத்து |
| **எப்போது** `child_process`, `exec()`, `spawn()` ஐப் பயன்படுத்த வேண்டாம் | கட்டளைகளுக்கு `ctx.terminal.send()` ஐ மட்டும் பயன்படுத்தவும் |
| **எப்போது** வெளிப்புற URLs ஐப் பெற வேண்டாம் | விலக்கு: `wiasoom.com` API முடிவுகள் |
| **எப்போது** `process.env` ஐ அணு��� வேண்டாம் | சுற்றுப்புற மாறிகள் ரகசியங்களை உள்ளடக்கலாம் |
| **எப்போது** `require('fs')` ஐ நேரடியாகப் பயன்படுத்த வேண்டாம் | சேமிப்புக்கு `ctx.settings` ஐ, கோப்பு மாற்றத்திற்கு `ctx.sftp` ஐப் பயன்படுத்தவும் |
| **எப்போது** npm வெளிப்புற தொகுப்புகளைப் பயன்படுத்த வேண்டாம் | தூய JavaScript மட்டுமே — node_modules இல்லை |
| **கட்டாயம்** அனைத்து தொலைக்காட்சி கட்டளைகளுக்காக `ctx.terminal.send()` ஐப் பயன்படுத்தவும் | இது பாதுகாப்பான SSH சேனலின் வழியாக செல்கிறது |
| **கட்டாயம்** `deactivate()` இல் சுத்தம் செய்யவும் | கேட்குபவர்களை அகற்றவும், இடைவெளிகளை தெளிவுபடுத்தவும் |

### பிழை கையாளுதல்

எப்போதும் ஆபத்தான செயல்களை try/catch இல் சுற்றிக்கொள்:
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
### deactivate() இல் சுத்தம் செய்யவும்

உங்கள் பிளக்கின் இடைவெளிகள், கேட்குபவர்கள், அல்லது சந்தாக்களை உருவாக்கினால் — அவற்றை சுத்தம் செய்யவும்:
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
### i18n ஆதரவு

WIA SOOM 254 மொழிகளை ஆதரிக்கிறது. உங்கள் பிளக்கின் லேபிள் மொழிபெயர்க்கக்கூடியதாக இருக்க, எளிய அணுகுமுறை பயன்படுத்தவும்:
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

## பகுதி 6: உண்மையான உலக எடுத்துக்காட்டுகள்

### எடுத்துக்காட்டு 1: சர்வர் டிஸ்க் சரிபார்ப்பவர்

தொலைக்காட்சி சர்வரில் `df -h` ஐ இயக்குகிறது மற்றும் நிலுவையில் ��ள்ள/கிடைக்கும் இடத்தை நிலைத் தட்டில் காட்டுகிறது.
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### எடுத்துக்காட்டு 2: TODO மேலாளர்

ஒரு TODO பட்டியலை நிர்வகிக்கும் பிளக்கின், நிலையான சேமிப்புக்கு அமைப்புகளை மற்றும் காட்சிக்காக ஒரு வலைக்காட்சி பயன்படுத்துகிறது.

> **வடிவமைப்பு மாதிரி:** வலைக்காட்சிகள் நேரடியாக பிளக்கின் APIகளை அழைக்க முடியாததால், இந்த பிளக்கின் "ஸ்நாப்ஷாட்" அணுகுமுறையைப் பயன்படுத்துகிறது — இது அமைப்புகளில் இருந்து TODOகளைப் படிக்கிறது, அவற்றைப் படிக்கக்கூடிய HTML ஆக உருவாக்குகிறது, மற்றும் உருப்படிகளைச் சேர்க்கும் பக்கத்திற்கேற்ப நடவடிக்கைகளை வழங்குகிறது. வலைக்காட்சி ஒரு **காட்சி** அடுக்கு, தொடர்புடைய வடிவம் அல்ல.
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

### எடுத்துக்காட்டு 3: பிழை கண்காணிப்பவர்

தொலைக்காட்சி வெளியீட்டை கண்காணிக்கிறது மற்றும் குறிப்பிட்ட மாதிரிகள் கண்டுபிடிக்கப்பட்டால் ஒரு அறிவிப்பை அனுப்புகிறது.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
---

## அத்தியாயம்: வகைகள் & சின்னங்கள்

### பிளக்கின் வகைகள் (29)

இவை உங்கள் `package.json` `keywords` இல் அல்லது பதிவு செய்யும்போது பயன்படுத்தவும்:

| வகை | விளக்கம் |
|------|----------|
| `server` | பொதுவான சர்வர் மேலாண்மை |
| `devtools` | மேம்பாட்டு கருவிகள் |
| `calculator` | கணக்கீடுகள் மற்றும் மாற்றிகள் |
| `simulator` | சிமுலேட்டர்கள் |
| `game` | டெர்மினல் விளையாட்டுகள் |
| `business` | வணிக கருவிகள் |
| `security` | பாதுகாப்பு மற்றும் ஆய்வு |
| `web` | வலை சர்வர் மேலாண்மை |
| `education` | கல்வி கருவிகள் |
| `health` | ஆரோ��்கியம் தொடர்பான கருவிகள் |
| `islamic` | இஸ்லாமிய கருவிகள் (பிரார்த்தனை நேரங்கள், முதலியன) |
| `science` | அறிவியல் கருவிகள் |
| `quantum` | க்வாண்டம் கணினி கருவிகள் |
| `ai` | AI-ஆயிரம் கருவிகள் |
| `biotech` | உயிரியல் தொழில்நுட்ப கருவிகள் |
| `space` | விண்வெளி மற்றும் விண்வெளியியல் கருவிகள் |
| `network` | நெட்வொர்க் கருவிகள் |
| `database` | தரவுத்தளம் மேலாண்மை |
| `monitoring` | சர்வர் கண்காணிப்பு |
| `devops` | DevOps மற்றும் CI/CD |
| `utility` | பொதுவான பயன்பாடுகள் |
| `design` | வடிவமைப்பு கருவிகள் |
| `ecommerce` | மின் வர்த்தகம் கருவிகள் |
| `automation` | தானியங்கி கருவிகள் |
| `kpop` | K-pop தொடர்பான கருவிகள் |
| `accessibility` | அணுகல் கருவிகள் |
| `analytics` | பகுப்பாய்வு மற்றும் அறிக்கையிடல் |
| `wia` | WIA சூழல் கருவிகள் |
| `all` | அனைத்து வகைகளிலும் தோன்றுகிறது |

### பரிந்துரைக்கப்பட்ட சின்னங்கள் (Lucide)

| சின்னத்தின் பெயர் | பயன்படுத்தவும் |
|------------------|---------------|
| `server` | சர்வர் மேலாண்மை |
| `shield` | பாதுகாப்பு |
| `database` | தரவுத்தளம் |
| `activity` | கண்காணிப்பு |
| `terminal` | டெர்மினல் கருவிகள் |
| `code` | மேம்பாடு |
| `hard-drive` | டிஸ்க்/சேமிப்பு |
| `network` | நெட்வொர்கிங் |
| `lock` | அங்கீகாரம்/குறியாக்கம் |
| `eye` | பார்க்கing/கண்காணிப்பு |
| `check-square` | பணிகள்/TODO |
| `layout-dashboard` | டாஷ்போர்டுகள் |
| `settings` | கட்டமைப்பு |
| `zap` | தானியக்கம் |
| `globe` | வலை/உலகளாவிய |

எல்லா 1,500+ சின்னங்களையும் உலாவவும்: [lucide.dev/icons](https://lucide.dev/icons)

---

## உதவி தேவைவா?

- **GitHub பிரச்சினைகள்:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **பிளக்கின் பிரச்சினைகள்:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **எடுத்துக்காட்டு பிளக்குகள்:** [Website](https://wiasoom.com)
- **வலைத்தளம்:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>சிறந்த ஒன்றை உருவாக்குங்கள். அதை உலகத்துடன் பகிருங்கள்.</em></p>
<p align="center"><em>— WIA SOOM குழு</em></p>
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
