# Repositorios y Pull Requests

<!-- markdownlint-disable-file -->

## Tabla de contenido

- [Repositorios](#repositorios)
- [Importacion de repositorios](#importacion-de-repositorios)
- [Pull Requests](#pull-requests)
- [Referencias Git](#referencias-git)
- [Politicas de repositorio](#politicas-de-repositorio)

---

## Repositorios

### Listar repositorios

```bash
az repos list --org https://dev.azure.com/{org} --project {project}
az repos list --output table
```

### Mostrar detalles del repositorio

```bash
az repos show --repository {repo-name} --project {project}
```

### Crear repositorio

```bash
az repos create --name {repo-name} --project {project}
```

### Eliminar repositorio

```bash
az repos delete --id {repo-id} --project {project} --yes
```

### Actualizar repositorio

```bash
az repos update --id {repo-id} --name {new-name} --project {project}
```

## Importacion de repositorios

### Importar repositorio Git

```bash
# Importar desde repositorio Git publico
az repos import create \
  --git-source-url https://github.com/user/repo \
  --repository {repo-name}

# Importar con autenticacion
az repos import create \
  --git-source-url https://github.com/user/private-repo \
  --repository {repo-name} \
  --user {username} \
  --password {password-or-pat}
```

## Pull Requests

### Crear Pull Request

```bash
# Creacion basica de PR
az repos pr create \
  --repository {repo} \
  --source-branch {source-branch} \
  --target-branch {target-branch} \
  --title "PR Title" \
  --description "PR description" \
  --open

# PR con work items
az repos pr create \
  --repository {repo} \
  --source-branch {source-branch} \
  --work-items 63 64

# PR borrador con revisores
az repos pr create \
  --repository {repo} \
  --source-branch feature/new-feature \
  --target-branch main \
  --title "Feature: New functionality" \
  --draft true \
  --reviewers user1@example.com user2@example.com \
  --required-reviewers lead@example.com \
  --labels "enhancement" "backlog"
```

### Listar Pull Requests

```bash
# Todos los PR
az repos pr list --repository {repo}

# Filtrar por estado
az repos pr list --repository {repo} --status active

# Filtrar por creador
az repos pr list --repository {repo} --creator {email}

# Salida en formato tabla
az repos pr list --repository {repo} --output table
```

### Mostrar detalles de PR

```bash
az repos pr show --id {pr-id}
az repos pr show --id {pr-id} --open  # Open in browser
```

### Actualizar PR (completar/abandonar/borrador)

```bash
# Completar PR
az repos pr update --id {pr-id} --status completed

# Abandonar PR
az repos pr update --id {pr-id} --status abandoned

# Pasar a borrador
az repos pr update --id {pr-id} --draft true

# Publicar PR borrador
az repos pr update --id {pr-id} --draft false

# Auto-completar cuando pasen las politicas
az repos pr update --id {pr-id} --auto-complete true

# Definir titulo y descripcion
az repos pr update --id {pr-id} --title "New title" --description "New description"
```

### Hacer checkout local del PR

```bash
# Checkout de la rama del PR
az repos pr checkout --id {pr-id}

# Checkout con remoto especifico
az repos pr checkout --id {pr-id} --remote-name upstream
```

### Votar en PR

```bash
az repos pr set-vote --id {pr-id} --vote approve
az repos pr set-vote --id {pr-id} --vote approve-with-suggestions
az repos pr set-vote --id {pr-id} --vote reject
az repos pr set-vote --id {pr-id} --vote wait-for-author
az repos pr set-vote --id {pr-id} --vote reset
```

### Revisores de PR

```bash
# Agregar revisores
az repos pr reviewer add --id {pr-id} --reviewers user1@example.com user2@example.com

# Listar revisores
az repos pr reviewer list --id {pr-id}

# Quitar revisores
az repos pr reviewer remove --id {pr-id} --reviewers user1@example.com
```

### Work Items de PR

```bash
# Agregar work items al PR
az repos pr work-item add --id {pr-id} --work-items {id1} {id2}

# Listar work items del PR
az repos pr work-item list --id {pr-id}

# Quitar work items del PR
az repos pr work-item remove --id {pr-id} --work-items {id1}
```

### Politicas de PR

```bash
# Listar politicas de un PR
az repos pr policy list --id {pr-id}

# Encolar evaluacion de politicas para un PR
az repos pr policy queue --id {pr-id} --evaluation-id {evaluation-id}
```

## Referencias Git

### Listar referencias (ramas)

```bash
az repos ref list --repository {repo}
az repos ref list --repository {repo} --query "[?name=='refs/heads/main']"
```

### Crear referencia (rama)

```bash
az repos ref create --name refs/heads/new-branch --object-type commit --object {commit-sha}
```

### Eliminar referencia (rama)

```bash
az repos ref delete --name refs/heads/old-branch --repository {repo} --project {project}
```

### Bloquear/desbloquear rama

```bash
az repos ref lock --name refs/heads/main --repository {repo} --project {project}
az repos ref unlock --name refs/heads/main --repository {repo} --project {project}
```

## Politicas de repositorio

### Listar todas las politicas

```bash
az repos policy list --repository {repo-id} --branch main
```

### Crear/actualizar/eliminar politica

```bash
# Crear desde archivo de configuracion
az repos policy create --config policy.json

# Actualizar
az repos policy update --id {policy-id} --config updated-policy.json

# Eliminar
az repos policy delete --id {policy-id} --yes
```

### Politica de cantidad de aprobadores

```bash
az repos policy approver-count create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id} \
  --minimum-approver-count 2 \
  --creator-vote-counts true
```

### Politica de build

```bash
az repos policy build create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id} \
  --build-definition-id {definition-id} \
  --queue-on-source-update-only true \
  --valid-duration 720
```

### Politica de vinculacion de Work Items

```bash
az repos policy work-item-linking create \
  --blocking true \
  --branch main \
  --enabled true \
  --repository-id {repo-id}
```

### Politica de revisor requerido

```bash
az repos policy required-reviewer create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id} \
  --required-reviewers user@example.com
```

### Politica de estrategia de merge

```bash
az repos policy merge-strategy create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id} \
  --allow-squash true \
  --allow-rebase true \
  --allow-no-fast-forward true
```

### Politica de enforcement de mayusculas/minusculas

```bash
az repos policy case-enforcement create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id}
```

### Politica de comentario obligatorio

```bash
az repos policy comment-required create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id}
```

### Politica de tamano de archivo

```bash
az repos policy file-size create \
  --blocking true \
  --enabled true \
  --branch main \
  --repository-id {repo-id} \
  --maximum-file-size 10485760  # 10MB in bytes
```