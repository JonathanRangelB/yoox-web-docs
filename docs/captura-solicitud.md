# Captura de Solicitudes de Préstamo

![Captura de solicitudes](img/2.avif)

## Introducción

El módulo de captura de solicitudes es la puerta de entrada para los nuevos clientes que desean obtener un préstamo con nuestra financiera. Esta sección del manual explica detalladamente el proceso para registrar correctamente una nueva solicitud en el sistema.

## Acceso al módulo

Para acceder al módulo de captura de solicitudes:

1. Inicie sesión en la plataforma con sus credenciales
2. En el menú principal, haga clic en la opción "Solicitudes"
3. Seleccione "Nueva Solicitud" en el submenú desplegable

![Acceso al módulo de solicitudes](img/6.avif)

## Formulario de captura

El formulario de captura está dividido en varias secciones para facilitar la organización de la información. A continuación, se describe cada una de ellas:

### 1. Información personal del solicitante

![Información personal](img/7.avif)

En esta sección se capturan los datos básicos del cliente:

| Campo | Descripción | Obligatorio |
|-------|-------------|-------------|
| Nombre completo | Nombres y apellidos del solicitante | Sí |
| Fecha de nacimiento | En formato DD/MM/AAAA | Sí |
| CURP | Clave Única de Registro de Población | Sí |
| RFC | Registro Federal de Contribuyentes | No |
| Estado civil | Soltero, casado, divorciado, etc. | Sí |
| Ocupación | Actividad laboral actual | Sí |

### 2. Datos de contacto

En esta sección se registra la información para ubicar y comunicarse con el cliente:

| Campo | Descripción | Obligatorio |
|-------|-------------|-------------|
| Dirección | Calle, número, colonia | Sí |
| Ciudad/Municipio | Localidad de residencia | Sí |
| Estado | Entidad federativa | Sí |
| Código postal | 5 dígitos | Sí |
| Teléfono fijo | Con clave lada | No |
| Teléfono móvil | 10 dígitos | Sí |
| Correo electrónico | Dirección de email válida | No |

> **Importante**: Verificar que el teléfono móvil sea correcto, ya que será el principal medio de comunicación con el cliente.

### 3. Información del préstamo

![Detalles del préstamo](img/3.avif)

En esta sección se establecen los parámetros del préstamo solicitado:

| Campo | Descripción | Obligatorio |
|-------|-------------|-------------|
| Monto solicitado | Cantidad en pesos mexicanos | Sí |
| Plazo | Número de semanas para pago | Sí |
| Destino del préstamo | Motivo del préstamo | Sí |
| Fecha de solicitud | Generada automáticamente | Automático |

### 4. Precálculo de pagos

El sistema genera automáticamente una tabla de amortización con los siguientes datos:

- Monto del pago semanal
- Número total de pagos
- Desglose de capital e intereses
- Fechas tentativas de pago

> **Nota**: Esta información es preliminar y está sujeta a la aprobación final del préstamo.

### 5. Información de avales

Para cada solicitud es necesario registrar al menos un aval:

| Campo | Descripción | Obligatorio |
|-------|-------------|-------------|
| Nombre completo del aval | Nombres y apellidos | Sí |
| Parentesco | Relación con el solicitante | Sí |
| Dirección | Domicilio completo | Sí |
| Teléfono | Número de contacto | Sí |

Se pueden agregar hasta 2 avales por solicitud haciendo clic en el botón "Agregar otro aval".

### 6. Documentación

En esta sección se registran los documentos proporcionados por el cliente:

- Identificación oficial (INE/IFE)
- Comprobante de domicilio
- Comprobante de ingresos
- Firma de solicitud

Para cada documento, se debe indicar si fue recibido marcando la casilla correspondiente.

## Guardar la solicitud

Una vez completados todos los campos requeridos:

1. Haga clic en el botón "Vista previa" para verificar la información
2. Si todo es correcto, haga clic en "Guardar solicitud"
3. El sistema generará un número de folio único para la solicitud
4. Se mostrará un mensaje de confirmación con el número de folio

![Confirmación de solicitud](img/5.avif)

## Modificación de una solicitud

Si necesita modificar algún dato después de guardar la solicitud:

1. En el menú principal, haga clic en "Solicitudes"
2. Seleccione "Consultar solicitudes"
3. Busque la solicitud por folio o nombre del cliente
4. Haga clic en el botón "Editar"
5. Realice los cambios necesarios
6. Guarde los cambios

> **Importante**: Las solicitudes solo pueden ser modificadas mientras se encuentren en estado "Pendiente de revisión".

## Preguntas frecuentes

**P: ¿Puedo guardar una solicitud incompleta?**  
R: Sí, puede guardar una solicitud como borrador haciendo clic en "Guardar borrador". Sin embargo, no se generará un folio hasta que se completen todos los campos obligatorios.

**P: ¿Cómo puedo recuperar un borrador?**  
R: En el menú "Solicitudes", seleccione "Borradores" para ver todas las solicitudes guardadas como borrador.

**P: ¿Qué hago si el cliente no tiene correo electrónico?**  
R: Este campo no es obligatorio, puede dejarlo en blanco.

**P: ¿Se puede cambiar el monto o plazo después de crear la solicitud?**  
R: Sí, estos campos pueden modificarse mientras la solicitud esté en estado "Pendiente de revisión".

## Consejos útiles

- Verifique la identidad del cliente mediante su identificación oficial
- Confirme que el domicilio en el comprobante coincida con el registrado
- Asegúrese de que los teléfonos proporcionados sean correctos realizando una llamada de prueba
- Explique claramente al cliente los términos y condiciones del préstamo
- Imprima una copia de la solicitud para firma del cliente
