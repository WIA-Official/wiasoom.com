<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuíndo a WIA SOOM</h1>
<p align="center"><strong>Encántannos as túas contribucións!</strong></p>
<p align="center">Se se trata dun erro, unha nova función, un complemento ou unha tradución — cada contribución conta.</p>

---

## Contido

- [Código de Conducta](#código-de-conduta)
- [Como Informar Erros](#-como-informar-erros)
- [Como Suxerir Funcións](#-como-suxerir-funcións)
- [Como Presentar un Complemento](#-como-presentar-un-complemento)
- [Como Presentar unha Solicitude de Extracción](#-como-presentar-unha-solicitude-de-extracción)
- [Contribucións de Tradución (254 Linguas)](#-contribucións-de-tradución-254-linguas)
- [Configuración do Desenvolvemento](#-configuración-do-desenvolvemento)

---

## Código de Conducta

Comprometémonos a proporcionar unha experiencia acolledora e inclusiva para todos.

- **Se respectuoso.** Tratar a todos con dignidade.
- **Se construtivo.** Ofrecer comentarios útiles, non críticas destructivas.
- **Se inclusivo.** Apoiamo 254 linguas e damos a benvida a contribuidores de todos os países do mundo.
- **Sen acoso.** Tolerancia cero para a discriminación de calquera tipo.

---

## 🐛 Como Informar Erros

1. Vai a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Fai clic en **"Nova Cuestión"**
3. Elixe a plantilla **"Informe de Erro"**
4. Inclúe:
   - Versión de WIA SOOM (Configuración → Acerca de)
   - SO e versión (Windows/macOS/Linux)
   - Pasos para reproducir
   - Comportamento esperado vs. real
   - Capturas de pantalla ou saída do terminal se é posible

---

## 💡 Como Suxerir Funcións

1. Vai a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Fai clic en **"Nova Cuestión"**
3. Elixe a plantilla **"Solicitude de Función"**
4. Describe:
   - Que problema estás a resolver
   - Como imaginas que funcionaría
   - Calquera alternativa que consideraches

---

## 🔌 Como Presentar un Complemento

WIA SOOM ten un potente sistema de complementos — podes crear o teu propio complemento en 5 minutos.

### Comezo Rápido
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guía Completa

Ler a **[Guía do Desenvolvedor de Complementos](docs/PLUGIN_DEVELOPER_GUIDE.md)** para:
- Referencia completa da API
- Exemplos de traballo
- Tutoriais paso a paso
- Melores prácticas e regras de seguridade

### Presenta o Teu Complemento

1. Fai un fork de [Plugin Store](https://wiasoom.com)
2. Engade o teu complemento a `plugins/{your-plugin-name}/`
3. Presenta unha Solicitude de Extracción
4. Despois da revisión, o teu complemento aparecerá na Tienda de Complementos para todos os usuarios!

---

## 🔀 Como Presentar unha Solicitude de Extracción

### Para a aplicación principal (wia-soom)

1. Fai un fork do repositorio
2. Crea unha rama de función: `git checkout -b feat/my-feature`
3. Fai os teus cambios
4. Proba localmente:
   ```bash
   ```
5. Fai un commit cunha mensaxe clara:
   ```
   feat: engadir alternador de modo escuro a configuracións
   ```
6. Fai push e abre un PR contra `main`

### Convención de Mensaxes de Commit

| Prefixo | Uso para |
|---------|----------|
| `feat:` | Nova función |
| `fix:`  | Corrección de erros |
| `docs:` | Só documentación |
| `refactor:` | Reestruturación de código (sen cambio de comportamento) |
| `i18n:` | Actualizacións de tradución |
| `plugin:` | Cambios relacionados co complemento |

### Lista de Verificación do PR

- [ ] O código execútase sen erros
- [ ] Sen cadeas codificadas (usa chaves i18n)
- [ ] Sen `console.log` deixados no código de produción
- [ ] As probas existentes seguen pasando

---

## 🌐 Contribucións de Tradución (254 Linguas)

WIA SOOM soporta **254 linguas** — desde amárico ata zulú, incluíndo braille e linguas RTL.

### Como Funciona a Tradución

- Arquivo de lingua base: `src/renderer/src/i18n/en.json`
- Todos os 254 arquivos de lingua están na mesma carpeta
- A tradución faise a través de `scripts/translate-patch.js` (API GPT-4o-mini)

### Como Contribuír con Traducións

#### Opción 1: Corrixir unha tradución específica

1. Atopa o arquivo de lingua: `src/renderer/src/i18n/{lang-code}.json`
2. Corrixe a tradución incorrecta
3. Presenta un PR co cambio

#### Opción 2: Engadir chaves que falten
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opción 3: Revisar traducións automáticas

Moitas das nosas 254 linguas foron traducidas por máquina. As revisións de falantes nativos son incrible valiosas!

1. Escolla o seu arquivo de lingua
2. Revisa as traducións
3. Corrixe calquera tradución incómoda ou incorrecta
4. Presenta un PR

### Códigos de Lingua

Usamos códigos estándar ISO 639-1 (por exemplo, `ko`, `en`, `ja`, `ar`, `hi`) con variantes regionais onde sexa necesario (por exemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Configuración do Desenvolvemento

### Requisitos Previos

- Node.js 18+
- npm 9+
- Git

### Configuración
```bash
```
### Compilación
```bash
```
> Nota: A memoria heap predeterminada de 2GB non é suficiente debido aos 254 arquivos de lingua + paquete do editor Monaco (~38MB renderer).

### Estructura do Proxecto
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

## 🙏 Grazas

Cada contribución fai que WIA SOOM sexa mellor para os desenvolvedores de todo o mundo.

Se arranxas un erro tipográfico, traduces unha cadea, construíres un plugin ou engades unha característica importante — **ti es parte desta historia.**

---

<p align="center"><em>Construído con ❤️ por SmileStory Inc. e contribuidores de todo o mundo.</em></p>