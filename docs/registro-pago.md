# Registro de Pagos

![Sistema de registro de pagos](img/4.avif)

## Índice

- [Introducción](#introducción)
- [Acceso al módulo](#acceso-al-módulo)
- [Proceso de registro de pagos](#proceso-de-registro-de-pagos)
- [Tipos de pagos](#tipos-de-pagos)
- [Comprobantes](#comprobantes)
- [Casos especiales](#casos-especiales)
- [Solución de problemas](#solución-de-problemas)
- [Referencia rápida](#referencia-rápida)

---

## Introducción

El módulo de registro de pagos permite al personal de la financiera documentar los abonos realizados por los clientes a sus préstamos. Esta funcionalidad es esencial para mantener actualizado el estado de las cuentas y generar los comprobantes correspondientes.

> **Objetivo principal:** Mantener un registro preciso y actualizado de todos los pagos recibidos, minimizando errores y garantizando la correcta aplicación de los abonos a los préstamos correspondientes.

---

## Acceso al módulo

Para acceder al módulo de registro de pagos, siga estos pasos:

1. Inicie sesión en la plataforma con sus credenciales
2. En el menú principal, haga clic en `Operaciones`
3. Seleccione la opción `Registro de Pagos`

*El sistema mostrará la pantalla principal del módulo de pagos.*

---

## Proceso de registro de pagos

### 1. Identificación del cliente y préstamo

```
Existen dos métodos para identificar un préstamo:

1. Por número de folio:
   - Ingrese el número de folio del préstamo en el campo correspondiente
   - Presione Enter o haga clic en "Buscar"

2. Por datos del cliente:
   - Ingrese el nombre, CURP o teléfono del cliente
   - Seleccione el préstamo correcto de la lista de resultados
```

### 2. Verificación de información del préstamo

Antes de registrar un pago, verifique la siguiente información:

- Nombre del cliente
- Saldo actual
- Monto de pago semanal
- Estatus del préstamo
- Fecha del último pago
- Pagos vencidos (si aplica)

### 3. Captura del pago

Para registrar un nuevo pago:

1. Haga clic en el botón `+ Nuevo Pago`
2. Complete los siguientes campos:

   | Campo | Descripción |
   |-------|-------------|
   | Fecha | Fecha en que se recibió el pago |
   | Monto | Cantidad recibida |
   | Método de pago | Efectivo, transferencia, etc. |
   | Referencia | Número de referencia (si aplica) |
   | Concepto | Regular, pago anticipado, liquidación |
   | Comentarios | Observaciones adicionales |

3. Verifique que toda la información sea correcta
4. Haga clic en `Registrar Pago`

### 4. Confirmación y emisión de comprobante

Después de registrar el pago:

1. El sistema mostrará un resumen del pago registrado
2. Confirme que los datos sean correctos
3. Haga clic en `Imprimir Comprobante`

> ***¡Importante!*** *Siempre entregue un comprobante al cliente como evidencia del pago realizado.*

---

## Tipos de pagos

El sistema permite registrar diferentes tipos de pagos:

### Pago regular

Es el pago semanal estándar según el plan de pagos.

*Ejemplo:*

```json
{
  "tipo": "regular",
  "monto": 500.00,
  "aplicacion": "Semana 4 de 20",
  "estatus": "Al corriente"
}
```

### Pago parcial

Cuando el cliente abona menos del monto semanal establecido.

*Ejemplo:*

```json
{
  "tipo": "parcial",
  "monto": 300.00,
  "aplicacion": "Abono a Semana 4 de 20",
  "estatus": "Pago incompleto",
  "pendiente": 200.00
}
```

### Pago múltiple

Cuando el cliente paga más de una semana a la vez.

*Ejemplo:*

```json
{
  "tipo": "multiple",
  "monto": 1000.00,
  "aplicacion": "Semanas 4 y 5 de 20",
  "estatus": "Adelantado"
}
```

### Pago anticipado

Cuando el cliente adelanta pagos futuros.

### Liquidación

Cuando el cliente paga la totalidad del saldo pendiente.

#### Ejemplo de aplicación de descuento por liquidación anticipada

```text
Si el préstamo tiene un saldo de $5,000 y faltan 10 semanas:
- Aplicar descuento del 5% sobre intereses pendientes
- Calcular nuevo saldo: $4,750
- Registrar como "Liquidación anticipada"
```

---

## Comprobantes

### Elementos del comprobante de pago

Todo comprobante debe incluir:

- Nombre de la financiera y logo
- Folio único del comprobante
- Fecha y hora del pago
- Nombre del cliente
- Número de préstamo
- Monto pagado (en números y letras)
- Método de pago
- Concepto del pago
- Saldo anterior y nuevo saldo
- Semanas pagadas
- Firma del cajero
- Sello de pagado

### Duplicados de comprobantes

Para imprimir un duplicado:

1. Busque el pago en el historial
2. Seleccione el pago específico
3. Haga clic en `Reimprimir Comprobante`
4. El sistema marcará el documento como "DUPLICADO"

---

## Casos especiales

### Pago con días de retraso

Cuando un cliente realiza un pago después de la fecha programada:

~~~
PROCEDIMIENTO PARA PAGOS CON RETRASO:

1. Verificar días de retraso
2. Calcular interés moratorio (2% por día de retraso)
3. Informar al cliente del monto adicional
4. Registrar el pago con el concepto "Pago con interés moratorio"
5. Detallar en comentarios los días de retraso
~~~

### Corrección de pagos

Si se cometió un error al registrar un pago:

1. Localice el pago en el historial
2. Haga clic en `Opciones` > `Solicitar corrección`
3. Complete el formulario de solicitud indicando:
   - Motivo de la corrección
   - Datos correctos
   - Justificación
4. Envíe la solicitud a su supervisor

> **Nota:** Solo los supervisores pueden aprobar correcciones de pagos ya registrados. Las solicitudes quedan registradas en el sistema para auditoría.

### Cancelación de pagos

La cancelación solo está permitida en los siguientes casos:

- Error en el monto
- Error en la identificación del cliente
- Pago duplicado

Requiere autorización de nivel gerencial.

---

## Solución de problemas

| Problema | Posible causa | Solución |
|----------|---------------|----------|
| No se encuentra el préstamo | Folio incorrecto o préstamo liquidado | Verificar el folio o buscar por nombre del cliente |
| Error al calcular nuevo saldo | Pagos no aplicados o en proceso | Actualizar la página y verificar historial de pagos |
| Impresora no responde | Problemas de conexión o configuración | Revisar conexión de impresora y reiniciar si es necesario |
| El sistema no acepta el monto | Inconsistencia con el saldo | Verificar el saldo actual y los pagos vencidos |

---

## Referencia rápida

### Atajos de teclado

- <kbd>F2</kbd> - Nueva búsqueda
- <kbd>F3</kbd> - Nuevo pago
- <kbd>F4</kbd> - Imprimir comprobante
- <kbd>F5</kbd> - Actualizar información
- <kbd>Alt</kbd> + <kbd>H</kbd> - Ver historial de pagos

### Códigos de estado de pago

| Código | Estado | Descripción |
|--------|--------|-------------|
| `OK` | Exitoso | Pago registrado correctamente |
| `PND` | Pendiente | Pago en proceso de verificación |
| `REV` | Revisión | Pago marcado para revisión |
| `CNL` | Cancelado | Pago cancelado |
| `ERR` | Error | Error en el registro del pago |

---

<div style="background-color: #f8f9fa; padding: 15px; border-radius: 5px; border-left: 5px solid #007bff;">
<h3>Recordatorio de mejores prácticas</h3>
<ol>
<li>Siempre verifique la identidad del cliente antes de registrar un pago</li>
<li>Confirme verbalmente el monto con el cliente</li>
<li>Cuente el efectivo en presencia del cliente (si aplica)</li>
<li>Entregue comprobante impreso</li>
<li>Actualice datos de contacto del cliente si han cambiado</li>
</ol>
</div>

---

*Última actualización: Marzo 2025*
