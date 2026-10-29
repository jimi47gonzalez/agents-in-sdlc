---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que pueda acceder a ella. Hay varias formas de compartir contexto con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para generar código.

En este ejercicio:

- explorarás cómo el contexto del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos para determinadas rutas.
- generarás la primera parte de la capa de datos del filtrado (un helper de editores) con las instrucciones actuales.
- añadirás un estándar general al repositorio en `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y la primera parte del filtrado en `main` para que Copilot utilice las directrices actualizadas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir siempre los estándares definidos. Copilot no es determinista. El objetivo es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para realizar el trabajo de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las interacciones con Copilot, como la pila tecnológica, una descripción general, las prácticas recomendadas y otras indicaciones globales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivos específicos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones en otros formatos, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Este taller no pretende enseñar todo sobre la creación de archivos de instrucciones, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en indicaciones generales del proyecto, como su descripción, estructura y estándares globales de código.
- Utiliza archivos `*.instructions.md` para tipos de archivos concretos —como pruebas unitarias, componentes de Astro o la capa de datos— o para tareas específicas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de código correctos e incorrectos.

No existe una única forma de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar la opción que mejor se adapte al proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos del proyecto, puede que veas instrucciones para distintos tipos de tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y el código de [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece opciones distintas; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller correspondiente lo indicará cuando sea pertinente.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y examínala. Contiene instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` del principio define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir instrucciones grandes en archivos pequeños y reutilizables y señalar ejemplos para que Copilot los siga. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para incluir comentarios TSDoc y encabezados de comentarios en los archivos.

## Crear una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utilizar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo especifica cuando es importante.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas de editor abiertas en ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe crear el helper.
3. En el selector de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto al helper como a sus pruebas.
6. Observa que el helper propuesto es una función tipada que acepta un cliente `db` como primer argumento y devuelve una lista tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que incluya comentarios incluso sin que se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto. Añade allí un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Describe los estándares del proyecto, pero aún no exige documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones generales en `.github/copilot-instructions.md` o instrucciones `*.instructions.md` para lenguajes, tipos de archivos y tareas concretas. El archivo general es adecuado para estándares de todo el proyecto, como la regla sobre los comentarios de documentación.

## Repetir el prompt y observar los cambios

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editores para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editores de la base de datos.
    */
   ```

5. Comprueba que la función propuesta incluye ahora un comentario TSDoc parecido al siguiente:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirmar las instrucciones y publicar la rama

Los archivos de instrucciones se gestionan con el mismo control de código fuente que los demás archivos del repositorio. Vamos a confirmar los cambios y publicar la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están presentes las instrucciones y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Después de publicar la rama, crea una solicitud de incorporación de cambios vinculada a la incidencia del backlog sobre la documentación. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub en tu nombre. La plantilla ya incluye las herramientas de GitHub en el entorno. La primera vez que Copilot utilice una, es posible que VS Code te pida que inicies sesión. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre las instrucciones y cree una PR desde la rama actual, indicando que incluye tanto las instrucciones como el helper inicial de editores:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Si Copilot solicita permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
6. Cuando se cree la PR, pídele que la combine y vuelva a `main` local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot solicita permiso para realizar acciones de GitHub o ejecutar comandos de shell, revisa cada comando y selecciona **Allow** según corresponda.

Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar lo que produce con las instrucciones existentes.
- añadir un estándar general al repositorio en `.github/copilot-instructions.md`.
- repetir el mismo prompt y observar cómo el código propuesto sigue el estándar nuevo.
- confirmar las instrucciones y el helper inicial de editores en `main` para que el ejercicio siguiente pueda utilizarlos.

A continuación, aplicarás las instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad nueva en varios archivos.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
