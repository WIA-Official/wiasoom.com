<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-a goşulyşmak</h1>
<p align="center"><strong>Biz siziň goşandyňyzy isleýäris!</strong></p>
<p align="center">Bu bir hata düzedişi, täze aýratynlyk, plugin ýa-da terjime bolsun — her goşandyň ähmiýeti bar.</p>

---

## Mazmunyň Jady

- [Döwlet Kody](#code-of-conduct)
- [Hatalary nädip habar bermeli](#-how-to-report-bugs)
- [Aýratynlyklary nädip teklip etmeli](#-how-to-suggest-features)
- [Plugin nädip bermeli](#-how-to-submit-a-plugin)
- [Pull Request nädip bermeli](#-how-to-submit-a-pull-request)
- [Terjime goşandy (254 Dil)](#-translation-contributions-254-languages)
- [Önümçilik Gurulmasy](#-development-setup)

---

## Döwlet Kody

Biz hemmeler üçin hoş geldiňiz we goşulyşýan tejribe bermek üçin özümizi bagyşlaýarys.

- **Hormat ediň.** Hemmeleri ynam bilen garanyňyzda, özüňizi gowy duýuň.
- **Gurluşyk ediň.** Kömekçi pikirleri teklip ediň, ýok ediji tankyt däl.
- **Hüjüm etme.** Her hili diskriminasiýa üçin zero toleransiýa.
- **Hüjüm etme.** Her hili diskriminasiýa üçin zero toleransiýa.

---

## 🐛 Hatalary nädip habar bermeli

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) sahypasyna giriň
2. **"Täze Meselä"** basyň
3. **"Hata Habar"** şablonyny saýlaň
4. Şu maglumatlary goşuň:
   - WIA SOOM wersiýasy (Parametrler → Hakkında)
   - OS we wersiýa (Windows/macOS/Linux)
   - Üçünji ädimler
   - Gözleýän we hakykat arasyndaky hereket
   - Eger mümkin bolsa, ekran suratlary ýa-da terminal çykmagy

---

## 💡 Aýratynlyklary nädip teklip etmeli

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) sahypasyna giriň
2. **"Täze Meselä"** basyň
3. **"Aýratynlyk Talap"** şablonyny saýlaň
4. Şu maglumatlary düşündiriň:
   - Haýsy problemi çözýärsiňiz
   - Onuň nädip işleýändigini göz öňünde tutýarsyňyz
   - Eger göz öňünde tutan başga mümkinçilikler bolsa

---

## 🔌 Plugin nädip bermeli

WIA SOOM güýçli plugin ulgamyna eýe — öz plugin-iňizi 5 minutda döretmek mümkin.

### Tez Başlangyç
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Doly Gideni

**[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** okanyňyzdan soň:
- Dolandyryş API maglumatlary
- Işleýän mysallar
- Ädim-ädim boýunça öwredişler
- Iň gowy tejribeler we howpsuzlyk düzgünleri

### Plugin-iňizi Bermek

1. [Plugin Store](https://wiasoom.com) gaýtadan işläp çykaryň
2. Plugin-iňizi `plugins/{your-plugin-name}/` goşuň
3. Pull Request bermek
4. Gözden geçirişden soň, plugin-iňiz ähli ulanyjylar üçin Plugin Store-da peýda bolar!

---

## 🔀 Pull Request nädip bermeli

### Esasy programma üçin (wia-soom)

1. Repo gaýtadan işläp çykaryň
2. Aýratynlyk şahamçasyny döredin: `git checkout -b feat/my-feature`
3. Üýtgetmeleri ediň
4. Lokal taýdan synag ediň:
   ```bash
   ```
5. Açyk bir habar bilen baglamaly:
   ```
   feat: sazlamalara garaňkylyk moduny goşuň
   ```
6. `main` bilen PR açyň

### Baglamanyň Habar Düzgüni

| Prefix | Ulanyş üçin |
|--------|-------------|
| `feat:` | Täze aýratynlyk |
| `fix:` | Hata düzedişi |
| `docs:` | Faýdalanyş diňe |
| `refactor:` | Kodyň gurluşyny üýtgetmek (hereket üýtgemek däl) |
| `i18n:` | Terjime täzelenmeleri |
| `plugin:` | Plugin bilen baglanyşykly üýtgetmeler |

### PR Çeklist

- [ ] Kod hatasyz işleýär
- [ ] Heňi ýazylan sözler ýok (i18n açarlaryny ulanyň)
- [ ] Üpjünçilik kodunda `console.log` ýok
- [ ] Bar bolan synaglar henizem geçýär

---

## 🌐 Terjime goşandy (254 Dil)

WIA SOOM **254 dili** goldaýar — Amharic-den Zulu-a çenli, Braille we RTL dillerini öz içine alýar.

### Terjime nädip işleýär

- Esas dil faýly: `src/renderer/src/i18n/en.json`
- 254 dil faýllarynyň hemmesi şol bir katalogda
- Terjime `scripts/translate-patch.js` arkaly amala aşyrylýar (GPT-4o-mini API)

### Terjime goşandy nädip bermeli

#### 1-nji saýlaw: Belirli bir terjime düzediň

1. Dil faýlını tapyň: `src/renderer/src/i18n/{lang-code}.json`
2. Nädogry terjime düzediň
3. Üýtgeşme bilen PR bermek

#### 2-nji saýlaw: Ýitirim açarlary goşuň
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### 3-nji saýlaw: Maşyn terjimelerini gözden geçiriň

Bizim 254 dilimiziň köpüsi maşyn arkaly terjime edilendir. Dilliň ýerli sözleri gözden geçirmek gaty gymmatlydyr!

1. Dil faýlňyzy saýlaň
2. Terjimeleri gözden geçiriň
3. Her hili geň ýa-da nädogry terjime düzediň
4. PR bermek

### Dil Kodlary

Biz standart ISO 639-1 kodlaryny (meselem, `ko`, `en`, `ja`, `ar`, `hi`) we zerur bolan ýerlerde sebit görnüşlerini (meselem, `zh-CN`, `pt-BR`) ulanyarys.

---

## 🛠 Önümçilik Gurulmasy

### Talaplar

- Node.js 18+
- npm 9+
- Git

### Gurmak
```bash
```
### Gurmak
```bash
```
> Bellik: Awtomatiki 2GB heap 254 dil faýllary + Monaco redaktory üçin ýeterlik däl (~38MB renderer).

### Taslama gurluşy
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

## 🙏 Sag boluň

Her bir goşant WIA SOOM-y dünýäniň programmistleri üçin has gowy edýär.

Siz ýazgy düzedişi, bir setiri terjime etmek, plugin döretmek ýa-da möhüm aýratynlygy goşmak bilen — **siz bu hekaýanyň bir bölegisiniz.**

---

<p align="center"><em>❤️ bilen SmileStory Inc. we dünýäniň goşant berijileri tarapyndan döredildi.</em></p>