---
name: dotnet-best-practices
description: 'Asegura que el código .NET/C# cumpla las mejores prácticas de la solución/proyecto.'
---

# Buenas prácticas de .NET/C#

Tu tarea es asegurar que el código .NET/C# en ${selection} cumpla las buenas prácticas específicas de esta solución/proyecto. Esto incluye:

## Documentación y estructura

- Crea comentarios XML de documentación completos para todas las clases, interfaces, métodos y propiedades públicas
- Incluye descripciones de parámetros y valores de retorno en los comentarios XML
- Sigue la estructura de namespace establecida: {Core|Console|App|Service}.{Feature}

## Patrones de diseño y arquitectura

- Usa sintaxis de constructor primario para inyección de dependencias (p. ej., `public class MyClass(IDependency dependency)`)
- Implementa el patrón Command Handler con clases base genéricas (p. ej., `CommandHandler<TOptions>`)
- Usa segregación de interfaces con convenciones de nombre claras (prefijar interfaces con 'I')
- Sigue el patrón Factory para la creación compleja de objetos.

## Inyección de dependencias y servicios

- Usa inyección de dependencias por constructor con comprobaciones nulas mediante ArgumentNullException
- Registra servicios con ciclos de vida apropiados (Singleton, Scoped, Transient)
- Usa patrones de Microsoft.Extensions.DependencyInjection
- Implementa interfaces de servicio para mejorar la testabilidad

## Gestión de recursos y localización

- Usa ResourceManager para mensajes localizados y cadenas de error
- Separa los archivos de recursos LogMessages y ErrorMessages
- Accede a los recursos mediante `_resourceManager.GetString("MessageKey")`

## Patrones Async/Await

- Usa async/await en todas las operaciones de E/S y tareas de larga duración
- Devuelve Task o Task<T> desde métodos asíncronos
- Usa ConfigureAwait(false) cuando corresponda
- Maneja correctamente las excepciones asíncronas

## Estándares de pruebas

- Usa el framework MSTest con FluentAssertions para aserciones
- Sigue el patrón AAA (Arrange, Act, Assert)
- Usa Moq para simular dependencias
- Prueba tanto escenarios exitosos como de fallo
- Incluye pruebas de validación de parámetros nulos

## Configuración y ajustes

- Usa clases de configuración fuertemente tipadas con data annotations
- Implementa atributos de validación (Required, NotEmptyOrWhitespace)
- Usa enlace de IConfiguration para ajustes
- Soporta archivos de configuración appsettings.json

## Semantic Kernel e integración de IA

- Usa Microsoft.SemanticKernel para operaciones de IA
- Implementa configuración adecuada del kernel y registro de servicios
- Gestiona ajustes de modelos de IA (ChatCompletion, Embedding, etc.)
- Usa patrones de salida estructurada para respuestas de IA confiables

## Manejo de errores y logging

- Usa logging estructurado con Microsoft.Extensions.Logging
- Incluye logging con alcance y contexto significativo
- Lanza excepciones específicas con mensajes descriptivos
- Usa bloques try-catch para escenarios de fallo esperados

## Rendimiento y seguridad

- Usa características de C# 12+ y optimizaciones de .NET 8 cuando aplique
- Implementa validación y saneamiento de entrada adecuados
- Usa consultas parametrizadas para operaciones de base de datos
- Sigue prácticas de codificación segura para operaciones de IA/ML

## Calidad de código

- Asegura el cumplimiento de los principios SOLID
- Evita duplicación de código mediante clases base y utilidades
- Usa nombres significativos que reflejen conceptos del dominio
- Mantén los métodos enfocados y cohesionados
- Implementa patrones adecuados de liberacion de recursos
