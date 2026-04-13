# Traceability Standard

## Objetivo

Garantizar trazabilidad end-to-end de cada flujo critico en produccion.

## 1. IDs obligatorios

Cada request debe propagar:

- correlationId: id funcional de punta a punta.
- traceId: id de traza distribuida.
- requestId: id tecnico por solicitud.

## 2. Logging estructurado

Campos minimos por evento:

- timestamp
- level
- service
- environment
- operation
- correlationId
- traceId
- requestId
- actorId (si aplica)
- entityType
- entityId
- outcome (success/failure)
- errorCode (si aplica)
- durationMs

## 3. Reglas de logs

- No loggear secretos, tokens, passwords ni PII sin redaccion.
- Usar niveles coherentes: Debug, Information, Warning, Error, Critical.
- Todo error en API debe contener correlationId para soporte.

## 4. OpenTelemetry

- Activar instrumentacion de ASP.NET Core, HttpClient y base de datos.
- Enviar traces a backend de observabilidad (App Insights, OTLP, etc.).
- Relacionar traces con logs via traceId.

## 5. Evidencia operativa minima

Para cada incidente relevante, debe ser posible reconstruir:

- quien ejecuto la accion
- que paso
- en que entidad
- cuando ocurrio
- con que resultado
- por que fallo
