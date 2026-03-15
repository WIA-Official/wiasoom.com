<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">შეთანხმება WIA SOOM-ისთვის</h1>
<p align="center"><strong>ჩვენ გვინდა თქვენი წვლილი!</strong></p>
<p align="center">თუ ეს არის ბაგის გამოსწორება, ახალი ფუნქცია, პლაგინი, თუ თარგმანი — თითოეული წვლილი მნიშვნელოვანია.</p>

---

## შინაარსის ცხრილი

- [ქცევის კოდექსი](#code-of-conduct)
- [როგორ უნდა ვაუწყოთ ბაგები](#-how-to-report-bugs)
- [როგორ უნდა შევთავაზოთ ფუნქციები](#-how-to-suggest-features)
- [როგორ უნდა შევიტანოთ პლაგინი](#-how-to-submit-a-plugin)
- [როგორ უნდა შევიტან��თ Pull Request](#-how-to-submit-a-pull-request)
- [თარგმანის წვლილი (254 ენა)](#-translation-contributions-254-languages)
- [განვითარების კონფიგურაცია](#-development-setup)

---

## ქცევის კოდექსი

ჩვენ ვალდებულნი ვართ უზრუნველვყოთ მისასალმებელი და ინკლუზიური გამოცდილება ყველასთვის.

- **იყავით პატივისცემით.** მოექეცით ყველას ღირსებით.
- **იყავით კონსტრუქციული.** მიაწვდეთ სასარგებლო გამოხმაურება, არა დამანგრეველი კრიტიკა.
- **იყავით ინკლუზიური.** ჩვენ ვუჭერთ მხარს 254 ენას და ვიღებთ წვლილებს ყველა ქვეყნის წარმომადგენლებისგან.
- **არავითარი შევიწროება.** ნულოვანი ტოლერანტობა ნებისმიერი სახის დისკრიმინაციის მიმართ.

---

## 🐛 როგორ უნდა ვაუწყოთ ბაგები

1. გადადით [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. დააწკაპუნეთ **"ახალი საკითხი"**
3. აირჩიეთ **"ბაგის ანგარიში"** შაბლონი
4. მოიცავს:
   - WIA SOOM ვერსია (პარამეტრები → შესახებ)
   - ოპერაციული სისტემა და ვერსია (Windows/macOS/Linux)
   - გამეორების ნაბიჯები
   - მოსალოდნელი და რეალური ქცევა
   - ეკრანის სურათები ან ტერმინალის გამომავალი, თუ შესაძლებელია

---

## 💡 როგორ უნდა შევთავაზოთ ფუნქციები

1. გადადით [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. დააწკაპუნეთ **"ახალი საკითხი"**
3. აირჩიეთ **"ფუნქციის მოთხოვნა"** შაბლონი
4. აღწერეთ:
   - რა პრობლემას ხსნით
   - როგორ წარმოგიდგენთ მისი მუშაობის პროცესს
   - ნებისმიერი ალტერნატივა, რომელიც განიხილეთ

---

## 🔌 როგორ უნდა შევიტანოთ პლაგინი

WIA SOOM-ის ძლიერი პლაგინის სისტემა აქვს — შეგიძლიათ შექმნათ თქვენი პლაგინი 5 წუთში.

### სწრაფი დაწყება
§§§CHUNK_SEPARATOR§§§
### სრული სახელმძღვანელო

წაიკითხეთ **[პლაგინის შემქმნელის სახელმძღვანელო](docs/PLUGIN_DEVELOPER_GUIDE.md)**:
- სრული API სია
- სამუშაო მაგალითები
- ნაბიჯ-ნაბიჯ გაწვდილი
- საუკეთესო პრაქტიკები და უსაფრთხოების წესები

### შეიტანეთ თქვენი პლაგინი

1. გააკეთეთ Fork [Plugin Store](https://wiasoom.com)
2. დაამატეთ თქვენი პლაგინი `plugins/{your-plugin-name}/`
3. შეიტანეთ Pull Request
4. მიმოხილვის შემდეგ, თქვენი პლაგინი გამოჩნდება Plugin Store-ში ყველა მომხმარებლისთვის!

---

## 🔀 როგორ უნდა შევიტანოთ Pull Request

### მთავარი აპლიკაციისთვის (wia-soom)

1. გააკეთეთ Fork რეპოზიტორიის
2. შექმენით ფუნქციის შტო: `git checkout -b feat/my-feature`
3. გააკეთეთ თქვენი ცვლილებები
4. ტესტირება ადგილობრივად:
   ```bash
   ```
5. გააკეთეთ კომიტი ნათელი შეტყობინებით:
   ```
   feat: დაამატეთ მუქი რეჟიმის გადართვა პარამეტრებში
   ```
6. დააწვინეთ და გახსენით PR `main`-ზე

### კომიტის შეტყობინების კონვენცია

| პრეფიქსი | გამოყენება |
|----------|-----------|
| `feat:`  | ახალი ფუნქცია |
| `fix:`   | ბაგის გამოსწორება |
| `docs:`  | მხოლოდ დოკუმენტაცია |
| `refactor:` | კოდის გადახედვა (ბრაუზერის ცვლილება არ არის) |
| `i18n:`  | თარგმანის განახლებები |
| `plugin:` | პლაგინთან დაკავშირებული ცვლილებები |

### PR სია

- [ ] კოდი მუშაობს შეცდომების გარეშე
- [ ] არ არის Hardcoded სტრიქონები (გამოიყენეთ i18n გასაღებები)
- [ ] არ არის `console.log`, რომელიც დარჩა წარმოების კოდში
- [ ] არსებული ტესტები კვლავ წარმატებით გადის

---

## 🌐 თარგმანის წვლილი (254 ენა)

WIA SOOM უჭერს მხარს **254 ენას** — ამჰარიკიდან ზულუზე, მათ შორის ბრაილი და RTL ენები.

### როგორ მუშაობს თარგმანი

- ბაზური ენის ფაილი: `src/renderer/src/i18n/en.json`
- ყველა 254 ენის ფაილი იმავე დირექტორიაშია
- თარგმანი ხდება `scripts/translate-patch.js` (GPT-4o-mini API)-ს მეშვეობით

### როგორ უნდა შევიტანოთ თარგმანები

#### ვარიანტი 1: კონკრეტული თარგმანის გამოსწორება

1. მოიძიეთ ენის ფაილი: `src/renderer/src/i18n/{lang-code}.json`
2. გამოსწორეთ არასწორი თარგმანი
3. შეიტანეთ PR ცვლილებით

#### ვარიანტი 2: დაკარგული გასაღებების დამატება
§§§CHUNK_SEPARATOR§§§
#### ვარიანტი 3: მანქანური თარგმანების მიმოხილვა

ჩვენი 254 ენის ბევრი მანქანურად თარგმნილია. მშვენიერი ღირებულებაა მშვენიერი მნახველების მიმოხილვა!

1. აირჩიეთ თქვენი ენის ფაილი
2. მიმოიხილეთ თარგმანები
3. გამოსწორეთ ნებისმიერი უხერხული ან არასწორი თარგმანი
4. შეიტანეთ PR

### ენის კოდები

ჩვენ ვიყენებთ სტანდარტულ ISO 639-1 კოდებს (მაგ., `ko`, `en`, `ja`, `ar`, `hi`) რეგიონალური ვარიანტებით საჭიროების შემთხვევაში (მაგ., `zh-CN`, `pt-BR`).

---

## 🛠 განვითარების კონფიგურაცია

### წინაპირობები

- Node.js 18+
- npm 9+
- Git

### კონფიგურაცია
§§§CHUNK_SEPARATOR§§§
### მშენებლობა
§§§CHUNK_SEPARATOR§§§
> შენიშვნა: სტანდარტული 2GB heap არ არის საკმარისი 254 ენის ფაილების + Monaco რედაქტორის პაკეტის (~38MB რენდერი).

### პროექტის სტრუქტურა
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 მადლობა

ყოველი წვლილი აუმჯობესებს WIA SOOM-ს მსოფლიოს მასშტაბით დეველოპერებისთვის.

დამატებით, თუ თქვენ გამოასწორებთ შეცდომას, თარგმნით სტრიქონს, შექმნით პლაგინს ან დაამატებთ მნიშვნელოვან ფუნქციას — **თქვენ ამ ისტორიაში ხართ.**

---

<p align="center"><em>შექმნილია ❤️ SmileStory Inc.-ის და მსოფლიოს მასშტაბით წვლილის შემომწირველების მიერ.</em></p>
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
