---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es esencial cuando trabajas con IA generativa. Si una tarea debe hacerse de una manera concreta o Copilot necesita información previa, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] permiten explicar no solo *qué* quieres, sino también *cómo* quieres que se haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos asociados a rutas.
- añadirás un nuevo estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás el resultado de un mensaje anterior y otro posterior. El agente en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Lo observarás más adelante al revisar las solicitudes de incorporación de cambios que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su propósito.

Usarás archivos de instrucciones para que Copilot disponga de la información necesaria y realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que comprenda mejor el estilo y los requisitos del código. Puedes especificar convenciones, bibliotecas y tipos de comentarios, así como crear instrucciones para todo el repositorio o para determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general, como la pila tecnológica, la estructura del proyecto, las prácticas recomendadas y otras directrices.
- `.github/instructions/*.instructions.md`, archivos que proporcionan directrices para lenguajes, tipos de archivos o tareas concretas.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni sugerir ediciones siguientes.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan metadatos `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto, como su descripción, estructura y estándares globales.
- Usa archivos `*.instructions.md` para tipos de archivos (pruebas unitarias, componentes Astro o capa de datos) y tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de lo que se debe y no se debe hacer.

No hay una única forma de crear archivos de instrucciones o de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. Encontrarás instrucciones para tareas como actualizar la [interfaz de usuario][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar instrucciones. Cada entorno lo ofrece de una manera distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora las instrucciones personalizadas del proyecto

Lee los archivos de instrucciones del repositorio en la interfaz web de GitHub o en tu codespace.

1. Abre `.github/copilot-instructions.md`.
2. Observa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, fíjate en las directrices de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Explora los archivos de `.github/instructions`, que incluyen instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Su campo `applyTo` define el patrón glob relativo a la raíz del repositorio que indica a qué archivos se aplican las instrucciones; por ejemplo, `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos y ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> **Code formatting requirements** en `copilot-instructions.md` describe los estándares de programación, pero aún no exige documentar el código. Añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Añade un estándar general al repositorio

Esta es la única edición que harás en este ejercicio: añadir una regla general para incluir comentarios TSDoc y un encabezado explicativo en cada archivo. El agente en la nube podrá usarla en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia; aun así, confirmarás los cambios de forma ordenada.

1. En el terminal del codespace, crea y selecciona una rama:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca **Code formatting requirements**, cerca de la línea 27. Observa que aún no exige documentación del código.
4. Añade estas líneas de Markdown después de los estándares existentes:

   ```markdown
   - Cada función exportada debe tener un comentario TSDoc que describa su propósito, sus parámetros y el valor que devuelve.
   - Antes de las importaciones o del código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

5. Guarda el archivo.

> [!TIP]
> Puedes definir instrucciones para todo el repositorio en `.github/copilot-instructions.md` o instrucciones para tipos de archivos y tareas concretas en archivos `*.instructions.md`. El archivo general es el lugar apropiado para reglas del proyecto, como la documentación del código.

## Confirma, publica y combina la actualización

El agente en la nube lee las instrucciones de la rama de destino de la incidencia. En el siguiente ejercicio, la incidencia se asignará desde `main`, así que los cambios deben llegar a `main` para que el agente los use.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Añade estándares de comentarios y encabezados a las instrucciones de Copilot"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. El agente en la nube podrá leer estas instrucciones en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar el cambio en `main`. Se incluye el paso de la rama para reflejar el trabajo habitual en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás su efecto en las solicitudes de incorporación de cambios que genere el agente en la nube en los siguientes módulos.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
