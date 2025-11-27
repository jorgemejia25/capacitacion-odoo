# Conceptos Fundamentales de Odoo

## Introducción

Antes de profundizar en los aspectos técnicos, es importante comprender los conceptos fundamentales que forman la base de cómo funciona Odoo. Estos conceptos te ayudarán a entender mejor todo el sistema.

## Conceptos Clave

### 1. Base de Datos (Database)

**¿Qué es?**

Una base de datos es un sistema organizado para almacenar, gestionar y recuperar información. En Odoo, toda la información se guarda en una base de datos PostgreSQL.

**Por qué es importante:**

- Cada empresa tiene su propia base de datos o puede compartir una base de datos con múltiples empresas (multi-company)
- La base de datos contiene todas las transacciones, configuraciones y datos históricos
- Es el componente crítico que requiere respaldos regulares

**Ejemplo práctico:**

Imagina que tu empresa vende productos. Cada vez que creas un pedido, se guarda en la base de datos. La información del cliente, productos, precios, fechas, todo queda almacenado de forma estructurada y recuperable.

### 2. Instancia de Odoo

**¿Qué es?**

Una instancia es una instalación completa de Odoo funcionando en un servidor específico. Puedes tener múltiples instancias en el mismo servidor o en diferentes servidores.

**Tipos comunes de instancias:**

- **Producción**: La instancia que usan los usuarios diariamente
- **Pruebas/Testing**: Para probar cambios antes de aplicarlos en producción
- **Desarrollo**: Para desarrollar nuevas funcionalidades o personalizaciones

**Por qué es importante:**

- Separar producción de desarrollo evita afectar a usuarios finales
- Permite probar actualizaciones antes de aplicarlas
- Facilita el mantenimiento y desarrollo

### 3. Módulos (Modules)

**¿Qué es?**

Un módulo es la unidad básica de funcionalidad en Odoo. Cada módulo agrega características específicas al sistema.

**Características de un módulo:**

- Contiene modelos de datos (estructuras de información)
- Define vistas (interfaces de usuario)
- Incluye lógica de negocio
- Puede depender de otros módulos

**Ejemplo:**

El módulo "Ventas" te permite gestionar pedidos de clientes. El módulo "Inventario" gestiona productos y stock. Puedes instalar solo los módulos que necesitas.

### 4. Aplicaciones (Apps)

**¿Qué es?**

Una aplicación es un conjunto de módulos relacionados que trabajan juntos para resolver una necesidad empresarial específica.

**Diferencias con módulos:**

- Las aplicaciones son más amplias y completas
- Una aplicación puede incluir varios módulos
- Se instalan desde el App Store de Odoo
- Ejemplos: CRM, Ventas, Contabilidad, Recursos Humanos

**Flujo típico:**

1. Instalas una aplicación (ej: Ventas)
2. Odoo instala automáticamente los módulos necesarios
3. Los módulos se configuran y están listos para usar

### 5. Modelos de Datos (Models)

**¿Qué es?**

Un modelo define la estructura de una entidad en Odoo. Es como un "molde" que define qué información se puede guardar.

**Ejemplo:**

El modelo "Cliente" define que cada cliente tiene:
- Nombre
- Dirección
- Teléfono
- Email
- etc.

**Por qué es importante:**

- Cada registro en Odoo (un cliente específico, un pedido, etc.) sigue un modelo
- Los modelos definen qué campos son obligatorios
- Los modelos determinan cómo se relacionan los datos entre sí

### 6. Vistas (Views)

**¿Qué es?**

Una vista es la forma en que se presenta la información al usuario en la interfaz de Odoo.

**Tipos de vistas:**

- **Vista de lista**: Tabla con filas y columnas
- **Vista de formulario**: Formulario detallado de un registro
- **Vista de calendario**: Información en formato de calendario
- **Vista kanban**: Tarjetas organizadas en columnas
- **Vista gráfica**: Gráficos y estadísticas

**Ejemplo:**

Cuando ves una lista de clientes en una tabla, estás viendo una "vista de lista" del modelo "Cliente".

### 7. Registros (Records)

**¿Qué es?**

Un registro es una instancia específica de un modelo. Es como una fila en una tabla de base de datos.

**Ejemplo:**

Si el modelo es "Cliente", cada cliente individual (Juan Pérez, María García, etc.) es un registro.

### 8. Campos (Fields)

**¿Qué es?**

Un campo es un atributo de un modelo. Define una pieza específica de información.

**Tipos de campos comunes:**

- **Texto**: Nombres, descripciones
- **Número**: Cantidades, precios
- **Fecha**: Fechas de pedidos, fechas de vencimiento
- **Booleano**: Sí/No, Activo/Inactivo
- **Relacional**: Conexiones con otros modelos (ej: "Cliente" tiene campo "Dirección")

### 9. Usuarios y Permisos

**Usuarios:**

- Cada persona que accede a Odoo tiene una cuenta de usuario
- Los usuarios tienen credenciales (usuario y contraseña)
- Pueden estar activos o inactivos

**Grupos de Usuarios:**

- Conjunto de usuarios con permisos similares
- Ejemplos: "Vendedores", "Contadores", "Administradores"
- Facilita la gestión de permisos

**Permisos de Acceso:**

- **Lectura (Read)**: Puede ver la información
- **Escritura (Write)**: Puede crear y modificar
- **Crear (Create)**: Puede crear nuevos registros
- **Eliminar (Delete)**: Puede eliminar registros

### 10. Multi-Compañía (Multi-Company)

**¿Qué es?**

La funcionalidad multi-compañía permite gestionar múltiples empresas desde una misma instancia de Odoo.

**Cuándo se usa:**

- Empresas con múltiples subsidiarias
- Grupos empresariales
- Necesidad de separación contable y operativa

**Ventajas:**

- Una sola instalación para múltiples empresas
- Compartir algunos recursos (productos, proveedores)
- Separar datos contables y operativos

### 11. Multi-Idioma (Multi-Language)

**¿Qué es?**

Odoo puede funcionar en múltiples idiomas simultáneamente.

**Características:**

- Interfaz traducida
- Datos en diferentes idiomas
- Usuarios pueden elegir su idioma preferido
- Traducciones para clientes y proveedores

### 12. Entornos (Environments)

**¿Qué son?**

Diferentes configuraciones del mismo sistema para distintos propósitos.

**Tipos comunes:**

- **Producción**: Sistema en uso real
- **Pruebas/Staging**: Copia para pruebas
- **Desarrollo**: Para crear personalizaciones
- **Backup**: Respaldo para recuperación

**Importante:**

Nunca se deben hacer cambios directamente en producción sin probarlos primero en otro entorno.

## Diagrama de Conceptos Relacionados

```
┌─────────────────────────────────────────┐
│         Instancia de Odoo               │
│                                         │
│  ┌───────────────────────────────────┐ │
│  │      Aplicación: Ventas           │ │
│  │                                   │ │
│  │  ┌─────────────────────────────┐ │ │
│  │  │  Módulo: Pedidos            │ │ │
│  │  │                             │ │ │
│  │  │  Modelo: Pedido de Venta    │ │ │
│  │  │  ├─ Campo: Cliente          │ │ │
│  │  │  ├─ Campo: Fecha            │ │ │
│  │  │  ├─ Campo: Total            │ │ │
│  │  │  │                          │ │ │
│  │  │  Vista: Lista de Pedidos    │ │ │
│  │  │  Vista: Formulario Pedido   │ │ │
│  │  │                             │ │ │
│  │  │  Registro 1: Pedido #001    │ │ │
│  │  │  Registro 2: Pedido #002    │ │ │
│  │  │                             │ │ │
│  │  └─────────────────────────────┘ │ │
│  │                                   │ │
│  └───────────────────────────────────┘ │
│                                         │
│  Usuarios → Permisos → Acceso          │
│                                         │
└──────────────┬──────────────────────────┘
               │
               │ Almacenamiento
               │
┌──────────────▼──────────────────────────┐
│      Base de Datos PostgreSQL           │
│  (Contiene todos los datos)             │
└─────────────────────────────────────────┘
```

## Puntos Clave para Recordar

1. **Todo en Odoo está organizado en módulos** que se agrupan en aplicaciones
2. **Los modelos definen la estructura** de la información
3. **Los registros son datos específicos** que siguen un modelo
4. **Las vistas muestran los datos** de diferentes formas al usuario
5. **Los permisos controlan** quién puede hacer qué
6. **La base de datos** almacena toda la información
7. **Las instancias separadas** protegen el entorno de producción

## Siguiente Paso

Ahora que comprendes los conceptos fundamentales, el siguiente módulo explicará la arquitectura técnica detallada de Odoo: cómo está construido el sistema internamente.

