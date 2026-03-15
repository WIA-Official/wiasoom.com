<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribye dan WIA SOOM</h1>
<p align="center"><strong>Nou kontan ou kontribisyon!</strong></p>
<p align="center">Si sa se enn bug fix, nouvo karakteristik, plugin, ouswa tradiksyon — tou kontribisyon i enportan.</p>

---

## Tablo Konteni

- [Kòd Konduit](#code-of-conduct)
- [Koman pou Raporte Bug](#-how-to-report-bugs)
- [Koman pou Sijere Karakteristik](#-how-to-suggest-features)
- [Koman pou Soumet enn Plugin](#-how-to-submit-a-plugin)
- [Koman pou Soumet enn Pull Request](#-how-to-submit-a-pull-request)
- [Kontribisyon Tradiksyon (254 Lang)](#-translation-contributions-254-languages)
- [Set up Devlopman](#-development-setup)

---

## Kòd Konduit

Nou angaze pou ofer enn lexperyans akeyan ek inklizif pou tou dimoun.

- **Sey respekte.** Trat tou dimoun avek dignite.
- **Sey konstriktif.** Ofer feedback itil, pa kritik destriktif.
- **Sey inklizif.** Nou sipor 254 lang ek akey tou kontribitè depi tou pei dan latè.
- **Pa fer okenn harasman.** Zero tolerans pou diskriminasyon dan okenn fason.

---

## 🐛 Koman pou Raporte Bug

1. Ale dan [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klike **"New Issue"**
3. Chwazi **"Bug Report"** template
4. Enklir:
   - WIA SOOM vèsyon (Settings → About)
   - OS ek vèsyon (Windows/macOS/Linux)
   - Etap pou reproduir
   - Konportman atann vs. aktyel
   - Ekran oubyen output terminal si posib

---

## 💡 Koman pou Sijere Karakteristik

1. Ale dan [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klike **"New Issue"**
3. Chwazi **"Feature Request"** template
4. Dekri:
   - Ki problèm ou pe rezourd
   - Koman ou imazinen i pe travay
   - Nenport altènatif ki ou'n konsidere

---

## 🔌 Koman pou Soumet enn Plugin

WIA SOOM in annan enn sistem plugin pwisan — ou kapav konstrir ou prop plugin dan 5 minit.

### Komen Komanse
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Gid Konplè

Li **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** pou:
- Referans API konplè
- Egzanp ki pe travay
- Tutoriels pa pa
- Mezi pratik ek regleman sekirite

### Soumet Ou Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Azout ou plugin dan `plugins/{your-plugin-name}/`
3. Soumet enn Pull Request
4. Apre revizyon, ou plugin pou parèt dan Plugin Store pou tou itilizater!

---

## 🔀 Koman pou Soumet enn Pull Request

### Pou aplikasyon prensipal (wia-soom)

1. Fork sa repository la
2. Kre enn feature branch: `git checkout -b feat/my-feature`
3. Fer ou chanjman
4. Test lokalman:
   ```bash
   ```
5. Komit avek enn mesaj klèr:
   ```
   feat: add dark mode toggle to settings
   ```
6. Pouse ek ouvri enn PR kont `main`

### Konvansyon Mesaz Komit

| Prefix | Servi pou |
|--------|---------|
| `feat:` | Nouvo karakteristik |
| `fix:` | Bug fix |
| `docs:` | Dokimantasyon selman |
| `refactor:` | Restructirasyon kòd (pa okenn chanjman konportman) |
| `i18n:` | Mizajou tradiksyon |
| `plugin:` | Chanjman ki lye avek plugin |

### PR Lis Verifikasyon

- [ ] Kòd pe kouri san erè
- [ ] Pa okenn string hardcoded (servi i18n keys)
- [ ] Pa okenn `console.log` laisse dan kòd prodiksyon
- [ ] Test ki egziste ankor pe pase

---

## 🌐 Kontribisyon Tradiksyon (254 Lang)

WIA SOOM sipor **254 lang** — depi Amharic ziska Zulu, enkli Braille ek lang RTL.

### Koman Tradiksyon I Travay

- Dosye lang baz: `src/renderer/src/i18n/en.json`
- Tou 254 dosye lang dan menm repozitwar
- Tradiksyon i fer atraver `scripts/translate-patch.js` (GPT-4o-mini API)

### Koman pou Kontribye Tradiksyon

#### Opsyon 1: Fix enn tradiksyon spesifik

1. Trouv dosye lang: `src/renderer/src/i18n/{lang-code}.json`
2. Fix tradiksyon ki pa kòrèk
3. Soumet enn PR avek sa chanjman

#### Opsyon 2: Azout kle ki manke
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsyon 3: Reviz tradiksyon machin

Plizyer parmi nou 254 lang ti tradwir par machin. Revizyon par natif se enn gran valè!

1. Pran ou dosye lang
2. Reviz tradiksyon
3. Fix okenn tradiksyon ki pa bon ouswa ki pa kòrèk
4. Soumet enn PR

### Kòd Lang

Nou servi kòd ISO 639-1 standar (par egzanp, `ko`, `en`, `ja`, `ar`, `hi`) avek variant regional kot neseser (par egzanp, `zh-CN`, `pt-BR`).

---

## 🛠 Set up Devlopman

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Set up
```bash
```
### Bati
```bash
```
> Remak: Sa 2GB heap par defo pa ase akoz 254 dosye lang + bundle editor Monaco (~38MB renderer).

### Estrikti Proze
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

## 🙏 Mersi

Tou kontribisyon fer WIA SOOM pli bon pou devlopers dan lemonn.

Si ou korek enn tip, tradwir enn string, bati enn plugin, ou ajoute enn gran karakteristik — **ou enn parti sa l'histoire.**

---

<p align="center"><em>Bati avek ❤️ par SmileStory Inc. ek kontribitè dan lemonn.</em></p>