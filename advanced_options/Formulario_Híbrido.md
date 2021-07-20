# Formulario híbrido

**Conceptos básicos**<br>
El formulario híbrido es una alternativa al medio de pago actual por redirección al formulario externo de TodoPago.<br>
Con el mismo, se busca que el comercio pueda adecuar el look and feel del formulario a su propio diseño.

**Librería**<br>
El formulario requiere incluir en la página una librería Javascript de TodoPago.<br>
El endpoint depende del entorno:
+ Desarrollo: https://apis.developers.todopago.com.ar/resources/v2/TPBSAForm.min.js
+ Produccion: https://forms.todopago.com.ar/resources/v2/TPBSAForm.min.js

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
	<select id="formaDePagoCbx"></select>
	<select id="bancoCbx"></select>
	<select id="promosCbx"></select>

    <!-- Para los casos en el que el comercio opera con PEI -->
        <label id="labelPeiCheckboxId"></label>
    	<input id="peiCbx"/>
    <!-- -->
	<label id="labelPromotionTextId"></label>
	<input id="numeroTarjetaTxt"/>
	<input id="mesTxt"/>
	<input id="anioTxt"/>
	<input id="codigoSeguridadTxt"/>
	<label id="labelCodSegTextId"></label>
	<input id="apynTxt"/>
	<select id="tipoDocCbx"></select>
	<input id="nroDocTxt"/>
	<input id="emailTxt"/><br/>

    <!-- Para los casos en el que el comercio opera con PEI -->
	    <label id="labelPeiTokenTextId"></label>
	    <input id="peiTokenTxt"/>
    <!-- -->

        <button id="MY_btnPagarConBilletera"/>
	<button id="MY_btnConfirmarPago"/>
</body>
```

**Inizialización y parametros requeridos**<br>
Para inicializar el formulario se usa window.TPFORMAPI.hybridForm.initForm. El cual permite setear los elementos e ids requeridos.

Para inicializar un ítem de pago, es necesario llamar a window.TPFORMAPI.hybridForm.setItem. Éste requiere obligatoriamente el parámetro publicKey que corresponde al PublicRequestKey (entregado por el SAR).
Se sugiere agregar los parámetros usuario, e-mail, tipo de documento y número.

**Javascript**
```js
window.TPFORMAPI.hybridForm.initForm({
    callbackValidationErrorFunction: 'validationCollector',
	callbackCustomSuccessFunction: 'customPaymentSuccessResponse',
	callbackCustomErrorFunction: 'customPaymentErrorResponse',
        callbackBilleteraFunction: 'billeteraPaymentResponse',
	botonPagarId: 'MY_btnConfirmarPago',
	modalCssClass: 'modal-class',
	modalContentCssClass: 'modal-content',
	beforeRequest: 'initLoading',
	afterRequest: 'stopLoading'
});

window.TPFORMAPI.hybridForm.setItem({
    publicKey: 'taf08222e-7b32-63d4-d0a6-5cabedrb5782', //obligatorio
    defaultNombreApellido: 'Usuario',
    defaultNumeroDoc: 20234211,
    defaultMail: 'todopago@mail.com',
    defaultTipoDoc: 'DNI'
});

//callbacks de respuesta del pago
function validationCollector(parametros) {
}
function billeteraPaymentResponse(response) {
}
function customPaymentSuccessResponse(response) {
}
function customPaymentErrorResponse(response) {
}
function initLoading() {
}
function stopLoading() {
}
```

**Callbacks**<br>
El formulario define callbacks javascript, que son llamados según el estado y la información del pago realizado:
+ billeteraPaymentResponse: Devuelve response si el pago se realizó con Billetera.
+ customPaymentSuccessResponse: Devuelve response si el pago se realizó correctamente.
+ customPaymentErrorResponse: Si hubo algun error durante el proceso de pago, este devuelve el response con el código y mensaje correspondiente.

**Ejemplo de Implementación**:
<a href="/resources/form_hibrido-ejemplo/index.html" target="blank">Formulario híbrido</a>

[<sub>Volver a inicio</sub>](#inicio)

<a name="credenciales"></a>
#### Obtener credenciales
![credenciales](https://raw.githubusercontent.com/TodoPago/imagenes/master/README.img/secuencia-credenciales.jpg)

El SDK permite obtener las credenciales "Authentification", "MerchandId" y "Security" de la cuenta de Todo Pago, ingresando el usuario y contraseña.<br>
Esta funcionalidad es útil para obtener los parámetros de configuración dentro de la implementación.

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
