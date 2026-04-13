---
name: identity-access-security
description: "Skill de autenticacion y autorizacion para entornos productivos. Define estrategia de identidad de extremo a extremo: roles/subroles, claims, fronteras de confianza, guards frontend, validacion de tokens en backend, rotacion de refresh token, revocacion, integracion con proveedores externos (incluida FNMT cuando aplique), auditoria y trazabilidad de eventos de seguridad. Usala para diseno, implementacion, fortalecimiento o revision de seguridad de identidad y acceso."
---

# Seguridad de Identidad y Acceso

Skill para estandarizar autenticacion, autorizacion y trazabilidad de acceso en aplicaciones modernas.

## Cuando usar esta skill

Usala cuando se pida alguno de estos temas:

- Estrategia de autenticacion y autorizacion de extremo a extremo
- Roles, subroles, permisos y claims
- Guards de rutas en frontend
- Validacion y autorizacion por claims en backend
- Access token y refresh token (rotacion y revocacion)
- Integracion con identidad externa (OIDC, SAML, certificados, FNMT)
- fortalecimiento de sesiones, cookies y flujos de login/logout
- Auditoria de eventos de seguridad y trazabilidad de acceso

## Artefactos esperados

Las plantillas canonicas viven en `templates/` dentro de esta skill.
Este Boost se mantiene en modo solo-plantillas.
Los archivos en `docs/` se generan en el proyecto destino al aplicar la skill.

1. docs/security/identity-access-standard.md
- Modelo de identidad y flujo de extremo a extremo
- Roles/subroles/claims y politicas de autorizacion
- Estrategia de tokens y sesiones
- Reglas frontend/backend

2. docs/security/trust-boundary-map.md
- Quien autentica a quien
- Fronteras de confianza
- Que dato viaja por cada tramo
- Riesgos y controles por frontera

3. docs/security/refresh-token-policy.md
- Vida de access token y refresh token
- Rotacion, revocacion y deteccion de reuse
- Almacenamiento seguro y logout global

4. docs/observability/security-audit-events.md
- Catalogo de eventos de autenticacion/autorizacion
- Campos obligatorios para forense
- Alertas y runbooks de seguridad

## Reglas no negociables

- El frontend nunca es autoridad de autorizacion; solo mejora UX.
- Todo acceso sensible se valida en backend con identidad y claims vigentes.
- Access tokens cortos y refresh tokens rotados de forma segura.
- Cualquier reuse de refresh token invalida la sesion/token family.
- Todo cambio de rol/permiso y toda denegacion debe auditarse.
- No incluir secretos en JWT; usar claims minimos necesarios.

## Modelo recomendado (alto nivel)

1. Autenticacion
- Preferir OIDC/OAuth2 con proveedor confiable.
- Si aplica certificado FNMT, validar cadena, vigencia y estado de revocacion (OCSP/CRL) en backend.
- Asociar identidad externa a identidad interna con mapping controlado.

2. Autorizacion
- RBAC base (roles) + permisos por claim para granularidad.
- Subroles como agregados de permisos, no como hardcode en UI.
- Politicas de autorizacion centralizadas en backend.

3. Frontend
- Guards para control de navegacion, no para seguridad definitiva.
- Interceptor HTTP para bearer token/correlationId y renovacion controlada.
- Manejo consistente de 401/403 y flujos de reautenticacion.

4. Backend
- Validar issuer, audience, firma, expiracion y not-before del token.
- Aplicar autorizacion por politicas/claims por endpoint.
- Rechazar tokens invalidados y claims fuera de contrato.

5. Sesiones y tokens
- Access token de corta duracion.
- Refresh token en cookie segura (httpOnly, secure, sameSite segun caso) o almacenamiento seguro equivalente.
- Rotacion por uso, revocacion por logout/compromiso y trazabilidad de token family.

## Prompt rapido sugerido

Usa la skill "identity-access-security" para definir estrategia completa de autenticacion/autorizacion (roles, subroles, claims, guards frontend, validacion backend, refresh token y federacion FNMT) con trazabilidad de extremo a extremo.

## Templates de referencia en esta skill

- `templates/identity-access-standard.template.md`
- `templates/trust-boundary-map.template.md`
- `templates/refresh-token-policy.template.md`
- `templates/security-audit-events.template.md`
- `templates/role-permission-matrix.template.md`
- `templates/token-claims-contract.template.md`
- `templates/fnmt-integration-checklist.template.md`
