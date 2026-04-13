---
name: create-implementation-plan
description: 'Create a new implementation plan file for new features, refactoring existing code or upgrading packages, design, architecture or infrastructure.'
---

# Create Implementation Plan

## Primary Directive

Tu objetivo es crear un nuevo archivo de plan de implementación para `${input:PlanPurpose}`. Tu salida debe ser legible por máquina, determinista y estructurada para ejecución autónoma por otros sistemas de IA o por personas.

## Execution Context

Este prompt está diseñado para comunicación IA-a-IA y procesamiento automatizado. Todas las instrucciones deben interpretarse literalmente y ejecutarse de forma sistemática, sin interpretación ni aclaraciones humanas.

## Core Requirements

- Genera planes de implementación totalmente ejecutables por agentes de IA o por personas
- Usa lenguaje determinista y sin ambigüedad
- Estructura todo el contenido para parseo y ejecución automatizados
- Asegura autocontención completa sin dependencias externas para su comprensión

## Plan Structure Requirements

Los planes deben consistir en fases discretas y atómicas con tareas ejecutables. Cada fase debe poder procesarse de forma independiente por agentes de IA o personas, sin dependencias entre fases salvo que se declaren explícitamente.

## Phase Architecture

- Cada fase debe tener criterios de finalización medibles
- Las tareas dentro de cada fase deben poder ejecutarse en paralelo salvo dependencias explícitas
- Todas las descripciones de tareas deben incluir rutas de archivo específicas, nombres de función y detalles exactos de implementación
- Ninguna tarea debe requerir interpretación o toma de decisiones humana

## AI-Optimized Implementation Standards

- Usa lenguaje explícito, no ambiguo y sin necesidad de interpretación
- Estructura todo el contenido en formatos parseables por máquina (tablas, listas, datos estructurados)
- Incluye rutas específicas, números de línea y referencias de código exactas cuando aplique
- Define explícitamente todas las variables, constantes y valores de configuración
- Proporciona contexto completo dentro de cada descripción de tarea
- Usa prefijos estandarizados para todos los identificadores (REQ-, TASK-, etc.)
- Incluye criterios de validación verificables automáticamente

## Output File Specifications

- Guarda los archivos de plan de implementación en el directorio `/plan/`
- Usa la convención de nombre: `[purpose]-[component]-[version].md`
- Prefijos de propósito: `upgrade|refactor|feature|data|infrastructure|process|architecture|design`
- Ejemplo: `upgrade-system-command-4.md`, `feature-auth-module-1.md`
- El archivo debe ser Markdown válido con estructura correcta de front matter

## Mandatory Template Structure

Todos los planes de implementación deben seguir estrictamente la plantilla siguiente. Cada sección es obligatoria y debe completarse con contenido específico y accionable. Los agentes de IA deben validar el cumplimiento de la plantilla antes de ejecutar.

## Template Validation Rules

- Todos los campos de front matter deben estar presentes y bien formateados
- Todos los encabezados de sección deben coincidir exactamente (sensible a mayúsculas/minúsculas)
- Todos los prefijos de identificador deben seguir el formato especificado
- Las tablas deben incluir todas las columnas requeridas
- No debe quedar texto placeholder en la salida final

## Status

El estado del plan de implementación debe definirse claramente en el front matter y reflejar el estado actual del plan. El estado puede ser uno de los siguientes (status_color entre paréntesis): `Completed` (badge verde brillante), `In progress` (badge amarillo), `Planned` (badge azul), `Deprecated` (badge rojo), o `On Hold` (badge naranja). También debe mostrarse como badge en la sección de introducción.

```md
---
goal: [Concise Title Describing the Package Implementation Plan's Goal]
version: [Optional: e.g., 1.0, Date]
date_created: [YYYY-MM-DD]
last_updated: [Optional: YYYY-MM-DD]
owner: [Optional: Team/Individual responsible for this spec]
status: 'Completed'|'In progress'|'Planned'|'Deprecated'|'On Hold'
tags: [Optional: List of relevant tags or categories, e.g., `feature`, `upgrade`, `chore`, `architecture`, `migration`, `bug` etc]
---

# Introduction

![Status: <status>](https://img.shields.io/badge/status-<status>-<status_color>)

[A short concise introduction to the plan and the goal it is intended to achieve.]

## 1. Requirements & Constraints

[Explicitly list all requirements & constraints that affect the plan and constrain how it is implemented. Use bullet points or tables for clarity.]

- **REQ-001**: Requirement 1
- **SEC-001**: Security Requirement 1
- **[3 LETTERS]-001**: Other Requirement 1
- **CON-001**: Constraint 1
- **GUD-001**: Guideline 1
- **PAT-001**: Pattern to follow 1

## 2. Implementation Steps

### Implementation Phase 1

- GOAL-001: [Describe the goal of this phase, e.g., "Implement feature X", "Refactor module Y", etc.]

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-001 | Description of task 1 | ✅ | 2025-04-25 |
| TASK-002 | Description of task 2 | |  |
| TASK-003 | Description of task 3 | |  |

### Implementation Phase 2

- GOAL-002: [Describe the goal of this phase, e.g., "Implement feature X", "Refactor module Y", etc.]

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-004 | Description of task 4 | |  |
| TASK-005 | Description of task 5 | |  |
| TASK-006 | Description of task 6 | |  |

## 3. Alternatives

[A bullet point list of any alternative approaches that were considered and why they were not chosen. This helps to provide context and rationale for the chosen approach.]

- **ALT-001**: Alternative approach 1
- **ALT-002**: Alternative approach 2

## 4. Dependencies

[List any dependencies that need to be addressed, such as libraries, frameworks, or other components that the plan relies on.]

- **DEP-001**: Dependency 1
- **DEP-002**: Dependency 2

## 5. Files

[List the files that will be affected by the feature or refactoring task.]

- **FILE-001**: Description of file 1
- **FILE-002**: Description of file 2

## 6. Testing

[List the tests that need to be implemented to verify the feature or refactoring task.]

- **TEST-001**: Description of test 1
- **TEST-002**: Description of test 2

## 7. Risks & Assumptions

[List any risks or assumptions related to the implementation of the plan.]

- **RISK-001**: Risk 1
- **ASSUMPTION-001**: Assumption 1

## 8. Related Specifications / Further Reading

[Link to related spec 1]
[Link to relevant external documentation]
```
