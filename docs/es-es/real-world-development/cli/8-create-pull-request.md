---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están confirmados en una misma rama. Ahora revísalos en conjunto y abre una solicitud de incorporación de cambios. Combinaste manualmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en una misma PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- revisarás el cambio completo de la funcionalidad y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge únicamente después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el trabajo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR —corrige fallos de integración continua (CI), responde a comentarios de revisión y actualiza la rama mediante rebase cuando es necesario— y, después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ello. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utiliza Agent Merge para gestionar la PR

Ya has creado todo el código. Revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de agentes con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se supere, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, así como la gestión de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has revisado el cambio completo de la funcionalidad y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [explorarás comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
