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
