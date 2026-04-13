# Trust Boundary Map

## Objetivo

Definir fronteras de confianza y responsabilidades de autenticacion/autorizacion en cada tramo del flujo.

## 1. Entidades y responsabilidades

- Usuario final: aporta credenciales/factor de autenticacion.
- Frontend: inicia flujo, mantiene estado de sesion de UX, aplica guards de navegacion.
- Proveedor de identidad: autentica identidad primaria y emite assertions/tokens.
- Backend API: valida token, aplica autorizacion por politicas/claims, ejecuta negocio.
- Servicios internos: consumen identidad tecnica y alcance minimo necesario.

## 2. Fronteras de confianza

1. Usuario <-> Frontend

- Riesgo: manipulacion de estado local.
- Control: nunca confiar en permisos almacenados en cliente para decisiones de seguridad.

1. Frontend <-> Backend

- Riesgo: token robado, replay, omision de cabeceras de seguridad.
- Control: HTTPS, validacion estricta de JWT, expiracion corta, revocacion, correlationId.

1. Backend <-> IdP

- Riesgo: issuer falso o metadata comprometida.
- Control: issuer/audience fijos, validacion de firma y rotacion de claves.

1. Backend <-> Servicios internos

- Riesgo: escalado de privilegios entre servicios.
- Control: tokens con scopes minimos, autorizacion por servicio, mTLS cuando aplique.

1. Certificados externos (FNMT) <-> Backend

- Riesgo: certificado revocado o no valido.
- Control: validacion de cadena de confianza, vigencia y estado de revocacion (OCSP/CRL).

## 3. Quien identifica a quien

- El IdP identifica al usuario y emite evidencia criptografica.
- El backend identifica al frontend por origen/protocolo y valida su token.
- El backend identifica al usuario por claims verificadas y reglas de negocio.
- Los servicios internos identifican al llamador por identidad tecnica y scopes.

## 4. Que dato viaja por tramo

Frontend -> Backend:

- access token
- correlationId
- metadatos de cliente permitidos

Backend -> Servicios:

- identidad tecnica
- claims minimas para autorizacion downstream
- traceId/correlationId para trazabilidad

## 5. Reglas de diseno

- Toda decision de autorizacion critica se toma en backend.
- No enviar datos sensibles en claims si no son imprescindibles.
- Minimizar alcance temporal y funcional de credenciales.
- Auditar cada cruce de frontera sensible (authz denegada, elevacion, refresh reuse).
