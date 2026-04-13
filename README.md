# Boost: guia clara de uso

Este repositorio organiza la forma de trabajar con Copilot mediante:

- `instructions` (reglas globales y por tipo de archivo)
- `agents` (modos especializados que puedes invocar directamente)
- `skills` (playbooks reutilizables para tareas concretas)

## Indice rapido

- [Uso innato vs uso explicito](#1-uso-innato-vs-uso-explicito)
- [Agentes disponibles](#2-agentes-disponibles-invocacion-directa)
- [Skills disponibles](#3-skills-disponibles-cuando-invocarlas-explicitamente)
- [Artefactos template de este repo](#artefactos-template-de-este-repo)
- [Flujos recomendados](#4-flujos-recomendados-listos-para-copiar)
- [Modo legacy a moderno](#5-modo-legacy-a-moderno-fuente-de-verdad-y-ejecucion)
- [Cuando NO invocar explicitamente](#6-cuando-no-invocar-explicitamente)
- [Buenas practicas de uso](#7-buenas-practicas-de-uso-en-este-repo)
- [Plantillas de prompt utiles](#8-plantillas-de-prompt-utiles)
- [Estrategia multiplataforma](#9-estrategia-multiplataforma-un-solo-boost)
- [Matriz de paridad Azure vs AWS](#10-matriz-de-paridad-azure-vs-aws)
- [Decision rapida entre skills de trazabilidad](#11-decision-rapida-entre-skills-de-trazabilidad)

## 1. Uso innato vs uso explicito

### Uso innato (sin invocar nada)

Copilot aplica automaticamente:

- Instrucciones de `.github/instructions/*.instructions.md` segun `applyTo`.
- Buenas practicas generales de edicion, busqueda, validacion y seguridad.
- Flujo base: entender peticion -> explorar -> editar -> validar -> resumir.

Cuando pedirlo asi:

- "Refactoriza este archivo"
- "Arregla los errores del proyecto"
- "Traduce esta documentacion"

### Uso explicito (invocando agente o skill)

Usa invocacion explicita cuando quieres control fino del enfoque.

Patrones recomendados de prompt:

- `Usa el agente "DevOps Expert" para preparar un plan de despliegue.`
- `Usa la skill "context-map" antes de tocar codigo.`
- `Usa "Polyglot Test Generator" para crear tests del modulo X.`

Regla practica:

- Si necesitas especializacion profunda, trazabilidad o un entregable estructurado, usa agente/skill explicitos.
- Si es una tarea simple y local, uso innato suele bastar.

## 2. Agentes disponibles (invocacion directa)

Todos los agentes estan en `.github/agents`.

| Agente | Cuando usarlo | Ejemplo de prompt |
| --- | --- | --- |
| `DevOps Expert` | CI/CD, despliegue, observabilidad, automatizacion | `Usa el agente "DevOps Expert" para proponer pipeline y estrategia de rollback.` |
| `Expert .NET software engineer mode instructions` | Diseno e implementacion .NET avanzada | `Usa "Expert .NET software engineer mode instructions" para redisenar este servicio con patrones modernos.` |
| `Implementation Plan Generation Mode` | Crear plan de implementacion detallado | `Usa "Implementation Plan Generation Mode" para planificar esta feature en fases.` |
| `Agente de modernizacion` | Analisis y plan de modernizacion de extremo a extremo | `Usa "Agente de modernizacion" para evaluar deuda tecnica y roadmap de modernizacion.` |
| `Playwright Tester Mode` | Exploracion y pruebas de extremo a extremo con Playwright | `Usa "Playwright Tester Mode" para crear y validar pruebas del login.` |
| `Polyglot Test Fixer` | Corregir errores de compilacion en tests/codigo | `Usa "Polyglot Test Fixer" para arreglar fallos de compilacion en los tests.` |
| `Polyglot Test Generator` | Generar tests completos multi-fase | `Usa "Polyglot Test Generator" para subir cobertura del modulo de pagos.` |
| `Polyglot Test Implementer` | Ejecutar una fase concreta del plan de tests | `Usa "Polyglot Test Implementer" para implementar la fase 2 del plan.` |
| `Polyglot Test Planner` | Planificar estrategia de tests por fases | `Usa "Polyglot Test Planner" para dividir testing por prioridad y riesgo.` |
| `SE: Architect` | Revisión de arquitectura y escalabilidad | `Usa "SE: Architect" para revisar riesgos de escalabilidad del sistema.` |
| `SE: Tech Writer` | Documentacion tecnica, guias, tutoriales | `Usa "SE: Tech Writer" para redactar la guia de uso del modulo API.` |
| `Instrucciones del Task Planner` | Planes accionables con artefactos de seguimiento | `Usa "Instrucciones del Task Planner" para generar plan y lista de verificacion de implementacion.` |
| `Instrucciones del Task Researcher` | Investigacion tecnica previa a implementar | `Usa "Instrucciones del Task Researcher" para investigar opciones de autenticacion.` |

## 3. Skills disponibles (cuando invocarlas explicitamente)

Todas las skills estan en `.github/skills/*/SKILL.md`.

| Skill | Cuando usarla | Ejemplo de prompt |
| --- | --- | --- |
| `architecture-blueprint-generator` | Blueprint arquitectonico completo del proyecto | `Usa la skill "architecture-blueprint-generator" para documentar la arquitectura actual.` |
| `azure-devops-cli` | Operaciones Azure DevOps por CLI | `Usa "azure-devops-cli" para listar pipelines y revisar builds fallidos.` |
| `aws-core` | Marco base para trabajar en AWS (discovery, plan, ejecucion y validacion) | `Usa "aws-core" para definir estrategia de trabajo en AWS antes de implementar.` |
| `aws-deploy` | Despliegues en AWS con enfoque seguro y rollback | `Usa "aws-deploy" para preparar y ejecutar despliegue en ECS con rollback.` |
| `aws-security` | Revisiones de seguridad y fortalecimiento en AWS | `Usa "aws-security" para revisar IAM, secretos y configuracion insegura.` |
| `aws-observability` | Observabilidad en AWS (logs, metricas, alarmas, trazas) | `Usa "aws-observability" para definir dashboard y alertas de CloudWatch.` |
| `aws-iac` | IaC en AWS con Terraform/CloudFormation y cambios controlados | `Usa "aws-iac" para planificar y aplicar cambios de infraestructura con rollback.` |
| `aws-cost` | Optimizacion de coste en AWS con impacto medible | `Usa "aws-cost" para detectar top cost drivers y plan de ahorro.` |
| `aws-diagnostics` | Diagnostico de incidencias en AWS con causa raiz | `Usa "aws-diagnostics" para investigar errores 5xx y degradacion de latencia.` |
| `aws-governance` | Gobierno multi-cuenta, politicas y cumplimiento en AWS | `Usa "aws-governance" para definir baseline de politicas y tagging.` |
| `breakdown-epic-arch` | Arquitectura tecnica de una epica | `Usa "breakdown-epic-arch" sobre este PRD.` |
| `breakdown-epic-pm` | Crear PRD de epica | `Usa "breakdown-epic-pm" para redactar el PRD de la nueva epica.` |
| `breakdown-plan` | Plan maestro Epic -> Feature -> Story/Test | `Usa "breakdown-plan" para generar el plan completo del proyecto.` |
| `breakdown-test` | Estrategia de pruebas y QA por epica/feature | `Usa "breakdown-test" para definir el plan de testing de esta feature.` |
| `context-map` | Mapear archivos relevantes antes de editar | `Usa "context-map" y no escribas codigo todavia.` |
| `create-implementation-plan` | Crear plan de implementacion para cambios | `Usa "create-implementation-plan" para la migracion a .NET 10.` |
| `create-specification` | Especificacion funcional/tecnica consumible por IA | `Usa "create-specification" para formalizar esta solucion.` |
| `devops-rollout-plan` | Plan de rollout con verificaciones y rollback | `Usa "devops-rollout-plan" para desplegar sin downtime.` |
| `documentation-writer` | Documentacion siguiendo Diataxis | `Usa "documentation-writer" para crear tutorial y referencia API.` |
| `dotnet-best-practices` | Revisar cumplimiento de buenas practicas .NET | `Usa "dotnet-best-practices" en este proyecto C#.` |
| `dotnet-design-pattern-review` | Revisar patrones de diseno en C#/.NET | `Usa "dotnet-design-pattern-review" en Application y Domain.` |
| `engineering-quality-traceability` | Estandares de ingenieria, arquitectura y trazabilidad de extremo a extremo para produccion | `Usa "engineering-quality-traceability" para definir estandares y auditoria operativa de punta a punta.` |
| `generate-custom-instructions-from-codebase` | Crear instrucciones desde diferencias entre versiones | `Usa "generate-custom-instructions-from-codebase" comparando rama main y release.` |
| `identity-access-security` | Estrategia completa de autenticacion/autorizacion (roles, claims, guards, tokens, federacion) | `Usa "identity-access-security" para disenar autenticacion y autorizacion de extremo a extremo con trazabilidad.` |
| `playwright-generate-test` | Generar test Playwright desde escenario | `Usa "playwright-generate-test" para el flujo de checkout.` |
| `polyglot-test-agent` | Generacion integral de tests multi-lenguaje | `Usa "polyglot-test-agent" para subir cobertura a 80%.` |
| `project-workflow-analysis-blueprint-generator` | Documentar flujos de extremo a extremo reales del codigo | `Usa "project-workflow-analysis-blueprint-generator" para mapear entrypoints y flujo de datos.` |
| `quality-playbook` | Sistema de calidad completo (QUALITY, protocolos, tests) | `Usa "quality-playbook" para crear playbook de calidad del repo.` |
| `refactor-plan` | Plan de refactor multiarchivo con secuencia segura | `Usa "refactor-plan" para este refactor en 5 modulos.` |
| `security-review` | Auditoria de seguridad razonada | `Usa "security-review" para buscar SQLi, XSS y fallos de auth.` |
| `webapp-testing` | Pruebas de app web local con Playwright | `Usa "webapp-testing" para validar UI y capturar evidencia.` |
| `write-coding-standards-from-file` | Generar estandares de codigo desde ejemplos reales | `Usa "write-coding-standards-from-file" tomando src/core y src/api.` |

## Artefactos template de este repo

Regla de mantenimiento:

- Fuente de verdad reutilizable: `templates/` dentro de cada skill.
- Calidad/trazabilidad: `.github/skills/engineering-quality-traceability/templates/*`
- Identidad/acceso: `.github/skills/identity-access-security/templates/*`

## 4. Flujos recomendados (listos para copiar)

### Flujo A: implementar feature nueva

1. `Usa "context-map" para mapear archivos impactados.`
2. `Usa "create-specification" para la especificacion funcional y tecnica.`
3. `Usa "create-implementation-plan" para plan por fases.`
4. Implementar en modo innato.
5. `Usa "polyglot-test-agent" para crear/ajustar tests.`

### Flujo B: auditoria de seguridad

1. `Usa "context-map" para superficie de ataque.`
2. `Usa "security-review" para hallazgos priorizados.`
3. Implementar fixes en modo innato o con agente especializado.
4. Revalidar tests y reportar riesgos residuales.

### Flujo C: modernizacion

1. `Usa "Agente de modernizacion" para analisis inicial.`
2. `Usa "Implementation Plan Generation Mode" para plan accionable.`
3. `Usa "refactor-plan" para secuenciar cambios complejos.`
4. `Usa "polyglot-test-agent" para cobertura de regresion.`

### Flujo D: documentacion tecnica

1. `Usa "SE: Tech Writer" para estructura y tono.`
2. `Usa "documentation-writer" para formato Diataxis.`
3. Revisar consistencia con instrucciones de `.github/instructions`.

### Flujo E: calidad y trazabilidad de extremo a extremo

1. `Usa "engineering-quality-traceability" para baseline de estandares y trazabilidad.`
2. Instanciar templates de `.github/skills/engineering-quality-traceability/templates/*` en el proyecto destino.
3. Validar Lista de verificacion operativo generado antes de merge.

### Flujo F: identidad y acceso

1. `Usa "identity-access-security" para definir modelo de identidad y autorizacion.`
2. Instanciar templates de `.github/skills/identity-access-security/templates/*` en el proyecto destino.
3. Ajustar rotacion/revocacion y contrato de claims con los templates correspondientes.
4. Verificar auditoria de seguridad con el template de eventos de seguridad.

## 5. Modo legacy a moderno (fuente de verdad y ejecucion)

Este apartado es clave para este repo: casi todos los proyectos de entrada seran legacy. La regla principal es modernizar sin romper negocio, con una fuente de verdad viva y versionada.

### Objetivo del modo legacy

- Entender primero el sistema real (no el deseado).
- Definir una fuente de verdad unica y actualizable.
- Modernizar por incrementos pequenos, medibles y reversibles.
- Mantener trazabilidad entre decision tecnica, codigo y resultado.

### Protocolo al llegar a un proyecto legacy

1. Descubrimiento inicial (sin tocar codigo)

- Usa `context-map` para identificar modulos, dependencias, entrypoints y zonas de riesgo.
- Usa `project-workflow-analysis-blueprint-generator` para mapear flujos reales de extremo a extremo.
- Si hay riesgo arquitectonico alto, usa `SE: Architect` para una evaluacion inicial.

1. Baseline y riesgos

- Define baseline tecnico: build actual, tests existentes, errores abiertos, deuda visible.
- Define baseline funcional: que procesos de negocio no se pueden romper.
- Registra riesgos por prioridad: seguridad, disponibilidad, datos, rendimiento, mantenibilidad.

1. Fuente de verdad (single source of truth)

- Crea un documento central versionado (por ejemplo `docs/modernization/source-of-truth.md`).
- Ese documento debe contener, minimo:
  - estado actual validado (arquitectura, flujos, constraints),
  - decisiones tecnicas activas (ADR resumidas o enlazadas),
  - backlog de modernizacion priorizado,
  - metricas de progreso y criterios de done,
  - riesgos abiertos y mitigaciones,
  - historial de cambios relevantes.

1. Plan de modernizacion incremental

- Usa `Agente de modernizacion` para estrategia global.
- Usa `Implementation Plan Generation Mode` o `create-implementation-plan` para descomponer en fases.
- Usa `refactor-plan` para cambios multiarchivo con secuencia segura y rollback.

1. Ejecucion y control

- Ejecuta en lotes pequenos (un objetivo tecnico por PR).
- Cada lote debe incluir:
  - evidencia de no regresion (tests/build),
  - actualizacion de la fuente de verdad,
  - estado de riesgos (cerrado/reducido/nuevo).

1. Validacion continua

- Seguridad: `security-review` en hitos importantes.
- Calidad y cobertura: `polyglot-test-agent`.
- Operacion/despliegue: `DevOps Expert` y `devops-rollout-plan` si hay salida a entorno.

### Regla de oro: mantener viva la fuente de verdad

Cada cambio relevante en legacy debe actualizar la fuente de verdad en el mismo ciclo de trabajo. Si no se actualiza, el conocimiento vuelve a degradarse y el proyecto regresa al estado legacy opaco.

Lista de verificacion minimo por cambio:

- Se actualizo el estado actual impactado.
- Se registraron decisiones tomadas y su por que.
- Se reflejo el avance real del backlog.
- Se actualizaron riesgos y mitigaciones.
- Se enlazo evidencia (PR, test, reporte).

### Prompts listos para proyectos legacy

Diagnostico inicial:

`Estamos en un proyecto legacy. Usa "context-map" y luego "project-workflow-analysis-blueprint-generator" para mapear el sistema real. No implementes nada todavia.`

Crear fuente de verdad:

`Crea docs/modernization/source-of-truth.md con estado actual, decisiones tecnicas, backlog priorizado, riesgos y metricas de avance.`

Plan de modernizacion:

`Usa "Agente de modernizacion" y despues "create-implementation-plan" para una hoja de ruta por fases, con rollback por fase.`

Ejecucion controlada:

`Implementa solo la fase 1. Incluye pruebas de no regresion, actualiza la fuente de verdad y resume riesgos residuales.`

### Anti-patrones a evitar en legacy

- Reescritura total sin baseline ni ruta incremental.
- Refactor grande sin plan de rollback.
- Cambios tecnicos sin actualizar la fuente de verdad.
- Medir avance por volumen de codigo en vez de por riesgo reducido y estabilidad.
- Mezclar discovery, diseno e implementacion en un solo paso sin checkpoints.

## 6. Cuando NO invocar explicitamente

No hace falta agente/skill explicitos para:

- cambios pequenos en 1-2 archivos,
- correcciones de lint simples,
- traducciones puntuales,
- consultas rapidas del codigo.

En esos casos, pide la tarea directamente y Copilot operara en modo innato.

## 7. Buenas practicas de uso en este repo

- Pide siempre objetivo + contexto + criterio de exito.
- Si la tarea es grande: primero investigacion/plan, luego implementacion.
- Para cambios riesgosos: pedir validacion (tests, build, diff resumido).
- En refactors: exigir plan de rollback.
- Para seguridad y compliance: usar skill dedicada (`security-review`).
- Para estandares de ingenieria y trazabilidad de extremo a extremo en produccion: usar (`engineering-quality-traceability`).
- Para autenticacion/autorizacion avanzada (roles, claims, tokens, guards, federacion): usar (`identity-access-security`).
- Si hay solape de trazabilidad: usa `engineering-quality-traceability` para trazabilidad general de sistema y `identity-access-security` para trazabilidad de seguridad (auth/authz/tokens/denegaciones).

## 8. Plantillas de prompt utiles

### Prompt corto (modo innato)

`Refactoriza este modulo para mejorar legibilidad sin cambiar comportamiento. Ejecuta validaciones y resume cambios.`

### Prompt con agente

`Usa el agente "DevOps Expert" para proponer una estrategia de despliegue canary con rollback y checks.`

### Prompt con skill

`Usa la skill "quality-playbook" para crear artefactos de calidad en este codebase y dame plan de ejecucion.`

`Usa la skill "engineering-quality-traceability" para definir convenciones de codigo/arquitectura y reforzar la auditoria de extremo a extremo del aplicativo.`

`Usa la skill "identity-access-security" para definir modelo de identidad, autorizacion por claims y politica de refresh token con controles frontend/backend.`

### Prompt mixto (recomendado)

`Primero usa "context-map" para identificar archivos clave. Luego usa "create-implementation-plan". Despues implementa y valida con tests.`

## 9. Estrategia multiplataforma (un solo Boost)

La recomendacion para este repo es mantener un unico Boost y no separarlo por proveedor cloud.

Modelo operativo:

- Capa comun (80-90%): legacy->moderno, source of truth, planning, testing, seguridad y rollout.
- Capa especifica por plataforma (10-20%): skills por proveedor (`azure-*`, `aws-*`, y futuro `gcp-*` si aplica).

Reglas de decision:

- Si el contexto es cloud-neutral, usa flujo comun.
- Si hay contexto Azure claro, prioriza skills Azure.
- Si hay contexto AWS claro, prioriza skills AWS.

Ventajas:

- Evita duplicar mantenimiento en dos repositorios de Boost.
- Conserva una forma unica de trabajar y medir calidad.
- Permite crecer por adaptadores cloud sin rehacer el core.

## 10. Matriz de paridad Azure vs AWS

Esta matriz te ayuda a pedir lo mismo cambiando solo proveedor cloud.

| Necesidad | Azure (referencia) | AWS (equivalente en este Boost) | Prompt rapido |
| --- | --- | --- | --- |
| Marco base de trabajo | `azure-prepare`/flujos base | `aws-core` | `Usa aws-core para discovery, baseline y plan por fases.` |
| Despliegue y rollback | `azure-deploy` | `aws-deploy` | `Usa aws-deploy para plan de despliegue con rollback.` |
| Seguridad | `azure-compliance`/`azure-rbac` | `aws-security` | `Usa aws-security para auditar IAM, secretos y red.` |
| Observabilidad | `azure-observability` | `aws-observability` | `Usa aws-observability para SLI/SLO y alertas.` |
| IaC / cambios infra | `bicep`/`terraform` (Azure) | `aws-iac` | `Usa aws-iac para plan y apply controlado.` |
| Diagnostico de incidentes | `azure-diagnostics` | `aws-diagnostics` | `Usa aws-diagnostics para hallar causa raiz.` |
| Optimizacion de coste | `azure-cost` | `aws-cost` | `Usa aws-cost para plan de ahorro con riesgo bajo.` |
| Gobernanza y cumplimiento | politicas Azure y landing zone | `aws-governance` | `Usa aws-governance para baseline multi-cuenta.` |

Regla practica: primero skill de base (`aws-core`), luego skill especializada por necesidad.

## 11. Decision rapida entre skills de trazabilidad

Usa esta regla cuando dudes entre skills:

- `engineering-quality-traceability`: estandares de codigo/arquitectura y observabilidad operativa de punta a punta.
- `identity-access-security`: autenticacion, autorizacion, roles/subroles, claims, tokens y eventos de seguridad.
- `security-review`: auditoria de vulnerabilidades y hallazgos de seguridad sobre codigo existente.

Combinacion recomendada para escenarios reales:

1. Define estrategia con `identity-access-security`.
2. Integra trazabilidad operativa global con `engineering-quality-traceability`.
3. Ejecuta auditoria tecnica final con `security-review`.

Templates adicionales recomendados (ya incluidos):

- Calidad/trazabilidad: `pr-quality-checklist.template.md`, `sli-slo-observability.template.md`.
- Identidad/acceso: `role-permission-matrix.template.md`, `token-claims-contract.template.md`, `fnmt-integration-checklist.template.md`.

---

Si este README se mantiene actualizado cuando se agregan nuevos agentes o skills, se convierte en la referencia operativa unica del repo para trabajar con Boost.

> *Nota de procedencia: parte del contenido de `agents`, `instructions` y `skills` esta reutilizado o inspirado en recursos de awesome-copilot y fuentes similares. Sobre esa base, este Boost esta customizado por su autor para su propia experiencia de trabajo (legacy->moderno, convenciones en espanol, flujos y prompts operativos), incluyendo criterios de calidad, seguridad y documentacion ajustados a su forma de trabajar. Si reutilizas o amplias este repositorio, conviene mantener trazabilidad de origen y respetar licencias de los contenidos base.*
