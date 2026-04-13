# SLI/SLO Observability Template

## Servicio

- Nombre:
- Owner:
- Entorno:

## SLI definidos

1. Disponibilidad
- SLI: porcentaje de requests exitosas.
- Fuente: metricas de gateway/API.

2. Latencia
- SLI: p95 y p99 de endpoint critico.
- Fuente: APM/tracing.

3. Errores
- SLI: tasa de 5xx/errores de negocio.
- Fuente: logs estructurados + metricas.

## SLO propuestos

- Disponibilidad mensual: ____%
- Latencia p95: <= ____ ms
- Error rate: <= ____%

## Alertas

- Alerta 1:
  - Condicion:
  - Severidad:
  - Canal:
- Alerta 2:
  - Condicion:
  - Severidad:
  - Canal:

## Runbooks asociados

- Runbook latencia:
- Runbook errores:
- Runbook caida parcial/total:

## Trazabilidad

- CorrelationId obligatorio: si/no
- TraceId obligatorio: si/no
- Campos minimos de log:
