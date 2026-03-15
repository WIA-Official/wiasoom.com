<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-a Töhfə Vermək</h1>
<p align="center"><strong>Biz sizin töhfələrinizi gözləyirik!</strong></p>
<p align="center">İstər bir səhv düzəlişi, yeni xüsusiyyət, plagin, istərsə də tərcümə olsun — hər bir töhfə önəmlidir.</p>

---

## Məzmun Cədvəli

- [Davranış Qaydaları](#code-of-conduct)
- [Səhvləri Necə Bildirmək](#-how-to-report-bugs)
- [Xüsusiyyətləri Necə Təklif Etmək](#-how-to-suggest-features)
- [Plagini Necə Təqdim Etmək](#-how-to-submit-a-plugin)
- [Çəkmə Tələbini Necə Təqdim Etmək](#-how-to-submit-a-pull-request)
- [Tərcümə Töhfələri (254 Dil)](#-translation-contributions-254-languages)
- [İnkişaf Mühitinin Quraşdırılması](#-development-setup)

---

## Davranış Qaydaları

Biz hər kəs üçün dostluq və inklüziv bir təcrübə təqdim etməyə sadiqik.

- **Hörmətli olun.** Hər kəsi ləyaqətlə qarşılayın.
- **Konstruktiv olun.** Yardımçı geribildirim verin, dağıdıc�� tənqid etməyin.
- **İnklüziv olun.** Biz 254 dili dəstəkləyirik və dünyanın hər ölkəsindən iştirakçıları qarşılayırıq.
- **Hər hansı bir təhqirə yol verməyin.** Hər cür ayrı-seçkiliyə sıfır tolerantlıq.

---

## 🐛 Səhvləri Necə Bildirmək

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) səhifəsinə keçin
2. **"Yeni Məsələ"** düyməsini basın
3. **"Səhv Bildirişi"** şablonunu seçin
4. Aşağıdakılar daxil edin:
   - WIA SOOM versiyası (Ayarlar → Haqqında)
   - ƏS və versiya (Windows/macOS/Linux)
   - Təkrarlama addımları
   - Gözlənilən vs. faktiki davranış
   - Mümkünsə ekran görüntüləri və ya terminal çıxışı

---

## 💡 Xüsusiyyətləri Necə Təklif Etmək

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) səhifəsinə keçin
2. **"Yeni Məsələ"** düyməsini basın
3. **"Xüsusiyyət Tələbi"** şablonunu seçin
4. Aşağıdakılara təsvir edin:
   - Hangi problemi həll etdiyinizi
   - Bunun necə işlədiyini təsəvvür etdiyinizi
   - Düşündüyünüz alternativləri

---

## 🔌 Plagini Necə Təqdim Etmək

WIA SOOM güclü bir plagin sisteminə malikdir — öz plaginizi 5 dəqiqəyə qura bilərsiniz.

### Tez Başlanğıc
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tam B��lədçi

**[Plagin İnkişafçı Bələdçisi](docs/PLUGIN_DEVELOPER_GUIDE.md)**-ni oxuyun:
- Tam API istinad
- İşləyən nümunələr
- Addım-addım dərsliklər
- Ən yaxşı təcrübələr və təhlükəsizlik qaydaları

### Plaginizi Təqdim Edin

1. [Plugin Store](https://wiasoom.com) layihəsini forklayın
2. Plaginizi `plugins/{your-plugin-name}/` qovluğuna əlavə edin
3. Çəkmə Tələbini təqdim edin
4. İcmaldan sonra plagininiz bütün istifadəçilər üçün Plagin Mağazasında görünəcək!

---

## 🔀 Çəkmə Tələbini Necə Təqdim Etmək

### Əsas tətbiq (wia-soom) üçün

1. Anbarı forklayın
2. Xüsusiyyət qolu yaradın: `git checkout -b feat/my-feature`
3. Dəyişikliklərinizi edin
4. Yerli olaraq test edin:
   ```bash
   ```
5. Aydın bir mesajla təsdiq edin:
   ```
   feat: ayarları üçün qaranlıq rejim açarı əlavə et
   ```
6. `main`-a qarşı PR açın

### Təsdiq Mesajı Konvensiyası

| Ön Söz | İstifadə üçün |
|--------|---------------|
| `feat:` | Yeni xüsusiyyət |
| `fix:` | Səhv düzəlişi |
| `docs:` | Yalnız sənəd |
| `refactor:` | Kodun yenidən qurulması (davranış dəyişmədən) |
| `i18n:` | Tərcümə yeniləmələri |
| `plugin:` | Plaginlə bağlı dəyişikliklər |

### PR Yoxlama Siyahısı

- [ ] Kod səhvsiz işləyir
- [ ] Hardcoded stringlər yoxdur (i18n açarlarından istifadə edin)
- [ ] İstehsal kodunda `console.log` qalmayıb
- [ ] Mövcud testlər hələ də keçdi

---

## 🌐 Tərcümə Töhfələri (254 Dil)

WIA SOOM **254 dili** dəstəkləyir — Amharic-dən Zulu-ya, Braille və RTL dilləri də daxil olmaqla.

### Tərcümə Necə İşləyir

- Əsas dil faylı: `src/renderer/src/i18n/en.json`
- Bütün 254 dil faylı eyni qovluqdadır
- Tərcümə `scripts/translate-patch.js` vasitəsilə edilir (GPT-4o-mini API)

### Tərcümələrə Necə Töhfə Vermək

#### Seçim 1: Müəyyən bir tərcüməni düzəltmək

1. Dil faylını tapın: `src/renderer/src/i18n/{lang-code}.json`
2. Yanlış tərcüməni düzəldin
3. Dəyişikliyi olan PR təqdim edin

#### Seçim 2: Əskik açarları əlavə edin
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Seçim 3: Maşın tərcümələrini nəzərdən keçirin

Bizim 254 dilimizin çoxu maşın tərcüməsi ilə əldə edilib. Ana dil danışanların nəzərdən keçirməsi son dərəcə dəyərlidir!

1. Dil faylınızı seçin
2. Tərcümələri nəzərdən keçirin
3. Hər hansı bir qəribə və ya yanlış tərcüməni düzəldin
4. PR təqdim edin

### Dil Kodu

Biz standart ISO 639-1 kodlarından (məsələn, `ko`, `en`, `ja`, `ar`, `hi`) istifadə edirik, lazım olduqda regional variantlarla (məsələn, `zh-CN`, `pt-BR`).

---

## 🛠 İnkişaf Mühitinin Quraşdırılması

### Tələblər

- Node.js 18+
- npm 9+
- Git

### Quraşdırma
```bash
```
### Yığma
```bash
```
> Qeyd: 254 dil faylı + Monaco redaktoru paketi (~38MB render) səbəbindən standart 2GB heap kifayət etmir.

### Layihə Strukturunuz
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

## 🙏 Təşəkkürlər

Hər bir töhfə WIA SOOM-u dünya üzrə inkişaf etdiricilər üçün daha yaxşı edir.

İstər bir yazı səhvini düzəldəsiniz, istər bir mətn tərcümə edəsiniz, istərsə də bir plugin yaradasınız və ya əsas bir xüsusiyyət əlavə edəsiniz — **siz bu hekayənin bir hissəsisiniz.**

---

<p align="center"><em>❤️ ilə hazırlanmışdır SmileStory Inc. və dünya üzrə töhfə verənlər tərəfindən.</em></p>