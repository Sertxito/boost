# Organizacion, seguridad y administracion

<!-- markdownlint-disable-file -->

## Tabla de contenido

- [Proyectos](#proyectos)
- [Gestion de extensiones](#gestion-de-extensiones)
- [Service Endpoints](#service-endpoints)
- [Equipos](#equipos)
- [Usuarios](#usuarios)
- [Grupos de seguridad](#grupos-de-seguridad)
- [Permisos de seguridad](#permisos-de-seguridad)
- [Wikis](#wikis)
- [Administracion](#administracion)
- [Extensiones de DevOps](#extensiones-de-devops)

---

## Proyectos

### Listar proyectos

```bash
az devops project list --organization https://dev.azure.com/{org}
az devops project list --top 10 --output table
```

### Crear proyecto

```bash
az devops project create \
  --name myNewProject \
  --organization https://dev.azure.com/{org} \
  --description "My new DevOps project" \
  --source-control git \
  --visibility private
```

### Mostrar detalles del proyecto

```bash
az devops project show --project {project-name} --org https://dev.azure.com/{org}
```

### Eliminar proyecto

```bash
az devops project delete --id {project-id} --org https://dev.azure.com/{org} --yes
```

## Gestion de extensiones

### Listar extensiones

```bash
# List available extensions
az extension list-available --output table

# List installed extensions
az extension list --output table
```

### Gestionar extension de Azure DevOps

```bash
# Install Azure DevOps extension
az extension add --name azure-devops

# Update Azure DevOps extension
az extension update --name azure-devops

# Remove extension
az extension remove --name azure-devops

# Install from local path
az extension add --source ~/extensions/azure-devops.whl
```

## Service Endpoints

### Listar Service Endpoints

```bash
az devops service-endpoint list --project {project}
az devops service-endpoint list --project {project} --output table
```

### Mostrar Service Endpoint

```bash
az devops service-endpoint show --id {endpoint-id} --project {project}
```

### Crear Service Endpoint

```bash
# Using configuration file
az devops service-endpoint create --service-endpoint-configuration endpoint.json --project {project}
```

### Eliminar Service Endpoint

```bash
az devops service-endpoint delete --id {endpoint-id} --project {project} --yes
```

## Equipos

### Listar equipos

```bash
az devops team list --project {project}
```

### Mostrar equipo

```bash
az devops team show --team {team-name} --project {project}
```

### Crear equipo

```bash
az devops team create \
  --name {team-name} \
  --description "Team description" \
  --project {project}
```

### Actualizar equipo

```bash
az devops team update \
  --team {team-name} \
  --project {project} \
  --name "{new-team-name}" \
  --description "Updated description"
```

### Eliminar equipo

```bash
az devops team delete --team {team-name} --project {project} --yes
```

### Mostrar miembros del equipo

```bash
az devops team list-member --team {team-name} --project {project}
```

## Usuarios

### Listar usuarios

```bash
az devops user list --org https://dev.azure.com/{org}
az devops user list --top 10 --output table
```

### Mostrar usuario

```bash
az devops user show --user {user-id-or-email} --org https://dev.azure.com/{org}
```

### Agregar usuario

```bash
az devops user add \
  --email user@example.com \
  --license-type express \
  --org https://dev.azure.com/{org}
```

### Actualizar usuario

```bash
az devops user update \
  --user {user-id-or-email} \
  --license-type advanced \
  --org https://dev.azure.com/{org}
```

### Quitar usuario

```bash
az devops user remove --user {user-id-or-email} --org https://dev.azure.com/{org} --yes
```

## Grupos de seguridad

### Listar grupos

```bash
# List all groups in project
az devops security group list --project {project}

# List all groups in organization
az devops security group list --scope organization

# List with filtering
az devops security group list --project {project} --subject-types vstsgroup
```

### Mostrar detalles del grupo

```bash
az devops security group show --group-id {group-id}
```

### Crear grupo

```bash
az devops security group create \
  --name {group-name} \
  --description "Group description" \
  --project {project}
```

### Actualizar grupo

```bash
az devops security group update \
  --group-id {group-id} \
  --name "{new-group-name}" \
  --description "Updated description"
```

### Eliminar grupo

```bash
az devops security group delete --group-id {group-id} --yes
```

### Membresias de grupo

```bash
# List memberships
az devops security group membership list --id {group-id}

# Add member
az devops security group membership add \
  --group-id {group-id} \
  --member-id {member-id}

# Remove member
az devops security group membership remove \
  --group-id {group-id} \
  --member-id {member-id} --yes
```

## Permisos de seguridad

### Listar namespaces

```bash
az devops security permission namespace list
```

### Mostrar detalles del namespace

```bash
# Show permissions available in a namespace
az devops security permission namespace show --namespace "GitRepositories"
```

### Listar permisos

```bash
# List permissions for user/group and namespace
az devops security permission list \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project}

# List for specific token (repository)
az devops security permission list \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project} \
  --token "repoV2/{project}/{repository-id}"
```

### Mostrar permisos

```bash
az devops security permission show \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project} \
  --token "repoV2/{project}/{repository-id}"
```

### Actualizar permisos

```bash
# Grant permission
az devops security permission update \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project} \
  --token "repoV2/{project}/{repository-id}" \
  --permission-mask "Pull,Contribute"

# Deny permission
az devops security permission update \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project} \
  --token "repoV2/{project}/{repository-id}" \
  --permission-mask 0
```

### Restablecer permisos

```bash
# Reset specific permission bits
az devops security permission reset \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project} \
  --token "repoV2/{project}/{repository-id}" \
  --permission-mask "Pull,Contribute"

# Reset all permissions
az devops security permission reset-all \
  --id {user-or-group-id} \
  --namespace "GitRepositories" \
  --project {project} \
  --token "repoV2/{project}/{repository-id}" --yes
```

## Wikis

### Listar wikis

```bash
# List all wikis in project
az devops wiki list --project {project}

# List all wikis in organization
az devops wiki list
```

### Mostrar wiki

```bash
az devops wiki show --wiki {wiki-name} --project {project}
az devops wiki show --wiki {wiki-name} --project {project} --open
```

### Crear wiki

```bash
# Create project wiki
az devops wiki create \
  --name {wiki-name} \
  --project {project} \
  --type projectWiki

# Create code wiki from repository
az devops wiki create \
  --name {wiki-name} \
  --project {project} \
  --type codeWiki \
  --repository {repo-name} \
  --mapped-path /wiki
```

### Eliminar wiki

```bash
az devops wiki delete --wiki {wiki-id} --project {project} --yes
```

### Paginas de wiki

```bash
# List pages
az devops wiki page list --wiki {wiki-name} --project {project}

# Show page
az devops wiki page show \
  --wiki {wiki-name} \
  --path "/page-name" \
  --project {project}

# Create page
az devops wiki page create \
  --wiki {wiki-name} \
  --path "/new-page" \
  --content "# New Page\n\nPage content here..." \
  --project {project}

# Update page
az devops wiki page update \
  --wiki {wiki-name} \
  --path "/existing-page" \
  --content "# Updated Page\n\nNew content..." \
  --project {project}

# Delete page
az devops wiki page delete \
  --wiki {wiki-name} \
  --path "/old-page" \
  --project {project} --yes
```

## Administracion

### Gestion de banners

```bash
# List banners
az devops admin banner list

# Show banner details
az devops admin banner show --id {banner-id}

# Add new banner
az devops admin banner add \
  --message "System maintenance scheduled" \
  --level info  # info, warning, error

# Update banner
az devops admin banner update \
  --id {banner-id} \
  --message "Updated message" \
  --level warning \
  --expiration-date "2025-12-31T23:59:59Z"

# Remove banner
az devops admin banner remove --id {banner-id}
```

## Extensiones de DevOps

Gestiona extensiones instaladas en una organizacion de Azure DevOps (distintas de las extensiones de CLI).

```bash
# List installed extensions
az devops extension list --org https://dev.azure.com/{org}

# Search marketplace extensions
az devops extension search --search-query "docker"

# Show extension details
az devops extension show --ext-id {extension-id} --org https://dev.azure.com/{org}

# Install extension
az devops extension install \
  --ext-id {extension-id} \
  --org https://dev.azure.com/{org} \
  --publisher {publisher-id}

# Enable extension
az devops extension enable \
  --ext-id {extension-id} \
  --org https://dev.azure.com/{org}

# Disable extension
az devops extension disable \
  --ext-id {extension-id} \
  --org https://dev.azure.com/{org}

# Uninstall extension
az devops extension uninstall \
  --ext-id {extension-id} \
  --org https://dev.azure.com/{org} --yes
```