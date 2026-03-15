<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Ikwuputa na WIA SOOM</h1>
<p align="center"><strong>Anyị chọrọ ka ị tinye aka gị!</strong></p>
<p align="center">Ma ọ bụ ndozi njehie, atụmatụ ọhụrụ, plugin, ma ọ bụ ntụgharị asụsụ — ọ bụla nkwado dị mkpa.</p>

---

## Ndepụta nke ọdịnaya

- [Iwu omume](#code-of-conduct)
- [Otu esi akpọpụta njehie](#-how-to-report-bugs)
- [Otu esi atụ aro atụmatụ](#-how-to-suggest-features)
- [Otu esi nyefee plugin](#-how-to-submit-a-plugin)
- [Otu esi nyefee Pull Request](#-how-to-submit-a-pull-request)
- [Nkwado ntụgharị (254 Asụsụ)](#-translation-contributions-254-languages)
- [Ntọala mmepe](#-development-setup)

---

## Iwu omume

Anyị kwenyere na inye ahụmịhe na-atọ ụtọ na nke gụnyere maka onye ọ bụla.

- **Bụrụ onye na-asọpụrụ.** Kwuo onye ọ bụla n'anya.
- **Bụrụ onye na-eweta ihe.** Nye nzaghachi bara uru, ọ bụghị nkwupụta na-emebi.
- **Bụrụ onye gụnyere.** Anyị na-akwado asụsụ 254 ma na-anabata ndị na-enye aka si mba niile n'ụwa.
- **Enweghị iwe.** Zero tolerance maka ịkpa ókè nke ụdị ọ bụla.

---

## 🐛 Otu esi akpọpụta njehie

1. Gaa na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pịa **"New Issue"**
3. Họrọ **"Bug Report"** template
4. Tinye:
   - WIA SOOM version (Ntọala → Banyere)
   - OS na version (Windows/macOS/Linux)
   - Nzọụkwụ iji mepụta
   - Atụmatụ a tụrụ anya vs. omume n'ezie
   - Ihe oyiyi ma ọ bụ mmepụta terminal ma ọ bụrụ na ọ ga-ekwe omume

---

## 💡 Otu esi atụ aro atụmatụ

1. Gaa na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Pịa **"New Issue"**
3. Họrọ **"Feature Request"** template
4. Kọwaa:
   - Kedu nsogbu ị na-edozi
   - Olee otú ị chere na ọ ga-arụ ọrụ
   - Otu ihe ọ bụla ọzọ ị tụlere

---

## 🔌 Otu esi nyefee plugin

WIA SOOM nwere usoro plugin siri ike — ị nwere ike wuo plugin gị n'ime nkeji 5.

### Mmalite ngwa ngwa
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Ntuziaka zuru ezu

Gụọ **[Plugin Developer Guide](docs/PLUGIN_DEVELOPER_GUIDE.md)** maka:
- Nkọwa API zuru ezu
- Ihe atụ na-arụ ọrụ
- Ntuziaka nzọụkwụ-nzọụkwụ
- Omume kacha mma na iwu nchekwa

### Nyefee Plugin Gị

1. Fork [Plugin Store](https://wiasoom.com)
2. Tinye plugin gị na `plugins/{your-plugin-name}/`
3. Nyefee Pull Request
4. Mgbe nyocha gasịrị, plugin gị ga-apụta na Plugin Store maka ndị ọrụ niile!

---

## 🔀 Otu esi nyefee Pull Request

### Maka ngwa isi (wia-soom)

1. Fork repository
2. Mepụta otu ngalaba atụmatụ: `git checkout -b feat/my-feature`
3. Mee mgbanwe gị
4. Nyochaa na mpaghara:
   ```bash
   ```
5. Kpọọ na ozi doro anya:
   ```
   feat: tinye toggle ọnọdụ ọchịchịrị na ntọala
   ```
6. Push ma mepee PR megide `main`

### Iwu Ozi Commit

| Prefix | Jiri maka |
|--------|---------|
| `feat:` | Atụmatụ ọhụrụ |
| `fix:` | Ndozi njehie |
| `docs:` | Nkọwa naanị |
| `refactor:` | Nchekwa koodu (enweghị mgbanwe omume) |
| `i18n:` | Ntụgharị mmelite |
| `plugin:` | Mgbanwe metụtara plugin |

### Ndepụta PR

- [ ] Koodu na-agba na-enweghị njehie
- [ ] Enweghị ahịrịokwu ndị e dere na koodu (jiri i18n keys)
- [ ] Enweghị `console.log` fọdụrụ na koodu mmepụta
- [ ] Nnwale dị adị ka na-aga n'ihu

---

## 🌐 Nkwado ntụgharị (254 Asụsụ)

WIA SOOM na-akwado **254 asụsụ** — site na Amharic ruo Zulu, gụnyere Braille na asụsụ RTL.

### Otu Ntụgharị Si Eme

- Faịlụ asụsụ bụ isi: `src/renderer/src/i18n/en.json`
- Faịlụ asụsụ 254 niile dị na otu nchekwa
- Ntụgharị na-eme site na `scripts/translate-patch.js` (GPT-4o-mini API)

### Otu esi tinye nkwado ntụgharị

#### Nhọrọ 1: Dozie ntụgharị pụrụ iche

1. Chọta faịlụ asụsụ: `src/renderer/src/i18n/{lang-code}.json`
2. Dozie ntụgharị na-ezighị ezi
3. Nyefee PR na mgbanwe ahụ

#### Nhọrọ 2: Tinye igodo na-adịghị
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Nhọrọ 3: Nyochaa ntụgharị igwe

Ọtụtụ n'ime asụsụ 254 anyị bụ ntụgharị igwe. Nyocha ndị na-asụ asụsụ bụ ihe dị oke mkpa!

1. Họrọ faịlụ asụsụ gị
2. Nyochaa ntụgharị
3. Dozie ntụgharị ọ bụla na-adịghị mma ma ọ bụ na-ezighị ezi
4. Nyefee PR

### Koodu Asụsụ

Anyị na-eji koodu ISO 639-1 nke a na-ahụkarị (dịka, `ko`, `en`, `ja`, `ar`, `hi`) na ụdị mpaghara ebe ọ dị mkpa (dịka, `zh-CN`, `pt-BR`).

---

## 🛠 Ntọala mmepe

### Ihe ndị dị mkpa

- Node.js 18+
- npm 9+
- Git

### Ntọala
```bash
```
### Wuo
```bash
```
> Nkwupụta: 2GB heap ndabara adịghị ezuru n'ihi faịlụ asụsụ 254 + Monaco editor bundle (~38MB renderer).

### Ọrụ Project
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

## 🙏 Daalụ

Nke ọ bụla ị na-enye na-eme WIA SOOM ka mma maka ndị mmepe gburugburu ụwa.

Ma ị na-emezi njehie, na-asụgharị ahịrịokwu, na-ewu plugin, ma ọ bụ tinye atụmatụ dị mkpa — **ị bụ akụkụ nke akụkọ a.**

---

<p align="center"><em>Wụrụ na ❤️ site na SmileStory Inc. na ndị na-enye aka n'ụwa niile.</em></p>