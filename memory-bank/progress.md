# Progreso

## Estado actual

El proyecto está en la **fase inicial de infraestructura de agentes**. La empresa elegida es TrackFlow y el foco de producto seleccionado es un agente de CX para consultas de tracking y devoluciones, con atención propuesta en español e inglés y escalado a soporte humano.

El repositorio conserva principalmente la estructura documental de una plantilla: existen README de carpetas y una plantilla de agente, además de metadata para `@repo/shared-types`. No hay una aplicación `uis/website`, `uis/backoffice` o `services/` ejecutable, ni agente de CX implementado, API integrada con transportistas, base de conocimiento, workflow de tickets, entorno Docker Compose o runner de workspace raíz. Tampoco aparece actualmente `.agents/`; cuando se cree, debe contener solo configuración de asistentes de desarrollo y permanecer separada del código y los artefactos de producto.

Este momento consiste en preparar límites, convenciones y contexto para poder empezar el trabajo de agentes con una base coherente. No debe interpretarse como una integración ya conectada a sistemas reales de TrackFlow.

## Próximos pasos previstos

1. Confirmar el alcance mínimo del agente de CX: consultas de seguimiento y devoluciones, canales iniciales, datos necesarios, reglas de confianza y criterios de escalado.
2. Crear el esqueleto documentado del agente en `agents/` a partir de `agents/_template/`, incluyendo prompts de producto, herramientas permitidas y pruebas funcionales/regresión. Mantener la configuración de asistentes de desarrollo, si se incorpora, fuera de ese código en `.agents/`.
3. Definir un contrato API en `services/` para consultar un pedido/tracking por guía, recuperar políticas y crear/derivar tickets. Empezar por una API FastAPI modular conforme a la guía; usar datos simulados hasta disponer de integraciones autorizadas.
4. Preparar una base de conocimiento de políticas de envío y devoluciones y un conjunto de evaluación bilingüe con casos normales, ambiguos, datos faltantes, información obsoleta y solicitudes que requieren intervención humana.
5. Implementar la primera interfaz de uso en la ubicación adecuada: `uis/website` para experiencia pública de destinatarios, `uis/backoffice` para revisión y gestión por agentes humanos. No mezclar interfaz con lógica de negocio del backend.
6. Documentar la ejecución local, configuración y límites; incorporar observabilidad, manejo de fallos y protección de datos antes de conectar datos reales o habilitar atención fuera de horario.
7. Medir calidad y operación (resolución automática, exactitud, escalado, latencia y satisfacción), validar la meta orientativa del 80% y ajustar antes de ampliar a más dominios.

## Criterio para declarar el siguiente hito

El primer hito funcional estará listo cuando un usuario pueda consultar un tracking de prueba, recibir una respuesta sustentada en datos y políticas, y obtener un escalado/ticket verificable para un caso que el agente no deba resolver. Debe estar cubierto por pruebas reproducibles en español e inglés y documentado sin depender de sistemas productivos aún no identificados.
