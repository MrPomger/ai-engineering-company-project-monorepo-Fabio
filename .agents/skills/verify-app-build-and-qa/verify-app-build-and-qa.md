---
name: verify-app-build-and-qa
description: Skill reutilizable para comprobar compilado, tipado TypeScript y estado de salud de cualquier aplicacion en ./uis o ./services antes de cada commit.
---

# Verify App Build and QA

## Objetivo

Estandarizar la comprobación local de compilación, tipado y salud de un módulo de aplicación del monorepo antes de dar por completado un cambio o preparar un commit. Esta skill cubre `uis/website`, `uis/backoffice` y los módulos correspondientes dentro de `services/`.

## Inputs

- **Ruta objetivo:** subdirectorio del módulo modificado, relativo a la raíz del monorepo (por ejemplo, `uis/website` o `services/api`).
- **Scripts a ejecutar:** nombres de los scripts de `package.json` disponibles en ese módulo para build, typecheck, lint y test, según corresponda.
- **Cambio bajo verificación:** resumen de los archivos o comportamiento que se modificaron.

Si el módulo no tiene `package.json`, consulta su README y configuración local para identificar los comandos equivalentes. No inventes scripts ni presupongas que todos los módulos usan npm o TypeScript.

## Procedimiento

1. Confirma la ruta objetivo y lee el README del módulo para identificar sus requisitos y comandos.
2. Desde la raíz, cambia explícitamente al subdirectorio del módulo antes de ejecutar cada comando de npm o script de build/test. Ejemplo: `cd uis/website && npm run build`.
3. Ejecuta los scripts disponibles y pertinentes: compilación, typecheck TypeScript, lint y pruebas. Registra el comando y su código de salida.
4. Comprueba el estado de salud que corresponda al módulo. Si hay una aplicación ejecutable, sigue sus instrucciones locales de health check; no declares una comprobación de runtime si no se pudo iniciar o no existe ese mecanismo.
5. Actualiza `memory-bank/progress.md` con los cambios revisados, validaciones ejecutadas, resultados y bloqueos pendientes.

## Criterios de aceptación

- El proceso se ejecutó desde el subdirectorio del módulo objetivo, no desde la raíz.
- La compilación terminó con código de salida `0`.
- Si existe tipado TypeScript aplicable, su comprobación terminó correctamente; si no aplica o no existe, indícalo expresamente.
- Las pruebas, lint y health checks disponibles se ejecutaron y sus resultados quedaron reportados; cualquier verificación no aplicable o no disponible queda identificada con su motivo.
- `memory-bank/progress.md` refleja el estado real de la validación.
- Si la compilación, el tipado o una comprobación requerida falla, informa el error, no declares el cambio listo y no permitas avanzar al commit hasta corregirlo y repetir la validación. Si una validación requerida no puede ejecutarse, bloquea la aceptación hasta resolver el bloqueo o recibir una decisión explícita del desarrollador.