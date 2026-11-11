# Protocolo para agentes del repositorio

Estas directrices se aplican a cualquier agente que lea, edite, pruebe o prepare cambios en este monorepo. Si una instrucción del desarrollador contradice este protocolo, prevalece la instrucción explícita del desarrollador.

## 1. Contexto obligatorio al inicio de cada sesión

Antes de explorar o modificar código, lee en su totalidad y en este orden:

1. `CONTEXT.es.md`, fuente del contexto de negocio y requisitos vigentes.
2. `memory-bank/projectbrief.md`.
3. `memory-bank/techContext.md`.
4. `memory-bank/progress.md`.

Comprueba también los README de las carpetas que vayas a modificar. Si falta `CONTEXT.es.md` o cualquiera de los archivos obligatorios, no supongas ni inventes su contenido: informa al desarrollador y solicita que lo restaure o confirme cómo proceder. No continúes con cambios de producto hasta resolver esa falta.

## 2. Flujo obligatorio antes de cada commit

Completa los siguientes pasos en orden. No prepares el commit hasta que los pasos anteriores estén terminados:

1. **Lectura y verificación de contexto.** Confirma que has leído `CONTEXT.es.md` y los tres archivos de `memory-bank/`. Revisa además la documentación específica de las rutas afectadas y comprueba que la solución respeta el dominio, la arquitectura y el estado del proyecto.
2. **Validación de calidad y compilación.** Ejecuta las pruebas, el lint, el typecheck y/o la compilación que correspondan a la aplicación o servicio modificado. Usa los comandos documentados por ese componente. Si no existen o no pueden ejecutarse, deja constancia del motivo y del alcance no verificado; no declares una validación exitosa que no ocurrió.
3. **Actualización del progreso.** Actualiza `memory-bank/progress.md` con lo completado, los resultados relevantes de validación y los siguientes pasos reales. No marques como implementado trabajo pendiente ni elimines avances ajenos al cambio actual.
4. **Generación del commit.** Prepara un mensaje semántico que identifique el área o la skill aplicada. Formato recomendado: `tipo(área): descripción [skill: nombre]`; por ejemplo, `feat(cx): añade consulta de tracking [skill: customer-support-agent]`. Omite el sufijo de skill únicamente cuando no se haya aplicado ninguna. No incluyas en el commit cambios preexistentes o ajenos a la tarea.

## 3. Archivos y estructura protegidos

No modifiques los siguientes elementos sin confirmación explícita del desarrollador:

- `CONTEXT.es.md`, que es la fuente de verdad del contexto del negocio.
- Archivos de secretos, credenciales o configuración local, incluidos `.env`, `.env.*` y cualquier archivo que contenga valores secretos. Nunca leas, registres en logs ni incluyas secretos en commits.
- Configuración de despliegue o infraestructura, incluidos `infra/`, `docker-compose.yml` y variantes, Dockerfiles, manifiestos de despliegue y configuración CI/CD.
- La estructura base del monorepo: crear, eliminar, renombrar o mover carpetas de primer nivel, cambiar convenciones de ubicación o alterar configuración compartida que afecte a varios componentes.
- Este protocolo (`AGENTS.md`) y la configuración de agentes de desarrollo en `.agents/`.

La protección no impide leer estos archivos cuando sea necesario para comprender una tarea. Si un cambio solicitado parece requerir modificar un elemento protegido, explica el motivo y pide confirmación antes de editarlo. No reviertas cambios preexistentes de otras personas.

## 4. Ubicación de las aplicaciones y el código de producto

El código ejecutable de las aplicaciones de producto solo puede vivir en estas rutas:

- `uis/website/`: sitio público y experiencias de cara a clientes o destinatarios.
- `uis/backoffice/`: interfaces internas de administración, operaciones y atención al cliente.
- `services/`: APIs, lógica backend y workers que dan soporte a las aplicaciones.

No crees aplicaciones paralelas ni ubiques código ejecutable de estas aplicaciones en otras carpetas de primer nivel, como `apps/`, `frontend/`, `backend/` o `api/`. Si una nueva aplicación no encaja en las tres rutas autorizadas, consulta al desarrollador antes de crearla. Mantén en `agents/`, `skills/`, `mcps/` y `workflows/` únicamente los artefactos propios de esas áreas según sus README; la lógica de producto servida por una API debe residir en `services/` y las interfaces en `uis/`.

La carpeta oculta `.agents/`, si se incorpora, es exclusivamente para configuración y asistencia al desarrollo. No contiene código de producto. A la inversa, `agents/` forma parte de la estructura del monorepo para los artefactos de agentes del proyecto y no sustituye a `.agents/`.

## 5. Criterios generales de trabajo

- Respeta las convenciones y README de cada componente; documenta aplicaciones y servicios nuevos en su propia carpeta.
- Mantén los cambios acotados a la tarea y no inventes stack, integraciones o capacidades que no estén confirmados en el briefing o en documentación técnica.
- Antes de afirmar que un cambio está listo, resume qué cambió y qué validaciones se ejecutaron o quedaron pendientes.