<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Doprinos WIA SOOM-u</h1>
<p align="center"><strong>Voleli bismo vaše doprinose!</strong></p>
<p align="center">Bilo da se radi o ispravci greške, novoj funkciji, dodatku ili prevodu — svaki doprinos je važan.</p>

---

## Sadržaj

- [Kodeks ponašanja](#code-of-conduct)
- [Kako prijaviti greške](#-how-to-report-bugs)
- [Kako predložiti funkcije](#-how-to-suggest-features)
- [Kako poslati dodatak](#-how-to-submit-a-plugin)
- [Kako poslati Pull Request](#-how-to-submit-a-pull-request)
- [Doprinosi u prevođenju (254 jezika)](#-translation-contributions-254-languages)
- [Postavljanje okruženja za razvoj](#-development-setup)

---

## Kodeks ponašanja

Posvećeni smo pružanju dobrodošle i inkluzivne iskustva za sve.

- **Budite poštovani.** Ponašajte se prema svima s dostojanstvom.
- **Budite konstruktivni.** Ponudite korisne povratne informacije, a ne destruktivnu kritiku.
- **Budite inkluzivni.** Podržavamo 254 jezika i dobrodošli su doprinosioci iz svake zemlje na Zemlji.
- **Bez uznemiravanja.** Nulta tolerancija na bilo kakvu diskriminaciju.

---

## 🐛 Kako prijaviti greške

1. Idite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite **"New Issue"**
3. Odaberite **"Bug Report"** šablon
4. Uključite:
   - Verziju WIA SOOM-a (Podešavanja → O programu)
   - OS i verziju (Windows/macOS/Linux)
   - Korake za reprodukciju
   - Očekivano vs. stvarno ponašanje
   - Screenshots ili terminalni izlaz ako je moguće

---

## 💡 Kako predložiti funkcije

1. Idite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite **"New Issue"**
3. Odaberite **"Feature Request"** šablon
4. Opišite:
   - Koji problem rešavate
   - Kako zamišljate da to funkcioniše
   - Sve alternative koje ste razmatrali

---

## 🔌 Kako poslati dodatak

WIA SOOM ima moćan sistem dodataka — možete izgraditi svoj dodatak za 5 minuta.

### Brzi početak
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Potpuni vodič

Pročitajte **[Vodič za programere dodataka](docs/PLUGIN_DEVELOPER_GUIDE.md)** za:
- Potpunu API referencu
- Radne primere
- Tutorijale korak po korak
- Najbolje prakse i pravila sigurnosti

### Pošaljite svoj dodatak

1. Forkujte [Plugin Store](https://wiasoom.com)
2. Dodajte svoj dodatak u `plugins/{your-plugin-name}/`
3. Pošaljite Pull Request
4. Nakon pregleda, vaš dodatak će se pojaviti u Plugin Store-u za sve korisnike!

---

## 🔀 Kako poslati Pull Request

### Za glavnu aplikaciju (wia-soom)

1. Forkujte repozitorij
2. Kreirajte granu za funkciju: `git checkout -b feat/my-feature`
3. Napravite svoje izmene
4. Testirajte lokalno:
   ```bash
   ```
5. Commitujte sa jasnom porukom:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push-ujte i otvorite PR protiv `main`

### Konvencija za poruke o commit-u

| Prefiks | Koristi se za |
|---------|---------------|
| `feat:` | Nova funkcija |
| `fix:`  | Ispravka greške |
| `docs:` | Samo dokumentacija |
| `refactor:` | Prestrukturiranje koda (bez promene ponašanja) |
| `i18n:` | Ažuriranja prevoda |
| `plugin:` | Izmene vezane za dodatke |

### PR Checklista

- [ ] Kod se izvršava bez grešaka
- [ ] Nema hardkodiranih stringova (koristite i18n ključeve)
- [ ] Nema `console.log` u produkcijskom kodu
- [ ] Postojeći testovi i dalje prolaze

---

## 🌐 Doprinosi u prevođenju (254 jezika)

WIA SOOM podržava **254 jezika** — od amharikog do zulua, uključujući Brajevo pismo i jezike sa RTL.

### Kako funkcioniše prevođenje

- Osnovna jezička datoteka: `src/renderer/src/i18n/en.json`
- Sve 254 jezičke datoteke su u istoj direktoriji
- Prevođenje se vrši putem `scripts/translate-patch.js` (GPT-4o-mini API)

### Kako doprineti prevođenju

#### Opcija 1: Ispravite specifičan prevod

1. Pronađite jezičku datoteku: `src/renderer/src/i18n/{lang-code}.json`
2. Ispravite netačan prevod
3. Pošaljite PR sa izmenom

#### Opcija 2: Dodajte nedostajuće ključeve
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opcija 3: Pregledajte mašinske prevode

Mnogi od naših 254 jezika su mašinski prevedeni. Pregledi izvornih govornika su izuzetno vredni!

1. Odaberite svoju jezičku datoteku
2. Pregledajte prevode
3. Ispravite sve nezgrapne ili netačne prevode
4. Pošaljite PR

### Jezički kodovi

Koristimo standardne ISO 639-1 kodove (npr., `ko`, `en`, `ja`, `ar`, `hi`) sa regionalnim varijantama gde je to potrebno (npr., `zh-CN`, `pt-BR`).

---

## 🛠 Postavljanje okruženja za razvoj

### Preduslovi

- Node.js 18+
- npm 9+
- Git

### Postavljanje
```bash
```
### Gradnja
```bash
```
> Napomena: Podrazumevani 2GB heap nije dovoljan zbog 254 jezičkih datoteka + Monaco editor bundle (~38MB renderer).

### Struktura projekta
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

## 🙏 Hvala Vam

Svaki doprinos čini WIA SOOM boljim za programere širom svijeta.

Bilo da ispravite grešku, prevedete string, izradite plugin ili dodate značajnu funkciju — **vi ste dio ove priče.**

---

<p align="center"><em>Izgrađeno s ❤️ od strane SmileStory Inc. i doprinosa širom svijeta.</em></p>