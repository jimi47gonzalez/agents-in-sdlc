---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat de VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar cada diferencia, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás al agente durante la sesión para que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones propuestas. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, verás las diferencias en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará la solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la sesión actual, el agente conservará el contexto del trabajo previo: los componentes modificados, el enfoque de estilos y el uso de almacenamiento local.

1. Envía este prompt de seguimiento desde el cuadro de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde donde se quedó, explorará los cambios realizados y propondrá modificaciones para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca. Utiliza **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, deténlo con el botón **Stop** junto al cuadro de chat y envía un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que modificó el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes de Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y examina las diferencias. Comprueba que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos concretas.
- has orientado una sesión en curso con prompts de seguimiento.

En la [lección siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que haya producido el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
