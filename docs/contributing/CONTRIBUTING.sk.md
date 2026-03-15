<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Prispievanie k WIA SOOM</h1>
<p align="center"><strong>Radi by sme prijali vaše príspevky!</strong></p>
<p align="center">Či už ide o opravu chyby, novú funkciu, plugin alebo preklad — každý príspevok má význam.</p>

---

## Obsah

- [Etický kódex](#code-of-conduct)
- [Ako hlásiť chyby](#-how-to-report-bugs)
- [Ako navrhnúť funkcie](#-how-to-suggest-features)
- [Ako odoslať plugin](#-how-to-submit-a-plugin)
- [Ako odoslať pull request](#-how-to-submit-a-pull-request)
- [Príspevky v prekladoch (254 jazykov)](#-translation-contributions-254-languages)
- [Nastavenie vývoja](#-development-setup)

---

## Etický kódex

Sme odhodlaní poskytovať vítajúcu a inkluzívnu skúsenosť pre všetkých.

- **Buďte rešpektujúci.** Zaobchádzajte so všetkými s dôstojnosťou.
- **Buďte konštruktívni.** Ponúkajte užitočnú spätnú väzbu, nie deštruktívnu kritiku.
- **Buďte inkluzívni.** Podporujeme 254 jazykov a vítame prispievateľov z každej krajiny na Zemi.
- **Žiadne obťažovanie.** Žiadna tolerancia voči diskriminácii akéhokoľvek druhu.

---

## 🐛 Ako hlásiť chyby

1. Prejdite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite na **"Nový problém"**
3. Vyberte šablónu **"Hlásenie chyby"**
4. Zahrňte:
   - Verziu WIA SOOM (Nastavenia → O aplikácii)
   - OS a verziu (Windows/macOS/Linux)
   - Kroky na reprodukciu
   - Očakávané vs. skutočné správanie
   - Screenshoty alebo výstup z terminálu, ak je to možné

---

## 💡 Ako navrhnúť funkcie

1. Prejdite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite na **"Nový problém"**
3. Vyberte šablónu **"Žiadosť o funkciu"**
4. Opíšte:
   - Aký problém riešite
   - Ako si predstavujete, že to bude fungovať
   - Akékoľvek alternatívy, ktoré ste zvážili

---

## 🔌 Ako odoslať plugin

WIA SOOM má mocný systém pluginov — môžete si vytvoriť vlastný plugin za 5 minút.

### Rýchly štart
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kompletný sprievodca

Prečítajte si **[Sprievodcu pre vývojárov pluginov](docs/PLUGIN_DEVELOPER_GUIDE.md)** pre:
- Kompletnú referenciu API
- Pracovné príklady
- Podrobné tutoriály
- Najlepšie praktiky a bezpečnostné pravidlá

### Odoslať svoj plugin

1. Forknite [Plugin Store](https://wiasoom.com)
2. Pridajte svoj plugin do `plugins/{your-plugin-name}/`
3. Odoslať pull request
4. Po preskúmaní sa váš plugin objaví v Plugin Store pre všetkých používateľov!

---

## 🔀 Ako odoslať pull request

### Pre hlavnú aplikáciu (wia-soom)

1. Forknite repozitár
2. Vytvorte vetvu funkcie: `git checkout -b feat/my-feature`
3. Urobte svoje zmeny
4. Testujte lokálne:
   ```bash
   ```
5. Commitnite s jasnou správou:
   ```
   feat: pridať prepínač tmavého režimu do nastavení
   ```
6. Pushnite a otvorte PR proti `main`

### Konvencia správ commitov

| Prefix | Použiť pre |
|--------|------------|
| `feat:` | Nová funkcia |
| `fix:` | Oprava chyby |
| `docs:` | Iba dokumentácia |
| `refactor:` | Preusporiadanie kódu (bez zmeny správania) |
| `i18n:` | Aktualizácie prekladu |
| `plugin:` | Zmeny súvisiace s pluginom |

### Kontrolný zoznam PR

- [ ] Kód beží bez chýb
- [ ] Žiadne hardcodované reťazce (použite i18n kľúče)
- [ ] Žiadne `console.log` zostali v produkčnom kóde
- [ ] Existujúce testy stále prechádzajú

---

## 🌐 Príspevky v prekladoch (254 jazykov)

WIA SOOM podporuje **254 jazykov** — od amharčiny po zulu, vrátane Braillovho písma a jazykov s RTL písmom.

### Ako funguje preklad

- Základný jazykový súbor: `src/renderer/src/i18n/en.json`
- Všetky 254 jazykové súbory sú v rovnakom adresári
- Preklad sa vykonáva prostredníctvom `scripts/translate-patch.js` (GPT-4o-mini API)

### Ako prispieť prekladmi

#### Možnosť 1: Opraviť konkrétny preklad

1. Nájdite jazykový súbor: `src/renderer/src/i18n/{lang-code}.json`
2. Opraviť nesprávny preklad
3. Odoslať PR so zmenou

#### Možnosť 2: Pridať chýbajúce kľúče
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Možnosť 3: Preskúmať strojové preklady

Mnohé z našich 254 jazykov boli preložené strojovo. Recenzie od rodených hovorcov sú nesmierne cenné!

1. Vyberte si svoj jazykový súbor
2. Preskúmajte preklady
3. Opraviť akékoľvek neobratné alebo nesprávne preklady
4. Odoslať PR

### Jazykové kódy

Používame štandardné kódy ISO 639-1 (napr. `ko`, `en`, `ja`, `ar`, `hi`) s regionálnymi variantmi, kde je to potrebné (napr. `zh-CN`, `pt-BR`).

---

## 🛠 Nastavenie vývoja

### Predpoklady

- Node.js 18+
- npm 9+
- Git

### Nastavenie
```bash
```
### Build
```bash
```
> Poznámka: Predvolený 2GB heap nie je dostatočný kvôli 254 jazykovým súborom + bal��ku Monaco editor (~38MB renderer).

### Štruktúra projektu
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

## 🙏 Ďakujeme

Každý príspevok robí WIA SOOM lepším pre vývojárov po celom svete.

Či už opravíte preklep, preložíte reťazec, vytvoríte plugin alebo pridáte hlavnú funkciu — **ste súčasťou tohto príbehu.**

---

<p align="center"><em>Postavené s ❤️ od SmileStory Inc. a prispievateľov z celého sveta.</em></p>