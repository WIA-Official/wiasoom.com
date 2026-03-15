<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuïr a WIA SOOM</h1>
<p align="center"><strong>Nos agradariá fòrça las vòstras contribucions!</strong></p>
<p align="center">Que siá un correccion de bug, una novèla foncionalitat, un plugin, o una traduccion — cada contribucion compta.</p>

---

## Taula de Contenuts

- [Code de Conduita](#code-of-conduct)
- [Cossí Raportar de Bugs](#-how-to-report-bugs)
- [Cossí Sugerir de Foncionalitats](#-how-to-suggest-features)
- [Cossí Submetre un Plugin](#-how-to-submit-a-plugin)
- [Cossí Submetre una Demanda de Tirar](#-how-to-submit-a-pull-request)
- [Contribucions de Traduccion (254 Lengas)](#-translation-contributions-254-languages)
- [Configuracion de Desvolopament](#-development-setup)

---

## Code de Conduita

Sèm engatjats a proporcionar una experiéncia accueillanta e inclusiva per totes e tots.

- **Sètz respectuós.** Tractatz cadun amb dignitat.
- **Sètz constructius.** Ofritz de retroaccions utilas, pas de criticas destructivas.
- **Sètz inclusius.** Sostenèm 254 lengas e accueillim los contributors de cada país de la Tèrra.
- **Pas de harcèlement.** Tolerància zero per la discriminacion de tota mena.

---

## 🐛 Cossí Raportar de Bugs

1. Allez a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicatz sus **"Nova Problematica"**
3. Esculhatz lo model **"Rapòrt de Bug"**
4. Incluètz:
   - Version de WIA SOOM (Paramètres → A prepaus)
   - SO e version (Windows/macOS/Linux)
   - Passes per reproduire
   - Comportament esperat vs. real
   - Capturas d'escèna o sortida de terminal se possible

---

## 💡 Cossí Sugerir de Foncionalitats

1. Allez a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clicatz sus **"Nova Problematica"**
3. Esculhatz lo model **"Demanda de Foncionalitat"**
4. Descrivètz:
   - Quin problèma resòlz
   - Cossí o imaginas que fonciona
   - Quinas alternatives avètz considerat

---

## 🔌 Cossí Submetre un Plugin

WIA SOOM a un sistema de plugins poderós — pòdetz crear vòstre pròpri plugin en 5 minutas.

### Començament Rapide
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guia Completa

Legissètz lo **[Guia del Desvolopador de Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Referéncia completa de l'API
- Exemples que foncionan
- Tutorials pas a pas
- Melhores practicas e règlas de seguretat

### Submetètz Vòstre Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Apondètz vòstre plugin a `plugins/{your-plugin-name}/`
3. Submetètz una Demanda de Tirar
4. Après la revisa, vòstre plugin apareis dins lo Plugin Store per totes los utilizators!

---

## 🔀 Cossí Submetre una Demanda de Tirar

### Per l'aplicacion principal (wia-soom)

1. Fork la repositori
2. Creètz una branca de foncionalitat: `git checkout -b feat/my-feature`
3. Fai vòstras modificacions
4. Testatz localament:
   ```bash
   ```
5. Commitatz amb un messatge clar:
   ```
   feat: apondre l'alternador de mòda escura a las configuracions
   ```
6. Push e ouvretz una PR contra `main`

### Convencion de Messatge de Commit

| Prefixe | Utilizat per |
|---------|--------------|
| `feat:` | Novèla foncionalitat |
| `fix:`  | Correccion de bug |
| `docs:` | Somenta de documentacion |
| `refactor:` | Reestructuracion de còde (sens cambiament de comportament) |
| `i18n:` | Actualizacions de traduccion |
| `plugin:` | Cambiaments ligats als plugins |

### Lista de Verificacion de PR

- [ ] Lo còde fonciona sens errors
- [ ] Pas de cadenas codadas (utilizatz las claus i18n)
- [ ] Pas de `console.log` deixat dins lo còde de produccion
- [ ] Los tests existents continuan de passar

---

## 🌐 Contribucions de Traduccion (254 Lengas)

WIA SOOM sosten **254 lengas** — d'Amharic a Zulu, en incluent lo Braille e las lengas RTL.

### Cossí Fonciona la Traduccion

- Fichièr de lenga de basa: `src/renderer/src/i18n/en.json`
- Totes los 254 fichièrs de lenga son dins la meteissa directòria
- La traduccion se fa via `scripts/translate-patch.js` (API GPT-4o-mini)

### Cossí Contribuir a las Traduccions

#### Opció 1: Corregir una traduccion específica

1. Trobatz lo fichièr de lenga: `src/renderer/src/i18n/{lang-code}.json`
2. Corregissètz la traduccion incorrecta
3. Submetètz una PR amb la modificacion

#### Opció 2: Apondre claus manquantes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opció 3: Revisar las traduccions automaticas

Mantas de nòstras 254 lengas son estadas traduïdas per un mòtur. Las revisas de locutors natius son incrediblament valuosas!

1. Escollissètz vòstre fichièr de lenga
2. Revisatz las traduccions
3. Corregissètz las traduccions estranhas o incorrectas
4. Submetètz una PR

### Codis de Lenga

Utilizam los codis ISO 639-1 estandards (per exemple, `ko`, `en`, `ja`, `ar`, `hi`) amb variants regionalas quand es necessari (per exemple, `zh-CN`, `pt-BR`).

---

## 🛠 Configuracion de Desvolopament

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Configuracion
```bash
```
### Construccion
```bash
```
> Remarca: Lo tas de 2GB per defaut es pas sufisent a causa dels 254 fichièrs de lenga + paquet del editor Monaco (~38MB renderer).

### Estructura del Projecte
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

## 🙏 Mercés

Cada contribucion melhora WIA SOOM per los desvolopaires del mond entièr.

Que siá que corregissètz un error tipografic, traduïssètz una cadena, construïssètz un plugin, o que siá que s'agís d'ajustar una caracteristica majora — **sètz una part d'aquesta istòria.**

---

<p align="center"><em>Construït amb ❤️ per SmileStory Inc. e contributors del mond entièr.</em></p>