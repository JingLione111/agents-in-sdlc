---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Recapitula el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y encontraste la incidencia de filtros del backlog.
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de cambios (PR).
- partiste de la incidencia de filtros, definiste el enfoque en el modo Plan, lo implementaste en Autopilot y lo revisaste en Interactive.
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad existente `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor del Protocolo de contexto de modelos (MCP) de Playwright y lo usaste para explorar los filtros en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la aptitud y las pruebas en el navegador.
- revisaste el cambio completo de los filtros y autorizaste Agent Merge para combinar la PR.
- exploraste comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en una rama basada en `main` actualizada:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtros y flujo de calidad:** implementa los filtros, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación de los filtros hasta la apertura de su PR, has usado la misma conversación y rama. Hemos combinado ese trabajo en una sola PR para agilizar el taller.

## Distintos tipos de validación

Has comprobado el código de varias formas: con pruebas automatizadas, examinando el sitio en un navegador y pidiendo a Copilot que lo explorara con MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó el nuevo formato de informe. QA reunió esos resultados con una revisión de los requisitos y de la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir lagunas reales; una ejecución de QA que no necesite pruebas nuevas puede ser correcta. Revisa el código y las pruebas, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para estudiar un enfoque antes de implementarlo, **Interactive** para mantener el control de cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para cambios rutinarios y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más funciones que explorar

Has cubierto el flujo de trabajo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Utilízala para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación de GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
