<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM Eklenti Geliştirici Kılavuzu</h1>
<p align="center"><strong>Kendi eklentinizi 5 dakikada oluşturun.</strong></p>
<p align="center">Güçlü sunucu araçları, paneller ve otomasyonlar oluşturun — doğrudan WIA SOOM içinde.</p>

---

## İçindekiler

- [Bölüm 1: Hızlı Başlangıç — İlk Eklentiniz 5 Dakikada](#bölüm-1-hızlı-başlangıç--ilk-eklentiniz-5-dakikada)
- [Bölüm 2: Eklenti Bağlamı API Referansı](#bölüm-2-eklenti-bağlamı-api-referansı)
  - [ctx.terminal](#ctxterminal--uzaktan-sunucularda-komut-çalıştırma)
  - [ctx.sftp](#ctxsftp--dosya-taşıma)
  - [ctx.ui](#ctxui--kullanıcı-arayüzü)
  - [ctx.settings](#ctxsettings--kalıcı-depolama)
  - [ctx.ai](#ctxai--yapay-zeka-entegre)
- [Bölüm 3: Webview ile Özel UI Oluşturma](#bölüm-3-webview-ile-özel-ui-oluşturma)
- [Bölüm 4: Eklentinizi Yayınlama](#bölüm-4-eklentinizi-yayınlama)
- [Bölüm 5: En İyi Uygulamalar](#bölüm-5-en-iyi-uygulamalar)
- [Bölüm 6: Gerçek Dünya Örnekleri](#bölüm-6-gerçek-dünya-örnekleri)
- [Ek: Kategoriler & İkonlar](#ek-kategoriler--ikonlar)

---

## Bölüm 1: Hızlı Başlangıç — İlk Eklentiniz 5 Dakikada

### Ne yapacaksınız

Bir düğme ekleyen "Merhaba Dünya" eklentisi. Tıklandığında, bir bildirim gösterir.

### Adım 1: Eklenti klasörünü oluşturun
§§§CHUNK_SEPARATOR§§§
### Adım 2: package.json oluşturun
§§§CHUNK_SEPARATOR§§§
**Gerekli alanlar:** `name`, `version`, `description`, `author`, `main`

### Adım 3: index.js oluşturun
§§§CHUNK_SEPARATOR§§§
### Adım 4: WIA SOOM'u Yeniden Başlatın

Uygulamayı yeniden başlatın (veya Ayarlar → Eklentiler'den eklentiyi kapatıp açın).

Yan panelde bir **"Merhaba Dünya"** düğmesi görmelisiniz. Tıklayın — bir başarı bildirimi göreceksiniz!

### Nasıl çalışır
§§§CHUNK_SEPARATOR§§§
---

## Bölüm 2: Eklenti Bağlamı API Referansı

`activate(context)` fonksiyonunuz çağrıldığında, `context` (veya `ctx`) bu API'leri sağlar:
§§§CHUNK_SEPARATOR§§§
---

### `ctx.terminal` — Uzaktan sunucularda komut çalıştırma

#### `terminal.send(sessionId, data)`

Aktif bir terminal oturumuna bir komut (veya herhangi bir veri) gönderin.

| Parametre | Tür | Açıklama |
|-----------|------|-------------|
| `sessionId` | `string` | Gönderilecek terminal oturumu |
| `data` | `string` | Gönderilecek komut veya veri |
§§§CHUNK_SEPARATOR§§§
#### `terminal.onOutput(sessionId, callback)`

Bir terminal oturumundan gelen tüm çıktılara abone olun. Bir **abonelikten çıkma fonksiyonu** döner.

| Parametre | Tür | Açıklama |
|-----------|------|-------------|
| `sessionId` | `string` | İzlenecek terminal oturumu |
| `callback` | `(data: string) => void` | Her çıktı parçası ile çağrılır |
| **Döner** | `() => void` | Dinlemeyi durdurmak için bunu çağırın |
§§§CHUNK_SEPARATOR§§§
**Önemli:** Her zaman abonelikten çıkma fonksiyonunu kaydedin ve `deactivate()` içinde çağırın, bellek sızıntılarını önlemek için.

---

### `ctx.sftp` — Dosya taşıma

> **Durum: Çok Yakında** — SFTP API'si tanımlandı ancak henüz uygulamanın SFTP motoruna bağlanmadı. `list()` şu anda boş bir dizi döner ve `upload()`/`download()` işlevleri çalışmaz. Bu, gelecekteki bir sürümde tamamen uygulanacaktır. Şimdilik, bir geçici çözüm olarak `ctx.terminal.send()` ile `scp` veya `rsync` komutlarını kullanın.

#### `sftp.list(sessionId, path)`

Uzak bir dizindeki dosyaları listeleyin.
§§§CHUNK_SEPARATOR§§§
#### `sftp.upload(sessionId, localPath, remotePath)`

Yerel makineden uzak sunucuya bir dosya yükleyin.
§§§CHUNK_SEPARATOR§§§
#### `sftp.download(sessionId, remotePath, localPath)`

Uzak sunucudan yerel makineye bir dosya indirin.
§§§CHUNK_SEPARATOR§§§
**Geçici çözüm (SFTP API'si aktif olana kadar):**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ui` — Kullanıcı arayüzü

#### `ui.addSidebarButton(options)`

WIA SOOM yan paneline bir düğme ekleyin.

| Seçenek | Tür | Gerekli | Açıklama |
|---------|------|---------|-------------|
| `id` | `string` | Hayır | Benzersiz ID (varsayılan olarak eklenti adı) |
| `icon` | `string` | Evet | Lucide ikon adı (örneğin, `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | Evet | Yan panelde gösterilen düğme metni |
| `onClick` | `() => void` | Evet | Düğmeye tıklandığında çağrılan fonksiyon |
§§§CHUNK_SEPARATOR§§§
**İkon referansı:** Mevcut tüm ikonları [lucide.dev/icons](https://lucide.dev/icons) adresinden inceleyin.

> **Uyumluluk notu:** Bazı eski eklentiler, `addSidebarButton(id, icon, label, onClick)` gibi konumsal argümanlar kullanır. Resmi API, yukarıda belgelenen **seçenek nesnesini** kullanır. Yeni eklentiler için her zaman nesne stilini kullanın.

#### `ui.openWebview(options)`

Özel HTML içeriği ile bir açılır pencere açın. Zengin kullanıcı arayüzleri oluşturmanın yolu budur.

| Seçenek | Tür | Açıklama |
|---------|------|-------------|
| `title` | `string` | Pencere başlığı |
| `html` | `string` | Render edilecek tam HTML içeriği |
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
> [Bölüm 3](#part-3-building-custom-ui-with-webviews) için gelişmiş webview desenlerine bakın.

#### `ui.showNotification(type, message)`

Bir toast bildirimi gösterir.

| Parametre | Tür | Açıklama |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | Bildirim stili |
| `message` | `string` | Gösterilecek metin |
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

Alt durum çubuğuna kalıcı bir metin öğesi ekler.

| Parametre | Tür | Açıklama |
|-----------|------|-------------|
| `id` | `string` | Bu durum öğesi için benzersiz kimlik |
| `text` | `string` | Gösterilecek metin |
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

### `ctx.settings` — Kalıcı depolama

Eklenti ayarları kalıcı olarak `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json` dosyasında saklanır.

#### `settings.get(key)`

Kaydedilmiş bir değeri okur.
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
Anahtar mevcut değilse `undefined` döner.

#### `settings.set(key, value)`

Bir değeri kaydeder. Dize, sayı, boolean, dizi ve nesne destekler.
§§§CHUNK_SEPARATOR§§��
**Örnek: Kullanıcı tercihlerini hatırlama**
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

### `ctx.ai` — AI entegrasyonu

> **Durum: Yakında** — AI API'si tanımlandı ancak henüz Soomy'e bağlanmadı. Şu anda `{ response: 'AI not yet connected' }` döner. Tam AI entegrasyonu gelecekteki bir sürüm için planlanmaktadır.

#### `ai.chat(messages, options?)`

AI asistanına (Soomy) mesaj gönderir.
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
---

## Bölüm 3: Webview'lerle Özel UI Oluşturma

`openWebview()` API'si, HTML, CSS ve JavaScript ile pano UI'ları oluşturmanıza olanak tanır — hepsi bir açılır pencerede.

> **Önemli kısıtlama:** Webview'ler **sadece görüntüleme** içindir. Eklenti API'lerine (`ctx.settings`, `ctx.terminal`, vb.) geri çağrı yapamazlar. Tüm kullanıcı eylemleri için yan menü düğmelerini kullanın ve mevcut durumu göstermek için `openWebview()` kullanın. Etkileşimli özelliklere ihtiyacınız varsa, bunları yan menü düğmelerinden tetikleyin ve görüntüyü yenilemek için webview'i yeniden açın.

### Desen: Terminal Komutu → Çıktıyı Ayrıştır → HTML'de Göster

Bu, en yaygın eklenti desenidir. Bir komut çalıştırırsınız, sonucu ayrıştırırsınız ve görsel olarak gösterirsiniz.
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
### Desen: Otomatik Yenileme ile Etkileşimli Pano
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
### Desen: Webview'de Ayarları Gösterme

> **Not:** Webview'ler sadece görüntüleme içindir — eklenti API'lerine geri çağrı yapamazlar. Ayarları değiştirmek için yan menü düğmesi işleyicilerinizde `ctx.settings` kullanın ve mevcut durumu göstermek için `openWebview()` kullanın.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
---

## Bölüm 4: Eklentinizi Yayınlama

### Adım 1: Yerel olarak test edin

1. Eklentinizi `~/.wia-soom/plugins/{your-plugin}/` dizinine kopyalayın
2. WIA SOOM'u yeniden başlatın
3. Çalıştığını doğrulayın: yan menü düğmesi görünür, özellikler doğru çalışıyor
4. Kenar durumlarını test edin: terminal bağlı değilse ne olur?

### Adım 2: Gönderime hazırlanın

Eklenti klasörünüz şunları içermelidir:
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**Gerekli `package.json` alanları:**

| Alan | Açıklama | Örnek |
|------|----------|-------|
| `name` | Benzersiz kebab-case kimliği | `"my-awesome-plugin"` |
| `version` | Anlamlı sürüm | `"1.0.0"` |
| `description` | Tek satırlık açıklama | `"Gerçek zamanlı nginx erişim günlüklerini izler"` |
| `author` | Adınız | `"John Doe"` |
| `main` | Giriş noktası | `"index.js"` |

**Opsiyonel alanlar:**

| Alan | Açıklama |
|------|----------|
| `license` | Lisans türü (MIT önerilir) |
| `keywords` | Arama etiketleri dizisi |
| `soom.minVersion` | Gerekli minimum WIA SOOM sürümü |

### Adım 3: Eklentiyi Plugin Kaydına Gönderin

1. ****Package** your plugin as a ZIP file
2. **Ekle** eklentinizi `plugins/{your-plugin-name}/`
3. **Gönder** bir Pull Request

### Adım 4: İnceleme ve onay

Her eklentiyi şu kriterler için inceliyoruz:

- **Güvenlik** — tehlikeli API'ler yok (bkz. [Güvenlik Kuralları](#security-rules))
- **Kalite** — çalışıyor mu? Kod temiz mi?
- **Kullanışlılık** — gerçek bir sorunu çözüyor mu?

Onaydan sonra:
1. Eklentiniz `registry.json` dosyasına eklenir
2. `dist/` dizininde bir ZIP paketi oluşturulur
3. Eklentiniz tüm WIA SOOM kullanıcıları için **Plugin Store**'da görünür!

---

## Bölüm 5: En İyi Uygulamalar

### Güvenlik Kuralları

Bu kurallar **zorunludur**. Bu kurallara uymayan eklentiler reddedilecektir.

| Kural | Neden |
|-------|-------|
| **ASLA** `eval()` veya `new Function()` kullanmayın | Kod enjeksiyonu riski |
| **ASLA** `child_process`, `exec()`, `spawn()` kullanmayın | Komutlar için yalnızca `ctx.terminal.send()` kullanın |
| **ASLA** harici URL'leri çekmeyin | İstisna: `wiasoom.com` API uç noktaları |
| **ASLA** `process.env` erişmeyin | Ortam değişkenleri gizli bilgiler içerebilir |
| **ASLA** `require('fs')` doğrudan kullanmayın | Depolama için `ctx.settings`, dosya transferi için `ctx.sftp` kullanın |
| **ASLA** npm harici paketleri kullanmayın | Sadece saf JavaScript — node_modules yok |
| **MECBUR** tüm uzaktan komutlar için `ctx.terminal.send()` kullanın | Bu güvenli SSH kanalı üzerinden geçer |
| **MECBUR** `deactivate()` içinde temizlik yapın | Dinleyicileri kaldırın, aralıkları temizleyin |

### Hata Yönetimi

Her zaman riskli işlemleri try/catch içinde sarın:
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
### deactivate() içinde Temizlik

Eklentiniz aralıklar, dinleyiciler veya abonelikler oluşturuyorsa — bunları temizleyin:
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
### i18n Desteği

WIA SOOM 254 dili destekler. Eklentinizin etiketini çevrilebilir hale getirmek için basit bir yaklaşım kullanın:
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
---

## Bölüm 6: Gerçek Dünya Örnekleri

### Örnek 1: Sunucu Disk Kontrolü

Uzaktaki sunucuda `df -h` çalıştırır ve durum çubuğunda kullanılan/mevcut alanı gösterir.
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
---

### Örnek 2: TODO Yöneticisi

Ayarları kalıcı depolama için ve görüntüleme için bir webview kullanan bir TODO listesi yöneticisi eklentisi.

> **Tasarım deseni:** Webview'ler doğrudan eklenti API'lerini çağırmadığı için, bu eklenti "anlık görüntü" yaklaşımını kullanır — TODO'ları ayarlardan okur, bunları yalnızca okunabilir HTML olarak işler ve öğe eklemek için yan çubuk tabanlı eylemler sağlar. Webview, **görüntüleme** katmanıdır, etkileşimli bir form değildir.
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
---

### Örnek 3: Hata İzleyici

Terminal çıktısını izler ve belirli desenler tespit edildiğinde bir bildirim gönderir.
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
---

## Ek: Kategoriler & İkonlar

### Eklenti Kategorileri (29)

Bunları `package.json` `keywords` kısmında veya kayıt defterine gönderirken kullanın:

| Kategori | Açıklama |
|----------|-------------|
| `server` | Genel sunucu yönetimi |
| `devtools` | Geliştirme araçları |
| `calculator` | Hesap makineleri ve dönüştürücüler |
| `simulator` | Simülatörler |
| `game` | Terminal oyunları |
| `business` | İş araçları |
| `security` | Güvenlik ve denetim |
| `web` | Web sunucu yönetimi |
| `education` | Eğitim araçları |
| `health` | Sağlıkla ilgili araçlar |
| `islamic` | İslami araçlar (namaz vakitleri vb.) |
| `science` | Bilimsel araçlar |
| `quantum` | Kuantum hesaplama araçları |
| `ai` | Yapay zeka destekli araçlar |
| `biotech` | Biyoteknoloji araçları |
| `space` | Uzay ve astronomi araçları |
| `network` | Ağ araçları |
| `database` | Veritabanı yönetimi |
| `monitoring` | Sunucu izleme |
| `devops` | DevOps ve CI/CD |
| `utility` | Genel yardımcı programlar |
| `design` | Tasarım araçları |
| `ecommerce` | E-ticaret araçları |
| `automation` | Otomasyon araçları |
| `kpop` | K-pop ile ilgili araçlar |
| `accessibility` | Erişilebilirlik araçları |
| `analytics` | Analiz ve raporlama |
| `wia` | WIA ekosistem araçları |
| `all` | Tüm kategorilerde görünür |

### Önerilen İkonlar (Lucide)

| İkon Adı | Kullanım Alanı |
|-----------|---------|
| `server` | Sunucu yönetimi |
| `shield` | Güvenlik |
| `database` | Veritabanı |
| `activity` | İzleme |
| `terminal` | Terminal araçları |
| `code` | Geliştirme |
| `hard-drive` | Disk/depolama |
| `network` | Ağ bağlantısı |
| `lock` | Kimlik doğrulama/şifreleme |
| `eye` | İzleme/gözlemleme |
| `check-square` | Görevler/TODO |
| `layout-dashboard` | Gösterge panelleri |
| `settings` | Yapılandırma |
| `zap` | Otomasyon |
| `globe` | Web/uluslararası |

Tüm 1,500+ ikona göz atın: [lucide.dev/icons](https://lucide.dev/icons)

---

## Yardıma mı İhtiyacınız Var?

- **GitHub Sorunları:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Eklenti Sorunları:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Örnek Eklentiler:** [Website](https://wiasoom.com)
- **Web Sitesi:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Harika bir şey inşa edin. Dünyayla paylaşın.</em></p>
<p align="center"><em>— WIA SOOM Ekibi</em></p>
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
