---
description: 'Instrucciones para generación de tests con Playwright'
applyTo: '**'
---

## Lineamientos para Escribir Tests

### Estandares de Calidad de Codigo
- **Locators**: Prioriza locators orientados al usuario y basados en roles (`getByRole`, `getByLabel`, `getByText`, etc.) para resiliencia y accesibilidad. Usa `test.step()` para agrupar interacciones y mejorar legibilidad y reportes.
- **Assertions**: Usa web-first assertions con reintento automatico. Estas assertions comienzan con la palabra clave `await` (por ejemplo, `await expect(locator).toHaveText()`). Evita `expect(locator).toBeVisible()` salvo que estes probando especificamente cambios de visibilidad.
- **Timeouts**: Apoyate en los mecanismos de auto-wait integrados de Playwright. Evita esperas hard-coded o aumentar los timeouts por defecto.
- **Clarity**: Usa titulos descriptivos de test y step que indiquen claramente la intencion. Agrega comentarios solo para explicar logica compleja o interacciones no evidentes.


### Estructura de Tests
- **Imports**: Comienza con `import { test, expect } from '@playwright/test';`.
- **Organization**: Agrupa pruebas relacionadas de una funcionalidad bajo un bloque `test.describe()`.
- **Hooks**: Usa `beforeEach` para acciones de setup comunes a todos los tests dentro de un bloque `describe` (por ejemplo, navegar a una pagina).
- **Titles**: Sigue una convencion clara de nombres, como `Feature - Specific action or scenario`.


### Organizacion de Archivos
- **Location**: Guarda todos los archivos de test en el directorio `tests/`.
- **Naming**: Usa la convencion `<feature-or-page>.spec.ts` (por ejemplo, `login.spec.ts`, `search.spec.ts`).
- **Scope**: Apunta a un archivo de test por funcionalidad principal o pagina de la aplicacion.

### Buenas Practicas de Assertions
- **UI Structure**: Usa `toMatchAriaSnapshot` para verificar la estructura del arbol de accesibilidad de un componente. Esto proporciona un snapshot completo y accesible.
- **Element Counts**: Usa `toHaveCount` para comprobar la cantidad de elementos encontrados por un locator.
- **Text Content**: Usa `toHaveText` para coincidencias exactas de texto y `toContainText` para coincidencias parciales.
- **Navigation**: Usa `toHaveURL` para verificar la URL de la pagina despues de una accion.


## Estructura de Test de Ejemplo

```typescript
import { test, expect } from '@playwright/test';

test.describe('Movie Search Feature', () => {
  test.beforeEach(async ({ page }) => {
    // Navigate to the application before each test
    await page.goto('https://debs-obrien.github.io/playwright-movies-app');
  });

  test('Search for a movie by title', async ({ page }) => {
    await test.step('Activate and perform search', async () => {
      await page.getByRole('search').click();
      const searchInput = page.getByRole('textbox', { name: 'Search Input' });
      await searchInput.fill('Garfield');
      await searchInput.press('Enter');
    });

    await test.step('Verify search results', async () => {
      // Verify the accessibility tree of the search results
      await expect(page.getByRole('main')).toMatchAriaSnapshot(`
        - main:
          - heading "Garfield" [level=1]
          - heading "search results" [level=2]
          - list "movies":
            - listitem "movie":
              - link "poster of The Garfield Movie The Garfield Movie rating":
                - /url: /playwright-movies-app/movie?id=tt5779228&page=1
                - img "poster of The Garfield Movie"
                - heading "The Garfield Movie" [level=2]
      `);
    });
  });
});
```

## Estrategia de Ejecucion de Tests

1. **Initial Run**: Ejecuta los tests con `npx playwright test --project=chromium`
2. **Debug Failures**: Analiza los fallos de tests e identifica causas raiz
3. **Iterate**: Refina locators, assertions o logica de test segun sea necesario
4. **Validate**: Asegura que los tests pasen de forma consistente y cubran la funcionalidad esperada
5. **Report**: Entrega feedback sobre resultados de tests y cualquier problema detectado

## Checklist de Calidad

Antes de finalizar los tests, asegura:
- [ ] Todos los locators son accesibles y especificos, y evitan violaciones de strict mode
- [ ] Los tests estan agrupados logicamente y siguen una estructura clara
- [ ] Las assertions son significativas y reflejan expectativas del usuario
- [ ] Los tests siguen convenciones de nombres consistentes
- [ ] El codigo esta correctamente formateado y comentado
