---
name: playwright-generate-test
description: 'Generate a Playwright test based on a scenario using Playwright MCP'
---

# Test Generation with Playwright MCP

Tu objetivo es generar una prueba de Playwright basada en el escenario proporcionado, después de completar todos los pasos indicados.

## Specific Instructions

- Se te proporciona un escenario y debes generar una prueba de playwright para él. Si la persona usuaria no proporciona un escenario, debes solicitarlo.
- NO generes código de prueba de forma prematura ni basándote solo en el escenario sin completar todos los pasos indicados.
- SÍ ejecuta los pasos uno a uno usando las herramientas proporcionadas por Playwright MCP.
- Solo después de completar todos los pasos, emite una prueba Playwright TypeScript que use `@playwright/test` basada en el historial de mensajes.
- Guarda el archivo de prueba generado en el directorio `tests`
- Ejecuta el archivo de prueba e itera hasta que la prueba pase
