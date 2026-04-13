---
name: aws-cost
description: 'Optimizacion de coste en AWS: analisis de consumo, deteccion de desperdicio, rightsizing y plan de ahorro con impacto medible.'
---

# AWS Cost

Skill para analizar gasto en AWS y proponer optimizaciones con riesgo controlado.

## Cuando usarla

- Aumento de factura sin causa clara.
- Revisiones FinOps periodicas.
- Preparacion de presupuestos y objetivos de ahorro.

## Flujo recomendado

1. Baseline de coste por cuenta, servicio y etiqueta.
2. Deteccion de desperdicio (idle, sobredimensionado, retencion excesiva).
3. Recomendaciones priorizadas por impacto y riesgo.
4. Plan de implementacion por oleadas.
5. Seguimiento de ahorro real vs proyectado.

## Guardrails

- No optimizar coste rompiendo SLO/SLA.
- Validar capacidad antes de rightsizing.
- Todo ahorro debe tener metrica verificable.

## Prompts de ejemplo

- `Usa aws-cost para detectar top cost drivers y plan de ahorro en 30 dias.`
- `Usa aws-cost para rightsizing de EC2/RDS con riesgo bajo.`
