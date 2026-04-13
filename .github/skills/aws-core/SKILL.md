---
name: aws-core
description: 'Skill base para trabajar en AWS con un enfoque legacy->moderno: discovery, plan incremental, ejecucion segura, validacion y mantenimiento de fuente de verdad.'
---

# AWS Core

Skill troncal para iniciar trabajo en AWS manteniendo el mismo modelo operativo del Boost: entender primero, planificar por fases, ejecutar en lotes pequenos y validar con evidencia.

## Cuando usarla

- Inicio de trabajo en un proyecto AWS sin contexto claro.
- Migraciones o modernizaciones con riesgo funcional/operativo.
- Necesidad de establecer baseline, riesgos y source of truth.

## Protocolo recomendado

1. Discovery: mapear servicios, dependencias y entrypoints.
2. Baseline: estado actual de build, despliegue, observabilidad y seguridad.
3. Source of truth: documento vivo de estado, decisiones y backlog.
4. Plan por fases: cambios pequenos con criterios de salida.
5. Ejecucion y validacion: pruebas, evidencia y riesgos residuales.

## Guardrails

- No cambios destructivos sin plan de rollback.
- No secretos en codigo.
- Minimo privilegio en IAM.
- Preferir IaC para cambios repetibles.

## Prompts de ejemplo

- `Usa aws-core para analizar este entorno AWS y definir plan de modernizacion por fases.`
- `Usa aws-core y genera source of truth inicial con riesgos y backlog priorizado.`
