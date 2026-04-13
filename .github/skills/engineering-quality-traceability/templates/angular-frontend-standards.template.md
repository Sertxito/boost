# Angular Frontend Standards

## Objetivo

Definir un estandar frontend Angular para entornos productivos, basado en la documentacion oficial de angular.dev.

Referencias base:

- [Angular Style Guide](https://angular.dev/style-guide)
- [Angular Security Best Practices](https://angular.dev/best-practices/security)
- [Angular Performance Best Practices](https://angular.dev/best-practices/performance)
- [Angular Signals Guide](https://angular.dev/guide/signals)
- [Angular HTTP Guide](https://angular.dev/guide/http)
- [Angular Routing Guide](https://angular.dev/guide/routing)

## 1. Arquitectura y estructura

- Organizar el codigo por feature areas, no por tipo de artefacto.
- Mantener `src` como raiz de codigo UI.
- Usar un concepto principal por archivo (componente, directiva o servicio).
- Mantener archivos del componente con mismo prefijo:
  - `feature-card.ts`
  - `feature-card.html`
  - `feature-card.css`
  - `feature-card.spec.ts`

## 2. Nomenclatura y coherencia

- Archivos en kebab-case.
- Selectores con prefijo del proyecto.
- Event handlers nombrados por accion (`saveUserData`) y no por evento (`handleClick`).
- Si una regla entra en conflicto con estilo local consolidado, priorizar consistencia dentro del modulo.

## 3. Componentes, DI y estado

- Priorizar componentes standalone para aplicaciones modernas.
- Mantener componentes orientados a presentacion.
- Mover logica compleja a servicios o funciones reutilizables.
- Preferir `inject()` cuando mejore legibilidad y mantenimiento.
- Para estado local, usar `signal()` y `computed()` cuando simplifiquen el flujo reactivo.
- Exponer estado de solo lectura cuando corresponda (`asReadonly()`).

## 4. Templates y rendimiento de render

- Evitar expresiones complejas en templates.
- Llevar calculos costosos a `computed()` o capa TypeScript.
- Preferir bindings de `class` y `style` sobre `ngClass` y `ngStyle` cuando sea posible.
- Mantener lifecycle hooks delgados; delegar a metodos con nombre de negocio.

## 5. Routing y carga

- Definir rutas por feature y lazy loading por defecto.
- Usar guards y resolvers segun necesidad funcional.
- Aplicar `@defer` para componentes pesados o contenido no critico inicial.
- Evitar cargar toda la aplicacion en el bundle inicial.

## 6. HTTP, interceptores y errores

- Usar `HttpClient` tipado para requests/responses.
- Centralizar headers, auth, correlationId y manejo comun de errores en interceptores.
- Mantener contratos de API tipados y mapeados a modelos del frontend.
- Estandarizar manejo de errores en capa de datos para no duplicar logica en componentes.

## 7. Seguridad

- No construir templates concatenando input de usuario.
- Evitar acceso directo al DOM salvo necesidad justificada.
- Auditar cualquier uso de `bypassSecurityTrustHtml`, `bypassSecurityTrustUrl`, etc.
- Mantener AOT en produccion.
- Configurar CSP y Trusted Types cuando el entorno lo permita.
- Mantener proteccion XSRF activa e integrada con backend.

## 8. Performance operativa

- Priorizar:
  - lazy-loaded routes
  - `@defer`
  - optimizacion de imagenes
  - SSR/hydration cuando aplique al producto
- Medir antes de optimizar con Angular DevTools y Chrome DevTools.
- Definir objetivos minimos de CWV (LCP, INP, CLS) y revisar en cada release.

## 9. Testing

- Mantener tests unitarios `*.spec.ts` junto al codigo.
- Cubrir al menos:
  - render y bindings criticos
  - flujos de usuario clave
  - servicios HTTP y manejo de errores
  - guards/resolvers de rutas sensibles
- Usar testing utilities oficiales de Angular para HTTP y routing.

## 10. Trazabilidad frontend -> backend

- Propagar `correlationId` en llamadas HTTP hacia backend.
- Loggear errores relevantes de UI con contexto funcional (feature, accion, usuario si aplica).
- Alinear naming de eventos de frontend con el catalogo de auditoria backend.

## Checklist rapido para PR Angular

- Estructura por feature respetada.
- Componentes con responsabilidad clara.
- Estado local claro con signals/computed cuando aplica.
- Routing lazy y carga diferida para vistas pesadas.
- HttpClient tipado + interceptores para concerns transversales.
- Seguridad DOM/sanitizacion revisada.
- Pruebas y trazabilidad cubiertas.
