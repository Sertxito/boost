---
name: polyglot-test-agent
description: 'Generates comprehensive, workable unit tests for any programming language using a multi-agent pipeline. Use when asked to generate tests, write unit tests, improve test coverage, add test coverage, create test files, or test a codebase. Supports C#, TypeScript, JavaScript, Python, Go, Rust, Java, and more. Orchestrates research, planning, and implementation phases to produce tests that compile, pass, and follow project conventions.'
---

# Skill de generación de pruebas políglota

Una habilidad impulsada por IA que genera pruebas unitarias completas y funcionales para cualquier lenguaje de programación usando un pipeline coordinado de múltiples agentes.

## Cuándo usar esta skill

Usa esta habilidad cuando necesites:
- Generar pruebas unitarias para un proyecto completo o archivos concretos
- Mejorar la cobertura de pruebas de código existente
- Crear archivos de prueba que sigan convenciones del proyecto
- Escribir pruebas que realmente compilen y pasen
- Añadir pruebas para nuevas funcionalidades o código sin pruebas

## Cómo funciona

Esta habilidad coordina múltiples agentes especializados en un pipeline **Research → Plan → Implement**:

### Resumen del pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                     TEST GENERATOR                          │
│  Coordinates the full pipeline and manages state            │
└─────────────────────┬───────────────────────────────────────┘
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
┌───────────┐  ┌───────────┐  ┌───────────────┐
│ RESEARCHER│  │  PLANNER  │  │  IMPLEMENTER  │
│           │  │           │  │               │
│ Analyzes  │  │ Creates   │  │ Writes tests  │
│ codebase  │→ │ phased    │→ │ per phase     │
│           │  │ plan      │  │               │
└───────────┘  └───────────┘  └───────┬───────┘
                                      │
                    ┌─────────┬───────┼───────────┐
                    ▼         ▼       ▼           ▼
              ┌─────────┐ ┌───────┐ ┌───────┐ ┌───────┐
              │ BUILDER │ │TESTER │ │ FIXER │ │LINTER │
              │         │ │       │ │       │ │       │
              │ Compiles│ │ Runs  │ │ Fixes │ │Formats│
              │ code    │ │ tests │ │ errors│ │ code  │
              └─────────┘ └───────┘ └───────┘ └───────┘
```

## Step-by-Step Instructions

### Paso 1: determinar la solicitud de la persona usuaria

Asegúrate de entender qué está pidiendo la persona usuaria y en qué alcance.
Cuando la persona usuaria no exprese requisitos claros sobre estilo de test, objetivos de cobertura o convenciones, usa las guías de [unit-test-generation.prompt.md](unit-test-generation.prompt.md). Este prompt aporta buenas prácticas para descubrir convenciones, estrategias de parametrización, metas de cobertura (objetivo 80%) y patrones por lenguaje.

### Paso 2: invocar el generador de pruebas

Empieza invocando el agente `polyglot-test-generator` con tu solicitud de generación de pruebas:

```
Generate unit tests for [path or description of what to test], following the [unit-test-generation.prompt.md](unit-test-generation.prompt.md) guidelines
```

El Test Generator gestionará automáticamente todo el pipeline.

### Paso 3: fase de investigación (automática)

El agente `polyglot-test-researcher` analiza tu código para entender:
- **Lenguaje y framework**: detecta C#, TypeScript, Python, Go, Rust, Java, etc.
- **Framework de pruebas**: identifica MSTest, xUnit, Jest, pytest, go test, etc.
- **Estructura del proyecto**: mapea archivos fuente, pruebas existentes y dependencias
- **Comandos de build**: descubre cómo compilar y probar el proyecto

Salida: `.testagent/research.md`

### Paso 4: fase de planificación (automática)

El agente `polyglot-test-planner` crea un plan de implementación estructurado:
- Agrupa archivos en fases lógicas (típicamente 2-5 fases)
- Prioriza por complejidad y dependencias
- Especifica casos de prueba para cada archivo
- Define criterios de éxito por fase

Salida: `.testagent/plan.md`

### Paso 5: fase de implementación (automática)

El agente `polyglot-test-implementer` ejecuta cada fase de forma secuencial:

1. **Read** archivos fuente para entender la API
2. **Write** archivos de prueba siguiendo los patrones del proyecto
3. **Build** usando el subagente `polyglot-test-builder` para verificar compilación
4. **Test** usando el subagente `polyglot-test-tester` para verificar que las pruebas pasen
5. **Fix** usando el subagente `polyglot-test-fixer` si aparecen errores
6. **Lint** usando el subagente `polyglot-test-linter` para el formateo del código

Cada fase se completa antes de empezar la siguiente, asegurando progreso incremental.

### Tipos de cobertura
- **Ruta feliz**: entradas válidas producen salidas esperadas
- **Casos límite**: valores vacíos, fronteras, caracteres especiales
- **Casos de error**: entradas inválidas, manejo de null, excepciones

## Gestión del estado

Todo el estado del pipeline se guarda en la carpeta `.testagent/`:

| Archivo | Propósito |
|------|---------|
| `.testagent/research.md` | Resultados del análisis del codebase |
| `.testagent/plan.md` | Plan de implementación por fases |
| `.testagent/status.md` | Seguimiento de progreso (opcional) |

## Ejemplos

### Ejemplo 1: pruebas del proyecto completo
```
Generate unit tests for my Calculator project at C:\src\Calculator
```

### Ejemplo 2: pruebas de un archivo específico
```
Generate unit tests for src/services/UserService.ts
```

### Ejemplo 3: cobertura focalizada
```
Añade pruebas para el módulo de autenticación con foco en casos límite
```

## Referencia de agentes

| Agente | Propósito | Herramientas |
|-------|---------|-------|
| `polyglot-test-generator` | Coordina el pipeline | runCommands, codebase, editFiles, search, runSubagent |
| `polyglot-test-researcher` | Analiza el codebase | runCommands, codebase, editFiles, search, fetch, runSubagent |
| `polyglot-test-planner` | Crea el plan de pruebas | codebase, editFiles, search, runSubagent |
| `polyglot-test-implementer` | Escribe archivos de prueba | runCommands, codebase, editFiles, search, runSubagent |
| `polyglot-test-builder` | Compila el código | runCommands, codebase, search |
| `polyglot-test-tester` | Ejecuta pruebas | runCommands, codebase, search |
| `polyglot-test-fixer` | Corrige errores | runCommands, codebase, editFiles, search |
| `polyglot-test-linter` | Formatea código | runCommands, codebase, search |

## Requisitos

- El proyecto debe tener configurado un sistema de build/test
- El framework de pruebas debe estar instalado (o ser instalable)
- VS Code con la extensión GitHub Copilot

## Resolución de problemas

### Las pruebas no compilan
El agente `polyglot-test-fixer` intentará resolver errores de compilación. Revisa `.testagent/plan.md` para verificar la estructura esperada de pruebas.

### Las pruebas fallan
Revisa la salida de las pruebas y ajusta expectativas. Algunas pruebas pueden requerir mocks de dependencias.

### Se detectó un framework de pruebas incorrecto
Especifica el framework preferido en la solicitud inicial: "Generate Jest tests for..."
