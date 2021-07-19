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
```

<ins><strong>Datos propios de la operación</strong></ins>
$optionsSAR_operacion debe ser un array con la siguiente estructura:
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


__*__ _Importante:_ Tambíen deben mandarse los datos correspondientes a [Prevención de Fraude](#datosadicionales)

**Respuesta**

<table><tr>
<td>Campo</td><td>Requerido</td><td>Descripción</td><td>Tipo de Dato</td><td>Valores posibles / Ejemplo</td></tr>
<tr><td>**StatusCode**</td><td>Sí</td><td>Código de estado o valor de retorno del Servicio</td><td>Numérico de 5 posiciones</td><td> <ul><li>-1 -> OK</li><li>otro ->Error</li></ul></td></tr>
<tr><td>**StatusMessage**</td><td>Sí</td><td>Descripción del códgo de retorno o estado del servicio</td><td>Alfanumérico hasta 256</td><td>Ejemplo: Solicitud de Autorización Registrada</td></tr>
<tr><td>**URL_Request**</td><td>Sí</td><td>Url del formulario de pago</td><td>URL</td><td>https://forms.todopago.com.ar/formulario/commands?command=formulario&m=t7d3938c9-f7b1-4ee9-e76b-9cc84f73fe81</td></tr>
<tr><td>**RequestKey**</td><td>No</td><td>Identificador Privado del Requerimiento obtenido en la respuesta de la operación SendAuthorizeRequest. Nunca debe ser expuesto hacia el Web Browser. Solo será utilizado entre el ecommerce y TodoPago</td><td>Alfanumérico hasta 48 caracteres</td><td>8496472a-8c87-e35b-dcf2-94d5e31eb12f</td></tr>
<tr><td>**PublicRequestKey**</td><td>No</td><td>Identificador Público del Requerimiento obenido en la respuesta de la operación SendAuthorizeRequest</td><td>Alfanumérico de hasta 48 caracteres</td><td>t7d3938c9-f7b1-4ee9-e76b-9cc84f73fe81</td></tr>
</table>


**Ejemplo de respuesta**

```java	
Map<String, Object> 	
	{ StatusCode = -1,
	  PublicRequestKey = te0b9bba5-cff9-173a-20da-b9bc8a389ac7, 
	  URL_Request = https://developers.todopago.com.ar/formulario/commands?command=formulario&m=te0b9bba5-cff9-173a-20da-b9bc8a389ac7, 
	  StatusMessage = Solicitud de Autorizacion Registrada, 
	  RequestKey = ff0f6434-a2ab-e87f-3ece-37f7081e671a }
```

La **url_request** es donde está hosteado el formulario de pago y donde hay que redireccionar al usuario, una vez realizado el pago según el éxito o fracaso del mismo, el formulario redireccionará a una de las 2 URLs seteadas en **$optionsSAR_comercio** ([URL_OK](#url_ok), en caso de éxito o [URL_ERROR](#url_error), en caso de que por algún motivo el formulario rechace el pago)

Si, por ejemplo, se pasa mal el <strong>MerchantID</strong> se obtendrá la siguiente respuesta:
```java
Map<String, Object> 
	{ StatusCode = 702,
	  StatusMessage = ERROR: Cuenta Inexistente,
	  PublicRequestKey = null, 
	  URL_Request = null, 
	  RequestKey = null }
```
