# Categorias de vulnerabilidad — referencia detallada

<!-- markdownlint-disable-file -->

Este archivo contiene guia detallada de deteccion para cada categoria de vulnerabilidad.
Carga esto durante el Paso 4 del flujo de escaneo.

---

## 1. Fallos de inyeccion

### Inyeccion SQL
**Que buscar:**
- String concatenation or interpolation inside SQL queries
- Raw `.query()`, `.execute()`, `.raw()` calls with variables
- ORM `whereRaw()`, `selectRaw()`, `orderByRaw()` with user input
- Second-order SQLi: data stored safely, then used unsafely later
- Stored procedures called with unsanitized input

**Senales de deteccion (todos los lenguajes):**
```
"SELECT ... " + variable
`SELECT ... ${variable}`
f"SELECT ... {variable}"
"SELECT ... %s" % variable   # Only safe with proper driver parameterization
cursor.execute("... " + input)
db.raw(`... ${req.params.id}`)
```

**Patrones seguros (parametrizados):**
```js
db.query('SELECT * FROM users WHERE id = ?', [userId])
User.findOne({ where: { id: userId } })  // ORM safe
```

**Comprobaciones de escalado:**
- La salida de la consulta se reutiliza en otra consulta? (segundo orden)
- El nombre de tabla/columna lo controla el usuario? (no se puede parametrizar, debe ir por allowlist)

---

### Cross-Site Scripting (XSS)
**Que buscar:**
- `innerHTML`, `outerHTML`, `document.write()` with user data
- `dangerouslySetInnerHTML` in React
- Template engines rendering unescaped: `{{{ var }}}` (Handlebars), `!= var` (Pug)
- jQuery `.html()`, `.append()` with user data
- `eval()`, `setTimeout(string)`, `setInterval(string)` with user data
- DOM-based: `location.hash`, `document.referrer`, `window.name` written to DOM
- Stored XSS: user input saved to DB, rendered without escaping later

**Deteccion por framework:**
- **React**: Safe by default EXCEPT `dangerouslySetInnerHTML`
- **Angular**: Safe by default EXCEPT `bypassSecurityTrustHtml`
- **Vue**: Safe by default EXCEPT `v-html`
- **Vanilla JS**: Every DOM write is suspect

---

### Command Injection
**What to look for (Node.js):**
```js
exec(userInput)
execSync(`ping ${host}`)
spawn('sh', ['-c', userInput])
child_process.exec('ls ' + dir)
```

**What to look for (Python):**
```python
os.system(user_input)
subprocess.call(user_input, shell=True)
eval(user_input)
```

**What to look for (PHP):**
```php
exec($input)
system($_GET['cmd'])
passthru($input)
`$input`  # backtick operator
```

**Safe alternatives:** Use array form of spawn/subprocess without shell=True; use allowlists for commands.

---

### Server-Side Request Forgery (SSRF)
**What to look for:**
- HTTP requests where the URL is user-controlled
- Webhooks, URL preview, image fetch features
- PDF generators that fetch external URLs
- Redirects to user-supplied URLs

**High-risk targets:**
- AWS metadata service: `169.254.169.254`
- Internal services: `localhost`, `127.0.0.1`, `10.x.x.x`, `192.168.x.x`
- Cloud metadata endpoints

**Detection:**
```js
fetch(req.body.url)
axios.get(userSuppliedUrl)
http.get(params.webhook)
```

---

## 2. Autenticacion y control de acceso

### Broken Object Level Authorization (BOLA / IDOR)
**Que buscar:**
- Resource IDs taken directly from URL/params without ownership check
- `findById(req.params.id)` without verifying `userId === currentUser.id`
- Numeric sequential IDs (easily guessable)

**Ejemplo de patron vulnerable:**
```js
// VULNERABLE: no ownership check
app.get('/api/documents/:id', async (req, res) => {
  const doc = await Document.findById(req.params.id);
  res.json(doc);
});

// SAFE: verify ownership
app.get('/api/documents/:id', async (req, res) => {
  const doc = await Document.findOne({ _id: req.params.id, owner: req.user.id });
  if (!doc) return res.status(403).json({ error: 'Forbidden' });
  res.json(doc);
});
```

---

### Vulnerabilidades JWT
**Que buscar:**
- `alg: "none"` accepted
- Weak or hardcoded secrets: `secret`, `password`, `1234`
- No expiry (`exp` claim) validation
- Algorithm confusion (RS256 → HS256 downgrade)
- JWT stored in `localStorage` (XSS risk; prefer httpOnly cookie)

**Detection:**
```js
jwt.verify(token, secret, { algorithms: ['HS256'] })  // Check algorithms array
jwt.decode(token)  // WARNING: decode does NOT verify signature
```

---

### Falta de autenticacion / autorizacion
**Que buscar:**
- Admin or sensitive endpoints missing auth middleware
- Routes defined after `app.use(authMiddleware)` vs before it
- Feature flags or debug endpoints left exposed in production
- GraphQL resolvers missing auth checks at field level

---

### CSRF
**Que buscar:**
- State-changing operations (POST/PUT/DELETE) without CSRF token
- APIs relying only on cookies for auth without SameSite attribute
- Missing `SameSite=Strict` or `SameSite=Lax` on session cookies

---

## 3. Exposicion de secretos y datos sensibles

### Secretos en codigo
Busca patrones como:
```
API_KEY = "sk-..."
password = "hunter2"
SECRET = "abc123"
private_key = "-----BEGIN RSA PRIVATE KEY-----"
aws_secret_access_key = "wJalrXUtn..."
```

Heuristica de entropia: cadenas > 20 caracteres con alta variedad en contexto de asignacion
suelen ser secretos aunque el nombre de variable no lo indique.

### En logs / mensajes de error
```js
console.log('User password:', password)
logger.info({ user, token })   // token shouldn't be logged
res.status(500).json({ error: err.stack })  // stack traces expose internals
```

### Datos sensibles en respuestas API
- Returning full user object including `password_hash`, `ssn`, `credit_card`
- Including internal IDs or system paths in error responses

---

## 4. Criptografia

### Algoritmos debiles
| Algoritmo | Riesgo | Sustituir por |
|-----------|-------|--------------|
| MD5 | Broken for security | SHA-256 or bcrypt (passwords) |
| SHA-1 | Collision attacks | SHA-256 |
| DES / 3DES | Weak key size | AES-256-GCM |
| RC4 | Broken | AES-GCM |
| ECB mode | No IV, patterns visible | GCM or CBC with random IV |

### Aleatoriedad debil
```js
// VULNERABLE
Math.random()                    // not cryptographically secure
Date.now()                       // predictable
Math.random().toString(36)       // weak token generation

// SAFE
crypto.randomBytes(32)           // Node.js
secrets.token_urlsafe(32)        // Python
```

### Hash de contrasenas
```python
# VULNERABLE
hashlib.md5(password.encode()).hexdigest()
hashlib.sha256(password.encode()).hexdigest()

# SAFE
bcrypt.hashpw(password, bcrypt.gensalt(rounds=12))
argon2.hash(password)
```

---

## 5. Dependencias inseguras

### Que marcar:
- Paquetes con CVE conocidas en el rango de version instalado
- Paquetes abandonados > 2 anos sin actualizaciones de seguridad
- Paquetes con permisos excesivamente amplios para su proposito
- Dependencias transitivas que arrastran paquetes problematicos conocidos
- Versiones fijadas muy por detras de la actual (posibles vulnerabilidades sin parche)

### Lista de vigilancia de paquetes de alto riesgo: ver `references/vulnerable-packages.md`

---

## 6. Logica de negocio

### Condiciones de carrera (TOCTOU)
```js
// VULNERABLE: check then act without atomic lock
const balance = await getBalance(userId);
if (balance >= amount) {
  await deductBalance(userId, amount);  // race condition between check and deduct
}

// SAFE: use atomic DB transaction or optimistic locking
await db.transaction(async (trx) => {
  const user = await User.query(trx).forUpdate().findById(userId);
  if (user.balance < amount) throw new Error('Insufficient funds');
  await user.$query(trx).patch({ balance: user.balance - amount });
});
```

### Falta de rate limiting
Marca endpoints que:
- Accept authentication credentials (login, 2FA)
- Send emails or SMS
- Perform expensive operations
- Expose user enumeration (password reset, registration)

---

## 7. Path Traversal
```python
# VULNERABLE
filename = request.args.get('file')
with open(f'/var/uploads/{filename}') as f:  # ../../../../etc/passwd

# SAFE
filename = os.path.basename(request.args.get('file'))
safe_path = os.path.join('/var/uploads', filename)
if not safe_path.startswith('/var/uploads/'):
    abort(400)
```
