---
title: "Ejercicio 2: Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Normalmente, un desarrollador localiza los archivos necesarios, realiza los cambios, ejecuta las pruebas, depura los problemas, descubre qué archivo falta y vuelve a cambiar el código. Y así sucesivamente.

El modo de agente de Copilot ayuda a automatizar este proceso.

El modo de agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- implementar funciones en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos en el código base.

## Escenario

El catálogo de juegos está creciendo y quieres permitir que quienes lo visitan filtren por categoría y editorial. En el ejercicio anterior añadiste un asistente para editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio de Tailspin Toys para entender sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo de software. Permite mostrar los juegos y sus detalles. Es una aplicación Astro única que genera HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde sus metadatos con Drizzle ORM; no hay una API independiente ni un framework de interfaz en el cliente. Los asistentes de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama nueva para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje parecido al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace local desde el terminal se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado hacia el servidor que se ejecuta en el codespace.

### Explora el sitio

Cuando el sitio esté en funcionamiento, explora sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, verás más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; los pasos lo indicarán cuando sea necesario.

La implementación inicial funciona, pero queremos ampliarla. Empieza por revisar el backlog de incidencias que se creó automáticamente al configurar el repositorio a partir de la plantilla.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. Este agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Se usó Claude Sonnet 4.5 al crear el taller para que las sugerencias fueran relativamente coherentes; aun así, los modelos de lenguaje son probabilísticos, así que los resultados pueden ser distintos a los ejemplos. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede usar un comando MCP diferente y, aun así, completar la tarea.

3. Envía este mensaje para pedir a Copilot que muestre las incidencias del backlog:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista. Copilot también las habrá priorizado según su utilidad para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo que proporciona contexto a Copilot: [ui.instructions.md](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md). Incluye información sobre cómo añadir funciones al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividirlas en partes más pequeñas y proporcionar ejemplos.

## Implementa el filtrado

Para completar el filtrado tendrás que hacer al menos tres cambios:

- añadir o mejorar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

También tendrás que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de la interfaz de usuario.](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes.
4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Envía este mensaje para implementar los requisitos de la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para que permita filtrar por editorial y categoría según los requisitos de la incidencia correspondiente del backlog. Ya existe un asistente de editoriales del ejercicio anterior, así que conserva y mejora ese trabajo cuando sea necesario mientras completas la capa de datos, la interfaz de usuario y las pruebas restantes. Confirma que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función, incluidos los asistentes de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de pantalla de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos según la tarea y el código base. Durante el trabajo, el editor puede mostrar los botones **Keep** y **Undo**; cuando termine, habrá una opción para aceptar o deshacer cada cambio.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.
8. Puede que haya pausas o que fallen algunas pruebas durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.
9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has usado el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz de usuario y la capa de datos.
- explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas antes de combinarlos en el código base.

A continuación, [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: Implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación y si quieres permitir que el usuario elija el tamaño de página.
- incluye en el mensaje las directrices necesarias para obtener el resultado que quieres.
- puede que tengas que iterar, pedir cambios y proporcionar más contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
