---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a modificar el código... La lista continúa.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Explora cómo usar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE para facilitar ciclos de respuesta e interacción rápidos. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- el modo agente de GitHub Copilot puede implementar funciones en la interfaz de usuario (UI) y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código.

## Situación

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. Ya has creado un helper de editoriales en el ejercicio anterior. Ahora completarás el resto del trabajo de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas en el momento de la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el mapeador objeto-relacional (ORM) Drizzle en los metadatos de sus páginas; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/`, y cualquier interacción se añade con un `<script>` de Astro pequeño y específico que usa las API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio

Para facilitar la ejecución del sitio, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Ejecútala en GitHub Codespaces de la siguiente manera:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en Codespaces pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para trabajar en los filtros y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En Codespaces, al seleccionar el enlace local desde el terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al Codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección del modo y el modelo varía entre ejercicios; se indica en los pasos pertinentes.

La implementación inicial del sitio funciona, pero queremos añadir más funcionalidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te lo muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar y aplicar cambios en varios archivos de forma autónoma, ejecutar comandos del terminal e invocar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Al preparar el taller, usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea buena. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM, por sus siglas en inglés) son probabilísticos, las sugerencias que recibas pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot podría usar un comando distinto del Protocolo de contexto de modelos (MCP) y, aun así, completar la tarea.

3. Envía a Copilot esta instrucción para consultar las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista que ha generado.

Observa que Copilot también ha priorizado las incidencias según cuáles cree que serán más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para tipos de archivo concretos, como:
     - Astro.
     - Tailwind CSS y las hojas de estilo en cascada (CSS).
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar y dividir instrucciones complejas en archivos más pequeños y manejables, y proporcionar ejemplos y plantillas.

## Implementa los filtros

Para completar los filtros, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de los filtros.
- actualizar la página de lista de juegos para incluir la interfaz de los filtros.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios en el código. El modo agente de Copilot puede realizar estas tareas. Añade la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla que muestra cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar los helpers de la capa de datos, la interfaz y las pruebas; después, modificará los archivos y ejecutará las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras tanto, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, tendrás un botón **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla del cuadro de diálogo de Copilot Chat para confirmar que quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se detenga varias veces y que algunas pruebas fallen. No pasa nada: Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. En concreto:

- viste cómo el modo agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- viste cómo puede implementar funcionalidades en la interfaz y la capa de datos.
- revisaste los cambios y las pruebas generados antes de combinarlos en el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que aumente la lista de juegos, será necesario implementar la paginación. Usa lo que has aprendido en este ejercicio y pide a Copilot que añada esta función al sitio. Al planificar la instrucción:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación, por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- proporciona a Copilot en la instrucción el contexto necesario para que la implementación cumpla tus requisitos.
- puede que tengas que iterar con GitHub Copilot y pedir cambios o proporcionar más contexto. Es el proceso habitual al trabajar con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
