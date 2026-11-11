# Contexto técnico

## Stack conocido y nivel de certeza

TrackFlow no parte de una plataforma homogénea. El entorno empresarial descrito incluye dos sistemas de gestión de almacén (uno comercial y otro basado en una hoja de cálculo avanzada), un ERP de principios de la década de 2010, scripts punto a punto en Python sin documentación y bases de datos alojadas en dos proveedores cloud distintos. Hay ocho transportistas entre ambos países y sus portales/APIs se consultan de forma fragmentada. No se especifican los nombres de los SGA, ERP, proveedores cloud, motores de base de datos ni tecnologías de frontend; no deben inventarse.

El stack técnico documentado para el proyecto es una combinación de convenciones y piezas iniciales, no un stack ya desplegado:

- Backend empresarial recomendado: Python con FastAPI, en una API centralizada y modular.
- Datos: existen scripts heredados en Python; el monorepo reserva `data/raw/`, `data/pipelines/`, `data/process/` y `data/eval/` para fuentes, transformaciones, resultados intermedios y evaluación.
- Tipos compartidos: `packages/shared/` contiene metadata inicial de `@repo/shared-types`, con `index.ts` como entrada declarada. Esto no demuestra que haya un frontend, un workspace Node ni un paquete ya desarrollado.
- Entorno y despliegue: el README recomienda `docker-compose.yml` en la raíz para orquestación local e `infra/` para Dockerfiles, despliegue y configuración de infraestructura. No hay un `docker-compose.yml` ni una aplicación ejecutable en la estructura actual.
- Frontend: `uis/website` y `uis/backoffice` son las ubicaciones previstas. El README no decide framework, lenguaje ni sistema de diseño; cualquier elección deberá documentarse al crear cada app.
- IA: el proyecto contempla agentes, skills reutilizables, servidores MCP, workflows y datasets de evaluación en sus carpetas respectivas. El agente de CX se propone como caso prioritario, pero aún no está implementado.

## Decisiones de arquitectura

1. **Monorepo con límites por responsabilidad.** `uis/` aloja interfaces, `services/` aloja APIs y workers, `data/` ingiere y transforma datos, `agents/` contiene agentes de producto y `docs/` registra decisiones transversales. `packages/` y `shared/` son para reutilización, con roles distintos: código versionable frente a recursos/esquemas pequeños.
2. **Backend centralizado al inicio.** Seguir la recomendación de una API FastAPI con módulos/routers por dominio (por ejemplo, pedidos, inventario, tracking, devoluciones y CX). Evitar microservicios prematuros; extraer un worker cuando el aislamiento o el ciclo de ejecución lo justifique.
3. **Separación estricta de agentes de desarrollo y producto.** `.agents/` queda reservada exclusivamente a configuración de asistentes que ayudan a desarrollar el repositorio (instrucciones, perfiles o flujos de trabajo del entorno). No alberga código, prompts operativos, herramientas ni lógica que forme parte del producto TrackFlow. Los agentes de IA que ejecuta el producto, sus prompts, integraciones y pruebas viven en `agents/<agente>/`; sus APIs en `services/`, sus interfaces en `uis/` y sus conectores MCP en `mcps/` cuando correspondan. La carpeta `.agents/` todavía no aparece en el estado actual del repositorio: esta es una regla de organización para cuando se incorpore.
4. **Integraciones explícitas y documentadas.** La API debe aislar sistemas heredados y transportistas detrás de contratos/adaptadores documentados, evitando nuevas integraciones punto a punto opacas. Las fuentes y transformaciones de datos deben quedar trazables y evaluables.

## Restricciones y consideraciones del escenario

- Operación en Estados Unidos y España, con almacenes, equipos y proveedores distintos; las diferencias entre sistemas y procesos locales deben modelarse, no ocultarse mediante supuestos de un único almacén.
- Atención en español e inglés, disponible 24/7. El agente debe degradar con claridad cuando tracking o políticas no estén disponibles y escalar a una persona cuando no pueda responder con fundamento.
- La información de pedidos, destinatarios, direcciones y devoluciones puede ser sensible. Aplicar mínimo privilegio, limitar los datos devueltos al agente, proteger secretos y definir retención/acceso antes de conectar fuentes reales. El briefing no identifica obligaciones normativas concretas ni permite afirmar una certificación determinada.
- Los datos actuales están fragmentados y carecen de telemetría centralizada; cualquier respuesta automática debe indicar su fuente/estado y no presentar datos desactualizados como seguimiento en vivo.
- No se conocen contratos, límites, disponibilidad ni capacidades exactas de las ocho integraciones de transportistas. Diseñar fallos parciales, tiempos de espera y pruebas por proveedor; no asumir que todas ofrecen una API uniforme.
- No hay runner de workspace en la raíz ni scripts globales definidos en `package.json`; documentar comandos por aplicación/servicio y no suponer que una herramienta común ya está instalada.

## Fuentes de verdad

`CONTEXT.es.md` define el negocio y sus necesidades; `company-choice.md` concreta la prioridad individual de CX y el agente propuesto; `README.es.md` y los README de `uis/` y `services/` definen la organización recomendada del monorepo. Si una futura implementación toma decisiones adicionales de stack o despliegue, registrarlas en su README y, si afectan a varias áreas, en `docs/`.
