# Brief del proyecto: TrackFlow Tech

## Empresa y problema de negocio

TrackFlow es una empresa de logística de última milla y gestión de almacenes fundada en 2009. Opera en Estados Unidos y España, con almacenes en Los Ángeles y Zaragoza, unos 130 empleados y alrededor de 9 millones de euros de facturación anual. Su servicio cubre el ciclo logístico de las marcas de e-commerce: almacenar inventario, preparar y empaquetar pedidos, coordinar su entrega mediante transportistas y gestionar devoluciones.

La operación ha crecido sobre sistemas que no se integran bien: cada almacén usa un sistema de gestión distinto, el ERP es antiguo, hay integraciones rápidas sin documentar y los datos están repartidos entre sistemas y proveedores cloud. Inventario, envíos y rendimiento de transportistas no tienen una visión común en tiempo real. Pedidos, devoluciones, atención al cliente e informes ejecutivos dependen de tareas manuales. Esto aumenta los errores y los tiempos de respuesta, reduce la rentabilidad y dificulta competir a medida que crece el negocio.

TrackFlow Tech es la unidad interna encargada de construir sistemas, integraciones y automatizaciones inteligentes para modernizar esa operación de dos países. La prioridad personal elegida para este proyecto es Experiencia del Cliente (CX): automatizar consultas repetitivas de marcas y destinatarios sin perder la capacidad de derivar los casos complejos a los agentes humanos.

## Objetivos

- Reducir trabajo manual y errores a lo largo del ciclo de pedidos, almacén, transporte, devoluciones y soporte.
- Unificar y exponer información fiable de ambas operaciones para que clientes, operadores y dirección puedan actuar con datos actuales.
- Mejorar la trazabilidad, observabilidad y documentación de una arquitectura hoy fragmentada y poco documentada.
- Para el caso prioritario de CX, ofrecer atención 24/7 por chat o correo en español e inglés, con una meta de resolver automáticamente cerca del 80% de consultas repetitivas.
- Responder consultas de seguimiento y estado de devoluciones con información del pedido, tracking y políticas; crear y derivar un ticket a una persona cuando falte información o el caso sea complejo.

El agente de CX propuesto necesita consultar el tracking en tiempo real, recuperar el historial del pedido mediante el número de guía y acceder a una base de conocimiento de políticas de envío y devoluciones. Su respuesta debe ser contextual y ofrecer un enlace al estado del paquete. La automatización debe complementar a los 15 agentes de CX de Los Ángeles y Zaragoza, no asumir que toda consulta puede cerrarse sin intervención humana.

Otros resultados de negocio descritos en el briefing incluyen visibilidad global de inventario, ingesta automatizada de pedidos, selección y seguimiento unificados de transportistas, reglas configurables de devoluciones, informes para clientes y un tablero ejecutivo actualizado. Son oportunidades del programa de modernización, no funcionalidades ya implementadas en este repositorio.

## Estructura oficial de aplicaciones

- `uis/website`: presencia web pública de TrackFlow y experiencias orientadas a usuarios externos, como clientes o destinatarios. Toda interfaz visual vive bajo `uis/`; su README permite también portales y otras interfaces, documentados por separado.
- `uis/backoffice`: aplicación interna de administración y operación. Es el espacio para reunir capacidades de back-office como gestión operativa, administración y herramientas para los equipos de CX; cada aplicación debe documentar su propósito, tecnología y ejecución. `website` y `backoffice` son las dos áreas principales indicadas por el README de `uis/`, no aplicaciones que ya existan.
- `services/`: servicios backend concretos, APIs y workers en segundo plano, cada uno en su subcarpeta y con documentación propia. La guía raíz recomienda comenzar con una API empresarial centralizada en FastAPI, organizada por routers o módulos de dominio, en vez de dividir pronto el sistema en muchos microservicios; extraer workers solo cuando haya una necesidad real de ejecución independiente.

Las interfaces consumen lógica y datos de `services/`; no se debe duplicar allí la lógica backend. El servicio de tracking, los tickets y las consultas de pedido/políticas del agente CX deben integrarse mediante contratos documentados. Los README de carpetas definen una organización prevista; el estado actual del repositorio sigue siendo una plantilla y no contiene estas aplicaciones ejecutables.

## Criterios de éxito del proyecto

El avance debe medirse con resultados operativos, no solo por tener un agente conversacional: proporción de consultas resueltas sin intervención, precisión de las respuestas de tracking y políticas, tasa de derivación apropiada, tiempo de respuesta, disponibilidad y satisfacción del cliente. El objetivo del 80% procede del briefing y es una aspiración a validar con datos y evaluación, no una garantía de rendimiento.
