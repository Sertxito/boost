---
name: security-review
description: 'AI-powered codebase security scanner that reasons about code like a security researcher — tracing data flows, understanding component interactions, and catching vulnerabilities that pattern-matching tools miss. Use this skill when asked to scan code for security vulnerabilities, find bugs, check for SQL injection, XSS, command injection, exposed API keys, hardcoded secrets, insecure dependencies, access control issues, or any request like "is my code secure?", "review for security issues", "audit this codebase", or "check for vulnerabilities". Covers injection flaws, authentication and access control bugs, secrets exposure, weak cryptography, insecure dependencies, and business logic issues across JavaScript, TypeScript, Python, Java, PHP, Go, Ruby, and Rust.'
---

# Security Review

Un escáner de seguridad impulsado por IA que razona sobre tu código como lo haría una persona investigadora de seguridad: rastreando flujos de datos, entendiendo interacciones entre componentes y detectando vulnerabilidades que las herramientas basadas en patrones no encuentran.

## When to Use This Skill

Usa esta habilidad cuando la solicitud incluya:

- Escanear un código o archivo en busca de vulnerabilidades de seguridad
- Ejecutar una revisión de seguridad o verificación de vulnerabilidades
- Comprobar SQL injection, XSS, command injection u otros fallos de inyección
- Encontrar API keys expuestas, secretos hardcodeados o credenciales en código
- Auditar dependencias en busca de CVEs conocidos
- Revisar lógica de autenticación, autorización o control de acceso
- Detectar criptografía insegura o aleatoriedad débil
- Realizar análisis de flujo de datos para trazar entrada de usuario hasta sinks peligrosos
- Cualquier solicitud como "is my code secure?", "scan this file" o "check my repo for vulnerabilities"
- Ejecutar `/security-review` o `/security-review <path>`

## How This Skill Works

A diferencia de las herramientas tradicionales de análisis estático que solo comparan patrones, esta habilidad:
1. **Lee código como una persona investigadora de seguridad**: entendiendo contexto, intención y flujo de datos
2. **Traza entre archivos**: siguiendo cómo se mueve la entrada de usuario por tu aplicación
3. **Auto-verifica hallazgos**: vuelve a examinar cada resultado para filtrar falsos positivos
4. **Asigna severidad**: CRITICAL / HIGH / MEDIUM / LOW / INFO
5. **Propone parches dirigidos**: cada hallazgo incluye una corrección concreta
6. **Requiere aprobación humana**: nada se aplica automáticamente; tú revisas primero

## Execution Workflow

Sigue estos pasos **en orden** cada vez:

### Step 1 — Scope Resolution
Determina qué escanear:
- If a path was provided (`/security-review src/auth/`), scan only that scope
- If no path given, scan the **entire project** starting from the root
- Identify the language(s) and framework(s) in use (check package.json, requirements.txt,
  go.mod, Cargo.toml, pom.xml, Gemfile, composer.json, etc.)
- Read `references/language-patterns.md` to load language-specific vulnerability patterns

### Step 2 — Dependency Audit
Antes de escanear el código fuente, audita primero dependencias (quick wins):
- **Node.js**: Check `package.json` + `package-lock.json` for known vulnerable packages
- **Python**: Check `requirements.txt` / `pyproject.toml` / `Pipfile`
- **Java**: Check `pom.xml` / `build.gradle`
- **Ruby**: Check `Gemfile.lock`
- **Rust**: Check `Cargo.toml`
- **Go**: Check `go.sum`
- Flag packages with known CVEs, deprecated crypto libs, or suspiciously old pinned versions
- Read `references/vulnerable-packages.md` for a curated watchlist

### Step 3 — Secrets & Exposure Scan
Escanea TODOS los archivos (incluyendo config, env, CI/CD, Dockerfiles, IaC) buscando:
- Hardcoded API keys, tokens, passwords, private keys
- `.env` files accidentally committed
- Secrets in comments or debug logs
- Cloud credentials (AWS, GCP, Azure, Stripe, Twilio, etc.)
- Database connection strings with credentials embedded
- Read `references/secret-patterns.md` for regex patterns and entropy heuristics to apply

### Step 4 — Vulnerability Deep Scan
Este es el escaneo principal. Razona sobre el código, no te limites a patrones.
Lee `references/vuln-categories.md` para ver el detalle completo de cada categoría.

**Injection Flaws**
- SQL Injection: raw queries with string interpolation, ORM misuse, second-order SQLi
- XSS: unescaped output, dangerouslySetInnerHTML, innerHTML, template injection
- Command Injection: exec/spawn/system with user input
- LDAP, XPath, Header, Log injection

**Authentication & Access Control**
- Missing authentication on sensitive endpoints
- Broken object-level authorization (BOLA/IDOR)
- JWT weaknesses (alg:none, weak secrets, no expiry validation)
- Session fixation, missing CSRF protection
- Privilege escalation paths
- Mass assignment / parameter pollution

**Data Handling**
- Sensitive data in logs, error messages, or API responses
- Missing encryption at rest or in transit
- Insecure deserialization
- Path traversal / directory traversal
- XXE (XML External Entity) processing
- SSRF (Server-Side Request Forgery)

**Cryptography**
- Use of MD5, SHA1, DES for security purposes
- Hardcoded IVs or salts
- Weak random number generation (Math.random() for tokens)
- Missing TLS certificate validation

**Business Logic**
- Race conditions (TOCTOU)
- Integer overflow in financial calculations
- Missing rate limiting on sensitive endpoints
- Predictable resource identifiers

### Step 5 — Cross-File Data Flow Analysis
Después del escaneo por archivo, realiza una **revisión holística**:
- Trace user-controlled input from entry points (HTTP params, headers, body, file uploads)
  all the way to sinks (DB queries, exec calls, HTML output, file writes)
- Identify vulnerabilities that only appear when looking at multiple files together
- Check for insecure trust boundaries between services or modules

### Step 6 — Self-Verification Pass
Para CADA hallazgo:
1. Re-read the relevant code with fresh eyes
2. Ask: "Is this actually exploitable, or is there sanitization I missed?"
3. Check if a framework or middleware already handles this upstream
4. Downgrade or discard findings that aren't genuine vulnerabilities
5. Assign final severity: CRITICAL / HIGH / MEDIUM / LOW / INFO

### Step 7 — Generate Security Report
Entrega el informe completo con el formato definido en `references/report-format.md`.

### Step 8 — Propose Patches
Para cada hallazgo CRITICAL y HIGH, genera un parche concreto:
- Show the vulnerable code (before)
- Show the fixed code (after)
- Explain what changed and why
- Preserve the original code style, variable names, and structure
- Add a comment explaining the fix inline

Indica explícitamente: **"Review each patch before applying. Nothing has been changed yet."**

## Severity Guide

| Severity | Meaning | Example |
|----------|---------|---------|
| 🔴 CRITICAL | Immediate exploitation risk, data breach likely | SQLi, RCE, auth bypass |
| 🟠 HIGH | Serious vulnerability, exploit path exists | XSS, IDOR, hardcoded secrets |
| 🟡 MEDIUM | Exploitable with conditions or chaining | CSRF, open redirect, weak crypto |
| 🔵 LOW | Best practice violation, low direct risk | Verbose errors, missing headers |
| ⚪ INFO | Observation worth noting, not a vulnerability | Outdated dependency (no CVE) |

## Output Rules

- **Always** produce primero una tabla resumen de hallazgos (conteo por severidad)
- **Never** apliques parches automáticamente; presenta parches solo para revisión humana
- **Always** incluye un nivel de confianza por hallazgo (High / Medium / Low)
- **Group findings** por categoría, no por archivo
- **Be specific**: incluye ruta de archivo, línea y snippet exacto vulnerable
- **Explain the risk** en lenguaje claro: ¿qué podría hacer una persona atacante con esto?
- Si el código está limpio, dilo claramente: "No vulnerabilities found" e indica qué se escaneó

## Reference Files

Para guía detallada de detección, carga los siguientes archivos de referencia cuando sea necesario:

- `references/vuln-categories.md` — Deep reference for every vulnerability category with detection signals, safe patterns, and escalation checkers
  - Search patterns: `SQL injection`, `XSS`, `command injection`, `SSRF`, `BOLA`, `IDOR`, `JWT`, `CSRF`, `secrets`, `cryptography`, `race condition`, `path traversal`
- `references/secret-patterns.md` — Regex patterns, entropy-based detection, and CI/CD secret risks
  - Search patterns: `API key`, `token`, `private key`, `connection string`, `entropy`, `.env`, `GitHub Actions`, `Docker`, `Terraform`
- `references/language-patterns.md` — Framework-specific vulnerability patterns for JavaScript, Python, Java, PHP, Go, Ruby, and Rust
  - Search patterns: `Express`, `React`, `Next.js`, `Django`, `Flask`, `FastAPI`, `Spring Boot`, `PHP`, `Go`, `Rails`, `Rust`
- `references/vulnerable-packages.md` — Curated CVE watchlist for npm, pip, Maven, Rubygems, Cargo, and Go modules
  - Search patterns: `lodash`, `axios`, `jsonwebtoken`, `Pillow`, `log4j`, `nokogiri`, `CVE`
- `references/report-format.md` — Structured output template for security reports with finding cards, dependency audit, secrets scan, and patch proposal formatting
  - Search patterns: `report`, `format`, `template`, `finding`, `patch`, `summary`, `confidence`
