<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Wkład w WIA SOOM</h1>
<p align="center"><strong>Chętnie przyjmiemy Twoje wkłady!</strong></p>
<p align="center">Niezależnie od tego, czy to poprawka błędu, nowa funkcja, wtyczka czy tłumaczenie — każdy wkład ma znaczenie.</p>

---

## Spis treści

- [Kodeks postępowania](#code-of-conduct)
- [Jak zgłaszać błędy](#-how-to-report-bugs)
- [Jak sugerować funkcje](#-how-to-suggest-features)
- [Jak przesłać wtyczkę](#-how-to-submit-a-plugin)
- [Jak przesłać Pull Request](#-how-to-submit-a-pull-request)
- [Wkłady w tłumaczenia (254 języki)](#-translation-contributions-254-languages)
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
   - Jak wyobrażasz sobie jego działanie
   - Jakie alternatywy rozważałeś

---

## 🔌 Jak przesłać wtyczkę

WIA SOOM ma potężny system wtyczek — możesz stworzyć własną wtyczkę w 5 minut.

### Szybki start
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Pełny przewodnik

Przeczytaj **[Przewodnik dla deweloperów wtyczek](docs/PLUGIN_DEVELOPER_GUIDE.md)**, aby uzyskać:
- Pełną dokumentację API
- Działające przykłady
- Samouczki krok po kroku
- Najlepsze praktyki i zasady bezpieczeństwa

### Prześlij swoją wtyczkę

1. Fork [Plugin Store](https://wiasoom.com)
2. Dodaj swoją wtyczkę do `plugins/{twoja-nazwa-wtyczki}/`
3. Prześlij Pull Request
4. Po przeglądzie, Twoja wtyczka pojawi się w Sklepie Wtyczek dla wszystkich użytkowników!

---

## 🔀 Jak przesłać Pull Request

### Dla głównej aplikacji (wia-soom)

1. Forkuj repozytorium
2. Utwórz gałąź funkcji: `git checkout -b feat/my-feature`
3. Wprowadź zmiany
4. Przetestuj lokalnie:
   ```bash
   ```
5. Zatwierdź z jasnym komunikatem:
   ```
   feat: dodaj przełącznik trybu ciemnego w ustawieniach
   ```
6. Wypchnij i otwórz PR przeciwko `main`

### Konwencja wiadomości commit

| Prefiks | Użyj do |
|---------|---------|
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

## 🌐 Wkłady w tłumaczenia (254 języki)

WIA SOOM wspiera **254 języki** — od amharskiego po zulu, w tym brajl i języki RTL.

### Jak działa tłumaczenie

- Plik językowy bazowy: `src/renderer/src/i18n/en.json`
- Wszystkie 254 pliki językowe znajdują się w tym samym katalogu
- Tłumaczenie odbywa się za pomocą `scripts/translate-patch.js` (API GPT-4o-mini)

### Jak przyczynić się do tłumaczeń

#### Opcja 1: Napraw konkretną tłumaczenie

1. Znajdź plik językowy: `src/renderer/src/i18n/{kod-języka}.json`
2. Napraw niepoprawne tłumaczenie
3. Prześlij PR ze zmianą

#### Opcja 2: Dodaj brakujące klucze
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opcja 3: Przejrzyj tłumaczenia maszynowe

Wiele z naszych 254 języków zostało przetłumaczonych maszynowo. Recenzje rodzimych użytkowników są niezwykle cenne!

1. Wybierz swój plik językowy
2. Przejrzyj tłumaczenia
3. Napraw wszelkie niezręczne lub niepoprawne tłumaczenia
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
```bash
```
### Budowanie
```bash
```
> Uwaga: Domyślna pamięć 2GB nie wystarcza z powodu 254 plików językowych + pakiet edytora Monaco (~38MB renderer).

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

## 🙏 Dziękujemy

Każdy wkład sprawia, że WIA SOOM jest lepszy dla deweloperów na całym świecie.

Niezależnie od tego, czy poprawiasz literówkę, tłumaczysz tekst, tworzysz wtyczkę, czy dodajesz dużą funkcję — **jesteś częścią tej historii.**

---

<p align="center"><em>Zbudowane z ❤️ przez SmileStory Inc. i współpracowników na całym świecie.</em></p>