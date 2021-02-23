# Crear Formularios

Desafortunadamente, la entrada del usuario puede exponer su sitio a 
ataques maliciosos. Es importante verificar los datos enviados desde 
un formulario antes de aceptarlo. Aunque los elementos de formulario 
HTML5 validan la entrada del usuario en los navegadores modernos, 
aún debe verificar los datos en el servidor. La validación de HTML5 
ayuda a los usuarios legítimos a evitar enviar un formulario con 
errores, pero los usuarios malintencionados pueden eludir fácilmente 
las comprobaciones realizadas en el navegador. La validación del 
lado del servidor no es opcional, pero es esencial. Las soluciones 
PHP de este capítulo le muestran cómo filtrar o bloquear cualquier 
cosa sospechosa o peligrosa. Ninguna aplicación en línea es 
completamente a prueba de piratería, pero no se necesita mucho 
esfuerzo para mantener a raya a todos, excepto a los merodeadores 
más decididos. También es una buena idea conservar la entrada del 
usuario y volver a mostrarla si el formulario está incompleto o se 
descubren errores. 

Estas soluciones crean un script de procesamiento de correo completo 
que se puede reutilizar en diferentes formularios, por lo que es 
importante leerlas en secuencia.

El contenido de este capitulo incluye siguiente:

- Comprender cómo se transmite la entrada del usuario desde un 
formulario en línea
- Visualización de errores sin perder la entrada del usuario
- Validando la entrada del usuario
- Envío de información de usuario por correo electrónico

## Cómo PHP recopila información de un formulario

Aunque HTML contiene todas las etiquetas necesarias para construir un 
formulario, no proporciona ningún medio para procesar el formulario 
una ves este se envía. Para eso, se necesita una solución del lado del 
servidor, como PHP.

El sitio web de Japan Journey contiene un formulario de comentarios 
simple (ver Figura 6-1). Otros elementos, como botones de opción, 
casillas de verificación y menús desplegables, se agregarán más adelante.

Primero, echemos un vistazo al código HTML del formulario (está en 
`contact_01.php` en la carpeta ch06):


```html

<form method="post" action="">
    <p>
        <label for="name">Name:</label>
        <input name="name" id="name" type="text">
    </p>
    <p>
        <label for="email">Email:</label>
        <input name="email" id="email" type="text">
    </p>
    <p>
        <label for="comments">Comments:</label>
        <textarea name="comments" id="comments"></textarea>
    </p>
    <p>
        <input name="send" type="submit" value="Send message">
    </p>
</form>
```

Las dos primeras etiquetas _<input>_ y la etiqueta _<textarea>_ contienen 
atributos de nombre e identificación configurados con el mismo valor. 
La razón de esta duplicación es la accesibilidad. HTML usa el atributo 
_id_ para asociar el elemento _<label>_ con el elemento _<input>_ correcto. 
Los scripts de procesamiento de formularios, sin embargo, se basan en 
el atributo de _name_. Por lo tanto, aunque el atributo _id_ es opcional 
en el botón _submit_, debe usar el atributo _name_ para cada elemento 
del formulario que desee procesar.

El atributo _name_ de un elemento de entrada de formulario normalmente no 
debe contener espacios. Si desea combinar varias palabras, únalas con un 
guión bajo (PHP lo hará automáticamente si deja espacios). Debido a que el 
script desarrollado más adelante en este capítulo convierte los atributos 
_name_ en variables PHP, no use guiones ni ningún otro carácter que no sea 
válido en los nombres de las variables PHP.

Otras dos cosas a tener en cuenta son los atributos de _method_ y _action_
dentro de la etiqueta de apertura _<form>_. El atributo de _method_ determina 
cómo el formulario envía datos. Se puede configurar para _post_ y _get_. El 
atributo _action_ le dice al navegador dónde enviar los datos para su 
procesamiento cuando se hace clic en el botón _submit_. Si el valor se deja 
vacío, como aquí, la página intenta procesar el formulario en sí. Sin embargo, 
un atributo _action_ vacío no es válido en HTML5, por lo que será necesario corregirlo.

He evitado deliberadamente el uso de cualquiera de las nuevas funciones 
de formulario HTML5, como _type = "email"_ y el atributo _required_. Esto 
facilita la prueba de los scripts de validación del lado del servidor PHP. 
Después de la prueba, puede actualizar sus formularios para utilizar las 
funciones de validación de HTML5. La validación en el navegador es 
principalmente una cortesía hacia el usuario para evitar que se envíe 
información incompleta, por lo que es opcional. La validación del lado 
del servidor nunca debe omitirse.

## Diferencia entre Post y Get


De lo contrario, la carpeta ch06 contiene un conjunto completo de archivos 
para el sitio Japan Journey con todo el código del Capítulo 5 incorporado. 
Copie _contact_01.php_ a la raíz del sitio y cámbielo al nombre _contact.php_. 
También copie _footer.php_, _menu.php_ y _title.php_ de la carpeta _ch06 / includes_
a la carpeta _includes_ en la raíz del sitio.

1. Localice la etiqueta de apertura _<form>_ en _contact.php_ y cambie el valor 
del atributo del método de _post_ para obtener, así:

```html
<form method="get" action="">
```

2. Guarde _contact.php_ y cargue la página en un navegador. Escriba su nombre, 
dirección de correo electrónico y un mensaje corto en el formulario, luego haga 
clic en _Enviar mensaje_.


3. Busque en la barra de direcciones del navegador. Debería ver el contenido 
del formulario adjunto al final de la URL.  Si divide la URL, se verá así:

```
http://localhost/phpsols-4e/contact.php
?name=David
&email=david%40example.com
&comments=Greetings%21+%3A-%29
&send=Send+message
```

Los datos enviados por el formulario se han agregado a la URL básica 
como una cadena de consulta que comienza con un signo de interrogación. 
El valor de cada campo y el botón de envío se identifica mediante el 
atributo _name_ del elemento de formulario, seguido de un signo igual y 
los datos enviados. Los datos de cada elemento de entrada están separados 
por un signo comercial (&). Las URL no pueden contener espacios o ciertos 
caracteres (como un signo de exclamación o un emoticón), por lo que el 
navegador reemplaza los espacios con + y codifica otros caracteres como 
valores hexadecimales, un proceso conocido como codificación de URL (para 
obtener una lista completa de valores, consulte 
www.degraeve.com/reference/urlencoding.php).
 
4. Vuelva al código de _contact.php_ y cambie el método a _post_, así:

```html
<form method="post" action="">
```

Guarde _contact.php_ y vuelva a cargar la página en su navegador. Escribe 
otro mensaje y haz clic en _Enviar mensaje_. Su mensaje debería desaparecer, 
pero no sucede nada. No se ha perdido, pero todavía no ha hecho nada para 
procesarlo.
 
6. En _contact.php_, agregue el siguiente código inmediatamente debajo de 
la etiqueta de cierre `</form>`:

```html
<pre>
<?php if ($_POST) { print_r($_POST); } ?>
</pre>
```

Esto muestra el contenido del array superglobal `$ _POST` si se han 
enviado datos _post_. Como se explicó en el Capítulo 4, la función 
_print_r()_ le permite inspeccionar el contenido de los array; las 
etiquetas `<pre>` simplemente facilitan la lectura de la salida.

7. Guarde la página y haga clic en el botón Actualizar en su navegador. 
Probablemente verá una advertencia similar a la siguiente. Esto le 
indica que los datos se volverán a enviar, que es exactamente lo que 
desea. Confirme que desea enviar la información nuevamente.

8. El código del paso 6 ahora debería mostrar el contenido de su mensaje 
debajo del formulario. Todo se ha almacenado en uno de los array 
superglobales de PHP, `$ _POST`, que contiene los datos enviados mediante 
el método _post_. El atributo _name_  de cada elemento del formulario 
se utiliza como clave del array, lo que facilita la recuperación del contenido.

```
       array
(
    [name] => David
    [email] => david@example.com
    [comments] => Hi!
    [send] => Send message
)
```

El array `$ _POST` usa los atributos `name` del formulario para 
identificar cada elemento de datos.

Como acaba de ver, el método `get` envía sus datos adjuntos a la URL, 
mientras que el método `post` los envía con los encabezados HTTP para 
que estén ocultos a la vista. Algunos navegadores limitan la longitud 
máxima de una URL a unos 2000 caracteres, por lo que el método `get` 
solo se puede utilizar para pequeñas cantidades de datos. El método 
`post` se puede utilizar para cantidades de datos mucho mayores. De 
forma predeterminada, PHP permite hasta 8 MB de datos `post` aunque las 
empresas de alojamiento pueden establecer un límite diferente.

Sin embargo, la diferencia más importante entre los dos métodos es su 
uso previsto. El método de `get` está diseñado para usarse en solicitudes 
que no generan cambios en el servidor, sin importar cuántas veces se 
haya realizado. En consecuencia, se utiliza principalmente para búsquedas 
en bases de datos; marcar el resultado de la búsqueda es útil porque todos 
los criterios de búsqueda están en la URL. Por otro lado, el método 
`post` está diseñado para peticiones que provocan cambios en el servidor. 
Por lo tanto, se usa para insertar, actualizar o eliminar registros en 
una base de datos, cargar archivos o enviar un correo electrónico.

Este capítulo se concentra en el método `post` y su array superglobal 
asociada, `$ _POST`.

## Obtener datos de formularios con PHP superglobals

El array superglobal `$_POST` contiene datos enviados mediante el 
método `post`. No debería sorprender que los datos enviados por el 
método `get` estén en el array `$_GET`.

Para acceder a los valores enviados por un formulario, simplemente 
coloque el atributo `name` elemento del formulario entre comillas 
entre corchetes después de `$ _POST` o `$ _GET`, según el atributo 
del método del formulario. Por tanto, el correo electrónico se convierte 
en `$_POST['email']` si se envía mediante el método `post` y 
`$_GET['email']` si se envía mediante el método `get`. 

Puede encontrar scripts que usen `$_REQUEST`, lo que evita la necesidad 
de distinguir entre `$_POST` o `$_GET`. Es menos seguro. Siempre 
debe saber de dónde proviene la información del usuario. `$_REQUEST` 
también incluye los valores de las cookies, por lo que no tiene idea 
si está tratando con un valor enviado por el método `post`, uno 
transmitido a través de la URL o inyectado por una cookie. Utilice 
siempre `$_POST` o `$_GET`.

Los scripts antiguos pueden usar `$HTTP_POST_VARS` o `$HTTP_GET_VARS`, 
que tienen el mismo significado que `$_POST` y `$_GET`. Se han 
eliminado las versiones antiguas. Utilice `$_POST` y `$_GET` en su lugar.


## Procesamiento y validación de la entrada del usuario

El objetivo final de este capítulo es enviar la entrada del formulario 
en `contact.php` por correo electrónico a su bandeja de entrada. El uso 
de la función `mail ()` de PHP es relativamente sencillo. Requiere un 
mínimo de tres argumentos: la dirección o direcciones a las que se envía 
el correo electrónico, una cadena que contiene la línea de asunto y una 
cadena que contiene el cuerpo del mensaje. El cuerpo del mensaje se 
construye concatenando (uniendo) el contenido de los campos de entrada 
en una sola cadena.

Las medidas de seguridad implementadas por la mayoría de los proveedores 
de servicios de Internet (ISP) hacen que sea difícil, si no imposible, 
probar la función `mail ()` en un entorno de prueba local. En lugar de 
saltar directamente al uso de `mail ()`, las Soluciones PHP 6-2 a 6-5 
se concentran en validar la entrada del usuario para asegurarse de que 
los campos obligatorios estén completos y muestren mensajes de error. 
La implementación de estas medidas hace que sus formularios en línea 
sean más fáciles de usar y seguros.

La utilizacion de JavaScript o elementos y atributos de formulario HTML5
para verificar la entrada del usuario se denomina validación del lado del 
cliente porque ocurre en la computadora del usuario (o cliente). Es útil 
porque es casi instantáneo y puede alertar al usuario sobre un problema 
sin hacer un viaje de ida y vuelta innecesario al servidor. Sin embargo, 
la validación del lado del cliente es fácil de eludir. Todo lo que tiene 
que hacer un usuario malintencionado es enviar datos desde un script 
personalizado y sus comprobaciones se vuelven inútiles. También es vital 
verificar la entrada del usuario con PHP.

Consejo

La validación del lado del cliente por sí sola es insuficiente. Siempre 
verifique los datos de una fuente externa usando la validación del lado 
del servidor con PHP.

### Creando un script reutilizable

La capacidad de reutilizar el mismo script, tal vez con solo unas pocas 
ediciones, para varios sitios web es un gran ahorro de tiempo. Sin embargo, 
enviar los datos de entrada a un archivo separado para su procesamiento 
dificulta alertar a los usuarios sobre errores sin perder su entrada. Para 
solucionar este problema, el enfoque adoptado en este capítulo es utilizar 
lo que se conoce como formulario de autoprocesamiento.

Cuando se envía el formulario, la página se vuelve a cargar y una 
declaración condicional ejecuta el script de procesamiento. Si la 
validación del lado del servidor detecta errores, el formulario se puede 
volver a mostrar con mensajes de error mientras se conserva la entrada 
del usuario. Partes del script específicas del formulario se incrustarán 
encima de la declaración `DOCTYPE`. Las partes genéricas y reutilizables 
estarán en un archivo separado que se puede incluir en cualquier página 
que requiera un script de procesamiento de correo electrónico.

Solución PHP 6-1: Prevención de secuencias de comandos entre sitios en 
una forma de autoprocesamiento

Dejar vacío el atributo `action` de una etiqueta de formulario de 
apertura u omitirlo por completo vuelve a cargar el formulario cuando 
se envían los datos. Sin embargo, un atributo `action` vacío no es 
válido en HTML5. PHP tiene una variable superglobal muy conveniente 
(`$_SERVER ['PHP_SELF']`) que contiene la ruta relativa a la raíz del 
sitio del archivo actual. Establecerlo como el valor del atributo 
`action` inserta automáticamente el valor correcto para un formulario 
de autoprocesamiento, pero usarlo por sí solo expone su sitio a un 
ataque malicioso conocido como `cross-site scripting (XSS)`. Esta 
solución PHP explica el riesgo y muestra cómo usar `$_SERVER['PHP_SELF']` 
de forma segura.

1. Cargue `bad_link.php` en la carpeta ch06 en un navegador. Contiene 
un único enlace a `form.php` en la misma carpeta; pero el enlace en el 
HTML subyacente se ha deformado deliberadamente para simular un ataque XSS.
     
2. Haga clic en el enlace. Dependiendo del navegador que esté utilizando, 
debería ver que la página de destino ha sido bloqueada (como se muestra 
en la Figura 6-3) o el diálogo de alerta de JavaScript que se muestra en 
la Figura 6-4.

Google Chrome automatically blocks suspected XSS attacks. 
Not all browsers are capable of blocking XSS attacks. 

Nota

Los enlaces en los archivos de ejercicios para esta solución PHP asumen 
que están en una carpeta llamada phpsols-4e / ch06 en la raíz de su 
servidor localhost. Ajústelos si es necesario para que coincidan con 
su configuración de prueba.

3. Descarte la alerta de JavaScript, si es necesario, y haga clic con 
el botón derecho para ver la fuente de la página. La línea 10 debería 
verse similar a esto:

```
<form method="post" action="/phpsols-4e/ch06/form.php">
<script>alert('Boo!')</script><foo"">
```

El enlace mal formado en `bad_link.php` ha inyectado un fragmento de 
JavaScript en la página inmediatamente después de la etiqueta de 
apertura `<form>`. En este caso, es una alerta JavaScript inofensiva; 
pero en un ataque XSS real, podría intentar robar cookies u otra 
información personal. Tal ataque sería silencioso, dejando al usuario 
inconsciente de lo que ha sucedido a menos que note el script en la 
barra de direcciones del navegador.

Esto ha sucedido porque `form.php` usa `$_SERVER['PHP_SELF']` para 
generar el valor del atributo `action`. El enlace mal formado inserta 
la ubicación de la página en el atributo `action`, cierra la etiqueta 
del formulario de apertura y luego inyecta la etiqueta `<script>`, que 
se ejecuta inmediatamente cuando se carga la página.


4. Una forma simple, pero efectiva, de neutralizar este tipo de ataque 
XSS es pasar `$_SERVER['PHP_SELF']` a la función `htmlentities()` 
de esta manera:

```html
<form method="post"  action="<?=
 htmlentities($_SERVER['PHP_SELF']) ?>">
```

Esto convierte los corchetes angulares de las etiquetas `<script`> en 
sus equivalentes de entidad HTML, evitando que se ejecute el script. 
Aunque funciona, deja la URL mal formada en la barra de direcciones 
del navegador, lo que podría llevar a los usuarios a cuestionar la 
seguridad de su sitio. Creo que una mejor solución es redirigir a los 
usuarios a una página de error cuando se detecta XSS.


5. En form.php, cree un bloque PHP encima de la declaración `DOCTYPE` 
y defina una variable con la ruta relativa a la raíz del sitio al 
archivo actual de esta manera:

```html
<?php
$currentPage = '/phpsols-4e/ch06/form.php';
?>
<!doctype html>
```

6. Ahora compare el valor de `$currentPage` con `$ _SERVER['PHP_SELF']`. 
Si no son idénticos, use la función `header()` para redirigir al usuario 
a una página de error y salir inmediatamente del script de esta manera:

```html
if ($currentPage !== $_SERVER['PHP_SELF']) {
    header('Location: http://localhost/phpsols-4e/ch06/missing.php');
    exit;
}
```

Precaución

La ubicación pasada a la función `header()` debe ser una URL completamente 
calificada. Si utiliza un vínculo relativo al documento, el destino se 
agrega al vínculo mal formado, lo que evita que la página se redirija 
correctamente.

7. Use `$currentPage` como el valor del atributo `action` en la etiqueta 
del formulario de apertura:

```html
<form method="post"  action="<?= $currentPage ?>">
```

8. Guarde `form.php`, vuelva a `bad_link.php` y vuelva a hacer clic en el 
enlace. Esta vez debería ser llevado directamente a `missing.php`.
 
9. Cargue `form.php` directamente en el navegador. Debería cargarse y 
funcionar como se esperaba.  La versión final está en `form_end.php` en 
la carpeta ch06. El archivo llamado `bad_link_end.php` enlaza con la 
versión terminada si solo desea probar el script.

Esta técnica implica más código que simplemente pasar `$_SERVER['PHP_SELF']` 
a la función `htmlentities()`; pero tiene la ventaja de guiar a los 
usuarios sin problemas a una página de error si han seguido un enlace 
malicioso a su formulario. Obviamente, la página de error debería 
vincularse a su menú principal.

Solución PHP 6-2: Asegúrese de que los campos obligatorios no estén en blanco

Cuando los campos obligatorios se dejan en blanco, no obtiene la 
información que necesita y es posible que el usuario nunca obtenga una 
respuesta, especialmente si se han omitido los datos de contacto.
Continúe usando el archivo de "Comprender la diferencia entre `post` y `get`" 
anteriormente en este capítulo. Alternativamente, use `contact_02.php` de 
la carpeta ch06 y elimine _02 del nombre del archivo.

1. La secuencia de comandos de procesamiento utiliza dos arrays llamadas 
`$errores` y` $missing` para almacenar los detalles de los errores y los 
campos obligatorios que no se han completado. Estos arrays se utilizarán 
para controlar la visualización de los mensajes de error junto con las
 etiquetas del formulario. No habrá ningún error cuando la página se 
cargue por primera vez, así que inicialice `$errors` y `$missing` como 
arrays vacíos en el bloque de código PHP en la parte superior de 
`contact.php`, así:


```html
<?php
include './includes/title.php';
$errors = [];
$missing = [];
?>
```

 
2. La secuencia de comandos de procesamiento de correo electrónico debe 
ejecutarse solo si se ha enviado el formulario. Use una declaración 
condicional para verificar el valor de la variable superglobal 
`$_SERVER['REQUEST_METHOD']`. Si es `POST` (todo en mayúsculas), sabrá 
que el formulario se envió mediante el método `post`. Agregue el código 
resaltado en negrita al bloque PHP en la parte superior de la página.

```html
<?php
include './includes/title.php';
$errors = [];
$missing = [];
// check if the form has been submitted
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // email processing script
}
?>
```

&-------------------------------------------------------------------
