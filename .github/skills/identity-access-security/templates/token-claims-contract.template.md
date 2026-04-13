# Plantilla de Contrato de Tokens y Claims

<!-- markdownlint-disable MD060 -->

## Contexto

- IdP:
- Emisor esperado:
- Audiencia esperada:
- Algoritmo de firma:

## Claims estandar

| Claim | Requerido | Tipo | Ejemplo | Uso |
| --- | --- | --- | --- | --- |
| sub | si | string | user-123 | Identidad unica |
| iss | si | string | <https://idp.example> | Validacion de emisor |
| aud | si | string/array | api://my-api | Validacion de audiencia |
| exp | si | number | 1735689600 | Expiracion |
| nbf | recomendado | number | 1735686000 | Not-before |
| iat | recomendado | number | 1735686000 | Emision |
| jti | recomendado | string | uuid | Trazabilidad/revocacion |

## Claims de autorizacion

| Claim | Requerido | Fuente | Regla de validacion |
| --- | --- | --- | --- |
| roles |  |  |  |
| permissions |  |  |  |
| tenant |  |  |  |

## Reglas de backend

- Rechazar token si falla firma/issuer/audience/exp/nbf.
- Rechazar claims fuera de contrato.
- No confiar en claims no documentadas.

## Estrategia de refresh token

- Rotacion por uso: si/no
- Deteccion de reutilizacion: si/no
- Revocacion por cambio de contrasena: si/no
- Revocacion por cambio de rol: si/no

## Auditoria requerida

- auth_token_validated
- auth_token_rejected
- auth_claims_validation_failed
- auth_token_refresh_reuse_detected
