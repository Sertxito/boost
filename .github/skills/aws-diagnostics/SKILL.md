---
name: aws-diagnostics
description: 'Diagnostico de incidencias en AWS con enfoque sistematico: sintomas, señales, causa raiz, mitigacion y prevencion.'
---

# AWS Diagnostics

Skill para investigar incidentes y degradaciones en workloads AWS.

## Cuando usarla

- Caidas, latencia alta o errores intermitentes.
- Fallos de despliegue o dependencia entre servicios.
- Incidencias de red, permisos o configuracion.

## Flujo recomendado

1. Definir sintoma y ventana temporal.
2. Correlacionar logs, metricas, eventos y despliegues.
3. Aislar causa raiz tecnica.
4. Aplicar mitigacion segura.
5. Registrar acciones preventivas y actualizar source of truth.

## Guardrails

- Evitar cambios de alto riesgo durante incidente sin rollback.
- Priorizar restaurar servicio antes de optimizacion.
- Documentar cronologia y evidencia tecnica.

## Prompts de ejemplo

- `Usa aws-diagnostics para analizar errores 5xx en API Gateway + Lambda.`
- `Usa aws-diagnostics para incidentes de latencia en EKS y dependencia RDS.`
