# Proyecto Final DevOps - API de Tareas

**Estudiante:** estudiante04  
**Servidor:** 178.128.14.98  
**Puerto:** 8004

## Descripción del Proyecto

API REST de gestión de tareas desarrollada en Python (Flask) con persistencia en PostgreSQL, desplegada mediante Docker Compose. El proyecto implementa prácticas DevSecOps con herramientas de seguridad y monitoreo.

## Arquitectura

```
┌─────────────────────────────────────────────────────────┐
│                    Servidor (178.128.14.98)              │
│                                                         │
│   ┌─────────────────────────────────────────────────┐   │
│   │            Docker Compose (estudiante04)         │   │
│   │                                                 │   │
│   │   ┌─────────────┐    ┌─────────────────────┐   │   │
│   │   │   est04_api │───▶│  PostgreSQL (db)    │   │   │
│   │   │   :8004:5000│    │  :5432 (interno)    │   │   │
│   │   └─────────────┘    └─────────────────────┘   │   │
│   │                                                 │   │
│   └─────────────────────────────────────────────────┘   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

## Requisitos

- Docker Engine 20.10+
- Docker Compose V2
- Acceso SSH al servidor 178.128.14.98

## Endpoints de la API

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/` | Información del servicio y estudiante |
| GET | `/health` | Estado de la API y conexión a BD |
| GET | `/tareas` | Listar todas las tareas |
| POST | `/tareas` | Crear tarea: `{"titulo": "..."}` |
| PUT | `/tareas/<id>` | Actualizar: `{"titulo": "...", "completada": true}` |
| DELETE | `/tareas/<id>` | Eliminar tarea |

## Instalación y Despliegue

### 1. Conexión al servidor

```bash
ssh estudiante04@178.128.14.98
```

### 2. Navegación al directorio del proyecto

```bash
cd ~/estudiante04/devops-final
```

### 3. Inicializar repositorio Git

```bash
git init
git add .
git commit -m "Initial commit: Proyecto Final DevOps - estudiante04"
```

### 4. Configurar remoto (primera vez)

```bash
git remote add origin https://github.com/KatorSarah/E04devops-final.git
```

### 5. Construir y levantar contenedores

```bash
docker compose build
docker compose up -d
```

### 6. Verificar funcionamiento

```bash
# Verificar estado de contenedores
docker ps -a --filter "name=est04"

# Probar endpoint de salud
curl http://localhost:8004/health

# Probar API completa
curl http://178.128.14.98:8004/health
```

## Comandos de Gestión

### Ver logs

```bash
# Logs de la API
docker logs est04_api -f

# Logs de la base de datos
docker logs est04_db -f
```

### Detener contenedores

```bash
docker compose down
```

### Reiniciar

```bash
docker compose restart
```

### Ver estado de recursos

```bash
docker stats --filter "name=est04"
```

## DevSecOps - Herramientas de Seguridad

### Ejecución del script de seguridad

```bash
# Hacer ejecutable el script
chmod +x seguridad.sh

# Ejecutar todas las herramientas
./seguridad.sh

# O solo monitoreo
./seguridad.sh monitorear
```

### Herramientas incluidas

1. **Gitleaks**: Detección de secretos en el código
2. **Semgrep**: Análisis estático de seguridad
3. **Syft**: Inventario de dependencias (SBOM)
4. **Trivy**: Escáner de vulnerabilidades
5. **Hadolint**: Linting del Dockerfile

### Reportes generados

Los reportes se guardan en la carpeta `reportes/`:
- `gitleaks-report.json` - Reporte de Gitleaks
- `semgrep-report.json` - Reporte de Semgrep
- `syft-sbom.json` - Bill of Materials
- `trivy-vulnerabilities.json` - Vulnerabilidades
- `hadolint-report.log` - Lint del Dockerfile

## Monitoreo

El script `seguridad.sh` incluye una función `monitorear()` que:

1. Verifica si la API responde en `/health`
2. Muestra el estado de los contenedores `est04_api` y `est04_db`
3. Visualiza los últimos logs de ambos contenedores
4. Registra todo con fecha en `monitoreo.log`

```bash
./seguridad.sh monitorear
```

## Variables de Entorno

El archivo `.env` contiene las configuraciones sensibles:

```bash
ESTUDIANTE=estudiante04
API_PORT=8004
POSTGRES_DB=tareas
POSTGRES_USER=estudiante
POSTGRES_PASSWORD=devops2024seguro
DB_HOST=db
DB_PORT=5432
```

**⚠️ IMPORTANTE:** El archivo `.env` nunca debe subirse a GitHub.

## Git - Historial de Commits

Ver historial completo:

```bash
git log --oneline --graph --all
```

## Solución de Problemas

### La API no responde

1. Verificar que los contenedores estén corriendo:
   ```bash
   docker ps -a --filter "name=est04"
   ```

2. Verificar logs:
   ```bash
   docker logs est04_api
   ```

3. Reiniciar contenedores:
   ```bash
   docker compose restart
   ```

### Error de conexión a base de datos

1. Verificar que PostgreSQL esté saludable:
   ```bash
   docker exec est04_db pg_isready -U estudiante
   ```

2. Revisar logs de la base de datos:
   ```bash
   docker logs est04_db
   ```

### Contenedores con nombres duplicados

Si existen contenedores anteriores con el mismo nombre:

```bash
docker stop est04_db est04_api
docker rm est04_db est04_api
docker compose up -d
```

## Entrega

- **Repositorio:** https://github.com/KatorSarah/E04devops-final
- **API funcionando:** http://178.128.14.98:8004/health
