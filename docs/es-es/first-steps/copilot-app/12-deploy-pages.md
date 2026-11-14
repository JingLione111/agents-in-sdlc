---
title: "Lección 12 - Publica el sitio con GitHub Pages"
description: "Usa la aplicación de GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de cambios y verificar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte en la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de cambios antes de combinarlos.
- seguirás el proceso de publicación y verificarás el sitio publicado.

## Situación

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio, que consta de un solo archivo, con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> La creación de una solicitud de cambios puede confirmar y publicar código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar esta instrucción. Si tienes activada la opción **Approve all**, no esperes que aparezca otra solicitud de confirmación.

En la sesión de la aplicación de Copilot para `space-quiz`, envía esta instrucción:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisa el flujo de trabajo y la solicitud de cambios

1. Confirma que el archivo README solo se ha añadido si el repositorio no tenía ya uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, los desencadenadores y los permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de cambios que aún no se haya combinado.
4. Pide las correcciones necesarias y revisa las diferencias actualizadas. Espera a que se completen correctamente las comprobaciones obligatorias de la solicitud antes de autorizar la combinación.

## Autoriza la combinación y la publicación

> [!WARNING]
> La siguiente instrucción autoriza al agente a combinar la solicitud de cambios revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíala solo después de terminar la revisión y confirmar el repositorio y la solicitud. La configuración de aprobaciones puede permitir estas acciones sin pedir otra confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía esta instrucción independiente:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una solicitud de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifica la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages ni combinar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación se haya completado correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado y has verificado la publicación en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
