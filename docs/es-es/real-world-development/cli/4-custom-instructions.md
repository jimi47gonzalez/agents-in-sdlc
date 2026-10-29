---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga esas indicaciones disponibles. Los [archivos de instrucciones][instruction-files] describen no solo el código que quieres, sino también cómo debe estructurarse. Ahora que has desarrollado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para garantizar que se sigan los estándares de código.
- comprobarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no resulten evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su finalidad, los parámetros y los valores devueltos, incluido un argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos de `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Permiten orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones de código, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las interacciones con Copilot.
- `.github/instructions/*.instructions.md`: archivos que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Examina lo que hay antes de hacer cambios para comprobar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y lee la descripción del proyecto y las directrices de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y examina los archivos. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define los archivos a los que se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta sus enlaces a otros archivos de instrucciones y archivos del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos pequeños y reutilizables y señalar ejemplos para Copilot.

## Actualizar las instrucciones del equipo

Los archivos existentes son un buen punto de partida, pero aún falta algo. Modifica el archivo principal `copilot-instructions.md` para exigir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento como último punto:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, cierra y retoma la conversación de filtrado para que Copilot las lea sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- has actualizado el archivo de instrucciones para garantizar que se sigan los estándares de código.
- has comprobado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que el lint y las pruebas se ejecuten de forma uniforme.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
