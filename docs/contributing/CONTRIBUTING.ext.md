<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuyendo a WIA SOOM</h1>
<p align="center"><strong>¡Nos encantarían tus contribuciones!</strong></p>
<p align="center">Ya sea una corrección de errores, una nueva función, un plugin o una traducción — cada contribución cuenta.</p>

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
- **Sé constructivo.** Ofrece comentarios útiles, no críticas destructivas.
- **Sé inclusivo.** Apoyamos 254 idiomas y damos la bienvenida a contribuyentes de todos los países del mundo.
- **No al acoso.** Tolerancia cero a la discriminación de cualquier tipo.

---

## 🐛 Cómo Reportar Errores

1. Ve a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Haz clic en **"Nueva Problema"**
3. Elige la plantilla **"Informe de Error"**
4. Incluye:
   - Versión de WIA SOOM (Configuración → Acerca de)
   - SO y versión (Windows/macOS/Linux)
   - Pasos para reproducir
   - Comportamiento esperado vs. real
   - Capturas de pantalla o salida de terminal si es posible

---

## 💡 Cómo Sugerir Funciones

1. Ve a [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Haz clic en **"Nueva Problema"**
3. Elige la plantilla **"Solicitud de Función"**
4. Describe:
   - Qué problema estás resolviendo
   - Cómo imaginas que funcionaría
   - Cualquier alternativa que hayas considerado

---

## 🔌 Cómo Enviar un Plugin

WIA SOOM tiene un poderoso sistema de plugins — puedes construir tu propio plugin en 5 minutos.

### Inicio Rápido
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guía Completa

Lee la **[Guía para Desarrolladores de Plugins](docs/PLUGIN_DEVELOPER_GUIDE.md)** para:
- Referencia completa de la API
- Ejemplos funcionales
- Tutoriales paso a paso
- Mejores prácticas y reglas de seguridad

### Envía Tu Plugin

1. Haz un fork de [Plugin Store](https://wiasoom.com)
2. Agrega tu plugin a `plugins/{tu-nombre-de-plugin}/`
3. Envía una Solicitud de Extracción
4. Después de la revisión, ¡tu plugin aparecerá en la Tienda de Plugins para todos los usuarios!

---

## 🔀 Cómo Enviar una Solicitud de Extracción

### Para la aplicación principal (wia-soom)

1. Haz un fork del repositorio
2. Crea una rama de función: `git checkout -b feat/mi-función`
3. Realiza tus cambios
4. Prueba localmente:
   ```bash
   ```
5. Haz un commit con un mensaje claro:
   ```
   feat: añadir interruptor de modo oscuro a la configuración
   ```
6. Haz push y abre un PR contra `main`

### Convención de Mensajes de Commit

| Prefijo | Usar para |
|--------|---------|
| `feat:` | Nueva función |
| `fix:` | Corrección de errores |
| `docs:` | Solo documentación |
| `refactor:` | Reestructuración de código (sin cambio de comportamiento) |
| `i18n:` | Actualizaciones de traducción |
| `plugin:` | Cambios relacionados con plugins |

### Lista de Verificación de PR

- [ ] El código se ejecuta sin errores
- [ ] No hay cadenas codificadas (usar claves i18n)
- [ ] No hay `console.log` en el código de producción
- [ ] Las pruebas existentes aún pasan

---

## 🌐 Contribuciones de Traducción (254 Idiomas)

WIA SOOM soporta **254 idiomas** — desde amhárico hasta zulú, incluyendo braille e idiomas RTL.

### Cómo Funciona la Traducción

- Archivo de idioma base: `src/renderer/src/i18n/en.json`
- Todos los 254 archivos de idioma están en el mismo directorio
- La traducción se realiza a través de `scripts/translate-patch.js` (API GPT-4o-mini)

### Cómo Contribuir Traducciones

#### Opción 1: Corregir una traducción específica

1. Encuentra el archivo de idioma: `src/renderer/src/i18n/{código-de-idioma}.json`
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

Muchos de nuestros 254 idiomas fueron traducidos por máquina. ¡Las revisiones de hablantes nativos son increíblemente valiosas!

1. Elige tu archivo de idioma
2. Revisa las traducciones
3. Corrige cualquier traducción incómoda o incorrecta
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
```bash
```
### Compilación
```bash
```
> Nota: El heap predeterminado de 2GB no es suficiente debido a los 254 archivos de idioma + paquete del editor Monaco (~38MB renderer).

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

## 🙏 Gràcies

Cada contribució fa que WIA SOOM sigui millor per als desenvolupadors de tot el món.

Ja sigui que corregeixes un error tipogràfic, tradueixes una cadena, construeixes un plugin o afegeixes una característica important — **tu formes part d'aquesta història.**

---

<p align="center"><em>Construït amb ❤️ per SmileStory Inc. i col·laboradors de tot el món.</em></p>