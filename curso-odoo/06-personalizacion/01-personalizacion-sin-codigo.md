# Personalización de Odoo sin Programar

## Introducción

Odoo ofrece muchas herramientas para personalizar el sistema sin necesidad de escribir código. Este documento explica cómo usar estas herramientas para adaptar Odoo a las necesidades específicas de tu empresa.

## Personalización de Vistas

### ¿Qué son las Vistas?

Las vistas son las pantallas que los usuarios ven cuando trabajan con datos. Puedes personalizarlas sin programar.

### Tipos de Vistas Personalizables

**Vista de Lista (Tree View):**
- Tablas con filas y columnas
- Personalizable: ordenar, filtrar, agrupar columnas

**Vista de Formulario (Form View):**
- Formulario detallado de un registro
- Personalizable: reorganizar campos, ocultar/mostrar campos

**Vista Kanban:**
- Tarjetas organizadas en columnas
- Personalizable: diseño de tarjetas, colores

**Vista de Calendario:**
- Vista de calendario
- Personalizable: qué mostrar, colores

### Personalizar Vista de Lista

**Proceso:**

1. Ir a la vista de lista (ej: Lista de Clientes)
2. Hacer clic en el menú de acciones (☰) en la esquina superior derecha
3. Seleccionar "Personalizar Vista"
4. Aparecerá el editor de personalización

**Opciones Disponibles:**

- **Agregar Columnas**: Seleccionar campos adicionales para mostrar
- **Eliminar Columnas**: Ocultar columnas no necesarias
- **Reordenar Columnas**: Arrastrar para cambiar orden
- **Ancho de Columnas**: Ajustar ancho
- **Guardar**: Guardar personalización para ti o para todos

**Tipos de Guardado:**

- **Solo para mí**: Solo tú verás los cambios
- **Para todos los usuarios**: Todos verán los cambios

### Personalizar Vista de Formulario

**Activar Modo de Edición:**

1. Abrir un formulario (ej: Formulario de Cliente)
2. Ir a: Configuración → Activar Modo de Desarrollo (si no está activo)
3. Hacer clic en el icono de edición en la vista

**Opciones Disponibles:**

- **Reorganizar Campos**: Arrastrar y soltar campos
- **Agregar Separadores**: Dividir secciones
- **Agregar Pestañas**: Organizar en pestañas
- **Ocultar Campos**: Ocultar campos no necesarios
- **Cambiar Etiquetas**: Modificar nombres de campos (limitado)

**Limitaciones:**

- No puedes agregar nuevos campos (requiere desarrollo)
- No puedes cambiar tipos de campos
- Algunas personalizaciones requieren modo de desarrollo

### Personalizar Vista Kanban

**Proceso:**

Similar a otras vistas:

1. Ir a vista Kanban
2. Activar modo de personalización
3. Modificar diseño de tarjetas
4. Cambiar colores y estilos
5. Guardar cambios

## Campos Personalizados

### Agregar Campos Personalizados

**Ubicación:** Configuración → Técnico → Base de Datos → Campos Personalizados

**Proceso:**

1. Clic en "Crear"
2. Seleccionar modelo (ej: Cliente, Producto, Pedido)
3. Configurar:
   - **Etiqueta**: Nombre del campo
   - **Nombre Técnico**: Generado automáticamente
   - **Tipo de Campo**: Texto, número, fecha, selección, etc.
   - **Requerido**: Si es obligatorio
   - **Solo Lectura**: Si no se puede editar
   - **Valor por Defecto**: Valor inicial

**Tipos de Campos Disponibles:**

- **Texto**: Texto libre
- **Texto Multilínea**: Texto largo
- **Entero**: Números enteros
- **Decimal**: Números con decimales
- **Booleano**: Sí/No
- **Fecha**: Solo fecha
- **Fecha y Hora**: Fecha con hora
- **Selección**: Lista desplegable
- **Muchos a Uno**: Relación con otro modelo
- **Uno a Muchos**: Relación inversa
- **Muchos a Muchos**: Relación múltiple

**Ejemplo Práctico:**

Agregar campo "Vendedor Asignado" a clientes:
1. Crear campo personalizado
2. Modelo: Cliente (res.partner)
3. Tipo: Muchos a Uno
4. Relación: Usuario (res.users)
5. Guardar
6. El campo aparecerá en formularios de cliente

### Gestión de Campos Personalizados

**Ver Todos los Campos:**

- Configuración → Técnico → Base de Datos → Campos Personalizados
- Muestra todos los campos creados

**Editar Campos:**

- Seleccionar campo
- Modificar configuración
- Guardar cambios

**Eliminar Campos:**

- **Advertencia**: Puede eliminar datos almacenados
- Solo eliminar si estás seguro
- Respaldar antes de eliminar

## Filtros y Búsquedas Personalizadas

### Crear Filtros

**Proceso:**

1. Ir a cualquier vista de lista
2. Aplicar filtros deseados
3. Clic en el icono de "Guardar Filtro" o "Favoritos"
4. Dar nombre al filtro
5. Seleccionar si es compartido o personal
6. Guardar

**Opciones:**

- **Personal**: Solo tú lo ves
- **Compartido**: Todos pueden usarlo
- **Por Defecto**: Se aplica automáticamente

### Búsquedas Personalizadas

**Crear Búsqueda:**

1. Usar barra de búsqueda
2. Aplicar filtros
3. Guardar como filtro favorito
4. Aparecerá en menú de búsqueda

**Gestionar Filtros:**

- Editar filtros guardados
- Eliminar filtros no usados
- Reorganizar favoritos

## Reglas de Acceso Personalizadas

### Reglas de Registro

**¿Qué son?**

Reglas que determinan qué registros puede ver cada usuario, sin programar.

**Ubicación:** Configuración → Técnico → Seguridad → Reglas de Registro

**Casos de Uso:**

- Vendedores solo ven sus propios pedidos
- Cada departamento ve solo sus documentos
- Usuarios ven solo registros de su región

**Crear Regla:**

1. Clic en "Crear"
2. Configurar:
   - **Nombre**: Nombre descriptivo
   - **Modelo**: A qué se aplica (ej: Pedidos)
   - **Dominio**: Condición (ej: usuario actual es vendedor)
   - **Grupos**: Qué grupos de usuarios se aplica
3. Guardar

**Ejemplo:**

Regla: "Vendedores ven solo sus pedidos"
- Modelo: Pedido de Venta
- Dominio: `[('user_id', '=', user.id)]`
- Grupos: Vendedores

### Permisos de Acceso

**Ubicación:** Configuración → Técnico → Seguridad → Control de Acceso

**Tipos:**

- **Lectura**: Ver registros
- **Escritura**: Modificar registros
- **Crear**: Crear nuevos registros
- **Eliminar**: Eliminar registros

**Gestionar Permisos:**

- Asignar a grupos de usuarios
- Crear grupos personalizados
- Configurar permisos por modelo

## Acciones Automatizadas

### ¿Qué son las Acciones Automatizadas?

Reglas que ejecutan acciones automáticamente cuando ocurren ciertos eventos, sin programar.

**Ubicación:** Configuración → Técnico → Automatización → Acciones Automatizadas

### Tipos de Acciones

**Acciones de Servidor:**

- Ejecutan acciones cuando se cumple una condición
- Ejemplo: Enviar email cuando se crea un pedido

**Crear Acción:**

1. Clic en "Crear"
2. Configurar:
   - **Nombre**: Nombre descriptivo
   - **Modelo**: A qué se aplica
   - **Condición**: Cuándo se ejecuta
   - **Acción**: Qué hacer
3. Guardar y activar

**Ejemplo Práctico:**

Acción: "Enviar email cuando se confirma pedido"
- Modelo: Pedido de Venta
- Condición: Estado = Confirmado
- Acción: Enviar email al cliente

**Limitaciones:**

- Acciones disponibles son predefinidas
- No puedes crear lógica compleja personalizada
- Para lógica avanzada se requiere desarrollo

## Trabajos Programados

### ¿Qué son los Trabajos Programados?

Tareas que se ejecutan automáticamente según un horario, sin intervención manual.

**Ubicación:** Configuración → Técnico → Automatización → Acciones Planificadas

### Crear Trabajo Programado

**Proceso:**

1. Clic en "Crear"
2. Configurar:
   - **Nombre**: Nombre descriptivo
   - **Modelo**: Qué modelo procesar
   - **Método**: Qué acción ejecutar
   - **Intervalo**: Cada cuánto ejecutar
   - **Hora**: A qué hora ejecutar
3. Guardar y activar

**Ejemplos:**

- Enviar recordatorios de facturas pendientes diariamente
- Actualizar estadísticas cada hora
- Limpiar datos antiguos semanalmente

**Configurar Horario:**

- **Cada**: Minutos, horas, días, semanas
- **Número**: Cada cuántos intervalos
- **Hora**: Hora específica del día

## Reportes Personalizados

### Personalizar Reportes Existentes

**Ubicación:** Configuración → Técnico → Reportes → Reportes

**Opciones:**

- Cambiar logo
- Modificar encabezados y pies de página
- Ajustar formato básico

**Limitaciones:**

- Personalización limitada sin desarrollo
- Para cambios significativos se requiere desarrollo

### Crear Reportes Básicos

**Herramientas de Reportes:**

- Exportar datos a Excel
- Crear vistas personalizadas que actúan como reportes
- Usar funcionalidades de exportación integradas

## Flujos de Trabajo (Workflows)

### Configurar Flujos

Muchas aplicaciones tienen flujos de trabajo configurables:

**Ejemplos:**

- Flujo de aprobación de pedidos
- Flujo de facturación
- Flujo de inventario

**Configuración:**

Generalmente en: Configuración de la Aplicación → Flujos de Trabajo

**Opciones:**

- Definir estados
- Configurar transiciones
- Establecer permisos por estado
- Configurar acciones automáticas

## Personalización de Interfaz

### Temas y Colores

**Ubicación:** Configuración → Personalización → Temas

**Opciones:**

- Cambiar colores principales
- Seleccionar tema (si hay múltiples)
- Personalizar colores corporativos

### Menú Principal

**Personalizar Menú:**

- Reorganizar elementos del menú
- Ocultar aplicaciones no usadas
- Crear accesos directos

**Ubicación:** Configuración → Personalización → Vistas de Menú

## Limitaciones de Personalización sin Código

### Qué NO Puedes Hacer

**Sin Programación:**

- Crear modelos de datos completamente nuevos
- Lógica de negocio compleja personalizada
- Integraciones profundas con sistemas externos
- Reportes completamente personalizados
- Funcionalidades completamente nuevas

**Cuando Necesitas Desarrollo:**

- Cambios significativos en funcionalidad
- Integraciones complejas
- Reportes muy personalizados
- Nuevas características no disponibles

## Mejores Prácticas

### Planificación

1. **Identificar necesidades** de personalización
2. **Priorizar** cambios más importantes
3. **Documentar** personalizaciones realizadas
4. **Probar** cambios antes de aplicar a todos

### Documentación

1. **Registrar** todas las personalizaciones
2. **Explicar** propósito de cada cambio
3. **Mantener** registro de quién hizo qué
4. **Versionar** cambios importantes

### Mantenimiento

1. **Revisar** personalizaciones periódicamente
2. **Actualizar** cuando cambien necesidades
3. **Limpiar** personalizaciones no usadas
4. **Verificar** después de actualizaciones de Odoo

## Puntos Clave para Recordar

1. **Muchas personalizaciones** son posibles sin programar
2. **Vistas** pueden personalizarse fácilmente
3. **Campos personalizados** extienden funcionalidad básica
4. **Filtros y búsquedas** mejoran la usabilidad
5. **Acciones automatizadas** ahorran tiempo
6. **Hay límites** a la personalización sin código
7. **Documentación** es esencial para mantenimiento

## Siguiente Paso

El siguiente documento explicará cómo administrar el sistema diariamente, incluyendo tareas rutinarias y gestión de usuarios.


