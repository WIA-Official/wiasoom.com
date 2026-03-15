<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribuindo pa WIA SOOM</h1>
<p align="center"><strong>Nos ta gosta di se kontribusons!</strong></p>
<p align="center">Se é un correção di bug, nova função, plugin, ou tradução — cada kontribuson é importante.</p>

---

## Tabela di Conteúdu

- [Kode di Conduta](#kode-di-conduta)
- [Komo Reporta Bugs](#-komo-reporta-bugs)
- [Komo Sugeri Funções](#-komo-sugeri-funções)
- [Komo Submete un Plugin](#-komo-submete-un-plugin)
- [Komo Submete un Pull Request](#-komo-submete-un-pull-request)
- [Kontribusons di Tradução (254 Línguas)](#-kontribusons-di-tradução-254-línguas)
- [Setup di Desenvolvimento](#-setup-di-desenvolvimento)

---

## Kode di Conduta

Nos ta kometi pa ofrese un experiência di boas-vindas e inklusiva pa tudu.

- **Sê respeitoso.** Trata tudu ku dignidade.
- **Sê konstrutivo.** Ofere feedback útil, não crítica destrutiva.
- **Sê inklusivo.** Nos suporta 254 línguas e da boas-vindas pa kontribudors di tudu país na Terra.
- **Nha assédio.** Zero tolerância pa diskriminação di kualker tipo.

---

## 🐛 Komo Reporta Bugs

1. Bô vai na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clica **"New Issue"**
3. Escolhe o template di **"Bug Report"**
4. Inklui:
   - versão di WIA SOOM (Configurações → Sobre)
   - SO e versão (Windows/macOS/Linux)
   - Passus pa reproduzi
   - Comportamentu esperadu vs. real
   - Screenshots ou saída di terminal se possível

---

## 💡 Komo Sugeri Funções

1. Bô vai na [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clica **"New Issue"**
3. Escolhe o template di **"Feature Request"**
4. Descreve:
   - Kual problema bô ta resolvi
   - Komo bô imagina ki ta funciona
   - Kualker alternativas ki bô já considerá

---

## 🔌 Komo Submete un Plugin

WIA SOOM ten un sistema di plugin poderosu — bô pode konstrui bô própi plugin na 5 minutos.

### Início Rápidu
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guia Completa

Lê o **[Guia di Desenvolvedor di Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** pa:
- Referência di API completa
- Exemplus di trabadju
- Tutoriais passo-a-passo
- Melhores práticas e regras di segurança

### Submete Bô Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Adiciona bô plugin na `plugins/{seu-nome-di-plugin}/`
3. Submete un Pull Request
4. Dpois di revisão, bô plugin ta aparese na Plugin Store pa tudu usuários!

---

## 🔀 Komo Submete un Pull Request

### Pa o app principal (wia-soom)

1. Fork o repositório
2. Krea un branch di função: `git checkout -b feat/my-feature`
3. Faz bô mudanças
4. Testa lokalmente:
   ```bash
   ```
5. Commit ku un mensagem klaru:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push e abre un PR kontra `main`

### Convenção di Mensagem di Commit

| Prefixo | Usar pa |
|---------|---------|
| `feat:` | Nova função |
| `fix:`  | Correção di bug |
| `docs:` | Somente documentação |
| `refactor:` | Reestruturação di código (sem mudança di comportamento) |
| `i18n:` | Atualizações di tradução |
| `plugin:` | Mudanças relacionadas a plugin |

### Checklist di PR

- [ ] Código ta roda sem erros
- [ ] Nha strings hardcoded (usa chaves i18n)
- [ ] Nha `console.log` deixadu na código di produção
- [ ] Testes existentes ainda ta passa

---

## 🌐 Kontribusons di Tradução (254 Línguas)

WIA SOOM suporta **254 línguas** — di Amárico a Zulu, inkluindo Braille e línguas RTL.

### Komo Tradução Ta Funciona

- Arquivu di língua base: `src/renderer/src/i18n/en.json`
- Tudu 254 arquivos di língua ta na mesmu diretório
- Tradução é feita via `scripts/translate-patch.js` (GPT-4o-mini API)

### Komo Kontribui Traduções

#### Opção 1: Corrigi un tradução específica

1. Encontra o arquivo di língua: `src/renderer/src/i18n/{lang-code}.json`
2. Corrigi a tradução incorreta
3. Submete un PR ku mudança

#### Opção 2: Adiciona chaves faltantes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opção 3: Revê traduções di máquina

Muitas di nos 254 línguas foram traduzidas por máquina. Revisões di falantes nativos são incrivelmente valiosas!

1. Escolhe bô arquivo di língua
2. Revê as traduções
3. Corrigi kualker traduções estranhas ou incorretas
4. Submete un PR

### Códigos di Língua

Nos usa códigos padrão ISO 639-1 (ex: `ko`, `en`, `ja`, `ar`, `hi`) ku variantes regionais onde necessário (ex: `zh-CN`, `pt-BR`).

---

## 🛠 Setup di Desenvolvimento

### Pré-requisitos

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Nota: O heap padrão di 2GB não é suficiente devido aos 254 arquivos di língua + pacote di editor Monaco (~38MB renderer).

### Estrutura di Projeto
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

## 🙏 Obrigadu

Cada kontribuçon ta faze WIA SOOM más bon pa desenvolvedores ao redor di mundu.

Se bu ta corrigi un erro, tradusi un string, konstrui un plugin, ou adici un fitur major — **bu é parte di sta história.**

---

<p align="center"><em>Konstrudidu ku ❤️ pa SmileStory Inc. i kontribidore di tudu mundu.</em></p>