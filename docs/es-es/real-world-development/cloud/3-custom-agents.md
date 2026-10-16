---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes especializados para tareas o ámbitos concretos del desarrollo. Se definen en archivos Markdown de `.github/agents` y pueden incluir instrucciones, prácticas recomendadas, patrones de código y conocimiento del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables, como uno que compruebe la accesibilidad según [WCAG][wcag], uno de seguridad o uno de pruebas.

También se pueden definir agentes de forma global en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con `name` y `description`, seguidos de instrucciones Markdown que describen el comportamiento, la experiencia y las directrices del agente.

### Agentes personalizados y aptitudes de agente

Los agentes personalizados y las [aptitudes de agente][agent-skills-concept] tienen puntos en común: ambos se suelen definir con Markdown y explican a la IA cómo realizar tareas. La distinción más sencilla es que el **agente personalizado** realiza el trabajo y las **aptitudes** son herramientas que puede utilizar.

Los agentes tienen una ventana de contexto propia y pueden coordinar aptitudes y otros agentes. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; como parte del proceso, podría usar aptitudes para gestionar una PR o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera hasta encontrar el enfoque adecuado para tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás:

- cómo se definen los agentes personalizados.
- cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que les cuesta leer el tema oscuro actual por el contraste insuficiente entre texto y fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para generar esta función.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente de accesibilidad. Revisa el archivo para entender cómo orienta a Copilot.

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios en los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades al generar código para sitios accesibles.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

Mission Control es el lugar central para trabajar con agentes: permite asignar tareas al agente en la nube de Copilot, supervisarlas y proporcionar directrices adicionales. Asignaremos al agente la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio y abre **Issues**.
2. Selecciona **New issue** y, después, **Blank issue**.
3. Establece **Title** en `Añadir un modo de alto contraste al sitio web`.
4. En **Description**, escribe:

   ```plaintext
   Necesitamos un modo de alto contraste para el sitio. Debe haber un control que permita activarlo y desactivarlo, y la preferencia debe guardarse en el almacenamiento local del navegador.
   ```

5. Selecciona **Create** para crear la incidencia.
6. A la derecha, selecciona **Assign to Copilot**.
7. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Captura de pantalla de la asignación del agente en la nube, con Accessibility agent resaltado](../../../_images/ex5-select-custom-agent.png)

8. Selecciona **Assign**. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot y has asignado una tarea a un agente especializado. Los agentes permiten convertir la experiencia y los estándares del equipo en asistentes reutilizables.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo asignarles una tarea.

Mientras Copilot implementa el modo de alto contraste, [supervisarás y orientarás la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
