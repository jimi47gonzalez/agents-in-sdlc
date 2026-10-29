---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan indicaciones para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo el contexto del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos para determinadas rutas.
- añadirás un estándar general al repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los talleres de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que su efecto es más difícil de mostrar en tiempo real. Verás cómo influyen tus instrucciones cuando revises las solicitudes de incorporación de cambios que genere en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información que necesita para realizar el trabajo de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivo y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las interacciones con Copilot.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivos específicos, como TypeScript, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto el archivo del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones en otros formatos, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Este taller no pretende enseñar todo sobre la creación de archivos de instrucciones, pero el proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en indicaciones generales del proyecto, como su descripción, estructura y estándares globales de código.
- Utiliza archivos `*.instructions.md` para tipos de archivos concretos, como pruebas unitarias, componentes de Astro o la capa de datos.
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

## Explorar los archivos de instrucciones personalizados del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas instrucciones se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y examínala. Contiene instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` del principio define un patrón que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así se pueden dividir instrucciones grandes en archivos pequeños y reutilizables, y señalar ejemplos para que Copilot los siga. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás una regla para incluir comentarios TSDoc y encabezados de comentarios en los archivos.

## Añadir un estándar para todo el repositorio

Ahora añadirás una regla general: la documentación del código debe escribirse con comentarios TSDoc e incluir un encabezado de comentario a nivel de archivo. El agente en la nube utilizará la regla cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así podrás confirmar los cambios de forma ordenada:

1. En el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`copilot-instructions.md` contiene información general del proyecto. Añade estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Describe los estándares del proyecto, pero aún no incluye documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en la raíz del repositorio (`.github/copilot-instructions.md`) para dar indicaciones generales, o crear archivos `*.instructions.md` para determinados lenguajes, tipos de archivos o tareas. El archivo general es adecuado para estándares de todo el proyecto, como la regla sobre los comentarios de documentación.

## Confirmar, publicar y combinar la actualización

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot, que partirá de `main`. Por eso, los cambios de instrucciones deben llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` hacia `main` y combínala. El agente en la nube podrá leer las instrucciones cuando trabaje en las incidencias del ejercicio siguiente.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar el cambio directamente en `main`. El paso de la rama permite practicar un flujo de trabajo más parecido al de un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del taller.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
