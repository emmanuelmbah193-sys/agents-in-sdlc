---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y así sucesivamente.

El modo de agente de Copilot te ayuda con este flujo.

El modo de agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, lo que permite recibir comentarios rápidamente e interactuar con él. El agente en la nube está pensado para trabajar de forma asíncrona como un miembro del equipo y comunicarse contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de incorporarlos al código base.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio de Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/` y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio

Para facilitar la ejecución del sitio, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Sigue estos pasos en el codespace de GitHub:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y cámbiate a una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con el siguiente comando:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor de desarrollo de Astro, aparecerá una dirección URL parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, donde se está ejecutando el servidor web.

### Explora el sitio

Cuando el sitio esté en marcha, explora sus funcionalidades principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla se generó automáticamente un conjunto de incidencias; pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no están recomendando un modelo frente a otro. Durante la elaboración del taller usamos Claude Sonnet 4.5, por lo que lo hemos incluido en estas instrucciones. El objetivo es que las sugerencias sean relativamente uniformes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es normal y esperable que recibas sugerencias distintas a las del taller.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP diferente, pero debería completar la tarea.

3. Envía a Copilot esta indicación para que te muestre el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionarás a Copilot como contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del propio archivo. Así se pueden reutilizar ejemplos, dividir instrucciones complejas en partes pequeñas y fáciles de gestionar, y proporcionar plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben superarse antes de integrar los cambios en el código base. El modo de agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura en la que se muestra cómo seleccionar el archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar los helpers de la capa de datos, la interfaz de usuario y las pruebas. Después modificará los archivos y ejecutará las pruebas.

   ![Captura en la que Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso quizá veas aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no hace falta que selecciones estos botones mientras está trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo en el panel Copilot Chat que pregunta si quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es normal que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completada con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

Has utilizado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has aprendido a:

- utilizar el modo de agente para implementar funcionalidades en la interfaz y la capa de datos;
- explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados antes de integrarlos en el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido para pedir a Copilot que la añada al sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación: por ejemplo, si permites que el usuario elija el tamaño de página o lo estableces de forma fija;
- proporciona a Copilot las indicaciones necesarias para crear la implementación que necesitas;
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Ese es el flujo normal cuando se trabaja con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: para qué sirven y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
