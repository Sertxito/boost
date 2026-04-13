# Lista de vigilancia de paquetes vulnerables y de alto riesgo

<!-- markdownlint-disable-file -->

Carga esto durante el Paso 2 (auditoria de dependencias). Verifica versiones en los lock files del proyecto.

---

## npm / Node.js

| Paquete | Versiones vulnerables | Riesgo | Version segura |
|---------|-------------------|-------|--------------|
| lodash | < 4.17.21 | Prototype pollution (CVE-2021-23337) | >= 4.17.21 |
| axios | < 1.6.0 | SSRF, open redirect | >= 1.6.0 |
| jsonwebtoken | < 9.0.0 | Algorithm confusion bypass | >= 9.0.0 |
| node-jose | < 2.2.0 | Key confusion | >= 2.2.0 |
| shelljs | < 0.8.5 | ReDoS | >= 0.8.5 |
| tar | < 6.1.9 | Path traversal | >= 6.1.9 |
| minimist | < 1.2.6 | Prototype pollution | >= 1.2.6 |
| qs | < 6.7.3 | Prototype pollution | >= 6.7.3 |
| express | < 4.19.2 | Open redirect | >= 4.19.2 |
| multer | < 1.4.4 | DoS | >= 1.4.4-lts.1 |
| xml2js | < 0.5.0 | Prototype pollution | >= 0.5.0 |
| fast-xml-parser | < 4.2.4 | ReDoS | >= 4.2.4 |
| semver | < 7.5.2 | ReDoS | >= 7.5.2 |
| tough-cookie | < 4.1.3 | Prototype pollution | >= 4.1.3 |
| word-wrap | < 1.2.4 | ReDoS | >= 1.2.4 |
| vm2 | ANY | Sandbox escape (deprecated) | Use isolated-vm instead |
| serialize-javascript | < 3.1.0 | XSS | >= 3.1.0 |
| node-fetch | < 2.6.7 | Open redirect | >= 2.6.7 or 3.x |

### Patrones para marcar (independientemente de la version):
- `eval` o `vm.runInContext` en dependencias
- Cualquier paquete que incorpore addons nativos con `node-gyp` de publicadores desconocidos
- Paquetes con < 1000 descargas semanales pero requeridos en codigo de produccion (riesgo de cadena de suministro)

---

## Python / pip

| Paquete | Versiones vulnerables | Riesgo | Version segura |
|---------|-------------------|-------|--------------|
| Pillow | < 10.0.1 | Multiple CVEs, buffer overflow | >= 10.0.1 |
| cryptography | < 41.0.0 | OpenSSL vulnerabilities | >= 41.0.0 |
| PyYAML | < 6.0 | Arbitrary code via yaml.load() | >= 6.0 |
| paramiko | < 3.4.0 | Authentication bypass | >= 3.4.0 |
| requests | < 2.31.0 | Proxy auth info leak | >= 2.31.0 |
| urllib3 | < 2.0.7 | Header injection | >= 2.0.7 |
| Django | < 4.2.16 | Various | >= 4.2.16 |
| Flask | < 3.0.3 | Various | >= 3.0.3 |
| Jinja2 | < 3.1.4 | HTML attribute injection | >= 3.1.4 |
| sqlalchemy | < 2.0.28 | Various | >= 2.0.28 |
| aiohttp | < 3.9.4 | SSRF, path traversal | >= 3.9.4 |
| werkzeug | < 3.0.3 | Various | >= 3.0.3 |

---

## Java / Maven

| Paquete | Versiones vulnerables | Riesgo |
|---------|-------------------|-------|
| log4j-core | 2.0-2.14.1 | Log4Shell RCE (CVE-2021-44228) — CRITICAL |
| log4j-core | 2.15.0 | Incomplete fix — still vulnerable |
| Spring Framework | < 5.3.28, < 6.0.13 | Various CVEs |
| Spring Boot | < 3.1.4 | Various |
| Jackson-databind | < 2.14.0 | Deserialization |
| Apache Commons Text | < 1.10.0 | Text4Shell RCE (CVE-2022-42889) |
| Apache Struts | < 6.3.0 | Various RCE |
| Netty | < 4.1.94 | HTTP request smuggling |

---

## Ruby / Gems

| Gem | Versiones vulnerables | Riesgo |
|-----|-------------------|-------|
| rails | < 7.1.3 | Various | 
| nokogiri | < 1.16.2 | XXE, various |
| rexml | < 3.2.7 | ReDoS |
| rack | < 3.0.9 | Various |
| devise | < 4.9.3 | Various |

---

## Rust / Cargo

| Crate | Riesgo |
|-------|-------|
| openssl | Check advisory db for current version |
| hyper | Check advisory db for current version |

Referencia: https://rustsec.org/advisories/

---

## Go

Referencia: https://pkg.go.dev/vuln/ y https://vuln.go.dev

Patrones de riesgo comunes:
- `golang.org/x/crypto` — comprobar si la version esta dentro de los ultimos 6 meses
- Cualquier dependencia que use directamente el paquete `syscall` — revisar con cuidado

---

## Senales rojas generales (cualquier ecosistema)

Marca cualquier dependencia que:
1. No se ha actualizado en > 2 anos Y tiene > 10 issues de seguridad abiertos
2. Ha sido deprecada por su mantenedor con una advertencia de seguridad
3. Es un fork de un paquete conocido de un publicador desconocido (typosquatting)
4. Tiene un nombre a un caracter de un paquete popular (ej., `lodash` vs `1odash`)
5. Fue transferida recientemente a un nuevo propietario (revisar historial git / avisos de transferencia npm)
