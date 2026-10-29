---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados, adaptados a tareas o ámbitos concretos del flujo de desarrollo. Se definen mediante archivos Markdown en `.github/agents` y proporcionan a Copilot instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que utilice patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` dentro del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen aspectos en común: ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad y podría utilizar habilidades para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la adecuada para cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Los comentarios de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro debido al bajo contraste entre el texto y el fondo. Para solucionarlo, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y abre un terminal. Cambia a una rama nueva desde `main` para mantener separada la tarea de accesibilidad de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que se deben evitar.

## Utilizar el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el selector de agentes situado en la parte inferior de Copilot Chat. Puedes seleccionar uno para aplicar sus instrucciones y herramientas a la sesión.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo especifica cuando es importante.

1. Si no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

    ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes de la parte inferior del chat. Muestra el agente activo; de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarlas.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes de Astro, CSS y pruebas relacionadas.

En el ejercicio siguiente revisarás y orientarás el trabajo en curso.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents], asistentes de GitHub Copilot especializados en tareas concretas. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
