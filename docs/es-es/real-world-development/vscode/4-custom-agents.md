---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents`, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimiento del dominio para orientar a Copilot. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con al menos `name` y `description`, seguidos de una instrucción en Markdown que define el comportamiento, la experiencia y las directrices del agente.

### Diferencias entre los agentes personalizados y las aptitudes de agente

Los agentes personalizados y las [aptitudes de agente][agent-skills-concept] tienen algunos puntos en común: ambos se suelen definir mediante archivos Markdown y explican a la IA cómo realizar tareas. La distinción más sencilla es que el **agente personalizado** es quien realiza el trabajo y las **aptitudes** son las herramientas que utiliza.

Los agentes personalizados tienen su propia ventana de contexto y coordinan aptitudes (e incluso otros agentes) para realizar el trabajo. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Para hacerlo, podría usar aptitudes como una que gestione solicitudes de incorporación de cambios o que ejecute pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier función de IA, prueba distintas opciones e itera hasta encontrar lo adecuado para tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolverlo, el equipo de diseño ha pedido un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante y queremos implementar esta función cuanto antes. Usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente de accesibilidad. Revisaremos su contenido para entender cómo orienta a Copilot.

Vuelve al codespace, abre un terminal y crea una rama nueva desde `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en los metadatos de los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

La vista Copilot Chat de VS Code muestra todos los agentes personalizados de `.github/agents` en el selector situado en la parte inferior. Puedes seleccionar uno para limitar la sesión de Chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; los pasos lo indicarán cuando sea necesario.

1. Si aún no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes de la parte inferior de Chat. De forma predeterminada, muestra el agente **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía el mensaje siguiente:

   ```plaintext
   Añade un modo de alto contraste al sitio. Debe haber un control que permita activarlo y desactivarlo, y la preferencia debe conservarse al recargar la página mediante el almacenamiento local del navegador.
   ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot hará cambios reales en el repositorio: puede editar componentes Astro, CSS y pruebas relacionadas.

Revisarás y orientarás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo usar uno en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se produzcan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la función con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
