<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ag Cur le Chéile ar WIA SOOM</h1>
<p align="center"><strong>Ba bhreá linn do chuid comhoibrithe!</strong></p>
<p align="center">Cibé an bhfuil sé ina dheisiú bug, gné nua, plug-in, nó aistriúchán — tá tábhacht ag baint le gach comhoibriú.</p>

---

## Clár Ábhar

- [Cód Iompair](#code-of-conduct)
- [Conas Tuairisc a Thabhairt ar Bhuí](#-how-to-report-bugs)
- [Conas Gnéithe a Mholadh](#-how-to-suggest-features)
- [Conas Plug-in a Thabhairt isteach](#-how-to-submit-a-plugin)
- [Conas Iarratas Tarraing a Thabhairt isteach](#-how-to-submit-a-pull-request)
- [Comhoibrithe Aistriúcháin (254 Teanga)](#-translation-contributions-254-languages)
- [Socrú Forbartha](#-development-setup)

---

## Cód Iompair

Táimid tiomanta do sholáthar taithí fáilteach agus cuimsitheach do chách.

- **Bí measúil.** Déan cóir le gach duine.
- **Bí tógálach.** Tairg comhoiriúnachtaí cabhracha, ní criticeadh díobhálach.
- **Bí cuimsitheach.** Tacaímid le 254 teanga agus cuirimid fáilte roimh chomhoibrithe ó gach tír ar domhan.
- **Níl ciapadh.** Níl aon tolra do dhíobháil ar bith.

---

## 🐛 Conas Tuairisc a Thabhairt ar Bhuí

1. Téigh go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliceáil **"New Issue"**
3. Roghnaigh an **"Bug Report"** teimpléad
4. Cuir isteach:
   - Leagan WIA SOOM (Socruithe → Faoin)
   - OS agus leagan (Windows/macOS/Linux)
   - Céimeanna chun a athdhéanamh
   - Iompar atá súil agat vs. iompar iarbhír
   - Grianghraif scáileáin nó aschur téarma más féidir

---

## 💡 Conas Gnéithe a Mholadh

1. Téigh go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliceáil **"New Issue"**
3. Roghnaigh an **"Feature Request"** teimpléad
4. Déan cur síos:
   - Cad é an fadhb atá á réiteach agat
   - Conas a shamhlaíonn tú go n-oibríonn sé
   - Aon roghanna a mheas tú

---

## 🔌 Conas Plug-in a Thabhairt isteach

Tá córas plug-in cumhachtach ag WIA SOOM — is féidir leat do phlug-in féin a thógáil i 5 nóiméad.

### Tús gasta
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Treoir Iomlán

Léigh an **[Treoir do Dhéantóirí Plug-in](docs/PLUGIN_DEVELOPER_GUIDE.md)** le haghaidh:
- Tagairt API iomlán
- Samplaí oibre
- Taispeántais céim ar chéim
- Cleachtais is fearr agus rialacha slándála

### Tabhair isteach do Plug-in

1. Fork [Plugin Store](https://wiasoom.com)
2. Cuir do phlug-in le `plugins/{your-plugin-name}/`
3. Tabhair isteach Iarratas Tarraing
4. Tar éis athbhreithnithe, feicfidh do phlug-in i gCró na bPlug-in do gach úsáideoir!

---

## 🔀 Conas Iarratas Tarraing a Thabhairt isteach

### Don aip phríomha (wia-soom)

1. Fork an stór
2. Cruthaigh craobh ghné: `git checkout -b feat/my-feature`
3. Déan do chuid athruithe
4. Tástáil go háitiúil:
   ```bash
   ```
5. Conradh le teachtaireacht shoiléir:
   ```
   feat: add dark mode toggle to settings
   ```
6. Brúigh agus oscail PR i gcoinne `main`

### Conradh Teachtaireachtaí

| Réamhrá | Úsáidtear le haghaidh |
|--------|---------|
| `feat:` | Gné nua |
| `fix:` | Deisiú bug |
| `docs:` | Doiciméadú amháin |
| `refactor:` | Athstruchtúr cód (gan athrú ar iompar) |
| `i18n:` | Nuashonruithe aistriúcháin |
| `plugin:` | Athruithe a bhaineann le plug-in |

### Liosta Seiceáil PR

- [ ] Rith an cód gan earráidí
- [ ] Níl aon shnáitheanna crua (úsáid eochracha i18n)
- [ ] Níl aon `console.log` fágtha i gcód táirgthe
- [ ] Leanann tástálacha atá ann cheana ag rith

---

## 🌐 Comhoibrithe Aistriúcháin (254 Teanga)

Tacaíonn WIA SOOM le **254 teanga** — ó Amharic go Zulu, ag áireamh Braille agus teangacha RTL.

### Conas a Oibríonn Aistriúchán

- Comhad teanga bun: `src/renderer/src/i18n/en.json`
- Tá gach comhad teanga 254 sa chatalóg céanna
- Déantar aistriúchán trí `scripts/translate-patch.js` (GPT-4o-mini API)

### Conas Comhoibriú le hAistriúcháin

#### Rogha 1: Deisigh aistriúchán ar leith

1. Faigh an comhad teanga: `src/renderer/src/i18n/{lang-code}.json`
2. Deisigh an t-aistriúchán mícheart
3. Tabhair isteach PR leis an athrú

#### Rogha 2: Cuir eochracha atá in easnamh leis
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Rogha 3: Athbhreithniú a dhéanamh ar aistriúcháin meaisín

Bhí go leor dár 254 teangacha aistrithe ag meaisín. Tá athbhreithnithe ó chainteoirí dúchais an-tábhachtach!

1. Roghnaigh do chomhad teanga
2. Athbhreithnigh na haistriúcháin
3. Deisigh aon aistriúcháin awkward nó mícheart
4. Tabhair isteach PR

### Cóid Teanga

Úsáidimid cóid ISO 639-1 caighdeánacha (e.g., `ko`, `en`, `ja`, `ar`, `hi`) le leaganacha réigiúnacha nuair is gá (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Socrú Forbartha

### Réamhchoinníollacha

- Node.js 18+
- npm 9+
- Git

### Socrú
```bash
```
### Tógáil
```bash
```
> Nóta: Ní leor an heap 2GB réamhshocraithe de bharr na gcomhad teanga 254 + pacáiste eagarthóra Monaco (~38MB renderer).

### Struchtúr Tionscadail
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

## 🙏 Go raibh maith agat

Déantar WIA SOOM níos fearr do dhéantóirí ar fud an domhain le gach comhoibriú.

Cibé an gcuireann tú ceartú ar thypo, aistríonn tú sreang, tógann tú plug-in, nó cuireann tú gné mhór leis — **tá tú mar chuid den scéal seo.**

---

<p align="center"><em>Déanta le ❤️ ag SmileStory Inc. agus comhoibrithe ar fud an domhain.</em></p>