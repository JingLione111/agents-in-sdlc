---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido a Copilot que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Ahora explorarás los cambios de código y, si hace falta, enviarás comentarios a Copilot para que mejore el resultado.

### Situación

Como se ha señalado varias veces, los fundamentos del diseño de software y las operaciones de desarrollo (DevOps) no cambian con la llegada de la inteligencia artificial (IA) generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisaremos las propuestas de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza las tareas de forma asíncrona y sin supervisión, se han establecido medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para los cambios.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Protocolo de contexto de modelos (MCP).

## Revisa la documentación generada

Empezaremos con la primera solicitud de cambios (PR) que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas una advertencia que indica que el firewall bloquea a GitHub Copilot. Es lo esperado: el acceso del agente en la nube a recursos externos, incluidas las llamadas a servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar un momento o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la única aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de cambios no es un proceso unidireccional. Puedes mencionarlo en un comentario de la PR o en un comentario en línea sobre el código, igual que harías con otra persona del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados son probabilísticos, no podemos indicar un texto concreto que debas usar. Algunas ideas para solicitar cambios:

- Añadir a cada archivo de código un comentario de cabecera que describa brevemente su contenido.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot** como mencionarías a cualquier usuario. Puedes usar una de las sugerencias anteriores o pedir otro cambio en la documentación del código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Fíjate en que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud de cambios.

    ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, convierte la PR de borrador en una PR lista para revisión y combínala en la rama principal.

![Convertir una PR en borrador en una PR lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la única aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube para pedir cambios. Por ejemplo, podrías pedirle que ajuste la cantidad de juegos relacionados o que añada comentarios de cabecera y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de que actualizaras las instrucciones personalizadas). Para ello, añade otro comentario en la pestaña **Conversation**. Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste desde el panel de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de interfaz que permiten alternar entre los modos.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de hojas de estilo en cascada (CSS) o de estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las aplicaciones de internet enriquecidas accesibles (ARIA, por sus siglas en inglés) y la navegación con el teclado.
   - El código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la única aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Confirma que funcionan como se espera y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario para solicitar una actualización, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras tareas. Para adquirir esta práctica, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para que los patrocinadores muestren su interés en la página de detalles del juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizadas del repositorio** para observar más adelante su efecto en el trabajo del agente en la nube.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** para mejorar la accesibilidad con controles para los modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como panel de control** para supervisar y orientar la sesión de accesibilidad en curso.
- **Revisaste e iteraste sobre las solicitudes de cambios del agente en la nube**, mencionaste a `@copilot` para solicitar cambios y aprobaste los flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y la opción de delegar tareas al agente en la nube con `/delegate`.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

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
