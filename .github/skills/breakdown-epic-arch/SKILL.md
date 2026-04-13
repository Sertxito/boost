---
name: breakdown-epic-arch
description: 'Prompt para crear la arquitectura técnica de alto nivel de una épica, basada en un Product Requirements Document.'
---

# Prompt de especificación de arquitectura de épica

## Objetivo

Actúa como una persona Senior Software Architect. Tu tarea es tomar un PRD de épica y crear una especificación técnica de arquitectura de alto nivel. Este documento guiará el desarrollo de la épica, definiendo los componentes principales, funcionalidades y habilitadores técnicos requeridos.

## Consideraciones de contexto

- El PRD de la épica proporcionado por Product Manager.
- Patrón de **arquitectura orientada a dominio** para aplicaciones modulares y escalables.
- Requisitos de **despliegue self-hosted y SaaS**.
- **Containerización con Docker** para todos los servicios.
- Stack **TypeScript/Next.js** con App Router.
- Patrones de **monorepo con Turborepo**.
- **tRPC** para APIs type-safe.
- **Stack Auth** para autenticación.

**Nota:** NO escribas código en la salida salvo que sea pseudocódigo para situaciones técnicas.

## Formato de salida

La salida debe ser una Epic Architecture Specification completa en formato Markdown, guardada en `/docs/ways-of-work/plan/{epic-name}/arch.md`.

### Estructura de la especificación

#### 1. Vista general de la arquitectura de la épica

- Un resumen breve del enfoque técnico de la épica.

#### 2. Diagrama de arquitectura del sistema

Crea un diagrama Mermaid integral que ilustre la arquitectura completa del sistema para esta épica. El diagrama debe incluir:

- **Capa de usuario**: Muestra cómo distintos tipos de usuario (navegadores web, apps móviles, interfaces de administración) interactúan con el sistema
- **Capa de aplicación**: Representa balanceadores de carga, instancias de aplicación y servicios de autenticación (Stack Auth)
- **Capa de servicios**: Incluye APIs tRPC, servicios en segundo plano, motores de workflow (n8n) y cualquier servicio específico de la épica
- **Capa de datos**: Muestra bases de datos (PostgreSQL), bases vectoriales (Qdrant), capas de caché (Redis) e integraciones con APIs externas
- **Capa de infraestructura**: Representa containerización con Docker y arquitectura de despliegue

Usa subgraphs claros para organizar estas capas, aplica codificación de color consistente para diferentes tipos de componentes y muestra el flujo de datos entre componentes. Incluye tanto rutas síncronas de petición como flujos asíncronos de procesamiento cuando sea relevante para la épica.

#### 3. Funcionalidades de alto nivel y habilitadores técnicos

- Una lista de funcionalidades de alto nivel a construir.
- Una lista de habilitadores técnicos (por ejemplo, nuevos servicios, librerías, infraestructura) requeridos para soportar esas funcionalidades.

#### 4. Stack tecnológico

- Una lista de tecnologías, frameworks y librerías clave a utilizar.

#### 5. Valor técnico

- Estima el valor técnico (por ejemplo, High, Medium, Low) con una justificación breve.

#### 6. Estimación en talla T-Shirt

- Proporciona una estimación de alto nivel en t-shirt size para la épica (por ejemplo, S, M, L, XL).

## Plantilla de contexto

- **Epic PRD:** [El contenido del archivo markdown del Epic PRD]
