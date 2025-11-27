# Arquitectura Completa de Odoo

## Introducción

Este documento proporciona una visión completa de cómo todos los componentes de Odoo trabajan juntos para formar un sistema integrado y funcional.

## Vista General de la Arquitectura

```
┌─────────────────────────────────────────────────────────────┐
│                    USUARIOS                                  │
│  (Empleados, Administradores, Clientes Externos)            │
└─────────────────────┬───────────────────────────────────────┘
                      │
          ┌───────────┴───────────┐
          │                       │
    ┌─────▼─────┐         ┌──────▼──────┐
    │  Navegador│         │  Aplicación │
    │   Web     │         │   Móvil     │
    └─────┬─────┘         └──────┬──────┘
          │                      │
          │  HTTP/HTTPS          │  API REST
          │                      │
┌─────────▼──────────────────────▼──────────────────────────┐
│              SERVidor Odoo                                  │
│  ┌────────────────────────────────────────────────────┐   │
│  │  Interfaz Web (OWL Framework)                      │   │
│  └────────────────────────────────────────────────────┘   │
│  ┌────────────────────────────────────────────────────┐   │
│  │  Motor de Aplicación                               │   │
│  │  - Controladores                                   │   │
│  │  - ORM (Object-Relational Mapping)                 │   │
│  │  - Motor de Reportes                               │   │
│  │  - Sistema de Trabajos Programados                 │   │
│  └────────────────────────────────────────────────────┘   │
│  ┌────────────────────────────────────────────────────┐   │
│  │  Módulos                                           │   │
│  │  - Módulos Core                                    │   │
│  │  - Módulos de Aplicaciones                         │   │
│  │  - Módulos Personalizados                          │   │
│  └────────────────────────────────────────────────────┘   │
└─────────┬──────────────────────────────────────────────────┘
          │
          │  SQL Queries
          │
┌─────────▼──────────────────────────────────────────────────┐
│          Base de Datos PostgreSQL                          │
│  ┌────────────────────────────────────────────────────┐   │
│  │  Tablas de Datos                                   │   │
│  │  - res_users (usuarios)                            │   │
│  │  - res_partner (clientes/proveedores)              │   │
│  │  - product_product (productos)                     │   │
│  │  - sale_order (pedidos)                            │   │
│  │  - ... y muchas más                                │   │
│  └────────────────────────────────────────────────────┘   │
│  ┌────────────────────────────────────────────────────┐   │
│  │  Configuración del Sistema                         │   │
│  │  - res_company (empresas)                          │   │
│  │  - ir_config_parameter (parámetros)                │   │
│  └────────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────┘
          │
          │  Archivos
          │
┌─────────▼──────────────────────────────────────────────────┐
│          Sistema de Archivos                               │
│  - Filestore (adjuntos)                                    │
│  - Logs                                                    │
│  - Módulos personalizados                                  │
└────────────────────────────────────────────────────────────┘
```

## Componentes Detallados

### 1. Capa de Acceso

**Navegadores Web**

Los usuarios acceden principalmente a través de navegadores web estándar:
- Chrome, Firefox, Safari, Edge
- Compatible con dispositivos móviles (responsive design)
- No requiere plugins o extensiones especiales

**Aplicaciones Móviles**

Odoo ofrece aplicaciones móviles para:
- iOS
- Android

Estas aplicaciones se comunican con el servidor mediante APIs REST.

**APIs para Integraciones**

Sistemas externos pueden conectarse mediante:
- XML-RPC (legacy, pero aún soportado)
- REST API (moderno y recomendado)
- JSON-RPC (usado internamente)

### 2. Servidor de Aplicación Odoo

El servidor de Odoo es un servidor de aplicaciones Python que contiene:

#### 2.1 Framework Web

**Controladores (Controllers)**

- Manejan las solicitudes HTTP entrantes
- Definen las rutas/URLs del sistema
- Procesan los datos y generan respuestas

**Vistas y Templates**

- QWeb: Motor de plantillas para generar HTML
- Define cómo se muestra la información
- Permite personalización sin tocar el código Python

#### 2.2 ORM (Object-Relational Mapping)

**¿Qué hace el ORM?**

Convierte objetos Python en consultas SQL y viceversa. Esto significa que los desarrolladores trabajan con objetos en lugar de escribir SQL directamente.

**Ventajas:**

- Más fácil de mantener
- Protección contra inyección SQL
- Código más legible
- Portabilidad entre diferentes bases de datos

**Ejemplo conceptual:**

En lugar de escribir:
```sql
SELECT name, email FROM res_partner WHERE id = 1;
```

Los desarrolladores escriben:
```python
partner = self.env['res.partner'].browse(1)
name = partner.name
email = partner.email
```

#### 2.3 Sistema de Modelos

**Modelos (Models)**

Define la estructura de datos:
- Campos (fields)
- Relaciones entre modelos
- Restricciones y validaciones
- Métodos de negocio

**Herencia de Modelos**

Los módulos pueden extender modelos existentes:
- Agregar campos nuevos
- Modificar comportamiento
- Agregar funcionalidades

**Ejemplo:**

El módulo "Ventas" extiende el modelo "Cliente" agregando campos específicos de ventas como "Equipo de Ventas" o "Oportunidades".

#### 2.4 Sistema de Permisos y Seguridad

**Control de Acceso Basado en Registros (Record Rules)**

Reglas que determinan qué registros puede ver cada usuario:

- Ejemplo: "Los vendedores solo pueden ver sus propios pedidos"
- Se aplican automáticamente a todas las consultas
- Protegen los datos a nivel de base de datos

**Permisos de Acceso**

- Lectura (Read)
- Escritura (Write)
- Crear (Create)
- Eliminar (Unlink)

**Grupos de Usuarios**

Organizan usuarios con permisos similares:
- Administrador
- Vendedor
- Contador
- Usuario de Inventario
- etc.

#### 2.5 Sistema de Trabajos Programados

**Cron Jobs**

Tareas que se ejecutan automáticamente en intervalos definidos:

- Verificación de correos entrantes
- Envío de recordatorios
- Actualización de estadísticas
- Generación de reportes programados
- Sincronización con sistemas externos

**Configuración:**

- Frecuencia (cada hora, diario, semanal, etc.)
- Hora específica de ejecución
- Condiciones de activación

### 3. Sistema de Módulos

#### 3.1 Estructura de un Módulo

Cada módulo contiene típicamente:

```
mi_modulo/
├── __manifest__.py      # Metadata del módulo
├── models/              # Modelos de datos
│   ├── __init__.py
│   └── mi_modelo.py
├── views/               # Vistas (interfaz)
│   └── mi_vista.xml
├── security/            # Permisos y reglas
│   ├── ir.model.access.csv
│   └── security_rules.xml
├── data/                # Datos iniciales
│   └── datos.xml
└── static/              # Archivos estáticos
    ├── description/
    │   └── icon.png
    └── src/
        └── js/
```

#### 3.2 Carga y Activación de Módulos

**Proceso de instalación:**

1. Odoo lee el manifiesto del módulo
2. Verifica dependencias
3. Carga modelos y vistas
4. Crea/actualiza tablas en la base de datos
5. Aplica datos iniciales
6. Configura permisos

**Actualización de módulos:**

Cuando se actualiza un módulo:
- Se actualizan modelos y campos
- Se migran datos si es necesario
- Se refrescan vistas
- Se actualizan permisos

### 4. Base de Datos

#### 4.1 Organización de Datos

**Tablas Principales:**

- `res_users`: Usuarios del sistema
- `res_partner`: Clientes, proveedores, contactos
- `res_company`: Empresas (multi-company)
- `ir_model`: Modelos definidos
- `ir_model_fields`: Campos de los modelos
- `ir_ui_view`: Vistas definidas
- `ir_model_access`: Permisos de acceso
- `ir_rule`: Reglas de acceso a registros

**Tablas de Aplicaciones:**

Cada aplicación agrega sus propias tablas:
- Ventas: `sale_order`, `sale_order_line`
- Inventario: `stock_picking`, `stock_move`
- Contabilidad: `account_move`, `account_payment`
- etc.

#### 4.2 Multi-Tenancy

Odoo soporta múltiples bases de datos en una misma instancia:

- Cada empresa puede tener su propia base de datos
- O múltiples empresas pueden compartir una base de datos (multi-company)
- La configuración determina el comportamiento

### 5. Sistema de Archivos

#### 5.1 Filestore

Almacena archivos adjuntos:
- Facturas PDF
- Imágenes de productos
- Documentos subidos
- Archivos generados

**Configuración:**

Puede almacenarse en:
- Sistema de archivos local
- Almacenamiento en la nube (S3, etc.)
- Base de datos (no recomendado para producción)

#### 5.2 Logs

Registros del sistema:
- Errores de aplicación
- Accesos de usuarios
- Operaciones del sistema
- Depuración

## Flujo de Datos Completo

### Ejemplo: Crear un Pedido de Venta

```
1. Usuario hace clic en "Nuevo Pedido"
   ↓
2. Navegador envía solicitud HTTP al servidor
   ↓
3. Controlador recibe la solicitud
   ↓
4. Servidor verifica autenticación y permisos
   ↓
5. ORM carga el modelo "sale.order"
   ↓
6. Sistema muestra formulario vacío al usuario
   ↓
7. Usuario completa el formulario y guarda
   ↓
8. Servidor valida los datos
   ↓
9. ORM crea nuevo registro en base de datos
   ↓
10. Base de datos confirma creación
   ↓
11. Se ejecutan métodos post-guardado (ej: calcular totales)
   ↓
12. Servidor envía respuesta al navegador
   ↓
13. Navegador muestra pedido creado
```

## Escalabilidad

### Escalado Vertical

Aumentar recursos del servidor:
- Más CPU
- Más RAM
- Discos más rápidos

**Ventajas:** Simple de implementar
**Desventajas:** Límites físicos, puede ser costoso

### Escalado Horizontal

Agregar más servidores:
- Múltiples servidores Odoo
- Base de datos replicada
- Balanceador de carga

**Ventajas:** Escalabilidad prácticamente ilimitada
**Desventajas:** Más complejo de configurar y mantener

## Alta Disponibilidad

### Estrategias Comunes

**Respaldos Regulares**

- Respaldos diarios de la base de datos
- Respaldos incrementales frecuentes
- Almacenamiento fuera del servidor

**Réplicas de Base de Datos**

- Base de datos maestra
- Bases de datos réplica para lectura
- Failover automático en caso de falla

**Balanceadores de Carga**

- Distribuyen solicitudes entre servidores
- Detectan servidores inactivos
- Garantizan servicio continuo

## Puntos Clave para Recordar

1. **Arquitectura en capas**: Presentación, Lógica, Datos
2. **ORM simplifica el acceso a datos** sin escribir SQL directo
3. **Módulos extienden funcionalidad** sin modificar el core
4. **Sistema de permisos multinivel** protege los datos
5. **Trabajos programados** automatizan tareas repetitivas
6. **Escalabilidad horizontal** permite crecer sin límites
7. **Alta disponibilidad** requiere configuración específica

## Siguiente Paso

Ahora que comprendes la arquitectura completa, el siguiente módulo cubrirá los procesos de instalación y despliegue de Odoo.

