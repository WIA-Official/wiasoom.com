<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Hozzájárulás a WIA SOOM-hoz</h1>
<p align="center"><strong>Örömmel fogadjuk a hozzájárulásaidat!</strong></p>
<p align="center">Legyen szó hibajavításról, új funkcióról, plug-inról vagy fordításról — minden hozzájárulás számít.</p>

---

## Tartalomjegyzék

- [Etikai Kódex](#code-of-conduct)
- [Hogyan Jelentsünk Hibákat](#-how-to-report-bugs)
- [Hogyan Javasoljunk Funkciókat](#-how-to-suggest-features)
- [Hogyan Nyújtsunk Be Egy Plug-int](#-how-to-submit-a-plugin)
- [Hogyan Nyújtsunk Be Egy Pull Kérelmet](#-how-to-submit-a-pull-request)
- [Fordítási Hozzájárulások (254 Nyelv)](#-translation-contributions-254-languages)
- [Fejlesztési Beállítás](#-development-setup)

---

## Etikai Kódex

Elkötelezettek vagyunk amellett, hogy mindenki számára barátságos és befogadó élményt nyújtsunk.

- **Légy tiszteletteljes.** Tartsd tiszteletben mindenki méltóságát.
- **Légy építő jellegű.** Adj hasznos visszajelzést, ne romboló kritikát.
- **Légy befogadó.** Támogatjuk a 254 nyelvet, és üdvözöljük a hozzájárulókat a Föld minden országából.
- **Nincs zaklatás.** Nulla tolerancia mindenféle diszkriminációval szemben.

---

## 🐛 Hogyan Jelentsünk Hibákat

1. Lépj a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) oldalra
2. Kattints a **"Új probléma"** gombra
3. Válaszd a **"Hibajelentés"** sablont
4. Tartsd be a következőket:
   - WIA SOOM verzió (Beállítások → Névjegy)
   - Operációs rendszer és verzió (Windows/macOS/Linux)
   - Reprodukálási lépések
   - Várt vs. tényleges viselkedés
   - Képernyőképek vagy terminál kimenet, ha lehetséges

---

## 💡 Hogyan Javasoljunk Funkciókat

1. Lépj a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) oldalra
2. Kattints a **"Új probléma"** gombra
3. Válaszd a **"Funkciókérés"** sablont
4. Írd le:
   - Milyen problémát oldasz meg
   - Hogyan képzeled el a működését
   - Milyen alternatívákat fontolgattál

---

## 🔌 Hogyan Nyújtsunk Be Egy Plug-int

A WIA SOOM egy erőteljes plug-in rendszert kínál — saját plug-inodat 5 perc alatt elkészítheted.

### Gyors Kezdés
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Teljes Útmutató

Olvasd el a **[Plug-in Fejlesztői Útmutatót](docs/PLUGIN_DEVELOPER_GUIDE.md)** a következőkért:
- Teljes API referencia
- Működő példák
- Lépésről lépésre útmutatók
- Legjobb gyakorlatok és biztonsági szabályok

### Nyújtsd Be A Plug-inodat

1. Forkold a [Plugin Store](https://wiasoom.com) repót
2. Add hozzá a plug-inodat a `plugins/{your-plugin-name}/` mappába
3. Nyújts be egy Pull Kérelmet
4. A felülvizsgálat után a plug-inod megjelenik a Plugin Store-ban minden felhasználó számára!

---

## 🔀 Hogyan Nyújtsunk Be Egy Pull Kérelmet

### A fő alkalmazás számára (wia-soom)

1. Forkold a repót
2. Hozz létre egy funkció ágat: `git checkout -b feat/my-feature`
3. Végezze el a módosításokat
4. Teszteld helyben:
   ```bash
   ```
5. Kötelezd el egy világos üzenettel:
   ```
   feat: sötét mód kapcsoló hozzáadása a beállításokhoz
   ```
6. Pushold és nyiss egy PR-t a `main` ellen

### Kötelezvény Üzenet Konvenció

| Előtag | Használat |
|--------|-----------|
| `feat:` | Új funkció |
| `fix:` | Hibajavítás |
| `docs:` | Csak dokumentáció |
| `refactor:` | Kód átszervezése (nincs viselkedésbeli változás) |
| `i18n:` | Fordítási frissítések |
| `plugin:` | Plug-inhez kapcsolódó változások |

### PR Ellenőrzőlista

- [ ] A kód hibák nélkül fut
- [ ] Nincsenek hardkódolt szövegek (használj i18n kulcsokat)
- [ ] Nincs `console.log` a termelési kódban
- [ ] A meglévő tesztek továbbra is átmennek

---

## 🌐 Fordítási Hozzájárulások (254 Nyelv)

A WIA SOOM támogatja a **254 nyelvet** — az amhara nyelvtől a zulura, beleértve a Braille-t és az RTL nyelveket is.

### Hogyan Működik a Fordítás

- Alap nyelvi fájl: `src/renderer/src/i18n/en.json`
- Az összes 254 nyelvi fájl ugyanabban a könyvtárban található
- A fordítás a `scripts/translate-patch.js` (GPT-4o-mini API) segítségével történik

### Hogyan Hozzájárulj Fordításokkal

#### 1. Opció: Javíts egy konkrét fordítást

1. Keresd meg a nyelvi fájlt: `src/renderer/src/i18n/{lang-code}.json`
2. Javítsd ki a helytelen fordítást
3. Nyújts be egy PR-t a változtatással

#### 2. Opció: Adj hozzá hiányzó kulcsokat
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### 3. Opció: Ellenőrizd a gépi fordításokat

Sok a 254 nyelvünk gépi fordítással készült. A helyi beszélők véleménye rendkívül értékes!

1. Válaszd ki a nyelvi fájlodat
2. Ellenőrizd a fordításokat
3. Javítsd ki a furcsa vagy helytelen fordításokat
4. Nyújts be egy PR-t

### Nyelvi Kódok

A standard ISO 639-1 kódokat használjuk (pl. `ko`, `en`, `ja`, `ar`, `hi`) regionális változatokkal, ahol szükséges (pl. `zh-CN`, `pt-BR`).

---

## 🛠 Fejlesztési Beállítás

### Előfeltételek

- Node.js 18+
- npm 9+
- Git

### Beállítás
```bash
```
### Építés
```bash
```
> Megjegyzés: Az alapértelmezett 2GB-os heap nem elegendő a 254 nyelvi fájl + Monaco szerkesztő csomag (~38MB renderer) miatt.

### Projekt Struktúra
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

## 🙏 Köszönjük

Minden hozzájárulás jobbá teszi a WIA SOOM-ot a világ fejlesztői számára.

Akár egy elírást javítasz, egy szöveget fordítasz, egy plugint építesz, vagy egy jelentős funkciót adsz hozzá — **te is része vagy ennek a történetnek.**

---

<p align="center"><em>Szeretettel építette: a SmileStory Inc. és a világ minden tájáról érkező hozzájárulók.</em></p>