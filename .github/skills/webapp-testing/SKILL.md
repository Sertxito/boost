---
name: webapp-testing
description: Toolkit for interacting with and testing local web applications using Playwright. Supports verifying frontend functionality, debugging UI behavior, capturing browser screenshots, and viewing browser logs.
---

# Web Application Testing

Esta habilidad permite pruebas y depuración integrales de aplicaciones web locales usando automatización con Playwright.

Debes usar el servidor Playwright MCP para realizar el trabajo siempre que sea posible. Si el servidor MCP no está disponible, puedes ejecutar el código en un entorno local de Node.js con Playwright instalado.

## When to Use This Skill

Usa esta habilidad cuando necesites:

- Probar funcionalidad de frontend en un navegador real
- Verificar el comportamiento e interacciones de la UI
- Depurar problemas de aplicaciones web
- Capturar capturas de pantalla para documentación o depuración
- Inspeccionar logs de consola del navegador
- Validar envíos de formularios y flujos de usuario
- Comprobar diseño responsive en distintos viewports

## Prerequisites

- Node.js instalado en el sistema
- Una aplicación web ejecutándose localmente (o una URL accesible)
- Playwright se instalará automáticamente si no está presente

## Core Capabilities

### 1. Browser Automation

- Navegar a URLs
- Hacer clic en botones y enlaces
- Rellenar campos de formulario
- Seleccionar desplegables
- Gestionar diálogos y alertas

### 2. Verification

- Verificar presencia de elementos
- Verificar contenido de texto
- Comprobar visibilidad de elementos
- Validar URLs
- Probar comportamiento responsive

### 3. Debugging

- Capturar capturas de pantalla
- Ver logs de consola
- Inspeccionar peticiones de red
- Depurar pruebas fallidas

## Usage Examples

### Example 1: Basic Navigation Test

```javascript
// Navigate to a page and verify title
await page.goto("http://localhost:3000");
const title = await page.title();
console.log("Page title:", title);
```

### Example 2: Form Interaction

```javascript
// Fill out and submit a form
await page.fill("#username", "testuser");
await page.fill("#password", "password123");
await page.click('button[type="submit"]');
await page.waitForURL("**/dashboard");
```

### Example 3: Screenshot Capture

```javascript
// Capture a screenshot for debugging
await page.screenshot({ path: "debug.png", fullPage: true });
```

## Guidelines

1. **Verifica siempre que la app esté en ejecución** - Comprueba que el servidor local sea accesible antes de ejecutar pruebas
2. **Usa esperas explícitas** - Espera a que elementos o navegación finalicen antes de interactuar
3. **Captura screenshots en fallo** - Toma capturas para facilitar la depuración
4. **Limpia recursos** - Cierra siempre el navegador al terminar
5. **Gestiona timeouts con criterio** - Define timeouts razonables para operaciones lentas
6. **Prueba de forma incremental** - Empieza con interacciones simples antes de flujos complejos
7. **Usa selectores con criterio** - Prioriza selectores `data-testid` o basados en roles frente a clases CSS

## Common Patterns

### Pattern: Wait for Element

```javascript
await page.waitForSelector("#element-id", { state: "visible" });
```

### Pattern: Check if Element Exists

```javascript
const exists = (await page.locator("#element-id").count()) > 0;
```

### Pattern: Get Console Logs

```javascript
page.on("console", (msg) => console.log("Browser log:", msg.text()));
```

### Pattern: Handle Errors

```javascript
try {
  await page.click("#button");
} catch (error) {
  await page.screenshot({ path: "error.png" });
  throw error;
}
```

## Limitations

- Requiere entorno Node.js
- No puede probar apps móviles nativas (usa React Native Testing Library en su lugar)
- Puede tener problemas con flujos de autenticación complejos
- Algunos frameworks modernos pueden requerir configuración específica

## Helper Functions

Hay funciones auxiliares disponibles en [`test-helper.js`](./assets/test-helper.js) para simplificar tareas comunes como esperar elementos, capturar screenshots y manejar errores. Puedes importar y usar estas funciones en tus pruebas para mejorar legibilidad y mantenibilidad.
