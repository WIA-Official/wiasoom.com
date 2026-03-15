<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Přispěwanje k WIA SOOM</h1>
<p align="center"><strong>Bychom byli rad, kdybychom dostali vaše přispěwki!</strong></p>
<p align="center">Ať už je to oprava chyby, nová funkce, plugin nebo překlad — každé přispěwki ma znaczenie.</p>

---

## Obsah

- [Kodex chování](#code-of-conduct)
- [Jak hlásit chyby](#-how-to-report-bugs)
- [Jak navrhnout funkce](#-how-to-suggest-features)
- [Jak předložit plugin](#-how-to-submit-a-plugin)
- [Jak předložit pull request](#-how-to-submit-a-pull-request)
- [Přispěwki v překladech (254 jazyků)](#-translation-contributions-254-languages)
- [Nastavení vývoje](#-development-setup)

---

## Kodex chování

Jsme odhodláni poskytovat vítající a inkluzivní zkušenost pro všechny.

- **Buďte uctiví.** Zacházejte se všemi s důstojností.
- **Buďte konstruktivní.** Nabídněte užitečnou zpětnou vazbu, ne destruktivní kritiku.
- **Buďte inkluzivní.** Podporujeme 254 jazyk�� a vítáme přispěwatele z každé země na Zemi.
- **Žádné obtěžování.** Nulová tolerance pro jakoukoli diskriminaci.

---

## 🐛 Jak hlásit chyby

1. Přejděte na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikněte na **"Nový problém"**
3. Vyberte šablonu **"Hlášení chyby"**
4. Zahrňte:
   - Verzi WIA SOOM (Nastavení → O aplikaci)
   - OS a verzi (Windows/macOS/Linux)
   - Krok za krokem, jak chybu reprodukovat
   - Očekávané vs. skutečné chování
   - Snímky obrazovky nebo výstup z terminálu, pokud je to možné

---

## 💡 Jak navrhnout funkce

1. Přejděte na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikněte na **"Nový problém"**
3. Vyberte šablonu **"Žádost o funkci"**
4. Popište:
   - Jaký problém řešíte
   - Jak si představujete, že to bude fungovat
   - Jaké alternativy jste zvažovali

---

## 🔌 Jak předložit plugin

WIA SOOM má silný systém pluginů — můžete si vytvořit vlastní plugin za 5 minut.

### Rychlý start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kompletní průvodce

Přečtěte si **[Příručku pro vývojáře pluginů](docs/PLUGIN_DEVELOPER_GUIDE.md)** pro:
- Kompletní API referenci
- Funkční příklady
- Krok za krokem návody
- Nejlepší praxe a bezpečnostní pravidla

### Předložte svůj plugin

1. Forkněte [Plugin Store](https://wiasoom.com)
2. Přidejte svůj plugin do `plugins/{your-plugin-name}/`
3. Předložte pull request
4. Po kontrole se váš plugin objeví v Plugin Store pro všechny uživatele!

---

## 🔀 Jak předložit pull request

### Pro hlavní aplikaci (wia-soom)

1. Forkněte repozitář
2. Vytvořte větev pro funkci: `git checkout -b feat/my-feature`
3. Proveďte změny
4. Testujte lokálně:
   ```bash
   ```
5. Zapište jasnou zprávu:
   ```
   feat: přidat přepínač tmavého režimu do nastavení
   ```
6. Pushněte a otevřete PR proti `main`

### Konvence zpráv o commitech

| Předpona | Použít pro |
|----------|------------|
| `feat:`  | Nová funkce |
| `fix:`   | Oprava chyby |
| `docs:`  | Pouze dokumentace |
| `refactor:` | Přestrukturování kódu (bez změny chování) |
| `i18n:`  | Aktualizace překladu |
| `plugin:` | Změny související s pluginem |

### Kontrolní seznam PR

- [ ] Kód běží bez chyb
- [ ] Žádné hardcodované řetězce (použijte i18n klíče)
- [ ] Žádné `console.log` zanechané v produkčním kódu
- [ ] Existující testy stále procházejí

---

## 🌐 Přispěwki v překladech (254 jazyků)

WIA SOOM podporuje **254 jazyků** — od amharštiny po zulštinu, včetně Braille a jazyků s RTL.

### Jak fungují překlady

- Základní jazykový soubor: `src/renderer/src/i18n/en.json`
- Všechny 254 jazykové soubory jsou ve stejné složce
- Překlad se provádí pomocí `scripts/translate-patch.js` (GPT-4o-mini API)

### Jak přispět překlady

#### Možnost 1: Opravit konkrétní překlad

1. Najděte jazykový soubor: `src/renderer/src/i18n/{lang-code}.json`
2. Opravit nesprávný překlad
3. Předložte PR se změnou

#### Možnost 2: Přidat chybějící klíče
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Možnost 3: Zkontrolovat strojové překlady

Mnoho z našich 254 jazyků bylo strojově přeloženo. Recenze rodilých mluvčích jsou nesmírně cenné!

1. Vyberte svůj jazykový soubor
2. Zkontrolujte překlady
3. Opravit jakékoli neobratné nebo nesprávné překlady
4. Předložte PR

### Jazykové kódy

Používáme standardní kódy ISO 639-1 (např. `ko`, `en`, `ja`, `ar`, `hi`) s regionálními variantami, kde je to potřeba (např. `zh-CN`, `pt-BR`).

---

## 🛠 Nastavení vývoje

### Požadavky

- Node.js 18+
- npm 9+
- Git

### Nastavení
```bash
```
### Sestavení
```bash
```
> Poznámka: Výchozí 2GB zásobník nestačí kvůli 254 jazykovým souborům + balíčku editoru Monaco (~38MB renderer).

### Struktura projektu
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

## 🙏 Dźakuju

Každa přispěwa sprawi WIA SOOM lepszym za programěry wšěm svěće.

Či juž poprawiš typ, přeložiš tekst, postawiš plugin, abo přidaš wjeliku funkciju — **ty jěš część tej historije.**

---

<p align="center"><em>Postawjeno z ❤️ od SmileStory Inc. a přispěwowców wšěm svěće.</em></p>