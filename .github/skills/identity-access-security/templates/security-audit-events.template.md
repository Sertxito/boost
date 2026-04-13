# Security Audit Events

## Objetivo

Estandarizar eventos de seguridad para deteccion, forense y cumplimiento.

## 1. Campos obligatorios

- eventName
- eventVersion
- severity
- timestampUtc
- serviceName
- environment
- correlationId
- traceId
- actorType
- actorId
- sessionId
- action
- resourceType
- resourceId
- outcome
- reasonCode
- ip
- userAgent

## 2. Eventos minimos requeridos

| eventName | severity | Descripcion |
| --- | --- | --- |
| auth_login_success | Info | Login autenticado |
| auth_login_failed | Warning | Login fallido |
| auth_logout | Info | Cierre de sesion |
| auth_token_refresh_success | Info | Refresh correcto |
| auth_token_refresh_failed | Warning | Refresh fallido |
| auth_token_refresh_reuse_detected | Error | Reuso de refresh token detectado |
| auth_access_denied | Warning | Denegacion por autorizacion |
| auth_role_changed | Warning | Cambio de rol/subrol |
| auth_claims_validation_failed | Error | Claims invalidas o fuera de contrato |
| auth_certificate_fnmt_validation_failed | Error | Fallo validando certificado FNMT |

## 3. Correlacion con operaciones

- Cada evento de seguridad debe correlacionarse con logs de aplicacion via correlationId/traceId.
- Un incidente debe poder reconstruirse desde el primer intento de autenticacion hasta la denegacion, escalado o exito final.

## 4. Alertas recomendadas

- Pico de auth_login_failed por usuario/IP.
- auth_token_refresh_reuse_detected >= 1.
- auth_role_changed fuera de ventana operativa.
- auth_certificate_fnmt_validation_failed repetido.

## 5. Retencion sugerida

- Eventos de seguridad criticos: 365 dias o segun normativa aplicable.
- Eventos de baja severidad: 90-180 dias segun coste/riesgo.
