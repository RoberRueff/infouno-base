last-updated: 2026-05-13

# Rule: plugin-infouno-custom

## Qué es

Plugin WordPress custom de infouno. Es el área principal de desarrollo custom.
Vive en `plugins/infouno-custom/`.

## Cómo se trabaja

- Toda la lógica custom del sitio va acá (custom post types, hooks, shortcodes, etc.)
- Seguir convenciones WordPress: hooks y filters, no override de funciones core
- Prefijo obligatorio en funciones y clases: `infouno_` / `Infouno_`
- Un archivo PHP por responsabilidad (no todo en infouno-custom.php)

## Estructura esperada del plugin

```
plugins/infouno-custom/
  infouno-custom.php     ← bootstrap, solo includes y definición del plugin
  includes/              ← lógica organizada por área
```

## Permitido

- Agregar y modificar PHP en `plugins/infouno-custom/`
- Crear subdirectorios dentro del plugin para organizar código
- Registrar hooks y filters sobre Astra o WordPress core

## No permitido

- Modificar plugins de terceros
- Hardcodear URLs, credenciales, o IDs de posts
- Lógica de presentación en archivos de lógica (separar concerns)
