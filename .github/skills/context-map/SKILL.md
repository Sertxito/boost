---
name: context-map
description: 'Genera un mapa de todos los archivos relevantes para una tarea antes de hacer cambios'
---

# Mapa de Contexto

Antes de implementar cualquier cambio, analiza el código y crea un mapa de contexto.

## Tarea

{{task_description}}

## Instrucciones

1. Busca en el código los archivos relacionados con esta tarea
2. Identifica dependencias directas (imports/exports)
3. Encuentra pruebas relacionadas
4. Busca patrones similares en el código existente

## Formato de salida

```markdown
## Mapa de Contexto

### Archivos a Modificar
| Archivo | Proposito | Cambios Necesarios |
|------|---------|----------------|
| path/to/file | description | what changes |

### Dependencias (pueden requerir actualizacion)
| Archivo | Relacion |
|------|--------------|
| path/to/dep | imports X from modified file |

### Archivos de Prueba
| Prueba | Cobertura |
|------|----------|
| path/to/test | tests affected functionality |

### Patrones de Referencia
| Archivo | Patron |
|------|---------|
| path/to/similar | example to follow |

### Evaluacion de Riesgo
- [ ] Breaking changes to public API
- [ ] Database migrations needed
- [ ] Configuration changes required
```

No continúes con la implementación hasta que este mapa sea revisado.
