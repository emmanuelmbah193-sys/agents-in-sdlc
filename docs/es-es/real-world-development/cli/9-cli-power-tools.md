---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI completa, GitHub Copilot CLI incluye muchos comandos de barra. Estos permiten acceder a funcionalidades avanzadas, consultar información interna y cambiar la configuración. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explora algunos más.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, pero también puedes consultar o compactar el contexto tú mismo con comandos de barra.

1. Vuelve al codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se divide el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, quizá no veas un cambio importante.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Utiliza `/compact` si quieres elegir el momento. Usa `/clear` o `/new` cuando vayas a cambiar a una tarea que no esté relacionada y necesites iniciar una conversación nueva.

## Elige un modelo

Los distintos modelos tienen capacidades distintas, y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar y seleccionar el modelo que quieres utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y lo aprendido con el equipo ayuda a que todos mejoren el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede dividir subtareas independientes entre subagentes y ejecutarlas en paralelo;
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea;
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, define una tarea nueva con un alcance claro y revisa los resultados con tu proceso habitual. Para aprender más sobre el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar información interna. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has utilizado `/model` para explorar los modelos disponibles;
- has aprendido cómo `/share` puede exportar o compartir una sesión;
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI incluye más comandos de barra y capacidades por explorar. A continuación, [repasarás lo aprendido][next-lesson] y descubrirás cómo seguir practicando.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
