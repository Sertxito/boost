# PR Quality Checklist Template

## Contexto del cambio

- Objetivo de negocio:
- Riesgo tecnico principal:
- Modulos impactados:

## Arquitectura y diseno

- [ ] Respeta limites de capa.
- [ ] No introduce acoplamiento innecesario.
- [ ] Clases/metodos con responsabilidad unica.
- [ ] Cambios de contrato documentados.

## Calidad de codigo

- [ ] Convenciones de naming aplicadas.
- [ ] Duplicacion reducida o justificada.
- [ ] Uso de genericos justificado.
- [ ] Helpers con ownership claro.

## Observabilidad y trazabilidad

- [ ] CorrelationId/traceId propagados.
- [ ] Logs estructurados en puntos criticos.
- [ ] Eventos de auditoria actualizados.
- [ ] Dashboards/alertas revisados.

## Seguridad y cumplimiento

- [ ] Input validation en fronteras.
- [ ] Secretos y datos sensibles protegidos.
- [ ] AuthN/AuthZ verificada si aplica.

## Verificacion

- [ ] Unit tests en verde.
- [ ] Integration tests en verde.
- [ ] Smoke test de rutas criticas.

## Rollback

- [ ] Criterio de rollback definido.
- [ ] Procedimiento de rollback documentado.
- [ ] Responsable de activacion identificado.
