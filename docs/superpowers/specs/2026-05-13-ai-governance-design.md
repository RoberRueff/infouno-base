# AI Governance Structure — infouno-base

**Date:** 2026-05-13
**Status:** Approved
**Scope:** Governance infrastructure for AI-assisted development of the infouno-base WordPress project.

---

## 1. Taxonomía del Proyecto

### Estructura de carpetas

```
infouno-base/
│
├── CLAUDE.md                              ← Entry point. Lo primero que lee el modelo.
│                                             Referencia al context-loader.
│
├── .claude/
│   ├── settings.local.json                ← Permisos (ya existe)
│   ├── taxonomy.md                        ← Mapa del proyecto: qué es cada cosa y dónde vive
│   ├── branch-registry.md                 ← Registro activo de todas las branches
│   │
│   ├── rules/                             ← 1:1 componente → regla
│   │   ├── plugin-infouno-custom.md       ← Cómo se trabaja el plugin custom
│   │   ├── theme-astra.md                 ← Cómo se trabaja el tema
│   │   └── [nuevo-componente].md          ← Cada área nueva suma su archivo acá
│   │
│   ├── guardrails/                        ← Límites que la AI nunca cruza
│   │   ├── github.md                      ← Sin PRs, issues ni ops de repo autónomas
│   │   └── destructive-ops.md             ← Confirmación obligatoria antes de ops destructivas
│   │
│   ├── checks/                            ← Verificaciones por momento del ciclo
│   │   ├── pre-task.md                    ← Qué revisar antes de arrancar una tarea
│   │   └── pre-merge.md                   ← Qué verificar antes de mergear
│   │
│   ├── context-loader.md                  ← Protocolo de recolección de contexto (referenciado desde CLAUDE.md)
│   │
│   └── templates/
│       ├── execution.md                   ← Template de ejecución (lineamientos fijos)
│       ├── branch.md                      ← Convención de nombres y flujo de branches
│       └── component-registration.md      ← Checklist para introducir nuevos componentes
│
├── plugins/
│   └── infouno-custom/
│       └── infouno-custom.php
│
└── themes/
    └── astra/
```

### Principio central

Cada área del proyecto tiene exactamente un archivo de reglas en `rules/`. Cuando se agrega un nuevo componente, se agrega su `.md` en `rules/` siguiendo el proceso definido en `templates/component-registration.md`. No hay reglas dispersas.

### Campo `last-updated` en rules/ y guardrails/

Todo archivo dentro de `rules/` y `guardrails/` debe tener este campo al tope del archivo:

```
last-updated: YYYY-MM-DD
```

Cuando la AI modifica cualquiera de estos archivos en una tarea, debe:
1. Actualizar el campo `last-updated` con la fecha del día
2. Incluir en el commit message el nombre del archivo y la fecha:
   ```
   rules/plugin-infouno-custom.md (2026-05-13)
   ```

Esto permite saber qué versión de las reglas estaba vigente en cada punto del historial de git.

---

## 2. Branch Management

### Modelo de branches

```
main          ← producción. Solo recibe merges desde dev con aprobación explícita del usuario.
  └── dev     ← integración. AI puede pushear libremente.
        └── feature/[nombre]
        └── fix/[nombre]
        └── chore/[nombre]
```

### Reglas

| Regla | Detalle |
|---|---|
| Nunca trabajar directo en `main` | `main` solo recibe merges desde `dev` |
| Siempre partir de `dev` | Toda branch nueva se crea desde `dev`, nunca desde `main` |
| Naming convention | `feature/`, `fix/`, `chore/` + kebab-case. Ej: `feature/custom-post-types` |
| AI puede pushear | A branches de feature y a `dev` libremente |
| Merge a `main` | AI ejecuta solo con aprobación explícita del usuario en la conversación |

### Ciclo de vida de una tarea

```
1. AI verifica rama actual contra branch-registry.md
2. AI propone branch a usar (existente o nueva desde dev) y espera confirmación
3. Trabajo + commits + push durante el desarrollo
4. AI corre checks/pre-merge.md antes de mergear
5. AI mergea feature → dev y pushea dev
6. Merge dev → main: AI pregunta confirmación explícita, espera "sí", luego ejecuta
```

### Branch Registry

Archivo: `.claude/branch-registry.md`

La AI mantiene este archivo actualizado. Formato:

```markdown
| Branch | Propósito | Estado | Creada desde | Destino |
|--------|-----------|--------|--------------|---------|
| dev    | Integración principal | activa | main | main |
```

**Reglas del registry:**
- AI agrega la branch al registry antes de empezar a trabajar en ella
- Al mergear: estado → `mergeada`
- Al abandonar: estado → `abandonada` + motivo
- Antes de crear una branch nueva: consultar el registry para evitar duplicados

---

## 3. Guardrails

### GitHub — nunca de forma autónoma

| Operación | Regla |
|---|---|
| `git push` a `main` | Solo después de merge aprobado explícitamente |
| Crear / cerrar PRs | Nunca |
| Crear / cerrar Issues | Nunca |
| Modificar settings del repo | Nunca |
| Invitar colaboradores | Nunca |
| Modificar branch protection rules | Nunca |

La AI opera con git local libremente. GitHub como plataforma queda fuera de su scope autónomo.

### Operaciones destructivas — requieren confirmación explícita

| Operación | Comportamiento |
|---|---|
| `git reset --hard` | AI explica qué va a hacer y espera "sí" |
| `git branch -D` | Idem |
| Borrar archivos | Idem |
| Rebase o amend en rama compartida | Idem |
| Modificar `settings.local.json` | Idem |

### Operaciones libres

- Leer cualquier archivo del repo
- Editar código dentro de `plugins/` y `themes/` (con reglas de cada componente)
- Crear branches desde `dev`
- Hacer commits
- Pushear a branches de feature y a `dev`
- Correr checks locales

---

## 4. Context Loader

Archivo: `.claude/context-loader.md`

Referenciado desde `CLAUDE.md`. Se ejecuta al inicio de cada tarea.

```
1. ORIENTACIÓN
   - Leer taxonomy.md
   - Leer branch-registry.md
   - Correr git branch -a y comparar con branch-registry.md
     → Si hay ramas en git que no están en el registry, o en el registry que no existen
       en git: PARAR y reportar la discrepancia antes de continuar.
   - Verificar rama actual (git branch --show-current)

2. IDENTIFICAR ÁREA DE TRABAJO
   - ¿Toca el plugin?  → leer rules/plugin-infouno-custom.md
   - ¿Toca el tema?    → leer rules/theme-astra.md
   - ¿Op de branches?  → leer templates/branch.md

3. CARGAR GUARDRAILS (siempre)
   - leer guardrails/github.md
   - leer guardrails/destructive-ops.md

4. CARGAR CHECKS RELEVANTES
   - Inicio de tarea   → leer checks/pre-task.md
   - Va a mergear      → leer checks/pre-merge.md
   IMPORTANTE: si alguno de estos archivos existe pero está vacío o sin contenido
   procesable, NO continuar — parar y reportar al usuario antes de seguir.

5. CARGAR EXECUTION TEMPLATE
   - leer .claude/templates/execution.md

6. CONFIRMAR ANTES DE ARRANCAR
   - Resumir en 2-3 líneas: tarea entendida, área de trabajo, branch propuesta
   - Esperar confirmación del usuario
```

**Manejo de secciones incompletas:** si el modelo no puede completar alguna sección (archivo no encontrado, ambigüedad, contexto insuficiente), pausa y reporta:

```
⚠ No pude completar: [nombre de sección]
Motivo: [qué faltó o qué es ambiguo]
¿Cómo querés proceder?
```

Nunca saltea una sección silenciosamente.

---

## 5. Execution Template

Archivo: `.claude/templates/execution.md`

Esqueleto que el modelo sigue en cada iteración de trabajo sin excepción.

```
## 1. ENTENDIMIENTO DE LA TAREA
- Reformular la tarea en mis propias palabras
- Identificar el área del proyecto que involucra
- Listar archivos que probablemente voy a tocar
- Si algo es ambiguo → preguntar antes de continuar

## 2. BRANCH
- Verificar rama actual contra branch-registry.md
- Proponer branch a usar (existente o nueva)
- Esperar confirmación del usuario

## 3. CONTEXTO
- Ejecutar context-loader.md para el área identificada
- Si alguna sección no se puede completar → pausar y reportar

## 4. PLAN DE EJECUCIÓN
- Listar los pasos concretos antes de ejecutar cualquiera
- Identificar pasos irreversibles o riesgosos
- Esperar confirmación del usuario

## 5. EJECUCIÓN
- Ejecutar paso a paso, reportando avance
- Si aparece algo inesperado → pausar y reportar antes de continuar

## 6. VERIFICACIÓN
- Correr checks/pre-merge.md si corresponde
- Confirmar que el resultado cumple lo pedido
- Listar qué cambió y en qué archivos

## 7. CIERRE
- Actualizar branch-registry.md si hubo cambios de branch
- Proponer push si hay commits nuevos
- Preguntar si la tarea está completa o hay iteración siguiente
```

Los pasos 2 y 4 siempre esperan confirmación explícita. "Explícita" significa un "sí", "ok", "dale" o equivalente del usuario en la conversación. El modelo nunca asume silencio como aprobación.

---

## 6. Component Registration Template

Archivo: `.claude/templates/component-registration.md`

Checklist obligatorio cada vez que se introduce algo nuevo al proyecto.

```
1. Crear rules/[nombre-componente].md
   → Documentar: qué es, cómo se trabaja, qué está permitido, qué no

2. Actualizar taxonomy.md
   → Agregar el componente al mapa del proyecto

3. Evaluar checks/
   → ¿Requiere check nuevo pre-task o pre-merge? Si sí, crearlo.

4. Evaluar guardrails/
   → ¿Tiene límites propios? Si sí, crear o actualizar el guardrail relevante.

5. Actualizar branch-registry.md si aplica

6. Verificar que context-loader.md sigue siendo válido
   → ¿El loader sabe cuándo cargar este componente?

7. Confirmar con el usuario que el registration está completo
```

---

## Decisiones de diseño

- **`.claude/` como hub de gobernanza:** convención nativa de Claude Code, mantiene root limpio, ya contiene `settings.local.json`.
- **1:1 componente → rule file:** escala sin ambigüedad. Nuevo componente = nuevo archivo en `rules/`.
- **Branch registry como fuente de verdad:** evita confusión en proyectos con múltiples branches activas.
- **Sync git ↔ registry al inicio:** correr `git branch -a` y comparar con el registry en cada tarea garantiza que el registry nunca quede desincronizado de la realidad.
- **Archivos vacíos = parar, no continuar:** si `checks/pre-task.md` o `checks/pre-merge.md` no tienen contenido procesable, el modelo para y reporta. Procesar un archivo vacío como si tuviera contenido es un error silencioso.
- **`last-updated` en rules/ y guardrails/:** permite auditar qué versión de las reglas estaba vigente en cada commit del historial.
- **Confirmation gates en pasos 2 y 4 del execution template:** el modelo nunca actúa sin haber mostrado su plan.
- **component-registration.md en `templates/`:** es un template aplicado al proyecto, no un doc de referencia estático.
