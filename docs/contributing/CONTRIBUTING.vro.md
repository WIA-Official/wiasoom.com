<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Panustamine WIA SOOM-i</h1>
<p align="center"><strong>Me ootame teie panust!</strong></p>
<p align="center">Olgu see siis vea parandamine, uus funktsioon, plugin või tõlge — iga panus on oluline.</p>

---

## Sisukord

- [Käitumisreeglid](#käitumisreeglid)
- [Kuidas vigu raporteerida](#-kuidas-vigu-raporteerida)
- [Kuidas funktsioone soovitada](#-kuidas-funktsioone-soovitada)
- [Kuidas pluginat esitada](#-kuidas-pluginat-esitada)
- [Kuidas esitada Pull Request](#-kuidas-esitada-pull-request)
- [Tõlke panused (254 keelt)](#-tõlke-panused-254-keelt)
- [Arenduse seadistamine](#-arenduse-seadistamine)

---

## Käitumisreeglid

Me oleme pühendunud kõigile tervitava ja kaasava kogemuse pakkumisele.

- **Ole lugupidav.** Kohtle kõiki väärikusega.
- **Ole konstruktiivne.** Paku kasulikku tagasisidet, mitte hävitavat kriitikat.
- **Ole kaasav.** Me toetame 254 keelt ja ootame panustajaid igast riigist Maal.
- **Ei ahistamisele.** Nulltolerants igasuguse diskrimineerimise suhtes.

---

## 🐛 Kuidas vigu raporteerida

1. Mine [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliki **"Uus probleem"**
3. Vali **"Vea raport"** mall
4. Lisa:
   - WIA SOOM versioon (Seaded → Teave)
   - OS ja versioon (Windows/macOS/Linux)
   - Sammud, et viga taastoota
   - Oodatud vs. tegelik käitumine
   - Ekraanipildid või terminali väljund, kui võimalik

---

## 💡 Kuidas funktsioone soovitada

1. Mine [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliki **"Uus probleem"**
3. Vali **"Funktsiooni soovitus"** mall
4. Kirjeldage:
   - Millist probleemi te lahendate
   - Kuidas te kujutate ette, et see töötab
   - Kõik alternatiivid, mida olete kaalunud

---

## 🔌 Kuidas pluginat esitada

WIA SOOM-il on võimas pluginasüsteem — saate oma plugina 5 minutiga luua.

### Kiire algus
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Täielik juhend

Loe **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)**, et:
- Täielik API viide
- Töö näited
- Samm-sammult õpetused
- Parimad tavad ja turvareeglid

### Esita oma plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Lisa oma plugin `plugins/{your-plugin-name}/`
3. Esita Pull Request
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
5. Komiteerige selge sõnumiga:
   ```
   feat: lisa tume režiimi lüliti seadetele
   ```
6. Pushige ja avage PR `main` vastu

### Komitee sõnumi konventsioon

| Eesliide | Kasutamiseks |
|----------|--------------|
| `feat:`  | Uus funktsioon |
| `fix:`   | Vea parandamine |
| `docs:`  | Ainult dokumentatsioon |
| `refactor:` | Koodi ümberstruktureerimine (käitumise muutuseta) |
| `i18n:`  | Tõlkeuuendused |
| `plugin:` | Pluginaga seotud muudatused |

### PR kontrollnimekiri

- [ ] Kood töötab ilma vigadeta
- [ ] Ei ole kõvade stringe (kasutage i18n võtmeid)
- [ ] Ei ole `console.log` tootmisoodes
- [ ] Olemasolevad testid töötavad endiselt

---

## 🌐 Tõlke panused (254 keelt)

WIA SOOM toetab **254 keelt** — alates amhari keeles kuni zulu keeleni, sealhulgas Braille'i ja RTL keeli.

### Kuidas tõlge töötab

- Põhikeele fail: `src/renderer/src/i18n/en.json`
- Kõik 254 keelefaili on samas kataloogis
- Tõlge toimub `scripts/translate-patch.js` (GPT-4o-mini API) kaudu

### Kuidas panustada tõlgetesse

#### Valik 1: Parandage konkreetne tõlge

1. Leidke keelefail: `src/renderer/src/i18n/{lang-code}.json`
2. Parandage vale tõlge
3. Esitage PR muudatusega

#### Valik 2: Lisage puuduolevad võtmed
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Valik 3: Kontrollige masintõlkeid

Paljud meie 254 keelt on masintõlgitud. Emakeelsete kõnelejate ülevaated on äärmiselt väärtuslikud!

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
```bash
```
### Ehitus
```bash
```
> Märkus: Vaikimisi 2GB mälu ei piisa 254 keelefaili + Monaco redaktori paketi (~38MB renderdaja) tõttu.

### Projekti struktuur
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

## 🙏 Aitäh

Iga panus teeb WIA SOOM paremaks arendajatele üle kogu maailma.

Olgu see siis trükivea parandamine, stringi tõlkimine, plugina loomine või suure funktsiooni lisamine — **sa oled osa sellest loost.**

---

<p align="center"><em>Valmistatud ❤️ SmileStory Inc. ja ülemaailmsete panustajate poolt.</em></p>