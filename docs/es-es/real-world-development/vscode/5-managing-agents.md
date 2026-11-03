---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como este trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición de archivo mientras se producen. Puedes revisar las diferencias insertadas, aceptar o rechazar cambios concretos y orientar la conversación con indicaciones de seguimiento para mejorar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- pedirás al agente, durante la sesión, que añada un modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat junto a las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramienta, lectura y edición de archivos, así como los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá junto con las diferencias:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para deshacerlo y dejar que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante esta sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que ya está implementado el modo de alto contraste, amplía la misma conversación para añadir un modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local que configuró.

1. Envía esta indicación de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios existentes y propondrá cómo añadir el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, igual que hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres (por ejemplo, intenta cambiar la interfaz de los controles de un modo que no te gusta), detenlo con el botón **Stop** junto al campo de chat y envía una indicación para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza indicaciones de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre varios archivos modificados y revisa sus diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición a medida que aparecía, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones de archivos;
- has dirigido una sesión en curso mediante indicaciones de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
