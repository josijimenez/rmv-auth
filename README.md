# RMV Auth — Servicio de Autenticación y RBAC

Microservicio de autenticación, roles y permisos para el sistema **RMV** (Registro de Matriculación Vehicular) del GAD Municipal de Loja — cubre el proceso de matriculación, revisión técnica y multas vehiculares del Cantón Loja.

## Origen de este proyecto

Esta base parte de [`fastapi_rbac`](https://github.com/mnaimfaizy/fastapi_rbac) (mnaimfaizy), licenciado bajo **MIT** — ver [`LICENSE`](./LICENSE), que se conserva intacto tal como exige la licencia. Se usó como punto de partida porque ya resuelve, de forma funcional (sin necesidad de tocar código), la administración de:

- Usuarios
- Roles
- Permisos individuales
- Grupos de roles y grupos de permisos

Todo administrable desde el panel web, con asignación dinámica de permisos a roles y de roles a usuarios.

## Qué se modificó respecto al original

1. **Arreglo de dependencia rota**: `backend/requirements.txt` fijaba `backports-asyncio-runner==1.2.0`, versión inexistente en PyPI — corregido a `1.0.0`.
2. **Archivos `.env.development` creados**: el `docker-compose.dev.yml` de `backend/` y `react-frontend/` esperan un `.env.development` que no viene incluido en el repo original — se generó a partir de `.env.example`.
3. **Renombrado de identificadores** del proyecto original (`fastapi-rbac` / `fastapi_rbac`) a `rmv-auth` / `rmv_auth` en: `pyproject.toml`, `package.json`, variables de entorno, nombres de contenedores/imágenes/red Docker.

## Roadmap de este proyecto

- [x] Validar que el RBAC (usuarios, roles, permisos, grupos) funciona de extremo a extremo.
- [ ] Integrar autenticación con **Keycloak** (mismo realm que usa `gim2`):
      - Keycloak aporta identidad (nombre, apellido, identificación, correo, teléfono, estado de cuenta).
      - Roles y permisos siguen viviendo en las tablas propias de este proyecto (no en Keycloak).
      - Se reemplaza el login/JWT propio del template por validación de tokens emitidos por Keycloak + sincronización JIT (just-in-time) del perfil de usuario contra la tabla `User` local.
- [ ] Definir los permisos concretos del dominio RMV (ej. `vehiculo.matricular`, `revision.aprobar`, `multa.crear`, `multa.anular`, etc.) y los roles operativos (inspector, cajero, supervisor, etc.).
- [ ] Exponer/consumir los endpoints que afectan a `gim2` (altas, revisiones, multas) protegidos por estos permisos.

## Puesta en marcha local

Ver la guía de despliegue con Docker en el proyecto Claude "RMV" (`claude/guia-despliegue-fastapi-rbac.md`), válida también aquí — solo cambian los nombres de contenedores (`rmv_auth_*` en vez de `fastapi_rbac_*`).

---

Documentación original del template (arquitectura, testing, etc.) conservada en [`DOCUMENTATION.md`](./DOCUMENTATION.md) y [`docs/`](./docs/).
