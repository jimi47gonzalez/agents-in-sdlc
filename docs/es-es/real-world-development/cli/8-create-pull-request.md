---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- examinarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve lo que bloquea la PR —corrige comprobaciones de integración continua (CI) con errores, responde a comentarios de revisión y reorganiza la base cuando hace falta— y, después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad. Revisa el trabajo y confirma que está listo antes de activarlo.

## Utilizar Agent Merge para gestionar la PR

Cuando hayas creado todo el código, revísalo, crea la PR y deja que Agent Merge se ocupe del resto del proceso.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando todas las comprobaciones se superen, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación, las pruebas y la validación del código, y ahora también el proceso de la solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has examinado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
