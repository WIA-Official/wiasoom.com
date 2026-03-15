<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-era ekarpenak</h1>
<p align="center"><strong>Zure ekarpenak gustatuko litzaizkiguke!</strong></p>
<p align="center">Akats bat konpontzea, funtzio berri bat, plugin bat edo itzulpen bat izan, ekarpen guztiek garrantzia dute.</p>

---

## Edukien Taula

- [Jokabide Kodea](#jokabide-kodea)
- [Nola Jakinarazi Akatsak](#-nola-jakinarazi-akatsak)
- [Nola Iradoki Funtzioak](#-nola-iradoki-funtzioak)
- [Nola Aurkeztu Plugin Bat](#-nola-aurkeztu-plugin-bat)
- [Nola Aurkeztu Pull Request Bat](#-nola-aurkeztu-pull-request-bat)
- [Itzulpen Ekarpenak (254 Hizkuntza)](#-itzulpen-ekarpenak-254-hizkuntza)
- [Garapen Konfigurazioa](#-garapen-konfigurazioa)

---

## Jokabide Kodea

Guztientzat ongi etorria eta inklusiboa den esperientzia eskaintzearen konpromisoa dugu.

- **Errespetuz jokatu.** Denak duintasunarekin tratatu.
- **Eraikitzailea izan.** Lagungarriak diren iritziak eman, ez kritikak suntsitzaileak.
- **Inklusiboa izan.** 254 hizkuntza babesten ditugu eta Lurrako herrialde guztietako ekarpenak onartzen ditugu.
- **Ez da jazarpenik onartzen.** Edozein diskriminazio mota ez da onartzen.

---

## 🐛 Nola Jakinarazi Akatsak

1. Joan [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) atalera
2. Egin klik **"New Issue"**
3. Aukeratu **"Bug Report"** txantiloia
4. Gehitu:
   - WIA SOOM bertsioa (Ezarpenak → Informazioa)
   - OS eta bertsioa (Windows/macOS/Linux)
   - Errepikatzeko pausoak
   - Espero zen vs. benetako portaera
   - Pantaila irudiak edo terminaleko irteera, posible bada

---

## 💡 Nola Iradoki Funtzioak

1. Joan [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) atalera
2. Egin klik **"New Issue"**
3. Aukeratu **"Feature Request"** txantiloia
4. Deskribatu:
   - Zein arazo konpontzen ari zaren
   - Nola irudikatzen duzun funtzionatzen
   - Kontuan izan dituzun alternatibak

---

## 🔌 Nola Aurkeztu Plugin Bat

WIA SOOM-ek plugin sistema indartsua du — 5 minututan zure plugin propioa sortu dezakezu.

### Azkar Hasiera
§§§CHUNK_SEPARATOR§§§
### Gida Osoa

Irakurri **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** honetarako:
- API erreferentzia osoa
- Laneko adibideak
- Pausoz pauso tutorialak
- Praktika onenak eta segurtasun arauak

### Aurkeztu Zure Plugin-a

1. Fork egin [Plugin Store](https://wiasoom.com)
2. Gehitu zure plugin-a `plugins/{zure-plugin-izena}/` direktorioan
3. Aurkeztu Pull Request bat
4. Azterketa egin ondoren, zure plugin-a Plugin Store-n agertuko da erabiltzaile guztientzat!

---

## 🔀 Nola Aurkeztu Pull Request Bat

### Aplikazio nagusiarentzat (wia-soom)

1. Fork egin biltegian
2. Sortu funtzio adar bat: `git checkout -b feat/my-feature`
3. Egin zure aldaketak
4. Probatu lokalmente:
   ```bash
   ```
5. Konprometitu mezu argi batekin:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push egin eta PR bat ireki `main`-en aurka

### Konpromiso Mezuaren Ohiturak

| Aurrefijo | Erabiltzeko |
|------------|-------------|
| `feat:`    | Funtzio berri bat |
| `fix:`     | Akats konponketa |
| `docs:`    | Dokumentazioa soilik |
| `refactor:`| Kodearen egituraketa (portzentaia aldaketarik gabe) |
| `i18n:`    | Itzulpen eguneratzeak |
| `plugin:`  | Pluginarekin lotutako aldaketak |

### PR Zerrenda

- [ ] Kodeak akatsik gabe funtzionatzen du
- [ ] Ez dago string gogorrez (erabili i18n gakoak)
- [ ] Ez dago `console.log` produkzio kodean
- [ ] Existitzen diren probak oraindik pasatzen dira

---

## 🌐 Itzulpen Ekarpenak (254 Hizkuntza)

WIA SOOM-ek **254 hizkuntza** babesten ditu — Amharic-tik Zulu-ra, Braille eta RTL hizkuntzak barne.

### Nola Funtzionatzen Duen Itzulpena

- Oinarrizko hizkuntza fitxategia: `src/renderer/src/i18n/en.json`
- 254 hizkuntza fitxategi guztiak direktorio berean daude
- Itzulpena `scripts/translate-patch.js` bidez egiten da (GPT-4o-mini API)

### Nola Ekarri Itzulpenak

#### Aukera 1: Itzulpen zehatz bat konpondu

1. Aurkitu hizkuntza fitxategia: `src/renderer/src/i18n/{hizkuntza-kodea}.json`
2. Konpondu itzulpen okerra
3. Aurkeztu PR bat aldaketarekin

#### Aukera 2: Gako faltak gehitu
§§§CHUNK_SEPARATOR§§§
#### Aukera 3: Makina-itzulpenak berrikusi

Gure 254 hizkuntzetako asko makina bidez itzulita daude. Hiztun natiboen iritziak oso baliotsuak dira!

1. Aukeratu zure hizkuntza fitxategia
2. Berrikusi itzulpenak
3. Konpondu edozein itzulpen arraro edo oker
4. Aurkeztu PR bat

### Hizkuntza Kodeak

ISO 639-1 kode estandarrak erabiltzen ditugu (adibidez, `ko`, `en`, `ja`, `ar`, `hi`) beharrezko aldaerekin (adibidez, `zh-CN`, `pt-BR`).

---

## 🛠 Garapen Konfigurazioa

### Aurreko Baldintzak

- Node.js 18+
- npm 9+
- Git

### Konfigurazioa
§§§CHUNK_SEPARATOR§§§
### Eraikuntza
§§§CHUNK_SEPARATOR§§§
> Oharrak: 2GB-ko heap lehenetsia ez da nahikoa 254 hizkuntza fitxategi + Monaco editore paketea (~38MB renderer) direla eta.

### Proiektuaren Egitura
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Eskerrik Asko

Edozein ekarpenek WIA SOOM hobetzen dute mundu osoko garatzaileentzat.

Akats bat zuzentzen baduzu, kate bat itzultzen baduzu, plugin bat eraikitzen baduzu edo funtzionalitate garrantzitsu bat gehitzen baduzu — **zu zati bat zara istorio honetan.**

---

<p align="center"><em>❤️rekin eraikia SmileStory Inc.-k eta mundu osoko ekarpenek.</em></p>
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
