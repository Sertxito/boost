---
name: context-map
description: 'Generate a map of all files relevant to a task before making changes'
---

# Context Map

Antes de implementar cualquier cambio, analiza el código y crea un mapa de contexto.

## Task

{{task_description}}

## Instructions

1. Busca en el código los archivos relacionados con esta tarea
2. Identifica dependencias directas (imports/exports)
3. Encuentra pruebas relacionadas
4. Busca patrones similares en el código existente

## Output Format

```markdown
## Context Map

### Files to Modify
| File | Purpose | Changes Needed |
|------|---------|----------------|
| path/to/file | description | what changes |

### Dependencies (may need updates)
| File | Relationship |
|------|--------------|
| path/to/dep | imports X from modified file |

### Test Files
| Test | Coverage |
|------|----------|
| path/to/test | tests affected functionality |

### Reference Patterns
| File | Pattern |
|------|---------|
| path/to/similar | example to follow |

### Risk Assessment
- [ ] Breaking changes to public API
- [ ] Database migrations needed
- [ ] Configuration changes required
```

No continúes con la implementación hasta que este mapa sea revisado.
