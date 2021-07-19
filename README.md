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


