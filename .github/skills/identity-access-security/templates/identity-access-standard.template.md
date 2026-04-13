# Identity and Access Standard

## Objetivo

Definir como se autentica y autoriza en el sistema de extremo a extremo.

## 1. Principios

- El backend es la autoridad final de autorizacion.
- El frontend solo aplica controles de navegacion y experiencia.
- Minimo privilegio para usuarios, servicios y procesos.
- Todo acceso sensible debe ser trazable y auditable.

## 2. Flujo de identidad (quien identifica a quien)

1. Usuario -> Proveedor de identidad: autenticacion primaria.
2. Proveedor de identidad -> Backend: emision/validacion de token.
3. Backend -> Frontend: estado de sesion y permisos efectivos.
4. Frontend -> Backend: envio de access token + correlationId.
5. Backend -> Backend/servicios: propagacion de identidad tecnica segun necesidad.

## 3. Roles, subroles y claims

- Rol: categoria funcional principal (ejemplo: Admin, Operador, Consulta).
- Subrol: especializacion de permisos dentro de un rol.
- Claims: atributos verificables para decisiones finas de autorizacion.

Reglas:

- No hardcodear autorizacion en frontend.
- Modelar permisos en backend como politicas reutilizables.
- Mantener una matriz role -> permission -> claim documentada.

## 4. Frontend (Angular u otro)

- Guards para controlar acceso visual y navegacion.
- Interceptor para:
  - adjuntar token
  - adjuntar correlationId
  - manejar 401/403 de forma centralizada
- No tomar decisiones criticas solo con estado local del browser.

## 5. Backend

Validaciones minimas por token:

- firma
- issuer
- audience
- exp/nbf
- revocacion

Autorizacion:

- Por endpoint, aplicar politica por rol/claim.
- Para operaciones criticas, verificar condicion de negocio adicional (ownership, alcance, tenant).

## 6. Access token y refresh token

- Access token: corta vida.
- Refresh token: rotado por uso.
- Detectar reuse de refresh token:
  - invalidar token family
  - forzar reautenticacion
  - registrar evento de seguridad

## 7. Integracion FNMT (si aplica)

- Validar certificado cliente en backend o pasarela de confianza.
- Verificar cadena de confianza, vigencia y revocacion (OCSP/CRL).
- Mapear identidad de certificado a usuario interno con control de unicidad.
- Auditar alta, renovacion, rechazo y revocacion de identidad ligada a certificado.

## 8. Cookies, cabeceras y transporte

- Si hay cookies de sesion/refresh: httpOnly + secure + sameSite definido.
- Forzar HTTPS extremo a extremo.
- Proteger contra CSRF segun arquitectura (token/header y/o sameSite).

## 9. Auditoria minima de seguridad

Registrar al menos:

- Login success/failure
- Logout
- Token refresh success/failure
- Reuse detectado de refresh token
- Acceso denegado por autorizacion
- Elevacion/cambio de rol
- Cambios de politicas de acceso

## 10. Checklist minimo para PR de seguridad de acceso

- Hay decision explicita de roles/subroles/claims impactados.
- Hay politica backend para cada endpoint nuevo o modificado.
- Frontend solo replica UX (no seguridad definitiva).
- Flujo de refresh token cubierto con pruebas.
- Eventos de auditoria actualizados.
