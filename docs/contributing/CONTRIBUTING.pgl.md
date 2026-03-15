<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ag Contriú do WIA SOOM</h1>
<p align="center"><strong>Ba mhaith linn do chiontachtaí!</strong></p>
<p align="center">Cibé an bhfuil sé ina dheisiú bug, gné nua, plugain, nó aistriúchán — tá gach ciontacht tábhachtach.</p>

---

## Tábla Ábhair

- [Cód Iompair](#code-of-conduct)
- [Conas Tuairisc a Thabhairt ar Bhuí](#-how-to-report-bugs)
- [Conas Gnéithe a Mholadh](#-how-to-suggest-features)
- [Conas Plugain a Chur isteach](#-how-to-submit-a-plugin)
- [Conas Iarratas Pull a Chur isteach](#-how-to-submit-a-pull-request)
- [Ciontachtaí Aistriúcháin (254 Teangacha)](#-translation-contributions-254-languages)
- [Socruithe Forbartha](#-development-setup)

---

## Cód Iompair

Táimid tiomanta do sholáthar taithí fáilteach agus chuimsitheach do gach duine.

- **Bí measúil.** Déan cóir le gach duine.
- **Bí tógálach.** Tairg aiseolas cabhrach, ní criticeoireacht scriosta.
- **Bí chuimsitheach.** Tacaímid le 254 teanga agus cuirimid fáilte roimh chiontóirí ó gach tír ar an Domhan.
- **Níl ciapadh.** Níl aon tolra do dhíobháil ar bith.

---

## 🐛 Conas Tuairisc a Thabhairt ar Bhuí

1. Téigh go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliceáil **"New Issue"**
3. Roghnaigh an teimpléad **"Bug Report"**
4. Cuir isteach:
   - leagan WIA SOOM (Socruithe → Faoi)
   - OS agus leagan (Windows/macOS/Linux)
   - Céimeanna chun a athdhéanamh
   - Iompraíocht atá súil agat vs. iompraíocht atá ann i ndáiríre
   - Grianghraif scáileáin nó aschur téarma más féidir

---

## 💡 Conas Gnéithe a Mholadh

1. Téigh go [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliceáil **"New Issue"**
3. Roghnaigh an teimpléad **"Feature Request"**
4. Déan cur síos:
   - Cad é an fadhb atá tú ag réiteach
   - Conas a shamhlaíonn tú go n-oibreoidh sé
   - Aon roghanna a mheas tú

---

## 🔌 Conas Plugain a Chur isteach

Tá córas plugain cumhachtach ag WIA SOOM — is féidir leat do phugain féin a thógáil i 5 nóiméad.

### Tús Tapa
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Treoir Iomlán

Léigh an **[Treoir do Chiontóir�� Plugain](docs/PLUGIN_DEVELOPER_GUIDE.md)** le haghaidh:
- Tagairt API iomlán
- Samplaí oibre
- Ceachtanna céim ar chéim
- Cleachtais is fearr agus rialacha slándála

### Cuir do Phugain isteach

1. Fork [Plugin Store](https://wiasoom.com)
2. Cuir do phugain le `plugins/{your-plugin-name}/`
3. Cuir isteach Iarratas Pull
4. Tar éis athbhreithnithe, feicfidh do phugain i Stór Plugain do gach úsáideoir!

---

## 🔀 Conas Iarratas Pull a Chur isteach

### Don aip phríomha (wia-soom)

1. Fork an stór
2. Cruthaigh craobh ghné: `git checkout -b feat/my-feature`
3. Déan do chuid athruithe
4. Tástáil go háitiúil:
   ```bash
   ```
5. Comhoiriún le teachtaireacht shoiléir:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push agus oscail PR i gcoinne `main`

### Conradh Teachtaireachta

| Réamhrá | Úsáidtear le haghaidh |
|--------|---------|
| `feat:` | Gné nua |
| `fix:` | Deisiú bug |
| `docs:` | Doiciméadacht amháin |
| `refactor:` | Athstruchtúrú cód (gan athrú iompraíochta) |
| `i18n:` | Nuashonruithe aistriúcháin |
| `plugin:` | Athruithe a bhaineann le plugain |

### Liosta Seiceála PR

- [ ] Ritheann an cód gan earr��idí
- [ ] Níl aon sreangáin chrua (úsáid eochracha i18n)
- [ ] Níl `console.log` fágtha i gcód táirgthe
- [ ] Tástálacha atá ann fós ag pas

---

## 🌐 Ciontachtaí Aistriúcháin (254 Teangacha)

Tacaíonn WIA SOOM le **254 teanga** — ó Amharic go Zulu, lena n-áirítear Braille agus teangacha RTL.

### Conas a Oibríonn Aistriúchán

- Comhad teanga bun: `src/renderer/src/i18n/en.json`
- Tá gach comhad teanga 254 sa chatalóg céanna
- Déantar aistriúchán trí `scripts/translate-patch.js` (GPT-4o-mini API)

### Conas Ciontachtaí Aistriúcháin a Chur isteach

#### Rogha 1: Deisigh aistriúchán ar leith

1. Faigh an comhad teanga: `src/renderer/src/i18n/{lang-code}.json`
2. Deisigh an t-aistriúchán mícheart
3. Cuir isteach PR leis an athrú

#### Rogha 2: Cuir eochracha atá in easnamh leis
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Rogha 3: Athbhreithniú a dhéanamh ar aistriúcháin meaisín

Bhí go leor dár 254 teangacha aistrithe ag meaisín. Tá athbhreithnithe ó labhraoirí dúchais an-tábhachtach!

1. Roghnaigh do chomhad teanga
2. Athbhreithnigh na haistriúcháin
3. Deisigh aon aistriúcháin awkward nó mícheart
4. Cuir isteach PR

### Códanna Teanga

Úsáidimid códanna ISO 639-1 caighdeánacha (m.sh., `ko`, `en`, `ja`, `ar`, `hi`) le leaganacha réigiúnacha nuair is gá (m.sh., `zh-CN`, `pt-BR`).

---

## 🛠 Socruithe Forbartha

### Réamhchoinníollacha

- Node.js 18+
- npm 9+
- Git

### Socruithe
```bash
```
### Tógáil
```bash
```
> Nóta: Níl an heap 2GB réamhshocraithe go leor de bharr na gcomhad teanga 254 + pacáiste eagarthóra Monaco (~38MB renderer).

### Struchtúr an Tionscadail
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

## 🙏 Go Raibh Maith Agat

Gach comhoibriú a dhéanann WIA SOOM níos fearr do dhéantóirí ar fud an domhain.

Cibé an gcuireann tú ceartú ar earráid, aistríonn tú sreang, tógann tú breiseán, nó cuireann tú gné mhór leis — **tá tú mar chuid den scéal seo.**

---

<p align="center"><em>Déanta le ❤️ ag SmileStory Inc. agus comhoibrithe ar fud an domhain.</em></p>