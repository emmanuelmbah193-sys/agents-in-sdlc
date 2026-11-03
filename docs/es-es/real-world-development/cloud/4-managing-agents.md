---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, también es importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, como la [página de agentes][agents-page] en GitHub.com. Desde este centro puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, explorar sus operaciones e incluso dirigir las sesiones que están en curso.

En esta lección:

- explorarás la página de agentes para supervisar sus tareas en la nube;
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo funcionalidad para cambiar de modo, parecía razonable incluirlo. Dirigirás el trabajo del agente para que añada también el modo claro.

## Revisa las tareas del agente de GitHub Copilot en la nube

Consulta el estado de las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - clonar el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos concretos que se muestran dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios que aparece a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados para seguir el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que utilice Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurarse y ha empezado a trabajar. Verás mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta indicación:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot la recibe y la incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos; puedes aprovechar para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro desde el que supervisar y dirigir las tareas del agente de GitHub Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo realizado y redirigir las tareas en curso para ajustar su alcance o añadir indicaciones.

En este ejercicio:

- has explorado mission control y la página de agentes para supervisar las tareas;
- has redirigido una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
