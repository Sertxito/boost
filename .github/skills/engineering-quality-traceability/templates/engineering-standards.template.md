# Engineering Standards

## Objetivo

Definir un estandar minimo de calidad para codigo productivo.

## 1. Convenciones de nomenclatura

- Clases, records, enums, interfaces publicas: PascalCase.
- Metodos publicos y privados: PascalCase.
- Variables locales y parametros: camelCase.
- Campos privados: _camelCase.
- Interfaces: prefijo I (ejemplo: IUserService).
- DTOs: sufijo Dto.
- Comandos y queries: sufijo Command / Query.
- Handlers: sufijo Handler.

## 2. Diseno y arquitectura

- Regla de capas: API -> Application -> Domain -> Infrastructure.
- Domain no referencia Infrastructure.
- Clases con responsabilidad unica.
- Evitar clases anidadas salvo casos documentados.
- DTOs y modelos de transporte en carpetas dedicadas.

## 3. Atomicidad y complejidad

- Metodo ideal: una sola responsabilidad observable.
- Si un metodo crece en decisiones o ramas, extraer funciones privadas o servicios.
- Evitar metodos muy largos y con multiples niveles de anidacion.

## 4. Helpers y utilidades

- Extraer solo logica transversal y repetida.
- No mover reglas de negocio criticas a helpers genericos.
- Evitar carpetas helper sin ownership ni proposito claro.

## 5. Genericos

- Usar genericos cuando reduzcan duplicacion real.
- No crear abstracciones genericas prematuras.
- Justificar su uso con beneficio concreto: testabilidad, extension o reduccion de codigo.

## 6. Errores y validacion

- Validar input en fronteras (controllers, endpoints, handlers).
- Usar ProblemDetails para respuestas de error en APIs.
- No exponer stack traces en produccion.

## 7. Testing minimo

- Nuevas rutas criticas deben incluir pruebas unitarias e integracion cuando aplique.
- Cubrir casos felices, limites y errores esperados.
- Evitar pruebas que solo inflan cobertura sin validar comportamiento.

## 8. Definition of Done (DoD)

- Cumple convenciones de nombres y capas.
- Incluye pruebas para comportamiento nuevo o cambiado.
- Incluye impacto en trazabilidad/auditoria si aplica.
- No introduce deuda de seguridad evidente.
