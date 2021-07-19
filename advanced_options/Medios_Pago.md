# Descubrimiento de Medios de Pago
La SDK cuenta con un método para obtener todos los medios de pago habilitados en TodoPago para la cuenta definida. Veamos el funcionamiento.<br>


![medios de pago](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-paymentmethods.jpg)


```java
Map<String, Object> g = tpc.discoverPaymentMethods();
```

<a name="devolucion"></a>
#### Devolución

![devolucion parcial](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-devolucion-total.jpg)

El SDK dispone de métodos para realizar la devolución, de una transacción realizada a traves de TodoPago.

Se debe llamar al método ```voidRequest``` de la siguiente manera:

Campo            | Requerido  | Descripción                                                              | Tipo de Dato | Valores posibles / Ejemplo
-----------------|------------|---------------------------------------------------------------------     |--------------|---------------------------
Security         | Sí         | API Key del comercio asignada por TodoPago                               | alfanumérico | 837BE68A892F06C17B944F344AEE8F5F
Merchant         | Sí         | Nro de comercio asignado por TodoPago                                    | numérico     | 12345
RequestKey       | No*        | RequestKey devuelto como respuesta del servicio SendAutorizeRequest      | alfanumérico | 6d2589f2-37e6-1334-7565-3dc19404480c
AuthorizationKey | No*        | AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer | alfanumérico | 4a2569a2-38e6-4589-1564-4480c3dc1940

_*Es requerida la presencia de sólo uno de estos 2 campos_

**Ejemplo:**

```php

$options = array(
	"Security" => "837BE68A892F06C17B944F344AEE8F5F", // API Key del comercio asignada por TodoPago
	"Merchant" => "12345", // Merchant o Nro de comercio asignado por TodoPago
	"RequestKey" => "6d2589f2-37e6-1334-7565-3dc19404480c" // RequestKey devuelto como respuesta del servicio SendAutorizeRequest
);
$resp = $todopago->voidRequest($options);
```
También se puede llamar al método ```voidRequest``` de esta otra manera:
```php

$options = array(
	"Security" => "837BE68A892F06C17B944F344AEE8F5F", // API Key del comercio asignada por TodoPago
	"Merchant" => "35", // Merchant o Nro de comercio asignado por TodoPago
	"AuthorizationKey" => "6d2589f2-37e6-1334-7565-3dc19404480c" // AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer
);
$resp = $todopago->voidRequest($options);
```


**Respuesta del servicio:**

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
