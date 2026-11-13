---
title: "Ejercicio 4 - Supervisión y administración de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente de GitHub Copilot en la nube tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente, incluida la [página de agentes][agents-page] de GitHub.com. Desde Mission Control puedes consultar las tareas con solicitudes de incorporación de cambios abiertas, revisar las operaciones realizadas y orientar una sesión que sigue en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar una función adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de cambio de modo, quieres orientar el trabajo del agente para que incluya ambos modos.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente de GitHub Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel lateral. Deberías encontrar las tareas que asignaste, entre ellas:
   - actualizar la documentación del código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - descargar el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores Model Context Protocol (MCP).
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados.

## Dirige el trabajo del agente en la nube

Ahora que has visto las tareas activas, pedirás a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que terminó la configuración y empezó a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud después de terminar el paso que estaba realizando cuando enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y dirigir el trabajo del agente de GitHub Copilot en la nube. Puedes hacer seguimiento de tareas activas y completadas, revisar el trabajo y cambiar el alcance o proporcionar instrucciones adicionales durante una sesión.

Has explorado estos aspectos:

- has usado Mission Control y la página de agentes para supervisar tareas.
- has redirigido una sesión en curso para solicitar una función adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que creó][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
