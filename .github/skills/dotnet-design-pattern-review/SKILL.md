---
name: dotnet-design-pattern-review
description: 'Revisa el código C#/.NET para la implementación de patrones de diseño y sugiere mejoras.'
---

# Revisión de patrones de diseño en .NET/C#

Revisa el código C#/.NET en ${selection} respecto a implementación de patrones de diseño y sugiere mejoras para la solución/proyecto. No realices cambios en el código; solo proporciona la revisión.

## Patrones de diseño requeridos

- **Command Pattern**: Clases base genéricas (`CommandHandler<TOptions>`), interfaz `ICommandHandler<TOptions>`, herencia de `CommandHandlerOptions`, métodos estáticos `SetupCommand(IHost host)`
- **Factory Pattern**: Integración del proveedor de servicios para creación compleja de objetos
- **Dependency Injection**: Sintaxis de constructor primario, comprobaciones nulas con `ArgumentNullException`, abstracciones por interfaz, ciclos de vida de servicio adecuados
- **Repository Pattern**: Interfaces asincronas de acceso a datos y abstracciones de proveedor para conexiones
- **Provider Pattern**: Abstracciones de servicios externos (base de datos, IA), contratos claros, manejo de configuración
- **Resource Pattern**: ResourceManager para mensajes localizados, archivos .resx separados (LogMessages, ErrorMessages)

## Checklist de revisión

- **Design Patterns**: Identifica los patrones usados. ¿Están correctamente implementados Command Handler, Factory, Provider y Repository? ¿Faltan patrones beneficiosos?
- **Architecture**: ¿Se siguen las convenciones de namespace (`{Core|Console|App|Service}.{Feature}`)? ¿Hay separación adecuada entre proyectos Core/Console? ¿Es modular y legible?
- **Buenas prácticas de .NET**: ¿Constructores primarios, async/await con retornos Task, uso de ResourceManager, logging estructurado, configuración fuertemente tipada?
- **GoF Patterns**: ¿Patrones Command, Factory, Template Method y Strategy correctamente implementados?
- **SOLID Principles**: ¿Hay violaciones de Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation y Dependency Inversion?
- **Performance**: ¿Uso correcto de async/await, liberación de recursos, ConfigureAwait(false), oportunidades de procesamiento en paralelo?
- **Maintainability**: ¿Separación clara de responsabilidades, manejo consistente de errores, uso adecuado de configuración?
- **Testability**: ¿Dependencias abstraídas mediante interfaces, componentes mockeables, capacidad de prueba asíncrona, compatibilidad con patrón AAA?
- **Security**: ¿Validación de entrada, manejo seguro de credenciales, consultas parametrizadas, manejo seguro de excepciones?
- **Documentation**: ¿Documentación XML para APIs públicas, descripciones de parámetros/retornos, organización de archivos de recursos?
- **Code Clarity**: ¿Nombres significativos que reflejan conceptos del dominio, intención clara mediante patrones, estructura autoexplicativa?
- **Clean Code**: ¿Estilo consistente, tamaño adecuado de métodos/clases, complejidad mínima, duplicación eliminada?

## Áreas foco de mejora

- **Command Handlers**: Validación en clase base, manejo consistente de errores, gestión adecuada de recursos
- **Factories**: Configuración de dependencias, integración del proveedor de servicios, patrones de liberación de recursos
- **Providers**: Gestión de conexiones, patrones asíncronos, manejo de excepciones y logging
- **Configuration**: Data annotations, atributos de validación, manejo seguro de valores sensibles
- **AI/ML Integration**: Patrones de Semantic Kernel, manejo de salida estructurada, configuración del modelo

Proporciona recomendaciones específicas y accionables de mejora, alineadas con la arquitectura del proyecto y las buenas prácticas de .NET.
