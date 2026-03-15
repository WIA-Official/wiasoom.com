<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Sɛnea Wɔbɛboa WIA SOOM</h1>
<p align="center"><strong>Yɛpɛ sɛ wopɛ wo boa!</strong></p>
<p align="center">Sɛ ɛyɛ bɔne a wɔyɛ no ho nhyehyɛe, nsɛm foforɔ, plugin anaa nsɛm a wɔkyerɛ ase — abɔde biara yɛ ɔhoɔfɛ.</p>

---

## Nsɛm a Ɛda Ho Adwene

- [Code of Conduct](#code-of-conduct)
- [Sɛnea Wɔbɛka Bɔne Ho Asɛm](#-how-to-report-bugs)
- [Sɛnea Wɔbɛda Nsɛm Foforɔ Ho Adwene](#-how-to-suggest-features)
- [Sɛnea Wɔbɛma Plugin](#-how-to-submit-a-plugin)
- [Sɛnea Wɔbɛma Pull Request](#-how-to-submit-a-pull-request)
- [Nsɛm a Wɔkyerɛ ase (254 Nsɛm)](#-translation-contributions-254-languages)
- [Nkɔsoɔ Nhyehyɛe](#-development-setup)

---

## Code of Conduct

Yɛyɛ ɔman a yɛda ho adwene sɛ yɛbɛma ɔdɔ ne ɔdɔfoɔ a ɛyɛ fɛ ma obiara.

- **Sɛ yɛda ho adwene.** Fa ɔdɔ bɔ ɔman no nyinaa.
- **Sɛ yɛyɛ ɔdɔfoɔ.** Ma nsɛm a ɛyɛ mmerɛ ne nsɛm a ɛyɛ fɛ, na ɛnyɛ nsɛm a ɛyɛ bɔne.
- **Sɛ yɛyɛ ɔman a yɛda ho adwene.** Yɛyɛ ɔman a yɛda ho adwene sɛ yɛbɛyɛ nsɛm 254 na yɛda ho adwene sɛ yɛbɛyɛ abɔde fi ɔman biara.
- **Nni ɔhaw.** Ɛyɛ ɔhaw a ɛyɛ ɔhaw a ɛyɛ bɔne.

---

## 🐛 Sɛnea Wɔbɛka Bɔne Ho Asɛm

1. Kɔ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pɛ **"New Issue"**
3. Pɛ **"Bug Report"** nhyehyɛe
4. Fa nsɛm a ɛda ho adwene:
   - WIA SOOM nsɛm (Settings → About)
   - OS ne nsɛm (Windows/macOS/Linux)
   - Nsɛm a ɛda ho adwene
   - Nsɛm a wɔyɛ no ne nea ɛyɛ nokware
   - Nsɛm a ɛda ho adwene anaa terminal nsɛm sɛ ɛyɛ mmerɛ

---

## 💡 Sɛnea Wɔbɛda Nsɛm Foforɔ Ho Adwene

1. Kɔ [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pɛ **"New Issue"**
3. Pɛ **"Feature Request"** nhyehyɛe
4. Kyerɛ:
   - Dɛn na wopɛ sɛ woyɛ
   - Sɛnea wopɛ sɛ ɛyɛ
   - Nsɛm a ɛyɛ foforɔ a wopɛ sɛ woyɛ

---

## 🔌 Sɛnea Wɔbɛma Plugin

WIA SOOM wɔ plugin a ɛyɛ den — wubetumi ayɛ wo plugin wɔ 5 nsɛm mu.

### Ntɛm Nkyerɛkyerɛ
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Nkyerɛkyerɛ Kɛse

Kenkan **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** de:
- API nsɛm a ɛyɛ nokware
- Nsɛm a ɛyɛ nokware
- Nsɛm a ɛyɛ mmerɛ
- Nsɛm a ɛyɛ mmerɛ ne ɔdɔfoɔ nsɛm

### Ma Wo Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Fa wo plugin kɔ `plugins/{your-plugin-name}/`
3. Ma Pull Request
4. Ɛyɛ adwuma a wɔhwɛ no, wo plugin bɛda ho adi wɔ Plugin Store ma ɔman nyinaa!

---

## 🔀 Sɛnea Wɔbɛma Pull Request

### Ma ɔman no (wia-soom)

1. Fork repository no
2. Bɔ feature branch: `git checkout -b feat/my-feature`
3. Yɛ nsɛm a wopɛ
4. Sɔ hwɛ wɔ ɔman no mu:
   ```bash
   ```
5. Commit fa nsɛm a ɛyɛ nokware:
   ```
   feat: fa dark mode toggle kɔ settings
   ```
6. Push na bue PR wɔ `main`

### Commit Nsɛm Nkyerɛkyerɛ

| Prefix | Fa ma |
|--------|---------|
| `feat:` | Nsɛm foforɔ |
| `fix:` | Bɔne a wɔyɛ no ho nhyehyɛe |
| `docs:` | Nsɛm a ɛyɛ nokware nko |
| `refactor:` | Code a wɔyɛ no ho nhyehyɛe (nni nsɛm a ɛyɛ bɔne) |
| `i18n:` | Nsɛm a wɔkyerɛ ase nsɛm |
| `plugin:` | Nsɛm a ɛyɛ plugin ho nsɛm |

### PR Checklist

- [ ] Code no yɛ adwuma a ɛnni nsɛm a ɛyɛ bɔne
- [ ] Nni nsɛm a wɔde yɛ adwuma (fa i18n keys)
- [ ] Nni `console.log` a ɛda ho adi wɔ ɔman no mu
- [ ] Nsɛm a ɛda ho adi yɛ nokware

---

## 🌐 Nsɛm a Wɔkyerɛ ase (254 Nsɛm)

WIA SOOM yɛ **254 nsɛm** — fi Amharic kɔ Zulu, ka Braille ne RTL ns��m ho.

### Sɛnea Nsɛm a Wɔkyerɛ ase yɛ Adwuma

- Nsɛm a ɛyɛ fɛ: `src/renderer/src/i18n/en.json`
- Nsɛm 254 nyinaa wɔ baabi koro
- Nsɛm a wɔkyerɛ ase yɛ wɔ `scripts/translate-patch.js` (GPT-4o-mini API)

### Sɛnea Wɔbɛboa Nsɛm a Wɔkyerɛ ase

#### Ɔkwan 1: Sɛ yɛyɛ nsɛm a ɛda ho adwene

1. Hwehwɛ nsɛm a ɛda ho adwene: `src/renderer/src/i18n/{lang-code}.json`
2. Sɛ yɛyɛ nsɛm a ɛda ho adwene
3. Ma PR fa nsɛm a ɛda ho adwene

#### Ɔkwan 2: Fa nsɛm a ɛda ho adwene a ɛnni hɔ
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Ɔkwan 3: Sɔ nsɛm a wɔyɛ no ho nhwehwɛmu

Nsɛm a yɛyɛ no mu 254 nsɛm yɛ nsɛm a wɔyɛ no ho. Nsɛm a wɔyɛ no mu yɛ nsɛm a ɛyɛ fɛ!

1. Pɛ wo nsɛm a ɛda ho adwene
2. Sɔ nsɛm a wɔyɛ no ho nhwehwɛmu
3. Sɛ yɛyɛ nsɛm a ɛda ho adwene anaa nsɛm a ɛyɛ bɔne
4. Ma PR

### Nsɛm Kɔd

Yɛde ISO 639-1 kɔd a ɛyɛ nokware (e.g., `ko`, `en`, `ja`, `ar`, `hi`) fa nsɛm a ɛda ho adwene a ɛyɛ nokware (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Nkɔsoɔ Nhyehyɛe

### Nsɛm a Ɛda Ho Adwene

- Node.js 18+
- npm 9+
- Git

### Nhyehyɛe
```bash
```
### Bɔ
```bash
```
> Nsɛm: Ɛda ho adwene 2GB heap no nni hɔ a ɛyɛ nokware efisɛ nsɛm 254 a wɔyɛ no mu + Monaco editor bundle (~38MB renderer).

### Projeɛkt Sɛnkanee
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

## 🙏 Meda Wo Ase

Nkyɛkyerɛ biara ma WIA SOOM yɛ papa ma abɔfo a wɔwɔ wiase nyinaa.

Sɛ wopɛ sɛ wopɛ a, yɛ bɔne bi, kɔyɛ nsɛm a ɛyɛ nokware, yɛ plugin bi, anaa ka nsɛm kɛse bi ho — **woyɛ abatoɔ yi mu baako.**

---

<p align="center"><em>Wɔyɛɛ no de ❤️ fi SmileStory Inc. ne abatoɔfo a wɔwɔ wiase nyinaa.</em></p>