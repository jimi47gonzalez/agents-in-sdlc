---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muchas organizaciones tengan deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de tareas pendientes. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, abre un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas repetitivas y centrarte en el trabajo de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para hacer pruebas de aceptación de la experiencia. No necesita una implementación refinada todavía; por ahora, esta tarea bloquea otras, aunque hay incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas con facilidad y son buenos ejemplos de trabajo que se puede asignar al agente en la nube. El agente las aborda de forma asíncrona, de modo que puedes ocuparte de otras tareas y volver después para revisar los cambios.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Puedes, por ejemplo, [asignar una incidencia de GitHub a Copilot][assign-issue]. Después, Copilot crea un borrador de PR para registrar el progreso, configura un entorno y empieza a trabajar. Puedes inspeccionar la sesión mientras se ejecuta o cuando haya terminado. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden volverse complejas al examinarlas con detalle. Ten en cuenta las [prácticas recomendadas para definir el alcance del trabajo del agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: divide el trabajo en etapas, aprende, experimenta y adapta el enfoque. Los principios del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste un taller práctico sobre instrucciones personalizadas, habrás visto cómo se utiliza `.github/copilot-instructions.md`. Si no, este es un buen momento para revisar los archivos de instrucciones del repositorio antes de asignar trabajo al agente en la nube.

Abre estos archivos desde la interfaz web de GitHub o desde un Codespace activo:

- `.github/copilot-instructions.md`: examina la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios que incluya la PR. Estas pautas proceden de los archivos de instrucciones. Volverás a examinarlas al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

Para crear código hay que configurar el entorno y ejecutar scripts de preparación. Lo mismo ocurre cuando se asignan tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que Copilot empiece. Así puedes instalar las herramientas y dependencias necesarias. El taller ya incluye la configuración para que Copilot tenga Node.js, las dependencias del proyecto, los navegadores de Playwright y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a tus pasos los permisos mínimos necesarios. Copilot recibirá su propio token para las operaciones.
    permissions:
      # Para clonar el repositorio durante la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas aquí, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación utiliza Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta incidencia es adecuada para el agente en la nube.

1. Abre tu repositorio en una pestaña nueva de github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que Copilot acaba de crear; su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está configurando el entorno mediante el flujo de trabajo descrito antes, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR y busca un mensaje que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza su estado, elige un plan y crea código.

La tarea puede tardar varios minutos. Una de las principales ventajas del agente en la nube es que puede trabajar de forma asíncrona mientras tú te ocupas de otras cosas. Aprovecharás esa función asignándole otra tarea y, después, añadirás una funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados

Puedes dividir el trabajo: mientras el agente aborda una tarea, tú puedes ocuparte de otra. Vamos a asignarle la solicitud del equipo de diseño para añadir juegos relacionados a la página de detalles.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después verás varios emoji 👀 en el primer comentario de la incidencia. Indican que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente ya está trabajando en la segunda solicitud. Como trabaja de forma parecida a un ingeniero de software, no hace falta que lo supervises continuamente: revisa el resultado cuando termine. Ahora puedes crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y se ocupe de deuda técnica, cree funcionalidades o ayude a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y puede acceder a instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
