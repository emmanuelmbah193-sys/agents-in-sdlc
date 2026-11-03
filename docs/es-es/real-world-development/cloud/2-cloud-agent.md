---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas con la deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizaciones o funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, abre una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en asuntos más importantes.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que, de hecho, falta por completo. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. En la página de detalles de cada juego deberían aparecer otros títulos de la misma categoría para que los usuarios puedan seguir explorando. No necesitan una implementación pulida todavía, solo algo que les permita probar la experiencia. Aunque esto bloquea su trabajo, hay otras incidencias de mayor prioridad.

Estas son tareas que suelen quedar relegadas. Son buenas candidatas para el agente de GitHub Copilot en la nube, que puede trabajar en segundo plano mientras tú te ocupas de otras tareas. Después podrás volver para revisar su trabajo y comprobar que el resultado sea el esperado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que con una persona, puedes trabajar con él de varias formas, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una solicitud de incorporación de cambios en borrador para seguir el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando esté lista para revisar, Copilot te mencionará en la solicitud.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador en pareja con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En los ejercicios anteriores has aprendido cómo las instrucciones personalizadas guían a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, aprovecha ahora para revisarlo rápidamente. Antes de asignar trabajo al agente, lee los archivos de instrucciones que incluye este repositorio sin modificarlos para poder observar después su efecto.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation` en la sección siguiente, observa si la PR resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas indicaciones proceden de los archivos de instrucciones. Volveremos a comentarlo al revisar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente

Crear código suele requerir un entorno específico y scripts de configuración para instalar dependencias y preparar el proyecto. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para el laboratorio: instala Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todos reconocen la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esa deuda técnica ralentiza el trabajo y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación; por eso, esta es una buena tarea para asignarle. Trabajará en segundo plano y revisarás sus cambios en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. En unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando su entorno (ejecutando el flujo de trabajo descrito anteriormente), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la solicitud y busca una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve la tarea: explora el código y comprende su estado, se detiene a pensar y planificar el enfoque, y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que puede trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharemos esa característica para asignarle otra tarea y, mientras tanto, escribir código que añada funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una gran ventaja del agente en la nube es que permite dividir el trabajo: tú puedes centrarte en una tarea y el agente en otra. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes aprovechar ese tiempo para otras cosas. Asígnale la tarea al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo verás 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Como el agente funciona de forma parecida a un ingeniero de software, no hace falta supervisarlo continuamente: puedes revisar el resultado cuando termine. Mientras tanto, crea y utiliza agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona: por ejemplo, resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir claramente el alcance de las incidencias;
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y recibe las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
