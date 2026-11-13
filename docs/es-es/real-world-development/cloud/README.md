---
slug: es-es/real-world-development/cloud
title: "Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[El agente de GitHub Copilot en la nube][cloud-agent]** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asigna tareas en GitHub y el agente las realiza en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios mientras tú te ocupas de otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá el agente en la nube. Después, le asignarás una incidencia para que implemente el trabajo. Revisarás y usarás agentes personalizados para orientar su enfoque, supervisarás y dirigirás las sesiones desde el panel de agentes y, por último, revisarás sus solicitudes e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigna incidencias al agente de GitHub Copilot en la nube |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Administración de agentes][ex4] | Supervisión | Supervisa y dirige las sesiones del agente |
| [5. Iteración][ex5] | Revisión | Revisa solicitudes, itera sobre el trabajo de Copilot y decide los siguientes pasos |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] El agente de GitHub Copilot en la nube habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller, por lo que puedes completar todos los itinerarios con él.

> [!NOTE]
> El agente en la nube está disponible en los planes **Copilot Student, Pro, Pro+, Business y Enterprise**, es decir, en todos salvo Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este itinerario no trata MCP. Para aprender a usar servidores MCP con Copilot, consulta el [itinerario de CLI][cli] o el [itinerario de VS Code][vscode].

## Empieza

**[Empieza por los requisitos previos][ex0]**

[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[education]: https://github.com/education/students
[cli]: ../cli/
[vscode]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
