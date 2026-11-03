---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot utilice un navegador real para probar la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor Playwright MCP al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede utilizar herramientas externas al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a las herramientas y los recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios o herramientas externos. El servidor MCP gestiona la comunicación, por ejemplo, con API existentes o herramientas locales como paquetes de NPM. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub. Permite que el agente realice acciones como crear repositorios, actualizarlos y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador mediante Playwright. Permite que el agente navegue por páginas web, rellene formularios y seleccione botones.

Hay muchos otros servidores MCP que proporcionan acceso a herramientas y recursos distintos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar un servidor MCP, revisa su código fuente, comprueba quién lo publica y ten en cuenta las consecuencias de seguridad. Utiliza únicamente servidores de confianza y sé prudente al conceder acceso a recursos o acciones confidenciales.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`, que permitió a Copilot leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hay que instalar nada en local. VS Code se conecta mediante HTTP y la primera vez que Copilot utiliza una herramienta te autenticas con GitHub.

## Añade el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable que utilizará para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es **local**: VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio que la muestre.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente ya está instalado el navegador que necesita. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando los necesita**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y selecciona la opción para confiar en él.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y te pedirá que confirmes si confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real. En lugar de que compruebes manualmente la aplicación, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Es una forma rápida de comprobar la funcionalidad sin salir de la conversación.

Internamente, el servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como la tecnología de asistencia, por lo que una prueba funcional también sirve como comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor Playwright MCP, realizará cada paso y comunicará el resultado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona como esperabas, haz preguntas de seguimiento o pide que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot utiliza el servidor Playwright MCP puede tener que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Has confirmado que la funcionalidad funciona. Ahora abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una solicitud desde github.com, la interfaz de línea de comandos de GitHub u otros medios. Como ya estás utilizando GitHub Copilot, deja que la cree por ti. Puede buscar la incidencia correspondiente y vincularla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si hace falta, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que utilice una herramienta de GitHub, quizá debas autenticarte; sigue las instrucciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplía Copilot con nuevas herramientas;
- has añadido el servidor Playwright MCP al espacio de trabajo;
- lo has utilizado para probar manualmente la funcionalidad de filtrado en el navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de incorporación de cambios.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
