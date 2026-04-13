---
name: devops-rollout-plan
description: 'Generate comprehensive rollout plans with preflight checks, step-by-step deployment, verification signals, rollback procedures, and communication plans for infrastructure and application changes'
---

# DevOps Rollout Plan Generator

Tu objetivo es crear un plan de despliegue integral y listo para producción para cambios de infraestructura o aplicación.

## Input Requirements

Reúne estos detalles antes de generar el plan:

### Change Description
- Qué cambia (infraestructura, aplicación, configuración)
- Transición de versión o estado (from/to)
- Problema resuelto o funcionalidad añadida

### Environment Details
- Entorno objetivo (dev, staging, production, all)
- Tipo de infraestructura (Kubernetes, VMs, serverless, containers)
- Servicios afectados y dependencias
- Capacidad y escala actuales

### Constraints & Requirements
- Ventana de downtime aceptable
- Restricciones de ventana de cambio
- Requisitos de aprobación
- Consideraciones regulatorias o de cumplimiento

### Risk Assessment
- Blast radius del cambio
- Migraciones de datos o cambios de esquema
- Complejidad y seguridad del rollback
- Riesgos conocidos

## Output Format

Genera un plan de despliegue estructurado con estas secciones:

### 1. Executive Summary
- Qué, por qué, cuándo, duración
- Nivel de riesgo y tiempo de rollback
- Sistemas afectados e impacto a usuarios
- Downtime esperado

### 2. Prerequisites & Approvals
- Aprobaciones requeridas (technical lead, security, compliance, negocio)
- Recursos requeridos (capacidad, backups, monitoring, automatización de rollback)
- Backups previos al despliegue

### 3. Preflight Checks
- Validación de salud de infraestructura
- Línea base de salud de la aplicación
- Disponibilidad de dependencias
- Métricas base de monitoring
- Checklist de decisión go/no-go

### 4. Step-by-Step Rollout Procedure
**Phases**: Pre-deployment, deployment, progressive verification
- Comandos específicos para cada paso
- Validación después de cada paso
- Estimaciones de duración

### 5. Verification Signals
**Immediate** (0-2 min): Deployment success, pods/containers started, health checks passing
**Short-term** (2-5 min): Application responding, error rates acceptable, latency normal
**Medium-term** (5-15 min): Sustained metrics, stable connections, integrations working
**Long-term** (15+ min): No degradation, capacity healthy, business metrics normal

### 6. Rollback Procedure
**Decision Criteria**: When to initiate rollback
**Rollback Steps**: Automated, infrastructure revert, or full restore
**Post-Rollback Verification**: Confirm system health restored
**Communication**: Stakeholder notification

### 7. Communication Plan
- Pre-deployment (T-24h): Schedule and impact notice
- Deployment start: Commencement notice
- Progress updates: Status every X minutes
- Completion: Success confirmation
- Rollback (if needed): Issue notification

**Stakeholder Matrix**: Who to notify, when, via what method, with what content

### 8. Post-Deployment Tasks
- Immediate (1h): Verify criteria met, review logs
- Short-term (24h): Monitor metrics, review errors
- Medium-term (1 week): Post-deployment review, lessons learned

### 9. Contingency Plans
Scenarios: Partial failure, performance degradation, data inconsistency, dependency failure
For each: Symptoms, response, timeline

### 10. Contact Information
- Primary and secondary on-call
- Escalation path
- Emergency contacts (infrastructure, security, database, networking)

## Plan Customization

Adapta en función de:
- **Infrastructure Type**: Kubernetes, VMs, serverless, databases
- **Risk Level**: Low (simplified), medium (standard), high (additional gates)
- **Change Type**: Code deployment, infrastructure, configuration, data migration
- **Environment**: Production (full plan), staging (simplified), development (minimal)

## Remember

- Ten siempre un plan de rollback probado
- Comunica pronto y con frecuencia
- Monitoriza métricas, no solo logs
- Documenta todo
- Aprende de cada despliegue
- Nunca despliegues un viernes por la tarde (salvo que sea crítico)
- Nunca te saltes pasos de verificación
- Nunca asumas "it should work"
