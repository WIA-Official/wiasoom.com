<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ho kenya letsoho ho WIA SOOM</h1>
<p align="center"><strong>Re thabela litlhahiso tsa hau!</strong></p>
<p align="center">E le hore e be phetoho, sehlahisoa se secha, plugin, kapa phetolelo — kontribushene e ngoe le e ngoe e bohlokoa.</p>

---

## Lenane la Litaba

- [Molao oa Boitšoaro](#code-of-conduct)
- [Mokhoa oa ho Reporta Bugs](#-how-to-report-bugs)
- [Mokhoa oa ho Khothaletsa Litlhahiso](#-how-to-suggest-features)
- [Mokhoa oa ho Fana ka Plugin](#-how-to-submit-a-plugin)
- [Mokhoa oa ho Fana ka Pull Request](#-how-to-submit-a-pull-request)
- [Litlhahiso tsa Phetho (254 Languages)](#-translation-contributions-254-languages)
- [Setheo sa Nts'etsopele](#-development-setup)

---

## Molao oa Boitšoaro

Re ikemiselitse ho fa boiphihlelo bo amohelehang le bo kenyelletsang bakeng sa bohle.

- **E-ba le tlhompho.** Hlahisa bohle ka borai.
- **E-ba le bohlale.** Fa maikutlo a thusang, eseng a senyang.
- **E-ba le kenyelletso.** Re tšehetsa lipuo tse 254 le ho amohela ba kenang ho tsoa naheng e 'ngoe le e 'ngoe lefatšeng.
- **Ha ho ho hlaseloa.** Ho se na mamello bakeng sa khethollo efe kapa efe.

---

## 🐛 Mokhoa oa ho Reporta Bugs

1. Eya ho [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tobetsa **"New Issue"**
3. Khetha template ea **"Bug Report"**
4. Kenyeletsa:
   - WIA SOOM version (Settings → About)
   - OS le version (Windows/macOS/Linux)
   - Mehato ea ho hlahisa
   - Boitšoaro bo lebelletsoeng vs. bo amanang
   - Lifoto kapa output ea terminal haeba ho khoneha

---

## 💡 Mokhoa oa ho Khothaletsa Litlhahiso

1. Eya ho [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Tobetsa **"New Issue"**
3. Khetha template ea **"Feature Request"**
4. Hlalosa:
   - Bothata boo u bo rarollang
   - Mokhoa oo u nahanang hore e tla sebetsa
   - Le mekhahlelo efe kapa efe eo u e nahanang

---

## 🔌 Mokhoa oa ho Fana ka Plugin

WIA SOOM e na le sistimi e matla ea plugin — u ka haha plugin ea hau ka metsotso e 5.

### Qalo e Potlakileng
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Tataiso e Felletseng

Bala **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** bakeng sa:
- Tlhahlobo e felletseng ea API
- Mehlala e sebetsang
- Lithuto tse amanang le mehato
- Mekhoa e metle le melao ea ts'ireletso

### Fana ka Plugin ea Hao

1. Fork [Plugin Store](https://wiasoom.com)
2. Kenyeletsa plugin ea hau ho `plugins/{your-plugin-name}/`
3. Fana ka Pull Request
4. Ka mor'a ho hlahloba, plugin ea hau e tla bonahala ho Plugin Store bakeng sa basebelisi bohle!

---

## 🔀 Mokhoa oa ho Fana ka Pull Request

### Bakeng sa app e kholo (wia-soom)

1. Fork repository
2. Bua le branch ea feature: `git checkout -b feat/my-feature`
3. Etsa liphetoho tsa hau
4. Testa locally:
   ```bash
   ```
5. Commit ka molaetsa o hlakileng:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push le ho bula PR khahlanong le `main`

### Molao oa Molaetsa oa Commit

| Prefix | Sebelisa bakeng sa |
|--------|---------|
| `feat:` | Feature e ncha |
| `fix:` | Phetoho ea bug |
| `docs:` | Tokomane feela |
| `refactor:` | Ho hlophisa khoutu (ha ho na phetoho ea boitšoaro) |
| `i18n:` | Litlhahiso tsa phetolelo |
| `plugin:` | Liphetoho tse amanang le plugin |

### Checklist ea PR

- [ ] Khoutu e sebetsa ntle le liphoso
- [ ] Ha ho litlhaku tse tiileng (sebelisa i18n keys)
- [ ] Ha ho `console.log` e setseng khoutu ea tlhahiso
- [ ] Liteko tse teng li ntse li feta

---

## 🌐 Litlhahiso tsa Phetho (254 Languages)

WIA SOOM e tšehetsa **254 languages** — ho tloha Amharic ho isa Zulu, ho kenyelletsa Braille le lipuo tsa RTL.

### Mokhoa oa Phetho

- Faele ea puo ea motheo: `src/renderer/src/i18n/en.json`
- Li-file tsohle tse 254 tsa lipuo li ho foldara e le 'ngoe
- Phetho e etsoa ka `scripts/translate-patch.js` (GPT-4o-mini API)

### Mokhoa oa ho Kenyeletsa Phetho

#### Khetho 1: Lokisa phetolelo e itseng

1. Fumana faele ea puo: `src/renderer/src/i18n/{lang-code}.json`
2. Lokisa phetolelo e fosahetseng
3. Fana ka PR ka phetoho

#### Khetho 2: Kenyeletsa likhetho tse fosahetseng
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Khetho 3: Hlahloba phetolelo ea mochini

Lipuo tse ngata tsa rona tse 254 li phetolelitsoe ke mochini. Tlhahlobo ea batho ba buang puo e bohlokoa haholo!

1. Khetha faele ea hau ea puo
2. Hlahloba liphetho
3. Lokisa liphetho leha e le life tse sa lokelang kapa tse fosahetseng
4. Fana ka PR

### Likhoele tsa Puo

Re sebelisa likhoele tse tloaelehileng tsa ISO 639-1 (mohlala, `ko`, `en`, `ja`, `ar`, `hi`) le mekhahlelo ea sebaka ha ho hlokahala (mohlala, `zh-CN`, `pt-BR`).

---

## 🛠 Setheo sa Nts'etsopele

### Melemo e hlokahalang

- Node.js 18+
- npm 9+
- Git

### Setheo
```bash
```
### Haha
```bash
```
> Tlhokomeliso: The default 2GB heap ha e lekane ka lebaka la li-file tse 254 tsa lipuo + Monaco editor bundle (~38MB renderer).

### Sehlahisoa sa Morero
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

Lefapha le leng le le leng le thusa ho ntlafatsa WIA SOOM bakeng sa bahlahisi lefatšeng ka bophara.

Na u lokisa phoso, u fetole mola, u haha plugin, kapa u eketsa feature e kholo — **u karolo ea pale ena.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>