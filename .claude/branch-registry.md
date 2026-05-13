# Branch Registry

Mantener este archivo actualizado en cada tarea que cree, mergee, o abandone una branch.
La AI corre `git branch -a` al inicio de cada tarea y compara con este registro.
Cualquier discrepancia → parar y reportar al usuario antes de continuar.

## Formato de fecha: YYYY-MM-DD

| Branch | Propósito | Estado | Creada desde | Destino | Fecha creación |
|--------|-----------|--------|--------------|---------|----------------|
| main | Producción | activa | — | — | 2026-05-13 |
| dev | Integración principal | activa | main | main | 2026-05-13 |

## Estados válidos
- `activa` — en uso
- `mergeada` — integrada a su destino, puede borrarse
- `abandonada` — descartada, motivo en columna Propósito
