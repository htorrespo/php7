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

Consejo

Verificar que el valor de `$_SERVER['REQUEST_METHOD']` sea `POST` es 
una condición genérica que se puede usar con cualquier formulario 
independientemente del nombre del botón Enviar.

3. Aunque aun no enviará el correo electrónico, defina dos variables 
para almacenar la dirección de destino y la línea de asunto del 
correo electrónico. El siguiente código va dentro de la declaración 
condicional que creó en el paso anterior:

```
if ( $_SERVER['REQUEST_METHOD'] == 'POST') {
    // email processing script
    $to = 'david@example.com'; // use your own email address
    $subject = 'Feedback from Japan Journey';
}
```

4. A continuación, cree dos arrays, uno que enumere el atributo `name` 
de cada campo en el formulario y la otra que enumere todos los campos 
obligatorios. Por el bien de esta demostración, haga que el campo de 
correo electrónico sea opcional, de modo que solo se requieran los 
campos `name` y `comments` Agregue el siguiente código dentro del 
bloque condicional inmediatamente después del código que define la 
línea de asunto:

```
    $subject = 'Feedback from Japan Journey';
    // list expected fields
    $expected = ['name', 'email', 'comments'];
    // set required fields
    $required = ['name', 'comments'];
}
```

Consejo

¿Por qué es necesaria ek array `$expected` ? Es para evitar que un 
atacante inyecte otras variables en el array `$_POST` en un intento de 
sobrescribir sus valores predeterminados. Al procesar solo las variables 
que espera, su formulario es mucho más seguro. Se ignoran los valores 
falsos.

5. La siguiente sección de código no es específica de este formulario, 
por lo que debe ir en un archivo externo que se puede incluir en 
cualquier secuencia de comandos de procesamiento de correo electrónico. 
Cree un nuevo archivo PHP llamado `processmail.php` en la carpeta 
`includes`. Luego inclúyalo en `contact.php` inmediatamente después 
del código que ingresó en el paso anterior, así:

```
    $required = ['name', 'comments'];
    require './includes/processmail.php';
}
```

6. El código en `processmail.php` comienza verificando las variables 
`$_POST` para los campos obligatorios que se han dejado en blanco. 
Elimine cualquier código predeterminado insertado por su editor y 
agregue lo siguiente a `processmail.php`: 

```
<?php
foreach ($_POST as $key => $value) {
    // strip whitespace from $value if not an array
    if (!is_array($value)) {
        $value = trim($value);
    }
    if (!in_array($key, $expected)) {
        // ignore the value, it's not in $expected
        continue;
    }
    if (in_array($key, $required) && empty($value)) {
        // required value is missing
        $missing[] = $key;
        $$key = "";
        continue;
    }
    $$key = $value;
}
```

Este bucle `foreach` procesa el array `$_POST` eliminando los espacios 
en blanco iniciales y finales de los campos de texto y asignando el 
contenido del campo a una variable con un nombre simplificado. 
Como resultado, `$_POST['email'] `se convierte en `$email`, y así 
sucesivamente. También verifica si los campos obligatorios se dejan en 
blanco y los agrega al array `$missing`, estableciendo la variable 
relacionada en una cadena vacía.

El array `_POST` es un array asociativa, por lo que el ciclo asigna la 
clave y el valor del elemento actual a `$key` y `$value`, respectivamente. 
El ciclo comienza verificando que el valor actual no sea un array, 
usando la función `is_array()` con el operador lógico `Not` (!). Si no 
es así, la función `trim()` elimina los espacios en blanco iniciales 
y finales y los reasigna a `$value`. Eliminar los espacios en blanco 
iniciales y finales evita que alguien presione la barra espaciadora 
varias veces para evitar completar un campo obligatorio.


Nota

Actualmente, el formulario solo tiene campos de entrada de texto, pero 
se ampliará más adelante para incluir elementos `<select>` y casillas 
de verificación que envían datos como arrays. Es necesario verificar 
si el valor del elemento actual es un array porque pasar un array a la 
función `trim()` desencadena un error.

La siguiente declaración condicional verifica si la clave actual no 
está en el array `$expected`. Si no es así, la palabra clave `continue` 
obliga al ciclo a dejar de procesar el elemento actual y pasar al 
siguiente. Por lo tanto, se ignora todo lo que no esté en el array 
$expected.

A continuación, verificamos si la clave del array actual está en el 
array `$required` y si no tiene ningún valor. Si la condición devuelve 
verdadera, la clave se agrega al arrayz `$missing` y una variable 
basada en el nombre de la clave se crea dinámicamente y su valor se 
establece en una cadena vacía. Observe que `$$key` comienza con dos 
signos de dólar en la siguiente línea:

```
$$key = "";
```

Esto significa que es una variable variable (consulte "Crear de nuevas 
variables dinámicamente" en el Capítulo 4). Entonces, si el valor de 
`$key` es `"name"`, `$$key` se convierte en `$name`.

Nuevamente, `continue` mueve el bucle al siguiente elemento.

Pero, si llegamos hasta la línea final del ciclo, sabemos que estamos 
tratando con un elemento que necesita ser procesado, por lo que se 
crea una variable basada en el nombre de la clave de forma dinámica y 
el valor actual se asigna a este.

7. Guarde `processmail.php`. Le agregará más código más adelante, pero 
pasemos ahora al cuerpo principal de `contact.php`. El atributo `action` 
en la etiqueta del formulario de apertura está vacío. Para propósitos 
de prueba local, simplemente establezca su valor en el nombre de la 
página actual:

```
<form method="post" action="contact.php">
```


8. Debe mostrar una advertencia si falta algo. Agregue una declaración 
condicional en la parte superior del contenido de la página entre el 
encabezado <h2> y el primer párrafo, así:


```html
<h2>Contact us</h2>
<?php if ($missing || $errors) { ?>
<p class="warning">Please fix the item(s) indicated.</p>
<?php } ?>
<p>Ut enim ad minim veniam . . . </p>
```


Esto verifica `$missing` y `$errors`, que inicializó como arrays vacios 
en el paso 1. Como se explica en "La verdad según PHP" en el Capítulo 4, 
un array vacío se trata como falso, por lo que el párrafo dentro de la 
declaración condicional no lo es se muestra cuando se carga la página 
por primera vez. Sin embargo, si no se ha completado un campo obligatorio 
cuando se envía el formulario, su nombre se agrega a la matriz `$missing`. 
Un array con al menos un elemento se trata como verdadera. El || significa 
"o", por lo que este párrafo de advertencia se mostrará si un campo 
obligatorio se deja en blanco o si se descubre un error. (El array 
$erros entra en juego en la Solución PHP 6-4).


9. Para asegurarse de que funciona hasta ahora, guarde `contact.php` 
y cárguelo normalmente en un navegador (no haga clic en el botón 
Actualizar). No se muestra el mensaje de advertencia. Haz clic en 
Enviar mensaje sin completar ninguno de los campos. Ahora debería ver 
el mensaje sobre elementos faltantes, como se muestra en la siguiente 
captura de pantalla.


10. Para mostrar un mensaje adecuado junto a cada campo obligatorio 
que falta, use una declaración condicional de PHP para insertar un 
`<span>` dentro de la etiqueta `<label>`, así:

```html
<label for="name">Name:
<?php if (in_array('name', $missing)) { ?>
    <span class="warning">Please enter your name</span>
<?php } ?>
</label>
```

La condición usa la función `in_array()` para verificar si el array 
`$missing` contiene el valor `name`. Si es así, se muestra el `<span>.` 
`$missing` se define como un array vacío en la parte superior de la 
secuencia de comandos, por lo que el intervalo no se mostrará cuando 
se cargue la página por primera vez.


11. Inserte advertencias similares para los campos de correo electrónico 
y comentarios como este:

```html
    <label for="email">Email:
    <?php if (in_array('email', $missing)) { ?>
        <span class="warning">Please enter your email address</span>
    <?php } ?>
    </label>
    <input name="email" id="email" type="text">
</p>
<p>
    <label for="comments">Comments:
    <?php if (in_array('comments', $missing)) { ?>
        <span class="warning">Please enter your comments</span>
    <?php } ?>
    </label>
```

El código PHP es el mismo excepto por el valor que busca en el array 
`$missing`. Es lo mismo que el atributo `name` del elemento de formulario.
 
12. Guarde `contact.php` y pruebe la página nuevamente, primero 
ingresando nada en ninguno de los campos. Las etiquetas del formulario 
deben verse como la Figura 6-5.


Aunque agregó una advertencia a `<label>` para el campo de correo 
electrónico, no se muestra porque el correo electrónico no se agregó 
al array `$required`. Como resultado, el código en `processmail.php` 
no lo agrega al array `$missing`.

13. Agregue un correo electrónico al array `$required` en el bloque 
de código en la parte superior de `comments.php`, así:

```
$required = ['name', 'comments', 'email'];
```

14. Haz clic en Enviar mensaje nuevamente sin completar ningún campo. 
Esta vez, verá un mensaje de advertencia junto a cada etiqueta.
     
15. Escriba su nombre en el campo `name`. En los campos Correo 
electrónico y Comentarios, simplemente presione la barra espaciadora 
varias veces y luego haga clic en Enviar mensaje. El mensaje de 
advertencia junto al campo Nombre desaparece, pero los otros dos 
mensajes de advertencia permanecen. El código en `processmail.php` 
quita los espacios en blanco de los campos de texto, por lo que 
rechaza los intentos de omitir los campos obligatorios ingresando 
una serie de espacios. Si tiene algún problema, compare su código 
con `contact_03.php` e `includes/processmail_01.php` en la carpeta 
ch06.
     

Todo lo que se necesita hacer para cambiar los campos obligatorios 
es cambiar los nombres en el array `$required` y agregar una alerta 
adecuada dentro de la etiqueta `<label>` del elemento de entrada 
apropiado dentro del formulario. Es fácil de hacer porque siempre 
usa el atributo de nombre del elemento de entrada del formulario.

## Conservar la entrada del usuario cuando un formulario está incompleto

Imagínese que ha pasado 10 minutos rellenando un formulario. Hace 
clic en el botón Enviar y vuelve la respuesta de que falta un campo 
obligatorio. Es exasperante tener que rellenar todos los campos de 
nuevo. Dado que el contenido de cada campo está en el array `$_POST`, 
es fácil volver a mostrarlo cuando se produce un error.


Solución PHP 6-3: Creación de campos de formulario fijos

Esta solución PHP muestra cómo usar una declaración condicional para 
extraer la entrada del usuario del array `$_POST` y volver a mostrarla 
en los campos de entrada de texto y áreas de texto. Continúe trabajando 
con los mismos archivos de antes. Alternativamente, use `contact_03.php` 
e `includes/processmail_01.php` de la carpeta ch06.


1. Cuando la página se carga por primera vez, no desea que aparezca 
nada en los campos de entrada, pero desea volver a mostrar el contenido 
si falta un campo obligatorio o hay un error. Esa es la clave: si los 
arrays `$missing` o `$errors` contienen algún valor, el contenido de 
cada campo debe volver a mostrarse. Establece el texto predeterminado 
para un campo de entrada de texto con el atributo de valor de la 
etiqueta `<input>`, así que modifique la etiqueta `<input`> para un 
nombre como este:

```html
<input name="name" id="name" type="text"
<?php if ($missing || $errors) {
    echo 'value="' . htmlentities($name) . '"';
} ?>>
```

La línea dentro de las llaves contiene una combinación de comillas y 
puntos que pueden confundirlo. Lo primero que debe darse cuenta es que 
solo hay un punto y coma, justo al final, por lo que el comando echo 
se aplica a toda la línea. Como se explicó en el Capítulo 3, un punto 
se denomina operador de concatenación, que une cadenas y variables. 
Puede dividir el resto de la línea en tres secciones, de la siguiente manera:

```
'value="' .

htmlentities($name)

. '"'
```

La primera sección genera `value="` como texto y usa el operador de 
concatenación para unirlo a la siguiente sección, que pasa `$name` a 
una función llamada `htmlentities()`. Explicaré por qué es necesario 
en un momento, pero la tercera sección usa el operador de concatenación 
nuevamente para unirse a la salida final, que consta únicamente de 
comillas dobles. Por lo tanto, si `$missing` o `$errors` contienen 
algún valor, y `$_POST['name']` contiene a Joe, terminará con esto 
dentro de la etiqueta `<input>` :

```html
<input name="name" id="name" type="text" value="Joe">
```

La variable `$name` contiene la entrada del usuario original, que se 
transmitió a través del array `$_POST`. El bucle `foreach` que creó en 
`processmail.php` en la Solución PHP 6-2 procesa el array `$_POST` y 
asigna cada elemento a una variable con el mismo nombre. Esto le permite 
acceder a `$_POST['nombre']` simplemente como `$name`.

Entonces, ¿por qué necesitamos la función `htmlentities()`? Como sugiere 
el nombre de la función, convierte ciertos caracteres en sus entidades 
de caracteres HTML equivalentes. El que le preocupa aquí es la comilla 
doble. Supongamos que Eric "Slowhand" Clapton decide enviar comentarios 
a través del formulario. Si usa `$name` por sí solo, la Figura 6-6 muestra 
lo que sucede cuando se omite un campo `required` y no usa `htmlentities()`.

Pasar el contenido del elemento array `$_POST` a `htmlentities()`, sin 
embargo, convierte las comillas dobles en el medio de la cadena a `&quot;`. 
Y, como muestra la Figura 6-7, el contenido ya no está truncado.


Lo bueno de esto es que la entidad de carácter `&quot;` se convierte 
de nuevo a comillas dobles cuando se vuelve a enviar el formulario. 
Como resultado, no es necesario realizar más conversiones antes de que 
se pueda enviar el correo electrónico.

Nota

Si `htmlentities()` corrompe su texto, puede establecer la codificación 
directamente dentro de un script pasando el segundo y tercer argumento 
opcional a la función. Por ejemplo, para establecer la codificación en 
chino simplificado, use `htmlentities($name, ENT_COMPAT, 'GB2312')`. 
Para obtener más información, consulte la documentación en www.php.net/manual/en/function.htmlentities.php.

2. Edite el campo de correo electrónico de la misma manera, usando 
`$email` en lugar de `$name`.
     
3. El área de texto de comentarios debe manejarse de manera ligeramente 
diferente porque las etiquetas `<textarea>` no tienen un atributo 
`value`. Debe colocar el bloque PHP entre las etiquetas de apertura y
 cierre del área de texto, así:

```html
<textarea name="comments" id="comments"><?php
  if ($missing || $errors) {
      echo htmlentities($comments);
  } ?></textarea>
```

Es importante colocar las etiquetas PHP de apertura y cierre contra 
las etiquetas `<textarea>`. Si no lo hace, obtendrá espacios en blanco 
no deseados dentro del área de texto.
 
4. Guarde `contact.php` y pruebe la página en un navegador. Si se 
omite algún campo obligatorio, el formulario muestra el contenido 
original junto con los mensajes de error.

Puede verificar su código con `contact_04.php` en la carpeta ch06.


Precaución

El uso de esta técnica evita que el botón de restablecimiento de un 
formulario restablezca cualquier campo que haya sido modificado por 
el script PHP porque establece explícitamente el atributo de valor de 
cada campo.


## Filtrar posibles ataques

Un exploit particularmente desagradable conocido como inyección de 
encabezado de correo electrónico busca convertir formularios en línea 
en transmisores de spam. El atacante intenta engañar a su script para 
que envíe un correo electrónico HTML con copias a muchas personas. Esto 
es posible si incorpora la entrada del usuario sin filtrar en los 
encabezados adicionales que se pueden pasar como el cuarto argumento a 
la función `mail()`. Es común agregar la dirección de correo electrónico 
del usuario como un encabezado de respuesta. Si detecta espacios, nuevas 
líneas, retornos de carro o cualquiera de las cadenas "Content-type:", 
"Cc:" o "Bcc:" en el valor enviado, es el objetivo de un ataque, por lo 
que debe bloquear el mensaje.

Solución PHP 6-4: bloqueo de direcciones de correo electrónico que 
contienen contenido sospechoso

Esta solución PHP verifica la entrada de la dirección de correo 
electrónico del usuario en busca de contenido sospechoso. Si se detecta, 
una variable booleana se establece en verdadera. Esto se utilizará más 
adelante para evitar que se envíe el correo electrónico.

Continúe trabajando con la misma página que antes. Alternativamente, 
use `contact_04.php` e `includes/processmail_01.php` de la carpeta ch06.

1. Para detectar las frases sospechosas, usaremos un patrón de búsqueda 
o una expresión regular. Agregue el siguiente código en la parte superior 
de `processmail.php` antes del bucle `foreach` existente:

```html
// pattern to locate suspect phrases
$pattern = '/[\s\r\n]|Content-Type:|Bcc:|Cc:/i';
foreach ($_POST as $key => $value) {
```

La cadena asignada a `$pattern` se utilizará para realizar una búsqueda 
que no distinga entre mayúsculas y minúsculas para cualquiera de los 
siguientes: un espacio, retorno de carro, salto de línea, "Content-Type:", 
"Bcc:" o "Cc:". Está escrito en un formato llamado expresión regular 
compatible con Perl (PCRE). El patrón de búsqueda está encerrado en un 
par de barras diagonales, y la i después de la barra final hace que el 
patrón no distinga entre mayúsculas y minúsculas.

Consejo

Las expresiones regulares son una herramienta extremadamente poderosa 
para hacer coincidir patrones de texto. Es cierto que no son fáciles 
de aprender; pero es una habilidad esencial si se toma en serio el 
trabajo con lenguajes de programación como PHP y JavaScript. Eche un 
vistazo a Introducción a las expresiones regulares de Jörg Krause 
(Apress, 2017, ISBN 978-1-4842-2508-0). Está dirigido principalmente 
a desarrolladores de JavaScript, pero solo existen pequeñas diferencias 
de implementación entre JavaScript y PHP. La sintaxis básica es idéntica.

2. Ahora puede usar el PCRE almacenado en `$pattern` para detectar 
cualquier entrada de usuario sospechosa en la dirección de correo 
electrónico enviada. Agregue el siguiente código inmediatamente después 
de la variable `$pattern` del paso 1:

```html
// check the submitted email address
$suspect = preg_match($pattern,  $_POST['email']);
```

La función `preg_match()` compara la expresión regular pasada como 
primer argumento con el valor del segundo argumento, en este caso, 
el valor del campo de correo electrónico. Devuelve verdadero si 
encuentra una coincidencia. Por lo tanto, si se encuentra contenido 
sospechoso, `$suspect` será cierto. Pero si no hay coincidencia, 
será falso.

3. Si se detecta contenido sospechoso en la dirección de correo 
electrónico, no tiene sentido seguir procesando el array `$_POST`. 
Envuelva el código que procesa las variables `$_POST` en una 
declaración condicional como esta:

```html
if (!$suspect) {
    foreach ($_POST as $key => $value) {
        // strip whitespace from $value if not an array
        if (!is_array($value)) {
           $value = trim($value);
        }
        if (!in_array($key, $expected)) {
            // ignore the value, it's not in $expected
            continue;
        }
        if (in_array($key, $required) && empty($value)) {
            // required value is missing
            $missing[] = $key;
            $$key = "";
            continue;
        }
    $$key = $value;
    }
}
```

Esto procesa las variables en el array `$_POST` solo si `$suspect`no 
es verdadero.

No olvide la llave extra para cerrar la declaración condicional.

4. Edite el bloque PHP después del encabezado `<h2>` en `contact.php` 
para agregar un nuevo mensaje de advertencia sobre el formulario, 
como este:

```html
<h2>Contact Us</h2>
<?php if ($_POST && $suspect) { ?>
    <p class="warning">Sorry, your mail could not be sent.
    Please try later.</p>
<?php } elseif ($missing || $errors) { ?>
  <p class="warning">Please fix the item(s) indicated.</p>
<?php } ?>
```

Esto establece una nueva condición que tiene prioridad sobre el mensaje 
de advertencia original al ser considerado primero. Comprueba si el 
array `$_POST` contiene algún elemento (en otras palabras, el formulario 
ha sido enviado) y si `$suspect`es verdadero. La advertencia tiene un 
tono deliberadamente neutro. No tiene sentido provocar a los atacantes.

5. Guarde `contact.php` y pruebe el formulario escribiendo cualquier 
contenido sospechoso en el campo de correo electrónico. Debería ver el 
nuevo mensaje de advertencia, pero su entrada no se conservará.

Puede verificar su código con `contact_05.php` e `includes/processmail_02.php` 
en la carpeta ch06.

## Envío de correo electrónico

Antes de continuar, es necesario explicar cómo funciona la función PHP 
`mail()`, ya que le ayudará a comprender el resto del script de 
procesamiento. La función PHP `mail()` toma hasta cinco argumentos, 
todos ellos cadenas, de la siguiente manera:

- La (s) dirección (es) del (los) destinatario (s)
- La línea de asunto
- El cuerpo del mensaje
- Una lista de otros encabezados de correo electrónico (opcional)
- Parámetros adicionales (opcional)

Las direcciones de correo electrónico del primer argumento pueden estar 
en cualquiera de los siguientes formatos:

```
'user@example.com'
'Some Guy <user2@example.com>'
```


Para enviar a más de una dirección, use una cadena separada por comas 
como esta:

```
'user@example.com, another@example.com, Some Guy <user2@example.com>'
```


El cuerpo del mensaje debe presentarse como una sola cadena. Esto 
significa que debe extraer los datos de entrada del array `$_POST` y 
formatear el mensaje, agregando etiquetas para identificar cada campo. 
De forma predeterminada, la función `mail()` solo admite texto sin formato. 
Las líneas nuevas deben utilizar tanto un retorno de carro como un carácter 
de nueva línea. También se recomienda restringir la longitud de las líneas 
a no más de 78 caracteres. Aunque suene complicado, puede construir el 
cuerpo del mensaje automáticamente con aproximadamente 20 líneas de 
código PHP, como verá en la Solución PHP 6-6. La adición de otros 
encabezados de correo electrónico se explica en detalle en la siguiente 
sección.

Muchas empresas de hosting ahora exigen el quinto argumento. Garantiza 
que el correo electrónico sea enviado por un usuario de confianza, y 
normalmente consta de su propia dirección de correo electrónico con el 
prefijo -f (sin un espacio entre ellos), todo entre comillas. Consulte 
las instrucciones de su empresa de alojamiento para ver si es necesario 
y el formato exacto que debe adoptar.

Precaución

Nunca debe incorporar la entrada del usuario en el quinto argumento de 
la función `mail()` porque puede usarse para ejecutar un script arbitrario 
en el servidor web.


## Uso seguro de encabezados de correo electrónico adicionales

Puede encontrar una lista completa de encabezados de correo electrónico 
en www.faqs.org/rfcs/rfc2076, pero algunos de los más conocidos y útiles 
le permiten enviar copias de un correo electrónico a otras direcciones 
(Cc y Bcc) o cambiar la codificación. Cada nuevo encabezado, excepto el 
final, debe estar en una línea separada terminada por un retorno de carro 
y un carácter de nueva línea. Esto significa usar las secuencias de 
escape \ r y \ n en cadenas entre comillas dobles (consulte la Tabla 4-5 
en el Capítulo 4).

Consejo

Una forma conveniente de formatear encabezados adicionales es definir 
cada encabezado como un elemento de aray separado y luego usar la 
función `implode()` para unirlos con "\ r \ n" entre comillas dobles.

De forma predeterminada, `mail()` usa la codificación Latin1 (ISO-8859-1), 
que no admite caracteres acentuados. En la actualidad, los editores de 
páginas web utilizan con frecuencia Unicode (UTF-8), que admite la mayoría 
de los idiomas escritos, incluidos los acentos comúnmente utilizados en 
los idiomas europeos, así como las escrituras no alfabéticas, como el 
chino y el japonés. Para asegurarse de que los mensajes de correo 
electrónico no estén distorsionados, use el encabezado `Content-Type` 
para configurar la codificación en UTF-8, así:


```
$headers[] = "Content-Type: text/plain; charset=utf-8";
```


También necesita agregar UTF-8 como el atributo charset en una etiqueta 
`<meta>` en el `<head>` de sus páginas web como esta:


```html
<meta charset="utf-8">
```

Supongamos que desea enviar copias a otros departamentos, además de 
una copia a otra dirección que no desea que los demás vean. El correo 
electrónico enviado por `mail()` a menudo se identifica como proveniente 
de `nadie@tudominio` (o cualquier nombre de usuario asignado al servidor 
web), por lo que es una buena idea agregar una dirección "From" más 
amigable. Así es como construyes esos encabezados adicionales, finalmente 
uniéndolos con `implode()`:

```
$headers[] = 'From: Japan Journey<feedback@example.com>';
$headers[] = 'Cc: sales@example.com, finance@example.com';
$headers[] = 'Bcc: secretplanning@example.com';
$headers = implode("\r\n", $headers);
```

La función `implode()` convierte un array en una cadena uniendo cada 
elemento del array con la cadena suministrada como primer argumento. 
Por lo tanto, esto devuelve el array `$headers` como una sola cadena 
con un retorno de carro y un carácter de nueva línea entre cada 
elemento del array.

Después de construir el conjunto de encabezados que desea usar, pasa 
la variable que los contiene como el cuarto argumento a `mail()`, así 
(asumiendo que la dirección de destino, el asunto y el cuerpo del 
mensaje ya se han almacenado en variables):

```
$mailSent = mail($to, $subject, $message, $headers);
```


Los encabezados adicionales codificados como este no presentan ningún 
riesgo de seguridad, pero cualquier cosa que provenga de la entrada 
del usuario debe filtrarse antes de que se use. El mayor peligro 
proviene de un campo de texto que solicita la dirección de correo 
electrónico del usuario. Una técnica ampliamente utilizada es incorporar 
la dirección de correo electrónico del usuario en un encabezado "From" 
o "Resply-to" a, que le permite responder directamente a los mensajes 
entrantes haciendo clic en el botón "Reply" en su programa de correo 
electrónico. Es muy conveniente, pero los atacantes con frecuencia 
intentan empaquetar un campo de entrada de correo electrónico con una 
gran cantidad de encabezados falsos. La solución PHP anterior eliminó 
los encabezados más comúnmente utilizados por los atacantes, pero 
debemos verificar más la dirección de correo electrónico antes de 
incorporarla en los encabezados adicionales.

Precaución

Aunque los campos de correo electrónico son el objetivo principal de 
los atacantes, la dirección de destino y la línea de asunto son vulnerables 
si permite que los usuarios cambien el valor. La entrada del usuario 
siempre debe considerarse sospechosa. Siempre codifique la dirección de 
destino y la línea de asunto. Alternativamente, proporcione un menú 
desplegable de valores aceptables y verifique el valor enviado con un 
array de los mismos valores.

Solución PHP 6-5: agregar encabezados y automatizar la dirección de respuesta

Esta solución PHP agrega tres encabezados al correo electrónico: 
`From`, `Content-Type` (para establecer la codificación en UTF-8) y 
`Reply-To`. Antes de agregar la dirección de correo electrónico del 
usuario al encabezado final, utiliza un filtro PHP integrado para 
verificar que el valor enviado se ajusta al formato de una dirección 
de correo electrónico válida.

Continúe trabajando con la misma página que antes. Alternativamente, 
use `contact_05.php` e `includes/processmail_02.php` de la carpeta ch06.

1. Los encabezados suelen ser específicos de un sitio web o página en 
particular, por lo que los encabezados `From`, `Content-Type` se 
agregarán al script en `contact.php`. Agregue el siguiente código al 
bloque PHP en la parte superior de la página justo antes de que se 
incluya `processmail.php`:

```
$required = ['name', 'comments', 'email'];
// create additional headers
$headers[] = 'From: Japan Journey<feedback@example.com>';
$headers[] = 'Content-Type: text/plain; charset=utf-8';
require './includes/processmail.php';
```

2. El propósito de validar la dirección de correo electrónico es 
asegurarse de que esté en un formato válido, pero el campo puede estar 
vacío porque decides no hacerlo obligatorio o porque el usuario 
simplemente lo ignoró. Si el campo es obligatorio pero está vacío, 
se agregará al array `$missing` y se mostrará la advertencia que agregó 
en la Solución PHP 6-2. Si el campo no está vacío, pero la entrada no 
es válida, debe mostrar un mensaje diferente.

Cambie a `processmail.php` y agregue este código en la parte inferior 
del script:

```
// validate the user's email
if (!$suspect && !empty($email)) {
    $validemail = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);
    if ($validemail) {
        $headers[] = "Reply-To: $validemail";
    } else {
        $errors['email'] = true;
    }
}
```

Esto comienza por verificar que no se haya encontrado contenido 
sospechoso y que el campo de correo electrónico no esté vacío. Ambas 
condiciones están precedidas por el operador lógico Not, por lo que 
devuelven verdadero si `$suspect` y vacío (`$email`) son ambos falsos. 
El bucle `foreach` que agregó en la Solución PHP 6-2 asigna todos los 
elementos esperados en el array `$_POST` a variables más simples, por 
lo que `$email` contiene el mismo valor que `$_POST['email']`.

La siguiente línea usa `filter_input()` para validar la dirección de 
correo electrónico. El primer argumento es una constante `PHP`, 
`INPUT_POST`, que especifica que el valor debe estar en el array 
`$_POST`. El segundo argumento es el nombre del elemento que desea 
probar. El argumento final es otra constante de PHP que especifica 
que desea verificar que el elemento cumple con el formato válido para 
un correo electrónico.

La función `filter_input()` devuelve el valor que se está probando si 
es válido. De lo contrario, devuelve falso. Entonces, si el valor 
enviado por el usuario parece una dirección de correo electrónico 
válida, `$validemail` contiene la dirección. Si el formato no es 
válido, `$validemail` es falso. La constante `FILTER_VALIDATE_EMAIL` 
acepta solo una única dirección de correo electrónico, por lo que se 
rechazará cualquier intento de insertar varias direcciones de correo 
electrónico.

Nota

`FILTER_VALIDATE_EMAIL` verifica el formato, no si la dirección es 
genuina.  Si `$validemail` no es falso, es seguro incorporarlo en un 
encabezado de correo electrónico `Reply-To`. Pero si `$validemail` es 
falso, `$errors['email']` se agrega al array `$errors`.

3. Ahora necesita modificar <label> para el campo de correo electrónico 
en `contact.php`, así:


```html
<label for="email">Email:
<?php if (in_array('email', $missing)) { ?>
    <span class="warning">Please enter your email address</span>
<?php } elseif (isset($errors['email'])) { ?>
    <span class="warning">Invalid email address</span>
<?php } ?>
</label>
```

Esto agrega una cláusula `elseif` a la primera declaración condicional 
y muestra una advertencia diferente si la dirección de correo electrónico 
falla en la validación.


4. Guarde `contact.php` y pruebe el formulario dejando todos los campos 
en blanco y haciendo clic en Enviar mensaje. Verá el mensaje de error 
original. Pruébelo nuevamente ingresando un valor que no sea una dirección 
de correo electrónico en el campo Correo electrónico o ingresando dos 
direcciones de correo electrónico. Debería ver el mensaje no válido.

Puede verificar su código con `contact_06.php` e `includes/processmail_03.php` 
en la carpeta ch06.


Solución PHP 6-6: Creación del cuerpo del mensaje y envío del correo

Muchos tutoriales de PHP muestran cómo construir el cuerpo del mensaje 
manualmente de esta manera:

```
$message = "Name: $name\r\n\r\n";
$message .= "Email: $email\r\n\r\n";
$message .= "Comments: $comments";
```

Esto agrega etiquetas para identificar de qué campo proviene la entrada 
e inserta dos retornos de carro y caracteres de nueva línea entre cada 
uno. Esto está bien para una pequeña cantidad de campos, pero pronto se 
vuelve tedioso con más campos. Siempre que le dé a sus campos de 
formulario atributos de nombre significativos, puede construir el cuerpo 
del mensaje automáticamente con un bucle `foreach`, que es el enfoque 
adoptado en esta solución PHP.

Continúe trabajando con los mismos archivos que antes. Alternativamente, 
use `contact_06.php` e `includes/processmail_03.php` de la carpeta ch06.



1. Agregue el siguiente código en la parte inferior del script en 
`processmail.php`:

```
$mailSent = false;
```

Esto inicializa una variable para redirigir a una página de agradecimiento 
después de que se haya enviado el correo. Debe establecerse en falso hasta que sepa que la función mail () se ha realizado correctamente.
 
2. Ahora agregue el código que genera el mensaje inmediatamente después:

```

// go ahead only if not suspect, all required fields OK, and no errors
if (!$suspect && !$missing && !$errors) {
    // initialize the $message variable
    $message = ";
    // loop through the $expected array
    foreach($expected as $item) {
        // assign the value of the current item to $val
        if (isset($$item) && !empty($$item)) {
            $val = $$item;
        } else {
            // if it has no value, assign 'Not selected'
            $val = 'Not selected';
        }
        // if an array, expand as comma-separated string
        if (is_array($val)) {
            $val = implode(', ', $val);
        }
        // replace underscores in the label with spaces
        $item = str_replace('_', ' ', $item);
        // add label and value to the message body
        $message .= ucfirst($item).": $val\r\n\r\n";
    }
    // limit line length to 70 characters
    $message = wordwrap($message, 70);
    // format headers as a single string
    $headers = implode("\r\n", $headers);
    $mailSent = true;
}
```



Este bloque de código comienza verificando que `$suspects, `$missing` 
y `$errors` sean todos falsos. Si es así, construye el cuerpo del 
mensaje recorriendo el  array $expected, almacenando el resultado en 
`$message` como una serie de pares `label/value`.

La clave de cómo funciona radica en la siguiente declaración condicional:

```

if (isset($$item) && !empty($$item)) {
    $val = $$item;
}
```

Este es otro ejemplo del uso de una variable variable (consulte “Creación 
de nuevas variables dinámicamente” en el Capítulo 4). Cada vez que se 
ejecuta el ciclo, `$item` contiene el valor del elemento actual en el 
array `$expected`. El primer elemento es `name`, por lo que `$$item` 
crea dinámicamente una variable llamada `$name. En efecto, la 
declaración condicional se convierte en esto:

```

if (isset($name) && !empty($name)) {
    $val = $name;
}
```

En el siguiente paso, `$$item` crea una variable llamada `$email`, y 
así sucesivamente.
     

Precaución

Esta secuencia de comandos crea el cuerpo del mensaje solo a partir 
de elementos del array `$expected`. Debe enumerar los nombres de todos 
los campos de formulario en el array `$expected` para que funcione.

Si un campo no especificado como obligatorio se deja vacío, su valor 
se establece en "No seleccionado". El código también procesa valores 
de elementos de opción múltiple, como grupos de casillas de verificación 
y listas `<select>`, que se transmiten como subarrays del array `$_POST`. 
La función `implode()` convierte los subarrays en cadenas separadas por 
comas.

Cada etiqueta se deriva del atributo `name` del campo de entrada en el 
elemento actual del array `$expected`. El primer argumento de `str_replace()` 
es un guión bajo. Si se encuentra un guión bajo en el atributo de `name`, 
se reemplaza por el segundo argumento, una cadena que consta de un solo 
espacio. Luego, `ucfirst()` establece la primera letra en mayúsculas. 
Observe que el tercer argumento de `str_replace()` es `$item` (con un 
solo signo de dólar), por lo que esta vez es una variable ordinaria, 
no una variable variable. Contiene el valor actual del array `$expected`.


Una vez que el cuerpo del mensaje se ha combinado en una sola cadena, 
la función `wordwrap()` limita la longitud de la línea a 70 caracteres. 
Los encabezados están formateados como una sola cadena con un retorno 
de carro y un carácter de nueva línea entre cada uno usando la función 
`implode()`.

El código que envía el correo electrónico aún debe agregarse, pero para 
fines de prueba, `$mailSent` se establece en verdadero.

3. Guarde `processmail.php`. Ubique este bloque de código en la parte 
inferior de `contact.php`:


```
<pre>
<?php if ($_POST) {print_r($_POST);} ?>
</pre>
```

Cámbielo a esto:


```

<pre>
<?php if ($_POST && $mailSent) {
    echo "Message body\n\n";
    echo htmlentities($message) . "\n";
    echo 'Headers: '. htmlentities($headers);
} ?>
</pre>
```


Esto verifica que el formulario se haya enviado y que el correo esté listo 
para enviarse. Luego muestra los valores en `$message` y `$headers`.  
Ambos valores se pasan a `htmlentities()` para garantizar que se muestren 
correctamente en el navegador.
 
4. Guarde `contact.php` y pruebe el formulario ingresando su nombre, 
dirección de correo electrónico y un breve comentario. Al hacer clic en 
Enviar mensaje, debería ver el cuerpo y los encabezados del mensaje en 
la parte inferior de la página, como se muestra en la Figura 6-8.


Suponiendo que el cuerpo del mensaje y los encabezados se muestren 
correctamente en la parte inferior de la página, está listo para agregar 
el código para enviar el correo electrónico. Si es necesario, verifique 
su código con `contact_07.php` e `includes/processmail_04.php` en la 
carpeta ch06.
 
5. En `processmail.php`, agregue el código para enviar el correo. 
Busque la siguiente línea:


```
$mailSent = true;
```


Cámbielo a esto:


```
$mailSent = mail($to, $subject, $message, $headers);
if (!$mailSent) {
    $errors['mailfail'] = true;
}
```

Esto pasa la dirección de destino, la línea de asunto, el cuerpo del 
mensaje y los encabezados a la función `mail()`, que devuelve verdadero 
si logra entregar el correo electrónico al agente de transporte de 
correo (MTA) del servidor web. Si falla, `$mailSent` se establece en 
falso y la declaración condicional agrega un elemento al array `$errors`, 
lo que le permite preservar la entrada del usuario cuando se vuelve a 
mostrar el formulario.
 
6. En el bloque PHP en la parte superior de `contact.php`, agregue la 
siguiente declaración condicional inmediatamente después del comando 
que incluye `processmail.php`:

```
    require './includes/processmail.php';
    if ($mailSent) {
        header('Location: http://www.example.com/thank_you.php');
        exit;
    }
}
?>
```


Debe probar esto en su servidor remoto, así que reemplace `www.example.com` 
con su propio nombre de dominio. Esto verifica si `$mailSent` es verdadero. 
Si es así, la función `header()` redirige a `thank_you.php`, se ha enviado 
una página reconociendo el mensaje. El comando de salida en la siguiente 
línea asegura que el script finalice después de que la página haya sido 
redirigida.

Hay una copia de `thank_you.php` en la carpeta ch06.
 
7. Si `$mailSent` es falso, se vuelve a mostrar `contact.php`; debe advertir 
al usuario que el mensaje no se pudo enviar. Edite la declaración condicional 
justo después del encabezado `<h2>`, así:


```

<h2>Contact Us </h2>
<?php if (($_POST && $suspect) || ($_POST && isset($errors['mailfail']))) { ?>
    <p class="warning">Sorry, your mail could not be sent

. . . .
```



Las condiciones originales y nuevas se han envuelto entre paréntesis, 
por lo que cada par se considera por separado. La advertencia sobre 
el mensaje no enviado se muestra si el formulario ha sido enviado y 
se han encontrado frases sospechosas, o si el formulario ha sido enviado 
y se ha configurado `$errors['mailfail']`.
 
8. Elimine el bloque de código (incluidas las etiquetas `<pre>`) que 
muestra el cuerpo del mensaje y los encabezados en la parte inferior 
de `contact.php`.
 
9. Probar esto localmente probablemente resultará en que se muestre la 
página de agradecimiento, pero el correo electrónico nunca llegue. Esto 
se debe a que la mayoría de los entornos de prueba no tienen un MTA. 
Incluso si configura uno, la mayoría de los servidores de correo rechazan 
el correo de fuentes no reconocidas. Sube `contact.php` y todos los 
archivos relacionados, incluidos `processmail.php` y `thank_you.php`, 
a tu servidor remoto y prueba el formulario de contacto allí. No
olvide que `processmail.php` debe estar en una subcarpeta llamada 
`includes`.

Puede verificar su código con `contact_08.php` e 
`includes/processmail_05.php` en la carpeta ch06.


## Solución a problemas de `mail()`

Es importante comprender que `mail()` no es un programa de correo 
electrónico. La responsabilidad de PHP termina tan pronto como pasa 
la dirección, el asunto, el mensaje y los encabezados al MTA. No tiene 
forma de saber si el correo electrónico se entrega a su destino previsto. 
Normalmente, el correo electrónico llega instantáneamente, pero los 
atascos en la red pueden retrasarlo horas o incluso un par de días.
Si se te redirige a la página de agradecimiento después de enviar un 
mensaje desde `contact.php`, pero no llega nada a tu bandeja de entrada, 
verifica lo siguiente:


- ¿El mensaje ha sido detectado por un filtro de spam?
- ¿Ha comprobado la dirección de destino almacenada en `$to`? Pruebe 
una dirección de correo electrónico alternativa para ver si marca la 
diferencia.
- ¿Ha utilizado una dirección genuina en el encabezado "From"? Es 
probable que el uso de una dirección falsa o no válida provoque el 
rechazo del correo. Utilice una dirección válida que pertenezca al mismo 
dominio que su servidor web.
- Consulte con su empresa de alojamiento para ver si se requiere el quinto 
argumento para `mail()`. Si es así, normalmente debería ser una cadena 
compuesta por `-f` seguida de su dirección de correo electrónico. Por 
ejemplo, `david@example.com` se convierte en `-fdavid@example.com`.


Si aún no recibe mensajes de `contact.php`, cree un archivo con este script:


```
<?php
ini_set('display_errors', '1');
$mailSent = mail('you@example.com', 'PHP mail test', 'This is a test email');
if ($mailSent) {
    echo 'Mail sent';
} else {
    echo 'Failed';
}
```


Reemplace `you@example.com` con su propia dirección de correo electrónico. 
Sube el archivo a tu sitio web y carga la página en un navegador.

Si ve un mensaje de error acerca de que no hay un encabezado From, agregue 
uno como cuarto argumento a la función `mail()`, así:

```
$mailSent = mail('you@example.com', 'PHP mail test', 'This is a test email',
'From: me@example.com');
```


Por lo general, es una buena idea usar una dirección diferente a la 
dirección de destino en el primer argumento.

Si su empresa de alojamiento requiere el quinto argumento, ajuste el 
código de esta manera:


```
$mailSent = mail('you@example.com', 'PHP mail test', 'This is a test email', null,
'-fme@example.com');
```


El uso del quinto argumento normalmente reemplaza la necesidad de 
proporcionar un encabezado From, asi que usar null(sin comillas) como 
cuarto argumento indica que no tiene valor.

Si ve "Correo enviado" y no llega ningún correo, o si ve "Error" después 
de probar los cinco argumentos, consulte a su empresa de alojamiento para obtener asesoramiento.

Si recibe el correo electrónico de prueba de este script pero no de 
`contact.php`, significa que ha cometido un error en el código o que 
ha olvidado cargar `processmail.php`. Active la visualización de errores 
temporalmente, como se describe en "¿Por qué mi página está en blanco?" 
en el Capítulo 3, para comprobar que `contact.php` puede encontrar 
`processmail.php`.

Consejo

Estaba enseñando en una universidad en Inglaterra y no podía entender 
por qué no se entregaban los correos de los estudiantes, a pesar de 
que su código era perfecto. Resultó que el departamento de TI había 
desactivado `Sendmail` (el MTA) para evitar que el servidor se utilizara 
para enviar spam.


## Manejo de elementos de formulario de opción múltiple

El formulario en `contact.php` usa solo campos de entrada de texto y 
un área de texto. Para trabajar con éxito con formularios, también 
necesita saber cómo manejar elementos de opción múltiple, a saber:

- Botones de radio
- Casillas de verificación
- Menús de opciones desplegables
- Listas de opción múltiple


El principio detrás de ellos es el mismo que el de los campos de entrada 
de texto con los que ha estado trabajando: el atributo de nombre del 
elemento de formulario se usa como clave en el array `$_POST`. Sin 
embargo, hay algunas diferencias importantes:

- Los grupos de casillas de verificación y las listas de opción múltiple 
almacenan los valores seleccionados como un array, por lo que debe 
agregar un par de corchetes vacíos al final del atributo de nombre 
para estos tipos de entrada. Por ejemplo, para un grupo de casillas 
de verificación llamado intereses, el atributo de nombre en cada 
etiqueta `<input>` debe ser `name="interest []"`. Si omite los corchetes, 
solo el último elemento seleccionado se transmite a través del array 
`$_POST`.

- Los valores de los elementos seleccionados en un grupo de casillas 
de verificación o una lista de opción múltiple se transmiten como un 
subarray del array `$_POST`. El código de la Solución PHP 6-6 
convierte automáticamente estos subarrays en cadenas separadas por 
comas. Sin embargo, cuando utilice un formulario para otros fines, 
debe extraer los valores de los subarrays. Verá cómo hacerlo en 
capítulos posteriores.

- Los botones de opción, las casillas de verificación y las listas de 
opción múltiple no se incluyen en el array `$_POST` si no se selecciona 
ningún valor. En consecuencia, es vital usar `isset()` para verificar 
su existencia antes de intentar acceder a sus valores al procesar el 
formulario.

Las restantes soluciones PHP de este capítulo muestran cómo manejar 
elementos de formulario de opción múltiple. En lugar de analizar 
cada paso en detalle, solo destacaré los puntos importantes. Tenga 
en cuenta los siguientes puntos cuando trabaje en el resto de este 
capítulo:

- El procesamiento de estos elementos se basa en el código de 
`processmail.php`.
- Debe agregar el atributo de nombre de cada elemento al arreglo 
`$expected para que se agregue al cuerpo del mensaje.
- Para hacer que un campo sea obligatorio, agregue su atributo `name` 
al array `$required`.
- Si un campo que no es obligatorio se deja en blanco, el código en 
`processmail.php` establece su valor en "No seleccionado".

La Figura 6-9 muestra `contact.php` con cada tipo de entrada agregada 
al diseño original.

Consejo

Todos los elementos de entrada de formulario HTML5 usan el atributo 
de `name` y envían valores como texto o como un subarray del array 
`$_POST`, por lo que debería poder adaptar el código en consecuencia.


Solución PHP 6-7: Manejo de grupos de botones de opción

Los grupos de botones de opción le permiten elegir solo un valor. 
Aunque es común establecer un valor predeterminado en el marcado HTML, 
no es obligatorio. Esta solución PHP muestra cómo manejar ambos escenarios.

1. La forma más sencilla de tratar los botones de opción es hacer 
que uno de ellos sea el predeterminado. El grupo de radio siempre se 
incluye en el array `$_POST` porque siempre se selecciona un valor.

El código para un grupo de radio con un valor predeterminado se ve 
así (los atributos `name` y el código PHP están resaltados en negrita):

```
<fieldset id="subscribe">
    <h2>Subscribe to newsletter?</h2>
    <p>
    <input name="subscribe" type="radio" value="Yes" id="subscribe-yes"
    <?php
    if ($_POST && $_POST['subscribe'] == 'Yes') {
        echo 'checked';
    } ?>>
    <label for="subscribe-yes">Yes</label>
    <input name="subscribe" type="radio" value="No" id="subscribe-no"
    <?php
    if (!$_POST || $_POST['subscribe'] == 'No') {
       echo 'checked';
    } ?>>
    <label for="subscribe-no">No</label>
    </p>
</fieldset>
```


Todos los miembros del grupo de radio comparten el mismo atributo `name`. 
Debido a que solo se puede seleccionar un valor, el atributo `name` no 
termina con un par de corchetes vacíos.

La declaración condicional relacionada con el botón Sí comprueba `$_POST` 
para ver si se ha enviado el formulario. Si es así y el valor de `$_POST['subscribe']` 
es “Yes”, el atributo marcado se agrega a la etiqueta `<input>`.

En el botón No, la declaración condicional usa || (o). La primera condición 
es `!$_ POST`, que es verdadera cuando no se ha enviado el formulario. 
Si es verdadero, el atributo marcado se agrega como valor predeterminado 
cuando se carga la página por primera vez. Si es falso, significa que 
el formulario ha sido enviado, por lo que el valor `$_POST['subscribe']` 
está marcado.
 
2. Cuando un botón de radio no tiene un valor predeterminado, no se 
incluye en el array `$_POST`, por lo que no es detectado por el bucle 
en `processmail.php` que construye el array `$missing`. Para asegurarse 
de que el elemento del botón de opción esté incluido en el array 
`$_POST`, debe probar su existencia después de que se haya enviado 
el formulario. Si no está incluido, debe establecer su valor en una 
cadena vacía, como esta:


```
$required = ['name', 'comments', 'email', 'subscribe'];
// set default values for variables that might not exist
if (!isset($_POST['subscribe'])) {
    $_POST['subscribe'] = ";
}
```


3. Si el grupo de botones de opción es obligatorio pero no está 
seleccionado, debe mostrar un mensaje de error cuando se vuelva 
a cargar el formulario. También necesita cambiar las declaraciones 
condicionales en las etiquetas `<input>` para reflejar el 
comportamiento diferente.

El siguiente listado muestra el grupo de botones de opción de 
suscripción de `contact_09.php`, con todo el código PHP resaltado 
en negrita:



```
<fieldset id="subscribe">
    <h2>Subscribe to newsletter?
    <?php if (in_array('subscribe', $missing)) { ?>
    <span class="warning">Please make a selection</span>
    <?php } ?>
    </h2>
    <p>
    <input name="subscribe" type="radio" value="Yes" id="subscribe-yes"
    <?php
    if ($_POST && $_POST['subscribe'] == 'Yes') {
        echo 'checked';
    } ?>>
    <label for="subscribe-yes">Yes</label>
    <input name="subscribe" type="radio" value="No" id="subscribe-no"
    <?php
    if ($_POST && $_POST['subscribe'] == 'No') {
        echo 'checked';
    } ?>>
    <label for="subscribe-no">No</label>
    </p>
</fieldset>
```


La declaración condicional que controla el mensaje de advertencia en la 
etiqueta `<h2>` utiliza la misma técnica que para los campos de entrada 
de texto. El mensaje se muestra si el grupo de radio es un elemento 
obligatorio y está en el array `$missing`.

La declaración condicional que rodea al atributo verificado es la misma 
en ambos botones de opción. Comprueba si el formulario ha sido enviado 
y muestra el atributo marcado solo si el valor en `$_POST['subscribe']` 
coincide.


Solución PHP 6-8: Manejo de grupos de casillas de verificación

Las casillas de verificación se pueden utilizar individualmente o en 
grupos. El método para manejarlos es ligeramente diferente. Esta solución 
PHP muestra cómo lidiar con un grupo de casillas de verificación llamado 
intereses. PHP Solution 6-11 explica cómo manejar una sola casilla de 
verificación.

Cuando se usa como un grupo, todas las casillas de verificación del 
grupo comparten el mismo atributo `name`, que debe terminar con un par 
de corchetes vacíos para que PHP transmita los valores seleccionados 
como un array. Para identificar qué casillas de verificación se han 
seleccionado, cada una necesita un atributo de valor único.

Si no se selecciona ningún elemento, el grupo de casillas de 
verificación no se incluye en el array `$_POST`. Una vez enviado el 
formulario, debe verificar el array `$_POST` para ver si contiene un 
subarray para el grupo de casillas de verificación. Si no es así, debe 
crear un subarray vacío como valor predeterminado para el script en 
`processmail.php`.

1. Para ahorrar espacio, solo se muestran las dos primeras casillas 
de verificación del grupo. El atributo `name` y las secciones de código 
PHP están resaltadas en negrita. 

```
<fieldset id="interests">
<h2>Interests in Japan</h2>
<div>
    <p>
        <input type="checkbox" name="interests[]" value="Anime/manga"
        id="anime"
        <?php
        if ($_POST && in_array('Anime/manga', $_POST['interests'])) {
            echo 'checked';
        } ?>>
        <label for="anime">Anime/manga</label>
    </p>
    <p>
        <input type="checkbox" name="interests[]" value="Arts & crafts"
        id="art"
        <?php
        if ($_POST && in_array('Arts & crafts', $_POST['interests'])) {
            echo 'checked';
        } ?>>
        <label for="art">Arts & crafts</label>
    </p>
. . .
</div>
</fieldset>
```


Cada casilla de verificación comparte el mismo atributo `name`, que 
termina con un par de corchetes vacíos, por lo que los datos se tratan 
como un array. Si omite los corchetes, `$_POST['intereses']` contiene 
el valor de solo la primera casilla de verificación seleccionada. 
Además, `$_POST['intereses']` no existirá si no se ha seleccionado 
ninguna casilla de verificación. Lo arreglarás en el siguiente paso.
     

Nota

Aunque los corchetes deben agregarse al atributo `name` para selecciones 
múltiples, el subarray de los valores seleccionados está en 
`$_POST['intereses']`, no `$_POST['intereses []']`.

El código PHP dentro de cada elemento de la casilla de verificación 
desempeña la misma función que en el grupo de botones de opción, 
envolviendo el atributo marcado en una declaración condicional. La 
primera condición verifica que se haya enviado el formulario. La 
segunda condición usa la función `in_array()` para verificar si el 
valor asociado con esa casilla de verificación está en el subarray 
`$_POST['intereses']`. Si es así, significa que se seleccionó la 
casilla de verificación.


2. Una vez enviado el formulario, debe verificar la existencia de 
`$_POST['intereses']`. Si no se ha configurado, debe crear un array 
vacío como valor predeterminado para que se procese el resto del 
script. El código sigue el mismo patrón que para el grupo de radio:


```
$required = ['name', 'comments', 'email', 'subscribe', 'interests'];
// set default values for variables that might not exist
if (!isset($_POST['subscribe'])) {
    $_POST['subscribe'] = ";
}
if (!isset($_POST['interests'])) {
    $_POST['interests'] = [];
}
```


3. Para establecer un número mínimo de casillas de verificación 
requeridas, use la función `count()` para confirmar el número de 
valores transmitidos desde el formulario. Si es menor que el mínimo 
requerido, agregue el grupo al array `$errors`, así:


```
if (!isset($_POST['interests'])) {
    $_POST['interests'] = [];
}
// minimum number of required check boxes
$minCheckboxes = 2;
if (count($_POST['interests']) < $minCheckboxes) {
    $errors['interests'] = true;
}
```


La función `count()` devuelve el número de elementos en un array, por 
lo que esto crea `$errors['interests']` si se han seleccionado menos 
de dos casillas de verificación. Quizás se pregunte por qué he usado 
una variable en lugar de un número como este:


```
if (count($_POST['interests']) < 2) {
```


Esto ciertamente funciona e implica menos escritura, pero `$minCheckboxes` 
se pueden reutilizar en el mensaje de error. Almacenar el número en 
una variable significa que esta condición y el mensaje de error siempre permanecen sincronizados.
 
4. El mensaje de error en el cuerpo del formulario tiene este aspecto:

``
<h2>Interests in Japan
<?php if (isset($errors['interests'])) { ?>
    <span class="warning">Please select at least <?= $minCheckboxes ?></span>
<?php } ?>
</h2>
```

Solución PHP 6-9: uso de un menú de opciones desplegable

Los menús de opciones desplegables creados con la etiqueta `<select>` 
son similares a los grupos de botones de radio en el sentido de que 
normalmente permiten al usuario elegir solo una opción de varias. Donde 
difieren es que siempre se selecciona un elemento en un menú desplegable, 
incluso si es solo el primer elemento que invita al usuario a seleccionar 
uno de los otros. Como resultado, el array `$_POST` siempre contiene un 
elemento que hace referencia a un menú `<select>`, mientras que un grupo 
de botones de opción se ignora a menos que se preestablezca un valor 
predeterminado.

1. El siguiente código muestra los dos primeros elementos del menú 
desplegable en `contact_09.php`, con el código PHP resaltado en negrita. 
Al igual que con todos los elementos de opción múltiple, el código PHP 
envuelve el atributo que indica qué elemento se ha elegido. Aunque este 
atributo se llama checked tanto en los botones de opción como en las 
casillas de verificación, se llama seleccionado en los menús y listas 
`<select>`. Es importante utilizar el atributo correcto para volver a 
mostrar la selección si el formulario se envía sin los elementos 
obligatorios. Cuando la página se carga por primera vez, el array `$_POST` 
no contiene elementos, por lo que puede seleccionar la primera `<option>` 
probando `!$_ POST`. Una vez que se envía el formulario, el array `$_POST` 
siempre contiene un elemento de un menú desplegable, por lo que no es 
necesario que pruebe su existencia.

```
<p>
    <label for="howhear">How did you hear of Japan Journey?</label>
    <select name="howhear" id="howhear">
        <option value="No reply"
        <?php
        if (!$_POST || $_POST['howhear'] == 'No reply') {
            echo 'selected';
        } ?>>Select one</option>
        <option value="Apress"
        <?php
        if (isset($_POST && $_POST['howhear'] == 'Apress') {
            echo 'selected';
        } ?>>Apress</option>
    . . .
    </select>
</p>
```

2. Aunque siempre se selecciona una opción en un menú desplegable, es 
posible que desee obligar a los usuarios a realizar una selección 
diferente a la predeterminada. Para hacerlo, agregue el atributo 
`name` del menú `<select>` aa array `$required`, luego establezca 
el atributo `value` y el elemento de array `$_POST` para la opción 
predeterminada en una cadena vacía, como esta:

```
<option value=""
<?php
if (!$_POST || $_POST['howhear'] == '') {
    echo 'selected';
} ?>>Select one</option>
```

El atributo `value` no es obligatorio en la etiqueta `<option>`, pero 
si lo omite, el formulario usa el texto entre las etiquetas de apertura 
y cierre como el valor seleccionado. Por lo tanto, es necesario establecer 
el atributo value explícitamente en una cadena vacía. De lo contrario, 
“Seleccionar uno” se transmite como valor seleccionado.
 
3. El código que muestra un mensaje de advertencia si no se ha realizado 
ninguna selección sigue un patrón familiar:


```
<label for="select">How did you hear of Japan Journey?
<?php if (in_array('howhear', $missing)) { ?>
    <span class="warning">Please make a selection</span>
<?php } ?>
</label>
```


Solución PHP 6-10: Manejo de una lista de opción múltiple

Las listas de opción múltiple son similares a los grupos de casillas 
de verificación: permiten al usuario elegir cero o más elementos, por 
lo que el resultado se almacena en un array. Si no se selecciona ningún 
elemento, la lista de opción múltiple no se incluye en el array `$_POST`, 
por lo que debe agregar u subarray vacío de la misma manera que con un 
grupo de casillas de verificación.

1. El siguiente código muestra los dos primeros elementos de la lista 
de opción múltiple en `contact_09.php`, con el atributo `name` y el 
código PHP resaltados en negrita. Los corchetes anexados al atributo 
de nombre aseguran que almacena los resultados como un array. El código 
funciona de manera idéntica al grupo de casillas de verificación en 
la Solución PHP 6-8.


```
<p>
    <label for="characteristics">What characteristics do you associate with
    Japan?</label>
    <select name="characteristics[]" size="6" multiple="multiple"
    id="characteristics">
        <option value="Dynamic"
        <?php
        if ($_POST && in_array('Dynamic', $_POST['characteristics'])) {
            echo 'selected';
        } ?>>Dynamic</option>
        <option value="Honest"
        <?php
        if ($_POST && in_array('Honest', $_POST['characteristics'])) {
            echo 'selected';
        } ?>>Honest</option>
. . .
    </select>
</p>
```


2. En el código que procesa el mensaje, establezca un valor predeterminado 
para una lista de opción múltiple de la misma manera que para un array de 
casillas de verificación:


```
if (!isset($_POST['interests'])) {
  $_POST['interests'] = [];
}
if (!isset($_POST['characteristics'])) {
  $_POST['characteristics'] = [];
}
```

3. Para hacer que se requiera una lista de opción múltiple y establecer 
un número mínimo de opciones, use la misma técnica utilizada para un 
grupo de casillas de verificación en la Solución PHP 6-8.


Solución PHP 6-11: Manejo de una única casilla de verificación

La forma en que maneja una sola casilla de verificación es ligeramente 
diferente a la de un grupo de casillas de verificación. Con una casilla 
de verificación individual, no agrega corchetes al atributo `name` porque 
no necesita procesarse como un array. Además, el atributo `value` es 
opcional. Si no establece el atributo `value`, el valor predeterminado 
es "On" si la casilla de verificación está seleccionada. Sin embargo, 
si la casilla de verificación no está seleccionada, su nombre no se 
incluye en el array `$_POST`, por lo que debe probar su existencia.
Esta solución PHP muestra cómo agregar una única casilla de 
verificación que busca la confirmación de que se han aceptado los 
términos del sitio. Se asume que es necesario seleccionar la casilla 
de verificación.

1. Este código muestra la casilla de verificación única, con el 
atributo `name` y el código PHP resaltados en negrita.


```
<p>
    <input type="checkbox" name="terms" value="accepted" id="terms"
    <?php
    if ($_POST && !isset($errors['terms'])) {
        echo 'checked';
    } ?>>
    <label for="terms">I accept the terms of using this website
    <?php if (isset($errors['terms'])) { ?>
        <span class="warning">Please select the check box</span>
    <?php } ?></label>
</p>
```


El bloque PHP dentro del elemento `<input>` inserta el atributo marcado 
solo si el array `$_POST` contiene valores y no se ha establecido 
`$errors['terms']`. Esto asegura que la casilla de verificación no esté 
seleccionada cuando la página se carga por primera vez. También permanece 
sin marcar si el usuario envió el formulario sin confirmar la 
aceptación de los términos.

El segundo bloque PHP muestra un mensaje de error junto a la etiqueta si se ha establecido $ errors ['terms'].
 
2. Además de agregar términos a las matrices $ esperado y $ requerido, debe establecer un valor predeterminado para $ _POST ['términos']; luego configure $ errors ['terms'] en el código que procesa los datos cuando se envía el formulario:

&-------------------------------------------------------------------
