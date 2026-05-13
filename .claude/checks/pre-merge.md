# Check: Pre-Merge

Ejecutar antes de mergear cualquier branch, sin excepción.

## Checklist

- [ ] Todos los archivos modificados son relevantes para la tarea declarada
- [ ] No hay archivos de debug, logs temporales, ni `.DS_Store` en el commit
- [ ] Si se tocó `rules/` o `guardrails/`: campo `last-updated` actualizado en cada archivo
- [ ] Si se tocó `rules/` o `guardrails/`: commit message incluye nombre del archivo + fecha
- [ ] `branch-registry.md` refleja el estado actual de la branch (activa/mergeada/abandonada)
- [ ] El merge target es el correcto (feature/* → dev, nunca feature/* → main directamente)

## Si algún ítem falla

No mergear. Reportar al usuario qué ítem falló y por qué.
