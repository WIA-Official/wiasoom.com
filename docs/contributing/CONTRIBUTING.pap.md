<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribuí pa WIA SOOM</h1>
<p align="center"><strong>Nos ta stima bo kontribushon!</strong></p>
<p align="center">Si ta un correkshon di bug, un nuevo karakterístika, un plugin, of un tradukshon — cada kontribushon ta importante.</p>

---

## Tabla di Kontenido

- [Código di Conducta](#código-di-conducta)
- [Komo pa Reporta Bugs](#-komo-pa-reporta-bugs)
- [Komo pa Sugeri Karakterístikas](#-komo-pa-sugeri-karakterístikas)
- [Komo pa Somete un Plugin](#-komo-pa-somete-un-plugin)
- [Komo pa Somete un Pull Request](#-komo-pa-somete-un-pull-request)
- [Kontribushon di Tradukshon (254 Idioma)](#-kontribushon-di-tradukshon-254-idioma)
- [Setup di Desaroyo](#-setup-di-desaroyo)

---

## Código di Conducta

Nos ta komitá pa ofresé un eksperensia acojedor i inklusivo pa tur hende.

- **Ta respetuoso.** Trata tur hende ku dignidad.
- **Ta konstruktivo.** Ofresé feedback útil, no kritik destructivo.
- **Ta inklusivo.** Nos ta suporta 254 idioma i ta akseptá kontribuidor di tur pais riba Tierra.
- **No acoso.** Toleransia cero pa diskriminashon di kualke tipo.

---

## 🐛 Komo pa Reporta Bugs

1. Bini na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Escoge e **"Bug Report"** template
4. Inkludí:
   - WIA SOOM versi (Instellingen → Sobre)
   - OS i versi (Windows/macOS/Linux)
   - Pasos pa reproduci
   - Komportashon esperado vs. real
   - Screenshots of salida di terminal si posibel

---

## 💡 Komo pa Sugeri Karakterístikas

1. Bini na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klik **"New Issue"**
3. Escoge e **"Feature Request"** template
4. Deskribi:
   - Kual problema bo ta solushoná
   - Kon bo ta imagina ku ta traha
   - Cualkier alternatíva ku bo a konsiderá

---

## 🔌 Komo pa Somete un Plugin

WIA SOOM tin un sistema di plugin potente — bo por konstrui bo propio plugin den 5 minuto.

### Komensamentu Rapid
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guía Completo

Lé e **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** pa:
- Referensia API kompleto
- Ejemplonan ku ta traha
- Tutorialnan paso-paso
- Mejornan praktiká i regla di seguridad

### Somete Bo Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Adicioná bo plugin na `plugins/{bo-plugin-nan}/`
3. Somete un Pull Request
4. Despues di revisión, bo plugin lo aparé na Plugin Store pa tur usuario!

---

## 🔀 Komo pa Somete un Pull Request

### Pa e aplikashon principal (wia-soom)

1. Fork e repositorio
2. Crea un feature branch: `git checkout -b feat/my-feature`
3. Hasi bo kambio
4. Test local:
   ```bash
   ```
5. Commit ku un mensahe klaro:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push i abri un PR kontra `main`

### Konvenshon di Mensaje di Commit

| Prefix | Usa pa |
|--------|---------|
| `feat:` | Nuevo karakterístika |
| `fix:` | Correksion di bug |
| `docs:` | Dokumentashon solamente |
| `refactor:` | Reestructurashon di kódigo (sin kambio di komportashon) |
| `i18n:` | Aktualisashon di tradukshon |
| `plugin:` | Kambio relashoná ku plugin |

### PR Checklist

- [ ] Kódigo ta corre sin eror
- [ ] No tin stringnan hardcoded (usa i18n keys)
- [ ] No tin `console.log` dejá den kódigo di produksyon
- [ ] Testnan existente ta sigue pasa

---

## 🌐 Kontribushon di Tradukshon (254 Idioma)

WIA SOOM ta suporta **254 idioma** — desde Amhárico te Zulu, inkluyendo Braille i idioma RTL.

### Komo Tradukshon Ta Trabaha

- Base idioma file: `src/renderer/src/i18n/en.json`
- Tur 254 idioma files ta den e mesmó direktorio
- Tradukshon ta hasi via `scripts/translate-patch.js` (GPT-4o-mini API)

### Komo pa Kontribuí Tradukshon

#### Opción 1: Korigi un tradukshon spesífiko

1. Buska e idioma file: `src/renderer/src/i18n/{lang-code}.json`
2. Korigi e tradukshon incorrecto
3. Somete un PR ku e kambio

#### Opción 2: Adicioná klaves ku falta
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opción 3: Revisa tradukshon di makina

Muchanan di nos 254 idioma a ser tradukido di makina. Revisión di hablante nativo ta inkrédibelmente valioso!

1. Escoge bo idioma file
2. Revisa e tradukshon
3. Korigi kualkier tradukshon incomoda of incorrecto
4. Somete un PR

### Códigos di Idioma

Nos ta usa códigos estándar ISO 639-1 (ejemplo, `ko`, `en`, `ja`, `ar`, `hi`) ku variantnan regional kaminda necesario (ejemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Setup di Desaroyo

### Prerequisitos

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Nota: E heap di 2GB predeterminado no ta suficiente debi di e 254 idioma files + Monaco editor bundle (~38MB renderer).

### E Estructura di Proyekto
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

## 🙏 Danki

Cada kontribushon ta hasi WIA SOOM mihó pa desaroyadó alrededor di mundo.

Si bo ta korigi un typo, tradusi un string, konstrui un plugin, of agrega un karakterístika grandi — **bo ta parte di e historia aki.**

---

<p align="center"><em>Konstruí ku ❤️ pa SmileStory Inc. i kontribuhon di todo mundo.</em></p>