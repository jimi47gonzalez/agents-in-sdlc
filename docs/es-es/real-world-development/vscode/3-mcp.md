---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo sus servidores amplían las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una PR con los cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede utilizar herramientas fuera del editor. Model Context Protocol (MCP) es una forma estándar para que Copilot se comunique con herramientas y servicios externos.

![Diagrama del modo de agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente y gestiona la comunicación con servicios externos, como API existentes o herramientas locales instaladas mediante paquetes de npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador mediante Playwright. El agente puede navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla del proyecto incluye la entrada `github`, por lo que Copilot pudo leer la lista de incidencias en el ejercicio anterior. Utiliza el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hace falta instalar nada en local. VS Code se conecta por HTTP y, la primera vez que Copilot usa una de sus herramientas, te autenticas en GitHub.

## Añadir el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en un Codespace, que no tiene escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot solicita sus herramientas, VS Code inicia el servidor y pide confirmar que confías en él.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa su configuración y autorízalo para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá autorización la primera vez que Copilot necesite sus herramientas.

## Probar el filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de navegar manualmente por la aplicación, el agente puede abrir una página, aplicar filtros, leer los resultados e informar de lo que ha observado. Así puedes comprobar rápidamente el comportamiento sin salir de la conversación.

El servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente analiza elementos estructurados y etiquetados —como botones, enlaces y elementos de listas— de forma parecida a las tecnologías de asistencia. Por eso, una comprobación funcional también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá cada paso y comunicará los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más detalles o solicita que Copilot corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar ejecutándose en `http://localhost:4321` para realizar esta prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice Playwright MCP, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ahora que has comprobado que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisar los cambios.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Utiliza el botón **Sparkle** para generar un mensaje de commit.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una PR, incluida la interfaz de github.com y GitHub CLI. En este caso, pide a GitHub Copilot que encuentre la incidencia correspondiente y vincule la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Confirma que está seleccionado **Agent** para que Copilot pueda utilizar las herramientas de GitHub.
3. Envía este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, es posible que tengas que iniciar sesión.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace de Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y pasos siguientes

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo sus servidores amplían las herramientas de Copilot.
- has añadido el servidor Playwright MCP al espacio de trabajo.
- lo has utilizado para probar manualmente el filtrado en el navegador antes de publicar los cambios.
- has publicado la rama y abierto una PR.

Has utilizado MCP para *probar* una funcionalidad, pero es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
