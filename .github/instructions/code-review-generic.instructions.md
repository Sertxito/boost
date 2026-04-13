---
description: 'Instrucciones genéricas de revisión de código que pueden personalizarse para cualquier proyecto con GitHub Copilot'
applyTo: '**'
excludeAgent: ["coding-agent"]
---

# Instrucciones genéricas de revisión de código

Guía integral de revisión de código para GitHub Copilot que puede adaptarse a cualquier proyecto. Estas instrucciones siguen buenas prácticas de prompt engineering y proporcionan un enfoque estructurado para calidad de código, seguridad, pruebas y revisión de arquitectura.

## Idioma de la revisión

Al realizar una revisión de código, responde en **inglés** (o especifica tu idioma preferido).

> **Sugerencia de personalización**: Cambia a tu idioma preferido reemplazando "English" por "Portuguese (Brazilian)", "Spanish", "French", etc.

## Prioridades de revisión

Al realizar una revisión de código, prioriza los problemas en el siguiente orden:

### 🔴 CRITICAL (Bloquea el merge)
- **Security**: Vulnerabilidades, secretos expuestos, problemas de autenticacion/autorizacion
- **Correctness**: Errores logicos, riesgos de corrupcion de datos, condiciones de carrera
- **Breaking Changes**: Cambios de contrato API sin versionado
- **Data Loss**: Riesgo de perdida o corrupcion de datos

### 🟡 IMPORTANT (Requiere discusión)
- **Code Quality**: Violaciones severas de principios SOLID, duplicacion excesiva
- **Test Coverage**: Falta de pruebas en rutas criticas o funcionalidad nueva
- **Performance**: Cuellos de botella evidentes de rendimiento (consultas N+1, fugas de memoria)
- **Architecture**: Desviaciones significativas de patrones establecidos

### 🟢 SUGGESTION (Mejoras no bloqueantes)
- **Readability**: Nombres deficientes, logica compleja que podria simplificarse
- **Optimization**: Mejoras de rendimiento sin impacto funcional
- **Buenas practicas**: Desviaciones menores de convenciones
- **Documentation**: Comentarios/documentacion faltantes o incompletos

## Principios generales de revisión

Al realizar una revisión de código, sigue estos principios:

1. **Be specific**: Referencia lineas y archivos exactos, y aporta ejemplos concretos
2. **Provide context**: Explica POR QUE algo es un problema y su impacto potencial
3. **Suggest solutions**: Muestra codigo corregido cuando aplique, no solo lo que esta mal
4. **Be constructive**: Enfocate en mejorar el codigo, no en criticar a la autoria
5. **Recognize good practices**: Reconoce codigo bien escrito y soluciones inteligentes
6. **Be pragmatic**: No toda sugerencia requiere implementacion inmediata
7. **Group related comments**: Evita multiples comentarios sobre el mismo tema

## Estándares de calidad de código

Al realizar una revisión de código, verifica lo siguiente:

### Código limpio
- Descriptive and meaningful names for variables, functions, and classes
- Single Responsibility Principle: each function/class does one thing well
- DRY (Don't Repeat Yourself): no code duplication
- Functions should be small and focused (ideally < 20-30 lines)
- Avoid deeply nested code (max 3-4 levels)
- Avoid magic numbers and strings (use constants)
- Code should be self-documenting; comments only when necessary

### Examples
```javascript
// ❌ BAD: Poor naming and magic numbers
function calc(x, y) {
    if (x > 100) return y * 0.15;
    return y * 0.10;
}

// ✅ GOOD: Clear naming and constants
const PREMIUM_THRESHOLD = 100;
const PREMIUM_DISCOUNT_RATE = 0.15;
const STANDARD_DISCOUNT_RATE = 0.10;

function calculateDiscount(orderTotal, itemPrice) {
    const isPremiumOrder = orderTotal > PREMIUM_THRESHOLD;
    const discountRate = isPremiumOrder ? PREMIUM_DISCOUNT_RATE : STANDARD_DISCOUNT_RATE;
    return itemPrice * discountRate;
}
```

### Manejo de errores
- Proper error handling at appropriate levels
- Meaningful error messages
- No silent failures or ignored exceptions
- Fail fast: validate inputs early
- Use appropriate error types/exceptions

### Examples
```python
# ❌ BAD: Silent failure and generic error
def process_user(user_id):
    try:
        user = db.get(user_id)
        user.process()
    except:
        pass

# ✅ GOOD: Explicit error handling
def process_user(user_id):
    if not user_id or user_id <= 0:
        raise ValueError(f"Invalid user_id: {user_id}")

    try:
        user = db.get(user_id)
    except UserNotFoundError:
        raise UserNotFoundError(f"User {user_id} not found in database")
    except DatabaseError as e:
        raise ProcessingError(f"Failed to retrieve user {user_id}: {e}")

    return user.process()
```

## Revisión de seguridad

Al realizar una revisión de código, revisa problemas de seguridad:

- **Sensitive Data**: No passwords, API keys, tokens, or PII in code or logs
- **Input Validation**: All user inputs are validated and sanitized
- **SQL Injection**: Use parameterized queries, never string concatenation
- **Authentication**: Proper authentication checks before accessing resources
- **Authorization**: Verify user has permission to perform action
- **Cryptography**: Use established libraries, never roll your own crypto
- **Dependency Security**: Check for known vulnerabilities in dependencies

### Examples
```java
// ❌ BAD: SQL injection vulnerability
String query = "SELECT * FROM users WHERE email = '" + email + "'";

// ✅ GOOD: Parameterized query
PreparedStatement stmt = conn.prepareStatement(
    "SELECT * FROM users WHERE email = ?"
);
stmt.setString(1, email);
```

```javascript
// ❌ BAD: Exposed secret in code
const API_KEY = "sk_live_abc123xyz789";

// ✅ GOOD: Use environment variables
const API_KEY = process.env.API_KEY;
```

## Estándares de pruebas

Al realizar una revisión de código, verifica la calidad de las pruebas:

- **Coverage**: Critical paths and new functionality must have tests
- **Test Names**: Descriptive names that explain what is being tested
- **Test Structure**: Clear Arrange-Act-Assert or Given-When-Then pattern
- **Independence**: Tests should not depend on each other or external state
- **Assertions**: Use specific assertions, avoid generic assertTrue/assertFalse
- **Edge Cases**: Test boundary conditions, null values, empty collections
- **Mock Appropriately**: Mock external dependencies, not domain logic

### Examples
```typescript
// ❌ BAD: Vague name and assertion
test('test1', () => {
    const result = calc(5, 10);
    expect(result).toBeTruthy();
});

// ✅ GOOD: Descriptive name and specific assertion
test('should calculate 10% discount for orders under $100', () => {
    const orderTotal = 50;
    const itemPrice = 20;

    const discount = calculateDiscount(orderTotal, itemPrice);

    expect(discount).toBe(2.00);
});
```

## Consideraciones de rendimiento

Al realizar una revisión de código, revisa problemas de rendimiento:

- **Database Queries**: Avoid N+1 queries, use proper indexing
- **Algorithms**: Appropriate time/space complexity for the use case
- **Caching**: Utilize caching for expensive or repeated operations
- **Resource Management**: Proper cleanup of connections, files, streams
- **Pagination**: Large result sets should be paginated
- **Lazy Loading**: Load data only when needed

### Examples
```python
# ❌ BAD: N+1 query problem
users = User.query.all()
for user in users:
    orders = Order.query.filter_by(user_id=user.id).all()  # N+1!

# ✅ GOOD: Use JOIN or eager loading
users = User.query.options(joinedload(User.orders)).all()
for user in users:
    orders = user.orders
```

## Arquitectura y diseño

Al realizar una revisión de código, verifica principios de arquitectura:

- **Separation of Concerns**: Clear boundaries between layers/modules
- **Direccion de dependencias**: Los modulos de alto nivel no dependen de detalles de bajo nivel
- **Interface Segregation**: Prefer small, focused interfaces
- **Loose Coupling**: Components should be independently testable
- **High Cohesion**: Related functionality grouped together
- **Consistent Patterns**: Follow established patterns in the codebase

## Estándares de documentación

Al realizar una revisión de código, revisa la documentación:

- **API Documentation**: Public APIs must be documented (purpose, parameters, returns)
- **Complex Logic**: Non-obvious logic should have explanatory comments
- **README Updates**: Update README when adding features or changing setup
- **Breaking Changes**: Document any breaking changes clearly
- **Examples**: Provide usage examples for complex features

## Plantilla de formato para comentarios

Al realizar una revisión de código, usa este formato para comentarios:

```markdown
**[PRIORITY] Category: Brief title**

Detailed description of the issue or suggestion.

**Why this matters:**
Explanation of the impact or reason for the suggestion.

**Suggested fix:**
[code example if applicable]

**Reference:** [link to relevant documentation or standard]
```

### Comentarios de ejemplo

#### Problema crítico
````markdown
**🔴 CRITICAL - Security: SQL Injection Vulnerability**

The query on line 45 concatenates user input directly into the SQL string,
creating a SQL injection vulnerability.

**Why this matters:**
An attacker could manipulate the email parameter to execute arbitrary SQL commands,
potentially exposing or deleting all database data.

**Suggested fix:**
```sql
-- Instead of:
query = "SELECT * FROM users WHERE email = '" + email + "'"

-- Use:
PreparedStatement stmt = conn.prepareStatement(
    "SELECT * FROM users WHERE email = ?"
);
stmt.setString(1, email);
```

**Reference:** OWASP SQL Injection Prevention Cheat Sheet
````

#### Problema importante
````markdown
**🟡 IMPORTANT - Testing: Missing test coverage for critical path**

The `processPayment()` function handles financial transactions but has no tests
for the refund scenario.

**Why this matters:**
Refunds involve money movement and should be thoroughly tested to prevent
financial errors or data inconsistencies.

**Suggested fix:**
Add test case:
```javascript
test('should process full refund when order is cancelled', () => {
    const order = createOrder({ total: 100, status: 'cancelled' });

    const result = processPayment(order, { type: 'refund' });

    expect(result.refundAmount).toBe(100);
    expect(result.status).toBe('refunded');
});
```
````

#### Sugerencia
````markdown
**🟢 SUGGESTION - Readability: Simplify nested conditionals**

The nested if statements on lines 30-40 make the logic hard to follow.

**Why this matters:**
Simpler code is easier to maintain, debug, and test.

**Suggested fix:**
```javascript
// Instead of nested ifs:
if (user) {
    if (user.isActive) {
        if (user.hasPermission('write')) {
            // do something
        }
    }
}

// Consider guard clauses:
if (!user || !user.isActive || !user.hasPermission('write')) {
    return;
}
// do something
```
````

## Lista de verificación de revisión

Al realizar una revisión de código, verifica de forma sistemática:

### Code Quality
- [ ] Code follows consistent style and conventions
- [ ] Names are descriptive and follow naming conventions
- [ ] Functions/methods are small and focused
- [ ] No code duplication
- [ ] Complex logic is broken into simpler parts
- [ ] Error handling is appropriate
- [ ] No commented-out code or TODO without tickets

### Security
- [ ] No sensitive data in code or logs
- [ ] Input validation on all user inputs
- [ ] No SQL injection vulnerabilities
- [ ] Authentication and authorization properly implemented
- [ ] Dependencies are up-to-date and secure

### Testing
- [ ] New code has appropriate test coverage
- [ ] Tests are well-named and focused
- [ ] Tests cover edge cases and error scenarios
- [ ] Tests are independent and deterministic
- [ ] No tests that always pass or are commented out

### Performance
- [ ] No obvious performance issues (N+1, memory leaks)
- [ ] Appropriate use of caching
- [ ] Efficient algorithms and data structures
- [ ] Proper resource cleanup

### Architecture
- [ ] Follows established patterns and conventions
- [ ] Proper separation of concerns
- [ ] No architectural violations
- [ ] Dependencies flow in correct direction

### Documentation
- [ ] Public APIs are documented
- [ ] Complex logic has explanatory comments
- [ ] README is updated if needed
- [ ] Breaking changes are documented

## Personalizaciones específicas del proyecto

Para personalizar esta plantilla para tu proyecto, agrega secciones para:

1. **Language/Framework specific checks**
   - Example: "When performing a code review, verify React hooks follow rules of hooks"
   - Example: "When performing a code review, check Spring Boot controllers use proper annotations"

2. **Build and deployment**
   - Example: "When performing a code review, verify CI/CD pipeline configuration is correct"
   - Example: "When performing a code review, check database migrations are reversible"

3. **Business logic rules**
   - Example: "When performing a code review, verify pricing calculations include all applicable taxes"
   - Example: "When performing a code review, check user consent is obtained before data processing"

4. **Team conventions**
   - Example: "When performing a code review, verify commit messages follow conventional commits format"
   - Example: "When performing a code review, check branch names follow pattern: type/ticket-description"

## Recursos adicionales

Para más información sobre revisiones de código efectivas y personalización de GitHub Copilot:

- [GitHub Copilot Prompt Engineering](https://docs.github.com/en/copilot/concepts/prompting/prompt-engineering)
- [GitHub Copilot Custom Instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Awesome GitHub Copilot Repository](https://github.com/github/awesome-copilot)
- [GitHub Code Review Guidelines](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests)
- [Google Engineering Practices - Code Review](https://google.github.io/eng-practices/review/)
- [OWASP Security Guidelines](https://owasp.org/)

## Consejos de prompt engineering

Al realizar una revisión de código, aplica estos principios de prompt engineering de la [documentación de GitHub Copilot](https://docs.github.com/en/copilot/concepts/prompting/prompt-engineering):

1. **Empieza general y luego concreta**: Comienza con una revision de arquitectura de alto nivel y despues profundiza en los detalles de implementacion
2. **Give Examples**: Reference similar patterns in the codebase when suggesting changes
3. **Break Complex Tasks**: Review large PRs in logical chunks (security → tests → logic → style)
4. **Avoid Ambiguity**: Be specific about which file, line, and issue you're addressing
5. **Indicate Relevant Code**: Reference related code that might be affected by changes
6. **Experiment and Iterate**: If initial review misses something, review again with focused questions

## Contexto del proyecto

Esta es una plantilla genérica. Personaliza esta sección con la información específica de tu proyecto:

- **Tech Stack**: [e.g., Java 17, Spring Boot 3.x, PostgreSQL]
- **Architecture**: [e.g., Hexagonal/Clean Architecture, Microservices]
- **Build Tool**: [e.g., Gradle, Maven, npm, pip]
- **Testing**: [e.g., JUnit 5, Jest, pytest]
- **Code Style**: [e.g., follows Google Style Guide]
