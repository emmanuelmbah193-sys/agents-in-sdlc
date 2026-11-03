---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos comentado, escribir código implica mucho más que teclearlo. Necesitamos trabajar con datos y servicios externos, y permitir que Copilot utilice automatizaciones adicionales. Para ello sirven los servidores MCP: permiten ampliar las capacidades incluidas en Copilot CLI con más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor Playwright MCP si aún no está disponible;
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot utilice el sitio como lo haría un usuario para automatizar las comprobaciones y aumentar la confianza en que los cambios funcionen según lo esperado.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a las herramientas y los recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios de GitHub, incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP][mcp-registry] para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las consecuencias de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Introduce este comando en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes has comprobado manualmente que la funcionalidad se comporta como esperabas. Ahora utiliza el servidor Playwright MCP disponible en Copilot para hacer la misma comprobación.

1. Envía esta indicación para que Copilot utilice Playwright MCP y valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario indicar a Copilot que utilice el servidor MCP, ya que normalmente lo determina por sí mismo. Sin embargo, especificar la herramienta puede ayudarle a elegir el enfoque adecuado, ofrecer resultados más coherentes y reducir el uso de tokens.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperabas.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has utilizado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- has añadido el servidor Playwright MCP si aún no estaba disponible;
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
