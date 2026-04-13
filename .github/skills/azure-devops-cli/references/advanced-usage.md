# Uso avanzado: salida, consultas y parámetros

<!-- markdownlint-disable-file -->

## Tabla de contenido

- [Formatos de salida](#formatos-de-salida)
- [Consultas JMESPath](#consultas-jmespath)
- [Consultas JMESPath avanzadas](#consultas-jmespath-avanzadas)
- [Argumentos globales](#argumentos-globales)
- [Parámetros comunes](#parámetros-comunes)
- [Alias de Git](#alias-de-git)
- [Obtener ayuda](#obtener-ayuda)

---

## Formatos de salida

Todos los comandos admiten múltiples formatos de salida:

```bash
# Table format (human-readable)
az pipelines list --output table

# JSON format (default, machine-readable)
az pipelines list --output json

# JSONC (colored JSON)
az pipelines list --output jsonc

# YAML format
az pipelines list --output yaml

# YAMLC (colored YAML)
az pipelines list --output yamlc

# TSV format (tab-separated values)
az pipelines list --output tsv

# None (no output)
az pipelines list --output none
```

## Consultas JMESPath

Filtra y transforma la salida:

```bash
# Filter by name
az pipelines list --query "[?name=='myPipeline']"

# Get specific fields
az pipelines list --query "[].{Name:name, ID:id}"

# Chain queries
az pipelines list --query "[?name.contains('CI')].{Name:name, ID:id}" --output table

# Get first result
az pipelines list --query "[0]"

# Get top N
az pipelines list --query "[0:5]"
```

## Consultas JMESPath avanzadas

### Filtrado y ordenacion

```bash
# Filter by multiple conditions
az pipelines list --query "[?name.contains('CI') && enabled==true]"

# Filter by status and result
az pipelines runs list --query "[?status=='completed' && result=='succeeded']"

# Sort by date (descending)
az pipelines runs list --query "sort_by([?status=='completed'], &finishTime | reverse(@))"

# Get top N items after filtering
az pipelines runs list --query "[?result=='succeeded'] | [0:5]"
```

### Consultas anidadas

```bash
# Extract nested properties
az pipelines show --id $PIPELINE_ID --query "{Name:name, Repo:repository.{Name:name, Type:type}, Folder:folder}"

# Query build details
az pipelines build show --id $BUILD_ID --query "{ID:id, Number:buildNumber, Status:status, Result:result, Requested:requestedFor.displayName}"
```

### Filtrado complejo

```bash
# Find pipelines with specific YAML path
az pipelines list --query "[?process.type.name=='yaml' && process.yamlFilename=='azure-pipelines.yml']"

# Find PRs from specific reviewer
az repos pr list --query "[?contains(reviewers[?displayName=='John Doe'].displayName, 'John Doe')]"

# Find work items with specific iteration and state
az boards work-item show --id $WI_ID --query "{Title:fields['System.Title'], State:fields['System.State'], Iteration:fields['System.IterationPath']}"
```

### Agregacion

```bash
# Count items by status
az pipelines runs list --query "groupBy([?status=='completed'], &[result]) | {Succeeded: [?key=='succeeded'][0].count, Failed: [?key=='failed'][0].count}"

# Get unique reviewers
az repos pr list --query "unique_by(reviewers[], &displayName)"

# Sum values
az pipelines runs list --query "[?result=='succeeded'] | [].{Duration:duration} | [0].Duration"
```

### Transformacion condicional

```bash
# Format dates
az pipelines runs list --query "[].{ID:id, Date:createdDate, Formatted:createdDate | format_datetime(@, 'yyyy-MM-dd HH:mm')}"

# Conditional output
az pipelines list --query "[].{Name:name, Status:(enabled ? 'Enabled' : 'Disabled')}"

# Extract with defaults
az pipelines show --id $PIPELINE_ID --query "{Name:name, Folder:folder || 'Root', Description:description || 'No description'}"
```

### Flujos complejos

```bash
# Find longest running builds
az pipelines build list --query "sort_by([?result=='succeeded'], &queueTime) | reverse(@) | [0:3].{ID:id, Number:buildNumber, Duration:duration}"

# Get PR statistics per reviewer
az repos pr list --query "groupBy([], &reviewers[].displayName) | [].{Reviewer:@.key, Count:length(@)}"

# Find work items with multiple child items
az boards work-item relation list --id $PARENT_ID --query "[?rel=='System.LinkTypes.Hierarchy-Forward'] | [].{ChildID:url | split('/', @) | [-1]}"
```

## Argumentos globales

Disponibles en todos los comandos:

| Parameter | Description |
| --- | --- |
| `--help` / `-h` | Mostrar ayuda del comando |
| `--output` / `-o` | Formato de salida (json, jsonc, none, table, tsv, yaml, yamlc) |
| `--query` | Consulta JMESPath para filtrar salida |
| `--verbose` | Aumentar nivel de detalle del log |
| `--debug` | Mostrar todos los logs de depuración |
| `--only-show-errors` | Mostrar solo errores, ocultar advertencias |
| `--subscription` | Nombre o ID de la suscripción |
| `--yes` / `-y` | Omitir confirmaciones |

## Parámetros comunes

| Parameter | Description |
| --- | --- |
| `--org` / `--organization` | URL de organización de Azure DevOps (ej., `https://dev.azure.com/{org}`) |
| `--project` / `-p` | Nombre o ID del proyecto |
| `--detect` | Autodetectar organización desde configuración de git |
| `--yes` / `-y` | Omitir confirmaciones |
| `--open` | Abrir recurso en el navegador |
| `--subscription` | Suscripción de Azure (para recursos de Azure) |

## Alias de Git

Después de habilitar alias de git:

```bash
# Enable Git aliases
az devops configure --use-git-aliases true

# Use Git commands for DevOps operations
git pr create --target-branch main
git pr list
git pr checkout 123
```

## Obtener ayuda

```bash
# General help
az devops --help

# Help for specific command group
az pipelines --help
az repos pr --help

# Help for specific command
az repos pr create --help

# Search for examples
az find "az repos pr create"
```