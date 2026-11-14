---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa los filtros, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación de los filtros, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlo todo junto y abrir una solicitud de cambios (PR). Combinaste la PR de las valoraciones con estrellas manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtros y calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de validación.
- crearás la PR de los filtros.
- activarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Situación

Durante el flujo de trabajo de los filtros, has usado Copilot para planificar, implementar y validar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes de la PR sin quitar al desarrollador el control de autorización de la combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Deja que Agent Merge gestione la PR

Ya has creado todo el código. Ahora revisarás los cambios, crearás la PR y dejarás que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la combinación.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y, cuando el repositorio lo permita, realiza manualmente la combinación revisada.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación y, ahora, el proceso de la solicitud de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de validación.
- creaste la PR de los filtros.
- activaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a un agente en la nube.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
