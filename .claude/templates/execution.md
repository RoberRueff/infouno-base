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
