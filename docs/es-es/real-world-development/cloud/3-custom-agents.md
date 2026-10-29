---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados que puedes adaptar a tareas o ámbitos concretos de tu flujo de desarrollo. Se definen mediante archivos Markdown en la carpeta `.github/agents` del repositorio. Estos perfiles proporcionan instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para ayudar a Copilot a realizar determinadas tareas. Los equipos pueden convertir sus conocimientos en agentes reutilizables; por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones uniformes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` dentro del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son las herramientas.

Los agentes personalizados tienen una ventana de contexto propia y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad y podría utilizar habilidades como una que gestiona solicitudes de incorporación de cambios o ejecuta pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la adecuada para cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen.
- cómo se asigna una tarea a uno de ellos.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. Para solucionar el problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que utilizarás un agente personalizado para generar la funcionalidad cuanto antes.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Examina el perfil para saber cómo orientará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que se deben evitar.

## Crear y asignar una incidencia

El panel de agentes es el centro de control desde el que puedes asignar tareas al agente en la nube de Copilot, supervisarlas y orientar las sesiones. Vamos a asignarle una tarea para crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. En la lista de agentes personalizados, selecciona **Accessibility agent**.

   ![Cuadro de asignación del agente en la nube con el agente personalizado Accessibility resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y pasos siguientes

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a uno de ellos.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el panel de agentes.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para utilizar agentes personalizados][org-custom-agents]
- [Preparar la empresa para utilizar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
