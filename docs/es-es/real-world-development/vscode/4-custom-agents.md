---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio que ayuden a Copilot a realizar ciertas tareas con más eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las [pautas de accesibilidad para el contenido web (WCAG, por sus siglas en inglés)][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con al menos `name` y `description`, seguidos de una instrucción Markdown que define el comportamiento, la experiencia y las directrices del agente.

### Comparación entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; como parte del trabajo, podría utilizar habilidades para gestionar solicitudes de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte al entorno y a las situaciones de tu equipo.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes indican que a algunos usuarios les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad cuanto antes. Para ello, usarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente de accesibilidad. Primero, crea una rama nueva a partir de `main` para mantener separado el trabajo de accesibilidad y la solicitud de cambios (PR) de filtros del ejercicio 3.

1. Abre un terminal y ejecuta estos comandos:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Fíjate en los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

4. Examina las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript.
   - errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección del modo y el modelo varía entre ejercicios; se indica en los pasos pertinentes.

1. Si aún no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía esta instrucción al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en línea en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para mostrar el conjunto de archivos que ha modificado el agente.

> [!NOTE]
> El proceso probablemente tardará unos minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

En el ejercicio siguiente revisarás y orientarás este trabajo en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados de GitHub Copilot para tareas y ámbitos concretos. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En este ejercicio:

- revisaste un agente personalizado de accesibilidad existente.
- usaste el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
