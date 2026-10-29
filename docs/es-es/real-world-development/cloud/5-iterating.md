---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

En este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidades: documentar el código, crear una sección de juegos relacionados y añadir funciones de accesibilidad con modos de alto contraste y claro. Vamos a revisar los cambios y, si hace falta, pedir a Copilot que los mejore.

### Escenario

Los principios del diseño de software y DevOps no cambian por incorporar IA generativa. Hay que revisar el código generado y seguir el proceso habitual. Revisaremos las sugerencias de Copilot para crear documentación, añadir juegos relacionados e implementar las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente trabaja en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- De forma predeterminada, el acceso a recursos externos está limitado, incluidos los servidores MCP.

## Revisar la documentación generada

Empezaremos con la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube: añadir documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre un aviso de que el cortafuegos bloqueó a Copilot. Es normal: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [configurar o desactivar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** o que sea más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos. Mientras tanto, puedes tomarte un descanso o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluye nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Mientras revisas los cambios, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios. Estos proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. Puede tardar varios minutos.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados son variables, no hay un prompt único. Por ejemplo, puedes pedirle que:

- añada un encabezado con una breve descripción a cada archivo de código.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Usa una de las ideas anteriores o propón otra mejora.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un nuevo commit en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para añadir juegos relacionados a la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay un indicador de flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Confirma que las comprobaciones del proyecto para la aplicación Astro se han completado correctamente. Puede tardar varios minutos.
9. **Opcional:** cambia a la rama de Copilot en el Codespace para probar manualmente los juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir pidiéndole cambios con comentarios en la pestaña **Conversation**. Por ejemplo, puedes ajustar la cantidad de juegos relacionados o solicitar los encabezados de comentarios y comentarios TSDoc que faltan. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad del agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el panel de agentes en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código.
5. Examina la implementación y presta especial atención a:
   - los componentes de alternancia de los modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que controla el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Confirma que las comprobaciones del proyecto para la aplicación Astro se han completado correctamente.
10. **Opcional:** cambia a la rama del Codespace para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la preferencia se conserva después de volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas. Si detectas problemas o mejoras posibles, puedes mencionarlo en un comentario de la PR, igual que con las solicitudes anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para delegar tareas reales de la lista de trabajo pendiente mientras te ocupas de otras cosas. Para practicar, crea más incidencias en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles de los juegos.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el taller del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones** del repositorio para ver cómo influyen en los resultados del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado las PR del agente e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y pasos siguientes

Has terminado el taller del agente en la nube. Si quieres seguir explorando, los otros talleres complementan lo que has practicado:

- 🖥️ **[Taller de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Taller de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
