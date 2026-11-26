# Proyecto Final DevOps - API de Tareas

**Estudiante:** estudiante04
**Servidor:** 178.128.14.98
**Puerto:** 8004

## Descripción

API REST de gestión de tareas con PostgreSQL.

## Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/` | Información |
| GET | `/health` | Estado |
| GET | `/tareas` | Listar |
| POST | `/tareas` | Crear |

## Despliegue

```bash
docker compose up -d
curl http://localhost:8004/health
```

## Entrega

- **Repositorio:** https://github.com/KatorSarah/E04devops-final
- **API:** http://178.128.14.98:8004/health
