---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información previa, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda tanto qué debe hacer como cómo debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de código y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos para determinadas rutas.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás mensajes anteriores y posteriores al cambio. El agente de GitHub Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las solicitudes de cambios que genere el agente en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su propósito.

Los archivos de instrucciones permiten proporcionar a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que pueda entender el estilo de código y los requisitos del proyecto. Puedes especificar convenciones, bibliotecas y el tipo de comentarios que debe incluir el código. Hay instrucciones para todo el repositorio y para determinados tipos de archivo o tareas.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción de lo que se está desarrollando, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan pautas para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Recomendaciones para administrar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices generales, como la descripción y estructura del proyecto y los estándares de código.
- Usa archivos `*.instructions.md` para tipos de archivo o tareas concretos, como pruebas unitarias, componentes Astro o la capa de datos.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de cómo debería ser el código y de qué debería evitarse.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar IA. La experimentación permitirá encontrar el enfoque adecuado para cada proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás instrucciones para tareas como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio de cada entorno indica cuándo corresponde usarla.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: el archivo principal `copilot-instructions.md` y los archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas sobre flujos de trabajo de GitHub Actions. Se aplican a las interacciones con Copilot en este repositorio.
3. Explora la carpeta `.github/instructions`. Incluye instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del principio. Define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta sus enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así se pueden dividir instrucciones amplias en archivos reutilizables más pequeños y enlazar ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código, pero todavía no exige documentación en el código. En los pasos siguientes añadirás directrices para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Añade un estándar para todo el repositorio

Añadirás una regla general para que el agente en la nube la use cuando trabaje en incidencias asignadas durante este itinerario. Antes de editar, crea una rama; el agente leerá las instrucciones de la rama de destino de la incidencia, pero así también confirmarás los cambios de forma ordenada.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abre `.github/copilot-instructions.md` y localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27.
3. Añade estas líneas debajo de los estándares existentes para exigir encabezados de comentario en los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto, las instrucciones generales pertenecen a `.github/copilot-instructions.md`; las instrucciones para lenguajes, tipos de archivo o tareas concretos pueden ir en archivos `*.instructions.md`. El archivo general es el lugar adecuado para estándares de todo el proyecto, como la regla de comentarios que acabas de añadir.

## Confirma, publica y combina la actualización

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`. Por tanto, tus cambios deben estar en `main` para que el agente pueda usarlos.

1. Prepara y confirma el archivo:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama refleja el flujo de trabajo habitual en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las solicitudes de cambios que genere el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Recomendaciones para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
