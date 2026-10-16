---
title: "Ejercicio 5: Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando activas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu máquina y en tiempo real, Copilot Chat de VS Code muestra todas las llamadas a herramientas y las ediciones a medida que se producen. Puedes revisar cada diferencia, aceptar o rechazar cambios y orientar la conversación con mensajes de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás al agente durante la sesión para que añada un control de modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, destinado a quienes encuentran demasiado oscuro el tema predeterminado. En lugar de iniciar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat junto a la diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas, lectura y edición de archivos y comando de terminal que pide ejecutar.
3. Cuando proponga una edición, verás la diferencia en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para revisar todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta pide permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Autoriza solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine el modo de alto contraste. Verás un mensaje final con un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que está listo el modo de alto contraste, ampliarás la conversación para añadir un control de modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior: los componentes modificados, el enfoque de estilos y el uso del almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

   ```plaintext
   ¡Buen trabajo! Ahora añade un control de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservarse en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el alto contraste de forma independiente.
   ```

2. El agente retomará el trabajo, explorará los cambios anteriores y propondrá las modificaciones necesarias para el modo claro.
3. Revisa cada edición cuando aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, si intenta reorganizar la interfaz de los controles—, detenlo con el botón **Stop** junto al cuadro de mensaje y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes («simplifica el CSS; usa variables en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos cambiados. Deberías ver componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos de los archivos y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste las ediciones, aceptaste o rechazaste cambios y enviaste mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección aprendiste a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar o rechazar ediciones individuales.
- orientar una sesión en curso con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
