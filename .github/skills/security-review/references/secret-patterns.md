# Patrones de deteccion de secretos y credenciales

<!-- markdownlint-disable-file -->

Carga este archivo durante el Paso 3 (escaneo de secretos y exposicion).

---

## Patrones de secretos de alta confianza

Estos patrones casi siempre indican un secreto real:

### API keys y tokens
```regex
# OpenAI
sk-[a-zA-Z0-9]{48}

# Anthropic
sk-ant-[a-zA-Z0-9\-_]{90,}

# AWS Access Key
AKIA[0-9A-Z]{16}

# AWS Secret Key (look for near AWS_ACCESS_KEY_ID assignment)
[0-9a-zA-Z/+]{40}

# GitHub Token
gh[pousr]_[a-zA-Z0-9]{36,}
github_pat_[a-zA-Z0-9]{82}

# Stripe
sk_live_[a-zA-Z0-9]{24,}
rk_live_[a-zA-Z0-9]{24,}

# Twilio Account SID
AC[a-z0-9]{32}
# Twilio API Key
SK[a-z0-9]{32}

# SendGrid
SG\.[a-zA-Z0-9\-_.]{66}

# Slack
xoxb-[0-9]+-[0-9]+-[a-zA-Z0-9]+
xoxp-[0-9]+-[0-9]+-[0-9]+-[a-zA-Z0-9]+
xapp-[0-9]+-[A-Z0-9]+-[0-9]+-[a-zA-Z0-9]+

# Google API Key
AIza[0-9A-Za-z\-_]{35}

# Google OAuth
[0-9]+-[0-9A-Za-z_]{32}\.apps\.googleusercontent\.com

# Cloudflare (near CF_API_TOKEN)
[a-zA-Z0-9_\-]{37}

# Mailgun
key-[a-zA-Z0-9]{32}

# Heroku
[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}
```

### Claves privadas
```regex
-----BEGIN (RSA |EC |OPENSSH |DSA |PGP )?PRIVATE KEY( BLOCK)?-----
-----BEGIN CERTIFICATE-----
```

### Cadenas de conexion de base de datos
```regex
# MongoDB
mongodb(\+srv)?:\/\/[^:]+:[^@]+@

# PostgreSQL / MySQL
(postgres|postgresql|mysql):\/\/[^:]+:[^@]+@

# Redis with password
redis:\/\/:[^@]+@

# Generic connection string with password
(connection[_-]?string|connstr|db[_-]?url).*password=
```

### Contrasenas hardcodeadas (senales por nombre de variable)
```regex
# Variable names that suggest secrets
(password|passwd|pwd|secret|api_key|apikey|auth_token|access_token|private_key)
  \s*[=:]\s*["'][^"']{8,}["']
```

---

## Deteccion basada en entropia

Aplicar a literales de cadena > 20 caracteres en contexto de asignacion.
Alta entropia (entropia de Shannon > 4.5 bits/char) + longitud > 20 = probable secreto.

```
Calcular entropia: -sum(p * log2(p)) para cada frecuencia de caracter p
Umbral: > 4.5 bits/char Y > 20 chars Y asignada a una variable
```

Falsos positivos comunes a excluir:
- Lorem ipsum text
- HTML/CSS content
- Base64-encoded non-sensitive config (but flag and note)
- UUID/GUID (entropy is high but format is recognizable)

---

## Archivos que nunca deben commitearse

Marca si estos archivos existen en la raiz del repo o estan trackeados por git:
```
.env
.env.local
.env.production
.env.staging
*.pem
*.key
*.p12
*.pfx
id_rsa
id_ed25519
credentials.json
service-account.json
gcp-key.json
secrets.yaml
secrets.json
config/secrets.yml
```

Revisa tambien `.gitignore` — si un patron de secreto NO esta en .gitignore, marcalo.

---

## Riesgos de secretos en CI/CD e IaC

### GitHub Actions — marcar estos patrones:
```yaml
# Hardcoded values in env: blocks (should use ${{ secrets.NAME }})
env:
  API_KEY: "actual-value-here"   # VULNERABLE

# Printing secrets
- run: echo ${{ secrets.MY_SECRET }}   # leaks to logs
```

### Docker — marcar estos:
```dockerfile
# Secrets in ENV (persisted in image layers)
ENV AWS_SECRET_KEY=actual-value

# Secrets passed as build args (visible in image history)
ARG API_KEY=actual-value
```

### Terraform — marcar estos:
```hcl
# Hardcoded sensitive values (should use var or data source)
password = "hardcoded-password"
access_key = "AKIAIOSFODNN7EXAMPLE"
```

---

## Patrones seguros (NO marcar)

Estos son placeholders intencionales — reconocer y omitir:
```
"your-api-key-here"
"<YOUR_API_KEY>"
"${API_KEY}"
"${process.env.API_KEY}"
"os.environ.get('API_KEY')"
"REPLACE_WITH_YOUR_KEY"
"xxx...xxx"
"sk-..." (in documentation/comments)
```
