<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Dálkkádat WIA SOOM</h1>
<p align="center"><strong>Meahccat dohkkehuvvojuvvo!</strong></p>
<p align="center">Mii lea bug fiks, njuolga, plugin, dahje translate — gávdnet dohkkehuvvojuvvo lea muhtun.</p>

---

## Sisdoalu

- [Dálkkáda Gávdno](#dálkkáda-gávdno)
- [Mii geahččat bug](#-mii-geahččat-bug)
- [Mii geahččat njuolga](#-mii-geahččat-njuolga)
- [Mii geahččat plugin](#-mii-geahččat-plugin)
- [Mii geahččat Pull Request](#-mii-geahččat-pull-request)
- [Translate Dálkkádat (254 Máhttu)](#-translate-dálkkádat-254-máhttu)
- [Dálkkáda Setup](#-dálkkáda-setup)

---

## Dálkkáda Gávdno

Mii lea oahppat gávdno ja inklusiivalaš oahpahus gávdnon.

- **Báhccat gávdno.** Dálkkádat gávdnon mii gávdno.
- **Báhccat konstruktivalaš.** Dálkkádat gávdno ja oahppat, ii oahppat destruktivalaš.
- **Báhccat inklusiivalaš.** Mii geavahuvvo 254 máhttu ja gávdnojuvvo gávdnon mii geavahuvvo.
- **Ihkkáldat.** Nolla toleranssi diskrimináhtta.

---

## 🐛 Mii geahččat bug

1. Gii [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Čáje **"New Issue"**
3. Valjja **"Bug Report"** maldá
4. Gávdnet:
   - WIA SOOM versjon (Settings → About)
   - OS ja versjon (Windows/macOS/Linux)
   - Steppat geahččat
   - Oahpahus vs. álbmot oahpahus
   - Skjemaid dahje terminál oahpahus jos mii geahččat

---

## 💡 Mii geahččat njuolga

1. Gii [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Čáje **"New Issue"**
3. Valjja **"Feature Request"** maldá
4. Dálkkádat:
   - Mii problema geahččat
   - Mii geavahuvvo
   - Njuolga mii gávdnojuvvo

---

## 🔌 Mii geahččat plugin

WIA SOOM lea máhttu plugin sistema — mii sáhtát gávdnojuvvo dohkkehuvvo 5 minuttas.

### Ráhkadus
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Dálkkáda Gávdno

Láhkka **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** gávdnet:
- Dálkkáda API referenssi
- Dálkkáda eksempla
- Steppat gávdnojuvvo
- Bästa praktikat ja turvva reglá

### Geahččat dohkkehuvvo

1. Fork [Plugin Store](https://wiasoom.com)
2. Gávdnet dohkkehuvvo `plugins/{your-plugin-name}/`
3. Geahččat Pull Request
4. Dálkkáda, dohkkehuvvo lea Plugin Store'sa gávdnon!

---

## 🔀 Mii geahččat Pull Request

### Dálkkáda main app (wia-soom)

1. Fork repository
2. Dálkkáda feature branch: `git checkout -b feat/my-feature`
3. Gávdnet dohkkehuvvo
4. Testa lokálala:
   ```bash
   ```
5. Commit gávdnojuvvo:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ja álgga PR `main` álgga

### Commit Gávdnojuvvo

| Prefix | Gávdnet |
|--------|---------|
| `feat:` | Njuolga |
| `fix:` | Bug fiks |
| `docs:` | Dokumentašuvdna |
| `refactor:` | Koda struktura (ii oahpahus gávdno) |
| `i18n:` | Translate gávdnojuvvo |
| `plugin:` | Plugin-aldaga gávdnojuvvo |

### PR Checklist

- [ ] Koda gávdnojuvvo ii oahpahus virhe
- [ ] Ii lea hardcoded strings (geahččat i18n avdaga)
- [ ] Ii lea `console.log` gávdnon produksjon koda
- [ ] Dálkkáda testat gávdnon

---

## 🌐 Translate Dálkkádat (254 Máhttu)

WIA SOOM geavahuvvo **254 máhttu** — Amharic'di Zulu'ii, mii geavahuvvo Braille ja RTL máhttu.

### Mii Translate Gávdnojuvvo

- Bassi máhttu fáile: `src/renderer/src/i18n/en.json`
- Gávdnet 254 máhttu fáile gávdnon sama directory
- Translate gávdnojuvvo lea `scripts/translate-patch.js` (GPT-4o-mini API)

### Mii geahččat Translate

#### Option 1: Fiksa muhtun translate

1. Gii máhttu fáile: `src/renderer/src/i18n/{lang-code}.json`
2. Fiksa muhtun translate
3. Geahččat PR gávdnon

#### Option 2: Gávdnet muhtun avdaga
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Dálkkádat maskin translate

Muhtun gávdnet 254 máhttu lea maskin-translate. Dálkkádat gávdnojuvvo lea muhtun gávdnojuvvo!

1. Valjja máhttu fáile
2. Dálkkádat translate
3. Fiksa muhtun dahje muhtun translate
4. Geahččat PR

### Máhttu Koodit

Mii geavahuvvo standard ISO 639-1 koodit (e.g., `ko`, `en`, `ja`, `ar`, `hi`) mii geavahuvvo regionálalaš variantat jos mii geahččat (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Dálkkáda Setup

### Álgga

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Huom: Default 2GB heap ii lea gávdnojuvvo 254 máhttu fáile + Monaco editor bundle (~38MB renderer).

### Projehtta Struktura
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

## 🙏 Giitu

Gávdnet mii WIA SOOM áddjá buorre dáhpáhuvvon álbmot mátkásvuođain.

Mii geahččat dušše oahpahus, translate-a geavahit, byggjat plugin, dahkat maŋŋá álbmot — **du lea partalaš dán sihkkar.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>