<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Přispívání do WIA SOOM</h1>
<p align="center"><strong>Rádi bychom uvítali vaše příspěvky!</strong></p>
<p align="center">Ať už se jedná o opravu chyby, novou funkci, plugin nebo překlad — každý příspěvek má význam.</p>

---

## Obsah

- [Etický kodex](#code-of-conduct)
- [Jak hlásit chyby](#-how-to-report-bugs)
- [Jak navrhnout funkce](#-how-to-suggest-features)
- [Jak odeslat plugin](#-how-to-submit-a-plugin)
- [Jak odeslat pull request](#-how-to-submit-a-pull-request)
- [Příspěvky v překladech (254 jazyků)](#-translation-contributions-254-languages)
- [Nastavení vývoje](#-development-setup)

---

## Etický kodex

Zavazujeme se poskytovat vítající a inkluzivní zkušenost pro všechny.

- **Buďte respektující.** Zacházejte se všemi s důstojností.
- **Buďte konstruktivní.** Nabízejte užitečnou zpětnou vazbu, ne destruktivní kritiku.
- **Buďte inkluzivní.** Podporujeme 254 jazyků a vítáme přispěvatele ze všech zemí na Zemi.
- **Žádné obtěžování.** Nulová tolerance pro jakoukoli diskriminaci.

---

## 🐛 Jak hlásit chyby

1. Přejděte na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klikněte na **"Nový problém"**
3. Vyberte šablonu **"Hlášení chyby"**
4. Zahrňte:
   - Verzi WIA SOOM (Nastavení → O aplikaci)
   - OS a verzi (Windows/macOS/Linux)
   - Kroky k reprodukci
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

## 🔌 Jak odeslat plugin

WIA SOOM má silný systém pluginů — můžete vytvořit svůj vlastní plugin za 5 minut.

### Rychlý start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kompletní průvodce

Přečtěte si **[Příručku pro vývojáře pluginů](docs/PLUGIN_DEVELOPER_GUIDE.md)** pro:
- Kompletní API referenci
- Funkční příklady
- Krok-za-krokem návody
- Nejlepší praktiky a bezpečnostní pravidla

### Odeslání vašeho pluginu

1. Forkněte [Plugin Store](https://wiasoom.com)
2. Přidejte svůj plugin do `plugins/{název-vášho-pluginu}/`
3. Odeslání pull requestu
4. Po revizi se váš plugin objeví v Plugin Store pro všechny uživatele!

---

## 🔀 Jak odeslat pull request

### Pro hlavní aplikaci (wia-soom)

1. Forkněte repozitář
2. Vytvořte větev pro funkci: `git checkout -b feat/my-feature`
3. Proveďte své změny
4. Otestujte lokálně:
   ```bash
   ```
5. Zapište jasnou zprávu o commitu:
   ```
   feat: přidat přepínač tmavého režimu do nastavení
   ```
6. Pushněte a otevřete PR proti `main`

### Konvence zprávy o commitu

| Předpona | Použít pro |
|----------|------------|
| `feat:`  | Nová funkce |
| `fix:`   | Oprava chyby |
| `docs:`  | Pouze dokumentace |
| `refactor:` | Přestrukturování kódu (bez změny chování) |
| `i18n:`  | Aktualizace překladů |
| `plugin:` | Změny související s pluginy |

### Kontrolní seznam PR

- [ ] Kód běží bez chyb
- [ ] Žádné hardcodované řetězce (použijte i18n klíče)
- [ ] Žádné `console.log` zanechané v produkčním kódu
- [ ] Existující testy stále procházejí

---

## 🌐 Příspěvky v překladech (254 jazyků)

WIA SOOM podporuje **254 jazyků** — od amharského po zulštinu, včetně Brailleova písma a jazyků s RTL.

### Jak funguje překlad

- Základní jazykový soubor: `src/renderer/src/i18n/en.json`
- Všechny 254 jazykové soubory jsou ve stejné složce
- Překlad se provádí pomocí `scripts/translate-patch.js` (GPT-4o-mini API)

### Jak přispět překlady

#### Možnost 1: Opravit konkrétní překlad

1. Najděte jazykový soubor: `src/renderer/src/i18n/{kód-jazyka}.json`
2. Opravit nesprávný překlad
3. Odeslat PR se změnou

#### Možnost 2: Přidat chybějící klíče
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Možnost 3: Zkontrolovat strojové překlady

Mnoho našich 254 jazyků bylo strojově přeloženo. Recenze rodilých mluvčích jsou nesmírně cenné!

1. Vyberte svůj jazykový soubor
2. Zkontrolujte překlady
3. Opravit jakékoli neobratné nebo nesprávné překlady
4. Odeslat PR

### Kódy jazyků

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
> Poznámka: Výchozí 2GB heap není dostatečný kvůli 254 jazykovým souborům + balíčku editoru Monaco (~38MB renderer).

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

## 🙏 Děkujeme

Každý příspěvek činí WIA SOOM lepším pro vývojáře po celém světě.

Ať už opravíte překlep, přeložíte řetězec, vytvoříte plugin nebo přidáte hlavní funkci — **jste součástí tohoto příběhu.**

---

<p align="center"><em>Vytvořeno s ❤️ od SmileStory Inc. a přispěvatelů po celém světě.</em></p>