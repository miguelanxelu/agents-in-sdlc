---
title: "Ejercicio 5 - Supervisión y administración de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones a medida que ocurren. Puedes revisar cada diferencia, aceptar o rechazar cambios concretos y orientar la conversación con mensajes de seguimiento, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás la sesión para añadir un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, destinado a quienes consideran demasiado oscuro el tema predeterminado. En vez de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat junto con las opciones **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando proponga editar un archivo, aparecerán las diferencias:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Orienta al agente para añadir el modo claro

Una vez implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo: los componentes que modificó, el enfoque de estilos y el uso del almacenamiento local.

1. Escribe este mensaje de seguimiento en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente retomará el trabajo, revisará los cambios anteriores y propondrá modificaciones para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca. Usa **Keep** o **Undo** para aceptarlo o descartarlo.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, detenlo con el botón **Stop** junto al campo de mensaje y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes («simplifica el CSS y usa variables CSS en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los archivos modificados

Antes de confirmar el trabajo, revisa los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías encontrar componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y examina las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas accesibles (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado mensajes de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos.
- has orientado una sesión en curso con mensajes de seguimiento.

En [la lección siguiente][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios del agente local generados durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
