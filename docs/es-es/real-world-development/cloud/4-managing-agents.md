---
title: "Ejercicio 4: Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se encargue de tres tareas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar las tareas.

Hay varias herramientas para gestionar las tareas que has asignado al agente, incluida la [página de agentes][agents-page] de GitHub.com. Este centro de control muestra las tareas que tienen solicitudes de incorporación de cambios (PR) abiertas. Puedes consultar el trabajo realizado e incluso orientar una sesión que siga en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de pedir al agente que creara el modo de alto contraste, el equipo decidió que también sería útil incluir un modo claro. Como ya se están actualizando los estilos y añadiendo controles, conviene ampliar el trabajo en curso para incluirlo.

## Revisa las tareas del agente en la nube

Vamos a consultar el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas del panel principal y del panel lateral. Deberían aparecer las tareas que asignaste, por ejemplo:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones de Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo y el enfoque del agente.

4. Observa también el panel de PR de la derecha. Permite ver la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión del modo de alto contraste. El título exacto puede variar según el nombre que haya elegido Copilot y el estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que termine la configuración y empiece a trabajar. Lo verás cuando aparezcan mensajes de actividad.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añade también un modo claro. Debe haber un control para este modo que permita a los usuarios elegir cómo quieren ver el sitio.
   ```

   ![Captura de pantalla de la tarea del agente en la nube, con el cuadro de diálogo para orientar la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud cuando termine el paso en el que estaba trabajando. El proceso tardará varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y cambiar el alcance de una sesión en curso.

Has aprendido a:

- supervisar las tareas desde Mission Control y la página de agentes.
- orientar una sesión en curso para solicitar funcionalidad adicional.

Cuando Copilot termine las funciones de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
