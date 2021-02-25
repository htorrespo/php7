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


## Configuración de variables PHP con el método POST

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


## Ejecución de programas del lado del cliente frente a del lado del servidor

A continuación, comparará cómo actúan los programas del lado del 
servidor y los programas del lado del cliente, utilizando JavaScript 
y código fuente PHP incrustado en la misma página web. Ambos lenguajes 
se utilizan para realizar la misma operación, específicamente una suma 
aritmética. JavaScript se ejecuta en el lado del cliente en el navegador 
del cliente y muestra la página web. PHP se ejecuta en el lado del 
servidor, ya que la adición se realiza en el servidor web que proporciona 
la página web específica.

Un patrón simple para definir el proceso de programación es ingresar 
los datos al código fuente y generar los resultados. Con JavaScript, 
el servidor web envía el código fuente a través de la red y luego los 
datos de entrada/salida se utilizan localmente en el sistema del cliente.
Con PHP, se aplica lo contrario: el cliente envía los datos al servidor,
que realiza el cálculo de forma remota al cliente, y la salida se envía 
de vuelta al cliente, como se muestra en la Figura 2-9.

Abrir imagen en una ventana nueva Figura 2-9

Figura 2-9 Definición del proceso de programación


## La página web de adición JavaScript / PHP

En esta sección, creará dos calculadoras, que por simplicidad aquí 
solo suman, en la misma página web, `program.html`. La primera es una 
calculadora del lado del cliente que ejecuta JavaScript y la segunda 
es una calculadora del lado del servidor que ejecuta PHP. En la terminal 
de Linux, cree `program.html` en la raíz del documento del servidor 
web con los siguientes comandos de terminal:

```
$ cd /var/www/html
$ sudo gedit program.html
```

En la ventana de `gedit`, ingrese la siguiente página HTML que incluye 
el código fuente PHP y JavaScript y guarde el archivo:

```
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
```

El archivo HTML se usa para realizar sumas de dos maneras: usando 
JavaScript y usando PHP. Para JavaScript, los campos `t1` y` t2` se 
usan para completar los operandos y `t3` se usa para generar la suma. 
Cuando se hace clic en el botón con el signo igual, se realiza el 
cálculo. A este botón se le asigna la función `f1()` para manejar el 
evento `onclick`. La sección de JavaScript implementa la función `f1()`.
Esta rutina establece la variable JavaScript `x` en el valor actual 
del elemento `t1` (el primer cuadro de texto) y establece la variable 
JavaScript y en el valor actual del elemento `t2` (el segundo cuadro 
de texto). La función de JavaScript `Number()` se utiliza para convertir 
el valor del cuadro de texto en un valor numérico. Sin el uso de 
`Number()`, el valor `5` representaría el carácter `5`, y la suma 
`5 + 5` daría como resultado `55`. Debido a que con JavaScript el 
cálculo se realiza localmente en el navegador, no se transfieren datos 
hacia y desde el servidor web, y por lo tanto, cuando se usa JavaScript, 
la URL nunca cambia.

La segunda forma de hacer la suma es usar PHP. Con PHP, los valores de 
los cuadros de texto `t4` y `t5` se transfieren al servidor web mediante 
el envío de un formulario cuando se hace clic en el botón Enviar. El 
botón enviar se parece al botón igual de la técnica de JavaScript, pero 
funciona enviando los datos al servidor web al contrario que el botón 
JavaScript que invoca una función para realizar el cálculo localmente. 
El programa que recibe los datos en el servidor web está determinado 
por el valor del atributo `action` del formulario. Se establece en 
`added.php`, que es un archivo PHP que se debe encontrar en la raíz del 
documento del servidor web, como lo indica la URL relativa que incluye 
solo el nombre del archivo.

Crear este programa con el siguiente comando:

```
$ sudo gedit addition.php
```

En la ventana de gedit, ingrese el siguiente código fuente y haga clic 
en el botón Guardar:

```
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
```

El archivo PHP `added.php` incluye un bloque PHP donde el valor enviado 
por el cuadro de texto `t4` se recupera del array PHP global `$_GET[]`, 
y luego se asigna a la variable PHP `$x`. De manera similar, el valor 
enviado por el cuadro de texto `t5` se almacena en la variable PHP `$y`. 
A continuación, se suman `$x` y `$y`, y el resultado se asigna a la 
variable `$z`. El valor `$z` se imprime con el comando echo en la página 
web que se envía al navegador.

Para probar las calculadoras, ingrese la siguiente URL en la barra de 
direcciones de su navegador:

```
localhost/program.html
```

En la página web, primero puede probar el código JavaScript. En el 
ejemplo de la Figura 2-10, el resultado de sumar 6 y 9 aparece cuando 
se hace clic en el primer botón de signo igual.

Abrir imagen en una ventana nueva Figura 2-10

Figura 2-10 Realización de la suma localmente con la calculadora 
JavaScript

Pruebe la adición de PHP a continuación. Ingrese dos números en los 
campos de PHP. En el ejemplo de la Figura 2-9, se ingresaron los números 
8 y 12. Haga clic en el botón con el signo igual, que es un botón de 
envío de formulario HTML. El código fuente evaluado del archivo 
`action`, `added.php`, aparece, como se muestra en la Figura 2-11.

Abrir imagen en una ventana nueva Figura 2-11

Figura 2-11 El resultado de la adición de PHP, devuelto por el 
servidor web

En el caso de JavaScript, el código fuente se ejecuta localmente en 
la misma computadora y la página web sigue siendo la misma. Para el 
código PHP, el código se ejecuta en el servidor web y la respuesta se 
envía al cliente con una nueva página. En el ejemplo anterior, puede 
parecer una comparación injusta porque PHP proporciona el resultado en 
una página web que no es consistente con la inicial. Con las próximas 
versiones de los archivos `program.html` y `added.php`, esto se puede 
solucionar.


### Segunda versión de la página web de adición JavaScript/PHP

Cree un archivo llamado `program2.html` con gedit o cualquier otro 
editor de texto. Ingrese el siguiente código fuente en `program2.html`:

```
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
```

El archivo HTML `program2.html` difiere de `program.html` por el valor 
del atributo `action` del elemento de formulario. En lugar de 
`added.php`, se utiliza el programa `added2.php`. Pero también hay 
otra diferencia: los primeros tres cuadros de texto, `t1`, `t2` y `t3`, 
que se usan para la sección de JavaScript ahora usan un atributo 
`name`, que se usará para transferir sus valores al servidor web cuando 
se haga clic en el botón Enviar. Esos valores no serán usados para 
ningún cálculo por el programa de servidor web PHP, a`dded2.php`, pero 
los valores serán enviados al código fuente de `added2.php` para ser 
devueltos como valores en los cuadros de texto correspondientes en la 
página evaluada. Por lo tanto, se mantendrá una continuidad con el 
estado de la página original, `program2.html`.

Crear un archivo llamado `added2.html` en la raíz del documento de su 
servidor con los siguientes comandos:

```
$ cd /var/www/html
$ sudo gedit addition2.php
```

Enter the following code and save the file:

```
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
```


En esta nueva versión del archivo PHP, el resultado de la adición de 
PHP aparece como el valor de un nuevo cuadro de texto, llamado `t6`, 
que simula visualmente la calculadora JavaScript. Puede probar ambas 
calculadoras en esta nueva versión. Como se muestra en la Figura 2-12, 
los campos de la calculadora de JavaScript reflejan la adición emitida 
previamente, y en la calculadora de PHP los campos de operandos se 
establecen para la suma.

Abrir imagen en una ventana nueva Figura 2-12

Figura 2-12 La página web `program2.html` antes de enviar el formulario

Después de hacer clic en el botón enviar, el resultado aparece en la 
página web de `add2.php` evaluada. Como se muestra en la Figura 2-13, 
`added2.php` tiene la misma apariencia que la página web anterior, 
`program2.html`. Además, se conservan todos los valores de cuadro de 
texto de las secciones de JavaScript y PHP.

Abrir imagen en una ventana nueva Figura 2-13

Figura 2-13 La página web evaluada `added2.php` cuando se envía el 
formulario

Esto se logra con el siguiente bloque PHP:

```
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
```

En el bloque PHP anterior, los valores de los cuadros de texto de 
JavaScript, `t1`, `t2` y `t3`, se envían junto con dos de los valores 
de la sección PHP, `t4` y `t5`. El valor del resultado de la sección 
PHP, que aparece en `t6`, se calcula a continuación a partir de los 
valores de `t4` y `t5`. La función `isset()` se usa para verificar si 
una variable está configurada.

Para devolver los valores de `t1` hasta `t6` en los cuadros de texto 
correspondientes, se incluye un comando `echo` para cada elemento de 
entrada de texto en un bloque PHP separado. Por ejemplo, para `t1`, su 
último valor, mantenido en el código PHP en la variable `$t1`, se 
imprime en el primer cuadro de texto con el siguiente bloque PHP 
contenido en el elemento de entrada:

```
<input type = "text" id = "t1" name = "t1" size = 5 value = "<? php echo $ t1?>">
```

La etiqueta de entrada anterior procesada por el motor PHP, por ejemplo, 
cuando `t1` tiene actualmente el valor `12`, es la siguiente:

```
<input type = "text" id = "t1" name = "t1" size = 5 value = "12">
```

Este valor, trasladado al lado del servidor y devuelto al lado del 
cliente, se mantiene para conservar el estado de las dos calculadoras.


Para permitir que la página evaluada utilice la calculadora JavaScript, 
el script `program2.html` también se incluye en `additional2.php`.

```
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
```

La figura 2-14 muestra los resultados. Puede acceder a los campos de 
JavaScript en `added2.php` para realizar más adiciones locales.

Abrir imagen en una ventana nueva Figura 2-14

Figura 2-14 La nueva versión `added2.php` permite adiciones remotas de 
PHP y adiciones locales de JavaScript

A continuación, creará la versión final de este sitio.


### Tercera versión de la página web adición de JavaScript/PHP

Dado que `added2.ph`p evalúa la página web `program2.php` que envía 
los datos, `program2.php` puede omitirse por completo del sitio. Por 
lo tanto, el único archivo PHP requerido, `added.php`, puede reemplazar 
a `program2.ph`p como directorio de inicio del sitio. Se utilizará la 
siguiente URL:

```
localhost/added2.php
```

En la siguiente sección, creará un sitio que valida los códigos 
promocionales enviados desde un formulario HTML y utiliza PHP para 
validar los datos enviados por este formulario.


## Form Validation with PHP

Hasta ahora ha utilizado HTML y JavaScript para validar una página web. 
Otra opción que tienes es usar PHP. Con PHP valida la página web de 
forma remota. Para probar este esquema, cree un nuevo sitio PHP que 
simule un escenario donde se canjeen códigos promocionales para que el 
usuario pueda ganar productos gratis. En la terminal de Linux, cree el 
archivo `validate.php` en la raíz del documento del servidor web.

```
$ cd /var/www/html
$ sudo gedit validate.php
```

Ingrese el siguiente código fuente y guarde el archivo:

```
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
```

Como se muestra en la Figura 2-15, `validate.php` crea una página web 
que incluye un formulario con tres campos y un botón de envío, con el 
título `Go`.

Abrir imagen en una ventana nueva Figura 2-15

Figura 2-15 La página web creada por `validate.php`

Para este formulario, todos los campos deben completarse y el código 
debe ser una cadena alfanumérica de 10 caracteres. La Figura 2-16 
muestra los mensajes de error generados por el código PHP cuando el 
usuario hace clic en el botón `Go` sin completar todos los campos.

Abrir imagen en una ventana nueva Figura 2-16

Figura 2-16 Los mensajes de error que se muestran en la página web 
cuando los campos están incompletos

Pruebe el formulario completando solo dos de los campos, por ejemplo, 
Nombre completo y Código, y este último incluye solo tres caracteres. 
La Figura 2-17 muestra un ejemplo. Haga clic en el botón `Go`.

Abrir imagen en una ventana nueva Figura 2-17

Figura 2-17 Prueba del formulario con solo dos campos completados

El código fuente PHP valida el formulario de acuerdo con el conjunto de 
reglas y muestra dos advertencias, una para el campo vacío y otra para 
la longitud de la cadena ingresada en el cuadro de texto Código. La 
Figura 2-18 muestra las advertencias.

Abrir imagen en una ventana nueva Figura 2-18

Figura 2-18 Las advertencias que se muestran para las entradas de la 
Figura 2-16

En la siguiente sección, discutiré el código fuente PHP para la 
validación del formulario.


### Comentario para el código fuente `validate.php`

El botón `Go` del formulario, que es de tipo enviar, transmite los 
datos al archivo PHP indicado por el valor del atributo de acción del 
formulario. Se establece en `$_SERVER["PHP_SELF"]`, que es otra variable 
del array global PHP `$_SERVER[]`. El motor PHP rellena este valor sobre 
la marcha en relación con la ruta raíz del documento del archivo actual, 
para este `example/validate.php`. Observe que el atributo `action` del 
formulario se establece de la siguiente manera:

```
action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>
```

El valor de `action` se incluye en las etiquetas PHP para que el valor de


```
htmlspecialchars($_SERVER["PHP_SELF"])
```

se imprime realmente como el valor de `action`. La función 
`htmlspecialchars()` se usa para desinfectar el nombre de la ruta del 
archivo de modo que los símbolos como las comillas dobles (") se 
conviertan en `&quot`;, que es la entidad HTML equivalente. El propósito 
de esto es evitar un posible `$_SERVER[" PHP_SELF "]` exploit, que es 
un exploit de secuencias de comandos entre sitios (XSS), en el que un 
pirata informático inyecta código JavaScript en las páginas web que ven 
otros usuarios.

Al comienzo del código fuente PHP, se evalúan tres campos. Por ejemplo, 
considere el siguiente código de validación para el valor del primer 
campo enviado como

```
$_POST["name"]:
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
```

Con la función `isset()`, se garantiza que la evaluación se lleve a 
cabo solo cuando se hace clic en el botón de envío y el método de envío 
es `POST`. De lo contrario, los campos se evaluarían incluso antes de 
que el usuario tuviera la oportunidad de completarlos.

```
if (isset($_POST['submit']))
```

Las variables `$valid1, `$valid2` y `$valid3` se utilizan como 
indicadores para señalar un valor válido (verdadero) o no válido 
(falso) para el primer, segundo y tercer campo, respectivamente. 
Además, las variables `$errormsg1`, `$errormsg2` y `$errormsg3` se 
utilizan para especificar el mensaje de error apropiado para el primer, 
segundo y tercer campo, respectivamente. Por ejemplo, con los 
siguientes comandos, aparece `errormsg3` cuando el valor del código no 
tiene una longitud de diez caracteres:

```
if($len==10)
 $valid3=true;
 else
 {
 $errormsg3.='<p class="error">* Code should be in alphabetic letters and numerical digits format with 10 characters in it.</p>';
```


El operador de asignación de concatenación (`.=`) Es un operador de 
cadena de PHP que agrega el argumento del lado derecho al argumento 
del lado izquierdo.

El formulario se envía al mismo archivo PHP, donde se incluye, mientras 
que el usuario no proporciona los valores esperados, lo que permite 
validar el formulario. Cuando los tres campos se llenan con valores 
válidos, lo siguiente si la condición es verdadera y se ejecuta la 
función `header()`:

```
if($valid1==true && $valid2==true && $valid3==true)
 header("Location:process.php? code=$code&name=$name&email=$email");
```

La función `header()`, que envía encabezados HTTP sin procesar, se usa 
aquí con el encabezado HTTP Location. Esta función redirige el navegador 
a la URL indicada por el valor de Ubicación. En este ejemplo, el valor 
incluye la cadena de consulta adjunta con los pares de variable-valor 
que se deben reenviar al archivo de destino, `process.php`. Por lo 
tanto, la función `header()` proporciona el mecanismo de escape para 
`validar.php` para salir del ciclo de envío continuo de datos a sí mismo.

Cree el archivo `process.php` en la raíz del documento con los 
siguientes comandos:

```
$ cd /var/www/html
$ sudo gedit process.php
```

Ingrese el siguiente código fuente y guarde el archivo:

```
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
```

Los tres valores de variable enviados (`code`, `name` y `email`) desde 
el `header()` en el archivo `validate.php` ahora se recuperan como 
`var`, `var2` y `var3`, respectivamente. La siguiente condición `if` 
busca el código de la suerte:

```
if(isset($var) && $var == 'SX1DF908RW')
```

Si la condición es verdadera, el siguiente mensaje se devuelve al 
navegador:

```
echo nl2br("$var2 congratulations you have entered the lucky code: $var\n You have won one T-shirt. We will contact you soon.");
```

La función `nl2br()` se utiliza para traducir los caracteres de escape 
a su significado; por ejemplo, `\n` ahora se trata como una nueva línea.

Pruebe el formulario `validate.php` proporcionando el código correcto, 
`SX1DF908RW`, como se muestra en la Figura 2-19.

Abrir imagen en una ventana nueva Figura 2-19

Figura 2-19 El formulario `validate.php` lleno con el código correcto

Haga clic en el botón `Go`. Se envía el formulario y el navegador 
redirige a `process.php`, que muestra el mensaje que se muestra en la 
Figura 2-20 al navegador del cliente.

Abrir imagen en una ventana nueva Figura 2-20

Figura 2-20 El mensaje mostrado desde `process.php` para el código 
correcto

Intente validar `validate.php` con un código incorrecto, como se 
muestra en la Figura 2-21.

Abrir imagen en una ventana nueva Figura 2-21

Figura 2-21 Prueba de `validate.php` con un código incorrecto

Haga clic en el botón `Go` para redirigir a `process.php`. El mensaje 
que se muestra en el navegador del cliente se muestra en la Figura 2-22.

Abrir imagen en una ventana nueva Figura 2-22

Figura 2-22 La respuesta vista en el navegador del usuario por un 
código incorrecto

No olvidemos la promesa de volver con el usuario. Para guardar los 
detalles del visitante, el código fuente PHP se utiliza para proporcionar 
la interfaz entre el servidor web y el sistema de archivos local. Se 
requiere un nuevo archivo llamado `code.txt` para almacenar los datos 
personales del ganador: el nombre completo y el correo electrónico. Con 
el siguiente archivo de código PHP, se usa `code.txt` para almacenar 
esta información. El identificador de archivo `$handle` se crea con 
`fopen()`, que en este ejemplo se usa para escribir (modo `w`) en el 
archivo, con el nombre indicado en el valor de `$filename`. La función 
`fwrite()` se usa dos veces para completar el nombre y el correo 
electrónico del ganador y también el valor PHP_EOL entre. Esto inserta 
un salto de línea.

```
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
```

Para permitir que el servidor web (y el motor PHP) acceda al archivo 
`code.txt`, primero créelo con el comando `touch` y luego cambie su 
propiedad para que pertenezca al usuario `www-data`, un miembro del 
grupo `www-data`, que es el usuario asumido por el servidor web. En 
la línea de comando, ingrese lo siguiente:

```
$ sudo touch /var/www/html/code.txt
$ sudo chown www-data:www-data /var/www/html/code.txt
```

La Figura 2-23 muestra el contenido de `code.txt`, incluidos los datos 
personales del usuario que proporcionó el código correcto.

Abrir imagen en una ventana nueva Figura 2-23

Figura 2-23 Los datos personales del ganador almacenados en el 
archivo `code.txt`

De este modo, los datos personales del usuario se guardan para 
contactar con el usuario y reclamar su premio.


