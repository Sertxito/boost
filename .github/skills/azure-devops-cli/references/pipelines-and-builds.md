# Pipelines, Builds y Releases

<!-- markdownlint-disable-file -->

## Tabla de contenido

- [Pipelines](#pipelines)
- [Ejecuciones de pipeline](#ejecuciones-de-pipeline)
- [Builds](#builds)
- [Definiciones de build](#definiciones-de-build)
- [Releases](#releases)
- [Definiciones de release](#definiciones-de-release)
- [Paquetes universales (artefactos)](#paquetes-universales-artefactos)

---

## Pipelines

### Listar pipelines

```bash
az pipelines list --output table
az pipelines list --query "[?name=='myPipeline']"
az pipelines list --folder-path 'folder/subfolder'
```

### Crear pipeline

```bash
# From local repository context (auto-detects settings)
az pipelines create --name 'ContosoBuild' --description 'Pipeline for contoso project'

# With specific branch and YAML path
az pipelines create \
  --name {pipeline-name} \
  --repository {repo} \
  --branch main \
  --yaml-path azure-pipelines.yml \
  --description "My CI/CD pipeline"

# For GitHub repository
az pipelines create \
  --name 'GitHubPipeline' \
  --repository https://github.com/Org/Repo \
  --branch main \
  --repository-type github

# Skip first run
az pipelines create --name 'MyPipeline' --skip-run true
```

### Mostrar pipeline

```bash
az pipelines show --id {pipeline-id}
az pipelines show --name {pipeline-name}
```

### Actualizar pipeline

```bash
az pipelines update --id {pipeline-id} --name "New name" --description "Updated description"
```

### Eliminar pipeline

```bash
az pipelines delete --id {pipeline-id} --yes
```

### Ejecutar pipeline

```bash
# Run by name
az pipelines run --name {pipeline-name} --branch main

# Run by ID
az pipelines run --id {pipeline-id} --branch refs/heads/main

# With parameters
az pipelines run --name {pipeline-name} --parameters version=1.0.0 environment=prod

# With variables
az pipelines run --name {pipeline-name} --variables buildId=123 configuration=release

# Open results in browser
az pipelines run --name {pipeline-name} --open
```

## Ejecuciones de pipeline

### Listar ejecuciones

```bash
az pipelines runs list --pipeline {pipeline-id}
az pipelines runs list --name {pipeline-name} --top 10
az pipelines runs list --branch main --status completed
```

### Mostrar detalles de ejecucion

```bash
az pipelines runs show --run-id {run-id}
az pipelines runs show --run-id {run-id} --open
```

### Artefactos de pipeline

```bash
# List artifacts for a run
az pipelines runs artifact list --run-id {run-id}

# Download artifact
az pipelines runs artifact download \
  --artifact-name '{artifact-name}' \
  --path {local-path} \
  --run-id {run-id}

# Upload artifact
az pipelines runs artifact upload \
  --artifact-name '{artifact-name}' \
  --path {local-path} \
  --run-id {run-id}
```

### Etiquetas de ejecucion de pipeline

```bash
# Add tag to run
az pipelines runs tag add --run-id {run-id} --tags production v1.0

# List run tags
az pipelines runs tag list --run-id {run-id} --output table
```

## Builds

### Listar builds

```bash
az pipelines build list
az pipelines build list --definition {build-definition-id}
az pipelines build list --status completed --result succeeded
```

### Encolar build

```bash
az pipelines build queue --definition {build-definition-id} --branch main
az pipelines build queue --definition {build-definition-id} --parameters version=1.0.0
```

### Mostrar detalles de build

```bash
az pipelines build show --id {build-id}
```

### Cancelar build

```bash
az pipelines build cancel --id {build-id}
```

### Etiquetas de build

```bash
# Add tag to build
az pipelines build tag add --build-id {build-id} --tags prod release

# Delete tag from build
az pipelines build tag delete --build-id {build-id} --tag prod
```

## Definiciones de build

### Listar definiciones de build

```bash
az pipelines build definition list
az pipelines build definition list --name {definition-name}
```

### Mostrar definicion de build

```bash
az pipelines build definition show --id {definition-id}
```

## Releases

### Listar releases

```bash
az pipelines release list
az pipelines release list --definition {release-definition-id}
```

### Crear release

```bash
az pipelines release create --definition {release-definition-id}
az pipelines release create --definition {release-definition-id} --description "Release v1.0"
```

### Mostrar release

```bash
az pipelines release show --id {release-id}
```

## Definiciones de release

### Listar definiciones de release

```bash
az pipelines release definition list
```

### Mostrar definicion de release

```bash
az pipelines release definition show --id {definition-id}
```

## Paquetes universales (artefactos)

### Publicar paquete

```bash
az artifacts universal publish \
  --feed {feed-name} \
  --name {package-name} \
  --version {version} \
  --path {package-path} \
  --project {project}
```

### Descargar paquete

```bash
az artifacts universal download \
  --feed {feed-name} \
  --name {package-name} \
  --version {version} \
  --path {download-path} \
  --project {project}
```