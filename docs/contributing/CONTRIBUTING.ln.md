<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kokanga na WIA SOOM</h1>
<p align="center"><strong>Tolingi yo na kokanga!</strong></p>
<p align="center">Soki ezali likambo ya kosala, fonction ya sika, plugin, to tradiksyon — nyonso ezali na ntina.</p>

---

## Titre ya Mibeko

- [Mibeko ya Komportement](#code-of-conduct)
- [Ndenge ya Koyebisa Makambo ya Mabe](#-how-to-report-bugs)
- [Ndenge ya Koyebisa Fonction](#-how-to-suggest-features)
- [Ndenge ya Koyebisa Plugin](#-how-to-submit-a-plugin)
- [Ndenge ya Koyebisa Pull Request](#-how-to-submit-a-pull-request)
- [Kokanga na Tradiksyon (254 Langues)](#-translation-contributions-254-languages)
- [Mokonzi ya Development](#-development-setup)

---

## Mibeko ya Komportement

Tozali na esengo ya kopesa expérience ya malamu mpe ya kokanga mpo na nyonso.

- **Zala na esengo.** Tika nyonso na bokonzi.
- **Zala na ntango.** Pesa nzela ya kosala, te kobanga.
- **Zala na kokanga.** Tozali na lisungi ya 254 langues mpe tozali na esengo ya kokanga bato nyonso ya mokili.
- **Te na kolanda.** Tika na bobangi ya ndenge nyonso.

---

## 🐛 Ndenge ya Koyebisa Makambo ya Mabe

1. Yaka na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Koma **"Nouveau Problème"**
3. Séléctionne **"Rapport de Bug"** template
4. Kanga:
   - Version ya WIA SOOM (Paramètres → À propos)
   - OS mpe version (Windows/macOS/Linux)
   - Bato ya kosala
   - Komportement oyo esengeli vs. oyo ezalaka
   - Bakateli to output ya terminal soki ezali na possibilité

---

## 💡 Ndenge ya Koyebisa Fonction

1. Yaka na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Koma **"Nouveau Problème"**
3. Séléctionne **"Demande de Fonction"** template
4. Koma:
   - Likambo oyo ozali kokanga
   - Ndenge ozali komona yango esalema
   - Bato nyonso oyo olingi kosala

---

## 🔌 Ndenge ya Koyebisa Plugin

WIA SOOM ezali na système ya plugin ya makasi — okoki kosala plugin na yo na miniti 5.

### Koyebisa ya Mbala
§§§CHUNK_SEPARATOR§§§
### Guide ya Pene

Luka **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** mpo na:
- Referans API ya bokomoko
- Bato ya kosala
- Tutoriels ya nzela-nzela
- Malamu ya kosala mpe mibeko ya sécurité

### Koyebisa Plugin na Yo

1. Fork [Plugin Store](https://wiasoom.com)
2. Kanga plugin na yo na `plugins/{your-plugin-name}/`
3. Koyebisa Pull Request
4. Na nsima ya kokanga, plugin na yo ekozala na Plugin Store mpo na bato nyonso!

---

## 🔀 Ndenge ya Koyebisa Pull Request

### Mpo na application principale (wia-soom)

1. Fork repository
2. Salela branche ya fonction: `git checkout -b feat/my-feature`
3. Salela biloko na yo
4. Teste na ndako:
   ```bash
   ```
5. Koma na message ya malamu:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push mpe buka PR na `main`

### Mibeko ya Koma

| Prefix | Koyebisa mpo na |
|--------|------------------|
| `feat:` | Fonction sika |
| `fix:` | Kosala mabe |
| `docs:` | Lokasa kaka |
| `refactor:` | Kosala code (te na kobongola comportement) |
| `i18n:` | Mibeko ya tradiksyon |
| `plugin:` | Biloko ya plugin |

### PR Checklist

- [ ] Code ebandaka na mabe
- [ ] Te na biloko ya kokanga (salela i18n clés)
- [ ] Te na `console.log` eza na code ya production
- [ ] Teste oyo esalaka ezali kokanga

---

## 🌐 Kokanga na Tradiksyon (254 Langues)

WIA SOOM ezali na lisungi ya **254 langues** — na Amharic tii na Zulu, na kati ya Braille mpe langues ya RTL.

### Ndenge Tradiksyon Esalama

- Fichier ya langue ya base: `src/renderer/src/i18n/en.json`
- Biloko nyonso ya 254 ya langue ezali na esika moko
- Tradiksyon esalema na `scripts/translate-patch.js` (GPT-4o-mini API)

### Ndenge ya Kokanga Tradiksyon

#### Option 1: Kosala tradiksyon moko

1. Zwa fichier ya langue: `src/renderer/src/i18n/{lang-code}.json`
2. Salela tradiksyon oyo ezali mabe
3. Koyebisa PR na changement

#### Option 2: Kanga biloko oyo ezangi
§§§CHUNK_SEPARATOR§§§
#### Option 3: Tanga tradiksyon ya machine

Mokolo mingi ya 254 langues na biso ezalaka na tradiksyon ya machine. Tanga ya moto ya mboka ezali na ntina mingi!

1. Zwa fichier ya langue na yo
2. Tanga tradiksyon
3. Salela biloko nyonso oyo ezali na mabe to oyo ezali na mabe
4. Koyebisa PR

### Kodes ya Langue

Tozali na salela kodes ya ISO 639-1 (e.g., `ko`, `en`, `ja`, `ar`, `hi`) na ba variantes ya région soki ezali na ntina (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Mokonzi ya Development

### Makambo ya Liboso

- Node.js 18+
- npm 9+
- Git

### Mokonzi
§§§CHUNK_SEPARATOR§§§
### Koyebisa
§§§CHUNK_SEPARATOR§§§
> Note: Heap ya 2GB ya liboso ezali te ebele mpo na biloko ya 254 ya langue + bundle ya Monaco editor (~38MB renderer).

### Structure ya Projet
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Merci

Bokonzi nyonso eko WIA SOOM malamu mpo na ba développeurs na mokili mobimba.

Soki otya motuna, otranslate string, okoma plugin, to okokisa fonction moko — **ozali na ntina na lisolo oyo.**

---

<p align="center"><em>Esalemi na ❤️ na SmileStory Inc. mpe ba contributeurs na mokili mobimba.</em></p>
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
