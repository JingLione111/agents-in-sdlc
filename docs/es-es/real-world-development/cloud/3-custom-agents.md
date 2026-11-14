---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio que ayuden a Copilot a realizar ciertas tareas con más eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con al menos `name` y `description`, seguidos de una instrucción en Markdown que define el comportamiento, la experiencia y las directrices del agente.

### Comparación entre agentes personalizados y aptitudes de agente

Los agentes personalizados y las [aptitudes de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo y las **aptitudes** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar aptitudes e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; como parte del trabajo, podría utilizar aptitudes para gestionar solicitudes de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte al entorno y a las situaciones de tu equipo.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Explorarás los siguientes aspectos de los agentes personalizados:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Situación

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes indican que a algunos usuarios les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad cuanto antes. Para ello, usarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

El panel de control es el centro para gestionar los agentes del entorno. Desde allí puedes asignar tareas al agente en la nube de Copilot, supervisarlas y redirigirlas o proporcionar instrucciones adicionales. Asigna al agente de Copilot la tarea de crear el modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña de incidencias.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Add high contrast mode to website` en el campo **Title**.
6. Escribe este texto en el campo **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación del agente en la nube, con las opciones del agente personalizado y de accesibilidad resaltadas](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En esta lección:

- aprendiste cómo se definen los agentes personalizados.
- asignaste una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y orientar la sesión del agente][next-lesson] desde el panel de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
