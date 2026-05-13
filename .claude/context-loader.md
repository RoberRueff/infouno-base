# Context Loader

Protocolo obligatorio al inicio de cada tarea. Seguir en orden.
Si no podés completar alguna sección → pausar y reportar antes de continuar.

---

## 1. ORIENTACIÓN

- [ ] Leer `.claude/taxonomy.md`
- [ ] Leer `.claude/branch-registry.md`
- [ ] Correr `git branch -a` y comparar con branch-registry.md
  - Si hay ramas en git que no están en el registry → PARAR y reportar
  - Si hay ramas en el registry que no existen en git → PARAR y reportar
- [ ] Identificar rama actual: `git branch --show-current`

## 2. IDENTIFICAR ÁREA DE TRABAJO

Según lo que pide la tarea:

- ¿Toca el plugin? → leer `.claude/rules/plugin-infouno-custom.md`
- ¿Toca el tema? → leer `.claude/rules/theme-astra.md`
- ¿Es una operación de branches o git? → leer `.claude/templates/branch.md`
- ¿Se introduce un componente nuevo? → leer `.claude/templates/component-registration.md`

## 3. CARGAR GUARDRAILS (siempre, sin excepción)

- [ ] Leer `.claude/guardrails/github.md`
- [ ] Leer `.claude/guardrails/destructive-ops.md`

## 4. CARGAR CHECKS RELEVANTES

- [ ] Inicio de tarea → leer `.claude/checks/pre-task.md`
- [ ] Va a mergear → leer `.claude/checks/pre-merge.md`

**IMPORTANTE:** Si alguno de estos archivos existe pero está vacío o sin contenido
procesable → NO continuar. Parar y reportar al usuario:

```
⚠ No pude completar: checks/[nombre]
Motivo: el archivo existe pero no tiene contenido procesable
¿Cómo querés proceder?
```

## 5. CARGAR EXECUTION TEMPLATE

- [ ] Leer `.claude/templates/execution.md`

## 6. CONFIRMAR ANTES DE ARRANCAR

Resumir al usuario en 2-3 líneas:
- Qué entendí de la tarea
- En qué área trabajo y qué rule file cargué
- Qué branch voy a usar (existente o propuesta nueva)

Esperar confirmación antes de continuar.

---

## Manejo de secciones incompletas

Si en cualquier sección algo falla (archivo no encontrado, ambigüedad, contenido vacío):

```
⚠ No pude completar: [nombre de sección]
Motivo: [qué faltó o qué es ambiguo]
¿Cómo querés proceder?
```

Nunca saltear una sección silenciosamente.
