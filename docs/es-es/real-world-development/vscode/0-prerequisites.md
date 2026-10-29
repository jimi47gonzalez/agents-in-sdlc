---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys, pondrás en marcha un [Codespace][codespaces] y comprobarás que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del taller

Para crear una copia del repositorio que utilizarás para el código del taller, crearás una instancia a partir del [repositorio de plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** en la página del repositorio del taller y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si haces el taller como parte de un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante.

> [!NOTE]
> **La lista de incidencias está preparada**
>
> Al crear el repositorio a partir de la plantilla, se genera automáticamente una lista de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

## Crear un Codespace

Utilizarás un Codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad para distintos lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

    ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación puede tardar varios minutos, aunque sigue siendo más rápida que instalar todos los servicios manualmente. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está preparado para ejecutarse en un Codespace o en un [contenedor de desarrollo local][dev-containers]. Ambos incluyen los requisitos necesarios para que el taller funcione sin problemas. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza Codespaces.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar GitHub Copilot Chat y el modo de agente

Para utilizar el modo de agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Si utilizas un Codespace, ya debería estar instalada.

> [!TIP]
> Si no tienes la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista **Extensions** en Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, es posible que tengas que iniciar sesión en GitHub para habilitarla.

1. Vuelve a tu Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del Codespace.
3. Escribe «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Copilot Chat debería activarse.
4. Si aún no has iniciado sesión, se te pedirá que autentiques tu cuenta de GitHub. Sigue las instrucciones.

    ![Ejemplo de un mensaje para autenticar Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio del taller y empezado a crear el Codespace que utilizarás para desarrollar código.

## Siguiente paso

Empezarás a utilizar Copilot en el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde aprenderás a comunicarle las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
