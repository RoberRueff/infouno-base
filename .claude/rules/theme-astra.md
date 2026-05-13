last-updated: 2026-05-13

# Rule: theme-astra

## Qué es

Tema base Astra de terceros. Vive en `themes/astra/`.
No es código propio — no se modifica directamente.

## Cómo se trabaja

- El tema Astra NO se modifica directamente (es de terceros, se rompe al actualizar)
- Toda customización visual o funcional va via `plugins/infouno-custom/` usando los
  hooks y filters que Astra expone
- Si se necesita un override de template, crear un child theme y seguir el proceso
  de component-registration antes de tocarlo

## Permitido

- Leer archivos del tema para entender su estructura y hooks disponibles
- Registrar hooks/filters de Astra desde el plugin custom

## No permitido

- Editar cualquier archivo dentro de `themes/astra/` directamente
- Actualizar el tema manualmente (se gestiona desde WP Admin)
- Crear archivos nuevos dentro de `themes/astra/`
