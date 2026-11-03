---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En este taller:

- has preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado precargada;
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado la funcionalidad en modo Autopilot y revisado el resultado en modo Interactive;
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- has revisado el cambio completo de filtrado y autorizado Agent Merge para combinar la PR;
- has explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Qué has publicado

El taller incluye dos cambios principales, cada uno en una rama distinta basada en `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito para los juegos que aún no tienen valoración.
2. **Flujo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Para agilizar el taller, esos cambios se combinaron en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y comunicó los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben resolver carencias reales. Si una ejecución de QA determina que no hacen falta pruebas adicionales, puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación y vuelve a recopilar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para cada tarea. Usa **Plan** para analizar el enfoque antes de programar, **Interactive** para seguir de cerca los cambios y **Autopilot** para tareas bien delimitadas. Utiliza un modelo rápido para cambios rutinarios y otro más capaz para tareas complejas.

El contexto importa tanto como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* puede cambiar considerablemente el resultado.

## Más opciones para explorar

Ya conoces el flujo de trabajo principal. Estas funcionalidades de CLI también pueden resultar útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir utilizándola: en código de producción, en proyectos personales o en esa pequeña aplicación que llevas años pensando en crear. Comparte con el equipo lo que aprendas y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
