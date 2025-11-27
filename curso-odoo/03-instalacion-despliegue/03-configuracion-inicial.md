# Configuración Inicial del Sistema

## Introducción

Una vez que Odoo está instalado, necesitas configurarlo para tu empresa. Este documento explica los pasos esenciales de configuración inicial que debes realizar antes de comenzar a usar el sistema productivamente.

## Acceso Inicial al Sistema

### Primera Conexión

1. Abrir navegador web
2. Ir a la URL de tu instancia (ej: `http://localhost:8069` o `http://tu-servidor:8069`)
3. Deberías ver la pantalla de configuración inicial

### Crear la Primera Base de Datos

Si es la primera vez que accedes, verás un formulario para crear la base de datos:

**Campos Requeridos:**

- **Nombre de Base de Datos**: Nombre único para identificar tu base de datos
- **Email**: Correo electrónico del administrador
- **Contraseña**: Contraseña para el usuario administrador
- **Número de Teléfono**: Opcional
- **País**: Selecciona tu país (afecta configuraciones contables)
- **Datos de Demostración**: Marca esta opción si quieres datos de ejemplo para aprender

**Proceso:**

1. Completa todos los campos requeridos
2. Haz clic en "Crear base de datos"
3. Espera mientras Odoo crea la base de datos (puede tomar varios minutos)
4. Serás redirigido al dashboard una vez completado

## Configuración de la Empresa

### Acceder a Configuración

1. Ir a: **Aplicaciones** → buscar **Configuración**
2. O usar el menú: **Configuración** (ícono de engranaje)

### Información de la Empresa

**Ubicación:** Configuración → Empresas → Seleccionar tu empresa

**Información Básica:**

- **Nombre**: Nombre completo de la empresa
- **Dirección**: Dirección física
- **Teléfono**: Número de contacto
- **Email**: Correo electrónico principal
- **Sitio Web**: URL del sitio web (si aplica)
- **Logo**: Subir logo de la empresa

**Información Fiscal:**

- **Número de Identificación Fiscal**: Número de identificación tributaria
- **Moneda**: Moneda principal de la empresa
- **Formato de Fecha**: Formato preferido (ej: DD/MM/YYYY)
- **Zona Horaria**: Zona horaria de la empresa

**Configuración Contable:**

- **Plan Contable**: Seleccionar plan contable según tu país
- **Año Fiscal**: Fechas de inicio y fin del año fiscal
- **Código de Empresa**: Código interno si aplica

## Configuración de Usuarios y Permisos

### Crear Usuarios

**Ubicación:** Configuración → Usuarios y Compañías → Usuarios

**Crear Nuevo Usuario:**

1. Clic en "Crear"
2. Completar información:
   - **Nombre**: Nombre completo
   - **Email**: Correo electrónico (debe ser único)
   - **Teléfono**: Opcional
   - **Compañía**: Empresa asignada
   - **Idioma**: Idioma de interfaz preferido
   - **Zona Horaria**: Zona horaria del usuario

**Configuración de Acceso:**

- **Grupo de Usuarios**: Seleccionar grupos apropiados (más sobre esto después)
- **Permisos Especiales**: Si aplica
- **Contraseña**: Establecer contraseña inicial

### Grupos de Usuarios

Los grupos definen qué puede hacer cada usuario. Grupos comunes:

**Gestión:**

- **Administrador**: Acceso total al sistema
- **Acceso a Configuración**: Puede cambiar configuraciones

**Ventas:**

- **Usuario: Ventas**: Acceso básico a ventas
- **Vendedor**: Puede crear y gestionar pedidos
- **Manager: Ventas**: Acceso completo a ventas

**Compras:**

- **Usuario: Compras**: Acceso básico a compras
- **Comprador**: Puede crear órdenes de compra
- **Manager: Compras**: Acceso completo a compras

**Inventario:**

- **Usuario: Inventario**: Acceso básico
- **Encargado: Inventario**: Gestión completa de inventario
- **Manager: Inventario**: Acceso total

**Contabilidad:**

- **Usuario: Facturación**: Puede crear facturas
- **Contador**: Acceso a contabilidad
- **Manager: Contabilidad**: Acceso completo

**Recursos Humanos:**

- **Usuario: RRHH**: Acceso básico
- **Manager: RRHH**: Gestión completa

### Asignar Permisos

Para cada usuario:

1. Seleccionar usuario
2. Ir a la pestaña "Acceso Derechos"
3. Asignar grupos apropiados según su rol
4. Guardar

**Recomendación:**

- Asignar solo los permisos necesarios (principio de menor privilegio)
- Revisar permisos periódicamente
- Usar grupos en lugar de permisos individuales cuando sea posible

## Configuración de Módulos Principales

### Activar Modo de Desarrollo

Para acceso completo a configuraciones:

1. Ir a: Configuración → Activar Modo de Desarrollo
2. Aparecerán opciones adicionales de configuración

**Nota:** El modo de desarrollo muestra opciones técnicas avanzadas. Actívalo solo si necesitas configuraciones avanzadas.

### Instalar Aplicaciones Principales

**Ubicación:** Aplicaciones

**Aplicaciones Esenciales:**

1. **Ventas**
   - Gestión de pedidos y cotizaciones
   - Instalar desde Aplicaciones

2. **Compras**
   - Gestión de proveedores y órdenes de compra
   - Instalar desde Aplicaciones

3. **Inventario**
   - Control de almacenes y stock
   - Instalar desde Aplicaciones

4. **Contabilidad**
   - Facturación y contabilidad
   - Instalar desde Aplicaciones

5. **Facturación**
   - Gestión de facturas
   - Instalar desde Aplicaciones

**Proceso de Instalación:**

1. Ir a Aplicaciones
2. Buscar la aplicación
3. Clic en "Instalar"
4. Esperar mientras se instala
5. La aplicación aparecerá en el menú principal

### Configurar Aplicaciones Instaladas

Cada aplicación tiene su propia configuración. Acceder a:

**Configuración → [Nombre de la Aplicación]**

**Configuraciones Comunes:**

- **Números de Secuencia**: Formato de números de pedidos, facturas, etc.
- **Políticas por Defecto**: Comportamientos predeterminados
- **Notificaciones**: Configuración de emails automáticos
- **Flujos de Trabajo**: Procesos automatizados

## Configuración de Secuencias y Numeración

### ¿Qué son las Secuencias?

Las secuencias generan números automáticos para documentos (pedidos, facturas, etc.).

**Ubicación:** Configuración → Secuencias e Identificadores → Secuencias

### Configurar Secuencias Comunes

**Pedidos de Venta:**

1. Buscar "Pedido de Venta"
2. Editar secuencia
3. Configurar:
   - **Prefijo**: Ej: SO, PED, VENT
   - **Sufijo**: Opcional
   - **Relleno**: Número de dígitos (ej: 5 = 00001)
   - **Inicio**: Número inicial (ej: 1)

**Facturas:**

- Configurar formato similar
- Prefijo típico: FACT, INV, FAC

**Órdenes de Compra:**

- Prefijo típico: PO, OC, COMP

**Ejemplo de Formato:**

- Prefijo: "SO"
- Relleno: 5
- Resultado: SO00001, SO00002, SO00003...

## Configuración de Email

### Configurar Servidor de Email

**Ubicación:** Configuración → Técnico → Parámetros → Servidores de Email Saliente

**Configuración SMTP:**

1. Clic en "Crear"
2. Configurar:
   - **Descripción**: Nombre descriptivo
   - **SMTP Server**: Servidor SMTP (ej: smtp.gmail.com)
   - **SMTP Port**: Puerto (generalmente 587 para TLS)
   - **Conexión Segura**: TLS/SSL según servidor
   - **Usuario SMTP**: Email de envío
   - **Contraseña SMTP**: Contraseña del email

**Servidores Comunes:**

- **Gmail**: smtp.gmail.com, puerto 587, TLS
- **Outlook**: smtp-mail.outlook.com, puerto 587, TLS
- **Servidor propio**: Consultar con IT

### Configurar Email de la Empresa

**Ubicación:** Configuración → Empresas → Seleccionar empresa

Configurar:
- Email predeterminado de envío
- Nombre que aparece en emails enviados

## Configuración de Monedas

### Configurar Monedas

**Ubicación:** Configuración → Monedas

**Configuraciones:**

1. Moneda Principal: Ya configurada según país
2. Monedas Adicionales: Si manejas múltiples monedas
3. Tasas de Cambio: Configurar si necesitas conversión automática

### Tasas de Cambio Automáticas

**Ubicación:** Configuración → Configuración → Configuración Financiera

- Activar actualización automática de tasas de cambio
- Configurar frecuencia de actualización
- Seleccionar proveedor de tasas (si aplica)

## Configuración de Impuestos

### Configurar Impuestos Básicos

**Ubicación:** Contabilidad → Configuración → Impuestos

**Impuestos Comunes:**

1. **IVA/VAT**: Impuesto al valor agregado
2. **Impuesto de Venta**: Impuesto sobre ventas
3. **Impuesto de Compra**: Impuesto sobre compras

**Configuración:**

- **Nombre**: Nombre descriptivo
- **Tipo**: Porcentaje, fijo, etc.
- **Porcentaje**: Valor del impuesto
- **Tipo de Impuesto**: Sobre venta, sobre compra, ambos

### Reglas Fiscales

**Ubicación:** Contabilidad → Configuración → Reglas Fiscales

Las reglas fiscales determinan qué impuestos se aplican según:
- País del cliente
- Tipo de producto
- Condiciones específicas

## Configuración de Almacenes

### Crear Almacenes

**Ubicación:** Inventario → Configuración → Almacenes

**Configuración Básica:**

1. Clic en "Crear"
2. Configurar:
   - **Nombre**: Nombre del almacén
   - **Código**: Código corto (ej: ALM1, WH01)
   - **Dirección**: Ubicación física
   - **Compañía**: Empresa asociada

**Configuraciones Avanzadas:**

- **Rutas de Abastecimiento**: Cómo se reabastece
- **Reglas de Ubicación**: Dónde se almacenan productos
- **Valoración**: Método de valoración de inventario

## Verificaciones Post-Configuración

### Lista de Verificación

Después de la configuración inicial, verificar:

- [ ] Información de empresa completa
- [ ] Usuarios creados con permisos apropiados
- [ ] Aplicaciones principales instaladas
- [ ] Secuencias configuradas correctamente
- [ ] Email funcionando (enviar prueba)
- [ ] Monedas configuradas
- [ ] Impuestos básicos configurados
- [ ] Almacenes creados (si aplica)

### Prueba de Funcionalidad

Realizar pruebas básicas:

1. **Crear un pedido de prueba**: Verificar que la numeración funciona
2. **Enviar email de prueba**: Verificar configuración de email
3. **Acceder con diferentes usuarios**: Verificar permisos
4. **Generar reporte simple**: Verificar que todo funciona

## Configuraciones Adicionales Recomendadas

### Respaldos Automáticos

**Ubicación:** Configuración → Técnico → Automatización → Acciones Planificadas

Configurar respaldo automático de base de datos (si no se hace a nivel de servidor).

### Seguridad

**Ubicación:** Configuración → Técnico → Seguridad

- Configurar política de contraseñas
- Configurar sesiones (tiempo de expiración)
- Revisar reglas de acceso

### Personalización de Interfaz

**Ubicación:** Configuración → Personalización

- Configurar colores corporativos
- Personalizar menú principal
- Configurar dashboard por defecto

## Puntos Clave para Recordar

1. **Configuración inicial** es fundamental antes de uso productivo
2. **Información de empresa** afecta todas las funcionalidades
3. **Permisos** deben configurarse cuidadosamente
4. **Secuencias** definen la numeración de documentos
5. **Email** debe probarse antes de uso productivo
6. **Verificaciones** previenen problemas futuros
7. **Documentación** de configuraciones ayuda en el mantenimiento

## Siguiente Paso

Ahora que el sistema está configurado inicialmente, el siguiente módulo explicará en detalle la gestión de módulos y aplicaciones en Odoo.


