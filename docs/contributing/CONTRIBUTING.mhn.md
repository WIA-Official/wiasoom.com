<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribuendo a WIA SOOM</h1>
<p align="center"><strong>Nos plase tu kontribucions!</strong></p>
<p align="center">Sia ke sia un bug fix, nova funkcio, plugin, o tradukcio — ĉiu kontribucio gravas.</p>

---

## Tablo de Enhavo

- [Kodo de Konduto](#kodo-de-konduto)
- [Kiel Raporti Bugojn](#-kiel-raporti-bugojn)
- [Kiel Sugesti Funkciojn](#-kiel-sugesti-funkciojn)
- [Kiel Sendi Pluginon](#-kiel-sendi-pluginon)
- [Kiel Sendi Pull Request](#-kiel-sendi-pull-request)
- [Tradukaj Kontribucioj (254 Lingvoj)](#-tradukaj-kontribucioj-254-lingvoj)
- [Disvolva Agordo](#-disvolva-agordo)

---

## Kodo de Konduto

Ni engaĝiĝas al provizi bonvenigan kaj inkluzivan sperton por ĉiuj.

- **Estu respektinda.** Traktu ĉiujn kun digno.
- **Estu konstruktiva.** Ofertu helpeman reagadon, ne detruan kritikadon.
- **Estu inkluziva.** Ni subtenas 254 lingvojn kaj bonvenigas kontribuantojn el ĉiu lando sur Tero.
- **Neniu ĝenado.** Nula toleremo por diskriminacio de iu ajn tipo.

---

## 🐛 Kiel Raporti Bugojn

1. Iru al [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klaku **"Nova Problemo"**
3. Elektu la **"Bug Raporto"** ŝablonon
4. Inkluzivu:
   - WIA SOOM versio (Agordoj → Pri)
   - OS kaj versio (Windows/macOS/Linux)
   - Paŝoj por reprodukti
   - Antaŭvidita kontraŭ fakta konduto
   - Ekrankopioj aŭ terminala eligo se eble

---

## 💡 Kiel Sugesti Funkciojn

1. Iru al [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klaku **"Nova Problemo"**
3. Elektu la **"Funkcio Peto"** ŝablonon
4. Priskribu:
   - Kian problemon vi solvas
   - Kiel vi imagas ke ĝi funkcias
   - Iuj alternativoj, kiujn vi konsideris

---

## 🔌 Kiel Sendi Pluginon

WIA SOOM havas potenca plugin sistemo — vi povas konstrui vian propran plugin en 5 minutoj.

### Rapida Komenco
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Plena Gvidilo

Legu la **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** por:
- Kompleta API referenco
- Funkciaj ekzemploj
- Paŝo-post-paŝo instrukcioj
- Plej bonaj praktikoj kaj sekurecaj reguloj

### Sendu Vian Pluginon

1. Forku [Plugin Store](https://wiasoom.com)
2. Aldonu vian pluginon al `plugins/{via-plugin-nomo}/`
3. Sendu Pull Request
4. Post revizio, via plugin aperas en la Plugin Store por ĉiuj uzantoj!

---

## 🔀 Kiel Sendi Pull Request

### Por la ĉefa aplikaĵo (wia-soom)

1. Forku la repositorion
2. Kreu funkcia branĉo: `git checkout -b feat/mia-funkcio`
3. Faru viajn ŝanĝojn
4. Testu loke:
   ```bash
   ```
5. Commit kun klara mesaĝo:
   ```
   feat: aldoni malhelan reĝimon al agordoj
   ```
6. Puŝu kaj malfermu PR kontraŭ `main`

### Commit Mesaĝa Konvencio

| Prefikso | Uzu por |
|----------|---------|
| `feat:`  | Nova funkcio |
| `fix:`   | Bug fix |
| `docs:`  | Nur dokumentado |
| `refactor:` | Kodo restrukturado (sen kondutŝanĝo) |
| `i18n:`  | Tradukaj ĝisdatigoj |
| `plugin:` | Plugin-rilataj ŝanĝoj |

### PR Kontrolisto

- [ ] Kodo funkcias sen eraroj
- [ ] Neniuj hardkoditaj ŝnuroj (uzu i18n ŝlosilojn)
- [ ] Neniu `console.log` lasita en produktada kodo
- [ ] Ekzistantaj testoj ankoraŭ pasas

---

## 🌐 Tradukaj Kontribucioj (254 Lingvoj)

WIA SOOM subtenas **254 lingvojn** — de Amhara ĝis Zulu, inkluzive Brajlon kaj RTL lingvojn.

### Kiel Traduko Funkcias

- Bazlingva dosiero: `src/renderer/src/i18n/en.json`
- Ĉiuj 254 lingvaj dosieroj estas en la sama dosierujo
- Traduko farita per `scripts/translate-patch.js` (GPT-4o-mini API)

### Kiel Kontribui Tradukojn

#### Opcio 1: Ripari specifan tradukon

1. Trovu la lingvan dosieron: `src/renderer/src/i18n/{lingvo-kodo}.json`
2. Riparu la malĝustan tradukon
3. Sendu PR kun la ŝanĝo

#### Opcio 2: Aldoni mankantajn ŝlosilojn
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opcio 3: Revizii maŝintradukojn

Multaj el niaj 254 lingvoj estis maŝintradukitaj. Revizioj de denaskaj parolantoj estas tre valoraj!

1. Elektu vian lingvan dosieron
2. Revizu la tradukojn
3. Riparu ajnajn malĝustajn aŭ strangajn tradukojn
4. Sendu PR

### Lingvaj Kodoj

Ni uzas normajn ISO 639-1 kodojn (ekz., `ko`, `en`, `ja`, `ar`, `hi`) kun regionaj variantaj kodoj kie necesas (ekz., `zh-CN`, `pt-BR`).

---

## 🛠 Disvolva Agordo

### Antaŭkondiĉoj

- Node.js 18+
- npm 9+
- Git

### Agordo
```bash
```
### Konstrui
```bash
```
> Notu: La defaŭlta 2GB heap ne sufiĉas pro la 254 lingvaj dosieroj + Monaco redaktoro pako (~38MB renderer).

### Projekta Strukturo
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

## 🙏 Dëgëzë

Këdë kontribucijë bën WIA SOOM më të mirë për zhvilluesit në të gjithë botën.

Pavarësisht nëse korrigjoni një gabim, përktheni një varg, ndërtoni një plugin, ose shtoni një veçori të madhe — **ju jeni pjesë e kësaj historie.**

---

<p align="center"><em>Ndërtuar me ❤️ nga SmileStory Inc. dhe kontribuues të gjithë botës.</em></p>