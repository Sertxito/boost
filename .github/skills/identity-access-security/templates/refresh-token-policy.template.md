# Refresh Token Policy

## Objetivo

Definir una politica segura y operable de renovacion de sesion.

## 1. Modelo de tokens

- Access token: corta duracion.
- Refresh token: mayor duracion, de un solo uso por rotacion.
- Token family: cadena de refresh tokens asociados a una sesion.

## 2. Emision y almacenamiento

- Access token en memoria de aplicacion (preferible en frontend SPA).
- Refresh token en cookie httpOnly + secure + sameSite acorde al flujo.
- No guardar refresh token en localStorage/sessionStorage.

## 3. Rotacion

- Cada uso valido de refresh token emite:
  - nuevo access token
  - nuevo refresh token
- El refresh token previo queda invalidado inmediatamente.

## 4. Reuse detection

Si se detecta uso de refresh token ya invalidado:

1. Invalidar token family completa.
2. Revocar sesiones activas asociadas.
3. Forzar reautenticacion del usuario.
4. Emitir evento de seguridad de severidad alta.

## 5. Revocacion

Disparadores minimos:

- Logout explicito.
- Cambio de password.
- Cambio de rol/permisos criticos.
- Sospecha de compromiso de sesion.

## 6. Duraciones recomendadas

- Access token: 5 a 15 minutos.
- Refresh token: definido por riesgo y normativa (por ejemplo 7-30 dias con controles de rotacion y revocacion).

## 7. Validaciones backend

- Validar firma, issuer, audience, exp y nbf.
- Verificar estado de revocacion y pertenencia a token family activa.
- Aplicar rate limiting al endpoint de refresh.

## 8. Trazabilidad obligatoria

Registrar:

- refresh_success
- refresh_failed
- refresh_reuse_detected
- refresh_family_revoked

Campos minimos:

- userId
- sessionId
- tokenFamilyId
- correlationId
- traceId
- ip y userAgent (cuando politica lo permita)
- timestamp y outcome
