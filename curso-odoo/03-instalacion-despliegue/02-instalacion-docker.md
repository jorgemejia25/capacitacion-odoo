# Instalación de Odoo con Docker

## Introducción

Docker es una de las formas más populares y manejables de instalar Odoo. Este documento explica el proceso completo de instalación usando Docker, incluyendo configuración y gestión.

## ¿Qué es Docker?

Docker es una plataforma que permite empaquetar aplicaciones y sus dependencias en "contenedores" que pueden ejecutarse de forma consistente en cualquier sistema que tenga Docker instalado.

**Ventajas para Odoo:**

- Instalación rápida y sencilla
- Aislamiento de componentes
- Fácil de actualizar
- Misma configuración en desarrollo y producción
- Simplifica la gestión de dependencias

## Requisitos Previos

### Software Necesario

1. **Docker**
   - Docker Engine (para ejecutar contenedores)
   - Docker Compose (para orquestar múltiples contenedores)

2. **Sistema Operativo**
   - Linux (recomendado)
   - macOS
   - Windows (con WSL2)

### Recursos del Sistema

- **RAM**: Mínimo 4 GB (recomendado 8 GB o más)
- **Disco**: Al menos 20 GB de espacio libre
- **CPU**: Mínimo 2 cores (recomendado 4 o más)

## Estructura del Proyecto

Una instalación típica de Odoo con Docker incluye:

```
proyecto-odoo/
├── docker-compose.yml      # Configuración de contenedores
├── config/
│   └── odoo.conf          # Configuración de Odoo
├── addons/                 # Módulos personalizados (opcional)
└── odoo-data/              # Datos persistentes (creado automáticamente)
```

## Proceso de Instalación

### Paso 1: Instalar Docker y Docker Compose

**En Linux (Ubuntu/Debian):**

```bash
# Actualizar sistema
sudo apt update

# Instalar Docker
sudo apt install docker.io docker-compose

# Iniciar Docker
sudo systemctl start docker
sudo systemctl enable docker

# Verificar instalación
docker --version
docker-compose --version
```

**En macOS:**

Descargar e instalar Docker Desktop desde:
https://www.docker.com/products/docker-desktop

**En Windows:**

Instalar Docker Desktop para Windows desde:
https://www.docker.com/products/docker-desktop

### Paso 2: Crear Estructura de Carpetas

```bash
# Crear directorio del proyecto
mkdir proyecto-odoo
cd proyecto-odoo

# Crear subdirectorios
mkdir config
mkdir addons
```

### Paso 3: Crear Archivo docker-compose.yml

El archivo `docker-compose.yml` define todos los servicios (contenedores) necesarios:

```yaml
version: "3.9"

services:
  db:
    image: postgres:15-alpine
    container_name: odoo-db
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
    volumes:
      - db-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U odoo"]
      interval: 10s
      timeout: 5s
      retries: 5

  odoo:
    image: odoo:17.0
    container_name: odoo-web
    depends_on:
      db:
        condition: service_healthy
    ports:
      - "8069:8069"
    environment:
      HOST: 0.0.0.0
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: odoo
      DB_PASSWORD: odoo
      DB_NAME: postgres
    volumes:
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
      - odoo-data:/var/lib/odoo

volumes:
  db-data:
  odoo-data:
```

**Explicación de componentes:**

- **db**: Contenedor de PostgreSQL (base de datos)
- **odoo**: Contenedor de Odoo (aplicación)
- **volumes**: Almacenamiento persistente de datos

### Paso 4: Crear Archivo de Configuración

Crear `config/odoo.conf`:

```ini
[options]
xmlrpc_port = 8069
db_host = db
db_port = 5432
db_user = odoo
db_password = odoo
addons_path = /usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons
data_dir = /var/lib/odoo
```

**Parámetros importantes:**

- `xmlrpc_port`: Puerto donde escucha Odoo
- `db_host`: Nombre del servicio de base de datos en Docker
- `addons_path`: Ruta donde busca módulos
- `data_dir`: Directorio para archivos adjuntos y logs

### Paso 5: Iniciar los Contenedores

```bash
# Iniciar todos los servicios
docker-compose up -d

# Verificar que están corriendo
docker-compose ps
```

El parámetro `-d` ejecuta los contenedores en segundo plano (detached mode).

### Paso 6: Acceder a Odoo

1. Abrir navegador web
2. Ir a: `http://localhost:8069`
3. Deberías ver la pantalla de configuración inicial de Odoo

## Configuración Inicial de Odoo

### Primera Configuración

1. **Idioma**: Seleccionar idioma preferido
2. **Nombre de Base de Datos**: Ingresar nombre único
3. **Email y Contraseña**: Para el usuario administrador
4. **País**: Seleccionar país (afecta configuraciones contables)
5. **Datos de Demostración**: Opcional, útil para aprender

### Crear Base de Datos

Después de completar el formulario:

- Odoo creará la base de datos automáticamente
- Esto puede tomar varios minutos
- Al finalizar, serás redirigido al dashboard

## Comandos Útiles de Docker

### Gestión de Contenedores

```bash
# Iniciar contenedores
docker-compose up -d

# Detener contenedores
docker-compose stop

# Detener y eliminar contenedores
docker-compose down

# Ver logs
docker-compose logs -f odoo

# Ver logs de base de datos
docker-compose logs -f db

# Reiniciar un servicio específico
docker-compose restart odoo
```

### Acceso a Contenedores

```bash
# Acceder a la terminal del contenedor Odoo
docker-compose exec odoo bash

# Acceder a la terminal de la base de datos
docker-compose exec db psql -U odoo -d postgres

# Ejecutar comando específico
docker-compose exec odoo odoo --version
```

### Gestión de Volúmenes

```bash
# Listar volúmenes
docker volume ls

# Inspeccionar un volumen
docker volume inspect proyecto-odoo_db-data

# Respaldar volumen (ejemplo)
docker run --rm -v proyecto-odoo_db-data:/data -v $(pwd):/backup alpine tar czf /backup/db-backup.tar.gz /data
```

## Configuraciones Avanzadas

### Cambiar Versión de Odoo

Editar `docker-compose.yml`:

```yaml
odoo:
  image: odoo:16.0  # Cambiar versión aquí
```

Luego:

```bash
docker-compose pull odoo
docker-compose up -d
```

### Agregar Módulos Personalizados

1. Colocar módulos en la carpeta `addons/`
2. Asegurarse de que `addons_path` en `config/odoo.conf` incluya `/mnt/extra-addons`
3. Reiniciar Odoo:

```bash
docker-compose restart odoo
```

### Configurar Variables de Entorno

Editar `docker-compose.yml`:

```yaml
odoo:
  environment:
    - ADMIN_PASSWORD=tu_contraseña_admin
    - UNACCENT=true
    - WITHOUT_DEMO=all
```

### Configurar Puertos Personalizados

```yaml
odoo:
  ports:
    - "9070:8069"  # Puerto externo:interno
```

### Agregar Trabajos Programados (Cron)

Odoo ejecuta trabajos programados automáticamente. Para asegurar que funcionen:

```yaml
odoo:
  command: ["odoo", "-c", "/etc/odoo/odoo.conf"]
  # Los cron jobs se ejecutan automáticamente
```

## Respaldo y Restauración

### Respaldar Base de Datos

```bash
# Respaldar desde contenedor
docker-compose exec db pg_dump -U odoo postgres > backup.sql

# O usar docker run
docker run --rm \
  --volumes-from proyecto-odoo-db-1 \
  -v $(pwd):/backup \
  postgres:15-alpine \
  pg_dump -U odoo postgres > backup.sql
```

### Restaurar Base de Datos

```bash
# Restaurar
docker-compose exec -T db psql -U odoo postgres < backup.sql

# O usar docker run
docker run --rm \
  --volumes-from proyecto-odoo-db-1 \
  -v $(pwd):/backup \
  postgres:15-alpine \
  psql -U odoo postgres < /backup/backup.sql
```

### Respaldar Filestore (Archivos Adjuntos)

```bash
# Respaldar directorio de archivos
docker run --rm \
  -v proyecto-odoo_odoo-data:/data \
  -v $(pwd):/backup \
  alpine tar czf /backup/filestore-backup.tar.gz -C /data filestore
```

## Solución de Problemas Comunes

### Odoo no Inicia

```bash
# Ver logs detallados
docker-compose logs odoo

# Verificar que la base de datos esté lista
docker-compose logs db

# Verificar configuración
docker-compose exec odoo cat /etc/odoo/odoo.conf
```

### Error de Conexión a Base de Datos

Verificar:
1. Que el contenedor `db` esté corriendo
2. Que las credenciales en `odoo.conf` coincidan con las de `docker-compose.yml`
3. Que `db_host` sea `db` (nombre del servicio)

### Puerto Ya en Uso

```bash
# Ver qué está usando el puerto
sudo lsof -i :8069

# Cambiar puerto en docker-compose.yml
ports:
  - "8070:8069"  # Usar otro puerto externo
```

### Limpiar Todo y Empezar de Nuevo

```bash
# CUIDADO: Esto elimina todo, incluyendo datos
docker-compose down -v
docker-compose up -d
```

## Mejores Prácticas

### Seguridad

1. **Cambiar contraseñas por defecto**: No usar `odoo` como contraseña en producción
2. **Variables de entorno**: Usar archivos `.env` para credenciales sensibles
3. **Redes**: Aislar contenedores en redes Docker privadas
4. **Firewall**: Configurar firewall para limitar acceso

### Rendimiento

1. **Recursos**: Asignar suficiente RAM y CPU
2. **Volúmenes**: Usar volúmenes nombrados en lugar de bind mounts para mejor rendimiento
3. **Caché**: Configurar caché apropiadamente

### Mantenimiento

1. **Actualizaciones**: Actualizar imágenes regularmente
2. **Respaldos**: Automatizar respaldos diarios
3. **Logs**: Configurar rotación de logs
4. **Monitoreo**: Monitorear uso de recursos

## Estructura Recomendada para Producción

```
proyecto-odoo/
├── docker-compose.yml
├── docker-compose.prod.yml  # Configuración de producción
├── .env                     # Variables de entorno (NO commitear)
├── config/
│   └── odoo.conf
├── addons/
│   └── custom_modules/
├── scripts/
│   ├── backup.sh
│   └── restore.sh
└── backups/
    └── (respaldos automáticos)
```

## Puntos Clave para Recordar

1. **Docker simplifica** la instalación y gestión de Odoo
2. **Docker Compose** orquesta múltiples contenedores juntos
3. **Volúmenes** persisten los datos entre reinicios
4. **Respaldos regulares** son esenciales para la base de datos
5. **Logs** son tu mejor herramienta para diagnosticar problemas
6. **Configuración** se hace mediante archivos, no interfaz web
7. **Seguridad** requiere configuración explícita

## Siguiente Paso

Ahora que sabes cómo instalar Odoo con Docker, el siguiente documento explicará la configuración inicial del sistema una vez que Odoo está instalado.


