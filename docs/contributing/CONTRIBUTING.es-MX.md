<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuyendo a WIA SOOM</h1>
<p align="center"><strong>¡Nos encantarían tus contribuciones!</strong></p>
<p align="center">Ya sea una corrección de errores, una nueva función, un plugin o una traducción, cada contribución cuenta.</p>

---

## Tabla de Contenidos

- [Código de Conducta](#código-de-conducta)
- [Cómo Reportar Errores](#-cómo-reportar-errores)
- [Cómo Sugerir Funciones](#-cómo-sugerir-funciones)
- [Cómo Enviar un Plugin](#-cómo-enviar-un-plugin)
- [Cómo Enviar una Solicitud de Extracción](#-cómo-enviar-una-solicitud-de-extracción)
- [Contribuciones de Traducción (254 Idiomas)](#-contribuciones-de-traducción-254-idiomas)
- [Configuración de Desarrollo](#-configuración-de-desarrollo)

---

## Código de Conducta

Estamos comprometidos a proporcionar una experiencia acogedora e inclusiva para todos.

- **Sé respetuoso.** Trata a todos con dignidad.
- **S�� constructivo.** Ofrece comentarios útiles, no críticas destructivas.
- **Sé inclusivo.** Apoyamos 254 idiomas y damos la bienvenida a contribuyentes de todos los países del mundo.
- **No al acoso.** Tolerancia cero a la discriminación de cualquier tipo.

---

## 🐛 Cómo Reportar Errores

1. Ve a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Haz clic en **"New Issue"**
3. Elige la plantilla **"Bug Report"**
4. Incluye:
   - Versión de WIA SOOM (Configuración → Acerca de)
   - SO y versión (Windows/macOS/Linux)
   - Pasos para reproducir
   - Comportamiento esperado vs. real
   - Capturas de pantalla o salida de terminal si es posible

---

## 💡 Cómo Sugerir Funciones

1. Ve a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Haz clic en **"New Issue"**
3. Elige la plantilla **"Feature Request"**
4. Describe:
   - Qué problema estás resolviendo
   - Cómo imaginas que funcionaría
   - Cualquier alternativa que hayas considerado

---

## 🔌 Cómo Enviar un Plugin

WIA SOOM tiene un poderoso sistema de plugins: puedes construir tu propio plugin en 5 minutos.

### Inicio Rápido
§§§CHUNK_SEPARATOR§§§
### Guía Completa

Lee la **[Guía para Desarrolladores de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** para:
- Referencia completa de la API
- Ejemplos de trabajo
- Tutoriales paso a paso
- Mejores prácticas y reglas de seguridad

### Envía Tu Plugin

1. Haz un fork de [Plugin Store](https://wiasoom.com)
2. Agrega tu plugin a `plugins/{tu-nombre-de-plugin}/`
3. Envía una Solicitud de Extracción
4. ¡Después de la revisión, tu plugin aparecerá en la Tienda de Plugins para todos los usuarios!

---

## 🔀 Cómo Enviar una Solicitud de Extracción

### Para la aplicación principal (wia-soom)

1. Haz un fork del repositorio
2. Crea una rama de función: `git checkout -b feat/my-feature`
3. Realiza tus cambios
4. Prueba localmente:
   ```bash
   ```
5. Realiza un commit con un mensaje claro:
   ```
   feat: agregar interruptor de modo oscuro a la configuración
   ```
6. Haz push y abre un PR contra `main`

### Convención de Mensajes de Commit

| Prefijo | Usar para |
|---------|-----------|
| `feat:` | Nueva función |
| `fix:`  | Corrección de errores |
| `docs:` | Solo documentación |
| `refactor:` | Reestructuración de código (sin cambios en el comportamiento) |
| `i18n:` | Actualizaciones de traducción |
| `plugin:` | Cambios relacionados con plugins |

### Lista de Verificación de PR

- [ ] El código se ejecuta sin errores
- [ ] Sin cadenas codificadas (usar claves i18n)
- [ ] Sin `console.log` dejados en el código de producción
- [ ] Las pruebas existentes siguen pasando

---

## 🌐 Contribuciones de Traducción (254 Idiomas)

WIA SOOM soporta **254 idiomas** — desde amhárico hasta zulú, incluyendo braille e idiomas RTL.

### Cómo Funciona la Traducción

- Archivo de idioma base: `src/renderer/src/i18n/en.json`
- Todos los 254 archivos de idioma están en el mismo directorio
- La traducción se realiza a través de `scripts/translate-patch.js` (API GPT-4o-mini)

### Cómo Contribuir con Traducciones

#### Opción 1: Corregir una traducción específica

1. Encuentra el archivo de idioma: `src/renderer/src/i18n/{código-de-idioma}.json`
2. Corrige la traducción incorrecta
3. Envía un PR con el cambio

#### Opción 2: Agregar claves faltantes
§§§CHUNK_SEPARATOR§§§
#### Opción 3: Revisar traducciones automáticas

Muchos de nuestros 254 idiomas fueron traducidos automáticamente. ¡Las revisiones de hablantes nativos son increíblemente valiosas!

1. Elige tu archivo de idioma
2. Revisa las traducciones
3. Corrige cualquier traducción torpe o incorrecta
4. Envía un PR

### Códigos de Idioma

Usamos códigos estándar ISO 639-1 (por ejemplo, `ko`, `en`, `ja`, `ar`, `hi`) con variantes regionales donde sea necesario (por ejemplo, `zh-CN`, `pt-BR`).

---

## 🛠 Configuración de Desarrollo

### Requisitos Previos

- Node.js 18+
- npm 9+
- Git

### Configuración
§§§CHUNK_SEPARATOR§§§
### Compilación
§§§CHUNK_SEPARATOR§§§
> Nota: El heap predeterminado de 2GB no es suficiente debido a los 254 archivos de idioma + paquete del editor Monaco (~38MB de renderizado).

### Estructura del Proyecto
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Gracias

Cada contribución hace que WIA SOOM sea mejor para los desarrolladores de todo el mundo.

Ya sea que corrijas un error tipográfico, traduzcas una cadena, construyas un plugin o agregues una función importante — **eres parte de esta historia.**

---

<p align="center"><em>Construido con ❤️ por SmileStory Inc. y contribuyentes de todo el mundo.</em></p>
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
