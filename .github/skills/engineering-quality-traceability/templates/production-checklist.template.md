# Production Checklist

## Objetivo

Estandar minimo antes de desplegar cambios a produccion.

## 1. Calidad de codigo

- Convenciones de nombres respetadas.
- Cambios alineados a arquitectura por capas.
- Clases con responsabilidad unica.
- Sin logica critica duplicada sin justificacion.

## 2. API y contratos

- Swagger/OpenAPI actualizado.
- Endpoints documentados con responses esperadas.
- Versionado de API respetado cuando hay breaking changes.

## 3. Seguridad minima

- Secretos fuera del codigo.
- Validacion de input en fronteras.
- AuthN/AuthZ verificada en endpoints sensibles.
- Logs sin exposicion de datos sensibles.

## 4. Observabilidad y auditoria

- correlationId/traceId/requestId propagados.
- Logs estructurados en operaciones criticas.
- Eventos de auditoria actualizados en catalogo.
- Dashboards y alertas revisados.

## 5. Pruebas y verificacion

- Unit tests relevantes en verde.
- Integration tests relevantes en verde.
- Smoke test de endpoints criticos.
- Health checks operativos.

## 6. Despliegue y rollback

- Plan de despliegue definido.
- Plan de rollback probado o validado.
- Runbook actualizado con responsables.
- Criterios de rollback definidos (errores, latencia, disponibilidad).

## 7. Cierre

- Evidencia de validacion adjunta al PR.
- Riesgos residuales declarados.
- Aprobacion final registrada.
