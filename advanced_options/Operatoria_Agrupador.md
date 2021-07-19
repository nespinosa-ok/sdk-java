# Operatoria Agrupador

Mediante una única y simple adhesión, los vendedores acceden a todos los medios de pago que el Botón de pago ofrezca sin necesidad de contar con ningún tipo de contrato adicional con cada medio de pago. La funcionalidad “agrupador” de TodoPago, se ocupa de gestionar los acuerdos necesarios con todos los medios de pago a efectos de disponibilizarlos en el Botón.

Para acceder al servicio, los vendedores podrán adherirse en el sitio exclusivo de TodoPago o a través de su ejecutivo comercial. En estos procesos se generará el usuario y clave para este servicio.

Una vez adheridos se creará automáticamente una cuenta virtual, en la cual se acreditarán los fondos provenientes de los cobros realizados con la presente modalidad de pago.

<a name="secuencia"></a>
## Diagrama de secuencia
![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-003.jpg)



<a name="solicitudautorizacion"></a>
#### Solicitud de autorización
En este caso hay que llamar a sendAuthorizeRequest().
 		
```java		
Map<String, Object> a = tpc.sendAuthorizeRequest(parameters, getFraudControlParameters());		
```	

<ins><strong>Datos propios del comercio</strong></ins>		
El primer atributo parameters, debe ser un Map<String, String> con la siguiente estructura:	
	
<table>
  <tr>
    <th>Campo</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valores posibles / Ejemplo</th>
  </tr>
  <tr>
    <td><b>Security</b></td>
    <td>Sí</td>
    <td>API Keys sin PRISMA o TODOPAGO y sin espacio.</td>
    <td>Alfanumérico hasta 32 caracteres</td>
    <td>912EC803B2CE49E4A541068D495AB570</td>
  </tr>
  <tr>
    <td><b>Merchant</b></td>
    <td>Sí</td>
    <td>Nro. de Comercio (Merchant ID) provisto por TodoPago</td>
    <td>Numérico</td>
    <td>12345678</td>
  </tr>
  <tr>
    <td><b>URL_OK</b></td>
    <td>No</td>
    <td>URL a la que el comprador será dirigido cuando la compra resulte exitosa</td>
    <td>Alfanumérico hasta 256 caracteres</td>
    <td>http://susitio.com/payment/Ok</td>
  </tr>
  <tr>
    <td><b>URL_Error</b></td>
    <td>No</td>
    <td>URL a la que el comprador será dirigido cuando la compra no resulte exitosa</td>
    <td>Alfanumérico hasta 256 caracteres</td>
    <td>http://susitio.com/payment/Error</td>
  </tr>
  <tr>
    <td><b>OPERATIONID</b></td>
    <td>Sí</td>
    <td>Identificación de la transacción para el Comercio. Debe ser distinto para cada operación.</td>
    <td>Alfanumérico de 1 a 40 caracteres</td>
    <td>10000012</td>
  </tr>
  <tr>
    <td><b>CURRENCYCODE</b></td>
    <td>Sí</td>
    <td>Tipo de moneda de la operación. Sólo válido pesos argentinos (32)</td>
    <td>Numérico de dos posiciones</td>
    <td>32</td>
  </tr>
  <tr>
    <td><b>AMOUNT</b></td>
    <td>Sí</td>
    <td>Importe en Pesos de la transacción.</td>
    <td>Numérico con 9 dígitos con hasta 2 decimales 999999[.CC]
Usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>$125,38 -> 125.38</td>
  </tr>
  <tr>
    <td><b>EMAILCLIENTE</b></td>
    <td>Si</td>
    <td>El comercio deberá enviar a TodoPago el email del cliente. Esta dirección se utilizará para enviar el mail de confirmación de la compra al cliente</td>
    <td>Alfanumérico de hasta 80 caracteres.</td>
    <td>cliente@mail.com</td>
  </tr>
</table>

<ins><strong>Datos propios del comercio - Ejemplo</strong></ins>
		
```java
Map<String, String> parameters = new HashMap<String, String>();
	parameters.put(ElementNames.Session, "ABCDEF-1234-12221-FDE1-00000200");
	parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF");
	parameters.put(ElementNames.EncodingAlfanumérico de 1 a 8 caracteres.Method, "XML");
	parameters.put(ElementNames.Merchant, "12345678"); //dato fijo (número identificador del comercio)
	parameters.put(ElementNames.OperationID, "8000"); //número único que identifica la operación, generado por el comercio.
	parameters.put(ElementNames.CurrencyCode, "032"); //por el momento es el único tipo de moneda aceptada
	parameters.put(ElementNames.Amount, "1.00");
	parameters.put(ElementNames.UrlOK, "http,//someurl.com/ok/");
	parameters.put(ElementNames.UrlError, "http,//someurl/fail/");
	parameters.put(ElementNames.EMAILCLIENTE, "some@someurl.com");
```	

<ins><strong>Datos propios de la operación</strong></ins>
parameters debe ser un array con la siguiente estructura:
<table>
  <tr>
    <th><b>Campo</b></th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valores Posibles / Ejemplos</th>
  </tr>
  <tr>
    <td><b>MERCHANT</b></td>
    <td>Sí</td>
    <td>Nro. de Comercio (Merchant ID) provisto por TodoPago</td>
    <td>Numérico</td>
    <td>12345</td>
  </tr>
  <tr>
    <td><b>OPERATIONID</b></td>
    <td>Sí</td>
    <td>Identificación de la transacción para el Comercio. Debe ser distinto para cada operación.</td>
    <td>Alfanumérico de 1 a 40 caracteres</td>
    <td>10000012</td>
  </tr>
    <tr>
    <td><b>CURRENCYCODE</b></td>
    <td>Sí</td>
    <td>Tipo de moneda de la operación. Sólo válido pesos argentinos (32)</td>
    <td>Numérico de dos posiciones</td>
    <td>32</td>
  </tr>
  <tr>
    <td><b>AMOUNT</b></td>
    <td>Sí</td>
    <td>Importe en Pesos de la transacción.</td>
    <td>Numérico con 9 dígitos con hasta 2 decimales 999999[.CC]
Usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>$125,38 -> 125.38</td>
  </tr>
  <tr>
    <td><b>EMAILCLIENTE</b></td>
    <td>Si</td>
    <td>El comercio deberá enviar a TodoPago el email del cliente. Esta dirección se utilizará para enviar el mail de confirmación de la compra al cliente</td>
    <td>Alfanumérico de hasta 80 caracteres.</td>
    <td>cliente@mail.com</td>
  </tr>
</table>
