<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Doprinos WIA SOOM-u</h1>
<p align="center"><strong>Veselimo se vašim doprinosima!</strong></p>
<p align="center">Bilo da se radi o ispravci greške, novoj funkciji, pluginu ili prijevodu — svaki doprinos je važan.</p>

---

## Sadržaj

- [Kodeks ponašanja](#code-of-conduct)
- [Kako prijaviti greške](#-how-to-report-bugs)
- [Kako predložiti funkcije](#-how-to-suggest-features)
- [Kako poslati plugin](#-how-to-submit-a-plugin)
- [Kako poslati Pull Request](#-how-to-submit-a-pull-request)
- [Doprinosi u prijevodu (254 jezika)](#-translation-contributions-254-languages)
- [Postavljanje okruženja za razvoj](#-development-setup)

---

## Kodeks ponašanja

Obvezujemo se pružiti dobrodošlo i uključivo iskustvo za sve.

- **Budite poštovani.** Tretirajte sve s dostojanstvom.
- **Budite konstruktivni.** Ponudite korisne povratne informacije, a ne destruktivnu kritiku.
- **Budite uključivi.** Podržavamo 254 jezika i dobrodošli su doprinositelji iz svake zemlje na Zemlji.
- **Bez uznemiravanja.** Nulta tolerancija na bilo kakvu diskriminaciju.

---

## 🐛 Kako prijaviti greške

1. Idite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite **"New Issue"**
3. Odaberite **"Bug Report"** predložak
4. Uključite:
   - Verziju WIA SOOM-a (Postavke → O programu)
   - OS i verziju (Windows/macOS/Linux)
   - Korake za reprodukciju
   - Očekivano vs. stvarno ponašanje
   - Snimke zaslona ili izlaz iz terminala ako je moguće

---

## 💡 Kako predložiti funkcije

1. Idite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite **"New Issue"**
3. Odaberite **"Feature Request"** predložak
4. Opišite:
   - Koji problem rješavate
   - Kako zamišljate da to funkcionira
   - Sve alternative koje ste razmatrali

---

## 🔌 Kako poslati plugin

WIA SOOM ima moćan sustav plugina — možete izraditi vlastiti plugin za 5 minuta.

### Brzi početak
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Potpuni vodič

Pročitajte **[Vodič za razvoj plugina](docs/PLUGIN_DEVELOPER_GUIDE.md)** za:
- Potpunu API referencu
- Radne primjere
- Upute korak po korak
- Najbolje prakse i sigurnosna pravila

### Pošaljite svoj plugin

1. Forkajte [Plugin Store](https://wiasoom.com)
2. Dodajte svoj plugin u `plugins/{your-plugin-name}/`
3. Pošaljite Pull Request
4. Nakon pregleda, vaš plugin će se pojaviti u Plugin Store-u za sve korisnike!

---

## 🔀 Kako poslati Pull Request

### Za glavnu aplikaciju (wia-soom)

1. Forkajte repozitorij
2. Kreirajte granu za značajku: `git checkout -b feat/my-feature`
3. Napravite svoje promjene
4. Testirajte lokalno:
   ```bash
   ```
5. Pošaljite s jasnom porukom:
   ```
   feat: dodaj preklopnik za tamni način u postavkama
   ```
6. Pushajte i otvorite PR protiv `main`

### Konvencija za poruke o commit-u

| Prefiks | Koristite za |
|---------|--------------|
| `feat:` | Nova značajka |
| `fix:` | Ispravka greške |
| `docs:` | Samo dokumentacija |
| `refactor:` | Prestrukturiranje koda (bez promjene ponašanja) |
| `i18n:` | Ažuriranja prijevoda |
| `plugin:` | Promjene vezane uz plugin |

### PR Checklista

- [ ] Kod radi bez grešaka
- [ ] Nema hardkodiranih stringova (koristite i18n ključeve)
- [ ] Nema `console.log` u produkcijskom kodu
- [ ] Postojeći testovi i dalje prolaze

---

## 🌐 Doprinosi u prijevodu (254 jezika)

WIA SOOM podržava **254 jezika** — od amharskog do zulua, uključujući Braille i jezike s RTL-om.

### Kako funkcionira prijevod

- Osnovna jezična datoteka: `src/renderer/src/i18n/en.json`
- Sve 254 jezične datoteke su u istoj direktoriji
- Prijevod se vrši putem `scripts/translate-patch.js` (GPT-4o-mini API)

### Kako doprinijeti prijevodima

#### Opcija 1: Ispravite specifičan prijevod

1. Pronađite jezičnu datoteku: `src/renderer/src/i18n/{lang-code}.json`
2. Ispravite netočan prijevod
3. Pošaljite PR s promjenom

#### Opcija 2: Dodajte nedostajuće ključeve
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opcija 3: Pregledajte strojne prijevode

Mnogi od naših 254 jezika su strojno prevedeni. Pregledi izvornih govornika su nevjerojatno vrijedni!

1. Odaberite svoju jezičnu datoteku
2. Pregledajte prijevode
3. Ispravite sve nezgrapne ili netočne prijevode
4. Pošaljite PR

### Jezični kodovi

Koristimo standardne ISO 639-1 kodove (npr., `ko`, `en`, `ja`, `ar`, `hi`) s regionalnim varijantama gdje je to potrebno (npr., `zh-CN`, `pt-BR`).

---

## 🛠 Postavljanje okruženja za razvoj

### Preduvjeti

- Node.js 18+
- npm 9+
- Git

### Postavljanje
```bash
```
### Izgradnja
```bash
```
> Napomena: Zadani 2GB heap nije dovoljan zbog 254 jezičnih datoteka + Monaco editor bundle (~38MB renderer).

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

## 🙏 Hvala vam

Svaki doprinos čini WIA SOOM boljim za programere diljem svijeta.

Bilo da ispravite tipfeler, prevedete string, izradite plugin ili dodate veliku značajku — **dio ste ove priče.**

---

<p align="center"><em>Izrađeno s ❤️ od strane SmileStory Inc. i suradnika širom svijeta.</em></p>