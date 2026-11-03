---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, autoriza Agent Merge y confirma que la PR se ha combinado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están confirmados en una rama. Ahora revísalos en conjunto y abre una solicitud de incorporación de cambios. Combinaste manualmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad, y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en una misma PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- revisarás la PR completa de la funcionalidad y las pruebas de verificación;
- autorizarás Agent Merge únicamente después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige comprobaciones de integración continua (CI) con errores, responde a comentarios de revisión y actualiza la rama mediante rebase cuando es necesario. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de ello, pero aún necesita tu autorización explícita para editar código y combinar cambios. Revisa las acciones permitidas y el trabajo antes de conceder el permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Permite que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Agent Merge empezará a trabajar. Hará lo siguiente:

- creará una solicitud de incorporación de cambios con un título y una descripción;
- si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción;
- actualizará la rama mediante rebase o gestionará conflictos de combinación con la rama de destino;
- supervisará el proceso de CI para comprobar que se superen todas las comprobaciones;
- supervisará los comentarios de otros desarrolladores y de la revisión de código de Copilot, y realizará cambios para resolverlos;
- opcionalmente, combinará la PR automáticamente cuando todo se haya completado correctamente.

Permite que Agent Merge también combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que está marcada la opción **Merge pull request**.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR sea **Merged**; que Agent Merge esté habilitado o trabajando no significa que la PR ya se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, así como la gestión de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has revisado la PR completa de la funcionalidad y las pruebas de verificación;
- has autorizado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de revisar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
