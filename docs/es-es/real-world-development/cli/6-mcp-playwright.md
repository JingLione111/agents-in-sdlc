---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y proporcionar a Copilot automatizaciones adicionales. Para ello sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en CLI y acceda a más herramientas y servicios.

En esta lección:

- aprenderás qué es el Protocolo de contexto de modelos (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor Playwright MCP si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtros.

## Situación

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario también hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría un usuario y automatizar aún más la comprobación de los cambios.

## ¿Qué es el Protocolo de contexto de modelos (MCP)?

El [Protocolo de contexto de modelos (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. Cada servidor proporciona al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP con el comando siguiente:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora usarás el servidor Playwright MCP disponible en Copilot para repetir la comprobación.

1. Usa esta instrucción para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no hace falta pedir expresamente a Copilot que use el servidor MCP, porque suele seleccionarlo por sí mismo. Aun así, indicar la herramienta que debe usar ayuda a orientar la tarea y a obtener resultados más coherentes.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y comprueba que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- aprendiste qué es el Protocolo de contexto de modelos (MCP) y cómo lo usa Copilot CLI.
- añadiste el servidor Playwright MCP si aún no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad de filtros.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
