---
scope: global
---

# Estructura y rutas del monorepo

**Alcance:** global. Esta regla está siempre activa para todo el proyecto.

## Rutas oficiales de aplicación

Todo el código ejecutable de aplicaciones de producto debe ubicarse exclusivamente en estas rutas:

- `uis/website/`: sitio público y experiencias para clientes o destinatarios.
- `uis/backoffice/`: interfaces internas para administración, operaciones y atención al cliente.
- `services/`: APIs, lógica backend y workers que dan soporte a las aplicaciones.

Está prohibido crear carpetas paralelas de frontend o API fuera de estas rutas, por ejemplo `apps/`, `frontend/`, `backend/` o `api/` en la raíz. Si una aplicación no encaja en las ubicaciones oficiales, solicita confirmación al desarrollador antes de crear o mover carpetas. Respeta la organización documentada en los README de cada área.

## Navegación obligatoria para comandos

Antes de ejecutar cualquier comando `npm` o script de build, test, lint o typecheck, cambia al subdirectorio del módulo objetivo. No ejecutes estos comandos desde la raíz del monorepo ni asumas que existe un workspace npm global.

Ejemplos:

```sh
cd uis/website && npm run build
cd uis/backoffice && npm run test
cd services/api && npm run lint
```

Primero identifica el módulo modificado y revisa sus scripts disponibles. Ejecuta los comandos documentados desde esa carpeta; si el módulo no usa npm, sigue su README y usa su herramienta local desde el mismo subdirectorio.

## Configuración de herramientas IA y módulos de producto

- `.agents/` contiene exclusivamente reglas, instrucciones y skills para herramientas de desarrollo asistidas por IA. No coloques ahí lógica, configuración operativa, prompts ejecutados en producción ni código de producto.
- `agents/` y `skills/` en la raíz del monorepo son áreas del proyecto para módulos y capacidades de agentes de producto, conforme a sus README. No son alias de `.agents/`.
- El código ejecutable de la aplicación de producto sigue limitado a `uis/website/`, `uis/backoffice/` y `services/`; las carpetas de agentes y skills no justifican crear aplicaciones paralelas.