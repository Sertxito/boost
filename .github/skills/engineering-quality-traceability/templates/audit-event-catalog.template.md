# Audit Event Catalog

## Objetivo

Definir eventos auditables y su contrato minimo para trazabilidad y compliance.

## Campos obligatorios por evento

- eventName
- eventVersion
- timestampUtc
- serviceName
- environment
- correlationId
- traceId
- actorType (user/system)
- actorId
- action
- entityType
- entityId
- before
- after
- outcome
- errorCode
- errorMessage

## Eventos base recomendados

| eventName | action | entityType | severity |
| --- | --- | --- | --- |
| UserAuthenticated | Login | UserSession | Info |
| UserAuthenticationFailed | LoginFailed | UserSession | Warning |
| OrderCreated | Create | Order | Info |
| OrderCancelled | Cancel | Order | Warning |
| PaymentProcessed | ProcessPayment | Payment | Info |
| PaymentFailed | ProcessPayment | Payment | Error |
| RoleChanged | UpdateRole | User | Warning |
| DataExportStarted | Export | Report | Warning |
| DataExportCompleted | Export | Report | Info |
| DataExportFailed | Export | Report | Error |

## Politica de retencion sugerida

- Seguridad y acceso: minimo 365 dias.
- Auditoria funcional critica: minimo 180 dias.
- Eventos operativos de bajo riesgo: 30-90 dias.

## Reglas de calidad del catalogo

- Todo evento nuevo debe incluir owner y consumidor.
- Toda version nueva del evento debe documentar cambios de contrato.
- Eventos criticos deben tener alerta y runbook asociado.
