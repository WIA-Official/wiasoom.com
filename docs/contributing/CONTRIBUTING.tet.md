<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Kontribuisaun ba WIA SOOM</h1>
<p align="center"><strong>Hau hakarak ita-nia kontribuisaun!</strong></p>
<p align="center">Se liu, se mak bug fix, fitur baru, plugin, ka tradusaun — cada kontribuisaun importante.</p>

---

## Tabela de Conteúdos

- [Kode de Conduta](#kode-de-conduta)
- [Como Relatar Bugs](#-como-relatar-bugs)
- [Como Sugerir Fitur](#-como-sugerir-fitur)
- [Como Submeter um Plugin](#-como-submeter-um-plugin)
- [Como Submeter um Pull Request](#-como-submeter-um-pull-request)
- [Kontribuisaun Tradusaun (254 Línguas)](#-kontribuisaun-tradusaun-254-línguas)
- [Preparasaun Desenvolvimento](#-preparasaun-desenvolvimento)

---

## Kode de Conduta

Hau komitida ba fornese experiensia ne'ebe acolhente no inklusiva ba todos.

- **Se respeitavel.** Trata todos ho dignidade.
- **Se konstrutivu.** Ofere feedback útil, la destrutivu kritika.
- **Se inklusivu.** Hau suporta 254 línguas no acolhe kontribuidore husi cada país iha mundu.
- **La haras.** Toleránsia zero ba diskriminasaun katak.

---

## 🐛 Como Relatar Bugs

1. Ba [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klika **"Nova Questão"**
3. Escolhe o template **"Relatório de Bug"**
4. Inklui:
   - WIA SOOM versão (Configurações → Sobre)
   - SO e versão (Windows/macOS/Linux)
   - Passos ba reproduzir
   - Comportamento esperado vs. real
   - Screenshots ka saída do terminal se possível

---

## 💡 Como Sugerir Fitur

1. Ba [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klika **"Nova Questão"**
3. Escolhe o template **"Pedido de Fitur"**
4. Descreve:
   - Qual problema ita está resolvendo
   - Como ita imagina ne'e funciona
   - Qualquer alternativas ne'ebe ita konsidera

---

## 🔌 Como Submeter um Plugin

WIA SOOM iha sistema plugin forte — ita bele halo ita-nia plugin iha 5 minutos.

### Início Rápido
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guia Completo

Ler o **[Guia do Desenvolvedor de Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** ba:
- Referência API completa
- Exemplos funcionais
- Tutoriais passo-a-passo
- Melhores práticas no regras de segurança

### Submete Ita-Nia Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Adiciona ita-nia plugin ba `plugins/{seu-nome-plugin}/`
3. Submete um Pull Request
4. Depois de revisão, ita-nia plugin aparece iha Plugin Store ba todos os usuários!

---

## 🔀 Como Submeter um Pull Request

### Ba o aplicativo principal (wia-soom)

1. Fork o repositório
2. Cria uma branch de fitur: `git checkout -b feat/minha-fitur`
3. Faz ita-nia mudanças
4. Testa localmente:
   ```bash
   ```
5. Commit ho mensajem klaru:
   ```
   feat: adiciona alternador de modo escuro nas configurações
   ```
6. Push no abre um PR contra `main`

### Convenção de Mensagem de Commit

| Prefixo | Usa para |
|---------|----------|
| `feat:` | Nova fitur |
| `fix:` | Correção de bug |
| `docs:` | Somente documentação |
| `refactor:` | Reestruturação de código (sem mudança de comportamento) |
| `i18n:` | Atualizações de tradução |
| `plugin:` | Mudanças relacionadas a plugin |

### Checklist de PR

- [ ] Código roda sem erros
- [ ] Sem strings codificadas (usa chaves i18n)
- [ ] Sem `console.log` deixado no código de produção
- [ ] Testes existentes ainda passam

---

## 🌐 Kontribuisaun Tradusaun (254 Línguas)

WIA SOOM suporta **254 línguas** — husi Amárico até Zulu, inkluindo Braille no línguas RTL.

### Como a Tradução Funciona

- Arquivo de língua base: `src/renderer/src/i18n/en.json`
- Todos os 254 arquivos de língua estão no mesmo diretório
- A tradução é feita via `scripts/translate-patch.js` (GPT-4o-mini API)

### Como Contribuir com Traduções

#### Opção 1: Corrigir uma tradução específica

1. Encontra o arquivo de língua: `src/renderer/src/i18n/{código-língua}.json`
2. Corrige a tradução incorreta
3. Submete um PR com a mudança

#### Opção 2: Adicionar chaves faltantes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opção 3: Revisar traduções de máquina

Muitas das nossas 254 línguas foram traduzidas por máquina. Revisões de falantes nativos são incrivelmente valiosas!

1. Escolhe o arquivo de língua
2. Revise as traduções
3. Corrige qualquer tradução estranha ou incorreta
4. Submete um PR

### Códigos de Língua

Ita usa códigos padrão ISO 639-1 (por exemplo, `ko`, `en`, `ja`, `ar`, `hi`) com variantes regionais onde necessário (por exemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Preparasaun Desenvolvimento

### Pré-requisitos

- Node.js 18+
- npm 9+
- Git

### Preparasaun
```bash
```
### Compilação
```bash
```
> Nota: O heap padrão de 2GB não é suficiente devido aos 254 arquivos de língua + pacote do editor Monaco (~38MB renderer).

### Estrutura do Projeto
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

Cada kontribisaun torna WIA SOOM liu diak ba desenvolvedores iha todo o mundo.

Se ita halo remata, tradusi uma string, construi uma plugin, ou adicione uma karateristika importante — **ita mak parte husi istoria ne'e.**

---

<p align="center"><em>Construído ho ❤️ husi SmileStory Inc. no kontribuidore sira iha todo o mundo.</em></p>