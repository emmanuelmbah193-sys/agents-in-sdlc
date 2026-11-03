---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, desarrollara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios de código y, si hace falta, proporciona comentarios a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos comentado, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las sugerencias de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para garantizar que el entorno siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente de GitHub Copilot en la nube: la documentación del código. Lo harás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas una advertencia que indica que el firewall bloqueó a GitHub Copilot. Es lo esperado: el acceso del agente a recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com en una nueva pestaña del navegador.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso que indica que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es una comunicación unidireccional. Puedes mencionarlo en comentarios de la solicitud o en comentarios insertados en el código, igual que harías con otra persona del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos prescribir un texto concreto. Por ejemplo, podrías pedirle que:

- añada encabezados de comentario al principio de cada archivo de código con una breve descripción de su propósito;
- añada comentarios TSDoc a los archivos TypeScript y Astro;
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes utilizar una de las ideas anteriores o pedir otra mejora de documentación para el código base.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para actualizar los archivos.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una nueva confirmación en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, marca la PR como lista para revisión y combínala.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para resolver la incidencia sobre la visualización de juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un aviso que indica que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa y pedirle que haga cambios; por ejemplo, puedes modificar el número de juegos relacionados o pedirle que añada encabezados de comentario y comentarios TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Añade un comentario en la pestaña **Conversation** y Copilot lo utilizará para iniciar otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisa las mejoras de accesibilidad que implementó el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios CSS o de estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso que indica que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu codespace para probar manualmente las mejoras de accesibilidad. Abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionen y que la selección se conserve al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a guiar a Copilot para implementar estas funcionalidades siguiendo las prácticas recomendadas. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para encargarse de tareas reales del backlog mientras tú trabajas en otra cosa. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para indicar interés en un juego en la página de detalles;
- añadir paginación a la página de listado de juegos;
- validar las entradas y gestionar errores en los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas del repositorio** para ver cómo influyen más adelante en los resultados del agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como mission control** para supervisar y dirigir la sesión de accesibilidad en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y pasos siguientes

Has completado el recorrido del agente en la nube. Si quieres seguir aprendiendo, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
