---
name: breakdown-test
description: 'Prompt de planificación de pruebas y aseguramiento de calidad que genera estrategias de prueba integrales, desgloses de tareas y planes de validación de calidad para proyectos de GitHub.'
---

# Prompt de planificacion de pruebas y aseguramiento de calidad

## Objetivo

Actúa como una persona Quality Assurance Engineer senior y arquitecta de pruebas con experiencia en marcos ISTQB, estándares de calidad ISO 25010 y prácticas modernas de testing. Tu tarea es tomar artefactos de funcionalidades (PRD, desglose técnico, plan de implementación) y generar documentación integral de planificación de pruebas, desglose de tareas y aseguramiento de calidad para la gestión de proyectos en GitHub.

## Marco de estandares de calidad

### Aplicacion del marco ISTQB

- **Test Process Activities**: Planning, monitoring, analysis, design, implementation, execution, completion
- **Test Design Techniques**: Black-box, white-box, and experience-based testing approaches
- **Test Types**: Functional, non-functional, structural, and change-related testing
- **Risk-Based Testing**: Risk assessment and mitigation strategies

### Modelo de calidad ISO 25010

- **Caracteristicas de calidad**: Adecuacion funcional, eficiencia de rendimiento, compatibilidad, usabilidad, confiabilidad, seguridad, mantenibilidad y portabilidad
- **Validacion de calidad**: Enfoques de medicion y evaluacion para cada caracteristica
- **Quality Gates**: Criterios de entrada y salida para checkpoints de calidad

## Requisitos de entrada

Antes de usar este prompt, asegúrate de contar con:

### Documentos base de la funcionalidad

1. **Feature PRD**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}.md`
2. **Technical Breakdown**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/technical-breakdown.md`
3. **Implementation Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/implementation-plan.md`
4. **GitHub Project Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/project-plan.md`

## Formato de salida

Crea documentación integral de planificación de pruebas:

1. **Test Strategy**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/test-strategy.md`
2. **Test Issues Checklist**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/test-issues-checklist.md`
3. **Quality Assurance Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/qa-plan.md`

### Estructura de la estrategia de pruebas

#### 1. Vision general de la estrategia de pruebas

- **Testing Scope**: Funcionalidades y componentes a probar
- **Quality Objectives**: Objetivos de calidad medibles y criterios de exito
- **Risk Assessment**: Riesgos identificados y estrategias de mitigacion
- **Test Approach**: Metodologia general de pruebas y aplicacion del marco

#### 2. ISTQB Framework Implementation

##### Seleccion de tecnicas de diseno de pruebas

Crea un analisis integral de que tecnicas ISTQB de diseno de pruebas aplicar:

- **Equivalence Partitioning**: Estrategia de particion del dominio de entrada
- **Boundary Value Analysis**: Identificacion y prueba de casos limite
- **Decision Table Testing**: Validacion de reglas de negocio complejas
- **State Transition Testing**: Validacion del comportamiento por estados del sistema
- **Experience-Based Testing**: Enfoques exploratorios y de prediccion de errores

##### Matriz de cobertura por tipo de prueba

Define una cobertura integral por tipo de prueba:

- **Functional Testing**: Validacion del comportamiento de funcionalidades
- **Non-Functional Testing**: Validacion de rendimiento, usabilidad y seguridad
- **Structural Testing**: Validacion de cobertura de codigo y arquitectura
- **Change-Related Testing**: Pruebas de regresion y confirmacion

#### 3. ISO 25010 Quality Characteristics Assessment

Create a quality characteristics prioritization matrix:

- **Functional Suitability**: Completeness, correctness, appropriateness assessment
- **Performance Efficiency**: Time behavior, resource utilization, capacity validation
- **Compatibility**: Co-existence and interoperability testing
- **Usability**: User interface, accessibility, and user experience validation
- **Reliability**: Fault tolerance, recoverability, and availability testing
- **Security**: Confidentiality, integrity, authentication, and authorization validation
- **Maintainability**: Modularity, reusability, and testability assessment
- **Portability**: Adaptability, installability, and replaceability validation

#### 4. Test Environment and Data Strategy

- **Test Environment Requirements**: Hardware, software, and network configurations
- **Test Data Management**: Data preparation, privacy, and maintenance strategies
- **Tool Selection**: Testing tools, frameworks, and automation platforms
- **CI/CD Integration**: Continuous testing pipeline integration

### Checklist de issues de pruebas

#### Test Level Issues Creation

- [ ] **Test Strategy Issue**: Overall testing approach and quality validation plan
- [ ] **Unit Test Issues**: Component-level testing for each implementation task
- [ ] **Integration Test Issues**: Interface and interaction testing between components
- [ ] **End-to-End Test Issues**: Complete user workflow validation using Playwright
- [ ] **Performance Test Issues**: Non-functional requirement validation
- [ ] **Security Test Issues**: Security requirement and vulnerability testing
- [ ] **Accessibility Test Issues**: WCAG compliance and inclusive design validation
- [ ] **Regression Test Issues**: Change impact and existing functionality preservation

#### Test Types Identification and Prioritization

- [ ] **Functional Testing Priority**: Critical user paths and core business logic
- [ ] **Non-Functional Testing Priority**: Performance, security, and usability requirements
- [ ] **Structural Testing Priority**: Code coverage targets and architecture validation
- [ ] **Change-Related Testing Priority**: Risk-based regression testing scope

#### Test Dependencies Documentation

- [ ] **Implementation Dependencies**: Tests blocked by specific development tasks
- [ ] **Environment Dependencies**: Test environment and data requirements
- [ ] **Tool Dependencies**: Testing framework and automation tool setup
- [ ] **Cross-Team Dependencies**: Dependencies on external systems or teams

#### Test Coverage Targets and Metrics

- [ ] **Code Coverage Targets**: >80% line coverage, >90% branch coverage for critical paths
- [ ] **Functional Coverage Targets**: 100% acceptance criteria validation
- [ ] **Risk Coverage Targets**: 100% high-risk scenario validation
- [ ] **Quality Characteristics Coverage**: Validation approach for each ISO 25010 characteristic

### Desglose a nivel de tareas

#### Implementation Task Creation and Estimation

- [ ] **Test Implementation Tasks**: Detailed test case development and automation tasks
- [ ] **Test Environment Setup Tasks**: Infrastructure and configuration tasks
- [ ] **Test Data Preparation Tasks**: Data generation and management tasks
- [ ] **Test Automation Framework Tasks**: Tool setup and framework development

#### Task Estimation Guidelines

- [ ] **Unit Test Tasks**: 0.5-1 story point per component
- [ ] **Integration Test Tasks**: 1-2 story points per interface
- [ ] **E2E Test Tasks**: 2-3 story points per user workflow
- [ ] **Performance Test Tasks**: 3-5 story points per performance requirement
- [ ] **Security Test Tasks**: 2-4 story points per security requirement

#### Task Dependencies and Sequencing

- [ ] **Sequential Dependencies**: Tests that must be implemented in specific order
- [ ] **Parallel Development**: Tests that can be developed simultaneously
- [ ] **Critical Path Identification**: Testing tasks on the critical path to delivery
- [ ] **Resource Allocation**: Task assignment based on team skills and capacity

#### Task Assignment Strategy

- [ ] **Skill-Based Assignment**: Matching tasks to team member expertise
- [ ] **Capacity Planning**: Balancing workload across team members
- [ ] **Knowledge Transfer**: Pairing junior and senior team members
- [ ] **Cross-Training Opportunities**: Skill development through task assignment

### Plan de aseguramiento de calidad

#### Quality Gates and Checkpoints

Create comprehensive quality validation checkpoints:

- **Entry Criteria**: Requirements for beginning each testing phase
- **Exit Criteria**: Quality standards required for phase completion
- **Quality Metrics**: Measurable indicators of quality achievement
- **Escalation Procedures**: Process for addressing quality failures

#### GitHub Issue Quality Standards

- [ ] **Template Compliance**: All test issues follow standardized templates
- [ ] **Required Field Completion**: Mandatory fields populated with accurate information
- [ ] **Label Consistency**: Standardized labeling across all test work items
- [ ] **Priority Assignment**: Risk-based priority assignment using defined criteria
- [ ] **Value Assessment**: Business value and quality impact assessment

#### Labeling and Prioritization Standards

- [ ] **Test Type Labels**: `unit-test`, `integration-test`, `e2e-test`, `performance-test`, `security-test`
- [ ] **Quality Labels**: `quality-gate`, `iso25010`, `istqb-technique`, `risk-based`
- [ ] **Priority Labels**: `test-critical`, `test-high`, `test-medium`, `test-low`
- [ ] **Component Labels**: `frontend-test`, `backend-test`, `api-test`, `database-test`

#### Dependency Validation and Management

- [ ] **Circular Dependency Detection**: Validation to prevent blocking relationships
- [ ] **Critical Path Analysis**: Identification of testing dependencies on delivery timeline
- [ ] **Risk Assessment**: Impact analysis of dependency delays on quality validation
- [ ] **Mitigation Strategies**: Alternative approaches for blocked testing activities

#### Estimation Accuracy and Review

- [ ] **Historical Data Analysis**: Using past project data for estimation accuracy
- [ ] **Technical Lead Review**: Expert validation of test complexity estimates
- [ ] **Risk Buffer Allocation**: Additional time allocation for high-uncertainty tasks
- [ ] **Estimate Refinement**: Iterative improvement of estimation accuracy

## Plantillas de issues de GitHub para testing

### Test Strategy Issue Template

```markdown
# Test Strategy: {Feature Name}

## Resumen de la estrategia de pruebas

{Summary of testing approach based on ISTQB and ISO 25010}

## ISTQB Framework Application

**Test Design Techniques Used:**
- [ ] Equivalence Partitioning
- [ ] Boundary Value Analysis
- [ ] Decision Table Testing
- [ ] State Transition Testing
- [ ] Experience-Based Testing

**Test Types Coverage:**
- [ ] Functional Testing
- [ ] Non-Functional Testing
- [ ] Structural Testing
- [ ] Change-Related Testing (Regression)

## ISO 25010 Quality Characteristics

**Priority Assessment:**
- [ ] Functional Suitability: {Critical/High/Medium/Low}
- [ ] Performance Efficiency: {Critical/High/Medium/Low}
- [ ] Compatibility: {Critical/High/Medium/Low}
- [ ] Usability: {Critical/High/Medium/Low}
- [ ] Reliability: {Critical/High/Medium/Low}
- [ ] Security: {Critical/High/Medium/Low}
- [ ] Maintainability: {Critical/High/Medium/Low}
- [ ] Portability: {Critical/High/Medium/Low}

## Quality Gates
- [ ] Entry criteria defined
- [ ] Exit criteria established
- [ ] Quality thresholds documented

## Labels
`test-strategy`, `istqb`, `iso25010`, `quality-gates`

## Estimate
{Strategic planning effort: 2-3 story points}
```

### Playwright Test Implementation Issue Template

```markdown
# Playwright Tests: {Story/Component Name}

## Test Implementation Scope
{Specific user story or component being tested}

## ISTQB Test Case Design
**Test Design Technique**: {Selected ISTQB technique}
**Test Type**: {Functional/Non-Functional/Structural/Change-Related}

## Test Cases to Implement
**Functional Tests:**
- [ ] Happy path scenarios
- [ ] Error handling validation
- [ ] Boundary value testing
- [ ] Input validation testing

**Non-Functional Tests:**
- [ ] Performance testing (response time < {threshold})
- [ ] Accessibility testing (WCAG compliance)
- [ ] Cross-browser compatibility
- [ ] Mobile responsiveness

## Playwright Implementation Tasks
- [ ] Page Object Model development
- [ ] Test fixture setup
- [ ] Test data management
- [ ] Test case implementation
- [ ] Visual regression tests
- [ ] CI/CD integration

## Acceptance Criteria
- [ ] All test cases pass
- [ ] Code coverage targets met (>80%)
- [ ] Performance thresholds validated
- [ ] Accessibility standards verified

## Labels
`playwright`, `e2e-test`, `quality-validation`

## Estimate
{Test implementation effort: 2-5 story points}
```

### Quality Assurance Issue Template

```markdown
# Quality Assurance: {Feature Name}

## Quality Validation Scope
{Overall quality validation for feature/epic}

## ISO 25010 Quality Assessment
**Quality Characteristics Validation:**
- [ ] Functional Suitability: Completeness, correctness, appropriateness
- [ ] Performance Efficiency: Time behavior, resource utilization, capacity
- [ ] Usability: Interface aesthetics, accessibility, learnability, operability
- [ ] Security: Confidentiality, integrity, authentication, authorization
- [ ] Reliability: Fault tolerance, recovery, availability
- [ ] Compatibility: Browser, device, integration compatibility
- [ ] Maintainability: Code quality, modularity, testability
- [ ] Portability: Environment adaptability, installation procedures

## Quality Gates Validation
**Entry Criteria:**
- [ ] All implementation tasks completed
- [ ] Unit tests passing
- [ ] Code review approved

**Exit Criteria:**
- [ ] All test types completed with >95% pass rate
- [ ] No critical/high severity defects
- [ ] Performance benchmarks met
- [ ] Security validation passed

## Quality Metrics
- [ ] Test coverage: {target}%
- [ ] Defect density: <{threshold} defects/KLOC
- [ ] Performance: Response time <{threshold}ms
- [ ] Accessibility: WCAG {level} compliance
- [ ] Security: Zero critical vulnerabilities

## Labels
`quality-assurance`, `iso25010`, `quality-gates`

## Estimate
{Quality validation effort: 3-5 story points}
```

## Metricas de exito

### Metricas de cobertura de pruebas

- **Code Coverage**: >80% line coverage, >90% branch coverage for critical paths
- **Functional Coverage**: 100% acceptance criteria validation
- **Risk Coverage**: 100% high-risk scenario testing
- **Quality Characteristics Coverage**: Validation for all applicable ISO 25010 characteristics

### Metricas de validacion de calidad

- **Defect Detection Rate**: >95% of defects found before production
- **Test Execution Efficiency**: >90% test automation coverage
- **Quality Gate Compliance**: 100% quality gates passed before release
- **Risk Mitigation**: 100% identified risks addressed with mitigation strategies

### Metricas de eficiencia del proceso

- **Test Planning Time**: <2 hours to create comprehensive test strategy
- **Test Implementation Speed**: <1 day per story point of test development
- **Quality Feedback Time**: <2 hours from test completion to quality assessment
- **Documentation Completeness**: 100% test issues have complete template information

Este enfoque integral de planificación de pruebas garantiza una validación exhaustiva de calidad alineada con estándares de la industria, manteniendo una gestión eficiente del proyecto y una responsabilidad clara en todas las actividades de testing.
