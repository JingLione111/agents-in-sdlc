---
title: "Lección 5 - Personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué pasa con las pruebas? ¿Y el lint? ¿Y las demás partes de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción adecuada. Las habilidades ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de los resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Situación

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo contiene metadatos al principio, con un nombre y una descripción que definen la habilidad, seguidos de una descripción general de sus funciones e indicaciones sobre cuándo debe usarse. La carpeta también puede contener subcarpetas con scripts y otros recursos que la habilidad utiliza cuando se invoca.

> [!NOTE]
> Las carpetas y los archivos adicionales no son obligatorios. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el valor de `name` y la descripción que aparecen al principio. La descripción ayuda a Copilot a saber cuándo debe usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el proceso de lint.
4. Fíjate en que la habilidad aún no incluye una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Puedes invocar las habilidades directamente desde Copilot CLI o pedirlo con lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado, que debe estar en el modo Interactive.
2. Envía esta instrucción:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe que aparece al final.

## Personaliza el informe

Mejora el informe para que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han devuelto las herramientas. Actualiza la habilidad para generar ese informe.

1. Vuelve a abrir `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Ahora que has actualizado la habilidad, comprueba cómo funciona. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente la misma instrucción que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de los resultados.
- volviste a cargar la habilidad y la ejecutaste.

Este cambio se incluirá con los filtros en la solicitud de cambios (PR) de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright, basado en el Protocolo de contexto de modelos (MCP)][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo para contribuir: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
