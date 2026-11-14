---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear los filtros con el modo agente. Antes de abrir una solicitud de cambios, confirma que funcionan en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real. Después, publicarás la rama y abrirás la solicitud de cambios.

En este ejercicio:

- aprenderás qué es el Protocolo de contexto de modelos (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente los filtros en un navegador.
- publicarás la rama y abrirás una solicitud de cambios para el trabajo de filtros.

## ¿Qué es el Protocolo de contexto de modelos (MCP)?

El modo agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante el Protocolo de contexto de modelos (MCP), un estándar que permite al agente de inteligencia artificial (IA) comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje grande y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [Protocolo de contexto de modelos (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como las API existentes o las herramientas locales en paquetes de npm. Cada servidor proporciona al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a distintas interfaces de programación de aplicaciones (API) para gestionar repositorios de GitHub. Permite al agente realizar acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de cambios.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador con Playwright. Permite al agente, por ejemplo, navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar un servidor MCP, revisa cuidadosamente su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones confidenciales.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías encontrar un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto ya incluye la entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada localmente: VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

A continuación, añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en un Codespace, que no tiene escritorio para mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP **a petición**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar ahora el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code iniciará el servidor y te preguntará si confías en él la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la funcionalidad de filtros

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de recorrer la aplicación manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados para resumir lo que observa. Así puedes comprobar rápidamente el comportamiento de una funcionalidad sin salir de la conversación.

El servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Por eso, el agente procesa elementos estructurados y etiquetados —como botones, enlaces y elementos de lista— de una forma parecida a las tecnologías de asistencia. Una comprobación funcional rápida también permite hacer una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe los filtros que acabas de implementar.

1. Envía a Copilot esta instrucción:

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

Copilot abrirá un navegador mediante el servidor Playwright MCP, seguirá los pasos y te informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como esperas, haz preguntas de seguimiento o pide que corrija el código antes de abrir una solicitud de cambios.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la instrucción. La primera vez que Copilot use Playwright MCP, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de cambios

Ahora que has confirmado que la funcionalidad funciona, puedes abrir una solicitud de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** en el Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crea la solicitud de cambios

Hay varias formas de crear una solicitud de cambios, por ejemplo, desde github.com o mediante la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que cree la PR. Puede encontrar la incidencia pertinente y relacionarla con la solicitud.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si hace falta, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte. Sigue las instrucciones para permitir el acceso.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- aprendiste qué es el Protocolo de contexto de modelos (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadiste el servidor Playwright MCP al espacio de trabajo.
- lo usaste para probar manualmente los filtros en un navegador antes de publicar el cambio.
- publicaste la rama y abriste una solicitud de cambios para el trabajo de filtros.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
