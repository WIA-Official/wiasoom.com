<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">คู่มือการพัฒนา Plugin ของ WIA SOOM</h1>
<p align="center"><strong>สร้าง Plugin ของคุณเองใน 5 นาที.</strong></p>
<p align="center">สร้างเครื่องมือเซิร์ฟเวอร์ที่ทรงพลัง, แดชบอร์ด, และการทำงานอัตโนมัติ — ภายใน WIA SOOM.</p>

---

## เนื้อหาสำหรับอ่าน

- [ส่วนที่ 1: เริ่มต้นอย่างรวดเร็ว — Plugin แรกของคุณใน 5 นาที](#part-1-quick-start--your-first-plugin-in-5-minutes)
- [ส่วนที่ 2: เอกสารอ้างอิง API ของ Plugin Context](#part-2-plugin-context-api-reference)
  - [ctx.terminal](#ctxterminal--run-commands-on-remote-servers)
  - [ctx.sftp](#ctxsftp--file-transfer)
  - [ctx.ui](#ctxui--user-interface)
  - [ctx.settings](#ctxsettings--persistent-storage)
  - [ctx.ai](#ctxai--ai-integration)
- [ส่วนที่ 3: การสร้าง UI ที่กำหนดเองด้วย Webviews](#part-3-building-custom-ui-with-webviews)
- [ส่วนที่ 4: การเผยแพร่ Plugin ของคุณ](#part-4-publishing-your-plugin)
- [ส่วนที่ 5: แนวทางปฏิบัติที่ดีที่สุด](#part-5-best-practices)
- [ส่วนที่ 6: ตัวอย่างในโลกจริง](#part-6-real-world-examples)
- [ภาคผนวก: หมวดหมู่ & ไอคอน](#appendix-categories--icons)

---

## ส่วนที่ 1: เริ่มต้นอย่างรวดเร็ว — Plugin แรกของคุณใน 5 นาที

### สิ่งที่คุณจะสร้าง

Plugin "Hello World" ที่เพิ่มปุ่มในแถบด้านข้าง เมื่อคลิกจะมีการแสดงการแจ้งเตือน

### ขั้นตอนที่ 1: สร้างโฟลเดอร์ Plugin
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### ขั้นตอนที่ 2: สร้าง package.json
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
**ฟิลด์ที่จำเป็��:** `name`, `version`, `description`, `author`, `main`

### ขั้นตอนที่ 3: สร้าง index.js
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
### ขั้นตอนที่ 4: รีสตาร์ท WIA SOOM

รีสตาร์ทแอป (หรือเปลี่ยนสถานะของ Plugin ในการตั้งค่า → Plugins)

คุณจะเห็นปุ่ม **"Hello World"** ในแถบด้านข้าง คลิกมัน — คุณจะเห็นการแจ้งเตือนความสำเร็จ!

### วิธีการทำงาน
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

## ส่วนที่ 2: เอกสารอ้างอิง API ของ Plugin Context

เมื่อฟังก์ชัน `activate(context)` ของคุณถูกเรียกใช้ `context` (หรือ `ctx`) จะให้ API เหล่านี้:
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

### `ctx.terminal` — รันคำสั่งบนเซิร์ฟเวอร์ระยะไกล

#### `terminal.send(sessionId, data)`

ส่งคำสั่ง (หรือข้อมูลใด ๆ) ไปยังเซสชันเทอร์มินัลที่ใช้งานอยู่

| พารามิเตอร์ | ประเภท | คำอธิบาย |
|--------------|--------|-----------|
| `sessionId`  | `string` | เซสชันเทอร์มินัลที่ต้องการส่งไป |
| `data`       | `string` | คำสั่งหรือข้อมูลที่จะส่ง |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

สมัครสมาชิกเพื่อรับข้อมูลทั้งหมดจากเซสชันเทอร์มินัล คืนค่าฟังก์ชัน **ยกเลิกการสมัครสมาชิก** 

| พารามิเตอร์ | ประเภท | คำอธิบาย |
|--------------|--------|-----------|
| `sessionId`  | `string` | เซสชันเทอร์มินัลที่ต้องการติดตาม |
| `callback`   | `(data: string) => void` | ถูกเรียกใช้กับแต่ละชิ้นส่วนของข้อมูลที่ส่งออก |
| **คืนค่า**   | `() => void` | เรียกใช้เพื่อหยุดการฟัง |
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
**สำคัญ:** ควรบันทึกฟังก์ชันยกเลิกการสมัคร��มาชิกและเรียกใช้ใน `deactivate()` เพื่อป้องกันการรั่วไหลของหน่วยความจำ

---

### `ctx.sftp` — การถ่ายโอนไฟล์

> **สถานะ: กำลังจะมา** — API SFTP ได้รับการกำหนดแล้วแต่ยังไม่ได้เชื่อมต่อกับเครื่องยนต์ SFTP ของแอป `list()` ขณะนี้คืนค่าผลลัพธ์เป็นอาร์เรย์ว่าง และ `upload()`/`download()` เป็นการทำงานที่ไม่มีผลลัพธ์ สิ่งนี้จะถูกนำไปใช้เต็มรูปแบบในเวอร์ชันถัดไป สำหรับตอนนี้ให้ใช้ `ctx.terminal.send()` กับคำสั่ง `scp` หรือ `rsync` เป็นการแก้ไขชั่วคราว

#### `sftp.list(sessionId, path)`

แสดงรายการไฟล์ในไดเรกทอรีระยะไกล
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

อัปโหลดไฟล์จากเครื่องคอมพิวเ��อร์ท้องถิ่นไปยังเซิร์ฟเวอร์ระยะไกล
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

ดาวน์โหลดไฟล์จากเซิร์ฟเวอร์ระยะไกลไปยังเครื่องคอมพิวเตอร์ท้องถิ่น
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**การแก้ไขชั่วคราว (จนกว่า API SFTP จะใช้งานได้):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — ส่วนติดต่อผู้ใช้

#### `ui.addSidebarButton(options)`

เพิ่มปุ่มในแถบด้านข้างของ WIA SOOM

| ตัวเลือก | ประเภท | จำเป็น | คำอธิบาย |
|----------|--------|--------|-----------|
| `id`     | `string` | ไม่ | รหัสประจำตัวที่ไม่ซ้ำกัน (ค่าเริ่มต้นเป็นชื่อ Plugin) |
| `icon`   | `string` | ใช่ | ชื่อไอคอน Lucide (เช่น `'server'`, `'shield'`, `'database'`) |
| `label`  | `string` | ใช่ | ข้อความปุ่มที่แสดงในแถบด้านข้าง |
| `onClick`| `() => void` | ใช่ | ฟังก์ชันที่ถูกเรียกเมื่อคลิกปุ่ม |
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
**การอ้างอิงไอคอน:** เรียกดูไอคอนทั้งหมดที่มีที่ [lucide.dev/icons](https://lucide.dev/icons)

> **หมายเหตุเกี่ยวกับความเข้ากันได้:** Plugin เก่าบางตัวใช้พารามิเตอร์ตามตำแหน่งเช่น `addSidebarButton(id, icon, label, onClick)` API อย่างเป็นทางการใช้ **อ็อบเจ็กต์ตัวเลือก** ตามที่ระบุไว้ข้างต้น ควรใช้รูปแบบอ็อบเจ็กต์สำหรับ Plugin ใหม่เสมอ

#### `ui.openWebview(options)`

เปิดหน้าต่างป๊อปอัปด้วยเนื้อหา HTML ที่กำหนดเอง นี่คือวิธีที่คุณสร้าง UI ที่มีความซับซ้อน

| ตัวเลือก | ประเภท | คำอธิบาย |
|----------|--------|-----------|
| `title`  | `string` | ชื่อหน้��ต่าง |
| `html`   | `string` | เนื้อหา HTML เต็มที่ต้องการแสดง |
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
> ดูที่ [ส่วนที่ 3](#part-3-building-custom-ui-with-webviews) สำหรับรูปแบบเว็บวิวขั้นสูง

#### `ui.showNotification(type, message)`

แสดงการแจ้งเตือนแบบทอสต์

| พารามิเตอร์ | ประเภท | คำอธิบาย |
|--------------|--------|-----------|
| `type` | `'success' \| 'error' \| 'info'` | รูปแบบการแจ้งเตือน |
| `message` | `string` | ข้อความที่จะแสดง |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

เพิ่มรายการข้อความที่คงอยู่ในแถบสถานะด้านล่าง

| พารามิเตอร์ | ประเภท | คำอธิบาย |
|--------------|--------|-----------|
| `id` | `string` | ID ที่ไม่ซ้ำกันสำหรับรายการสถานะนี้ |
| `text` | `string` | ข้อความที่จะแสดง |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — การจัดเก็บข้อมูลถาวร

การตั้งค่��� Plugin จะถูกเก็บถาวรใน `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`

#### `settings.get(key)`

อ่านค่าที่บันทึกไว้
§§§CHUNK_SEPARATOR§§§
จะคืนค่า `undefined` หากคีย์ไม่มีอยู่

#### `settings.set(key, value)`

บันทึกค่า รองรับสตริง ตัวเลข บูลีน อาร์เรย์ และอ็อบเจกต์
§§§CHUNK_SEPARATOR§§§
**ตัวอย่าง: จดจำความชอบของผู้ใช้**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — การรวม AI

> **สถานะ: กำลังจะมา** — AI API ถูกกำหนดไว้แต่ยังไม่ได้เชื่อมต่อกับ Soomy ขณะนี้คืนค่า `{ response: 'AI not yet connected' }` การรวม AI แบบเต็มรูปแบบมีแผนสำหรับการเปิดตัวในอนาคต

#### `ai.chat(messages, options?)`

ส่งข้อความไปยังผู้ช่วย AI (Soomy)
§§§CHUNK_SEPARATOR§§§
---

## ส่วนที่ 3: การสร้าง UI ที่กำหนดเองด้วย Webviews

API `openWebview()` ช่วยให้คุณสร้าง UI แดชบอร์ดด้วย HTML, CSS และ JavaScript — ทั้งหมดอยู่ในหน้าต่างป๊อปอัป

> **ข้อจำกัดที่สำคัญ:** เว็บวิวเป็น **แสดงผลเท่านั้น** ไม่สามารถเรียกกลับไปยัง API ของปลั๊กอิน (`ctx.settings`, `ctx.terminal`, ฯลฯ) ใช้ปุ่มด้านข้างสำหรับการกระทำของผู้ใช้ทั้งหมด และใช้ `openWebview()` เพื่อแสดงสถานะปัจจุบัน หากคุณต้องการฟีเจอร์ที่โต้ตอบได้ ให้กระตุ้นจากปุ่มด้านข้างและเปิดเว็บวิวใหม่เพื่อรีเฟรชการแสดงผล

### รูปแ��บ: คำสั่ง Terminal → แยกผลลัพธ์ → แสดงใน HTML

นี่คือรูปแบบปลั๊กอินที่พบบ่อยที่สุด คุณเรียกใช้คำสั่ง แยกผลลัพธ์ และแสดงผลในลักษณะภาพ
§§§CHUNK_SEPARATOR§§§
### รูปแบบ: แดชบอร์ดเชิงโต้ตอบพร้อมการรีเฟรชอัตโนมัติ
§§§CHUNK_SEPARATOR§§§
### รูปแบบ: การแสดงการตั้งค่าในเว็บวิว

> **หมายเหตุ:** เว็บวิวเป็นการแสดงผลเท่านั้น — ไม่สามารถเรียกกลับไปยัง API ของปลั๊กอิน ใช้ `ctx.settings` ในตัวจัดการปุ่มด้านข้างของคุณเพื่อแก้ไขการตั้งค่า และใช้ `openWebview()` เพื่อแสดงสถานะปัจจุบัน
§§§CHUNK_SEPARATOR§§§
---

## ส่วนที่ 4: การเผยแพร่ปลั๊กอินของคุณ

### ขั้นตอนที่ 1: ทดสอบ���นเครื่อง

1. คัดลอกปลั๊กอินของคุณไปที่ `~/.wia-soom/plugins/{your-plugin}/`
2. รีสตาร์ท WIA SOOM
3. ตรวจสอบให้แน่ใจว่ามันทำงาน: ปุ่มด้านข้างปรากฏขึ้น ฟีเจอร์ทำงานอย่างถูกต้อง
4. ทดสอบกรณีขอบ: จะเกิดอะไรขึ้นหากไม่มีเทอร์มินัลเชื่อมต่อ?

### ขั้นตอนที่ 2: เตรียมการส่ง

โฟลเดอร์ปลั๊กอินของคุณต้องมี:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
**ฟิลด์ที่จำเป็นใน `package.json`:**

| ฟิลด์ | คำอธิบาย | ตัวอย่าง |
|-------|-------------|---------|
| `name` | รหัสแบบ kebab-case ที่ไม่ซ้ำกัน | `"my-awesome-plugin"` |
| `version` | เวอร์ชันตามหลักการ | `"1.0.0"` |
| `description` | คำอธิบายสั้น ๆ | `"Monitors nginx access logs in real-time"` |
| `author` | ชื่อของคุณ | `"John Doe"` |
| `main` | จุดเริ่มต้น | `"index.js"` |

**ฟิลด์ที่ไม่บังคับ:**

| ฟิลด์ | คำอธิบาย |
|-------|-------------|
| `license` | ประเภทใบอนุญาต (แนะนำ MIT) |
| `keywords` | อาร์เรย์ของแท็กค้นหา |
| `soom.minVersion` | เวอร์ชัน WIA SOOM ที่ต้องการขั้นต่ำ |

### ขั้นตอนที่ 3: ส่งไปยัง Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **เพิ่ม** ปลั๊กอินของคุณไปที่ `plugins/{your-plugin-name}/`
3. **ส่ง** Pull Request

### ขั้นตอนที่ 4: การตรวจสอบและการอนุมัติ

เราจะตรวจสอบปลั๊กอินทุกตัวสำหรับ:

- **ความปลอดภัย** — ไม่มี API ที่อันตราย (ดู [Security Rules](#security-rules))
- **คุณภาพ** — มันทำงานหรือไม่? โค้ดสะอาดหรือไม่?
- **ความมีประโยชน์** — มันแก้ปัญหาจริงหรือไม่?

หลังจากการอนุมัติ:
1. ปลั๊กอินของคุณจะถูกเพิ่มไปที่ `registry.json`
2. แพ็คเกจ ZIP จะถูกสร้างใน `dist/`
3. ปลั๊กอินของคุณจะปรากฏใน **Plugin Store** สำหรับผู้ใช้ WIA SOOM ทุกคน!

---

## ส่วนที่ 5: แนวปฏิบัติที��ดีที่สุด

### กฎความปลอดภัย

กฎเหล่านี้เป็น **ข้อบังคับ** ปลั๊กอินที่ละเมิดจะถูกปฏิเสธ

| กฎ | ทำไม |
|------|-----|
| **ห้าม** ใช้ `eval()` หรือ `new Function()` | ความเสี่ยงจากการฉีดโค้ด |
| **ห้าม** ใช้ `child_process`, `exec()`, `spawn()` | ใช้เฉพาะ `ctx.terminal.send()` สำหรับคำสั่ง |
| **ห้าม** ดึง URL ภายนอก | ข้อยกเว้น: จุดสิ้นสุด API ของ `wiasoom.com` |
| **ห้าม** เข้าถึง `process.env` | ตัวแปรสภาพแวดล้อมอาจมีความลับ |
| **ห้าม** ใช้ `require('fs')` โดยตรง | ใช้ `ctx.settings` สำหรับการจัดเก็บ, `ctx.sftp` สำหรับการถ่ายโอนไฟล์ |
| **ต้อง** ใช้ `ctx.terminal.send()` สำหรับคำสั่งระยะไกลทั้งหมด | นี้จะผ่านช่องทาง SSH ที่ปลอดภัย |
| **ต้อง** ��ำความสะอาดใน `deactivate()` | ลบผู้ฟัง, เคลียร์ช่วงเวลา |

### การจัดการข้อผิดพลาด

ห่อหุ้มการดำเนินการที่มีความเสี่ยงเสมอใน try/catch:
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
### การทำความสะอาดใน deactivate()

หากปลั๊กอินของคุณสร้างช่วงเวลา, ผู้ฟัง, หรือการสมัครสมาชิก — ทำความสะอาดพวกมัน:
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
### การสนับสนุน i18n

WIA SOOM รองรับ 254 ภาษา เพื่อทำให้ป้ายชื่อปลั๊กอินของคุณแปลได้ ใช้แนวทางที่เรียบง่าย:
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## ส่วนที่ 6: ตัวอย่างในโลกจริง

### ตัวอย่างที่ 1: ตัวตรวจสอบดิสก์เซิร์ฟเวอร์

รัน `df -h` บนเซิร์ฟเวอร์ระยะไกลและแสดงพื้นที่ที่ใช้/���่างในแถบสถานะ.
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

### ตัวอย่างที่ 2: ผู้จัดการ TODO

ปลั๊กอินที่จัดการรายการ TODO โดยใช้การตั้งค่าสำหรับการจัดเก็บถาวรและเว็บวิวสำหรับการแสดงผล

> **รูปแบบการออกแบบ:** เนื่องจากเว็บวิวไม่สามารถเรียก API ของปลั๊กอินโดยตรง ปลั๊กอินนี้จึงใช้แนวทาง "snapshot" — มันอ่าน TODOs จากการตั้งค่า, แสดงผลเป็น HTML ที่อ่านได้เท่านั้น, และให้การกระทำที่อยู่ด้านข้างสำหรับการเพิ่มรายการ เว็บวิวเป็นชั้น **แสดงผล** ไม่ใช่แบบฟอร์มที่โต้ตอบได้.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

### ตัวอย่างที่ 3: ตัวตรวจสอบข้อผิดพลาด

ตรวจสอบผลลัพธ์���องเทอร์มินัลและส่งการแจ้งเตือนเมื่อมีรูปแบบเฉพาะที่ถูกตรวจพบ.
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
---

## ภาคผนวก: หมวดหมู่ & ไอคอน

### หมวดหมู่ปลั๊กอิน (29)

ใช้สิ่งเหล่านี้ใน `package.json` `keywords` หรือเมื่อส่งไปยังรีจิสทรี:

| หมวดหมู่ | คำอธิบาย |
|----------|-------------|
| `server` | การจัดการเซิร์ฟเวอร์ทั่วไป |
| `devtools` | เครื่องมือพัฒนา |
| `calculator` | เครื่องคิดเลขและตัวแปลง |
| `simulator` | เครื่องจำลอง |
| `game` | เกมในเทอร์มินัล |
| `business` | เครื่องมือทางธุรกิจ |
| `security` | ความปลอดภัยและการตรวจสอบ |
| `web` | การจัดการเซิร์ฟเวอร์เว็บ |
| `education` | เครื่องมือการศึกษา |
| `health` | เครื่องมือที่เกี่ยวกับสุขภาพ |
| `islamic` | เครื่องมืออิสลาม (เวลาอธิษฐาน ฯลฯ) |
| `science` | เครื่องมือทางวิทยาศาสตร์ |
| `quantum` | เครื่องมือการคำนวณควอนตัม |
| `ai` | เครื่องมือที่ขับเคลื่อนด้วย AI |
| `biotech` | เครื่องมือชีวภาพเทคโนโลยี |
| `space` | เครื่องมือด้านอวกาศและดาราศาสตร์ |
| `network` | เครื่องมือเครือข่าย |
| `database` | การจัดการฐานข้อมูล |
| `monitoring` | การตรวจสอบเซิร์ฟเวอร์ |
| `devops` | DevOps และ CI/CD |
| `utility` | ยูทิลิตี้ทั่วไป |
| `design` | เครื่องมือออกแบบ |
| `ecommerce` | เครื่องมืออีคอมเมิร์ซ |
| `automation` | เครื่องมืออัตโนมัติ |
| `kpop` | เครื่องมือที่เกี่ยวข้องกับ K-pop |
| `accessibility` | เครื่องมือการเข้าถึง |
| `analytics` | การวิเคราะห์และรายงาน |
| `wia` | เครื��องมือในระบบนิเวศ WIA |
| `all` | ปรากฏในทุกหมวดหมู่ |

### ไอคอนที่แนะนำ (Lucide)

| ชื่อไอคอน | ใช้สำหรับ |
|-----------|---------|
| `server` | การจัดการเซิร์ฟเวอร์ |
| `shield` | ความปลอดภัย |
| `database` | ฐานข้อมูล |
| `activity` | การตรวจสอบ |
| `terminal` | เครื่องมือเทอร์มินัล |
| `code` | การพัฒนา |
| `hard-drive` | ดิสก์/การจัดเก็บ |
| `network` | การเชื่อมต่อเครือข่าย |
| `lock` | การตรวจสอบ/การเข้ารหัส |
| `eye` | การดู/การตรวจสอบ |
| `check-square` | งาน/TODO |
| `layout-dashboard` | แดชบอร์ด |
| `settings` | การตั้งค่า |
| `zap` | การทำงานอัตโนมัติ |
| `globe` | เว็บ/นานาชาติ |

เรียกดูไอคอนทั้งหมด 1,500+ ไอคอน: [lucide.dev/icons](https://lucide.dev/icons)

---

## ต้องการความช่วยเหลือ?

- **ปัญหา GitHub:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ปัญหาปลั๊กอิน:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **ปลั๊กอินตัวอย่าง:** [Website](https://wiasoom.com)
- **เว็บไซต์:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>สร้างสิ่งที่น่าทึ่งบางอย่าง แชร์มันกับโลก.</em></p>
<p align="center"><em>— ทีม WIA SOOM</em></p>
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
