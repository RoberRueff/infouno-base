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
