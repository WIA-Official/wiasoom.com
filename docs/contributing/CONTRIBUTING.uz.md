<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM'ga Hissa Qo'shish</h1>
<p align="center"><strong>Sizning hissangizni kutamiz!</strong></p>
<p align="center">Bu xato tuzatish, yangi xususiyat, plagin yoki tarjima bo'lsin — har bir hissa muhimdir.</p>

---

## Mazmun Jadvali

- [Davranish Qoidalari](#code-of-conduct)
- [Xatolarni Qanday Hisobga Olish](#-how-to-report-bugs)
- [Xususiyatlarni Qanday Taklif Qilish](#-how-to-suggest-features)
- [Plaginni Qanday Taqdim Qilish](#-how-to-submit-a-plugin)
- [Pull Requestni Qanday Taqdim Qilish](#-how-to-submit-a-pull-request)
- [Tarjima Hissalari (254 Tilda)](#-translation-contributions-254-languages)
- [Rivojlanish Sozlamalari](#-development-setup)

---

## Davranish Qoidalari

Biz har kim uchun qulay va inklyuziv tajriba taqdim etishga intilamiz.

- **Hurmat qiling.** Har kimni qadr-qimmati bilan muomala qiling.
- **Foydali bo'ling.** Foydali fikrlar bering, yomon tanqid emas.
- **Inkluziv bo'ling.** Biz 254 tilni qo'llab-quvvatlaymiz va dunyodagi har bir mamlakatdan hissa qo'shuvchilarni kutamiz.
- **Ta’qib qilmaslik.** Har qanday kamsitishga nisbatan nol tolarans.

---

## 🐛 Xatolarni Qanday Hisobga Olish

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) sahifasiga o'ting
2. **"Yangi Muammo"** tugmasini bosing
3. **"Xato Hisoboti"** shablonini tanlang
4. Quyidagilarni qo'shing:
   - WIA SOOM versiyasi (Sozlamalar → Haqida)
   - OS va versiya (Windows/macOS/Linux)
   - Takrorlash uchun qadamlar
   - Kutilgan va haqiqiy xulq-atvor
   - Ekran rasmlari yoki terminal chiqishi agar mumkin bo'lsa

---

## 💡 Xususiyatlarni Qanday Taklif Qilish

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) sahifasiga o'ting
2. **"Yangi Muammo"** tugmasini bosing
3. **"Xususiyat Taklifi"** shablonini tanlang
4. Quyidagilarni tasvirlang:
   - Qanday muammoni hal qilmoqdasiz
   - Bu qanday ishlashini tasavvur qilasiz
   - O'ylab ko'rgan alternativalaringiz

---

## 🔌 Plaginni Qanday Taqdim Qilish

WIA SOOM kuchli plagin tizimiga ega — siz o'z plaginlaringizni 5 daqiqada yaratishingiz mumkin.

### Tez Boshlash
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### To'liq Qo'llanma

**[Plagin Ishlab Chiquvchi Qo'llanmasi](docs/PLUGIN_DEVELOPER_GUIDE.md)** ni o'qing:
- To'liq API ma'lumotnomasi
- Ishlayotgan misollar
- Qadam-baqadam darsliklar
- Eng yaxshi amaliyotlar va xavfsizlik qoidalari

### Plaginingizni Taqdim Qiling

1. [Plugin Store](https://wiasoom.com) ni fork qiling
2. Plaginingizni `plugins/{your-plugin-name}/` ga qo'shing
3. Pull Requestni taqdim qiling
4. Tekshirilgandan so'ng, plaginingiz barcha foydalanuvchilar uchun Plagin Do'konida paydo bo'ladi!

---

## 🔀 Pull Requestni Qanday Taqdim Qilish

### Asosiy ilova uchun (wia-soom)

1. Repozitoriyani fork qiling
2. Xususiyat tarmog'ini yarating: `git checkout -b feat/my-feature`
3. O'zgarishlaringizni qiling
4. Mahalliy sinov qiling:
   ```bash
   ```
5. Aniq xabar bilan commit qiling:
   ```
   feat: sozlamalarga qorong'u rejimni qo'shish
   ```
6. `main` ga qarshi PR oching

### Commit Xabari Konvensiyasi

| Prefiks | Foydalanish uchun |
|---------|-------------------|
| `feat:` | Yangi xususiyat |
| `fix:`  | Xato tuzatish |
| `docs:` | Faqat hujjatlar |
| `refactor:` | Kodni qayta tuzish (xulq-atvor o'zgarishisiz) |
| `i18n:` | Tarjima yangilanishlari |
| `plugin:` | Plagin bilan bog'liq o'zgarishlar |

### PR Ro'yxati

- [ ] Kod xatosiz ishlaydi
- [ ] Hech qanday qattiq kodlangan satrlar yo'q (i18n kalitlarini ishlating)
- [ ] Ishlab chiqarish kodida hech qanday `console.log` qoldirilmagan
- [ ] Mavjud testlar hali ham o'tadi

---

## 🌐 Tarjima Hissalari (254 Tilda)

WIA SOOM **254 tilni** qo'llab-quvvatlaydi — Amharicdan Zulu gacha, Braille va RTL tillarini o'z ichiga olgan.

### Tarjima Qanday Ishlaydi

- Asosiy til fayli: `src/renderer/src/i18n/en.json`
- Barcha 254 til fayllari bir xil katalogda
- Tarjima `scripts/translate-patch.js` orqali amalga oshiriladi (GPT-4o-mini API)

### Tarjimalarni Qanday Hissa Qo'shish

#### Variant 1: Muayyan tarjimani tuzatish

1. Til faylini toping: `src/renderer/src/i18n/{lang-code}.json`
2. Noto'g'ri tarjimani tuzating
3. O'zgarish bilan PR taqdim qiling

#### Variant 2: Yo'qolgan kalitlarni qo'shish
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Variant 3: Mashina tarjimalarini ko'rib chiqish

Bizning 254 tilimizdan ko'pchilik mashina tarjima qilingan. Mahalliy spikerlarning ko'riklari juda qadrli!

1. O'z til faylingizni tanlang
2. Tarjimalarni ko'rib chiqing
3. Har qanday noqulay yoki noto'g'ri tarjimalarni tuzating
4. PR taqdim qiling

### Til Kodlari

Biz standart ISO 639-1 kodlarini (masalan, `ko`, `en`, `ja`, `ar`, `hi`) kerak bo'lganda mintaqaviy variantlar bilan ishlatamiz (masalan, `zh-CN`, `pt-BR`).

---

## 🛠 Rivojlanish Sozlamalari

### Talablar

- Node.js 18+
- npm 9+
- Git

### Sozlash
```bash
```
### Qurish
```bash
```
> Eslatma: 254 til fayllari + Monaco tahrirchisi to'plami (~38MB render) sababli standart 2GB xotira yetarli emas.

### Loyihaning Tuzilishi
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

## 🙏 Rahmat

Har bir hissa WIA SOOM'ni butun dunyo bo'ylab dasturchilar uchun yaxshiroq qiladi.

Siz xatoni tuzatsangiz, bir satrni tarjima qilsangiz, plagin yaratsangiz yoki katta xususiyat qo'shsangiz — **siz bu hikoyaning bir qismisiz.**

---

<p align="center"><em>❤️ bilan qurilgan SmileStory Inc. va butun dunyo bo'ylab hissa qo'shuvchilar tomonidan.</em></p>