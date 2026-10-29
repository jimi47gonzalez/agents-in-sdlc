---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas para mejorar la experiencia y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante que puedas supervisar las tareas.

Puedes gestionar las tareas asignadas al agente en la nube desde varias herramientas, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen PR abiertas, examinar lo que han hecho y orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de añadir un modo de alto contraste, el equipo decide que también sería buen momento para añadir un modo claro. Como ya se está trabajando en los estilos y en la funcionalidad de alternancia, quieres orientar al agente para que implemente ambos modos.

## Revisar las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente en la nube de Copilot:

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina la lista de tareas en los paneles principal e izquierdo. Deberían aparecer, entre otras, las tareas que asignaste a Copilot:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea activa y revisa las operaciones que ha realizado Copilot, como:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de PR de la derecha. Desde allí puedes consultar la solicitud y los archivos modificados.

## Orientar al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada una alternancia de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado de la tarea.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que termine la configuración y empiece a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los del ejemplo.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo recibe y lo incorpora al trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud a su tarea cuando termine el paso que estaba ejecutando al recibir el mensaje.

El trabajo tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- utilizar el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
