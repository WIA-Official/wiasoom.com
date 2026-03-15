<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuint a WIA SOOM</h1>
<p align="center"><strong>Ens encantaria les teves contribucions!</strong></p>
<p align="center">Ja sigui una correcció d'errors, una nova funcionalitat, un plugin o una traducció — cada contribució compta.</p>

---

## Taula de Continguts

- [Codi de Conducta](#code-of-conduct)
- [Com Reportar Errors](#-how-to-report-bugs)
- [Com Sugerir Funcionalitats](#-how-to-suggest-features)
- [Com Presentar un Plugin](#-how-to-submit-a-plugin)
- [Com Presentar una Sol·licitud de Tirada](#-how-to-submit-a-pull-request)
- [Contribucions de Traducció (254 Idiomes)](#-translation-contributions-254-languages)
- [Configuració de Desenvolupament](#-development-setup)

---

## Codi de Conducta

Estem compromesos a proporcionar una experiència acollidora i inclusiva per a tothom.

- **Sigues respectuós.** Tracta tothom amb dignitat.
- **Sigues constructiu.** Ofereix comentaris útils, no crítiques destructives.
- **Sigues inclusiu.** Donem suport a 254 idiomes i donem la benvinguda a contribuïdors de tots els països del món.
- **Sense assetjament.** Zero tolerància per a la discriminació de qualsevol tipus.

---

## 🐛 Com Reportar Errors

1. Ves a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Fes clic a **"Nova Problema"**
3. Tria la plantilla **"Informe d'Error"**
4. Inclou:
   - Versió de WIA SOOM (Configuració → Informació)
   - SO i versió (Windows/macOS/Linux)
   - Passos per reproduir
   - Comportament esperat vs. real
   - Captures de pantalla o sortida de terminal si és possible

---

## 💡 Com Sugerir Funcionalitats

1. Ves a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Fes clic a **"Nova Problema"**
3. Tria la plantilla **"Sol·licitud de Funcionalitat"**
4. Descriu:
   - Quin problema estàs resolent
   - Com t'imagines que funcioni
   - Quines alternatives has considerat

---

## 🔌 Com Presentar un Plugin

WIA SOOM té un potent sistema de plugins — pots construir el teu propi plugin en 5 minuts.

### Començar Ràpidament
§§§CHUNK_SEPARATOR§§§
### Guia Completa

Llegeix la **[Guia per a Desenvolupadors de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** per:
- Referència completa de l'API
- Exemples funcionals
- Tutorials pas a pas
- Millors pràctiques i normes de seguretat

### Presenta el Teu Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Afegeix el teu plugin a `plugins/{your-plugin-name}/`
3. Presenta una Sol·licitud de Tirada
4. Després de la revisió, el teu plugin apareixerà a la Botiga de Plugins per a tots els usuaris!

---

## 🔀 Com Presentar una Sol·licitud de Tirada

### Per a l'aplicació principal (wia-soom)

1. Fork el repositori
2. Crea una branca de funcionalitat: `git checkout -b feat/my-feature`
3. Fes els teus canvis
4. Prova localment:
   ```bash
   ```
5. Comet amb un missatge clar:
   ```
   feat: afegir commutador de mode fosc a configuració
   ```
6. Pujar i obrir un PR contra `main`

### Convenció del Missatge de Compromís

| Prefix | Ús per |
|--------|---------|
| `feat:` | Nova funcionalitat |
| `fix:` | Correcció d'errors |
| `docs:` | Només documentació |
| `refactor:` | Reestructuració de codi (sense canvi de comportament) |
| `i18n:` | Actualitzacions de traducció |
| `plugin:` | Canvis relacionats amb plugins |

### Llista de Verificació del PR

- [ ] El codi s'executa sense errors
- [ ] Sense cadenes codificades (utilitza claus i18n)
- [ ] Sense `console.log` deixat en codi de producció
- [ ] Les proves existents encara passen

---

## 🌐 Contribucions de Traducció (254 Idiomes)

WIA SOOM dóna suport a **254 idiomes** — des de l'amharic fins al zulú, incloent-hi el braille i idiomes RTL.

### Com Funciona la Traducció

- Fitxer de llengua base: `src/renderer/src/i18n/en.json`
- Tots els 254 fitxers d'idioma estan al mateix directori
- La traducció es fa a través de `scripts/translate-patch.js` (API GPT-4o-mini)

### Com Contribuir amb Traduccions

#### Opció 1: Corregir una traducció específica

1. Troba el fitxer d'idioma: `src/renderer/src/i18n/{lang-code}.json`
2. Corregeix la traducció incorrecta
3. Presenta un PR amb el canvi

#### Opció 2: Afegir claus mancants
§§§CHUNK_SEPARATOR§§§
#### Opció 3: Revisar traduccions automàtiques

Molts dels nostres 254 idiomes han estat traduïts per màquina. Les revisions de parlants nadius són increïblement valuoses!

1. Escull el teu fitxer d'idioma
2. Revisa les traduccions
3. Corregeix qualsevol traducció incòmoda o incorrecta
4. Presenta un PR

### Codis d'Idiomes

Utilitzem codis estàndard ISO 639-1 (per exemple, `ko`, `en`, `ja`, `ar`, `hi`) amb variants regionals quan és necessari (per exemple, `zh-CN`, `pt-BR`).

---

## 🛠 Configuració de Desenvolupament

### Requisits Prèvis

- Node.js 18+
- npm 9+
- Git

### Configuració
§§§CHUNK_SEPARATOR§§§
### Compilació
§§§CHUNK_SEPARATOR§§§
> Nota: La memòria per defecte de 2GB no és suficient a causa dels 254 fitxers d'idioma + paquet de l'editor Monaco (~38MB renderer).

### Estructura del Projecte
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Gràcies

Cada contribució fa que WIA SOOM sigui millor per als desenvolupadors de tot el món.

Ja sigui que corregeixes un error tipogràfic, tradueixes una cadena, construeixes un plugin o afegeixes una funcionalitat important — **tu formes part d'aquesta història.**

---

<p align="center"><em>Construït amb ❤️ per SmileStory Inc. i col·laboradors de tot el món.</em></p>
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
