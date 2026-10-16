---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has pedido a GitHub Copilot que resuelva varias tareas para mejorar la experiencia de usuario y añadir funcionalidad: documentar el código, crear una sección de juegos relacionados y añadir funciones de accesibilidad con controles de alto contraste y modo claro. Ahora revisarás los cambios y, si es necesario, enviarás comentarios para que Copilot los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso DevOps habitual. Vamos a revisar las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

El agente en la nube realiza tareas de forma asíncrona y sin supervisión, por lo que se han establecido límites de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- El agente se ejecuta en GitHub Actions, en un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- De forma predeterminada, el acceso a recursos externos, incluidos los servidores MCP, está limitado mediante un [firewall][agent-firewall].

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) del agente: la documentación del código. La revisarás con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que veas una advertencia de que el firewall bloquea a GitHub Copilot. Es **normal**: el acceso a recursos externos, incluidas las llamadas a servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para ver las solicitudes abiertas.
3. Abre la PR cuyo título sea parecido a **Añadir la documentación que falta**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR llevará la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código y la documentación nuevos. El resultado puede variar. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios, que proceden de los archivos de instrucciones personalizados revisados al principio del ejercicio 2.
6. Cuando termines, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro finalizarán correctamente. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de una PR, como harías con otros miembros del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para responder. Como los resultados no son deterministas, no hay un texto único que debas usar. Por ejemplo, puedes pedirle:

- que añada un breve encabezado explicativo al principio de cada archivo de código.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario sobre la documentación generada y menciona a **@copilot**. Usa una de las ideas anteriores u otra mejora que quieras solicitar.
2. Selecciona **View Session** para observar el nuevo trabajo de Copilot.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Sigue iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Ahora revisa la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada **Show related games on the game details page** o algo parecido.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando termines, vuelve a **Conversation** y desplázate hacia abajo.
6. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Revisa las comprobaciones de la PR. Si todo va bien, las comprobaciones de la aplicación Astro finalizarán correctamente.
8. **Opcional:** cambia a la rama del agente en tu Codespace para probar manualmente la función. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ya ha creado la función de juegos relacionados. Puedes seguir iterando mediante comentarios en **Conversation**, por ejemplo, para cambiar cuántos juegos se muestran o solicitar encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad creadas por el agente personalizado. Esta PR debería incluir el modo de alto contraste del ejercicio 3 y el modo claro solicitado desde Mission Control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada **Añadir un modo de alto contraste al sitio web** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la PR llevará la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código.
5. Presta atención a:
   - los controles de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona los cambios de modo.

6. Cuando termines, vuelve a **Conversation** y desplázate hacia abajo.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Revisa las comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto finalizarán correctamente.
9. **Opcional:** cambia a la rama del agente en tu Codespace para probar manualmente las funciones de accesibilidad:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba los controles de alto contraste y modo claro. Confirma que funcionan y que las preferencias se conservan al recargar la página.

El agente personalizado de accesibilidad habrá orientado a Copilot según las prácticas recomendadas. Si detectas problemas de accesibilidad, menciona a **@copilot** en un comentario para solicitar cambios.

## Ejercicio opcional: Sigue delegando

El agente en la nube resulta útil cuando puedes asignarle elementos reales del backlog y ocuparte de otra cosa. Para practicar, crea más incidencias en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés de patrocinadores en la página de detalles.
- implementar la paginación de la lista de juegos.
- añadir validación de entrada y control de errores a los asistentes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizadas** del repositorio para observar su efecto en el agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir controles de alto contraste y modo claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión.
- **Revisaste e iteraste sobre las solicitudes del agente**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el recorrido del agente en la nube. Para seguir explorando, prueba los otros entornos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora Copilot Chat, el modo de agente y la integración con MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan, Autopilot, aptitudes, un agente de control de calidad, Playwright MCP, Agent Merge y la opción `/delegate` para volver al agente en la nube.

En tu repositorio, también puedes probar estas ideas:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

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
