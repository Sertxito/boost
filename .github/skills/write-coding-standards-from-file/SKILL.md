---
name: write-coding-standards-from-file
description: 'Escribe un documento de estándares de código para un proyecto usando los estilos de codificación de los archivos y/o carpetas pasados como argumentos en el prompt.'
---

# Escribir estándares de código desde archivo

Usa la sintaxis existente del/los archivo(s) para establecer los estándares y guías de estilo del proyecto. Si se pasa más de un archivo o una carpeta, recorre cada archivo, acumulando sus datos en memoria temporal o en un archivo temporal; al finalizar, usa esos datos como una única instancia, como si fuera el archivo base para definir los estándares y la guía de estilo.

## Reglas y configuración

A continuación se muestra un conjunto de variables de cuasi-configuración `boolean` y `string[]`. Las condiciones para manejar `true` u otros valores de cada variable están bajo el encabezado de nivel dos `## Condiciones de configuración de variables y parámetros`.

Los parámetros del prompt tienen una definición textual. Hay un parámetro obligatorio **`${fileName}`**, y varios parámetros opcionales **`${folderName}`**, **`${instructions}`** y cualquiera de **`[configVariableAsParameter]`**.

### Variables de configuración

* addStandardsTest = false;
* addToREADME = false;
* addToREADMEInsertions = ["atBegin", "middle", "beforeEnd", "bestFitUsingContext"];
  - Valor por defecto: **beforeEnd**.
* createNewFile = true;
* fetchStyleURL = true;
* findInconsistencies = true;
* fixInconsistencies = true;
* newFileName = ["CONTRIBUTING.md", "STYLE.md", "CODE_OF_CONDUCT.md", "CODING_STANDARDS.md", "DEVELOPING.md", "CONTRIBUTION_GUIDE.md", "GUIDELINES.md", "PROJECT_STANDARDS.md", "BEST_PRACTICES.md", "HACKING.md"];
  - For each file in `${newFileName}`, if file does not exist, use that file name and `break`, else continue to next file name of `${newFileName}`.
* outputSpecToPrompt = false;
* useTemplate = "verbose"; // or "v"
  - Los valores posibles son `[["v", "verbose"], ["m", "minimal"], ["b", "best fit"], ["custom"]]`.
  - Selecciona una de las dos plantillas de ejemplo al final del archivo del prompt bajo el encabezado de nivel dos `## Plantillas de estandares de codigo`, o usa otra composición que encaje mejor.
  - Si es **custom**, aplica según la solicitud.

### Variables de configuración como parámetros del prompt

Si alguno de los nombres de variable se pasa al prompt tal cual, o como un valor textual similar pero claramente relacionado, sobrescribe el valor por defecto con el valor recibido.

### Parámetros del prompt

* **fileName** = Nombre del archivo que se analizará en términos de: indentación, nombrado de variables, comentarios, procedimientos condicionales, procedimientos funcionales y otros datos de sintaxis del lenguaje del archivo.
* folderName = Nombre de la carpeta que se usará para extraer datos de múltiples archivos en un único conjunto agregado que será analizado en términos de: indentación, nombrado de variables, comentarios, procedimientos condicionales, procedimientos funcionales y otros datos de sintaxis del lenguaje de los archivos.
* instructions = Instrucciones, reglas y procedimientos adicionales para casos específicos.
* [configVariableAsParameter] = Si se pasa, sobrescribirá el estado por defecto de la variable de configuración. Ejemplo:
  - useTemplate = Si se pasa, sobrescribirá el valor por defecto de configuración `${useTemplate}`. Los valores son `[["v", "verbose"], ["m", "minimal"], ["b", "best fit"]]`.

#### Parámetros obligatorios y opcionales

* **fileName** - obligatorio
* folderName - *opcional*
* instructions - *opcional*
* [configVariableAsParameter] - *opcional*

## Condiciones de configuración de variables y parámetros

### `${fileName}.length > 1 || ${folderName} != undefined`

* Si es true, cambia `${fixInconsistencies}` a false.

### `${addToREADME} == true`

* Inserta los estándares de código en `README.md` en lugar de mostrarlos en el prompt o crear un archivo nuevo.
* Si es true, cambia `${createNewFile}` y `${outputSpecToPrompt}` a false.

### `${addToREADMEInsertions} == "atBegin"`

* Si `${addToREADME}` es true, inserta los estándares al **inicio** del archivo `README.md`, después del título.

### `${addToREADMEInsertions} == "middle"`

* Si `${addToREADME}` es true, inserta los estándares en la **mitad** del archivo `README.md`, ajustando el encabezado del título para que encaje con la composición de `README.md`.

### `${addToREADMEInsertions} == "beforeEnd"`

* Si `${addToREADME}` es true, inserta los estándares al **final** de `README.md`, agregando una línea nueva tras el último carácter y luego el contenido en una nueva línea.

### `${addToREADMEInsertions} == "bestFitUsingContext"`

* Si `${addToREADME}` es true, inserta los estándares en la **línea que mejor encaje** de `README.md` según el contexto y flujo del documento.

### `${addStandardsTest} == true`

* Una vez completado el archivo de estándares, crea un archivo de pruebas para verificar que el/los archivo(s) analizados cumplan esos estándares.

### `${createNewFile} == true`

* Crea un archivo nuevo usando el valor, o uno de los valores posibles, de `${newFileName}`.
* Si es true, cambia `${outputSpecToPrompt}` y `${addToREADME}` a false.

### `${fetchStyleURL} == true`

* Usa además los datos obtenidos de los enlaces bajo el encabezado de nivel tres `### Enlaces de referencia` como contexto para crear estándares, especificaciones y datos de estilo para el nuevo archivo, prompt o `README.md`.
* Para cada elemento relevante en `### Enlaces de referencia`, ejecuta `#fetch ${item}`.

### `${findInconsistencies} == true`

* Evalúa sintaxis relacionada con indentación, saltos de línea, comentarios, anidamiento de condicionales y funciones, comillas para strings (`'` o `"`), etc., y categorízala.
* Para cada categoría, cuenta ocurrencias y, si algún item no coincide con la mayoría, guárdalo en memoria temporal.
* Según el estado de `${fixInconsistencies}`, edita y corrige las categorías minoritarias para alinearlas con la mayoría, o muestra en el prompt las inconsistencias almacenadas en memoria temporal.

### `${fixInconsistencies} == true`

* Edit and fix the low count categories of syntax data to match the majority of corresponding syntax data using inconsistencies stored in temporary memory.

### `typeof ${newFileName} == "string"`

* If specifically defined as a `string`, create a new file using the value from `${newFileName}`.

### `typeof ${newFileName} != "string"`

* If **NOT** specifically defined as a `string`, but instead an `object` or an array, create a new file using a value from `${newFileName}` by applying this rule:
  - For each file name in `${newFileName}`, if file does not exist, use that file name and `break`, else continue to the next.

### `${outputSpecToPrompt} == true`

* Output the coding standards to the prompt instead of creating a file or adding to README.
* If true, toggle both `${createNewFile}` and `${addToREADME}` to false.

### `${useTemplate} == "v" || ${useTemplate} == "verbose"`

* Use data under the level three heading `### "v", "verbose"` as guiding template when composing the data for coding standards.

### `${useTemplate} == "m" || ${useTemplate} == "minimal"`

* Use data under the level three heading `### "m", "minimal"` as guiding template when composing the data for coding standards.

### `${useTemplate} == "b" || ${useTemplate} == "best"`

* Use either the data under the level three heading `### "v", "verbose"` or `### "m", "minimal"`, depending on the data extracted from `${fileName}`, and use the best fit as guiding template when composing the data for coding standards.

### `${useTemplate} == "custom" || ${useTemplate} == "<ANY_NAME>"`

* Use the custom prompt, instructions, template, or other data passed as guiding template when composing the data for coding standards.

## **if** `${fetchStyleURL} == true`

Depending on the programming language, for each link in list below, run `#fetch (URL)`, if programming language is `${fileName} == [<Language> Style Guide]`.

### Enlaces de referencia

- [C Style Guide](https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html)
- [C# Style Guide](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [C++ Style Guide](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [Go Style Guide](https://github.com/golang-standards/project-layout)
- [Java Style Guide](https://coderanch.com/wiki/718799/Style)
- [AngularJS App Style Guide](https://github.com/mgechev/angularjs-style-guide)
- [jQuery Style Guide](https://contribute.jquery.org/style-guide/js/)
- [JavaScript Style Guide](https://www.w3schools.com/js/js_conventions.asp)
- [JSON Style Guide](https://google.github.io/styleguide/jsoncstyleguide.xml)
- [Kotlin Style Guide](https://kotlinlang.org/docs/coding-conventions.html)
- [Markdown Style Guide](https://cirosantilli.com/markdown-style-guide/)
- [Perl Style Guide](https://perldoc.perl.org/perlstyle)
- [PHP Style Guide](https://phptherightway.com/)
- [Python Style Guide](https://peps.python.org/pep-0008/)
- [Ruby Style Guide](https://rubystyle.guide/)
- [Rust Style Guide](https://github.com/rust-lang/rust/tree/HEAD/src/doc/style-guide/src)
- [Swift Style Guide](https://www.swift.org/documentation/api-design-guidelines/)
- [TypeScript Style Guide](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)
- [Visual Basic Style Guide](https://en.wikibooks.org/wiki/Visual_Basic/Coding_Standards)
- [Shell Script Style Guide](https://google.github.io/styleguide/shellguide.html)
- [Git Usage Style Guide](https://github.com/agis/git-style-guide)
- [PowerShell Style Guide](https://github.com/PoshCode/PowerShellPracticeAndStyle)
- [CSS](https://cssguidelin.es/)
- [Sass Style Guide](https://sass-guidelin.es/)
- [HTML Style Guide](https://github.com/marcobiedermann/html-style-guide)
- [Linux kernel Style Guide](https://www.kernel.org/doc/html/latest/process/coding-style.html)
- [Node.js Style Guide](https://github.com/felixge/node-style-guide)
- [SQL Style Guide](https://www.sqlstyle.guide/)
- [Angular Style Guide](https://angular.dev/style-guide)
- [Vue Style Guide](https://vuejs.org/style-guide/rules-strongly-recommended.html)
- [Django Style Guide](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/)
- [SystemVerilog Style Guide](https://github.com/lowRISC/style-guides/blob/master/VerilogCodingStyle.md)

## Plantillas de estandares de codigo

### `"m", "minimal"`

```text
    ```markdown
    ## 1. Introduction
    *   **Purpose:** Briefly explain why the coding standards are being established (e.g., to improve code quality, maintainability, and team collaboration).
    *   **Scope:** Define which languages, projects, or modules this specification applies to.

    ## 2. Naming Conventions
    *   **Variables:** `camelCase`
    *   **Functions/Methods:** `PascalCase` or `camelCase`.
    *   **Classes/Structs:** `PascalCase`.
    *   **Constants:** `UPPER_SNAKE_CASE`.

    ## 3. Formatting and Style
    *   **Indentation:** Use 4 spaces per indent (or tabs).
    *   **Line Length:** Limit lines to a maximum of 80 or 120 characters.
    *   **Braces:** Use the "K&R" style (opening brace on the same line) or the "Allman" style (opening brace on a new line).
    *   **Blank Lines:** Specify how many blank lines to use for separating logical blocks of code.

    ## 4. Commenting
    *   **Docstrings/Function Comments:** Describe the function's purpose, parameters, and return values.
    *   **Inline Comments:** Explain complex or non-obvious logic.
    *   **File Headers:** Specify what information should be included in a file header, such as author, date, and file description.

    ## 5. Error Handling
    *   **General:** How to handle and log errors.
    *   **Specifics:** Which exception types to use, and what information to include in error messages.

    ## 6. Buenas practicas y anti-patrones
    *   **General:** List common anti-patterns to avoid (e.g., global variables, magic numbers).
    *   **Language-specific:** Specific recommendations based on the project's programming language.

    ## 7. Examples
    *   Provide a small code example demonstrating the correct application of the rules.
    *   Provide a small code example of an incorrect implementation and how to fix it.

    ## 8. Contribution and Enforcement
    *   Explain how the standards are to be enforced (e.g., via code reviews).
    *   Provide a guide for contributing to the standards document itself.
    ```
```

### `"v", verbose"`

```text
    ```markdown

    # Style Guide

    This document defines the style and conventions used in this project.
    All contributions should follow these rules unless otherwise noted.

    ## 1. General Code Style

    - Favor clarity over brevity.
    - Keep functions and methods small and focused.
    - Avoid repeating logic; prefer shared helpers/utilities.
    - Remove unused variables, imports, code paths, and files.

    ## 2. Naming Conventions

    Use descriptive names. Avoid abbreviations unless well-known.

    | Item            | Convention           | Example            |
    |-----------------|----------------------|--------------------|
    | Variables       | `lower_snake_case`   | `buffer_size`      |
    | Functions       | `lower_snake_case()` | `read_file()`      |
    | Constants       | `UPPER_SNAKE_CASE`   | `MAX_RETRIES`      |
    | Types/Structs   | `PascalCase`         | `FileHeader`       |
    | File Names      | `lower_snake_case`   | `file_reader.c`    |

    ## 3. Formatting Rules

    - Indentation: **4 spaces**
    - Line length: **max 100 characters**
    - Encoding: **UTF-8**, no BOM
    - End files with a newline

    ### Braces (example in C, adjust for your language)

        ```c
        if (condition) {
            do_something();
        } else {
            do_something_else();
        }
        ```

    ### Spacing

    - One space after keywords: `if (x)`, not `if(x)`
    - One blank line between top-level functions

    ## 4. Comments & Documentation

    - Explain *why*, not *what*, unless intent is unclear.
    - Keep comments up-to-date as code changes.
    - Public functions should include a short description of purpose and parameters.

    Recommended tags:

        ```text
        TODO: follow-up work
        FIXME: known incorrect behavior
        NOTE: non-obvious design decision
        ```

    ## 5. Error Handling

    - Handle error conditions explicitly.
    - Avoid silent failures; either return errors or log them appropriately.
    - Clean up resources (files, memory, handles) before returning on failure.

    ## 6. Commit & Review Practices

    ### Commits
    - One logical change per commit.
    - Write clear commit messages:

        ```text
        Short summary (max ~50 chars)
        Optional longer explanation of context and rationale.
        ```

    ### Reviews
    - Keep pull requests reasonably small.
    - Be respectful and constructive in review discussions.
    - Address requested changes or explain if you disagree.

    ## 7. Tests

    - Write tests for new functionality.
    - Tests should be deterministic (no randomness without seeding).
    - Prefer readable test cases over complex test abstraction.

    ## 8. Changes to This Guide

    Style evolves.
    Propose improvements by opening an issue or sending a patch updating this document.
    ```
```
