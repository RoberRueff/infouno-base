# Branch Management

## Modelo

```
main     ← producción. Solo recibe merges desde dev con aprobación explícita.
  └── dev     ← integración. AI puede pushear libremente.
        ├── feature/[nombre]   ← funcionalidad nueva
        ├── fix/[nombre]       ← corrección de bug
        └── chore/[nombre]     ← tareas de mantenimiento, config, docs
```

## Naming convention

- Prefijo: `feature/`, `fix/`, o `chore/`
- Nombre: kebab-case, descriptivo, sin fechas
- Ejemplos válidos: `feature/custom-post-types`, `fix/header-mobile`, `chore/update-rules`
- Ejemplos inválidos: `nueva-feature`, `fix1`, `feature/05-13-changes`

## Ciclo de vida de una tarea

1. Verificar rama actual en branch-registry.md
2. Crear branch desde dev: `git checkout -b feature/nombre dev`
3. Agregar al registry antes de empezar
4. Trabajar, commitear, pushear a la branch
5. Correr checks/pre-merge.md
6. Mergear feature → dev: `git checkout dev && git merge feature/nombre`
7. Pushear dev: `git push origin dev`
8. Actualizar estado en registry: `mergeada`
9. Merge dev → main: solo con "sí" explícito del usuario

## Antes de crear una branch nueva

Correr `git branch -a` y revisar branch-registry.md.
Si ya existe una branch para ese propósito → usar la existente, no crear otra.
