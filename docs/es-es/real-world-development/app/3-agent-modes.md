---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más completo. La aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta y varias lecciones siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código, validarás el comportamiento y, finalmente, integrarás el cambio en el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de la funcionalidad, utilizarás la misma sesión. Normalmente se utilizarían sesiones o PR diferentes para distintos tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva sesión de agente desde una incidencia de GitHub;
- definirás los requisitos en modo **Plan**;
- implementarás la funcionalidad con el modo **Autopilot**;
- revisarás el código;
- validarás manualmente la funcionalidad en un lienzo de navegador.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan acotar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar algunos detalles, como la combinación de categorías, antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, adquieren aún más importancia. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia registrada que describa el trabajo.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se superen las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos son similares al anterior.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla la autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo de indicaciones y cambiarlo en cualquier momento:

- **Interactive**: trabajas junto al agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente primero crea un plan. Revisas y apruebas el plan antes de que lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu rama local `main` está actualizada.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente incluida en la plantilla; no necesitas crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que hayan fallado. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobaciones de tipos mediante las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador de modo muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Verás cómo sigue el plan, genera código y ejecuta pruebas de forma iterativa.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y valida la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisa el código y ejecuta el sitio para comprobar que todo funcione.

1. Abre **Changes** y revisa la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y las comprobaciones de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor que se está probando pertenece a este worktree y no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y ábrela en un lienzo de navegador.

1. Envía esta indicación para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro y, después, selecciona una editorial y confirma que los resultados coinciden.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició en esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el worktree y la rama para las Lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y añadirás mejoras de habilidad y QA antes de crear y combinar la PR conjunta en la Lección 8. Cuando hayas detenido el servidor, puedes dejar la sesión inactiva y volver a ella en la lección siguiente.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- has iniciado una nueva sesión de agente desde una incidencia de GitHub;
- has definido los requisitos en modo **Plan**;
- has implementado la funcionalidad con **Autopilot**;
- has revisado el código;
- has validado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
