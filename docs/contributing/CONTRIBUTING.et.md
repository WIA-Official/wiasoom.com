<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Panustamine WIA SOOM-i</h1>
<p align="center"><strong>Me hindame teie panust!</strong></p>
<p align="center">Olgu see siis veaparandus, uus funktsioon, plugin või tõlge — iga panus on oluline.</p>

---

## Sisukord

- [Käitumisreeglid](#code-of-conduct)
- [Kuidas vead raporteerida](#-how-to-report-bugs)
- [Kuidas funktsioone soovitada](#-how-to-suggest-features)
- [Kuidas pluginat esitada](#-how-to-submit-a-plugin)
- [Kuidas esitada Pull Request](#-how-to-submit-a-pull-request)
- [Tõlke panused (254 keelt)](#-translation-contributions-254-languages)
- [Arenduse seadistamine](#-development-setup)

---

## Käitumisreeglid

Me oleme pühendunud kõigile sõbraliku ja kaasava kogemuse pakkumisele.

- **Ole lugupidav.** Kohtle kõiki väärikusega.
- **Ole konstruktiivne.** Paku kasulikku tagasisidet, mitte hävitavat kriitikat.
- **Ole kaasav.** Toetame 254 keelt ja tervitame panustajaid igast riigist maailmas.
- **Ei ahistamisele.** Nulltolerants igasuguse diskrimineerimise suhtes.

---

## 🐛 Kuidas vead raporteerida

1. Mine [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliki **"New Issue"**
3. Vali **"Bug Report"** mall
4. Lisa:
   - WIA SOOM versioon (Seaded → Teave)
   - OS ja versioon (Windows/macOS/Linux)
   - Sammud, et viga uuesti esitada
   - Oodatav vs tegelik käitumine
   - Ekraanipildid või terminali väljund, kui võimalik

---

## 💡 Kuidas funktsioone soovitada

1. Mine [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliki **"New Issue"**
3. Vali **"Feature Request"** mall
4. Kirjeldage:
   - Millist probleemi te lahendate
   - Kuidas te kujutate ette, et see töötab
   - Kõik alternatiivid, mida olete kaalunud

---

## 🔌 Kuidas pluginat esitada

WIA SOOM-il on võimas pluginasüsteem — saate oma plugina 5 minutiga valmis teha.

### Kiire algus
§§§CHUNK_SEPARATOR§§§
### Täielik juhend

Lugege **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)**, et:
- Täielik API viide
- Tööalased näited
- Samm-sammult juhendid
- Parimad praktikad ja turvareeglid

### Esitage oma plugin

1. Forkige [Plugin Store](https://wiasoom.com)
2. Lisage oma plugin `plugins/{your-plugin-name}/`
3. Esitage Pull Request
4. Pärast ülevaatamist ilmub teie plugin Plugin Store'i kõigile kasutajatele!

---

## 🔀 Kuidas esitada Pull Request

### Peamise rakenduse jaoks (wia-soom)

1. Forkige hoidla
2. Looge funktsiooniharu: `git checkout -b feat/my-feature`
3. Tehke oma muudatused
4. Testige kohapeal:
   ```bash
   ```
5. Commitige selge sõnumiga:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pushige ja avage PR `main` vastu

### Commiti sõnumi konventsioon

| Eesliide | Kasutatakse |
|----------|-------------|
| `feat:`  | Uus funktsioon |
| `fix:`   | Veaparandus |
| `docs:`  | Ainult dokumentatsioon |
| `refactor:` | Koodi ümberstruktureerimine (käitumise muutuseta) |
| `i18n:`  | Tõlkeuuendused |
| `plugin:` | Pluginaga seotud muudatused |

### PR kontrollnimekiri

- [ ] Kood töötab ilma vigadeta
- [ ] Pole kõvakooditud stringe (kasutage i18n võtmeid)
- [ ] Pole `console.log` tootmisoodes
- [ ] Olemasolevad testid läbivad endiselt

---

## 🌐 Tõlke panused (254 keelt)

WIA SOOM toetab **254 keelt** — alates amhari keelest kuni zulu keelteni, sealhulgas Braille'i ja RTL keeli.

### Kuidas tõlge töötab

- Põhikeelefail: `src/renderer/src/i18n/en.json`
- Kõik 254 keelefaili on samas kataloogis
- Tõlge toimub `scripts/translate-patch.js` (GPT-4o-mini API) kaudu

### Kuidas tõlgetesse panustada

#### Valik 1: Parandage konkreetne tõlge

1. Leidke keelefail: `src/renderer/src/i18n/{lang-code}.json`
2. Parandage vale tõlge
3. Esitage PR muudatusega

#### Valik 2: Lisage puuduvad võtmed
§§§CHUNK_SEPARATOR§§§
#### Valik 3: Kontrollige masintõlkeid

Paljusid meie 254 keelt on tõlgitud masinaga. Emakeelena rääkijate ülevaated on äärmiselt väärtuslikud!

1. Valige oma keelefail
2. Kontrollige tõlkeid
3. Parandage kõik kohmakad või valed tõlked
4. Esitage PR

### Keelekoodid

Kasutame standardseid ISO 639-1 koode (nt `ko`, `en`, `ja`, `ar`, `hi`) piirkondlike variantidega, kui vajalik (nt `zh-CN`, `pt-BR`).

---

## 🛠 Arenduse seadistamine

### Eeltingimused

- Node.js 18+
- npm 9+
- Git

### Seadistamine
§§§CHUNK_SEPARATOR§§§
### Ehitus
§§§CHUNK_SEPARATOR§§§
> Märkus: Vaikimisi 2GB mälu ei piisa, kuna on 254 keelefaili + Monaco redigeerija paketid (~38MB renderdaja).

### Projekti struktuur
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Aitäh

Iga panus muudab WIA SOOM'i paremaks arendajatele üle kogu maailma.

Olgu see siis õigekirja parandamine, stringi tõlkimine, plugina loomine või suure funktsiooni lisamine — **sa oled osa sellest loost.**

---

<p align="center"><em>Valmistatud ❤️ poolt SmileStory Inc. ja panustajatelt üle kogu maailma.</em></p>
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
