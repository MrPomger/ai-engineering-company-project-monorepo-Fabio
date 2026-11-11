# Progreso

## Estado actual

La **infraestructura inicial de agentes está completada al 100%**. La empresa elegida es TrackFlow y el foco de producto seleccionado es un agente de CX para consultas de tracking y devoluciones, con atención propuesta en español e inglés y escalado a soporte humano.

Tareas de infraestructura completadas:

- [x] Configuración del briefing vigente de TrackFlow en `CONTEXT.es.md`. **Nota:** `CONTEXT.md` todavía conserva el placeholder de la plantilla; queda pendiente alinearlo si se requiere que ese archivo sea también la fuente del briefing.
- [x] Inicialización y alineación del Banco de Memoria en `memory-bank/` (`projectbrief.md`, `techContext.md` y `progress.md`).
- [x] Definición del protocolo global de agentes en `AGENTS.md`, con archivos requeridos, flujo pre-commit de cuatro pasos, archivos protegidos y mapeo de rutas.
- [x] Creación de `./.agents/rules/` y de la regla global `monorepo-structure-and-paths.md`.
- [x] Implementación de la skill reutilizable `verify-app-build-and-qa` en `./.agents/skills/verify-app-build-and-qa/SKILL.md`.

Las aplicaciones de producto aún no están creadas: `./uis/website/`, `./uis/backoffice/` y los módulos concretos bajo `./services/` se desarrollarán en próximos hitos. Las carpetas raíz `uis/` y `services/` contienen por ahora documentación, no aplicaciones ejecutables. Tampoco están implementados el agente de CX, la API integrada con transportistas, la base de conocimiento ni el workflow de tickets. La infraestructura de agentes y documentación no debe confundirse con esas futuras capacidades de producto ni con una integración real de TrackFlow.

## Validación de entrega

- El cambio pendiente es solo documental (`memory-bank/progress.md`); la validación del archivo no encontró errores.
- No se ejecutaron build, typecheck ni pruebas de aplicación: `./uis/website/`, `./uis/backoffice/` y los módulos de `./services/` aún no existen, y el repositorio no tiene un runner de workspace en la raíz.
- La rama `feature/agent-memory-bank` está publicada y sincronizada con `origin/feature/agent-memory-bank`; antes del commit, el único cambio pendiente es este archivo.

## Próximos pasos previstos

El flujo pre-commit de documentación ya fue ejecutado. Para cerrar y publicar la revisión de esta infraestructura:

1. Generar el commit semántico de los cambios previstos en la rama `feature/agent-memory-bank`.
2. Abrir el Pull Request desde `feature/agent-memory-bank` hacia `main`.

Después de ese hito, los siguientes trabajos de producto serán:

1. Confirmar el alcance mínimo del agente de CX: consultas de seguimiento y devoluciones, canales iniciales, datos necesarios, reglas de confianza y criterios de escalado.
2. Crear el esqueleto documentado del agente en `agents/` a partir de `agents/_template/`, incluyendo prompts de producto, herramientas permitidas y pruebas funcionales/regresión. Mantener la configuración de asistentes de desarrollo en `./.agents/`, separada del código de producto.
3. Crear los módulos de aplicación previstos en `./services/`, comenzando por el contrato API de consulta de tracking/pedido, políticas y creación o derivación de tickets. Seguir la recomendación de API FastAPI modular y usar datos simulados hasta disponer de integraciones autorizadas.
4. Preparar una base de conocimiento de políticas de envío y devoluciones y un conjunto de evaluación bilingüe con casos normales, ambiguos, datos faltantes, información obsoleta y solicitudes que requieren intervención humana.
5. Crear `./uis/website/` para la experiencia pública de destinatarios y `./uis/backoffice/` para revisión y gestión por agentes humanos. No mezclar interfaz con lógica de negocio del backend.
6. Documentar la ejecución local, configuración y límites; incorporar observabilidad, manejo de fallos y protección de datos antes de conectar datos reales o habilitar atención fuera de horario.
7. Medir calidad y operación (resolución automática, exactitud, escalado, latencia y satisfacción), validar la meta orientativa del 80% y ajustar antes de ampliar a más dominios.

## Criterio para declarar el siguiente hito

El siguiente hito de infraestructura estará listo cuando el flujo pre-commit se haya ejecutado, los cambios estén revisados en `feature/agent-memory-bank` y el Pull Request hacia `main` esté abierto.

El primer hito funcional de producto estará listo cuando un usuario pueda consultar un tracking de prueba en una aplicación creada bajo `./uis/website/`, recibir una respuesta sustentada en datos y políticas de un servicio en `./services/`, y obtener un escalado/ticket verificable para un caso que el agente no deba resolver. Debe estar cubierto por pruebas reproducibles en español e inglés y documentado sin depender de sistemas productivos aún no identificados.
