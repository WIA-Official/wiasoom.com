<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuer a WIA SOOM</h1>
<p align="center"><strong>Nos contributions son benvengudas!</strong></p>
<p align="center">Que si es una correcció de bug, una nòva caracteristica, un plugin, o una traduccion — cada contribucion compte.</p>

---

## Taula de Contenuts

- [Codi de Conducta](#codi-de-conducta)
- [Com Reportar Bugs](#-com-reportar-bugs)
- [Com Sugerir Caracteristiques](#-com-sugerir-caracteristiques)
- [Com Submetre un Plugin](#-com-submetre-un-plugin)
- [Com Submetre una Pull Request](#-com-submetre-una-pull-request)
- [Contribucions de Traduccion (254 Lengües)](#-contribucions-de-traduccion-254-lengües)
- [Configuracion de Desenvolupament](#-configuracion-de-desenvolupament)

---

## Codi de Conducta

Nos s'engajam a proporcionar una experiéncia accueillenta e inclusiva per totes e tots.

- **Se respectuós.** Tractar totes e tots amb dignitat.
- **Se constructiu.** Ofrir de retroaccions utilas, pas de criticas destructivas.
- **Se inclusiu.** Nos sostenèm 254 lengües e benvenguem las contribucions de totes los païses de la Tèrra.
- **Pas de harcèlement.** Zero tolerància per la discriminacion de tot tipe.

---

## 🐛 Com Reportar Bugs

1. Vas a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clica **"Nova Problema"**
3. Còja lo template **"Report de Bug"**
4. Incluís:
   - Version de WIA SOOM (Configuracions → A prepaus)
   - SO e version (Windows/macOS/Linux)
   - Passes per reproduire
   - Comportament esperat vs. real
   - Capturas d'escòta o sortida de terminal se possible

---

## 💡 Com Sugerir Caracteristiques

1. Vas a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clica **"Nova Problema"**
3. Còja lo template **"Demanda de Caracteristica"**
4. Descriu:
   - Quina problema resòlz
   - Cossí t'imagines que fonciona
   - Quinas alternatives as considerat

---

## 🔌 Com Submetre un Plugin

WIA SOOM a un poderós sistèma de plugins — pòdes construir ton pròpri plugin en 5 minutas.

### Començament Rapide
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guia Completa

Legís lo **[Guia del Desvolopador de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Referéncia completa de l'API
- Exemples funcionals
- Tutorials pas a pas
- Melhores practicas e règlas de seguretat

### Submetre Ton Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Apond ton plugin a `plugins/{ton-nom-de-plugin}/`
3. Submet una Pull Request
4. Après la revisió, ton plugin apareis dins lo Plugin Store per totes los utilizators!

---

## 🔀 Com Submetre una Pull Request

### Per l'aplicacion principal (wia-soom)

1. Forka lo repositori
2. Crea una branca de caracteristica: `git checkout -b feat/ma-caracteristica`
3. Fai las modificacions
4. Testeja localament:
   ```bash
   ```
5. Commit amb un messatge clar:
   ```
   feat: apondre un commutador de mòda escura a las configuracions
   ```
6. Pòsta e obri una PR contra `main`

### Convencions de Messatge de Commit

| Prefixe | Utilizar per |
|---------|--------------|
| `feat:` | Nòva caracteristica |
| `fix:`  | Correcció de bug |
| `docs:` | Somente documentacion |
| `refactor:` | Reestructuracion de codi (sens cambiament de comportament) |
| `i18n:` | Actualizacions de traduccion |
| `plugin:` | Cambiaments relatius als plugins |

### Lista de Verificacion de PR

- [ ] Lo codi fonciona sens errors
- [ ] Pas de cadenas codadas (utiliza claus i18n)
- [ ] Pas de `console.log` deixat dins lo codi de produccion
- [ ] Los tests existents continúan a passar

---

## 🌐 Contribucions de Traduccion (254 Lengües)

WIA SOOM sosten **254 lengües** — d'Amharic a Zulu, incluent Braille e lengües RTL.

### Cossí Fonciona la Traduccion

- Fichièr de lenga de basa: `src/renderer/src/i18n/en.json`
- Totes los 254 fichièrs de lenga son dins la meteissa directòria
- La traduccion se fa via `scripts/translate-patch.js` (API GPT-4o-mini)

### Cossí Contribuir a las Traduccions

#### Opció 1: Corregir una traduccion específica

1. Troba lo fichièr de lenga: `src/renderer/src/i18n/{lang-code}.json`
2. Corrige la traduccion incorrecta
3. Submet una PR amb la modificacion

#### Opció 2: Apondre claus manquantes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opció 3: Revisar traduccions automáticas

Moltas de las nòstras 254 lengües son estadas traduïdes per maquinas. Las revisons de locutors natius son inestimablas!

1. Còja ton fichièr de lenga
2. Revisar las traduccions
3. Corrige las traduccions estranhas o incorrectas
4. Submet una PR

### Codis de Lenga

Utilizam los codis ISO 639-1 estandards (per exemple, `ko`, `en`, `ja`, `ar`, `hi`) amb variants regionals quand es necessari (per exemple, `zh-CN`, `pt-BR`).

---

## 🛠 Configuracion de Desenvolupament

### Prerequisits

- Node.js 18+
- npm 9+
- Git

### Configuracion
```bash
```
### Construccion
```bash
```
> Remarca: Lo heap de 2GB per default es pas sufisent a causa dels 254 fichièrs de lenga + paquet de l'editor Monaco (~38MB renderer).

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

## 🙏 Mercé

Cada contribucion fa de WIA SOOM un mieu per los desvolopaires d'entorn del mond.

Que siá que corregis un errò, tradusís una cadena, construís un plugin, o ajòsta una caracteristica importanta — **tu fas partida d'aquesta istòria.**

---

<p align="center"><em>Construit amb ❤️ per SmileStory Inc. e los contributors del mond entièr.</em></p>