# Datos adicionales para el control de fraude
Los datos adicionales para control de fraude son **obligatorios** para la operatoria con TodoPago. En las tablas podras observar el nombre del parametro, su obligatoriedad, la descripción, descripción del tipo de dato y el valor de ejemplo.<br>

**Parámetros Generales:**
<table>
  <tr>
    <th>Parámetro</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Descripción</th>
    <th>Valor Ejemplo</th>
  </tr>
  <tr>
    <td><b>CSBTCITY</b></td>
    <td>Sí</td>
    <td>Ciudad de facturación</td>
    <td>Alfanumérico de 50 caracteres.</td>
    <td>Ejemplo: Villa General Belgrano</td>
  </tr>
  <tr>
    <td><b>CSBTCOUNTRY</b></td>
    <td>Sí</td>
    <td>País de facturación</td>
    <td>Alfanumérico de 2 caracteres.</td>
    <td>Código ISO</td>
  </tr>
  <tr>
    <td><b>CSBTCUSTOMERID</b></td>
    <td>Si</td>
    <td>Identificador del usuario al que se le emite la factura. No puede contener un correo electrónico</td>
    <td>Alfanumérico de 50 caracteres.</td>
    <td>Ejemplos:
A.Carlos
453458</td>
  </tr>
  <tr>
    <td><b>CSBTIPADDRESS</b></td>
    <td>Sí</td>
    <td>IP de la PC del comprador </td>
    <td>Alfanumérico de 15 caracteres.</td>
    <td>Ejemplo: 10.1.27.63</td>
  </tr>
  <tr>
    <td><b>CSBTEMAIL</b></td>
    <td>Si</td>
    <td>Mail del usuario al que se le emite la factura</td>
    <td>Alfanumérico de 100 caracteres.</td>
    <td>Ejemplo: todopago@hotmail.com</td>
  </tr>
  <tr>
    <td><b>CSBTFIRSTNAME</b></td>
    <td>Si</td>
    <td>Nombre del usuario al que se le emite la factura</td>
    <td>Alfanumérico de 60 caracteres.</td>
    <td>Ejemplo: Juan</td>
  </tr>
  <tr>
    <td><b>CSBTLASTNAME</b></td>
    <td>Si</td>
    <td>Apellido del usuario al que se le emite la factura</td>
    <td>Alfanumérico de 60 caracteres.</td>
    <td>Ejemplo: Perez</td>
  </tr>
  <tr>
    <td><b>CSBTPHONENUMBER</b></td>
    <td>Si</td>
    <td>Teléfono del usuario al que se le emite la factura. No utilizar guiones, puntos o espacios. Incluir código de país</td>
    <td>Alfanumérico de 15 caracteres.</td>
    <td>Ejemplo: 541160913988</td>
  </tr>
  <tr>
    <td><b>CSBTPOSTALCODE</b></td>
    <td>Si</td>
    <td>Código Postal de la dirección de facturación</td>
    <td>Alfanumérico de 10 caracteres.</td>
    <td>Ejemplo: C1010AAP ó 1010</td>
  </tr>
  <tr>
    <td><b>CSBTSTATE</b></td>
    <td>Si</td>
    <td>Provincia de la dirección de facturación</td>
    <td>Alfanumérico de 2 caracteres.</td>
    <td><a href="#tablareferencia">Ver Provincias</a>
Ejemplo: Enviar C si corresponde a CABA</td>
  </tr>
  <tr>
    <td><b>CSBTSTREET1</b></td>
    <td>Si</td>
    <td>Domicilio de facturación (Calle Numero interior Numero Exterior)</td>
    <td>Alfanumérico de 60 caracteres.</td>
    <td>Ejemplo: Cerrito 740 piso 8</td>
  </tr>
  <tr>
    <td><b>CSBTSTREET2</b></td>
    <td>No</td>
    <td>Localidad</td>
    <td>Alfanumérico de 60 caracteres.</td>
    <td>Ejemplo: CABA</td>
  </tr>
  <tr>
    <td><b>CSPTCURRENCY</b></td>
    <td>Si</td>
    <td>Moneda</td>
    <td>Alfanumérico de 5 caracteres.</td>
    <td>Ejemplo: ARS</td>
  </tr>
  <tr>
    <td><b>CSPTGRANDTOTALAMOUNT</b></td>
    <td>Si</td>
    <td>"999999.CC"
Con decimales obligatorios, usando el puntos como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>Numérico de 15 posiciones</td>
    <td>Ejemplos:
$125,38 -> 125.38
$12 -> 12.00</td>
  </tr>
  <tr>
    <td><b>CSMDD6</b></td>
    <td>No</td>
    <td>Canal de venta</td>
    <td>Alfanumérico de 255 caracteres.</td>
    <td>Valor Ejemplo: Web, Mobile, Telefonica</td>
  </tr>
  <tr>
    <td><b>CSMDD7</b></td>
    <td>No</td>
    <td>Cantidad de días que está registrado el cliente en el sitio del comercio.</td>
    <td>Alfanumérico de 255 caracteres.</td>
    <td>Ejemplo: 178</td>
  </tr>
  <tr>
    <td><b>CSMDD8</b></td>
    <td>No</td>
    <td>Para indicar si el usuario está comprando como invitado en la página del comercio. En caso de ser "S", el campo CSMDD9 no deberá enviarse.</td>
    <td>Valor Booleano</td>
    <td>Valor Ejemplo (S/N)</td>
  </tr>
  <tr>
    <td><b>CSMDD9</b></td>
    <td>No</td>
    <td>Valor del password del usuario registrado en el portal del comercio. Incluir el valor en hash</td>
    <td>Alfanumérico de 255 caracteres.</td>
    <td>Ejemplo: "4ac1503de8c50a81213f2d5bac49628b"</td>
  </tr>
  <tr>
    <td><b>CSMDD10</b></td>
    <td>No</td>
    <td>Cantidad de transacciones realizadas por el mismo usuario registrado en el portal del comercio (Num transacciones)</td>
    <td>Alfanumérico de 255 caracteres.</td>
    <td>Ejemplo: 5</td>
  </tr>
  <tr>
    <td><b>CSMDD11</b></td>
    <td>No</td>
    <td>Celular del cliente</td>
    <td>Alfanumérico de 255 caracteres.</td>
    <td>Ejemplo: "1155001122"</td>
  </tr>
</table>

**Parámetros del vertical "Retail":**
<table>
  <tr>
    <th>Parámetro</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Descripción</th>
    <th>Valor Ejemplo</th>
  </tr>
  <tr>
    <td><b>CSSTCITY</b></td>
    <td>Si</td>
    <td>Ciudad de envío de la orden</td>
    <td>Alfanumérico de 50 caracteres</td>
    <td>Ejemplo: "Capital Federal"</td>
  </tr>
  <tr>
    <td><b>CSSTCOUNTRY</b></td>
    <td>Si</td>
    <td>País de envío de la orden</td>
    <td>Alfanumérico de 2 caracteres</td>
    <td>Código ISO</td>
  </tr>
  <tr>
    <td><b>CSSTEMAIL</b></td>
    <td>Si</td>
    <td>Correo electrónico del comprador</td>
    <td>Alfanumérico de 100 caracteres</td>
    <td>Ejemplo: "mail@empresa.com"</td>
  </tr>
  <tr>
    <td><b>CSSTFIRSTNAME</b></td>
    <td>Si</td>
    <td>Nombre de la persona que recibe el producto</td>
    <td>Alfanumérico de 60 caracteres</td>
    <td>Ejemplo: "Juan"</td>
  </tr>
  <tr>
    <td><b>CSSTLASTNAME</b></td>
    <td>Si</td>
    <td>Apellido de la persona que recibe el producto</td>
    <td>Alfanumérico de 60 caracteres</td>
    <td>Ejemplo: "Pérez"</td>
  </tr>
  <tr>
    <td><b>CSSTPHONENUMBER</b></td>
    <td>Si</td>
    <td>Número de teléfono del destinatario</td>
    <td>Alfanumérico de 15 caracteres</td>
    <td>Ejemplo: "45004500"</td>
  </tr>
  <tr>
    <td><b>CSSTPOSTALCODE</b></td>
    <td>Si</td>
    <td>Código postal del domicilio de envío</td>
    <td>Alfanumérico de 10 caracteres</td>
    <td>Ejemplos: "C1006DRW", "C1006" "1006"</td>
  </tr>
  <tr>
    <td><b>CSSTSTATE</b></td>
    <td>Si</td>
    <td>Provincia de envío</td>
    <td>Alfanumérico de 2 caracteres</td>
    <td>Es un carácter. <a href="#tablareferencia">Ver Provincias</a></td>
  </tr>
  <tr>
    <td><b>CSSTSTREET1</b></td>
    <td>Si</td>
    <td>Domicilio de envío</td>
    <td>Alfanumérico de 60 caracteres</td>
    <td>Ejemplo: "Cerrito 740"</td>
  </tr>
  <tr>
    <td><b>CSSTSTREET2</b></td>
    <td>No</td>
    <td>Datos adicionales del domicilio del envío</td>
    <td>Alfanumérico de 60 caracteres</td>
    <td>Ejemplo: "Piso 7"</td>
  </tr>
  <tr>
    <td><b>CSMDD12</b></td>
    <td>No</td>
    <td>Cantidad de días que tiene el comercio para hacer la entrega</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>Ejemplo: 10</td>
  </tr>
  <tr>
    <td><b>CSMDD13</b></td>
    <td>No</td>
    <td>Método de Despacho</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>Valor Ejemplo:  storepickup, deliverypropio, deliverycarrier <br>
    Notas:  deliverypropio: envío realizado con operador logístico propio.
    deliverycarrier: envío realizado con operador logístico tercerizado (ej. Andreani).
    </td>
    </tr>
  <tr>
    <td><b>CSMDD14</b></td>
    <td>No</td>
    <td>Valor booleano para identificar si el cliente requiere un comprobante fiscal o no S / N</td>
    <td>Valor Booleano</td>
    <td>S/N</td>
  </tr>
  <tr>
    <td><b>CSMDD15</b></td>
    <td>No</td>
    <td>CustomerLoyalityNumber - número de cliente frecuente</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>Ejemplo: "789875"</td>
  </tr>
  <tr>
    <td><b>CSMDD16</b></td>
    <td>No</td>
    <td>Promotional / CouponCode - número de cupón de descuento</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>Ejemplo: "PROMO30"</td>
  </tr>
</table>

**Datos a enviar por cada producto, los valores deben estar separados con "#":**

<table>
  <tr>
    <th><b>Parámetro</b></th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Descripción</th>
    <th>Valor Ejemplo</th>
  </tr>
  <tr>
    <td><b>CSITPRODUCTCODE</b></td>
    <td>Condicional</td>
    <td>Categoria del producto</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>Valor por defecto: default</td>
  </tr>
  <tr>
    <td><b>CSITPRODUCTDESCRIPTION</b></td>
    <td>Condicional</td>
    <td>Descripción del producto </td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>NOTEBOOK L845 SP4304LA DF TOSHIBA 4GB RAM 233 MHZ</td>
  </tr>
  <tr>
    <td><b>CSITPRODUCTNAME</b></td>
    <td>Condicional</td>
    <td>Nombre del producto</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>NOTEBOOK L845 SP4304LA DF TOSHIBA</td>
  </tr>
  <tr>
    <td><b>CSITPRODUCTSKU</b></td>
    <td>Condicional</td>
    <td>Código identificador del producto</td>
    <td>Alfanumérico de 255 caracteres</td>
    <td>Ejemplo: LEVJNSL36GN</td>
  </tr>
  <tr>
    <td><b>CSITTOTALAMOUNT</b></td>
    <td>Condicional</td>
    <td>CSITTOTALAMOUNT = CSITUNITPRICE * CSITQUANTITY
"999999.CC"
Es mandatorio informar los decimales, usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>Numérico</td>
    <td>Ejemplos:
$125,38 -> 125.38
$12 -> 12.00</td>
  </tr>
  <tr>
    <td><b>CSITQUANTITY</b></td>
    <td>Condicional</td>
    <td>Cantidad del producto</td>
    <td>Numérico</td>
    <td>Ejemplo: 1</td>
  </tr>
  <tr>
    <td><b>CSITUNITPRICE</b></td>
    <td>Condicional</td>
    <td>"999999.CC"
Es mandatorio informar los decimales, usando el punto como separador de decimales. No se permiten comas, ni como separador de miles ni como separador de decimales.</td>
    <td>Numérico</td>
    <td>Ejemplos:
$125,38 -> 125.38
$12 ->  12.00</td>
  </tr>
</table>

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


[<sub>Volver a inicio</sub>](#inicio)
<br>

<a name="opcionesadicionales"></a>
#### Opciones adicionales
Dentro del parámetro *$optionsSAR_operacion* pueden enviarse opciones adicionales que habilitan características para esa transacción en particular. A continuación se describen las mismas

<a name="coutas"></a>
##### Rango de Cuotas
Es posible setear el rango de cuotas a mostrar en el formulario entre un mínimo y un máximo, enviando los siguientes parametros adicionales

<table>
  <tr>
    <th>Campo</th>
    <th>Requerido</th>
    <th>Descripción</th>
    <th>Tipo de Dato</th>
    <th>Valor Ejemplo / Ejemplo</th>
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

```php
$optionsSAR_operacion = array (
...................................
	'MININSTALLMENTS'=>3,
	'MAXINSTALLMENTS'=>6,
...................................
```
