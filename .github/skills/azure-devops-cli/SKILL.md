---
name: azure-devops-cli
description: Gestiona recursos de Azure DevOps mediante CLI, incluyendo proyectos, repositorios, pipelines, builds, pull requests, work items, artefactos y service endpoints. Usar cuando se trabaje con Azure DevOps, comandos az, automatización DevOps, CI/CD o cuando la persona usuaria mencione Azure DevOps CLI.
---

# Azure DevOps CLI

Gestiona recursos de Azure DevOps usando Azure CLI con la extensión de Azure DevOps.

**Version de CLI:** 2.81.0 (vigente en 2025)

## Prerrequisitos

```bash
# Instalar Azure CLI
brew install azure-cli  # macOS
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash  # Linux

# Instalar la extension de Azure DevOps
az extension add --name azure-devops
```

## Autenticación

```bash
# Iniciar sesion con token PAT
az devops login --organization https://dev.azure.com/{org} --token YOUR_PAT_TOKEN

# Establecer organizacion y proyecto por defecto (evita repetir --org/--project)
# Nota: La URL legacy https://{org}.visualstudio.com debe reemplazarse por https://dev.azure.com/{org}
az devops configure --defaults organization=https://dev.azure.com/{org} project={project}

# Listar configuracion actual
az devops configure --list
```

## Estructura de la CLI

```
az devops          # Main DevOps commands
├── admin          # Administration (banner)
├── extension      # Extension management
├── project        # Team projects
├── security       # Security operations
│   ├── group      # Security groups
│   └── permission # Security permissions
├── service-endpoint # Service connections
├── team           # Teams
├── user           # Users
├── wiki           # Wikis
├── configure      # Set defaults
├── invoke         # Invoke REST API
├── login          # Authenticate
└── logout         # Clear credentials

az pipelines       # Azure Pipelines
├── agent          # Agents
├── build          # Builds
├── folder         # Pipeline folders
├── pool           # Agent pools
├── queue          # Agent queues
├── release        # Releases
├── runs           # Pipeline runs
├── variable       # Pipeline variables
└── variable-group # Variable groups

az boards          # Azure Boards
├── area           # Area paths
├── iteration      # Iterations
└── work-item      # Work items

az repos           # Azure Repos
├── import         # Git imports
├── policy         # Branch policies
├── pr             # Pull requests
└── ref            # Git references

az artifacts       # Azure Artifacts
└── universal      # Universal Packages
```

## Archivos de referencia

Lee el archivo de referencia correspondiente según la tarea de la persona usuaria. Cada archivo contiene sintaxis completa de comandos y ejemplos de su dominio.

| Archivo | Cuando leerlo | Cubre |
|---|---|---|
| `references/repos-and-prs.md` | Repositorios, ramas, pull requests, politicas de rama | Repositorios, importacion, PRs (crear/listar/votar/revisores/politicas), referencias Git, politicas de rama |
| `references/pipelines-and-builds.md` | Pipelines, builds, releases, artefactos | CRUD de pipelines, ejecuciones, builds, releases, descarga/subida de artefactos |
| `references/boards-and-iterations.md` | Work items, sprints, rutas de area | Work items (WIQL/crear/actualizar/relaciones), rutas de area, iteraciones, iteraciones de equipo |
| `references/variables-and-agents.md` | Variables de pipeline, pools de agentes | Variables de pipeline, grupos de variables, carpetas de pipeline, pools/colas de agentes |
| `references/org-and-security.md` | Proyectos, equipos, usuarios, permisos, wikis | Proyectos, extensiones, equipos, usuarios, grupos/permisos de seguridad, service endpoints, wikis, administracion |
| `references/advanced-usage.md` | Formato de salida, consultas JMESPath | Formatos de salida, consultas JMESPath (basicas y avanzadas), argumentos globales, parametros comunes, alias Git |
| `references/workflows-and-patterns.md` | Scripts de automatizacion, buenas practicas, manejo de errores | Workflows comunes, buenas practicas, manejo de errores, patrones de scripting, ejemplos reales |
