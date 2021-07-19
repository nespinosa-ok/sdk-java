# Devolución parcial

El SDK dispone de métodos para realizar la devolución parcial, de una transacción realizada a traves de TodoPago. Veamos el funcionamiento.


![devolucion parcial](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-devolucion-parcial.jpg)

_Nota: Para el caso de promociones con costo financiero, se deberá enviar el monto a devolver en base al valor original de la transacción y no del monto finalmente cobrado. TodoPago se encargará de devolver el porcentaje del costo financiero correspondiente a la devolución parcial._

Se debe llamar al método ```returnRequest``` de la siguiente manera:

Campo            | Requerido | Descripción                                                              | Tipo de Dato                                                                  | Valores posibles / Ejemplo
-----------      |------------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------
Security         | Sí         | API Key del comercio asignada por TodoPago                               | alfanumérico                                                                  | 837BE68A892F06C17B944F344AEE8F5F
Merchant         | Sí         | Nro de comercio asignado por TodoPago                                    | numérico                                                                      | 12345
RequestKey       | No*        | RequestKey devuelto como respuesta del servicio SendAutorizeRequest      | alfanumérico                                                                  | 6d2589f2-37e6-1334-7565-3dc19404480c
AuthorizationKey | No*        | AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer | alfanumérico                                                                  | 4a2569a2-38e6-4589-1564-4480c3dc1940
AMOUNT           | No         | Monto a devolver, si no se envía, se trata de una devolución total       | string usando . como separador decimal, incluyendo SIEMPRE 2 cifras decimales | 23.50

_*Es requerida la presencia de sólo uno de estos 2 campos_

**Ejemplo:**

```php

$options = array(
	"Security" => "837BE68A892F06C17B944F344AEE8F5F", // API Key del comercio asignada por TodoPago
	"Merchant" => "35", // Merchant o Nro de comercio asignado por TodoPago
	"RequestKey" => "6d2589f2-37e6-1334-7565-3dc19404480c" // RequestKey devuelto como respuesta del servicio SendAutorizeRequest
	"AMOUNT" => "23.50" // Opcional. Monto a devolver, si no se envía, se trata de una devolución total
);
$resp = $todopago->returnRequest($options);
```

También se puede llamar al método ```returnRequest``` de esta otra manera:
```php

$options = array(
	"Security" => "837BE68A892F06C17B944F344AEE8F5F", // API Key del comercio asignada por TodoPago
	"Merchant" => "35", // Merchant o Nro de comercio asignado por TodoPago
	"AuthorizationKey" => "6d2589f2-37e6-1334-7565-3dc19404480c" // AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer
	"AMOUNT" => "23.50" // Opcional. Monto a devolver, si no se envía, se trata de una devolución total
);
$resp = $todopago->returnRequest($options);
```

**Respuesta de servicio:**

Campo         | Requerido   | Descripción                                      |Tipo de Dato  | Valores posibles / Ejemplo
--------------|-------------|--------------------------------------------------|--------------|----------------------------------
StatusCode    | Sí          |Número de identificación del motivo del resultado | Numérico     | 2011
StatusMessage | Sí          |Resultado de la devolución                        | Alfanumérico | Operación realizada correctamente

Si la operación fue realizada correctamente se informará con un código 2011 y un mensaje indicando el éxito de la operación.

```php
array(
	"StatusCode" => 2011,
	"StatusMessage" => "Operación realizada correctamente",
);
```
