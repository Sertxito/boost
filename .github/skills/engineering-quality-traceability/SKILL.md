---
name: engineering-quality-traceability
description: "Skill de calidad de ingenieria y trazabilidad de extremo a extremo para entornos productivos. Define estandares de programacion y arquitectura, convenciones de nomenclatura, catalogo de eventos de auditoria, correlacion de trazas, logging estructurado, y lista de verificacion de controles minimos en CI/CD. Usala cuando se pida fortalecimiento de calidad, estandarizar practicas de equipo, mejorar observabilidad, o auditar el comportamiento de una aplicacion de punta a punta."
---

# Calidad de Ingenieria y Trazabilidad

Skill para elevar la calidad tecnica y la trazabilidad operativa de un proyecto en produccion.

## Cuando usar esta skill

Usala cuando la solicitud incluya alguna de estas intenciones:

- Estandarizar buenas practicas de programacion y arquitectura
- Definir convenciones de nombres para variables, metodos y clases
- Mejorar auditoria y observabilidad de extremo a extremo
- Crear o fortalecer trazabilidad funcional y tecnica en produccion
- Establecer controles minimos de calidad en pull requests y CI/CD

## Resultados esperados

La skill debe generar o actualizar estos artefactos (adaptados al stack real del repo):

Las plantillas canonicas viven en `templates/` dentro de esta skill.
Este Boost se mantiene en modo solo-plantillas.
Los archivos en `docs/` se generan en el proyecto destino al aplicar la skill.

1. `docs/quality/engineering-standards.md`
- Convenciones de codigo y nomenclatura
- Principios de diseno y arquitectura aplicables
- Reglas de revisiones de codigo y criterio de finalizacion

2. `docs/observability/traceability-standard.md`
- Estandar de trazabilidad de extremo a extremo
- Esquema de correlation/request/trace IDs
- Politica de logs estructurados y niveles
- Integracion logs + metricas + trazas

3. `docs/observability/audit-event-catalog.md`
- Catalogo de eventos auditables por dominio
- Campos obligatorios (who, what, when, where, before, after, outcome)
- Clasificacion de severidad y retencion

4. `docs/ops/production-checklist.md`
- Lista de verificacion operativa para despliegues
- Alertas, SLO/SLI, runbooks y rollback
- Controles de seguridad y compliance de observabilidad

5. `.github/pull_request_template.md` (si no existe)
- Seccion de calidad: arquitectura, nomenclatura, pruebas
- Seccion de trazabilidad: eventos auditables, correlation IDs, dashboards

6. `docs/backend/dotnet-backend-standards.md` (si aplica a backend .NET)
- Configuracion recomendada de Swagger/OpenAPI
- Estructura y composicion de `Program.cs`
- Reglas de inyeccion de dependencias (lifetimes y boundaries)
- Principios de clases atomicas, uso de helpers y criterios para genericos

7. `docs/frontend/angular-frontend-standards.md` (si aplica a frontend Angular)
- Convenciones de estructura y nomenclatura segun `angular.dev/style-guide`
- Estandar de componentes standalone, DI con `inject()` y estado con signals
- Reglas de routing, HttpClient (interceptores/XSRF), seguridad y performance

## Flujo recomendado

1. Descubrir el contexto
- Identificar stack, arquitectura, y flujos criticos
- Ubicar puntos de entrada/salida y operaciones sensibles

2. Definir baseline
- Capturar estado actual de naming, arquitectura y observabilidad
- Detectar gaps: falta de correlacion, logs no estructurados, ausencia de auditoria

3. Diseñar el estandar minimo
- Normalizar convenciones de codigo
- Definir lineamientos de arquitectura y boundaries
- Establecer contrato de trazabilidad transversal

4. Aplicar controles en el ciclo de entrega
- Incorporar Lista de verificacion de PR y gates de CI/CD
- Exigir evidencia operativa para cambios criticos

5. Verificar en produccion
- Validar cobertura de eventos auditables
- Confirmar visibilidad de extremo a extremo de un caso real

## Perfil backend .NET (reglas explicitas)

Cuando el proyecto tenga backend en .NET, esta skill debe forzar ademas los siguientes estandares:

1. Swagger/OpenAPI completo
- Configurar OpenAPI con metadata de version, contacto y descripcion de API.
- Incluir XML comments para enriquecer contratos de endpoints y modelos.
- Definir esquema de seguridad (Bearer/JWT u otro) y aplicarlo en la UI de Swagger.
- Documentar codigos de respuesta y errores de negocio de forma consistente.

2. `Program.cs` limpio y escalable
- Registrar servicios por extension methods (`AddApplication`, `AddInfrastructure`, etc.) para evitar un `Program.cs` monolitico.
- Separar composicion de dependencias, middleware, endpoints y configuracion de observabilidad.
- Mantener orden estable: configuracion, servicios, pipeline HTTP, mapeo de endpoints, health checks.

3. Inyeccion de dependencias con criterio
- Usar interfaces en capas de aplicacion y dominio, evitando acoplamiento a detalles de infraestructura.
- Definir lifetimes correctos (`Singleton`, `Scoped`, `Transient`) segun semantica real.
- Evitar service locator y resolucion manual de dependencias fuera del contenedor.

4. Clases atomicas y responsabilidades
- Cada clase con una unica responsabilidad observable.
- Evitar clases anidadas salvo excepciones justificadas y documentadas.
- Mantener DTOs y modelos en carpetas dedicadas; no mezclar entidades de dominio con transporte.

5. Helpers y utilidades
- Extraer logica transversal repetida a helpers/utilidades reutilizables.
- No mover reglas de negocio a helpers genericos; la logica de negocio vive en dominio/aplicacion.
- Evitar helpers tipo "cajon desastre" sin ownership.

6. Genericos: uso optimo, no dogmatico
- Usar genericos cuando reducen duplicacion y mantienen legibilidad.
- Evitar abstracciones genericas prematuras que oculten reglas del dominio.
- Exigir evidencia de beneficio (menos codigo duplicado, mejor testabilidad o extensibilidad).

7. Estructura y nomenclatura
- Convenciones claras para nombres de variables, metodos, comandos, queries, handlers y servicios.
- Limites de capa explicitos: API -> Application -> Domain -> Infrastructure.
- Prohibido referenciar Infrastructure desde Domain.

## Perfil frontend Angular (reglas explicitas)

Cuando el proyecto tenga frontend Angular, esta skill debe forzar ademas los siguientes estandares:

1. Basarse en guia oficial de Angular
- Adoptar convenciones de `https://angular.dev/style-guide` como base por defecto.
- Priorizar consistencia local del archivo/modulo cuando haya una excepcion justificada.

2. Estructura y nomenclatura
- Organizar por feature area, no por tipo tecnico (`components`, `services`, etc.).
- Un concepto principal por archivo y nombres en kebab-case para archivos.
- Mantener `*.spec.ts` junto al codigo probado.

3. Componentes y estado
- Favorecer componentes standalone en aplicaciones modernas.
- Mantener componentes enfocados en presentacion; mover logica compleja a servicios/utilidades.
- Usar signals/computed para estado local derivado cuando aporte claridad y rendimiento.

4. DI y servicios
- Preferir `inject()` sobre inyeccion por constructor cuando mejore legibilidad y tipado.
- Servicios enfocados, con responsabilidad acotada y sin side effects ocultos.

5. Routing y carga
- Definir rutas por feature con lazy loading y guards/resolvers cuando aplique.
- Usar `@defer` en vistas pesadas o below-the-fold.

6. HTTP y errores
- Usar `HttpClient` tipado y centralizar cross-cutting concerns en interceptores.
- Estandarizar manejo de errores y mapeo de respuestas en capa de datos.
- Mantener proteccion XSRF habilitada salvo excepcion documentada.

7. Seguridad
- Evitar acceso directo al DOM cuando haya alternativa de template binding.
- Usar `DomSanitizer` con criterio y auditar cualquier `bypassSecurityTrust*`.
- Mantener compilacion AOT para produccion.
- Reforzar CSP y Trusted Types cuando sea viable.

8. Performance y observabilidad frontend
- Aplicar lazy routes, `@defer`, optimizacion de imagenes y estrategia de render adecuada.
- Medir con Angular DevTools/Chrome DevTools antes de optimizar.
- Definir trazabilidad cliente-servidor con correlationId propagado en requests.

## Reglas no negociables de trazabilidad

- Toda solicitud debe tener un identificador de correlacion propagado de extremo a extremo.
- Todo evento critico debe registrar actor, accion, entidad, timestamp, resultado y causa de error.
- No registrar secretos ni datos sensibles sin redaccion/mascarado.
- Toda alerta debe mapear a un runbook operativo.
- Toda falla relevante debe quedar trazable desde sintoma hasta causa raiz.

## Prompt rapido sugerido

`Usa la skill "engineering-quality-traceability" para crear estandares de codigo y arquitectura, y dejar implementada una auditoria de extremo a extremo con trazabilidad operativa para produccion.`

## Templates de referencia en esta skill

- `templates/engineering-standards.template.md`
- `templates/dotnet-backend-standards.template.md`
- `templates/angular-frontend-standards.template.md`
- `templates/traceability-standard.template.md`
- `templates/audit-event-catalog.template.md`
- `templates/production-checklist.template.md`
- `templates/pr-quality-checklist.template.md`
- `templates/sli-slo-observability.template.md`
