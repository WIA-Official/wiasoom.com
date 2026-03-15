<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuindo para WIA SOOM</h1>
<p align="center"><strong>Adoramos as suas contribuições!</strong></p>
<p align="center">Seja uma correção de bug, uma nova funcionalidade, um plugin ou uma tradução — cada contribuição é importante.</p>

---

## Índice

- [Código de Conduta](#código-de-conduta)
- [Como Reportar Bugs](#-como-reportar-bugs)
- [Como Sugerir Funcionalidades](#-como-sugerir-funcionalidades)
- [Como Submeter um Plugin](#-como-submeter-um-plugin)
- [Como Submeter um Pull Request](#-como-submeter-um-pull-request)
- [Contribuições de Tradução (254 Línguas)](#-contribuições-de-tradução-254-línguas)
- [Configuração de Desenvolvimento](#-configuração-de-desenvolvimento)

---

## Código de Conduta

Estamos comprometidos em proporcionar uma experiência acolhedora e inclusiva para todos.

- **Seja respeitoso.** Trate todos com dignidade.
- **Seja construtivo.** Ofereça feedback útil, não críticas destrutivas.
- **Seja inclusivo.** Apoiamo-nos em 254 línguas e damos as boas-vindas a contribuintes de todos os países da Terra.
- **Sem assédio.** Tolerância zero para discriminação de qualquer tipo.

---

## 🐛 Como Reportar Bugs

1. Vá para [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clique em **"Nova Questão"**
3. Escolha o template **"Relatório de Bug"**
4. Inclua:
   - Versão do WIA SOOM (Configurações → Sobre)
   - SO e versão (Windows/macOS/Linux)
   - Passos para reproduzir
   - Comportamento esperado vs. real
   - Capturas de tela ou saída do terminal, se possível

---

## 💡 Como Sugerir Funcionalidades

1. Vá para [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Clique em **"Nova Questão"**
3. Escolha o template **"Solicitação de Funcionalidade"**
4. Descreva:
   - Que problema você está resolvendo
   - Como você imagina que funcionaria
   - Quaisquer alternativas que você considerou

---

## 🔌 Como Submeter um Plugin

WIA SOOM tem um poderoso sistema de plugins — você pode construir seu próprio plugin em 5 minutos.

### Início Rápido
§§§CHUNK_SEPARATOR§§§
### Guia Completo

Leia o **[Guia do Desenvolvedor de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** para:
- Referência completa da API
- Exemplos funcionais
- Tutoriais passo a passo
- Melhores práticas e regras de segurança

### Submeta Seu Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Adicione seu plugin a `plugins/{seu-nome-de-plugin}/`
3. Submeta um Pull Request
4. Após a revisão, seu plugin aparecerá na Plugin Store para todos os usuários!

---

## 🔀 Como Submeter um Pull Request

### Para o aplicativo principal (wia-soom)

1. Fork o repositório
2. Crie uma branch de funcionalidade: `git checkout -b feat/minha-funcionalidade`
3. Faça suas alterações
4. Teste localmente:
   ```bash
   ```
5. Faça commit com uma mensagem clara:
   ```
   feat: adicionar alternância de modo escuro nas configurações
   ```
6. Faça push e abra um PR contra `main`

### Convenção de Mensagem de Commit

| Prefixo | Uso para |
|---------|----------|
| `feat:` | Nova funcionalidade |
| `fix:` | Correção de bug |
| `docs:` | Somente documentação |
| `refactor:` | Reestruturação de código (sem mudança de comportamento) |
| `i18n:` | Atualizações de tradução |
| `plugin:` | Alterações relacionadas a plugins |

### Checklist do PR

- [ ] O código roda sem erros
- [ ] Sem strings codificadas (use chaves i18n)
- [ ] Sem `console.log` deixados no código de produção
- [ ] Testes existentes ainda passam

---

## 🌐 Contribuições de Tradução (254 Línguas)

WIA SOOM suporta **254 línguas** — do Amárico ao Zulu, incluindo Braille e línguas em RTL.

### Como Funciona a Tradução

- Arquivo de língua base: `src/renderer/src/i18n/en.json`
- Todos os 254 arquivos de língua estão no mesmo diretório
- A tradução é feita via `scripts/translate-patch.js` (API GPT-4o-mini)

### Como Contribuir com Traduções

#### Opção 1: Corrigir uma tradução específica

1. Encontre o arquivo de língua: `src/renderer/src/i18n/{código-lingua}.json`
2. Corrija a tradução incorreta
3. Submeta um PR com a alteração

#### Opção 2: Adicionar chaves faltantes
§§§CHUNK_SEPARATOR§§§
#### Opção 3: Revisar traduções automáticas

Muitas das nossas 254 línguas foram traduzidas automaticamente. Revisões de falantes nativos são incrivelmente valiosas!

1. Escolha seu arquivo de língua
2. Revise as traduções
3. Corrija quaisquer traduções estranhas ou incorretas
4. Submeta um PR

### Códigos de Língua

Usamos códigos padrão ISO 639-1 (por exemplo, `ko`, `en`, `ja`, `ar`, `hi`) com variantes regionais onde necessário (por exemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Configuração de Desenvolvimento

### Pré-requisitos

- Node.js 18+
- npm 9+
- Git

### Configuração
§§§CHUNK_SEPARATOR§§§
### Construção
§§§CHUNK_SEPARATOR§§§
> Nota: O heap padrão de 2GB não é suficiente devido aos 254 arquivos de língua + pacote do editor Monaco (~38MB de renderização).

### Estrutura do Projeto
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Obrigado

Cada contribuição torna WIA SOOM melhor para desenvolvedores ao redor do mundo.

Seja corrigindo um erro de digitação, traduzindo uma string, construindo um plugin, ou adicionando uma funcionalidade importante — **você faz parte desta história.**

---

<p align="center"><em>Construído com ❤️ por SmileStory Inc. e colaboradores em todo o mundo.</em></p>
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
