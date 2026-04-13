---
name: aws-iac
description: 'Infraestructura como codigo en AWS con Terraform y CloudFormation: plan, validacion, drift detection, cambios seguros y rollback.'
---

# AWS IaC

Skill para gestionar infraestructura AWS de forma declarativa y segura.

## Cuando usarla

- Provisionar o modificar infraestructura en AWS.
- Migrar cambios manuales a Terraform/CloudFormation.
- Estandarizar pipelines de infraestructura.

## Flujo recomendado

1. Discovery de estado real y drift.
2. Definicion de cambios IaC pequenos.
3. `plan`/`what-if` y revision de impacto.
4. Aplicacion controlada por entorno.
5. Verificacion post-cambio y rollback documentado.

## Guardrails

- Sin cambios manuales en produccion sin reflejar en IaC.
- `plan` obligatorio antes de `apply`.
- Cambios de red, IAM y datos con doble validacion.

## Prompts de ejemplo

- `Usa aws-iac para convertir este cambio manual en Terraform con plan y rollback.`
- `Usa aws-iac para revisar drift entre AWS y CloudFormation.`
