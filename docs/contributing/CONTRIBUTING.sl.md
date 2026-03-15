<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Prispevek k WIA SOOM</h1>
<p align="center"><strong>Veseli nas vaši prispevki!</strong></p>
<p align="center">Ne glede na to, ali gre za popravilo napake, novo funkcijo, vtičnik ali prevod — vsak prispevek šteje.</p>

---

## Kazalo vsebine

- [Kodeks ravnanja](#code-of-conduct)
- [Kako poročati o napakah](#-how-to-report-bugs)
- [Kako predlagati funkcije](#-how-to-suggest-features)
- [Kako predložiti vtičnik](#-how-to-submit-a-plugin)
- [Kako predložiti pull request](#-how-to-submit-a-pull-request)
- [Prispevki za prevod (254 jezikov)](#-translation-contributions-254-languages)
- [Nastavitev za razvoj](#-development-setup)

---

## Kodeks ravnanja

Zavezani smo k zagotavljanju prijazne in vključujoče izkušnje za vse.

- **Bodite spoštljivi.** Z vsemi ravnajte z dostojanstvom.
- **Bodite konstruktivni.** Ponudite koristne povratne informacije, ne uničujoče kritike.
- **Bodite vključujoči.** Podpiramo 254 jezikov in pozdravljamo prispevke iz vseh držav na Zemlji.
- **Brez nadlegovanja.** Ni tolerance za kakršno koli diskriminacijo.

---

## 🐛 Kako poročati o napakah

1. Pojdite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite **"New Issue"**
3. Izberite predlogo **"Bug Report"**
4. Vključite:
   - različico WIA SOOM (Nastavitve → O aplikaciji)
   - operacijski sistem in različico (Windows/macOS/Linux)
   - korake za reprodukcijo
   - pričakovano in dejansko vedenje
   - posnetke zaslona ali izhod terminala, če je mogoče

---

## 💡 Kako predlagati funkcije

1. Pojdite na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kliknite **"New Issue"**
3. Izberite predlogo **"Feature Request"**
4. Opisujte:
   - Kakšen problem rešujete
   - Kako si predstavljate delovanje
   - Kakšne alternative ste upoštevali

---

## 🔌 Kako predložiti vtičnik

WIA SOOM ima močan sistem vtičnikov — svoj vtičnik lahko zgradite v 5 minutah.

### Hiter začetek
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Celoten vodnik

Preberite **[Vodnik za razvijalce vtičnikov](docs/PLUGIN_DEVELOPER_GUIDE.md)** za:
- Popolno referenco API
- Delujoče primere
- Korak za korakom vadnice
- Najboljše prakse in varnostna pravila

### Predložite svoj vtičnik

1. Forknite [Plugin Store](https://wiasoom.com)
2. Dodajte svoj vtičnik v `plugins/{your-plugin-name}/`
3. Predložite pull request
4. Po pregledu se vaš vtičnik prikaže v Trgovini z vtičniki za vse uporabnike!

---

## 🔀 Kako predložiti pull request

### Za glavno aplikacijo (wia-soom)

1. Forknite repozitorij
2. Ustvarite funkcijsko vejo: `git checkout -b feat/my-feature`
3. Naredite svoje spremembe
4. Testirajte lokalno:
   ```bash
   ```
5. Potrdite s jasnim sporočilom:
   ```
   feat: dodaj preklop za temni način v nastavitvah
   ```
6. Potisnite in odprite PR proti `main`

### Konvencija sporočila o potrditvi

| Predpona | Uporabi za |
|----------|------------|
| `feat:`  | Nova funkcija |
| `fix:`   | Popravilo napake |
| `docs:`  | Samo dokumentacija |
| `refactor:` | Prestrukturiranje kode (brez spremembe obnašanja) |
| `i18n:`  | Posodobitve prevodov |
| `plugin:` | Spremembe, povezane z vtičniki |

### Seznam nalog za PR

- [ ] Koda deluje brez napak
- [ ] Brez trdno kodiranih nizov (uporabite i18n ključe)
- [ ] Brez `console.log` v produkcijski kodi
- [ ] Obstoječi testi še vedno delujejo

---

## 🌐 Prispevki za prevod (254 jezikov)

WIA SOOM podpira **254 jezikov** — od amharščine do zulujščine, vključno z Braillom in jeziki z RTL.

### Kako deluje prevod

- Osnovna jezikovna datoteka: `src/renderer/src/i18n/en.json`
- Vse 254 jezikovne datoteke so v isti mapi
- Prevod se izvaja preko `scripts/translate-patch.js` (GPT-4o-mini API)

### Kako prispevati prevode

#### Možnost 1: Popravite specifičen prevod

1. Poiščite jezikovno datoteko: `src/renderer/src/i18n/{lang-code}.json`
2. Popravite napačen prevod
3. Predložite PR s spremembo

#### Možnost 2: Dodajte manjkajoče ključe
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Možnost 3: Preglejte strojne prevode

Mnogi od naših 254 jezikov so bili strojno prevedeni. Pregledi maternih govorcev so izjemno dragoceni!

1. Izberite svojo jezikovno datoteko
2. Preglejte prevode
3. Popravite morebitne nerodne ali napačne prevode
4. Predložite PR

### Kode jezika

Uporabljamo standardne kode ISO 639-1 (npr. `ko`, `en`, `ja`, `ar`, `hi`) z regionalnimi variacijami, kjer je to potrebno (npr. `zh-CN`, `pt-BR`).

---

## 🛠 Nastavitev za razvoj

### Predpogoji

- Node.js 18+
- npm 9+
- Git

### Nastavitev
```bash
```
### Gradnja
```bash
```
> Opomba: Privzeta 2GB kup ni dovolj zaradi 254 jezikovnih datotek + paket Monaco (~38MB renderer).

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

## 🙏 Hvala

Vsak prispevek naredi WIA SOOM boljši za razvijalce po vsem svetu.

Ne glede na to, ali popravite tipkarsko napako, prevedete niz, zgradite vtičnik ali dodate glavno funkcijo — **del te zgodbe ste vi.**

---

<p align="center"><em>Zgrajeno z ❤️ s strani SmileStory Inc. in prispevkov iz celega sveta.</em></p>