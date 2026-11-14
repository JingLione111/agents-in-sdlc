---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de cambios (PR) en borrador para que la revise un desarrollador. Así, permite delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante definir claramente el alcance de las incidencias.
- cómo asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que los visitantes puedan seguir explorando. No necesita una implementación refinada; solo quiere algo que pueda usar para las pruebas de aceptación de la experiencia de usuario (UX). Aunque es un impedimento para el equipo, hay otras incidencias que tienen más prioridad en este momento.

Estas son tareas que pueden quedar relegadas con facilidad y que se pueden asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo de Copilot para asegurarse de que todo está bien.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Como ocurre al trabajar con una persona, hay varias formas de asignarle tareas; por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una solicitud de cambios en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes explorar la sesión de Copilot mientras está en curso o después de que termine. Cuando la solución esté lista para que la revises, Copilot te mencionará en la solicitud de cambios.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas al examinarlas en detalle, así que [define con cuidado el alcance de las tareas del agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian con la llegada de la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, examina en modo de solo lectura los archivos de instrucciones que ya incluye este repositorio. Así podrás observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub de tu repositorio o en un Codespace que ya esté en ejecución:

1. Abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en los metadatos `applyTo`, que limitan estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Estas prácticas proceden de los archivos de instrucciones. Volveremos a señalarlas al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Para generar código suele hacer falta un entorno específico y algunos scripts de configuración que garanticen que todo está listo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo de trabajo se ha configurado de antemano para facilitar el taller y permitirte explorar esta función. Proporciona a Copilot Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura solo los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo, para instalar dependencias, necesitan el permiso `contents: read`. Si no se clona el repositorio en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación completa ahora usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes probar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Todo el mundo entiende la importancia de la documentación, pero muchos proyectos tienen documentación desactualizada o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores al equipo. Copilot es especialmente útil para crear documentación, así que esta es una incidencia ideal para asignar al agente en la nube. El agente trabajará en segundo plano y generará la documentación necesaria. En un ejercicio posterior volverás para revisar su trabajo.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios (PR) nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está creando el entorno (ejecutando el flujo de trabajo de configuración anterior), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve el problema: explora el código y comprende el estado del proyecto, se detiene a analizar y decidir el plan adecuado y, finalmente, crea código.

Este proceso probablemente tardará varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es trabajar de forma asíncrona y permitirnos ocuparnos de otras tareas. Aprovecharás esta función asignándole otra tarea y, mientras tanto, escribirás código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas principales del agente en la nube de GitHub Copilot es que permite repartir el trabajo. Tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero ese tiempo se podría dedicar a otras tareas. Asigna el trabajo al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá una serie de 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot trabaja de forma parecida a un ingeniero de software; no hace falta que lo supervises continuamente. Revisa su trabajo cuando haya terminado. Mientras tanto, aprenderás a crear y usar agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Copilot puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

En este ejercicio:

- personalizaste el entorno de generación de código.
- comprobaste que las operaciones se realizaran de forma segura.
- aprendiste por qué es importante definir claramente el alcance de las incidencias.
- asignaste incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. [El agente en la nube de Copilot también admite servidores del Protocolo de contexto de modelos (MCP)][cloud-agent-mcp] y puede usar las instrucciones personalizadas que exploraste en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
