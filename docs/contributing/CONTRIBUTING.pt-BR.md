<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuindo para o WIA SOOM</h1>
<p align="center"><strong>Adoraríamos suas contribuições!</strong></p>
<p align="center">Seja uma correção de bug, um novo recurso, um plugin ou uma tradução — toda contribuição é importante.</p>

---

## Índice

- [Código de Conduta](#código-de-conduta)
- [Como Reportar Bugs](#-como-reportar-bugs)
- [Como Sugerir Recursos](#-como-sugerir-recursos)
- [Como Submeter um Plugin](#-como-submeter-um-plugin)
- [Como Submeter um Pull Request](#-como-submeter-um-pull-request)
- [Contribuições de Tradução (254 Idiomas)](#-contribuições-de-tradução-254-idiomas)
- [Configuração de Desenvolvimento](#-configuração-de-desenvolvimento)

---

## Código de Conduta

Estamos comprometidos em proporcionar uma experiência acolhedora e inclusiva para todos.

- **Seja respeitoso.** Trate todos com dignidade.
- **Seja construtivo.** Ofereça feedback útil, não críticas destrutivas.
- **Seja inclusivo.** Apoiamo-nos em 254 idiomas e recebemos contribuições de todos os países do mundo.
- **Sem assédio.** Tolerância zero para discriminação de qualquer tipo.

---

## 🐛 Como Reportar Bugs

1. Vá para [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clique em **"Nova Issue"**
3. Escolha o modelo **"Relatório de Bug"**
4. Inclua:
   - Versão do WIA SOOM (Configurações → Sobre)
   - SO e versão (Windows/macOS/Linux)
   - Passos para reproduzir
   - Comportamento esperado vs. real
   - Capturas de tela ou saída do terminal, se possível

---

## 💡 Como Sugerir Recursos

1. Vá para [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clique em **"Nova Issue"**
3. Escolha o modelo **"Solicitação de Recurso"**
4. Descreva:
   - Qual problema você está resolvendo
   - Como você imagina que funcionaria
   - Quaisquer alternativas que você considerou

---

## 🔌 Como Submeter um Plugin

O WIA SOOM possui um poderoso sistema de plugins — você pode criar seu próprio plugin em 5 minutos.

### Início Rápido
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guia Completo

Leia o **[Guia do Desenvolvedor de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** para:
- Referência completa da API
- Exemplos funcionais
- Tutoriais passo a passo
- Melhores práticas e regras de segurança

### Submeta Seu Plugin

1. Faça um fork do [Plugin Store](https://wiasoom.com)
2. Adicione seu plugin em `plugins/{seu-nome-de-plugin}/`
3. Submeta um Pull Request
4. Após a revisão, seu plugin aparecerá na Plugin Store para todos os usuários!

---

## 🔀 Como Submeter um Pull Request

### Para o aplicativo principal (wia-soom)

1. Faça um fork do repositório
2. Crie uma branch de recurso: `git checkout -b feat/meu-recurso`
3. Faça suas alterações
4. Teste localmente:
   ```bash
   ```
5. Faça um commit com uma mensagem clara:
   ```
   feat: adicionar alternância de modo escuro nas configurações
   ```
6. Faça push e abra um PR contra `main`

### Convenção de Mensagem de Commit

| Prefixo | Uso para |
|---------|----------|
| `feat:` | Novo recurso |
| `fix:` | Correção de bug |
| `docs:` | Apenas documentação |
| `refactor:` | Reestruturação de código (sem alteração de comportamento) |
| `i18n:` | Atualizações de tradução |
| `plugin:` | Alterações relacionadas a plugins |

### Checklist do PR

- [ ] O código roda sem erros
- [ ] Sem strings hardcoded (use chaves i18n)
- [ ] Sem `console.log` deixados no código de produção
- [ ] Testes existentes ainda passam

---

## 🌐 Contribuições de Tradução (254 Idiomas)

O WIA SOOM suporta **254 idiomas** — do amárico ao zulu, incluindo braille e idiomas RTL.

### Como Funciona a Tradução

- Arquivo de idioma base: `src/renderer/src/i18n/en.json`
- Todos os 254 arquivos de idioma estão no mesmo diretório
- A tradução é feita via `scripts/translate-patch.js` (API GPT-4o-mini)

### Como Contribuir com Traduções

#### Opção 1: Corrigir uma tradução específica

1. Encontre o arquivo de idioma: `src/renderer/src/i18n/{código-do-idioma}.json`
2. Corrija a tradução incorreta
3. Submeta um PR com a alteração

#### Opção 2: Adicionar chaves ausentes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opção 3: Revisar traduções automáticas

Muitos dos nossos 254 idiomas foram traduzidos por máquinas. Revisões de falantes nativos são incrivelmente valiosas!

1. Escolha seu arquivo de idioma
2. Revise as traduções
3. Corrija quaisquer traduções estranhas ou incorretas
4. Submeta um PR

### Códigos de Idioma

Usamos códigos padrão ISO 639-1 (por exemplo, `ko`, `en`, `ja`, `ar`, `hi`) com variantes regionais onde necessário (por exemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Configuração de Desenvolvimento

### Pré-requisitos

- Node.js 18+
- npm 9+
- Git

### Configuração
```bash
```
### Compilação
```bash
```
> Nota: O heap padrão de 2GB não é suficiente devido aos 254 arquivos de idioma + pacote do editor Monaco (~38MB renderer).

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

## 🙏 Obrigado

Cada contribuição torna o WIA SOOM melhor para desenvolvedores ao redor do mundo.

Seja corrigindo um erro de digitação, traduzindo uma string, construindo um plugin ou adicionando um recurso importante — **você faz parte desta história.**

---

<p align="center"><em>Construído com ❤️ pela SmileStory Inc. e colaboradores de todo o mundo.</em></p>