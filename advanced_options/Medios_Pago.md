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
```java
private static Map<String, String> getVRParameters() {
		Map<String, String> parameters = new HashMap<String, String>();
		parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF"); // API Key del comercio asignada por TodoPago 
		parameters.put(ElementNames.Merchant, "12345678");// Merchant o Nro de comercio asignado por TodoPago
		parameters.put(ElementNames.RequestKey, "e31d340c-690c-afe6-c478-fc1bef3fc157");  // RequestKey devuelto como respuesta del servicio SendAutorizeRequest
		return parameters;
}
	
Map<String, Object> h = tpc.voidRequest(getVRParameters());
```
También se puede llamar al método ```voidRequest``` de esta otra manera:
```java

private static Map<String, String> getVRParameters() {
		Map<String, String> parameters = new HashMap<String, String>();
		parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF"); // API Key del comercio asignada por TodoPago 
		parameters.put(ElementNames.Merchant, "12345678");// Merchant o Nro de comercio asignado por TodoPago
		parameters.put(ElementNames.AuthorizationKey, "6d2589f2-37e6-1334-7565-3dc19404480c"); // AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer
		return parameters;
}
	
Map<String, Object> h = tpc.voidRequest(getVRParameters());


**Respuesta del servicio:**

Campo         | Requerido   | Descripción                                      |Tipo de Dato  | Valores posibles / Ejemplo
--------------|-------------|--------------------------------------------------|--------------|----------------------------------
StatusCode    | Sí          |Número de identificación del motivo del resultado | Numérico     | 2011
StatusMessage | Sí          |Resultado de la devolución                        | Alfanumérico | Operación realizada correctamente

Si la operación fue realizada correctamente se informará con un código 2011 y un mensaje indicando el éxito de la operación.

```java
Map<String, Object> 
	{ StatusCode = 2011,
	  StatusMessage = Operación realizada correctamente }
```
