<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Wnoszenie wkładu do WIA SOOM</h1>
<p align="center"><strong>Chcielibyśmy Twojego wkładu!</strong></p>
<p align="center">Niezależnie od tego, czy to poprawka błędu, nowa funkcja, wtyczka czy tłumaczenie — każdy wkład ma znaczenie.</p>

---

## Spis treści

- [Kodeks postępowania](#code-of-conduct)
- [Jak zgłaszać błędy](#-how-to-report-bugs)
- [Jak sugerować funkcje](#-how-to-suggest-features)
- [Jak przesłać wtyczkę](#-how-to-submit-a-plugin)
- [Jak przesłać Pull Request](#-how-to-submit-a-pull-request)
- [Wkład w tłumaczenia (254 języki)](#-translation-contributions-254-languages)
- [Ustawienia deweloperskie](#-development-setup)

---

## Kodeks postępowania

Zobowiązujemy się do zapewnienia przyjaznego i inkluzywnego doświadczenia dla wszystkich.

- **Bądź szanowany.** Traktuj wszystkich z godnością.
- **Bądź konstruktywny.** Oferuj pomocne opinie, a nie destrukcyjną krytykę.
- **Bądź inkluzywny.** Wspieramy 254 języki i witamy współpracowników z każdego kraju na Ziemi.
- **Brak nękania.** Zero tolerancji dla jakiejkolwiek dyskryminacji.

---

## 🐛 Jak zgłaszać błędy

1. Przejdź do [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknij **"Nowy problem"**
3. Wybierz szablon **"Zgłoszenie błędu"**
4. Dołącz:
   - Wersję WIA SOOM (Ustawienia → O programie)
   - System operacyjny i wersję (Windows/macOS/Linux)
   - Kroki do odtworzenia
   - Oczekiwane vs. rzeczywiste zachowanie
   - Zrzuty ekranu lub wyjście z terminala, jeśli to możliwe

---

## 💡 Jak sugerować funkcje

1. Przejdź do [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknij **"Nowy problem"**
3. Wybierz szablon **"Prośba o funkcję"**
4. Opisz:
   - Jaki problem rozwiązujesz
   - Jak wyobrażasz sobie, że to działa
   - Jakie alternatywy rozważałeś

---

## 🔌 Jak przesłać wtyczkę

WIA SOOM ma potężny system wtyczek — możesz zbudować swoją własną wtyczkę w 5 minut.

### Szybki start
§§§CHUNK_SEPARATOR§§§
### Pełny przewodnik

Przeczytaj **[Przewodnik dla deweloperów wtyczek](docs/PLUGIN_DEVELOPER_GUIDE.md)**, aby:
- Uzyskać pełną dokumentację API
- Zobaczyć działające przykłady
- Przejść przez samouczki krok po kroku
- Poznać najlepsze praktyki i zasady bezpieczeństwa

### Prześlij swoją wtyczkę

1. Fork [Plugin Store](https://wiasoom.com)
2. Dodaj swoją wtyczkę do `plugins/{twoja-nazwa-wtyczki}/`
3. Prześlij Pull Request
4. Po przeglądzie, Twoja wtyczka pojawi się w Sklepie Wtyczek dla wszystkich użytkowników!

---

## 🔀 Jak przesłać Pull Request

### Dla głównej aplikacji (wia-soom)

1. Fork repozytorium
2. Utwórz gałąź funkcji: `git checkout -b feat/my-feature`
3. Wprowadź zmiany
4. Testuj lokalnie:
   ```bash
   ```
5. Zatwierdź z jasnym komunikatem:
   ```
   feat: dodaj przełącznik trybu ciemnego do ustawień
   ```
6. Wypchnij i otwórz PR przeciwko `main`

### Konwencja komunikatów zatwierdzeń

| Prefiks | Użyj dla |
|--------|---------|
| `feat:` | Nowa funkcja |
| `fix:` | Poprawka błędu |
| `docs:` | Tylko dokumentacja |
| `refactor:` | Restrukturyzacja kodu (bez zmiany zachowania) |
| `i18n:` | Aktualizacje tłumaczeń |
| `plugin:` | Zmiany związane z wtyczkami |

### Lista kontrolna PR

- [ ] Kod działa bez błędów
- [ ] Brak zakodowanych na sztywno ciągów (użyj kluczy i18n)
- [ ] Brak `console.log` w kodzie produkcyjnym
- [ ] Istniejące testy nadal przechodzą

---

## 🌐 Wkład w tłumaczenia (254 języki)

WIA SOOM wspiera **254 języki** — od amharskiego do zulu, w tym Braille'a i języków RTL.

### Jak działa tłumaczenie

- Plik językowy bazowy: `src/renderer/src/i18n/en.json`
- Wszystkie 254 pliki językowe znajdują się w tym samym katalogu
- Tłumaczenie odbywa się za pomocą `scripts/translate-patch.js` (API GPT-4o-mini)

### Jak wnieść wkład w tłumaczenia

#### Opcja 1: Napraw konkretne tłumaczenie

1. Znajdź plik językowy: `src/renderer/src/i18n/{kod-języka}.json`
2. Napraw błędne tłumaczenie
3. Prześlij PR z tą zmianą

#### Opcja 2: Dodaj brakujące klucze
§§§CHUNK_SEPARATOR§§§
#### Opcja 3: Przejrzyj tłumaczenia maszynowe

Wiele z naszych 254 języków zostało przetłumaczonych maszynowo. Recenzje przez native speakerów są niezwykle cenne!

1. Wybierz swój plik językowy
2. Przejrzyj tłumaczenia
3. Napraw wszelkie niezręczne lub błędne tłumaczenia
4. Prześlij PR

### Kody językowe

Używamy standardowych kodów ISO 639-1 (np. `ko`, `en`, `ja`, `ar`, `hi`) z regionalnymi wariantami tam, gdzie to potrzebne (np. `zh-CN`, `pt-BR`).

---

## 🛠 Ustawienia deweloperskie

### Wymagania wstępne

- Node.js 18+
- npm 9+
- Git

### Ustawienia
§§§CHUNK_SEPARATOR§§§
### Budowanie
§§§CHUNK_SEPARATOR§§§
> Uwaga: Domyślna pamięć 2GB nie wystarcza z powodu 254 plików językowych + pakiet edytora Monaco (~38MB renderer).

### Struktura projektu
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Dziękuję

Każdy wkład sprawia, że WIA SOOM jest lepszy dla deweloperów na całym świecie.

Niezależnie od tego, czy poprawisz literówkę, przetłumaczysz ciąg, zbudujesz plugin, czy dodasz dużą funkcję — **jesteś częścią tej historii.**

---

<p align="center"><em>Zbudowane z ❤️ przez SmileStory Inc. i współpracowników z całego świata.</em></p>
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
