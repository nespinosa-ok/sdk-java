<a name="inicio"></a>		
Todo Pago - módulo SDK-JAVA para conexión con gateway de pago
=======

 + [Instalación](#instalacion)
 	+ [Versiones de Java soportadas](#Versionesdejavasoportadas)
 	+ [Generalidades](#general)
  + [Uso](#uso)		
    + [Inicializar la clase correspondiente al conector (TodoPago\Sdk)](#initconector)
    + [Solicitud de autorización](#solicitudautorizacion)
    + [Confirmación de transacción](#confirmatransaccion)
    + [Ejemplo](#ejemplo)
    + [Modo test](#test)
 + [Datos adicionales para prevención de fraude](#datosadicionales) 
    + [Datos de referencia](#datosreferencia) 
 + [Opciones adicionales](#opcionesadicionales)
    + [Rango de cuotas](#coutas)
    + [Split de pago](#split)
    + [Filtrado de Medios de pago](#filtromp)
    + [Tiempo de vida de la transacción](#timeout)
 + [Características](#caracteristicas)
    + [Estado de la operación](#status)
    + [Consulta de operaciones por rango de tiempo](#statusdate)
    + [Descubrimiento de Medios de Pago](#discover)
    + [Devolucion](#devolucion)
    + [Devolucion parcial](#devolucionparcial)
    + [Formulario hibrido](#formhidrido)
    + [Obtener Credenciales](#credenciales)
    + [Estado del Servicio](#echo)
    + [Máximo de cuotas a mostrar en formulario](#maxcuotas)
    + [Minimo de cuotas a mostrar en formulario](#mincuotas)
    + [Tiempo de vida del formulario](#timeout)    
    + [Filtrado de medios de pago](#filtromp)    
 + [Diagrama de secuencia](#secuencia)
 + [Tablas de referencia](#tablareferencia)		
 + [Tabla de errores](#codigoerrores)
 + [Agregar el proyecto a Eclipse](#eclipse)
 
 
<a name="instalacion"></a>		
## Instalación
Se debe descargar la última versión del SDK desde el botón Download ZIP del branch master.

En caso de utilizar Maven, se puede agregar el jar TodoPago.jar al repositorio local de Maven utilizando la siguinte linea de comando:
```
mvn install:install-file -Dfile=<path-to-file> -DpomFile=<path-to-pomfile>
``` 

Una vez hecho esto se puede agregar la dependencia a este paquete desde el pom.xml
```xml
<dependency>
	<groupId>com.ar.todopago</groupId>
	<artifactId>sdk-java</artifactId>
	<version>1.5.0</version>
</dependency>
```
De ser necesario agregar la siguiente dependencia requerida por TodoPago desde el pom.xml
 ```xml
 <dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
     <version>20090211</version>
  </dependency>
```

Una vez descargado se deben hacer los siguientes imports.
```java
import ar.com.todopago.api.ElementNames;
import ar.com.todopago.api.TodoPagoConector;
import ar.com.todopago.api.model.*;
import ar.com.todopago.api.exceptions.*;
```

El Ejemplo es un proyecto hecho en maven, con un pom.xml que incluye la configuracion para importar y exportar las librerias requeridas.

Para agregar el proyecto de ejemplo a Eclipse, una vez descargado, por consola ir hasta su carpeta y ejecutar las siguientes lineas:
```
mvn clean install -Dwtpversion=2.0
mvn eclipse:clean eclipse:eclipse -Dwtpversion=2.0
```
Luego, importar el proyecto normalmente en Eclipse.

<a name="Versionesdejavasoportadas"></a>   
#### 1. Versiones de Java soportadas
La versi&oacute;n implementada de la SDK, esta testeada para versiones desde Java 5 en adelante con JAX-WS.

<a name="general"></a>
#### 2. Generalidades
Esta versión soporta únicamente pago en moneda nacional argentina (CURRENCYCODE = 32).

[<sub>Volver a inicio</sub>](#inicio)	

<a name="uso"></a>		
## Uso	

<a name="initconector"></a>
#### Inicializar la clase correspondiente al conector (TodoPago\Sdk).

Si se cuenta con los http header suministrados por Todo Pago

- Crear un Map con dichos http header
```java
Map<String, List<String>> auth = new HashMap<String, List<String>>();
auth.put(ElementNames.Authorization, Collections.singletonList("PRISMA f3d8b72c94ab4a06be2ef7c95490f7d3"));
```

- Crear una instancia de la clase TodoPago
```java		
TodoPagoConector tpc = new TodoPagoConector(TodoPagoConector.developerEndpoint, auth);//End Point developer y http_header provisto por TODO PAGO	
```	

Si se cuenta el con User y Password del login en TodoPago

- Crear una instancia de la clase TodoPago
```java		
TodoPagoConector tpc = new TodoPagoConector(TodoPagoConector.developerEndpoint);//End Point developer
```	
- Obtener las credenciales a traves  del m&eacute;todo getCredentials de TodoPago. Ver [Obtener Credenciales](#credenciales)

<br/>
<a name="agrupador"></a>
### Operatoria Agrupador

Mediante una única y simple adhesión, los vendedores acceden a todos los medios de pago que el Botón de pago ofrezca sin necesidad de contar con ningún tipo de contrato adicional con cada medio de pago. La funcionalidad “agrupador” de TodoPago, se ocupa de gestionar los acuerdos necesarios con todos los medios de pago a efectos de disponibilizarlos en el Botón.

Para acceder al servicio, los vendedores podrán adherirse en el sitio exclusivo de TodoPago o a través de su ejecutivo comercial. En estos procesos se generará el usuario y clave para este servicio.

Una vez adheridos se creará automáticamente una cuenta virtual, en la cual se acreditarán los fondos provenientes de los cobros realizados con la presente modalidad de pago.

<br/>
<a name="secuencia"></a>
## Diagrama de secuencia

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-003.jpg)

<br/>
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

<ins><strong>Datos prevención de fraude: </strong></ins>El segundo atributo getFraudControlParameter().

Ver [Datos adicionales para prevención de fraude](#datosadicionales)  

**Respuesta**

<table><tr>
<td>Campo</td><td>Requerido</td><td>Descripción</td><td>Tipo de Dato</td><td>Valores posibles / Ejemplo</td></tr>
<tr><td>**StatusCode**</td><td>Sí</td><td>Código de estado o valor de retorno del Servicio</td><td>Numérico de 5 posiciones</td><td> <ul><li>-1 -> OK</li><li>otro ->Error</li></ul></td></tr>
<tr><td>**StatusMessage**</td><td>Sí</td><td>Descripción del códgo de retorno o estado del servicio</td><td>Alfanumérico hasta 256</td><td>Ejemplo: Solicitud de Autorización Registrada</td></tr>
<tr><td>**URL_Request**</td><td>Sí</td><td>Url del formulario de pago</td><td>URL</td><td>https://forms.todopago.com.ar/formulario/commands?command=formulario&m=t7d3938c9-f7b1-4ee9-e76b-9cc84f73fe81</td></tr>
<tr><td>**RequestKey**</td><td>No</td><td>Identificador Privado del Requerimiento obtenido en la respuesta de la operación SendAuthorizeRequest. Nunca debe ser expuesto hacia el Web Browser. Solo será utilizado entre el ecommerce y TodoPago</td><td>Alfanumérico hasta 48 caracteres</td><td>8496472a-8c87-e35b-dcf2-94d5e31eb12f</td></tr>
<tr><td>**PublicRequestKey**</td><td>No</td><td>Identificador Público del Requerimiento obenido en la respuesta de la operación SendAuthorizeRequest</td><td>Alfanumérico de hasta 48 caracteres</td><td>t7d3938c9-f7b1-4ee9-e76b-9cc84f73fe81</td></tr>
</table>


```java	
Map<String, Object> 	
	{ StatusCode = -1,
	  PublicRequestKey = te0b9bba5-cff9-173a-20da-b9bc8a389ac7, 
	  URL_Request = https://developers.todopago.com.ar/formulario/commands?command=formulario&m=te0b9bba5-cff9-173a-20da-b9bc8a389ac7, 
	  StatusMessage = Solicitud de Autorizacion Registrada, 
	  RequestKey = ff0f6434-a2ab-e87f-3ece-37f7081e671a }
```

La **URL_Request** es donde está hosteado el formulario de pago y donde hay que redireccionar al usuario, una vez realizado el pago según el éxito o fracaso del mismo, el formulario redireccionará a una de las 2 URLs seteadas en **parameters** ([URL_OK](#url_ok), en caso de éxito o [URL_ERROR](Alfanumérico de 1 a 8 caracteres.#url_error), en caso de que por algún motivo el formulario rechace el pago)

Si, por ejemplo, se pasa mal el <strong>MerchantID</strong> se obtendrá la siguiente respuesta:

```java
Map<String, Object> 
	{ StatusCode = 702,
	  StatusMessage = ERROR: Cuenta Inexistente,
	  PublicRequestKey = null, 
	  URL_Request = null, 
	  RequestKey = null }
```

<a name="confirmatransaccion"></a>
#### Confirmación de transacción.
En este caso hay que llamar a **getAuthorizeAnswer()**, enviando como parámetro un Map<String, String> como se describe a continuación.

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
    <td>No</td>
    <td>Token  de Seguridad Generado en el Portal de TodoPago</td>
    <td>Alfanumérico hasta 32 caracteres</td>
    <td>1234567890ABCDEF1234567890ABCDEF</td>
  </tr>
  <tr>
    <td><b>Merchant</b></td>
    <td>Si</td>
    <td>Nro. de Comercio (Merchant ID) provisto por TodoPago</td>
    <td>Alfanumérico de  8 caracteres</td>
    <td>12345678</td>
  </tr>
  <tr>
    <td><b>RequestKey</b></td>
    <td>Si</td>
    <td>Identificador Privado del Requerimiento obtenido en la respuesta de la operación SendAuthorizeRequest . Nunca debe ser expuesto hacia el Web Browser. Solo será utilizado entre el ecommerce y TodoPago</td>
    <td>Alfanumérico hasta 48 caracteres</td>
    <td>8496472a-8c87-e35b-dcf2-94d5e31eb12f</td>
  </tr>
  <tr>
    <td><b>AnswerKey</b></td>
    <td>Sí</td>
    <td>Identificador Público de la Respuesta. Recibido según el formulario utilizado, en la url de redirección hacia el ecommerce, o como propiedad retornada en el callback del formulario híbrido.</td>
    <td>Alfanumérico hasta 48 caracteres</td>
    <td>8496472a-8c87-e35b-dcf2-94d5e31eb12f</td>
  </tr>
</table>



```java	
Map<String, String> parameters = new HashMap<String, String>();		
	parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF"); // Token de seguridad, provisto por TODO PAGO. MANDATORIO.
	parameters.put(ElementNames.Merchant, "12345678");
	parameters.put(ElementNames.RequestKey, "0123-1234-2345-3456-4567-5678-6789");
	parameters.put(ElementNames.AnswerKey, "1111-2222-3333-4444-5555-6666-7777"); // *Importante

Map<String, Object> b = tpc.getAuthorizeAnswer(parameters);	
```

Se deben guardar y recuperar los valores de los campos <strong>RequestKey</strong> y <strong>AnswerKey</strong>.

El parámetro <strong>RequestKey</strong> es siempre distinto y debe ser persistido de alguna forma cuando el comprador es redirigido al formulario de pagos.

<ins><strong>Importante</strong></ins> El campo **AnswerKey** se adiciona  en la redirección que se realiza a alguna de las direcciones ( URL ) epecificadas en el  servicio **SendAurhorizationRequest**, esto sucede cuando la transacción ya fue resuelta y es necesario regresar al site para finalizar la transacción de pago, también se adiciona el campo Order, el cual tendrá el contenido enviado en el campo **OPERATIONID**. Para nuestro ejemplo: <strong>http://susitio.com/paydtodopago/ok?Order=27398173292187&Answer=1111-2222-3333-4444-5555-6666-7777</strong>		

<table>
<tr><td>Campo</td><td>Requerido</td><td>Descripción</td><td>Tipo de Dato</td><td>Valores posibles / Ejemplo</td></tr>
<tr><td>**StatusCode** </td><td>Si</td><td>Código de estado o valor de retorno del Servicio</td><td>Numèrico de 5 posiciones</td><td> <b>-1 -> OK<br> 0 a 99999 o vacío -> error</b></td></tr>
<tr><td>**StatusMessage**</td><td>Si</td><td>Descripción del código de retorno o estado del servicio</td><td>Alfanumérico hasta 256</td><td>Ejemplo: "APROBADA"</td></tr>
<tr><td>**AuthorizationKey**</td><td>No</td><td>Identificador Privado de la Respuesta</td><td>Alfanumérico hasta 256 caracteres</td><td>Ejemplo: "9c2f0109-e585-0776-d3d0-f934ed50ccd4"</td></tr>
<tr><td>**EncodingMethod**</td><td>No</td><td>Especifica el tipo codificación que se usa para los datos de la transacciones de pagos</td><td>Alfanumérico hasta 16 caracteres</td><td>XML</td></tr>
<tr><td>**Payload**</td><td>No</td><td>Documento codificado  en el  formato especificado en el campo EncodingMethod  el cual contiene los datos de la transacción ejecutada</td><td>Alfanumérico hasta 2048 caracteres</td><td>-</td></tr></table>
.

El campo o elemento Payload es utilizado para retornar los datos de la respuesta de la transacción. En la siguiente Tabla se muestran los valores enviados en el campo _Answer_ de dicho elemento. (El otro campo presente, de nombre _Request_ contiene información enviada en el requerimiento del _GetAuthorizeAnswer_)

<table>
<tr><td>Campo</td><td>Requerido</td><td>Descripción</td><td>Tipo de Dato</td><td>Valores posibles / Ejemplo</td></tr>
<tr><td>**DATETIME**</td><td>Si</td><td>Fecha y Hora de la Transacción</td><td>Fecha y Hora. aaaammddTHHMMSSZ La hora se expresa en formato 24 hs.</td><td>Ejemplo: "2017-07-28T15:54:14Z"</td></tr>
<tr><td>**RESULTCODE**</td><td>Si</td><td>Código de estado o valor de retorno del Servicio</td><td>Numérico de 5 posiciones</td><td> <b>-1 -> OK<br> 0 a 99999 o vacío -> error</b></td></tr>
<tr><td>**RESULTMESSAGE**</td><td>Si</td><td>Descripción del código de retorno o estado del servicio</td><td>Alfanumérico hasta 256</td><td>Ejemplo: "APROBADA"</td></tr>
<tr><td>**CURRENCYNAME**</td><td>No</td><td>Nombre de la Moneda</td><td>Alfanumérico</td><td>Ejemplo: "Peso Argentino"</td></tr>
<tr><td>**PAYMENTMETHODNAME**</td><td>Sí </td><td>Medio de pago usado para la operación</td><td>Alfanumérico</td><td>Ejemplo: "VISA"</td></tr>
<tr><td>**TICKETNUMBER** </td><td>No</td><td>Número de Ticket o Voucher</td><td>Numérico de Hasta 4 dígitos</td><td>Ejemplo: 7057</td></tr>
<tr><td>**CARDNUMBERVISIBLE**</td><td>No</td><td>Número de Tarjeta, enmascarado según normativas nacionales, regionales o globales</td><td></td><td>Ejemplo: "450799XXXXXX0010"</td></tr>
<tr><td>**AUTHORIZATIONCODE**</td><td>No</td><td>Código de Autorización</td><td>Alfanumérico de hasta 8 caracteres</td><td>Ejemplo: "014158"</td></tr>
<tr><td>**INSTALLMENTPAYMENTS**</td><td>No</td><td>Cantidad de cuotas elegidas para la operación</td><td>Numérico</td><td> Ejemplo: 03</td></tr>
<tr><td>**AMOUNTBUYER**</td><td>Si</td><td>Monto final (incluyendo Costo Financiero) pagado por el comprador</td><td>Decimal</td><td> Ejemplo: 129.68</td></tr>
<tr><td>**CFT**</td><td>Si</td><td>CFT de la promoción aplicada.</td><td>Decimal</td><td> Ejemplo: 0.00</td></tr>
<tr><td>**TEA**</td><td>Si</td><td>TEA de la promoción aplicada.</td><td>Decimal</td><td> Ejemplo: 22.00</td></tr>
</table>

```java		
Map<String, Object>		
	{ StatusCode = -1, 		
      StatusMessage = APROBADA,		
	  AuthorizationKey = 1294-329E-F2FD-1AD8-3614-1218-2693-1378,		
      EncodingMethod = XML,		
      Payload = { Answer = { DATETIME = 2014/08/11 15:24:38,		
						     RESULTCODE = -1,		
							 RESULTMESSAGE = APROBADA,		
							 CURRENCYNAME = Pesos,		
							 PAYMENTMETHODNAME = VISA,		
							 TICKETNUMBER = 12,		
							 CARDNUMBERVISIBLE = 450799******4905,		
							 AUTHORIZATIONCODE = TEST38,
							 INSTALLMENTPAYMENTS = 6,
							 TEA = 22.00,
							 CFT = 0.00
							 }, 
				{ Request = { MERCHANT = 12345678,
						      OPERATIONID = ABCDEF-1234-12221-FDE1-00000012,
							  AMOUNT = 1.00,
							  CURRENCYCODE = 032}
				}
	}
	  
```		
Este método devuelve el resumen de los datos de la transacción

Si se pasa mal el <strong>AnswerKey</strong> o el <strong>RequestKey</strong> se verá el siguiente rechazo:

```java
Map<String, Object> 
	{ StatusCode = 404,
	  StatusMessage = ERROR: Transaccion Inexistente,
          AuthorizationKey = null,
          EncodingMethod = null }
```

<a name="ejemplo"></a>      
#### Ejemplo
Existe un ejemplo ejecutable en https://github.com/TodoPago/SDK-Java/blob/master/Ejemplo/src/main/java/com/ar/todopago/ejemplo/SampleUI.java que muestra los resultados de los métodos principales del SDK y su correcta implementacion.<br>

Existe un ejemplo en la carpeta https://github.com/TodoPago/sdk-java/tree/master/src/test que muestra los resultados de los métodos principales del SDK.	

<a name="test"></a>      
#### Modo Test

El SDK-JAVA permite trabajar con los ambiente de desarrollo y de produccion de Todo Pago.<br>
El ambiente se debe instanciar como se indica a continuacion.

```java
TodoPagoConector tpc = new TodoPagoConector(TodoPagoConector.developerEndpoint, getAuthorization());

private static Map<String, List<String>> getAuthorization() {
	Map<String, List<String>> parameters = new HashMap<String, List<String>>();
	parameters.put(ElementNames.Authorization, Collections.singletonList("PRISMA f3d8b72c94ab4a06be2ef7c95490f7d3"));
	return parameters;
}
```

[<sub>Volver a inicio</sub>](#inicio)
<br>

<a name="datosadicionales"></a>		
## Datos adicionales para control de fraude		
Los datos adicionales para control de fraude son **obligatorios**, de lo contrario baja el score de la transacción.

Los campos marcados como **condicionales** afectan al score negativamente si no son enviados, pero no son mandatorios o bloqueantes.

```java		
private static Map<String, String> getFraudControlParameters() {

	Map<String, String> parameters = new HashMap<String, String>();		
	parameters.put("CSBTCITY", "Villa General Belgrano"); //Ciudad de facturación, MANDATORIO.		
	parameters.put("CSBTCOUNTRY", "AR");//País de facturación. MANDATORIO. Código ISO. (http://apps.cybersource.com/library/documentation/sbc/quickref/countries_alpha_list.pdf)	
	parameters.put("CSBTCUSTOMERID", "453458"); //Identificador del usuario al que se le emite la factura. MANDATORIO. No puede contener un correo electrónico.		
	parameters.put(CSBTIPADDRESS", "192.0.0.4"); //IP de la PC del comprador. MANDATORIO.		
	parameters.put(CSBTEMAIL", "some@someurl.com"); //Mail del usuario al que se le emite la factura. MANDATORIO.
	parameters.put(CSBTFIRSTNAME", "Juan");//Nombre del usuario al que se le emite la factura. MANDATORIO.		
	parameters.put(CSBTLASTNAME", "Perez");//Apellido del usuario al que se le emite la factura. MANDATORIO.
	parameters.put(CSBTPHONENUMBER", "541160913988");//Teléfono del usuario al que se le emite la factura. No utilizar guiones, puntos o espacios. Incluir código de país. MANDATORIO.		
	parameters.put(CSBTPOSTALCODE", "1010");//Código Postal de la dirección de facturación. MANDATORIO.	
	parameters.put(CSBTSTATE", "B");//Provincia de la dirección de facturación. MANDATORIO. Ver tabla anexa de provincias.	
	parameters.put(CSBTSTREET1", "Some Street 2153");//Domicilio de facturación (calle y nro). MANDATORIO.			
	parameters.put("CSBTSTREET2", "Piso 8");//Complemento del domicilio. (piso, departamento). NO MANDATORIO.
	parameters.put(CSPTCURRENCY", "ARS");//Moneda. MANDATORIO.		
	parameters.put(CSPTGRANDTOTALAMOUNT", "125.38");//Con decimales opcional usando el puntos como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales. MANDATORIO.(Ejemplos:$125,38-> 125.38 $12-> 12 o 12.00)		
	parameters.put(CSMDD7", "");// Fecha registro comprador(num Dias). NO MANDATORIO.		
	parameters.put(CSMDD8", "Y"); //Usuario Guest? (Y/N). En caso de ser Y, el campo CSMDD9 no deberá enviarse. NO MANDATORIO.		
	parameters.put(CSMDD9", "");//Customer password Hash: criptograma asociado al password del comprador final. NO MANDATORIO.		
	parameters.put(CSMDD10", "");//Histórica de compras del comprador (Num transacciones). NO MANDATORIO.	
	parameters.put(CSMDD11", "");//Customer Cell Phone. NO MANDATORIO.		

	//Retail
	parameters.put("CSSTCITY", "Villa General Belgrano");//Ciudad de enví­o de la orden. MANDATORIO.	
	parameters.put("CSSTCOUNTRY", "AR");//País de envío de la orden. MANDATORIO.	
	parameters.put("CSSTEMAIL", "some@someurl.com");//Mail del destinatario, MANDATORIO.			
	parameters.put("CSSTFIRSTNAME", "Juan");//Nombre del destinatario. MANDATORIO.		
	parameters.put("CSSTLASTNAME", "Perez");//Apellido del destinatario. MANDATORIO.		
	parameters.put("CSSTPHONENUMBER", "541160913988");//Número de teléfono del destinatario. MANDATORIO.	
	parameters.put("CSSTPOSTALCODE", "1010");//Código postal del domicilio de envío. MANDATORIO.		
	parameters.put("CSSTSTATE", "B");//Provincia de envío. MANDATORIO. Son de 1 caracter			
	parameters.put("CSSTSTREET1", "Some Street 2153");//Domicilio de envío. MANDATORIO.		
	parameters.put("CSSTSTREET2", "Piso 8");//Complemento del domicilio. (piso, departamento). NO MANDATORIO.
	parameters.put("CSMDD12", "");//Shipping DeadLine (Num Dias). NO MADATORIO.		
	parameters.put("CSMDD13", "");//Método de Despacho. NO MANDATORIO.		
	parameters.put("CSMDD14", "");//Customer requires Tax Bill ? (Y/N). NO MANDATORIO.		
	parameters.put("CSMDD15", "");//Customer Loyality Number. NO MANDATORIO. 		
	parameters.put("CSMDD16", "");//Promotional / Coupon Code. NO MANDATORIO. 		
	
	//datos a enviar por cada producto, los valores deben estar separado con #:		
	parameters.put("CSITPRODUCTCODE", "electronic_good");//Código de producto. MANDATORIO. Valores posibles(adult_content;coupon;default;electronic_good;electronic_software;gift_certificate;handling_only;service;shipping_and_handling;shipping_only;subscription)	
	parameters.put("CSITPRODUCTDESCRIPTION", "Test Prd Description");//Descripción del producto. MANDATORIO.	
	parameters.put("CSITPRODUCTNAME", "TestPrd");//Nombre del producto. CONDICIONAL.	
	parameters.put("CSITPRODUCTSKU", "SKU1234");//Código identificador del producto. MANDATORIO.		
	parameters.put("CSITTOTALAMOUNT", "10.01");//CSITTOTALAMOUNT=CSITUNITPRICE*CSITQUANTITY "999999[.CC]" Con decimales opcional usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales. MANDATORIO.		
	parameters.put("CSITQUANTITY", "1");//Cantidad del producto. CONDICIONAL.		
	parameters.put("CSITUNITPRICE", "10.01");//Formato Idem CSITTOTALAMOUNT. CONDICIONAL.	
}	
```


<a name="datosreferencia"></a>    
#### Datos de referencia   

<table>
<tr><th>Nombre del campo</th><th>Required/Optional</th><th>Data Type</th><th>Mínimo</th><th>Comentarios</th></tr>
<tr><td>CSBTCITY</td><td>Required</td><td>String (60)</td><td>6</td><td>Ciudad / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr>
<tr><td>CSBTCOUNTRY</td><td>Required</td><td>String (2)</td><td>1</td><td>Código ISO</td></tr>
<tr><td> CSBTCUSTOMERID</td><td>Required</td><td>String (50)</td><td>1</td><td>Identificador del usuario unico logueado al portal (No puede ser una direccion de email)</td></tr>
<tr><td> CSBTEMAIL</td><td>Required</td><td>String (100)</td><td>1</td><td>Correo electrónico del comprador con formato válido (solo letras (a-z,A-Z), números, puntos, guiones y sin espacios).</td></tr>
<tr><td>CSBTFIRSTNAME</td><td>Required</td><td>String (60)</td><td>6</td><td>Nombre del tarjeta habiente / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr>
<tr><td>CSBTIPADDRESS</td><td>Required</td><td>String (15)</td><td>1</td><td>"End Customer´s IP address, such as 10.1.27.63, reported by your Web server via socket information."</td></tr>
<tr><td> CSBTLASTNAME</td><td>Required</td><td>String (60)</td><td>6</td><td>Apellido del tarjeta habiente / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr> 
<tr><td>CSBTPHONENUMBER</td><td>Required</td><td>String (30)</td><td>6</td><td>Número de telefono, acepta números, espacios y/o paréntesis. *Ejemplo *(011) 4567 8910</td></tr>
<tr><td>CSBTPOSTALCODE</td><td>Required</td><td>String (10)</td><td>1</td><td>Codigo Postal</td></tr> 
<tr><td>CSBTSTATE</td><td>Required</td><td>String (2)</td><td>1</td><td>Estado (Si el country = US, el campo se valida para un estado valido en USA)</td></tr>
<tr><td>CSBTSTREET1</td><td>Required</td><td>String (60)</td><td>6</td><td>Calle Numero interior Numero Exterior / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr> 
<tr><td>CSBTSTREET2</td><td>Optional</td><td>String (60)</td><td></td><td>Barrio</td></tr>
<tr><td>CSITPRODUCTCODE</td><td>Conditional</td><td>String (255)</td><td></td><td></td> </tr>
<tr><td>CSITPRODUCTDESCRIPTION</td><td>Conditional</td><td>String (255)</td><td></td><td>Descripción general del producto</td></tr> 
<tr><td>CSITPRODUCTNAME</td><td>Conditional</td><td>String (255)</td><td></td><td>Nombre en catalogo del producto</td></tr>
<tr><td>CSITPRODUCTSKU</td><td>Conditional</td><td>String (255)</td><td></td><td>SKU en catalogo</td></tr> 
<tr><td>CSITQUANTITY</td><td>Conditional</td><td>Integer (10)</td><td></td><td>Cantidad productos del mismo tipo agregados al carrito</td></tr> 
<tr><td>CSITTOTALAMOUNT</td><td>Conditional</td><td></td><td></td><td>"Precio total = Precio unitario * quantity / CSITTOTALAMOUNT = CSITUNITPRICE * CSITQUANTITY ""999999.CC"" Es mandatorio informar los decimales, usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales."</td></tr>
<tr><td>CSITUNITPRICE</td><td>Conditional</td><td>String (15)</td><td></td><td>"Precio Unitaro del producto / ""999999.CC"" Es mandatorio informar los decimales, usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales."</td></tr> 
<tr><td>CSPTCURRENCY</td><td>Required</td><td>String (5)</td><td>1</td><td>Currencies=>'032'(Peso Argentino)</td></tr> 
<tr><td>CSPTGRANDTOTALAMOUNT</td><td>Required</td><td>Decimal (15)</td><td>1</td><td>"Cantidad total de la transaccion./""999999.CC"" Con decimales obligatorios, usando el puntos como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales."</td></tr> 
<tr><td>CSSTCITY</td><td>Required</td><td>String (60)</td><td>6</td><td>Ciudad / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td>
<tr><td> CSSTCOUNTRY</td><td>Required</td><td>String (2)</td><td>1</td><td>Código ISO</td></tr>
<tr><td>CSSTEMAIL</td><td>Required</td><td>String (100)</td><td>1</td><td>Correo electrónico del comprador con formato válido (solo letras (a-z,A-Z), números, puntos, guiones y sin espacios).</td></tr> 
<tr><td>CSSTFIRSTNAME</td><td>Required</td><td>String (60)</td><td>6</td><td>Nombre del tarjeta habiente / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr> 
<tr><td>CSSTLASTNAME</td><td>Required</td><td>String (60)</td><td>6</td><td>Apellido del tarjetahabiente / Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr> 
<tr><td>CSSTPHONENUMBER</td><td>Required</td><td>String (30)</td><td>6</td><td>"Número de telefono. Cuidar el hecho que por default algunos comercios envían ""54"", contando entonces con 2 de los 6 caracteres requeridos. Acepta números, espacios y/o paréntesis. *Ejemplo *(011) 4567 8910"</td></tr> 
<tr><td>CSSTPOSTALCODE</td><td>Required</td><td>String (10)</td><td>1</td><td>Código Postal</td></tr>
<tr><td>CSSTSTATE</td><td>Required</td><td>String (2)</td><td>1</td><td>Estado (Si el country = US, el campo se valida para un estado v lido en USA)</td><tr> 
<tr><td>CSSTSTREET1</td><td>Required</td><td>String (60)</td><td>6</td><td>Calle Numero interior Numero Exterior / Para los casos que no son de envío a domicilio, jam s enviar la dirección propia del comercio o correo donde se retire la mercadería, en ese caso replicar los datos de facturación. Acepta acentos comunes, puntos, guion medio y alto, letras con acentos invertidos, ñ / No acepta guion bajo.</td></tr>
<tr><td> CSSTSTREET2</td><td>Optional</td><td>String (60)</td><td></td><td>Barrio</td></tr> 
<tr><td>CSMDD1 </td><td>Required</td><td>String (255)</td><td>1</td><td>Incluir numero de comercio proveniente del campo NROCOMERCIO del API DECIDIR</td></tr> 
<tr><td>CSMDD2</td><td>Required</td><td>String (255)</td><td>1</td><td>Incluir el nombre del comercio, Decidir puede obtener este dato del portal de configuracion de comercios</td></tr>
<tr><td> CSMDD3</td><td>Required (Catalogo)</td><td>String (255)</td><td>1</td><td>"Valores ejemplo: (retail, digital goods, services, travel, ticketing) Es recomendable que el API de decidir fije opciones seleccionables y no sean de captura libre para el comercio"</td></tr> 
<tr><td>CSMDD4</td><td>Optional (Catalogo)</td><td>String (255)</td><td></td><td>"Valores ejemplo: (Visa, Master Card, Tarjeta Shopping, Banelco...) Es recomendable que el API de decidir fije opciones seleccionables y no sean de captura libre para el comercio. Se tienen que incluir todos los medios de pago aceptados"</td></tr>
<tr><td> CSMDD5</td><td>Optional</td><td>String (255)</td><td></td><td>Valor numerico que detalle el numero de cuotas</td></tr>
<tr><td>CSMDD6</td><td>Optional (Catalogo)</td><td>String (255)</td><td></td><td>"Valores ejemplo: (Web, Call Center, Mobile, Kiosko) Es recomendable que el API de decidir fije opciones seleccionables y no sean de captura libre para el comercio."</td></tr> 
<tr><td>CSMDD7</td><td>Optional</td><td>String (255)</td><td></td><td>Numero de dias que tiene registrado un cliente en el portal del comercio.</td></tr>
<tr><td>CSMDD8</td><td>Optional</td><td>String (255)</td><td></td><td>Valor Boleano para indicar si el usuario esta comprando como invitado en la pagina del comercio. Valores posibles (S/N)</td></tr> 
<tr><td>CSMDD9</td><td>Optional</td><td>String (255)</td><td></td><td>Valor del password del usuario registrado en el portal del comercio. Incluir el valor en hash</td></tr> 
<tr><td>CSMDD10</td><td>Optional</td><td>String (255)</td><td></td><td>Conteo de transacciones realizadas por el mismo usuario registrado en el portal del comercio</td></tr>
<tr><td>CSMDD11</td><td>Optional</td><td>String (255)</td><td></td><td>Incluir numero de telefono adicional del comprador</td></tr> 
<tr><td>CSMDD12</td><td>Optional</td><td>String (255)</td><td></td><td>Numero de dias que tiene el comercio para hacer la entrega</td></tr> 
<tr><td>CSMDD13</td><td>Optional (Catalogo)</td><td>String (255)</td><td></td><td>"Valores ejemplo: (domicilio, click and collect, carrier) Es recomendable que el API de decidir fije opciones seleccionables y no sean de captura libre para el comercio."</td></tr> 
<tr><td>CSMDD14</td><td>Optional</td><td>String (255)</td><td></td><td>Valor booleano para identificar si el cliente requiere un comprobante fiscal o no S / N</td></tr>
<tr><td>CSMDD15</td><td>Optional</td><td>String (255)</td><td></td><td>Incluir numero de cliente frecuente</td></tr>
<tr><td>CSMDD16</td><td>Optional</td><td>String (255)</td><td></td><td>Incluir numero de cupon de descuento</td></tr>
<tr><td>CSMDD35</td><td>Conditional (Transaccion con Visa)</td><td>String (255)</td><td></td><td>Tipo de documento solicitado por el comercio al cliente</td></tr>
<tr><td>CSMDD36</td><td>Conditional (Transaccion con Visa)</td><td>String (255)</td><td></td><td>Numero de documento solicitado por el comercio al cliente</td></tr>
<tr><td>CSMDD37</td><td>Conditional (Transaccion con Visa)</td><td>String (255)</td><td></td><td>Numero de puerta</td></tr>
<tr><td> CSMDD38</td><td>onditional (Transaccion con Visa)</td><td>String (255)</td><td></td><td>Fecha de nacimiento del comprador, dato solicitado por el comercio. DECIDIR tiene el formato exacto de como se debe de capturar</td></tr>
<tr><td>CSMDD39</td><td>Conditional (Transaccion con Visa)</td><td>String (255)</td><td></td><td>Valor numero correspondiente a la validacion de cada uno de los datos anteriores ejemplo: 1012</td></tr>
<tr><td> CSMDD40</td><td>Optional</td><td>String(1)</td><td></td><td>"Valor para identificar si la transaccion ha sido reportada como fraude por parte del emisor. Incluir el parametro con valor = S Este parametro lo genera decidir a partir de la respuesta del emisor. En caso de una transaccion aceptada por el emisor o con rechazo diferente a fraude, NO INCLUIR"</td></tr> 
<tr><td>CSMDD41</td><td>Optional</td><td>String(1)</td><td></td><td>Datos proporcionado por DECIDIR en el form. De pago. Valores posibles S/N</td></tr> 
<tr><td>CSMDD42</td><td>Optional</td><td>String(1)</td><td></td><td>Datos proporcionado por DECIDIR en el form. De pago. Valores posibles S/N</td></tr>
<tr><td>CSMDD80</td><td>Required </td><td>Integer (20)</td><td></td><td>Número de cuenta del vendedor</td></tr>
<tr><td> CSMDD81</td><td>Required </td><td>String(255)</td><td></td><td>Mail del vendedor en TP</td></tr>
<tr><td> CSMDD82</td><td>Required </td><td>Integer (6)</td><td></td><td>Rubro asignado por el analista de riesgos de Back Office</td></tr>
<tr><td>CSMDD83</td><td>Required </td><td>Integer (2)</td><td></td><td>Antigüedad de la cuenta vendedor</td></tr> 
<tr><td>CSMDD84</td><td>Required </td><td>String (15)</td><td></td><td>Consumidor Final / Profesional / Empresa</td></tr> 
<tr><td>CSMDD85</td><td>Required </td><td>Integer(1)</td><td></td><td>0 (No se le pidió) / 1 (Se le pidió y se validó) / 2 (Uso Futuro)</td></tr>
<tr><td> CSMDD86</td><td>Requerido (para Billetera)</td><td>Integer(20)</td><td></td><td>Número de cuenta del comprador</td></tr>
<tr><td>CSMDD87</td><td>Requerido (para Billetera)</td><td>Integer (3)</td><td></td><td>Antigüedad de la cuenta comprador (Meses)</td></tr> 
<tr><td>CSMDD88</td><td>Requerido (para Billetera)</td><td>Integer (3)</td><td></td><td>Cantidad de tarjetas Habilitadas de la Billetera</td></tr> 
<tr><td>CSMDD89</td><td>Requerido (para Billetera)</td><td>Integer (2)</td><td></td><td>Nivel de Riesgo asignado al Medio de Pago que Utiliza</td></tr>
</table>

[<sub>Volver a inicio</sub>](#inicio)
<br>

<a name="caracteristicas"></a>
## Características

<a name="status"></a>
#### Estado de la operación

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-status.jpg)

La SDK cuenta con un método para consultar el status de la transacción desde la misma SDK. El método se utiliza de la siguiente manera:

<table>
  <tr>
    <th>Campo</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valores posibles / Ejemplo</th>
  </tr>
  <tr>
    <td><b>MERCHANT</b></td>
    <td>Sí</td>
    <td>Código de comercio o cuenta provisto por TodoPago</td>
    <td>Alfanumérico de 8 caracteres</td>
    <td>12345678</td>
  </tr>
  <tr>
    <td><b>OPERATIONID</b></td>
    <td>Sí</td>
    <td>Identificación de la transacción para el Comercio. Debe ser distinto para cada operación.</td>
    <td>Alfanumérico de 1 a 40 caracteres.</td>
    <td>141120084707</td>
  </tr>
</table>


```java

private static Map<String, String> getSParameters(){
	Map<String, String> parameters = new HashMap<String, String>();
	parameters.put("Merchant", "12345678");
	parameters.put("OperationID", "8000");
	return parameters;
}	

Map<String, Object> d = tpc.getStatus(getSParameters());// Merchant es el id site y OperationID es el id operación que se envió en el array a través del método sendAuthorizeRequest() 
```
El siguiente método retornará el status actual de la transacción en Todopago.



<table>
  <tr>
    <th>Campo</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valores posibles / Ejemplo</th>
  </tr>
  <tr>
  <td><b>RESULTCODE</b></td>
  <td>Sí</td>
  <td>Número identificador del estado en el que se encuentra la transacción</td>
  <td>Numérico</td>
  <td>Ejemplo: -1</td>
  </tr>
  <tr>
  <td><b>RESULTMESSAGE</b></td>
  <td>Sí</td>
  <td>Describe el estado en el que se encuentra la transacción</td>
  <td>Alfanumérico</td>
  <td>Ejemplo: "APROBADA"</td>
  </tr>
  <tr>
    <td><b>CUSTOMERID</b></td>
    <td>Sí</td>
    <td>Identificación, provista por el sistema de comercio, del cliente que realizó la transacción.</td>
    <td>Alfanumérico de 1 a 8 caracteres.</td>
    <td>42</td>
  </tr>
  <tr>
    <td><b>DATETIME</b></td>
    <td>No</td>
    <td></td>
    <td></td>
    <td>2015-05-13T14:11:38.287+00:00</td>
  </tr>
  <tr>
    <td><b>OPERATIONID</b></td>
    <td>Sí</td>
    <td>Identificación de la transacción para el Comercio. Debe ser distinto para cada operación.</td>
    <td>Alfanumérico de 1 a 40 caracteres.</td>
    <td>141120084707</td>
  </tr>
  <tr>
    <td><b>CURRENCYCODE</b></td>
    <td>Sí</td>
    <td>Código de moneda utilizado en la transacción. Por el momento solo 32 (Pesos)</td>
    <td>Numérico</td>
    <td>32</td>
  </tr>
  <tr>
    <td><b>AMOUNT</b></td>
    <td>Sí</td>
    <td>Importe original en Pesos de la transacción.</td>
    <td>Numérico con 9 dígitos con hasta 2 decimales 999999[.CC]
Usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>$125,38 -> 125.38 <br />$12 -> 12.00</td>
  </tr>
  <tr>
    <td><b>AMOUNTBUYER</b></td>
    <td>Sí</td>
    <td>Importe final en Pesos de la transacción.</td>
    <td>Numérico con 9 dígitos con hasta 2 decimales 999999[.CC]
Usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>$125,38 -> 125.38 <br />$12 -> 12.00</td>
  </tr>
  <tr>
    <td><b>TYPE</b></td>
    <td>Sí</td>
    <td>Tipo de Operación, en el caso del GetStatus siempre será *compra_online*</td>
    <td>Alfanumérico</td>
    <td>compra_online</td>
  </tr>
  <tr>
    <td><b>INSTALLMENTPAYMENTS</b></td>
    <td>No</td>
    <td>Código de autorización generado por el medio de pago</td>
    <td>Decimal de hasta dos dígitos.</td>
    <td>01, 02, 06, 12, etc.</td>
  </tr>
  <tr>
  <td><b>CUSTOMEREMAIL</b></td>
  <td>Sí</td>
  <td>Mail del usuario al que se le emite la factura</td>
  <td>Alfanumérico de 100 caracteres.</td>
  <td>Ejemplo: cosme@fulanito.com</td>
  </tr>
  <tr>
  <td><b>IDENTIFICATIONTYPE</b></td>
  <td>No</td>
  <td>Tipo de documento</td>
  <td></td>
  <td>DNI<br />CI<br />LE<br />LC</td>
  </tr>
  <tr>
    <td><b>IDENTIFICATION</b></td>
    <td>No</td>
    <td>Número de documento</td>
    <td>Numérico</td>
    <td>Ejemplo: 1987234</td>
  </tr>
  <tr>
    <td><b>CARDNUMBER</b></td>
    <td>No</td>
    <td>Número de Tarjeta, enmascarado según normativas nacionales</td>
    <td>alfanumérico de 20 caracteres</td>
    <td>Ejemplo: "450799XXXXXX0010"</td>
  </tr>
  <tr>
  <td><b>CARDHOLDERNAME</b></td>
  <td>No</td>
  <td>Nombre del titular de la tarjeta.</td>
  <td>Alfanumérico</td>
  <td>Ejemplo: "Juan Pérez"</td>
  </tr>
  <tr>
    <td><b>TICKETNUMBER</b></td>
    <td>No</td>
    <td>Número de Ticket o Voucher</td>
    <td>Numérico de Hasta 4 dígitos</td>
    <td>Ejemplo: 7509</td>
  </tr>
  <tr>
    <td><b>AUTHORIZATIONCODE</b></td>
    <td>Si</td>
    <td>Código de autorización</td>
    <td>Numérico</td>
    <td>Ejemplo: 109345</td>
  </tr>
	
  <tr>
    <td><b>CFT</b></td>
    <td>Si</td>
    <td>CFT de la promoción aplicada</td>
    <td>Decimal</td>
    <td>Ejemplo: 0.00</td>
  </tr>
  <tr>
    <td><b>TEA</b></td>
    <td>Si</td>
    <td>TEA de la promoción aplicada</td>
    <td>Decimal</td>
    <td>Ejemplo: 22.00</td>
  </tr>
  <tr>
    <td><b>FEEAMOUNT</b></td>
    <td>Si</td>
    <td>Comisión por la transacción</td>
    <td>Decimal</td>
    <td>Ejemplo: 22.00</td>
  </tr>
  <tr>
    <td><b>TAXAMOUNT</b></td>
    <td>Si</td>
    <td>Retenciones de impuestos</td>
    <td>Decimal</td>
    <td>Ejemplo: 22.00</td>
  </tr>
  <tr>
    <td><b>SERVICECHAGEAMOUNT</b></td>
    <td>Si</td>
    <td>Costo del servicio</td>
    <td>Decimal</td>
    <td>Ejemplo: 22.00</td>
  </tr>
  <tr>
    <td><b>CREDITEDAMOUNT</b></td>
    <td>Si</td>
    <td>Monto neto acreditado</td>
    <td>Decimal</td>
    <td>Ejemplo: 22.00</td>
  </tr>
  <tr>
    <td><b>FEEAMOUNT</b></td>
    <td>Si</td>
    <td>Comisión por la transacción</td>
    <td>Decimal</td>
    <td>Ejemplo: 22.00</td>
  </tr>
  <tr>
    <td><b>RELEASESTATUS</b></td>
    <td>Si</td>
    <td>Estado de la transacción</td>
    <td>Alfanumérico</td>
    <td>Ejemplo: TX_APROBADA</td>
  </tr>
  <tr>
    <td><b>RELEASEDATETIME</b></td>
    <td>Si</td>
    <td>Fecha de disponibilización de la transacción</td>
    <td>Datetime</td>
    <td>Ejemplo: 2017-11-28T11:24:11</td>
  </tr>
  <tr>
    <td><b>PHONENUMBER</b></td>
    <td>Si</td>
    <td>Número de teléfono de facturación</td>
    <td>Alfanumérico</td>
    <td>Ejemplo: 541152528100</td>
  </tr>
  <tr>
    <td><b>ADDRESS</b></td>
    <td>Si</td>
    <td>Dirección de facturación</td>
    <td>Alfanumérico</td>
    <td>Ejemplo: Callao 78</td>
  </tr>  
  <tr>
    <td><b>POSTALCODE</b></td>
    <td>Si</td>
    <td>Código postal de facturación</td>
    <td>Alfanumérico</td>
    <td>Ejemplo: C1430DRW</td>
  </tr>  
  <tr>
    <td><b>REFUNDS</b></td>
    <td>Si</td>
    <td>Listado de devoluciones realizadas sobre la transacción</td>
    <td>Array</td>
    <td>-</td>
  </tr>  
  <tr>
    <td><b>ITEMS</b></td>
    <td>Si</td>
    <td>Listado de items de la compra</td>
    <td>Array</td>
    <td>&lt;code&gt;Camisetas&lt;/code&gt; &lt;description&gt;Fashion lleva diseando colecciones increbles desde&lt;/description&gt; &lt;name&gt;Camiseta efecto desteido de manga corta&lt;/name&gt; &lt;price&gt;19.98&lt;/price&gt; &lt;quantity&gt;1&lt;/quantity&gt; &lt;sku&gt;demo1&lt;/sku&gt; &lt;totalAmount&gt;19.98&lt;/totalAmount&gt;</td>
  </tr>    
</table>

<ins><strong>Ejemplo de Respuesta</strong></ins>

```java
Map<String, Object>
	{ OperationsColections = {
			Operations = {
				RESULTCODE = 999 ,
				RESULTMESSAGE = RECHAZADA,
				DATETIME = 2015-05-13T14:11:38.287+00:00,
				OPERATIONID = 01,
				CURRENCYCODE = 32,
				AMOUNT = 54,
				TYPE = compra_online,
				INSTALLMENTPAYMENTS = 4,
				CUSTOMEREMAIL = jose@perez.com,
				IDENTIFICATIONTYPE = DNI,
				IDENTIFICATION = 1212121212,
				CARDNUMBER = 12121212XXXXXX1212,
				CARDHOLDERNAME = Jose Perez,
				TICKETNUMBER = 0,
				AUTHORIZATIONCODE = null,
				BARCODE = null,
				COUPONEXPDATE = null,
				COUPONSECEXPDATE = null,
     			        COUPONSUBSCRIBER = null,
				BANKID = 11,
				PAYMENTMETHODTYPE = Crédito,
				PAYMENTMETHODCODE = 42,
				PROMOTIONID = 2706,
                                AMOUNTBUYER = 10.00,
                                PAYMENTMETHODNAME = VISA,
				TEA = 22.00,
				CFT = 0.00,
				RELEASESTATUS : TX_APROBADA,
				RELEASEDATETIME : 2017-11-28T11:24:11,
				PHONENUMBER: 541141231234,
				ADDRESS: Uspallata 123,
				POSTALCODE: A1430AAA,
				CUSTOMERID: 10,
				REFUNDED = null, 
				REFUNDS = { REFUND0 = { ID = 50163419,
										DATETIME = 2016-03-18T16:03:54.987-03:00,
										AMOUNT = 10.00 },
							REFUND1 = { ID = 50163416,
										DATETIME = 2016-03-18T15:52:07.877-03:00,
										AMOUNT = 2.00 },		
    					    REFUND2 = { ID = 50163414,
										DATETIME = 2016-03-18T15:51:17.447-03:00,
										AMOUNT = 2.00 }
							}
						}
				},
				ITEMS= {
					ITEM={
						code= Cameras,
						description= Engineered with,
						name= Nikon D300,
						price= 80.00,
						quantity= 3,
						sku= 31,
						totalAmount= 240.00
					}, {
						code= Desktops,
						description= HTC Touch  in,
						name= HTC Touch HD,
						price= 100.00,
						quantity= 3,
						sku=28,
						totalAmount= 300.00
					}
				}
				PUSHNOTIFYENDPOINT = null,
				PUSHNOTIFYMETHOD = null,
				PUSHNOTIFYSTATES = null,
	}					
```
Además, se puede conocer el estado de las transacciones a través del portal [www.todopago.com.ar](http://www.todopago.com.ar/). Desde el portal se verán los estados "Aprobada" y "Rechazada". Si el método de pago elegido por el comprador fue Pago Fácil o RapiPago, se podrán ver en estado "Pendiente" hasta que el mismo sea pagado.

<a name="statusdate"></a>

#### Consulta de operaciones por rango de tiempo

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-getoperations.jpg)

En este caso hay que llamar a getByRangeDateTime() y devolvera todas las operaciones realizadas en el rango de fechas dado

Campo      | Requerido  | Descripción                                 | Tipo de Dato | Valores posibles / Ejemplo
-----------|------------|---------------------------------------------|--------------|----------------------------------
MERCHANT   | Sí         | Nro identificador del comercio              | numérico     | 12305
STARTDATE  | Sí         | Fecha y hora desde                          | date         | "2017-03-03T16:15:00"
ENDDATE    | Sí         | Fecha y hora hasta                          | date         | "2017-08-07T22:34:00"
PAGENUMBER | Sí         | Número de página a la que se desea acceder* | entero       | 2

```java
private static Map<String, String> getBRYParameters() {
		Map<String, String> parameters = new HashMap<String, String>();
		parameters.put(ElementNames.Merchant, "12345678");
		parameters.put(ElementNames.STARTDATE, "2016-01-01");
		parameters.put(ElementNames.ENDDATE, "2016-03-03");
		parameters.put(ElementNames.PAGENUMBER, "1");	
		return parameters;
}
	
Map<String, Object> j = tpc.getByRangeDateTime(getBRYParameters());
```

####Descubrimiento de medios de pago

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-paymentmethods.jpg)

La SDK cuenta con un método para obtener todos los medios de pago habilitados en TodoPago.
```java
	
Map<String, Object> discoverPaymentMethods=tpc.discoverPaymentMethods();
```

<a name="discover"></a>
#### Descubrimiento de Medios de Pago

La SDK cuenta con un método para obtener todos los medios de pago habilitados en TodoPago.

```java
Map<String, Object> g = tpc.discoverPaymentMethods();
```



<a name="devolucion"></a>
#### Devolución

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-devolucion-total.jpg)


La SDK dispone de métodos para realizar la devolución, de una transacción realizada a traves de TodoPago.

Se debe llamar al método ```voidRequest``` de la siguiente manera:

Campo            | Requerido  | Descripción                                                              | Tipo de Dato | Valores posibles / Ejemplo
-----------------|------------|---------------------------------------------------------------------     |--------------|---------------------------
Security         | Sí         | API Key del comercio asignada por TodoPago                               | alfanumérico | 837BE68A892F06C17B944F344AEE8F5F
Merchant         | Sí         | Nro de comercio asignado por TodoPago                                    | numérico     | 12345
RequestKey       | No*        | RequestKey devuelto como respuesta del servicio SendAutorizeRequest      | alfanumérico | 6d2589f2-37e6-1334-7565-3dc19404480c
AuthorizationKey | No*        | AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer | alfanumérico | 4a2569a2-38e6-4589-1564-4480c3dc1940

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

También se puede llamar al método ```voidRequest``` de la esta otra manera:

```java
private static Map<String, String> getVRParameters() {
		Map<String, String> parameters = new HashMap<String, String>();
		parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF"); // API Key del comercio asignada por TodoPago 
		parameters.put(ElementNames.Merchant, "12345678");// Merchant o Nro de comercio asignado por TodoPago
		parameters.put(ElementNames.AuthorizationKey, "6d2589f2-37e6-1334-7565-3dc19404480c"); // AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer
		return parameters;
}
	
Map<String, Object> h = tpc.voidRequest(getVRParameters());	
```

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
<br>

<a name="devolucionparcial"></a>
#### Devolución parcial

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-devolucion-parcial.jpg)


La SDK dispone de métodos para realizar la devolución parcial, de una transacción realizada a traves de TodoPago.

_Nota: Para el caso de promociones con costo financiero, se deberá enviar el monto a devolver en base al valor original de la transacción y no del monto finalmente cobrado. TodoPago se encargará de devolver el porcentaje del costo financiero correspondiente a la devolución parcial._

Se debe llamar al método ```returnRequest``` de la siguiente manera:

Campo            | Requerido | Descripción                                                              | Tipo de Dato                                                                  | Valores posibles / Ejemplo
-----------      |------------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------
Security         | Sí         | API Key del comercio asignada por TodoPago                               | alfanumérico                                                                  | 837BE68A892F06C17B944F344AEE8F5F
Merchant         | Sí         | Nro de comercio asignado por TodoPago                                    | numérico                                                                      | 12345
RequestKey       | No*        | RequestKey devuelto como respuesta del servicio SendAutorizeRequest      | alfanumérico                                                                  | 6d2589f2-37e6-1334-7565-3dc19404480c
AuthorizationKey | No*        | AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer | alfanumérico                                                                  | 4a2569a2-38e6-4589-1564-4480c3dc1940
AMOUNT           | No         | Monto a devolver, si no se envía, se trata de una devolución total       | string usando . como separador decimal, incluyendo SIEMPRE 2 cifras decimales | 23.50

```java
private static Map<String, String> getRRParameters() {
		Map<String, String> parameters = new HashMap<String, String>();
		parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF"); // API Key del comercio asignada por TodoPago 
		parameters.put(ElementNames.Merchant, "12345678"); // Merchant o Nro de comercio asignado por TodoPago
		parameters.put(ElementNames.RequestKey, "e31d340c-690c-afe6-c478-fc1bef3fc157");  // RequestKey devuelto como respuesta del servicio SendAutorizeRequest
		parameters.put(ElementNames.Amount, "10.5"); // Opcional. Monto a devolver, si no se envía, se trata de una devolución total
		return parameters;
}
Map<String, Object> i = tpc.returnRequest(getRRParameters());
```

También se puede llamar al método ```returnRequest``` de la esta otra manera:

```java
private static Map<String, String> getRRParameters() {
		Map<String, String> parameters = new HashMap<String, String>();
		parameters.put(ElementNames.Security, "1234567890ABCDEF1234567890ABCDEF"); // API Key del comercio asignada por TodoPago 
		parameters.put(ElementNames.Merchant, "12345678"); // Merchant o Nro de comercio asignado por TodoPago
		parameters.put(ElementNames.AuthorizationKey, "6d2589f2-37e6-1334-7565-3dc19404480c");  // AuthorizationKey devuelto como respuesta del servicio GetAuthorizeAnswer
		parameters.put(ElementNames.Amount, "10.5"); // Opcional. Monto a devolver, si no se envía, se trata de una devolución total
		return parameters;
}
Map<String, Object> i = tpc.returnRequest(getRRParameters());	
```

**Respuesta de servicio:**

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
<br>

<a name="formhidrido"></a>
#### Formulario híbrido

**Conceptos básicos**<br>
El formulario híbrido, es una alternativa al medio de pago actual por redirección al formulario externo de TodoPago.<br>
Con el mismo, se busca que el comercio pueda adecuar el look and feel del formulario a su propio diseño.

**Librería**
El formulario requiere incluir en la página una librería Javascript de TodoPago.<br>
El endpoint depende del entorno:
+ Desarrollo: https://developers.todopago.com.ar/resources/v2/TPBSAForm.min.js
+ Producción: https://forms.todopago.com.ar/resources/v2/TPBSAForm.min.js

**Restricciones y libertades en la implementación**

+ Por ningún motivo podrá bajarse el javascript provisto ni realizar cambios en el mismo. Siempre deberá ser tomado de los servidores de TodoPago.
+ Ninguno de los campos del formulario podrá contar con el atributo name.
+ Se deberá proveer de manera obligatoria un botón para gestionar el pago con Billetera Todo Pago.
+ Todos los elementos de tipo <option> son completados por la API de Todo Pago.
+ Los campos tienen un id por defecto. Si se prefiere utilizar otros ids se deberán especificar los
mismos cuando se inicialice el script de Todo Pago.
+ Pueden aplicarse todos los detalles visuales que se crean necesarios, la API de Todo Pago no
altera los atributos class y style.
+ Puede utilizarse la API para setear los atributos placeholder del formulario, para ello deberá
especificar dichos placeholders en la inicialización del formulario "window.TPFORMAPI.hybridForm.setItem". En caso de que no se especifiquen los placeholders se usarán los valores por defecto de la API.

**HTML del formulario**

El formulario implementado debe contar al menos con los siguientes campos.

```html
<body>
  <select id="formaPagoCbx"></select>
  <input id="numeroTarjetaTxt"/>
  <label id="numeroTarjetaLbl"></label>
  <select id="medioPagoCbx"></select>
  <select id="bancoCbx"></select>
  <select id="promosCbx"></select>
  <label id="promosLbl"></label>
  <label id="peiLbl"></label>
  <input id="peiCbx"/>
  <select id="mesCbx"></select>
  <select id="anioCbx"></select>
  <label id="fechaLbl"></label>
  <input id="codigoSeguridadTxt"/>
  <label id="codigoSeguridadLbl"></label>
  <input id="nombreTxt"/>
  <select id="tipoDocCbx"></select>
  <input id="nroDocTxt"/>
  <input id="emailTxt"/>
  <label id="tokenPeiLbl"></label>
  <input id="tokenPeiTxt"/>
  <button id="MY_btnConfirmarPago"/>
  <button id="MY_btnPagarConBilletera"/>
</body>
```

**Inizialización y parámetros requeridos**<br>
Para inicializar el formulario se usa window.TPFORMAPI.hybridForm.initForm. El cual permite setear los elementos y ids requeridos.

Para inicializar un ítem de pago, es necesario llamar a window.TPFORMAPI.hybridForm.setItem. Este requiere obligatoriamente el parámetro publicKey que corresponde al PublicRequestKey (entregado por el SAR).
Se sugiere agregar los parámetros usuario, e-mail, tipo de documento y numero.

**Javascript**
```js
/************* CONFIGURACION DEL API ***********************/
window.TPFORMAPI.hybridForm.initForm({
callbackValidationErrorFunction: 'validationCollector',
callbackBilleteraFunction: 'billeteraPaymentResponse',
callbackCustomSuccessFunction: 'customPaymentSuccessResponse',
callbackCustomErrorFunction: 'customPaymentErrorResponse',
botonPagarId: 'MY_btnConfirmarPago',
botonPagarConBilleteraId: 'MY_btnPagarConBilletera',
modalCssClass: 'modal-class',
modalContentCssClass: 'modal-content',
beforeRequest: 'initLoading',
afterRequest: 'stopLoading'
});

/************* SETEO UN ITEM PARA COMPRAR ******************/
window.TPFORMAPI.hybridForm.setItem({
    publicKey: 'taf08222e-7b32-63d4-d0a6-5cabedrb5782', //obligatorio
    defaultNombreApellido: 'Usuario',
    defaultNumeroDoc: 20234211,
    defaultMail: 'todopago@mail.com',
    defaultTipoDoc: 'DNI'
});

/************* FUNCIONES CALLBACKS *************************/
function validationCollector(parametros) {
console.log("My validation collector callback");
console.log(parametros.field + " -> " + parametros.error);
}
function billeteraPaymentResponse(response) {
console.log("My billetera payment callback");
console.log(response.ResultCode + " -> " +response.ResultMessage);
}
function customPaymentSuccessResponse(response) {
console.log("My custom payment success callback");
console.log(response.ResultCode + " -> " +response.ResultMessage);
}
function customPaymentErrorResponse(response) {
console.log("My custom payment error callback");
console.log(response.ResultCode + " -> " +response.ResultMessage);
}
function initLoading() {
console.log('Loading...');
}
function stopLoading() {
console.log('Stop loading...');
}

```

**Callbacks**<br>
El formulario define callbacks javascript, que son llamados según el estado y la información del pago realizado:
+ billeteraPaymentResponse: Devuelve response si el pago con billetera se realizó correctamente.
+ customPaymentSuccessResponse: Devuelve response si el pago se realizo correctamente.
+ customPaymentErrorResponse: Si hubo algún error durante el proceso de pago, este devuelve el response con el código y mensaje correspondiente.
[<sub>Volver a inicio</sub>](#inicio)

[<sub>Volver a inicio</sub>](#inicio)

<a name="credenciales"></a>
#### Obtener credenciales

![imagen de configuracion](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-credenciales.jpg)

El SDK permite obtener las credenciales "Authentification", "MerchandId" y "Security" de la cuenta de Todo Pago, ingresando el usuario y contraseña.<br>
Esta funcionalidad es util para obtener los parametros de configuracion dentro de la implementacion.

- Crear una instancia de la clase User:
```java

public void getCredentials(TodoPagoConector tpc) {
		
		User user = new User("test@test.com", "test1234");// user y pass de TodoPago
		
		try {
			user = tpc.getCredentials(user);
			tpc.setAuthorize(getAuthorization(user));// set de la APIKey a TodoPagoConector 
			
		} catch (EmptyFieldException e) {//se debe realizar catch por campos en blanco
			logger.log(Level.WARNING, e.getMessage());						
		} catch (MalformedURLException e) {
			logger.log(Level.WARNING, e.getMessage());	
		} catch (ResponseException e) {
			logger.log(Level.WARNING, e.getMessage());
		} catch (ConnectionException e) {
			logger.log(Level.WARNING, e.getMessage());
		}
		System.out.println(user.toString());	
}
	
	private Map<String, List<String>> getAuthorization(User user) {
		Map<String, List<String>> parameters = new HashMap<String, List<String>>();
		parameters.put(ElementNames.Authorization,Collections.singletonList(user.getApiKey()));
		
		return parameters;
}
```
[<sub>Volver a inicio</sub>](#inicio)

<a name="echo"></a>
#### Estado del servicio

La SDK dispone de un método para verificar el estado de los servicios de TodoPago..<br>
Se debe llamar al método ```healthCheck``` de la siguiente manera:

```java
	Boolean check = tpc.healthCheck();	
```

<a name="opcionesadicionales"></a>
#### Opciones adicionales
Dentro del primer parámetro pasado al método sendAuthorizeRequest() pueden enviarse opciones adicionales que habilitan características para esa transacción en particular. A continuación se describen las mismas.

<a name="coutas"></a>
##### Rango de Cuotas
Es posible setear el rango de cuotas a mostrar en el formulario entre un mínimo y un máximo, enviando los siguientes parametros adicionales

<table>
  <tr>
    <th>Campo</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valores posibles / Ejemplo</th>
  </tr>
  <tr>
    <td><b>MININSTALLMENTS</b></td>
    <td>No</td>
    <td>Mínimo de cuotas a mostrar en el formulario</td>
    <td>Numérico</td>
    <td>3</td>
  </tr>
  <tr>
    <td><b>MAXINSTALLMENTS</b></td>
    <td>No</td>
    <td>Máximo de cuotas a mostrar en el formulario</td>
    <td>Numérico</td>
    <td>9</td>
  </tr>  
</table>

##### Ejemplo
 
```java		
Map<String, String> parameters = new HashMap<String, String>();
parameters.put(ElementNames.MAXINSTALLMENTS, "12");
parameters.put(ElementNames.MININSTALLMENTS, "1");	
```
 
 [<sub>Volver a inicio</sub>](#inicio)
 <br>
 
 <a name="split"></a>
#### Split de Pago
Para aquellos comercios que operen con terceros y requieran dividir una transacción entre varios establecimientos Todopago permite dividir el pago.

Esta funcionalidad permite que distintas subtransacciones con su propio merchants y montos operen sobre una transacción padre. Esto funciona enviado en el SAR dos campos, el "DISTRIBUTEDMERCHANT" y "DISTRIBUTEDAMOUNT". 
El primero envía la lista de los merchant hijos de la transacción separados por **#** y el campo "DISTRIBUTEDAMOUNT" enviá los montos de cada uno de los merchants en el mismo orden respecto a los merchant del campo "DISTRIBUTEDMERCHANT" y separados por **#**.

<table>
  <tr>
    <th>Campo</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valores posibles / Ejemplo</th>
  </tr>
  <tr>
    <td><b>DISTRIBUTEDMERCHANT</b></td>
    <td>No</td>
    <td>Listado de Merchants de los establecimientos separados por #</td>
    <td>Numérico</td>
    <td>3#20</td>
  </tr>
  <tr>
    <td><b>DISTRIBUTEDAMOUNT</b></td>
    <td>No</td>
    <td>Listado de montos del split de pago separados por #</td>
    <td>Numérico con decimales</td>
    <td>10.00#15.00</td>
  </tr>  
</table>

##### Ejemplo
 
```java		
Map<String, String> parameters = new HashMap<String, String>();
parameters.put(ElementNames.DISTRIBUTEDMERCHANT, "3#20");
parameters.put(ElementNames.DISTRIBUTEDAMOUNT, "10.00#15.00");	
```
 
 Nota: Actualmente no esta disponible la opcion realizar una devolución parcial de un Split de pago.
 
 [<sub>Volver a inicio</sub>](#inicio)

<a name="filtromp"></a>
## Filtrado de Medios de Pago
Mediante esta funcionalidad es posible filtrar los medios de pago habilitados en el formulario de pago. Se debe pasar en la llamada al servicio SendAuthorizeRequest un parámetro adicional con los ids de los medio de pago que se desean habilitar, los cuales pueden consultarse mediante el método de [Descubrimiento de Medios de Pago](#discover)

Para hacer uso de esta funcionalidad debe agregarse en el **Map<String, String> parameters** del método **sendAuthorizeRequest** el campo **AVAILABLEPAYMENTMETHODSIDS** con el valor de los ids de los medios de pago habilitados, separados por #

##### Ejemplo

```java		
Map<String, String> parameters = new HashMap<String, String>();
parameters.put("AVAILABLEPAYMENTMETHODSIDS", "1#42#500");	
```

[<sub>Volver a inicio</sub>](#inicio)
<br>

<a name="timeout"></a>
## Tiempo de vida del formulario.

Mediante esta funcionalidad se permite setear el tiempo maximo en el que se puede realizar el pago en el formulario en milisegundos. Por defecto si no se envia, 1800000 (30 minutos).
Valor minimo: 300000 (5 minutos)
Valor maximo: 21600000 (6hs) 

Para hacer uso de esta funcionalidad debe agregarse en el **Map<String, String> parameters** del método **sendAuthorizeRequest** el campo **TIMEOUT** con el valor en milisegundos
##### Ejemplo
 
```java		
Map<String, String> parameters = new HashMap<String, String>();
parameters.put(ElementNames.TIMEOUT, "350000");	
```
 
 [<sub>Volver a inicio</sub>](#inicio)
 <br>

<a name="tablareferencia"></a>    
## Tablas de Referencia   
###### [Provincias](#p)    
				
<p>Los siguientes códigos son utilizados para control de fraude y para el cálculo de retenciones del Impuesto sobre los Ingresos Brutos.</p>
<table>		
<tr><th>Provincia</th><th>Código</th></tr>		
<tr><td>CABA</td><td>C</td></tr>		
<tr><td>Buenos Aires</td><td>B</td></tr>		
<tr><td>Catamarca</td><td>K</td></tr>		
<tr><td>Chaco</td><td>H</td></tr>		
<tr><td>Chubut</td><td>U</td></tr>		
<tr><td>Córdoba</td><td>X</td></tr>		
<tr><td>Corrientes</td><td>W</td></tr>		
<tr><td>Entre Ríos</td><td>E</td></tr>		
<tr><td>Formosa</td><td>P</td></tr>		
<tr><td>Jujuy</td><td>Y</td></tr>		
<tr><td>La Pampa</td><td>L</td></tr>		
<tr><td>La Rioja</td><td>F</td></tr>		
<tr><td>Mendoza</td><td>M</td></tr>		
<tr><td>Misiones</td><td>N</td></tr>		
<tr><td>Neuquén</td><td>Q</td></tr>		
<tr><td>Río Negro</td><td>R</td></tr>		
<tr><td>Salta</td><td>A</td></tr>		
<tr><td>San Juan</td><td>J</td></tr>		
<tr><td>San Luis</td><td>D</td></tr>		
<tr><td>Santa Cruz</td><td>Z</td></tr>		
<tr><td>Santa Fe</td><td>S</td></tr>		
<tr><td>Santiago del Estero</td><td>G</td></tr>		
<tr><td>Tierra del Fuego</td><td>V</td></tr>		
<tr><td>Tucumán</td><td>T</td></tr>		
</table>

[<sub>Volver a inicio</sub>](#inicio)

<a name="codigoerrores"></a>
## Tabla de errores operativos

<table>
<tr><th>Id mensaje</th><th>Mensaje</th></tr>
<tr><td>-1</td><td>Tu compra fue exitosa.</td></tr>
<tr><td>1081</td><td>Tu saldo es insuficiente para realizar la transacción.</td></tr>
<tr><td>1100</td><td>El monto ingresado es menor al mínimo permitido</td></tr>
<tr><td>1101</td><td>El monto ingresado supera el máximo permitido.</td></tr>
<tr><td>1102</td><td>La tarjeta ingresada no corresponde al Banco indicado. Revisalo.</td></tr>
<tr><td>1104</td><td>El precio ingresado supera al máximo permitido.</td></tr>
<tr><td>1105</td><td>El precio ingresado es menor al mínimo permitido.</td></tr>
<tr><td>2010</td><td>En este momento la operación no pudo ser realizada. Por favor intentá más tarde. Volver a Resumen.</td></tr>
<tr><td>2031</td><td>En este momento la validación no pudo ser realizada, por favor intentá más tarde.</td></tr>
<tr><td>2050</td><td>Lo sentimos, el botón de pago ya no está disponible. Comunicate con tu vendedor.</td></tr>
<tr><td>2051</td><td>La operación no pudo ser procesada. Por favor, comunicate con tu vendedor.</td></tr>
<tr><td>2052</td><td>La operación no pudo ser procesada. Por favor, comunicate con tu vendedor.</td></tr>
<tr><td>2053</td><td>La operación no pudo ser procesada. Por favor, intentá más tarde. Si el problema persiste comunicate con tu vendedor</td></tr>
<tr><td>2054</td><td>Lo sentimos, el producto que querés comprar se encuentra agotado por el momento. Por favor contactate con tu vendedor.</td></tr>
<tr><td>2056</td><td>La operación no pudo ser procesada. Por favor intentá más tarde.</td></tr>
<tr><td>2057</td><td>La operación no pudo ser procesada. Por favor intentá más tarde.</td></tr>
<tr><td>2059</td><td>La operación no pudo ser procesada. Por favor intentá más tarde.</td></tr>
<tr><td>90000</td><td>La cuenta destino de los fondos es inválida. Verificá la información ingresada en Mi Perfil.</td></tr>
<tr><td>90001</td><td>La cuenta ingresada no pertenece al CUIT/ CUIL registrado.</td></tr>
<tr><td>90002</td><td>No pudimos validar tu CUIT/CUIL.  Comunicate con nosotros <a href="#contacto" target="_blank">acá</a> para más información.</td></tr>
<tr><td>99005</td><td>Tu compra no pudo realizarse. Iniciala nuevamente.</td></tr>
<tr><td>99900</td><td>El pago fue realizado exitosamente</td></tr>
<tr><td>99901</td><td>No hemos encontrado tarjetas vinculadas a tu Billetera. Podés  adherir medios de pago desde www.todopago.com.ar</td></tr>
<tr><td>99902</td><td>No se encontro el medio de pago seleccionado</td></tr>
<tr><td>99903</td><td>Lo sentimos, hubo un error al procesar la operación. Por favor reintentá más tarde.</td></tr>
<tr><td>99904</td><td>Tu compra no puede ser realizada. Comunicate con tu vendedor.</td></tr>
<tr><td>99953</td><td>Tu compra no pudo realizarse. Iniciala nuevamente o utilizá otro medio de pago.</td></tr>
<tr><td>99960</td><td>Esta compra requiere autorización de VISA. Comunicate al número que se encuentra al dorso de tu tarjeta.</td></tr>
<tr><td>99961</td><td>Esta compra requiere autorización de AMEX. Comunicate al número que se encuentra al dorso de tu tarjeta.</td></tr>
<tr><td>99970</td><td>Lo sentimos, no pudimos procesar la operación. Por favor reintentá más tarde.</td></tr>
<tr><td>99971</td><td>Lo sentimos, no pudimos procesar la operación. Por favor reintentá más tarde.</td></tr>
<tr><td>99977</td><td>Lo sentimos, no pudimos procesar la operación. Por favor reintentá más tarde.</td></tr>
<tr><td>99978</td><td>Lo sentimos, no pudimos procesar la operación. Por favor reintentá más tarde.</td></tr>
<tr><td>99979</td><td>Lo sentimos, el pago no pudo ser procesado.</td></tr>
<tr><td>99980</td><td>Ya realizaste un pago en este sitio por el mismo importe. Si querés realizarlo nuevamente esperá 5 minutos.</td></tr>
<tr><td>99982</td><td>Tu compra no pudo ser procesada. Iniciala nuevamente utilizando otro medio de pago.</td></tr>
<tr><td>99983</td><td>Lo sentimos, el medio de pago no permite la cantidad de cuotas ingresadas. Por favor intentá más tarde.</td></tr>
<tr><td>99984</td><td>Lo sentimos, el medio de pago seleccionado no opera en cuotas.</td></tr>
<tr><td>99985</td><td>Lo sentimos, el pago no pudo ser procesado.</td></tr>
<tr><td>99986</td><td>Lo sentimos, en este momento la operación no puede ser realizada. Por favor intentá más tarde.</td></tr>
<tr><td>99987</td><td>Lo sentimos, en este momento la operación no puede ser realizada. Por favor intentá más tarde.</td></tr>
<tr><td>99988</td><td>Lo sentimos, momentaneamente el medio de pago no se encuentra disponible. Por favor intentá más tarde.</td></tr>
<tr><td>99989</td><td>La tarjeta ingresada no está habilitada. Comunicate con la entidad emisora de la tarjeta para verificar el incoveniente.</td></tr>
<tr><td>99990</td><td>La tarjeta ingresada está vencida. Por favor seleccioná otra tarjeta o actualizá los datos.</td></tr>
<tr><td>99991</td><td>Los datos informados son incorrectos. Por favor ingresalos nuevamente.</td></tr>
<tr><td>99992</td><td>La fecha de vencimiento es incorrecta. Por favor seleccioná otro medio de pago o actualizá los datos.</td></tr>
<tr><td>99993</td><td>La tarjeta ingresada no está vigente. Por favor seleccioná otra tarjeta o actualizá los datos.</td></tr>
<tr><td>99994</td><td>El saldo de tu tarjeta no te permite realizar esta compra. Iniciala nuevamente utilizando otro medio de pago.</td></tr>
<tr><td>99995</td><td>La tarjeta ingresada es invalida. Seleccioná otra tarjeta para realizar el pago.</td></tr>
<tr><td>99996</td><td>La operación fué rechazada por el medio de pago porque el monto ingresado es inválido.</td></tr>
<tr><td>99997</td><td>Lo sentimos, en este momento la operación no puede ser realizada. Por favor intentá más tarde.</td></tr>
<tr><td>99998</td><td>Lo sentimos, la operación fue rechazada. Comunicate con la entidad emisora de la tarjeta para verificar el incoveniente o seleccioná otro medio de pago.</td></tr>
<tr><td>99999</td><td>Lo sentimos, la operación no pudo completarse. Comunicate con la entidad emisora de la tarjeta para verificar el incoveniente o seleccioná otro medio de pago.</td></tr>
</table>

[<sub>Volver a inicio</sub>](#inicio)

<a name="interrores"></a>
## Tabla de errores de integración

<table>
<tr><td>**Id mensaje**</td><td>**Descripción**</td></tr>
<tr><td>98001 </td><td>ERROR: El campo CSBTCITY es requerido</td></tr>
<tr><td>98002 </td><td>ERROR: El campo CSBTCOUNTRY es requerido</td></tr>
<tr><td>98003 </td><td>ERROR: El campo CSBTCUSTOMERID es requerido</td></tr>
<tr><td>98004 </td><td>ERROR: El campo CSBTIPADDRESS es requerido</td></tr>
<tr><td>98005 </td><td>ERROR: El campo CSBTEMAIL es requerido</td></tr>
<tr><td>98006 </td><td>ERROR: El campo CSBTFIRSTNAME es requerido</td></tr>
<tr><td>98007 </td><td>ERROR: El campo CSBTLASTNAME es requerido</td></tr>
<tr><td>98008 </td><td>ERROR: El campo CSBTPHONENUMBER es requerido</td></tr>
<tr><td>98009 </td><td>ERROR: El campo CSBTPOSTALCODE es requerido</td></tr>
<tr><td>98010 </td><td>ERROR: El campo CSBTSTATE es requerido</td></tr>
<tr><td>98011 </td><td>ERROR: El campo CSBTSTREET1 es requerido</td></tr>
<tr><td>98012 </td><td>ERROR: El campo CSBTSTREET2 es requerido</td></tr>
<tr><td>98013 </td><td>ERROR: El campo CSPTCURRENCY es requerido</td></tr>
<tr><td>98014 </td><td>ERROR: El campo CSPTGRANDTOTALAMOUNT es requerido</td></tr>
<tr><td>98015 </td><td>ERROR: El campo CSMDD7 es requerido</td></tr>
<tr><td>98016 </td><td>ERROR: El campo CSMDD8 es requerido</td></tr>
<tr><td>98017 </td><td>ERROR: El campo CSMDD9 es requerido</td></tr>
<tr><td>98018 </td><td>ERROR: El campo CSMDD10 es requerido</td></tr>
<tr><td>98019 </td><td>ERROR: El campo CSMDD11 es requerido</td></tr>
<tr><td>98020 </td><td>ERROR: El campo CSSTCITY es requerido</td></tr>
<tr><td>98021 </td><td>ERROR: El campo CSSTCOUNTRY es requerido</td></tr>
<tr><td>98022 </td><td>ERROR: El campo CSSTEMAIL es requerido</td></tr>
<tr><td>98023 </td><td>ERROR: El campo CSSTFIRSTNAME es requerido</td></tr>
<tr><td>98024 </td><td>ERROR: El campo CSSTLASTNAME es requerido</td></tr>
<tr><td>98025 </td><td>ERROR: El campo CSSTPHONENUMBER es requerido</td></tr>
<tr><td>98026 </td><td>ERROR: El campo CSSTPOSTALCODE es requerido</td></tr>
<tr><td>98027 </td><td>ERROR: El campo CSSTSTATE es requerido</td></tr>
<tr><td>98028 </td><td>ERROR: El campo CSSTSTREET1 es requerido</td></tr>
<tr><td>98029 </td><td>ERROR: El campo CSMDD12 es requerido</td></tr>
<tr><td>98030 </td><td>ERROR: El campo CSMDD13 es requerido</td></tr>
<tr><td>98031 </td><td>ERROR: El campo CSMDD14 es requerido</td></tr>
<tr><td>98032 </td><td>ERROR: El campo CSMDD15 es requerido</td></tr>
<tr><td>98033 </td><td>ERROR: El campo CSMDD16 es requerido</td></tr>
<tr><td>98034 </td><td>ERROR: El campo CSITPRODUCTCODE es requerido</td></tr>
<tr><td>98035 </td><td>ERROR: El campo CSITPRODUCTDESCRIPTION es requerido</td></tr>
<tr><td>98036 </td><td>ERROR: El campo CSITPRODUCTNAME es requerido</td></tr>
<tr><td>98037 </td><td>ERROR: El campo CSITPRODUCTSKU es requerido</td></tr>
<tr><td>98038 </td><td>ERROR: El campo CSITTOTALAMOUNT es requerido</td></tr>
<tr><td>98039 </td><td>ERROR: El campo CSITQUANTITY es requerido</td></tr>
<tr><td>98040 </td><td>ERROR: El campo CSITUNITPRICE es requerido</td></tr>
<tr><td>98101 </td><td>ERROR: El formato del campo CSBTCITY es incorrecto</td></tr>
<tr><td>98102 </td><td>ERROR: El formato del campo CSBTCOUNTRY es incorrecto</td></tr>
<tr><td>98103 </td><td>ERROR: El formato del campo CSBTCUSTOMERID es incorrecto</td></tr>
<tr><td>98104 </td><td>ERROR: El formato del campo CSBTIPADDRESS es incorrecto</td></tr>
<tr><td>98105 </td><td>ERROR: El formato del campo CSBTEMAIL es incorrecto</td></tr>
<tr><td>98106 </td><td>ERROR: El formato del campo CSBTFIRSTNAME es incorrecto</td></tr>
<tr><td>98107 </td><td>ERROR: El formato del campo CSBTLASTNAME es incorrecto</td></tr>
<tr><td>98108 </td><td>ERROR: El formato del campo CSBTPHONENUMBER es incorrecto</td></tr>
<tr><td>98109 </td><td>ERROR: El formato del campo CSBTPOSTALCODE es incorrecto</td></tr>
<tr><td>98110 </td><td>ERROR: El formato del campo CSBTSTATE es incorrecto</td></tr>
<tr><td>98111 </td><td>ERROR: El formato del campo CSBTSTREET1 es incorrecto</td></tr>
<tr><td>98112 </td><td>ERROR: El formato del campo CSBTSTREET2 es incorrecto</td></tr>
<tr><td>98113 </td><td>ERROR: El formato del campo CSPTCURRENCY es incorrecto</td></tr>
<tr><td>98114 </td><td>ERROR: El formato del campo CSPTGRANDTOTALAMOUNT es incorrecto</td></tr>
<tr><td>98115 </td><td>ERROR: El formato del campo CSMDD7 es incorrecto</td></tr>
<tr><td>98116 </td><td>ERROR: El formato del campo CSMDD8 es incorrecto</td></tr>
<tr><td>98117 </td><td>ERROR: El formato del campo CSMDD9 es incorrecto</td></tr>
<tr><td>98118 </td><td>ERROR: El formato del campo CSMDD10 es incorrecto</td></tr>
<tr><td>98119 </td><td>ERROR: El formato del campo CSMDD11 es incorrecto</td></tr>
<tr><td>98120 </td><td>ERROR: El formato del campo CSSTCITY es incorrecto</td></tr>
<tr><td>98121 </td><td>ERROR: El formato del campo CSSTCOUNTRY es incorrecto</td></tr>
<tr><td>98122 </td><td>ERROR: El formato del campo CSSTEMAIL es incorrecto</td></tr>
<tr><td>98123 </td><td>ERROR: El formato del campo CSSTFIRSTNAME es incorrecto</td></tr>
<tr><td>98124 </td><td>ERROR: El formato del campo CSSTLASTNAME es incorrecto</td></tr>
<tr><td>98125 </td><td>ERROR: El formato del campo CSSTPHONENUMBER es incorrecto</td></tr>
<tr><td>98126 </td><td>ERROR: El formato del campo CSSTPOSTALCODE es incorrecto</td></tr>
<tr><td>98127 </td><td>ERROR: El formato del campo CSSTSTATE es incorrecto</td></tr>
<tr><td>98128 </td><td>ERROR: El formato del campo CSSTSTREET1 es incorrecto</td></tr>
<tr><td>98129 </td><td>ERROR: El formato del campo CSMDD12 es incorrecto</td></tr>
<tr><td>98130 </td><td>ERROR: El formato del campo CSMDD13 es incorrecto</td></tr>
<tr><td>98131 </td><td>ERROR: El formato del campo CSMDD14 es incorrecto</td></tr>
<tr><td>98132 </td><td>ERROR: El formato del campo CSMDD15 es incorrecto</td></tr>
<tr><td>98133 </td><td>ERROR: El formato del campo CSMDD16 es incorrecto</td></tr>
<tr><td>98134 </td><td>ERROR: El formato del campo CSITPRODUCTCODE es incorrecto</td></tr>
<tr><td>98135 </td><td>ERROR: El formato del campo CSITPRODUCTDESCRIPTION es incorrecto</td></tr>
<tr><td>98136 </td><td>ERROR: El formato del campo CSITPRODUCTNAME es incorrecto</td></tr>
<tr><td>98137 </td><td>ERROR: El formato del campo CSITPRODUCTSKU es incorrecto</td></tr>
<tr><td>98138 </td><td>ERROR: El formato del campo CSITTOTALAMOUNT es incorrecto</td></tr>
<tr><td>98139 </td><td>ERROR: El formato del campo CSITQUANTITY es incorrecto</td></tr>
<tr><td>98140 </td><td>ERROR: El formato del campo CSITUNITPRICE es incorrecto</td></tr>
<tr><td>98201 </td><td>ERROR: Existen errores en la información de los productos</td></tr>
<tr><td>98202 </td><td>ERROR: Existen errores en la información de CSITPRODUCTDESCRIPTION los productos</td></tr>
<tr><td>98203 </td><td>ERROR: Existen errores en la información de CSITPRODUCTNAME los productos</td></tr>
<tr><td>98204 </td><td>ERROR: Existen errores en la información de CSITPRODUCTSKU los productos</td></tr>
<tr><td>98205 </td><td>ERROR: Existen errores en la información de CSITTOTALAMOUNT los productos</td></tr>
<tr><td>98206 </td><td>ERROR: Existen errores en la información de CSITQUANTITY los productos</td></tr>
<tr><td>98207 </td><td>ERROR: Existen errores en la información de CSITUNITPRICE de los productos</td></tr>
</table>

[<sub>Volver a inicio</sub>](#inicio)

<a name="eclipse"></a>		
## Agregar el proyecto a Eclipse		
- Bajar Maven 3 de la siguiente URL: http://maven.apache.org/download.cgi
- Descomprimir lo descargado.
- Agregar o modificar la variable de entorno M2_HOME con path a donde se descomprime Maven 
- Descargar el proyecto de GitHub
- Por consola ir a la carpeta donde se descargo el proyecto
- Ejecutar las siguientes lineas:
	- mvn clean install -Dwtpversion=2.0
	- mvn eclipse:clean eclipse:eclipse -Dwtpversion=2.0
- Ir a Eclipse e importar el Proyecto normalmente: File - Import - Existing Projects into Workspace

<br />		
[<sub>Volver a inicio</sub>](#inicio)
