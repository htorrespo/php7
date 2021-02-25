<!-- https://link-springer-com.ezproxy.unal.edu.co/chapter/10.1007/978-1-4842-4463-0_2 -->

# Programación del lado del servidor con PHP


En este capítulo, conectará el servidor Apache a un motor PHP para 
crear sitios que acepten datos específicos del cliente de formularios 
HTML, que se procesan, y luego los resultados se envían de vuelta 
al navegador del cliente, proporcionando así contenido dinámico. Se 
le presentará PHP y luego comparará la programación del lado del cliente 
en JavaScript con la programación del lado del servidor en PHP. Por 
último, creará un sitio basado en PHP que valida los códigos de función 
que se pueden ingresar en línea en un sitio web a cambio de premios.


## El motor PHP

Un servidor web habilitado para PHP puede recibir una solicitud de un 
archivo PHP principalmente de dos formas.

- Utilizando la URL del archivo PHP en la barra de direcciones del 
navegador web

- Al enviar un formulario HTML, donde el atributo `action` se establece 
en la URL del archivo PHP

Al recibir la solicitud de un archivo PHP, el servidor web localiza el 
archivo y lo pasa al motor PHP para su procesamiento. El motor PHP es 
un intérprete de PHP que está asignado actualmente al servidor web. Lo 
que el usuario realmente ve en el navegador no es el archivo PHP en sí, 
sino la salida de todos los comandos PHP, enviados por el servidor web. 
Por ejemplo, el comando echo PHP es el que se usa para enviar mensajes. 
Aquí tienes un ejemplo:

```
echo "Hello world!";
```

Las etiquetas PHP de inicio/fin (`<?` y `?>`) Se utilizan como delimitadores 
de un programa PHP.


```
<?php
echo "Hello world!";
?>
```

El motor PHP evalúa el programa anterior en una cadena, que es lo que 
el usuario realmente ve cuando request `hello.php`.


```
Hello world!
```

Aparte de los comandos PHP, el motor PHP acepta etiquetas HTML, por 
ejemplo:

```
<?php
echo "<b>Hello <br>world!</b>";
?>
```


El usuario ve el siguiente mensaje en negrita:

```
Hello world!
```


Esta capacidad convierte a PHP en un lenguaje ideal para usar en la Web.

PHP es un lenguaje de programación completo que puede usar para crear 
programas que se ejecutan en el lado del servidor, limitado solo por 
su creatividad. Aquí hay algunos usos epigramáticos de PHP:

- Interfaz con el shell del sistema operativo, por ejemplo, el terminal 
de Linux, y ejecutando comandos de terminal

- Interfaz con programas de usuario, más comúnmente bases de datos, y 
así permitir al usuario realizar una búsqueda y hacer que el servidor 
web ofrezca contenido dinámico.

-Interfaz con el sistema de archivos y permitiendo leer y escribir 
archivos en el lado del servidor

- Interfaz con servidores remotos mediante el establecimiento de 
conexiones de red

Verá ejemplos de todos estos usos de PHP en los siguientes capítulos.



## Instalacion y Prueba de PHP


En este libro, se utiliza PHP versión 7.0. Para instalar PHP 7.0, siga 
los pasos de esta sección.


Agregue el archivo de paquetes personales (PPA) `ondrej/php`.


```terminal
$ sudo add-apt-repository ppa: ondrej / php
```


Ejecute la actualización.


```terminal
$ sudo apt-get update
```


Instale PHP 7.


```terminal
$ sudo apt-get install `php7.0`
```



Habilite el módulo de Apache `php7.0`, también instalado con el 
comando anterior.


```terminal
$ sudo a2enmod php7.0
```


Asegúrese de que el módulo Apache `mpm_prefork` (multiprocesamiento prefork) 
esté habilitado y el módulo `mpm_event` esté deshabilitado.


```terminal
$ sudo a2dismod mpm_event && sudo a2enmod mpm_prefork
```


Instale también la CLI (Interfaz de línea de comandos) PHP:


```terminal
$ sudo apt-get install php7.0-cli
```

Reinicie Apache.


```terminal
$ sudo systemctl reiniciar apache2
```

Para probar su motor PHP recién instalado, cree un archivo PHP, llamado 
`info.php`, en el directorio raíz del documento. Ingrese el siguiente 
código fuente PHP:


```html
<? php
phpinfo ();
?>
```

El fragmento de código anterior incluye solo un comando, la llamada a 
la función `phpinfo()`. Al igual que con cada comando de PHP, termina 
con un punto y coma. La función `phpinfo()` devuelve una gran cantidad 
de información sobre la versión de PHP, el sistema operativo, el 
servidor web, etc., que abarca varias pantallas.


En la barra de direcciones de su navegador, escriba la URL que conduce 
a `info.php`, por ejemplo:


```
192.168.1.100/info.php
```

 `192.168.1.100` en este ejemplo es la dirección IP privada del servidor 
web.

La Figura 2-1 muestra la página de información.

La información de la página web info.php se utilizará en los siguientes 
ejemplos. En la siguiente sección, probará PHP independientemente de un 
servidor web; utilizará la línea de comandos para simular el proceso 
seguido por un servidor web equipado con un motor PHP.


## Probando PHP sin un servidor web


A veces es útil ejecutar y probar el código fuente PHP y ver el resultado 
como aparecería en el navegador del usuario sin necesidad de un servidor 
web. Hay intérpretes de PHP, como CLI PHP, que se ejecutan desde la línea 
de comandos de su sistema operativo; sin embargo, no ofrecen el efecto 
exacto porque no procesan etiquetas HTML. También hay editores de PHP en 
línea, algunos de los cuales, como `phptester.ne`t, procesan etiquetas 
HTML, pero no puedes combinarlas con el software de tu computadora. Para 
ello, se puede utilizar el siguiente ejemplo, que se basa en CLI PHP.


Para probar si ya ha instalado PHP CLI, ingrese lo siguiente en la línea 
de comando:


```
$ php -v
```

La salida en mi sistema es la siguiente:


## Prueba de PHP sin un servidor web

A veces es útil ejecutar y probar el código fuente PHP y ver el 
resultado como aparecería en el navegador del usuario sin necesidad 
de un servidor web. Hay intérpretes de PHP, como CLI PHP, que se 
ejecutan desde la línea de comandos de su sistema operativo; sin 
embargo, no ofrecen el efecto exacto porque no procesan etiquetas HTML. 
También hay editores de PHP en línea, algunos de los cuales, como 
`phptester.net`, procesan etiquetas HTML, pero no puedes combinarlas 
con el software de tu computadora. Para ello, se puede utilizar el 
siguiente ejemplo, que se basa en CLI PHP.

Para probar si ya ha instalado PHP CLI, ingrese lo siguiente en la 
línea de comando:


```
$ php -v
```

La salida en mi sistema es la siguiente:


```
PHP 7.1.17-0ubuntu0.17.10.1 (cli) (built: May  9 2018 17:28:01) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2018 Zend Technologies
with Zend OPcache v7.1.17-0ubuntu0.17.10.1, Copyright (c) 1999-2018, by Zend Technologies
```


Para ver las opciones disponibles, use la opcion `-h (help)`.

Para simular el proceso de usar código PHP dentro de una página web y 
ver el resultado con un navegador, sin requerir un servidor web, siga 
los siguientes pasos. Primero cree un archivo PHP que use etiquetas HTML 
y también código fuente PHP. Específicamente, cree un archivo llamado 
`a.php` en su directorio de inicio usando estos comandos:


```
$ cd ~
$ gedit a.php
```


Ingrese el siguiente código fuente y guarde el archivo:

```
<html>
<head>
<title>Testing PHP without a web server</title>
</head>
<body>
<b>
<?php
$command =  "users";
$output = shell_exec($command);
echo $output;
?>
</b>
</body>
</html>
```


Se incluyen tres comandos PHP entre las etiquetas PHP inicial (`<?`) 
y final (`?>`). El primero establece la variable `$command` con el valor 
del nombre del comando que será ejecutado por el sistema operativo. 
En este ejemplo, se utilizó el comando shell `users`, que enumera los 
usuarios que iniciaron sesión. El segundo comando usa `shell_exec()`, 
que es la función PHP que interactúa con el shell y ejecuta el comando 
dado en el argumento, en este caso `users`, que es el valor de `$command`. 
Como resultado, se ejecuta el comando `users` y su salida se almacena 
en la variable `$output`. El tercer comando imprime el contenido de la 
variable `$output` en negrita ya que el bloque PHP está incluido entre 
las etiquetas de inicio y finalización `<b>`.

El archivo `a.php` es el archivo PHP original que será evaluado y visto 
desde un navegador; sin embargo, para este archivo, se requiere el 
archivo auxiliar `c.php` para ejecutarse. Cree el archivo `c.php` a 
continuación.


```
$ gedit c.php
```

Ingrese el siguiente código fuente y guarde el archivo:


```
<?php
shell_exec("php a.php > b.html");
shell_exec("firefox b.html");
?>
```

Lo que hace el programa `c.php` es ejecutar el código del archivo 
`a.php` con PHP CLI y luego redirigir (`>`) la salida al archivo 
HTML `b.html`.


```
$ php a.php > b.html
```

A continuación, se invoca el navegador web Firefox desde la línea de 
comandos para abrir la página web `b.html`. Este es el equivalente a 
ejecutar el siguiente comando desde la línea de comandos:


```
$ firefox b.html
```


Después de crear `a.php` y `c.php`, el comando que emite desde la línea 
de comandos que inicia todo es el siguiente:

```
$ php c.php
```

La Figura 2-2 muestra el resultado de ejecutar el comando anterior: se
abre la ventana de Firefox y enumera los usuarios que han iniciado sesión 
actualmente.

Abrir imagen en una ventana nueva Figura 2-2

Figura 2-2 La ejecución de `c.php` desde CLI PHP da como resultado la 
visualización del archivo `a.php` en una página web

Una forma simple pero eficiente de depurar el código PHP recibido de 
un servidor web por el navegador del usuario es hacer clic con el botón 
derecho en la página web y seleccionar Ver código fuente de la página 
(o similar) en el menú emergente. Como se muestra en la Figura 2-3, se 
abre una nueva pestaña con el código PHP evaluado.

Abrir imagen en una nueva ventana Figura 2-3

Figura 2-3 El código fuente del código PHP evaluado

Como se muestra en el codigo fuente de la página web resultante del 
código PHP evaluado, solo un usuario está conectado actualmente, y el 
nombre de usuario se incluye entre las etiquetas en negrita de `start/end` 
fin en la sección `body` código fuente HTML. A partir de la siguiente 
sección, todos los ejemplos de PHP se ejecutarán a través del servidor 
web. El servidor web Apache, presentado en el capítulo anterior, se 
utilizará aquí (a partir del Capítulo 5 utilizará otro servidor web de 
código abierto, Lighttpd).


## Ejecución de sus primeros ejemplos de PHP desde el servidor web

Ahora probará los archivos PHP evaluados por el motor PHP y enviados 
por el servidor web Apache. Los siguientes ejemplos muestran cómo se 
combinan las variables con `strings` y la diferencia entre comillas 
simples y dobles cuando se utilizan con `strings`. Además, explico cómo 
puede mezclar `JavaScript` y `PHP` en el mismo comando. Comprender esos 
detalles en el código fuente es esencial para comprender los proyectos 
basados en la web incluidos en el resto del libro.


### Trabajar con variables y cadenas

Primero verá cómo se pueden combinar `strings` y variables de PHP. Cree 
un nuevo archivo con una extensión `.php` en el directorio raíz de su 
documento.


```
$ cd /var/www/html
$ sudo gedit test.php
```

Ingrese el siguiente código fuente que no incluye ningun `string` y 
guarde el archivo `test.php`:


```
<!DOCTYPE html>
<html>
<head>
<title>
Testing PHP
</title>
<style>
p {
font-size:24px;
color:blue;
</style>
</head>
<body>
<?php
// working with variables
$var1 = 5;
$var2 = 8;
$var3 = $var1 + $var2;
echo $var3;
?>
</body>
</html>
```


Pruebe el archivo PHP ingresando su URL en la barra de direcciones de 
su navegador. Si realiza la prueba desde el servidor web, utilice lo 
siguiente:


```
localhost/test.php
```


Si también realiza la prueba desde el servidor web o desde otra 
computadora de su LAN, puede usar lo siguiente:


```
192.168.1.100/test.php
```


Aquí, `192.168.1.100` es la dirección IP privada de la computadora que 
aloja el servidor web. La Figura 2-4 muestra la página web descargada 
en el navegador. Abrir imagen en una ventana nueva Figura 2-4

Figura 2-4 La página web de `test.php` evaluada

El programa PHP utiliza tres variables: `$var1` y `$var2` que se 
inicializan a valores específicos, y `$var3`, cuyo valor se calcula 
sumando los valores de las dos variables anteriores. El comando 
`echo PHP` se usa para enviar el valor de la variable `$var3` a la 
página web que se muestra en su navegador. Además, se usa una barra 
doble (`//`) para marcar una línea como comentario. Para incluir varias 
líneas continuas en un comentario, use la notación de barra y estrella, 
es decir,`/*` para comenzar y `*/` para finalizar.


Observe que el nombre de una variable en PHP comienza con el signo 
dólar (`$`). Anteponer una variable con el signo dólar puede parecer 
una sobrecarga, pero es útil cuando se incluye la variable en una cadena. 
Para mostrar esta función, reemplace el código PHP anterior en `test.php` 
con lo siguiente:


```
<?php
// working with variables
$var1 = 5;
$var2 = 8;
$var3 = $var1 + $var2;
echo "The sum of $var1 and $var2 is $var3";
?>
```


Las cadenas en PHP se pueden incluir entre comillas dobles (`"`) o 
simples (`'`). Cuando se inserta una variable en una cadena con 
delimitadores de comillas dobles, se evalúa a su valor real. La página 
web de este ejemplo muestra lo siguiente:


```
The sum of 5 and 8 is 13
```


Sin embargo, este no es el caso cuando se utilizan comillas simples 
como delimitadores de cadenas. Reemplace el código PHP anterior en 
`test.php` con lo siguiente:


```
<?php
// working with variables
$var1 = 5;
$var2 = 8;
$var3 = $var1 + $var2;
echo 'The sum of $var1 and $var2 is $var3';
?>
```


La página web muestra lo siguiente:


```
The sum of $var1 and $var2 is $var3
```


También puede utilizar el operador de concatenación (`.`) Para concatenar 
cadenas y variables. Cambie el código PHP anterior en `test.php` a lo 
siguiente:


```
<?php
// working with variables
$var1 = 5;
$var2 = 8;
$var3 = $var1 + $var2;
echo "The sum of " . $var1 . " and ". $var2 . " is " . $var3;
?>
```


La página web `test.php` evalúa ahora lo siguiente:

```
The sum of 5 and 8 is 13
```

En su lugar, puede utilizar comillas simples para tener el mismo efecto.


```
<?php
// working with variables
$var1 = 5;
$var2 = 8;
$var3 = $var1 + $var2;
echo 'The sum of ' . $var1 . ' and ' . $var2 . ' is ' . $var3;
?>
```

#### Escaping Double or Single Quotes in PHP


Para incluir comillas dobles en una cadena impresa en el comando `echo` 
sin confundirlas con las comillas dobles del mensaje utilizadas como 
delimitadores, puede anteponerlas con el carácter de barra invertida 
(`\`). Esto se conoce como escapar de las comillas dobles. Otro método es 
incluir el mensaje con los caracteres de comillas dobles en delimitadores 
de comillas simples. Por ejemplo, el siguiente código fuente del archivo 
PHP permite incluir comillas dobles en los mensajes de `echo`:

```
<?php
echo "This message includes a double quote(\")<br>";
echo 'This message also includes a double quote(")<br>';
?>
```


De manera similar, puede incluir comillas simples en una cadena 
escapándolas con el carácter de barra invertida en una cadena entre 
comillas simples o utilizar comillas dobles como delimitadores de cadena. 
El siguiente ejemplo de PHP muestra esta técnica:

```
<?php
echo 'This message includes a single quote(\')<br>';
echo "This message also includes a single quote(')";
?>
```

#### Mezcla de JavaScript y PHP


Si bien JavaScript se usa para el lenguaje de programación del lado del 
cliente y PHP se usa para el lenguaje del lado del servidor, hay casos 
en los que HTML y JavaScript deben mezclarse con PHP. Esta sección 
describe las reglas generales para mezclar JavaScript, PHP y etiquetas 
HTML.

Para empezar, un archivo PHP puede incluir etiquetas HTML como un archivo 
HTML común, por ejemplo:

```html
<!DOCTYPE html>
<html>
<head>
<style>
p{
font-size:24px;
}
</style>
</head>
<body>
<?php
echo "<p>Hello World!</p>";
?>
</body>
</html>
```

En su lugar, puede utilizar un archivo PHP con contenido sin HTML como 
el siguiente:

```
<?php
echo "Hello World!";
?>
```

Un archivo PHP también puede incluir código fuente JavaScript, como en 
el siguiente ejemplo:


```
<script>
document.write("Hello ");
</script>
<?php
echo "World!";
?>
```

En el archivo PHP anterior, los dos bloques de código fuente no están 
mezclados y ambas funciones imprimen individualmente su propia parte 
en la página web, con JavaScript ejecutándose cuando el navegador carga 
la página y PHP ejecutándose antes de que la página web se envíe al 
navegador del usuario. A continuación, verá algunos casos en los que 
se mezclan el código fuente PHP y JavaScript. Como regla general, las 
etiquetas HTML, como `<script>`, `<style>`, etc., pueden incluirse en 
un archivo PHP, sin embargo, no dentro de un bloque de código fuente 
PHP, definido entre los delimitadores `<php` y `?>`. De lo contrario, 
el código PHP no se ejecutará y la página web evaluada mostrará un 
mensaje de error (`error HTTP 500`). El siguiente ejemplo se ejecutará 
porque las etiquetas del script de inicio y finalización están fuera 
del bloque de PHP y los comandos de JavaScript se imprimen en su lugar 
con un comando `echo` de PHP:
 
```
<script>
<?php
echo '
alert("Hello World!");
';
?>
</script>
```


Además, el siguiente fragmento de código fuente se ejecutará porque 
todas las etiquetas del script están incluidas en el mensaje de PHP 
`echo` y, por lo tanto, el script completo se imprime en la página web 
cuando se evalúa el código fuente PHP:

```
<?php
echo '
<script>
alert("Hello World!");
</script>
';
?>
```

En el siguiente ejemplo, el código fuente no se ejecutará porque las 
etiquetas de inicio y fin del script están incluidas dentro del bloque 
PHP, sin embargo, sin ser incluidas en el comando `echo`:

```
<?php
<script>
echo '
alert("Hello World!");
';
</script>
?>
```

En este libro, verá muchos casos en los que debe usar PHP y JavaScript 
en el mismo comando. Por ejemplo, a veces es necesario que el usuario 
vea una variable de PHP en una ventana emergente en lugar de escribirla 
en la ventana del navegador con el comando `echo`. Con PHP que pertenece 
al lado del servidor, no le da control al navegador del usuario para 
crear una ventana emergente. En su lugar, incluye JavaScript en la 
página evaluada de PHP. El siguiente archivo PHP contiene código PHP 
en dos bloques. En el primer bloque, por simplicidad, la variable 
`$name` está cableada a un valor específico, y en el segundo bloque, 
un bloque PHP se combina con código JavaScript como parte del texto 
`alert()` para que la variable PHP `$name` aparezca en un mensaje de 
ventana emergente:

```
<?php
$name = "Madison";
?>
<script>
alert("Hello <?php echo $name; ?> ");
</script>
```

Considere también el ejemplo donde la ventana emergente aparece solo 
si el valor de `$name` es 'Jennifer'. En este caso, todo el script debe 
incluirse condicionalmente en el código fuente PHP, pero es posible que 
nunca se ejecute.

```
<?php
$name = 'Susan';
if ($name === Susan) {
echo '
<script>
alert("Hello ' . $name . ' ");
</script>
';
} else {
echo  'Hello';
}
?>
```

En el ejemplo anterior, por simplicidad, la variable `$name` estaba 
cableada al código fuente en lugar de permitir múltiples valores, por 
ejemplo, valores derivados de un formulario HTML. Al igual que con la 
etiqueta de secuencia de comandos, el código PHP se puede mezclar con 
otras etiquetas HTML.

A continuación, se muestra un ejemplo de código fuente que no se ejecuta:

```
<?php
<b>
echo "Hello World!";
</b>
?>
```

Por otro lado, el siguiente ejemplo se ejecuta como se esperaba:

```
<b>
<?php
echo "Hello World!";
?>
</b>
```

O también puede utilizar lo siguiente:

```
<?php
echo "<b>Hello World!</b>";
?>
```

En los ejemplos usados en esta sección, el programa PHP inicializó las 
variables PHP a sus valores. A continuación, utilizará formularios HTML 
que el visitante del sitio utiliza para enviar datos específicos del 
usuario al servidor web y luego establecerá las variables PHP de acuerdo 
con las preferencias del usuario.


## Configuración de las variables PHP con el método GET

La forma más común de permitir que el usuario envíe datos al servidor 
web es usar un formulario HTML que implemente el método GET o POST y 
también se refiere a la URL del programa PHP que recibe y procesa los 
datos del usuario del atributo `action` del elemento de formulario. En 
el siguiente ejemplo, creará una página web que incluye un formulario 
HTML con el atributo de método establecido en GET. Primero cree el 
archivo HTML `get.html` que incluye un formulario para permitir al 
usuario insertar números en dos campos y también seleccionar un operador 
aritmético con una lista desplegable. Cuando se hace clic en el botón 
enviar, los datos del usuario se transmiten al programa `get.php`, que 
se encuentra, como indica la URL relativa del atributo de `action`, en 
el servidor web que envió la página web actual y en el mismo directorio 
que el página web actual.

Utilice los siguientes comandos para cambiar a la raíz del documento 
del servidor y crear el archivo `get.html`:

```
$ cd /var/www/html
$ sudo gedit get.html
```

Inserte el siguiente código fuente HTML:

```
<!DOCTYPE html>
<html>
<head>
<style>
input, select{
font-size:24px;
}
</style>
</head>
<body>
<form method="get" action="get.php">
<input type="number" name="n1">
<select name="operator">
  <option value="add">+</option>
  <option value="subtract">-</option>
  <option value="multiply">*</option>
  <option value="divide">/</option>
</select>
<input type="number" name="n2">
<input type="submit" value="Calculate">
</form>
</body>
</html>
```

Crear el archivo `get.php` en el mismo directorio.

```
$ sudo gedit get.php
```

Insertar el siguiente código fuente HTML que incrusta el código 
fuente PHP:

```
<!DOCTYPE html>
<html>
<head>
<style>
p{
font-size:24px;
}
</style>
</head>
<body>
<p>
<?php
$num1 = $_GET["n1"];
$num2 = $_GET["n2"];
$operator = $_GET["operator"];
switch($operator) {
    case "add":
         echo $num1 + $num2;
         break;
    case "subtract":
         echo $num1 - $num2;
         break;
    case "multiply":
         echo $num1 * $num2;
         break;
    case "divide":
         echo $num1 / $num2;
         break;
}
?>
</p>
</body>
</html>
```

Con la matriz de métodos de formulario GET `$_GET`, el motor PHP pasa 
una variable PHP global a `get.php`, el programa de acción PHP. Los 
elementos del array `$_GET` consisten en los datos enviados por el 
formulario. Al usar el nombre del elemento del formulario con `$_GET` 
como índice para enviar un valor, se devuelve el valor específico. Por 
ejemplo, con el siguiente comando, la variable PHP `$num1` obtiene el 
valor enviado por el campo del formulario llamado `n1`:

```
$num1 = $_GET["n1"];
```

De manera similar, el valor ingresado por el usuario en el segundo 
campo del número de tipo se asigna a la variable PHP `$num2`.

```
$num2 = $_GET["n2"];
```

La lista desplegable del formulario también envía un valor que 
corresponde a uno de los siguientes operadores aritméticos: suma (`+`), 
resta (`-`), multiplicación (`*`), división (`/`). A continuación, se 
accede a la elección del usuario para el operador con la variable de 
operador PHP `$` de la siguiente manera:

```
$operator = $_GET["operator"];
```
A continuación, `$operator` ingresa un comando de cambio de PHP que, 
de acuerdo con el valor del operador `$`, realiza la operación aritmética 
correspondiente entre `$num1` y `$num2`.

Para probar la interacción cliente-servidor de este ejemplo, use otra 
computadora en su LAN e ingrese lo siguiente en la barra de direcciones 
del navegador:

```
192.168.1.100/get.html
```
192.168.1.100 es en este ejemplo la dirección IP privada de la 
computadora que aloja su servidor web. Alternativamente, puede probar 
el ejemplo desde la misma computadora donde reside su servidor web 
ingresando lo siguiente:

```
194.0.0.1/get.html
```

Complete los campos del formulario con valores numéricos y seleccione 
un operador aritmético. La Figura 2-5 muestra la página web `get.html` 
con los valores 200 y 100 ingresados en los campos numéricos y también 
el signo de multiplicación seleccionado de la lista desplegable. Haga 
clic en el botón Calcular para enviar los datos del formulario a 
`get.php`. Abrir imagen en una ventana nueva Figura 2-5

Figura 2-5 La página web `get.html` con los campos del formulario 
completados

La Figura 2-6 muestra la página web resultante de la evaluación de 
`get.php`, el programa `action` en el servidor web, que recibió los 
datos del formulario `get.html`. El código PHP multiplica las dos 
variables numéricas `$num1` y `$num2`, y el resultado se imprime con 
el comando `echo` en la salida devuelta por el servidor web al navegador 
del cliente. Abrir imagen en una ventana nueva Figura 2-6

Figura 2-6 La página web `get.php` evaluada tal como aparece en el 
navegador del usuario

Los datos enviados por el usuario se agregan como la cadena de consulta 
(query string) en la URL. La cadena de consulta, para este ejemplo, es 
la siguiente:

```
n1=200&operator=multiply&n2=100
```
Esto incluye los pares nombre-valor de todos los campos de formulario 
enviados al servidor y comprende la información recibida por el programa 
PHP, que se utiliza para completar el array `$_GET` y proporcionar al 
programa PHP las variables definidas por el usuario.


## Configuración de las variables PHP con el método POST

Probemos ahora el método POST del elemento de formulario, que es otra 
opción para enviar datos al servidor web. Cree el archivo `post.html` 
de la siguiente manera:

```
$ cd /var/www/html
$ sudo gedit post.html
```

Introduzca el siguiente código fuente HTML, que se diferencia del 
código `get.html` solo por los valores del método y la acción de los 
atributos del formulario:

```
<!DOCTYPE html>
<html>
<head>
<style>
input, select{
font-size:24px;
}
</style>
</head>
<body>
<form method="post" action="post.php">
<input type="number" name="n1">
<select name="operator">
  <option value="add">+</option>
  <option value="subtract">-</option>
  <option value="multiply">*</option>
  <option value="divide">/</option>
</select>
<input type="number" name="n2">
<input type="submit" value="Calculate">
</form>
</body>
</html>
```
Por tanto, el método utilizado es `POST`, y el archivo PHP `post.php` 
se utiliza para gestionar la peticion del usuario. Cree el archivo 
`post.php` también en la raíz del documento de la siguiente manera:

```
$ cd /var/www/html
$ sudo gedit post.php
```

Ingrese el siguiente código fuente y guarde el archivo:

```
<!DOCTYPE html>
<html>
<head>
<style>
p{
font-size:24px;
}
</style>
</head>
<body>
<p>
<?php
$num1 = $_POST["n1"];
$num2 = $_POST["n2"];
$operator = $_POST["operator"];
switch($operator) {
    case "add":
         echo $num1 + $num2;
         break;
    case "subtract":
         echo $num1 - $num2;
         break;
    case "multiply":
         echo $num1 * $num2;
         break;
    case "divide":
         echo $num1 / $num2;
         break;
}
?>
</p>
</body>
</html>
```

Para probar la interacción cliente-servidor de este ejemplo, use otra 
computadora en su LAN y en la barra de direcciones del navegador ingrese 
lo siguiente:

```
192.168.1.100/post.html
```

Aquí, `192.168.1.100` es la dirección IP privada de la computadora que 
aloja su servidor web. Alternativamente, puede probar el ejemplo desde 
la misma computadora donde reside su servidor web ingresando lo 
siguiente:

```
127.0.0.1/post.html
```

Proporcione un valor numérico para cada uno de los dos campos y elija 
también un operador aritmético de la lista desplegable. En la Figura 
2-7, los dos campos de formulario de la página web `post.html` se 
completan con los valores numéricos `3000` y `5`, respectivamente, y 
el operador división se selecciona de la lista desplegable.
Abrir imagen en una ventana nueva Figura 2-7

Figura 2-7 La página web `post.html` con el formulario completado

Haga clic en el botón Calcular. Los valores del formulario se envían a 
`post.php`. En el servidor web, se invoca el motor PHP y los valores 
del formulario se recuperan de la variable global `$_POST`, que es un 
array similar a `$_GET` con los valores enviados desde los elementos 
del formulario. El código fuente PHP realiza la división entre `3000` 
y `5`. El resultado se envía con el comando `echo` a la página web, 
como se muestra en la Figura 2-8, y se envía desde el servidor web al 
usuario.

Abrir imagen en una ventana nueva Figura 2-8

Figura 2-8 La página web `post.php` evaluada como aparece en el 
navegador del usuario

Tenga en cuenta que en la barra de direcciones de su navegador, la URL 
sigue siendo la misma. Con el método `POST`, los datos no se incluyen 
en la URL de la línea de solicitud HTTP, como con el método `GET`, sino 
que se añaden al cuerpo de la peticion HTTP enviada al servidor web. En 
la siguiente sección, se utiliza otro formulario para enviar datos a un 
programa PHP en el servidor, y este proceso se compara con el código 
JavaScript que realiza el cálculo correspondiente localmente.


## Running Client-Side vs. Server-Side Programs

You will next compare how server-side programs and client-side programs act, using JavaScript and PHP source code embedded in the same web page. Both languages are used to perform the same operation, specifically an arithmetic addition. JavaScript runs on the client side in the client’s browser and displays the web page. PHP runs on the server side since the addition is performed on the web server that provides the specific web page.

A simple pattern for defining the programming process is to input the data to the source code and output the results. With JavaScript, the web server submits the source code via the network, and then the input/output data is used locally on the client’s system. With PHP, the contrary applies: the client submits the data to the server, which makes the calculation remotely to the client, and the output is sent back to the client, as shown in Figure 2-9.

Open image in new windowFigure 2-9

Figure 2-9 Defining the programming process


## The JavaScript/PHP Addition Web Page

In this section, you’ll create two calculators, which for simplicity here only do addition, in the same web page, program.html. The first is a client-side calculator that runs JavaScript, and the second is a server-side calculator that runs PHP. At the Linux terminal, create program.html in the document root of the web server with the following terminal commands:
$ cd /var/www/html
$ sudo gedit program.html

In the gedit window, enter the following HTML page that embeds the PHP and JavaScript source code and save the file:
<!DOCTYPE html>
<html>
<head>
<title>Client side and Server side programs</title>
<style>
input{
font-size:24px;
text-align:right;
background-color:lime;
}
input[type="submit"] {
background-color:yellow;
color:lime;
}
button{
font-size:24px;
color:lime;
background-color:yellow;
}
span{
font-size:24px;
color:lime;
}
</style>
</head>
<body>
<div>
<span>Add using JavaScript: </span>
<input type="text" id = "t1" size=5>
<span>+</span>
<input type="text" id = "t2" size=5>
<button onclick="f1()">=</button>
<input type = "text" id = "t3" size=5>
</div>
<div>
<br>
<span>Add using PHP: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
<form method="get" action="addition.php" style="display:inline;">
<input type="text" name="t4" size=5>
<span>+</span>
<input type="text" name="t5" size=5>
<input type="submit" value="=">
</form>
</span>
</div>
<script>
function f1() {
var x = Number(document.getElementById("t1").value);
if (isNaN(x)) {
alert("Please enter a number");
return false;
}
var y = Number(document.getElementById("t2").value);
if (isNaN(y)) {
alert("Please enter a number");
return false;
}
var z = x + y;
document.getElementById("t3").value = z;
}
</script>
</body>
</html>

The HTML file is used to perform addition in two ways: by using JavaScript and by using PHP. For JavaScript, the fields t1 and t2 are used to fill the operands and t3 is used to output the sum. When the button with the equal sign is clicked, the calculation is performed. This button is assigned function f1() for handling the onclick event . The JavaScript section implements function f1(). This routine sets the JavaScript variable x to the current value of the element t1 (the first textbox) and sets the JavaScript variable y to the current value of the element t2 (the second textbox). The JavaScript function Number() is then used to convert the textbox value to a numeric value. Without using Number() , the value 5 would represent the character 5, and the addition 5+5 would result in 55. Because with JavaScript the calculation is performed locally in the browser, no data is transferred to and from the web server, and therefore when using JavaScript, the URL never changes.

The second way to do the addition is to use PHP. With PHP the values of textboxes t4 and t5 are transferred to the web server via a form submission when the submit button is clicked. The submit button looks like the equal button from the JavaScript technique, but it functions by sending the data to the web server contrary to the JavaScript button that invokes a function to make the calculation locally. The program that receives the data on the web server is determined by the value of the action attribute of the form. This is set to addition.php, which is a PHP file that should be found in the document root of the web server, as indicated by the relative URL that includes only the file name.

Create this program with the following command:
$ sudo gedit addition.php

In the gedit window, enter the following source code and click the Save button:
<!DOCTYPE html>
<html>
<head>
<style>
body{
background-color:lime;
font-size:24px;
}
</style>
</head>
<body>
<?php
$x = $_GET["t4"];
$y = $_GET["t5"];
$z = $x + $y;
echo $z;
?>
</body>
</html>

The PHP file addition.php includes a PHP block where the value sent by textbox t4 is retrieved from the $_GET[] global PHP array, and it is assigned then to the PHP variable $x. Similarly, the value sent by textbox t5 is stored in the PHP variable $y. Next, $x and $y are added together, and the result is assigned to the variable $z. The value of $z is printed with the echo command to the web page sent back to the browser.

To try the calculators, enter the following URL in your browser’s address bar:
localhost/program.html

On the web page you can test the JavaScript code first. In the example in Figure 2-10, the result of adding 6 and 9 appears when the first equal sign button is clicked.
Open image in new windowFigure 2-10
Figure 2-10

Performing the addition locally with the JavaScript calculator
Test the PHP addition next. Enter two numbers in the PHP fields. In the example in Figure 2-9, the numbers 8 and 12 were entered. Click the button with the equal sign, which is an HTML form submit button. The evaluated source code of the action file, addition.php , appears, as shown in Figure 2-11.
Open image in new windowFigure 2-11
Figure 2-11

The result of the PHP addition as returned from the web server

In the case of JavaScript, the source code is executed locally on the same computer, and the web page remains the same. For the PHP code, the code is executed in the web server, and the reply is submitted to the client with a new page. In the previous example, it may look like an unfair comparison because PHP provides the result in a web page that is not consistent with the initial one. With the next versions of the files program.html and addition.php, this can be fixed.


### The Second Version of the JavaScript/PHP Addition Web Page

Create a file called program2.html with gedit or any other text editor. Enter the following source code in program2.html:
<!DOCTYPE html>
<html>
<head>
<title>Client side and Server side programs</title>
<style>
input{
font-size:24px;
text-align:right;
background-color:lime;
}
input[type="submit"], input[type="button"] {
background-color:yellow;
color:lime;
}
button{
font-size:24px;
color:lime;
background-color:yellow;
}
span{
font-size:24px;
color:lime;
}
</style>
</head>
<body>
<div>
<span>Add using JavaScript: </span>
<form method="get" action="addition2.php" style="display:inline;">
<input type="text" id = "t1" size=5 name="t1">
<span>+</span>
<input type="text" id = "t2" size=5 name="t2">
<input type="button" onclick="f1()" value="=">
<input type = "text" id = "t3" size=5 name="t3">
</div>
<br>
<div>
<span>Add using PHP: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
<input type="text" name="t4" size=5>
<span>+</span>
<input type="text" name="t5" size=5>
<input type="submit" value="=">
<input type="text" name="t6" size=5>
</form>
</span>
</div>
<script>
function f1() {
var x = Number(document.getElementById("t1").value);
if (isNaN(x)) {
alert("Please enter a number");
return false;
}
var y = Number(document.getElementById("t2").value);
if (isNaN(y)) {
alert("Please enter a number");
return false;
}
var z = x + y;
document.getElementById("t3").value = z;
}
</script>
</body>
</html>

The HTML file program2.html differs from program.html because of the value of the action attribute of the form element. Instead of addition.php, the program addition2.php is used. But there is another difference as well: the first three textboxes, t1, t2, and t3, used for the JavaScript section now use a name attribute, which will be used to transfer their values to the web server when the submit button is clicked. Those values will not be used for any calculation by the PHP web server program, addition2.php, but the values will be submitted to the addition2.php source code to be returned as values in the corresponding textboxes in the evaluated page. Thus, a continuity will be retained with the state of the original page, program2.html.

Create a file named addition2.html in the document root of your server with the following commands:
$ cd /var/www/html
$ sudo gedit addition2.php

Enter the following code and save the file:
<!DOCTYPE html>
<html>
<head>
<title>Client side and Server side programs</title>
<style>
input{
font-size:24px;
text-align:right;
background-color:lime;
}
input[type="submit"], input[type="button"] {
background-color:yellow;
color:lime;
}
button{
font-size:24px;
color:lime;
background-color:yellow;
}
span{
font-size:24px;
color:lime;
}
</style>
</head>
<body>
<?php
if (isset($_GET["t1"])){
$t1 = $_GET["t1"];
}
if (isset($_GET["t2"])){
$t2 = $_GET["t2"];
}
if (isset($_GET["t3"])){
$t3 = $_GET["t3"];
}
if (isset($_GET["t4"])){
$t4 = $_GET["t4"];
}
if (isset($_GET["t5"])){
$t5 = $_GET["t5"];
}
if (isset($t4) && isset($t5)){
$t6 = $t4 + $t5;
}
?>
<form name="f1" action="addition2.php" method="GET">
<div>
<span>Add using JavaScript: </span>
<input type = "text" id="t1" name = "t1" size=5 value = "<?php echo $t1 ?>" >
<span>+</span>
<input type = "text" id="t2" name = "t2" size=5 value = "<?php echo $t2 ?>" >
<input type="button" value="=" onclick="function1()">
<input type = "text" id="t3" name = "t3" size=5 value = "<?php echo $t3 ?>" >
</div>
<br>
<div>
<span>Add using PHP: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
<input type = "text" name = "t4" size=5 value = "<?php echo $t4 ?>" >
<span>+</span>
<input type = "text" name = "t5" size=5 value = "<?php echo $t5 ?>" >
<input type="submit" value="=">
<input type = "text" name = "t6" size=5 value = "<?php echo $t6 ?>" >
</div>
</form>
<script>
function function1() {
var x = Number(document.getElementById("t1").value);
if (isNaN(x)) {
alert("Please enter a number");
return false;
}
var y = Number(document.getElementById("t2").value);
if (isNaN(y)) {
alert("Please enter a number");
return false;
}
var z = x + y;
document.getElementById("t3").value = z;
}
</script>
</body>
</html>

In this new version of the PHP file, the result of the PHP addition appears as the value of a new textbox, named t6, visually simulating the JavaScript calculator. You can test both calculators in this new version. As displayed in Figure 2-12, the fields of the JavaScript calculator reflect the addition previously issued, and in the PHP calculator the operand fields are set for the addition.
Open image in new windowFigure 2-12
Figure 2-12

The web page program2.html before submitting the form
After clicking the submit button, the result appears in the evaluated addition2.php web page. As displayed in Figure 2-13, addition2.php has the same look and feel as the previous web page, program2.html. Moreover, all textbox values from both JavaScript and PHP sections are retained.
Open image in new windowFigure 2-13
Figure 2-13

The evaluated web page addition2.php when the form is submitted

This is achieved with the following PHP block:
<?php
if (isset($_GET["t1"])){
$t1 = $_GET["t1"];
}
if (isset($_GET["t2"])){
$t2 = $_GET["t2"];
}
if (isset($_GET["t3"])){
$t3 = $_GET["t3"];
}
if (isset($_GET["t4"])){
$t4 = $_GET["t4"];
}
if (isset($_GET["t5"])){
$t5 = $_GET["t5"];
}
if (isset($t4) && isset($t5)){
$t6 = $t4 + $t5;
}
?>

In the previous PHP block, the values of the JavaScript textboxes, t1, t2, and t3, are submitted along with two of the values of the PHP section, t4 and t5. The value for the result of the PHP section, appearing in t6, is calculated next from the values of t4 and t5. Function isset() is used to check whether a variable is set.

To return the values of t1 up to t6 in the corresponding textboxes, an echo command is included for each text input element in a separate PHP block. For instance, for t1, its last value, maintained in the PHP code in variable $t1, is printed in the first textbox with the following PHP block contained in the input element:
<input type = "text" id="t1" name = "t1" size=5 value = "<?php echo $t1 ?>" >

The previous input tag processed by the PHP engine, for instance when t1 currently has the value 12, is as follows:
<input type = "text" id="t1" name = "t1" size=5 value = "12" >

This value, carried to the server side and returned to the client side, is maintained to retain the state of the two calculators.

To enable the evaluated page to use the JavaScript calculator, the script of program2.html is also included in addition2.php.
<script>
function function1() {
var x = Number(document.getElementById("t1").value);
if (isNaN(x)) {
alert("Please enter a number");
return false;
}
var y = Number(document.getElementById("t2").value);
if (isNaN(y)) {
alert("Please enter a number");
return false;
}
var z = x + y;
document.getElementById("t3").value = z;
}
</script>

Figure 2-14 shows the results. You can access the JavaScript fields in addition2.php to make further local additions.
Open image in new windowFigure 2-14
Figure 2-14

The new version addition2.php enables both remote PHP additions and local JavaScript additions

Next you’ll create the final version of this site.


### The Third Version of the JavaScript/PHP Addition Web Page

Since addition2.php evaluates to the program2.php web page that submits the data, program2.php can be completely omitted from the site. Therefore, the only PHP file required, addition.php, can replace program2.php as the home directory of the site. The following URL will be used:
localhost/addition2.php

In the following section, you will create a site that validates promotional codes dispatched from an HTML form and uses PHP to validate the data submitted by this form.


## Form Validation with PHP

So far you have used HTML and JavaScript to validate a web page. Another option you have is to use PHP. With PHP you validate the web page remotely. To test this scheme, create a new PHP site that simulates a scenario where promotional codes are redeemed so that the user can win free products. At the Linux terminal, create the validate.php file in the document root of the web server.
$ cd /var/www/html
$ sudo gedit validate.php

Enter the following source code and save the file:
<html>
<head>
<title>PHP Form Validation</title>
<style>
h1{
color:orange;
}
.error{
color:red;
font-size:20px;
}
label{
color:blue;
font-size:24px;
}
input{
color:blue;
font-size:24px;
background-color:orange;
}
</style>
</head>
<body>
<?php
 $errormsg1="";
 $errormsg2="";
 $errormsg3="";
 $valid1=false;
 $valid2=false;
 $valid3=false;
if (isset($_POST['submit']))
{
 $name=$_POST["name"];
 $email=$_POST["email"];
 $code=$_POST["code"];
 if(empty($name) || is_numeric($name))
 {
 $errormsg1.='<p class="error">* Please enter a valid name.</p>';
 $valid1=false;
 }
 else
 {
 if(is_string($name))
 $valid1=true;
 else
 {
 $errormsg1.='<p class="error">* Please use valid characters.</p>';
 $valid1=false;
 }
 }
 if(empty($email) || is_numeric($email))
 {
 $errormsg2.='<p class="error"> * Please enter your e-mail.</p>';
 $valid2=false;
 }
 else
 {
 if(is_string($email))
 $valid2=true;
 else
 {
 $errormsg2.='<p class="error">* Please use valid characters.</p>';
 $valid2=false;
 }
 }
 if(empty($code))
 {
 $errormsg3.='<p class="error">* Please enter your code number.</p>';
 $valid3=false;
 }
 else
 {
 $len=strlen($code);
 if($len==10)
 $valid3=true;
 else
 {
 $errormsg3.='<p class="error">* Code should be in alphabetic letters and numerical digits format with 10 characters in it.</p>';
 $valid3=false;
 }
 }
 if($valid1==true && $valid2==true && $valid3==true)
 header("Location:process.php? code=$code&name=$name&email=$email");
}
?>
<h1>Submit your name, your e-mail, and your code</h1>
<form name="form1" method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">
<label for="name">Full Name:</label>
<input type="text" name="name"><br>
<?php
 if((errormsg1!="") && isset($_POST['submit']))
 echo $errormsg1;
?>
<label for="email">E-mail:</label>
<input type="text" name="email"><br>
<?php
 if((errormsg2!="") && isset($_POST['submit']))
 echo $errormsg2;
?>
<label for="code">Code:</label>
<input type="text" name="code"><br>
<?php
 if((errormsg3!="") && isset($_POST['submit']))
 echo $errormsg3;
?>
<input type="submit" name="submit" value="Go">
</form>
</body>
</html>

As displayed in Figure 2-15, validate.php creates a web page that includes a form with three fields and a submit button, with the caption Go.
Open image in new windowFigure 2-15
Figure 2-15

The web page created by validate.php
For this form, all fields are required to be filled in, and the code must be an alphanumeric string of 10 characters. Figure 2-16 displays the error messages generated by the PHP code when the user clicks the Go button without completing all the fields.
Open image in new windowFigure 2-16
Figure 2-16

The error messages displayed in the web page when fields are incomplete
Test the form by completing just two of the fields, e.g., Full Name and Code, with the latter including only three characters. Figure 2-17 displays an example. Click the Go button.
Open image in new windowFigure 2-17
Figure 2-17

Testing the form with only two fields completed
The PHP source code validates the form according to the rule set and displays two warnings, one for the empty field and one for the length of the string entered in the Code textbox. Figure 2-18 displays the warnings.
Open image in new windowFigure 2-18
Figure 2-18

The warnings displayed for the entries of Figure 2-16

In the following section, I’ll discuss the PHP source code for the form validation.


### The validate.php Source Code Commentary

The Go button of the form, which is of type submit, relays the data to the PHP file indicated by the value of the form’s action attribute. This is set to $_SERVER["PHP_SELF"], which is another variable of the global PHP $_SERVER[] array. This value is filled by the PHP engine on the fly relative to the document root path of the current file, for this example /validate.php. Notice that the action attribute of the form is set as follows:
action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>

The value of the action is enclosed in the PHP tags so that the value of
htmlspecialchars($_SERVER["PHP_SELF"])

is actually printed as the action’s value. Function htmlspecialchars() is used to sanitize the file’s pathname so that symbols like the double quote (") is converted to &quot;, which is the equivalent HTML entity. The purpose of this is to avoid a possible $_SERVER["PHP_SELF"] exploit, which is a cross-site scripting (XSS) exploit, where a hacker injects JavaScript code into web pages viewed by other users.

At the beginning of the PHP source code, three fields are evaluated. For instance, consider the following validation code for the value of the first field sent as $_POST["name"]:
if (isset($_POST['submit']))
{
 $name=$_POST["name"];
 $email=$_POST["email"];
 $code=$_POST["code"];
 if(empty($name) || is_numeric($name))
 {
 $errormsg1.='<p class="error">* Please enter a valid name.</p>';
 $valid1=false;
 }
 else
 {
 if(is_string($name))
 $valid1=true;
 else
 {
 $errormsg1.='<p class="error">* Please use valid characters.</p>';
 $valid1=false;
 }
 }

With function isset() , it is ensured that the evaluation takes place only when the submit button is clicked and the submit method is POST. Otherwise, the fields would get evaluated even before the user had the chance to fill them.
if (isset($_POST['submit']))

The variables $valid1, $valid2, and $valid3 are used as flags to signal a valid (true) or an invalid (false) value for the first, second, and third fields, respectively. Also, the variables $errormsg1, $errormsg2, and $errormsg3 are used to specify the appropriate error message for the first, second, and third fields, respectively. For instance, with the following commands, errormsg3 appears when the value of the code does not have a length of ten characters:
if($len==10)
 $valid3=true;
 else
 {
 $errormsg3.='<p class="error">* Code should be in alphabetic letters and numerical digits format with 10 characters in it.</p>';

The concatenating assignment operator (.=) is a PHP string operator that appends the argument on the right side to the argument on the left side.

The form submits to the same PHP file, where it is included, while the user does not provide the expected values, thus allowing for validating the form. When all three fields are filled with valid values, the following if condition holds true and the header() function runs:
if($valid1==true && $valid2==true && $valid3==true)
 header("Location:process.php? code=$code&name=$name&email=$email");

The header() function , which sends raw HTTP headers, is used here with the Location HTTP header. This function redirects the browser to the URL indicated by the Location value. In this example, the value includes the attached query string with the variable-value pairs required to be forwarded to the destination file, process.php. Thus, the function header() provides the escape mechanism to validate.php to break out of the loop of continuously submitting data to itself.

Create the file process.php in the document root with the following commands:
$ cd /var/www/html
$ sudo gedit process.php

Enter the following source code and save the file:
<html>
<head>
<title>Code evaluation</title>
<style>
p{
color:green;
font-size:32px;
}
</style>
</head>
<body>
<p>
<?php
$var=$_GET['code'];
$var2=$_GET['name'];
$var3=$_GET['email'];
if(isset($var) && $var == 'SX1DF908RW')
{
echo nl2br("$var2 congratulations you have entered the lucky code: $var\n You have won one T-shirt. We will contact you soon.");
// Save the user's e-mail for contacting him/her
$filename = "code.txt";
$handle = fopen($filename, "w") or die(" Unable to open file!");
if ($handle)
{
fwrite($handle, $var2);
fwrite($handle, PHP_EOL);
fwrite($handle, $var3);
fwrite($handle, PHP_EOL);
fclose($handle);
}
}
else
{
    echo "You didn't win, please try another time!";
}
?>
</p>
</body>
</html>

The three variable values submitted (code, name, and email) from header() in file validate.php are now retrieved as var, var2, and var3, respectively. The following if condition looks for the lucky code:
if(isset($var) && $var == 'SX1DF908RW')

If the condition is true, the following message is returned to the browser:
echo nl2br("$var2 congratulations you have entered the lucky code: $var\n You have won one T-shirt. We will contact you soon.");

The function nl2br() is used to translate escape characters to their meaning; for instance, \n is treated now as a newline.
Test the validate.php form by providing the correct code, SX1DF908RW, as shown in Figure 2-19.
Open image in new windowFigure 2-19
Figure 2-19

The validate.php form filled with the correct code
Click the Go button. The form is submitted, and the browser redirects to process.php, which displays the message shown in Figure 2-20 to the client browser.
Open image in new windowFigure 2-20
Figure 2-20

The message displayed from process.php for the correct code
Try validating validate.php with a wrong code, as displayed in Figure 2-21.
Open image in new windowFigure 2-21
Figure 2-21

Testing validate.php with a wrong code
Click the Go button to redirect to process.php. The message displayed to the client’s browser is shown in Figure 2-22.
Open image in new windowFigure 2-22
Figure 2-22

The reply viewed in the user’s browser for a wrong code

Let’s not forget the promise to get back to the user. To save the visitor’s details, the PHP source code is used to provide the interface between the web server and the local filesystem. A new file called code.txt is required to store the winner’s personal details: the full name and the e-mail. With the following PHP code file, code.txt is used to store this information. The file handle $handle is created with fopen(), which in this example is used for writing (w mode) to the file, with the name indicated in the value of $filename. The function fwrite() is used twice to fill the winner’s name and e-mail and also the PHP_EOL value between. This inserts a line break.
// Save the user's e-mail for contacting him/her
$filename = "code.txt";
$handle = fopen($filename, "w") or die(" Unable to open file!");
if ($handle)
{
fwrite($handle, $var2);
fwrite($handle, PHP_EOL);
fwrite($handle, $var3);
fwrite($handle, PHP_EOL);
fclose($handle);
}

To enable the web server (and the PHP engine) to access file code.txt, create it first with the touch command and then change its ownership to belong to the user www-data, a member of the group www-data, which is the user assumed by the web server. At the command line, enter the following:
$ sudo touch /var/www/html/code.txt
$ sudo chown www-data:www-data /var/www/html/code.txt

Figure 2-23 displays the content of code.txt, including the personal details for the user who provided the correct code.
Open image in new windowFigure 2-23
Figure 2-23

The personal details of the winner stored in file code.txt

The user’s personal details are thus saved for contacting the user to claim their prize.

&____________________________________________________________________
