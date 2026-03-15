<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuții la WIA SOOM</h1>
<p align="center"><strong>Ne-ar plăcea contribuțiile tale!</strong></p>
<p align="center">Fie că este vorba de o corectare de bug-uri, o nouă funcționalitate, un plugin sau o traducere — fiecare contribuție contează.</p>

---

## Cuprins

- [Cod de conduită](#code-of-conduct)
- [Cum să raportezi bug-uri](#-how-to-report-bugs)
- [Cum să sugerezi funcționalități](#-how-to-suggest-features)
- [Cum să trimiți un plugin](#-how-to-submit-a-plugin)
- [Cum să trimiți o cerere de extragere](#-how-to-submit-a-pull-request)
- [Contribuții de traducere (254 limbi)](#-translation-contributions-254-languages)
- [Configurare pentru dezvoltare](#-development-setup)

---

## Cod de conduită

Ne angajăm să oferim o experiență primitoare și inclusivă pentru toată lumea.

- **Fii respectuos.** Tratează pe toată lumea cu demnitate.
- **Fii constructiv.** Oferă feedback util, nu critici distructive.
- **Fii inclusiv.** Susținem 254 de limbi și primim contribuții din fiecare țară de pe Pământ.
- **Fără hărțuire.** Toleranță zero pentru discriminare de orice fel.

---

## 🐛 Cum să raportezi bug-uri

1. Mergi la [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Fă clic pe **"New Issue"**
3. Alege șablonul **"Bug Report"**
4. Include:
   - Versiunea WIA SOOM (Setări → Despre)
   - OS și versiunea (Windows/macOS/Linux)
   - Pașii pentru a reproduce
   - Comportamentul așteptat vs. comportamentul real
   - Capturi de ecran sau ieșire din terminal, dacă este posibil

---

## 💡 Cum să sugerezi funcționalități

1. Mergi la [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Fă clic pe **"New Issue"**
3. Alege șablonul **"Feature Request"**
4. Descrie:
   - Ce problemă rezolvi
   - Cum îți imaginezi că va funcționa
   - Orice alternative pe care le-ai luat în considerare

---

## 🔌 Cum să trimiți un plugin

WIA SOOM are un sistem puternic de pluginuri — poți construi propriul tău plugin în 5 minute.

### Începere rapidă
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ghid complet

Citește **[Ghidul pentru dezvoltatori de pluginuri](docs/PLUGIN_DEVELOPER_GUIDE.md)** pentru:
- Referință API completă
- Exemple funcționale
- Tutoriale pas cu pas
- Cele mai bune practici și reguli de securitate

### Trimite pluginul tău

1. Fork [Plugin Store](https://wiasoom.com)
2. Adaugă pluginul tău în `plugins/{numele-pluginului-tău}/`
3. Trimite o cerere de extragere
4. După revizuire, pluginul tău va apărea în Plugin Store pentru toți utilizatorii!

---

## 🔀 Cum să trimiți o cerere de extragere

### Pentru aplicația principală (wia-soom)

1. Fork repository-ul
2. Creează o ramură de funcționalitate: `git checkout -b feat/my-feature`
3. Fă modificările tale
4. Testează local:
   ```bash
   ```
5. Comite cu un mesaj clar:
   ```
   feat: adaugă comutator de mod întunecat în setări
   ```
6. Pushează și deschide un PR împotriva `main`

### Convenția mesajului de commit

| Prefix | Utilizat pentru |
|--------|-----------------|
| `feat:` | Funcționalitate nouă |
| `fix:` | Corectare de bug |
| `docs:` | Doar documentație |
| `refactor:` | Restructurare de cod (fără schimbări de comportament) |
| `i18n:` | Actualizări de traducere |
| `plugin:` | Schimbări legate de pluginuri |

### Lista de verificare PR

- [ ] Codul rulează fără erori
- [ ] Fără șiruri hardcodate (folosește chei i18n)
- [ ] Fără `console.log` lăsat în codul de producție
- [ ] Testele existente continuă să treacă

---

## 🌐 Contribuții de traducere (254 limbi)

WIA SOOM suportă **254 de limbi** — de la amharică la zuluză, inclusiv Braille și limbi RTL.

### Cum funcționează traducerea

- Fișierul de bază al limbii: `src/renderer/src/i18n/en.json`
- Toate cele 254 de fișiere de limbă sunt în același director
- Traducerea se face prin `scripts/translate-patch.js` (API GPT-4o-mini)

### Cum să contribui cu traduceri

#### Opțiunea 1: Corectează o traducere specifică

1. Găsește fișierul de limbă: `src/renderer/src/i18n/{cod-limbă}.json`
2. Corectează traducerea incorectă
3. Trimite un PR cu modificarea

#### Opțiunea 2: Adaugă chei lipsă
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opțiunea 3: Revizuiește traducerile automate

Multe dintre cele 254 de limbi au fost traduse automat. Revizuirile de către vorbitori nativi sunt extrem de valoroase!

1. Alege fișierul tău de limbă
2. Revizuiește traducerile
3. Corectează orice traduceri ciudate sau incorecte
4. Trimite un PR

### Coduri de limbă

Folosim coduri standard ISO 639-1 (de exemplu, `ko`, `en`, `ja`, `ar`, `hi`) cu variante regionale, acolo unde este necesar (de exemplu, `zh-CN`, `pt-BR`).

---

## 🛠 Configurare pentru dezvoltare

### Cerințe preliminare

- Node.js 18+
- npm 9+
- Git

### Configurare
```bash
```
### Construire
```bash
```
> Notă: Heap-ul implicit de 2GB nu este suficient din cauza celor 254 de fișiere de limbă + pachetul editorului Monaco (~38MB renderer).

### Structura proiectului
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

## 🙏 Mulțumim

Fiecare contribuție face WIA SOOM mai bun pentru dezvoltatorii din întreaga lume.

Indiferent dacă corectezi o greșeală de tipar, traduci un șir, construiești un plugin sau adaugi o caracteristică majoră — **ești parte din această poveste.**

---

<p align="center"><em>Construit cu ❤️ de SmileStory Inc. și contribuabili din întreaga lume.</em></p>