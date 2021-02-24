
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


The PHP engine evaluates the previous program to a string, which is what the user actually sees when requesting hello.php.
Hello world!

Other than PHP commands, the PHP engine accepts HTML tags, for instance:
<?php
echo "<b>Hello <br>world!</b>";
?>

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



Habilite el módulo de Apache `php7.0`, también instalado con el comando 
anterior.


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


&____________________________________________________________________
