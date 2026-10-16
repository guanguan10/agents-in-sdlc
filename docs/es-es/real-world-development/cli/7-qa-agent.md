---
title: "Lección 7: Crea y usa un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la aptitud quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la aptitud `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP.
- crearás e inspeccionarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus resultados según la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar este proceso y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe su propósito, sus instrucciones y las herramientas disponibles. En este taller crearás el rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado cumplen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo; la aptitud `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil de control de calidad indica a Copilot cómo usar esas capacidades para evaluar los requisitos y presentar resultados. El perfil no sustituye a las demás personalizaciones ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la función, pide a Copilot que cree un perfil reutilizable. El perfil definirá las comprobaciones que realizará y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funciones según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la aptitud quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas cuando falte cobertura.

   Pídele que indique para cada requisito si se ha superado, ha fallado o está bloqueado, y que aporte pruebas. Debe pedir permiso antes de cambiar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, limítate a crear el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha recogido el proceso y los límites previstos. Así evitarás que un perfil incompleto o demasiado amplio cambie la función cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible claro.
3. Lee las instrucciones del perfil y confirma que el agente parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la aptitud `quality-checks` y usa Playwright MCP.
4. Confirma que presenta pruebas que respaldan sus resultados, pregunta antes de cambiar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si al perfil le falta alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de calidad para la incidencia

Copilot CLI carga los agentes del proyecto cuando se inicia una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona el agente de control de calidad para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista. Aun así, el comando anterior lo activará.

2. Envía este mensaje para pedirle que revise la función:

   ```plaintext
   Revisa la función de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una solicitud de incorporación de cambios?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP.
- creaste e inspeccionaste un perfil reutilizable de control de calidad.
- seleccionaste el agente y revisaste sus conclusiones según la incidencia.

Ya tienes la implementación, la actualización de la aptitud, el perfil de control de calidad, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de la función y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
