<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Osallistuminen WIA SOOM:iin</h1>
<p align="center"><strong>Olisimme iloisia panoksestasi!</strong></p>
<p align="center">Olipa kyseessä bugikorjaus, uusi ominaisuus, liitännäinen tai käännös — jokainen panos on tärkeä.</p>

---

## Sisällysluettelo

- [Käyttäytymissäännöt](#code-of-conduct)
- [Kuinka raportoida bugeja](#-how-to-report-bugs)
- [Kuinka ehdottaa ominaisuuksia](#-how-to-suggest-features)
- [Kuinka lähettää liitännäinen](#-how-to-submit-a-plugin)
- [Kuinka lähettää pull request](#-how-to-submit-a-pull-request)
- [Käännöspanokset (254 kieltä)](#-translation-contributions-254-languages)
- [Kehitysympäristön asetukset](#-development-setup)

---

## Käyttäytymissäännöt

Olemme sitoutuneet tarjoamaan kaikille tervetulleen ja osallistavan kokemuksen.

- **Ole kunnioittava.** Kohtele kaikkia arvokkaasti.
- **Ole rakentava.** Tarjoa hyödyllistä palautetta, ei tuhoisaa kritiikkiä.
- **Ole osallistava.** Tuemme 254 kieltä ja toivotamme tervetulleiksi osallistujat kaikista maista.
- **Ei häirintää.** Nollatoleranssi kaikenlaista syrjintää kohtaan.

---

## 🐛 Kuinka raportoida bugeja

1. Siirry [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Napsauta **"Uusi ongelma"**
3. Valitse **"Bugin raportti"** -malli
4. Sisällytä:
   - WIA SOOM -versio (Asetukset → Tietoja)
   - Käyttöjärjestelmä ja versio (Windows/macOS/Linux)
   - Toimenpiteet, joilla ongelma toistuu
   - Odotettu vs. todellinen käyttäytyminen
   - Kuvakaappaukset tai terminaaliloki, jos mahdollista

---

## 💡 Kuinka ehdottaa ominaisuuksia

1. Siirry [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Napsauta **"Uusi ongelma"**
3. Valitse **"Ominaisuuden pyyntö"** -malli
4. Kuvaile:
   - Mikä ongelma on ratkaistavana
   - Miten kuvittelet sen toimivan
   - Mahdolliset vaihtoehdot, joita olet harkinnut

---

## 🔌 Kuinka lähettää liitännäinen

WIA SOOM:illa on tehokas liitännäisjärjestelmä — voit rakentaa oman liitännäisesi 5 minuutissa.

### Nopeasti alkuun
§§§CHUNK_SEPARATOR§§§
### Täydellinen opas

Lue **[Liitännäisen kehittäjän opas](docs/PLUGIN_DEVELOPER_GUIDE.md)** saadaksesi:
- Täydellinen API-viite
- Toimivia esimerkkejä
- Askel askeleelta -opastuksia
- Parhaat käytännöt ja turvallisuussäännöt

### Lähetä liitännäisesi

1. Forkkaa [Plugin Store](https://wiasoom.com)
2. Lisää liitännäisesi `plugins/{your-plugin-name}/`
3. Lähetä Pull Request
4. Tarkastuksen jälkeen liitännäisesi näkyy Plugin Storessa kaikille käyttäjille!

---

## 🔀 Kuinka lähettää pull request

### Pääsovellukselle (wia-soom)

1. Forkkaa repositorio
2. Luo ominaisuushaara: `git checkout -b feat/my-feature`
3. Tee muutoksesi
4. Testaa paikallisesti:
   ```bash
   ```
5. Tee commit selkeällä viestillä:
   ```
   feat: lisää tumma tila asetuksiin
   ```
6. Pushaa ja avaa PR `main`:ia vastaan

### Commit-viestien konventio

| Etuliite | Käytetään |
|----------|-----------|
| `feat:`  | Uusi ominaisuus |
| `fix:`   | Bugin korjaus |
| `docs:`  | Vain dokumentaatio |
| `refactor:` | Koodin uudelleenjärjestely (ilman käyttäytymisen muutosta) |
| `i18n:`  | Käännöspäivitykset |
| `plugin:` | Liitännäisiin liittyvät muutokset |

### PR-tarkistuslista

- [ ] Koodi toimii ilman virheitä
- [ ] Ei kovakoodattuja merkkijonoja (käytä i18n-avaimia)
- [ ] Ei `console.log`-komentoja tuotantokoodissa
- [ ] Olemassa olevat testit läpäisevät edelleen

---

## 🌐 Käännöspanokset (254 kieltä)

WIA SOOM tukee **254 kieltä** — amharaasta zuluun, mukaan lukien pistekirjoitus ja RTL-kielet.

### Kuinka käännös toimii

- Peruskielitiedosto: `src/renderer/src/i18n/en.json`
- Kaikki 254 kielitiedostoa ovat samassa hakemistossa
- Käännös tehdään `scripts/translate-patch.js`:n kautta (GPT-4o-mini API)

### Kuinka osallistua käännöksiin

#### Vaihtoehto 1: Korjaa tietty käännös

1. Etsi kielitiedosto: `src/renderer/src/i18n/{lang-code}.json`
2. Korjaa virheellinen käännös
3. Lähetä PR muutoksella

#### Vaihtoehto 2: Lisää puuttuvat avaimet
§§§CHUNK_SEPARATOR§§§
#### Vaihtoehto 3: Tarkista konekäännökset

Monet 254 kielestämme on konekäännetty. Äidinkielen puhujien tarkistukset ovat äärimmäisen arvokkaita!

1. Valitse kielitiedostosi
2. Tarkista käännökset
3. Korjaa kaikki kömpelöt tai virheelliset käännökset
4. Lähetä PR

### Kielikoodit

Käytämme standardin ISO 639-1 koodeja (esim. `ko`, `en`, `ja`, `ar`, `hi`) alueellisia variantteja tarvittaessa (esim. `zh-CN`, `pt-BR`).

---

## 🛠 Kehitysympäristön asetukset

### Esivaatimukset

- Node.js 18+
- npm 9+
- Git

### Asetukset
§§§CHUNK_SEPARATOR§§§
### Kokoaminen
§§§CHUNK_SEPARATOR§§§
> Huom: Oletusarvoinen 2GB:n heap ei riitä 254 kielitiedoston + Monaco-editorin paketin (~38MB renderer) vuoksi.

### Projektin rakenne
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Aitäh

Iga panus teeb WIA SOOM'i paremaks arendajatele üle kogu maailma.

Olgu see siis vigade parandamine, stringi tõlkimine, plugina loomine või suure funktsiooni lisamine — **sa oled osa sellest loost.**

---

<p align="center"><em>Valmistatud ❤️ SmileStory Inc. ja ülemaailmsete panustajate poolt.</em></p>
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
