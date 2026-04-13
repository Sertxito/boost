---
description: 'Lineamientos para crear aplicaciones C#'
applyTo: '**/*.cs'
---

# Desarrollo en C#

## Instrucciones de C#
- Usa siempre la version mas reciente de C#, actualmente con funcionalidades de C# 14.
- Escribe comentarios claros y concisos para cada funcion.

## Instrucciones Generales
- Haz solo sugerencias de alta confianza al revisar cambios de codigo.
- Escribe codigo con buenas practicas de mantenibilidad, incluyendo comentarios sobre por que se tomaron ciertas decisiones de diseno.
- Maneja casos limite y escribe un manejo de excepciones claro.
- Para librerias o dependencias externas, menciona su uso y proposito en comentarios.

## Convenciones de Nombres

- Usa PascalCase para nombres de componentes, metodos y miembros publicos.
- Usa camelCase para campos privados y variables locales.
- Prefija los nombres de interfaces con "I" (por ejemplo, IUserService).

## Formato

- Aplica el estilo de formateo de codigo definido en `.editorconfig`.
- Prefiere declaraciones de namespace con alcance de archivo y directivas using en una sola linea.
- Inserta una nueva linea antes de la llave de apertura de cualquier bloque de codigo (por ejemplo, despues de `if`, `for`, `while`, `foreach`, `using`, `try`, etc.).
- Asegura que la sentencia return final de un metodo este en su propia linea.
- Usa pattern matching y switch expressions siempre que sea posible.
- Usa `nameof` en lugar de literales string al referirte a nombres de miembros.
- Asegura que se creen comentarios XML doc para cualquier API publica. Cuando aplique, incluye documentacion `<example>` y `<code>` en los comentarios.

## Configuracion y Estructura del Proyecto

- Guia a los usuarios para crear un nuevo proyecto .NET con los templates adecuados.
- Explica el proposito de cada archivo y carpeta generados para construir una buena comprension de la estructura del proyecto.
- Demuestra como organizar el codigo usando feature folders o principios de domain-driven design.
- Muestra una separacion correcta de responsabilidades con modelos, servicios y capas de acceso a datos.
- Explica Program.cs y el sistema de configuracion en ASP.NET Core 10, incluyendo settings especificos por entorno.

## Nullable Reference Types

- Declara variables como non-nullable y valida `null` en los puntos de entrada.
- Usa siempre `is null` o `is not null` en lugar de `== null` o `!= null`.
- Confia en las anotaciones null de C# y no agregues validaciones de null cuando el sistema de tipos indique que un valor no puede ser null.

## Patrones de Acceso a Datos

- Guia la implementacion de una capa de acceso a datos usando Entity Framework Core.
- Explica diferentes opciones (SQL Server, SQLite, In-Memory) para desarrollo y produccion.
- Demuestra la implementacion del patron repository y cuando es beneficioso.
- Muestra como implementar migraciones de base de datos y data seeding.
- Explica patrones de consulta eficientes para evitar problemas comunes de rendimiento.

## Autenticacion y Autorizacion

- Guia a los usuarios en la implementacion de autenticacion con JWT Bearer tokens.
- Explica conceptos de OAuth 2.0 y OpenID Connect en el contexto de ASP.NET Core.
- Muestra como implementar autorizacion basada en roles y en politicas.
- Demuestra la integracion con Microsoft Entra ID (antes Azure AD).
- Explica como proteger de forma consistente APIs basadas en controllers y Minimal APIs.

## Validacion y Manejo de Errores

- Guia la implementacion de validacion de modelos usando data annotations y FluentValidation.
- Explica el pipeline de validacion y como personalizar respuestas de validacion.
- Demuestra una estrategia global de manejo de excepciones usando middleware.
- Muestra como crear respuestas de error consistentes en toda la API.
- Explica la implementacion de problem details (RFC 9457) para respuestas de error estandarizadas.

## Versionado y Documentacion de API

- Guia a los usuarios para implementar y explicar estrategias de versionado de API.
- Demuestra la implementacion de Swagger/OpenAPI con documentacion adecuada.
- Muestra como documentar endpoints, parametros, respuestas y autenticacion.
- Explica el versionado tanto en APIs basadas en controllers como en Minimal APIs.
- Guia a los usuarios para crear documentacion de API significativa que ayude a los consumidores.

## Logging y Monitoreo

- Guia la implementacion de structured logging usando Serilog u otros proveedores.
- Explica los niveles de logging y cuando usar cada uno.
- Demuestra la integracion con Application Insights para recoleccion de telemetria.
- Muestra como implementar telemetria personalizada y correlation IDs para trazabilidad de requests.
- Explica como monitorear rendimiento de API, errores y patrones de uso.

## Testing

- Incluye siempre casos de prueba para rutas criticas de la aplicacion.
- Guia a los usuarios para crear unit tests.
- No emitas comentarios "Act", "Arrange" o "Assert".
- Copia el estilo existente en archivos cercanos para nombres de metodos de test y capitalizacion.
- Explica enfoques de integration testing para endpoints de API.
- Demuestra como hacer mock de dependencias para pruebas efectivas.
- Muestra como probar logica de autenticacion y autorizacion.
- Explica principios de test-driven development aplicados al desarrollo de API.

## Optimizacion de Rendimiento

- Guia a los usuarios para implementar estrategias de caching (in-memory, distributed, response caching).
- Explica patrones de programacion asincrona y por que importan para el rendimiento de API.
- Demuestra paginacion, filtrado y ordenamiento para conjuntos de datos grandes.
- Muestra como implementar compresion y otras optimizaciones de rendimiento.
- Explica como medir y hacer benchmark del rendimiento de API.

## Deployment y DevOps

- Guia a los usuarios para containerizar su API usando el soporte integrado de contenedores de .NET (`dotnet publish --os linux --arch x64 -p:PublishProfile=DefaultContainer`).
- Explica las diferencias entre crear Dockerfile manualmente y las capacidades de publicacion de contenedores de .NET.
- Explica pipelines de CI/CD para aplicaciones .NET.
- Demuestra despliegue en Azure App Service, Azure Container Apps u otras opciones de hosting.
- Muestra como implementar health checks y readiness probes.
- Explica configuraciones especificas por entorno para diferentes etapas de despliegue.
