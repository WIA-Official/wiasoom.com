<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kòman pou kontribye nan WIA SOOM</h1>
<p align="center"><strong>Nou ta renmen kontribisyon ou yo!</strong></p>
<p align="center">Kit se yon korije erè, yon nouvo karakteristik, yon plugin, oswa yon tradiksyon — chak kontribisyon gen valè.</p>

---

## Tablo Kontni

- [Kòd Konduit](#code-of-conduct)
- [Kòman pou Rapòte Erè](#-how-to-report-bugs)
- [Kòman pou Sijere Karakteristik](#-how-to-suggest-features)
- [Kòman pou Soumèt yon Plugin](#-how-to-submit-a-plugin)
- [Kòman pou Soumèt yon Pull Request](#-how-to-submit-a-pull-request)
- [Kontribisyon Tradiksyon (254 Lang)](#-translation-contributions-254-languages)
- [Konfigirasyon Devlopman](#-development-setup)

---

## Kòd Konduit

Nou angaje nou pou ofri yon eksperyans akeyan ak enklizif pou tout moun.

- **Respekte lòt moun.** Trete tout moun avèk diyite.
- **Fè konstriktif.** Ofri fidbak itil, pa kritik destriktif.
- **Fè enklizif.** Nou sipòte 254 lang ak akeyi kontribitè soti nan chak peyi sou Latè.
- **Pa gen asèlman.** Tolerans zewo pou diskriminasyon nan okenn fòm.

---

## 🐛 Kòman pou Rapòte Erè

1. Ale sou [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klike sou **"New Issue"**
3. Chwazi modèl **"Bug Report"**
4. Mete ladan:
   - Vèsyon WIA SOOM (Anviwònman → Sou)
   - OS ak vèsyon (Windows/macOS/Linux)
   - Etap pou repwodui
   - Konpòtman espere kont konpòtman aktyèl
   - Ekran oswa sòti tèminal si sa posib

---

## 💡 Kòman pou Sijere Karakteristik

1. Ale sou [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klike sou **"New Issue"**
3. Chwazi modèl **"Feature Request"**
4. Dekri:
   - Ki pwoblèm ou ap rezoud
   - Kijan ou imajine li ap fonksyone
   - Nenpòt altènatif ou te konsidere

---

## 🔌 Kòman pou Soumèt yon Plugin

WIA SOOM gen yon sistèm plugin pwisan — ou ka bati pwòp plugin ou nan 5 minit.

### Kòmanse Rapid
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Gid Konplè

Li **[Gid Devlopè Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** pou:
- Referans API konplè
- Egzanp ki mache
- Tutorial etap pa etap
- Pi bon pratik ak règ sekirite

### Soumèt Plugin Ou

1. Fork [Plugin Store](https://wiasoom.com)
2. Ajoute plugin ou nan `plugins/{your-plugin-name}/`
3. Soumèt yon Pull Request
4. Apre revizyon, plugin ou ap parèt nan Plugin Store pou tout itilizatè yo!

---

## 🔀 Kòman pou Soumèt yon Pull Request

### Pou aplikasyon prensipal la (wia-soom)

1. Fork repozitwa a
2. Kreye yon branch karakteristik: `git checkout -b feat/my-feature`
3. Fè chanjman ou yo
4. Tès lokalman:
   ```bash
   ```
5. Komite avèk yon mesaj klè:
   ```
   feat: ajoute yon switch mòd nwa nan anviwònman yo
   ```
6. Pouse epi ouvri yon PR kont `main`

### Konvansyon Mesaj Komit

| Prefix | Itilize pou |
|--------|-------------|
| `feat:` | Nouvo karakteristik |
| `fix:` | Korije erè |
| `docs:` | Dokimantasyon sèlman |
| `refactor:` | Restructurasyon kòd (san chanjman konpòtman) |
| `i18n:` | Mizajou tradiksyon |
| `plugin:` | Chanjman ki gen rapò ak plugin |

### Lis Chèk PR

- [ ] Kòd la kouri san erè
- [ ] Pa gen chenn ki kodifye (itilize kle i18n)
- [ ] Pa gen `console.log` kite nan kòd pwodiksyon
- [ ] Tès ki egziste yo toujou pase

---

## 🌐 Kontribisyon Tradiksyon (254 Lang)

WIA SOOM sipòte **254 lang** — soti nan Amharik rive Zulu, ki gen ladan Braille ak lang RTL.

### Kòman Tradiksyon an Fonksyone

- Dosye lang baz: `src/renderer/src/i18n/en.json`
- Tout 254 dosye lang yo nan menm anyè a
- Tradiksyon fèt atravè `scripts/translate-patch.js` (GPT-4o-mini API)

### Kòman pou Kontribye Tradiksyon

#### Opsyon 1: Korije yon tradiksyon espesifik

1. Jwenn dosye lang lan: `src/renderer/src/i18n/{lang-code}.json`
2. Korije tradiksyon ki pa kòrèk la
3. Soumèt yon PR ak chanjman an

#### Opsyon 2: Ajoute kle ki manke
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opsyon 3: Revize tradiksyon machin

Anpil nan 254 lang nou yo te tradui pa machin. Revizyon natif natal yo se trè valab!

1. Chwazi dosye lang ou
2. Revize tradiksyon yo
3. Korije nenpòt tradiksyon ki maladwa oswa ki pa kòrèk
4. Soumèt yon PR

### Kòd Lang

Nou itilize kòd estanda ISO 639-1 (pa egzanp, `ko`, `en`, `ja`, `ar`, `hi`) ak varyant rejyonal kote sa nesesè (pa egzanp, `zh-CN`, `pt-BR`).

---

## 🛠 Konfigirasyon Devlopman

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Konfigirasyon
```bash
```
### Konpilasyon
```bash
```
> Remak: Heap default 2GB la pa ase akòz 254 dosye lang + pakè Monaco (~38MB renderer).

### Estrikti Pwojè
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

## 🙏 Mèsi

Chak kontribisyon fè WIA SOOM pi bon pou devlopè atravè lemond.

Kit ou korije yon erè, tradui yon chèn, bati yon plugin, oswa ajoute yon gwo karakteristik — **ou se yon pati nan istwa sa a.**

---

<p align="center"><em>Konstwi ak ❤️ pa SmileStory Inc. ak kontribitè atravè lemond.</em></p>