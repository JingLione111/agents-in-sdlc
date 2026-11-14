---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita cierta información de fondo, debes asegurarte de que pueda acceder a ese contexto. Hay varias formas de compartir contexto específico con Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos aplicables a rutas concretas.
- generarás la primera parte de la capa de datos para los filtros (un helper de editoriales) con las instrucciones actuales.
- añadirás un estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a enviar la misma instrucción y observarás cómo el código generado aplica el estándar nuevo.
- confirmarás los cambios de instrucciones y la primera parte de la funcionalidad de filtros en `main` para que Copilot use las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas indicado. Copilot no es determinista. El objetivo es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener siempre exactamente el mismo resultado.

## Archivos de instrucciones

### Situación

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario (UI) debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su contenido.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor el estilo de programación y los requisitos. Permiten orientar sus respuestas y sugerencias de código. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, como la tecnología utilizada, una descripción de lo que se está creando, las prácticas recomendadas y otras directrices globales.
- `.github/instructions/*.instructions.md`: archivos que proporcionan directrices para tareas o tipos de archivos concretos. Puedes usarlos para lenguajes específicos, como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo` cuando generan código.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Hablar en detalle sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivos (pruebas unitarias, componentes Astro y la capa de datos) o de tareas concretas.
- Usa lenguaje natural. Redacta directrices claras e incluye ejemplos de cómo debe y no debe ser el código.

No hay una única forma de crear archivos de instrucciones ni de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para el proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería contar con un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que encuentres instrucciones para numerosas tareas, como las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando explicará la opción pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas sobre **GitHub Actions Workflows**. Estas instrucciones se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón global relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, se aplican a archivos de pruebas TypeScript como los que coinciden con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás directrices para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para estos cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección del modo y el modelo varía entre ejercicios; se indica en los pasos pertinentes.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una instrucción con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar la misma instrucción para comparar el resultado.

1. Cierra las pestañas del editor que dejaste abiertas en los ejercicios anteriores para que Copilot solo use el contexto necesario.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe estar el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda modificar los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta instrucción:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, que a menudo abarcan el archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como se ha explicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade a los estándares de programación del repositorio las directrices que faltan para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero aún no incluye reglas para documentar el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade estas líneas de Markdown justo después de los estándares existentes. Así pedirás a Copilot que añada comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a enviar la instrucción y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **la misma instrucción** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo del proyecto y lo aplique a código real que se usará en el siguiente ejercicio.

## Crea un commit y publica la rama con las instrucciones

Los archivos de instrucciones se gestionan con el control de código fuente, igual que los demás archivos del repositorio. Crea un commit con los cambios y publica la rama.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que los cambios en las instrucciones y el helper están presentes:

   ```bash
   git status
   ```

3. Prepara la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear la base del helper, y crea un commit:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros archivos auxiliares (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de cambios

Ahora que has publicado la rama, crea una solicitud de cambios (PR) y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear la PR para resolverla y, después, combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub en el entorno, así que no hace falta configurar nada. La primera vez que Copilot use una herramienta de GitHub, VS Code podría pedirte que inicies sesión. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante el Protocolo de contexto de modelos (MCP).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual que describa tanto las actualizaciones de instrucciones como el nuevo helper de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si estás de acuerdo.
6. Cuando se haya creado la PR, pídele a Copilot que la combine y vuelva a `main` en local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si estás de acuerdo.

¡Has creado y combinado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo llega a Copilot el contexto del proyecto y has usado Copilot Chat en VS Code para:

- explorar cómo las instrucciones personalizadas del repositorio y las instrucciones aplicables a rutas concretas proporcionan contexto, directrices de programación y estándares de documentación.
- generar la primera parte de la capa de datos para los filtros con las instrucciones actuales.
- añadir un estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar la misma instrucción y observar cómo el código propuesto aplicaba el estándar nuevo.
- confirmar en `main` los cambios de instrucciones y la primera parte de la funcionalidad de filtros para que Copilot use las directrices actualizadas en el siguiente ejercicio.

A continuación, aplicarás estas instrucciones con el [modo agente][next-lesson] para añadir una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
