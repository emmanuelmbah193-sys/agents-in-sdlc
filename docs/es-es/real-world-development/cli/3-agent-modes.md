---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, implementa el filtrado de una incidencia con Autopilot y revisa el resultado en modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más completo. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta y varias lecciones siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código, validarás el comportamiento y, finalmente, integrarás el cambio en el proyecto.

> [!NOTE]
> A medida que avances en este flujo de trabajo, utilizarás la misma conversación y rama para la funcionalidad. Normalmente se utilizarían ramas o PR diferentes para distintos tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub;
- definirás los requisitos en modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás manualmente la funcionalidad en un navegador reenviado.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Utilizarás el modo Plan para tomar esas decisiones y, después, autorizarás a Autopilot a implementar un alcance definido.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se superen las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos son similares al anterior.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar entre modos:

- **Interactive**: trabajas junto al agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente primero crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar el filtrado, vuelve al codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve al codespace. Si está detenido, reinícialo.
2. Confirma que la PR de valoraciones por estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Con las flechas, selecciona la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadirla a la indicación y volver a la pestaña **Session**.

Observa que la indicación ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planifica el filtrado

La planificación permite definir cómo se implementará una funcionalidad o realizará una tarea antes de delegársela a Copilot. Es buena idea dedicar algo de tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que elabore el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo de la indicación muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que cambie cualquier parte del plan que no coincida con tus decisiones.

## Autoriza Autopilot

El plan ya está redactado y revisado. Ahora permite que Copilot lo implemente con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre parecido en tu versión.
2. Confirma que el indicador debajo de la indicación muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y valida la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisa el código y ejecuta el sitio para confirmar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para pasar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Después de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier fallo.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y ábrela mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Comprueba que el reinicio de los filtros y el comportamiento cuando no hay resultados coincidan con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub;
- has definido los requisitos en modo Plan;
- has implementado la funcionalidad con Autopilot;
- has revisado el código;
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, descubrirás cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
