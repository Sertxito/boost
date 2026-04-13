# Plantilla de Observabilidad SLI/SLO

## Servicio

- Nombre:
- Responsable:
- Entorno:

## SLI definidos

1. Disponibilidad

- SLI: porcentaje de solicitudes exitosas.
- Fuente: metricas de pasarela/API.

1. Latencia

- SLI: p95 y p99 de endpoint critico.
- Fuente: APM/tracing.

1. Errores

- SLI: tasa de 5xx/errores de negocio.
- Fuente: logs estructurados + metricas.

## SLO propuestos

- Disponibilidad mensual: ____%
- Latencia p95: <= ____ ms
- Tasa de error: <= ____%

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
