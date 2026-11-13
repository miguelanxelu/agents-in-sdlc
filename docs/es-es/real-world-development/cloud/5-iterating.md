---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le pediste que documentara el código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad con controles para los modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, darás indicaciones a Copilot para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar inteligencia artificial (IA) generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión del resto del equipo, revisarás las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

El agente realiza tareas de forma asíncrona y sin supervisión, por lo que se aplican medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su código.
- El agente se ejecuta en GitHub Actions, en un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente: la documentación del código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre un aviso de que un firewall bloquea a GitHub Copilot. Es lo esperado: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o desactivar el firewall del agente en la nube][agent-firewall].

1. Abre el repositorio en github.com.
2. Selecciona **Pull Requests** para mostrar la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario: proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

El trabajo con Copilot no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otros integrantes del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados son no deterministas, no hay una única instrucción que debas usar. Por ejemplo, puedes pedirle que:

- añada encabezados de comentario que describan cada archivo de código.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Puedes elegir una de las ideas anteriores o proponer otra mejora.
2. Selecciona **View Session** para observar cómo Copilot inicia otra sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en las comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en el codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando y pedirle que ajuste, por ejemplo, el número de juegos que se muestran. También puedes pedir que añada encabezados de comentario y comentarios TSDoc; recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade una solicitud como comentario en la pestaña **Conversation** para que Copilot la vea e inicie otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - los controles para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de hojas de estilo en cascada (CSS) o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las aplicaciones de internet enriquecidas accesibles (ARIA) y la navegación con teclado.
   - el código JavaScript o TypeScript que controla el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en las comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en el codespace para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la preferencia se conserva al recargar la página.

El agente personalizado de accesibilidad ayudó a orientar a Copilot para implementar estas funciones siguiendo prácticas de accesibilidad. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa mientras trabaja. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar interés de patrocinadores en la página de detalles del juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los métodos auxiliares de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para implementar mejoras de accesibilidad, incluidos los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como Mission Control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de cambios del agente e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el itinerario del agente en la nube. Para seguir explorando, puedes probar los otros itinerarios:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Itinerario de interfaz de línea de comandos (CLI)](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades, un agente personalizado de control de calidad (QA), Playwright MCP, Agent Merge y el comando `/delegate` para volver a delegar en el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Recomendaciones para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configura el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
