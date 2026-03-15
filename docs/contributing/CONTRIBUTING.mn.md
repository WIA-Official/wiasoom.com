<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-д хувь нэмэр оруулах</h1>
<p align="center"><strong>Таны хувь нэмэрийг бид хүлээн зөвшөөрнө!</strong></p>
<p align="center">Алдаа засах, шинэ функц, плагин, эсвэл орчуулга — бүх хувь нэмэр чухал.</p>

---

## Агуулгын хүснэгт

- [Зохион байгуулалтын дүрэм](#code-of-conduct)
- [Алдааг хэрхэн мэдээлэх вэ](#-how-to-report-bugs)
- [Функц санал болгох](#-how-to-suggest-features)
- [Плагин хэрхэн илгээх вэ](#-how-to-submit-a-plugin)
- [Pull Request хэрхэн илгээх вэ](#-how-to-submit-a-pull-request)
- [Орчуулгын хувь нэмэр (254 хэл)](#-translation-contributions-254-languages)
- [Хөгжүүлэлтийн тохиргоо](#-development-setup)

---

## Зохион байгуулалтын дүрэм

Бид бүх хүнд тавтай морилох, оролцох боломжийг олгохыг зорьж байна.

- **Хүндэтгэлтэй бай.** Бүх хүнд хүндэтгэлтэй хандах.
- **Бүтээлч бай.** Туслах санал, шүүмжийг санал болго.
- **Оролцогч бай.** Бид 254 хэлний дэмжлэг үзүүлж, дэлхийн бүх орны хувь нэмэр оруулагчдыг угтан авна.
- **Зодолдол байхгүй.** Ямар ч хэлбэрийн ялгаварлан гадуурхалтад тэсвэргүй.

---

## 🐛 Алдааг хэрхэн мэдээлэх вэ

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) руу орно уу
2. **"Шинэ асуудал"** дээр дарна уу
3. **"Алдааны тайлан"** загварыг сонгоно уу
4. Дараах мэдээллийг оруулна уу:
   - WIA SOOM хувилбар (Тохиргоо → Танилцуулга)
   - ОС болон хувилбар (Windows/macOS/Linux)
   - Дахин давтагдах алхмууд
   - Хүлээгдэж буй болон бодит зан үйл
   - Боломжтой бол дэлгэцийн зураг эсвэл терминалын гаралт

---

## 💡 Функц санал болгох

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) руу орно уу
2. **"Шинэ асуудал"** дээр дарна уу
3. **"Функцийн хүсэлт"** загварыг сонгоно уу
4. Дараах зүйлсийг тодорхойлно уу:
   - Та ямар асуудлыг шийдэж байгаа вэ
   - Энэ нь хэрхэн ажиллахыг та төсөөлж байна
   - Та ямар хувилбаруудыг авч үзсэн бэ

---

## 🔌 Плагин хэрхэн илгээх вэ

WIA SOOM нь хүчирхэг плагин системтэй — та 5 минутын дотор өөрийн плагинаа бүтээж болно.

### Түргэн эхлэл
§§§CHUNK_SEPARATOR§§§
### Бүтэн гарын авлага

**[Плагин хөгжүүлэгчийн гарын авлага](docs/PLUGIN_DEVELOPER_GUIDE.md)**-ыг уншина уу:
- Бүтэн API лавлах
- Ажиллаж буй жишээнүүд
- Алхам алхмаар заавар
- Шилдэг практик ба аюулгүй байдлын дүрэм

### Плагинаа илгээх

1. [Plugin Store](https://wiasoom.com) репозиторийг fork хийнэ
2. `plugins/{your-plugin-name}/` дотор плагинаа нэмнэ
3. Pull Request илгээнэ
4. Шинжилгээний дараа таны плагин бүх хэрэглэгчдийн Plugin Store-д гарч ирнэ!

---

## 🔀 Pull Request хэрхэн илгээх вэ

### Гол апп (wia-soom)

1. Репозиторийг fork хийнэ
2. Функцийн салбар бий болгоно: `git checkout -b feat/my-feature`
3. Өөрчлөлтөө хий
4. Орон нутгийн тест:
   ```bash
   ```
5. Тодорхой мессежтэйгээр commit хийнэ:
   ```
   feat: тохиргоонд харанхуй горимын шилжүүлэгч нэмэх
   ```
6. `main` руу PR нээнэ

### Commit мессежийн стандарт

| Таг | Ашиглах |
|--------|---------|
| `feat:` | Шинэ функц |
| `fix:` | Алдаа засах |
| `docs:` | Зөвхөн баримт бичиг |
| `refactor:` | Кодын бүтэц өөрчлөх (зан үйлд өөрчлөлтгүй) |
| `i18n:` | Орчуулгын шинэчлэлт |
| `plugin:` | Плагинтай холбоотой өөрчлөлт |

### PR-ийн шалгалтын жагсаалт

- [ ] Код алдаагүй ажиллаж байна
- [ ] Хатуу код��огдсон утгууд байхгүй (i18n түлхүүрийг ашиглана уу)
- [ ] Үйлдвэрлэлийн кодонд `console.log` үлдээгүй
- [ ] Одоогийн тестүүд одоо ч ажиллаж байна

---

## 🌐 Орчуулгын хувь нэмэр (254 хэл)

WIA SOOM нь **254 хэл**-ийг дэмждэг — Амхарик хэлнээс Зулу хэл хүртэл, Брайль болон RTL хэлүүдийг багтаасан.

### Орчуулга хэрхэн ажилладаг вэ

- Суурь хэлний файл: `src/renderer/src/i18n/en.json`
- Бүх 254 хэлний файлууд нэг директорт байна
- Орчуулга нь `scripts/translate-patch.js` (GPT-4o-mini API) ашиглан хийгддэг

### Орчуулгын хувь нэмэр оруулах

#### Сонголт 1: Тодорхой орчуулгыг засах

1. Хэлний файлыг олоорой: `src/renderer/src/i18n/{lang-code}.json`
2. Буруу орчуулгыг засна
3. Өөрчлөлттэй PR илгээнэ

#### Сонголт 2: Алга болсон түлхүүрүүдийг нэмэх
§§§CHUNK_SEPARATOR§§§
#### Сонголт 3: Машины орчуулгыг хянах

Манай 254 хэлний ихэнх нь машины орчуулгаар орчуулагдсан. Уншигчийн хянасан орчуулга үнэхээр үнэтэй!

1. Хэлний файлаа сонго
2. Орчуулгуудыг хяна
3. Ямар ч эвгүй эсвэл буруу орчуулгыг засна
4. PR илгээнэ

### Хэлний код

Бид стандарт ISO 639-1 кодыг ашигладаг (жишээ нь, `ko`, `en`, `ja`, `ar`, `hi`) шаардлагатай үед бүс нутгийн хувилбаруудтай (жишээ нь, `zh-CN`, `pt-BR`).

---

## 🛠 Хөгжүүлэлтийн тохиргоо

### Урьдчилсан шаардлага

- Node.js 18+
- npm 9+
- Git

### Тохиргоо
§§§CHUNK_SEPARATOR§§§
### Барих
§§§CHUNK_SEPARATOR§§§
> Тэмдэглэл: 254 хэлний файлууд + Monaco редакторын багц (~38MB renderer)-ын улмаас стандарт 2GB heap хангалтгүй.

### Төслийн бүтэц
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Баярлалаа

Таны хийсэн бүрэн бүтэн зүйлс WIA SOOM-ыг дэлхийн хөгжүүлэгчдэд илүү сайн болгоно.

Та алдааг засах, утгыг орчуулах, плагин бүтээх, эсвэл томоохон шинж чанар нэмэх — **та энэ түүхийн нэг хэсэг.**

---

<p align="center"><em>❤️-тайгаар SmileStory Inc. болон дэлхийн бүх хувь нэмэр оруулагчдын хамт бүтээсэн.</em></p>
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```

#### Option 3: Review machine translations

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup

```bash
```

### Build

```bash
```

> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

### Project Structure

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

## 🙏 Thank You

Every contribution makes WIA SOOM better for developers around the world.

Whether you fix a typo, translate a string, build a plugin, or add a major feature — **you are part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>
