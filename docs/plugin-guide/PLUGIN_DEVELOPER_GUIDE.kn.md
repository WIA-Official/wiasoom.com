<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM ಪ್ಲಗಿನ್ ಡೆವಲಪರ್ ಮಾರ್ಗದರ್ಶಿ</h1>
<p align="center"><strong>5 ನಿಮಿಷಗಳಲ್ಲಿ ನಿಮ್ಮದೇ ಆದ ಪ್ಲಗಿನ್ ಅನ್ನು ನಿರ್ಮಿಸಿ.</strong></p>
<p align="center">ಶಕ್ತಿಯುತ ಸರ್ವರ್ ಟೂಲ್‌ಗಳು, ಡ್ಯಾಶ್‌ಬೋರ್ಡ್‌ಗಳು ಮತ್ತು ಸ್ವಯಂಚಾಲಿತಗಳನ್ನು ರಚಿಸಿ — WIA SOOM ಒಳಗೆ ನೇರವಾಗಿ.</p>

---

## ವಿಷಯಗಳ ಪಟ್ಟಿಕೆ

- [ಭಾಗ 1: ತ್ವರಿತ ಪ್ರಾರಂಭ — ನಿಮ್ಮ ಮೊದಲ ಪ್ಲಗಿನ್ 5 ನಿಮಿಷಗಳಲ್ಲಿ](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ಭಾಗ 2: ಪ್ಲಗಿನ್ ಕಾನ್ಟೆಕ್ಸ್ಟ್ API ಉಲ್ಲೇಖ](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ಭಾಗ 3: ವೆಬ್‌ವೀವ್‌ಗಳೊಂದಿಗೆ ಕಸ್ಟಮ್ UI ನಿರ್ಮಾಣ](#part-3-building-custom-ui-with-webviews)
- [ಭಾಗ 4: ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಅನ್ನು ಪ್ರಕಟಿಸುವುದು](#part-4-publishing-your-plugin)
- [ಭಾಗ 5: ಉತ್ತಮ ಅಭ್ಯಾಸಗಳು](#part-5-best-practices)
- [ಭಾಗ 6: ವಾಸ್ತವಿಕ ಉದಾಹರಣೆಗಳು](#part-6-real-world-examples)
- [ಅಪೆಂಡಿಕ್ಸ್: ವರ್ಗಗಳು & ಐಕಾನ್‌ಗಳು](#appendix-categories--icons)

---

## ಭಾಗ 1: ತ್ವರಿತ ಪ್ರಾರಂಭ — ನಿಮ್ಮ ಮೊದಲ ಪ್ಲಗಿನ್ 5 ನಿಮಿಷಗಳಲ್ಲಿ

### ನೀವು ಏನು ನಿರ್ಮಿಸುತ್ತೀರಿ

ಬದಿಯಲ್ಲಿ ಬಟನ್ ಅನ್ನು ಸೇರಿಸುವ "ಹಲೋ ವರ್ಲ್ಡ್" ಪ್ಲಗಿನ್. ಕ್ಲಿಕ್ ಮಾಡಿದಾಗ, ಇದು ಒಂದು ಅಧಿಸೂಚನೆಯನ್ನು ತೋರಿಸುತ್ತದೆ.

### ಹಂತ 1: ಪ್ಲಗಿನ್ ಫೋಲ್ಡರ್ ಅನ್ನು ರಚಿಸಿ
§§§CHUNK_SEPARATOR§§§
### ಹಂತ 2: package.json ಅನ್ನು ರಚಿಸಿ
§§§CHUNK_SEPARATOR§§§
**ಅಗತ್ಯ ಕ್ಷೇತ್ರಗಳು:** `name`, `version`, `description`, `author`, `main`

### ಹಂತ 3: index.js ಅನ್ನು ರಚಿಸಿ
§§§CHUNK_SEPARATOR§§§
### ಹಂತ 4: WIA SOOM ಅನ್ನು ಪುನಾರಂಭಿಸಿ

ಆಪ್ ಅನ್ನು ಪುನಾರಂಭಿಸಿ (ಅಥವಾ ಸೆಟ್ಟಿಂಗ್‌ಗಳಲ್ಲಿ → ಪ್ಲಗಿನ್‌ಗಳಲ್ಲಿ ಪ್ಲಗಿನ್ ಅನ್ನು ಆಫ್/ಆನ್ ಮಾಡಿ).

ನೀವು ಬದಿಯಲ್ಲಿ **"ಹಲೋ ವರ್ಲ್ಡ್"** ಬಟನ್ ಅನ್ನು ನೋಡಬೇಕು. ಅದನ್ನು ಕ್ಲಿಕ್ ಮಾಡಿ — ನೀವು ಯಶಸ್ಸಿನ ಅಧಿಸೂಚನೆಯನ್ನು ಕಾಣುತ್ತೀರಿ!

### ಇದು ಹೇಗೆ ಕಾರ್ಯನಿರ್ವಹಿಸುತ್ತದೆ
§§§CHUNK_SEPARATOR§§§
---

## ಭಾಗ 2: ಪ್ಲಗಿನ್ ಕಾನ್ಟೆಕ್ಸ್ಟ್ API ಉಲ್ಲೇಖ

ನಿಮ್ಮ `activate(context)` ಕಾರ್ಯವನ್ನು ಕರೆ ಮಾಡಿದಾಗ, `context` (ಅಥವಾ `ctx`) ಈ APIಗಳನ್ನು ಒದಗಿಸುತ್ತದೆ:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — ದೂರದ ಸರ್ವರ್‌ಗಳಲ್ಲಿ ಆಜ್ಞೆಗಳನ್ನು ಕಾರ್ಯಗತಗೊಳಿಸಿ

#### `terminal.send(sessionId, data)`

ಸಕ್ರಿಯ ಟರ್ಮಿನಲ್ ಸೆಶನ್‌ಗೆ ಆಜ್ಞೆ (ಅಥವಾ ಯಾವುದೇ ಡೇಟಾ) ಕಳುಹಿಸಿ.

| ಪ್ಯಾರಾಮೀಟರ್ | ಪ್ರಕಾರ | ವಿವರಣೆ |
|--------------|--------|----------|
| `sessionId`  | `string` | ಕಳುಹಿಸಲು ಟರ್ಮಿನಲ್ ಸೆಶನ್ |
| `data`       | `string` | ಕಳುಹಿಸಲು ಆಜ್ಞೆ ಅಥವಾ ಡೇಟಾ |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

ಟರ್ಮಿನಲ್ ಸೆಶನ್‌ನ ಎಲ್ಲಾ ಔಟ್‌ಪುಟ್‌ಗಳಿಗೆ ಚಂದಾ ನೀಡಿ. **ಅನ್‌ಸಬ್‌ಸ್ಕ್ರೈಬ್ ಕಾರ್ಯವನ್ನು** ಹಿಂತಿರುಗಿಸುತ್ತದೆ.

| ಪ್ಯಾರಾಮೀಟರ್ | ಪ್ರಕಾರ | ವಿವರಣೆ |
|--------------|--------|----------|
| `sessionId`  | `string` | ವೀಕ್ಷಿಸಲು ���ರ್ಮಿನಲ್ ಸೆಶನ್ |
| `callback`   | `(data: string) => void` | ಪ್ರತಿಯೊಂದು ಔಟ್‌ಪುಟ್‌ನ ಚುಟುಕುಗಳೊಂದಿಗೆ ಕರೆಯಲಾಗುತ್ತದೆ |
| **ಹಿಂತಿರುಗಿಸುತ್ತದೆ** | `() => void` | ಕೇಳುವುದು ನಿಲ್ಲಿಸಲು ಇದನ್ನು ಕರೆ ಮಾಡಿ |
§§§CHUNK_SEPARATOR§§§
**ಮಹತ್ವದ:** ಯಾವಾಗಲೂ ಅನ್‌ಸಬ್‌ಸ್ಕ್ರೈಬ್ ಕಾರ್ಯವನ್ನು ಉಳಿಸಿ ಮತ್ತು ಮೆಮೊರಿ ಲೀಕ್‌ಗಳನ್ನು ತಡೆಯಲು `deactivate()` ನಲ್ಲಿ ಇದನ್ನು ಕರೆ ಮಾಡಿ.

---

### `ctx.sftp` — ಫೈಲ್ ವರ್ಗಾವಣೆ

> **ಸ್ಥಿತಿ: ಶೀಘ್ರದಲ್ಲೇ ಬರುವುದಾಗಿದೆ** — SFTP API ಅನ್ನು ವ್ಯಾಖ್ಯಾನಿಸಲಾಗಿದೆ ಆದರೆ ಆಪ್‌ನ SFTP ಎಂಜಿನ್‌ಗೆ ಇನ್ನೂ ಸಂಪರ್ಕಿಸಲಾಗಿಲ್ಲ. `list()` ಪ್ರಸ್ತುತ ಖಾಲಿ ಅರೆವನ್ನು ಹಿಂತಿರುಗಿಸುತ್ತದೆ, ಮತ್ತು `upload()`/`download()` ಕಾರ್ಯವಿಲ್ಲ. ಇದು ಭವಿಷ್ಯದ ಬಿಡುಗಡೆಗಳಲ್ಲಿ ಸಂಪೂರ್ಣವಾಗಿ ಕಾರ್ಯಗತಗೊಳ್ಳುತ್ತದೆ. ಈಗ, `scp` ಅಥವಾ `rsync` ಆಜ್ಞೆಗಳೊಂದಿಗೆ `ctx.terminal.send()` ಅನ್ನು ಪರ್ಯಾಯವಾಗಿ ಬಳಸಿರಿ.

#### `sftp.list(sessionId, path)`

ದೂರದ ಡೈರೆಕ್ಟರಿಯಲ್ಲಿನ ಫೈಲ್‌ಗಳನ್ನು ಪಟ್ಟಿಮಾಡಿ.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

ಸ್ಥಳೀಯ ಯಂತ್ರದಿಂದ ದೂರದ ಸರ್ವರ್‌ಗೆ ಫೈಲ್ ಅನ್ನು ಅಪ್ಲೋಡ್ ಮಾಡಿ.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

ದೂರದ ಸರ್ವರ್‌ನಿಂದ ಸ್ಥಳೀಯ ಯಂತ್ರಕ್ಕೆ ಫೈಲ್ ಅನ್ನು ಡೌನ್‌ಲೋಡ್ ಮಾಡಿ.
§§§CHUNK_SEPARATOR§§§
**ಪರ್ಯಾಯ (SFTP API ಲೈವ್ ಆಗುವ ತನಕ):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — ಬಳಕೆದಾರ ಇಂಟರ್ಫೇಸ್

#### `ui.addSidebarButton(options)`

WIA SOOM ಬದಿಯಲ್ಲಿ ಬಟನ್ ಅನ್ನು ಸೇರಿಸಿ.

| ಆಯ್ಕೆ | ಪ್ರಕಾರ | ಅಗತ್ಯ | ವಿವರಣೆ |
|-------|--------|-------|----------|
| `id`   | `string` | ಇಲ್ಲ | ವಿಶಿಷ್ಟ ID (ಪ್ಲಗಿನ್ ಹೆಸರಿನಲ್ಲಿ ಡೀಫಾಲ್ಟ್) |
| `icon` | `string` | ಹೌದು | Lucide ಐಕಾನ್ ಹೆಸರು (ಉದಾ: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | ಹೌದು | ಬದಿಯಲ್ಲಿ ತೋರಿಸುವ ಬಟನ್ ಪಠ್ಯ |
| `onClick` | `() => void` | ಹೌದು | ಬಟನ್ ಕ್ಲಿಕ್ ಮಾಡಿದಾಗ ಕರೆಯುವ ಕಾರ್ಯ |
§§§CHUNK_SEPARATOR§§§
**ಐಕಾನ್ ಉಲ್ಲೇಖ:** ಎಲ್ಲಾ ಲಭ್ಯವಿರುವ ಐಕಾನ್‌ಗಳನ್ನು [lucide.dev/icons](https://lucide.dev/icons) ನಲ್ಲಿ ಬ್ರೌಸ್ ಮಾಡಿ

> **ಸಂಗತತೆಯ ಟಿಪ್ಪಣಿ:** ಕೆಲವು ಹಳೆಯ ಪ್ಲಗಿನ್‌ಗಳು `addSidebarButton(id, icon, label, onClick)` ಎಂಬ ಸ್ಥಾನೀಯ ಆರ್ಗ್ಯುಮೆಂಟ್‌ಗಳನ್ನು ಬಳಸುತ್ತವೆ. ಅಧಿಕೃತ API ಮೇಲಿನಂತೆ **ಆಯ್ಕೆ ವಸ್ತುವನ್ನು** ಬಳಸುತ್ತದೆ. ಹೊಸ ಪ್ಲಗಿನ್‌ಗಳಿಗೆ ಸದಾ ವಸ್ತು ಶ್ರೇಣಿಯನ್ನು ಬಳಸಿರಿ.

#### `ui.openWebview(options)`

ಕಸ್ಟಮ್ HTML ವಿಷಯದೊಂದಿಗೆ ಪಾಪ್‌ಅಪ್ ಕಿಟಕಿ ತೆರೆಯಿರಿ. ಇದು ನೀವು ಶ್ರೀಮಂತ UIಗಳನ್ನು ನಿರ್ಮಿಸುವ ರೀತಿಯಾಗಿದೆ.

| ಆಯ್ಕೆ | ಪ್ರಕಾರ | ವಿವರಣೆ |
|-------|--------|----------|
| `title` | `string` | ಕಿಟಕಿಯ ಶೀರ್ಷಿಕೆ |
| `html`  | `string` | ರೆಂಡರ್ ಮಾಡಲು ಸಂಪೂರ್ಣ HTML ವಿಷಯ |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> See [Part 3](#part-3-building-custom-ui-with-webviews) for advanced webview patterns.

#### `ui.showNotification(type, message)`

ಒಂದು ಟೋಸ್ಟ್ ನೋಟಿಫಿಕೇಶನ್ ತೋರಿಸಿ.

| ಪ್ಯಾರಾಮೀಟರ್ | ಪ್ರಕಾರ | ವಿವರಣೆ |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | ನೋಟಿಫಿಕೇಶನ್ ಶೈಲಿ |
| `message` | `string` | ತೋರಿಸಲು ಪಠ್ಯ |

§§§CHUNK_SEPARATOR§§§

#### `ui.addStatusBarItem(id, text)`

ಕೆಳಗಿನ ಸ್ಥಿತಿಬಾರ್‌ಗೆ ಶಾಶ್ವತ ಪಠ್ಯ ಐಟಂ ಸೇರಿಸಿ.

| ಪ್ಯಾರಾಮೀಟರ್ | ಪ್ರಕಾರ | ವಿವರಣೆ |
|-----------|------|-------------|
| `id` | `string` | ಈ ಸ್ಥಿತಿ ಐಟಂಗೆ ವಿಶಿಷ್ಟ ID |
| `text` | `string` | ತೋರಿಸಲು ಪಠ್ಯ |

§§§CHUNK_SEPARATOR§§§

---

### `ctx.settings` — ಶಾಶ್ವತ ಸಂಗ್��ಹಣೆ

ಪ್ಲಗಿನ್ ಸೆಟ್ಟಿಂಗ್‌ಗಳನ್ನು ಶಾಶ್ವತವಾಗಿ `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` ನಲ್ಲಿ ಸಂಗ್ರಹಿಸಲಾಗುತ್ತದೆ.

#### `settings.get(key)`

ಉಳಿಸಲಾಗಿರುವ ಮೌಲ್ಯವನ್ನು ಓದಿ.

§§§CHUNK_SEPARATOR§§§

ಕೀ ಇರುವುದಿಲ್ಲದಿದ್ದರೆ `undefined` ಅನ್ನು ಹಿಂತಿರುಗಿಸುತ್ತದೆ.

#### `settings.set(key, value)`

ಒಂದು ಮೌಲ್ಯವನ್ನು ಉಳಿಸಿ. ಸ್ಟ್ರಿಂಗ್‌ಗಳು, ಸಂಖ್ಯೆಗಳು, ಬೂಲ್‌ಗಳು, ಅರೆಗಳು ಮತ್ತು ವಸ್ತುಗಳನ್ನು ಬೆಂಬಲಿಸುತ್ತದೆ.

§§§CHUNK_SEPARATOR§§§

**ಉದಾಹರಣೆ: ಬಳಕೆದಾರರ ಆಯ್ಕೆಯನ್ನು ನೆನೆಸು**

§§§CHUNK_SEPARATOR§§§

---

### `ctx.ai` — AI ಇಂಟಿಗ್ರೇಶನ್

> **ಸ್ಥಿತಿ: ಶೀಘ್ರದಲ್ಲೇ ಬರುವುದಾಗಿದೆ** — AI API ಅನ್ನು ವ್ಯಾಖ್ಯಾನಿಸಲಾಗಿದೆ ಆದರೆ Soomy ಗೆ ಸಂಪರ್ಕಿತವಾಗಿಲ್ಲ. ಪ್ರಸ��ತುತ `{ response: 'AI not yet connected' }` ಅನ್ನು ಹಿಂತಿರುಗಿಸುತ್ತದೆ. ಸಂಪೂರ್ಣ AI ಇಂಟಿಗ್ರೇಶನ್ ಭವಿಷ್ಯದ ಬಿಡುಗಡೆಗಾಗಿ ಯೋಜಿಸಲಾಗಿದೆ.

#### `ai.chat(messages, options?)`

AI ಸಹಾಯಕರಿಗೆ (Soomy) ಸಂದೇಶಗಳನ್ನು ಕಳುಹಿಸಿ.

§§§CHUNK_SEPARATOR§§§

---

## Part 3: Webviews ನೊಂದಿಗೆ ಕಸ್ಟಮ್ UI ನಿರ್ಮಾಣ

`openWebview()` API ನಿಮಗೆ HTML, CSS, ಮತ್ತು JavaScript ಬಳಸಿ ಡ್ಯಾಶ್‌ಬೋರ್ಡ್ UIs ಅನ್ನು ನಿರ್ಮಿಸಲು ಅವಕಾಶ ನೀಡುತ್ತದೆ — ಎಲ್ಲಾ ಪಾಪ್‌ಅಪ್ ಕಿಟಕಿಯ ಒಳಗೆ.

> **ಪ್ರಮುಖ ನಿರ್ಬಂಧ:** Webviews **ಪ್ರದರ್ಶನ-ಮಾತ್ರ**. ಅವು ಪ್ಲಗಿನ್ APIs ಗೆ ಹಿಂತಿರುಗಿಸಲು ಸಾಧ್ಯವಿಲ್ಲ (`ctx.settings`, `ctx.terminal`, ಇತ್ಯಾದಿ). ಎಲ್ಲಾ ಬಳಕೆದಾರ ಕ್ರಿಯೆಗಳಿಗೆ ಪಕ್ಕದ ಬಟನ್‌ಗಳನ್ನು ಬಳಸಿರಿ, ಮತ್ತು ಪ್ರಸ್ತುತ ಸ್ಥಿತಿಯನ್ನು ತೋ���ಿಸಲು `openWebview()` ಅನ್ನು ಬಳಸಿರಿ. ನೀವು ಪರಸ್ಪರ ವೈಶಿಷ್ಟ್ಯಗಳನ್ನು ಅಗತ್ಯವಿದ್ದರೆ, ಅವುಗಳನ್ನು ಪಕ್ಕದ ಬಟನ್‌ಗಳಿಂದ ಪ್ರೇರೇಪಿಸಿ ಮತ್ತು ಪ್ರದರ್ಶನವನ್ನು ನವೀಕರಿಸಲು webview ಅನ್ನು ಪುನಃ ತೆರೆಯಿರಿ.

### ಮಾದರಿ: ಟರ್ಮಿನಲ್ ಕಮಾಂಡ್ → ಪಾರ್ಸ್ ಔಟ್‌ಪುಟ್ → HTML ನಲ್ಲಿ ತೋರಿಸಿ

ಇದು ಅತ್ಯಂತ ಸಾಮಾನ್ಯ ಪ್ಲಗಿನ್ ಮಾದರಿ. ನೀವು ಒಂದು ಕಮಾಂಡ್ ಅನ್ನು ನಿರ್ವಹಿಸುತ್ತೀರಿ, ಫಲಿತಾಂಶವನ್ನು ಪಾರ್ಸ್ ಮಾಡುತ್ತೀರಿ, ಮತ್ತು ದೃಶ್ಯವಾಗಿ ತೋರಿಸುತ್ತೀರಿ.

§§§CHUNK_SEPARATOR§§§

### ಮಾದರಿ: ಸ್ವಯಂಚಾಲಿತ ನವೀಕರಣದೊಂದಿಗೆ ಪರಸ್ಪರ ಡ್ಯಾಶ್‌ಬೋರ್ಡ್

§§§CHUNK_SEPARATOR§§§

### ಮಾದರಿ: Webview ನಲ್ಲಿ ಸೆಟ್ಟಿಂಗ್‌ಗಳನ್ನು ತೋರಿಸುವುದು

> **ಗಮನ:** Webviews ಪ್ರದರ್ಶ��-ಮಾತ್ರ — ಅವು ಪ್ಲಗಿನ್ APIs ಗೆ ಹಿಂತಿರುಗಿಸಲು ಸಾಧ್ಯವಿಲ್ಲ. ಸೆಟ್ಟಿಂಗ್‌ಗಳನ್ನು ಬದಲಾಯಿಸಲು ನಿಮ್ಮ ಪಕ್ಕದ ಬಟನ್ ಹ್ಯಾಂಡ್ಲರ್‌ಗಳಲ್ಲಿ `ctx.settings` ಅನ್ನು ಬಳಸಿರಿ, ಮತ್ತು ಪ್ರಸ್ತುತ ಸ್ಥಿತಿಯನ್ನು ತೋರಿಸಲು `openWebview()` ಅನ್ನು ಬಳಸಿರಿ.

§§§CHUNK_SEPARATOR§§§

---

## Part 4: ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಅನ್ನು ಪ್ರಕಟಿಸುವುದು

### ಹಂತ 1: ಸ್ಥಳೀಯವಾಗಿ ಪರೀಕ್ಷಿಸಿ

1. ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಅನ್ನು `~/.wia-soom/plugins/{your-plugin}/` ಗೆ ನಕಲಿಸಿ
2. WIA SOOM ಅನ್ನು ಪುನರಾರಂಭಿಸಿ
3. ಇದು ಕಾರ್ಯನಿರ್ವಹಿಸುತ್ತೆ ಎಂದು ಖಚಿತಪಡಿಸಿಕೊಳ್ಳಿ: ಪಕ್ಕದ ಬಟನ್ ಕಾಣುತ್ತದೆ, ವೈಶಿಷ್ಟ್ಯಗಳು ಸರಿಯಾಗಿ ಕಾರ್ಯನಿರ್ವಹಿಸುತ್ತವೆ
4. ಎಡ್ಜ್ ಕೇಸ್‌ಗಳನ್ನು ಪರೀಕ್ಷಿಸಿ: ಯಾವುದೇ ಟರ್ಮಿನ��್ ಸಂಪರ್ಕಿತವಾಗಿಲ್ಲದಿದ್ದರೆ ಏನಾಗುತ್ತದೆ?

### ಹಂತ 2: ಸಲ್ಲಿಸಲು ತಯಾರಾಗಿರಿ

ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಫೋಲ್ಡರ್‌ನಲ್ಲಿ ಇವುಗಳನ್ನು ಒಳಗೊಂಡಿರಬೇಕು:
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
**ಅವಶ್ಯಕ `package.json` ಕ್ಷೇತ್ರಗಳು:**

| ಕ್ಷೇತ್ರ | ವಿವರಣೆ | ಉದಾಹರಣೆ |
|-------|-------------|---------|
| `name` | ವಿಶಿಷ್ಟ kebab-case ID | `"my-awesome-plugin"` |
| `version` | ಅರ್ಥಪೂರ್ಣ ಆವೃತ್ತಿ | `"1.0.0"` |
| `description` | ಒಂದು ಸಾಲಿನ ವಿವರಣೆ | `"Monitors nginx access logs in real-time"` |
| `author` | ನಿಮ್ಮ ಹೆಸರು | `"John Doe"` |
| `main` | ಪ್ರವೇಶ ಬಿಂದು | `"index.js"` |

**ಐಚ್ಛಿಕ ಕ್ಷೇತ್ರಗಳು:**

| ಕ್ಷೇತ್ರ | ವಿವರಣೆ |
|-------|-------------|
| `license` | ಪರವಾನಗಿ ಪ್ರಕಾರ (MIT ಶಿಫಾರಸು) |
| `keywords` | ಹುಡುಕಾಟ ಟ್ಯಾಗ್‌ಗಳ ಶ್ರೇಣೀಬದ್ಧತೆ |
| `soom.minVersion` | ಅಗತ್ಯವಿರುವ ಕನಿಷ್ಠ WIA SOOM ಆವೃತ್ತಿ |

### ಹಂತ 3: ಪ್ಲಗಿನ್ ರಿಜಿಸ್ಟ್ರಿಗೆ ಸಲ್ಲಿಸಿ

1. ****Package** your plugin as a ZIP file
2. **Add** ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಅನ್ನು `plugins/{your-plugin-name}/` ಗೆ
3. **Submit** a Pull Request

### ಹಂತ 4: ಪರಿಶೀಲನೆ ಮತ್ತು ಅನುಮೋದನೆ

ನಾವು ಪ್ರತಿಯೊಂದು ಪ್ಲಗಿನ್ ಅನ್ನು ಪರಿಶೀಲಿಸುತ್ತೇವೆ:

- **ಭದ್ರತೆ** — ಅಪಾಯಕಾರಿಯಲ್ಲದ APIs (ನೋಡು [ಭದ್ರತಾ ನಿಯಮಗಳು](#security-rules))
- **ಗುಣಮಟ್ಟ** — ಇದು ಕಾರ್ಯನಿರ್ವಹಿಸುತ್ತೆ? ಕೋಡ್ ಶುದ್ಧವೇ?
- **ಉಪಯುಕ್ತತೆ** — ಇದು ವಾಸ್ತವ ಸಮಸ್ಯೆಯನ್ನು ಪರಿಹರಿಸುತ್ತೆನಾ?

ಅನುಮೋದನೆಯ ನಂತರ:
1. ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಅನ್ನು `registry.json` ಗೆ ಸೇರಿಸಲಾಗುತ್ತದೆ
2. `dist/` ನಲ್ಲಿ ZIP ಬಂಡಲ್ ರಚಿಸಲಾಗುತ್ತದೆ
3. ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಎಲ್ಲಾ WIA SOOM ಬಳಕೆದಾರರಿಗೆ **Plugin Store** ನಲ್ಲಿ ಕಾಣಿಸುತ್ತದೆ!

---

## ಭಾಗ 5: ಉತ್ತಮ ಅಭ್ಯಾಸಗಳು

### ಭದ್ರತಾ ನಿಯಮಗಳು

ಈ ನಿಯಮಗಳು **ಕಡ್ಡಾಯ**. ಅವುಗಳನ್ನು ಉಲ್ಲಂಘಿಸುವ ಪ್ಲಗಿನ್‌ಗಳನ್ನು ನಿರಾಕರಿಸಲಾಗುತ್ತದೆ.

| ನಿಯಮ | ಏಕೆ |
|------|-----|
| **ಎಂದಿಗೂ** `eval()` ಅಥವಾ `new Function()` ಅನ್ನು ಬಳಸಬೇಡಿ | ಕೋಡ್ ಇಂಜೆಕ್ಷನ್ ಅಪಾಯ |
| **ಎಂದಿಗೂ** `child_process`, `exec()`, `spawn()` ಅನ್ನು ಬಳಸಬೇಡಿ | ಕಮಾಂಡ್‌ಗಳಿಗೆ ಮಾತ್ರ `ctx.terminal.send()` ಅನ್ನು ಬಳಸಿರಿ |
| **ಎಂದಿಗೂ** ಹೊರಗಿನ URLs ಅನ್ನು ಪಡೆಯಬೇಡಿ | ಹೊರತಾಗಿ: `wiasoom.com` API ಎಂಡ್ಪಾಯಿಂಟ್‌ಗಳು |
| **ಎಂದಿಗೂ** `process.env` ಗೆ ಪ್ರವೇಶಿಸಬೇಡಿ | ಪರಿಸರ ಚರಗಳು ರಹಸ್ಯಗಳನ್ನು ಒಳಗೊಂಡಿರಬಹುದು |
| **ಎಂದಿಗೂ** `require('fs')` ಅನ್ನು ನೇರವಾಗಿ ಬಳಸಬೇಡಿ | ಸಂಗ್ರಹಣೆಗೆ `ctx.settings` ಅನ್ನು ಬಳಸಿರಿ, ಫೈಲ್ ವರ್ಗಾವಣೆಗಾಗಿ `ctx.sftp` ಅನ್ನು ಬಳಸಿರಿ |
| **ಕಡ್ಡಾಯ** ಎಲ್ಲಾ ದೂರದ ಕಮಾಂಡ್‌ಗಳಿಗೆ `ctx.terminal.send()` ಅನ್ನು ಬಳಸಿ���ಿ | ಇದು ಭದ್ರ SSH ಚಾನೆಲ್ ಮೂಲಕ ಹೋಗುತ್ತದೆ |
| **ಕಡ್ಡಾಯ** `deactivate()` ನಲ್ಲಿ ಶುದ್ಧಗೊಳಿಸಿರಿ | ಶ್ರೋತಗಳನ್ನು ತೆಗೆದುಹಾಕಿ, ಅಂತರಗಳನ್ನು ಕ್ಲಿಯರ್ ಮಾಡಿ |

### ದೋಷ ನಿರ್ವಹಣೆ

ಹೆಚ್ಚಿನ ಅಪಾಯಕಾರಿಯಲ್ಲದ ಕಾರ್ಯಾಚರಣೆಗಳನ್ನು ಯಾವಾಗಲೂ try/catch ನಲ್ಲಿ ಲಿಪ್ತಗೊಳಿಸಿ:
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
### deactivate() ನಲ್ಲಿ ಶುದ್ಧಗೊಳಿಸುವುದು

ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಅಂತರಗಳು, ಶ್ರೋತಗಳು ಅಥವಾ ಚಂದಾದಾರಿಗಳನ್ನು ರಚಿಸಿದರೆ — ಅವುಗಳನ್ನು ಶುದ್ಧಗೊಳಿಸಿ:
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
### i18n ಬೆಂಬಲ

WIA SOOM 254 ಭಾಷೆಗಳನ್ನು ಬೆಂಬಲಿಸುತ್ತದೆ. ನಿಮ್ಮ ಪ್ಲಗಿನ್ ಲೇಬಲ್ ಅನ್ನು ಭಾಷಾಂತರಿಸಲು ಸಾಧ್ಯವಾಗಿಸಲು, ಸರಳ ವಿಧಾನವನ್ನು ಬಳಸಿರಿ:
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

## ಭಾಗ 6: ವಾಸ್ತವ ಜಗತ್��ಿನ ಉದಾಹರಣೆಗಳು

### ಉದಾಹರಣೆ 1: ಸರ್ವರ್ ಡಿಸ್ಕ್ ಚೆಕರ್

ದೂರದ ಸರ್ವರ್‌ನಲ್ಲಿ `df -h` ಅನ್ನು ನಿರ್ವಹಿಸುತ್ತದೆ ಮತ್ತು ಸ್ಥಿತಿಬಾರ್‌ನಲ್ಲಿ ಬಳಸಿದ/ಲಭ್ಯವಿರುವ ಸ್ಥಳವನ್ನು ತೋರಿಸುತ್ತದೆ.
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

### ಉದಾಹರಣೆ 2: TODO ನಿರ್ವಹಕ

ಸ್ಥಾಯೀ ಸಂಗ್ರಹಣೆಗೆ ಸೆಟಿಂಗ್‌ಗಳನ್ನು ಬಳಸುವ ಮತ್ತು ಪ್ರದರ್ಶನಕ್ಕಾಗಿ ವೆಬ್‌ವ್ಯೂ ಅನ್ನು ಬಳಸುವ TODO ಪಟ್ಟಿಯನ್ನು ನಿರ್ವಹಿಸುವ ಪ್ಲಗಿನ್.

> **ಡಿಸೈನ್ ಪ್ಯಾಟರ್ನ್:** ವೆಬ್‌ವ್ಯೂಗಳು ನೇರವಾಗಿ ಪ್ಲಗಿನ್ APIs ಅನ್ನು ಕರೆ ಮಾಡಲಾಗುವುದಿಲ್ಲ, ಈ ಪ್ಲಗಿನ್ "ಸ್ನಾಪ್‌ಶಾಟ್" ವಿಧಾನವನ್ನು ಬಳಸುತ್ತದೆ — ಇದು ಸೆಟಿಂಗ್‌ಗಳಿಂದ TODOಗಳನ್ನು ಓದುತ್ತದೆ, ಅವುಗಳನ್ನು ಓದಲು ಮಾತ್ರ HTML ರೂಪದಲ��ಲಿ ರೆಂಡರ್ ಮಾಡುತ್ತದೆ ಮತ್ತು ಐಟಮ್‌ಗಳನ್ನು ಸೇರಿಸಲು ಪಕ್ಕದ ಆಧಾರಿತ ಕ್ರಿಯೆಗಳನ್ನ��� ಒದಗಿಸುತ್ತದೆ. ವೆಬ್‌ವ್ಯೂ ಒಂದು **ಪ್ರದರ್ಶನ** ಹಂತ, ಪರಸ್ಪರ ರೂಪವಲ್ಲ.
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

### ಉದಾಹರಣೆ 3: ದೋಷ ವಾಚಕ

ಟರ್ಮಿನಲ್ ಔಟ್‌ಪುಟ್ ಅನ್ನು ಮೇಲ್ವಿಚಾರಣೆ ಮಾಡುತ್ತದೆ ಮತ್ತು ನಿರ್ದಿಷ್ಟ ಮಾದರಿಗಳು ಗುರುತಿಸಿದಾಗ ಸೂಚನೆ ಕಳುಹಿಸುತ್ತದೆ.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
---

## ಅಪೆಂಡಿಕ್ಸ್: ವರ್ಗಗಳು ಮತ್ತು ಐಕಾನ್ಸ್

### ಪ್ಲಗಿನ್ ವರ್ಗಗಳು (29)

ನೀವು ನಿಮ್ಮ `package.json` `keywords` ನಲ್ಲಿ ಅಥವಾ ನೋಂದಣಿಗೆ ಸಲ್ಲಿಸುವಾಗ ಈಗಳನ್ನು ಬಳಸಿರಿ:

| ವರ್ಗ | ವಿವರಣೆ |
|------|---------|
| `server` | ಸಾಮಾನ್ಯ ಸರ್ವರ್ ನಿರ್ವಹಣೆ |
| `devtools` | ಅಭಿವೃದ್ಧಿ ಸಾಧನಗಳು |
| `calculator` | ಲೆಕ್ಕಾಚಾರಗಳು ಮತ್ತು ಪರಿವರ್ತಕಗಳು |
| `simulator` | ಅನುಕರಣಗಳು |
| `game` | ಟರ್ಮಿನಲ್ ಆಟಗಳು |
| `business` | ವ್ಯಾಪಾರ ಸಾಧನಗಳು |
| `security` | ಭದ್ರತೆ ಮತ್ತು ಪರಿಶೀಲನೆ |
| `web` | ವೆಬ್ ಸರ್ವರ್ ನಿರ್ವಹಣೆ |
| `education` | ಶೈಕ್ಷಣಿಕ ಸಾಧನಗಳು |
| `health` | ಆರೋಗ್ಯ ��ಂಬಂಧಿತ ಸಾಧನಗಳು |
| `islamic` | ಇಸ್ಲಾಮಿಕ್ ಸಾಧನಗಳು (ಪ್ರಾರ್ಥನಾ ಸಮಯಗಳು, ಇತ್ಯಾದಿ) |
| `science` | ವೈಜ್ಞಾನಿಕ ಸಾಧನಗಳು |
| `quantum` | ಕ್ವಾಂಟಮ್ ಕಂಪ್ಯೂಟಿಂಗ್ ಸಾಧನಗಳು |
| `ai` | ಎಐ ಶಕ್ತಿಯ ಸಾಧನಗಳು |
| `biotech` | ಜೀವವಿಜ್ಞಾನ ಸಾಧನಗಳು |
| `space` | ಅಂತರಿಕ್ಷ ಮತ್ತು ಖಗೋಳ ಶಾಸ್ತ್ರ ಸಾಧನಗಳು |
| `network` | ನೆಟ್ವರ್ಕ್ ಸಾಧನಗಳು |
| `database` | ಡೇಟಾಬೇಸ್ ನಿರ್ವಹಣೆ |
| `monitoring` | ಸರ್ವರ್ ಮೇಲ್ವಿಚಾರಣೆ |
| `devops` | ಡೆವ್‌ಆಪ್ಸ್ ಮತ್ತು CI/CD |
| `utility` | ಸಾಮಾನ್ಯ ಉಪಕರಣಗಳು |
| `design` | ವಿನ್ಯಾಸ ಸಾಧನಗಳು |
| `ecommerce` | ಇ-ಕಾಮರ್ಸ್ ಸಾಧನಗಳು |
| `automation` | ಸ್ವಯಂಚಾಲಿತ ಸಾಧನಗಳು |
| `kpop` | K-pop ಸಂಬಂಧಿತ ಸಾಧನಗಳು |
| `accessibility` | ಪ್ರವೇಶಾರ್ಹತೆ ಸಾಧನಗಳು |
| `analytics` | ವಿಶ್ಲೇಷಣೆ ಮತ್ತು ವರದಿ |
| `wia` | WIA ಪರಿಸರ ಸಾಧನಗಳು |
| `all` | ಎಲ್ಲಾ ವರ್ಗಗಳಲ್ಲಿ ಕಾಣಿಸುತ್ತದೆ |

### ಶಿಫಾರಸು ಮಾಡಿದ ಐಕಾನ್ಸ್ (Lucide)

| ಐಕಾನ್ ಹೆಸರು | ಬಳಸಲು |
|--------------|--------|
| `server` | ಸರ್ವರ್ ನಿರ್ವಹಣೆ |
| `shield` | ಭದ್ರತೆ |
| `database` | ಡೇಟಾಬೇಸ್ |
| `activity` | ಮೇಲ್ವಿಚಾರಣೆ |
| `terminal` | ಟರ್ಮಿನಲ್ ಸಾಧನಗಳು |
| `code` | ಅಭಿವೃದ್ಧಿ |
| `hard-drive` | ಡಿಸ್ಕ್/ಸಂಗ್ರಹ |
| `network` | ನೆಟ್ವರ್ಕಿಂಗ್ |
| `lock` | ಪ್ರಮಾಣೀಕರಣ/ಎನ್‌ಕ್ರಿಪ್ಷನ್ |
| `eye` | ನೋಡುವುದು/ಮೇಲ್ವಿಚಾರಣೆ |
| `check-square` | ಕಾರ್ಯಗಳು/TODO |
| `layout-dashboard` | ಡ್ಯಾಶ್‌ಬೋರ್ಡ್‌ಗಳು |
| `settings` | ಕಾನ್ಫಿಗರೇಶನ್ |
| `zap` | ಸ್ವಯಂಚಾಲನೆ |
| `globe` | ವೆಬ್/ಅಂತರಾಷ್ಟ್ರೀಯ |

ಎಲ್ಲಾ 1,500+ ಐಕಾನ್ಸ್ ಅನ್ನು ಬ್ರೌಸ್ ಮಾಡಿ: [lucide.dev/icons](https://lucide.dev/icons)

---

## ಸಹಾಯ ಬೇಕಾ?

- **GitHub ಸಮಸ್ಯೆಗಳು:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ಪ್ಲಗಿನ್ ಸಮಸ್ಯೆಗಳು:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ಉದಾಹರಣೆ ಪ್ಲಗಿನ್‌ಗಳು:** [Website](https://wiasoom.com)
- **ವೆಬ್‌ಸೈಟ್:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>ಊಹಿಸುವಂತಹದ್ದೇನಾದರೂ ನಿರ್ಮಿಸಿ. ಅದನ್ನು ಜಗತ್ತಿನೊಂದಿಗೆ ಹಂಚಿಕೊಳ್ಳಿ.</em></p>
<p align="center"><em>— WIA SOOM ತಂಡ</em></p>
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
