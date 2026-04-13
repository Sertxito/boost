---
name: breakdown-plan
description: 'Prompt de planificación y automatización de issues que genera planes de proyecto integrales con jerarquía Epic > Feature > Story/Enabler > Test, dependencias, prioridades y seguimiento automatizado.'
---

# Prompt de planificacion de Issues de GitHub y automatizacion de proyectos

## Objetivo

Actúa como una persona Project Manager senior y especialista en DevOps con experiencia en metodología Agile y gestión de proyectos en GitHub. Tu tarea es tomar el conjunto completo de artefactos de la funcionalidad (PRD, diseño UX, desglose técnico, plan de pruebas) y generar un plan integral de proyecto en GitHub con creación automatizada de issues, enlazado de dependencias, asignación de prioridades y seguimiento estilo Kanban.

## Buenas practicas de gestion de proyectos en GitHub

### Jerarquia agil de work items

- **Epic**: Capacidad de negocio amplia que abarca múltiples funcionalidades (nivel hito)
- **Feature**: Funcionalidad entregable orientada a usuario dentro de una épica
- **Story**: Requisito orientado a usuario que aporta valor de forma independiente
- **Enabler**: Trabajo técnico de infraestructura o arquitectura que habilita historias
- **Test**: Trabajo de aseguramiento de calidad para validar historias y habilitadores
- **Task**: Desglose de trabajo a nivel de implementación para historias/habilitadores

### Principios de gestion de proyectos

- **Criterios INVEST**: Independiente, Negociable, Valioso, Estimable, Pequeño, Comprobable
- **Definición de Ready**: Criterios de aceptación claros antes de iniciar el trabajo
- **Definición de Done**: Quality gates y criterios de finalización
- **Gestión de dependencias**: Relaciones de bloqueo claras e identificación de ruta crítica
- **Priorización basada en valor**: Matriz valor de negocio vs. esfuerzo para la toma de decisiones

## Requisitos de entrada

Antes de usar este prompt, asegúrate de tener los artefactos completos del flujo de pruebas:

### Documentos base de la funcionalidad

1. **Feature PRD**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}.md`
2. **Technical Breakdown**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/technical-breakdown.md`
3. **Implementation Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/implementation-plan.md`

### Prompts de planificacion relacionados

- **Test Planning**: Usa el prompt `plan-test` para estrategia de pruebas integral, planificacion de aseguramiento de calidad y creacion de issues de testing
- **Architecture Planning**: Usa el prompt `plan-epic-arch` para arquitectura del sistema y diseno tecnico
- **Feature Planning**: Usa el prompt `plan-feature-prd` para requisitos y especificaciones detalladas de la funcionalidad

## Formato de salida

Crea dos entregables principales:

1. **Project Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/project-plan.md`
2. **Issue Creation Lista de verificacion**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/issues-checklist.md`

### Estructura del plan del proyecto

#### 1. Visión general del proyecto

- **Feature Resumen**: Descripcion breve y valor de negocio
- **Success Criteria**: Resultados medibles y KPI
- **Key Milestones**: Desglose de entregables principales sin cronograma
- **Risk Assessment**: Posibles bloqueos y estrategias de mitigacion

#### 2. Work Item Hierarchy

```mermaid
graph TD
    A[Epic: {Epic Name}] --> B[Feature: {Feature Name}]
    B --> C[Story 1: {User Story}]
    B --> D[Story 2: {User Story}]
    B --> E[Enabler 1: {Technical Work}]
    B --> F[Enabler 2: {Infrastructure}]

    C --> G[Task: Frontend Implementation]
    C --> H[Task: API Integration]
    C --> I[Test: E2E Scenarios]

    D --> J[Task: Component Development]
    D --> K[Task: State Management]
    D --> L[Test: Unit Tests]

    E --> M[Task: Database Schema]
    E --> N[Task: Migration Scripts]

    F --> O[Task: CI/CD Pipeline]
    F --> P[Task: Monitoring Setup]
```

#### 3. Desglose de issues de GitHub

##### Epic Issue Template

```markdown
# Epic: {Epic Name}

## Epic Description

{Epic summary from PRD}

## Business Value

- **Objetivo principal**: {Objetivo de negocio principal}
- **Success Metrics**: {KPIs and measurable outcomes}
- **User Impact**: {How users will benefit}

## Epic Acceptance Criteria

- [ ] {Requisito de alto nivel 1}
- [ ] {Requisito de alto nivel 2}
- [ ] {Requisito de alto nivel 3}

## Features in this Epic

- [ ] #{feature-issue-number} - {Feature Name}

## Definition of Done

- [ ] All feature stories completed
- [ ] Pruebas de extremo a extremo superadas
- [ ] Performance benchmarks met
- [ ] Documentation updated
- [ ] User acceptance testing completed

## Labels

`epic`, `{priority-level}`, `{value-tier}`

## Milestone

{Release version/date}

## Estimate

{Epic-level t-shirt size: XS, S, M, L, XL, XXL}
```

##### Feature Issue Template

```markdown
# Feature: {Feature Name}

## Feature Description

{Feature summary from PRD}

## User Stories in this Feature

- [ ] #{story-issue-number} - {User Story Title}
- [ ] #{story-issue-number} - {User Story Title}

## Technical Enablers

- [ ] #{enabler-issue-number} - {Enabler Title}
- [ ] #{enabler-issue-number} - {Enabler Title}

## Dependencies

**Blocks**: {List of issues this feature blocks}
**Blocked by**: {List of issues blocking this feature}

## Acceptance Criteria

- [ ] {Feature-level requirement 1}
- [ ] {Feature-level requirement 2}

## Definition of Done

- [ ] All user stories delivered
- [ ] Technical enablers completed
- [ ] Integration testing passed
- [ ] UX review approved
- [ ] Performance testing completed

## Labels

`feature`, `{priority-level}`, `{value-tier}`, `{component-name}`

## Epic

#{epic-issue-number}

## Estimate

{Story points or t-shirt size}
```

##### User Story Issue Template

```markdown
# User Story: {Story Title}

## Story Statement

As a **{user type}**, I want **{goal}** so that **{benefit}**.

## Acceptance Criteria

- [ ] {Specific testable requirement 1}
- [ ] {Specific testable requirement 2}
- [ ] {Specific testable requirement 3}

## Technical Tasks

- [ ] #{task-issue-number} - {Implementation task}
- [ ] #{task-issue-number} - {Integration task}

## Testing Requirements

- [ ] #{test-issue-number} - {Test implementation}

## Dependencies

**Blocked by**: {Dependencies that must be completed first}

## Definition of Done

- [ ] Acceptance criteria met
- [ ] Code review approved
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] UX design implemented
- [ ] Accessibility requirements met

## Labels

`user-story`, `{priority-level}`, `frontend/backend/fullstack`, `{component-name}`

## Feature

#{feature-issue-number}

## Estimate

{Story points: 1, 2, 3, 5, 8}
```

##### Technical Enabler Issue Template

```markdown
# Technical Enabler: {Enabler Title}

## Enabler Description

{Technical work required to support user stories}

## Technical Requirements

- [ ] {Technical requirement 1}
- [ ] {Technical requirement 2}

## Implementation Tasks

- [ ] #{task-issue-number} - {Implementation detail}
- [ ] #{task-issue-number} - {Infrastructure setup}

## User Stories Enabled

This enabler supports:

- #{story-issue-number} - {Story title}
- #{story-issue-number} - {Story title}

## Acceptance Criteria

- [ ] {Technical validation 1}
- [ ] {Technical validation 2}
- [ ] Performance benchmarks met

## Definition of Done

- [ ] Implementation completed
- [ ] Unit tests written
- [ ] Integration tests passing
- [ ] Documentation updated
- [ ] Code review approved

## Labels

`enabler`, `{priority-level}`, `infrastructure/api/database`, `{component-name}`

## Feature

#{feature-issue-number}

## Estimate

{Story points or effort estimate}
```

#### 4. Matriz de prioridad y valor

| Priority | Value  | Criteria                        | Labels                            |
| -------- | ------ | ------------------------------- | --------------------------------- |
| P0       | High   | Critical path, blocking release | `priority-critical`, `value-high` |
| P1       | High   | Core functionality, user-facing | `priority-high`, `value-high`     |
| P1       | Medium | Core functionality, internal    | `priority-high`, `value-medium`   |
| P2       | Medium | Important but not blocking      | `priority-medium`, `value-medium` |
| P3       | Low    | Nice to have, technical debt    | `priority-low`, `value-low`       |

#### 5. Guías de estimación

##### Escala de Story Points (Fibonacci)

- **1 point**: Simple change, <4 hours
- **2 points**: Small feature, <1 day
- **3 points**: Medium feature, 1-2 days
- **5 points**: Large feature, 3-5 days
- **8 points**: Complex feature, 1-2 weeks
- **13+ points**: Epic-level work, needs breakdown

##### Estimacion T-Shirt (Epicas/Funcionalidades)

- **XS**: 1-2 story points total
- **S**: 3-8 story points total
- **M**: 8-20 story points total
- **L**: 20-40 story points total
- **XL**: 40+ story points total (consider breaking down)

#### 6. Gestión de dependencias

```mermaid
graph LR
    A[Epic Planning] --> B[Feature Definition]
    B --> C[Enabler Implementation]
    C --> D[Story Development]
    D --> E[Testing Execution]
    E --> F[Feature Delivery]

    G[Infrastructure Setup] --> C
    H[API Design] --> D
    I[Database Schema] --> C
    J[Authentication] --> D
```

##### Tipos de dependencias

- **Blocks**: Work that cannot proceed until this is complete
- **Related**: Work that shares context but not blocking
- **Prerequisite**: Required infrastructure or setup work
- **Parallel**: Work that can proceed simultaneously

#### 7. Plantilla de planificación de sprint

##### Planificacion de capacidad de sprint

- **Team Velocity**: {Average story points per sprint}
- **Sprint Duration**: {2-week sprints recommended}
- **Buffer Allocation**: 20% for unexpected work and bug fixes
- **Focus Factor**: 70-80% of total time on planned work

##### Definicion del objetivo del sprint

```markdown
## Sprint {N} Goal

**Objetivo principal**: {Main deliverable for this sprint}

**Stories in Sprint**:

- #{issue} - {Story title} ({points} pts)
- #{issue} - {Story title} ({points} pts)

**Total Commitment**: {points} story points
**Success Criteria**: {Measurable outcomes}
```

#### 8. Configuración del tablero de proyecto en GitHub

##### Estructura de columnas (Kanban)

1. **Backlog**: Prioritized and ready for planning
2. **Sprint Ready**: Detailed and estimated, ready for development
3. **In Progress**: Currently being worked on
4. **In Review**: Code review, testing, or stakeholder review
5. **Testing**: QA validation and acceptance testing
6. **Done**: Completed and accepted

##### Configuracion de campos personalizados

- **Priority**: P0, P1, P2, P3
- **Value**: High, Medium, Low
- **Component**: Frontend, Backend, Infrastructure, Testing
- **Estimate**: Story points or t-shirt size
- **Sprint**: Current sprint assignment
- **Assignee**: Responsible team member
- **Epic**: Parent epic reference

#### 9. Automatización y GitHub Actions

##### Creacion automatizada de issues

```yaml
name: Create Feature Issues

on:
  workflow_dispatch:
    inputs:
      feature_name:
        description: 'Feature name'
        required: true
      epic_issue:
        description: 'Epic issue number'
        required: true

jobs:
  create-issues:
    runs-on: ubuntu-latest
    steps:
      - name: Create Feature Issue
        uses: actions/github-script@v7
        with:
          script: |
            const { data: epic } = await github.rest.issues.get({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: ${{ github.event.inputs.epic_issue }}
            });

            const featureIssue = await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: `Feature: ${{ github.event.inputs.feature_name }}`,
              body: `# Feature: ${{ github.event.inputs.feature_name }}\n\n...`,
              labels: ['feature', 'priority-medium'],
              milestone: epic.data.milestone?.number
            });
```

##### Actualizaciones automatizadas de estado

```yaml
name: Update Issue Status

on:
  pull_request:
    types: [opened, closed]

jobs:
  update-status:
    runs-on: ubuntu-latest
    steps:
      - name: Move to In Review
        if: github.event.action == 'opened'
        uses: actions/github-script@v7
        # Move related issues to "In Review" column

      - name: Move to Done
        if: github.event.action == 'closed' && github.event.pull_request.merged
        uses: actions/github-script@v7
        # Move related issues to "Done" column
```

### Lista de verificacion de creación de issues

#### Preparación previa a la creación

- [ ] **Feature artifacts complete**: PRD, UX design, technical breakdown, testing plan
- [ ] **Epic exists**: Parent epic issue created with proper labels and milestone
- [ ] **Project board configured**: Columns, custom fields, and automation rules set up
- [ ] **Team capacity assessed**: Sprint planning and resource allocation completed

#### Issues de nivel épica

- [ ] **Epic issue created** with comprehensive description and acceptance criteria
- [ ] **Epic milestone created** with target release date
- [ ] **Epic labels applied**: `epic`, priority, value, and team labels
- [ ] **Epic added to project board** in appropriate column

#### Issues de nivel funcionalidad

- [ ] **Feature issue created** linking to parent epic
- [ ] **Feature dependencies identified** and documented
- [ ] **Feature estimation completed** using t-shirt sizing
- [ ] **Feature acceptance criteria defined** with measurable outcomes

#### Story/Enabler Level Issues documented in `/docs/ways-of-work/plan/{epic-name}/{feature-name}/issues-checklist.md`

- [ ] **User stories created** following INVEST criteria
- [ ] **Technical enablers identified** and prioritized
- [ ] **Story point estimates assigned** using Fibonacci scale
- [ ] **Dependencies mapped** between stories and enablers
- [ ] **Acceptance criteria detailed** with testable requirements

## Métricas de éxito

### KPIs de gestión de proyecto

- **Sprint Predictability**: >80% of committed work completed per sprint
- **Cycle Time**: Average time from "In Progress" to "Done" <5 business days
- **Lead Time**: Average time from "Backlog" to "Done" <2 weeks
- **Defect Escape Rate**: <5% of stories require post-release fixes
- **Team Velocity**: Consistent story point delivery across sprints

### Métricas de eficiencia del proceso

- **Issue Creation Time**: <1 hour to create full feature breakdown
- **Dependency Resolution**: <24 hours to resolve blocking dependencies
- **Status Update Accuracy**: >95% automated status transitions working correctly
- **Documentation Completeness**: 100% of issues have required template fields
- **Cross-Team Collaboration**: <2 business days for external dependency resolution

### Metricas de entrega del proyecto

- **criterio de finalizacion Compliance**: 100% of completed stories meet DoD criteria
- **Acceptance Criteria Coverage**: 100% of acceptance criteria validated
- **Sprint Goal Achievement**: >90% of sprint goals successfully delivered
- **Stakeholder Satisfaction**: >90% stakeholder approval for completed features
- **Planning Accuracy**: <10% variance between estimated and actual delivery time

This comprehensive GitHub project management approach ensures complete traceability from epic-level planning down to individual implementation tasks, with automated tracking and clear accountability for all team members.
