---
name: refactor-plan
description: 'Plan a multi-file refactor with proper sequencing and rollback steps'
---

# Refactor Plan

Crea un plan detallado para esta tarea de refactorización.

## Refactor Goal

{{refactor_description}}

## Instructions

1. Busca en el código para entender el estado actual
2. Identifica todos los archivos afectados y sus dependencias
3. Planifica los cambios en una secuencia segura (tipos primero, luego implementaciones y después pruebas)
4. Incluye pasos de verificación entre cambios
5. Considera rollback si algo falla

## Output Format

```markdown
## Refactor Plan: [title]

### Current State
[Brief description of how things work now]

### Target State
[Brief description of how things will work after]

### Affected Files
| File | Change Type | Dependencies |
|------|-------------|--------------|
| path | modify/create/delete | blocks X, blocked by Y |

### Execution Plan

#### Phase 1: Types and Interfaces
- [ ] Step 1.1: [action] in `file.ts`
- [ ] Verify: [how to check it worked]

#### Phase 2: Implementation
- [ ] Step 2.1: [action] in `file.ts`
- [ ] Verify: [how to check]

#### Phase 3: Tests
- [ ] Step 3.1: Update tests in `file.test.ts`
- [ ] Verify: Run `npm test`

#### Phase 4: Cleanup
- [ ] Remove deprecated code
- [ ] Update documentation

### Rollback Plan
If something fails:
1. [Step to undo]
2. [Step to undo]

### Risks
- [Potential issue and mitigation]
```

¿Debo continuar con la Fase 1?
