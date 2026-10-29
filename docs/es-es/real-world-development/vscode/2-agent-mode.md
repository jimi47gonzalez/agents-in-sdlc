---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos suelen requerir modificar varios archivos y ejecutar tareas como las pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificar el código. La lista sigue creciendo.

Para eso sirve el modo de agente de Copilot.

El modo de agente puede trabajar de forma autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas detectados. Vamos a utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- dejar que el modo de agente de Copilot explore el proyecto, identifique archivos y coordine los cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generadas antes de integrarlos en el repositorio.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editor. En el ejercicio anterior añadiste un helper de editores; ahora completarás el resto del trabajo de datos, interfaz y pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web para conocer sus funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API estándar del DOM.

### Iniciar el sitio web

El proyecto incluye la tarea `npm run dev`, que inicia el servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
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

   Cuando se inicie el servidor Astro, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo con **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost desde el terminal se te redirigirá a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando se esté ejecutando, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, verás más información, como el título, la descripción, el editor y la categoría.

## Explorar la lista de trabajo pendiente con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo especifica cuando es importante.

El sitio funciona, pero queremos añadir capacidades. La plantilla creó automáticamente una lista de incidencias. Pide a GitHub Copilot que la muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar y aplicar cambios en distintos archivos, ejecutar comandos de terminal y utilizar herramientas.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo sobre otro. Utilizaron Claude Sonnet 4.5 al crear el taller para que las sugerencias fueran relativamente uniformes, pero los resultados pueden variar. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía este prompt para consultar las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado las incidencias según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa las instrucciones que proporcionarán contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Lee las directrices generales, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, pruebas y accesibilidad.
   - enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir instrucciones complejas en archivos más manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado hay que hacer al menos tres cambios:

- añadir o ajustar la lógica restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de listado de juegos con los controles de filtrado.

Las pruebas también deben superarse antes de integrar los cambios. El modo de agente puede realizar estas tareas.

1. Continúa la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Envía este prompt para implementar la funcionalidad descrita en la incidencia:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, editar archivos y ejecutar las pruebas. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá una opción **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el trabajo. Copilot alternará entre la generación de código y las pruebas hasta terminar y no detectar errores.

   ![Sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has utilizado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos.
- explorar el proyecto, identificar archivos y coordinar cambios.
- revisar los cambios y las pruebas antes de integrarlos en el repositorio.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- proporciona a Copilot el contexto y las indicaciones necesarios.
- es posible que tengas que iterar, pedir cambios y aportar más contexto. Así funciona el trabajo con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
