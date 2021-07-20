# TodoPago SDK-JAVA módulo para conexión con gateway de pago
<br>

## Donde comenzar?

<a name="merchant"></a>
Para comenzar con la instalacion, primero te recomendamos:

1. Ya estar registrado en la plataforma (https://portal.todopago.com.ar/web-app/registro).
2. Estar habilitado para cobrar en la plataforma (https://todopago.com.ar/cobrar).
3. Id Merchant y apikey para poder autentificarte en la plataforma. Si no sabes donde esta, anda a este link.
```
https://portal.todopago.com.ar/app/ 
```
Logueate y asegurate haber finalizado el proceso de alta. Si lo tenes finalizado anda a al menu vender y despues elegi el tab Integracion Avanzada y vas a ver:
```
Nro. de Comercio (lo vamos a llamar Merchant ID) formato(XXXXXX)
Credenciales (le vamos a decir API Keys) formato (TODOPAGO xxxx85cab46848c280ed44e81c27xxxx)
```

Vamos a tener credenciales de produccion y credenciales de prueba. en este caso vamos a usar las de pruebas, pero recorda cambiarlas en produccion.

<a name="tarjetas"></a>


Tambien vamos a tener tarjetas de pruebas con estas podras probar sin riesgos, y asi simular tus pagos con transacciones aprobadas: 
 
Nro de Tarjeta      Tipo de Tarjeta     CCV     Vencimiento
<table>
<tr><th>Nro de Tarjeta</th><th>Tipo de Tarjeta</th><th>CCV</th><th>Vencimiento</th></tr>
<tr><td>4546400034748181</td><td>Visa Crédito</td><td>123</td><td>10/30</td></tr>
<tr><td>4517510000020087</td><td>Visa Crédito</td><td>123</td><td>10/30</td></tr>
</table><br>


 Solo se aprobarán las transacciones entre los montos de $ 1,00 y $ 999,00. Además, podés realizar pruebas de transacciones rechazadas y analizar los mensajes de error:
<br><br>
 

 
<table>
<tr><th>Nro de Tarjeta</th><th>Tipo de Tarjeta</th><th>Resultado</th></tr>
<tr><td>4242424242424242</td><td>VISA</td><td>99955 - La tarjeta ingresada es invalida.</td></tr>
<tr><td>4012888888881881</td><td>VISA</td><td>99955 - La tarjeta ingresada es invalida.</td></tr>
<tr><td>5555555555554444</td><td>MasterCard</td><td>99997 - En este momento la operación no pudo ser realizada. Por favor intentá más tarde.</td></tr>
<tr><td>378282246310005</td><td>AmEx</td><td>99955 - La tarjeta ingresada es invalida.</td></tr>
<tr><td>371449635398431</td><td>AmEx</td><td>99955 - La tarjeta ingresada es invalida.</td></tr>
<tr><td>30569309025904</td><td>Diners Club </td><td>99997 - En este momento la operación no pudo ser realizada. Por favor intentá más tarde.</td></tr>
<tr><td>38520000023237</td><td>Diners Club </td><td>99997 - En este momento la operación no pudo ser realizada. Por favor intentá más tarde.</td></tr>
<tr><td>4000000000000010</td><td>VISA</td><td>99955 - La tarjeta ingresada es invalida.</td></tr>
</table>


Para una configuracion avanzada podes leer:<br>
*  [Operatoria Agrupador](../main/advanced_options/Operatoria_Agrupador.md)<br> 
*  [Formulario Híbrido](../main/advanced_options/Formulario_Híbrido.md)<br>
*  [Control de Fraude](../main/advanced_options/Control_Fraude.md)<br>
*  [Devolución Parcial](../main/advanced_options/Devolución_Parcial.md)<br>
*  [Medios de Pago](../main/advanced_options/Medios_Pago.md)<br>



Y aca tenes las tablas de referencias de error:<br>
* [Tabla de errores de integración](../main/tables/errores_integración.md)
* [Tabla de errores operativos](../main/tables/errores_operativos.md)


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
	<version>2.0.0</version>
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
- Obtener las credenciales a traves  del m&eacute;todo getCredentials de TodoPago. (API Keys, si no sabes que es,  [Volver a leer](#merchant)).

<br/>
<a name="agrupador"></a>
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


