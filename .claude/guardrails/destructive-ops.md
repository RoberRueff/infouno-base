last-updated: 2026-05-13

# Guardrail: Operaciones Destructivas

Estas operaciones requieren confirmación explícita del usuario antes de ejecutarse.
Mostrar siempre qué va a pasar y esperar "sí", "ok", "dale" o equivalente.

## Requieren confirmación

| Operación | Qué mostrar antes de ejecutar |
|-----------|-------------------------------|
| `git reset --hard` | Qué commits o cambios se van a perder |
| `git branch -D [branch]` | Nombre de la branch y si tiene cambios sin mergear |
| Borrar archivos | Lista completa de archivos a borrar |
| `git rebase` en rama con remote | Impacto en historial para quien tenga la rama |
| `git commit --amend` en rama con remote | Idem |
| Modificar `.claude/settings.local.json` | Diff del cambio propuesto |

## Formato de confirmación

```
Estoy por [operación].
Consecuencia: [qué se pierde o cambia de forma irreversible].
¿Confirmás? (sí/no)
```

No ejecutar hasta recibir confirmación afirmativa explícita.
