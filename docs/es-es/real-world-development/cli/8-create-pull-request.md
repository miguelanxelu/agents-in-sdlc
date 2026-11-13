---
title: "Lección 8 - Crea y combina la solicitud de la funcionalidad"
description: "Revisa juntos el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, crea una solicitud y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad modificada, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revísalos en conjunto y abre una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias solicitudes. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la solicitud de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el trabajo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar las tareas restantes de la solicitud, pero mantener el control del desarrollador sobre la autorización de combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Al activarlo, Copilot resuelve los impedimentos de la solicitud: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando es necesario. Después, activa la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la solicitud

Ya está creado el código. Revísalo, crea la solicitud y deja que Agent Merge gestione el resto.

1. Vuelve al codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud y la combinará cuando todo se haya completado correctamente.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y, cuando el repositorio lo permita, realiza manualmente la combinación después de revisarla.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación, pruebas y validación del código, y ahora también el proceso de solicitud de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la solicitud de filtrado.
- has activado Agent Merge después de revisar los cambios y has confirmado la combinación.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y el uso compartido, y para delegar trabajo opcionalmente en la nube.

## Recursos

- [Administra solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
