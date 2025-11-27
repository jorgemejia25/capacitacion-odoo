# Configuración Avanzada del Sistema

## Introducción

Una vez que Odoo está funcionando básicamente, hay configuraciones avanzadas que pueden mejorar el rendimiento, la seguridad y la funcionalidad del sistema. Este documento cubre configuraciones más técnicas que pueden ser útiles para empresas que necesitan optimizar su uso de Odoo.

## Configuraciones de Rendimiento

### Configuración del Servidor

**Ubicación:** Archivo de configuración (`odoo.conf` o interfaz de configuración)

**Parámetros Importantes:**

**Workers (Trabajadores):**

```
workers = 4
```

- Define cuántos procesos manejan solicitudes simultáneamente
- Recomendado: (CPU cores × 2) + 1
- Más workers = más usuarios simultáneos, pero más RAM necesaria

**Límite de Memoria por Worker:**

```
limit_memory_soft = 671088640  # 640 MB
limit_memory_hard = 805306368  # 768 MB
```

- Previene que un worker consuma toda la RAM
- Ajustar según RAM disponible

**Tiempo Máximo de Solicitud:**

```
limit_time_cpu = 600
limit_time_real = 1200
```

- Limita cuánto tiempo puede tomar procesar una solicitud
- Previene procesos que se cuelguen

### Configuración de Base de Datos

**Índices:**

Los índices mejoran la velocidad de consultas. Odoo los crea automáticamente, pero puedes optimizar:

- Revisar índices existentes
- Agregar índices personalizados si es necesario
- Limpiar índices no utilizados

**Conexiones:**

```
db_maxconn = 64
```

- Número máximo de conexiones a la base de datos
- Ajustar según capacidad del servidor PostgreSQL

### Configuración de Caché

**Activar Caché:**

```
cache_size_mb = 128
```

- Mejora rendimiento de consultas frecuentes
- Más caché = mejor rendimiento, pero más RAM

**TTL (Time To Live):**

```
cache_timeout = 3600
```

- Tiempo que los datos permanecen en caché
- Balancear entre rendimiento y actualización de datos

## Configuraciones de Seguridad

### Autenticación

**Expiración de Sesiones:**

```
session_expiration = 3600  # 1 hora en segundos
```

- Tiempo después del cual se cierra la sesión por inactividad
- Importante para seguridad

**Política de Contraseñas:**

Configurar en: Configuración → Técnico → Seguridad

- Longitud mínima
- Complejidad requerida
- Expiración de contraseñas
- Historial de contraseñas

### Encriptación

**HTTPS/SSL:**

Para producción, configurar certificado SSL:

1. Obtener certificado SSL (Let's Encrypt, comercial, etc.)
2. Configurar servidor web (Nginx, Apache) para SSL
3. Redirigir HTTP a HTTPS

**Encriptación de Datos Sensibles:**

- Configurar encriptación para campos sensibles
- Usar conexiones encriptadas para base de datos

### Firewall y Red

**Restringir Acceso:**

- Configurar firewall para limitar acceso por IP
- Permitir solo IPs conocidas si es necesario
- Bloquear intentos de acceso maliciosos

**Whitelist de IPs:**

Si es necesario acceso restringido:
- Configurar lista de IPs permitidas
- Usar VPN para acceso remoto

### Logging y Auditoría

**Activar Logs Detallados:**

```
log_level = info
log_handler = :INFO
```

**Tipos de Logs:**

- **debug**: Muy detallado, solo para desarrollo
- **info**: Información general
- **warning**: Advertencias
- **error**: Solo errores

**Auditoría de Accesos:**

- Activar logging de accesos de usuarios
- Revisar logs periódicamente
- Alertas por actividades sospechosas

## Configuraciones de Email

### Servidor de Email Saliente

**Configuración SMTP Avanzada:**

**Ubicación:** Configuración → Técnico → Parámetros → Servidores de Email

**Configuraciones Adicionales:**

- **Bounce Handling**: Manejo de emails rebotados
- **Retry Logic**: Reintentos automáticos
- **Queue Management**: Cola de emails pendientes

### Email Entrante

**Fetchmail (Recuperar Emails):**

Configurar para que Odoo reciba emails automáticamente:

1. Ir a: Configuración → Técnico → Email → Fetchmail Servers
2. Crear nuevo servidor
3. Configurar credenciales IMAP/POP3
4. Activar

**Uso Común:**

- Crear tareas desde emails
- Responder a tickets automáticamente
- Sincronizar emails con oportunidades

### Plantillas de Email

**Personalización:**

- Crear plantillas personalizadas
- Usar variables dinámicas
- Incluir logo y branding

**Ubicación:** Configuración → Técnico → Email → Plantillas

## Configuraciones de Backup

### Respaldos Automáticos

**Configuración en Odoo:**

Aunque es mejor hacerlo a nivel de servidor, Odoo puede configurar respaldos:

**Ubicación:** Configuración → Técnico → Automatización → Acciones Planificadas

**Configurar:**

1. Crear nueva acción planificada
2. Tipo: Backup de Base de Datos
3. Frecuencia: Diaria, semanal, etc.
4. Hora de ejecución
5. Destino del backup

### Estrategia de Respaldos

**Regla 3-2-1:**

- **3 copias** de tus datos
- **2 tipos diferentes** de almacenamiento
- **1 copia fuera del sitio**

**Frecuencia Recomendada:**

- **Base de datos**: Diaria (o más frecuente para empresas críticas)
- **Filestore**: Semanal (o cuando cambia)
- **Configuración**: Cuando cambia

**Retención:**

- Diarios: 30 días
- Semanales: 12 semanas
- Mensuales: 12 meses
- Anuales: Permanentes

## Configuraciones Multi-Compañía

### Configurar Múltiples Empresas

**Ubicación:** Configuración → Usuarios y Compañías → Compañías

**Crear Nueva Compañía:**

1. Clic en "Crear"
2. Configurar información básica
3. Configurar datos fiscales
4. Asignar usuarios

### Reglas de Intercompañía

**Compartir Recursos:**

- Productos compartidos
- Contactos compartidos
- Proveedores compartidos

**Separación de Datos:**

- Contabilidad separada
- Inventarios separados
- Ventas independientes

**Configuración:**

Configurar qué se comparte y qué se mantiene separado en:
Configuración → Usuarios y Compañías → Compañías → Reglas Intercompañía

## Configuraciones de Reportes

### Personalización de Reportes

**Ubicación:** Contabilidad → Configuración → Reportes

**Configuraciones:**

- Formato de números
- Formato de fechas
- Monedas y símbolos
- Encabezados y pies de página

### Generación de Reportes

**Ubicación:** Configuración → Técnico → Reportes

**Opciones:**

- Motor de reportes: QWeb (predeterminado)
- Formato por defecto: PDF, HTML, etc.
- Compresión de reportes grandes

## Configuraciones de API

### Activar REST API

**Ubicación:** Configuración → Técnico → Parámetros del Sistema

**Configuraciones:**

- Activar/desactivar API REST
- Configurar autenticación (OAuth, API Keys)
- Límites de tasa (rate limiting)

### Configuración de CORS

Si necesitas acceso desde aplicaciones web externas:

- Configurar dominios permitidos
- Métodos HTTP permitidos
- Headers permitidos

## Configuraciones de Localización

### Configuración Regional

**Ubicación:** Configuración → Parámetros del Sistema

**Ajustes:**

- Formato de fecha y hora
- Formato de números
- Moneda y símbolos
- Calendario (gregoriano, fiscal, etc.)
- Zona horaria

### Multi-Idioma

**Instalar Idiomas:**

1. Ir a: Configuración → Idiomas
2. Instalar idiomas necesarios
3. Configurar idioma por defecto
4. Traducir términos si es necesario

**Traducciones:**

- Traducir interfaz
- Traducir datos (productos, categorías, etc.)
- Traducir reportes

## Configuraciones de Logs y Monitoreo

### Niveles de Log

**Configurar Logging Detallado:**

```
log_level = debug  # Solo para desarrollo
log_level = info   # Producción normal
log_level = warning # Solo advertencias y errores
```

**Logs Específicos:**

```
log_handler = werkzeug:WARN
log_handler = odoo.addons.base:DEBUG
```

### Monitoreo

**Métricas a Monitorear:**

- Uso de CPU y RAM
- Conexiones a base de datos
- Tiempo de respuesta
- Errores en logs
- Espacio en disco
- Uso de caché

**Herramientas:**

- Logs de Odoo
- Monitoreo de servidor (Nagios, Zabbix, etc.)
- Monitoreo de base de datos

## Configuraciones de Desarrollo

### Modo de Desarrollo

**Activar:**

Configuración → Activar Modo de Desarrollo

**Características:**

- Acceso a opciones técnicas
- Información de depuración
- Herramientas de desarrollo
- Edición de vistas directamente

**Precaución:**

- Solo usar en desarrollo/staging
- No activar en producción a menos que sea necesario

### Actualizaciones

**Configuración de Actualizaciones:**

- Actualización automática de módulos
- Verificación de nuevas versiones
- Notificaciones de actualizaciones disponibles

## Mejores Prácticas de Configuración

### Documentación

1. **Documentar todas las configuraciones personalizadas**
2. **Mantener registro de cambios**
3. **Versionar archivos de configuración**

### Pruebas

1. **Probar configuraciones en entorno de desarrollo primero**
2. **Verificar rendimiento después de cambios**
3. **Probar funcionalidad afectada**

### Revisión Periódica

1. **Revisar configuraciones cada trimestre**
2. **Optimizar según uso real**
3. **Actualizar según mejores prácticas**

## Puntos Clave para Recordar

1. **Configuraciones de rendimiento** afectan directamente la experiencia del usuario
2. **Seguridad** debe configurarse desde el inicio
3. **Respaldos** son críticos y deben automatizarse
4. **Logs** ayudan a diagnosticar problemas
5. **Documentación** facilita el mantenimiento
6. **Pruebas** previenen problemas en producción
7. **Revisión periódica** mantiene el sistema optimizado

## Siguiente Paso

El siguiente módulo cubrirá la gestión y administración diaria de Odoo, incluyendo tareas comunes y mantenimiento rutinario.


