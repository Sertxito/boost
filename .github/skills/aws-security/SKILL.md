---
name: aws-security
description: 'Revisiones de seguridad en AWS con foco en IAM, secretos, red, cifrado, logging y postura de minimo privilegio.'
---

# AWS Security

Skill para revisar y reforzar seguridad en workloads AWS.

## Cuando usarla

- Auditorias de seguridad en cuentas/proyectos AWS.
- Revisión de IAM, Secrets Manager, KMS, red y cifrado.
- Endurecimiento antes de despliegues a produccion.

## Checklist base

1. IAM: minimo privilegio, roles claros, sin comodines innecesarios.
2. Secretos: uso de Secrets Manager/SSM, sin credenciales hardcodeadas.
3. Red: SG/NACL restrictivos, puertos y origenes justificados.
4. Cifrado: datos en reposo y en transito con KMS/TLS.
5. Logging: CloudTrail/CloudWatch activos y retenidos.
6. Deteccion: alarmas y respuesta ante eventos de seguridad.

## Guardrails

- Ningun secreto en repositorio.
- Evitar politicas IAM excesivas.
- No exponer servicios sin justificacion de negocio.

## Prompts de ejemplo

- `Usa aws-security para auditar IAM y secretos en este proyecto.`
- `Usa aws-security para revisar riesgos de red y cifrado antes de release.`
