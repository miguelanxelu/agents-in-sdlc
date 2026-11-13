---
title: "Ejercicio 2 - Añade funciones con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. Como desarrollador, normalmente tendrás que localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, actualizarlo y repetir el proceso.

El modo agente de Copilot puede ayudar con este trabajo.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el entorno de desarrollo integrado (IDE). Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios y ejecuta tareas como las pruebas; si encuentra errores, puede corregirlos. En este ejercicio lo usarás para añadir una función al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como un compañero de programación y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- implementar funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generadas antes de combinarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editor. En el ejercicio anterior añadiste un método auxiliar para obtener editores. Ahora completarás con el modo agente el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer su funcionamiento actual.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo de software. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas de lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle, un mapeador objeto-relacional (ORM), en los metadatos de Astro; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un marco de interfaz del lado del cliente. Los métodos reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts Astro pequeños y específicos que usan la API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal dentro de Codespaces con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con la dirección web (URL), similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost del terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones indican qué opciones seleccionar.

La implementación inicial del sitio funciona, pero queremos añadir funciones. Primero revisarás el backlog. Al crear el proyecto a partir de la plantilla se generaron incidencias automáticamente; pide a Copilot que las muestre.

1. Selecciona **Agent** en la lista de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo sobre otro. Este taller se preparó con Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes y la experiencia, más predecible. Los modelos de lenguaje de gran tamaño son probabilísticos, por lo que las sugerencias pueden diferir de los ejemplos. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot podría usar otro comando MCP y aun así completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según el valor que considera que aportan a los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar contenido, dividir instrucciones complejas en partes manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios:

- añadir o perfeccionar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos con la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente puede encargarse de estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista de agentes de Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Envía este mensaje para pedir a Copilot que implemente la función descrita en la incidencia correspondiente del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes para la función, incluidos los métodos auxiliares de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Mientras trabaja, quizá aparezcan las opciones **Keep** y **Undo** en el editor. Al terminar, podrás aceptar o deshacer todos los cambios; no es necesario seleccionarlas mientras el agente trabaja.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que fallen algunas pruebas durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador con el sitio en ejecución y explora la nueva función.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has explorado cómo usar el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. En este ejercicio has aprendido a:

- explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- implementar funciones en la interfaz y la capa de datos con el modo agente.
- revisar los cambios y las pruebas generadas antes de combinarlos.

A continuación, [probarás la función con el servidor Playwright MCP][next-lesson] y crearás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo aprendido para pedir a Copilot que implemente esta función. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes y usa los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el mensaje las directrices necesarias para obtener la implementación que necesitas.
- puede que tengas que iterar con Copilot, solicitar cambios y proporcionar contexto; es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
