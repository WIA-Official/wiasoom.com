<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Osallistuminen WIA SOOM:iin</h1>
<p align="center"><strong>Olisimme iloisia panoksistasi!</strong></p>
<p align="center">Olipa kyseessä bugikorjaus, uusi ominaisuus, liitännäinen tai käännös — jokainen panos on tärkeä.</p>

---

## Sisällysluettelo

- [Käyttäytymissäännöt](#code-of-conduct)
- [Kuinka raportoida bugeja](#-how-to-report-bugs)
- [Kuinka ehdottaa ominaisuuksia](#-how-to-suggest-features)
- [Kuinka lähettää liitännäinen](#-how-to-submit-a-plugin)
- [Kuinka lähettää pull-pyyntö](#-how-to-submit-a-pull-request)
- [Käännöspanokset (254 kieltä)](#-translation-contributions-254-languages)
- [Kehitysympäristön asetukset](#-development-setup)

---

## Käyttäytymissäännöt

Olemme sitoutuneet tarjoamaan kaikille tervetulleen ja osallistavan kokemuksen.

- **Ole kunnioittava.** Kohtele kaikkia arvokkaasti.
- **Ole rakentava.** Tarjoa hyödyllistä palautetta, ei tuhoavaa kritiikkiä.
- **Ole osallistava.** Tuemme 254 kieltä ja toivotamme tervetulleiksi osallistujat kaikista maista.
- **Ei häirintää.** Nollatoleranssi minkäänlaista syrjintää kohtaan.

---

## 🐛 Kuinka raportoida bugeja

1. Siirry [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Napsauta **"Uusi ongelma"**
3. Valitse **"Bugin raportti"** -malli
4. Sisällytä:
   - WIA SOOM -versio (Asetukset → Tietoja)
   - Käyttöjärjestelmä ja versio (Windows/macOS/Linux)
   - Vaiheet toistaa
   - Odotettu vs. todellinen käyttäytyminen
   - Kuvakaappaukset tai terminaalilähtö, jos mahdollista

---

## 💡 Kuinka ehdottaa ominaisuuksia

1. Siirry [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Napsauta **"Uusi ongelma"**
3. Valitse **"Ominaisuuspyyntö"** -malli
4. Kuvaile:
   - Mikä ongelma on ratkaistavana
   - Miten kuvittelet sen toimivan
   - Mahdolliset vaihtoehdot, joita olet harkinnut

---

## 🔌 Kuinka lähettää liitännäinen

WIA SOOM:lla on tehokas liitännäisjärjestelmä — voit rakentaa oman liitännäisesi 5 minuutissa.

### Nopeasti alkuun
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Täydellinen opas

Lue **[Liitännäiskehittäjän opas](docs/PLUGIN_DEVELOPER_GUIDE.md)** varten:
- Täydellinen API-viite
- Toimivia esimerkkejä
- Askel askeleelta -oppaat
- Parhaat käytänn��t ja turvallisuusohjeet

### Lähetä liitännäisesi

1. Forkkaa [Plugin Store](https://wiasoom.com)
2. Lisää liitännäisesi `plugins/{your-plugin-name}/`
3. Lähetä pull-pyyntö
4. Arvion jälkeen liitännäisesi näkyy Plugin Storessa kaikille käyttäjille!

---

## 🔀 Kuinka lähettää pull-pyyntö

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
6. Pushaa ja avaa PR `main`-haaraa vastaan

### Commit-viestien konventio

| Etuliite | Käytetään |
|----------|-----------|
| `feat:`  | Uusi ominaisuus |
| `fix:`   | Bugikorjaus |
| `docs:`  | Vain dokumentaatio |
| `refactor:` | Koodin uudelleenjärjestely (ei käyttäytymisen muutosta) |
| `i18n:`  | Käännöspäivitykset |
| `plugin:` | Liitännäisiin liittyvät muutokset |

### PR-tarkistuslista

- [ ] Koodi toimii ilman virheitä
- [ ] Ei kovakoodattuja merkkijonoja (käytä i18n-avain)
- [ ] Ei `console.log`-komentoja tuotantokoodissa
- [ ] Olemassa olevat testit läpäisevät edelleen

---

## 🌐 Käännöspanokset (254 kieltä)

WIA SOOM tukee **254 kieltä** �� amharista zuluun, mukaan lukien Braille ja RTL-kielet.

### Kuinka käännökset toimivat

- Peruskielitiedosto: `src/renderer/src/i18n/en.json`
- Kaikki 254 kielitiedostoa ovat samassa hakemistossa
- Käännös tehdään `scripts/translate-patch.js` (GPT-4o-mini API)

### Kuinka osallistua käännöksiin

#### Vaihtoehto 1: Korjaa tietty käännös

1. Etsi kielitiedosto: `src/renderer/src/i18n/{lang-code}.json`
2. Korjaa virheellinen käännös
3. Lähetä PR muutoksella

#### Vaihtoehto 2: Lisää puuttuvat avaimet
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Vaihtoehto 3: Tarkista konekäännökset

Monet 254 kielestämme on konekäännetty. Äidinkielen puhujien tarkastukset ovat äärimmäisen arvokkaita!

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
```bash
```
### Kokoaminen
```bash
```
> Huom: Oletusarvoinen 2GB:n heap ei riitä 254 kielitiedoston + Monaco-editorin paketin (~38MB renderer) vuoksi.

### Projektin rakenne
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

## 🙏 Kiitos

Jokainen panos tekee WIA SOOM:ista paremman kehittäjille ympäri maailmaa.

Olitpa sitten korjaamassa kirjoitusvirhettä, kääntämässä merkkijonoa, rakentamassa liitännäistä tai lisäämässä suurta ominaisuutta — **olet osa tätä tarinaa.**

---

<p align="center"><em>Rakennettu ❤️ SmileStory Inc.:n ja maailmanlaajuisten kontribuoijien toimesta.</em></p>