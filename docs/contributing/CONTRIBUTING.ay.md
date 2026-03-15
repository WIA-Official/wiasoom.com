<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM-ata katkhañani</h1>
<p align="center"><strong>Jikisiñani katkhañani!</strong></p>
<p align="center">Jikisiñani jach'a, nueva característica, plugin ukataki — cada katkhaña suma importancia.</p>

---

## T'ijsiñani

- [Conducta de Código](#code-of-conduct)
- [Jach'a Reportar](#-how-to-report-bugs)
- [Nueva Característica Sugerir](#-how-to-suggest-features)
- [Plugin Submitir](#-how-to-submit-a-plugin)
- [Pull Request Submitir](#-how-to-submit-a-pull-request)
- [Traducción Katkhañani (254 Idiomas)](#-translation-contributions-254-languages)
- [Desarrollo Setup](#-development-setup)

---

## Conducta de Código

Niyaw jach'a, inclusiva ukataki ch'amañani.

- **Respetuñani.** Juk'ampit jach'a ch'amañani.
- **Constructiva.** Jikisiñani ch'amañani, janiw destructiva crítica.
- **Inclusiva.** 254 idiomas apoyani, ukat jach'a jisk'achasiñani jach'a jisk'achasiñani.
- **Janiw acoso.** Janiw discriminar jach'a.

---

## 🐛 Jach'a Reportar

1. Jutir [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Elija el **"Bug Report"** plantilla
4. Incluye:
   - WIA SOOM versión (Configuraciones → Acerca de)
   - SO y versión (Windows/macOS/Linux)
   - Pasos para reproducir
   - Esperado vs. real comportamiento
   - Capturas de pantalla o salida de terminal si es posible

---

## 💡 Nueva Característica Sugerir

1. Jutir [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Click **"New Issue"**
3. Elija el **"Feature Request"** plantilla
4. Describe:
   - ¿Qué problema estás resolviendo?
   - ¿Cómo imaginas que funcione?
   - Cualquier alternativa que hayas considerado

---

## 🔌 Plugin Submitir

WIA SOOM tiene un poderoso sistema de plugins — puedes construir tu propio plugin en 5 minutos.

### Inicio Rápido
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guía Completa

Lee la **[Guía del Desarrollador de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** para:
- Referencia completa de la API
- Ejemplos funcionales
- Tutoriales paso a paso
- Mejores prácticas y reglas de seguridad

### Envía Tu Plugin

1. Fork [Plugin Store](https://wiasoom.com)
2. Agrega tu plugin a `plugins/{your-plugin-name}/`
3. Envía un Pull Request
4. Después de la revisión, tu plugin aparecerá en el Plugin Store para todos los usuarios!

---

## 🔀 Pull Request Submitir

### Para la aplicación principal (wia-soom)

1. Fork el repositorio
2. Crea una rama de característica: `git checkout -b feat/my-feature`
3. Realiza tus cambios
4. Prueba localmente:
   ```bash
   ```
5. Confirma con un mensaje claro:
   ```
   feat: agregar interruptor de modo oscuro a configuraciones
   ```
6. Empuja y abre un PR contra `main`

### Convención de Mensaje de Confirmación

| Prefijo | Uso para |
|--------|---------|
| `feat:` | Nueva característica |
| `fix:` | Corrección de errores |
| `docs:` | Solo documentación |
| `refactor:` | Reestructuración de código (sin cambio de comportamiento) |
| `i18n:` | Actualizaciones de traducción |
| `plugin:` | Cambios relacionados con plugins |

### Checklist de PR

- [ ] El código se ejecuta sin errores
- [ ] Sin cadenas codificadas (usar claves i18n)
- [ ] Sin `console.log` en el código de producción
- [ ] Las pruebas existentes aún pasan

---

## ���� Traducción Katkhañani (254 Idiomas)

WIA SOOM apoya **254 idiomas** — desde Amárico hasta Zulú, incluyendo Braille y idiomas RTL.

### Cómo Funciona la Traducción

- Archivo de idioma base: `src/renderer/src/i18n/en.json`
- Todos los archivos de idioma 254 están en el mismo directorio
- La traducción se realiza a través de `scripts/translate-patch.js` (GPT-4o-mini API)

### Cómo Contribuir Traducciones

#### Opción 1: Corregir una traducción específica

1. Encuentra el archivo de idioma: `src/renderer/src/i18n/{lang-code}.json`
2. Corrige la traducción incorrecta
3. Envía un PR con el cambio

#### Opción 2: Agregar claves faltantes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Opción 3: Revisar traducciones automáticas

Muchos de nuestros 254 idiomas fueron traducidos automáticamente. ¡Las revisiones de hablantes nativos son increíblemente valiosas!

1. Elige tu archivo de idioma
2. Revisa las traducciones
3. Corrige cualquier traducción incómoda o incorrecta
4. Envía un PR

### Códigos de Idioma

Usamos códigos estándar ISO 639-1 (por ejemplo, `ko`, `en`, `ja`, `ar`, `hi`) con variantes regionales donde sea necesario (por ejemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Desarrollo Setup

### Requisitos Previos

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Construcción
```bash
```
> Nota: El montón predeterminado de 2GB no es suficiente debido a los 254 archivos de idioma + paquete del editor Monaco (~38MB renderer).

### Estructura del Proyecto
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

## 🙏 Yuspagara

Kunjam qillqayki, qillqayki, plugin uñt'ayki, ukat jach'a ch'amañchayki — **jumanaka ukhamarak jach'a qillqatanaka.**

---

<p align="center"><em>❤️ ch'amañchayki SmileStory Inc. ukat jach'a qillqatanaka.</em></p>