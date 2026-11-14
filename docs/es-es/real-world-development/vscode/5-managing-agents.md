---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot está en modo agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición de archivos. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con instrucciones de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para que añada un control del modo claro además del modo de alto contraste.

## Situación

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un control para el modo claro, destinado a las personas a las que el tema predeterminado les resulta demasiado oscuro. En lugar de iniciar otra conversación, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía esta instrucción de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios existentes y propondrá lo necesario para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada conjunto de diferencias.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de alternancia de una forma que no te convence—, deténlo con el botón **Stop** junto al campo de chat y envía una instrucción para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía instrucciones de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa brevemente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que siguen las pautas de accesibilidad del agente personalizado: aplicaciones de internet enriquecidas accesibles (ARIA, por sus siglas en inglés), navegación con el teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara los cambios y crea un commit desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste cada edición cuando apareció, aceptaste o rechazaste cambios y enviaste instrucciones de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones de archivos concretas.
- orientaste la sesión en curso con instrucciones de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás las modificaciones del agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
