---
description: 'Buenas prácticas para archivos YAML de Azure DevOps Pipeline'
applyTo: '**/azure-pipelines.yml, **/azure-pipelines*.yml, **/*.pipeline.yml'
---

# Buenas Practicas de YAML para Azure DevOps Pipeline

## Lineamientos Generales

- Usa sintaxis YAML de forma consistente con indentacion correcta (2 espacios)
- Incluye siempre nombres y display names significativos para pipelines, stages, jobs y steps
- Implementa manejo de errores adecuado y ejecucion condicional
- Usa variables y parametros para que los pipelines sean reutilizables y mantenibles
- Sigue el principio de minimo privilegio para service connections y permisos
- Incluye logging y diagnostico completos para facilitar troubleshooting

## Estructura del Pipeline

- Organiza pipelines complejos usando stages para mejor visualizacion y control
- Usa jobs para agrupar steps relacionados y habilitar ejecucion en paralelo cuando sea posible
- Implementa dependencias correctas entre stages y jobs
- Usa templates para componentes reutilizables del pipeline
- Mantén los archivos de pipeline enfocados y modulares: divide pipelines grandes en multiples archivos

## Buenas Practicas de Build

- Usa versiones especificas de agent pool e imagenes VM para mantener consistencia
- Cachea dependencias (npm, NuGet, Maven, etc.) para mejorar el rendimiento del build
- Implementa gestion de artifacts con nombres significativos y politicas de retencion
- Usa variables de build para numeros de version y metadata de build
- Incluye quality gates de codigo (linting, testing, security scans)
- Asegura que los builds sean reproducibles e independientes del entorno

## Integracion de Testing

- Ejecuta unit tests como parte del proceso de build
- Publica resultados de pruebas en formatos estandar (JUnit, VSTest, etc.)
- Incluye reportes de code coverage y quality gates
- Implementa pruebas de integracion y end-to-end en los stages adecuados
- Usa test impact analysis cuando este disponible para optimizar la ejecucion de pruebas
- Aplica fail fast ante fallos de tests para dar feedback rapido

## Consideraciones de Seguridad

- Usa Azure Key Vault para configuracion sensible y secretos
- Implementa manejo de secretos adecuado con variable groups
- Usa service connections con los permisos minimos necesarios
- Habilita security scans (vulnerabilidades de dependencias, analisis estatico)
- Implementa approval gates para despliegues a produccion
- Usa managed identities cuando sea posible en lugar de service principals

## Estrategias de Despliegue

- Implementa promocion de entornos adecuada (dev → staging → production)
- Usa deployment jobs con target correcto por entorno
- Implementa estrategias de despliegue blue-green o canary cuando corresponda
- Incluye mecanismos de rollback y health checks
- Usa infrastructure as code (ARM, Bicep, Terraform) para despliegues consistentes
- Implementa gestion de configuracion adecuada por entorno

## Gestion de Variables y Parametros

- Usa variable groups para compartir configuracion entre pipelines
- Implementa runtime parameters para una ejecucion flexible del pipeline
- Usa variables condicionales segun ramas o entornos
- Protege variables sensibles y marcarlas como secretos
- Documenta el proposito de las variables y los valores esperados
- Usa variable templates para logica compleja de variables

## Optimizacion de Rendimiento

- Usa jobs en paralelo y estrategias de matriz cuando corresponda
- Implementa estrategias de cache adecuadas para dependencias y salidas de build
- Usa shallow clone para operaciones de Git cuando no se necesite el historial completo
- Optimiza builds de imagenes Docker con multi-stage builds y cache de capas
- Monitorea el rendimiento del pipeline y optimiza cuellos de botella
- Usa de forma eficiente los pipeline resource triggers

## Monitoreo y Observabilidad

- Incluye logging completo en todo el pipeline
- Usa Azure Monitor y Application Insights para seguimiento de despliegues
- Implementa estrategias de notificacion adecuadas para fallos y exitos
- Incluye health checks de despliegue y triggers de rollback automatico
- Usa analitica de pipeline para identificar oportunidades de mejora
- Documenta el comportamiento del pipeline y los pasos de troubleshooting

## Templates y Reutilizacion

- Crea pipeline templates para patrones comunes
- Usa extends templates para herencia completa del pipeline
- Implementa step templates para secuencias de tareas reutilizables
- Usa variable templates para logica compleja de variables
- Versiona templates adecuadamente para mantener estabilidad
- Documenta los parametros de templates y ejemplos de uso

## Estrategia de Ramas y Triggers

- Implementa triggers apropiados para diferentes tipos de rama
- Usa path filters para disparar builds solo cuando cambien archivos relevantes
- Configura triggers de CI/CD adecuados para ramas main/master
- Usa triggers de pull request para validacion de codigo
- Implementa triggers programados para tareas de mantenimiento
- Considera resource triggers para escenarios multi-repositorio

## Estructura de Ejemplo

```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
      - main
      - develop
  paths:
    exclude:
      - docs/*
      - README.md

variables:
  - group: shared-variables
  - name: buildConfiguration
    value: 'Release'

stages:
  - stage: Build
    displayName: 'Build and Test'
    jobs:
      - job: Build
        displayName: 'Build Application'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: UseDotNet@2
            displayName: 'Use .NET SDK'
            inputs:
              version: '8.x'
          
          - task: DotNetCoreCLI@2
            displayName: 'Restore dependencies'
            inputs:
              command: 'restore'
              projects: '**/*.csproj'
          
          - task: DotNetCoreCLI@2
            displayName: 'Build application'
            inputs:
              command: 'build'
              projects: '**/*.csproj'
              arguments: '--configuration $(buildConfiguration) --no-restore'

  - stage: Deploy
    displayName: 'Deploy to Staging'
    dependsOn: Build
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
    jobs:
      - deployment: DeployToStaging
        displayName: 'Deploy to Staging Environment'
        environment: 'staging'
        strategy:
          runOnce:
            deploy:
              steps:
                - download: current
                  displayName: 'Download drop artifact'
                  artifact: drop
                - task: AzureWebApp@1
                  displayName: 'Deploy to Azure Web App'
                  inputs:
                    azureSubscription: 'staging-service-connection'
                    appType: 'webApp'
                    appName: 'myapp-staging'
                    package: '$(Pipeline.Workspace)/drop/**/*.zip'
```

## Anti-Patrones Comunes a Evitar

- Hardcodear valores sensibles directamente en archivos YAML
- Usar triggers demasiado amplios que provocan builds innecesarios
- Mezclar logica de build y despliegue en un unico stage
- No implementar manejo de errores y limpieza adecuados
- Usar versiones de tasks deprecadas sin planes de actualizacion
- Crear pipelines monoliticos dificiles de mantener
- No usar convenciones de nombres claras
- Ignorar buenas practicas de seguridad del pipeline
