---
name: aws-deploy
description: 'Planificacion y ejecucion de despliegues en AWS con prechecks, estrategia de despliegue, verificacion, rollback y comunicacion operativa.'
---

# AWS Deploy

Skill para preparar y ejecutar despliegues en AWS con seguridad operativa.

## Cuando usarla

- Despliegues a ECS, EKS, Lambda o EC2.
- Cambios de infraestructura con impacto en produccion.
- Necesidad de plan de rollback y señales de salud.

## Flujo recomendado

1. Prechecks: permisos, capacidad, dependencias y ventanas.
2. Estrategia: rolling, blue/green o canary segun riesgo.
3. Verificacion: metricas, logs, health checks y errores.
4. Rollback: criterios claros, comando/procedimiento y responsables.
5. Cierre: lecciones aprendidas y actualizacion de source of truth.

## Guardrails

- No desplegar sin prechecks.
- No desplegar sin criterio de rollback.
- Priorizar cambios pequenos y reversibles.

## Prompts de ejemplo

- `Usa aws-deploy para preparar un despliegue canary de este servicio en ECS con rollback.`
- `Usa aws-deploy y define runbook de despliegue para Lambda con verificacion post-release.`
