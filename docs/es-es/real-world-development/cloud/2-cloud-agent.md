---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen dificultades con la deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o funciones que siguen en el backlog por falta de tiempo. El agente en la nube de GitHub Copilot puede actualizar código y añadir funciones de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise una persona. Así se pueden delegar tareas laboriosas, acelerar el desarrollo y dedicar tiempo a cuestiones de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver algunos problemas de deuda técnica. La documentación que dejaron los contratistas que crearon la primera versión del sitio está incompleta; para empezar, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. En la página de detalles de cada juego, quiere mostrar otros títulos de la misma categoría para que la gente pueda seguir explorando. Por ahora basta con una implementación que sirva para probar y aceptar la experiencia de usuario. Es una tarea bloqueante, pero hay otras incidencias de mayor prioridad.

Estas tareas suelen perder prioridad y son buenas candidatas para el agente en la nube. Puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y revisar el resultado cuando esté listo.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después, Copilot crea una PR en borrador para seguir el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando termina. Cuando la solución esté lista para revisar, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no es magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden tener complejidades ocultas, así que [define con cuidado las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador en pareja con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Antes de asignarle trabajo al agente en la nube, revisa de forma no destructiva los archivos incluidos en el repositorio para observar su efecto más adelante.

Abre estos archivos en github.com o en el codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, sobre todo las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo`, que aplica las instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube, observa los comentarios TSDoc, las convenciones de TypeScript y los encabezados que aparecen en la PR. Proceden de estos archivos de instrucciones.

## Configura el entorno de desarrollo del agente en la nube

Al generar código, normalmente hace falta un entorno específico y ejecutar scripts de configuración. Lo mismo ocurre cuando Copilot realiza tareas de ingeniería de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con el flujo de trabajo [`copilot-setup-steps.yml`][setup-workflow], que se ejecuta antes de que empiece a trabajar y le proporciona las herramientas y dependencias necesarias. El repositorio de este laboratorio ya está configurado para instalar Node.js, las dependencias, los navegadores de pruebas de extremo a extremo y la base de datos SQLite migrada y poblada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio durante la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en la base SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos aspectos importantes:

- Incluye un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutarlo manualmente desde la pestaña **Actions** del repositorio y comprobar que funciona.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica reduce la productividad y dificulta el mantenimiento o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que vamos a asignarle esta tarea.

1. Abre tu repositorio en github.com en una pestaña nueva.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Establece **Title** en `El código carece de documentación`.
5. En **Description**, escribe:

   ```plaintext
   Nuestra organización exige que las funciones y métodos incluyan comentarios TSDoc cuando resulten útiles. Por desgracia, los últimos cambios no han seguido este estándar. Hay que actualizar el código existente para incluir comentarios que aclaren el comportamiento.
   ```

6. Selecciona **Create** para crear la incidencia.
7. A la derecha, selecciona **Assign to Copilot**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.
9. Abre la pestaña **Pull Requests** y abre la PR que Copilot haya creado. El título será parecido a `[WIP]: El código carece de documentación`. Si aún no aparece, espera un momento y actualiza la página.
10. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot prepara el entorno con el flujo de trabajo descrito, analiza el proyecto y decide cómo abordar la tarea.

11. Revisa la lista y las tareas que va a completar.
12. Desplázate por la cronología de la PR y observa cuándo Copilot empieza a trabajar en la incidencia.
13. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

14. Recorre la sesión en directo para observar cómo Copilot explora el código, analiza el estado, planifica y escribe código.

El proceso puede tardar varios minutos. Una de las ventajas del agente en la nube es poder trabajar de forma asíncrona. Mientras lo hace, asignarás otra tarea y trabajarás en funciones de la aplicación.

## Añade una sección de juegos relacionados

Mientras el agente en la nube trabaja en una tarea, puedes dedicarte a otra. Vamos a asignarle una sección de juegos relacionados para que el equipo de diseño pueda probarla.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Establece **Title** en `Muestra juegos relacionados en la página de detalles`.
5. En **Description**, escribe:

   ```markdown
   Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada título. El equipo de diseño quiere explorar la experiencia de usuario y hacer pruebas de aceptación. Estos son los requisitos:

   - Añade un asistente de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego dado, excluyendo ese juego.
   - Muestra una sección "Juegos relacionados" en la página de detalles que use el asistente.
   - Gestiona el caso en que un juego no tenga juegos relacionados.
   - Crea pruebas unitarias para el nuevo asistente.
   - Antes de crear la PR, confirma que se superan todas las pruebas.
   ```

6. Selecciona **Create** para crear la incidencia.
7. A la derecha, selecciona **Assign to Copilot** y, después, **Assign**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

Al poco tiempo verás un emoji 👀 en el primer comentario de la incidencia, señal de que Copilot está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente ya trabaja en la segunda tarea. No hace falta supervisarlo continuamente; podrás revisar el resultado cuando termine. Ahora crearás y usarás agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents], que permite asignarle incidencias para que las aborde de forma asíncrona. Puede ayudar a resolver deuda técnica, crear funciones o migrar código entre frameworks.

Has aprendido a:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja, crearás y usarás agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que ya has explorado.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
