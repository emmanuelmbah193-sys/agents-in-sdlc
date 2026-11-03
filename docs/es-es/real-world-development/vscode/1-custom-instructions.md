---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Hay varias formas de compartir contexto específico con Copilot. Entre las más importantes están los [archivos de instrucciones][instruction-files], que permiten aportar indicaciones sobre cómo generar código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- generarás el primer fragmento de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un estándar general al repositorio en `.github/copilot-instructions.md`;
- volverás a ejecutar la misma indicación y observarás cómo el código generado incorpora el nuevo estándar;
- confirmarás los cambios de las instrucciones y del filtrado en `main` para que Copilot pueda utilizar las nuevas indicaciones en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que establezcas. Copilot no es determinista. El objetivo es observar la *tendencia* de los cambios en su comportamiento al actualizar las instrucciones, no conseguir que la salida coincida carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe estar en modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Mediante archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar convenciones de código, bibliotecas e incluso los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y aportar así contexto específico para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, relevante para la mayoría de las solicitudes que se envían a Copilot desde el chat o la CLI. Puede incluir la pila tecnológica, una descripción de lo que se está creando, prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos específicos. Sirven para proporcionar directrices para lenguajes (como TypeScript o Astro) o tareas (como crear un componente de interfaz o un conjunto de pruebas unitarias).

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat; no se aplican a las finalizaciones de código ni a las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite archivos de instrucciones con otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Una explicación completa sobre cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en indicaciones a nivel de proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas para tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas.
- Usa lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. Mediante la experimentación descubrirás qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, verás que hay archivos para distintos tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente lo indicará cuando sea relevante.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa la guía anidada **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un glob (relativo a la raíz del repositorio) que determina a qué archivos se aplica el contenido. En este caso, coincide con cualquier archivo de pruebas TypeScript, como uno que coincida con `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así se pueden dividir instrucciones extensas en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentario a nivel de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat para que Copilot pueda modificar archivos.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, normalmente en el helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una lista tipada de editoriales. Esta estructura procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de *consistencia* después de actualizar las instrucciones.

## Añade un estándar al repositorio

El archivo `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade estándares de programación al repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares de programación, pero todavía no incluye reglas para la documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Justo debajo de los estándares existentes, añade estas líneas de Markdown para indicar a Copilot que incluya encabezados de comentario a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones a nivel de repositorio (`.github/copilot-instructions.md`) para dar directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas específicos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a ejecutar la indicación y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contenido anterior.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **la misma indicación** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has indicado a Copilot que siga un estándar nuevo del proyecto y lo aplique al código real sobre el que trabajarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el mismo sistema de control de versiones que los demás archivos del repositorio. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirma que están presentes las actualizaciones de las instrucciones y el helper:

   ```bash
   git status
   ```

3. Prepara y confirma los cambios en las instrucciones y todos los archivos modificados por Copilot para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones auxiliares adicionales (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has publicado la rama. Ahora crea una solicitud de incorporación de cambios y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una solicitud que la cierre y combinarla.

> [!NOTE]
> En este ejercicio, Copilot realizará acciones en GitHub por primera vez. La plantilla del proyecto ya configura las herramientas de GitHub, así que no hace falta configurarlas. La primera vez que Copilot utilice una, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. Indícale que describa tanto las instrucciones como la base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la solicitud de incorporación de cambios.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
6. Después de crear la PR, pídele que la combine y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y, después, has usado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general al repositorio en `.github/copilot-instructions.md`;
- volver a enviar la misma indicación y observar cómo el código propuesto incorpora el nuevo estándar;
- confirmar los cambios de las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, aplicarás estas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad al código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
