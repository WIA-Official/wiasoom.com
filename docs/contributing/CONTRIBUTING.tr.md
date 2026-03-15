<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM'a Katkıda Bulunma</h1>
<p align="center"><strong>Katkılarınızı bekliyoruz!</strong></p>
<p align="center">İster bir hata düzeltmesi, yeni bir özellik, eklenti veya çeviri olsun — her katkı önemlidir.</p>

---

## İçindekiler

- [Davranış Kuralları](#code-of-conduct)
- [Hataları Nasıl Bildirirsiniz](#-how-to-report-bugs)
- [Özellikleri Nasıl Önerirsiniz](#-how-to-suggest-features)
- [Eklenti Nasıl Gönderilir](#-how-to-submit-a-plugin)
- [Pull Request Nasıl Gönderilir](#-how-to-submit-a-pull-request)
- [Çeviri Katkıları (254 Dil)](#-translation-contributions-254-languages)
- [Geliştirme Ortamı](#-development-setup)

---

## Davranış Kuralları

Herkes için davetkar ve kapsayıcı bir deneyim sunmaya kararlıyız.

- **Saygılı olun.** Herkese onurla davranın.
- **Yapıcı olun.** Yıkıcı eleştiriler yerine yardımcı geri bildirimde bulunun.
- **Kapsayıcı olun.** 254 dili destekliyoruz ve dünyanın her ülkesinden katkıda bulunanları karşılıyoruz.
- **Taciz yok.** Her türlü ayrımcılığa sıfır tolerans.

---

## 🐛 Hataları Nasıl Bildirirsiniz

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) sayfasına gidin
2. **"Yeni Sorun"** butonuna tıklayın
3. **"Hata Raporu"** şablonunu seçin
4. Şunları ekleyin:
   - WIA SOOM sürümü (Ayarlar → Hakkında)
   - İşletim sistemi ve sürümü (Windows/macOS/Linux)
   - Yeniden üretme adımları
   - Beklenen ve gerçek davranış
   - Mümkünse ekran görüntüleri veya terminal çıktısı

---

## 💡 Özellikleri Nasıl Önerirsiniz

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) sayfasına gidin
2. **"Yeni Sorun"** butonuna tıklayın
3. **"Özellik Talebi"** şablonunu seçin
4. Şunları tanımlayın:
   - Hangi sorunu çözdüğünüz
   - Nasıl çalıştığını hayal ettiğiniz
   - Düşündüğünüz alternatifler

---

## 🔌 Eklenti Nasıl Gönderilir

WIA SOOM, güçlü bir eklenti sistemine sahiptir — kendi eklentinizi 5 dakikada oluşturabilirsiniz.

### Hızlı Başlangıç
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tam Kılavuz

**[Eklenti Geliştirici Kılavuzu](docs/PLUGIN_DEVELOPER_GUIDE.md)**'nu okuyun:
- Tam API referansı
- Çalışan örnekler
- Adım adım eğitimler
- En iyi uygulamalar ve güvenlik kuralları

### Eklentinizi Gönderin

1. [Plugin Store](https://wiasoom.com) reposunu fork'layın
2. Eklentinizi `plugins/{your-plugin-name}/` dizinine ekleyin
3. Bir Pull Request gönderin
4. İncelendikten sonra, eklentiniz tüm kullanıcılar için Eklenti Mağazasında görünecektir!

---

## 🔀 Pull Request Nasıl Gönderilir

### Ana uygulama için (wia-soom)

1. Reposunu fork'layın
2. Bir özellik dalı oluşturun: `git checkout -b feat/my-feature`
3. Değişikliklerinizi yapın
4. Yerel olarak test edin:
   ```bash
   ```
5. Açık bir mesajla commit yapın:
   ```
   feat: ayarlar için karanlık mod geçişi ekle
   ```
6. `main` dalına karşı bir PR açın

### Commit Mesajı Konvansiyonu

| Ön ek | Kullanım alanı |
|-------|----------------|
| `feat:` | Yeni özellik |
| `fix:` | Hata düzeltmesi |
| `docs:` | Sadece dokümantasyon |
| `refactor:` | Kod yeniden yapılandırması (davranış değişikliği yok) |
| `i18n:` | Çeviri güncellemeleri |
| `plugin:` | Eklenti ile ilgili değişiklikler |

### PR Kontrol Listesi

- [ ] Kod hatasız çalışıyor
- [ ] Sabitlenmiş dizeler yok (i18n anahtarları kullanın)
- [ ] Üretim kodunda `console.log` bırakılmadı
- [ ] Mevcut testler hala geçiyor

---

## 🌐 Çeviri Katkıları (254 Dil)

WIA SOOM, **254 dili** desteklemektedir — Amharca'dan Zulu'ya, Braille ve RTL dilleri de dahil.

### Çevirinin Çalışma Şekli

- Temel dil dosyası: `src/renderer/src/i18n/en.json`
- Tüm 254 dil dosyası aynı dizindedir
- Çeviri, `scripts/translate-patch.js` (GPT-4o-mini API) aracılığıyla yapılır

### Çevirilere Nasıl Katkıda Bulunursunuz

#### Seçenek 1: Belirli bir çeviriyi düzeltin

1. Dil dosyasını bulun: `src/renderer/src/i18n/{lang-code}.json`
2. Yanlış çeviriyi düzeltin
3. Değişiklikle bir PR gönderin

#### Seçenek 2: Eksik anahtarlar ekleyin
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Seçenek 3: Makine çevirilerini gözden geçirin

254 dilimizin birçoğu makine çevirisi ile yapılmıştır. Ana dil konuşucularının incelemeleri son derece değerlidir!

1. Dil dosyanızı seçin
2. Çevirileri gözden geçirin
3. Herhangi bir garip veya yanlış çeviriyi düzeltin
4. Bir PR gönderin

### Dil Kodları

Standart ISO 639-1 kodlarını (örneğin, `ko`, `en`, `ja`, `ar`, `hi`) kullanıyoruz ve gerektiğinde bölgesel varyantlar ekliyoruz (örneğin, `zh-CN`, `pt-BR`).

---

## 🛠 Geliştirme Ortamı

### Ön Koşullar

- Node.js 18+
- npm 9+
- Git

### Kurulum
```bash
```
### Derleme
```bash
```
> Not: Varsayılan 2GB bellek, 254 dil dosyası + Monaco editörü paketi (~38MB render) nedeniyle yeterli değildir.

### Proje Yapısı
```
wia-soom/
├── src/
│   ├── main/          # Electron main process
│   ├── renderer/      # React frontend
│   └── preload/       # Preload scripts
├── docs/              # Documentation
├── scripts/           # Build & automation scripts
└── prompts/           # AI prompt engineering
```
---

## 🙏 Teşekkürler

Her katkı, WIA SOOM'u dünya genelindeki geliştiriciler için daha iyi hale getirir.

Bir yazım hatasını düzeltirseniz, bir dizeyi çevirirseniz, bir eklenti oluşturursanız veya büyük bir özellik eklerseniz — **bu hikayenin bir parçasısınız.**

---

<p align="center"><em>❤️ ile inşa edildi: SmileStory Inc. ve dünya genelindeki katkıda bulunanlar.</em></p>