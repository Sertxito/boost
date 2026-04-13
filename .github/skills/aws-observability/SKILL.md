---
name: aws-observability
description: 'Observabilidad en AWS: metricas, logs, trazas, alarmas y tableros para operar servicios con feedback rapido.'
---

# AWS Observability

Skill para disenar observabilidad operativa en servicios desplegados en AWS.

## Cuando usarla

- Falta de visibilidad en incidentes o degradaciones.
- Necesidad de SLI/SLO y alarmas accionables.
- Estandarizacion de logs, metricas y trazas.

## Flujo recomendado

1. Definir senales: disponibilidad, latencia, errores y saturacion.
2. Instrumentar: logs estructurados, metricas de negocio y trazas.
3. Alertar: umbrales claros, ruido bajo y accion concreta.
4. Visualizar: dashboards por servicio y por flujo de negocio.
5. Revisar: postmortem sin culpa y mejora continua.

## Guardrails

- No alertas sin accion clara.
- Evitar dashboards sin propietario.
- Integrar observabilidad al ciclo de despliegue.

## Prompts de ejemplo

- `Usa aws-observability para definir SLI/SLO y alarmas de este servicio.`
- `Usa aws-observability para crear un plan de logs y metricas en CloudWatch.`
