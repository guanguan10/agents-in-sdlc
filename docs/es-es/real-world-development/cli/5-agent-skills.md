---
title: "Lección 5: Personaliza y usa una aptitud quality-checks"
description: "Explora la aptitud quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has comprobado manualmente que el código funciona y has usado archivos de instrucciones para que siga los estándares del equipo. Pero ¿qué ocurre con las pruebas, lint y el resto de la integración continua (CI)?

Las **aptitudes de agente** son adecuadas para estas tareas. Ayudan a Copilot a saber cómo ejecutar operaciones repetibles.

En esta lección:

- explorarás la aptitud `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar y ejecutar la aptitud.

## Escenario

Tailspin Toys usa la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que sea más fácil de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En su forma más sencilla, una aptitud es una carpeta que contiene un archivo Markdown `SKILL.md`. Este incluye metadatos `name` y `description`, una descripción general y directrices sobre cuándo debe usarse. La carpeta también puede incluir scripts y otros recursos.

> [!NOTE]
> Una aptitud no necesita más carpetas ni archivos. `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos existentes del proyecto.

Las aptitudes pueden estar en `.github/skills` para que todo el equipo las comparta y reutilice, o en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explora la aptitud

Vamos a examinar la aptitud `quality-checks` que el equipo creó para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la aptitud.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y lint.
4. Fíjate en que todavía no incluye una sección **Results output formatting**.

## Ejecuta la aptitud antes de cambiarla

Puedes invocar aptitudes directamente desde Copilot CLI o pedirlo con lenguaje natural. Vamos a ejecutar las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este mensaje:

   ```plaintext
   Ejecuta la aptitud quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe que aparece al final.

## Personaliza el informe

Queremos un informe más claro que indique qué se ha ejecutado, si se ha superado y qué han comunicado las herramientas. Actualiza la aptitud para generarlo.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final:

   ```markdown
   ## Formato de salida de los resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o está bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo cuando la herramienta las comunique. Indica el siguiente paso ante cualquier fallo o bloqueo y nunca describas como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la aptitud actualizada

Copilot CLI puede volver a cargar las aptitudes modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje que antes:

   ```plaintext
   Ejecuta la aptitud quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una aptitud de agente existente. En esta lección:

- exploraste `quality-checks` para las pruebas unitarias, lint y las comprobaciones de tipos.
- personalizaste el formato de los resultados.
- volviste a cargar y ejecutaste la aptitud.

Este cambio se incluirá con el filtrado en la PR de la función. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
