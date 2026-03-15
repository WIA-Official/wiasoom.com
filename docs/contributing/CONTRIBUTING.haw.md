<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ke Kipa i ka WIA SOOM</h1>
<p align="center"><strong>Makemake mākou i nā kipa a kāu!</strong></p>
<p align="center">Inā he hoʻoponopono ʻino, ʻano hou, plugin, a i ʻole ka unuhi — ʻo nā kipa a pau he mea koʻikoʻi.</p>

---

## Pākuʻi o nā ʻĀkau

- [Ke Kānāwai o ka Hana](#code-of-conduct)
- [Pehea e Kipa ai i nā ʻino](#-how-to-report-bugs)
- [Pehea e Kipa ai i nā ʻano](#-how-to-suggest-features)
- [Pehea e Kipa ai i kahi Plugin](#-how-to-submit-a-plugin)
- [Pehea e Kipa ai i kahi Pull Request](#-how-to-submit-a-pull-request)
- [Nā Kipa Unuhi (254 Nā ʻŌlelo)](#-translation-contributions-254-languages)
- [Ke Kumu Hana](#-development-setup)

---

## Ke Kānāwai o ka Hana

Ua hoʻokomo mākou i ka hāʻawi ʻana i kahi ʻike hoʻokipa a me nā mea e komo pū ana no nā mea a pau.

- **E hoʻomaikaʻi.** E mālama i nā kanaka me ka hoʻohanohano.
- **E hoʻokumu.** E hāʻawi i nā manaʻo kōkua, ʻaʻole i nā manaʻo ʻino.
- **E komo pū.** E kākoʻo mākou i nā ʻōlelo 254 a me nā kipa mai nā ʻāina a pau o ka honua.
- **Aia nō ka hoʻohaʻāla.** ʻAʻohe hoʻohaʻāla o nā ʻano ʻē aʻe.

---

## 🐛 Pehea e Kipa ai i nā ʻino

1. E hele i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. E koho i **"New Issue"**
3. E koho i ka **"Bug Report"** template
4. E komo i:
   - WIA SOOM version (Settings → About)
   - OS a me ka version (Windows/macOS/Linux)
   - Nā hana e hoʻokumu
   - Nā manaʻo e manaʻo ʻia a me nā mea i hana ʻia
   - Nā kiʻi a i ʻole nā ​​nūpepa ke hiki

---

## 💡 Pehea e Kipa ai i nā ʻano

1. E hele i [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. E koho i **"New Issue"**
3. E koho i ka **"Feature Request"** template
4. E wehewehe:
   - I ka mea e hoʻoponopono ana ʻoe
   - Pehea ʻoe e manaʻo ai e hana
   - Nā koho ʻē aʻe i manaʻo ʻia

---

## 🔌 Pehea e Kipa ai i kahi Plugin

He ʻōnaehana plugin ikaika ko WIA SOOM — hiki iā ʻoe ke kūkulu i kāu plugin i loko o 5 mau minuke.

### Hoʻomaka Pōkole
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Kumu Pono

E heluhelu i ka **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** no:
- Ka ʻike API piha
- Nā laʻana e hana ana
- Nā tutorial i nā lā
- Nā hana maikaʻi a me nā kuleana palena

### E Kipa i kāu Plugin

1. E hoʻoikaika i [Plugin Store](https://wiasoom.com)
2. E hoʻohui i kāu plugin i `plugins/{your-plugin-name}/`
3. E Kipa i kahi Pull Request
4. Ma hope o ka nānā ʻana, e ʻike ʻia kāu plugin i loko o ka Plugin Store no nā mea hoʻohana a pau!

---

## 🔀 Pehea e Kipa ai i kahi Pull Request

### No ka polokalamu kūloko (wia-soom)

1. E hoʻoikaika i ka repository
2. E kūkulu i kahi lāʻau ʻano: `git checkout -b feat/my-feature`
3. E hana i kāu mau hoʻololi
4. E hoʻāʻo i loko:
   ```bash
   ```
5. E hoʻokomo me kahi ʻōlelo maʻalahi:
   ```
   feat: add dark mode toggle to settings
   ```
6. E hoʻoikaika a me ka wehe i kahi PR i ka `main`

### Kānāwai O nā ʻŌlelo Kipa

| Prefix | E hoʻohana no |
|--------|---------|
| `feat:` | ʻAno hou |
| `fix:` | Hoʻoponopono ʻino |
| `docs:` | No nā palena ʻike wale nō |
| `refactor:` | Hoʻololi i ka code (ʻaʻohe hoʻololi i ka hana) |
| `i18n:` | Nā hoʻololi unuhi |
| `plugin:` | Nā hoʻololi e pili ana i nā plugin |

### PR Checklist

- [ ] E holo ana ka code me nā hemahema
- [ ] ʻAʻohe mau ʻōlelo i hoʻokomo (e hoʻohana i nā ki i18n)
- [ ] ʻAʻohe `console.log` i waiho i loko o ka code o ka hana
- [ ] E noho ana nā hoʻāʻo i hoʻokumu ʻia

---

## 🌐 Nā Kipa Unuhi (254 Nā ʻŌlelo)

Ke kākoʻo nei ʻo WIA SOOM i **254 nā ʻōlelo** — mai Amharic a Zulu, me nā ʻōlelo Braille a me nā ʻōlelo RTL.

### Pehea e Hana ai ka Unuhi

- Kahi ʻōlelo kumu: `src/renderer/src/i18n/en.json`
- ʻO nā faila ʻōlelo 254 a pau i loko o ke aupuni hoʻokahi
- Hana ʻia ka unuhi ma ke ala `scripts/translate-patch.js` (GPT-4o-mini API)

### Pehea e Kipa ai i nā Unuhi

#### ʻŌlelo 1: Hoʻoponopono i kahi unuhi kūikawā

1. E ʻike i ka faila ʻōlelo: `src/renderer/src/i18n/{lang-code}.json`
2. E hoʻoponopono i ka unuhi hewa
3. E Kipa i kahi PR me ka hoʻololi

#### ʻŌlelo 2: E hoʻohui i nā ki i loaʻa ʻole
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### ʻŌlelo 3: E nānā i nā unuhi mīkini

He nui nā ʻōlelo 254 i unuhi ʻia e nā mīkini. ʻO nā nānā ʻana o nā mea ʻōlelo kūloko he mea koʻikoʻi!

1. E koho i kāu faila ʻōlelo
2. E nānā i nā unuhi
3. E hoʻoponopono i nā unuhi ʻino a i ʻole hewa
4. E Kipa i kahi PR

### Nā Koodu ʻŌlelo

Hoʻohana mākou i nā koodu ISO 639-1 maʻamau (e like me, `ko`, `en`, `ja`, `ar`, `hi`) me nā ʻano kūloko ke pono (e like me, `zh-CN`, `pt-BR`).

---

## 🛠 Ke Kumu Hana

### Nā Pono Pono

- Node.js 18+
- npm 9+
- Git

### Hoʻonohonoho
```bash
```
### Kumu
```bash
```
> ʻO ka ʻike: ʻAʻohe kūpono o ka 2GB heap no nā faila ʻōlelo 254 + Monaco editor bundle (~38MB renderer).

### Ke Structure o ka Pākīpika
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

## 🙏 Mahalo

He mea koʻikoʻi kēlā me kēia koho e hoʻomaikaʻi i ka WIA SOOM no nā mea hoʻomohala ma nā mokupuni a pau.

Inā e hoʻoponopono ʻoe i kahi typo, e unuhi i kahi ʻōlelo, e kūkulu i kahi plugin, a i ʻole e hoʻohui i kahi hiʻohiʻona koʻikoʻi — **ʻo ʻoe ka ʻāpana o kēia moʻolelo.**

---

<p align="center"><em>Ua kūkulu ʻia me ❤️ e SmileStory Inc. a me nā mea hāʻawi ma nā mokupuni a pau.</em></p>