<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">O le fesoasoani i le WIA SOOM</h1>
<p align="center"><strong>Matou te fiafia i lau fesoasoani!</strong></p>
<p align="center">E whether o se fa'asa'o bug, fa'avae fou, plugin, po'o se fa'ata'ita'iga — e taua uma le fesoasoani.</p>

---

## Taaloga o le Fa'avae

- [Tulaga o le Fa'avae](#code-of-conduct)
- [Fa'afeiloa'i i le Fa'asa'o Bug](#-how-to-report-bugs)
- [Fa'afeiloa'i i le Fa'avae Fou](#-how-to-suggest-features)
- [Fa'afeiloa'i i le Fa'avae o se Plugin](#-how-to-submit-a-plugin)
- [Fa'afeiloa'i i le Fa'avae o se Pull Request](#-how-to-submit-a-pull-request)
- [Fa'avae o le Fa'ata'ita'iga (254 Languages)](#-translation-contributions-254-languages)
- [Seti o le Atina'e](#-development-setup)

---

## Tulaga o le Fa'avae

O lo'o matou taula'i i le ofoina i se fa'avaa ma se fa'avae e aofia ai mo tagata uma.

- **Ia fa'amaoni.** Fa'avae i tagata uma i le agaga o le fa'amaoni.
- **Ia fa'avae i le fa'avae.** Ofo i fa'amaoniga fesoasoani, e le fa'avae i le fa'amaonia.
- **Ia aofia.** E lagolagoina e matou le 254 gagana ma fa'afeiloa'i i tagata fesoasoani mai le atunuu uma i le lalolagi.
- **E le fa'atekinolosi.** E leai se fa'amaonia i le fa'atekinolosi o so'o se ituaiga.

---

## 🐛 Fa'afeiloa'i i le Fa'asa'o Bug

1. O le alu i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kiliki **"New Issue"**
3. Filifili le **"Bug Report"** template
4. Fa'aopoopo:
   - WIA SOOM version (Settings → About)
   - OS ma le version (Windows/macOS/Linux)
   - La'asaga e toe fa'ata'ita'iga
   - Fa'amoemoega vs. fa'ata'ita'iga i le fa'avae
   - Ata o le lau po'o le fa'ata'ita'iga i le terminal pe a mafai

---

## 💡 Fa'afeiloa'i i le Fa'avae Fou

1. O le alu i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Kiliki **"New Issue"**
3. Filifili le **"Feature Request"** template
4. Fa'amatala:
   - O le a le fa'afitauli e te fo'ia
   - E fa'apena ona e fa'amoemoe e fa'agaioi
   - So'o se isi filifiliga e te manatu i ai

---

## 🔌 Fa'afeiloa'i i le Fa'avae o se Plugin

O le WIA SOOM e iai se faiga plugin malosi — e mafai ona e fa'avae i lau plugin i le 5 minute.

### Fa'avae Vave
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ta'iala Tumau

Fa'aoga le **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mo:
- Fa'amaoniga API atoa
- Fa'ata'ita'iga e galue
- Ta'iala i la'asaga i la'asaga
- Fa'avae lelei ma fa'avae i le saogalemu

### Fa'avae Lau Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Fa'aopoopo lau plugin i le `plugins/{your-plugin-name}/`
3. Fa'avae se Pull Request
4. A'o le fa'ata'ita'iga, o le a fa'aalia lau plugin i le Plugin Store mo tagata uma!

---

## 🔀 Fa'afeiloa'i i le Fa'avae o se Pull Request

### Mo le fa'avae autu (wia-soom)

1. Fork le repository
2. Fa'avae se laupapa fa'avae: `git checkout -b feat/my-feature`
3. Fa'amaonia au suiga
4. Fa'ata'ita'iga i le lotoifale:
   ```bash
   ```
5. Fa'amaonia ma se fa'amatalaga manino:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push ma fa'avae se PR i le `main`

### Fa'amatalaga o le Commit

| Prefix | Fa'aaoga mo |
|--------|---------|
| `feat:` | Fa'avae fou |
| `fix:` | Fa'asa'o bug |
| `docs:` | Fa'amaoniga na'o le fa'amaonia |
| `refactor:` | Fa'avae i le code (e le o se suiga i le fa'avae) |
| `i18n:` | Fa'asa'o fa'ata'ita'iga |
| `plugin:` | Suiga e fa'avae i le plugin |

### Fa'amaumauga o le PR

- [ ] O le code e galue e aunoa ma ni fa'aletonu
- [ ] E leai ni fa'amaoniga i le fa'avae (fa'aaoga i18n keys)
- [ ] E leai se `console.log` i le code fa'avae
- [ ] O su'esu'ega o lo'o i ai e fa'amaonia pea

---

## 🌐 Fa'avae o le Fa'ata'ita'iga (254 Languages)

O le WIA SOOM e lagolagoina **254 gagana** — mai le Amharic i le Zulu, e aofia ai le Braille ma gagana RTL.

### E fa'agaioi le Fa'ata'ita'iga

- Fa'avae i le gagana: `src/renderer/src/i18n/en.json`
- O fa'avae uma 254 o lo'o i le fa'avaa tutusa
- O le fa'ata'ita'iga e faia i le `scripts/translate-patch.js` (GPT-4o-mini API)

### Fa'afeiloa'i i le Fa'ata'ita'iga

#### Filifiliga 1: Fa'asa'o se fa'ata'ita'iga fa'avae

1. Fa'afa'i le fa'avae o le gagana: `src/renderer/src/i18n/{lang-code}.json`
2. Fa'asa'o le fa'ata'ita'iga sese
3. Fa'avae se PR ma le suiga

#### Filifiliga 2: Fa'aopoopo i le fa'avae e le i ai
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Filifiliga 3: Fa'amaonia fa'ata'ita'iga masini

E tele o le 254 gagana o lo'o i le fa'ata'ita'iga masini. O le fa'amaonia a tagata o le gagana e taua tele!

1. Filifili lau fa'avae o le gagana
2. Fa'amaonia le fa'ata'ita'iga
3. Fa'asa'o so'o se fa'ata'ita'iga leaga po'o le sese
4. Fa'avae se PR

### Fa'ailoga o le Gagana

E fa'aoga e matou fa'ailoga masani ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) ma fa'avae i le itulagi e manaʻomia (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Seti o le Atina'e

### Fa'avae i Luma

- Node.js 18+
- npm 9+
- Git

### Seti
```bash
```
### Fa'avae
```bash
```
> Fa'amatalaga: O le 2GB heap masani e le o le aoga ona o le 254 fa'avae o le gagana + Monaco editor bundle (~38MB renderer).

### Fa'avae o le Proiect
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

## 🙏 Fa'afetai

O le fesoasoani uma e fa'aleleia ai le WIA SOOM mo tagata atina'e i le lalolagi atoa.

E fa'avae i luga o le fa'asa'oina o se fa'ata'ita'iga, fa'aluaina o se fa'ata'ita'iga, fa'avae i se plugin, po'o le fa'aopoopoina o se vaega taua — **o le vaega o le tala lenei.**

---

<p align="center"><em>O le fa'avae i le ❤️ e le SmileStory Inc. ma tagata fesoasoani i le lalolagi atoa.</em></p>