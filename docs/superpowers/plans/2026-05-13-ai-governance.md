# AI Governance Structure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the complete AI governance infrastructure for infouno-base: folder structure, CLAUDE.md entry point, context loader, rules, guardrails, checks, and templates.

**Architecture:** All governance lives inside `infouno-base/.claude/`. `CLAUDE.md` at the repo root is the only visible entry point and immediately delegates to `.claude/context-loader.md`. Each component area has exactly one rule file in `.claude/rules/`. Guardrails, checks, and templates live in their own subfolders.

**Tech Stack:** Markdown files, git, Claude Code settings.local.json (JSON).

---

## Task 1: Create dev branch

**Files:**
- Modify: `infouno-base/.git` (branch state)

- [ ] **Step 1: Verify current branch is main**

```bash
git -C infouno-base branch --show-current
```
Expected output: `main`

- [ ] **Step 2: Create dev branch from main**

```bash
git -C infouno-base checkout -b dev
```
Expected output: `Switched to a new branch 'dev'`

- [ ] **Step 3: Verify both branches exist**

```bash
git -C infouno-base branch -a
```
Expected output includes: `* dev` and `main`

---

## Task 2: Create folder structure

**Files:**
- Create: `infouno-base/.claude/rules/` (dir)
- Create: `infouno-base/.claude/guardrails/` (dir)
- Create: `infouno-base/.claude/checks/` (dir)
- Create: `infouno-base/.claude/templates/` (dir)

- [ ] **Step 1: Create all subdirectories**

```bash
mkdir -p infouno-base/.claude/rules \
         infouno-base/.claude/guardrails \
         infouno-base/.claude/checks \
         infouno-base/.claude/templates
```

- [ ] **Step 2: Verify structure**

```bash
find infouno-base/.claude -type d | sort
```
Expected output:
```
infouno-base/.claude
infouno-base/.claude/checks
infouno-base/.claude/guardrails
infouno-base/.claude/rules
infouno-base/.claude/templates
```

---

## Task 3: Create project-level settings.local.json

**Files:**
- Create: `infouno-base/.claude/settings.local.json`

- [ ] **Step 1: Create settings file**

Create `infouno-base/.claude/settings.local.json` with this content:

```json
{
  "permissions": {
    "allow": [
      "Bash(git branch*)",
      "Bash(git checkout*)",
      "Bash(git status*)",
      "Bash(git log*)",
      "Bash(git diff*)",
      "Bash(git add*)",
      "Bash(git commit*)",
      "Bash(git merge*)",
      "Bash(git push origin dev)",
      "Bash(git push origin feature/*)",
      "Bash(git push origin fix/*)",
      "Bash(git push origin chore/*)"
    ],
    "deny": [
      "Bash(git push origin main)",
      "Bash(gh pr*)",
      "Bash(gh issue*)",
      "Bash(gh repo*)"
    ]
  }
}
```

- [ ] **Step 2: Verify file is valid JSON**

```bash
cat infouno-base/.claude/settings.local.json | python3 -m json.tool > /dev/null && echo "valid"
```
Expected output: `valid`

- [ ] **Step 3: Commit**

```bash
git -C infouno-base add .claude/settings.local.json
git -C infouno-base commit -m "chore: add project-level Claude Code permissions"
```

---

## Task 4: Create CLAUDE.md

**Files:**
- Create: `infouno-base/CLAUDE.md`

- [ ] **Step 1: Create CLAUDE.md**

Create `infouno-base/CLAUDE.md` with this content:

```markdown
# infouno-base

## Para el modelo: leé esto primero

Este repositorio usa un sistema de gobernanza para desarrollo asistido por AI.

**Antes de responder cualquier tarea**, ejecutá el protocolo en `.claude/context-loader.md`.

Pasos obligatorios:
1. Leer `.claude/context-loader.md`
2. Seguir cada sección en orden
3. Si no podés completar alguna sección → pausar y reportar al usuario

No arrancés sin haber completado ese protocolo.
```

- [ ] **Step 2: Verify file exists and is readable**

```bash
cat infouno-base/CLAUDE.md
```
Expected: file content printed without errors.

- [ ] **Step 3: Commit**

```bash
git -C infouno-base add CLAUDE.md
git -C infouno-base commit -m "chore: add CLAUDE.md entry point"
```

---

## Task 5: Create taxonomy.md

**Files:**
- Create: `infouno-base/.claude/taxonomy.md`

- [ ] **Step 1: Create taxonomy.md**

Create `infouno-base/.claude/taxonomy.md` with this content:

```markdown
# Taxonomía del Proyecto — infouno-base

## Qué es este proyecto

WordPress site con plugin custom (infouno-custom) y tema base Astra.
Desarrollo solo (un desarrollador + AI).

## Mapa de áreas de código

| Área | Ruta | Rule file |
|------|------|-----------|
| Plugin custom | `plugins/infouno-custom/` | `.claude/rules/plugin-infouno-custom.md` |
| Tema Astra | `themes/astra/` | `.claude/rules/theme-astra.md` |

## Gobernanza AI

Todo lo que está en `.claude/` es infraestructura de gobernanza. No modificar sin motivo claro.

| Archivo/Carpeta | Propósito |
|-----------------|-----------|
| `CLAUDE.md` | Entry point. Lo primero que lee el modelo. |
| `.claude/context-loader.md` | Protocolo de recolección de contexto pre-tarea |
| `.claude/taxonomy.md` | Este archivo. Mapa del proyecto. |
| `.claude/branch-registry.md` | Registro activo de branches |
| `.claude/rules/` | 1 archivo por componente. Reglas de cómo trabajar cada área. |
| `.claude/guardrails/` | Límites que la AI nunca cruza sin confirmación |
| `.claude/checks/` | Verificaciones por momento del ciclo de desarrollo |
| `.claude/templates/` | Esqueletos reutilizables para tareas y procesos |
| `.claude/settings.local.json` | Permisos de Claude Code a nivel proyecto |

## Agregar un nuevo componente

Seguir el proceso en `.claude/templates/component-registration.md` antes de tocar código.
```

- [ ] **Step 2: Commit**

```bash
git -C infouno-base add .claude/taxonomy.md
git -C infouno-base commit -m "chore: add taxonomy.md"
```

---

## Task 6: Create branch-registry.md

**Files:**
- Create: `infouno-base/.claude/branch-registry.md`

- [ ] **Step 1: Create branch-registry.md**

Create `infouno-base/.claude/branch-registry.md` with this content:

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git -C infouno-base add .claude/branch-registry.md
git -C infouno-base commit -m "chore: add branch-registry.md with main and dev"
```

---

## Task 7: Create context-loader.md

**Files:**
- Create: `infouno-base/.claude/context-loader.md`

- [ ] **Step 1: Create context-loader.md**

Create `infouno-base/.claude/context-loader.md` with this content:

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git -C infouno-base add .claude/context-loader.md
git -C infouno-base commit -m "chore: add context-loader.md protocol"
```

---

## Task 8: Create rules files

**Files:**
- Create: `infouno-base/.claude/rules/plugin-infouno-custom.md`
- Create: `infouno-base/.claude/rules/theme-astra.md`

- [ ] **Step 1: Create plugin-infouno-custom.md**

Create `infouno-base/.claude/rules/plugin-infouno-custom.md` with this content:

```markdown
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
```

- [ ] **Step 2: Create theme-astra.md**

Create `infouno-base/.claude/rules/theme-astra.md` with this content:

```markdown
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
```

- [ ] **Step 3: Verify both files have last-updated field**

```bash
head -1 infouno-base/.claude/rules/plugin-infouno-custom.md
head -1 infouno-base/.claude/rules/theme-astra.md
```
Expected: both lines start with `last-updated:`

- [ ] **Step 4: Commit**

```bash
git -C infouno-base add .claude/rules/
git -C infouno-base commit -m "chore: add rule files

rules/plugin-infouno-custom.md (2026-05-13)
rules/theme-astra.md (2026-05-13)"
```

---

## Task 9: Create guardrails files

**Files:**
- Create: `infouno-base/.claude/guardrails/github.md`
- Create: `infouno-base/.claude/guardrails/destructive-ops.md`

- [ ] **Step 1: Create guardrails/github.md**

Create `infouno-base/.claude/guardrails/github.md` with this content:

```markdown
last-updated: 2026-05-13

# Guardrail: GitHub

La AI opera con git local libremente. GitHub como plataforma queda fuera de su scope autónomo.

## Nunca hacer de forma autónoma

- `git push origin main` (requiere aprobación explícita del usuario)
- Crear Pull Requests (`gh pr create` o similar)
- Cerrar o comentar Pull Requests
- Crear o cerrar Issues
- Modificar settings del repositorio
- Invitar colaboradores
- Modificar branch protection rules
- Cualquier operación con `gh` CLI que afecte el estado del repo en GitHub

## Sí permitido (libre)

- `git push origin dev`
- `git push origin feature/*`, `fix/*`, `chore/*`
- `git log`, `git status`, `git diff`
- `git branch`, `git checkout`, `git merge` (local)

## Push a main

Solo ejecutar si el usuario dijo explícitamente "sí" o "mergeá a main" en la
conversación actual. El silencio no es aprobación.
```

- [ ] **Step 2: Create guardrails/destructive-ops.md**

Create `infouno-base/.claude/guardrails/destructive-ops.md` with this content:

```markdown
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
```

- [ ] **Step 3: Verify both files have last-updated field**

```bash
head -1 infouno-base/.claude/guardrails/github.md
head -1 infouno-base/.claude/guardrails/destructive-ops.md
```
Expected: both lines start with `last-updated:`

- [ ] **Step 4: Commit**

```bash
git -C infouno-base add .claude/guardrails/
git -C infouno-base commit -m "chore: add guardrail files

guardrails/github.md (2026-05-13)
guardrails/destructive-ops.md (2026-05-13)"
```

---

## Task 10: Create checks files

**Files:**
- Create: `infouno-base/.claude/checks/pre-task.md`
- Create: `infouno-base/.claude/checks/pre-merge.md`

- [ ] **Step 1: Create checks/pre-task.md**

Create `infouno-base/.claude/checks/pre-task.md` with this content:

```markdown
# Check: Pre-Task

Ejecutar al inicio de cada tarea, antes de tocar cualquier archivo.

## Checklist

- [ ] Rama actual identificada (`git branch --show-current`)
- [ ] `git branch -a` corrido y comparado con branch-registry.md — sin discrepancias
- [ ] Área de trabajo identificada y su rule file leído
- [ ] `guardrails/github.md` leído
- [ ] `guardrails/destructive-ops.md` leído
- [ ] Branch de trabajo propuesta y confirmada por el usuario
- [ ] Plan de ejecución con pasos listados, presentado y confirmado por el usuario

## Si algún ítem falla

Parar y reportar al usuario antes de continuar. No saltear ítems.
```

- [ ] **Step 2: Create checks/pre-merge.md**

Create `infouno-base/.claude/checks/pre-merge.md` with this content:

```markdown
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
```

- [ ] **Step 3: Verify files have content**

```bash
wc -l infouno-base/.claude/checks/pre-task.md infouno-base/.claude/checks/pre-merge.md
```
Expected: both files have more than 5 lines.

- [ ] **Step 4: Commit**

```bash
git -C infouno-base add .claude/checks/
git -C infouno-base commit -m "chore: add pre-task and pre-merge check files"
```

---

## Task 11: Create templates

**Files:**
- Create: `infouno-base/.claude/templates/execution.md`
- Create: `infouno-base/.claude/templates/branch.md`
- Create: `infouno-base/.claude/templates/component-registration.md`

- [ ] **Step 1: Create templates/execution.md**

Create `infouno-base/.claude/templates/execution.md` with this content:

```markdown
# Execution Template

Esqueleto obligatorio para cada iteración de trabajo. Sin excepción.

---

## 1. ENTENDIMIENTO DE LA TAREA

- Reformular la tarea en mis propias palabras
- Identificar el área del proyecto que involucra
- Listar archivos que probablemente voy a tocar
- Si algo es ambiguo → preguntar antes de continuar

## 2. BRANCH

- Verificar rama actual contra `.claude/branch-registry.md`
- Proponer branch a usar (existente o nueva desde dev)
- **Esperar confirmación del usuario** ("sí", "ok", "dale" o equivalente)

## 3. CONTEXTO

- Ejecutar `.claude/context-loader.md` para el área identificada
- Si alguna sección no se puede completar → pausar y reportar antes de continuar

## 4. PLAN DE EJECUCIÓN

- Listar los pasos concretos antes de ejecutar cualquiera
- Identificar pasos irreversibles o riesgosos y marcarlos
- **Esperar confirmación del usuario**

## 5. EJECUCIÓN

- Ejecutar paso a paso
- Reportar avance después de cada paso significativo
- Si aparece algo inesperado → pausar y reportar antes de continuar

## 6. VERIFICACIÓN

- Correr `.claude/checks/pre-merge.md` si se va a mergear
- Confirmar que el resultado cumple lo pedido en la tarea
- Listar qué cambió y en qué archivos

## 7. CIERRE

- Actualizar `.claude/branch-registry.md` si hubo cambios de branch
- Proponer push si hay commits nuevos
- Preguntar si la tarea está completa o hay iteración siguiente

---

"Explícita" en pasos 2 y 4 significa: "sí", "ok", "dale", o equivalente del usuario
en la conversación. El silencio no es aprobación.
```

- [ ] **Step 2: Create templates/branch.md**

Create `infouno-base/.claude/templates/branch.md` with this content:

```markdown
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
```

- [ ] **Step 3: Create templates/component-registration.md**

Create `infouno-base/.claude/templates/component-registration.md` with this content:

```markdown
# Component Registration

Checklist obligatorio cada vez que se introduce algo nuevo al proyecto
(plugin, integración, módulo, herramienta, child theme, etc.).

Completar en orden antes de tocar código del componente.

---

## Checklist

- [ ] **1. Crear rule file**
  - Crear `.claude/rules/[nombre-componente].md`
  - Incluir: qué es, cómo se trabaja, qué está permitido, qué no
  - Agregar campo `last-updated: YYYY-MM-DD` al tope del archivo

- [ ] **2. Actualizar taxonomy.md**
  - Agregar el componente a la tabla de áreas en `.claude/taxonomy.md`
  - Columnas: Área | Ruta | Rule file

- [ ] **3. Evaluar checks/**
  - ¿El nuevo componente requiere pasos de verificación propios?
  - Si sí → agregar sección en `checks/pre-task.md` o `checks/pre-merge.md`

- [ ] **4. Evaluar guardrails/**
  - ¿El componente tiene límites de operación propios (ej: no editar directamente)?
  - Si sí → crear `.claude/guardrails/[nombre].md` o agregar sección en uno existente

- [ ] **5. Actualizar context-loader.md**
  - Agregar la condición de cuándo cargar el rule file del nuevo componente
  - En la sección "2. IDENTIFICAR ÁREA DE TRABAJO"

- [ ] **6. Actualizar branch-registry.md si aplica**
  - Si el componente requiere una branch dedicada → agregarla al registry

- [ ] **7. Confirmar con el usuario**
  - Reportar que el registration está completo
  - Listar qué archivos se crearon o modificaron
```

- [ ] **Step 4: Commit**

```bash
git -C infouno-base add .claude/templates/
git -C infouno-base commit -m "chore: add execution, branch, and component-registration templates"
```

---

## Task 12: Final verification

- [ ] **Step 1: Verify complete folder structure**

```bash
find infouno-base/.claude -not -name '.DS_Store' | sort
```
Expected output:
```
infouno-base/.claude
infouno-base/.claude/branch-registry.md
infouno-base/.claude/checks
infouno-base/.claude/checks/pre-merge.md
infouno-base/.claude/checks/pre-task.md
infouno-base/.claude/context-loader.md
infouno-base/.claude/guardrails
infouno-base/.claude/guardrails/destructive-ops.md
infouno-base/.claude/guardrails/github.md
infouno-base/.claude/rules
infouno-base/.claude/rules/plugin-infouno-custom.md
infouno-base/.claude/rules/theme-astra.md
infouno-base/.claude/settings.local.json
infouno-base/.claude/taxonomy.md
infouno-base/.claude/templates
infouno-base/.claude/templates/branch.md
infouno-base/.claude/templates/component-registration.md
infouno-base/.claude/templates/execution.md
```

- [ ] **Step 2: Verify CLAUDE.md exists at repo root**

```bash
cat infouno-base/CLAUDE.md
```
Expected: file content printed, references `.claude/context-loader.md`

- [ ] **Step 3: Verify all rules and guardrails files have last-updated field**

```bash
for f in infouno-base/.claude/rules/*.md infouno-base/.claude/guardrails/*.md; do
  echo "=== $f ==="; head -1 "$f"
done
```
Expected: every file's first line starts with `last-updated:`

- [ ] **Step 4: Verify git history looks clean**

```bash
git -C infouno-base log --oneline
```
Expected: multiple commits, one per task, with descriptive messages.

- [ ] **Step 5: Verify dev branch exists**

```bash
git -C infouno-base branch -a
```
Expected: both `dev` and `main` listed.
